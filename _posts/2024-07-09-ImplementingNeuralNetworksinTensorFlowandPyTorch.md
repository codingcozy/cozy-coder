---
title: "2024년에 알아야 할 TensorFlow와 PyTorch로 신경망 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_0.png"
date: 2024-07-09 23:12
ogImage: 
  url: /assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_0.png
tag: Tech
originalTitle: "Implementing Neural Networks in TensorFlow (and PyTorch)"
link: "https://medium.com/towards-data-science/implementing-neural-networks-in-tensorflow-and-pytorch-3c1f097e412a"
---


환영합니다, 깊은 학습 그림 시리즈의 실용적인 실행 가이드로 여러분을 초대합니다. 이번 시리즈에서는 이전 글에서 탐구한 신경망 개념을 실제로 적용하여 이론과 실무 사이의 간극을 줄일 것입니다.

이전 글에서 언급한 아이스크림 수익을 예측하기 위한 간단한 신경망을 기억하시나요? TensorFlow를 사용하여 이를 구축할 것입니다. TensorFlow는 강력한 신경망을 생성하는 데 유용한 도구입니다.

그리고 가장 놀랍게도, 이를 27줄의 코드로 5분 미만에 할 수 있을 거에요!

먼저 시작해볼까요: TensorFlow가 무엇인가요?

<div class="content-ad"></div>

TensorFlow는 기계 학습 애플리케이션을 구축하고 배포하기 위한 풍부한 도구, 라이브러리 및 커뮤니티 리소스의 종합 생태계입니다. 구글에서 개발된 TensorFlow는 유연하고 효율적으로 설계되어 다양한 플랫폼에서 CPU부터 GPU, 심지어 TPU와 같은 특수 하드웨어에서 작동할 수 있습니다. "TensorFlow"라는 이름은 그 핵심 개념인 tensor flow에서 유래했습니다. 텐서는 다차원 배열로, 훈련 및 추론 과정 중에 계산 그래프를 통해 흐릅니다.

자, 이제 우리의 신경망을 구축해 봅시다. 이 모델의 목표는 온도와 요일 2가지 특성을 기반으로 매일의 아이스크림 매출을 예측하는 것입니다. 우리는 이 작업을 단계별로 접근하며, 각 프로세스 구성 요소를 설명하겠습니다.

## 단게 1: 데이터 준비

먼저, 이전에 사용한 아이스크림 매출 데이터를 번역해 보겠습니다…

<div class="content-ad"></div>

우리 신경망에 적합한 형식으로 변경되었습니다:

```js
import numpy as np

# 데이터
day = [2, 6, 1, 3, 2, 5, 7, 4, 3, 1]
temperature = [22, 33, 20, 25, 24, 30, 35, 28, 26, 21]
revenue = [1.51, 2.22, 1.37, 1.77, 1.64, 2.04, 2.42, 1.90, 1.75, 1.45]

# 일과 온도를 하나의 특성 배열로 결합
X_train = np.column_stack((day, temperature))
y_train = np.array(revenue)
```

이렇게하면 우리의 입력 특성인 X_train이 생성됩니다…

<div class="content-ad"></div>


![The first image](/assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_1.png)

…and the target values, y_train:

![The second image](/assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_2.png)

## Step 2: Standardize the data


<div class="content-ad"></div>

다음으로 데이터를 표준화할 것입니다. 데이터를 표준화하는 것은 평균이 0이고 표준 편차가 1이 되도록 특성을 변환하는 중요한 전처리 단계입니다.

```python
from sklearn.preprocessing import StandardScaler

# 특성 표준화
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
```

이렇게 하면 모든 특성이 모델에 동등하게 기여하게 되어 수렴 속도와 안정성을 향상시킵니다.

## 단계 3: 신경망 구축

<div class="content-ad"></div>

이번 단계에서는 우리의 신경망 모델을 정의합니다. 이전에 우리는 하나의 은닉층과 두 개의 뉴런, ReLU 활성화 함수를 사용한 하나의 출력 뉴런으로 구성된 구조로 결정했습니다.

![image](/assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_3.png)

이와 같은 구조를 유지하고 위의 내용을 코드로 번역해 봅시다. TensorFlow의 Keras API를 사용하여 신경망을 구성합니다.

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

# 모델 초기화
model = Sequential()

# 은닉층 추가 - 2개의 뉴런, ReLU 활성화 함수, 2개의 입력
model.add(Dense(2, input_dim=2, activation='relu'))

# 출력층 추가 - 1개의 뉴런, ReLU 활성화 함수
model.add(Dense(1, activation='relu'))
```

<div class="content-ad"></div>

시퀀셜 모델은 레이어 스택을 구축할 수 있게 해줍니다. Dense 레이어들은 완전히 연결된 레이어로, 한 레이어의 각 뉴런이 다음 레이어의 모든 뉴런과 연결됩니다.

## 단계 4: 모델 컴파일 및 학습

학습 전에, 모델을 컴파일해야 합니다:

```js
# 모델 컴파일하기
model.compile(optimizer='adam', loss='mean_squared_error')
```

<div class="content-ad"></div>

컴파일은 학습 과정을 구성하는 중요한 단계입니다. 여기서 우리는 다음을 지정합니다:

- 옵티마이저: adam (Adaptive Moment Estimation), 이는 학습 중 학습 속도를 조절합니다.
- 손실 함수: 평균 제곱 오차 (MSE), 이는 예측값과 실제 값 사이의 평균 제곱 차이를 측정합니다.

이제 모델을 학습할 수 있습니다:


# 모델 학습
history = model.fit(X_train_scaled, y_train, epochs=100, verbose=1)


<div class="content-ad"></div>

The fit method is where the actual learning happens. First, we need to specify the input features (X_train_scaled), target values (y_train), and the number of training cycles (epochs). By adjusting the verbose parameter, we can control the level of output during training.

Let's visualize the training process using the following code snippet:

```python
import matplotlib.pyplot as plt

# Plot training loss over epochs
plt.plot(history.history['loss'])
plt.title('Model Training Loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.show()
```

![Training Loss](/assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_4.png)

<div class="content-ad"></div>

이 플롯은 시간이 지남에 따라 손실(예측 오차)가 줄어드는 것을 보여줍니다. 학습 과정에 대한 통찰을 제공합니다.

## 단계 5: 예측하기

마지막으로, 훈련된 모델을 사용하여 예측할 수 있습니다:

```python
from sklearn.metrics import mean_squared_error

# 훈련 데이터에 대한 예측
예측 = model.predict(X_train_scaled)
print("훈련 데이터에서 예측된 수익:", 예측)
```

<div class="content-ad"></div>

Here, we're using our trained model to forecast ice cream sales using the input features. To evaluate the accuracy of our predictions, we can employ the Mean Squared Error (MSE) as a measure of our model's precision.

```python
# Calculating the Mean Squared Error
mse = mean_squared_error(y_train, predictions)
print("Mean Squared Error on Training Data:", mse)
```

![Image](/assets/img/2024-07-09-ImplementingNeuralNetworksinTensorFlowandPyTorch_6.png)

<div class="content-ad"></div>

MSE 값은 우리가 희망했던 것만큼 낮지는 않지만, 괜찮아요. 이건 매우 기본적인 신경망이고, 전체적으로 복잡성을 추가하고 아키텍처를 조정해 결과를 향상시키는 것이 목적입니다.

이 예제는 간단한 데이터셋과 모델 아키텍처를 사용하지만, 우리가 다룬 원칙들은 더 복잡한 신경망 응용 프로그램을 위한 기초를 마련해줍니다. 딥러닝 여정을 계속하면 더 정교한 아키텍처와 큰 데이터셋을 다뤄야 하지만, 기본적인 프로세스는 변하지 않습니다.

# 보너스: PyTorch

이제 TensorFlow를 사용해 모델을 구현하는 방법을 보았으니, 다른 강력한 프레임워크 PyTorch로 동일한 결과를 이끌어내는 방법을 살펴봅시다. Facebook의 AI 연구소에서 개발된 PyTorch는 유연성과 효율성으로 유명하여 많은 사람들에게 인기가 있습니다.

<div class="content-ad"></div>

이게 다야! TensorFlow와 PyTorch를 사용하여 아이스크림 판매를 예측하는 간단한 신경망을 구현하는 방법을 배웠어요. 다음 기사에서는 두 프레임워크 모두에서 합성곱 신경망(CNN)을 구현하는 방법을 다룰 거예요.

언제나 그렇듯이, 질문이나 의견이 있으시면 망설이지 마시고 LinkedIn에서 저와 연락하세요!