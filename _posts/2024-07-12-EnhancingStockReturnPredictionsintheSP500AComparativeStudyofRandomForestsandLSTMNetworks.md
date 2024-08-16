---
title: "랜덤 포레스트와 LSTM 네트워크 비교 연구 SP 500 주식 수익 예측 향상 방법"
description: ""
coverImage: "/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_0.png"
date: 2024-07-12 23:09
ogImage: 
  url: /assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_0.png
tag: Tech
originalTitle: "Enhancing Stock Return Predictions in the S,P 500: A Comparative Study of Random Forests and LSTM Networks"
link: "https://medium.com/@redeaddiscolll/enhancing-stock-return-predictions-in-the-s-p-500-a-comparative-study-of-random-forests-and-lstm-8fd030ceafcd"
isUpdated: true
---





이 기사에서는 S&P 500 지수의 주식 가격 움직임을 미래 예측하는 데 얼마나 잘 할 수 있는지 확인하기 위해 랜덤 포레스트(Random Forests)와 LSTM 네트워크(CuDNNLSTM) 두 가지 방법을 시험해 보았습니다. 우리는 1993년 1월부터 2018년 12월까지의 데이터를 살펴보았는데, 이는 주간 거래를 위한 것입니다. 우리의 접근 방식은 여러 주식의 종가, 시가 그리고 주간 수익을 고려하는 것이었습니다.

우리의 거래 전략은 2017년과 2018년 Krauss 및 동료들의 연구에서 설명된 방법을 따랐습니다. 매 거래일, 우리는 주가가 잘 될 것으로 예상되는 10개의 주식을 사고, 주가가 좋지 않을 것으로 예상되는 10개의 주식을 공매도했습니다. 이는 주간 수익에 기반하여 수행되었습니다. 우리는 각 주식에 동일한 금액을 투자했습니다.

우리의 결과는 여러 주식 특징을 사용하여, 거래 비용을 고려하기 전에 LSTM 네트워크를 통해 매일 0.64%의 수익을 얻었으며, 랜덤 포레스트를 통해 0.54%의 수익을 얻었습니다. 이러한 결과는 Fischer & Krauss(2018) 및 Krauss et al.(2017)의 연구에서 사용된 단일 특징 접근 방식보다 우수했습니다. 이들 연구에서는 종가에 기반하여 주간 수익만을 고려했었는데, 그 결과 LSTM과 랜덤 포레스트의 매일 수익은 각각 0.41%와 0.39%였습니다.

지난 10년 동안 기계 학습은 금융 트렌드를 예측하는 능력이 향상되었습니다. 인공신경망과 의사결정 방법을 사용하여 주식 가격을 예측하는 전략들이 있습니다. 다른 전략들은 인공지능과 서포트 벡터 머신을 사용하여 비트코인 가격을 예측하는 것을 포함합니다. 이 논문은 LSTM(Long Short-Term Memory) 네트워크와 랜덤 포레스트를 사용하여 주식 가격을 예측하는 것에 초점을 맞추었습니다. 이전 연구들은 LSTM이 ARIMA와 같은 다른 모델보다 주식 가격을 더 잘 예측할 수 있다고 보여주었습니다. 랜덤 포레스트 또한 주식 수익률을 예측하는 데 성공을 거두었습니다. 우리는 S&P 500에서의 데이터를 활용하여 우리의 모델이 일일 수익 측면에서 이전 연구들을 능가한다는 것을 발견했습니다. 오늘 논문의 나머지 부분에서는 저희의 데이터와 방법론을 더 자세히 설명할 것입니다.

<div class="content-ad"></div>

# 데이터와 기술

이 섹션에서는 우리가 사용한 정보와 기술에 대해 이야기합니다. 우리는 S&P 500 지수의 모든 주식 가격을 수집했습니다. 이 데이터는 Bloomberg에서 얻었습니다. 이 가격들은 1990년 1월부터 2018년 12월까지의 기간을 대상으로 합니다. 우리는 특정 날에 거래가 없는 주식은 포함시키지 않았습니다.

저희의 실험에는 강력한 NVIDIA Tesla V100 그래픽 처리 장치를 사용했습니다. 이 장치는 30 기가바이트의 메모리를 가지고 있습니다. 프로그래밍 및 시뮬레이션에는 Python 버전 3.6.5를 사용했습니다. 또한 머신 러닝 작업을 위해 TensorFlow 버전 1.14.0 및 scikit-learn 버전 0.20.4를 사용했습니다. 그래픽 생성 및 통계 계산을 위해 MATLAB R2016b의 금융 도구상자를 사용했습니다.

# 방법론

<div class="content-ad"></div>

이 섹션에서는 저희가 이 기사에서 따랐던 단계들을 설명하겠습니다. 주요 다섯 단계가 있습니다. 먼저, 우리는 원본 데이터를 가져와 다른 시기로 분할합니다. 각 기간은 훈련 부분과 거래 부분으로 나누어집니다. 여기서 우리는 데이터 내에서 거래를 수행하고, 예측을 하게 될 거래 부분을 만들게 됩니다.

다음으로, 분석에 사용할 특징들을 선택합니다.

그 다음에는 예측하고자 하는 목표를 결정합니다.

이후에는 사용한 두 가지 머신러닝 방법인 Random Forest와 CuDNNLSTM을 설정하는 방법을 설명하겠습니다.

<div class="content-ad"></div>

마침내 데이터의 거래 파트에 사용할 거래 전략을 작성했습니다.

# 데이터셋 생성

이 섹션에서는 우리가 겹치지 않는 테스트 기간을 가진 데이터셋을 생성하는 방법에 대해 설명합니다. 우리는 Krauss et al. (2017)과 Fischer & Krauss (2018)의 방법을 따랐습니다. 데이터셋은 1990년 1월부터 2018년 12월까지 29년을 커버합니다. 우리는 4년 창과 1년 스트라이드를 사용하여 데이터셋을 다양한 기간으로 나눴습니다. 각 기간은 약 3년의 교육 기간과 약 1년의 거래 기간으로 더 나눴습니다. 이것에 따라 1년 간격으로 26개의 전문 기간이 생겼는데, 각각이 겹치지 않는 거래 부분을 가지고 있습니다.

![이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_0.png)

<div class="content-ad"></div>

이 과정을 보여주는 그림을 첨부했습니다. 이 그림은 D1부터 D26으로 레이블이 지정된 시간대를 보여줍니다. 예를 들어, 첫 번째 연구 기간(D1)은 특징 생성에 1990년부터 1992년을 사용하고, 교육에 1991년부터 1993년을 사용하며, 시험에는 1993년을 사용했습니다. 이 패턴은 1년씩 계속됩니다. 그래서 두 번째 연구 기간(D2)은 1991년부터 1993년을 특징 생성에 사용하고, 1992년부터 1994년을 교육에 사용하며, 1994년을 시험에 사용했습니다. 이 접근 방식은 시험 기간이 겹치지 않도록하여 교육 및 시험 데이터 간에 명확한 구분이 만들어졌습니다. 이 그림은 또한 연구의 최종 기간인 D24부터 D26을 강조하는데, 이 기간은 2013년부터 2018년까지의 연도를 특징 생성, 교육 및 시험 세그먼트로 분할했습니다. 이 체계적인 분할은 29년의 전체 기간 동안 데이터 세트를 포괄적으로 분석할 수 있게 했습니다.

# 특징 선택

이 섹션에서는 연구를 위해 특징을 선택하는 것에 대해 이야기하고 있습니다. 연구 기간의 총 날짜 수를 T_study로 가정해 봅시다. 또한, 각 연구 기간 "i"의 끝에 "S"라는 집합에 있는 종목 중 완전한 기록 데이터를 가진 주식의 수를 n^i로 정의해 봅시다. 특정 시간 "t"에 집합 "S"에 있는 주식 "s"에 대해 조정 종가를 다음과 같이 정의합니다.

![그림](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_1.png)

<div class="content-ad"></div>

그리고 시작 가격이

![이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_2.png)

우리가 특정 예측 날짜를 볼 때

![이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_3.png)

<div class="content-ad"></div>

안녕하세요! 예측을 위해 필요한 정보가 있습니다.

우리는 "tau"를 포함한 0부터 "t"까지의 시간에 따른 역사적인 개장 가격을 갖고 있습니다.

![이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_4.png)

예측 날짜의 개장 가격을 함께 포함해서요.

<div class="content-ad"></div>

Oh, and don't forget about the historical adjusted closing prices, available for times "t" from 0 to "tau-1" excluding the closing price on the prediction day. 

Here's how the Markdown format would look:

![Enhancing Stock Return Predictions](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_5.png)

And the historical adjusted closing prices:

![Historical Adjusted Closing Prices](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_6.png)

<div class="content-ad"></div>

**Task:** 총 "n"개의 주식 중에서 가장 높은 및 낮은 일중 수익을 예측해야 합니다. 일중 수익 "ir_tau,0"은 예측일의 종가 "cp_tau"를 예측일의 시가 "op_tau"로 나눈 비율에 1을 빼준 값으로 계산됩니다. 간단히 말해, 다음과 같이 표현할 수 있습니다:

![](https://takeafastbreak.github.io/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_8.png)

## 랜덤 포레스트로부터 특성 생성

<div class="content-ad"></div>

이 섹션에서는 주식 데이터를 사용하여 랜덤 포레스트 모델의 특성을 만드는 방법에 대해 이야기하겠습니다. 우리는 랜덤 포레스트에 주식의 세 가지 종류의 신호를 제공합니다. 이를 s라고 하며, 241에서 t 사이의 어떤 시간 t에 해당합니다:

![Image 1](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_9.png)

1. Intraday returns(일중 수익): 이를 계산하려면 시간 t에서의 개장 가격을 시간 t에서의 마감 가격에서 뺀 다음, 시간 t에서의 개장 가격으로 나누고, 그 결과에서 1을 빼줍니다. 이는 다음과 같이 표현됩니다:

![Image 2](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_10.png)

<div class="content-ad"></div>

2.  지난 종가에 대한 수익률: 이를 계산하려면 시간 t-1의 종가에서 시간 t의 종가를 뺀 다음 시간 t의 종가로 나눈 다음 1을 빼면 됩니다. 이는 다음과 같은 방정식으로 나타낼 수 있습니다.

![equation 1](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_11.png)

3.  시가 대비 수익률: 이를 계산하려면 시간 t-1의 종가를 시간 t의 시가에서 빼고, 그 결과를 시간 t-1의 종가로 나누어 1을 빼면 됩니다. 이는 다음과 같은 방정식으로 나타낼 수 있습니다.

![equation 2](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_12.png)

<div class="content-ad"></div>

여기서 m값은 1에서 20 또는 40에서 240 사이의 범위에 있을 수 있으며, 총 93개의 특징을 제공합니다. 이 방법은 Takeuchi & Lee (2013) 및 Krauss 등 (2017)이 사용한 방법과 유사하며, 단일 특징을 고려한 단순한 수익 계산식을 사용합니다.

![image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_13.png)

첫 번째 달에는 매 거래일의 수익을 계산하고, 이후 11개월 동안은 각 달에 대한 다기간 수익만을 고려합니다. Random Forest의 경우 스케일링이나 중심화와 같은 변환은 수행하지 않습니다.

![image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_14.png)

<div class="content-ad"></div>

Figure 2는 랜덤 포레스트를 위해 생성된 특징들을 보여줍니다. 여러 시간대에서 시가 op와 종가 cp 간의 관계를 보여줍니다. 이러한 가격을 사용하여, 우리는 일중 수익률 ir, 지난 종가에 대한 수익률 cr 및 시가에 대한 수익률을 계산합니다. 목표 특징,

![image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_15.png)

, 다음과 같이 계산됩니다

![image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_16.png)

<div class="content-ad"></div>

**결정 지점에서 알려지지 않음.** 다이어그램으로 어떻게 이러한 특징이 생성되고 Random Forest 모델에서 사용되는지 시각적으로 보여줍니다.

**LSTM을 위한 특성 생성**

**“LSTM을 위한 특성 생성”** 부분에서 우리는 Long Short-Term Memory (LSTM) 모델을 훈련시키기 위한 데이터를 준비하는 방법을 사용합니다. 이 방법은 Fischer & Krauss (2018)의 작업을 바탕으로 합니다. 하나의 특성 대신 여러 특성을 사용합니다. 241번째 intraday 수익률의 방향을 예측하기 위해 모델을 240 단계와 세 개의 특성으로 훈련합니다.

각 주식에 대해 특정 시간 \( t \)에 대해 세 가지 특성을 고려합니다:

<div class="content-ad"></div>

안녕하세요! 이전에 정의한 특징들을 사용하여 위와 같은 방법으로 표준화합니다. Robust Scaler 방법을 사용하여 이러한 기능들을 표준화합니다. 표준화를 위한 방정식은 다음과 같습니다:

\[ \frac{x_i - Q_1(X)}{Q_3(X) - Q_1(X)} \]

언제든지 더 도움이 필요하시면 알려주세요. 감사합니다! 🌟

<div class="content-ad"></div>

이 기능의 첫 번째, 두 번째 및 세 번째 사분위수는 다음 이미지에서 확인할 수 있어요:

![사분위수 이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_19.png)

![사분위수 이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_20.png)

![사분위수 이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_21.png)

<div class="content-ad"></div>

Pedregosa et al. (2011)에 설명된 The Robust Scaler 표준화 방법은 데이터에서 중위수를 뺀 다음 사분위 범위를 사용하여 스케일링 하는 것을 포함합니다. 이는 데이터의 이상치를 처리하는 데 도움이 됩니다.

각 시점 t마다, 240개의 연속적이고 세 가지 표준화된 특징으로 이루어진 겹치는 일련의 시퀀스를 생성합니다. 이러한 특징은 아래와 같이 라벨이 지정됩니다:  

<div class="content-ad"></div>

이미지 태그는 다음과 같이 정의됩니다:

![이미지](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_24.png)

set 내의 i에 대해

<div class="content-ad"></div>

Figure 3는 LSTM을 위한 특성 생성 과정을 보여줍니다. 이 그림은 세 개의 특성을 가진 시계열 데이터(훈련 데이터)를 보여줍니다.

![image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_25.png)

각 행은 다른 시간 단계를 나타내며, 열은 각 시간 단계에서의 세 가지 특성을 나타냅니다. 모델이 예측하려는 대상 변수는 241번째 시간 단계에서의 특성입니다.

<div class="content-ad"></div>

# 훈련 및 테스트 분할

여기에서는 주어진 집합 \(S\)에서 각 주식에 대해 데이터를 훈련 및 테스트 세트로 어떻게 나누는지에 대해 이야기하겠습니다. 먼저, 특정 수의 열과 행을 가진 각 주식을 위한 행렬을 생성합니다. 열의 수는 특징(feature)을 나타내며, 93 또는 240 중 하나입니다. 우리는 해당 특징으로 이 행렬을 채우게 됩니다.

그러나, 해당 특징에 대한 규칙이 있습니다.

<div class="content-ad"></div>

`is not defined`이란 오류가 발생합니다.

![그림](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_28.png)

이는 처음 240개 행의 열이 부분적으로만 채워졌거나 삭제되었음을 의미합니다. 결과적으로 우리는 t = 241부터 t = 1008까지의 행들만 남게 됩니다. 이들을 두 부분으로 나눕니다: 훈련용 241부터 756까지의 행들과 테스트용 757부터 1008까지의 행들입니다.

다음으로, \(S\)에 있는 모든 주식의 훈련 데이터를 결합하여 합동 훈련 세트를 생성합니다. 이 세트는 약 500개의 행과 516개의 열이 있으며, 총 258,000개의 인스턴스가 있습니다. 반면 테스트 세트는 약 500개의 행과 252개의 열이 있어서 126,000개의 인스턴스를 가집니다. 두 세트 모두 기능을 나타내는 동일한 수의 열을 갖고 있습니다.

<div class="content-ad"></div>

이 프로세스를 설명하는 도표가 있습니다. 이 행렬은 다양한 특징을 기반으로 섹션으로 나뉘어 있습니다. 행은 1부터 1008까지 레이블이 붙어 있습니다. 처음 240개의 행은 빨간색으로 표시되어 있어서 특징을 생성하는 데 사용되지만 교육 또는 테스트 세트에는 포함되지 않습니다. 다음으로, 241부터 756까지의 행은 파란색으로 표시되어 교육 데이터를 나타냅니다. 마지막으로, 757부터 1008까지의 행은 초록색으로 표시되어 테스트 데이터를 나타냅니다.

그리고, 주식의 일중 수익률이 평균보다 우수하게 성과를 내는지 여부를 나타내는 대상 변수가 표시되어 있습니다. "1"로 레이블이 지정된 경우가 평균보다 더 나은 실행을 하고 있는 것을 의미하며, "0"으로 레이블이 지정된 경우가 그렇지 않은 것을 나타냅니다.

<div class="content-ad"></div>

# 타로 전문가안녕하세요,

2013년 Takeuchi와 Lee 그리고 2018년 Fischer와 Krauss가 사용한 동일한 방법을 따라가고 있어요.

우선, 특정 시간에 각 주식의 일중수익률을 살펴봐요. 이를 t로 지칭할 거예요. 특정 주식 s의 t시간의 일중수익률은 다음과 같이 표현돼요:

![image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_31.png)

<div class="content-ad"></div>

그럼, 주식 s의 일일 수익률이 시간 t에 모든 주식의 중앙값과 비교되어요.

만약 주식 s의 일일 수익률이 시간 t에 모든 주식의 중앙값보다 작다면, 주식 s를 Class 0에 넣어요. 반대로, 주식 s의 일일 수익률이 시간 t에 모든 주식의 중앙값보다 크다면, 주식 s를 Class 1에 배정해요.

요약하자면, 시간대별 모든 주식의 일일 수익률 중앙값을 기준으로 주식을 두 그룹으로 분류해요. 이를 통해 서로 다른 주식들의 성과를 분석하고 비교할 수 있어요.

여기서는 Random Forest 모델의 사양에 대해 이야기할 거에요. Random Forest 기술은 1995년 Ho에 의해 처음 소개되었으며, 이후 2001년 Breiman에 의해 확장되었어요. 저희 모델에서는 특정 매개변수를 설정했어요. 우선, 1000개의 의사 결정 트리가 숲에 있어요. 각 트리는 최대 10단계를 가질 수 있어요. 그리고 트리에서의 각 분할에 대해 93개의 특징의 제곱근을 무작위로 선택해요. Random Forest에 대한 보다 자세한 정보는 Krauss et al. (2017)와 Fischer & Krauss (2018)의 연구를 참고할 수 있어요.

<div class="content-ad"></div>

## LSTM 모델 명세

LSTM은 1997년에 Schmidhuber와 Hochreiter에 의해 발명되었습니다. 더 자세한 내용을 알고 싶다면, 2018년 Fischer와 Krauss의 연구를 읽어보세요. LSTM 모델을 훈련하는 데 시간이 오래 걸릴 수 있기 때문에 속도를 높이기 위해 CuDNNLSTMs를 사용합니다. CuDNNLSTMs는 2014년 Chetlur 등에 의해 소개되었으며 GPU를 활용하는 방법으로 만들어졌습니다. GPU는 매우 강력하며 훈련 및 예측을 훨씬 더 빠르게 할 수 있습니다. 2018년 Braun의 연구에 따르면, CuDNNLSTMs를 사용하면 최대 7.2배 빨라질 수 있습니다.

비교를 용이하게 하기 위해 Fischer와 Krauss가 사용한 계획과 동일한 방법을 따릅니다. 구체적으로, CuDNNLSTM을 사용하여 25개의 LSTM 셀이있는 모델을 만듭니다. LSTM 셀 이후에는 오버피팅을 방지하기 위해 드롭아웃 비율이 0.1인 드롭아웃 레이어를 넣습니다. 마지막으로, softmax 함수를 사용하여 최종 출력 확률을 제공하는 2개의 출력 노드가 있는 레이어를 추가합니다.

우리의 모델에는 몇 가지 설정이 있습니다. 분류 작업에 적합한 범주 교차 엔트로피 손실 함수를 사용합니다. 선택한 옵티마이저는 Keras 라이브러리의 기본 학습률인 0.001로 설정된 RMSProp입니다. 배치 크기는 512로 설정되어 있어서 모델을 업데이트하기 전에 512개의 샘플을 처리합니다. 오버피팅을 줄이기 위해 조기 종료를 사용합니다. 10번의 훈련 라운드 동안 검증 손실이 개선되지 않으면 중지합니다. 또한, 20%의 데이터가 훈련 중 검증에 사용되도록 검증 분할을 0.2로 설정합니다.

<div class="content-ad"></div>

## 예상

이 부분에서는 주식이 잘 수익을 내게 될지를 어떻게 예측하고, 어떤 주식을 사고 팔 것인지 결정하는 방법에 대해 이야기합니다. 하루 거래일 동안 특정 주식 s가 평균 수익을 넘을 확률을 추정하는 확률인 Pₜˢ을 사용합니다. 평균 수익은 iₜ,₀ˢ로 표시됩니다.

어떤 주식을 사고 팔 것인지 결정할 때 우리는 Krauss et al. (2017)과 Fischer & Krauss(2018)의 연구를 살펴봅니다. 우리는 최상위 k=10개 주식을 가장 높은 확률로 사고, 동시에 가장 낮은 확률로 최하위 k=10개 주식은 판매합니다. 이전이 각 주식에 동일한 금액을 투자하도록 합니다.

이러한 거래의 비용도 고려합니다. 매매 시 0.05%의 미끄러짐 비용이 발생한다고 가정합니다(Avellaneda & Lee, 2010). 따라서 우리가 매도, 매수를 하는 각 거래에 대해 총 미끄러짐 비용은 0.2%입니다. 이 비용은 우리의 거래가 시장에 미치는 영향에 대한 벌이 같은 역할을 합니다.

<div class="content-ad"></div>

## 결론

이 섹션에서는 프로젝트의 결과와 발견에 대해 논의하겠습니다. 우리의 결과는 시가 및 종가를 포함한 여러 기능을 사용하는 우리의 접근 방식이 이전 연구에서 사용된 방법보다 더 우수하게 수행됨을 보여줍니다. 우리는 우리의 접근 방식을 "IntraDay"라고 하고 이전 접근 방식을 "NextDay"라고 부릅니다. 이러한 결과를 표 1~3과 그림 5~7에서 제시합니다.

우리의 Long Short-Term Memory (LSTM) 모델을 사용하여, 우리의 접근 방식은 0.64%의 일일 수익률을 얻었으며, 이는 이전 연구에서 보고된 0.41%보다 높습니다. 비슷하게, 랜덤 포레스트를 사용하면, 우리의 접근 방식은 다른 연구에서의 0.39% 대비 0.54%의 일일 수익률을 달성합니다. 또한, 우리의 접근 방식이 더 많은 양의 수익을 가지고 있음을 발견했습니다. 게다가, 우리의 접근 방식은 더 높은 샤프 비율과 낮은 표준 편차를 가지고 있으며, 이는 리스크와 수익의 측정 지표입니다. 우리의 접근 방식은 또한 더 낮은 최대 하락폭과 낮은 일일 가치 위험을 가지고 있습니다. 또한, LSTM이 랜덤 포레스트를 능가함을 관찰했습니다.

그림 5~7은 서로 다른 시기에 대한 결과를 보여줍니다. 각 기간마다, 우리의 접근 방식이 이전 접근 방식보다 더 나은 성과를 거둡니다.

<div class="content-ad"></div>

다양한 특징을 사용하는 중요성을 강조하기 위해, 우리는 단일 기능으로 인트라데이 수익을 사용하여 인트라데이 거래의 성능을 분석했습니다. 다양한 특징을 사용할 때 우리의 접근 방식이 훨씬 우수한 성과를 거두는 것을 발견했습니다.

향후 기사에서는 주식이나 통화 움직임을 예측하기 위한 강화학습 접근법을 탐구하는 것이 흥미로울 것입니다, 특히 암호화폐의 맥락에서 LSTM과 랜덤 포레스트에 대해.

![Image](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_32.png)

표 1은 다른 설정에 대한 시간 비교를 제공합니다. 모델을 학습하고 결정을 내리는 데 걸리는 시간을 보여줍니다. 예를 들어, 3개 특징을 사용하는 인트라데이 LSTM 설정에서, 각 epoch마다 약 33.1초가 소요되며, 학습 시간은 약 24.21분, 결정 시간은 약 0.086924초입니다.

<div class="content-ad"></div>

**이미지 주소(링크):**

- ![링크](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_33.png)

- ![링크](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_34.png)

**테이블 2:**

일일 수익에 대한 성능 지표를 비거래 비용을 고려하지 않은 상태로 제시합니다. 평균 수익, 표준 편차, 최솟값 및 최댓값, 다양한 리스크 측정값 등이 포함됩니다. 예를 들어, 3개의 기능을 갖는 IntraDay LSTM 설정에서 평균 수익은 0.00332이며 Sharpe 비율은 6.3425입니다.

- ![링크](/assets/img/2024-07-12-EnhancingStockReturnPredictionsintheSP500AComparativeStudyofRandomForestsandLSTMNetworks_35.png)

<div class="content-ad"></div>

우리의 연구 전반은 하나의 특징을 사용하는 것과 비교하여 여러 특징을 사용하는 것이 주식 수익 예측 성능을 크게 향상시킨다는 것을 나타냅니다.