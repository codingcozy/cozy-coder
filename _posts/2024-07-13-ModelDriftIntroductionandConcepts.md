---
title: "모델 드리프트 입문 및 개념 이해하기"
description: ""
coverImage: "/assets/img/2024-07-13-ModelDriftIntroductionandConcepts_0.png"
date: 2024-07-13 02:46
ogImage:
  url: /assets/img/2024-07-13-ModelDriftIntroductionandConcepts_0.png
tag: Tech
originalTitle: "Model Drift Introduction and Concepts"
link: "https://medium.com/towards-data-science/model-drift-introduction-and-concepts-e32c5305da2a"
isUpdated: true
---

그 카드 그림을 Markdown 형식으로 변경해주세요.

세금, 죽음, 그리고 모델 드리프트는 삶에서 유일한 세 가지 확실한 것이에요. 광고 속에 마지막 부분은 제가 조금 덧붙인 거지만, 모든 모델이 퇴화되는 것은 사실이에요.

머신 러닝 모델을 개발한 후에는 언제나 같은 패턴이 반복될 거예요:

- 모델은 개발 단계에서 테스트 세트에서 예상 성능을 보여줘요.
- 모델은 운영 환경으로 들어간 후에 다르게 동작해요 (일반적으로 좀 더 나빠져요).
- 시간이 지남에 따라 모델의 성능이 저하되어요.

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

몇 년이 지난 후에는 모델의 성능이 처음에 개발했을 때보다 훨씬 나쁠 가능성이 높습니다. 이는 다양한 이유로 발생할 수 있지만, 근본적인 원인은 세상이 변화하기 때문입니다.

세상이 변화할 때, 현실 정보를 표현하는 데 사용하는 데이터도 변화합니다. 기저 데이터 분포가 변동되면 기계 학습 모델이 학습하고 수행하는 방식에 반드시 영향을 미칠 것입니다.

이 블로그 포스트에서는 세상의 기본적인 변화가 모델에 어떻게 영향을 미치는지 예시를 살펴보겠습니다. 이러한 예시를 이해하면 데이터 주도 조직의 성공을 정의하는 머신 러닝 운영(MLOps)의 중요성을 개발 계획을 수립하고 조직 리더십에 설명하는 데 더 잘 준비할 수 있을 것입니다.

# 데이터 분포 변화

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

Data distribution shift은 모델 드리프트의 가장 흔한 원인입니다.

개발 당시와 비교했을 때 변수 중 하나의 분포가 크게 변하는 경우에 발생합니다. 사용자의 나이를 기준으로 앱 이탈을 예측하는 모델이 있다고 가정해 봅시다.

![image](/assets/img/2024-07-13-ModelDriftIntroductionandConcepts_1.png)

앱 이탈이란 고객이 서비스 이용을 중단하기로 결정하는 행위를 말합니다.

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

뭐야, 친구! 시간 t에 모델을 개발했던 기억이 나? 이제 한 해 후에 사용자 그룹의 연령 분포가 아래처럼 변할 거라고 상상해 봐봐:

![Model Drift](/assets/img/2024-07-13-ModelDriftIntroductionandConcepts_2.png)

사용자 그룹은 이제 시간 t 때보다 훨씬 나이가 많아졌어. 이건 당신의 알고리즘이 성능이 나빠지게 만든단 말이야. 당신의 모델은 이제 더 이상 현재 현실을 반영하지 않는 특정 데이터 분포에 적응돼 있어. 이 변화가 너무 크면, 당신이 학습할 때 매우 적은 샘플 크기로 학습한 예측을 하게 될 수도 있어.

이 효과는 특히 feature 분포에 높이 의존하는 모델을 사용할 때 심화돼. (예를 들어, 트리 기반 모델은 가지 자르기를 하기 위해 분포 샘플 크기를 사용해.)

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

이제 수백 가지의 특성을 갖는 모델들을 고려해 보세요. 각 특성이 분포 변화로 인해 영향을 받을 것입니다. 이는 여러분의 모델에 엄청난 영향을 미칠 수 있어요.

데이터 분포 변화를 감지하는 것은 비교적 쉬워요:

- 훈련 시점의 분포 매개변수를 저장해요: 평균, 중위수, 표준 편차, IQR, 첨도(kurtosis) 등.
- 예측하려는 새로운 데이터에 대해 이러한 값들을 비교해 보세요. 이 값들이 서로 얼마나 벗어나는지에 따라 영향도가 달라져요.
- 중심 경향 및 분포 측정치를 동시에 비교해야 한다는 것을 잊지 마세요. 두 분포가 동일한 평균이나 중위수를 갖고 있을지라도 완전히 다른 모양을 가질 수 있어요.

MLFlow나 Azure Machine Learning Studio와 같은 모니터링 도구를 사용하면 특성 데이터 분포를 추적할 수 있어요. 이러한 도구를 사용하면 특정 분포 변화를 기반으로 모델을 자동으로 재학습하기도 매우 편해요.

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

# 엣지 케이스

머신 러닝 모델이 제품 생산에 영향을 미치는 일반적인 상황 중 하나는 엣지 케이스 문제입니다. 예를 들어, 특정 제품의 매출을 예측 중이라고 상상해 봅시다. 이상적으로는 제품 매출에 영향을 미치는 요소가 일정하게 유지되는 것이 좋겠죠.

하지만 엣지 케이스가 갑자기 발생하여 전체 모델을 파괴할 수 있습니다. 매출에 비정상적으로 낮거나 높은 값이 포함되면 예측이 잘못될 수 있습니다. 이는 COVID-19 팬데믹 중에 발생한 일이었습니다. 엣지 케이스가 주류가 되며 대부분의 머신 러닝 모델이 쓸모 없어졌습니다.

이러한 엣지 케이스 상황은 매우 명백하지만, 때로는 엣지 케이스를 식별하기가 더 어려울 수 있습니다. 일반적으로 우리가 다루는 기본 현상에 대한 심층적인 이해력이 필요합니다. 그럼에도 불구하고 할 수 있는 몇 가지 조치가 있습니다:

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

- 분포를 확인하는 것 외에도 변수에서 명백한 이상점을 확인하세요. 우리가 보통 유일한 차원의 가장자리 경우라고 부르는 것이에요.
- 또한, 변수와 특징들의 조합을 살펴보세요. 이들이 함께 볼 때 가장자리 걸림돌이 될 수 있어요. 보통 우리는 이를 다중 차원의 가장자리 경우라고 부르죠.

그러나 이상점을 보는 것이 가장자리 경우를 발견하는 유일한 방법일 필요는 없어요. 식별하기 어려운 "어려운" 가장자리 경우는 일반적으로 이상점이 아니며, 기밀 데이터의 맥락을 이해하는 비즈니스 사용자들의 도움으로 파악해야 해요.

# 피드백 루프

특정 시나리오에서는 모델이 다른 모델의 기능이나 심지어 자체 기능에 영향을 미칠 수 있어요, 시계열 모델의 경우처럼요.

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

일반적으로 모델 출력이 특징으로 작동하는 경우, 이는 어느 정도의 피드백 루프를 생성할 것입니다. 모델의 출력 값을 확인하고 분포를 계속 추적한다면 피드백 루프 발생 가능성을 이해하는 데 충분할 것입니다.

특히 모델에서 생성된 특징들이 시간이 지남에 따라 영향력이 변하는지 이해하기 위해 SHAP 값과 같은 설명 가능성 지표를 측정하는 것이 중요합니다. 이상적으로, 머신 러닝 모델의 출력으로 생성되거나 영향을 받을 수 있는 특징이 개입되지 않도록 노력해야 합니다. 특정 비즈니스나 기술 결정 때문에 이를 피할 수 없는 경우에는 이를 모니터링하는 것이 절대적으로 중요합니다.

# Missing-as-a-Feature

모델 개발 파이프라인에서 일반적으로 결측값 보완과 관련된 단계를 수행하며, 모델이 존재하지 않는 정보를 처리할 수 있도록 합니다. 모델 개발 중에는 특징들의 결측값 개수를 검토하고, 해당 열의 결측 데이터 비율이 특정 임계값 아래로 떨어질 때만 이를 처리하고 그 값을 초과할 경우 특징을 버리는 것이 일반적인 실천 방법입니다.

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

프로덕션 환경에 모델을 적용하면서 일반적으로 한 가지 문제가 발생합니다. 그 중 하나의 피처가 예상보다 더 많은 결측값을 받기 시작할 때 어떻게 될까요? 결국 여러분은 결측 정보(및 보완 방법)을 주로 기반으로 예측하게 됩니다!

이것은 일반적으로 "피쳐로서의 결측값"이라고 불리는 패턴입니다. 본질적으로 여러분은 예측에서 피처의 값으로 보완 방법을 사용하고 있습니다.

![Model Drift Introduction and Concepts](/assets/img/2024-07-13-ModelDriftIntroductionandConcepts_3.png)

분포 추적만으로는 결측값을 피처로 사용하는 상황을 알아내는 데 충분하지 않습니다. 파이프라인에서 보완 단계 이전에 모델로 들어가는 결측값의 %를 명시적으로 모니터링해야 합니다.

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

이상입니다! 이 포스트를 읽어주셔서 감사합니다. 모델 드리프트는 최근에 활발히 연구되고 있는 분야 중 하나입니다. 즉, 자동 트리거를 사용하여 모델을 다시 학습시키거나 처음부터 새로운 모델을 학습해야 하는지 이해하는 방법에 대해 연구하고 있습니다.

포스트 프로덕션 모델을 더 효과적으로 관리할 수 있는 많은 도구들이 있습니다. 아래는 모두를 다 포함한 목록은 아니지만, 실험해 볼 수 있는 가장 잘 알려진 프레임워크 중 일부를 포함하고 있습니다:

- [MLflow](https://mlflow.org/) — 멋진 기능을 갖춘 오픈 소스 MLOps 플랫폼입니다. 궁금하시다면 라이브러리의 리포지토리를 확인해보세요.
- [Azure ML Studio](https://azure.microsoft.com/en-us/products/machine-learning/) — Azure ML Studio. 조금 고급스럽고 Microsoft의 제품을 둘러보고 싶다면, Azure ML Studio는 MLFlow를 기반으로 한 많은 기능을 제공하며 Azure 스택의 다른 부분과 매우 잘 통합됩니다.
- [NannyML](https://github.com/NannyML/NannyML) — 모델 드리프트와 관련된 다양한 기능을 제공하는 오픈 소스 라이브러리인 NannyML입니다.

다음 포스트에서 만나요!

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

제 유튜브 채널, Udemy 프로필 또는 서브스택을 방문해 주세요:

- 서브스택: [thedatajourney.substack.com](https://thedatajourney.substack.com)
- 유튜브: [youtube.com/@TheDataJourney42](https://youtube.com/@TheDataJourney42)
- Udemy: [https://www.udemy.com/user/ivo-bernardo/](https://www.udemy.com/user/ivo-bernardo/)
