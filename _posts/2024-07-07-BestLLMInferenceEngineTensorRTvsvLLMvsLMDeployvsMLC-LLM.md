---
title: "최고의 LLM 추론 엔진은 TensorRT vs vLLM vs LMDeploy vs MLC-LLM 비교"
description: ""
coverImage: "/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_0.png"
date: 2024-07-08 00:00
ogImage:
  url: /assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_0.png
tag: Tech
originalTitle: "Best LLM Inference Engine? TensorRT vs vLLM vs LMDeploy vs MLC-LLM"
link: "https://medium.com/@zaiinn440/best-llm-inference-engine-tensorrt-vs-vllm-vs-lmdeploy-vs-mlc-llm-e8ff033d7615"
isUpdated: true
---

다양한 LLM 추론 엔진 벤치마킹 중입니다.

![LLM Inference Engines](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_0.png)

LLM은 채팅 및 코드 완성 모델과 같은 텍스트 생성 응용 프로그램에서 뛰어납니다. 그러나 그들의 큰 크기는 추론에 도전을 제공합니다. 기본 추론은 LLM이 토큰 단위로 텍스트를 생성하기 때문에 각 다음 토큰에 대한 반복 호출이 필요합니다. 입력 시퀀스가 커질수록 처리 시간이 증가합니다. 게다가, LLM은 수십억 개의 매개변수를 가지고 있어서, 모든 그 가중치들을 메모리에 저장하고 관리하기 어려워집니다.

LLM 추론 및 서비스 최적화를 위해 여러 가지 프레임워크와 패키지가 있습니다. 이 블로그에서는 다음과 같은 추론 엔진을 사용하고 비교해 보겠습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- TensorRT-LLM
- vLLM
- LMDeploy
- MLC-LLM

# 1. TensorRT-LLM

# 소개

TensorRT-LLM은 NVIDIA GPU에서 최신 LLM의 추론 성능을 가속화하고 최적화하는 또 다른 추론 엔진입니다. LLM은 TensorRT Engine으로 컴파일되고 In-Flight Batching(대기 시간 감소와 더 높은 GPU 활용률을 허용하는 처리)과 같은 추론 최적화를 활용하기 위해 Triton 서버와 함께 배포됩니다. 이러한 최적화에는 paged KV caching, MultiGPU-MultiNode Inference 및 FP8 지원이 포함됩니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 사용법

HF 모델, TensorRT 모델 및 TensorRT-INT8 모델(양자화) 간의 실행 시간, ROUGE 점수, 지연 시간 및 처리량을 비교할 것입니다.

Linux 시스템에 Nvidia-container-toolkit을 설치하고 HF 모델을 다운로드하기 위해 Git LFS를 초기화하고 필요한 패키지를 다음과 같이 다운로드해야 합니다.

```js
!curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
!apt-get update
!git clone https://github.com/NVIDIA/TensorRT-LLM/
!apt-get update && apt-get -y install python3.10 python3-pip openmpi-bin libopenmpi-dev
!pip3 install tensorrt_llm -U --pre --extra-index-url https://pypi.nvidia.com
!pip install -r TensorRT-LLM/examples/phi/requirements.txt
!pip install flash_attn pytest
!curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash
!apt-get install git-lfs
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

지금 모델 가중치를 검색하세요.

```js
PHI_PATH="TensorRT-LLM/examples/phi"
!rm -rf $PHI_PATH/7B
!mkdir -p $PHI_PATH/7B && git clone https://huggingface.co/microsoft/Phi-3-small-128k-instruct $PHI_PATH/7B
```

모델을 TensorRT-LLM 체크포인트 형식으로 변환하고 해당 체크포인트로 TensorRT-LLM을 빌드하세요.

```js
!python3 $PHI_PATH/convert_checkpoint.py --model_dir $PHI_PATH/7B/ \
                --dtype bfloat16 \
                --output_dir $PHI_PATH/7B/trt_ckpt/bf16/1-gpu/
# 체크포인트로부터 TensorRT-LLM 모델 빌드
!trtllm-build --checkpoint_dir $PHI_PATH/7B/trt_ckpt/bf16/1-gpu/ \
                --gemm_plugin bfloat16 \
                --output_dir $PHI_PATH/7B/trt_engines/bf16/1-gpu/
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이제 HF 모델에 INT8 가중치만 양자화를 적용하고 체크포인트를 TensorRT-LLM으로 변환해보세요.

```python
!python3 $PHI_PATH/convert_checkpoint.py --model_dir $PHI_PATH/7B \
                --dtype bfloat16 \
                --use_weight_only \
                --output_dir $PHI_PATH/7B/trt_ckpt/int8_weight_only/1-gpu/
!trtllm-build --checkpoint_dir $PHI_PATH/7B/trt_ckpt/int8_weight_only/1-gpu/ \
                --gemm_plugin bfloat16 \
                --output_dir $PHI_PATH/7B/trt_engines/int8_weight_only/1-gpu/
```

이제 베이스 phi3 및 두 TensorRT 모델을 요약 작업에서 테스트하세요.

```python
%%capture phi_hf_results
# Huggingface
!time python3 $PHI_PATH/../summarize.py --test_hf \
                       --hf_model_dir $PHI_PATH/7B/ \
                       --data_type bf16 \
                       --engine_dir $PHI_PATH/7B/trt_engines/bf16/1-gpu/
%%capture phi_trt_results
# TensorRT-LLM
!time python3 $PHI_PATH/../summarize.py --test_trt_llm \
                       --hf_model_dir $PHI_PATH/7B/ \
                       --data_type bf16 \
                       --engine_dir $PHI_PATH/7B/trt_engines/bf16/1-gpu/
%%capture phi_int8_results
# TensorRT-LLM (INT8)
!time python3 $PHI_PATH/../summarize.py --test_trt_llm \
                       --hf_model_dir $PHI_PATH/7B/ \
                       --data_type bf16 \
                       --engine_dir $PHI_PATH/7B/trt_engines/int8_weight_only/1-gpu/
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이제 결과를 캡처한 후 출력을 파싱하고 모든 모델 사이의 실행 시간, ROUGE 점수, 대기 시간 및 처리량을 비교하기 위해 그림을 그려볼 수 있습니다.

![Image 1](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_1.png)

![Image 2](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_2.png)

## 2. vLLM

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 소개

vLLM은 최신 기술을 활용한 LLM 추론 및 서비스를 제공합니다. 페이지 어텐션, 연속 배치, 양자화 (GPTQ, AWQ, FP8), 그리고 최적화된 CUDA 커널을 사용하여 최신 속도를 보장합니다.

# 사용 방법

마이크로소프트/Phi3-mini-4k-instruct의 처리량과 대기 시간을 평가해 봅시다. 먼저 의존성을 설정하고 라이브러리를 가져오는 것부터 시작해 보세요.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

!pip install -q vllm
!git clone https://github.com/vllm-project/vllm.git
!pip install -q datasets
!pip install transformers scipy
from vllm import LLM, SamplingParams
from datasets import load_dataset
import time
from tqdm import tqdm
from transformers import AutoTokenizer

이제 모델을 로드하고 데이터셋의 일부에서 출력을 생성해 봅시다.

dataset = load_dataset("akemiH/MedQA-Reason", split="train").select(range(10))
prompts = []
for sample in dataset:
prompts.append(sample)
sampling_params = SamplingParams(max_tokens=524)
llm = LLM(model="microsoft/Phi-3-mini-4k-instruct", trust_remote_code=True)
def generate_with_time(prompt):
start = time.time()
outputs = llm.generate(prompt, sampling_params)
taken = time.time() - start
generated_text = outputs[0].outputs[0].text
return generated_text, taken
generated_text = []
time_taken = 0
for sample in tqdm(prompts):
text, taken = generate_with_time(sample)
time_taken += taken
generated_text.append(text)

# 출력을 토크나이즈하고 처리량을 계산합니다.

tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
token = 1
for sample in generated_text:
tokens = tokenizer(sample)
tok = len(tokens.input_ids)
token += tok
print(token)
print("tok/s", token // time_taken)

![2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_3.png](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_3.png)

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이번에는 ShareGPT 데이터셋에서 vLLM을 통해 모델의 성능을 벤치마킹해보겠어요.

```js
!wget https://huggingface.co/datasets/anon8231489123/ShareGPT_Vicuna_unfiltered/resolve/main/ShareGPT_V3_unfiltered_cleaned_split.json
%cd vllm
!python benchmarks/benchmark_throughput.py --backend vllm --dataset ../ShareGPT_V3_unfiltered_cleaned_split.json --model microsoft/Phi-3-mini-4k-instruct --tokenizer microsoft/Phi-3-mini-4k-instruct --num-prompts=1000
```

![Markdown image](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_4.png)

# 3. LMDeploy

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 소개

이 패키지는 효율적인 추론을 제공하면서, 지속적인 배치, 블록된 KV 캐시, 동적 분할 및 합병, 텐서 병렬 처리, 고성능 CUDA 커널을 제공함으로써 LLMs를 압축, 배포 및 제공할 수 있습니다. 또한 4비트 추론 성능은 FP16의 2.4배 높다는 효과적인 양자화를 제공하며, 다중 모델 서비스를 여러 대의 머신과 카드에 걸쳐 배포하는 간편한 분산 서버 및 상호작용 추론 모드(대화 내역을 기억하고 과거 세션의 반복 처리를 피함)를 가능하게 합니다. 또한 토큰 대기 시간 및 처리량, 요청 처리량, API 서버 및 트리톤 추론 서버 성능 프로파일링도 가능합니다.

# 사용법

의존성을 설치하고 패키지를 가져옵니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

LMdeploy가 TurboMind와 PyTorch 두 개의 추론 엔진을 개발했다는 정보야.

마이크로소프트/Phi3-mini-128k-instruct의 PyTorch 엔진을 프로파일링해 보자구.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

**프로필 생성 코드:**

```shell
!python3 profile_generation.py microsoft/Phi-3-mini-128k-instruct --backend pytorch
```

이 코드는 엔진을 여러 라운드에 걸쳐 프로필링하고, 각 라운드에 대한 토큰 지연 시간 및 처리량을 보고합니다.

**Pytorch 엔진의 토큰 지연 시간 및 처리량 프로필:**

![이미지](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_5.png)

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 4. MLC-LLM

# 소개

MLC-LLM은 MLCEngine이라고 불리는 고성능 배포 및 추론 엔진을 제공합니다.

# 사용법

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

의존성을 설치한 후 conda로 종속 항목을 설정하고 conda 환경을 만듭니다. 그런 다음 git 저장소를 복제하고 설정합니다.

conda activate your-environment
python -m pip install --pre -U -f https://mlc.ai/wheels mlc-llm-nightly-cu121 mlc-ai-nightly-cu121
conda env remove -n mlc-chat-venv
conda create -n mlc-chat-venv -c conda-forge \
 "cmake>=3.24" \
 rust \
 git \
 python=3.11
conda activate mlc-chat-venv
git clone --recursive https://github.com/mlc-ai/mlc-llm.git && cd mlc-llm/
mkdir -p build && cd build
python ../cmake/gen_cmake_config.py
cmake .. && cmake --build . --parallel $(nproc) && cd ..
set(USE_FLASHINFER ON)
conda activate your-own-env
cd mlc-llm/python
pip install -e .

MLC LLM으로 모델을 실행하려면 모델 가중치를 MLC 형식으로 변환해야 합니다. Git LFS를 사용하여 HF 모델을 다운로드한 후 가중치를 변환하세요.

mlc_llm convert_weight ./dist/models/Phi-3-small-128k-instruct/ \
 --quantization q0f16 \
 --model-type "phi3" \
 -o ./dist/Phi-3-small-128k-instruct-q0f16-MLC

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이제 MLC 형식 모델을 MLC 엔진에 로드해 보세요.

```js
from mlc_llm import MLCEngine
# 엔진 생성
model = "HF://mlc-ai/Phi-3-mini-128k-instruct-q0f16-MLC"
engine = MLCEngine(model)

# 이제 처리량을 계산해 봅시다
import time
from transformers import AutoTokenizer
start = time.time()
response = engine.chat.completions.create(
    messages=[{"role": "user", "content": "머신 러닝이 무엇인가요?"}],
    model=model,
    stream=False,
)
taken = time.time() - start
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-128k-instruct")
print("tok/s", 82 // taken)
```

![이미지](/assets/img/2024-07-07-BestLLMInferenceEngineTensorRTvsvLLMvsLMDeployvsMLC-LLM_6.png)

# 개요

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

특히 HF 모델과 일반 TensorRT 대비 TensorRT INT8 모델이 추론 속도에서 우수한 성능을 보여줍니다. 반면 일반 TensorRT 모델은 요약 작업에서 가장 높은 ROUGE 점수를 기록하여 성능이 더 우수했습니다. LMDeploy는 A100에서 vLLM 대비 최대 1.8배 높은 요청 처리량을 제공합니다.

이 실험을 위한 컴퓨팅을 후원해준 QueryLoopAI에게 특별히 감사드립니다.

또한 메시지를 보내거나 아래와 같은 방식으로 저에게 연락하실 수 있습니다:

- LinkedIn 및 Twitter에서 연락하고 팔로우하기
- 📚 Medium에서 저를 팔로우하기
- 📢 주간 AI 뉴스레터를 구독하기
- 🤗 Hugging Face 사이트 방문하기
