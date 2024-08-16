---
title: "컨볼루션 신경망 이전의 기술은 무엇이었을까  1부"
description: ""
coverImage: "/assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_0.png"
date: 2024-07-12 23:42
ogImage: 
  url: /assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_0.png
tag: Tech
originalTitle: "But what was before Convolutional Neural Networks? | Part 1"
link: "https://medium.com/thedeephub/but-what-was-before-convolutional-neural-networks-part-1-d654737b026a"
isUpdated: true
---





## Histograms of Oriented Gradients에 대한 간단한 소개

안녕하세요! 오늘은 Histograms of Oriented Gradients (HOG)에 대해 이야기해보려고 해요.

HOG는 Navneet Dalal과 Bill Triggs에 의해 2005년에 소개되었어요. 이 기법은 사람 감지를 목적으로 만들어졌답니다.

그들의 논문 "Histograms of Oriented Gradients for Human Detection"은 IEEE Computer Society Conference on Computer Vision and Pattern Recognition (CVPR)에서 발표되었고, 컴퓨터 비전 분야에서 중요한 작품으로 손꼽히게 되었어요.

그럼 이제 Histograms of Oriented Gradients에 대해 기본적인 개념을 알아볼까요? 함께 공부해요! 🌟

<div class="content-ad"></div>

일반적으로 HOG(Histogram of Oriented Gradients)는 컴퓨터 비전과 이미지 처리에서 사용되는 피처 설명자로, 객체 감지를 목적으로 사용됩니다.

간단히 말하면, 이미지 내 객체를 이해하고 식별하는 컴퓨터의 방법입니다.

사진을 보면서 물체를 인식 가능하게 하는 윤곽선과 모양에 주목해 보세요. 예를 들어, 공의 곡선이나 의자 다리의 각도 등입니다.

HOG는 사진의 다른 부분에서 밝기나 색상이 어떻게 변하는지에 초점을 맞춤으로써 컴퓨터가 이러한 모양을 인식할 수 있도록 도와줍니다. 이미지의 작은 블록이나 패치를 살펴보고, 그림자나 빛의 강한 변화가 일어나는 곳을 주목하며 이러한 패턴을 추적합니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_0.png)

## # 특성 기술자

특성 기술자는 이미지를 대표하는 표현으로, 관련 정보를 추출하고 그렇게 유용하지 않은 정보를 제외하여 단순화하는 역할을 합니다.

일반적으로 이미지나 이미지 일부의 특성을 설명하는 벡터(또는 경우에 따라 벡터 세트)로 캡슐화됩니다.

<div class="content-ad"></div>

이 벡터의 각 요소는 이미지에서 추출된 특징을 나타냅니다. 이러한 속성은 가장자리, 모서리, 질감, 색상, 모양 등이 될 수 있어요...

![Link to the image](/assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_1.png)

## # 그라디언트

이미지에서의 그라디언트는 한 지점에서 다른 지점까지 색상이나 밝기가 얼마나 변하는지를 측정합니다. 이러한 변화는 일반적으로 물체의 가장자리에 발생합니다.

<div class="content-ad"></div>

언덕의 경사로를 상상해보세요. 언덕이 가파를수록(또는 밝기의 변화가 급격할수록) 그래디언트는 더 강해집니다.

![image](/assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_2.png)

위 그림에서 이것을 명확하게 볼 수 있습니다.

- 첫 번째 그림에서는 색상이 변하지 않기 때문에 강조된 사각형이 매우 약할 것입니다.
- 두 번째 그림에서는 색깔이 달라지면서 사각형의 한 모서리에서 그래디언트가 변화한다는 것을 나타냅니다.
- 세 번째 그림에서는 그래디언트의 급격한 변화가 두부와 귀를 구분짓는 것을 볼 수 있습니다.

<div class="content-ad"></div>

# HOG(호그) 계산 단계

## 1. 이미지 나누기

우선 이미지를 작은 정사각형인 '셀(cells)'로 나눕니다.

그것들은 그림을 이루는 작은 타일들 같아요.

<div class="content-ad"></div>

예시로, 이러한 셀들은 일반적으로 8x8 또는 16x16 픽셀과 같은 고정된 크기의 정사각형입니다.

## 2.- 그레디언트 계산

각 셀마다, 그레디언트의 방향과 세기를 계산하여 경사가 어느 방향으로 되어있고 얼마나 가파른지 알아냅니다.

## 그레디언트 방향
이것은 x (수평) 및 y (수직) 방향에서 강도 또는 색상의 변화를 찾아내어 계산됩니다.

<div class="content-ad"></div>

그라디언트 방향은 다음과 같이 주어집니다:

θ = arctan(Δy / Δx)

## 그라디언트 크기
크기 (에지가 강한지 약한지)는 각 셀마다 계산됩니다. 일반적으로 x와 y의 변화 제곱의 합의 제곱근을 취함으로써 계산됩니다.

|G| = √(Δx² + Δy²)

<div class="content-ad"></div>

### 3.- 히스토그램 만들기

각 셀마다 그레디언트 방향의 빈도를 보여주는 히스토그램이 만들어집니다.

히스토그램의 각 막대는 그레디언트의 각도를 나타냅니다. 막대의 길이는 해당 각도가 셀에서 얼마나 자주 나타나는지를 보여줍니다.

![image](https://miro.medium.com/v2/resize:fit:1194/0*Zs8z_vp_1fhWBEsQ.gif)

<div class="content-ad"></div>

## 4.- 정확도 향상을 위한 정규화

조명이나 그림자의 변화가 HOG 기술자를 방해하지 않도록 하기 위해 히스토그램은 정규화됩니다.

이는 히스토그램을 조건에 따라 동일하게 나타내도록 조정하는 것을 의미합니다.

일반적인 정규화 방법에는 다음과 같은 것들이 있습니다:

<div class="content-ad"></div>

- L2-노름
- L1-노름
- L1-제곱근
- L2-Hys

## 5.- 모두 결합하기

모든 셀의 히스토그램이 하나의 큰 특징 기술자로 결합됩니다.

## 6.- 탐지를 위한 HOG 사용하기

<div class="content-ad"></div>

오늘은 합친 히스토그램을 '지문'으로 사용해 물체를 식별하는 방법에 대해 이야기해 보려고 해요.

## 전체 과정 예시 (파이썬으로)

언스플래시의 카스턴 와인기어(Karsten Winegear) 님의 이미지를 사용할 거에요. 만약 이 과정을 따라하고 싶다면 자유롭게 다운로드해 보세요.

![이미지](/assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_3.png)

<div class="content-ad"></div>

## 1.- 그레이스케일로 변환하고 이미지를 분할합니다.

이미지는 그레이스케일로 변환된 후 16 × 16 셀로 분할됩니다.

![image](/assets/img/2024-07-12-ButwhatwasbeforeConvolutionalNeuralNetworksPart1_4.png)

## 1.1.- 파이썬 코드

<div class="content-ad"></div>


## 2. 그라디언트 계산

이제 이미지의 각 셀에 대한 그라디언트 크기와 방향을 계산할 수 있습니다.

그라디언트는 각 픽셀마다가 아닌 각 셀마다 계산된다는 점을 기억해주세요.


<div class="content-ad"></div>

아래 그림에서 사각형을 픽셀과 혼동하지 마세요, 이는 셀 영역입니다!

## 셀 그라디언트 크기
밝은 영역은 더 강한 그라디언트를 나타내며, 이는 인텐시티의 변화가 더 큼을 의미합니다.

그라디언트 크기를 통해 우리는 강아지의 형상을 명확히 볼 수 있습니다.

## 셀 그라디언트 방향
이미지 전체에서 색상의 변화는 원본 이미지의 가장자리와 질감이 향하는 다양한 방향을 나타냅니다.

<div class="content-ad"></div>

개의 윤곽은 파란 영역으로 표현되는 것으로 보입니다. 이는 해당 영역의 그라디언트 방향이 유사하다는 것을 나타내며, 일반적으로 연속적인 가장자리나 경계를 따라 발생합니다.

배경은 비교적 일정한 색상으로 보이는데, 이는 강한 또는 변화하는 그라디언트를 갖고 있지 않음을 시사합니다.

## 2.1.- 파이썬 코드

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# 이미지 로드
image_path = 'path_to_your_image.png'  # 여러분의 이미지 경로로 대체해주세요
image = cv2.imread(image_path)

# 그레이스케일로 변환
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# 셀 크기를 위한 매개변수
cell_width, cell_height = 16, 16

# 소벨 오퍼레이터를 사용하여 그라디언트 계산
grad_x = cv2.Sobel(gray_image, cv2.CV_32F, 1, 0, ksize=1)
grad_y = cv2.Sobel(gray_image, cv2.CV_32F, 0, 1, ksize=1)

# 그라디언트 크기와 방향 계산
magnitude = cv2.magnitude(grad_x, grad_y)
direction = cv2.phase(grad_x, grad_y, angleInDegrees=True)

# 셀마다 그라디언트 크기와 방향을 저장할 배열 초기화
cell_magnitude = np.zeros((num_cells_y, num_cells_x))
cell_direction = np.zeros((num_cells_y, num_cells_x))

# 각 셀의 그라디언트 크기와 방향 계산
for i in range(num_cells_y):
    for j in range(num_cells_x):
        cell_grad_x = grad_x[i*cell_height:(i+1)*cell_height, j*cell_width:(j+1)*cell_width]
        cell_grad_y = grad_y[i*cell_height:(i+1)*cell_height, j*cell_width:(j+1)*cell_width]
        cell_magnitude[i, j] = np.sum(cv2.magnitude(cell_grad_x, cell_grad_y))
        cell_direction[i, j] = np.sum(cv2.phase(cell_grad_x, cell_grad_y, angleInDegrees=True))

# 시각화 목적으로 배열 재구성
cell_magnitude = cell_magnitude.repeat(cell_height, axis=0).repeat(cell_width, axis=1)
cell_direction = cell_direction.repeat(cell_height, axis=0).repeat(cell_width, axis=1)

# 각 셀의 그라디언트 크기와 방향 플로팅
plt.figure(figsize=(20, 10))

plt.subplot(121)
plt.title('Cell Gradient Magnitude')
plt.imshow(cell_magnitude, cmap='hot')
plt.axis('off')

plt.subplot(122)
plt.title('Cell Gradient Direction')
plt.imshow(cell_direction, cmap='hsv')  # HSV 컬러 맵으로 각도 표현
plt.axis('off')

plt.tight_layout()
plt.show()
```

<div class="content-ad"></div>

## 3.- 히스토그램 작성하기

이미지의 그래디언트 세기와 방향을 계산했다면, 다음 단계는 이미지를 분할한 각 셀에 대한 그래디언트 방향의 히스토그램을 계산하는 것입니다.

개의 형상은 매우 주목할 만합니다.

더 균일한 회색 히스토그램에 두드러지는 파란색 정사각형은 가장자리가 집중되어 있는 영역이나 특정 기능 방향을 나타냅니다.

<div class="content-ad"></div>

이것은 개의 특징이 가장 두드러지게 나타나는 곳을 나타냅니다.

### 3.1.- Python 코드

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# 이미지 불러오기
image_path = 'path_to_your_image.png'  # 여러분의 이미지 경로로 대체하세요
image = cv2.imread(image_path)

# 흑백 이미지로 변환
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# 소벨 연산자를 사용하여 그래디언트 계산
grad_x = cv2.Sobel(gray_image, cv2.CV_32F, 1, 0, ksize=1)
grad_y = cv2.Sobel(gray_image, cv2.CV_32F, 0, 1, ksize=1)

# 그래디언트 크기와 방향 계산
magnitude = cv2.magnitude(grad_x, grad_y)
direction = cv2.phase(grad_x, grad_y, angleInDegrees=True)

# 히스토그램을 위한 셀 크기와 빈 수 정의
cell_size = (16, 16)  # 픽셀의 셀 크기 (높이, 너비)
nbins = 9  # 히스토그램의 빈 수
num_cells_x = int(gray_image.shape[1] / cell_size[1])
num_cells_y = int(gray_image.shape[0] / cell_size[0])

# 각 셀의 히스토그램을 보유할 배열 초기화
histograms = np.zeros((num_cells_y, num_cells_x, nbins))

# 각 셀의 히스토그램 계산
for i in range(num_cells_y):
    for j in range(num_cells_x):
        cell_direction = direction[i*cell_size[0]:(i+1)*cell_size[0], j*cell_size[1]:(j+1)*cell_size[1]]
        cell_magnitude = magnitude[i*cell_size[0]:(i+1)*cell_size[0], j*cell_size[1]:(j+1)*cell_size[1]]
        hist, _ = np.histogram(cell_direction, bins=nbins, range=(0, 180), weights=cell_magnitude)
        histograms[i, j, :] = hist

# 모든 셀 히스토그램을 하나의 히스토그램으로 집계
aggregated_histogram = np.sum(histograms, axis=(0, 1))

# 집계된 히스토그램 플롯
plt.figure(figsize=(10, 4))
plt.bar(np.arange(nbins), aggregated_histogram, align='center', width=0.8)
plt.title('전체 이미지를 위한 방향 그레디언트 집계 히스토그램')
plt.xlabel('방향 빈')
plt.ylabel('크기')
plt.xticks(np.arange(nbins))
plt.show()
```

### 4.- 히스토그램 정규화 및 결과 표시

<div class="content-ad"></div>

마지막 단계는 히스토그램을 정규화하여 디스크립터가 빛과 그림자 변화에 민감하지 않도록 하는 것입니다.

다음 그림에서 최종 결과를 시각화할 수 있습니다:

## 4.1.- Python 코드

```python
from skimage.feature import hog
from skimage import exposure
import matplotlib.pyplot as plt
import cv2

# 파일로부터 이미지 로드
image_path = '/path/to/your/image.png'  # 실제 이미지 경로로 대체하세요

image = cv2.imread(image_path)
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# HOG 특징과 HOG 이미지 계산
fd, hog_image = hog(gray_image, orientations=8, pixels_per_cell=(16, 16),
                    cells_per_block=(1, 1), visualize=True, block_norm='L2-Hys')

# 히스토그램 재조정하여 더 나은 표시
hog_image_rescaled = exposure.rescale_intensity(hog_image, in_range=(0, 10))

# 원본 이미지와 HOG 이미지 플롯
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 6), sharex=True, sharey=True)
ax1.axis('off')
ax1.imshow(gray_image, cmap=plt.cm.gray)
ax1.set_title('원본 이미지')
ax2.axis('off')
ax2.imshow(hog_image_rescaled, cmap=plt.cm.gray)
ax2.set_title('HOG 이미지')
plt.show()
```

<div class="content-ad"></div>

## 참고 자료

- [towardsdatascience](https://towardsdatascience.com/hog-histogram-of-oriented-gradients-67ecd887675f)
- [analytics-vidhya](https://medium.com/analytics-vidhya/a-gentle-introduction-into-the-histogram-of-oriented-gradients-fdee9ed8f2aa)
- [learnopencv](https://learnopencv.com/histogram-of-oriented-gradients/)

읽어 주셔서 감사합니다! 이 글이 마음에 드시면 박수를 빕니다(최대 50회!) 그리고 새로운 글을 확인하려면 Medium에서 제게 팔로우하세요.

또한, 새로운 퍼블리케이션도 팔로우해 주세요!