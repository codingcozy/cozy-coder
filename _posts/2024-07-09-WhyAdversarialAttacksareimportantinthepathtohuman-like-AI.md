---
title: "인간 같은 AI 개발을 위해 중요하게 여겨지는 적대적 공격의 역할"
description: ""
coverImage: "/assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_0.png"
date: 2024-07-09 10:20
ogImage:
  url: /assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_0.png
tag: Tech
originalTitle: "Why Adversarial Attacks are important in the path to human-like-AI"
link: "https://medium.com/bits-and-neurons/why-adversarial-attacks-are-important-in-the-path-to-human-like-ai-93475d0b7acc"
isUpdated: true
---

심층 학습 기반 알고리즘들은 이미 10년 이상에 걸쳐 시각 작업의 최전선에 있습니다. 이러한 대형 인공 신경망(CNN 및 Vision Transformer)은 이미지 분류와 물체 감지와 같은 시각 작업에서 인간 수준의 성능을 달성할 수 있습니다. 이 도메인에서의 성공의 증거가 있다고 하더라도, 그들의 부족함을 입증하는 증거 또한 부족하지 않습니다. 이러한 네트워크의 취약성 중 하나는 깊은 학습 커뮤니티에서 상당한 주목을 받고 있는데, 바로 이 네트워크의 부서지기 쉬운 특성입니다. 입력 이미지의 작은 뒤틀림이나 화소 값의 변경이 이러한 네트워크에서 예측할 수 없는 움직임을 일으키기 때문입니다. 이 기사에서는 적대적 공격의 문제, 그 원인 및 인공 시각 지능의 상태를 더욱 끌어올리는 데 왜 중요한지를 개요로 설명하겠습니다.

# 적대적 공격 소개

맞죠, 입력 이미지의 화소 값에 미세한 변화가 학습되고 능력 있는 합성곱 신경망(CNN)을 완전히 오분류하게 만들 수 있습니다. 얼마나 작은지 궁금하실까요? 한 픽셀만큼!

적대적 공격 현상은 2014 논문 Intriguing Properties of Neural Networks에서 Ian Goodfellow와 그 팀에 의해 널리 알려졌습니다. 이러한 공격은 이미지에 무작위 노이즈를 추가하여 이전에 원본 이미지를 성공적으로 분류 할 수 있던 CNN이 이제 속은 것처럼 하는 것입니다. "돼지를 날게"하는 딥 러닝의 유명한 예시가 아래에 표시되어 있습니다.

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

오른쪽 이미지를 인간 눈으로 봐도 분명히 돼지의 모습입니다. 그러나 최첨단 CNN은 비행기로 인식합니다. 이게 무슨 일일까요?

이러한 증상에 대한 일부 통찰은 이러한 적대적 예제가 어떻게 구축되는지 살펴봄으로써 얻을 수 있습니다. 이러한 예제들은 사실 구축된 것이며 무작위적이거나 자연적으로 발생한 것이 아님을 주목하는 것이 중요합니다. 그럼에도 불구하고, 현재 기계 시각에서 뚜렷한 약점을 드러냅니다.

## 적대적 공격을 어떻게 처리할까요?

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

적대적 공격에 대한 이야기에 앞서, CNN에 이미지를 분류하는 방법을 가르치는 것은 어떻게 할까요? 기본적으로, 가르치는 것은 최적화 문제입니다. 많은 [이미지, 레이블] 데이터 포인트가 주어졌을 때, 우리는 CNN이 가능한 많은 이미지의 예측된 레이블을 맞추도록 하고 싶습니다. 이것이 손실 함수가 등장하는 곳입니다. 이 특별한 함수는 CNN의 예측된 레이블과 데이터셋에서의 실제 레이블을 받아 예측이 얼마나 ‘틀렸는지’를 알려줍니다. 물론, 이 손실 값을 가능한 낮게 유지하고 싶습니다. 따라서, 우리가 가진 최적화 문제는 다음 방정식으로 요약될 수 있습니다. 여기서 우리는 데이터셋의 모든 N개의 이미지와 레이블에 대해 손실 함수를 최소화하는 CNN 매개변수(θ)를 찾습니다. 네, 기계 학습은 그렇게 간단한 일이죠!

![이미지](/assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_1.png)

이제, 유사한 방식으로 적대적 공격을 계획할 수 있습니다. 이제 우리의 의도가 '반대'로 바뀌었으므로, 모델을 속이기 위해 반대 방향으로 가면 됩니다: 손실을 증가시키세요! 입력 이미지가 주어졌을 때, 우리는 손실 함수를 최대화하도록 작은 변조(δ)만 추가하면 됩니다. 결과적으로 변조된 이미지는 여전히 인간의 눈에는 동일하게 보이도록 제한되어야 합니다.

![이미지](/assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_2.png)

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

이것이 우리가 입력 이미지와 CNN의 매개 변수를 제공받았을 때 적대적 공격을 구축하는 데 있어서 기본적인 아이디어입니다. 이러한 왜곡이 매개 변수에서 어떻게 생성되는지에 대한 자세한 정보는 동일한 저자들이 쓴 논문에서 찾아볼 수 있습니다. - Adversarial Examples를 설명하고 활용하다.

# 왜 네트워크는 적대적 공격에 취약한가요?

이제 이러한 공격에 좀 더 익숙해진 만큼, 왜 이러한 공격이 성공하는지에 대해 생각해볼 수 있습니다. 어떻게 자연 이미지에서 초인적인 분류 정확도를 갖는 최신 CNN이 이미지의 아주 작은 변화에 쉽게 속아넘어가는 것일까요? 더구나 그 변화 자체가 인간에게는 눈에 띄지 않을 정도로 미세할 때는요?

Madry Lab의 연구자들은 적대적 예시가 위처럼 발생하는 것은 우리가 CNN을 훈련시키는 방식의 불가피한 결과라고 제안합니다. 그들의 논문에서 이미지는 사람 중심적인 특징과 모델 중심적인 특징으로 구분될 수 있는 시각적 특징으로 구성된다고 주장합니다. 강력한 특징은 사람 중심적인 특징으로 생각할 수 있습니다. 즉, 사람이 이미지를 분류하는 데 사용하는 이미지의 구성요소입니다 (예: 개의 코와 귀 또는 고양이의 수염과 꼬리). 반면에, 비로보 안전한 특징 또는 모델 중심적인 특징은 사람에게는 인지할 수 없지만 CNN이 분류하는 데 인지 가능하고 유용한 특징입니다. 이러한 특징은 일반적으로 픽셀과 이미지의 레이블 간의 통계적 상관 관계로, 사람들에게는 무의미하지만 CNN에게는 유의미합니다.

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

이 논문은 이러한 비탄력적인 특징들이 적대적 공격 중에 '뒤바뀐다’고 주장합니다. 다른 말로 하면, 적대적 왜곡은 강아지 이미지의 비탄력적인 특징을 강아지에서 고양이로 바꿔주지만 견고한 특징은 건드리지 않습니다. 이로 인해 이미지는 우리 눈에는 여전히 강아지처럼 보이지만 CNN은 두 종류의 특징을 모두 볼 수 있기 때문에 혼란스러워하며 종종 잘못 분류됩니다.

![이미지](/assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_3.png)

해당 논문에서는 존재하고 있는 비탄력적 특징들의 예측력을 실험을 통해 검증했습니다. 같은 논문에서 새로운 데이터셋이 만들어졌는데, 이는 중요한 제약 하에 적대적 예제만 포함되었습니다. 이 제약은 모든 A 클래스 이미지가 B 클래스로, B 클래스가 C 클래스로 교란되었습니다. 이는 A 클래스의 이미지가 이제 A 클래스의 견고한 특징과 B 클래스의 비탄력적 특징을 가지게 되었다는 것을 의미합니다. 이제 데이터셋의 모든 라벨이 견고한 특징이 아닌 비탄력적 특징과 일치하도록 변경되었으므로(우리 눈에는 잘못된 라벨처럼 보입니다), 이 새로운 데이터셋의 예시를 아래 그림에 나타내었습니다.

![이미지](/assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_4.png)

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

CNN을 새로 훈련시킬 때 구성된 데이터셋(취약하지 않은 특성들만이 레이블과 관련이 있는 데이터셋)으로만 훈련한 새 CNN은 원본 데이터셋에서도 높은 정확도를 달성했습니다! 이는 모델이 취약하지 않은 특성들만을 학습하도록 강제했을 때(로버스트 특성들이 레이블과 제대로 상관되지 않았기 때문에), 여전히 원본 데이터에 대해 예측력을 획득했다는 것을 의미합니다. 이 새로운 CNN은 취약하지 않은 특성에 기반하여 분류를 하는 것이었죠!

![Image](/assets/img/2024-07-09-WhyAdversarialAttacksareimportantinthepathtohuman-like-AI_5.png)

Madry Lab의 로버스트 특성과 취약하지 않은 특성 사이의 구별을 넘어, 적대적 공격의 존재에 대한 몇 가지 다른 (그리 설득력 없는) 설명들이 있습니다. 그 중 하나는 RSA의 샤미르(Shamir)와 그 동료들이 제시한 오목한 다양체 모델입니다. 이 연구와 기타 가설들은 적대적 공격의 발생과 각 뉴런의 활성화(출력)이 만드는 고차원 공간/다양체와 그 특성들을 연관 짓습니다.

# 적대적 공격의 중요성

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

이 모든 것의 의의는 무엇인가요? 왜 딥 뉴럴 네트워크의 명백한 ‘특징’을 조사해야 할까요?

마드리의 연구로부터 우리는 기계들이 우리가 중요하게 생각하는 시각적 특징과 함께 비로버스트 특징을 학습하는 것을 우선시한다는 것을 알 수 있었습니다. 이러한 네트워크들에게 이러한 비로버스트 특징들은 최적화 문제를 해결하는 데 도움이 되며 손실 함수를 최소화하는 데 도움이 됩니다. 그러나 CNN을 예측 가능하고 신뢰할 수 있게 동작하게 하려면 그 분류 행동을 인간들과 일치시켜야 합니다. 신뢰할 수 없는 CNN은 자율 주행 자동차, 방어 또는 건강과 같이 보안이 중요한 시스템에 통합될 수 없습니다.

저는 적대적 공격 문제가 인공 일반 지능에 이르는 길에 있는 전통적인 장애물보다는 진단 도구처럼 느껴집니다. 인간과 인공 시각 지능 사이의 간극을 좁히기 위해, 양쪽 네트워크의 내부 메커니즘을 연구하여 후자가 왜 전자만큼 사소한 격동에 취약한지 이해해야 합니다. 새로운 네트워크는 생물학적 영감을 받은 우선을 가지도록 설계되어야 하므로 훈련 과정에서 비로버스트 특징이 우선되지 않습니다. 이것은 다음 세대의 인공 시각 지능을 향한 한 가지 길일지도 모릅니다.
