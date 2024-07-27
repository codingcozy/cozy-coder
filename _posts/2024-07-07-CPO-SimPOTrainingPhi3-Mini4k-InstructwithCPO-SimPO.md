---
title: "CPO-SimPO  CPO-SimPO를 사용해 Phi3-Mini4k-Instruct 훈련하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_0.png"
date: 2024-07-07 13:30
ogImage: 
  url: /assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_0.png
tag: Tech
originalTitle: "CPO-SimPO | Training Phi3-Mini4k-Instruct with CPO-SimPO"
link: "https://medium.com/@zaiinn440/cpo-simpo-training-phi3-mini4k-instruct-with-cpo-simpo-de8c58b3ac32"
---


친애하는 분들,

LLM을 DPO보다 덜 메모리 사용량과 빠른 속도로 효율적으로 정렬하는 방법을 알려드리겠습니다.

![이미지](/assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_0.png)

# 소개

최적의 성능을 위해 LLM을 정렬하는 일반적인 방법은 지도 Fine-Tuning (SFT)로 시작됩니다. 보통 모델을 4-Bit로 로드하고 LoRA 훈련을 위한 구성을 적용합니다. 표준 방법은 모델을 4-Bit 모드로 로드하고 LoRA (낮은 순위 적응) 훈련을 위한 구성을 적용하는 것입니다. DPO (직접 선호 최적화)는 비용을 낮추기 위해 모델을 최적화하는 또 다른 주요 기법입니다. 표준 방법은 SFT+DPO를 결합하여 모델 성능을 더 개선하지만 비용이 발생할 수 있습니다. Odds Ratio Preference Optimization (ORPO)는 SFT+DPO를 대체하여 선호 및 비선호 응답 사이의 생성 스타일을 구분하기 위해 확률 비율 기반의 패널티를 부과하여 보다 향상된 성능을 제공하는 단일 단계로 변환합니다.

<div class="content-ad"></div>

더 안정적인 훈련과 향상된 성능을 위한 또 다른 기술로 CPO-SimPO가 있습니다. 이 기술은 모델 성능에 대한 훈련 데이터 품질에 대한 SFT의 의존성을 극복하고, DPO의 메모리+속도 비효율성을 (parametrized와 reference policy 둘 다 다룰 때) 완화하며, 긴 ​​but 저품질 순서의 생성을 방지하는 것을 목표로 합니다. 이 블로그에서는 이 기술에 대해 자세히 소개하고 CPO-SimPO에서 Phi3-Mini-4K-Instruct를 추가로 훈련할 것입니다.

# CPO-SimPO는 무엇인가요?

이것은 두 선호 최적화 방법인 CPO와 SimPO를 결합한 것입니다.

# 대조적인 선호 최적화 — CPO

<div class="content-ad"></div>

하오란 씨 등이 제안한 CPO 목적은 원래 DPO 손실에서 이상적인 정책을 제외하고 근사하는 것입니다. 또한 행동 복제(BC) 정규화기가 통합되어 모델이 선호하는 데이터 분포에서 벗어나지 않도록 보장합니다.

![CPO Training](/assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_1.png)

CPO는 뛰어난 품질의 그러나 흠없는 선호 데이터셋(prompt, chosen, rejected 형식)이 필요합니다. 이를 통해 모델 출력의 완벽함을 달성하고 심지어 작은 결함도 완화할 수 있습니다.

# Simple Preference Optimization — SimPO

<div class="content-ad"></div>

Yu Meng님 등에 의해 소개된 2024년 SimPO는 일반 DPO와 대비하여 참조 모델이 필요 없어요. 이는 주요 정책 모델이 생성한 모든 토큰의 평균 로그 확률에 기반한 길이 정규화된 보상을 통해 명시적 보상 모델 대신에 직접 동작하는 모델입니다. 둘째로, 거절된 응답과 선택된 응답 간 보상 차이가 특정 보상 마진 γ를 초과하도록 하는 목표 보상 마진 γ를 도입합니다. 

![이미지](/assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_2.png)

SimPO는 명시적 보상 모델을 사용하지 않아 DPO보다 메모리 및 계산 효율이 높으며, AlpacaEval2와 ArenaHard에서 DPO를 능가하면서 긴 길이의 낮은 품질 시퀀스를 생성하는 것을 방지합니다.

![이미지](/assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_3.png)

<div class="content-ad"></div>

# CPO-SimPO

두 가지 목적을 결합하면 CPO-SimPO 손실이 발생하여 두 선호 최적화 방법의 혜택을 함께 활용할 수 있습니다.

![image](/assets/img/2024-07-07-CPO-SimPOTrainingPhi3-Mini4k-InstructwithCPO-SimPO_4.png)

# CPO-SimPO Training of Phi3-Mini-4k-Instruct

<div class="content-ad"></div>

HuggingFace 모델의 CPO-SimPO 교육을 공식 GitHub 저장소를 사용하여 수행할 수 있어요.

# 종속성 설정하기

conda를 사용하여 Python 환경을 만들어야 해요. conda가 설치되어 있지 않은 경우 conda를 설치하는 방법을 알려 드릴게요:

```js
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```

<div class="content-ad"></div>

새로운 효과가 적용되려면 새 터미널을 열어야 합니다. 이제 파이썬 가상 환경을 만들어보세요.

```bash
conda create -n handbook python=3.10 && conda activate handbook
```

그런 다음 시스템에 따라 설치 방법이 달라질 수 있는 pytorch v2.2.2를 설치해야 합니다.

```bash
conda install pytorch==2.2.2 torchvision==0.17.2 torchaudio==2.2.2 pytorch-cuda=11.8 -c pytorch -c nvidia
```


<div class="content-ad"></div>

이 코드베이스는 alignment-handbook 저장소를 기반으로 만들어졌어요. 그래서 해당 패키지 의존성을 설치할 수 있어요.

```bash
git clone https://github.com/huggingface/alignment-handbook.git
cd ./alignment-handbook/
python -m pip install .
cd ..
```

또한 Flash Attention 2도 설치해야 해요:

```bash
python -m pip install flash-attn --no-build-isolation
```

<div class="content-ad"></div>

이제 CPO_SimPO 저장소를 복제해 볼까요?

```js
git clone https://github.com/fe1ixxu/CPO_SIMPO.git
cd CPO_SIMPO
```

# 훈련 구성

훈련 인자를 지정하는 .yaml 구성 파일을 만들어야 합니다. GPU 사양에 따라 per_device_train_batch_size와 max_length를 조정하십시오. 'loss_type: simpo'와 'cpo_alpha'를 0이 아닌 값으로 설정했다는 점에 유의하세요.

<div class="content-ad"></div>


# 모델 인자
- model_name_or_path: microsoft/Phi-3-mini-4k-instruct
- torch_dtype: null
- use_flash_attention_2: false

# 데이터 훈련 인자
- dataset_mixer: princeton-nlp/llama3-ultrafeedback: 1.0
- dataset_splits:
    - train
    - test
- preprocessing_num_workers: 12

# CPOTrainer 인자
- bf16: true
- beta: 10
- simpo_gamma: 5.4
- cpo_alpha: 0.05
- loss_type: simpo
- do_eval: true
- evaluation_strategy: steps
- eval_steps: 400
- gradient_accumulation_steps: 4
- gradient_checkpointing: true
- gradient_checkpointing_kwargs:
    - use_reentrant: False
- hub_model_id: cpo-simpo-exps
- learning_rate: 1.0e-6
- log_level: info
- logging_steps: 5
- lr_scheduler_type: cosine
- max_length: 2048
- max_prompt_length: 1800
- num_train_epochs: 1
- optim: adamw_torch
- output_dir: outputs/phi3mini4k-cpo-simpo
- run_name: phi3mini4k-cpo-simpo
- per_device_train_batch_size: 2
- per_device_eval_batch_size: 2
- push_to_hub: false
- save_strategy: "steps"
- save_steps: 1000000
- report_to:
    - none
- save_total_limit: 20
- seed: 42
- warmup_ratio: 0.1


# 가속화 구성

다음으로 하드웨어 구성을 지정해야 합니다. accelerate_configs 디렉터리 아래의 저장소에서 제공된 deepspeed_zero3.yaml 구성을 활용하겠습니다. num_processes를 사용 가능한 GPU의 수로 선택하세요. CUDA 오류를 피하기 위해 A100 GPU가 필요할 수 있습니다.


- compute_environment: LOCAL_MACHINE
- debug: false
- deepspeed_config:
    - deepspeed_multinode_launcher: standard
    - offload_optimizer_device: none
    - offload_param_device: none
    - zero3_init_flag: true
    - zero3_save_16bit_model: true
    - zero_stage: 3
- distributed_type: DEEPSPEED
- downcast_bf16: 'no'
- machine_rank: 0
- main_training_function: main
- mixed_precision: bf16
- num_machines: 1
- num_processes: 1
- rdzv_backend: static
- same_network: true
- tpu_env: []
- tpu_use_cluster: false
- tpu_use_sudo: false
- use_cpu: false


<div class="content-ad"></div>

# 훈련 시작

훈련과 가속 구성 파일의 경로를 제공하고 훈련을 시작하세요. 최종 모델은 훈련 인수에서 지정된 output_dir에 사용 가능합니다.

```js
ACCELERATE_LOG_LEVEL=info accelerate launch --config_file accelerate_configs/deepspeed_zero3.yaml scripts/run_cpo.py training_configs/phi3-mini4k-instruct-cpo-simpo.yaml
```

# 추론

<div class="content-ad"></div>

허깅페이스 계정에 모델을 업로드한 후에는 다음과 같이 추론을 수행할 수 있어요:

```js
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline
torch.random.manual_seed(0)
model = AutoModelForCausalLM.from_pretrained(
    "abideen/Phi-3-mini-4K-instruct-cpo-simpo", 
    device_map="cuda", 
    torch_dtype="auto", 
    trust_remote_code=True, 
)
tokenizer = AutoTokenizer.from_pretrained("abideen/Phi-3-mini-4K-instruct-cpo-simpo")
messages = [
    {"role": "user", "content": "바나나와 드래곤프루츠의 조합을 먹는 방법을 알려주세요."},
    {"role": "assistant", "content": "물론이죠! 바나나와 드래곤프루츠를 함께 먹는 여러 가지 방법이 있어요: 1. 바나나와 드래곤프루츠 스무디: 바나나와 드래곤프루츠를 우유와 꿀과 함께 갈아 만들어요. 2. 바나나와 드래곤프루츠 샐러드: 슬라이스한 바나나와 드래곤프루츠를 레몬 주스와 꿀과 함께 섞어요."},
    {"role": "user", "content": "2x + 3 = 7 방정식을 풀면 어떻게 되나요?"},
]
pipe = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
)
generation_args = {
    "max_new_tokens": 500,
    "return_full_text": False,
    "temperature": 0.0,
    "do_sample": False,
}
output = pipe(messages, **generation_args)
print(output[0]['generated_text'])
```

이 실험에 사용된 컴퓨팅을 후원해준 QueryLoopAI에 특별히 감사드려요.

그리고 언제든지 메시지를 남기거나:

- [Twitter](https://twitter.com/example)
- [Email](mailto:example@example.com)
- [Website](https://www.example.com)

와 같은 방법으로 연락해도 돼요.

<div class="content-ad"></div>

- LinkedIn, Twitter에서 연락하고 팔로우해 주세요
- 📚 Medium에서 제 팔로우 해주세요
- 📢 주간 AI 뉴스레터를 구독해 주세요!
- 🤗 Hugging Face도 확인해 보세요