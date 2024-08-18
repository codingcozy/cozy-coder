---
title: "ChatGPT는 얼마나 많이 알고 있을까 LLM의 지식 수준 공개"
description: ""
coverImage: "/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_0.png"
date: 2024-07-14 01:44
ogImage:
  url: /assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_0.png
tag: Tech
originalTitle: "You Know Nothing, ChatGPT. How Much Does Your LLM Know?"
link: "https://medium.com/gitconnected/you-know-nothing-chatgpt-how-much-does-your-llm-know-03ce79d4b925"
isUpdated: true
---

## | LLM | 지식 | 스케일링 법칙 |

![image](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_0.png)

패턴 지식이란 무엇인가요? 그것을 어떻게 측정할 수 있으며, 어떤 요소들이 영향을 미치나요? 데이터셋인가 아니면 아키텍처인가요? 얼마나 많은 모델이 배우는지 조절하는 수학적 법칙을 정의할 수 있을까요? 모든 인간 지식을 보존하는 모델을 갖을 수 있을까요?

## 이러한 난해한 질문들에 대해 시도해봅시다.

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

# LLM의 확장

![YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_1.png](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_1.png)

확장 법칙은 LLM의 가장 흥미로운 동시에 논란이 되는 주제 중 하나입니다. 이 이론에 따르면 매개변수의 수가 증가함에 따라 모델을 훈련하는 데 사용되는 데이터는 손실을 비례적으로 줄입니다. 특정 임계값 이상의 매개변수로는 "마법 같이" 예상치 못한 특성이 나타납니다.

OpenAI의 저자들은 이 멱법칙을 정의했는데, 이는 성능을 예측하는 데 아키텍처보다 세 가지 요소가 더 중요하다는 것입니다. 또한 이러한 매개변수를 가지고 성능을 예측할 수 있습니다. 따라서 이론적으로 모델의 성능을 미리 예측할 수 있으며, 원하는 성능을 얻기 위해 모델과 필요한 데이터셋의 크기를 계산할 수 있습니다.

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

According to the latest research findings, it's possible to derive scaling laws for various aspects of Large Language Models (LLMs). As an example, we can establish a scaling law for determining the amount of data required for fine-tuning an LLM. If we have an LLM originally trained on natural language text and wish to enhance its programming abilities, we can calculate the necessary data volume.

Furthermore, OpenAI has shown that this scaling law isn't confined to just the realm of Natural Language Processing (NLP) but extends to other data modalities like images, videos, and even multimodal models. This principle also holds true for enhancing other model capabilities such as mathematical reasoning.

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

---

![image](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_4.png)

첫 번째 스케일링 법칙은 매개변수의 수의 중요성에 특별한 강조를 두었지만, DeepMind의 스케일링 법칙은 대신 완전하고 품질 좋은 데이터셋의 중요성을 강조합니다.

![image](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_5.png)

## 이러한 스케일링 법칙들은 나중에 그들의 진실성을 의심하는 기사에 의해 도전받았습니다. 예를 들어, 이 기사는 데이터의 양 뿐만 아니라 구성도 중요하다고 말합니다:

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

게다가 파이는 모델이 아주 작고 훨씬 적은 데이터로 훈련되었더라도 이러한 스케일링 법칙에 의해 예측된 행동이 더 큰 모델과 유사하다는 것을 보여줍니다:

![Phi](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_7.png)

다시 말해, 이러한 스케일링 주장은 매개변수의 수를 충분히 조정하면 기억, 일반화, 추론 기술 등이 발생한다고 주장합니다. 실제로 이러한 스케일링 법칙은 정확하지 않으며 실제 실험에서는 쓸모가 없습니다.

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

**지식의 확장 법칙**

![2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_8.png](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_8.png)

안녕하세요! 최근 META가 발표한 연구에 따르면, 매우 정확하게 정의되고 상세히 설명된 확장 법칙을 구체화하려고 했습니다. 저자들은 지식 저장 용량으로 시작하기로 결정했습니다. 이는 GPT-4 모델이 지식으로 유명하지만 이와 교육 또는 매개변수와의 관련이 명확하지 않기 때문입니다. 마찬가지로, 트랜스포머 자체나 특정 구성 요소와의 관련성이 어떻게 되는지 알려지지 않았습니다. 또한, 1조개의 매개변수를 넘는 모델을 확장시키고 있지만, 이것이 전체 인류 지식을 저장하는 데 충분한지 여부는 알 수 없습니다.

첫 번째 포인트는 무엇이 지식인가요? 명백히 이것은 어려운 질문입니다. 저자들은 "하나의 인간적 지식"을 정의하기 위한 간단한 개념에 주목합니다. 즉, 이름, 속성 및 값(예: Anya Forger, 생일, 1996년 10월 2일과 같은)으로 구성된 구조라고 정의했습니다. 그런 다음, 이러한 지식 조각들이 영어 설명에 포함되어 있는 합성 데이터셋을 구축했습니다. 이는 그들이 당시 다양한 언어 모델(GPT-2, LLaMA, Mistral 등)을 표준 자기회귀 학습을 사용하여 제로부터 훈련하기로 결정했기 때문입니다.

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

이것은 지식 스케일링 법칙을 이해하는 데 기준이 되는 이상적인 데이터셋입니다. 게다가, 지식은 저장으로 정의되지 않고 이 지식을 하류 작업에서 회상하고 사용하는 능력으로 정의됩니다 (“Anya Forger의 생일은 언제인가?”). 저자들에게 지식은 기준에서의 성능과 분리되어 있습니다. 한 모델이 다른 모델보다 30% 더 나은 성능을 보인다고 해서 이 모델이 30% 더 많은 지식을 갖고 있는 것은 아닙니다.

![이미지](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_9.png)

저자들은 N개의 지식 조각에 대한 bit 복잡성을 정의하고 있습니다. 100M 파라미터를 가진 모델이 220M 비트의 지식을 저장한다면, 이 모델의 용량 비율은 2.2입니다. 우리가 살펴볼 이 간단한 설정은 매우 흥미로운 결과를 만들어낼 것입니다.

저자들은 다양한 모델을 분석했습니다:

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

- GPT2의 회전 위치 임베딩(현대적인 변형)과 동일한 구조에 대한 다양한 레이어 및 헤드의 조합을 사용했습니다.
- LLaMA 아키텍처.
- Mistral 아키텍처.

이 글은 결과와 이론으로 가득 차 있고, 여러분을 읽으러 초대합니다. 여기서는 주요 결과에 대해 논의하겠습니다.

첫 번째 결과는 GPT2가 레이어 모델 크기, 깊이, 넓이, 데이터 크기, 유형(합성/반합성) 및 하이퍼파라미터의 다양한 조합에서 2비트/파라미터 용량 비율에 도달한다는 것입니다. 이는 충분한 학습으로 모델을 훈련시킬 경우 제공됩니다. 저자들은 모델을 동일한 정보 조각에 1000회 노출시켰습니다.

![image](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_10.png)

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

이 결과는 7B 모델이 14B 비트의 지식을 저장할 수 있다는 것을 의미합니다. 이는 영어 위키피디아와 교과서의 조합에서 찾을 수 있는 양보다 더 많은 양입니다. 또한, 이러한 지식은 후속 작업에서 활용될 수 있습니다.

또 다른 흥미로운 결과는 2비트/파라미터 용량을 달성하기 위해서는 각 지식 조각이 훈련 중에 1000번씩 방문되어야 한다는 것입니다. 이는 훈련 중에 이 정보가 1000번 보여져야 한다는 것을 의미합니다. 실제로, 100번 노출시 GPT-2의 용량 비율은 1비트/파라미터로 떨어집니다. 이는 희귀한 지식일수록 모델이 그 정보를 저장하는 데 어려움을 겪는다는 것을 의미합니다.

이에 저자들은 LLaMA, Mistral 및 GPT-2 아키텍처를 줄인 또는 아예 MLP 레이어가 없는 상태로 테스트했습니다. 이제 모델들 간에는 다음과 같은 차이가 있습니다:

- LLaMA/Mistral은 가중치를 연결하지 않으며 SiLU 활성화 함수를 사용합니다(GPT-2는 대신 GeLU를 사용) 그리고 GatedMLP 레이어를 사용합니다.
- Mistral은 더 큰 MLP 레이어를 가지고 있으며 더 빠른 추론을 위해 그룹-쿼리 어텐션을 사용합니다.
- 저자들은 GPT-2의 원래 임베딩을 변경하고 드롭아웃을 제거했습니다.

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

결과가 흥미로운 것으로 나타났어요:

- 저자들에게는 GPT2의 결과가 LLaMA와 Mistral과 비슷하다고 해요.
- 그러나 그 반대로, Mistral과 LLaMA는 묶인 가중치에 대한 낮은 규칙에서 낮은 결과를 보이네요.
- MLP 레이어 크기를 줄이거나 GPT2 구조에서 직접 제거함으로써 모델은 여전히 성능을 유지해요. 그러므로 널리 알려진 것과는 달리 Attention 레이어도 지식을 저장하는 데 능숙해요.

![그림](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_11.png)

역설적으로, 100회 노출로 볼 때 LLaMA/Mistral의 용량 비율은 GPT2의 것보다 1.3배 낮아요. 노력해봐도, 저자들은 GPT2를 따라가지 못했어요. 희귀한 데이터로는 MLPs를 완전히 제거하는 것이 비효율적이에요.

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

여기에 추가적으로, 게이트드 MLP에서 표준 MLP로 전환함으로써 모델의 지식 저장 능력이 향상됩니다. 마찬가지로, 작은 모델의 경우 GPT2Tokenizer가 더 나은 선택입니다.

![이미지](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_12.png)

저자들은 양자화의 영향에 대해 궁금해했습니다. 16비트 부동 소수점 모델부터 시작하여 int8에서 양자화하는 경우, 이들의 용량에는 무시할 만한 영향이 있습니다. 그러나 더 높은 int4 양자화는 용량을 2배 이상 줄입니다.

![이미지](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_13.png)

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

이는 모델의 양자화 또는 가지치기를 수행할 때 고려되지 않는 흥미로운 측면입니다. 성능에 중요해 보이는 일부 레이어가 실제로 정보 저장에 중요할 수 있습니다.

그 다음 저자들은 Mixture of Experts (MoE) 구조(자체 내에서 희소성을 통합한 모델)에 대해 다룹니다. 이러한 모델은 밀집 구성요소보다 추론 속도가 훨씬 빠르지만 성능이 낮을 수 있습니다.

저자들에 따르면 MoE는 지식을 저장하는 데에 약간 덜 효과적입니다. 따라서 GPT2-MoE 모델은 32개 전문가를 사용하여 매개변수의 지식을 효과적으로 활용합니다.

![이미지](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_14.png)

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

최종적으로, 저자들은 저품질 데이터의 존재가 스케일링 법칙에 어떤 영향을 미치는지에 대해 논의합니다. 예를 들어, 위키피디아는 지식의 훌륭한 출처이지만, 데이터 수집 시 이 지식은 훨씬 희석되어 있습니다 (인터넷의 많은 부분은 "쓰레기"입니다).

그 후 저자들은 "유용한"과 "쓰레기" 데이터의 다양한 혼합물을 다른 노출 설정(모델이 정보를 몇 번이나 볼 수 있는지)으로 시험하기로 결정합니다.

결과는 모델의 용량 비율이 쓰레기 데이터의 백분율에 영향을 받는다는 것을 보여줍니다. "유용한 대 쓰레기" 학습 토큰의 1:7 비율은 모델 용량이 최대 20배 감소하게 만듭니다. 저자들에 따르면, 지식이 풍부한 데이터가 더 많은 경우, 유용한 토큰이 어디에 있는지 신고하는 특별한 토큰을 추가함으로써 모델 용량을 향상시킬 수 있습니다.

![이미지](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLMKnow_15.png)

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

# 마무리 생각

![이미지](/assets/img/2024-07-14-YouKnowNothingChatGPTHowMuchDoesYourLLTKnow_16.png)

스케일링 법칙은 오랫동안 관심사로 여겨져 왔으며, 작은 모델을 사용하여 훨씬 큰 모델을 훈련하기 전에 매개변수를 계산할 수 있다는 희망이 있습니다. 그러나 이러한 스케일링 법칙의 문제는 복잡하며 이론적 결과가 실무와 다를 수 있다는 것입니다.

이 연구의 가치는 특정 케이스, 즉 지식 저장에 대한 LLMs의 행동을 자세히 연구한 것입니다.

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

이 연구는 다양한 하이퍼파라미터와 아키텍처 선택의 영향을 이해하는 데 도움이 됩니다. 더불어 양자화, 레이어 중복 및 쓸모없는 데이터의 영향을 조사합니다. 다시 말해서, 이 논문의 결과는 모델을 훈련하려는 사람들이나 사전 훈련된 모델에 양자화를 적용하려는 사람들에게 매우 유용합니다.

META가 가진 이러한 야심찬 프로그램 같은데요, 앞으로 이와 유사한 연구들을 더 많이 볼 것으로 예상됩니다.

## 어떻게 생각하시나요? 댓글로 의견 나눠주세요

# 흥미로운 내용이었다면:

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

다른 기사들도 참고하실 수 있어요. LinkedIn에서 저와 연결하거나 연락할 수도 있어요. 매주 업데이트되는 머신러닝 및 인공지능 뉴스가 담긴 저장소를 확인해보세요. 협업과 프로젝트에 대해 열려있고, LinkedIn에서 저에게 연락할 수도 있어요. 새로운 이야기를 게시할 때 알림을 받으려면 무료로 구독할 수도 있어요.

여기가 제 GitHub 저장소 링크에요. 머신러닝, 인공지능 및 관련 자료들을 수집하고 있는 곳이에요.

혹시 최근 기사 중 하나에 관심이 있을지도 몰라요:

# 참고

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

여기 이 글을 쓰기 위해 참고한 주요 참고 자료 목록입니다. 각 논문의 제목은 따로 언급하지 않았어요.

- Alabdulmohsin, 2022, 언어와 비전에서 신경 척도 법 다시 살펴보기, [링크]
- Henighan, 2020, 자기회귀 생성 모델링을 위한 척도 법, [링크]
- Hoffman, 2022, 계산 최적의 대규모 언어 모델 훈련, [링크]
- Kaplan, 2020, 신경 언어 모델을 위한 척도 법, [링크]
- Hernandez, 2021, 이전에 대한 척도 법, [링크]
- Muennighoff, 2023, 데이터 제약 언어 모델 척도 법, [링크]
- Gunasekar, 2023, 교과서만 있으면 충분해, [링크]
- Allen-Zhu, 2024, 언어 모델의 물리: 파트 3.3, 지식 용량 척도 법, [링크]
