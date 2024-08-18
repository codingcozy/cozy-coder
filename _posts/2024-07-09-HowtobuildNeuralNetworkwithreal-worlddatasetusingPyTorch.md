---
title: "PyTorch로 실세계 데이터셋을 사용하여 신경망을 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_0.png"
date: 2024-07-09 23:47
ogImage:
  url: /assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_0.png
tag: Tech
originalTitle: "How to build Neural Network with real-world dataset using PyTorch"
link: "https://medium.com/gopenai/how-to-build-neural-network-with-real-world-dataset-using-pytorch-45b34b4e9876"
isUpdated: true
---

### 신경망을 구축하고 훈련하는 단계별 가이드: FitBit Fitness Tracker Dataset 활용

![FitBit Fitness Tracker Dataset](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_0.png)

안녕하세요! 이번 포스트에서는 캐글(Kaggle)에서 제공하는 FitBit Fitness Tracker Dataset을 활용하여 실제 데이터셋을 사용해 신경망 모델을 구축하고 훈련하는 방법을 쉽게 보여드릴게요.

저희는 TotalSteps, TotalDistance, VeryActiveMinutes와 같은 익숙한 일상 사용자 활동을 데이터셋의 특징으로 사용하여 신경망 모델을 구축하고 훈련할 거예요. 그리고 새로운 데이터셋을 활용해 총 칼로리 소모에 대한 예측도 할 수 있답니다. 함께해요!

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

안녕하세요! 이번 글을 함께 따라오시는 분들께서는 Python, PyTorch, 그리고 신경망의 기본 개념을 알고 있다고 가정하고 진행하려고 합니다. 이 외에는 걱정하지 마세요. 이 포스트를 따라오시면 글을 끝까지 읽은 후에 여러분만의 신경망을 구축하고 학습시킬 수 있을 거에요. 또한 다른 사용 사례에 맞게 더 맞춤화할 수도 있답니다.

이 모든 것을 쉽게 달성할 수 있는 6가지 간단한 단계가 준비되어 있어요.

자, 그럼 시작해봅시다!

## 단계 1: 필요한 라이브러리 가져오기

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

먼저 필요한 클래스, 함수 및 모듈을 아래와 같이 가져와야 합니다. 각 항목에 대한 자세한 내용은 해당 구현 중에 후속적으로 제공될 것입니다. 아래의 가져오기 코드를 실행하기 전에 최신 버전의 Python 및 PyTorch가 설치되어 있는지 확인하십시오.

```python
import torch
import pandas as pd
import numpy as np

import torch.nn as nn
from torch.utils.데이터 import DataLoader, TensorDataset
from sklearn.model_selection import train_test_split

import matplotlib.pyplot as plt
```

## 단계 2: 데이터 로드 및 준비

우선 FitBit Fitness Tracker 데이터 세트를 여기에서 다운로드하세요. 아래와 같이 `pandas` 함수를 사용하여 데이터셋 파일 `dailyActivity_merged.csv`을 DataFrame 변수에 로드하고 데이터가 성공적으로 로드되었는지 확인하십시오.

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

```python
# 사용자가 CSV 파일을 저장한 파일 경로를 제공할 수 있습니다
fitbit_df = pd.read_csv("/content/dailyActivity_merged.csv")

# CSV 파일의 처음 5행을 표시합니다
fitbit_df.head()
```

![Fitbit Dataframe](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_1.png)

위 데이터프레임에는 많은 수의 특성이 있습니다. 교육 목적으로 우리는 특성의 수를 6개로 줄일 것입니다. 그러나 프로덕션 어플리케이션에서는 특성을 줄이는 것이 권장되지 않습니다. 모델은 더 많은 특성으로 더 나은 정확도를 달성할 수 있습니다.

```python
features = ['TotalSteps', 'TotalDistance', 'VeryActiveDistance', 'ModeratelyActiveDistance', 'VeryActiveMinutes', 'FairlyActiveMinutes', 'Calories']
fitbit_df = fitbit_df[features]

# 모든 특성 값은 숫자여야 합니다. 네트워크는 비숫자 데이터를 처리할 수 없습니다.
fitbit_df.head()
```

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

우주 카드를 당겨 주셔서 감사합니다!

여기서 어떤 작업을 수행해야 하는지 이미 잘 이해하신 것 같아요. 마법의 세계인 데이터 전처리를 통해 모델을 적합하게 만들어가는 과정을 함께 걸어나가는 건 정말 흥미롭죠. 데이터 품질이 매우 중요하다는 것을 잘 이해하셨군요. "내뱉지도 않고 듣지도 않는 쓰레기를 먹이면, 쓰레기만큼의 결과를 만들어낸다"고 하죠.

다음 단계로는 fitbit_df 필드를 입력 X와 출력 y로 분할하고 해당 값들을 PyTorch tensor로 변환하는 것이네요. 모든 데이터를 PyTorch tensor로 변환한다면 잠재적 데이터 타입 불일치 문제를 피할 수 있을 거에요.

X = torch.tensor(fitbit_df.drop(columns=["Calories"], axis=1).to_numpy(), dtype=torch.float)
y = torch.tensor(fitbit_df["Calories"].to_numpy(), dtype=torch.long)

# X, y 값 확인

print("Input tensors: ", X[:5], "\n")
print("Output tensors: ", y[:5], "\n\n")

print("입력 tensor의 형태: ", X.shape)
print("출력 tensor의 형태: ", y.shape)

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

<img src="/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_3.png" />

이제 모델을 훈련하고 검증하며 테스트하기 위해 이 단일 데이터셋을 3개의 데이터셋으로 나눌 거에요. 훈련 데이터셋 80%, 검증 데이터셋 10%, 테스트 데이터셋 10%로 나눌 거에요. 이 작업을 수행하려면 sklearn.model_selection 모듈의 train_test_split 함수를 사용할 거에요.

```js
X_train, X_eval, y_train, y_eval = train_test_split(X, y, test_size=0.2, random_state=42)
X_val, X_test, y_val, y_test = train_test_split(X_eval, y_eval, test_size=0.5, random_state=42)

# 데이터셋 분할 전 모양

print("입력 텐서 모양: ", X.shape)
print("출력 텐서 모양: ", y.shape, "\n")

# 분할된 훈련, 검증, 테스트 데이터셋의 모양. 총합은 원본 데이터셋과 같아요.
# 각 데이터셋은 여전히 6개의 특징을 가지고 있어요. 이게 기대한 결과에요.

print("훈련 입력: ", X_train.shape, "훈련 출력: ", y_train.shape)
print("검증 입력: ", X_val.shape, "검증 출력: ", y_val.shape)
print("테스트 입력: ", X_test.shape, "테스트 출력: ", y_test.shape)
```

<img src="/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_4.png" />

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

특성 정규화: TotalSteps 특성 값이 4자리 숫자 범위에 있고 TotalDistance, VeryActiveDistance와 같은 특성 값이 1자리 숫자 범위에 있다면 발견했을 것입니다. 이러한 범위의 엄청난 차이는 훈련 과정에 영향을 미쳐 매끄럽지 않게 만들 수 있으며 이 문제를 관리하지 않으면 발산을 초래할 수 있습니다. 그래서 이 문제를 해결하기 위해 데이터 정규화 작업을 수행했습니다.

특성을 정규화하는 일반적인 방법 중 하나는 아래와 같이 평균이 0이고 표준편차를 적용하는 것입니다.

```js
# 평균 계산
mean_train = X_train.mean(dim=0)
mean_val = X_val.mean(dim=0)
mean_test = X_test.mean(dim=0)

# 표준 편차 계산
std_train = X_train.std(dim=0)
std_val = X_val.std(dim=0)
std_test = X_test.std(dim=0)

# 입력 데이터셋에 평균과 표준편차 적용
X_train = (X_train - mean_train) / std_train
X_val = (X_val - mean_val) / std_val
X_test = (X_test - mean_test) / std_test

print("정규화된 X_train: ", X_train[:5], "\n")
print("정규화된 X_val: ", X_val[:5], "\n")
print("정규화된 X_train: ", X_test[:5], "\n")
```

[링크](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_5.png)

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

훈련, 검증 및 테스트 텐서 데이터의 각 (입력, 출력) 쌍에 대한 TensorDataset을 생성할 것입니다. TensorDataset은 PyTorch 기능으로, 텐서를 감싸주는 역할을 합니다. 이 기능은 DataLoader를 생성할 때 매우 유용하며, 다음에 우리가 생성할 것입니다.

```js
train_dataset = TensorDataset(X_train, y_train)
val_dataset = TensorDataset(X_val, y_val)
test_dataset = TensorDataset(X_test, y_test)

# train_dataset이 어떻게 보이는지 표시해보기
print(train_dataset[0])
```

![Image](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_6.png)

DataLoader는 데이터세트와 샘플러를 결합하고, 지정된 데이터세트에 대한 반복 가능 합니다. 훈련 데이터가 매우 많을 경우, 단일 훈련 데이터를 하나씩 반복하는 데는 많은 시간이 걸리며, 동시에 모든 데이터를 한꺼번에 훈련하는 것은 처리 리소스 제약으로 실제적으로 불가능할 수 있습니다. DataLoader는 배치 크기를 정의하여 한 번에 여러 훈련 데이터를 배치로 훈련할 수 있도록 한 문제를 해결했습니다. 이 모델을 위해 훈련, 검증 및 테스트 데이터세트 각각에 대한 DataLoader를 생성하고, 이 모델의 경우 배치 크기를 10으로 정의하겠습니다. shuffle=True는 배치 내 데이터의 발생 순서를 섞어 훈련과 학습을 더 잘 수행하게 도와줍니다.

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

```python
# 각각의 학습(train), 검증(val) 및 테스트(test) 데이터셋에 대한 데이터로더 생성하기
train_loader = DataLoader(train_dataset, batch_size=10, shuffle=True, drop_last=True)
val_loader = DataLoader(val_dataset, batch_size=10, shuffle=False, drop_last=True)
test_loader = DataLoader(test_dataset, batch_size=len(test_dataset.tensors[0]), shuffle=False, drop_last=True)

# train_loader의 첫 번째 배치 보여주기
for X, y in train_loader:
  print("train_loader 첫 번째 배치 입력의 형태: ", X.shape)
  print("train_loader 첫 번째 배치 출력의 형태: ", y.shape)
  break
```

![image](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_7.png)

## 단계 3: 모델 정의

이제 PyTorch nn.Module에서 기능적인 모델을 정의하기 시작합니다. 각 레이어의 수와 각 레이어의 노드 수를 지정하는 구체적인 규칙은 없습니다. 일반적으로, 올바르게 작동하는 적절한 숫자를 찾기 위해 여러 번의 시행착오가 필요합니다. 한 가지 규칙은 네트워크가 빠르게 처리될 수 있을 만큼 충분히 작아야 하며, 동시에 의도한 문제를 높은 정확도로 해결할 수 있을 만큼 충분히 커야 한다는 것입니다.

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

저희 모델은 3개의 층, 3가지 활성화 함수 그리고 1개의 출력층으로 이루어져 있어요.

- 모델의 첫 번째 층인 layer1은 입력 데이터에서의 특성값을 가지고 와요. 이 값은 총 특성의 수 또는 입력 벡터의 차원(6이에요)이어야 해요. 저희 경우에는 in_features = X_train.shape[1]이에요.
- 첫 번째 층인 layer1은 64개의 노드를 가지고 있어요. layer1 다음에는 첫 번째 활성화 함수인 activation1이 있어요. 우리는 ReLU를 활성화 함수로 사용 중이에요. ReLU는 성능 면에서 인기가 있을 뿐만 아니라 사라지는 그래디언트 문제를 피하는 데도 좋아요. ReLU는 이전 층에서의 선형 출력에 비선형성을 제공하여 모델이 훈련 중에 더 잘 학습할 수 있도록 도와줘요.
- 두 번째 층인 layer2는 64개의 노드를 가지고 있어요. layer2 뒤에는 또 다른 활성화 층인 activation2가 추가되어요.
- activation2 다음에는 세 번째 층인 layer3가 오고, 여기에는 32개의 노드가 있어요. layer3 뒤에는 activation3이 추가돼요.
- 출력층은 회귀 작업을 다루므로 노드가 1개입니다.

```python
class fitbit_model(nn.Module):
  def __init__(self, input_features, output_class):
    super(fitbit_model, self).__init__()
    self.layer1 = nn.Linear(in_features=input_features, out_features=64)
    self.activation1 = nn.ReLU()
    self.layer2 = nn.Linear(in_features=64, out_features=64)
    self.activation2 = nn.ReLU()
    self.layer3 = nn.Linear(in_features=64, out_features=32)
    self.activation3 = nn.ReLU()
    self.output = nn.Linear(in_features=32, out_features=output_class)

  def forward(self, x):
    x = self.activation1(self.layer1(x))
    x = self.activation2(self.layer2(x))
    x = self.activation3(self.layer3(x))
    return self.output(x)
```

- 모든 층은 fitbit_model 클래스 생성자 내에서 정의되어 있어요.
- super(fitbit_model, self).**init**()는 fitbit_model 모델을 부트스트랩하기 위해 호출되는 부모 클래스 nn.Module 생성자에요.
- forward 함수는 입력 텐서를 받아 모델의 출력 텐서를 반환해요.

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

다음으로, fitbit_model 생성자에 입력 특성 값과 출력 클래스 값을 제공하여 모델을 초기화합니다. 초기에는 가중치(weights)와 편향(biases) 매개변수가 모델 자체에 의해 자동으로 초기화되어 있습니다 (일부 랜덤 값으로 할당됨). 이러한 매개변수 값은 훈련 중에 이후에 업데이트될 것입니다.

```js
input_features = X_train.shape[1]
output_class = 1
model = fitbit_model(input_features, output_class)

print("모델 구조\n")
print(model)

print("\n레이어1의 초기 가중치 값")
print(model.layer1.weight[:2])
print("레이어1의 가중치 형태: ", model.layer1.weight.shape)

print("\n레이어1의 초기 편향 값")
print(model.layer1.bias[:2])
print("레이어1의 편향 형태: ", model.layer1.bias.shape)
```

![Image](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_8.png)

## 단계 4: 모델 훈련 및 평가

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

먼저 손실 함수(loss function), 옵티마이저(optimizer), 총 에폭 수 및 기타 변수들을 정의하여 준비를 시작할 거에요.

```js
# Loss function과 optimizer
loss_fn = nn.HuberLoss(delta=0.5)
optimizer = torch.optim.Adam(params=model.parameters(), lr=0.001)

# 메트릭 수집
train_losses = []
val_losses = []

# 총 에폭 수
n_epochs = 100
```

- loss_fn = nn.HuberLoss(delta=0.5): Huber Loss는 로버스트 회귀에 사용되는 손실 함수에요. 이는 MSE와 MAE의 조합이에요. delta는 MAE와 MSE에 대한 범위를 정의하는 하이퍼파라미터로, 작업에 따라 값이 변경될 수 있어요.
- optimizer = torch.optim.Adam(params=model.parameters(), lr=0.001): Adam은 현재 상태를 유지하고 계산된 그래디언트에 기반하여 매개변수를 업데이트하는 가장 일반적으로 사용되는 최적화 알고리즘 중 하나에요. 이는 (model.parameters - 학습 가능한 매개변수 (W와 b))와 모델이 부드럽게 수렴하기 위해 모델이 얼마나 빨리 또는 느리게 학습해야 하는지를 결정하는 학습률을 가져와요. 따라서 학습률 값은 신중히 선택돼야 해요.
- n_epochs = 100: 전체 훈련 및 검증 주기가 100번 실행될 거에요.

마지막으로, 모델을 100 에폭 동안 훈련하고 평가해 봅시다.

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

# 모델 훈련 및 평가하기

for epoch in range(n_epochs): # 모델 훈련
model.train()
train_loss = 0.0
for x_batch, y_batch in train_loader: # 순전파
y_pred = model(x_batch)
y_pred = torch.squeeze(y_pred)
y_batch = y_batch.float()
loss = loss_fn(y_pred, y_batch)

        # 역전파
        optimizer.zero_grad()
        loss.backward()

        # 가중치 업데이트
        optimizer.step()
        train_loss += loss.item()
    train_losses.append(train_loss / len(train_loader))

    # 모델 평가
    model.eval()
    val_loss = 0.0
    with torch.inference_mode():
        for x_batch, y_batch in val_loader:
            y_pred = model(x_batch)
            y_pred = torch.squeeze(y_pred)
            y_batch = y_batch.float()
            loss = loss_fn(y_pred, y_batch)

            val_loss += loss.item()
        val_losses.append(val_loss/len(val_loader))

    print(f'Epoch [{epoch+1}/{n_epochs}]: Train Loss: {train_losses[-1]:.2f}, Val Loss: {val_losses[-1]:.2f}')

- model.train(): 모델을 훈련 모드로 설정합니다. 훈련 모드에서 PyTorch는 일반적으로 훈련 중에만 사용되는 드롭아웃(dropout) 및 배치 정규화(batch normalization)와 같은 기능을 활성화합니다.
- 훈련 중에는 훈련 데이터가 train_loader에서 배치 단위로 가져와 모델로 추론됩니다.
- loss = loss_fn(y_pred, y_batch): 예측된 출력 값과 데이터셋의 실제 값 사이의 차이인 손실을 계산합니다.
- optimizer.zero_grad(): 역전파를 시작하기 전에 기울기를 0으로 설정합니다. PyTorch는 후속 역방향 패스에서 기울기를 누적하기 때문에 초기화해야 합니다.
- loss.backward(): 이 단계에서 손실에 대한 모든 학습 가능한 매개변수의 기울기를 계산합니다.
- optimizer.step(): 기울기가 계산된 후 optimzer.step() 메서드로 모든 매개변수를 업데이트합니다.
- train_losses.append(train_loss / len(train_loader)): 각 배치 후에 훈련 손실을 계산하고 누적합니다. 나중에 그래프 플롯 분석에 사용됩니다.
- model.eval(): 모델을 평가 모드로 설정하고, 평가 중에는 드롭아웃과 같은 작업이 비활성화되어 추론 및 테스트에만 집중할 수 있습니다.
- with torch.inference_mode(): 이 모드로 설정하면 autograd가 기울기를 추적하지 않으므로 추론 중에는 기울기 계산이 필요하지 않습니다. 기울기 계산은 훈련 중에만 필요합니다.
- val_losses.append(val_loss/len(val_loader)): 각 배치 후에 검증 손실을 계산하고 val_losses 배열에 누적됩니다. 나중에 그래프 플롯 분석에 사용됩니다

위의 훈련 및 평가 코드를 실행한 후, 에폭 수가 증가함에 따라 손실 값이 감소하는 것을 확인할 수 있습니다.

![How to build Neural Network with real-world dataset using PyTorch](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_9.png)

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

**5단계: 훈련 및 평가 결과 분석**

우리는 `matplotlib.pyplot` 라이브러리 함수를 사용하여 결과를 그래프로 그리고 이에 따라 분석합니다.

```python
plt.figure(figsize=(12,6))
plt.plot(train_losses, label="Train Loss")
plt.plot(val_losses, label="Validation Loss")
plt.ylabel("Epoch 당 손실")
plt.xlabel("에폭")
plt.legend()
plt.show()
```

![Plot Image](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_10.png)

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

- 20번째 이폭 이후에는 훈련 손실과 검증 손실이 크게 감소했습니다. 이 결과는 상당히 인상적이에요. 더 나은 결과를 얻으려면 훈련 데이터와 이폭 수를 늘려보세요.

## 단계 6: 새 데이터에 대한 예측하기

마침내, 모델이 성공적으로 구축되고 훈련되었으므로 이제 새 데이터셋에서 모델이 얼마나 잘 수행될지 궁금하실 거예요. 이 글의 주요 목표 중 하나죠. 한 번 해보겠습니다.

이전에 준비한 test_dataset이라는 새 데이터셋을 사용할 거에요. 이전까지 사용하지 않았던 데이터셋이죠. 이제 이를 사용하여 예측해 봅시다.

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

# 테스트 데이터셋 예측하기

model.eval()
pred_val = []
target_val = []

with torch.no_grad():
for x_test, y_test in test_loader:
y_test_pred = model(x_test)
pred_val.append(y_test_pred)
target_val.append(y_test)

새로운 테스트 데이터셋에 대한 예측 결과를 분석하기 위해 matplotlib.pyplot 라이브러리 함수를 사용하여 산점도를 그려보겠습니다.

labels = torch.cat(target_val).flatten().tolist()
predictions = torch.cat(pred_val).flatten().tolist()

plt.scatter(labels, predictions)

max_lim = max(max(predictions), max(labels))
max_lim += max_lim \* 0.1

plt.xlim(0, max_lim)
plt.ylim(0, max_lim)

plt.plot([0,max_lim], [0,max_lim], "b-")

plt.xlabel("True Values")
plt.ylabel("Predicted Values")
plt.tight_layout()

![Graph](/assets/img/2024-07-09-HowtobuildNeuralNetworkwithreal-worlddatasetusingPyTorch_11.png)

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

이 산포도를 통해 우리 모델이 예측한 값과 데이터셋의 실제 값이 매우 근접해 있음을 확인할 수 있습니다. 따라서 우리가 구축하고 학습한 모델이 이 새로운 테스트 데이터셋에 대해 꽤 잘 일반화되었음을 입증합니다.

축하합니다! 여러분은 파이토치를 이용하여 Fitbit 훈련 데이터셋에서 직접 신경망 모델을 성공적으로 구축하고 학습했습니다. 만약 함께 코딩을 따라오셨다면, 어떤 사용 사례에 대해서도 모델을 편안하게 구축하고 학습시킬 수 있다고 확신합니다.

참고로: 구글 코랩 노트북 링크 입니다.

읽어 주셔서 감사합니다!
