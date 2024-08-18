---
title: "AWS Inferentia2에서 PyTorch 모델 실행하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-RunaPyTorchModelonAWSInferentia2_0.png"
date: 2024-07-06 11:35
ogImage:
  url: /assets/img/2024-07-06-RunaPyTorchModelonAWSInferentia2_0.png
tag: Tech
originalTitle: "Run a PyTorch Model on AWS Inferentia2"
link: "https://medium.com/@tannermcrae/run-a-pytorch-model-on-aws-inferentia2-d629af3754e2"
isUpdated: true
---

**/assets/img/2024-07-06-RunaPyTorchModelonAWSInferentia2_0.png**

안녕하세요! 이 블로그 포스트에서는 AWS Inferentia2에 간단한 다층 퍼셉트론(MLP) 모델을 배포하는 방법을 보여드릴 거에요. 우리는 가장 전형적인 문제 중 하나인 캘리포니아 주택 가격 예측으로 시작할 거에요. 저는 간단한 신경망을 구축하고 Amazon EC2 inf2.XLarge 인스턴스에서 추론을 수행할 거에요.

모델이 CPU에서 실행하기에 충분히 작다 하더라도, 이 블로그의 목적은 Inf2에서 실행하는 방법을 보여주는 것이에요.

**DISCLAIMER**: 저는 AWS의 GenAI 아키텍트이며, 이 글의 의견은 전적으로 제 개인의 것입니다.

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

# 배경

AWS Inferentia2 & Neuron: 이 가속화기를 사용하기 위해 AWS는 AWS Neuron이라는 소프트웨어 개발 킷(SDK)을 만들었습니다. AWS Neuron에는 TensorFlow, PyTorch 및 Apache MXNet에 네이티브로 통합된 딥 러닝 컴파일러, 런타임 및 도구가 포함되어 있습니다.

설정: 이 실험에는 Ubuntu 20 AMI에서 실행되는 inf2.XLarge 인스턴스에서 Python 3.10의 PyTorch 2.1을 사용할 것입니다.

# 간단한 신경망

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

Neuron 컴파일 및 추론이 어떻게 작동하는지 살펴보기 위해, 칼리포니아 주택 가격 예측을 위한 간단한 신경망을 만들어보겠어요. 해당 데이터세트는 Hugging Face, scikit-learn 데이터세트 또는 다른 온라인 소스를 통해 액세스할 수 있어요.

신경망 정의:

다음 신경망은 특성을 입력으로 받아 예측된 주택 가치를 출력해요.

```js
class HousingPriceModel(nn.Module):
    def __init__(self, input_size):
        super().__init__()
        self.layers = nn.Sequential(
            nn.Linear(input_size, 256),
            nn.ReLU(),
            nn.Linear(256, 128),
            nn.ReLU(),
            nn.Linear(128, 64),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(64, 1) # 단일 값 예측
        )

    def forward(self, x):
        return self.layers(x)
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

당신의 모델을 훈련시키기 위한 간단한 훈련 스크립트를 작성해봅시다. 이 모델은 매우 작은 모델이기 때문에 (~50k 파라미터), CPU를 사용하여 데이터셋에서 약 30초 동안 훈련시킬 수 있습니다.

```python
def train_model(model, train_loader, val_loader, epochs=10, batch_size=16, lr=1e-3):
    loss_fn = nn.MSELoss()
    optimizer = Adam(model.parameters(), lr=lr)

    for epoch in range(epochs):
        train_loss, val_loss = 0.0, 0.0

        # 훈련
        model.train()
        for inputs, target in train_loader:
            optimizer.zero_grad()
            outputs = model(inputs)
            loss = loss_fn(outputs, target.unsqueeze(1))
            loss.backward()
            optimizer.step()
            train_loss += loss.item()

        # 검증
        model.eval()
        with torch.no_grad():
            for inputs, target in val_loader:
                outputs = model(inputs)
                loss = loss_fn(outputs, target.unsqueeze(1))
                val_loss += loss.item()

        # 지표 계산
        rmse_train_loss = np.sqrt(train_loss / len(train_loader))
        rmse_val_loss = np.sqrt(val_loss / len(val_loader))

        # 지표 출력
        print(f"에포크 [{epoch+1}/{epochs}], RMSE: {rmse_train_loss:.4f}")
        print(f"에포크 [{epoch+1}/{epochs}], 검증 RMSE: {rmse_val_loss:.4f}")

    # 체크포인트 저장
    checkpoint = {'state_dict': model.state_dict()}
    torch.save(checkpoint, 'model.pt')
```

모델 훈련하기

이제 데이터셋을 정리하고 훈련 작업을 시작해봅시다.

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
from sklearn.model_selection import train_test_split

# 데이터셋 다운로드하고 DataFrame에 넣기
data = load_dataset("leostelon/california-housing")['train']
df = pd.DataFrame(data)

# DataFrame을 학습 및 검증 세트로 분리하기
train_df, val_df = train_test_split(df, test_size=0.2, random_state=42)

# 데이터셋 정리하기
train_dataset = clean_dataset(train_df)
val_dataset = clean_dataset(val_df)

# 데이터 로더 생성하기
train_loader = DataLoader(train_dataset, batch_size=16, shuffle=True)
val_loader = DataLoader(val_dataset, batch_size=16)

# 입력 크기 구하기
input_size = train_dataset.X.shape[1]

# 모델 가져오기
model = HousingPriceModel(input_size)

# 모델 훈련하기
train_model(model, train_loader, val_loader)
```

훈련 후, model.pt 파일에 체크포인트가 저장되었습니다. 추론을 위해 NeuronX를 사용하여 모델을 컴파일할 것입니다.

예시 저장하기

Neuron을 사용하여 모델을 컴파일할 때, 추적을 실행할 예시가 필요합니다.

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
# 단일 예시 추출
for batch in train_dataset:
    # 입력값 가져오고 현재는 타겟은 무시
    example_input, _ = batch
    # 첫 번째 예시 선택하고 배치 차원 유지
    example_input = example_input[0:1]
    # 첫 번째 배치만 필요하므로 루프 종료
    break

# 예시 저장
torch.save(example_input, 'example_input.pt')
```

# Neuron SDK 및 종속성 설치

먼저 Neuron의 모든 종속성을 설치합니다. 기존의 딥 러닝 AMI(DLAMI)를 사용할 수 있습니다. 저는 그냥 일반적인 Ubuntu AMI를 사용하고 종속성을 직접 설치하기로 했습니다.

인스턴스에서 다음 명령을 실행하세요.

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

# Neuron 저장소 업데이트를 위해 Linux 구성하기

. /etc/os-release
sudo tee /etc/apt/sources.list.d/neuron.list > /dev/null <<EOF
deb https://apt.repos.neuron.amazonaws.com ${VERSION_CODENAME} main
EOF
wget -qO - https://apt.repos.neuron.amazonaws.com/GPG-PUB-KEY-AMAZON-AWS-NEURON.PUB | sudo apt-key add -

# OS 패키지 업데이트

sudo apt-get update -y

# OS 헤더 설치

sudo apt-get install linux-headers-$(uname -r) -y

# git 설치

sudo apt-get install git -y

# Neuron 드라이버 설치

sudo apt-get install aws-neuronx-dkms=2.\* -y

# Neuron 런타임 설치

sudo apt-get install aws-neuronx-collectives=2._ -y
sudo apt-get install aws-neuronx-runtime-lib=2._ -y

# Neuron 도구 설치

sudo apt-get install aws-neuronx-tools=2.\* -y

# PATH 추가

export PATH=/opt/aws/neuron/bin:$PATH

이제 torch-neuronx 패키지를 설치해 보겠습니다.

# Neuron 저장소를 가리키도록 pip 저장소 설정

python -m pip config set global.extra-index-url https://pip.repos.neuron.amazonaws.com

# Neuron 컴파일러 및 프레임워크 설치

python -m pip install neuronx-cc==2._ --pre torch-neuronx==2.1._ torchvision

그럼 이만!

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

# 모델 컴파일하기

PyTorch-Neuron의 trace() API를 사용하면 Inferentia2에서 실행할 PyTorch 모델을 생성할 수 있으며, TorchScript로 직렬화할 수 있습니다.

```python
import torch_neuronx

# 훈련 작업에서 모델 정의를 사용합니다.
# 만약 튜토리얼을 따라오고 있다면, 입력 크기는 11입니다.
model = HousingPriceModel(11)

# 체크포인트를 로드합니다.
checkpoint = torch.load('model.pt', map_location=torch.device('cpu'))

# 상태 사전을 추출합니다.
model_state_dict = checkpoint['state_dict']

# 모델에 상태 사전을 로드합니다.
model.load_state_dict(model_state_dict)

# 이전 단계에서 내보낸 예제를 로드합니다.
example_input = torch.load('example_input.pt')

# 모델을 컴파일합니다.
model_neuron = torch_neuronx.trace(model, example_input)

# 추론을 위한 TorchScript를 저장합니다.
filename = 'model_neuron.pt'
torch.jit.save(model_neuron, filename)
```

# 예측하기

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

import torch_xla.core.xla_model as xm

# 저장된 PyTorch 모델을 메모리에 로드합니다.

model_neuron = torch.jit.load(filename)

# XLA 장치를 가져옵니다.

device = xm.xla_device()

# 모델을 XLA 장치로 이동시킵니다.

# (기본값은 inf2 인스턴스의 NeuronCore로 설정됩니다)

model_neuron = model_neuron.to(device)

def invoke(example): # 뉴런 모델을 사용하여 예측을 수행합니다.
xla_example = example.to(device)
prediction = model_neuron(xla_example)

    # 모델의 예측을 가져와서 가장 가까운 정수로 반올림하고
    # 10,000을 곱하여 원래의 스케일로 조정합니다.
    price = round(prediction.item()) * 10000
    return {
        "house_value": price
    }

print(invoke(example_input))

## 요약

이 블로그 포스트에서는 기본적인 PyTorch 모델을 학습하고 NeuronSDK를 사용하여 Amazon EC2 inf2.XLarge 머신에서 추론을 실행했습니다. 이것은 간단한 예제이지만 이러한 대체 가속기에서 더 크고 / 더 복잡한 모델을 실행할 수 있습니다.

만약 이 글이 마음에 드셨다면 LinkedIn에서 저와 연락해보세요!
