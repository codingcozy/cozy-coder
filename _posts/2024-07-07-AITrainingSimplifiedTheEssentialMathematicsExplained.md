---
title: "AI 훈련 간단 요약 필수 수학 개념 설명"
description: ""
coverImage: "/assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_0.png"
date: 2024-07-07 14:06
ogImage:
  url: /assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_0.png
tag: Tech
originalTitle: "AI Training Simplified: The Essential Mathematics Explained"
link: "https://medium.com/towards-data-science/ai-training-simplified-the-essential-mathematics-explained-3a94ebeb4a3e"
isUpdated: true
---

**이미지**
![링크](/assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_0.png)

안녕하세요! Tarot 전문가 여러분!

모든 것이 어떻게 작동하는지 이해하는 것은 항상 유익합니다. 이 글에서는 AI 모델 학습에 사용되는 기본 수학 논리에 대한 매우 간단한 개요를 제공하겠습니다. 기본 교육을 받았다면 다음 예제가 이해 가능할 것이고 인공 지능 분야에 대해 조금 더 잘 이해하게 될 것을 약속드립니다.

# 영업 예측을 위한 AI 생성

우리 회사의 매출액을 예측하는 새로운 AI 모델을 만들고 싶다고 가정해 봅시다. 지난 두 달의 매출액, 광고 비용 및 제품 가격 데이터가 있습니다.

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

![Image](/assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_1.png)

즉, 우리는 우리 제품의 가격과 광고 비용이 어떻게 영향을 미치는지 알려주는 모델을 만들고 싶어합니다. 이러한 도구를 사용하면 마케팅 전문가는 예를 들어 광고에 50유로를 쓰고 제품 가격을 6유로로 설정했을 때 예상 매출을 계산할 수 있습니다.

# 수학식으로서의 AI

본질적으로 AI는 수학식(또는 수식 모음)에 불과합니다. 우리의 매출 예측 예시는 다음과 같이 수학식으로 나타낼 수 있습니다:

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

이미지를 아래와 같은 형식으로 변경해주세요.

![AITrainingSimplifiedTheEssentialMathematicsExplained](/assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_2.png)

# 학습 시작

우리가 AI를 학습시작할 때, 모델의 파라미터에 임의의 값을 할당할 수 있습니다. 예를 들어, 광고 비용 파라미터를 2로, 제품 가격 파라미터를 -2로 초기 설정할 수 있습니다.

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

안녕하세요! 이렇게 하면 모델이 너무 낙관적이라는 것을 알 수 있습니다. 광고 비용과 제품 가격을 각각 매개변수 값으로 곱하면 첫 번째 달에는 실제 매출이 €5이지만 모델은 €30을 예측했습니다. 두 번째 달에는 실제 매출이 €18이었고 모델은 €52를 예측했습니다.

# 학습 규칙

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

만약 오차가 0이면, 모델이 완벽하며 조정이 필요하지 않습니다.

만약 오차가 양수인 경우, 모델이 지나치게 낙관적인 결과를 제공했을 수 있습니다:

- 해당 입력 특성(예: 광고 비용 또는 제품 가격)이 양수인 경우, 가중치(매개변수)를 줄입니다.
- 해당 입력 특성이 음수인 경우, 가중치를 증가시킵니다.

만약 오차가 음수인 경우, 모델이 지나치게 비관적이었을 수 있습니다:

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

- 대응하는 입력 특성이 양의 값이면 가중치(매개변수)를 증가시킵니다.
- 대응하는 입력 특성이 음의 값을 가지면 가중치(매개변수)를 감소시킵니다.

학습 규칙을 따르면 광고 비용과 제품 가격이 모두 양의 값이기 때문에 두 매개변수를 감소해야 합니다. 예를 들어, 광고 비용 가중치를 2에서 1로, 제품 가격 매개변수를 -2에서 -3으로 줄입니다.

![AITraining](/assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_5.png)

재계산하면 우리의 모델이 이제 정확하게 예측한다는 것을 확인할 수 있습니다. 첫 번째 수동으로 훈련된 AI 모델이 준비되었습니다. 👍

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

![Image](/assets/img/2024-07-07-AITrainingSimplifiedTheEssentialMathematicsExplained_6.png)

# 훈련에 사용되지 않은 데이터로 모델 테스트하기

만약 위 모델이 너무 완벽해 보인다면, 당신이 옳습니다. 우리 모델은 훈련 데이터에서 완벽하게 작동했습니다. 모델의 정확도를 평가하기 위해, 훈련 과정에 사용되지 않은 데이터에서 테스트해야 합니다.

저희는 1월과 2월 데이터로 모델을 훈련했습니다. 이제 모델이 3월과 4월의 매출액을 얼마나 정확하게 예측하는지 확인해 봅시다.

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

위의 표를 보면, 모델은 3월의 매출 수익을 €28(실제 €24)로, 4월의 매출을 €21(실제 €18)로 예측합니다. 우리 모델은 새로운 데이터에 대해 평균적으로 €3.5의 오차를 만들어내며, 이것을 우리 모델의 정확도라고 할 수 있습니다.

**결론**

요약하자면, 인공지능은 근본적으로 수학적 공식입니다. 우리 예시에서는 공식에 두 개의 매개변수가 있었지만, GPT-4 모델은 1조(1조 = 1,000,000,000,000)개 이상의 매개변수를 가지고 있습니다. 두 경우 모두 동일한 원리로 훈련되어, 모델의 매개변수를 조정하여 오차를 줄이는 방향으로 작동합니다.

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

AI가 학습 데이터를 기반으로 학습하지만, 정확성은 훈련 중에 사용되지 않은 데이터(테스트 데이터)를 사용하여 평가해야 한다는 것을 기억하는 것이 중요합니다.
