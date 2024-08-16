---
title: "기계 학습 트레이딩 봇 CNN 모델을 사용한 3년간 9,883 수익과 최고의 암호화폐 자산 선정 방법"
description: ""
coverImage: "/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_0.png"
date: 2024-07-09 23:57
ogImage: 
  url: /assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_0.png
tag: Tech
originalTitle: "9,883+% Returns in 3 years on Cryptocurrency using 2D Convolutional Neural Network (CNN) Model and short listing Best Assets for Trading — VishvaAlgo Machine Learning Trading Bot"
link: "https://medium.com/@imbuedeskpicasso/9-883-returns-in-3-years-on-cryptocurrency-using-2d-convolutional-neural-network-cnn-model-and-2105ee7e2893"
isUpdated: true
---





## 최대 수익을 위한 트레이딩 봇을 만들기 위해 신경망의 힘을 발휘하다.

# 소개:

알고리즘 트레이딩과 머신 러닝의 세계에 오신 것을 환영합니다. 혁신과 수익성이 만나는 곳입니다. 지난 3년 동안 다양한 전략의 힘을 활용하는 알고리즘 트레이딩 시스템 개발에 전념해왔습니다. 끊임없는 실험과 세련됨으로 인해, 여러 전략에서 인상적인 수익을 거두어내며 Patreon 커뮤니티 회원들에게 일관된 수익을 제공하며 만족시켰습니다.

우수성을 추구하는 과정에서, 최근에 저는 VishvaAlgo를 선보였습니다. 신경망 분류 모델을 활용한 머신러닝 기반의 알고리즘 트레이딩 시스템입니다. 이 차세대 플랫폼은 이미 탁월한 결과를 보여주며, 암호화폐 시장에서 거래자들에게 놀라운 수익을 제공하고 있습니다. 일련의 기사와 실제 시연을 통해 전통적인 알고리즘 트레이딩에서 현실적인 머신 러닝 모델로 전환하는 방법에 대한 통찰을 공유하며, 실제 거래 환경에서의 효과를 보여주고 있습니다.

<div class="content-ad"></div>

이 기사에서는 알고리즘 거래와 머신 러닝의 변혁적 잠재력에 대해 탐구합니다. 특히, 합성곱 신경망(CNN) 모델의 효과에 초점을 맞추며, 이전의 성공을 바탕으로 비트코인(BTC)과 이더리움(ETH)을 주요 자산으로 활용하여 고도의 머신 러닝 모델을 통해 실현 가능한 놀라운 수익성을 증명하려 합니다.

우리의 분석은 USDT로 표시된 이더리움 가격에 초점을 맞추며, 2021년 1월 1일부터 2023년 10월 22일까지의 15분봉 캔들스틱 데이터를 사용합니다. 이 기간에는 97,000개 이상의 데이터 행과 190개 이상의 특징이 포함되어 있습니다. 우리는 예측을 위해 신경망 모델을 활용하여 최적의 롱 포지션과 숏 포지션을 식별하고, 금융 시장에서 딥 러닝의 잠재력을 보여주고자 합니다.

![그림](/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_0.png)

![그림](/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_1.png)

<div class="content-ad"></div>

# 우리의 알고리즘 트레이딩 대 머신 러닝 대 딥 러닝 여정은 지금까지?

## 단계 1:

우리는 크립토 알고리즘 전략을 개발했으며, 3년 동안 (거의) 수많은 크립토 자산(138개 이상)에서 실행했을 때 수익률 범위가 8787% 이상인 막대한 수익을 얻었습니다.

## "암호 화폐 선물을 위한 8787% 이상 ROI 알고리즘 전략 공개! 유명한 RSI, MACD, 볼린저 밴드, ADX, EMA로 혁신되었습니다" — 링크

<div class="content-ad"></div>

7일 동안 동일한 내용으로 라이브 트레이딩을 드라이런 모드로 실행했고, 관련 내용은 다른 글에서 공유되었습니다.

## “Freqtrade Revealed: 7-Day Journey in Algorithmic Trading for Crypto Futures Market” — [링크](링크)

성공적인 백테스트 결과와 포워드 테스트(드라이런 모드의 라이브 트레이딩) 이후, 동일한 것에 대해 더 많은 수익을 올리기 위한 가능성을 높이기로 했습니다. (손절률 줄이기, 더 많이 이기는 가능성 높이기, 리스크 요소 줄이기, 그리고 기타 중요한 사항들)

## Stage 2:

<div class="content-ad"></div>

우리는 Freqtrade 설정 없이 독자적으로 전략을 개발해 왔습니다 (트레일링 스탑 로스를 피하며, 여러 자산을 병렬로 운용하며, 더 높은 위험 관리 설정을 무료로 제공하는 freqtrade 플랫폼에서 얻을 수 있는 것을 피하며). 시장에서 시험하고, 하이퍼 파라미터를 튜닝하여 최적화한 후, 전략에서 양의 수익을 얻게 되었습니다.

## "다양한 알고리즘 트레이딩 봇에 대해 백테스팅에서 3000% 이상의 수익을 달성한 방법, 그리고 당신의 거래 전략에서 동일한 방법을 사용하는 방법 — 파이썬 코드 활용" — 링크

## 단계 3:

우리는 암호화폐 시장의 BTC/USDT 자산만을 테스트해본 상태입니다. 이전에 Freqtrade 전략을 개발할 때 사용한 총 자산을 나누어, 그들의 변동성에 따라 서로 다른 클러스터로 나눌 수 있는지 알고 싶었습니다. 특정하게 변동성이 높은 자산에 대해 거래를 할 때 쉬워지며, 코인의 변동성을 기반으로 구현하면 다른 자산에 대한 큰 손실을 피할 수 있습니다.

<div class="content-ad"></div>

우리는 Freqtrade 전략에서 사용하는 138가지 암호 자산 중 서로 다른 클러스터를 식별하기 위해 K-Nearest Neighbors(KNN Mean)를 사용했습니다. 백테스트 중에 8000% 이상의 수익을 올린 것이죠.

## "하이퍼 최적화된 알고리즘 전략 대 머신 러닝 모델 파트-1 (K-Nearest Neighbors)" — 링크

## 스테이지 4:

이제 우리는 또 다른 무지도 학습 모델인 숨겨진 마르코프 모델(HMMs)을 소개하고자 합니다. 이 모델은 시장의 추세를 식별하고, 유리한 추세에서만 거래를 하고 시장에서의 급등, 급락을 피하며 부정적인 추세 또한 피하려고 합니다. 아래 설명에서 이에 대해 자세히 살펴보겠습니다.

<div class="content-ad"></div>

## "하이퍼 최적화된 알고리즘 전략 대 머신 러닝 모델 파트 -2 (숨겨진 마르코프 모델 - HMM)" - 링크

## 단계 5:

XGBoost Classifier를 사용하여 옛 신호를 이용해 장기 및 단기 거래를 식별하는 작업을 진행했습니다. 이를 사용하기 전에 이전에 개발한 신호 알고리즘이 하이퍼 최적화되었는지 확인했습니다. 또한, 이 설정을 위해 서로 다른 손실 중지 및 이익 실현 매개변수를 도입하여 대상 값이 그에 따라 변경되도록 했습니다. 또한, 손실 중지 및 이익 실현 값을 기반으로 이익 거래를 얻기 위해 사용된 매개변수를 조정했습니다. 이후, 기본 XGBClassifier 설정을 시험한 뒤, 결과를 향상시키기 위해 재표본화 방법을 추가했습니다. 거래 실행 시기로 인해 0(중립), 1(장기 거래용), 2(단기 거래용)를 포함하는 대상 클래스가 불균형했습니다. 이 불균형을 해결하기 위해 재표본화 방법을 활용하고 분류기 모델의 하이퍼 최적화를 수행했습니다. 이후, 다른 분류기 모델인 SVC, CatBoost, LightGBM과 LSTM, XGBoost를 조합하여 모델이 더 나은 성능을 발휘하는지를 평가했습니다. 마지막으로, 결과를 분석하고 가장 생산적인 특징을 식별하기 위한 중요한 특징 매개변수를 결정하는 것으로 결론 내렸습니다.

## "하이퍼 최적화된 알고리즘 전략 대 머신 러닝 모델 파트 -3 (XGBoost Classifier, LGBM Classifier, CatBoost Classifier, SVC, LSTM with XGB 및 다단계 하이퍼 최적화)" - 링크

<div class="content-ad"></div>

## Stage 6:

For this stage, I've delved into utilizing the CatBoostClassifier alongside resampling and sample weights. Incorporating various time frame indicators like volume, momentum, trend, and volatility has been pivotal in enriching the model. Post model execution, I applied ensembling techniques to further boost its performance. The analysis yielded remarkable results, showcasing a tremendous profit surge from 54% to a whopping 4600% during backtesting. Moreover, the model exhibited outstanding performance metrics with recall, precision, accuracy, and F1 score all surpassing 80% across the three trading classes (0 for neutral, 1 for long, and 2 for short trades).

## "From 54% to a Staggering 4648%: Catapulting Cryptocurrency Trading with CatBoost Classifier, Machine Learning Model at Its Best" — [Link]

## Stage 7:

<div class="content-ad"></div>

이 단계에서는 TCN과 LSTM 신경망 모델을 결합한 앙상블 방법이 다양한 데이터셋에서 우수한 성능을 보여주며 개별 모델을 능가하고 매수 및 보유 전략조차 능가하고 있습니다. 이는 앙상블 학습의 효과적인 예측 정확도와 견고성 향상을 강조합니다.

**"비트코인/BTC 4750%+, 이더리움/ETH 11,270%+ 이익, Neural Networks, Algorithmic Trading Vs/+ Machine Learning Models Vs/+ Deep Learning Model Part — 4 (TCN, LSTM, Transformer with Ensemble Method)" — 링크**

**단계 8:**

VishvaAlgo v3.8로 미래의 거래를 경험하세요. 고급 기능, 비할 데 없는 리스크 관리 능력, ML 및 신경망 모형의 쉬운 통합을 갖춘 VishvaAlgo는 일관된 이익과 안정감을 추구하는 거래자들에게 최상의 선택입니다. 거래 여정을 혁신하고 싶은 거래자들에게 이 기회를 놓치지 마세요.

<div class="content-ad"></div>

# “VishvaAlgo v3.0 — Live Cryptocurrency Trading에 빛을 발하는 변화와 기계학습(신경망) 모델 강화. 라이브 수익 화면 공유” — 링크

# CNN과 시계열 분류 소개

합성곱 신경망(Convolutional Neural Networks, CNN)은 주로 이미지와 비디오와 같은 격자 형식의 데이터를 처리하기 위해 설계되어 있습니다. 이미지와 비디오 처리에 매우 효과적이라는 것이 증명되었습니다. CNN은 입력 데이터를 스캔하고 공간적인 특성의 계층을 학습하는 합성곱 필터를 적용하여 작동합니다. 일반적으로 이미지와 비디오 처리에 사용되지만, 지역적인 패턴과 추세를 포착할 수 있는 능력 때문에 시계열 데이터에 적응시킬 수도 있습니다.

# 이미지 및 비디오 처리를 위한 CNN

<div class="content-ad"></div>

- 이미지 처리: CNN은 객체를 식별하고 얼굴을 감지하며 이미지 내에서 패턴을 인식하는 데 사용됩니다. 이들은 일련의 합성곱 계층, 풀링 계층 및 완전히 연결된 계층을 적용하여 특징을 추출하고 예측을 수행합니다.
- 비디오 처리: 비디오 데이터에서 CNN은 행동 인식 및 비디오 분류와 같은 작업에 사용될 수 있습니다. 각 프레임을 이미지로 처리하고 시계열 계층을 사용하여 프레임의 순서를 캡처할 수 있습니다.

# 시계열 데이터에 대한 CNN

일반적으로 이미지 및 비디오 처리에 사용되지만, CNN은 시계열 분류에 대해 매우 효과적일 수 있습니다. 다음은 그 방법입니다:

- 특징 추출: CNN은 시계열 데이터에서 시간적인 특징을 추출하여 트렌드, 계절성 및 이상을 식별할 수 있습니다.
- 지역적 패턴 인식: 합성곱 필터는 시계열 데이터 내에서 지역적 패턴을 포착하여 단기간 트렌드와 변동이 중요한 금융 데이터에 중요합니다.
- 차원 축소: 풀링 계층은 데이터의 차원을 축소시켜주며 최우선적인 특징을 유지하면서 계산 복잡성을 줄일 수 있습니다.

<div class="content-ad"></div>

# 2차원 CNN으로 이더리움(ETH) 데이터 다중 분류하기

## 데이터 설명

- 자산: 이더리움(ETH)
- 시간 간격: 15분 단위
- 행: 10만개 이상
- 특성: 193개 이상 (예: OHLCV, 기술적 지표)

## 모델 아키텍처 및 훈련

<div class="content-ad"></div>

- 입력 형태: 모델의 입력 데이터는 2D 형식으로 구성되며, (샘플 수, 타임 스텝 수, 특성 수)의 모양을 갖습니다.
- 합성곱 레이어: 이러한 레이어는 입력 데이터 전체에 합성곱 필터를 적용하여 지역적인 시간적 특성을 학습합니다.


x = Conv2D(filters=64, kernel_size=(3, 3), activation='relu')(inputs)


3. 풀링 레이어: 이러한 레이어는 특성 맵의 차원을 줄이면서 중요한 정보를 유지합니다.


x = MaxPooling2D(pool_size=(2, 2))(x)


<div class="content-ad"></div>

4. 밀집 레이어: 완전 연결 레이어는 합성곱 레이어에서 추출된 특징을 해석합니다.

```python
x = Flatten()(x) 
x = Dense(units=128, activation='relu')(x)
```

5. 출력 레이어: 최종 레이어는 각 클래스(중립, 롱, 숏)에 대한 확률을 출력하기 위해 소프트맥스 활성화 함수를 사용합니다.

```python
outputs = Dense(3, activation='softmax')(x)
```

<div class="content-ad"></div>

## 모델 훈련

모델은 레이블이 지정된 시계열 데이터를 사용하여 훈련됩니다. 각 시계열 세그먼트는 0 (중립), 1 (길게), 또는 2 (짧게)로 레이블이 지정됩니다.

```javascript
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
model.fit(X_train, y_train, epochs=50, batch_size=64, validation_split=0.2)
```

## 예제 코드

<div class="content-ad"></div>

여기 ETH 시계열 데이터의 다중 분류를 위한 2D CNN의 완전한 예시가 있어요:

```js
import numpy as np
import keras
from keras.models import Model
from keras.layers import Input, Conv2D, MaxPooling2D, Flatten, Dense, Dropout
from keras.utils import to_categorical
from sklearn.model_selection import train_test_split

# 데이터 시뮬레이션
X = np.random.rand(100000, 15, 193, 1)  # 100,000 샘플, 15 타임스텝, 193 피처, 1 채널
y = np.random.randint(3, size=100000)   # 100,000 레이블 (0, 1, 2)
# 레이블을 원핫 인코딩으로 변환
y = to_categorical(y, num_classes=3)
# 데이터를 학습 및 검증 세트로 분할
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=42)
# 모델 구축
inputs = Input(shape=(15, 193, 1))
x = Conv2D(filters=64, kernel_size=(3, 3), activation='relu')(inputs)
x = MaxPooling2D(pool_size=(2, 2))(x)
x = Flatten()(x)
x = Dense(units=128, activation='relu')(x)
x = Dropout(0.5)(x)
outputs = Dense(3, activation='softmax')(x)
model = Model(inputs=inputs, outputs=outputs)
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
# 모델 훈련
model.fit(X_train, y_train, epochs=10, batch_size=64, validation_data=(X_val, y_val))
```

# 시계열 분류를 위한 CNN의 효과

- 패턴 인식: CNN은 시계열 데이터의 패턴을 효과적으로 인식하고 파악하여 금융 시장 분석에 적합합니다.
- 속도: CNN은 대규모 데이터셋을 효율적으로 처리할 수 있어 고빈도 거래 전략에 중요합니다.
- 정확도: 적절히 조정된 경우, CNN은 시장 움직임을 예측하는 데 높은 정확도를 달성하여 거래자가 정보에 기반한 결정을 내릴 수 있게 도와줍니다.

<div class="content-ad"></div>

요약하자면, CNN은 기존에는 이미지 및 비디오 처리에 사용되었지만, 지역적 패턴을 포착하고 차원을 감소시키는 능력으로 인해 금융 시장에서의 시계열 분류에 매우 효과적입니다. 이러한 적응성은 암호화폐 시장과 같은 시장 움직임을 예측할 수 있는 강력한 모델을 가능하게 하며, 롱(long), 숏(short) 및 중립(neutral)과 같은 포지션 분류를 지원합니다.

# 전체 코드 설명:

```js
# 미래 경고 제거
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)

# 성능 경고 억제
warnings.filterwarnings("ignore")
# 일반
import numpy as np
# 데이터 관리
import pandas as pd
# 머신러닝
from catboost import CatBoostClassifier
from sklearn.model_selection import train_test_split
from sklearn.model_selection import RandomizedSearchCV, cross_val_score
from sklearn.model_selection import RepeatedStratifiedKFold
from sklearn.linear_model import LogisticRegression
# 앙상블
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.ensemble import StackingClassifier
from sklearn.ensemble import VotingClassifier
# 샘플링 방법
from imblearn.over_sampling import ADASYN
# 스케일링
from sklearn.preprocessing import MinMaxScaler
# 이진 분류 특정 메트릭
from sklearn.metrics import RocCurveDisplay as plot_roc_curve
# 일반 메트릭
from sklearn.metrics import accuracy_score
from sklearn.metrics import precision_score
from sklearn.metrics import confusion_matrix, classification_report, roc_curve, roc_auc_score, accuracy_score
from sklearn.metrics import precision_score
from sklearn.metrics import ConfusionMatrixDisplay

# 보고
import matplotlib.pyplot as plt
from matplotlib.pylab import rcParams
from xgboost import plot_tree
# 백테스팅
from backtesting import Backtest
from backtesting import Strategy
# 하이퍼옵트
from hyperopt import fmin, tpe, hp
from pandas_datareader.data import DataReader
import json
from datetime import datetime
import talib as ta
import ccxt
# from sklearn.model_selection import train_test_split
from sklearn.utils import class_weight
from keras.models import Sequential
from keras.layers import LSTM, Dense, Dropout
from keras.optimizers import Adam
# from keras.wrappers.scikit_learn import KerasClassifier
from sklearn.ensemble import VotingClassifier
from hyperopt import fmin, tpe, hp, STATUS_OK, Trials
```

Import 문 (1-18행):

<div class="content-ad"></div>

**주의사항 (라인 1~4):**
이 부분은 실행 중 나타날 수 있는 경고 메시지를 억제합니다. 훈련 중단 없이 진행하는 데 도움이 되지만, 보다 효과적인 디버깅 및 잠재적 문제 이해를 위해 경고 자체에 대해 조치하는 것이 좋습니다.

**일반 라이브러리 (라인 5~7):**
- numpy (np): 배열 연산 및 수학 함수에 자주 사용되는 숫자 연산 기능을 제공합니다. 문서 작성에 직접적으로 적용되지는 않습니다.
- pandas (pd): 데이터 조작, 분석 및 시각화에 사용됩니다. 기사에서 구조화된 데이터(예: 표, 차트)를 다루는 데 필수적입니다.

**머신러닝 라이브러리 (라인 8~13):**
- catboost (명시적으로 여기에는 포함되지 않음): 머신러닝 작업을 위한 강력한 그래디언트 부스팅 라이브러리를 제공합니다. 특정 머신러닝 알고리즘을 논의하는 경우를 제외하고는 문서 작성과 직접적으로 관련이 없습니다.
- scikit-learn (다양한 하위 모듈): 포괄적인 머신러닝 라이브러리입니다. 개념 설명이나 접근법 비교를 위해 다음과 같은 부분은 기사에서 유용할 수 있습니다:
   - train_test_split: 모델 평가를 위해 데이터를 학습 및 테스트 세트로 분할합니다.
   - RandomizedSearchCV, cross_val_score, RepeatedStratifiedKFold: 초매개변수 조정 및 모델 평가(cross-validation) 기술에 대한 기법.
   - LogisticRegression: 선형 분류 모델로, 분류 알고리즘을 논의하는 경우 관련성이 있을 수 있습니다.

**앙상블 메소드 (라인 14~16):**
- scikit-learn (하위 모듈): 여러 모델을 결합하여 성능을 향상시키는 기법입니다. 문서 작성과 직접적으로 관련이 없습니다.

**샘플링 방법 (라인 17):**
- imblearn: 클래스 크기가 다른 불균형 데이터셋을 처리하는 데 도움을 주는 도구를 제공합니다. 일반적으로 문서 작성 자체에서 사용되지는 않습니다.

**스케일링 (라인 18):**
- scikit-learn: 데이터를 정규화하거나 표준화하는 기술을 제공합니다. 머신러닝 모델에 필요한 경우가 많은데, 문서에서 데이터 전처리 단계를 설명하는 데 유용할 수 있습니다.

**평가 지표 (라인 19~33):**

**이진 분류 평가 지표 (라인 19~21):**
- scikit-learn: 이진 분류(두 개의 클래스) 모델 성능을 평가하는 데 사용됩니다. 모델 평가 지표를 논의하는 경우를 제외하고는 문서 작성과 직접적으로 관련이 없습니다.

**일반 지표 (라인 22~33):**
- scikit-learn: 다양한 분류 작업에서 모델 성능을 평가하는 데 사용되는 여러 지표가 있습니다. 아래 내용은 모델 평가 방법을 설명하는 데 기사에서 유용할 수 있습니다:
   - accuracy_score: 정확한 예측의 비율.
   - precision_score: 예측된 긍정 중 실제 긍정의 비율.
   - confusion_matrix: 각 클래스에 대해 올바르게 또는 잘못 분류된 인스턴스를 시각화합니다.
   - classification_report: 각 클래스의 precision, recall, F1-score 및 지원을 포함한 모델 성능에 대한 상세 보고서.
   - roc_curve, roc_auc_score: Receiver Operating Characteristic(ROC) 곡선을 평가하는 데 사용되는 측정 항목. 이는 모델이 클래스 간의 차별 능력을 평가하는 데 도움이 됩니다.

**보고 (라인 34~36):**

<div class="content-ad"></div>

- matplotlib.pyplot (plt): 시각화 자료인 차트 및 그래프 작성에 사용됩니다. 기사에서 데이터 및 모델 결과를 제시하는 데 필수적입니다.

Backtesting (37-38행):

- backtesting: 거래 전략을 백테스트하는 라이브러리입니다. 금융 기계 학습의 응용 프로그램에 대해 논의하지 않는 이상 글쓰기에는 관련성이 없습니다.

Hyperparameter Optimization (39-42행):

<div class="content-ad"></div>

- hyperopt: 기계 학습 모델의 최적 설정을 찾는 하이퍼파라미터 튜닝을 위한 라이브러리입니다. 기사 작성에 직접적으로 적용되지는 않아요.

데이터 검색 (43번째 줄):

- pandas_datareader: 다양한 금융 데이터 소스로부터 데이터를 검색하는 것을 용이하게 해주는 라이브러리에요. 보통 기사 작성 자체에는 사용되지 않아요.

다른 임포트 (44~50번째 줄):

<div class="content-ad"></div>

- json: JSON 데이터 형식을 다루기 위한 라이브러리입니다 (여기서 직접적으로 사용되지는 않음).
- datetime: 날짜와 시간 객체를 다루기 위한 라이브러리입니다. 시계열 데이터를 처리하는 데 유용할 수 있습니다.
- talib: 금융 시장을 위한 기술적 분석 라이브러리입니다 (여기서 직접적으로 사용되지는 않음).
- ccxt (여기서 명시적으로 불려지지 않음): 암호화폐 거래소와 상호 작용하는 데 사용되는 라이브러리입니다 (글쓰기에는 관련이 없음).

맥락:
- 각 라이브러리와 모듈은 데이터 수정, 머신 러닝, 평가, 시각화, 백테스팅, 하이퍼파라미터 최적화 등과 같이 특정 목적을 위해 불려집니다.
- 이러한 라이브러리와 모듈은 데이터 전처리, 모델 학습, 평가, 최적화 및 시각화와 같은 다양한 작업에 코드 전반에 걸쳐 사용될 것입니다.


# JSON 파일 경로 정의
file_path = './ETH_USDT_USDT-15m-futures.json'
# 파일 열고 데이터 읽기
with open(file_path, "r") as f:
    data = json.load(f)

df = pd.DataFrame(data)
# OHLC 데이터 추출 (필요에 따라 열 이름 조정)
# ohlc_data = df[["date", "open", "high", "low", "close", "volume"]]
df.rename(columns={0: "날짜", 1: "시가", 2: "고가", 3: "저가", 4: "수정 종가", 5: "거래량"}, inplace=True)
# 타임스탬프를 날짜 시간 객체로 변환
df["날짜"] = pd.to_datetime(df['날짜'] / 1000, unit='s')
df.set_index("날짜", inplace=True)
# 날짜 인덱스 형식 변경
df.index = df.index.strftime("%m-%d-%Y %H:%M")
df['종가'] = df['수정 종가']
# print(df.dropna(), df.describe(), df.info())
data = df
data


<div class="content-ad"></div>

역사적 가상 통화 선물 데이터를 분석하기 위해서, 우선 JSON 파일에서 데이터를 로드할 수 있습니다. 제공된 코드는 Python의 json 라이브러리를 사용하여 JSON 내용을 사전으로 파싱하는 방법을 보여줍니다. 그런 다음 이 사전을 pandas DataFrame으로 변환하여 더 쉬운 조작이 가능합니다. DataFrame은 열 이름 변경, 타임스탬프를 날짜시간 객체로 변환, 날짜를 인덱스로 설정하고 날짜 표시 양식을 변경하여 가독성을 높이도록 정리 및 변환됩니다.

다음은 코드의 단계별 설명입니다:

1. JSON 데이터 로드:

- 코드는 가상 통화 데이터 (아마도 USDT로 거래되는 이더리움 선물 계약의 Open-High-Low-Close-Volume 형식)를 포함하는 JSON 파일에 대한 파일 경로 (file_path)를 정의합니다.
- 파일을 읽기 위해 열고 (with open(file_path, "r") as f:) json.load(f)를 사용하여 JSON 내용을 Python 사전 (data)으로 파싱합니다.

<div class="content-ad"></div>

2. DataFrame으로 변환:

- 이 코드는 로드된 딕셔너리(data)에서 판다스 DataFrame(df)을 생성합니다. DataFrame은 스프레드시트와 유사한 탭 형식의 데이터 구조로, 데이터를 처리하고 분석하는 데 편리합니다.

3. 데이터 정리 및 변환:

- 이 부분은 JSON 데이터가 의미있는 이름이 아닌 숫자 인덱스(0, 1, 2 등)를 가진 열을 갖고 있다고 가정합니다. 이를 df.rename(columns='...', inplace=True)를 사용하여 더 구체적인 레이블("Date", "Open", "High", "Low", "Adj Close", "Volume")로 다시 지정합니다.
- pd.to_datetime()을 사용하여 "Date" 열을 타임스탬프(아마도 어떤 시대부터의 밀리초)에서 날짜 시간 객체로 변환합니다. 이렇게 하면 날짜를 다루고 시간 기반 작업을 수행하는 것이 더 쉬워집니다.
- df.set_index("Date", inplace=True)를 사용하여 DataFrame의 "Date" 열을 색인으로 설정합니다. 이렇게 하면 날짜를 기반으로 데이터에 효율적으로 액세스하고 필터링할 수 있습니다.
- df.index.strftime("%m-%d-%Y %H:%M")을 사용하여 날짜 색인의 형식을 지정하여, 보다 가독성 있는 형식으로 날짜를 표시합니다 (예: "05-14-2024 16:35").
- 마지막으로 "Adj Close"라는 열을 "Close"라는 변수에 할당하여 조금 더 명확한 참조를 위해서입니다.

<div class="content-ad"></div>


가정: 'Open', 'High', 'Low', 'Close', 'Adj Close', 'Volume' 열이 포함된 'df'라는 DataFrame이 있다고 가정합니다.
target_prediction_number = 2
time_periods = [6, 8, 10, 12, 14, 16, 18, 22, 26, 33, 44, 55]
name_periods = [6, 8, 10, 12, 14, 16, 18, 22, 26, 33, 44, 55]
df = data.copy()
new_columns = []

for period in time_periods:
    for nperiod in name_periods:
        df[f'ATR_{period}'] = ta.ATR(df['High'], df['Low'], df['Close'], timeperiod=period)
        df[f'EMA_{period}'] = ta.EMA(df['Close'], timeperiod=period*2)
        df[f'RSI_{period}'] = ta.RSI(df['Close'], timeperiod=period*0.5)
        df[f'VWAP_{period}'] = ta.SUM(df['Volume'] * (df['High'] + df['Low'] + df['Close']) / 3, timeperiod=period) / ta.SUM(df['Volume'], timeperiod=period)
        df[f'ROC_{period}'] = ta.ROC(df['Close'], timeperiod=period)
        df[f'KC_upper_{period}'] = ta.EMA(df['High'], timeperiod=period*2)
        df[f'KC_middle_{period}'] = ta.EMA(df['Low'], timeperiod=period*2)
        df[f'Donchian_upper_{period}'] = ta.MAX(df['High'], timeperiod=period)
        df[f'Donchian_lower_{period}'] = ta.MIN(df['Low'], timeperiod=period)
        macd, macd_signal, _ = ta.MACD(df['Close'], fastperiod=(period + 12), slowperiod=(period + 26), signalperiod=(period + 9))
        df[f'MACD_{period}'] = macd
        df[f'MACD_signal_{period}'] = macd_signal
        bb_upper, bb_middle, bb_lower = ta.BBANDS(df['Close'], timeperiod=period*0.5, nbdevup=2, nbdevdn=2)
        df[f'BB_upper_{period}'] = bb_upper
        df[f'BB_middle_{period}'] = bb_middle
        df[f'BB_lower_{period}'] = bb_lower
        df[f'EWO_{period}'] = ta.SMA(df['Close'], timeperiod=(period+5)) - ta.SMA(df['Close'], timeperiod=(period+35))

df["Returns"] = (df["Adj Close"] / df["Adj Close"].shift(target_prediction_number)) - 1
df["Range"] = (df["High"] / df["Low"]) - 1
df["Volatility"] = df['Returns'].rolling(window=target_prediction_number).std()

# 기반 데이터의 지표
df['OBV'] = ta.OBV(df['Close'], df['Volume'])
df['ADL'] = ta.AD(df['High'], df['Low'], df['Close'], df['Volume'])

# 모멘텀 기반 지표
df['Stoch_Oscillator'] = ta.STOCH(df['High'], df['Low'], df['Close'])[0]
df['PSAR'] = ta.SAR(df['High'], df['Low'], acceleration=0.02, maximum=0.2)

# 모든 열을 표시하기 위해 판다스 옵션 설정
pd.set_option('display.max_columns', None)

# 계산된 지표 출력
print(df.tail())
df.dropna(inplace=True)
print("Length: ", len(df))
df


이 코드는 talib 라이브러리를 사용하여 다양한 기술적 지표를 계산하는 방법을 보여줍니다. 이 코드는 Average True Range (ATR), Exponential Moving Average (EMA), Relative Strength Index (RSI) 등의 지표를 계산합니다. 또한 시각적으로 표시하기 위한 판다스 옵션을 설정하고 계산된 지표를 출력합니다.

<div class="content-ad"></div>

# 데이터 전처리 — 미래 예측 값 추정을 위한 "목표" 값 설정

```js
# 목표 방식 유연하게 설정
pipdiff_percentage = 0.01  # TP에 대한 자산 가격의 1% (0.01)
SLTPRatio = 2.0  # pipdiff/Ratio는 SL을 제공
def mytarget(barsupfront, df1):
    length = len(df1)
    high = list(df1['High'])
    low = list(df1['Low'])
    close = list(df1['Close'])
    open_ = list(df1['Open'])  # Python의 내장 함수와 충돌을 피하기 위해 'open'을 'open_'으로 변경
    trendcat = [None] * length
    for line in range(0, length - barsupfront - 2):
        valueOpenLow = 0
        valueOpenHigh = 0
        for i in range(1, barsupfront + 2):
            value1 = open_[line + 1] - low[line + i]
            value2 = open_[line + 1] - high[line + i]
            valueOpenLow = max(value1, valueOpenLow)
            valueOpenHigh = min(value2, valueOpenHigh)
            if (valueOpenLow >= close[line + 1] * pipdiff_percentage) and (
                    -valueOpenHigh <= close[line + 1] * pipdiff_percentage / SLTPRatio):
                trendcat[line] = 2  # -1 downtrend
                break
            elif (valueOpenLow <= close[line + 1] * pipdiff_percentage / SLTPRatio) and (
                    -valueOpenHigh >= close[line + 1] * pipdiff_percentage):
                trendcat[line] = 1  # uptrend
                break
            else:
                trendcat[line] = 0  # no clear trend
return trendcat
```

이 코드는 트렌드를 식별하고 그에 따라 목표 값을 설정하려는 mytarget 함수를 정의합니다. 지정된 시간 프레임 (barsupfront) 내에서 open 가격과 예상 최고/최저치의 차이를 계산합니다. 이 차이와 pipdiff_percentage 및 SLTPRatio로 정의된 임계값에 기반하여, 함수는 상승 트렌드, 하락 트렌드 또는 명확한 트렌드가 없다고 분류합니다. 이러한 분류는 거래 전략에서 목표 매수/매도 가격을 설정하는 데 사용될 수 있습니다.

여기에 제공된 코드의 세부 사항입니다:

<div class="content-ad"></div>

The provided code defines a function `mytarget` that aims to set target values (presumably for buying and selling) based on a trend classification. Let's dive into how this function works:

Parameters:

- `barsupfront` (integer): The number of bars to look ahead from the current bar for trend classification.
- `df1` (pandas DataFrame): The DataFrame containing OHLC (Open, High, Low, Close) prices.

Function Logic:

<div class="content-ad"></div>

### 초기화:

- DataFrame의 길이를 가져옵니다 (length).
- 고가, 저가, 종가 및 시가 ​​가격의 목록을 추출합니다 (고가, 저가, 종가, open_). open은 Python의 내장 open 함수와의 충돌을 피하기 위해 open_로 이름이 변경됩니다.
- 길이 개수의 요소를 모두 None으로 설정한 trendcat 리스트를 초기화합니다. 이 리스트는 각 막대의 추세 카테고리 (상승 추세, 하락 추세 또는 추세 없음)를 나중에 저장합니다.

### 추세 분류 루프:

- 코드는 DataFrame을 반복하며, barsupfront-th 막대부터 뒤에서 두 번째 막대까지 진행됩니다 (length - barsupfront - 2).
- 루프 내부에서:
- 두 가지 값을 계산합니다:
- valueOpenLow: 현재 막대의 오픈 가격과 다음 barsupfront + 1개의 막대에서의 최저가 사이의 최대 차이입니다.
- valueOpenHigh: 현재 막대의 오픈 가격과 다음 barsupfront + 1개의 막대에서의 최고가 사이의 최소 차이입니다.
- asset의 가격을 기준으로 한 pipdiff_percentage (비율의 백분율) 및 SLTPRatio에 따라 이러한 값을 확인합니다:
- 만약 valueOpenLow가 당일 종가[line + 1] * pipdiff_percentage보다 크거나 같으면 (즉, 오픈 가격이 앞으로 다가올 최저가들보다 상당히 낮다는 것을 의미) AND -valueOpenHigh가 당일 종가[line + 1] * pipdiff_percentage / SLTPRatio보다 작거나 같으면 (즉, 오픈 가격이 앞으로 다가올 최고가들보다 상당히 높지 않다는 것을 의미), 해당 추세를 하락 추세로 분류합니다 (trendcat[line]이 2로 설정됨).
- 반대로, valueOpenLow가 당일 종가[line + 1] * pipdiff_percentage / SLTPRatio보다 작거나 같으면 (즉, 오픈 가격이 앞으로 다가올 최저가들보다 상당히 높다는 것을 의미) AND -valueOpenHigh가 당일 종가[line + 1] * pipdiff_percentage보다 크거나 같으면 (즉, 오픈 가격이 앞으로 다가올 최고가들보다 상당히 낮지 않다는 것을 의미), 해당 추세를 상승 추세로 분류합니다 (trendcat[line]이 1로 설정됨).
- 두 조건 중 어느 것도 충족되지 않으면 명확한 추세가 없음으로 표시됩니다 (trendcat[line]은 0으로 유지됨).



<div class="content-ad"></div>

3. 회귀:

- 이 함수는 Trendcat 목록을 반환합니다. 각 바에 대한 추세 분류가 포함되어 있습니다 (첫 번째 바를 제외한 모든 바).
- pen_spark

```js
#!!! 함정 하나의 범주 높은 빈도
df['Target'] = mytarget(2, df)
df['Target'] = df['Target'].shift(1)
#df.tail(20)
df.replace([np.inf, -np.inf], np.nan, inplace=True)
df.dropna(axis=0, inplace=True)
# 열을 정수형으로 변환
df = df.astype(int)
#df['Target'] = df['Target'].astype(int)
df['Target'].hist()
count_of_twos_target = df['Target'].value_counts().get(2, 0)
count_of_zeros_target = df['Target'].value_counts().get(0, 0)
count_of_ones_target = df['Target'].value_counts().get(1, 0)
percent_of_zeros_over_ones_and_twos = (100 - (count_of_zeros_target/ (count_of_zeros_target + count_of_ones_target + count_of_twos_target))*100)
print(f' count_of_zeros = {count_of_zeros_target}\n count_of_twos_target = {count_of_twos_target}\n count_of_ones_target={count_of_ones_target}\n percent_of_zeros_over_ones_and_twos = {round(percent_of_zeros_over_ones_and_twos,2)}%')
```  

![Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_2.png](/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_2.png)  

<div class="content-ad"></div>

Trend classifications(TARGET)는 mytarget 함수를 기반으로 할당하고 데이터 클리닝을 수행합니다. 이 과정에서 무한값을 처리하고 결측값이 있는 행을 제거합니다. 이후 히스토그램을 사용하여 TARGET 값의 분포를 분석하고, 각 트렌드 카테고리로 분류된 막대의 비율을 계산합니다. 이를 통해 데이터에서 명확한 상승 트렌드, 하락 트렌드 및 명확한 트렌드가 없는 기간 사이의 균형을 평가할 수 있습니다.

1. Target 값 할당과 Shift:

- 코드는 mytarget(2, df)의 출력(아마도 트렌드 분류)을 `Target` 열에 할당합니다(df[`Target`] = mytarget(2, df)).
- 그런 다음 `Target` 값을 하나씩 상향 시킵니다(df[`Target`] = df[`Target`].shift(1)), 왜냐하면 트렌드 분류는 미래 가격 변동을 기반으로 하기 때문이죠. 이는 n번 막대의 타겟 값은 n-1번 막대의 트렌드 분류에 기반을 두고 있음을 의미합니다.

2. 무한값 및 결측값 처리:

<div class="content-ad"></div>

- 코드는 데이터프레임(df.replace([np.inf, -np.inf], np.nan, inplace=True))에서 양의 무한대와 음의 무한대(np.inf 및 -np.inf)를 NaN(숫자가 아님) 값으로 대체합니다. 이것은 일부 수학 연산이 무한대를 처리하지 못하기 때문에 필요합니다.
- 그런 다음 데이터프레임에서 누락된 값(NaN)이 있는 행을 제거합니다(df.dropna(axis=0, inplace=True)) . 이는 추가 분석을 위해 깨끗한 데이터를 보장하기 위해 필요합니다.

3. 데이터 유형 변환 (주석 처리됨):

- df = df.astype(int)라는 코드는 주석 처리되어 있습니다. 이 코드는 데이터프레임의 모든 열을 정수로 변환하려고 시도할 것입니다. 그러나 `Target` 열은 아마도 범주 값(1, 2 또는 0)을 포함하고 있기 때문에 정수로 변환하는 것은 의미가 없을 수 있습니다. 일반적으로 숫자 열을 정수로 변환하는 것은 계산에 필요한 경우에만 수행합니다.

4. 대상 분포 분석:

<div class="content-ad"></div>

- 코드는 `Target` 열의 히스토그램을 플롯합니다 (df[`Target`].hist()). 이를 통해 데이터 내 목표 값의 분포를 시각화하는 데 도움이 됩니다 (상승추세, 하락추세 또는 추세 없음).
- 그런 다음 각 타겟 값 (1, 2 및 0)의 수를 value_counts()를 사용하여 계산합니다.
- 마지막으로, “no trend”로 분류된 막대의 백분율을 상승 및 하락추세로 분류된 막대 합에 대한 백분율로 계산합니다 (percent_of_zeros_over_ones_and_twos). 이를 통해 데이터에서 명확한 추세와 불명확한 추세 간의 균형에 대한 통찰을 제공합니다.

이 코드 세그먼트는 미리 정의된 기준에 따라 목표 카테고리를 효과적으로 계산하며, 이러한 카테고리의 분포에 대한 통찰을 제공합니다.

# 위의 코드가 생성된 “Target” 데이터에 대해 최상의 수익률을 제공하는지 확인하는 단계:


# NaN 값 확인:
has_nan = df['Target'].isnull().values.any()
print("NaN 값이 있음:", has_nan)
# 무한 값 확인:
has_inf = df['Target'].isin([np.inf, -np.inf]).values.any()
print("무한 값이 있음:", has_inf)
# NaN 및 무한 값의 수를 계산:
nan_count = df['Target'].isnull().sum()
inf_count = (df['Target'] == np.inf).sum() + (df['Target'] == -np.inf).sum()
print("NaN 값 수:", nan_count)
print("무한 값 수:", inf_count)
# NaN 및 무한 값의 인덱스 얻기:
nan_indices = df['Target'].index[df['Target'].isnull()]
inf_indices = df['Target'].index[df['Target'].isin([np.inf, -np.inf])]
print("NaN 값의 인덱스:", nan_indices)
df['Target']
df = df.reset_index(inplace=False)
df['Date'] = pd.to_datetime(df['Date'])
df.set_index('Date', inplace=True)
def SIGNAL(df):
    return df['Target']
from backtesting import Strategy
class MyCandlesStrat(Strategy):  
    def init(self):
        super().init()
        self.signal1 = self.I(SIGNAL, self.data)
    
    def next(self):
        super().next() 
        if self.signal1 == 1:
            sl_pct = 0.025  # 2.5% 손절
            tp_pct = 0.025  # 2.5% 익절
            sl_price = self.data.Close[-1] * (1 - sl_pct)
            tp_price = self.data.Close[-1] * (1 + tp_pct)
            self.buy(sl=sl_price, tp=tp_price)
        elif self.signal1 == 2:
            sl_pct = 0.025  # 2.5% 손절
            tp_pct = 0.025  # 2.5% 익절
            sl_price = self.data.Close[-1] * (1 + sl_pct)
            tp_price = self.data.Close[-1] * (1 - tp_pct)
            self.sell(sl=sl_price, tp=tp_price)
            
bt = Backtest(df, MyCandlesStrat, cash=100000, commission=.001, exclusive_orders = True)
stat = bt.run()
stat


<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_3.png)

- Missing and Infinite Values Check:

- This script examines whether there are any NaN (Not a Number) or infinite values in the `Target` column (df[`Target`]).
- It then tallies the occurrences and identifies the indices where these values are located.
- These validations are essential as backtesting libraries typically struggle with missing or infinite values in signals.

2. Configuring the Backtesting Framework:

<div class="content-ad"></div>

- 위 코드는 단순히 `Target` 열 값들을 반환하는 SIGNAL(df) 함수를 정의합니다. 이 함수는 본질적으로 목표 분류(상승 추세 매수를 위한 1 및 하락 추세 매도를 위한 2)에 기반한 매수/매도 신호를 제공합니다.
- 백테스팅 라이브러리에서 Strategy 클래스를 가져옵니다.
- Strategy를 상속하는 사용자 정의 전략 클래스 MyCandlesStrat을 정의합니다.
- init 메서드에서는 I 함수(아마도 백테스팅 기능에서 가져온 것)를 사용하여 target 값을 보유하는 signal1 지표를 초기화합니다.
- next 메서드는 거래 논리를 정의합니다:
  - signal1이 1(상승 추세)인 경우에는 종가를 기준으로 손절가 및 익절가를 고려한 매수 주문을 체결합니다.
  - signal1이 2(하락 추세)인 경우에는 종가를 기준으로 손절가 및 익절가를 고려한 매도 주문을 체결합니다.

3. 백테스팅 및 평가:

- 코드는 백테스팅 라이브러리를 사용하여 Backtest 객체를 생성합니다. 이 객체에는 DataFrame(df), 전략 클래스(MyCandlesStrat), 초기 자본(cash), 수수료율(commission) 및 exclusive_orders를 True로 설정하는 등과 같은 초기 설정이 전달됩니다(중첩 주문을 방지하기 위해).
- bt.run() 메서드를 사용하여 백테스트를 실행하고 결과를 stat 변수에 저장합니다.

이 코드는 목표 값의 효과를 명확하게 결정할까요?

<div class="content-ad"></div>

No, this code doesn’t definitively determine the effectiveness of the target values. Here’s why:

**Parameter Optimization**: The stop-loss and take-profit percentages (sl_pct and tp_pct) are fixed in the code. Optimizing these parameters for the specific strategy and market conditions could potentially improve performance.
  
**Single Backtest Run**: Running the backtest only once doesn’t account for the inherent randomness in financial markets. Ideally, you’d run the backtest multiple times with different random seeds to assess its robustness.

**How to improve the code for target evaluation?**

- **Calculate Performance Metrics**: Modify the code to calculate and print relevant performance metrics like Sharpe Ratio, drawdown, and total profit after the backtest run.

- **Optimize Stop-Loss and Take-Profit**: Implement a parameter optimization process to find the best stop-loss and take-profit values for the strategy using the target signals.

- **Multiple Backtest Runs**: Run the backtest with different random seeds (e.g., using a loop) and analyze the distribution of performance metrics to assess the strategy’s consistency.

<div class="content-ad"></div>

위의 향상된 내용을 통합하면, mytarget 함수의 대상 값이 백테스팅 프레임워크에서 얼마나 잘 수행되는지 보다 포괄적으로 이해할 수 있다. 기억해야 할 것은 백테스팅 결과가 미래 성과를 보장하는 것은 아니므로, 실제 자본을 할당하기 전에 실제 자금으로 전략을 배포하기 전에 실제 세계 테스트가 필수적이라는 것입니다.

# 훈련 및 테스트를 위한 데이터프레임의 조정 및 분할:

```python
scaler = MinMaxScaler(feature_range=(0,1))
df_model = df.copy()
# 학습용 (X) 및 대상 (y) 데이터로 분할
X = df_model.iloc[:, : -1]
y = df_model.iloc[:, -1]
X_scaled = scaler.fit_transform(X)
# 데이터를 재구성하는 함수 정의
def reshape_data(data, time_steps):
    samples = len(data) - time_steps + 1
    reshaped_data = np.zeros((samples, time_steps, data.shape[1]))
    for i in range(samples):
        reshaped_data[i] = data[i:i + time_steps]
    return reshaped_data
# 스케일 된 X 데이터 재구성
time_steps = 1  # 필요에 맞게 시간 단계의 수를 조정
X_reshaped = reshape_data(X_scaled, time_steps)
# 이제 X_reshaped의 원하는 3차원 모양이 됨: (samples, time_steps, features)
# 각 샘플은 특정 시간 창에 대한 스케일 된 데이터를 포함
# 초과 대상 값을 버림으로써 y를 X_reshaped와 정렬
y_aligned = y[time_steps - 1:]  # 처음 (time_steps - 1) 대상 값을 버림
X = X_reshaped
y = y_aligned
print(len(X),len(y))
# 데이터를 학습 및 테스트 세트로 분할 (시계열 데이터 고려)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, shuffle=False)
```

1. 데이터 준비:

<div class="content-ad"></div>

- **데이터 복사:**
여기서는 원본 DataFrame을 수정하지 않기 위해 복사본을 만듭니다. (`df_model = df.copy()`)

- **특징과 타겟 분리:**
특징(X)과 타겟(y)을 분리합니다. 여기서는 슬라이싱을 사용하여 특징(마지막 열 제외한 모든 열)과 타겟 변수(마지막 열)을 분리합니다. (`X = df_model.iloc[:, :-1]`, `y = df_model.iloc[:, -1]`)

- **특징 스케일링:**

<div class="content-ad"></div>

**4. 데이터 형태 재구성 (윈도잉):**

- **Reshape 함수:** 데이터와 타임 스텝 수(time_steps)를 입력으로 받는 reshape_data 함수를 정의합니다.
- 이 함수는 데이터를 타임 스텝 크기로 슬라이딩 윈도우하여 새로운 3D 배열(reshaped_data)을 생성합니다.
- 새 배열의 각 요소는 각 특성의 시퀀스가 포함된 샘플을 나타냅니다.
- **스케일링된 X를 재구성:** 타임 스텝 수(time_steps)를 정의하고 스케일된 특성 데이터(X_scaled)를 reshape_data 함수를 사용하여 재구성합니다(X_reshaped = reshape_data(X_scaled, time_steps)).
- 이 단계는 과거 관측값의 시퀀스를 사용하여 미래값을 예측하는 시계열 예측 모델에 적합한 형식으로 데이터를 변환합니다.

**5. 재구성된 데이터에 대상 맞추기:**


<div class="content-ad"></div>

- 과잉 대상 값 제거: X_reshaped는 시간 단계 창을 고려하기 때문에 해당 대상 값은 조정이 필요합니다. y의 처음 time_steps - 1 대상 값을 버려 X와 정렬된 데이터(y_aligned = y[time_steps - 1:])와 일치시킵니다.

6. 최종 분할(훈련-테스트):

- 훈련-테스트 분할: X와 정렬된 대상(y)을 훈련 및 테스트 세트로 분할합니다. 이는 scikit-learn의 train_test_split을 사용하여 처리됩니다(X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, shuffle=False)).
- test_size=0.3으로 설정하여 데이터의 30%를 테스트용으로 할당하며, 시계열 데이터를 섞는 것은 시간순서를 방해할 수 있으므로 shuffle=False로 설정합니다.

종합적으로, 이 코드는 시계열 예측 모델을 위한 데이터 준비의 주요 측면을 효과적으로 다룹니다:

<div class="content-ad"></div>

- 일부 알고리즘에 대해 모델 성능을 향상시키기 위해 일반 범위로 기능을 조정하는 것이 도움이 될 수 있어요.
- 시간 단계로 3D 구조로 데이터를 재구성하면 모델이 과거 관측 값의 연속체로부터 학습할 수 있어요.
- 타겟 변수를 재구성된 데이터와 일치시키면 모델이 올바른 시간 단계에 대해 예측할 수 있어요.
- shuffle=False로 훈련 및 테스트 세트로 데이터를 분할하면 시계열 예측을 위해 시간적 순서를 보존할 수 있어요.

추가 고려 사항:
- 스케일러의 선택(MinMaxScaler, StandardScaler 등)은 특정 모델 및 데이터 특성에 따라 다를 수 있어요.
- 모델 성능에 어떻게 영향을 주는지 확인하기 위해 다른 창 크기(시간 단계)를 살펴볼 수 있어요.
- 이러한 단계를 적용하기 전에 일부 시계열 데이터에 대해 안정성 확인 및 차이화와 같은 기법이 필요할 수 있어요.

# 2D CNN 모델 수동 최적화

<div class="content-ad"></div>

```js
from sklearn.utils.class_weight import compute_class_weight
from keras import backend as K
import keras_tuner as kt
from keras.models import Model
from keras.layers import Input, Conv2D, MaxPooling2D, Dense, Dropout, Flatten
from keras.optimizers import Adam
from keras.metrics import Precision, Recall
import numpy as np
```

이 코드는 ETH 가격 변동을 중립(0), 롱(1), 숏(2) 세 가지 범주로 분류하는 2D CNN 기반 모델을 정의하고 훈련합니다. 여기에서 설명하겠습니다:

## 라이브러리 가져오기


<div class="content-ad"></div>

## 모델 구축하기

해당 모델을 구축하기 위해 다음과 같은 단계를 따릅니다.

먼저, `compute_class_weight`는 데이터셋의 불균형을 처리하기 위해 클래스의 가중치를 계산합니다. `keras.backend`는 연산을 위한 백엔드 함수를 제공합니다. `keras_tuner`는 모델의 하이퍼파라미터 튜닝을 돕습니다. `Model`은 Keras 모델을 생성하는 데 사용됩니다. `Layers`는 CNN 모델을 구축하는 데 사용되는 다양한 층들을 의미합니다. `Adam`은 모델을 컴파일하는 데 사용되는 옵티마이저입니다. `Metrics`은 모델을 평가하는 데 사용되는 정밀도 및 재현율 지표를 나타냅니다.

```python
def build_model(hp):
    inputs = Input(shape=(X_train.shape[1], X_train.shape[2], 1))
    x = Conv2D(hp.Int('conv_units', min_value=16, max_value=64, step=16), (5, 5), padding='same', activation='relu')(inputs)
    x = MaxPooling2D(pool_size=(2, 2), padding='same')(x)
    x = Dense(units=hp.Int('dense_units', min_value=100, max_value=300, step=50), activation='relu')(x)
    x = Dropout(hp.Float('dropout_rate', min_value=0.1, max_value=0.5, step=0.1))(x)
    x = Flatten()(x)
    x = Dense(units=hp.Int('dense_units', min_value=100, max_value=300, step=50), activation='relu')(x)
    outputs = Dense(3, activation='softmax')(x)
    model = Model(inputs=inputs, outputs=outputs)
    optimizer = Adam(learning_rate=hp.Choice('learning_rate', values=[1e-2, 1e-3, 1e-4]))
    model.compile(optimizer=optimizer, loss='categorical_crossentropy', metrics=['accuracy', Precision(), Recall()])
    return model
```

위의 코드는 모델을 구축하는 함수를 보여줍니다. `inputs`는 (timesteps 수, 피쳐 수, 1) 모양의 데이터를 받는 입력 레이어입니다. `Conv2D`는 하이퍼파라미터로 정해진 필터를 가진 합성곱 레이어로, 지역적 패턴을 추출합니다. `MaxPooling2D`는 공간적 차원을 줄이기 위한 풀링 레이어입니다. `Dense`는 추출된 특징을 해석하기 위한 완전 연결 층입니다. `Dropout`는 과적합을 방지하는 정규화 기술입니다. `Flatten`은 밀집층을 위해 2D 행렬을 1D 벡터로 변환합니다. `outputs`는 다중 클래스 분류를 위한 소프트맥스 활성화 함수를 가진 최종 출력 레이어입니다.

<div class="content-ad"></div>

## 하이퍼파라미터 튜닝


tuner = kt.Hyperband(
    build_model,
    objective=kt.Objective("val_recall", direction="max"),
    max_epochs=20,
    factor=3,
    directory='my_dir',
    project_name='hyperopt_cnn'
)


- Hyperband: 하이퍼파라미터 튜닝을 위한 케라스 튜너 알고리즘.
- objective: 튜닝 중에 최적화할 메트릭(이 경우 검증 리콜).
- max_epochs: 훈련할 최대 에폭 수.
- factor: 조기 중지를 위한 축소 계수.
- directory/project_name: 튜닝 결과를 저장할 위치.

## 튜너 실행하기

<div class="content-ad"></div>

**하이퍼파라미터 탐색**

이 카드는 학습 데이터에서 하이퍼파라미터 튜닝 프로세스를 실행합니다.

```python
tuner.search(X_train, y_train_one_hot, epochs=10, validation_split=0.2)
```

## 가장 최적의 하이퍼파라미터 얻기

```python
best_hps = tuner.get_best_hyperparameters(num_trials=1)[0]
```

<div class="content-ad"></div>

- 조정 중에 식별된 최상의 하이퍼파라미터를 검색합니다.

## 클래스 가중치 계산

```js
class_weights = compute_class_weight('balanced', classes=np.unique(y_train), y=y_train)
class_weight_dict = dict(enumerate(class_weights))
```

- compute_class_weight: 클래스 빈도에 반비례하는 가중치를 계산하여 클래스를 균형있게 맞춥니다.
- class_weight_dict: 클래스 가중치의 사전입니다.

<div class="content-ad"></div>

## 최적 하이퍼파라미터로 모델 구축하고 학습시키기


best_cnn_model = tuner.hypermodel.build(best_hps)
best_cnn_model.fit(X_train, y_train_one_hot, epochs=100, batch_size=18, validation_split=0.2, verbose=1)


- build: 최적 하이퍼파라미터로 모델을 구축합니다.
- fit: 훈련 데이터로 모델을 학습시킵니다.

# 왜 2D CNN이 이 작업에 1D CNN보다 더 나을 수 있는지

<div class="content-ad"></div>

**특징 상호작용:**

- **2D CNN:** 시간에 따른 다양한 특징 간의 상호작용을 포착할 수 있습니다. 이는 다수의 특징을 가진 시계열 데이터에 중요한데, 복잡한 패턴을 학습할 수 있습니다.
- **1D CNN:** 주로 단일 특징이나 일차원 데이터 시퀀스 내의 시간적 패턴을 포착합니다.

**공간적 관계:**

- **2D CNN:** 특징 간의 공간적 관계를 이해하는 데 더 효과적이며, 관련된 다수의 특징이 존재할 때 가치가 있습니다.
- **1D CNN:** 일차원 시퀀스에 집중하여, 다수의 특징 간 상호작용을 효과적으로 포착하지 못할 수 있습니다.

<div class="content-ad"></div>

디멘젼 압축:

- 2D CNN: 풀링 레이어는 공간적 맥락을 고려하여 차원을 효율적으로 줄여 더 나은 일반화를 도모할 수 있습니다.
- 1D CNN: 유사한 차원 압축을 위해 보다 많은 레이어나 파라미터를 필요로 할 수 있으며, 복잡성이 증가할 수 있습니다.

패턴 인식:

- 2D CNN: 두 가지 차원(시간과 특징)에 걸쳐 필터를 적용하여 복잡한 패턴을 탐지할 수 있어 다중 특징 시계열 데이터에 유용합니다.
- 1D CNN: 한 차원에서의 패턴 인식에 한정되어 복잡한 다중 특징 데이터셋에는 부족할 수 있습니다.

<div class="content-ad"></div>

## 결론

주어진 코드는 Keras Tuner를 사용하여 하이퍼파라미터 튜닝된 2D CNN 모델을 설정합니다. 이 모델은 암호화폐의 시계열 데이터에 대한 다중 클래스 분류를 처리하도록 설계되었습니다. 2D CNN을 활용하여 모델은 시간과 특성 간의 복잡한 패턴을 포착할 수 있으며, 이는 1D CNN과 비교하여 더 정확하고 견고한 예측을 이끌어낼 수 있습니다. 하이퍼파라미터 튜닝의 사용은 모델이 주어진 작업에 최적화되도록 보장하며, 성능을 더욱 향상시킵니다.

추가 사항:

- 제공된 코드는 데이터와 원하는 성능에 따라 조정이 필요할 수 있습니다. 하이퍼파라미터 튜닝(예: 유닛 수, 드롭아웃 비율, 학습률)은 모델을 최적화하는 데 중요합니다.
- 모델 성능을 향상시키기 위해 특성에 대한 정규화나 표준화와 같은 기법을 사용하는 것을 고려해보세요.

<div class="content-ad"></div>

Further Exploration:

- 다양한 하이퍼파라미터(층 수, 유닛 수, 어텐션 헤드)를 실험하여 데이터와 작업에 가장 적합한 구성을 찾아보세요.
- 기술적 지표나 기본 데이터 등 추가 특성을 통합하여 모델의 예측 정확도를 향상시킬 수 있습니다.
- Precision, recall, F1-score 또는 특정 거래 전략에 기반한 맞춤 메트릭을 사용하여 모델의 성능을 평가해 보세요.

Real-World Considerations:

- 금융 시장은 복잡하며 다양한 요소에 영향을 받습니다. 과거 가격 변동이 미래 성과를 보장하지 않습니다.
- 모델 예측을 안내로 삼고 절대적인 신호로 간주하지 마세요. 거래 결정을 내리기 전에 리스크 관리 전략과 기타 요소를 고려해야 합니다.
- 역사적 데이터로 모델을 백테스트하여 다양한 시장 상황에서의 성능을 평가해 보세요.

<div class="content-ad"></div>

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.metrics import classification_report, confusion_matrix
import seaborn as sns

# Reshape X_train and X_test back to their original shapes
X_train_original_shape = X_train.reshape(X_train.shape[0], -1)
X_test_original_shape = X_test.reshape(X_test.shape[0], -1)

X_test_reshaped = X_test_original_shape.reshape(-1, 1, X_test_original_shape.shape[1])

# Now X_train_original_shape and X_test_original_shape have their original shapes

# Perform prediction on the original shape data
y_pred = best_cnn_model.predict(X_test)

# Perform any necessary post-processing on y_pred if needed
# For example, if your model outputs probabilities, you might convert them to class labels using argmax:

y_pred_classes = np.argmax(y_pred, axis=1)

# Convert one-hot encoded y_test to class labels
y_test_classes = y_test

# Plot confusion matrix for test data
conf_matrix_test = confusion_matrix(y_test_classes, y_pred_classes)

# Plot confusion matrix
plt.figure(figsize=(8, 6))
sns.heatmap(conf_matrix_test, annot=True, cmap='Blues', fmt='g', cbar=False)
plt.xlabel('Predicted labels')
plt.ylabel('True labels')
plt.title('Confusion Matrix - Test Data')
plt.show()

# Compute classification report
report = classification_report(y_test_classes, y_pred_classes)
print("Classification Report:\n", report)

print("Confusion Matrix for Hyperopt Model:")
print(confusion_matrix(y_test_classes, y_pred_classes))
```

![image](/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_4.png)

1. Imports:
   - `confusion_matrix` from `sklearn.metrics` for calculating the confusion matrix.
   - `matplotlib.pyplot` (`plt`) and `seaborn` (`sns`) for creating the confusion matrix visualization.
   - `classification_report` from `sklearn.metrics` for generating a classification report.

<div class="content-ad"></div>

2. 데이터 형태 재구성 (주석 처리):

주석 처리된 부분은 재구성 문제에 대해 다룹니다. 모델이 예측을 위해 기대하는 정확한 형태를 가진 테스트 데이터(X_test)를 보장하는 것이 중요합니다.

3. 예측:

- y_pred = model_cnn.predict(X_test)을 사용하여 훈련된 모델을 통해 테스트 데이터에 대한 예측을 수행합니다.

<div class="content-ad"></div>

4. Post-processing Predictions:

- `y_pred_classes = np.argmax(y_pred, axis=2)` assumes that your model produces probability values for each class (neutral, long, short). This line of code transforms these probabilities into class labels by selecting the index of the highest value along axis 2.

5. Converting True Labels:

- `y_test_classes = y_test` assumes that your `y_test` dataset already includes class labels (0, 1, 2) for the test set.

<div class="content-ad"></div>

6. 혼동 행렬:

- conf_matrix_test = confusion_matrix(y_test_classes, y_pred_classes)은 테스트 데이터에 대한 혼동 행렬을 계산합니다. 이는 모델에 의해 각 참된 클래스로부터 몇 개의 샘플이 어떤 클래스로 예측되었는지를 보여줍니다.

7. 시각화:

- 이 코드는 seaborn을 사용하여 혼동 행렬의 히트맵 시각화를 생성합니다. 이를 통해 각 클래스의 모델 분류가 얼마나 잘 되었는지를 시각적으로 확인할 수 있습니다. 이상적으로, 당신은 올바른 분류를 나타내는 대각선상의 높은 값들을 보고 싶어합니다.

<div class="content-ad"></div>

8. 분류 보고서:

- `class_report = classification_report(y_test, y_pred_classes)`는 테스트 데이터에 대한 분류 보고서를 생성합니다. 이 보고서는 각 클래스에 대한 정밀도, 재현율, F1-점수 및 지원을 포함하여 모델의 성능을 더 자세히 분석하는 메트릭을 제공합니다.
- pen_spark

# 테스트 및 전체 데이터로 백테스트:

```js
df_ens_test = df.copy() 

df_ens = df_ens_test[len(X_train):]

df_ens['best_cnn_model_scaled'] = np.argmax(best_cnn_model.predict(X_test), axis=1)

df_ens['bcns'] = df_ens['best_cnn_model_scaled'].shift(1).dropna().astype(int)

df_ens = df_ens.dropna()

df_ens['bcns']

# df_ens = df.copy() 

# # df_ens = df_ens_test[len(X_train):]

# df_ens['best_cnn_model_scaled'] = np.argmax(best_cnn_model.predict(X), axis=1)

# df_ens['bcns'] = df_ens['best_cnn_model_scaled'].shift(-1).dropna().astype(int)

# df_ens = df_ens.dropna()

# df_ens['bcns']

df_ens = df_ens.reset_index(inplace=False)
df_ens['Date'] = pd.to_datetime(df_ens['Date'])
df_ens.set_index('Date', inplace=True)

def SIGNAL_2_6(df_ens):
    return df_ens['bcns']

class MyCandlesStrat_2_6(Strategy):  
    def init(self):
        super().init()
        self.signal1_1 = self.I(SIGNAL_2_6, self.data)
    
    def next(self):
        super().next() 
        if self.signal1_1 == 1:
            sl_pct = 0.055  # 10% stop-loss
            tp_pct = 0.055  # 2.5% take-profit
            sl_price = self.data.Close[-1] * (1 - sl_pct)
            tp_price = self.data.Close[-1] * (1 + tp_pct)
            self.buy(sl=sl_price, tp=tp_price)
        elif self.signal1_1 == 2:
            sl_pct = 0.055  # 10% stop-loss
            tp_pct = 0.055  # 2.5% take-profit
            sl_price = self.data.Close[-1] * (1 + sl_pct)
            tp_price = self.data.Close[-1] * (1 - tp_pct)
            self.sell(sl=sl_price, tp=tp_price)

            
bt_2_6 = Backtest(df_ens, MyCandlesStrat_2_6, cash=100000, commission=.001)
stat_2_6 = bt_2_6.run()
stat_2_6
```

<div class="content-ad"></div>


## 데이터 준비

```js
df_ens_test = df.copy()
df_ens = df_ens_test[len(X_train):]
df_ens['best_cnn_model_scaled'] = np.argmax(best_cnn_model.predict(X_test), axis=1)
df_ens['bcns'] = df_ens['best_cnn_model_scaled'].shift(1).dropna().astype(int)
df_ens = df_ens.dropna()
```

- df_ens_test = df.copy(): 원본 데이터프레임 df를 복사하여 원본 데이터를 변경하지 않도록 합니다.
- df_ens = df_ens_test[len(X_train):]: 훈련 데이터의 길이를 제외한 부분을 잘라내어 테스트 세트에 해당하는 데이터프레임을 선택합니다.
- df_ens['best_cnn_model_scaled'] = np.argmax(best_cnn_model.predict(X_test), axis=1): 훈련된 CNN 모델을 사용하여 테스트 세트(X_test)에서 클래스 레이블을 예측합니다. np.argmax 함수를 사용하여 각 예측에 대해 가장 높은 확률을 가진 클래스를 가져옵니다.
- df_ens['bcns'] = df_ens['best_cnn_model_scaled'].shift(1).dropna().astype(int): 예측된 클래스 레이블을 한 위치씩 이동하여 미래 데이터를 사용하여 현재 단계의 예측을 하는 룩어헤드 편향을 방지합니다. 결과적으로 나타나는 NaN 값을 제거하고 시리즈를 정수로 변환합니다.
- df_ens = df_ens.dropna(): 데이터프레임에 남은 NaN 값이 없도록 합니다.

<div class="content-ad"></div>

## 색인 및 날짜 형식 지정

```js
df_ens = df_ens.reset_index(inplace=False)
df_ens['Date'] = pd.to_datetime(df_ens['Date'])
df_ens.set_index('Date', inplace=True)
```

- df_ens = df_ens.reset_index(inplace=False): 데이터프레임의 색인을 수정하지 않고 재설정합니다.
- df_ens['Date'] = pd.to_datetime(df_ens['Date']): 'Date' 열을 날짜 및 시간 형식으로 변환합니다.
- df_ens.set_index('Date', inplace=True): 'Date' 열을 데이터프레임의 색인으로 설정합니다.

## 시그널 함수

<div class="content-ad"></div>

```yaml
def SIGNAL_2_6(df_ens):
    return df_ens['bcns']
```

- SIGNAL_2_6(df_ens): 'bcns' 열을 반환하는 함수입니다. 이 열에는 이동된 클래스 라벨(신호)이 포함되어 있습니다.

## 사용자 정의 거래 전략

```yaml
class MyCandlesStrat_2_6(Strategy):  
    def init(self):
        super().init()
        self.signal1_1 = self.I(SIGNAL_2_6, self.data)
    
    def next(self):
        super().next() 
        if self.signal1_1 == 1:
            sl_pct = 0.055  # 5.5% 손절가
            tp_pct = 0.055  # 5.5% 익절가
            sl_price = self.data.Close[-1] * (1 - sl_pct)
            tp_price = self.data.Close[-1] * (1 + tp_pct)
            self.buy(sl=sl_price, tp=tp_price)
        elif self.signal1_1 == 2:
            sl_pct = 0.055  # 5.5% 손절가
            tp_pct = 0.055  # 5.5% 익절가
            sl_price = self.data.Close[-1] * (1 + sl_pct)
            tp_price = self.data.Close[-1] * (1 - tp_pct)
            self.sell(sl=sl_price, tp=tp_price)
```

<div class="content-ad"></div>

## 전략 백테스팅

```python
bt_2_6 = Backtest(df_ens, MyCandlesStrat_2_6, cash=100000, commission=.001)
stat_2_6 = bt_2_6.run()
stat_2_6
```

안녕하세요! 이번에는 사용자 정의 트레이딩 전략 클래스를 정의하고, 각 단계에서 실행되는 전략의 핵심 논리를 살펴보겠습니다.

- `MyCandlesStrat_2_6(Strategy) 클래스`: Strategy를 상속한 사용자 정의 트레이딩 전략 클래스를 정의합니다.
- `def init(self)`: 초기화 메서드입니다. self.signal1_1 속성은 거래 신호를 제공하는 SIGNAL_2_6 함수로 설정됩니다.
- `def next(self)`: 각 단계에서 실행되는 전략의 핵심 논리입니다.
- 만약 `self.signal1_1 == 1`: 신호가 1이면 5.5%의 손실 제한과 이익 실현을 가진 매수 포지션을 취합니다.
- 그렇지 않고, `elif self.signal1_1 == 2`: 신호가 2이면 5.5%의 손실 제한과 이익 실현을 가진 매도 포지션을 취합니다.

위의 코드는 백테스팅을 수행하는 부분입니다. 준비된 데이터프레임 `df_ens`, 사용자 정의 전략 `MyCandlesStrat_2_6`, 초기 자금 잔액 100,000 단위, 그리고 수수료율 0.1%로 초기화된 백테스트를 실행하고 그 결과와 통계를 확인하는 부분입니다.

<div class="content-ad"></div>

# 2D CNN이 1D CNN보다 우세한 이유

복잡한 패턴 포착:

- 2D CNN: 시공간 관계 및 서로 다른 특징 간의 상호 작용을 포착할 수 있어서 다변량 시계열 데이터에 중요하다.
- 1D CNN: 일반적으로 단일 특징 내의 시간적 패턴이나 1차원 순서에 초점을 맞춘다.

차원 축소:

<div class="content-ad"></div>

**2D CNN:** efficiently reduces dimensionality while preserving important features through pooling layers.

**1D CNN:** may require more layers or parameters to achieve similar results.

**Feature Interactions:**

**2D CNN:** can learn complex interactions between different features, providing a richer representation of the data.

**1D CNN:** limited to learning patterns in one dimension, which may not capture the full complexity of multivariate data.

By using a 2D CNN, this approach leverages its ability to capture intricate patterns and interactions in multivariate time series data, potentially leading to better performance in predicting trading signals.

<div class="content-ad"></div>


파이썬 코드:

from keras.models import save_model

best_cnn_model.save(f"./models/best_cnn_model_2d_15m_ETH_SL55_TP55_ShRa_{round(stat_2_6['Sharpe Ratio'],2)}_time_{time.strftime('%Y%m%d%H%M%S')}.keras")

설명:

- Import:

- keras.models에서 save_model을 불러와 모델을 저장합니다.


<div class="content-ad"></div>

**2. 파일 이름 정의:**

- 파일 이름은 f-string (포맷된 문자열 리터럴)을 사용하여 구성됩니다. 여러 세부 정보를 포함합니다:
- 경로: ./models/: 모델을 저장할 디렉토리를 지정합니다.
- 모델 이름: transformer_model: 모델의 기본 이름입니다.
- 하이퍼파라미터: _55sl_55tp: 백테스팅 전략에서 사용된 손절가 (Stop-Loss, SL) 및 익절가 (Take-Profit, TP) 값을 나타내는 것으로 보입니다.
- 데이터 정보: _eth_15m: 이더리움 (ETH) 가격과 15분 간격의 데이터를 가리킬 가능성이 있습니다.
- 날짜: _may_13th: 모델이 훈련된 날짜 (5월 13일)를 나타냅니다.
- 성과 지표: _ShRa_'round(stat_1[`Sharpe Ratio`],2)': 백테스팅 결과(stat_1)에서 얻은 샤프 지수를 소수점 둘째 자리까지 반올림하여 추가합니다.
- 파일 확장자: .keras: Keras 모델의 표준 확장자입니다.

**3. 모델 저장하기:**

- save_model(model_transformer, filename): 이 코드는 model_transformer를 지정된 파일에 구성된 파일 이름으로 저장합니다.

<div class="content-ad"></div>

중요한 포인트:

- 이 방식은 모델을 저장하는 명확하고 정보성 있는 방법을 제공합니다. 훈련 매개변수, 데이터, 그리고 성능에 대한 자세한 내용을 포함합니다.
- 파일 이름 구조를 수정하여 필요에 맞는 추가 정보를 포함할 수 있습니다.

# 저장된 모델로 전체 데이터를 백테스트해봅시다:

```python
from keras.models import load_model

# joblib를 사용하여 ensemble_predict 함수를 불러옵니다
best_model = load_model('./models/cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.68_time_20240527170917.keras')
```

<div class="content-ad"></div>

**의도된 기능:**

- Import:

- keras.models에서 load_model을 사용하여 저장된 모델을 불러옵니다.

2. 모델 불러오기:

<div class="content-ad"></div>

`./models/` 디렉토리에서 저장된 `transformer_model_55sl_55tp_eth_15m_may_13th_ShRa_0.78.keras` 파일을 로드하려는 코드입니다.

```python
df_ens = df.copy() 

# df_ens = df_ens_test[:len(X)]
y_pred = best_model.predict(X)

# 필요한 경우 y_pred에 대한 후처리 수행
# 예를 들어, 모델이 확률을 출력하는 경우 argmax를 사용하여 클래스 레이블로 변환할 수 있습니다:
# y_pred_classes = np.argmax(y_pred, axis=1)
# y_pred = np.argmax(y_pred, axis=1) # lstm, tcn, cnn 모델용
y_pred = np.argmax(y_pred, axis=2) # transformers 모델용
df_ens['best_model'] =  y_pred
df_ens['bm'] = df_ens['best_model'].shift(1).dropna().astype(int)
df_ens['ema_22'] = ta.EMA(df_ens['Close'], timeperiod=22)
df_ens['ema_55'] = ta.EMA(df_ens['Close'], timeperiod=55)
df_ens['ema_108'] = ta.EMA(df_ens['Close'], timeperiod=108)
df_ens = df_ens.dropna()
df_ens['bm']
df_ens = df_ens.reset_index(inplace=False)
df_ens['Date'] = pd.to_datetime(df_ens['Date'])
df_ens.set_index('Date', inplace=True)
def SIGNAL_010(df_ens):
    return df_ens['bm']
def SIGNAL_0122(df_ens):
    return df_ens['ema_22']
def SIGNAL_0155(df_ens):
    return df_ens['ema_55']
def SIGNAL_01108(df_ens):
    return df_ens['ema_108']
class MyCandlesStrat_010(Strategy):  
    def init(self):
        super().init()
        self.signal1_1 = self.I(SIGNAL_010, self.data)
        self.ema_1_22 = self.I(SIGNAL_0122, self.data)
        self.ema_1_55 = self.I(SIGNAL_0155, self.data)
        self.ema_1_108 = self.I(SIGNAL_01108, self.data)
    
    def next(self):
        super().next() 
        if (self.signal1_1 == 1) and (self.data.Close > self.ema_1_22) and (self.ema_1_22 > self.ema_1_55) and (self.ema_1_55 > self.ema_1_108):
            sl_pct = 0.025  # 10% stop-loss
            tp_pct = 0.025  # 2.5% take-profit
            sl_price = self.data.Close[-1] * (1 - sl_pct)
            tp_price = self.data.Close[-1] * (1 + tp_pct)
            self.buy(sl=sl_price, tp=tp_price)
        elif (self.signal1_1 == 2)  and (self.data.Close < self.ema_1_22) and (self.ema_1_22 < self.ema_1_55) and (self.ema_1_55 < self.ema_1_108):
            sl_pct = 0.025  # 10% stop-loss
            tp_pct = 0.025  # 2.5% take-profit
            sl_price = self.data.Close[-1] * (1 + sl_pct)
            tp_price = self.data.Close[-1] * (1 - tp_pct)
            self.sell(sl=sl_price, tp=tp_price)

        if (self.signal1_1 == 1):
            sl_pct = 0.035  # 10% stop-loss
            tp_pct = 0.025  # 2.5% take-profit
            sl_price = self.data.Close[-1] * (1 - sl_pct)
            tp_price = self.data.Close[-1] * (1 + tp_pct)
            self.buy(sl=sl_price, tp=tp_price)
        elif (self.signal1_1 == 2):
            sl_pct = 0.035  # 10% stop-loss
            tp_pct = 0.025  # 2.5% take-profit
            sl_price = self.data.Close[-1] * (1 + sl_pct)
            tp_price = self.data.Close[-1] * (1 - tp_pct)
            self.sell(sl=sl_price, tp=tp_price)
            
bt_010 = Backtest(df_ens, MyCandlesStrat_010, cash=100000, commission=.001)
stat_010 = bt_010.run()
stat_010
```

![Backtesting Results](/assets/img/2024-07-09-9883Returnsin3yearsonCryptocurrencyusing2DConvolutionalNeuralNetworkCNNModelandshortlistingBestAssetsforTradingVishvaAlgoMachineLearningTradingBot_6.png)

이 코드는 Transformer 모델 예측(`best_model`)과 지수 이동 평균(EMAs)을 사용하여 백테스트 전략을 생성하는 방식으로 이전 전략을 보완하고 있습니다. 위 코드를 분석해보겠습니다.

<div class="content-ad"></div>

**데이터 준비:**

- df_ens = df.copy(): 원본 DataFrame(df)의 복사본을 생성합니다.
- y_pred = best_model.predict(X): 적재한 변환 모델(best_model)을 사용하여 전체 DataFrame(X)에 대해 예측을 수행합니다.
- df_ens[`best_model`] = y_pred: 모델 예측값을 포함하는 `best_model`이라는 새 열을 DataFrame에 추가합니다.
- df_ens[`bm`] = df_ens[`best_model`].shift(1).dropna().astype(int): 이전과 유사하게, 예측된 레이블을 기반으로 시프트된 신호 열 `bm`을 생성하지만 여기에는 전체 DataFrame의 예측이 포함될 수 있습니다.
- df_ens[`ema_22`] = ta.EMA(df_ens[`Close`], timeperiod=22): `Close` 가격에 대한 22 기간 EMA를 계산하고 새 열 `ema_22`로 추가합니다.
- df_ens[`ema_55`] = ta.EMA(df_ens[`Close`], timeperiod=55): 위와 유사하게, 55 기간 EMA를 계산하고 `ema_55`로 추가합니다.
- df_ens[`ema_108`] = ta.EMA(df_ens[`Close`], timeperiod=108): 108 기간 EMA를 계산하고 `ema_108`로 추가합니다.
- df_ens = df_ens.dropna(): 누락된 값이 있는 행을 제거합니다(아마도 시프트로 인한 첫 번째 행일 수 있음).

**시그널 함수 (코드 블록 외):**

- 이러한 함수(SIGNAL_010, SIGNAL_0122 등)는 단순히 시그널을 생성하는 데 사용되는 DataFrame(`bm`, `ema_22` 등)에서 해당 열을 반환합니다.

<div class="content-ad"></div>

3. 백테스트 전략 클래스 (MyCandlesStrat_010):

- Strategy를 상속받음.
- def init(self): Transformer 모델 예측을 위한 지표 (self.signal1_1) 및 EMAs (self.ema_1_22 등)를 초기화합니다.

4. 백테스트 로직 (다음 함수 내):

- 주석 처리된 섹션은 Transformer 예측과 EMAs 간의 관계를 고려한 더 복잡한 논리를 보여줍니다.
- 현재 활성화된 섹션은 더 간단한 접근법을 사용합니다:
- 만약 self.signal1_1 (Transformer 예측)이 1 (long)이면:
- 현재 종가보다 3.5% 낮은 가격에 손절 (SL)과 2.5% 높은 가격에 익절 (TP)로 매수합니다.
- 만약 self.signal1_1이 2 (short)라면:
- 현재 종가보다 3.5% 높은 가격에 손절과 2.5% 낮은 가격에 익절로 매도합니다.

<div class="content-ad"></div>

### 5. 백테스팅과 결과:

- `bt_010 = Backtest(df_ens, MyCandlesStrat_010, cash=100000, commission=.001)`: 데이터프레임, 전략 클래스 및 기타 매개변수를 사용하여 백테스트 객체를 생성합니다.
- `stat_010 = bt_010.run()`: 백테스트를 실행하고 결과를 stat_010에 저장합니다.
- stat_010: 이 변수에는 분석할 수 있는 백테스팅 통계가 포함되어 있을 것입니다.

중요 사항:

- 이 전략은 저희의 트랜스포머 모델 예측과 기술적 지표(EMAs)를 결합하여 신호를 생성합니다.
- 다음 함수에서 다른 조건들을 실험하여 더 정교한 거래 전략을 만들어 볼 수 있습니다.
- 백테스팅 결과는 미래 성과를 보장하지 않을 수 있으며, 실제 거래에서 적절한 리스크 관리가 중요합니다.

<div class="content-ad"></div>

## 2D CNN 모델에 대한 결론:

해당 코드는 Keras Tuner를 사용하여 하이퍼파라미터 튜닝된 2D CNN 모델을 설정합니다. 이 모델은 암호화폐의 시계열 데이터에 대한 다중 클래스 분류를 처리하도록 설계되었습니다. 2D CNN을 활용함으로써 모델은 시간과 특성에 걸쳐 미묘한 패턴을 포착할 수 있으며, 이는 1D CNN과 비교하여 보다 정확하고 견고한 예측을 할 수 있을 것으로 기대됩니다. 하이퍼파라미터 튜닝의 사용은 모델이 주어진 작업에 최적화되어 성능이 더욱 향상되도록 보장합니다.

# 다른 자산에 2D CNN 모델 적용 및 최상의 자산을 선정하는 방법:

여기서부터는 동일한 훈련된 모델을 사용하여 다른 자산에 대한 백테스트를 수행한 후 최상의 자산을 선정하는 방법에 대해 설명하겠습니다. 트레이딩뷰에서 데이터를 다운로드한 후 모든 자산에 대해 백테스트를 진행한 결과를 기반으로 최상의 자산을 선정하는 방법에 대해 설명하겠습니다.

<div class="content-ad"></div>

## 필요한 패키지 가져오고 모델 및 거래소 API 설정하기

```js
import time
import logging
import io
import contextlib
import glob
import ccxt
from datetime import datetime, timedelta, timezone
import keras
from keras.models import save_model, load_model
import numpy as np
import pandas as pd
import talib as ta
from sklearn.preprocessing import MinMaxScaler
import warnings
from threading import Thread, Event
import decimal
import joblib
from tcn import TCN

# from pandas.core.computation import PerformanceWarning
# 성능 경고 무시
warnings.filterwarnings("ignore")
# 주의: 다른 노트북에서 교육한 모델로 모델을 교육하고 여기에서 가장 성공적인 훈련된 모델을 사용하십시오.
# model_file_path = './model_lstm_1tp_1sl_2p5SlTp_April_5th_ShRa_1_49_15m.hdf5'
model_file_path = './models/transformer_model_55sl_55tp_eth_15m_may_13th_ShRa_0.78.keras'
model_name = model_file_path.split('/')[-1]
##################################### 모델 로드 #######################################
# 주의: LSTM 기반 신경망 모델의 경우 아래와 같이 model_file_path를 사용하여 직접 load_model 할 수 있습니다.
# 미리 훈련된 모델을 로드하려면, 케라스 훈련 모델은 keras.models.load_model만 사용합니다. joblib에서는 사용하지 않습니다.
model = load_model(model_file_path)
# # 또는
# model = tf.keras.models.load_model(model_file_path)
# 주의: TCN 기반 신경망 모델의 경우 모델을로드 할 때 custom_objects를 추가해야 합니다. 아래에 제공되어 있습니다.
# # 사용자 정의 객체를 지정하기 위한 사전 정의
# custom_objects = {'TCN': TCN}
# model = load_model(model_file_path, custom_objects = custom_objects)

##########################################################################################
########################## 거래소 정보 추가 ##############################
exchange = ccxt.binanceusdm(
    {
        'enableRateLimit': True,  # Manual에서 필요
        # 필요한 경우 다른 인증 매개변수 추가
        'rateLimit': 250, 'verbose': True
    }
    )
# 주의: https://testnet.binancefuture.com/en/futures/BTCUSDT에서 테스트넷 API를 사용했습니다 (다양한 자산에 대한 유동성 문제 및 많은 다른 문제가 있지만 순수한 테스트 목적으로 사용할 수 있음)
#  kraken testnet creds pubkey - K9dS2SK8JURMl9F300lguUhOS/ao3HM+tfRMgJGed+JhDfpJhvsC/y           privatekey - /J/03PPyPwsrPsKZYtLqOQNPLKZJattT6i15Bpg14/6ALokHHY/MBb1p6tYKyFgkKXIJIOMbBsFRfL3aBZUvQ1
# api_key = '8f7080f8821b58a53f5c49f00cbff7fdccca9c9154ea'
# secret_key = '1e58391a46a7dbb098aa5121d3e69e3a6660ba8c38f'

# exchange.apiKey = api_key
# exchange.secret = secret_key
# exchange.set_sandbox_mode(True)

# 주의: 실제 거래를 원하시는 경우, 아래 5줄을 주석 처리하고 위의 5줄을 주석 처리하신 후, 본인의 api_key 및 secret_key로 변경하세요 (아래는 더미이므로 마음껏 사용하십시오. 그리고 거래소에서 API를 생성할 때 'futures' 권한을 주셔야 합니다)
api_key = 'CxUdC80c3Y5Nf1iRJMZJelOCfFJWISbQsasCb4Zdskx7MM8uCl'
secret_key = 'p4XwsZwmmNswzDHzE5TSUOgXT5tASArfSOfYrBMtezlCpDGtz'
exchange.apiKey = api_key
exchange.secret = secret_key
exchange.set_sandbox_mode(False)
#######################################################################################
# exchange.set_sandbox_mode(True)
exchange.has
# exchange.fetchBalance()["info"]["assets"]
exchange.options = {'defaultType': 'future', # 'margin' 또는 'spot'
                    'timeDifference': 0,  # 적절한 시간 차이 초기 값을 설정
                        'adjustForTimeDifference': True,
                        'newOrderRespType': 'FULL',
                        'defaultTimeInForce': 'GTC'}
```

제공된 코드 스니펫은 훈련된 모델을 로드하고 암호화폐 거래소(Binance)에 연결하여 백테스트를 기반으로 자산을 잠재적으로 선정하는 방법을 보여줍니다. 여기서는 다음과 같은 내용이 포함되어 있습니다:

Import:

<div class="content-ad"></div>

### 시간, 로깅, 데이터 조작 (판다스, 넘파이), 머신 러닝 (케라스, 사이킷런), 기술지표 (탈립), 쓰레딩 및 기타 표준 라이브러리

모델 로딩:

- 주석을 통해 각 모델 유형에 따른 모델 로딩 방법의 차이를 설명:
  - LSTM 모델: 직접 keras.models의 load_model을 사용합니다 (귀하의 코드에 표시된 대로).
  - TCN 모델: 로딩 시 사용자 정의 객체를 명시해야 합니다 (custom_objects='`TCN`: TCN'을 사용).

거래소 연결하기:

<div class="content-ad"></div>

- 바이낸스 거래소와 상호 작용하기 위해 ccxt.binanceusdm 오브젝트(거래소)를 생성합니다.
- API 자격 증명을 설정하고 책임 있는 API 이용을 위해 요율 제한을 활성화합니다.
- 테스트넷과 실 API 사용 옵션에 대한 주석이 있습니다.

중요 사항:

- API 키 교체: 더미 api_key와 secret_key를 실제 바이낸스 API 자격 증명으로 교체하십시오 (실거래 시). API에 "futures" 권한이 있는지 확인하세요.
- 백테스팅 표시되지 않음: 이 코드는 모델 로딩 및 거래소 연결에 중점을 두었습니다. 실제 백테스팅 루프와 자산 선정 로직은 포함되어 있지 않습니다.

다음 단계:

<div class="content-ad"></div>

- 백테스팅 루프: 원하는 자산을 반복하는 루프를 구현해야합니다:

   - 각 자산에 대한 거래소에서 historcal 데이터를 다운로드합니다 (exchange.fetch_ohlcv를 사용).
   - 데이터를 전처리합니다 (스케일링, 특성 공학).
   - 모델을 로드하여 예측을 수행합니다 (model.predict).
   - 이전 예제와 유사한 예측 및 기술적 지표를 사용하여 백테스트 전략을 적용합니다.
   - 각 자산에 대한 백테스트 결과를 저장합니다.

- 선정 기준: 저장된 백테스트 결과를 분석하고 선택한 메트릭에 기반하여 필터/정렬을 적용하여 성능이 우수한 자산을 추립니다.
- 리스크 관리: 백테스팅은 평가를 위한 것이며 앞으로의 성공을 보장하는 것이 아닙니다. 이러한 선정된 자산을 실제 거래에 사용하기 전에 적절한 리스크 관리 전략을 구현하세요.

```python
from sklearn.preprocessing import MinMaxScaler
from backtesting import Strategy, Backtest
import os
import json
import pandas as pd
import talib as ta
import numpy as np
from concurrent.futures import ThreadPoolExecutor
import threading

import time
import ccxt
from keras.models import save_model, load_model
import warnings
import decimal
import joblib
import nest_asyncio
# from pandas.core.computation import PerformanceWarning
# PerformanceWarning 억제
warnings.filterwarnings("ignore")
# 사전 훈련된 모델을 로드합니다.
# model = load_model('best_model_tcn_1sl_1tp_2p5SlTp_success.pkl')
# 백테스팅을 위한 사용자 정의 자산 사전을 루프 외부에서 정의합니다.
custom_assets = {}
# 텍스트 파일에서 사용자 정의 자산을 로드하는 함수입니다.
def load_custom_assets():
    if os.path.exists('custom_assets.txt'):
        try:
            with open('custom_assets.txt', 'r') as txt_file:
                return json.loads(txt_file.read())
        except json.JSONDecodeError as e:
            print(f"custom_assets.txt에서 JSON 디코딩 오류 발생: {e}")
            return {}
    else:
        print("custom_assets.txt 파일을 찾을 수 없습니다. 빈 사전을 초기화합니다.")
        custom_assets = {}
        save_custom_assets(custom_assets)
        return custom_assets
# 스레딩 락을 정의합니다.
file_lock = threading.Lock()
# 사용자 정의 자산을 텍스트 파일에 저장하는 함수입니다.
def save_custom_assets(custom_assets):
    with file_lock:
        with open('custom_assets.txt', 'w') as txt_file:
            json.dump(custom_assets, txt_file, indent=4)
```

<div class="content-ad"></div>

제공된 코드는 사용자 정의 자산을 관리하고 멀티스레드 백테스팅을 준비하는 데 초점을 맞추고 있어요. 여기에 대해 알아봅시다:

Imports:

- 데이터 조작을 위한 라이브러리(pandas, numpy), 기술적 지표(talib), 백테스팅 프레임워크(backtesting), 스레딩 및 기타 라이브러리들이 포함되어 있어요.

사용자 정의 자산 관리:

<div class="content-ad"></div>


친구들아~ 지금은 custom_assets 딕셔너리에 대해 이야기해볼게. 
이 딕셔너리는 주로 백테스팅을 위한 사용자 정의 자산(아마도 심볼이나 이름들)을 저장해. 

여기 load_custom_assets 함수가 있어. 요 함수는 custom_assets.txt 파일이 있는지 확인해.
- 파일이 존재하면, JSON 내용에서 딕셔너리를 로드하려 해. 잠재적인 JSON 디코딩 오류를 처리하도록 설정했어.
- 파일이 없으면, 빈 딕셔너리를 초기화하고 저장한 뒤, 반환해.

이렇게요! 이제 custom_assets와 load_custom_assets 함수에 대해 조금 더 이해가 가겠죠? 🌟


<div class="content-ad"></div>

'카스터마이즈 자산' 함수:

- 동시에 발생할 수 있는 쓰기 작업 중 안전하게 파일에 액세스하는 데 사용되는 스레딩 락(파일 락)을 활용합니다.
- custom_assets 사전을 custom_assets.txt 파일로 JSON 형식으로 저장합니다.

다음 단계:

- 백테스팅 함수: 백테스팅 로직을 위한 함수를 정의할 가능성이 높습니다. 이 함수는 다음과 같은 일을 할 것입니다:

<div class="content-ad"></div>

**주요 단골님의 심벌을 입력하세요.**

**- 주요 단골님의 심벌에 대한 과거 데이터를 다운로드합니다.**
**- 데이터를 전처리합니다 (스케일링, 피처 엔지니어링).**
**- 로드된 모델을 활용하여 예측을 수행합니다.**
**- 이전 예시와 유사한 백테스팅 전략을 적용하여 예측과 기술적 지표를 반영합니다.**
**- 주요 단골님의 시장 거래 결과 (샤프 비율, 손실액 등)를 계산하고 저장합니다.**

**2. 멀티스레드 백테스팅:**

**- ThreadPoolExecutor와 스레드 기능을 활용하여 여러 자산의 동시 다운로드와 백테스팅을 수행할 수 있습니다. 이러한 접근은 순차적인 방법에 비해 효율성을 크게 향상시킬 수 있습니다.**
**- 사용자 정의 자산 사전과 그 관리 함수는 스레드 풀 내에서 백테스팅 함수에 자산 심볼을 제공하는 데 중요한 역할을 합니다.**

**추가 사항:**

<div class="content-ad"></div>

**안녕하세요!** 

아래는 훈련된 모델 파일의 실제 경로로 `best_model_tcn_1sl_1tp_2p5SlTp_success.pkl`을(를) 바꿔주시기 바랍니다.
데이터 다운로드, 백테스팅 계산 중 발생할 수 있는 잠재적인 문제에 대한 오류 처리 및 로깅 메커니즘을 고려해주세요.

아래 코드에서는 공식 Binance API를 사용하여 Binance Futures에서 제공하는 USDT 퍼펫쥴 계약 목록을 가져옵니다. 정확히는 binance Futures 영구 USDT 자산에서 가져옵니다. 만약 4xx 오류가 발생하면, 국제 제한이나 VPN 서버가 binance의 작동을 제한하는 지역에 연결되었다는 것을 의미합니다. 다음 셀에서 제공되는 자산 컬렉션을 사용할 수 있습니다.

```python
#자산 수집 scell에서 주어진 값 사용
```

출력:
'BTCUSDT.P', 'ETHUSDT.P', 'BCHUSDT.P', 'XRPUSDT.P', 'EOSUSDT.P', 'LTCUSDT.P', 'TRXUSDT.P', 'ETCUSDT.P', 
'LINKUSDT.P', 'XLMUSDT.P', 'ADAUSDT.P', 'XMRUSDT.P', 'DASHUSDT.P', 'ZECUSDT.P', 'XTZUSDT.P', 'BNBUSDT.P', 
...

이 코드 조각은 Binance Futures에서 사용 가능한 USDT 퍼펫쥴 계약 목록을 검색합니다. 만약 4xx 또는 5xx 상태 코드가 발생하면 예외를 발생시킵니다. 데이터를 받아와서 퍼펫쥴 USDT 계약 목록을 반환하게 됩니다. 실패할 시 적절한 에러 핸들링이 이루어집니다.

언제든지 질문이 있으시면 말씀해주세요! 🙂

<div class="content-ad"></div>

기능:

`get_binance_futures_assets` 함수:

- 교환 정보를 검색하기 위한 API 엔드포인트 URL을 정의합니다.
- 요청 중 발생할 수 있는 오류를 처리하기 위해 try-except 블록을 사용합니다.

try 블록 내부에서:

<div class="content-ad"></div>

- 바이낸스 API 엔드포인트에 GET 요청을 보냅니다.
- 4xx(클라이언트 오류) 또는 5xx(서버 오류) 범위에 있는 상태 코드에 대해 예외를 발생시켜 실패를 나타냅니다.
- 성공한 요청의 JSON 응답을 구문 분석합니다.

응답 데이터에서 심볼을 추출합니다:

- JSON 데이터의 `symbols` 목록을 반복합니다.

다음 기준으로 자산을 필터링합니다:

<div class="content-ad"></div>

- `contractType` 값이 `PERPETUAL` 일 때는 무기한 계약을 나타냅니다.
- `quoteAsset` 값이 `USDT` 일 때는 USDT로 인용된 계약을 나타냅니다.
- 기준을 충족하는 자산 심볼의 목록을 작성하고 반환합니다.
- except 블록에서는 잠재적인 요청 예외를 catch하고 오류 메시지를 출력합니다. 또한 실패 시 빈 목록을 반환합니다.

결과 출력:

- get_binance_futures_assets 함수를 호출하여 자산 목록을 검색합니다.
- 검색된 자산과 그 개수를 나타내는 메시지를 인쇄합니다.

추가 사항:

<div class="content-ad"></div>

**이 접근 방식은 공식 바이낸스 API를 활용하며, 앞으로 요율 제한이나 변경 사항의 대상이 될 수 있습니다. 적절한 오류 처리 및 재시도 메커니즘을 구현하는 것을 고려해보세요.**

**코드는 성공적인 API 호출을 가정합니다. 특정 오류 코드(예: "Too Many Requests"에 대한 429)를 확인하고 이를 공소적으로 처리하는 추가 확인사항을 추가하는 것이 좋습니다. 예를 들어, 일정 지연 후 다시 시도하는 방식입니다.**

```python
# !pip install --upgrade --no-cache-dir git+https://github.com/rongardF/tvdatafeed.git

import os
import json
import asyncio
from datetime import datetime, timedelta
import pandas as pd
from tvDatafeed import TvDatafeed, Interval
```

**위 코드 조각은 tvDatafeed 라이브러리를 사용하여 TradingView에서 여러 자산에 대한 거래 데이터를 다운로드하는 방법을 보여줍니다. 여기에 대해 자세히 살펴보겠습니다:**

**Imports를 설정합니다:**

<div class="content-ad"></div>

- 비동기 프로그래밍을 위한 라이브러리(asyncio), 날짜 처리(datetime), 데이터 조작(pandas), 파일 처리(os, json)를 포함합니다.
- TradingView와 상호 작용하기 위해 tvDatafeed에서 TvDatafeed 클래스를 가져옵니다.

TvDatafeed 오브젝트:

- 사용자 이름 및 암호 없이 (무료 계정 가정) TvDatafeed 오브젝트(tv)를 초기화합니다. 유료 계정은 자격 증명이 필요할 수 있습니다.

타임프레임과 간격:

<div class="content-ad"></div>

- 데이터 다운로드를 위한 원하는 시간 프레임(timeframe) 설정 ("15m"은 15분 간격을 의미합니다).
- 일련의 if 문을 사용하여 timeframe을 해당하는 Interval 열거형 값에 매핑합니다.

심볼 목록:

- 바이낸스 선물에서 미결 USDT 계약을 가진 암호화폐를 나타내는 긴 심볼(symbol) 목록을 정의 (".P" 접미사로 식별함).

비동기 프로그래밍 설정:

<div class="content-ad"></div>

"nest_asyncio.apply()`을 초기화하여 비동기 함수를 비-비동기 컨텍스트 내에서 사용할 수 있도록 합니다.

다운로드 기능:

- `symbol`을 입력값으로 취하는 비동기 함수 `download_data(symbol)`을 정의합니다.

`tv.get_hist`를 사용하여 해당 심볼의 기록 데이터를 다운로드하려고 시도합니다:"

<div class="content-ad"></div>

- 이상적인 상항, 거래소(“바이낸스”), 간격, 바 수(20000), 그리고 확장된 세션(사전시장/사후시장 데이터를 캡처하기 위함)을 명시합니다.
- 다운로드된 데이터(data)가 비어 있는지 확인합니다.

데이터가 있을 경우:

- 인덱스(타임스탬프)를 “date”라는 새 열에 문자열로 변환합니다.
- 다운로드된 데이터를 저장하기 위해 tradingview_crypto_assets_'timeframe'이라는 폴더를 생성합니다(없을 경우 생성).
- 파일명을 생성하기 위해 심볼에서 “.P”를 “/USDT:USDT”로 대체하고 “.json”을 추가합니다.
- DataFrame을 to_dict(orient=`records`)를 사용해 사전으로 변환합니다.
- 이 사전을 생성된 파일명으로 JSON으로 저장합니다.
- 성공 메시지를 출력합니다.

데이터가 없는 경우:

<div class="content-ad"></div>

마법사의 주문: 마법진 활성화 중...

- 이 심볼에 대한 데이터가 없음을 나타내는 메시지를 출력합니다.
- 다운로드 중 예외(Exception)를 catch하고 예외 세부 정보와 함께 오류 메시지를 출력합니다.

주요 기능:

- 다음을 정의하는 비동기 함수 main:
- 데이터 목록에서 각 심볼에 대해 download_data를 호출하는 비동기 작업(tasks)의 목록을 생성합니다.
- 모든 다운로드 작업을 동시에 실행하기 위해 asyncio.gather(*tasks)를 사용합니다.

다운로드 실행 중: 🔮

<div class="content-ad"></div>

- main 함수 내에서 비동기 작업을 실행하기 위해 asyncio.run(main())을 사용합니다.

중요 사항:

- 이 코드는 많은 심볼의 데이터를 검색합니다. 상당량의 데이터를 다운로드하면 무료 계정 제한을 초과하거나 많은 시간이 소요될 수 있습니다. 요율 제한을 고려하고 그에 맞게 조정해주세요.
- 이 코드는 "P" 접미사가 있는 특정 심볼 형식을 가정합니다. 다른 심볼 형식에 대해 수정해야 할 수도 있습니다.
- 오류 처리를 개선하기 위해 네트워크 오류, API 오류 등 다양한 예외 유형에 대한 구체적인 확인을 구현할 수 있습니다.

# 특정 ML/DL 모델을 위한 다중 자산의 초강점화:

<div class="content-ad"></div>

이 코드는 암호화폐 거래 전략을 백테스팅하는 관련된 파이썬 코드입니다. 여기서 코드의 기능을 요약해보겠습니다:

1. 데이터 처리 및 분석:
   - JSON 파일을 읽어와서 데이터프레임으로 변환합니다.
   - 데이터프레임에 다양한 기술적 분석 지표를 계산하고 새로운 칼럼을 추가합니다.
   - 거래 기간(Timeframe)을 설정하고 필요한 전처리 및 데이터 변환을 수행합니다.

2. 모델 및 전략 작성:
   - 주어진 매개변수를 사용하여 전략을 초기화하고 거래 시그널을 정의합니다.
   - 매매 조건, 손실 한도, 수익 한도, 레버리지, 롤링 스톱 등을 설정합니다.

3. 백테스트 실행:
   - 백테스트를 실행하고 결과를 얻어와서 수익률, 드로다운, 승률, 최고 및 최악 거래 등을 평가합니다.
   - 최적의 매개변수를 찾기 위해 전략을 최적화하고 다시 백테스트를 실행합니다.

4. 결과 저장 및 처리:
   - 각 자산의 백테스트 결과를 평가하고, 일정 기준을 충족하는 경우 사용자 정의 자산 목록(custom_assets)에 추가합니다.
   - 최종적으로 custom_assets를 JSON 파일로 저장하고 필요한 처리를 수행합니다.

5. 병렬 처리 및 성능 최적화:
   - CPU 코어를 활용하여 데이터 처리를 병렬로 실행하고, 성능 최적화를 위해 여러 가지 시나리오에 대한 백테스트를 실행합니다.

이 코드는 암호화폐 거래 전략을 백테스팅하고 최적화하는 과정을 자동화하고 결과를 효율적으로 처리하는데 도움이 되는 전략입니다.

<div class="content-ad"></div>

데이터 처리:

- process_json 함수: 이 함수는 암호화폐 가격 데이터가 포함된 JSON 파일을 읽습니다.
- 데이터 클리닝 및 변환: 데이터를 정리하고 변환하는 과정은 다음과 같습니다:

- 열 이름을 표준 이름으로 변경합니다 (예: 'date'를 'Date'로).
- 'Date' 열을 datetime 형식으로 변환합니다.
- 'Date'를 인덱스로 설정합니다.
- 'Close' 열의 누락된 값을 이전 종가로 채웁니다.
- 'symbol' 열에서 심볼 이름을 추출합니다.

- 기술 지표 계산: 스크립트는 ta 라이브러리(가정)를 사용하여 ATR, EMA, RSI 등과 같은 다양한 기술 지표를 계산합니다.
- 피쳐 엔지니어링: 추가적인 기능인 수익률, 변동성, 거래량 기반 지표 및 모멘텀 기반 지표를 생성합니다.
- 데이터 스케일링: 스크립트는 MinMaxScaler를 사용하여 모델의 성능을 향상시키기 위해 데이터를 스케일링합니다.
- 데이터 재구성: 데이터는 거래 전략에 적합한 형식(예: 과거 가격 데이터의 시퀀스)으로 재구성됩니다.

<div class="content-ad"></div>

### 백테스팅 전략:

- **SIGNAL_3 함수**: 이 함수는 어떤 기준에 따라 거래 신호를 정의할 것으로 보입니다.
- **MyCandlesStrat_3 클래스**: 이 클래스는 Backtrader 라이브러리를 사용하여 거래 전략을 정의합니다. 핵심 요소는 다음과 같습니다:

    - **손절가 및 익절가**: 이들은 long 및 short 포지션에 대해 미리 정의된 백분율(BEST_STOP_LOSS_sl_pct_long 등)에 기초하여 설정됩니다.
    - **지정가 주문**: 주문 실행을 특정 가격 범위 내에서 보장하기 위해 사용됩니다.
    - **트레일링 손절가**: 현재 이익을 기반으로 동적으로 조정되어 이익을 확보합니다.
    - **시간 기반 수익 실현**: 일정 시간 동안 자산을 보유한 후에 수익이 자동으로 확정됩니다.
    - **레버리지**: 전략은 미리 정의된 레버리지 배수(BEST_LEVERAGE_margin_leverage)를 사용합니다.

### 백테스팅과 분석:

<div class="content-ad"></div>

- 백테스트: 이 스크립트는 MyCandlesStrat_3 전략을 사용하여 100,000의 초기 자본으로 처리된 데이터에 대한 백테스트를 수행합니다.
- 성능 지표: 백테스트 결과에는 수익률, 샤프 지수, 승률 및 낙폭과 같은 다양한 성능 지표가 포함될 것으로 예상됩니다 (제공된 코드에 명시적으로 표시되지는 않음).

조건부 논리:

- 이 스크립트는 특정 성능 조건(높은 수익률, 좋은 이익 요소 등)이 충족되었는지 확인합니다.
- 조건이 충족되면, 이 스크립트는 해당 특정 자산에 대한 거래 전략 매개변수를 저장할 수 있습니다.

ThreadPoolExecutor 클래스의 사용: JSON 파일의 병렬 처리를 위해 ThreadPoolExecutor 클래스를 사용하는 방법. 다음은 그 기능에 대한 세부적 설명입니다:

<div class="content-ad"></div>

# 스레드 워커 함수 (thread_worker):

- 이 함수는 하나의 JSON 파일 경로를 입력으로 받습니다 (file).
- JSON 데이터를 처리하는 process_json 함수를 호출합니다 (다른 곳에 정의되어 있다고 가정).
- 처리된 결과를 반환하는데, 일반적으로 Pandas DataFrame (df), 심볼 이름 (symbol_name) 및 다른 사용자 정의 데이터 (custom_assets)를 반환할 것으로 예상됩니다.

# 메인 함수 (main):

- 지정된 폴더(./tradingview_crypto_assets_15m/) 내 모든 JSON 파일 목록을 가져옵니다.
- os.cpu_count()를 사용하여 사용 가능한 CPU 코어 수를 결정합니다.
- CPU 코어 개수를 기반으로 ThreadPoolExecutor에 max_workers 매개변수를 설정합니다 (가능한 경우 모든 코어를 사용하고, 그렇지 않으면 기본값은 1입니다).
- 처리에 사용할 코어 수를 출력합니다.
- 결정된 max_workers로 ThreadPoolExecutor를 생성합니다.
- JSON 파일 목록을 반복하고 각 파일 경로를 executor.submit(thread_worker, file)를 사용하여 스레드 풀에 제출합니다. 이렇게하면 각 파일을 동시에 처리하는 작업이 생성됩니다.
- 제출된 모든 작업(futures)이 완료될 때까지 기다렸다가 결과를 리스트(results)에 저장합니다.
- 처리된 결과를 반복하면서:
    - 결과가 None이면 다음 반복으로 건너뜁니다 (오류 처리 가능).
    - 그렇지 않으면 결과를 풀어낸 후 (df, symbol_name 및 필요에 따라 custom_assets), 처리된 심볼과 사용자 지정 자산에 대한 정보를 출력합니다.
    - 사용자 정의 데이터(custom_data)가 있으면 custom_assets를 추가 사용자 정의 데이터로 조건부 업데이트합니다 (논리는 완전히 표시되지 않음).

<div class="content-ad"></div>

### 3. Continuous Loop Function (`run_continuous_loop`):

- This function sets up an infinite loop (while True).
- Within the loop, it calls the main function repeatedly, likely for processing multiple JSON files in a batch.

### 4. Starting the Loop:

- The code spawns a new thread using `threading.Thread` and assigns its target to the `run_continuous_loop` function.
- It then kicks off the thread, commencing the continuous processing loop.

<div class="content-ad"></div>

Overall, this code snippet shows how JSON files can be processed in parallel using a thread pool based on CPU cores. The loop handles batches of files continuously.

The code presents a structure for backtesting a cryptocurrency trading strategy that utilizes technical indicators and includes risk management methods such as stop-loss and trailing stop-loss.

**Disclaimer:**

- Always remember, backtesting results may not accurately reflect future performance.
- Trading cryptocurrencies comes with substantial risks, so conducting thorough research before making any investment decisions is essential.

<div class="content-ad"></div>

## custom_assets.txt 출력:

```js
{
    "QNT/USDT:USDT": {
        "Optimizer_used": "1st backtest - Expectancy",
        "model_name": "cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.91_time_20240516025817.keras",
        "Optimizer_result": "QNT/USDT:USDT에 대한 백테스트는 2024년 3월 2일 11:45:00부터 2024년 5월 14일 03:45:00까지 72일 16시간 00분 동안 15분 단위의 시간 프레임을 사용하여 진행되었습니다. 승률 - 68.54%, 수익률 - 30.059%, 기대수익률 - 0.40557 및 샤프 비율 - 0.6621.",
        "stop_loss_percent_long": 0.052,
        "take_profit_percent_long": 0.055,
        "limit_long": 0.054,
        "stop_loss_percent_short": 0.052,
        "take_profit_percent_short": 0.055,
        "limit_short": 0.054,
        "margin_leverage": 1,
        "TRAILING_ACTIVATE_PCT": 0.045,
        "TRAILING_STOP_PCT": 0.005,
        "roi_at_50": 0.054,
        "roi_at_100": 0.05,
        "roi_at_150": 0.045,
        "roi_at_200": 0.04,
        "roi_at_300": 0.03,
        "roi_at_500": 0.01
    },
    "NMR/USDT:USDT": {
        "Optimizer_used": "1st backtest - Expectancy",
        "model_name": "cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.91_time_20240516025817.keras",
        "Optimizer_result": "NMR/USDT:USDT에 대한 백테스트는 2024년 3월 2일 11:45:00부터 2024년 5월 14일 03:45:00까지 72일 16시간 00분 동안 15분 단위의 시간 프레임을 사용하여 진행되었습니다. 승률 - 64.15%, 수익률 - 58.843%, 기대수익률 - 0.81714 및 샤프 비율 - 0.7911.",
        "stop_loss_percent_long": 0.052,
        "take_profit_percent_long": 0.055,
        "limit_long": 0.054,
        "stop_loss_percent_short": 0.052,
        "take_profit_percent_short": 0.055,
        "limit_short": 0.054,
        "margin_leverage": 1,
        "TRAILING_ACTIVATE_PCT": 0.045,
        "TRAILING_STOP_PCT": 0.005,
        "roi_at_50": 0.054,
        "roi_at_100": 0.05,
        "roi_at_150": 0.045,
        "roi_at_200": 0.04,
        "roi_at_300": 0.03,
        "roi_at_500": 0.01
    },
    "BNT/USDT:USDT": {
        "Optimizer_used": "1st backtest - Expectancy",
        "model_name": "cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.91_time_20240516025817.keras",
        "Optimizer_result": "BNT/USDT:USDT에 대한 백테스트는 2024년 3월 2일 11:45:00부터 2024년 5월 14일 03:45:00까지 72일 16시간 00분 동안 15분 단위의 시간 프레임을 사용하여 진행되었습니다. 승률 - 59.86%, 수익률 - 25.737%, 기대수익률 - 0.50737 및 샤프 비율 - 0.6972.",
        "stop_loss_percent_long": 0.052,
        "take_profit_percent_long": 0.055,
        "limit_long": 0.054,
        "stop_loss_percent_short": 0.052,
        "take_profit_percent_short": 0.055,
        "limit_short": 0.054,
        "margin_leverage": 1,
        "TRAILING_ACTIVATE_PCT": 0.045,
        "TRAILING_STOP_PCT": 0.005,
        "roi_at_50": 0.054,
        "roi_at_100": 0.05,
        "roi_at_150": 0.045,
        "roi_at_200": 0.04,
        "roi_at_300": 0.03,
        "roi_at_500": 0.01
    },
    "GMX/USDT:USDT": {
        "Optimizer_used": "1st backtest - Expectancy",
        "model_name": "cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.91_time_20240516025817.keras",
        "Optimizer_result": "GMX/USDT:USDT에 대한 백테스트는 2024년 3월 2일 11:45:00부터 2024년 5월 14일 03:45:00까지 72일 16시간 00분 동안 15분 단위의 시간 프레임을 사용하여 진행되었습니다. 승률 - 65.38%, 수익률 - 48.712%, 기대수익률 - 1.00621 및 샤프 비율 - 1.293.",
        "stop_loss_percent_long": 0.052,
        "take_profit_percent_long": 0.055,
        "limit_long": 0.054,
        "stop_loss_percent_short": 0.052,
        "take_profit_percent_short": 0.055,
        "limit_short": 0.054,
        "margin_leverage": 1,
        "TRAILING_ACTIVATE_PCT": 0.045,
        "TRAILING_STOP_PCT": 0.005,
        "roi_at_50": 0.054,
        "roi_at_100": 0.05,
        "roi_at_150": 0.045,
        "roi_at_200": 0.04,
        "roi_at_300": 0.03,
        "roi_at_500": 0.01
    },
    "T/USDT:USDT": {
        "Optimizer_used": "1st backtest - Expectancy",
        "model_name": "cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.91_time_20240516025817.keras",
        "Optimizer_result": "T/USDT:USDT에 대한 백테스트는 2024년 3월 2일 11:45:00부터 2024년 5월 14일 03:45:00까지 72일 16시간 00분 동안 15분 단위의 시간 프레임을 사용하여 진행되었습니다. 승률 - 64.71%, 수익률 - 133.093%, 기대수익률 - 1.47402 및 샤프 비율 - 0.596.",
        "stop_loss_percent_long": 0.052,
        "take_profit_percent_long": 0.055,
        "limit_long": 0.054,
        "stop_loss_percent_short": 0.052,
        "take_profit_percent_short": 0.055,
        "limit_short": 0.054,
        "margin_leverage": 1,
        "TRAILING_ACTIVATE_PCT": 0.045,
        "TRAILING_STOP_PCT": 0.005,
        "roi_at_50": 0.054,
        "roi_at_100": 0.05,
        "roi_at_150": 0.045,
        "roi_at_200": 0.04,
        "roi_at_300": 0.03,
        "roi_at_500": 0.01
    },
    "MATIC/USDT:USDT": {
        "Optimizer_used": "1st backtest - Expectancy",
        "model_name": "cnn_model_2d_15m_ETH_May_16_SL55_TP55_ShRa_0.91_time

<div class="content-ad"></div>

- 화폐 쌍을 키로 사용하는 사전입니다 (예: "ATOM/USDT:USDT").

각 자산의 내용:

- Optimizer_used: 백테스팅에 사용된 최적화 방법을 지정합니다 (여기서는 "1차 백테스트 - 기대값"입니다).
- model_name: 거래 전략에 사용된 모델 이름을 나타냅니다 ("transformer_model_55sl_55tp_eth_15m_may_13th_ShRa_0.78.keras").
- Optimizer_result: 특정 자산에 대한 백테스팅 결과에 대한 상세 설명입니다. 이는 다음을 포함합니다:
  - 백테스트의 시작 및 종료 날짜.
  - 백테스트 기간.
  - 사용된 시간대 (예: 15분).
  - 이긴 비율 백분율.
  - 수익률 백분율.
  - 기대값 백분율.
  - 샤프 비율.
  - stop_loss_percent_long/short: 롱 및 숏 포지션에 대한 손절 백분율을 정의합니다.
  - take_profit_percent_long/short: 롱 및 숏 포지션에 대한 익절 백분율을 정의합니다.
  - limit_long/short: 진입 주문에 허용된 최대 가격 편차를 정의합니다 (과도한 슬리피지를 방지하기 위한 것으로 예상됨).
  - margin_leverage: 마진 트레이딩에 사용된 레버리지를 지정합니다 (1로 설정되어 있어 레버리지가 없음을 나타냄).
  - TRAILING_ACTIVATE_PCT 및 TRAILING_STOP_PCT: 이는 동적으로 손절을 조정하는 트레일링 손절 파라미터를 정의합니다.
  - roi_at_50, 100, 150 등: 이는 서로 다른 보유 기간에 대한 잠재적 이익 목표입니다 (예: roi_at_50은 시간의 50%를 보유할 때 목표 수익일 수 있습니다).

해석:

<div class="content-ad"></div>

- 이 정보는 특정 암호화폐에 대한 특정 거래 전략을 평가한 백테스팅 도구에서 나온 가능성이 높습니다.
- 결과는 각 자산에 대한 승률, 수익률 및 샤프 비율과 같은 성과 지표를 보여줍니다.
- 손절, 익절 및 레버리지 매개변수는 전략의 리스크 관리 측면을 정의합니다.

최종 선정된 자산 및 저장:

- 문서에는 "최종 선정된 자산"이 언급되지만 이들이 어떻게 식별되는지 명시적으로 표시되지는 않습니다. 백테스팅 결과를 기반으로 일정한 성과 기준을 충족하는 자산이 최종 선정된 자산으로 고려될 수 있다는 가능성이 있습니다.
- 이 최종 선정된 자산은 "saved_assets.txt"라는 파일에 저장되며, 제공된 데이터 스니펫과 동일한 형식으로 저장될 수 있습니다.
 
면책 성명:

<div class="content-ad"></div>

- 백테스팅 결과는 미래 성과를 보장하는 것은 아닙니다.
- 암호화폐 거래에는 상당한 위험이 딸립니다. 투자 결정 전 반드시 자신의 연구를 해야 합니다.

# 결론:

본 문서에서는 신경망 모델(구체적으로 2D CNN 모델)과 VishvaAlgo라는 트레이딩 봇을 활용하는 암호화폐 거래 시스템에 대해 설명합니다. 요약은 다음과 같습니다:

데이터 및 모델 훈련:

<div class="content-ad"></div>

- 시스템은 TradingView에서 바이낸스 선물거래소의 250개 이상의 암호화폐 자산에 대한 과거 데이터를 다운로드합니다.
- 2D CNN 기반의 신경망 모델을 학습하여, 15분 간격으로 3년 동안 Ethereum (ETHUSDT) 에서 9,800% 이상의 수익을 달성했습니다. 모델 학습에는 10만 개 이상의 행, 193가지 이상의 피처를 사용하며, 중립, 매수, 매도를 결정하기 위해 분류 기반 2D CNN 모델을 사용했습니다. (중요한 점: 이러한 수익은 시스템과 학습된 데이터에 따라 다르며 재확인이 필요합니다).

하이퍼파라미터 최적화와 자산 선택:

- 시스템은 학습된 모델에 가장 적합한 자산을 식별하기 위해 하이퍼파라미터 최적화 라이브러리인 Hyperopt를 사용합니다.
- 각 선정된 자산은 중지손실, 익절금액, 레버리지와 같이 모델 예측을 위해 특별히 맞춤화된 고유한 매개변수를 갖습니다.

VishvaAlgo — 거래 봇:

<div class="content-ad"></div>

- VishvaAlgo는 훈련된 모델과 사전 정의된 매개변수를 사용하여 실시간 거래를 자동화하는 데 도움을 줍니다.
- 이 봇은 다양한 신경망 모델과의 쉬운 통합을 제공합니다.
- VishvaAlgo의 기능과 혜택을 설명하는 비디오를 시청할 수 있습니다 — [링크](링크)

VishvaAlgo의 혜택:

- 훈련된 모델과 최적화된 자산 선택을 기반으로 거래를 자동화합니다.
- 사용자 정의 신경망 모델과의 쉬운 통합을 제공합니다.
- Patreon 페이지를 통해 구매를 위한 상세 설명 및 설치 가이드 제공됩니다.

면책 조항: 거래에는 리스크가 따릅니다. 지난 성과는 미래 결과를 보장하지 않습니다. VishvaAlgo는 트레이더들을 돕기 위한 도구로써 수익을 보장하지 않습니다. 책임있게 거래를 하시고 투자 결정을 내리시기 전에 철저한 연구를 진행해주세요.

<div class="content-ad"></div>

정성 가득한 인사,

Puranam Pradeep Picasso

Linkedin - [Puranam Pradeep Picasso](https://www.linkedin.com/in/puranampradeeppicasso/)

Patreon - [Puranam Pradeep Picasso](https://patreon.com/pppicasso)

<div class="content-ad"></div>

If you want to connect with me on social media, you can find me on:

Facebook - [Puranam P. Picasso](https://www.facebook.com/puranam.p.picasso/)

Twitter - [@picasso_999](https://twitter.com/picasso_999)