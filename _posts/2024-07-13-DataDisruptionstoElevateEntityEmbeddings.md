---
title: "엔티티 임베딩을 향상시키는 데이터 방해 기법 5가지"
description: ""
coverImage: "/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_0.png"
date: 2024-07-13 03:00
ogImage:
  url: /assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_0.png
tag: Tech
originalTitle: "Data Disruptions to Elevate Entity Embeddings"
link: "https://medium.com/towards-data-science/data-disruptions-to-elevate-entity-embeddings-b1ddf86a3c95"
isUpdated: true
---

오늘은 신경망 모델의 entity embeddings의 일반화 능력을 향상시키는 확률적 정규화 방법에 대해 이야기해보겠어요. 훈련 중에 데이터 생성기를 사용하여 선택한 입력 값을 무작위로 데이터에 주입하여, 모델이 보지 못한 코드와의 처리 방법을 학습하도록 돕습니다.

특히 계층적 범주형 특성의 경우 성능 향상이 두드러집니다. 무작위성은 모델이 누락된 하위 수준 코드를 보완하기 위해 상위 수준 그룹 정보를 활용하는 데 도움이 됩니다.

잡음 추가, 정보 제거 또는 데이터를 다루는 방법을 조정함으로써 모델의 강건성을 높이고 오버피팅을 줄이는 데 자주 사용됩니다. 여기서는 누락된 범주형 정보를 처리하는 방법을 모델이 배울 수 있도록 돕기 위해 활용되었어요. 특정 공개 테스트 데이터셋을 분석하여, 수정되지 않은 데이터와 두 가지 방식으로 수행된 무작위화를 비교해보았습니다.

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

랜덤값을 섞어 각 미니 배치가 다른 시나리오를 보는 데이터 생성기를 사용하는 것이 정적 데이터 수정보다 더 나은 결과를 내놓습니다.

그러나 주의할 점은, 코딩 계층구조가 문제와 관련이 없을 때 오버피팅이 발생할 수 있습니다. 나는 코드들을 랜덤 그룹으로 테스트하고 랜덤한 훈련 데이터로 성능이 저하되는 것을 확인했습니다.

# 배경

## 범주형 데이터

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

카테고리 피처는 숫자 값 대신 카테고리를 나타냅니다. 일상적인 예로는 성별, 자동차 브랜드, 티셔츠 사이즈 또는 미국 우편 번호가 있습니다.

카테고리 피처가 많은 수준(고 카디널리티)을 가지고 있으면, 모델링과 분석 모두 복잡해집니다. 미국 우편 번호는 약 40,000개의 값이 있는 고 카디널리티 카테고리의 한 예입니다.

많은 고 카디널리티 카테고리는 계층적으로 구성할 수 있습니다. 예를 들어 우편 번호를 군으로, 군을 주로, 주를 지역으로 대략적으로 그룹화할 수 있습니다. 계층구조는 대부분 산업 단체나 정부 기관의 소유인 코딩 시스템에서 항상 제공됩니다. 예를 들어 미국 노동 통계국 직위 코드나 ICD 진단 코드와 같이요. 여기서 보이지 않는 코드들이 매우 중요할 수 있습니다. 왜냐하면 이러한 시스템은 정기적으로 업데이트되기 때문이죠.

## Entity Embeddings

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

Neural network 모델에서 Entity Embeddings는 많은 이산값을 가질 수 있는 feature들의 낮은 차원 표현을 생성합니다. Entity Embeddings는 범주형 feature의 각 수준에 숫자 벡터를 할당합니다. 이 벡터들은 무작위 값으로 초기화되지만 훈련 중에 값이 업데이트됩니다. 마지막으로 훈련된 벡터들(Embeddings)은 범주형 feature 수준의 거리 측정을 제공하는 추가적인 이점이 있습니다.

## 방법

내 모델은 미국 소규모기업청 (US Small Business Administration)의 대출과 관련된 공개 (CC BY 4.0) 데이터셋에 대한 기본 값(defaults)을 예측합니다. 기존에 설명한 바와 같이, 일반적인 기업 데이터를 반영하는 feature 하위 집합을 선택합니다. 이 중에서 산업을 나타내는 고차원 NAICS feature를 포함합니다. 산업이 누락된 행(대부분이 예전 대출에서 나온 것)은 제거합니다. 훈련-테스트-검증을 70/15/15로 분할하고, NAICS 코드의 10%를 보이지 않는 코드에 대한 모델 결과 분석을 위한 예약 세트로 설정합니다.

Neural network 모델은 Tensorflow/Keras로 구축됩니다. 모든 모델에는 Early stopping이 사용되며, tanh 활성화도 사용됩니다. NAICS 정보는 Keras Embedding 레이어를 사용하여 모델에 통합됩니다. Embedding 레이어는 정수 입력을 취합니다. NAICS 코드를 정수로 매핑하기 위해 scikit-learn의 OrdinalEncoder를 사용합니다. (이에 대해 나중에 다시 언급하겠습니다.)

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

이 프로젝트의 코드는 [11]에서 찾을 수 있습니다. 테이블 데이터도 그곳에서 사용 가능합니다; 가독성을 위해 이 게시물에서 테이블 이미지를 사용합니다.

# "보이지 않는" 코드를 주입하여 오버피팅을 해결합니다

이 데이터셋에서 entity embeddings가 보이지 않는 코드에서 성능이 저하된다는 것을 이전에 보여드렸는데요, 훈련 데이터셋에서 NAICS 인코딩을 랜덤하게 대체하여 높아진 성능이 있었습니다. 그러나 이 해결책은 제게는 다소 난잡해 보였어요.

이를 더 생각해보니, 더 나은 방법이 될 수 있을 것 같아요. 랜덤 대체를 훈련 중에 작업한 후 수정된 사례를 각 배치마다 섞는 방식으로 하면 어떨까요? 이렇게 하면 모델이 시간이 지남에 따라 대부분의 훈련 데이터를 보게 될 거예요. 게다가 적절한 균형을 맞추기 위해 랜덤화의 양을 조절할 수도 있지 않을까요?

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

신경망은 정말로 유연합니다. 맞춤 데이터 생성기를 작성하고 사용하는 것이 간단하며, 각 에포크마다 수정을 위해 서로 다른 랜덤 샘플을 선택할 수 있습니다. 나는 누락된 값을 나타내기 위해 "1"을 코드로 선택했고, 랜덤화 과정에서 실제 NAICS 인코딩을 이 값으로 대체합니다.

여기서, 훈련용 NAICS의 10%를 보이지 않는 코드로 설정했는데, 두 가지 다른 방식으로 진행되었습니다. 고정된 랜덤화는 fitting 이전에 훈련 데이터의 10%를 무작위로 수정하는 것이며, 섞는 랜덤화는 데이터 생성기를 사용하여 각 미니 배치마다 다른 10% 샘플을 수정하는 것입니다. 훈련 데이터만 수정되며, 검증, 테스트, 그리고 NAICS 홀드아웃 데이터는 변경되지 않으며 테스트 간에 동일합니다.

![Image](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_1.png)

표 1은 정밀도-재현율 곡선 아래 영역(PR-AUC)으로 측정된 모델 성능을 포함하고 있습니다. Entity embeddings를 통해 NAICS를 통합하는 것은 무작위 선택된 테스트 데이터에 대해 큰 성과를 보여줍니다. 그러나 데이터 수정이 없을 경우, 성능은 보이지 않는 NAICS 코드(홀드아웃 샘플)에 대해 베이스라인 아래입니다. 흥미로운 점은 NAICS에 대해 랜덤 값을 주입하면 베이스라인 성능을 회복시킬 수 있다는 것입니다!

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

과적합을 줄이는 것은 좋지만, 그 이상이 있습니다. NAICS 코드는 계층 구조를 가지고 있습니다. 데이터 수정이 그것을 활용하는 데 도움이 될까요?

## 무작위화가 계층이 빛날 수 있게 합니다

위에서 논의한 대로, 일부 고차원 카테고리 데이터는 계층 구조로 구성되어 있습니다. 여기에서 사용하는 NAICS Xcode는 미국 정부에 의해 유지되며 사업체 유형의 5단계 분류를 포함하고 있습니다. 예시는 표 2에 표시되어 있습니다.

![Table 2](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_2.png)

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

내가 사용한 코드 구조를 기반으로 모델에 기능을 추가했어요. 표 3에서는 기본 NAICS(6 자리), 업종 그룹 (4 자리), 하위 부문 (3 자리) 및 섹터를 포함한 모델에 대한 테스트를 보여줍니다. 각 기능에 대한 entity embeddings을 사용했어요.

![이미지](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_3.png)

표 3에서는 계층 구조 기능이 데이터 수정이 없는 경우에도 성능을 향상시킨다는 것을 보여줍니다. 그러나 데이터 무작위화는 그것을 더욱 높은 수준으로 끌어올려요. 데이터 수정이 entity embeddings과 결합되면, 본적 없는 코드에 대한 성능은 훈련에 사용된 코드와 비슷해요!

# 섞인 무작위화가 고정된 것을 이깁니다.

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

표 1과 3에서는, 훈련 데이터에 무작위로 보이지 않는 코드를 주입할 때, 일정한 데이터 수정을 사용하든지 배치 중에 섞든지 거의 유사한 결과를 볼 수 있어요.

두 데이터 수정 방법이 동등한가요? 직관적으로 섞는 것이 더 나을 것 같아요. 고정 수정은 훈련 데이터에서 정보를 삭제하는 반면, 적은 조합이 주입된 값으로 훈련하는 데 사용돼요. 그림 4는 훈련 데이터만 다운샘플링한 효과를 보여줘요(검증, 테스트, NAICS 홀드아웃 데이터는 변경되지 않음).

![Figure 4](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_4.png)

그림 1에서 오른쪽 열의 플롯들은 보이지 않는 코드의 성능에 명확한 차이를 보여줘요. 수정되지 않은 데이터의 성능이 가장 낮아요. NAICS 계층구조를 사용할 때 데이터 수정의 개선이 가장 현격해요(그림 1D).

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

Figure 1의 비교에서 각 데이터 처리가 "이긴" 횟수를 따져 보면 다음과 같습니다:

![Figure 1](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_5.png)

36회의 비교 중, 무작위 섞기가 24회에서 이기는 것으로 나타났습니다. 고정된 무작위도 수정 없는 경우보다는 자주 더 나은 결과를 보이지만, 대체로 무작위 섞기보다 뒤처집니다.

Training case가 약 100,000개 이하인 경우, Figure 1 그래프가 급격히 하강합니다. 이 범위에서는 충분한 데이터가 없어 좋은 예측을 내리기 어렵습니다. 수정되지 않은 데이터가 이러한 적은 사례 수에서 우위를 차지하고 있는 것으로 보입니다.

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

# 일부 계층은 오버피팅의 위험 요소일 수 있습니다

트리 기반 모델에서 사용되는 일부 NAICS 인코딩은 범주형 코딩 시스템의 세부 사항에 민감할 수 있으며 잘못된 계층은 오버피팅을 초래할 수 있습니다 [6,7]. 신경망이 동일한 문제를 겪을 것이라고 생각하지 않았지만 테스트하기로 결정했습니다.

표준 NAICS 계층 대신에 코드들을 무작위로 그룹화하고 모델을 다시 시도했습니다. 베이스 NAICS와 표준 NAICS 계층과 거의 동일한 수준의 무작위 그룹에 대한 entity embeddings가 포함되었습니다.

![이미지](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_6.png)

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

안타깝게도, 표 5는 임의의 그룹에서 데이터 처리 과정을 통해 오버핏을 보여줍니다 (가정이 잘못되었군요). 안타깝게도, 성능 하락이 NAICS만 사용한 모델에 비해 특히 섞인 임의의 값들로 인해 최악의 영향을 미칩니다.

문제 해결에 의미 있는 계층 구조가 있을 때, 임의의 삽입을 함께 사용하는 것은 매우 강력합니다. 그러나 관련 없는 조직은 오히려 방해가 될 수 있습니다. 따라서 코딩 시스템을 잘 고민해야 합니다.

# 얼마나 많은 무작위화가 충분한가요?

무작위화가 유용한 상황에서 몇 개의 사례를 수정해야 하는지 결정하는 방법이 있을 수 있습니다. 이 질문에 대한 양적 답변이 있을 수 있지만, 이 블로그에서는 그냥 10%의 삽입 비율을 선택했습니다. 왜냐하면 그것이 옳다고 느껴졌기 때문입니다. 데이터 생성기를 사용하여 몇 가지 다른 삽입 비율을 테스트해 보겠습니다. (Figure 2):

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

![Image](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_7.png)

안녕하세요! NAICS 코드에 대한 최신 연구결과를 공유해드릴게요. Figure 2A에서 확인할 수 있는 바와 같이, 상위 수준의 기능이 없는 NAICS에 대해서는 효과적인 무작위화 수준의 범위가 있어요. 시험 데이터의 성능은 수정률이 약 80% 이상일 때 감소하는 것으로 나타났어요. 물론, 높은 무작위화 수준은 훈련용 NAICS 코드를 이용할 수 없다는 것을 의미해요.

Figure 2A에 나온 보류 데이터에 대해서는, 낮은 비율 곡선이 급격하게 증가하는 것을 확인할 수 있어요. 오버핏팅을 해결하기 위해 수정해야 하는 값은 많지 않아요. 심지어 1%도 효과적일 것 같아요! 게다가 성능 향상에는 상한선이 없어요. 이는 수정이 오버핏팅을 줄이기만 하고, 보지 못한 코드에 대한 NAICS 정보는 없기 때문에 예상할 수 있는 결과죠.

Figure 2B는 코딩 계층 구조의 여러 수준을 포함한 모델의 결과를 보여줍니다. 시험 데이터셋에 대해서는, 수정률이 약 40-50% 이상일 때 시험 데이터의 성능이 천천히 감소하는 것으로 나타났어요. 저는 이것이 일어나는 이유는 훈련 시 하위 수준 코드를 너무 많이 가려서 모델이 덜 구체적(그리고 예측력이 낮은) 상위 그룹에 의존하기 시작하는 것 같다고 생각해요.

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

Figure 2B에서 본 테스트 데이터의 낮은 요율부근에 급격한 상승이 보입니다. 그러나 높은 주입율(~80%)에서는 감소도 나타납니다. 이제 모델이 (고수준) NAICS 정보를 사용하며 충분한 볼륨이 필요합니다.

![image](/assets/img/2024-07-13-DataDisruptionstoElevateEntityEmbeddings_8.png)

# 이 방법이 좋을까요?

트레이닝 데이터의 정적 수정을 이기는 제너레이터 사용은 강점과 약점에 대해 논의하겠습니다.

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

단일 범주에 대한 보편적인 성능은 좋습니다. 최소한 이 테스트 데이터셋에서는 대부분의 조건에서 랜덤 테스트 데이터에 대한 성능이 변화 없이 유지되거나 개선되는 경우가 많습니다. 높은 수준의 무작위성은 성능이 저하될 수 있지만 잘 작동하는 다양한 주입률이 있습니다.

단일 범주에 대한 보이지 않는 코드의 성능은 좋습니다. NAICS만을 사용한 모델의 경우 보이지 않는 값의 주입은 오버피팅을 해소하고 기준치를 초과한 성능 향상을 이끌어냅니다.

관련 코드 계층을 활용하는 능력은 훌륭합니다. 계층이 사용될 때, 보이지 않는 코드에 대한 모델 성능은 보이는 코드와 비교 가능합니다! 이는 유사한 데이터에 대해 시도해본 XGBoost 기술에 비해 큰 발전입니다.

계층이 무관한 경우 편견 회피는 나쁩니다. 여기서는 무작위 주입이 부족한 부분입니다. 코드 계층이 응답과 관련이 없는 경우 성능이 저하될 수 있습니다.

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

**Feature Engineering 단계가 적어 편리하네요!** 지난 블로그에서 보지 못한 값을 위한 정적 랜덤 코드 주입을 시도해봤어요. 그러나 상류 수정은 조금 어색한 느낌이 들었죠. 발전기를 사용하는 것이 훨씬 나아 보였어요. 데이터 발전기는 훈련 데이터의 정보 손실을 완전히 방지하며, 누락된 코드와 함께 다양한 시나리오를 시뮬레이션할 수 있어요.

**구현의 용이성은 OK!** 발전기 자체는 매우 직관적이에요. 내 코드를 복사하고 개선해도 괜찮아요. 제 이상적인 세계에서는 Keras(PyTorch 등)가 완료된 무작위화를 내장하고 있으면 좋을 것 같아요. 아마 임베딩 레이어의 매개변수로 포함되어 있으면 좋을 거에요.

**그러나 정수 인코딩에 대해 조금 더 고려해야 해요.** 임베딩 레이어의 입력에 필요한 정수 인코딩에는 조금 꺼림칙해지죠. 제가 사용한 scikit-learn의 OrdinalEncoder는 본래 보이지 않거나 누락된 코드를 위해 특정 값 예약하기 어렵게 만들어요. 이를 우회하기 위해 OrdinalEncoder를 래핑하여 인코딩된 값에 2를 더해 0을 누락 상태, 1을 보이지 않는 코드로 예약할 수 있었어요. 이렇게 하지 않으면 각 범주형 값에 대해 보이지 않는/누락된 것을 표현하는 다른 코드가 필요하며, 해당 특성으로 이산값을 주입하기 위해 사용자 정의 데이터 발전기가 필요해요. 또한, 인코더를 맞추기 전에 훈련 데이터의 수준 개수를 세야 해요.

**임시 결론: 상당히 좋아 보여요!** 이 방법은 여러 시나리오에 유망한 것 같아요. 그러나 코드 계층을 사용할 때는 조금 더 신중한 생각과 테스트가 필요할 거예요. 하지만, 저는 하나의 데이터셋과 코딩 시스템만을 테스트해보았으니, 다양한 데이터셋 및 다른 코딩 시스템에서의 결과를 확인하는 것이 좋겠죠.

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

# 누락된 값은 어떻게 처리해야 할까요?

누락된 값은 보이지 않는 값과 동일한 값으로 인코딩하거나 다른 값으로 인코딩할 수 있습니다. 누락 및 알 수 없음을 동일한 코드로 매핑하고 데이터셋에 간헐적인 누락된 값이 포함되어 있다면, 랜덤화기를 사용할 필요가 없을 수 있습니다. 왜냐하면 NAICS와 함께 알 수 없음 값의 조합이 "자연스럽게" 존재할 것입니다.

그러나 많은 데이터셋, 이 데이터셋을 포함한 경우, 해당 전략은 문제가 될 수 있습니다. SBA 대출 데이터의 경우, 누락된 값은 매우 오래된 대출에 발생하며, 현재 대출과 여러 측면에서 다릅니다. 이 경우, 알 수 없는 값에 대해 누락된 코드를 사용하면 결과에 심각한 편향이 발생할 수 있습니다. 또한 해당 방법이 작동하려면 충분한 수의 누락된 값이 필요하며, 비율을 조정할 수 없습니다. 생성기가 보다 다양한 해결책을 제공할 수 있습니다.

# 마지막으로 생각할 것들

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

산업 데이터는 수업, 대회, 연구 논문에서 사용하는 데이터셋과는 조금 다르다는 것은 비밀이 아닙니다. 내 의견으로는, 그 중 하나는 야생에서 발생하는 고 카디널리티 부호 체계의 숫자가 다르다는 것입니다. 사실, 부호 체계 주변에는 전체적으로 산업과 그 목적과는 매우 다른 맥락에서 사용되는 직업과 회사가 있습니다. 정부에서 생성한 코드는 빠르게 산업에 도입되어 그 목적을 크게 벗어난 방식으로 사용됩니다.

일부 표준 부호 체계는 마치 내 신발에 있는 돌 같았습니다. 그들로 더 많은 일을 할 수 있을 것 같지만, 아직 탐구할 기회가 없었습니다. 종종, 일반적인 그룹화만 고려됩니다. 이것에 대해 다른 옵션을 탐색할 기회가 생겨 기대됩니다. 교육 과정 중에 무작위화는 내 일에 매우 유용할 것 같습니다.

다음으로는 NAICS 임베딩의 시각화와 분석을 좀 더심층적으로 살펴보고 싶습니다. 읽어 주셔서 감사합니다!

## 참고문헌

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

[1] Devansh Devansh, "Using Randomness Effectively in Deep Learning" (2022), _Medium_.

[2] R. Moradi, R. Berangi, and B. Minaei, "A Survey of Regularization Strategies for Deep Models" (2020), _Artificial Intelligence Review_, 53, 3947–3986.

[3] C. Guo and F. Berkhahn, "Entity Embeddings of Categorical Variables" (2016), arXiv:1604.06737.

[4] M. Li, A. Mickel, and S. Taylor, "Should This Loan be Approved or Denied?: A Large Dataset with Class Assignment Guidelines" (2018), _Journal of Statistics Education_, 26(1). (CC BY 4.0)

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

[5] M. Toktogaraev, **이 대출을 승인해야 할까요 거부해야 할까요?** (2020), Kaggle. (CC BY-SA 4.0)

[6] V. Carey, **목표 인코딩에서의 계층적 블렌딩 탐색** (2024), Towards Data Science.

[7] V. Carey, **라벨을 놓치지 말아라: 계층적 범주에 대한 대체 인코딩** (2024), Towards Data Science.

[8] 미국 인구 조사국, **북미 산업 분류 체계**.

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

이번에 소개해 드릴 것은 2024년에 발표된 몇 가지 흥미로운 소식이에요:

- [9] 케라스 3 API 문서에 나온 Embedding 레이어에 대한 정보입니다.
- [10] 사이킷런 문서에 소개된 OrdinalEncoder에 대한 내용을 확인해 보세요.
- [11] V. Carey씨가 운영하는 GitHub 저장소인 Blog_naics_nn을 살펴보세요! [여기](https://github.com/vla6/Blog_naics_nn)에서 확인할 수 있어요.
