---
title: "대형 오픈 소스 LLM 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-HowtoDeployLargeOpenSourceLLMs_0.png"
date: 2024-07-13 03:47
ogImage:
  url: /assets/img/2024-07-13-HowtoDeployLargeOpenSourceLLMs_0.png
tag: Tech
originalTitle: "How to Deploy (Large) Open Source LLMs"
link: "https://medium.com/ai-mind-labs/how-to-deploy-large-open-source-llms-3c62d216383b"
isUpdated: true
---

![Open Source LLMs](/assets/img/2024-07-13-HowtoDeployLargeOpenSourceLLMs_0.png)

# 오픈 소스 LLMs

많은 사람들이 오픈 소스가 미래라고 믿지만, 오픈 소스 LLMs를 사용하는 것은 가끔 귀찮은 경험일 수 있습니다. 더욱이 많은 인기 있는 LLM 프레임워크들이 OpenAI 모델을 기반으로 만들어진다는 사실이 문제를 악화시킵니다.

본 자습서에서는 애플리케이션 내에서 통합하기 위해 가장 큰 오픈 소스 LLMs를 신속히 배포하는 방법을 보여드리겠습니다. 배포에는 AutoGen을 사용할 것입니다. 하지만 우리가 생성하는 API 엔드포인트가 OpenAI 엔드포인트를 흉내내기 때문에 현재 OpenAI 모델을 사용하고 있는 프레임워크에 대체제로 제공되어야 할 것입니다. 단, 일부 구문 수정이 필요할 수 있습니다.

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

이 작업을 수행하는 데 몇 가지를 사용하게 될 거예요.

- 연상 리소스로는 LLM을 실행하기 위한 GPU 및 서버리스 컴퓨팅 인프라를 제공하는 Runpod 서비스를 사용할 거에요.
- 추론 서버를 생성하기 위해 OpenAI를 모방하는 API 엔드포인트를 만들 수 있는 오픈 소스 라이브러리인 vLLM을 사용할 거에요.
- 배포할 모델은 Hugging Face에서 사용 가능한 Llama-2 70B 모델입니다.

# 사전 요구 사항

시작하기 전에 다음 사전 요구 사항을 숙지해야 해요.

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

- **Hugging Face**, **Runpod**, 그리고 **Postman**과 계정을 설정해야 합니다.
- **Runpod** 크레딧을 위해 약 25달러의 예산을 투자할 의사가 있어야 합니다.
- **Llama Models**를 사용하기 위해 요청을 제출해야 합니다. 이 튜토리얼을 따라 진행하려면 이것을 반드시 처리해야 합니다.

# 배포로의 길

모든 전제조건이 갖추어지면, LLM을 배포하는 데는 다음과 같은 다섯 단계가 있습니다:

1. 컴퓨팅 요구 사항을 결정합니다.
2. Runpod 템플릿을 생성합니다.
3. 컨테이너를 배포합니다.
4. Postman으로 엔드포인트를 테스트합니다.
5. 엔드포인트를 애플리케이션에 삽입합니다.

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

# 1) 컴퓨팅 요구 사항 결정하기

선택한 모델을 실행하는 데 필요한 정확한 컴퓨팅 양을 결정해야 합니다. "Can you run it?" 앱을 사용하여 이 작업을 쉽게 수행할 수 있습니다. Hugging Face에서 모델 이름을 입력하고 (게이트된 모델의 경우 필요한 액세스 토큰을 제공한 후) 용도에 맞는 컴퓨팅 요구 사항을 확인하면 됩니다.

Llama-2–70B 모델의 경우 추론을 위해 153.78GB의 VRAM이 필요합니다.

![Link image](/assets/img/2024-07-13-HowtoDeployLargeOpenSourceLLMs_1.png)

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

The next step is to head to Runpod to figure out how many GPUs we need to hire. Visit the Secure Cloud page where you will be presented with a range of GPU options.

We need to calculate how many of these GPUs we require to host the model. We can see that for inference we need 153.78 GB of memory, the L40 GPU provides 48 GB of VRAM. Therefore, we need to rent 4 GPUs.

**Multi-GPU Inference with vLLM**

It’s essential to recognise that the deployment of multiple GPUs must be aligned with your model’s architecture, particularly when using vLLM’s tensor parallelism approach to support multi-gpu inference.

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

멀티-GPU 추론에서는 모델과 입력이 모든 GPU 작업자에 '분할(sharded)'되어 고르게 분산됩니다. 예를 들어, 68개의 어텐션 헤드를 가진 LLaMA-2-70b 모델은 네 개의 GPU 설정과 잘 맞습니다. 이 호환성을 통해 각 GPU가 모델 작업 부하의 동등한 부분을 처리하므로 효율적인 작업이 가능합니다. 모델 구성 요소의 고르게 분배가 매우 중요하며, vLLM을 사용할 때 멀티-GPU 환경에서 sharding 및 작업 효율성을 위해 필요합니다.

![Image](https://miro.medium.com/v2/resize:fit:1400/1*zYfpfIvvmYt-1BmCE4EXaA.gif)

## 2) Runpod 템플릿 만들기

이제 Runpod에 템플릿을 만들어야 합니다. 템플릿은 Runpod에게 컨테이너를 빌드하는 방법을 알려주는 지침일 뿐이며, 이후 GPU에 배포할 것입니다.

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

특정 템플릿 생성을 원하실 때는 사용자 정의 템플릿으로 이동해주세요. 거기서 vLLM의 공식 도커 이미지를 사용하여 새 컨테이너를 빌드할 거에요.

## 도커

"컨테이너 이미지" 창에 다음을 입력해주세요:

```js
vllm/vllm-openai:latest
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

**도커 명령어** 창에 다음과 같이 입력해주세요:

```js
--host 0.0.0.0 --model meta-llama/Llama-2-70b-chat-hf --tensor-parallel-size 4
```

## 포트 노출

**HTTP 포트 노출** 창에서는 포트 8000을 노출하도록 합니다.

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

## 컨테이너 및 볼륨 마운트

"컨테이너 디스크"와 "볼륨" 창에 Llama-2에 대해 150GB를 입력해야 합니다. 기본적으로, 당신은 vLLM 이미지로부터 생성된 도커 컨테이너의 다른 모든 내용과 함께 모델을 다운로드하고 저장할 충분한 공간이 필요합니다.

"볼륨 마운트 경로"에 다음을 입력하세요:

```js
/root/.cache/huggingface:/root/.cache/huggingface
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

## 환경 변수

만약 Llama-2 모델을 사용하거나 다른 게이트 모델을 사용 중이라면, "환경 변수" 메뉴를 확장하고 아래 내용을 입력해주세요.

- Key: HUGGING_FACE_HUB_TOKEN
- Value: `Your Hugging Face API Key`

오픈소스 라이브러리인 vLLM과 같은 것들은 계속해서 업데이트되고 있습니다. 여기에 있는 내용이 적용할 때까지 변경되었을 수 있기 때문에, 변경 사항이 있는지 확인하려면 vLLM 도커 배포 문서를 살펴봐주세요.

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

함께 가실 때 L4 인스턴스 4개를 선택해야 합니다. 선택이 완료되었고 서비스 요금도 확인하셨다면 배포를 진행해 주세요.

컨테이너를 배포하고 나면 다음과 같은 화면이 보일 것입니다:

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

'연결' 버튼을 눌러 HTTP 엔드포인트에 연결하세요. 그러면 아래 스크린샷에 나와있는 메뉴가 표시됩니다. 몇 분 동안 엔드포인트가 준비될 때까지 기다리세요.

준비가 되면 'HTTP 서비스에 연결하기 [포트 8000]'를 클릭하면 다음 메시지가 표시된 브라우저로 이동합니다.

```json
{ "detail": "Not Found" }
```

## 4) Postman으로 엔드포인트를 테스트하세요.

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

포스트맨에 들어가서 API 요청을 보내는 방법을 알아보세요. API 요청을 생성할 수 있는 몇 가지 옵션이 제시됩니다. 먼저 요청 유형을 POST로 변경하세요. 그 다음, 연결 화면에서 URL을 복사하고 보내기 창에 붙여넣으세요.

이제 "Body" 탭으로 이동해서 다음 내용을 붝여넣으세요:

# 5) 애플리케이션에 엔드포인트 추가하기

이제 엔드포인트를 애플리케이션에 추가할 수 있습니다. 시작 부분에서 언급했듯이, 우리의 엔드포인트는 OpenAI 엔드포인트를 모방하도록 설계되어, 그들의 API를 대신할 수 있습니다. 아래 코드는 어떻게 엔드포인트를 AutoGen 애플리케이션에 추가하는지를 보여줍니다.

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

⚠️프로젝트를 완료했다면 크레딧이 모두 소진되지 않도록 Runpod 인스턴스를 중지하는 것을 잊지 마세요!

## 결론

오픈 소스 LLM(Large Language Models)을 활용하면 추론과 배포 프레임워크에 익숙해지는 것을 의미합니다. 이 짧은 튜토리얼이 오픈 소스 LLM을 활용한 배포 및 개발을 시작할 충분한 지식을 제공해 드렸기를 바랍니다. 실제로 작동하는 데모를 보고 싶다면 아래 링크된 YouTube 비디오를 자유롭게 시청해보세요.

인공 지능 기술을 향상시키고 싶다면, 내 코스를 위한 대기 목록에 가입하세요. 거기서 영어 문장 생성에 사용되는 대형 언어 모델을 활용하는 애플리케이션 개발 과정을 안내해 드립니다.

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

금연 상담을 받아보시려면 당신의 비즈니스에 AI 전환을 찾고 계시다면, 오늘 바로 발견 전화를 예약하세요.

인공지능, 데이터 과학, 그리고 대형 언어 모델에 대한 더 많은 통찰력을 얻으시려면 YouTube 채널을 구독하세요.

## AI 마인드로부터의 메시지

![AI Mind](https://miro.medium.com/v2/resize:fit:500/0*5Wm7sOfTpe5DEbhg.gif)

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

우리 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 👏 이야기에 박수를 치시고 작가를 팔로우 해보세요 👉
- 📰 AI Mind Publication에서 더 많은 콘텐츠를 확인해 보세요
- 🧠 AI 프롬프트를 손쉽게 무료로 향상시키세요
- 🧰 직관적인 AI 도구들을 발견해 보세요
