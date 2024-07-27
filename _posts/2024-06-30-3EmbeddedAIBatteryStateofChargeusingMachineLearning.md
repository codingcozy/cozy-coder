---
title: "임베디드 AI  머신 러닝을 사용하여 배터리 충전 상태 확인 방법"
description: ""
coverImage: "/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_0.png"
date: 2024-06-30 23:36
ogImage: 
  url: /assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_0.png
tag: Tech
originalTitle: "3. Embedded AI — Battery State of Charge using Machine Learning"
link: "https://medium.com/@reefwing/3-embedded-ai-battery-state-of-charge-using-machine-learning-62a181cac16b"
---


## 배터리 충전 상태(SOC)는 배터리의 잔여 전하를 총 용량의 백분율로 표시합니다. 이 정보는 내장 장치의 에너지 관리에 매우 중요합니다. SOC를 정확하게 추정하는 것은 리튬이온 배터리의 복잡한 행동에 영향을 받아 어려운 작업입니다. 이는 온도, 배터리 건강 상태, 그리고 SOC 자체와 같은 요소들에 의해 영향을 받기 때문입니다. SOC를 추정하기 위한 전통적인 방법인 전기화학 모델은 정확한 매개변수와 배터리의 조성 및 물리적 특성에 대한 심층적인 이해를 요구합니다. 반면, 머신 러닝 모델은 데이터 기반 접근 방식을 제공하여 배터리의 행동에 대해 덜 자세한 지식만으로도 SOC 추정을 단순화시키므로 내장 시스템에 구현하기에 적합합니다.

![이미지](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_0.png)

## 머신 러닝 과정

내장형 AI 시리즈의 제1부에서, ML을 사용하여 값들을 예측하는 과정을 설명했습니다 (그림 1).

<div class="content-ad"></div>


![이미지](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_1.png)

Part 2에서 설명한 것처럼, 배터리 방전 곡선은 대부분의 사용 가능 범위에 걸쳐 거의 선형입니다. 그러므로 우리는 Part 1에서 설명한 선형 회귀 모델을 사용할 수 있을 것입니다. 그러나 출력물은 룩업 테이블 및 보간 방법과 매우 유사할 것입니다. 더 정확하고 유연한 예측을 제공해야 할 다른 접근 방식이 있습니다.

이러한 옵션을 탐색하기 위해 ML 프로세스 플로우차트의 단계를 따를 것입니다.

## 단계 1 — 문제 정의


<div class="content-ad"></div>

배터리의 SOC를 정확하게 예측하는 ML 모델을 개발하는 것이 목표입니다. 이 모델은 전압을 기반으로 배터리의 SOC를 예측하는 데 사용되며 전류, 온도, 그리고 시간과 같은 관련 특징도 함께 고려됩니다 (Figure 2). 모델은 다양한 작동 조건과 배터리 상태에 걸쳐 잘 일반화되어야 하며, 이는 실시간 응용 프로그램에서 신뢰할 수 있는 SOC 추정을 보장합니다.

Inputs and Features

- Voltage (V): 배터리의 순간 전압을 나타내는 주요 입력 특징입니다.
- Current (I): 배터리에 공급되거나 공급되는 전류로, 전압 값에 영향을 줄 수 있습니다.
- Temperature (T): 주변 온도 또는 배터리의 온도로, 온도 변화가 배터리 성능에 영향을 줍니다.
- Time (t): 전압이 측정된 시간으로, 배터리의 방전 또는 충전 주기를 이해하는 데 도움이 됩니다.

<div class="content-ad"></div>

- 전하 상태 (SOC): 배터리의 예상 SOC로서 총 용량의 백분율로 표시됩니다.

![image](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_2.png)

## 단계 2 — 훈련 데이터 수집 및 정리

이 단계에서 가장 많은 작업을 수행하게 될 것입니다. 모델을 훈련시킬 데이터가 더 좋을수록 예측이 더 좋아질 것입니다. 우리는 맥마스터 대학교에서 발행한 LG 18650HG2 리튬이온 배터리에 대한 Digital Commons 데이터를 사용할 것입니다. 아래의 Python 스크립트를 사용하여 이 데이터셋을 다운로드할 수 있습니다.

<div class="content-ad"></div>

```js
import os
import requests
import zipfile
import numpy as np

# 다운로드 할 파일의 URL
url = "https://data.mendeley.com/public-files/datasets/cp3473x7xv/files/ad7ac5c9-2b9e-458a-a91f-6f3da449bdfb/file_downloaded"

# 추출된 ZIP 파일이 들어 있는 출력 폴더
output_folder = os.path.expanduser("~/Documents/GitHub/Embedded-AI/data/LGHG2@n10C_to_25degC")
os.makedirs(output_folder, exist_ok=True)

# 데이터 세트 다운로드 및 추출
train_folder = os.path.join(output_folder, "Train")
test_folder = os.path.join(output_folder, "Test")
if not os.path.exists(train_folder) or not os.path.exists(test_folder):
    print("LGHG2@n10C_to_25degC.zip (56 MB) 다운로드 중... ")
    download_folder = os.path.dirname(output_folder)
    filename = os.path.join(download_folder, "LGHG2@n10C_to_25degC.zip")
    response = requests.get(url)
    with open(filename, 'wb') as file:
        file.write(response.content)
    with zipfile.ZipFile(filename, 'r') as zip_ref:
        zip_ref.extractall(output_folder)
```

이 스크립트에서 output_folder를 수정하여 데이터를 저장할 위치를 지정하십시오. 이 데이터를 사용하기 전에 데이터가 어떻게 구성되어 있는지 이해해야 합니다. Figure 3는 추출된 데이터 세트의 내용을 보여줍니다.

<img src="/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_3.png" />

각 파일에는 다섯 가지 예측 변수(전압, 전류, 온도, 평균 전압 및 평균 전류)로 이루어진 시계열 X와 하나의 대상 SOC로 이루어진 시계열 Y가 포함되어 있습니다. 서로 다른 환경 온도에서 수집된 데이터를 나타내는 하나의 훈련 파일과 네 개의 테스트 파일이 있습니다. 유일한 문제는 데이터가 MATLAB 형식(.mat)으로 저장되어 있다는 것입니다.


<div class="content-ad"></div>

.mat 파일은 MATLAB에서 변수를 저장하는 데 사용되는 이진 데이터 파일입니다. 배열, 행렬 및 기타 데이터 유형을 저장할 수 있습니다. 이진 파일이기 때문에 텍스트 편집기에서는 읽을 수 없지만, Python의 scipy.io 모듈을 사용하여 이러한 파일을 읽고 쓸 수 있습니다.

```python
import os
import scipy.io
import numpy as np

# .mat 파일의 처음 10줄을 읽어 출력
def read_and_print_mat_files(folder):
    for filename in os.listdir(folder):
        if filename.endswith(".mat"):
            filepath = os.path.join(folder, filename)
            mat_data = scipy.io.loadmat(filepath)
            print(f"{filename} 내용:")
            for key in mat_data:
                if not key.startswith("__"):
                    data = mat_data[key]
                    if isinstance(data, np.ndarray) and data.ndim > 1:  
                        print(f"{key}:")
                        print(data[:10])  
                    else:
                        print(f"{key}: {data}")
            print("\n")

# 폴더 경로
train_folder = os.path.expanduser("~/Documents/GitHub/Embedded-AI/data/LGHG2@n10C_to_25degC/Train")
test_folder = os.path.expanduser("~/Documents/GitHub/Embedded-AI/data/LGHG2@n10C_to_25degC/Test")

# Train 및 Test 파일의 내용 읽고 출력
print("훈련 데이터 파일:")
read_and_print_mat_files(train_folder)
print("\n테스트 데이터 파일:")
read_and_print_mat_files(test_folder)
```

훈련 데이터의 처음 다섯 행의 내용은 아래와 같이 표시됩니다.

```python
TRAIN_LGHG2@n10degC_to_25degC_Norm_5Inputs.mat 내용:
X:
[[0.38514793 0.38515183 0.38515573 ... 0.47884278 0.4789612  0.4789612 ]
 [0.75102009 0.75102009 0.75102009 ... 0.75102009 0.75102009 0.75102009]
 [0.30310108 0.30459129 0.3060815  ... 0.00847709 0.00847709 0.00847709]
 [0.38514793 0.38514988 0.38515183 ... 0.45983939 0.45997861 0.46011672]
 [0.75102009 0.75102009 0.75102009 ... 0.75102009 0.75102009 0.75102009]]
Y:
[[0.20641667 0.20641667 0.20641667 ... 0.28324333 0.28324333 0.28324333]]
```

<div class="content-ad"></div>

X는 각 행이 다음 다섯 개의 예측변수 중 하나에 해당하는 2D 배열입니다:

- 전압
- 전류
- 온도
- 평균 전압
- 평균 전류

각 열은 Series에서 다른 시간점을 나타내며, 측정은 1초마다 이루어졌습니다. 예를 들어, 첫 번째 행은 전압: [0.38514793, 0.38515183, 0.38515573, …, 0.47884278, 0.4789612, 0.4789612]이고, 두 번째 행은 전류: [0.75102009, 0.75102009, 0.75102009, …, 0.75102009, 0.75102009, 0.75102009]이며, 나머지 예측변수에 대한 내용도 같은 방식입니다.

Y도 하나의 행으로 이루어진 2D 배열이지만, SOC가 단일 대상 변수임을 나타냅니다. 각 열은 해당 시간점에서의 SOC를 나타냅니다.

<div class="content-ad"></div>

이제 파일 내용을 이해했으므로 훈련 및 테스트를 위해 데이터 저장소를 로드할 수 있습니다.

```js
def read_mat_files(folder):
    data = []
    for filename in os.listdir(folder):
        if filename.endswith(".mat"):
            filepath = os.path.join(folder, filename)
            mat_data = scipy.io.loadmat(filepath)
            data.append(mat_data)
    return data

# 훈련 데이터와 테스트 데이터를 위한 파일 데이터 저장소 생성
fds_train = read_mat_files(train_folder)
fds_test = read_mat_files(test_folder)

# 데이터 저장소에 있는 모든 데이터 읽기
train_data_full = fds_train[0]
test_data_full_n10deg = fds_test[0]
test_data_full_0deg = fds_test[1]
test_data_full_10deg = fds_test[2]
test_data_full_25deg = fds_test[3]

# 데이터 배열의 모양 출력하여 구조를 이해하기
print("train_data_full['X']의 모양: ", train_data_full['X'].shape)
print("train_data_full['Y']의 모양: ", train_data_full['Y'].shape)
print("test_data_full_n10deg['X']의 모양: ", test_data_full_n10deg['X'].shape)
print("test_data_full_n10deg['Y']의 모양: ", test_data_full_n10deg['Y'].shape)
```

shape 속성은 배열의 차원을 나타내는 튜플을 반환합니다. 2D 배열의 경우 shape는 (행, 열)로 이루어진 튜플을 반환합니다.

```js
train_data_full['X']의 모양:  (5, 669956)
train_data_full['Y']의 모양:  (1, 669956)
test_data_full_n10deg['X']의 모양:  (5, 47517)
test_data_full_n10deg['Y']의 모양:  (1, 47517)
```

<div class="content-ad"></div>

훈련 데이터 파일 내에서, 주변 온도 측정 값은 다음과 같이 분할되어 있습니다:

```js
idx0   = 1 - 184257        # 온도 = 0°C
idx10  = 184258 - 337973   # 온도 = 10°C
idx25  = 337974 - 510530   # 온도 = 25°C
idxN10 = 510531 - 669956   # 온도 = -10°C
```

우리가 출력한 훈련 데이터의 첫 5개 행을 살펴보면, X의 3번째 행이 온도를 나타냅니다. 이 값이 0이 될 것으로 예상할 수 있지만, 이 값들은 측정되고 정규화된 값입니다. 모든 예측 변수는 모델 수렴 및 성능을 향상시키기 위해 정규화되었습니다.

훈련 온도 데이터 세트를 분리하려면, 아래 코드를 로딩 스크립트에 추가하십시오.

<div class="content-ad"></div>

```js
# train_data_full에서 X와 Y 추출하기
X = train_data_full['X']
Y = train_data_full['Y']

# 인덱스 범위 정의
idx0 = slice(0, 184257)
idx10 = slice(184257, 337973)
idx25 = slice(337973, 510530)
idxN10 = slice(510530, 669956)

# 데이터 세그먼트 추출
X_idx0 = X[:, idx0]
Y_idx0 = Y[:, idx0]

X_idx10 = X[:, idx10]
Y_idx10 = Y[:, idx10]

X_idx25 = X[:, idx25]
Y_idx25 = Y[:, idx25]

X_idxN10 = X[:, idxN10]
Y_idxN10 = Y[:, idxN10]

# 추출 확인을 위해 형태 출력
print(f'X_idx0 형태: {X_idx0.shape}, Y_idx0 형태: {Y_idx0.shape}')
print(f'X_idx10 형태: {X_idx10.shape}, Y_idx10 형태: {Y_idx10.shape}')
print(f'X_idx25 형태: {X_idx25.shape}, Y_idx25 형태: {Y_idx25.shape}')
print(f'X_idxN10 형태: {X_idxN10.shape}, Y_idxN10 형태: {Y_idxN10.shape}')
```

트레이닝 데이터에는 총 669,956개의 열이 있어서 처리하기에는 조금 많습니다. 간단하게 만들기 위해 데이터를 다시 샘플링하여 매 100번째 점을 가져와 평균 전압 및 평균 전류 예측 변수의 새로운 이동 평균값을 계산할 것입니다. 재계산된 이동 평균은 각 포인트에 대해 최근 5개 샘플을 사용합니다. 이는 i-5부터 i까지의 값들의 평균을 계산하기 위해 루프를 사용하여 수행됩니다. 이동 평균을 계산하는 데 최근 5개 샘플을 사용하는 것은 시계열 분석에서 일반적인 기술입니다. 이 접근 방식은 데이터를 부드럽게 만들면서 최근 변화에 반응성을 유지하는 균형을 제공합니다.

데이터 전처리를 돕기 위해 파이썬 함수를 작성했습니다.

```js
def resample_and_compute_moving_averages(X, Y, step=100):
    # 데이터 재샘플링 (매 `step`번째 포인트 가져오기)
    X_resampled = X[:, ::step]
    Y_resampled = Y[:, ::step]
    
    # 새로운 이동 평균 계산
    n = X_resampled.shape[1]
    avg_voltage_idx = 3  # 4번째 열이 평균 전압이라고 가정
    avg_current_idx = 4  # 5번째 열이 평균 전류이라고 가정
    
    new_avg_voltage = np.empty(n)
    new_avg_current = np.empty(n)
    
    for i in range(n):
        new_avg_voltage[i] = np.mean(X_resampled[0, max(0, i-5):i+1])
        new_avg_current[i] = np.mean(X_resampled[1, max(0, i-5):i+1])
    
    X_resampled[avg_voltage_idx, :n] = new_avg_voltage
    X_resampled[avg_current_idx, :n] = new_avg_current
    
    return X_resampled, Y_resampled
```

<div class="content-ad"></div>

학습 및 테스트 데이터 재샘플링은 이제 간단합니다.

```js
# 학습 데이터를 재샘플링하고 새로운 이동 평균 계산
X_train_resampled, Y_train_resampled = resample_and_compute_moving_averages(X_train, Y_train)

# 테스트 데이터 추출 및 재샘플링
X_test_n10deg = test_data_full_n10deg['X']
Y_test_n10deg = test_data_full_n10deg['Y']
X_test_n10deg_resampled, Y_test_n10deg_resampled = resample_and_compute_moving_averages(X_test_n10deg, Y_test_n10deg)

X_test_0deg = test_data_full_0deg['X']
Y_test_0deg = test_data_full_0deg['Y']
X_test_0deg_resampled, Y_test_0deg_resampled = resample_and_compute_moving_averages(X_test_0deg, Y_test_0deg)

X_test_10deg = test_data_full_10deg['X']
Y_test_10deg = test_data_full_10deg['Y']
X_test_10deg_resampled, Y_test_10deg_resampled = resample_and_compute_moving_averages(X_test_10deg, Y_test_10deg)

X_test_25deg = test_data_full_25deg['X']
Y_test_25deg = test_data_full_25deg['Y']
X_test_25deg_resampled, Y_test_25deg_resampled = resample_and_compute_moving_averages(X_test_25deg, Y_test_25deg)

# 재샘플링 확인을 위해 형태 출력
print(f'재샘플링 후 학습 데이터 형태: X={X_train_resampled.shape}, Y={Y_train_resampled.shape}')
print(f'n10degC 테스트 데이터 형태: X={X_test_n10deg_resampled.shape}, Y={Y_test_n10deg_resampled.shape}')
print(f'0degC 테스트 데이터 형태: X={X_test_0deg_resampled.shape}, Y={Y_test_0deg_resampled.shape}')
print(f'10degC 테스트 데이터 형태: X={X_test_10deg_resampled.shape}, Y={Y_test_10deg_resampled.shape}')
print(f'25degC 테스트 데이터 형태: X={X_test_25deg_resampled.shape}, Y={Y_test_25deg_resampled.shape}')
```

데이터 처리 스케치의 전체 버전은 Reefwing Gist에서 제공됩니다. 데이터셋 크기를 재샘플링한 후에는 아래와 같은 크기가 됩니다.

```js
재샘플링 후 학습 데이터 형태: X=(5, 6700), Y=(1, 6700)
n10degC 테스트 데이터 형태: X=(5, 476), Y=(1, 476)
0degC 테스트 데이터 형태: X=(5, 443), Y=(1, 443)
10degC 테스트 데이터 형태: X=(5, 426), Y=(1, 426)
25degC 테스트 데이터 형태: X=(5, 393), Y=(1, 393)
```

<div class="content-ad"></div>

저희는 모든 전처리된 학습 및 테스트 데이터를 Preprocessed라는 새 하위 디렉토리에 저장할 거에요. 이 파일들은 CSV 형식으로 저장되며, 쉽게 읽고 쓸 수 있어서 사용자가 쉽게 확인할 수 있어요. 더 큰 데이터셋의 경우 HDF5 (계층적 데이터 형식)를 사용하면 효율적으로 대량의 데이터를 저장할 수 있고 복잡한 데이터 구조를 지원해요. 이는 과학 계산에서도 널리 사용되고 있어요. 관련 추가 코드는 아래에 나와 있어요.

```js
preprocessed_folder = os.path.join(output_folder, 'Preprocessed')
os.makedirs(preprocessed_folder, exist_ok=True)

...

# 학습 데이터를 리샘플링하고 새로운 이동 평균을 계산합니다.
X_train_resampled, Y_train_resampled = resample_and_compute_moving_averages(X_train, Y_train)

# DataFrame을 생성하고 CSV로 저장합니다.
train_df = pd.DataFrame(np.vstack((X_train_resampled, Y_train_resampled)).T,
                        columns=['Voltage', 'Current', 'Temperature', 'Average Voltage', 'Average Current', 'SOC'])
train_df.to_csv(os.path.join(preprocessed_folder, 'resampled_training_data.csv'), index=False)

# 테스트 데이터를 추출하고 리샘플링합니다.
test_data_files = ['n10degC', '0degC', '10degC', '25degC']
resampled_test_data_shapes = {}

for i, test_data_full in enumerate(fds_test):
    X_test = test_data_full['X']
    Y_test = test_data_full['Y']
    X_test_resampled, Y_test_resampled = resample_and_compute_moving_averages(X_test, Y_test)
    test_df = pd.DataFrame(np.vstack((X_test_resampled, Y_test_resampled)).T,
                           columns=['Voltage', 'Current', 'Temperature', 'Average Voltage', 'Average Current', 'SOC'])
    test_df.to_csv(os.path.join(preprocessed_folder, f'resampled_test_data_{test_data_files[i]}.csv'), index=False)
    resampled_test_data_shapes[test_data_files[i]] = (X_test_resampled.shape, Y_test_resampled.shape)
```

## 단계 3 — 모델 선택

배터리 충전 상태 (SOC) 및 건강 상태 (SOH)를 추정하기 위해 여러 머신러닝 알고리즘이 광범위하게 연구되어 왔습니다. 네 가지 가장 탁월한 유형은 얕은 신경망 (NN), 딥러닝 (DL), 서포트 벡터 머신 (SVM), 그리고 가우시안 프로세스 회귀 (GPR)입니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_4.png)

얕은 신경망(SNN)
얕은 신경망(Figure 4)은 한 개 또는 두 개의 은닉층으로 구성됩니다. 딥 러닝과 비교하면 비교적 단순하지만 전력 특성의 비선형성을 모델링하기에 충분히 강력합니다. 단층 신경망은 더 간단한 구조로, 계산 능력과 메모리가 덜 필요하여 자원 제한적인 임베디드 장치에서 실시간 SOC 및 SOH 추정에 적합합니다. 이러한 모델은 전압, 전류, 온도와 같은 다양한 입력 특성으로부터 SOC 및 SOH를 예측하는 데 학습될 수 있습니다.

![이미지](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_5.png)

딥 러닝(DL)
딥 러닝 모델인 딥 신경망(DNNs — Figure 5) 및 합성곱 신경망(CNNs)과 같이 여러 층을 가지고 있어 데이터의 복잡한 패턴과 상호작용을 포착할 수 있습니다. DL 모델은 입력 특성과 SOC 또는 SOH 사이의 복잡한 관계를 학습하여 매우 정확한 추정을 제공할 수 있습니다. 그러나 그들의 복잡성은 더 많은 계산 자원과 메모리를 필요로 하며, 이는 일부 임베디드 시스템에 제약 요인이 될 수 있습니다. 그러나 임베디드 하드웨어의 발전으로 실시간 응용 프로그램에서 딥 러닝 모델을 배포하긴 점점 더 가능해지고 있습니다.

<div class="content-ad"></div>

![SVM](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_6.png)

서포트 벡터 머신(SVM)
서포트 벡터 머신(도표 6)은 분류와 회귀 작업 모두에 사용할 수 있는 지도 학습 모델입니다. SOC 및 SOH 추정을 위해 SVM은 고차원 데이터를 효과적으로 처리할 수 있으며, 특히 훈련 데이터가 제한적인 상황에서 과적합에 강합니다. SVM은 데이터를 서로 다른 클래스로 분리하거나 연속 값을 예측하는 데 최적의 초평면을 찾아 작동합니다. 상대적으로 낮은 계산 요구로 인해 리소스가 제한된 임베디드 응용 프로그램에 적합합니다.

![GPR](/assets/img/2024-06-30-3EmbeddedAIBatteryStateofChargeusingMachineLearning_7.png)

가우시안 프로세스 회귀(GPR)
가우시안 프로세스 회귀(도표 7)는 예측 뿐만 아니라 불확실성 추정을 제공하는 비모수적인 확률적 접근 방식입니다. SOC 및 SOH 추정에 특히 유용한 GPR은 운영 조건 및 노화로 인한 배터리 행동의 기저 불확실성을 모델링할 수 있습니다. GPR 모델은 유연하며 새로운 데이터로 업데이트할 수 있어, 배터리 수명 주기 내의 변화에 적응할 수 있습니다. 이러한 이유로 GPR 모델을 채택하여 구현하기로 결정했습니다.

<div class="content-ad"></div>

하지만 GPR은 계산이 많이 필요하며, 실시간 임베디드 애플리케이션에 적합하게 만들기 위해 효율적인 구현이 필요합니다. 또는 장치 외부에서 처리를 수행하고 최적화된 후에 훈련된 모델을 배포할 수도 있습니다. 이것이 우리가 취할 접근 방식입니다.

## 파트 4

시리즈의 제 4부에서 배터리 SOC 예측기의 ML 프로세스를 계속할 것입니다. 이는 모델을 훈련하고 evaluate하는 데 중점을 둘 것입니다.

## 참고문헌

<div class="content-ad"></div>

[1] Kollmeyer, Philip; Vidal, Carlos; Naguib, Mina; Skells, Michael (2020), “LG 18650HG2 Li-ion Battery Data and Example Deep Neural Network xEV SOC Estimator Script”, Mendeley Data, V3, doi: 10.17632/cp3473x7xv.3

만약 이 글을 즐겁게 보셨고 제 글쓰기를 지원하고 싶으시다면, 팔로우를 누르거나 (최대 50번), 강조하기, 혹은 댓글을 남겨주세요! 혹은 다른 방법으로 커피를 사주시거나 구독해주셔도 됩니다. 새 글이 올라올 때마다 이메일을 받아보실 수 있습니다.