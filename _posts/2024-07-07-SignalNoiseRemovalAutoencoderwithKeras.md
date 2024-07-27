---
title: "Keras를 사용한 신호 노이즈 제거 오토인코더 만들기 방법"
description: ""
coverImage: "/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_0.png"
date: 2024-07-07 22:49
ogImage: 
  url: /assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_0.png
tag: Tech
originalTitle: "Signal Noise Removal Autoencoder with Keras"
link: "https://medium.com/codex/signal-noise-removal-autoencoder-with-keras-3dda589f8b24"
---


![Signal Noise Removal Autoencoder](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_0.png)

에
이론상으로만 순수 신호가 존재한다는 것은 사실이지만, 실제로는 신호 처리 관련 활동을 할 때 노이즈에 직면할 수 있습니다. 측정(또는 수신) 장치에 의해 또는 측정을 수행하는 매짤로 인해 노이즈가 발생하는 것이 일반적이죠. 이 노이즈를 제거하고 싶을 겁니다.

신호에서 노이즈를 필터링하기 위한 다양한 수학적 기교가 존재합니다. 예를 들어 노이즈가 신호 범위 전체에서 상대적으로 일정할 때, 모든 신호의 평균을 내고 각각의 신호에서 빼면 대체로 노이즈에 영향을 미치는 요소를 제거할 수 있을 것입니다.

그러나 이러한 기교들은 노이즈에 대해 앞서 몇 가지를 알고 있어야만 작동합니다. 많은 경우 노이즈의 정확한 형태가 알려지지 않거나 상대적으로 숨겨져 있어서 추정할 수 없는 경우도 있습니다. 이런 경우에 해결책은 예제 데이터에서 노이즈를 학습하는 것에 있을 수 있습니다.

<div class="content-ad"></div>

안녕하세요! 오늘은 오늘의 블로그에서 이야기할 주제를 시작하겠습니다.

Autoencoders는 이러한 목적으로 사용될 수 있습니다. 입력으로 노이즈가 있는 데이터를 주고 깨끗한 데이터를 출력으로 주면, 훈련 데이터의 독특한 노이즈를 인식하도록 만들 수 있습니다. 이렇게 함으로써, autoencoders는 노이즈 제거기로 사용할 수 있습니다.

그렇다면 autoencoders가 정확히 무엇이며, 그들의 작동 방식은 왜 노이즈 제거에 적합한 것일까요? 신호 노이즈 제거를 위해 하나를 구현하는 방법은 무엇일까요?

이러한 질문에 대한 답변을 오늘의 블로그에서 제공해 드리겠습니다. 먼저, autoencoders에 대한 요약을 제공하여 그들이 무엇이며 어떻게 작동하는지 이론적 이해를 되찾아보겠습니다. 이에는 노이즈 제거에 적용할 수 있는 이유에 대한 토론도 포함됩니다. 그런 다음, 이를 증명하기 위해 autoencoder를 구현하는 세 단계 과정을 통해 이를 시연하겠습니다.

<div class="content-ad"></div>

- x² 샘플의 대규모 데이터셋을 생성합니다.
- x² 샘플에 가우시안(랜덤) 노이즈가 추가된 대규모 데이터셋을 생성합니다.
- 노이즈를 제거하는 오토인코더를 만들어 노이즈가 추가된 x² 입력을 원본 사인으로 변환하도록 학습시킵니다 — 새로운 데이터에 대해서도 마찬가지!

자, 한번 살펴봅시다!

## 요약: 오토인코더란 무엇인가요?

오토인코더를 구성하기로 했다면, 그것이 무엇인지를 알아야 합니다. 오토인코더의 흐름을 다음과 같이 시각화할 수 있습니다:

<div class="content-ad"></div>

Autoencoders are composed of two parts: an encoder, which encodes some input into an encoded state, and a decoder which can decode the encoded state into another format. This can be a reconstruction of the original input, as we can see in the plot below, but it can also be something different.

![Autoencoder](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_3.png)

For example, autoencoders are learned for noise removal, but also for dimensionality reduction (Keras Blog, n.d.; we then use them to convert the input data into low-dimensional format, which might benefit training lower-dimensionality model types such as SVMs).

<div class="content-ad"></div>

블록 위의 빨간 부분에 대한 참고 - 즉, 인코더와 디코더는 데이터를 기반으로 학습된다(Keras Blog, n.d.). 이것은 좀 더 추상적인 수학적 함수들과는 달리, 그들이 하나의 도메인(예: x² 그림에서의 신호 잡음 제거)에서 매우 전문화되어 있지만 다른 도메인(예: 이미지 잡음 제거를 위해 동일한 오토인코더를 사용할 경우)에서는 성능이 매우 나쁘다는 것을 의미합니다.

## 자동인코더가 잡음 제거에 적용되는 이유

자동인코더는 인코더를 사용하여 인코딩된 상태를 학습하고, 디코더를 사용하여 이 상태를 다른 것으로 디코딩하는 것을 배웁니다.

이것을 신호 잡음의 맥락에서 생각해보세요: 신경망에 잡음이 섞인 데이터를 특징으로 제공하면서 순수 데이터를 대상으로 사용할 수 있다고 가정해봅시다. 앞에서 그려진 것을 따르면, 신경망은 잡음이 있는 이미지를 기준으로 인코딩된 상태를 학습하고, 최대한 순수 데이터와 일치하도록 해독을 시도할 것입니다. 순수 데이터와 잡음 데이터 사이에 서 있는 것은 무엇인가요? 물론 잡음입니다. 실제로, 자동인코더는 따라서 입력 이미지에서 잡음을 인식하고 제거하는 것을 학습하게 될 것입니다.

<div class="content-ad"></div>

자, 이제 우리가 Keras로 이러한 오토인코더를 만들 수 있는지 확인해봅시다.

## 오늘의 예시: 소음 제거를 위한 Keras 기반 오토인코더

다음 부분에서는 Keras 딥 러닝 프레임워크를 사용하여 노이즈 제거나 신호 제거 오토인코더를 만드는 방법을 보여 드리겠습니다. 여기서는 먼저 사용하는 데이터와 모델의 고수준 설명을 살펴볼 것입니다.

## 데이터

<div class="content-ad"></div>

먼저, 데이터에 대해 알아봅시다. 순수한 신호(그리고 따라서 오토인코더의 목표)로 작은 도메인에서 x² 샘플들을 사용합니다. 시각화된 결과, 샘플은 이렇게 보입니다:

![Signal with noise](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_4.png)

오늘의 모델에서는 100,000개의 샘플을 사용합니다. 각 샘플에 가우시안 노이즈 — 즉 랜덤 노이즈를 추가합니다. 전체적인 형태는 유지되지만, 플롯이 소음이 더해진 것은 명백합니다:

![Signal with noise](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_5.png)

<div class="content-ad"></div>

## The model

Now, let's take a look at the model.

![Model](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_6.png)

Here are the layers it consists of:

<div class="content-ad"></div>

- 입력 데이터를 가져오는 입력 레이어
- 인코더로 작용하는 두 개의 Conv1D 레이어
- 디코더로 작용하는 두 개의 Conv1D 전치 레이어
- 출력 레이어로 작용하는 하나의 출력을 가진 Conv1D 레이어(Sigmoid 활성화 함수 및 패딩 포함).

자세한 내용을 제공하면, 이 모델 요약입니다:


Model: "sequential"
_________________________________________________________________
Layer (type)                 Output Shape              Param #
=================================================================
conv1d (Conv1D)              (None, 148, 128)          512
_________________________________________________________________
conv1d_1 (Conv1D)            (None, 146, 32)           12320
_________________________________________________________________
conv1d_transpose (Conv1DTran (None, 148, 32)           3104
_________________________________________________________________
conv1d_transpose_1 (Conv1DTr (None, 150, 128)          12416
_________________________________________________________________
conv1d_2 (Conv1D)            (None, 150, 1)            385
=================================================================
Total params: 28,737
Trainable params: 28,737
Non-trainable params: 0


이제 첫 번째 단계인 순수한 waveform 생성부터 시작하겠습니다. 파일 관리자를 열고 파일을 생성하려는 폴더로 이동해주세요(예: keras-autoencoders). 그리고 signal_generator.py 라는 파일을 만들어주세요. 그 다음, 코드 에디터에서 이 파일을 열어 코딩 프로세스를 시작하세요!

<div class="content-ad"></div>

## 순수한 파형 생성

![waveform](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_7.png)

순수한 파형 생성은 위에 표시된 것과 같은 시각화를 만들기 위해 다음 단계로 구성되어 있습니다:

- Python 스크립트의 시작 부분에 필요한 import 추가;
- 신호 생성기의 구성 설정 정의;
- 데이터, 즉 순수 파형 생성;
- 생성된 파형 저장 및 일부 시각화.

<div class="content-ad"></div>

## Imports 추가하기

먼저, imports를 추가하세요. 단순한 리스트입니다:

```js
import matplotlib.pyplot as plt
import numpy as np
```

우리는 데이터 생성 및 처리에 Numpy를 사용하며, Matplotlib를 사용하여 샘플 중 일부를 시각화합니다.

<div class="content-ad"></div>

## 생성기 설정 구성하기

생성기 구성은 세 가지 단계로 이루어져 있어요: 샘플 전체 설정, 샘플 내 설정 및 기타 설정입니다. 먼저, 샘플 전체 설정부터 시작할게요. 이것은 생성할 샘플의 개수에 해당해요:

```js
# 샘플 설정
num_samples = 100000
```

다음은 샘플 내 설정이 따라옵니다:

<div class="content-ad"></div>


## 샘플 내 구성

num_elements는 도메인의 너비를 나타냅니다. interval_per_element는 샘플을 생성할 때 이터레이터가 이동할 단계 크기를 나타냅니다. 이 경우 도메인 (0, 1)은 따라서 100개의 샘플을 포함하게 될 것입니다 (즉, 1/interval per element = 1/0.01 = 100). 이것이 total_num_elements에 표시된 내용입니다.

시작 지점은 생성 프로세스를 시작할 위치를 결정합니다.

마지막으로, 원하는 샘플 수를 다른 구성 설정에서 시각화할 수 있습니다.


<div class="content-ad"></div>


# 다른 구성
시각화할 샘플 수 = 1


## 데이터 생성

다음 단계는 데이터를 생성하는 것입니다!

먼저 데이터와 하위 샘플 데이터를 포함하는 리스트를 지정해야 합니다 (샘플당 xs 및 ys를 여러 개 포함하는 샘플 리스트가 있습니다; totalnumelements = 100일 때, 각각 100개씩 있을 것입니다):


<div class="content-ad"></div>

```python
# 샘플 및 하위 샘플을 위한 컨테이너
samples = []
xs = []
ys = []
```

이제 실제 데이터 생성 부분입니다.

```python
# 샘플 생성
for j in range(0, num_samples):
  # 진행 상황 보고
  if j % 100 == 0:
    print(j)
  # 파형 생성
  for i in range(starting_point, total_num_elements):
    x_val = i * interval_per_element
    y_val = x_val * x_val
    xs.append(x_val)
    ys.append(y_val)
  # 샘플에 파형 추가
  samples.append((xs, ys))
  # 다음 샘플을 위해 하위 샘플 컨테이너 지우기
  xs = []
  ys = []
```

먼저 num_samples 변수를 범위로 하는 각 샘플을 반복합니다. 100개의 샘플마다 진행 상황 보고가 포함되어 있습니다.


<div class="content-ad"></div>

다음, 우리는 파형을 닠르며 함수 출력을 xs와 ys 변수에 추가하는 단계별 작업을 합니다.

이후에, 우리는 전체 파형을 샘플 리스트에 추가하고, 다음 샘플을 생성하기 위해 서브샘플 컨테이너를 초기화합니다.

## 저장과 시각화

다음 단계는 데이터를 저장하는 것입니다. 우리는 Numpy의 save 호출을 사용하여 samples를 ./signal_waves_medium.py라는 파일에 저장합니다.

<div class="content-ad"></div>

```python
# 입력 형태 확인
print(np.shape(np.array(samples[0][0])))
  
# 데이터를 파일로 저장하여 재사용
np.save('./signal_waves_medium.npy', samples)

# 몇 개의 랜덤 샘플 시각화
for i in range(0, num_samples_visualize):
  random_index = np.random.randint(0, len(samples)-1)
  x_axis, y_axis = samples[random_index]
  plt.plot(x_axis, y_axis)
  plt.title(f'샘플 시각화 {random_index} ---- y: f(x) = x^2')
  plt.show()
```

다음으로, Matplotlib 코드를 사용하여 samples 배열에서 num_samples_visualize 개의 랜덤 샘플을 시각화합니다. 이게 전부에요!

Numpy와 Matplotlib이 설치되어 있는지 확인한 후 python signal_generator.py로 코드를 실행하면 생성 과정이 시작되고, .npy 파일이 생성되며 프로세스가 완료되면 한 개 이상의 시각화가 표시될 것입니다.

## 전체 생성기 코드


<div class="content-ad"></div>

만약 한 번에 전체 신호 생성기를 얻고 싶다면, 여기 있어요:

```js
import matplotlib.pyplot as plt
import numpy as np

# 샘플 구성
num_samples = 100000

# 인트라샘플 구성
num_elements = 1
interval_per_element = 0.01
total_num_elements = int(num_elements / interval_per_element)
starting_point = int(0 - 0.5*total_num_elements)

# 기타 설정
num_samples_visualize = 1

# 샘플 및 서브샘플을 담을 컨테이너
samples = []
xs = []
ys = []

# 샘플 생성
for j in range(0, num_samples):
  # 진행 상황 보고
  if j % 100 == 0:
    print(j)
  # 파형 생성
  for i in range(starting_point, total_num_elements):
    x_val = i * interval_per_element
    y_val = x_val * x_val
    xs.append(x_val)
    ys.append(y_val)
  # 샘플에 파형 추가
  samples.append((xs, ys))
  # 다음 샘플을 위해 서브샘플 컨테이너 비우기
  xs = []
  ys = []

# 입력 형태
print(np.shape(np.array(samples[0][0])))
  
# 재사용하기 위해 데이터 저장
np.save('./signal_waves_medium.npy', samples)

# 몇 가지 무작위 샘플 시각화
for i in range(0, num_samples_visualize):
  random_index = np.random.randint(0, len(samples)-1)
  x_axis, y_axis = samples[random_index]
  plt.plot(x_axis, y_axis)
  plt.title(f'Visualization of sample {random_index} ---- y: f(x) = x^2')
  plt.show()
```

## 순수 파형에 잡음 추가하기

두 번째 부분: 이전 단계에서 생성한 100k 순수 파형에 잡음을 추가합니다.

<div class="content-ad"></div>

![Signal Noise Removal](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_8.png)

이것은 다음과 같은 개별 단계로 구성되어 있습니다:

- 다시 한번 import 추가;
- 노이즈 처리를 위한 구성 변수 설정;
- 데이터 로드;
- 노이즈 추가;
- 노이지가 추가된 샘플 저장 및 일부 시각화.

추가 파일을 생성하세요, 예를 들어 signal_apply_noise.py, 그리고 다음을 추가하겠습니다.

<div class="content-ad"></div>

## Imports 추가하기

우리의 imports는 신호 발생기에서 사용한 것과 동일합니다:

```js
import matplotlib.pyplot as plt
import numpy as np
```

## 노이즈 처리 과정 구성하기

<div class="content-ad"></div>

우리의 노이징 구성은 매우 간단합니다:

```js
# 샘플 구성
num_samples_visualize = 1
noise_factor = 0.05
```

num_samples_visualize는 노이징 프로세스가 끝난 후 시각화하려는 샘플의 수이며, noise_factor는 샘플에 추가할 노이즈의 양입니다 (0 = 노이즈 없음; 1 = 완전한 노이즈).

## 데이터 로딩

<div class="content-ad"></div>

다음으로, 데이터를 불러와서 샘플을 올바른 변수에 할당해줍니다. 바로 x_val과 y_val에요.

```js
# 데이터 불러오기
data = np.load('./signal_waves_medium.npy')
x_val, y_val = data[:,0], data[:,1]
```

## 노이즈 추가

이제 샘플에 노이즈를 추가해봅시다.

<div class="content-ad"></div>

```js
# 데이터에 노이즈 추가
noisy_samples = []
for i in range(0, len(x_val)):
  if i % 100 == 0:
    print(i)
  pure = np.array(y_val[i])
  noise = np.random.normal(0, 1, pure.shape)
  signal = pure + noise_factor * noise
  noisy_samples.append([x_val[i], signal])
```

먼저, 우리는 노이즈가 추가된 샘플을 담을 새로운 리스트를 정의합니다. 그 후 각 샘플을 반복하며 (매 100개의 샘플마다 진행 상황을 보고합니다), 몇 가지 작업을 수행합니다:

- 순수한 샘플 (즉, 노이즈가 없는 x² 플롯)를 pure 변수에 할당합니다.
- 그런 다음 np.random.normal을 사용하여 순수 샘플과 동일한 모양의 가우시안 노이즈를 생성합니다.
- 다음으로 노이즈를 순수 샘플에 추가하고 noise_factor를 사용합니다.
- 마지막으로 샘플의 도메인과 노이지 샘플을 noisy_samples 배열에 추가합니다.

## 저장 및 시각화

<div class="content-ad"></div>

다음은 전에 사용한 제너레이터와 다르지 않게 데이터를 .npy 파일에 저장하고 (이번에는 다른 이름으로 😃) 몇 가지 임의 샘플을 시각화합니다.


# 데이터를 파일로 저장하여 재사용하기
np.save('./signal_waves_noisy_medium.npy', noisy_samples)

# 임의 샘플 몇 개 시각화하기
for i in range(0, num_samples_visualize):
  random_index = np.random.randint(0, len(noisy_samples)-1)
  x_axis, y_axis = noisy_samples[random_index]
  plt.plot(x_axis, y_axis)
  plt.title(f'노이즈가 추가된 샘플 시각화 {random_index} ---- y: f(x) = x^2')
  plt.show()


이제 signal_apply_noise.py를 실행하면 100k개의 노이즈가 추가된 샘플을 얻게 됩니다. 이를 통해 우리는 이어서 개발할 오토인코더를 학습시킬 수 있습니다.

## 전체 노이징 코드

<div class="content-ad"></div>

이 full code에 관심이 있다면 여기 있어요:

```js
import matplotlib.pyplot as plt
import numpy as np

# 샘플 구성
num_samples_visualize = 1
noise_factor = 0.05

# 데이터 불러오기
data = np.load('./signal_waves_medium.npy')
x_val, y_val = data[:,0], data[:,1]

# 데이터에 노이즈 추가
noisy_samples = []
for i in range(0, len(x_val)):
  if i % 100 == 0:
    print(i)
  pure = np.array(y_val[i])
  noise = np.random.normal(0, 1, pure.shape)
  signal = pure + noise_factor * noise
  noisy_samples.append([x_val[i], signal])
  
# 재사용할 수 있도록 데이터 파일로 저장
np.save('./signal_waves_noisy_medium.npy', noisy_samples)

# 몇 가지 랜덤 샘플 시각화
for i in range(0, num_samples_visualize):
  random_index = np.random.randint(0, len(noisy_samples)-1)
  x_axis, y_axis = noisy_samples[random_index]
  plt.plot(x_axis, y_axis)
  plt.title(f'Visualization of noisy sample {random_index} ---- y: f(x) = x^2')
  plt.show()
```

## 오토인코더 생성

![Image](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_9.png)

<div class="content-ad"></div>

이제 재미있는 작업을 할 시간이에요: 오토인코더를 만들어봅시다.

오토인코더를 만드는 과정은 다음과 같아요:

- 먼저, 필요한 라이브러리를 import하기
- 모델에 대한 설정 세부사항 설정하기;
- 데이터 로딩 및 준비;
- 모델 구조 정의;
- 모델 컴파일 및 훈련 시작;
- 시각적으로 테스트 세트에서 노이즈 제거된 파형 시각화하여, 작업이 잘 되고 있는지 시각적으로 확인하기.

성공적으로 실행하려면 TensorFlow 2.4+, Matplotlib 및 Numpy가 필요합니다.

<div class="content-ad"></div>

세 번째(그리고 마지막) 파일을 만들어보겠습니다: `python signal_autoencoder.py`.

### Imports 추가하기

먼저, 다음과 같이 imports를 지정해봅시다:

```python
import tensorflow.keras
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv1D, Conv1DTranspose
from tensorflow.keras.constraints import max_norm
import matplotlib.pyplot as plt
import numpy as np
import math
```

<div class="content-ad"></div>

친애하는 여러분, 안녕하세요! 오늘은 Keras에서 Sequential API를 가져와 함께 사용하는 Conv1D 및 Conv1DTranspose 레이어와 무게 업데이트를 확인하게하는 MaxNorm constraint에 대해 이야기하려 합니다. 또한 Matplotlib, Numpy, 그리고 Python 수학 라이브러리를 가져옵니다.

## 모델 설정

다음으로, 모델을 위해 일부 구성 옵션을 설정합니다:


# 모델 구성
input_shape = (150, 1)
batch_size = 150
no_epochs = 5
train_test_split = 0.3
validation_split = 0.2
verbosity = 1
max_norm_value = 2.0


<div class="content-ad"></div>

요 모델 구성에 대한 몇 가지 통찰이 있어요:

- Conv1D 입력과 일치하게 input_shape은 (150, 1)이에요.
- 배치 크기는 150이에요. 이 숫자는 손실 값과 예측 시간 사이에 좋은 균형을 제공하는 것으로 보였어요.
- 에포크 수는 상대적으로 낮지만 실용적이에요: 이 숫자 이후에는 오토인코더가 크게 개선되지 않았어요.
- 전체 데이터의 30%, 즉 30,000개 샘플을 테스트 데이터로 사용해요.
- 훈련 데이터의 20% (70,000개)가 검증 목적으로 사용될 거예요. 따라서 에포크(심지어 각 미니배치별로)마다 모델을 검증하는 데 14,000개가 사용될 거에요. 그리고 실제 훈련에는 56,000개가 사용될 거에요.
- 모든 모델 출력이 화면에 표시되며, 출력 상세도 모드가 참으로 설정되어 있어요.
- max_norm_value는 2.0이에요. 이 값은 다른 시나리오에서 잘 작동했고 훈련 결과를 약간 향상시켰어요.

## 데이터 로딩 및 준비

이제 할 일은 데이터를 로드하는 것이에요. 노이지와 순수 샘플을 각각 해당 변수에 로드해요:

<div class="content-ad"></div>

```python
# 데이터 불러오기
data_noisy = np.load('./signal_waves_noisy_medium.npy')
x_val_noisy, y_val_noisy = data_noisy[:,0], data_noisy[:,1]
data_pure = np.load('./signal_waves_medium.npy')
x_val_pure, y_val_pure = data_pure[:,0], data_pure[:,1)
```

이제 데이터를 재구성할 차례에요. 각 샘플에 대해 다음 단계를 거치게 됩니다:

![Signal Noise Removal Autoencoder](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_10.png)

- 먼저, 이진 교차 엔트로피 손실의 작동 방식 때문에 샘플을 [0, 1] 범위로 정규화합니다. 이 정규화 단계 없이는 이상한 손실 값(극단적으로 음수인 값으로 BCE 손실에서는 불가능한 값)이 발생하기 시작합니다 (Quetzalcohuatl, 미상).
- 그런 다음, 노이즈가 섞인 샘플과 순수 샘플을 특정 *_r 배열에 추가합니다.

<div class="content-ad"></div>

```js
# 데이터 형태 변형하기
y_val_noisy_r = []
y_val_pure_r = []
for i in range(0, len(y_val_noisy)):
  noisy_sample = y_val_noisy[i]
  pure_sample = y_val_pure[i]
  noisy_sample = (noisy_sample - np.min(noisy_sample)) / (np.max(noisy_sample) - np.min(noisy_sample))
  pure_sample = (pure_sample - np.min(pure_sample)) / (np.max(pure_sample) - np.min(pure_sample))
  y_val_noisy_r.append(noisy_sample)
  y_val_pure_r.append(pure_sample)
y_val_noisy_r   = np.array(y_val_noisy_r)
y_val_pure_r    = np.array(y_val_pure_r)
noisy_input     = y_val_noisy_r.reshape((y_val_noisy_r.shape[0], y_val_noisy_r.shape[1], 1))
pure_input      = y_val_pure_r.reshape((y_val_pure_r.shape[0], y_val_pure_r.shape[1], 1))
```

샘플을 재샘플링한 후, 재샘플링된 잡음이 섞인 샘플과 재샘플링된 순수 샘플을 텐서플로우/케라스가 처리할 수 있는 구조로 변환합니다. 즉, 우리의 경우에는 채널의 수를 나타내는 다른 차원을 추가합니다. 여기서는 단지 1입니다.

마지막으로, 훈련 및 테스트 데이터로 분할을 수행합니다 (30,000개의 테스트 데이터, 56,000개의 훈련 데이터와 14,000개의 검증 데이터):

```js
# 훈련/테스트 분할
percentage_training = math.floor((1 - train_test_split) * len(noisy_input))
noisy_input, noisy_input_test = noisy_input[:percentage_training], noisy_input[percentage_training:]
pure_input, pure_input_test = pure_input[:percentage_training], pure_input[percentage_training:]
```

<div class="content-ad"></div>

## 모델 아키텍처 작성

저희 오토인코더의 아키텍처입니다:


# 모델 생성
model = Sequential()
model.add(Conv1D(128, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform', input_shape=input_shape))
model.add(Conv1D(32, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform'))
model.add(Conv1DTranspose(32, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform'))
model.add(Conv1DTranspose(128, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform'))
model.add(Conv1D(1, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='sigmoid', padding='same'))

model.summary()


- 우리는 Sequential API를 사용하여 레이어를 서로 쌓습니다.
- 두 Conv1D 레이어는 인코더로 작용하며, 각각 128개와 32개의 필터를 학습합니다. ReLU 활성화 함수로 활성화되며, 따라서 He 초기화가 필요합니다. 맥스-노름 규제가 각각 적용됩니다.
- 32개와 128개의 필터를 학습하는 두 Conv1DTranspose 레이어는 디코더로 작용합니다. ReLU 활성화 및 He 초기화뿐만 아니라 맥스-노름 규제도 사용합니다.
- 최종 Conv 레이어는 출력 레이어로 작용하며, (padding='same' 덕분에) 채널 수를 제외하고는 모양을 변경하지 않습니다.
- 커널 크기는 3픽셀입니다.

<div class="content-ad"></div>

모델 요약을 생성하려면 model.summary()를 호출하세요. 이 summary에는 훈련된 파라미터 수도 표시됩니다:


Model: "sequential"
_________________________________________________________________
Layer (type)                 Output Shape              Param #
=================================================================
conv1d (Conv1D)              (None, 148, 128)          512
_________________________________________________________________
conv1d_1 (Conv1D)            (None, 146, 32)           12320
_________________________________________________________________
conv1d_transpose (Conv1DTrans (None, 148, 32)           3104
_________________________________________________________________
conv1d_transpose_1 (Conv1DTra (None, 150, 128)          12416
_________________________________________________________________
conv1d_2 (Conv1D)            (None, 150, 1)            385
=================================================================
Total params: 28,737
Trainable params: 28,737
Non-trainable params: 0
_________________________________________________________________


## 모델 컴파일 및 훈련 프로세스 시작

다음으로 할 일은 모델을 컴파일하는 것입니다 (옵티마이저 및 손실 함수 지정) 그리고 훈련 프로세스를 시작하는 것입니다. 오늘날의 딥러닝 모델에 대한 상대적으로 기본적인 선택사항인 Adam과 Binary crossentropy를 사용합니다.

<div class="content-ad"></div>

데이터를 맞추는 것은 우리가 noisy_input(특징)에서 pure_input(타겟)으로 이동하고 있다는 것을 나타냅니다. 에포크 수, 배치 크기 및 검증 분할은 이전에 구성한 대로 설정되어 있습니다.

```js
# 데이터 컴파일 및 학습
model.compile(optimizer='adam', loss='binary_crossentropy')
model.fit(noisy_input, pure_input,
          epochs = no_epochs,
          batch_size = batch_size,
          validation_split = validation_split)
```

## 테스트 세트에서의 노이즈 제거된 파형 시각화

훈련 과정이 끝나면 실제로 모델이 작동하는지 확인할 때입니다. 테스트 세트에서 노이즈가 추가된 예제를 생성하고(이는 모델이 이전에 본 적이 없는 데이터입니다!) 노이즈가 없는 형태를 출력하는지 확인하는 것으로 이 코드입니다.

<div class="content-ad"></div>


# 재구성 생성
재구성 수 = 4
샘플 = 노이즈가 추가된 입력_테스트[:재구성 수]
재구성 = 모델.predict(샘플)

# 재구성 그래프로 표시
for i in np.arange(0, 재구성 수):
  # 예측 인덱스
  예측 인덱스 = i + 트레이닝 퍼센티지
  # 샘플 및 재구성 얻기
  원본 = y_val_noisy[예측 인덱스]
  순수 = y_val_pure[예측 인덱스]
  재구성 = np.array(재구성[i])
  # Matplotlib 준비
  fig, axes = plt.subplots(1, 3)
  # 샘플 및 재구성 그래프로 표시
  axes[0].plot(원본)
  axes[0].set_title('노이지 웨이브폼')
  axes[1].plot(순수)
  axes[1].set_title('순수한 웨이브폼')
  axes[2].plot(재구성)
  axes[2].set_title('컨브 인코더 디노이징 웨이브폼')
  plt.show()


터미널을 다시 열고 python signal_autoencoder.py를 실행하세요. 이제, 훈련 프로세스가 시작될 것입니다.

## 전체 모델 코드

전체 코드에 관심이 있다면, 여기에서 확인하세요:


<div class="content-ad"></div>


```js
import tensorflow.keras
from tensorflow.keras.models import Sequential, save_model
from tensorflow.keras.layers import Conv1D, Conv1DTranspose
from tensorflow.keras.constraints import max_norm
import matplotlib.pyplot as plt
import numpy as np
import math

# 모델 설정
input_shape = (150, 1)
batch_size = 150
no_epochs = 5
train_test_split = 0.3
validation_split = 0.2
verbosity = 1
max_norm_value = 2.0

# 데이터 불러오기
data_noisy = np.load('./signal_waves_noisy_medium.npy')
x_val_noisy, y_val_noisy = data_noisy[:,0], data_noisy[:,1]
data_pure = np.load('./signal_waves_medium.npy')
x_val_pure, y_val_pure = data_pure[:,0], data_pure[:,1]

# 데이터 재구성
y_val_noisy_r = []
y_val_pure_r = []
for i in range(0, len(y_val_noisy)):
  noisy_sample = y_val_noisy[i]
  pure_sample = y_val_pure[i]
  noisy_sample = (noisy_sample - np.min(noisy_sample)) / (np.max(noisy_sample) - np.min(noisy_sample))
  pure_sample = (pure_sample - np.min(pure_sample)) / (np.max(pure_sample) - np.min(pure_sample))
  y_val_noisy_r.append(noisy_sample)
  y_val_pure_r.append(pure_sample)
y_val_noisy_r   = np.array(y_val_noisy_r)
y_val_pure_r    = np.array(y_val_pure_r)
noisy_input     = y_val_noisy_r.reshape((y_val_noisy_r.shape[0], y_val_noisy_r.shape[1], 1))
pure_input      = y_val_pure_r.reshape((y_val_pure_r.shape[0], y_val_pure_r.shape[1], 1))

# 훈련 및 테스트 분리
percentage_training = math.floor((1 - train_test_split) * len(noisy_input))
noisy_input, noisy_input_test = noisy_input[:percentage_training], noisy_input[percentage_training:]
pure_input, pure_input_test = pure_input[:percentage_training], pure_input[percentage_training:]

# 모델 생성
model = Sequential()
model.add(Conv1D(128, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform', input_shape=input_shape))
model.add(Conv1D(32, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform'))
model.add(Conv1DTranspose(32, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform'))
model.add(Conv1DTranspose(128, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='relu', kernel_initializer='he_uniform'))
model.add(Conv1D(1, kernel_size=3, kernel_constraint=max_norm(max_norm_value), activation='sigmoid', padding='same'))

model.summary()

# 컴파일 및 데이터 피팅
model.compile(optimizer='adam', loss='binary_crossentropy')
model.fit(noisy_input, pure_input,
                epochs=no_epochs,
                batch_size=batch_size,
                validation_split=validation_split)

# 재구성 생성
num_reconstructions = 4
samples = noisy_input_test[:num_reconstructions]
reconstructions = model.predict(samples)

# 재구성 시각화
for i in np.arange(0, num_reconstructions):
  # Prediction index
  prediction_index = i + percentage_training
  # Get the sample and the reconstruction
  original = y_val_noisy[prediction_index]
  pure = y_val_pure[prediction_index]
  reconstruction = np.array(reconstructions[i])
  # Matplotlib preparations
  fig, axes = plt.subplots(1, 3)
  # Plot sample and reconstruciton
  axes[0].plot(original)
  axes[0].set_title('Noisy waveform')
  axes[1].plot(pure)
  axes[1].set_title('Pure waveform')
  axes[2].plot(reconstruction)
  axes[2].set_title('Conv Autoencoder Denoised waveform')
  plt.show()
```

## 결과

다음은 결과입니다.

다섯 번째 에폭 이후에, 검증 손실은 약 0.3556입니다. 이는 높은 편이지만 수용할만합니다. 더 중요한 것은 테스트 세트 예측 결과를 시각화하여 모델이 얼마나 잘 작동하는지 확인하는 것입니다.


<div class="content-ad"></div>

여기 있습니다:

![Image 1](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_11.png)

![Image 2](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_12.png)

![Image 3](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_13.png)

<div class="content-ad"></div>

![image](/assets/img/2024-07-07-SignalNoiseRemovalAutoencoderwithKeras_14.png)

안녕하세요! 오늘은 오토인코더가 많은 잡음을 제거하는 방법을 배웠어요. 봐봐, 정리된 샘플들은 완전히 잡음이 없지는 않지만 훨씬 나아졌죠. 멋진 결과들이 많군요!

## 요약

이 블로그 포스트에서, 우리는 Keras로 신호 처리에 특화된 노이즈 제거 오토인코더를 만들었습니다. 10만개의 순수 및 잡음이 섞인 샘플들을 생성함으로써, 우리는 입력 데이터에서 특정 잡음을 제거할 수 있는 훈련된 노이즈 제거 알고리즘을 만들어냈어요. 오늘 뭔가를 배우셨길 바라며, 궁금한 점이나 의견이 있으시다면 아래 댓글란에 자유롭게 남겨주세요! 댓글에 답변드릴 수 있도록 최선을 다할게요.

<div class="content-ad"></div>

## 참고 자료

Keras 블로그. (연도 미상). Keras에서 오토인코더 구축하기. [블로그 글 링크](https://blog.keras.io/building-autoencoders-in-keras.html)

Quetzalcohuatl. (연도 미상). 손실값이 음수가 됩니다 · 이슈 #1917 · keras-team/keras. [링크](https://github.com/keras-team/keras/issues/1917#issuecomment)

TensorFlow. (연도 미상). Tf.keras.layers.Conv1DTranspose. [링크](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv1DTranspose)

<div class="content-ad"></div>

TensorFlow에서는 Conv1D 레이어를 사용하는 방법에 대한 자세한 정보를 찾아볼 수 있어요. [여기](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv1D)를 참고해보세요! ✨