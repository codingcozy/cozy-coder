---
title: "3D 합성곱 ResNet과 모방 학습으로 AI 게임 봇 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-BuildinganAIGameBotUsingImitationLearningand3DConvolutionResNet_0.png"
date: 2024-07-13 22:50
ogImage: 
  url: /assets/img/2024-07-13-BuildinganAIGameBotUsingImitationLearningand3DConvolutionResNet_0.png
tag: Tech
originalTitle: "Building an AI Game Bot Using Imitation Learning and 3D Convolution ResNet"
link: "https://medium.com/gopenai/building-an-ai-game-bot-using-imitation-learning-and-3d-convolution-resnet-0edc6f38cd7d"
isUpdated: true
---





# 인사

안녕하세요 여러분, 이번 글은 정말 재미있을 거라고 확신합니다. AI를 사용해 게임을 플레이하는 방법에 대해 많이 배우게 될 거예요. 이 글은 게임을 플레이하는 AI 엔진을 만드는 내용이지만, 배운 내용은 프로젝트를 더 진지하게 다룰 때에도 활용할 수 있어요. 이전 글에서 언급한 것처럼 게임을 활용해 AI를 구축해 나가는 장점은 실험을 많이 해볼 수 있고 그 결과가 심각성을 가지고 있기 때문입니다. 보통 게임에서는 에이전트를 훈련시키기 위해 강화학습을 사용합니다. 이 방법도 가능하지만, 자신만의 시뮬레이터를 구축하는 데 많은 노력이 필요합니다. 하지만 이번에는 구글의 스네이크 게임을 하는 AI를 만들어볼 거에요. 여러분들은 게임을 직접 만들 필요는 없습니다.

<div class="content-ad"></div>

넓게 말하자면, 우리가 할 일은 이런 거야. Imitation learning 기술을 사용할 거야. 에이전트가 의사 결정하는 방법을 배우는 거지. 이 말은 우리가 먼저 게임을 한 동안 플레이하고 데이터를 수집한다는 뜻이야. 그리고 나서 AI가 이 데이터를 사용해서 어떻게 게임을 플레이하고 인간의 행동에서 패턴을 찾아내는지 학습할 거야. 이게 imitation learning이라고 해. (transfer learning과 혼동하지 말도록!)

기술적인 면으로 들어가보자면, 3D 컨볼루션 ResNet 모델을 훈련할 거야. 이유는 뱀이 어느 방향으로 향하고 있는지 파악하기 위해 동작을 잡아내고 싶기 때문이야. 그래서 우리는 모델에 한 번에 4프레임의 게임을 입력할 거야. 동작을 추론하기 위해. 한 프레임만 사용하고 EfficientNet 같은 표준 컨볼루션 모델을 사용해도 되지만, 동작 정보가 없으면 그렇게 잘 작동하지 않을 거야.

그럼 더 이상 말이 필요 없겠어. 시작해보자. 먼저, 데이터를 수집하는 방법을 확인해봅시다.

P.S. 문서 맨 끝에 완전한 코드 저장소를 찾아볼 수 있어.

<div class="content-ad"></div>

# 데이터 수집

데이터를 수집하는 두 가지 방법이 있습니다. 첫 번째 방법은 Python 스크립트를 사용하여 게임 화면을 캡처하고 이미지에 키보드 명령을 레이블하는 것입니다. 두 번째 방법은 우리가 사용할 방법으로, 브라우저 탐색 및 제어를 자동화하는 Python 도구인 selenium에 의존합니다. selenium을 사용하여 화면 캡처를 이미지로 저장하는 코드는 다음과 같습니다.

우리는 스크린샷을 수집하기 위해 python 스크립트 capture.py를 만들 수 있습니다.

```js
# 필요한 모듈 가져오기
import base64
import io
import cv2
from PIL import Image
import numpy as np
import keyboard
import os
from datetime import datetime
from selenium import webdriver

from selenium.webdriver.common.by import By
# captures 폴더가 있는지 확인하고 해당 폴더에 있는 모든 파일을 삭제
isExist = os.path.exists("captures")
if isExist:
    dir = "captures"
    for f in os.listdir(dir):
        os.remove(os.path.join(dir, f))
else:
    os.mkdir("captures")

current_key = "1"
buffer = []
# 키보드 입력을 기록하는 함수 정의
def keyboardCallBack(key: keyboard.KeyboardEvent):
    global current_key
    # 키가 눌렸고 버퍼에 없는 경우 버퍼에 추가
    if key.event_type == "down" and key.name not in buffer:
        buffer.append(key.name)
    # 키가 놓였을 때 버퍼에서 제거
    if key.event_type == "up":
        buffer.remove(key.name)
    # 버퍼 정렬 및 공백으로 키 결합
    buffer.sort()
    current_key = " ".join(buffer)

# 함수를 키보드 이벤트에 연결
keyboard.hook(callback=keyboardCallBack)
# 파이어폭스를 사용하여 webdriver 인스턴스 생성
driver = webdriver.Firefox()
# Google Snake 게임 웹 사이트로 이동
driver.get("<https://www.google.com/fbx?fbx=snake_arcade>")

# 무한 루프
while True:
    # 웹페이지에서 캔버스 요소 찾기
    canvas = driver.find_element(By.CSS_SELECTOR, "canvas")
    # 캔버스에서 base64로 인코딩된 이미지 데이터 가져오기
    canvas_base64 = driver.execute_script(
        "return arguments[0].toDataURL('image/png').substring(21);", canvas
    )
    # base64 데이터를 디코드하여 PNG 이미지 가져오기
    canvas_png = base64.b64decode(canvas_base64)

    image = cv2.cvtColor(
        np.array(Image.open(io.BytesIO(canvas_png))), cv2.COLOR_BGR2RGB
    )

    # 현재 타임스탬프 및 키보드 입력으로 이미지를 캡처 폴더에 저장
    if len(buffer) != 0:
        cv2.imwrite(
            "captures/"
            + str(datetime.now()).replace("-", "_").replace(":", "_").replace(" ", "_")
            + " "
            + current_key
            + ".png",
            image,
        )
    else:
        cv2.imwrite(
            "captures/"
            + str(datetime.now()).replace("-", "_").replace(":", "_").replace(" ", "_")
            + " n"
            + ".png",
            image,
        )
```

<div class="content-ad"></div>

한 번 이 스크립트를 실행하면 게임을 시작할 수 있어요. 배경에서는 스크립트가 계속해서 게임 화면의 스크린샷을 저장하고 고유한 타임스탬프와 현재 눌린 키를 이름으로 하는 이미지를 만들 거에요. 키가 눌리지 않을 때는 n으로 표시돼요.

이렇게 이미지가 저장된 후 디렉토리가 보일 거예요.

![Image](/assets/img/2024-07-13-BuildinganAIGameBotUsingImitationLearningand3DConvolutionResNet_1.png)  

# 이미지 폴더를 CSV 파일로 변환하기

<div class="content-ad"></div>

그럼 이제 이미지들을 파일 이름과 해당 동작으로 이루어진 csv 파일로 변환할 수 있어요. 이 작업은 새 파일 process.ipynb에서 수행합니다.

- 의존성 불러오기: 이미지 파일을 읽고 처리하고 레이블을 CSV 파일에 작성하기 위해 필요한 라이브러리와 모듈을 가져와야 해요. 이들은 프로세스 시작 시 불러와집니다.
- 디렉토리 생성: 'data'라는 디렉토리를 만들어요. 여기에 레이블과 이미지 파일 이름이 포함된 CSV 파일을 저장할 거예요.
- 파일 이름 읽기: 각 이미지의 파일 이름에서 눌린 키를 추출해요. 눌린 키는 왼쪽, 오른쪽, 위쪽, 아래쪽 중 하나일 수 있어요.
- 이미지 분류: 눌린 키를 기준으로 각 이미지를 0(키가 눌리지 않은 상태), 1(왼쪽), 2(위쪽), 3(오른쪽), 4(아래쪽) 중 하나의 클래스로 분류해요. 이 정보는 'labels'라는 리스트에 이미지 파일 이름과 함께 저장돼요.
- 레이블 작성: 마지막으로 레이블을 CSV 파일에 작성해요. 이 데이터셋은 이제 이미지를 통해 키가 눌리는 방향을 인식하는 머신 러닝 모델을 훈련하는 데 사용할 수 있어요.

<div class="content-ad"></div>

이제 새 파일 snake_resnet.ipynb을 생성합니다.

# 의존성 가져오기

```python
from torch.utils.data import Dataset, DataLoader, WeightedRandomSampler
import os
from PIL import Image
import torch
from sklearn.model_selection import train_test_split
import pandas as pd
from torchvision.transforms import transforms, Compose, ToTensor, Resize, Normalize, CenterCrop, Grayscale
from torch import nn
from tqdm import tqdm
from torchinfo import summary
import numpy as np
import math
from torchvision.models.video import r3d_18, R3D_18_Weights, mc3_18, MC3_18_Weights
```

# 데이터셋 생성

<div class="content-ad"></div>

시작하기 전에, PyTorch를 사용하여 사용자 정의 데이터셋 객체를 만들어야 합니다. 이 데이터셋은 시간순으로 정렬된 네 장의 이미지 스택으로 구성됩니다. 데이터셋에서 가져온 각 항목은 마지막 프레임과 연결된 키프레스를 나타냅니다. 이 데이터셋은 실제로 마지막 네 프레임을 통해 동작을 포착하고 그것을 키프레스와 연관시킵니다.

```python
class SnakeDataSet(Dataset):
    def __init__(self, dataframe, root_dir, stack_size, transform=None):
        self.stack_size = stack_size
        self.key_frame = dataframe
        self.root_dir = root_dir
        self.transform = transform
    def __len__(self):
        return len(self.key_frame) - self.stack_size*3
    def __getitem__(self, idx):
        if torch.is_tensor(idx):
            idx = idx.tolist()
            try:
                img_names = [os.path.join(self.root_dir, self.key_frame.iloc[idx + i, 0]) for i in range(self.stack_size)]
                images = [Image.open(img_name) for img_name in img_names]
                label = torch.tensor(self.key_frame.iloc[idx + self.stack_size, 1])
                if self.transform:
                    images = [self.transform(image) for image in images]
            except:
                img_names = [os.path.join(self.root_dir, self.key_frame.iloc[0 + i, 0]) for i in range(self.stack_size)]
                images = [Image.open(img_name) for img_name in img_names]
                label = torch.tensor(self.key_frame.iloc[0 + self.stack_size, 1])
                if self.transform:
                    images = [self.transform(image) for image in images]
            return torch.stack(images, dim=1).squeeze(), label
``` 

이 코드를 자세히 살펴보겠습니다:

- 초기화(__init__ 메서드):

<div class="content-ad"></div>

### 데이터세트 개요:

- `dataframe`: 이미지 및 레이블 정보를 포함하는 데이터셋입니다.
- `root_dir`: 이미지 파일이 위치한 루트 디렉토리입니다.
- `stack_size`: 하나의 데이터 포인트로 함께 쌓을 이미지 수입니다.
- `transform`: 이미지 변환을 위한 선택적 매개변수로, 데이터 증가(Data Augmentation) 등을 포함할 수 있습니다.

### 길이 메소드 (__len__ 메소드):

- 데이터셋의 길이, 즉 총 데이터 포인트 수를 반환합니다. 길이는 `key_frame`의 길이에서 `stack_size`를 세 번 곱한 값으로 계산됩니다. 이는 데이터셋이 이미지 시퀀스를 포함하며, 각 데이터 포인트가 이미지 스택으로 구성되어 있다는 것을 나타냅니다.

### 아이템 가져오기 메소드 (__getitem__ 메소드):

<div class="content-ad"></div>

- 인덱스 idx를 가져와 해당 데이터 포인트를 반환합니다.
- 이 코드는 인덱스 idx부터 시작하는 이미지 시퀀스를 로드하려고 합니다. 지정된 stack_size와 root_dir을 기반으로 이미지 파일 경로 목록(img_names)을 구성합니다.
- 그런 다음 Python Imaging Library (PIL)를 사용하여 각 이미지를 열고 결과 이미지 객체를 목록(images)에 저장합니다.
- 데이터 포인트의 레이블은 key_frame 데이터프레임에서 추출됩니다.
- 이미지 변환 함수(transform)가 제공된 경우, 각 이미지에 변환을 적용합니다.
- torch.stack(images, dim=1)를 사용하여 새로운 차원(차원 1)을 따라 이미지를 쌓은 다음, squeeze()를 사용하여 단일 차원을 제거합니다. 이렇게 하면 쌓인 이미지를 나타내는 텐서가 생성됩니다.
- 쌓인 이미지 텐서와 레이블 텐서가 튜플로 반환됩니다.
- 참고: 잠재적인 오류를 잡는 try-except 블록이 있으며 오류가 발생하면 데이터셋에서 첫 번째 시퀀스를 사용하도록 되돌아갑니다. 이것은 오류 처리의 기본적인 형태이지만 사용 사례에 따라 오류를 더 명시적으로 처리하는 것이 더 좋습니다.

# 데이터셋 균형 맞추기 및 데이터로더 생성

데이터셋을 세밀하게 분석하면 데이터셋에 심각한 클래스 불균형이 있음을 알 수 있습니다. 이는 게임을 할 때 대부분의 경우 사용자가 어떤 키도 누르지 않는 경우가 많기 때문입니다. 따라서 대부분의 데이터셋 항목은 클래스 0에 속합니다.

![이미지](/assets/img/2024-07-13-BuildinganAIGameBotUsingImitationLearningand3DConvolutionResNet_2.png)

<div class="content-ad"></div>

PyTorch의 RandomWeightedSampler를 사용하여 문제를 해결할 수 있습니다. RandomWeightedSampler는 각 샘플의 가중치를 입력으로 취합니다. 아래의 코드를 사용하여 이러한 가중치를 계산합니다.

```python
STACK_SIZE = 4
BATCH_SIZE = 32

train, test = train_test_split(pd.read_csv("data/labels_snake.csv"), test_size=0.2, shuffle=False)
classes = ["n", "left", "up", "right", "down"]

labels_unique, counts = np.unique(train["class"], return_counts=True)
class_weights = [sum(counts)/c for c in counts]
example_weights = np.array([class_weights[l] for l in train['class']])
example_weights = np.roll(example_weights, -STACK_SIZE)
sampler = WeightedRandomSampler(example_weights, len(train))

labels_unique, counts = np.unique(test["class"], return_counts=True)
class_weights = [sum(counts)/c for c in counts]
test_example_weights = np.array([class_weights[l] for l in test['class']])
test_example_weights = np.roll(test_example_weights, -STACK_SIZE)
test_sampler = WeightedRandomSampler(test_example_weights, len(test))
```

데이터셋의 각 샘플 가중치를 계산하는 과정은 이미지 분류, 텍스트 분류, 물체 감지 등과 같은 다양한 기계 학습 작업에서 중요한 단계입니다. 각 샘플의 가중치는 각 클래스가 학습 과정에 동등하게 기여하도록 데이터셋을 균형있게 만듭니다. 데이터셋이 불균형 할 때 즉, 일부 클래스에 다른 클래스보다 많은 샘플이 있는 경우, 학습 알고리즘은 주로 과반수 클래스에 집중하고 소수 클래스를 무시할 수 있어 성능이 저하될 수 있습니다.

각 샘플의 가중치를 계산하려면 먼저 scikit-learn (sklearn) 라이브러리의 train_test_split 메서드를 사용하여 데이터셋 csv를 train과 test로 분할합니다. 그 후 고유한 레이블 및 각 레이블의 발생 빈도를 가져옵니다. 이를 통해 데이터셋 내 클래스의 분포를 이해할 수 있습니다. 그런 다음 모든 카운트(데이터셋의 크기)의 합을 해당 클래스의 발생 빈도로 나누어 각 클래스의 가중치를 계산합니다. 이를 통해 데이터셋에서 각 클래스의 중요성을 파악할 수 있습니다.

<div class="content-ad"></div>

다음 단계는 각 예제의 가중치를 지정하여 예제에 클래스 가중치를 할당하는 것입니다. 이것은 데이터셋을 반복하고 각 샘플에 클래스 레이블에 기반한 가중치를 할당하여 수행됩니다. 특정 이미지와 관련된 레이블은 사실 이미지의 인덱스 + STACK_SIZE의 레이블입니다. 이렇게 함으로써 각 샘플이 해당하는 클래스 레이블에 기반하여 올바른 가중치를 부여받도록 합니다.

요약하자면, 데이터셋의 각 샘플에 대한 가중치를 계산하는 것은 기계 학습 작업에서 중요한 단계입니다. 이는 데이터셋을 균형 있게 유지하고 각 클래스가 학습 과정에 동등하게 기여하도록 돕습니다. 이 과정은 데이터셋을 학습 및 테스트 세트로 분할하고 고유한 레이블 및 각 레이블의 수를 얻은 후, 각 클래스의 가중치를 계산하고 각 샘플에 클래스 레이블에 기반한 가중치를 지정하는 것을 포함합니다.

마지막으로, 학습 및 테스트 데이터셋용 데이터로더를 생성할 수 있습니다.

```python
dataset = SnakeDataSet(root_dir="captures", dataframe=train, stack_size=STACK_SIZE, transform=transformer)
dataloader = DataLoader(dataset, batch_size=BATCH_SIZE, sampler=sampler, drop_last=True)
test_dataset = SnakeDataSet(root_dir="captures", dataframe=test, stack_size=STACK_SIZE, transform=transformer)
test_dataloader = DataLoader(test_dataset, batch_size=BATCH_SIZE, sampler=test_sampler, drop_last=True)
```

<div class="content-ad"></div>

# 변형 생성

이번에 사용할 모델에 대해 데이터에 일정한 변형을 수행해야 합니다. 먼저 데이터셋 이미지의 평균과 표준 편차를 계산해야 합니다. 다음 코드를 사용하여 이 작업을 수행할 수 있습니다.

```python
def compute_mean_std(dataloader):
    '''
    데이터로더의 이미지들이 동일한 높이와 너비를 가정합니다.
    출처: <https://github.com/aladdinpersson/Machine-Learning-Collection/blob/master/ML/Pytorch/Basics/pytorch_std_mean.py>
    '''
    # var[X] = E[X**2] - E[X]**2
    channels_sum, channels_sqrd_sum, num_batches = 0, 0, 0
    for batch_images, labels in tqdm(dataloader):  # (B,H,W,C)
            batch_images = batch_images.permute(0,3,4,2,1)
            channels_sum += torch.mean(batch_images, dim=[0, 1, 2, 3])
            channels_sqrd_sum += torch.mean(batch_images ** 2, dim=[0, 1, 2,3])
            num_batches += 1
        mean = channels_sum / num_batches
        std = (channels_sqrd_sum / num_batches - mean ** 2) ** 0.5
        return mean, std

compute_mean_std(dataloader)
```

이렇게 하면 평균과 표준 편차를 반환하며, 아래 변환에 적용할 수 있습니다.

<div class="content-ad"></div>


transformer = Compose([
    Resize((84,84), antialias=True),
    CenterCrop(84),
    ToTensor(),
    Normalize(mean =[ -0.7138, -2.9883,  1.5832], std =[0.2253, 0.2192, 0.2149]) 
])


This spell works like magic! It transforms the image into a perfect 84x84 size, then enhances its powers with center cropping, turning it into a powerful tensor, and finally balancing its energies through normalization.

# Unveiling the Mystery of the Model

Just as the tarot cards reveal secrets, we shall unravel the mysteries of the model we are about to create. Prepare yourselves as we delve into the realm of PyTorch and unveil the enigmatic r3d model - a mystical convolution spell infused with the arcane knowledge of ResNet architecture.


<div class="content-ad"></div>


model = r3d_18(weights = R3D_18_Weights.DEFAULT)
model.fc = nn.Linear(in_features=512, out_features=5, bias=True)
summary(model, (32,3,4,84,84))


우리는 기본 가중치를 로드하고 마지막 완전 연결 레이어를 교체하여 우리 애플리케이션에서 요구되는 5가지 출력 클래스를 예측합니다. 네트워크는 이렇게 구성되어 있습니다. 약 3300만 개의 학습 가능한 매개변수가 있습니다.

![Network](/assets/img/2024-07-13-BuildinganAIGameBotUsingImitationLearningand3DConvolutionResNet_3.png)

# 모델 훈련하기


<div class="content-ad"></div>

이제 모델을 마침내 훈련할 수 있습니다. 먼저 옵티마이저와 손실 기준을 만듭니다. 여기서는 학습률을 10e-5, 가중치 감소를 0.1로 사용합니다.

```python
num_epochs = 2
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
optimizer = torch.optim.AdamW(model.parameters(), lr=10e-5, weight_decay=0.1)
model.to(device)
criterion = nn.CrossEntropyLoss()
```

이제 훈련 루프를 만듭니다.

```python
for epoch in range(num_epochs):
    total_loss = 0.0
    correct_predictions = 0
    total_samples = 0

    val_loss = 0.0
    val_correct_predictions = 0
    val_total_samples = 0

    # 모델을 훈련 모드로 설정합니다
    model.train()

    # 작업 진행 상황을 시각화하기 위한 tqdm 막대
    pbar = tqdm(dataloader, desc=f'Epoch {epoch + 1}/{num_epochs}', leave=True)
    for inputs, labels in pbar:
        inputs, labels = inputs.to(device), labels.to(device)

        outputs = model(inputs.to(device))
        loss = criterion(outputs, labels)

        # 역전파 및 최적화
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        # 통계 업데이트
        total_loss += loss.item()
        _, predicted = torch.max(torch.softmax(outputs, 1), 1)
        correct_predictions += (predicted == labels).sum().item()
        total_samples += labels.size(0)

        # 현재 손실 및 정확도로 tqdm 막대 업데이트
        pbar.set_postfix({'Loss': total_loss / total_samples, 'Accuracy': correct_predictions / total_samples})

    model.eval()
    with torch.no_grad():
        for inputs, labels in test_dataloader:
            inputs, labels = inputs.to(device), labels.to(device)

            outputs = model(inputs.to(device))
            loss = criterion(outputs, labels)

            # 통계 업데이트
            val_loss += loss.item()
            _, predicted = torch.max(torch.softmax(outputs, 1), 1)
            val_correct_predictions += (predicted == labels).sum().item()
            val_total_samples += labels.size(0)

    # 검증을 위한 에폭별 정확도와 손실 계산 및 출력
    epoch_loss = val_loss / val_total_samples
    epoch_accuracy = val_correct_predictions / val_total_samples
    print(f'에폭 {epoch + 1}/{num_epochs}, 검증 손실: {epoch_loss:.4f}, 검증 정확도: {epoch_accuracy:.4f}')
    torch.save(model.state_dict(), "model_r3d.pth")
```

<div class="content-ad"></div>

# 게임 플레이

새로운 Python 스크립트 play.py를 만들어보세요. 이 스크립트에서는 모델을 불러와 [capture.py](`http://capture.py`)와 유사한 코드를 작성하여 게임을 불러오고 스크린샷을 수집할 겁니다. 4개의 스크린샷을 쌓아서 네트워크를 통과시킬 겁니다.

```js
import base64
import torch
import cv2
import keyboard
from PIL import Image
import numpy as np
from torchvision.transforms import Compose, Resize, CenterCrop, ToTensor, Normalize, Grayscale
from torch import nn
from collections import deque
from torchvision.models.video import r3d_18
from selenium import webdriver
from selenium.webdriver.common.by import By

label_keys = {
    0: "",
    1: "left",
    2: "up",
    3: "right",
    4: "down"
}
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = r3d_18(weights=None)
model.fc = nn.Linear(in_features=512, out_features=5, bias=True)
model.load_state_dict(torch.load("model_mc3.pth"))
model.to(device)
model.eval()
transformer = Compose([
    Resize((84, 84), antialias=True),
    CenterCrop(84),
    ToTensor(),
    Normalize(mean=[-0.7138, -2.9883, 1.5832], std=[0.2253, 0.2192, 0.2149])
])
# 파이어폭스를 사용하여 웹드라이버 인스턴스를 만들기
driver = webdriver.Firefox()
# Google 뱀 게임 웹사이트로 이동
driver.get("<https://www.google.com/fbx?fbx=snake_arcade>")
frame_stack = deque(maxlen=4)
while True:
    # 웹페이지에서 캔버스 요소 찾기
    canvas = driver.find_element(By.CSS_SELECTOR, "canvas")
    # 캔버스로부터 base64로 인코딩된 이미지 데이터 가져오기
    canvas_base64 = driver.execute_script(
        "return arguments[0].toDataURL('image/png').substring(21);", canvas
    )
    # base64 데이터를 디코딩하여 PNG 이미지 얻기
    canvas_png = base64.b64decode(canvas_base64)

    image = cv2.cvtColor(
        np.array(Image.open(io.BytesIO(canvas_png))), cv2.COLOR_BGR2RGB
    ) 
    frame_stack.append(transformer(image))
    input = torch.stack([*frame_stack], dim=1).to(device).squeeze().unsqueeze(0)
    if len(frame_stack) == 4:
        with torch.inference_mode():
            outputs = model(input).to(device)
            preds = torch.softmax(outputs, dim=1).argmax(dim=1)
            if preds.item() != 0:
                keyboard.press_and_release(label_keys[preds.item()])
```

이 스크립트를 실행하면 뱀이 플레이되는 것을 볼 수 있습니다.

<div class="content-ad"></div>

![Snake Game AI](https://miro.medium.com/v2/resize:fit:1200/1*rtqi2zJcne0xqJQyd9YSGw.gif)

# 결론

Google Snake를 위한 AI 게임 봇을 만드는 여정을 마무리하며, 이미테이션 러닝의 흥미로운 영역을 탐험하고 3D Convolution ResNet 모델의 파워를 이용했습니다. 게임 개발의 재미를 넘어서, 이 습득한 기술은 심각한 AI 상황에서도 폭넓게 적용될 수 있습니다.

우리는 먼저 이미테이션 러닝의 중요성을 이해하고, 인간의 게임 플레이를 통해 AI 에이전트를 훈련시키는 데 초점을 맞췄습니다. 3D Convolution ResNet 모델의 사용으로 우리는 게임 네 개의 프레임을 쌓아 움직임을 정확하게 캡처할 수 있었습니다.

<div class="content-ad"></div>

실용적인 세부 사항이 포함된 데이터 수집은 Selenium을 사용하여 레이블링된 데이터 세트를 생성하고 PyTorch 친화적 형식으로 변환하는 작업을 다뤘습니다. WeightedRandomSampler를 통해 클래스 불균형을 해결하는 중요성을 강조했습니다.

구현은 사용자 정의 데이터 집합 클래스 작성, 적절한 모델 아키텍처 선택 및 필수 변환을 사용하여 모델 훈련에 관여했습니다. 훈련 루프와 테스트 세트에서 평가를 통해 모델의 성능에 대한 통찰을 제공했습니다.

이 여정을 완성하기 위해 훈련된 모델이 실시간으로 게임을 플레이하는 방법을 소개했습니다. 제공된 Python 스크립트는 프레임 캡처, 예측 생성 및 뱀의 움직임 제어를 보여주었습니다.

간결한 이 가이드를 통해 당신은 모방 학습과 3D 합성 ResNet 모델을 통해 게임에서 인공지능을 습득하며, 이는 보다 넓은 인공지능 응용 분야로 확장될 수 있는 기술을 제공합니다.

<div class="content-ad"></div>

GitHub Repository: akshayballal95/autodrive-snake를 블로그(github.com)에서 확인해보세요.


Want to Connect?

- [Git Repository](https://github.com/akshayballal95/autodrive-snake)
- [My website](https://yourwebsite.com)
- [LinkedIn](https://www.linkedin.com/in/yourprofile)
- [Twitter](https://twitter.com/yourhandle)
