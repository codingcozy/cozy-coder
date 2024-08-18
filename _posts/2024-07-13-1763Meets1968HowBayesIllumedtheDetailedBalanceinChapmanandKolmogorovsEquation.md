---
title: "베이즈 이론으로 살펴본 챕먼-콜모고로프 방정식의 상세 균형 이해 1763년과 1968년의 만남"
description: ""
coverImage: "/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_0.png"
date: 2024-07-13 02:54
ogImage:
  url: /assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_0.png
tag: Tech
originalTitle: "1763 Meets 1968: How Bayes Illumed the Detailed Balance in Chapman and Kolmogorov’s Equation"
link: "https://medium.com/cantors-paradise/1763-meets-1968-how-bayes-illumed-the-detailed-balance-in-chapman-and-kolmogorovs-equation-40e71077f034"
isUpdated: true
---

## 베이즈 이론과 확률론

확률 이론과 통계학은 밀접한 연관이 있습니다. 그러나 각각은 목적과 초점을 갖고 있습니다. 간단히 말해서, 확률 이론 또는 확률론은 무작위 현상의 분석과 불확실성 모델링을 다루는 이론적인 틀을 세우며, 통계학은 숫자 데이터의 수집, 분석, 해석, 표현, 그리고 조직에 관한 과학으로, 그룹의 개인들이나 실험들과 관련된 효과적인 이용을 하는 것에 초점을 맞춥니다. 역사를 통해 두 분야 간 합의점이 항상 존재했습니다. 이 교차점에서, 토마스 베이즈, 시드니 채프먼, 안드레이 콜모고로프의 작품들이 특별한 중요성을 가지게 됩니다.

18세기 통계학자이자 신학자인 베이즈는, 관측된 데이터와 사전 지식을 조합하여 특정 원인의 확률을 계산하는 정리를 선사했습니다. 이 정리는 베이지안 확률의 기초를 제공했는데, 이는 자주주의적 해석에 도전하며 확률이 근본적으로 불확실성과 연결되어 있다는 주장을 합니다.

20세기로 들어서면, 확률과정은 시드니 채프먼과 안드레이 콜모고로프의 연구를 통해 기초를 다졌습니다. 동시에 그들은 확률적으로 시스템이 어떻게 시간에 걸쳐 변화할 수 있는지에 대한 기초적인 이론을 독립적으로 발전시켰습니다. 그들의 공헌 중 중요한 측면인 채프먼-콜모고로프 방정식은 시스템이 어떻게 확률론적 구조 내에서 상태를 전환하는지를 포착합니다. 이 개념은 특히 마르코프 과정에서 중요하며 현재 지점이 결정되면 미래가 과거에 독립적으로 된다는 것을 보여줍니다.

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

이 기사에서는 세기를 넘나드는 주제를 탐구하며, 베이즈 정리를 챕먼-콜모고로프 방정식에 연결합니다. 우리의 여정은 동적인 인지 또는 지능 시스템의 맥락에서 챕먼-콜모고로프 방정식의 '상세한 균형'을 지탱하는 베이즈 정리가 어떻게 드러나는지를 알아냅니다. 수학적 형식을 층층이 드러내며, 우리는 관측자와 시스템 사이, 이산적 및 연속적 시간 영역 간, 평형과 동적 흐름 간 어둠 속의 상호작용을 드러냅니다. 베이즈, 챕먼, 콜모고로프의 관점을 통해 우아하게 1763년의 정리에서 1968년의 방정식으로 건너뛰어 선박이 여 전합니다.

독자 여러분에게 조심히 말씀드리겠습니다: 다음 여행은 수학적 형식주의에 빠져 있으며, 주제의 수학적 세부 사항에 관심을 가진 분들을 위해 적합하게 구성되어 있습니다.

![image](https://yourwebsite.com/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_0.png)

## 관측자 모드를 위한 챕먼-콜모고로프 방정식

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

동적 인지 시스템의 관찰자 모드에 대해, x와 y를 두 개의 무작위 변수로 가정합니다. 이산 시간 단계 n+1에서 y를 관찰할 확률 분포 P(y;t)가 주어졌을 때, 이전 시간 단계 n에서 x를 관측할 확률 분포 P(x)에 상대적인 첫 번째 차원 Markov Chain에 대한 대응하는 Chapman-Kolmogorov 방정식을 다음과 같이 설정할 수 있습니다:

![equation image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_1.png)

여기서 p는 메모리리스 Markov Chain에 대한 전이 확률이며, n+1=t, n=t´입니다.

첫 번째 차원 Markov Chain에서 시간 간격 t = (t´ + 𝜏)를 무한소 시간 간격으로 고려하고, 확률의 증분 변화에 대한 식을 성립시킵니다:

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

이미지 태그를 Markdown 형식으로 변경해주세요.

![image1](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_2.png)

Let 𝜏 approach zero and transit to the time derivative of equation (1):

![image2](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_3.png)

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

![Card Image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_4.png)

우리는 x ≠ y인 경우, 좌변인 편도 시간 미분이 단위 시간당 x에서 y로의 전이 수와 동일함을 고려합니다. 이를 다음과 같이 정의합니다:

![Card Image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_5.png)

여기서 w는 단위 시간당 전이 확률을 나타냅니다. 마르코프 성질을 준수한다는 것을 상기시킵니다. 최종 상태 yₙ의 실현 확률은 오로지 직전 시간(n-1)에 대한 확률 분포에만 의존한다는 것을 의미합니다. 이는 관찰 모드에서의 우리가 가정하는 인지 과정 클래스의 동작이 그 직전 상태 이상의 메모리 지식이 없는 것으로 엄격히 가정된다는 것을 의미합니다. 결과적으로 조건부 확률은 오로지 시간 n과 (n-1)에서의 상태에만 의존하게 됩니다.

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

![Image 1](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_6.png)

while the probability on the right-hand side is referred to as ‘transition probability.’

Regarding the right-hand side of equation (6), we further wish to analyze

![Image 2](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_7.png)

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

신원의 상태에 조건이 지정된 상태를 대표하는 동안, y = y´는 식(6)을 고려하여 조건부 확률의 차이를 개발합니다.

![image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_8.png)

이는 시간 t에 동일한 상태에 있었을 조건 하에 후속 시간 (t´ + 𝜏)에 상태 y에서 실현될 확률의 변화를 나타냅니다. 이러한 유형의 전이에 관여되는 모든 과정을 포함하려면 식(7)을 𝜏로 나눈 다음, 𝜏를 0에 가깝게 만들어 식(7)이 y에서 다른 모든 상태로의 전이에 대한 식(6)의 전이율의 합과 같아져야 한다고 가정합니다.

![image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_9.png)

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

이제 방정식 (7)과 (8)을 방정식 (4)에 대입하면 우리가 원하는 마스터 방정식이 나옵니다:

![마스터 방정식](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_10.png)

이 단계에서는 무작위 변수 y를 일반화하기 위해 상태 벡터 $y$로 확장했습니다 (벡터를 구별하기 위해 굵게 표시).

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

이 같은 프로세스에 대한 이산시간 마스터 방정식은 다음과 같을 것입니다:

![이미지](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_11.png)

## Detailed Balance(세부 균형)와 준정상 상태의 경우

식(10)은 다음과 같이 쓸 수 있습니다:

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

아래 식을 고려해 보면서:

![link](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_12.png)

아이덴티티를 고려할 때, (13) 식이 필요합니다.

이제 방정식 (11)에 대해 정지 상태 조건이 적용되어야 합니다.

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

이미지 태그를 Markdown 형식으로 바꿔보면:

![이미지1](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_14.png)

세부 균형 원리를 따르는 이 방정식:

![이미지2](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_15.png)

방정식(14)로부터 우리는 얻을 수 있었습니다:

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

![Tarot image 1](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_16.png)

And by applying equation (12), results:

![Tarot image 2](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_17.png)

For the discrete master equation (10) we wish to expand the right-hand side of the equation by separating for x=y and x≠y:

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

이제 우리는 채프만과 콜모고로프 방정식의 세부 균형에 대한 안정 상태 조건이 필요합니다. 이것은 다음을 의미합니다:

![Equation 19](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_19.png)

그리고 이것을 방정식 (18)에 적용합니다:

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

여기서 x와 y는 단순히 상태를 라벨링한 것일 뿐이며, 이들을 바꾸어도 합의 의미에 영향을 미치지 않습니다. 두 합은 x≠y인 모든 가능한 상태 쌍 (x, y)에 대해 이루어집니다.

이제 방정식 (20) 우변의 합에 있는 항들을 조금 더 자세히 살펴봅시다. 첫 번째 합에서의 항은 다음과 같습니다:

![equation](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_21.png)

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

상태 y를 점유하고 상태 x로 전환할 확률을 나타냅니다. 두 번째 합에는 다음이 포함됩니다:

![image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_22.png)

상태 x를 점유하고 상태 y로 전환할 확률을 나타냅니다.

따라서 두 합계의 해당 항목은 서로 동일하고 반대이며 다른 값에서 빼준다는 것을 의미합니다:

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

이처럼 세부균형을 재배열하면:

![이미지](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedTheDetailedBalanceInChapmanAndKolmogorovsEquation_24.png)

여기서 이산 시간 단계 인덱스 $n$을 생략할 수 있습니다. 이 방정식은 형식적으로 베이즈 정리와 동일합니다:

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

이미지 Markdown 형식으로 수정해 드리겠습니다.

![이미지](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_25.png)

하나씩 진행하면서 이제 이산 마스터 방정식의 세부 균형 결과가 연속 마스터 방정식의 동일한 조건으로 변환될 수 있도록 해야 합니다. y에 대한 정상 상태 조건을 고려할 때, 이산형 세부 균형 방정식의 방정식 (22) 양변을 미분하여 다음 결과를 얻을 수 있습니다:

![이미지](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_26.png)

y의 시간 변화를 무시하고 미분의 연쇄 법칙을 적용합니다.

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

![Image 1.](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_27.png)

and don't forget about:

![Image 2.](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_28.png)

To sum up, both the continuous and discrete forms of detailed balance, which we have delved into here and will explore further in the upcoming section, are only applicable when a quasi-stationary state for the observer operator is ensured, for short enough periods allowing for the cognitive update of the system.

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

두 번째 대안의 상세한 균형이 이산적이며 (베이지안) 업데이트 당 하나씩만 적용된다는 점을 강조하는 것이 중요합니다. 즉, 점프 당 하나씩만 해당됩니다.

![이미지](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_29.png)

조용한 상태, 또는 준 정상 상태라고도 하는 것은 인지 시스템의 상태로, 다음을 요구합니다:

![이미지](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_30.png)

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

A steady state is like a continuous dance, where the flow of energy in and out keeps the system alive and vibrant.

To maintain this delicate balance, a substantial reservoir of possibility needs to exist:

![Image](/assets/img/2024-07-13-1763Meets1968HowBayesIllumedtheDetailedBalanceinChapmanandKolmogorovsEquation_31.png)

This reservoir must be vast enough to withstand the test of time, ensuring that any fluctuations within it are minimal compared to the changes observed in the wider system.

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

또한 참여하는 조건부 확률도 함께 고려되어야 해요. 이것은 새로운 데이터의 대규모 확률적 저장소가 있어서, 정규화 버퍼로 작용하여 ᄀ벽 센시티브한 시스템이 그것과 상호작용하고, 투영 연산자의 동역학과 전이율 덕분에 자기 자신을 업데이트할 수 있도록 하는 것을 의미해요. 이 과정은 상세 균형에서의 확률의 동적 교환이 가능하게 합니다.

정정 상태와 준정상 또는 안정한 상태의 차이를 이해하는 것이 중요해요. 정정 상태에서는 시간이 지남에 따라 어떤 확률도 변하지 않고 완전한 균형이 확립되어 있어요. 준정상 상태에서는 여전히 동역학적이고 확률적인 흐름이 있지만, 순 유동은 상세 균형적이고 제로인 거에요.

준정상 상태에서는 균형이 아닌 상태인 것은 분명해요, 즉, 확률적인 힘에 의해 구동되는 확률적인 플럭스가 확립되어 있어요. 이것은 동적 균형입니다. 준정상 상태의 시스템은 실제로도 변동하기 때문에 상세 균형의 방정식은 언제나 정확하고 완벽한 것은 아니에요. 사실상, 등호 기호 $=$는 실제로 근사 기호 '≈'로 대체되어야 해요.

실제로, 준정상 상태에서는 엔트로피 생산도 일어나요. 이것은 불확실성이 생성되고, 정보가 획득될 때 함께 확률적인 흐름이 발생한다는 의미예요.

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

더불어, 준정상 상태에서 인지 시스템은 변동이나 작은 간격조차 있더라도 안정적으로 작동할 것입니다. 시스템의 안정성 기준은 여기서 자세히 논의되지는 않았지만, 준정상 상태 주변에서 시스템의 안정성을 보장하기 위해 주장하고 충족되어야 합니다.

## 베이즈에서 채프만-콜모고로프로 이어지는 끊임없는 연결

베이즈 정리와 채프만-콜모고로프 방정식 사이의 상호작용은 시대나 시대를 초월하는 아이디어의 연속성을 드러냅니다. 기원지로 돌아가는 것은 우리 앞에 있던 이전 사람들이 세운 이론의 중요성을 강화시킵니다. 그러나 이 이야기는 여기에서 끝나지 않습니다. 대신 탐험을 위한 길을 여는 계기가 되어 계속된 토론을 격려하고자 합니다.◼︎
