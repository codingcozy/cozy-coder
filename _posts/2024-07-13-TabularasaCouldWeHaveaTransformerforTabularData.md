---
title: "Tabula rasa 테이블 데이터용 Transformer가 가능한가"
description: ""
coverImage: "/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_0.png"
date: 2024-07-13 22:43
ogImage: 
  url: /assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_0.png
tag: Tech
originalTitle: "Tabula rasa: Could We Have a Transformer for Tabular Data"
link: "https://medium.com/gitconnected/tabula-rasa-could-we-have-a-transformer-for-tabular-data-9e4b238cde2c"
---


## | 표 형식 데이터 | 인공 지능 | 트랜스포머 | LLMS |

![Tabular Data](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_0.png)

## 왜 트리 기반 모델이 표 형식 데이터에 대해 딥러닝을 능가하는 것일까요?

지금까지 우리가 계속 묻고 있던 질문입니다. 이 아이디어를 지지하는 이론적 근거부터 시작하여, 표 형식 데이터가 어디에서나 사용되고 있으며 이로 인해 이를 효율적으로 분석하고 점차적으로 다중 모달 세계에 통합할 수 있는 알고리즘을 갖고 싶어하는 욕망이 생겼습니다.

<div class="content-ad"></div>

몇 가지 이 문제는 딥러닝으로만 해결할 수 있습니다. 따라서 연구는 계속되었고, 먼저 신경망의 첫 번째 주요 장애물을 극복하려고 노력하면서 카테고리 변수 문제를 해결하려고 했습니다.

다음으로, 힘을 합치면 신경망에 이점이 있음을 논의했습니다. 사실, 앙상블 개념은 의사 결정 트리에만 해당하는 것이 아닙니다.

일부 사람들에게는 신경망의 과잉 용량이 탭 데이터에서의 성공을 제한하는 요인입니다. 실제로 탭 데이터 세트가 작기 때문에 신경망은 적절한 패턴을 식별하는 데 어려움을 겪습니다. 그래서 몇몇 저자들은 성능과 성능 사이에서 타협점을 찾으려고 노력했습니다. 그것이 정규화입니다.

동시에 다른 연구 방향은 결정 트리와 신경망 사이에 우아한 하이브리드 솔루션을 찾는 것이었습니다. 이러한 아이디어들 중 많은 것들이 흥미로운 것은 사실이지만 순수한 딥러닝 모델이 있는 것이 더 바람직할 것입니다.

<div class="content-ad"></div>

The more intuitive among you may have already noticed a noteworthy absence during this extensive adventure. Indeed, in recent years, a specific framework has gained widespread usage: His Majesty the Transformer.

As we delve into this new chapter of our thrilling expedition, we explore not only our fascination with the transformer but also the obstacles involved in adapting it for tabular data. We will analyze various sophisticated methods and the two primary research paradigms being pursued. Lastly, we will scrutinize the effectiveness of these models and determine if they truly hold an edge over traditional models.

## Unveiling the Transformer's Ascendancy

![Image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_1.png)

<div class="content-ad"></div>

2017년은 인공 지능에 대한 캄브리아 폭발이라고 불릴 수 있습니다. 이전까지는 고요하게 데이터 바다에서 헤엄치던 RNNs, CNNs 및 LSTMs가 게으르게 발견한 모든 것을 소화해 왔습니다. 그러나 2017년에는 거대하고 배고픈 괴물이 나타나, 생물학적 서식지에 적응할 수있는 날카로운 이빨을 가진 포식자로 등장했습니다: 트랜스포머.

“Attention Is All You Need"의 게시 이후, 그 욕심과 능력에 이끌려 트랜스포머는 NLP의 롤 모델이 되었습니다. RNNs를 소멸시킨 그는 CNNs (또는 비전 트랜스포머)도 멸종시키려고 합니다.

이 무서운 몰록은 포식욕에 제한을 받지만, 테이블 데이터셋은 작고 충분한 예시를 담고 있지 않아 그의 굶주림을 채울 수 없습니다. 그럼에도, 테이블 데이터에 특화된 여러 변종이 등장했습니다. 이 글은 바로 그것에 관한 것입니다.

# 트랜스포머의 5W

<div class="content-ad"></div>

이 변환기는 인코더와 디코더로 구성된 seq2seq 모델로, 각각 n개의 동일한 모듈로 이루어져 있습니다(주로 멀티 헤드 셀프 어텐션 다음에 피드 포워드 신경망으로 구성됩니다). 또한, 레이어 정규화와 잔여 연결이 있습니다.

변환기 뒤에 있는 원리는 간단합니다: 텍스트 시퀀스 x를 가져와서, 모델은 시퀀스에서 다음 단어 (또는 단어 조각)를 예측해야 합니다. 첫 번째 단계는 토큰화로, 텍스트가 조각으로 줄여지고 어휘가 구성됩니다. 그런 다음 숫자로 표현이 학습됩니다(임베딩) 그리고 모델은 시퀀스에서 단어의 위치를 알 수 있습니다 (위치 인코딩).

![이미지](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_3.png)

<div class="content-ad"></div>

트랜스포머에 흥미를 가지게 된 이유는 자기 지도 학습이 가능한 대형 모델이기 때문입니다. 대부분의 대형 언어 모델(Large Language Models, LLMs)의 경우 두 단계의 훈련을 거칩니다. 첫 번째는 모델을 대규모 텍스트로 자기 지도 학습하는 사전 훈련 단계이고, 두 번째는 특정 작업을 위한 소량 데이터로 모델을 지도하는 세밀 조정 단계입니다. 텍스트 데이터에서 트랜스포머가 매우 강력한 성능을 보여주었으므로, 테이블 데이터에서도 이 성공을 복제하고자 합니다.

일반적으로, 트랜스포머는 계층적 표현을 구축하고 입력의 서로 다른 부분 간의 관계를 학습하는 모델이라고 할 수 있습니다. 따라서 테이블 데이터에 대해서도 다른 특성 간의 관계를 학습하고 이 상호작용의 계층적 표현을 만들 수 있는 모델이 필요합니다.

물론, 테이블 데이터에 트랜스포머를 적용하는 데는 여러 어려움이 있습니다:

- 단어장 밖: 훈련 중에 보지 못한 테이블 내의 특성 또는 다른 엔티티가 있으면 임베딩 작업이 복잡해집니다.
- 테이블의 복잡성: 트랜스포머는 일반적으로 고정된 창을 가지고 있어 다른 형태의 테이블을 동일한 공간에 매핑하기 어려워집니다.

<div class="content-ad"></div>

# 타블라 데이터와 트랜스포머: 사랑 이야기?

![TabularasaCouldWeHaveaTransformerforTabularData_4](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_4.png)

## TabNet

TabNet은 이러한 방법 중에서 가장 잘 알려진 것입니다. 2019년 Google에 의해 최초로 공개되었으며 타블라 데이터의 전처리가 필요하지 않은 점, 더 유연한 통합을 위해 경사하강법으로 모델을 학습할 필요가 있는 점, 트리 기반 모델과 성능이 비슷하며, 특성 선택이 가능하다는 가능성 등을 고려하여 개발되었습니다.

<div class="content-ad"></div>

TabNet은 의사결정 트리와 유사한 면이 있습니다. 사실, TabNet은 순차적으로 훈련되는 일련의 서브네트워크로 구성되어 있습니다. 각 서브네트워크는 결정 단계에 대응하며, 이전 서브네트워크에서 입력을 받아 특성 선택을 수행하고 다음으로 출력을 보냅니다. 과정의 끝에서 각 서브네트워크의 모든 출력은 최종 예측을 위해 집계됩니다.

![TabNet 이미지](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_5.png)

이 모델은 제어된 희소성을 이용하여 소프트 특성 선택을 수행합니다. 모델은 숫자 및 범주형 특성을 모두 입력으로 받고(임베딩 단계가 있음), 그런 다음 특성 변환기에 의해 변환됩니다. 전역 정규화는 없지만 대신 각 블록마다 배치 정규화가 있으며 게이트드 선형 유닛(GLU) 비선형성이 있습니다. 각 블록은 동일한 크기이며 사용할 특성을 결정할 수 있으며 출력은 특성 표현입니다. 이러한 모든 출력은 결정 단계를 위해 집계됩니다.

![TabNet 이미지](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_6.png)

<div class="content-ad"></div>

작가들은 학습 가능한 희소 마스크를 사용하여 특성 선택을 합니다. 주의 깊은 트랜스포머는 각 블록마다 모델이 기다려야 할 특성들을 학습하는 데 사용됩니다. 이 아이디어는 모델이 가장 중요한 특성에 초점을 맞추고 중요하지 않은 특성에 시간을 낭비하지 않도록하여 파라미터를 효율적으로 사용하는 것입니다. 그럼으로 지역적 중요성을 나타내는 특성 마스크를 얻게 되지만 전역 점수로 결합될 수 있습니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_7.png)

TabNet의 추가적인 흥미로운 점은 다양한 인터프리터 레벨을 제공한다는 것입니다. 각 블록은 다른 특성 표현을 학습하고 문제의 특정 하위 도메인에 집중합니다 (합성곱 신경망에서 발생하는 것과 유사).

나중에 TabNet의 대체 버전이 제안되었는데, 예를 들어, 공정한 딥러닝 모델에 대한 확장인 Fair-TabNet이 있습니다. 이 변형에서 모델은 민감한 변수를 올바르게 분류하는 방법을 학습하는 데 초점을 맞춥니다.

<div class="content-ad"></div>

**TabTransformer**

아마존의 TabTransformer는 Self-Attention을 사용하여 범주형 변수를 문맥 임베딩으로 매핑합니다. 다시 말해, 각 범주형 변수는 고유한 밀집 벡터를 받게 되며(모델은 Word2vec에서 영감을 받음), 어텐션을 통해 문맥 정보가 임베딩됩니다.

이 임베딩은 따라서 노이즈에 더 강하고 모델의 해석 가능성을 허용해야 합니다. 사실, 저자들에게는 고전적인 범주형 임베딩은 문맥이 부족하며 다양한 범주형 특성들 간의 관계와 상호 작용을 인코딩하지 못한다고 합니다.

<div class="content-ad"></div>

이 구조는 간단합니다. 범주형과 수치형 특성이 병렬로 처리됩니다. 연속적인 특성은 단순히 정규화되고, 범주형 특성은 임베딩되어 n개의 트랜스포머 블록을 거칩니다. 그런 다음 특성들을 연결하고 다층 퍼셉트론을 통과합니다.

![Transformer Architecture](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_10.png)

이 중요한 점은 간단한 임베딩은 맥락 정보가 없지만, 트랜스포머 블록을 통해 다양한 범주형 변수들이 어떻게 관련되어 있는지를 학습할 수 있다는 것입니다.

<div class="content-ad"></div>

작가들은 레이블이 붙지 않은 데이터가 있는 경우에도 모델을 사용할 수 있는 준지도 학습 접근 방식을 제안합니다. 작가들은 두 가지 모드를 제안합니다:

- 마스크된 언어 모델링(MLM), 여기서 무작위 특징들이 가려지고 모델은 이러한 특징들을 예측해야 합니다.
- 대체 토큰 감지(RTD), 여기서 원래 특징들이 무작위 값으로 대체됩니다. 모델은 특징이 대체되었는지 아닌지를 예측해야 합니다. 생성기를 사용하여 무작위 특징들을 생성하는데, 언어 데이터에는 수천 개의 토큰이 있으며 균일하게 무작위로 선택된 토큰은 너무 쉽게 감지됩니다.

이 접근 방식은 유연성을 제공합니다. 레이블이 붙지 않은 데이터는 모델을 보다 견고하게 만드는 데 사용될 수 있습니다. 또한 레이블이 붙은 데이터로 모델을 세밀하게 조정하는 데에도 사용될 수 있습니다. 작가들에 따르면, 그들의 모델은 경사 부스팅 트리와 유사한 성능을 보입니다.

![TabularasaCouldWeHaveaTransformerforTabularData](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_11.png)

<div class="content-ad"></div>

## ARM-net

안녕하세요! 오늘은 Adaptive Relation Modeling Network (ARM-net)에 대해 이야기해보려고 해요. ARM-net은 피쳐들 사이의 상호작용을 찾는 데 초점을 맞춘 또 다른 모델이에요. 사실, 저자들에 따르면 탭러 데이터는 관계형 데이터베이스에서 추출된 요소들의 튜플로 모델링될 수 있다고 해요. 그래서 주어진 여러 개의 피쳐들 중에서 가장 어려운 과제는 어떤 관계가 피쳐들 사이의 상호작용을 지배하는지 이해하는 것이에요. 문제는 데이터셋에 있는 피쳐 수가 늘어날수록 피쳐들 사이의 잠재적인 상호작용이 기하급수적으로 증가한다는 것이죠.

저자들은 그 후 게이트드 어텐션 메커니즘과 지수 뉴런을 사용하여 이 문제를 해결하려고 노력했어요. 핵심 아이디어는 피쳐들 사이의 상호작용을 지수 공간에서 모델링하고 상호작용의 가중치가 동적으로 변화한다는 것이에요.

<div class="content-ad"></div>

저자들은 자신들의 모델을 사용하는 데 도움이 될 수 있는 프레임워크인 적응형 관계 모델링 프레임워크(ARMOR)를 제안합니다. 이 프레임워크는 예측 뿐만 아니라 전역 해석력과 지역 해석력을 모두 지원합니다.

![Image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_13.png)

## SAINT

Self-Attention and Intersample Attention Transformer (SAINT)은 self-attention과 inter-sample attention을 결합한 하이브리드 모델입니다. SAINT는 모든 특징(범주형 및 수치형 모두)을 결합된 밀도 벡터 공간으로 설계합니다. 이로써 생성된 토큰은 transformer 인코더로 전달될 수 있습니다.

<div class="content-ad"></div>

SAINT은 일련의 동일한 단계로 구성된 변환기입니다. 각 단계에는 자기 주의 변환기 블록과 샘플간 주의 변환기 블록이 있습니다. 첫 번째 블록은 변환기 블록과 동일하며(다중 헤드 자기 주의), 다른 하나는 타블러 데이터에 특화되어 있습니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_14.png)

샘플간 주의는 배치 내 서로 다른 데이터 포인트에 대한 주의를 계산합니다(타블러 매트릭스의 행을 나타냅니다). 일반적인 주의와는 달리, 이 계산은 단일 포인트의 특징에 대해 이루어집니다. 그러나 여기에서 데이터 포인트는 연결되어 있으므로 특정 포인트의 표현이 다른 포인트를 조사함으로써 개선됩니다. 이 메커니즘은 혼란스럽거나 누락된 데이터의 경우 모델을 더 견고하게 만들기 위해 포함되었습니다. 실제로 포인트의 특징이 누락되거나 소음이 있는 경우, 모델은 배치 내 유사한 포인트로부터 정보를 받습니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_15.png)

<div class="content-ad"></div>

작가들은 초기 데이터에 데이터 증강으로 CutMix를 사용하고 임베딩에서 사용되는 데이터에는 mixup을 사용합니다.

모델은 지도학습이든 비지도학습이든 훈련될 수 있습니다. 후자의 경우, 대조적 손실을 사용하여 동일한 샘플의 두 가지 다른 표현 간의 거리를 최소화합니다. 이 경우 두 개의 사전 훈련 작업을 혼합하여 성능을 향상시킬 수 있습니다.

작가들은 여러 데이터셋에서 결과를 테스트하여 샘플 간 주의를 기울이면 모델이 더 효율적이며 SAINT가 데이터 잡음에 강하다는 것을 확인합니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_16.png)

<div class="content-ad"></div>

저자들은 모델이 결정을 내리는 데 사용하는 기능과 데이터 지점을 식별하기 위해 주의 맵을 사용합니다.

![이미지](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_17.png)

## FT-Transformer

FT-Transformer에서 (SAINT에서 본 것과 같이) 범주형 및 연속적인 특징이 모두 임베딩됩니다 (연속적인 특징은 각각의 연속 특징에 대해 간단한 완전 연결 레이어로 임베딩됨). 이는 이러한 벡터들이 변환기 블록으로 전달되어 범주형 밀도 벡터와 함께 데이터 지점의 표현을 향상시킬 수 있기 때문입니다.

<div class="content-ad"></div>

실제로, 나중에 진행된 연구들은 숫자 특성을 포함하는 것이 트랜스포머에만 유익한 것이 아니라 심지어 간단한 MLP의 성능을 향상시킨다는 것을 보여줍니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_18.png)

그래서 이 토큰화와 임베딩 단계 이후에 특별한 CLS 토큰이 추가됩니다. 이 토큰과 특성 임베딩(범주형 및 숫자)은 다양한 블록의 통과 중에 문맥화됩니다. 이 최종 표현에서의 CLS는 최종 출력(예를 들어 예측)을 생성하는 간단한 MLP의 입력이 됩니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_19.png)

<div class="content-ad"></div>

작가들은 NODE, ResNet을 포함한 몇 가지 딥 러닝 모델과 비교했습니다. 결과에서 ResNet은 비교를 위한 좋은 기준선이 되었음을 보여줍니다 (하지만 같은 간단한 MLP는 여전히 기준으로 좋은 성능을 보여줍니다). FT-Transformer은 비교적 우수한 성능을 보여주며 때로는 더 뛰어난 성과를 보입니다.

[Tabularasa Could We Have a Transformer for Tabular Data](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_20.png)

## 비모수 트랜스포머 (NPTs)

마지막으로, 비모수 트랜스포머(NPTs)는 트랜스포머의 다른 변형입니다. 이 모델은 전체 데이터 세트를 입력으로 사용하고 데이터 포인트 간의 상호작용을 명시적으로 학습하여 s를 예측합니다. 이 모델은 이를 수행하기 위해 인자 및 비인자적 접근을 모두 활용하며, 훈련 중에는 이 두 가지 접근 방식을 균형있게 학습하는 방법을 익힙니다:

<div class="content-ad"></div>

자신의주의와 확률적마스킹을 사용하는 저자들은 모델이 데이터생성의 인과 메커니즘을 학습하고 포착하는 것으로 설명합니다. 즉, 매개변수가 없는 방법을 통합하여 다양한 데이터 포인트 간의 의존성을 학습하고 예측에 활용하려는 접근 방식에 중점을 둡니다.


![이미지](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_21.png)


# 타블라 데이터용 언어 모델

<div class="content-ad"></div>

![Tabular Data Transformer](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_23.png)

앞서 본 모델들은 변형되어 표 데이터를 네이티브로 사용할 수 있도록 조정된 트랜스포머입니다. 이것은 수많은 가능한 방법 중 하나입니다. 다른 작가들은 표 데이터를 트랜스포머로 도입하기 위해 특별한 전처리를 사용했습니다.

일반적으로 표 직렬화에는 네 가지 유형이 있습니다:
- 행별로 표를 스캔합니다. 각 행에는 특별한 토큰 [CLS]가 추가되고 표 행이 삽입됩니다. 또는 동일한 순서로 열 이름과 특정 행을 함께 넣습니다.
- 열별로 표를 스캔합니다. 이 경우 데이터 직렬화를 얻기 위해 열별로 스캔합니다.
- 행과 열 직렬화를 결합합니다. 이것은 여러 방법으로 달성할 수 있습니다: 요소별 곱셈, 평균 풀링, 연결 또는 행 및 열 임베딩의 평균. 다른 변형은 먼저 그래프를 통해 이동한 다음 순서를 생성하는 것입니다.
- 데이터를 순서로 나타내기 위해 템플릿을 사용합니다. 일부 접근 방식은 표 데이터에서 자연어 문장을 생성합니다. 예를 들어 고도로 튜닝된 seq2seq 트랜스포머(table-to-text)를 사용합니다.

<div class="content-ad"></div>

**2024년 07월 13일 - Tabularasa: 표형 데이터를 위한 트랜스포머가 가능할까?**

이러한 방법들의 일반적인 변형 중 하나는 각 시퀀스에 테이블 컨텍스트를 연결하는 것입니다. 몇몇 연구들은 이것이 성능을 향상시킨다고 보여줍니다. 어쨌든, 비교 연구가 거의 없으며, 심지어 더 적은 수가 소성 실험을 진행했습니다.

직렬화 이후에는 테이블 데이터가 평범한 트랜스포머로 입력될 수 있습니다. 이에도 불구하고, 특정 작업에 맞게 트랜스포머의 요소가 수정된 여러 가지 변형이 제안되었습니다:
- 위치 임베딩. 일부 모델은 테이블에서 셀의 위치를 식별하기 위해 추가적인 위치 임베딩을 삽입합니다. TUTA는 테이블 내 셀의 2차원 좌표를 모델에 제공하기 위해 세 기반 위치 임베딩을 도입합니다. 변형에는 중첩된 테이블이나 누락된 데이터를 처리하기 위한 시스템 (예: TaBERT)이 포함됩니다.
- 어텐션 모듈. 어텐션 메커니즘을 더 "구조 인식적"으로 만드는 것이 아이디어입니다. 예를 들어, 행 간 종속성을 포착하거나 행별 및 열별 어텐션 추가를 시도합니다. 다른 시스템인 TUTA는 모델이 특정 요소만을 기다리도록 강제하는 마스크를 추가하고 사실적인 지식에 집중합니다. 반면, MATE는 넓은 테이블을 처리하기 위해 희소화를 활용합니다.
- 추가적인 레이어. 평범한 트랜스포머에서 어텐션 블록 다음에 MLP가 따릅니다. 그러나 TaPas와 같은 모델은 테이블 데이터에 특화된 레이어를 추가합니다.
- 훈련 변형. 사전 훈련과 학습 목표 작업은 테이블 데이터에 적응됩니다. 예를 들어, 거의 모든 모델이 일부 셀이 마스킹된 재구성 작업에 초점을 맞추고 있습니다.

<div class="content-ad"></div>

이 모델들은 고전적인 하류 작업 뿐만 아니라 몇 가지 흥미로운 변형에도 사용되었습니다. 예를 들어 텍스트 입력과 표 형태의 데이터베이스를 비교하는 테이블 기반 사실 확인이나 답변이 테이블에서 찾아지는 질문 응답과 같은 작업입니다. 다른 작업으로는 의미론적 파싱(목적은 테이블에서 답을 찾기 위한 쿼리 생성)이나 테이블 검색(여러 테이블이 있는 경우 테이블을 찾기) 등이 있습니다.

# 누가 이기는 트랜스포머일까요?

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_26.png)

<div class="content-ad"></div>

타블러 데이터에 적합한 여러 변형된 트랜스포머를 본적 있습니다. 하나의 의문이 발생합니다:

다양한 모델을 비교하기 위해 여러 조사가 실시되었고, 우리는 주로 두 개의 연구에 초점을 맞출 것입니다. 이 연구들 각각은 선택된 데이터셋의 기준에서 다른 모델을 비교했습니다. 저자들은 트리 기반 모델뿐만 아니라 트랜스포머를 포함한 다양한 딥 러닝 모델을 사용했습니다.

2021년 Borisov의 연구에서는 트리 기반 모델이 여전히 최첨단 기술임을 보여줍니다. 앙상블 모델은 최고의 성능을 달성하지만, 반면에 이러한 모델들은 훈련 및 추론 모두에서 더 나은 성과를 보입니다.

![image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_27.png)

<div class="content-ad"></div>

저자들은 그러나 매우 큰 데이터셋에서는 딥러닝 모델이 성공할 수 있다고 언급합니다. 따라서 그들의 주장에 따르면, 딥러닝 모델은 딥러닝의 이점이 나타나는 충분한 데이터 포인트가 있는 경우에 사용되어야 합니다.

사실, 수백만개의 예시를 포함한 HIGGS와 같은 데이터셋을 살펴보면 고전 모델의 장점이 명확하지 않아 보입니다:

![Image](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_28.png)

저자들은 다른 중요한 점들도 강조하고 있습니다:

<div class="content-ad"></div>

- 이십 개 미만의 기사에서 100개 이상의 다양한 데이터셋을 사용했습니다. 따라서 실제 비교는 단일 기준에서 이루어져야 합니다.
- 딥 러닝에 따르면, 모델은 서로 다른 전처리를 필요로 하며 이는 다른 모델을 비교할 때 고려되어야 합니다.
- 트랜스포머로의 명백한 추세가 있지만, 매개변수 증가에도 불구하고 성능 향상이 상응하지는 않았습니다.
- 아키텍처는 점점 더 복잡해지고 있지만, 왜 고전적 모델이 경쟁 우위를 차지하는지에 대한 이론적 지식이 부족합니다. 따라서 점점 더 복잡한 모델을 만드는 대신에 이러한 미해결된 문제들을 조사해야 합니다.

Grinsztajn은 2022년 이러한 요점들에 대한 답을 시도했고, 분류와 회귀 모두에 대한 독특한 기준을 만들었습니다. 한편, 그는 고전적 모델이 여전히 최첨단 기술인 이유 중 일부를 분석하려 노력했습니다. 저자들은 다른 딥 러닝 및 트리 기반 모델을 비교하기로 결정했습니다.

첫 번째 실험에서는 숫자형 특징만 사용하여, 트리 기반 모델이 여전히 최첨단 기술임을 보여주었습니다:

![이미지](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_29.png)

<div class="content-ad"></div>

저자들에 따르면, 딥 러닝 모델의 가장 큰 약점은 범주형 변수가 아니라고 합니다. 이것들이 존재하더라도 갭이 여전히 남아 있기 때문입니다.

저자들은 이 우월성에는 주로 세 가지 이유가 있다고 합니다.

더 자세한 내용은 이미 여기서 논의했습니다:

**Parting thoughts**

<div class="content-ad"></div>

![Tabular Data Transformer](/assets/img/2024-07-13-TabularasaCouldWeHaveaTransformerforTabularData_30.png)

안녕하세요! 최근 10년간 가장 성공적인 모델인 트랜스포머가 거의 모든 유형의 데이터와 함께 사용되어 왔습니다. 물론 몇몇 그룹들이 이를 테이블 데이터에 적용해 보기도 했습니다. 주로 두 가지 다른 전략이 사용된 것을 볼 수 있죠:

- 트랜스포머 구조를 테이블 데이터에 적용하는 것. 테이블 데이터는 모델의 입력으로 사용되기 전에 최소한의 전처리를 거칩니다. 이후에는 행 주의, 임베딩 변경 등 다양한 전략이 사용됩니다.
- 테이블 데이터를 트랜스포머에 맞게 조정하는 것. 두 번째 전략에서는 시퀀스 내의 종속성을 모델링하는 트랜스포머의 능력을 이용하여 테이블 데이터가 시퀀스로 변환됩니다.

관찰해 보면, 첫 번째 전략이 연구 커뮤니티에서 가장 성공적이었습니다. 실제로 TabNet 및 TabTransformer와 같은 모델들은 많은 인용을 받았으며 여러 조사에서 사용되어 왔습니다. 이는 데이터를 기본적으로 활용하고 해석 가능성이 높다는 점에서 비롯될 수 있습니다. 또 다른 이유는 표를 시퀀스로 고려하는 것이 덜 의미가 있기 때문일 수도 있어요 (일반적으로 표의 열 순서는 중요하지 않거든요).

<div class="content-ad"></div>

어떤 경우에도, 각 접근법의 핵심은 주의를 활용하여 특성간의 의존성을 찾으려는 것입니다. 그러나 이러한 특성 상호작용은 매우 복잡할 수 있으며, 표 형식의 데이터셋은 모델이 효율적으로 이를 모델링하기에 충분히 크지 않을 수 있습니다. 반대로, 더 많은 매개변수가 주어지면 모델은 대신 가짜 상관 관계를 찾을 수도 있습니다. Borisov이 제안한 대로 거대한 데이터셋에서 트랜스포머가 우수한 성능을 보일 수 있다는 단서일 수 있습니다.

두 번째 조사는 딥러닝이 트리 기반 모델을 이기지 못하는 이유에 대한 추가 가설을 제안합니다. 그러나 그들의 벤치마크는 중간 규모의 데이터셋으로 구성되어 있고, 이진 분류와 회귀만을 고려합니다.

따라서 정답은 명확하지 않습니다. 사실, 여러 개의 미해결된 질문이 남아 있습니다. 그린슈타인이 제시한 것처럼 범주형 변수가 딥러닝 모델의 주요 약점은 아니지만, 인코딩은 핵심 문제입니다. 실제로 현재의 인코딩 모델은 정보 손실이 발생하며, 범주형 변수에 포함된 맥락 정보를 활용할 수 있는 딥러닝 모델에 벌칙을 주고 있습니다.

두 번째로, 더 복잡한 아키텍처보다도 표 형식 데이터는 이에 대한 더 나은 아키텍처를 영감받을 수 있는 새로운 이론적 발전을 수 년간 잃어버린 것입니다.

<div class="content-ad"></div>

세 번째로, 트랜스포머의 장점은 다중 모달 모델에 쉽게 통합할 수 있다는 것입니다. 이는 많은 응용 프로그램에 상당한 이점을 제공합니다.

마지막으로, 최근 몇 년간 딥 러닝의 성공 요인으로 꼽히는 이슈인 전이 학습을 아직 다루지 않았습니다. 유감스럽게도, 현재는 테이블 데이터에 적용된 전이 학습을 위한 효율적인 전략이 없습니다. 일부 전략은 데이터 스트리밍과 같은 특수 경우에 개발되었지만 일반적인 규칙은 아직 부족합니다.

## 생각이 어떠신가요? 이러한 방법을 시도해보고 싶으신가요? 댓글로 알려주세요

# 이 글이 흥미로우셨다면:

<div class="content-ad"></div>

다른 내 게시물도 확인해볼 수 있고, LinkedIn에서 연락도 가능합니다. 주간 업데이트되는 AI 및 기계 학습 뉴스를 담고 있는 이 저장소를 확인해보세요. 협업과 프로젝트 제안은 언제든 환영하니 LinkedIn을 통해 연락 주세요.

GitHub 저장소 링크는 여기 있습니다. 거기에는 기계 학습, 인공 지능과 관련된 코드와 다양한 자료들이 모여 있습니다.

최근 게시한 글 중 하나에 관심이 있으실 수도 있습니다:

# 참고 문헌

<div class="content-ad"></div>

이 글을 작성하는 데 참고한 주요 참고 자료 목록입니다. 각 논문의 저자는 이름에서만 인용되었습니다.

- Vaswani, 2017, Attention Is All You Need, [링크](링크)
- Erik Storrs, 2021, Explained: Multi-head Attention (Part 1), [링크](링크)
- Khan, 2022, Transformers in Vision: A Survey, [링크](링크)
- Borisov, 2021, Deep Neural Networks and Tabular Data: A Survey, [링크](링크)
- Arik, 2019, TabNet: Attentive Interpretable Tabular Learning, [링크](링크)
- Boughorbel, 2021, Fairness in TabNet Model by Disentangled Representation for the Prediction of Hospital No-Show, [링크](링크)
- Huang, 2020, TabTransformer: Tabular Data Modeling Using Contextual Embeddings, [링크](링크), [코드](코드)
- Cai, 2021, ARM-Net: Adaptive Relation Modeling Network for Structured Data, [링크](링크), [코드](코드)
- Somepalli, 2021, SAINT: Improved Neural Networks for Tabular Data via Row Attention and Contrastive Pre-Training, [링크](링크), [코드](코드)
- Gorishniy, 2021, Revisiting Deep Learning Models for Tabular Data, [링크](링크), [코드](코드) (여기 및 여기)
- Gorishniy, 2022, On Embeddings for Numerical Features in Tabular Deep Learning, [링크](링크)
- Kossen, 2021, Self-Attention Between Datapoints: Going Beyond Individual Input-Output Pairs in Deep Learning, [링크](링크), [코드](코드)
- Badaro, 2023, Transformers for Tabular Data Representation: A Survey of Models and Applications, [링크](링크)
- Wang, 2021, TUTA: Tree-based transformers for generally structured table pre-training, [링크](링크)
- Herzig, 2020, TaPas: Weakly Supervised Table Parsing via Pre-training, [링크](링크)
- Eisenschlos, 2021, MATE: Multi-view Attention for Table Transformer Efficiency, [링크](링크)
- Yin, 2020, TaBERT: Pretraining for Joint Understanding of Textual and Tabular Data, [링크](링크)
- Grinsztajn, 2022, Why do tree-based models still outperform deep learning on tabular data?, [링크](링크), [코드](코드)
- Saupin Guillaume, XGBoost explained: DIY XGBoost library in less than 200 lines of python, [링크](링크)