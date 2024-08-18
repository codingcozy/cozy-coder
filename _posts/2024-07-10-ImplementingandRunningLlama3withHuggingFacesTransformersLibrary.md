---
title: "Hugging Face의 Transformers 라이브러리로 Llama 3 구현 및 실행 방법"
description: ""
coverImage: "/assets/img/2024-07-10-ImplementingandRunningLlama3withHuggingFacesTransformersLibrary_0.png"
date: 2024-07-10 00:25
ogImage:
  url: /assets/img/2024-07-10-ImplementingandRunningLlama3withHuggingFacesTransformersLibrary_0.png
tag: Tech
originalTitle: "Implementing and Running Llama 3 with Hugging Face’s Transformers Library"
link: "https://medium.com/@manuelescobar-dev/implementing-and-running-llama-3-with-hugging-faces-transformers-library-40e9754d8c80"
isUpdated: true
---

## Llama 3을 Hugging Face Transformers와 함께 실행하는 단계별 가이드

![img](/assets/img/2024-07-10-ImplementingandRunningLlama3withHuggingFacesTransformersLibrary_0.png)

LLM 배포 시리즈의 일환으로, 이 기사는 Hugging Face의 Transformers 라이브러리를 사용하여 Llama 3를 구현하는 데 초점을 맞춥니다. 이 라이브러리는 가장 널리 활용되며 다양한 기능을 제공하여 Llama 3의 지역 구현에 대한 접근성과 사용자 정의 가능성을 높여줍니다.

튜토리얼은 Llama-3-8B-Instruct를 사용하지만 Hugging Face에서 선택한 모델에도 동일하게 적용됩니다. Llama-3-8B-Instruct는 요약 및 질문 응답과 같은 여러 작업에서 세밀 조정된 80억 개의 매개변수 모델에 해당합니다. 대안으로, sequence-to-sequence 생성에 교육된 베이스 모델인 Llama-3-8B를 사용할 수도 있습니다.

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

## 하드웨어 요구 사항

이 튜토리얼을 정확하게 따라가려면 적어도 8GB의 GPU 메모리가 필요합니다. 그러나 메서드와 라이브러리를 통해 추가 최적화가 가능합니다. 자세한 내용은 아래 자료를 확인해주세요:

- LLM 훈련 및 추론에 대한 메모리 요구 사항
- LLM 시스템 요구 사항 계산기

# Transformers 라이브러리 개요

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

트랜스포머는 수천 개의 미리 훈련된 모델을 포함한 강력한 라이브러리입니다. 텍스트, 비전 및 오디오 영역에서 작업을 수행할 준비가 되어 있습니다. 텍스트 분류, 질의 응답, 요약, 번역 또는 100여개 이상의 언어로 텍스트 생성이 필요한 경우, 트랜스포머가 모든 것을 제공합니다. 또한 분류, 객체 탐지 및 세분화와 같은 이미지 작업에서 우수하며, 음성 인식 및 음성 분류와 같은 오디오 작업에도 능가합니다. 게다가 테이블 질의 응답, OCR, 비디오 분류 등과 같은 복합 작업을 처리할 수 있습니다.

### 트랜스포머를 사랑할 이유

- 쉬운 사용: 미리 훈련된 모델을 빠르게 다운로드하여 사용하거나 데이터를 세밀하게 조정할 수 있습니다.
- 다목적: 텍스트, 이미지, 오디오 및 다중 모달 작업을 지원합니다.
- 매끄러운 통합: Jax, PyTorch 및 TensorFlow와 함께 작동하여 프레임워크 간에 쉽게 전환할 수 있습니다.
- 높은 성능: 낮은 진입 장벽과 함께 다양한 작업에 대한 최신 모델을 제공합니다.
- 비용 효율적: 공유 모델을 사용하여 계산 비용을 절약할 수 있습니다.

### 고려해볼 사항

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

- 모듈화되지 않음: 처음부터 신경망을 구축하기 위해 설계되지 않았습니다.
- 특정 훈련 API: 해당 모델에 최적화되어 있으며 일반적인 머신러닝 루프에 최적화되어 있지 않습니다.
- 예제 조정 필요: 스크립트를 조정하여 특정 요구에 맞게 맞춰야 할 수 있습니다.

트랜스포머는 몇 줄의 코드로 강력한 모델을 훈련하고 평가하며 배포하는 것을 간단하게 만들어 주어 다양한 응용 프로그램에서 유연성과 높은 성능을 제공합니다.

# 설치

## 가상 환경 생성 (권장됨)

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

먼저 프로젝트를 위한 가상 환경을 만들어주세요. 만약 이미 설정된 가상 환경이 있다면 이 단계는 선택 사항입니다.

- 프로젝트 디렉토리로 이동하여 가상 환경을 만듭니다:

```bash
python -m venv env_name
```

2. 환경을 활성화하세요:

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

환경이름\Scripts\activate

## 모델 다운로드

- Hugging Face CLI 설치하기:

pip install -U "huggingface_hub[cli]"

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

2. **Hugging Face 계정을 만들어보세요!** (https://huggingface.co/) 그리고 **액세스 토큰을 생성**해보세요! (https://huggingface.co/settings/tokens).

3. **Hugging Face 계정에 로그인**해보세요:

```bash
huggingface-cli login
```

4. **Llama-3–8B-Instruct** 모델의 조건과 개인정보 보호정책을 **수락**해주세요! (https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct). **액세스 권한이 부여될 때까지 기다려주세요**. 보통 15분 정도 걸리지만 그 이상의 시간이 소요될 수도 있습니다.

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

5. 모델을 다운로드하려면 (원하는 경로 지정):

```js
huggingface-cli download meta-llama/Meta-Llama-3-8B-Instruct --exclude "original/*" --local-dir meta-llama/Meta-Llama-3-8B-Instruct
```

## 패키지 설치

먼저, 필요한 패키지를 설치하세요.

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

Windows (CUDA 지원):

```js
pip install torch --index-url <https://download.pytorch.org/whl/cu121>
pip install accelerate transformers bitsandbytes
```

# 구현

## 파이프라인

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

**타로 전문가에게 문의해 보세요!**

피플라인 방법은 모델을 메모리로 로드하고 추론을 위해 설정합니다.

- model_id: 모델 경로
- torch_dtype: 가중치의 데이터 유형을 지정합니다.

옵션 1: 양자화 사용 (~8 GB)

양자화는 하드웨어 요구 사항을 줄이는 방법으로 모델 가중치를 낮은 정밀도로 로드합니다. 16비트(float16) 대신에 4비트로 로드하여 메모리 사용량을 대폭 줄여 약 20GB에서 약 8GB로 줄입니다. 🌟

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

self.pipeline = transformers.pipeline(
"text-generation",
model=self.model_id,
model_kwargs={
"torch_dtype": torch.float16,
"quantization_config": {"load_in_4bit": True},
"low_cpu_mem_usage": True,
},
)

Option 2: Without Quantization (~20 GB)

self.pipeline = transformers.pipeline(
"text-generation",
model=self.model_id,
model_kwargs={"torch_dtype": torch.float16},
)

## 종결자들

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

테르미네이터는 생성된 텍스트 시퀀스의 끝을 신호합니다. 이들은 모델이 텍스트 생성을 멈추어야 하는 시점을 알 수 있도록 도와줍니다. 코드에서 img 태그를 마크다운 형식으로 변경하면:

```js
self.terminators = [self.pipeline.tokenizer.eos_token_id, self.pipeline.tokenizer.convert_tokens_to_ids("")];
```

## 답변

LLM은 일반적으로 대화를 입력으로받습니다. 이 대화에는 정의된 역할을 가진 메시지 기록이 포함됩니다:

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

## 전체 구현 코드

```python
import torch
import transformers

class Llama3:
    def __init__(self, model_path):
        self.model_id = model_path
        self.pipeline = transformers.pipeline(
            "text-generation",
            model=self.model_id,
            model_kwargs={
                "torch_dtype": torch.float16,
                "quantization_config": {"load_in_4bit": True},
                "low_cpu_mem_usage": True,
            },
        )
        self.terminators = [
            self.pipeline.tokenizer.eos_token_id,
            self.pipeline.tokenizer.convert_tokens_to_ids(""),
        ]

    def get_response(
          self, query, message_history=[], max_tokens=4096, temperature=0.6, top_p=0.9
      ):
        user_prompt = message_history + [{"role": "user", "content": query}]
        prompt = self.pipeline.tokenizer.apply_chat_template(
            user_prompt, tokenize=False, add_generation_prompt=True
        )
        outputs = self.pipeline(
            prompt,
            max_new_tokens=max_tokens,
            eos_token_id=self.terminators,
            do_sample=True,
            temperature=temperature,
            top_p=top_p,
        )
        response = outputs[0]["generated_text"][len(prompt):]
        return response, user_prompt + [{"role": "assistant", "content": response}]

    def chatbot(self, system_instructions=""):
        conversation = [{"role": "system", "content": system_instructions}]
        while True:
            user_input = input("User: ")
            if user_input.lower() in ["exit", "quit"]:
                print("챗봇을 종료합니다. 안녕히 가세요!")
                break
            response, conversation = self.get_response(user_input, conversation)
            print(f"Assistant: {response}")

if __name__ == "__main__":
    bot = Llama3("여기에 모델 경로 입력")
    bot.chatbot()
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

# 결론

이 글에서는 Llama 3와 Hugging Face의 Transformers 라이브러리를 사용하여 간단한 챗봇을 만들었습니다. 이 구현은 여러분의 애플리케이션에 통합할 수 있습니다. 더 견고한 설정을 위해, 애플리케이션이 강제 종료될 때마다 다시 시작하는 것을 피하기 위해 Flask나 FastAPI를 사용하는 서버를 생성하는 것을 고려해 보세요.

백엔드 서버와 Streamlit UI를 사용한 ChatGPT와 유사한 인터페이스를 만드는 완벽한 튜토리얼을 기대해 주세요!

# 참고문헌

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

안녕하세요! 여기 타로 전문가 입니다.

- [메타-라마-3-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct)
- [라마3 블로그](https://huggingface.co/blog/llama3)
- [메타 라마 3 블로그](https://ai.meta.com/blog/meta-llama-3/)
- [라마3 깃허브](https://github.com/meta-llama/llama3)

이런 링크들이 있답니다. 자세한 내용을 보고 싶으시면 클릭해보세요! 즐거운 타로세상 여행되세요! 🌟
