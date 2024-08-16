---
title: "카멜레온 메타의 혼합 모달 기반 모델 "
description: ""
coverImage: "/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_0.png"
date: 2024-07-13 03:03
ogImage: 
  url: /assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_0.png
tag: Tech
originalTitle: "Chameleon, Meta’s Mixed-Modal Foundation Model"
link: "https://medium.com/generative-ai/chameleon-metas-mixed-modal-foundation-model-bfc259d11ac1"
isUpdated: true
---





최근에 Meta에서 새로운 혼합 형식의 기반 모델 가족을 출시했습니다. 이전 모델은 각 양식을 별도로 모델링하고 서로 다른 인코더 및 디코더를 의존하여 결합했던 것과 달리, Chameleon 모델 가족은 두 양식을 모델링하는 데 하나의 아키텍처를 사용합니다.

간단히 말하면, 이미지와 텍스트를 이해하는 모델을 만들고 싶다면, 이전 접근 방식은 두 가지 다른 모델을 결합했습니다: 이미지를 임베딩 공간으로 나타내고 텍스트를 다른 임베딩 공간으로 나타낸 후 이를 공동으로 훈련시킵니다.

Chameleon이 제안하는 것은 한 가지 통합된 임베딩 공간에서 두 양식을 모두 나타내는 단일 모델을 만드는 것입니다.

<div class="content-ad"></div>

동일한 공간에서 다양한 모달리티를 표현하는 것은 그들의 다른 본성 때문에 어려울 수 있습니다. 텍스트는 분리 가능하기 때문에 여러 개의 단어 또는 토큰으로 나눌 수 있지만, 이미지는 연속된 형태를 가지고 있습니다.

Meta의 팀은 이를 해결하기 위해 이미지 토크나이저를 제안합니다. 이 토크나이저의 역할은 이미지를 이산 집합의 토큰으로 변환하는 것입니다.

이러한 토큰들은 텍스트 토큰들과 결합되어 동일한 트랜스포머 아키텍처로 입력됩니다. 이러한 융합은 모델이 모달리티 간에 쉽게 추론하고 생성할 수 있게 합니다.

나중에 살펴볼 것처럼, 이 방법론은 교육 안정성과 확장성에 관련된 문제를 포함한 단점을 가지고 있습니다.

<div class="content-ad"></div>

위 그림은 논문에서 설명하는 내용을 개략적으로 보여줍니다: 두 가지 다른 토크나이저를 사용하면서 교차 모달리티를 생성하는 단일 균일한 모델입니다.

# 카멜레온 훈련

## 토큰화

<div class="content-ad"></div>

- 이미지 토크나이저: 팀은 Gafni 등의 연구를 기반으로 이미지 토크나이저를 훈련했습니다. 이 토크나이저는 크기가 512x512인 이미지를 1024개의 이산 토큰으로 변환합니다. 이미지 토큰 세트에서 8192개의 이미지 토큰 중에서 선택됩니다. 또한, 사전훈련 중 얼굴이 포함된 이미지 수를 2배로 증가시켰습니다. 이 모델의 약점은 이미지 내에 텍스트가 포함된 이미지를 재구성할 수 없으므로 OCR 관련 작업에는 효과적이지 않습니다.

- 텍스트 토크나이저: 텍스트 토크나이저의 경우 팀은 훈련 데이터의 일부를 사용하여 BPE 토크나이저를 훈련했습니다. 어휘 크기는 65,536이며 이미지 토큰 8192를 포함합니다. 이 훈련은 SentencePiece 라이브러리를 사용하여 진행되었습니다.

## 사전훈련 데이터

이제 토크나이저를 준비했으니 데이터를 준비하고 모델을 사전훈련해야 합니다. 사전훈련은 두 단계로 나뉩니다.

전체 사전훈련의 80%를 차지하는 단계 1에서는 다음과 같은 데이터 조합을 사용합니다:

<div class="content-ad"></div>

- 텍스트 전용 데이터는 LLaMa-2 및 CodeLLaMa의 교육용으로 사용되며 2.9 조 토큰의 텍스트 전용 데이터로 구성되어 있습니다.
- 텍스트-이미지 데이터는 공개 데이터 소스와 라이센스가 있는 데이터의 조합입니다. 이미지는 크기를 조절하고 가운데를 자르는 512x512로 준비되어 토큰화됩니다. 이로써 1.5 조 개의 텍스트-이미지 토큰이 생성됩니다.
- 텍스트-이미지 교차 데이터도 공개 데이터 소스로부터 수집되어 이미지에 적용한 것과 동일한 방법으로 4000 억 개의 토큰이 생성되었습니다. 아래 논문에 따라 데이터가 생성되었습니다.

텍스트-이미지 데이터는 텍스트와 이미지의 쌍이지만, 텍스트-이미지 교차 데이터는 아래 사진처럼 텍스트와 이미지가 교차되어 있습니다.

![image](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_2.png)

Stage2에서는 Stage1 교육 데이터의 가중치가 50% 낮추어지고, 동일한 비율의 텍스트 이미지 토큰으로 더 높은 품질의 데이터로 추가 사전 훈련됩니다.

<div class="content-ad"></div>

이 단계에는 인스트럭션 튜닝 데이터셋에서 선별된 부분이 포함되어 있어요.

## 카멜레온 모델 구조와 훈련 안정성

모델 구조

카멜레온 모델 구조는 LLaMa-2와 동일해요. 이에 대해 이전 기사인 여기와 여기에서 읽을 수 있어요.

<div class="content-ad"></div>

정규화를 위해 RMSNorm을 사용하며 활성화 함수로 SwiGLU를 사용합니다. 이에 대해 자세히 설명하겠습니다. 위치 임베딩에는 로터리 위치 임베딩을 사용합니다. 이에 대해 자세히 읽을 수 있습니다.

탈선의 원인과 해결책

모달리티를 추가할 때 LLaMa 아키텍처는 훈련의 중후반 단계에서 탈선을 시작합니다. 텍스트와 이미지 모달리티에 동일한 모델 가중치가 사용되기 때문에 서로 경쟁하며 norms를 증가시킵니다.

더 구체적으로는 소프트맥스 함수가 변환 불변성 속성을 갖고 있기 때문에 (소프트맥스(z) = 소프트맥스(z + c)), 신호가 압축됩니다.

<div class="content-ad"></div>

이 문제를 완화하기 위해, 소프트맥스 벡터인 키와 쿼리의 입력에 레이어 정규화가 적용됩니다. 게다가 드롭아웃 레이어도 제거되었습니다.

이러한 변화로 Chameleon 7B를 안정화시킬 수 있었지만, 34B로 더 확장할 때에는 모델이 여전히 발산합니다. 이는 피드포워드 네트워크가 성장하기 때문입니다. 스위글루 활성화와 곱셈을 사용하기 때문에 값이 계속 성장합니다.

이를 해결하기 위해 팀은 정규화와 피드포워드 블록의 순서를 변경하여 정규화가 이후에 오도록 제안합니다.

![Chameleon Metas Mixed Modal Foundation Model](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_3.png)

<div class="content-ad"></div>

Both the Chameleon 7B and 34B models can be effectively trained by applying normalization reordering and the QK norm technique. It's important to note that these models perform well without the need for dropout layers.

### Optimization Parameters

- **Optimizer:** AdamW
- **β1:** 0.9
- **β2:** 0.95
- **ϵ:** 10^-5
- **Warm-up Steps:** 4000
- **Learning Rate Schedule:** Exponential decay to 0
- **Weight Decay:** 0.1
- **Gradient Clipping Threshold:** 1.0

While the QK norm technique effectively deals with the inner softmax problem in the transformer, it doesn't address the logit shift in the final softmax operation. To handle this issue, Z-loss regularization is implemented by incorporating 10^-5 log2 Z into the loss function, with Z being the value to be determined.

Let me know if you need further clarification or have any questions!

<div class="content-ad"></div>

이미지를 Markdown 형식으로 변경해주세요.

![image1](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_4.png)

![image2](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_5.png)

아래 표는 모든 교육 하이퍼파라미터 및 LLaMa 모델과의 차이를 요약한 것입니다.

![image3](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_6.png)

<div class="content-ad"></div>

사전 훈련 하드웨어

이 모델들은 Meta의 연구용 슈퍼 클러스터, NVIDIA A100 GPU로 80GB의 메모리를 갖춘 환경에서 훈련되었습니다.

![이미지](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_7.png)

## 추론

<div class="content-ad"></div>

The team has built an inference pipeline tailored to the constraints we're working with. When generating, each token is carefully inspected for its modality to select the appropriate tokenizer for detokenization.

If we aim to generate content of only one specific modality, we apply a masking technique to block out the other modality.

Whether we're in streaming mode or generating large chunks of text, confirming the modality of tokens is crucial. Nonetheless, utilizing token masking can significantly speed up the process by reducing the number of tokens that need evaluation.

## Harmonizing Chameleon

<div class="content-ad"></div>

### 감독된 세밀한 조정을 위한 데이터

팀은 모델을 조정하기 위해 고품질의 다양한 데이터 세트를 활용했습니다.

![Image](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_8.png)

LLaMa-2의 Text SFT 데이터세트와 CodeLLaMa의 CodeSFT 데이터세트를 활용했습니다. 이미지 생성 SFT 데이터는 미학 분류기를 사용하여 생성되었는데, 여기서 64K개의 이미지 중 6점 이상 평가받았으며 종횡비가 512x512에 가까운 이미지들이 유지되었습니다.

<div class="content-ad"></div>

Visual Chat, Image generation, 그리고 Interleaved data에 대해서는, 데이터 수집과 매우 높은 품질을 제공하는 것에 대한 책임이 서드파티 벤더들에게 있었습니다.

Meta의 팀은 안전 데이터 또한 포함하고 있었는데, 이는 모델로부터 유해한 응답을 유도하기 위한 프롬프트로 구성되어 있습니다. 이러한 프롬프트들은 "저는 그것을 도와드릴 수 없어요” 라는 거절 응답과 일치시켰습니다. 이 데이터는 LLaMa-2 교육 데이터로부터의 예시, Pick a Pic으로부터의 이미지 생성 프롬프트, 그리고 내부적으로 수집된 혼합 모달 프롬프트를 포함하고 있습니다.

Finetuning

모달리티 균형 조절은 SFT 중에 중요한데, 그 이유는 모델에게 각 모달리티가 동등하게 중요하다는 것을 가르치기 때문입니다. 그렇지 않으면, 모델은 한 가지 모달리티를 우선시하면서 다른 하나를 무시하는 방식으로 배울 수 있습니다.

<div class="content-ad"></div>

**최적화**
사인 코사인 학습율 스케줄을 사용하여 초기 학습율을 1e-5로 설정하고 가중치 감쇠를 0.1로 설정했습니다. 배치 크기는 128로 설정되었고 최대 4096토큰 시퀀스를 처리합니다.

특별 토큰이 있는데, 이는 프롬프트의 끝과 답변의 시작을 나타냅니다. 각 시퀀스마다 가능한 만큼 많은 프롬프트와 답변 쌍을 활용했습니다.

모델은 자기회귀적인 방식으로 학습되었지만, 프롬프트 토큰의 손실은 가려졌습니다. 왜냐하면 모델이 답변을 학습하도록 하기 때문입니다. 0.05의 드롭아웃과 Z-loss가 적용되었습니다. 이미지에는 패딩이 추가되어 모든 이미지 정보가 포함되었으며, 학습데이터에 대해 augmentation이 적용되었습니다.

**# 평가**

<div class="content-ad"></div>

샤먼은 다중 모달 능력을 갖춘 새로운 모델이기 때문에 성능을 평가할 수 있는 기존의 기준이 많지 않습니다.

## 다중 모달 모델을 평가하는 프롬프트

메타는 제3자 공급 업체와 협력하여 평가 프롬프트를 수집했습니다. 그들은 주석 작업자들에게 다중 모달 모델이 실제 시나리오에서 어떻게 작동할 것으로 기대하는지에 대한 프롬프트를 작성하도록 요청했습니다.

예를 들어 "파스타를 어떻게 요리해야 하나요?" 또는 "섬의 레이아웃을 어떻게 디자인해야 할까요? 몇 가지 예를 보여주세요."와 같은 질문들이 있습니다. 프롬프트는 텍스트만이거나 이미지와 함께 있는 경우도 있지만 기대되는 응답은 다중 모달 형식이어야 합니다.

<div class="content-ad"></div>

세 명의 임의의 주석 달기자가 그 중 혼란스럽거나 복합적인 모달 응답을 기대하지 않는 프롬프트를 걸러 내도록 요청받았습니다.

최종 평가 세트에는 1,048개의 프롬프트가 포함되어 있습니다. 이 중 441개(42.1%)는 복합 모달(즉, 텍스트와 이미지를 모두 포함)이며 나머지 607개(57.9%)는 텍스트만 포함됩니다.

## 시각으로 모델 보강하기

Chameleon을 다른 모델과 비교하기 위해 팀은 Gemini Pro 및 GPT-4V에 시각을 보강합니다. 그들은 모델을 지시하기 위해 API를 호출하고 프롬프트 끝에 다음 구문을 추가합니다: "만약 질문에 이미지 생성이 필요하다면, 이미지 캡션을 생성하고 <caption> </caption> 태그 쌍으로 캡션을 둘러싸세요."

<div class="content-ad"></div>

OpenAI DALL-E 3은 이후 이 캡션을 조건으로 한 이미지를 생성하고, 그 생성된 이미지로 원래 응답의 캡션을 대체합니다.

![Image](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_9.png)

## 절대 및 상대 평가

수집된 프롬프트에 대해 두 가지 유형의 평가가 진행되었습니다.

<div class="content-ad"></div>

- 절대평가(Absolute Evaluation)는 주석 해석자들이 모델의 응답을 설명된 작업을 완전히 충족하는지, 일부 충족하는지, 또는 충족하지 않는지 선택하여 개별적으로 평가하는 것을 의미합니다.
- 상대평가(Relative Evaluation)는 주석 해석자들이 두 가지 서로 다른 모델의 두 응답을 동시에 비교하고, 어떤 응답이 더 나은지 또는 두 응답이 동일한지를 선택하는 것을 의미합니다.

![이미지](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_10.png)

위 그림에서 볼 수 있듯이, 주석 해석자들의 평가에 따르면 Chameleon은 절대평가와 상대평가에서 승리합니다. 해석자들은 Chameleon이 다른 어떤 모델보다도 필요한 작업을 완전히 수행한다는 데 동의합니다. 또한 대부분의 경우에 Chameleon의 응답을 다른 모델의 것보다 선호합니다.

하지만 이러한 결과를 사과와 사과를 비교하는 것으로 생각하지는 마세요. 다른 모델들은 이미지를 생성하는 것이 아니라 이미지 생성 모델로 보강된 상태입니다.

<div class="content-ad"></div>

내가 말하고 싶은 것은 어노테이터 편향이 존재할 수 있다는 것입니다. 왜냐하면 카멜레온의 SFT 데이터를 위한 동일한 제3자 공급 업체가 데이터를 주석 처리했으며, 평가 프롬프트를 생성한 사람들이기도 하기 때문입니다. 만약 여러분이 데이터를 직접 작성했다면, 카멜레온의 응답을 다른 것들보다 선호할 것으로 예상됩니다.

## 벤치마크 평가

팀은 기존 벤치마크에 대한 추가 평가를 실시하여 양쪽 모드가 모델 성능에 도움이 되는지 아니면 방해가 되는지 확인했습니다.

텍스트만 있는 평가

<div class="content-ad"></div>

위의 표를 통해 우리는 카멜레온이 일반적으로 LLaMa-2보다 우월하며 미스트럴 8x7B와 경쟁력이 있다는 것을 알 수 있습니다. 팀은 이러한 결과를 훈련 데이터에 대한 더 많은 epoch, 코드 데이터를 포함한 것으로 귀속하고, 그것이 추론을 향상시켰다고 설명했습니다. 마지막으로, 훈련 과정의 20%를 의미하는 스테이지 2의 데이터 품질이 높아져서 발생한 것입니다.

그러나 이미지 모달리티의 영향에 대한 언급은 없었습니다. 이미지 모달리티를 추가함으로써 텍스트 전용 작업에서 모델이 퇴화하지 않았다는 것을 적어도 말할 수 있습니다.

이미지를 텍스트로 평가

<div class="content-ad"></div>

![Chameleon Model](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_12.png)

안녕하세요! 오늘은 Chameleon 모델에 대해 이야기해 보겠습니다.

일반적으로 Chameleon은 더 큰 모델들(34B vs 80B)과 경쟁하는데 성공하며, 훨씬 적은 예제만 필요합니다. Flamingo와 IDEFICS는 32개의 예제로 테스트되었지만 Chameleon은 단 2개의 예제로 테스트되었습니다.

그러나 Chameleon은 VQAv2에서 GPT-4V와 Gemini보다 성능이 떨어집니다. 이는 더 좋은 전용 데이터로 세밀하게 조정되었기 때문일 수도 있습니다.

Chameleon에 대한 결론과 교훈에 대해 더 알아보도록 하겠습니다. 함께 해주셔서 감사합니다!

<div class="content-ad"></div>

- 메타팀은 7B 및 34B 패밀리 모델에서 카멜레온을 소개했습니다. "모든 토큰" 방법을 채택하고 텍스트 및 이미지 토크나이저를 사용함으로써 다양한 방식과의 원활한 통합이 가능해졌습니다.
- 카멜레온은 여러 가지 방식을 함께 학습하여 모달리티 균형, 안정성, 및 확장 문제와 같은 새로운 도전 과제를 도입하였습니다.
- QV 정규화, 드롭아웃 제거, Z-loss 사용, 및 정규화 재배치는 카멜레온 34B의 안정적인 훈련을 위해 필요합니다. 카멜레온은 다중 모달 응답 생성에서 다른 모델보다 더 나은 성능을 보입니다. 그러나 이는 사과와 사과를 비교하는 것이 아니며, 다른 모델들은 API를 통해 교배된 결과이고 시각 모델이 증가되었습니다. 또한, 동일한 제 3자 업체 업체가 지시 튜닝 데이터 및 평가를 생성한 것으로 파 인되어 있습니다.
- 이미지 모달리티를 추가하는 것은 카멜레온의 다른 모달리티에 이익을 주거나 해치지는 않습니다.
- 저는 7B에서 34B로 스케일을 조정하는 과정에서 확인된 안정성 문제들이 다시 나타나지 않고 카멜레온이 더 나은 성과를 낼 수 있는지 궁금해합니다.

이 기사를 즐겨 보셨다면 박수를 치시고 팔로우하고 댓글을 남겨주세요!

LinkedIn의 Aziz Belaweid나 GitHub을 통해 연락을 유지해주세요.

![ChameleonMetasMixed-ModalFoundationModel](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_13.png)

<div class="content-ad"></div>

This story is featured on Generative AI. Connect with us on LinkedIn and follow Zeniteq to keep up with the latest AI stories.

Subscribe to our newsletter to stay informed about the latest news and updates on generative AI. Let's shape the future of AI together!

![ChameleonMetasMixed-ModalFoundationModel_14](/assets/img/2024-07-13-ChameleonMetasMixed-ModalFoundationModel_14.png)