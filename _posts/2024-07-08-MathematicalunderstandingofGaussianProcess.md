---
title: "가우시안 프로세스의 수학적 이해"
description: ""
coverImage: "/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_0.png"
date: 2024-07-08 00:08
ogImage: 
  url: /assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_0.png
tag: Tech
originalTitle: "Mathematical understanding of Gaussian Process"
link: "https://medium.com/the-quantastic-journal/mathematical-understanding-of-gaussian-process-eaffc9c8a6d6"
---


## 기계 학습과 수학

![Mathematical understanding of Gaussian Process](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_0.png)

가우시안 프로세스는 데이터가 적을 때 특히 유용합니다. 저는 제조 산업에서 데이터 과학자로 일할 때, 우리 팀은 다음에 어떤 실험 조건을 시행해야 하는지 밝히기 위해 이 알고리즘을 사용했습니다. 그러나 이 알고리즘은 다른 알고리즘에 비해 덜 인기가 있습니다. 이 블로그에서는 시각화와 파이썬 구현을 통해 가우시안 프로세스의 수학적 배경을 설명하겠습니다.

## 목차

<div class="content-ad"></div>

## 1. 처음 단계: 다변량 가우시안 분포

다변량 가우시안 분포는 가우시안 프로세스를 이해하기 위한 필수적인 개념 중 하나입니다. 빠르게 복습해보겠습니다. 만약 다변량 가우시안 분포에 익숙하다면 이 섹션을 건너뛰어도 됩니다.

다변량 가우시안 분포는 두 개 이상의 차원을 갖는 가우시안 분포의 결합 확률입니다. 다변량 가우시안 분포의 확률 밀도 함수는 아래와 같습니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_1.png)

x은 D × 1 차원을 가진 입력 데이터이며, μ는 x와 같은 차원을 가진 평균 벡터이며, Σ는 D × D 차원을 가진 공분산 행렬입니다.

다변량 가우시안 분포는 아래 중요한 특징을 가지고 있습니다.

- 다변량 가우시안 분포의 주변 분포 또한 가우시안 분포를 따름
- 다변량 가우시안 분포의 조건부 분포 또한 가우시안 분포를 따름

<div class="content-ad"></div>

Now, let's delve into visualizing those ideas. Let's assume our data in D dimensions follows a Gaussian distribution.

For feature 1, if we split the D-dimensional input data into the first L dimensions and the remaining D-L=M dimensions, we can represent the Gaussian distribution like this:

![Gaussian Distribution](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_2.png)

Next, when we integrate out the distribution concerning x₂, we can express the probability distribution of x₁ as:

<div class="content-ad"></div>

이 공식 (1)을 기반으로 할 때, 다른 변수들을 주변화할 때 제외시킬 수 있습니다. 그래프는 이차원 가우시안 분포의 경우를 나타냅니다. 보시다시피, 주변화된 분포는 각 축에 매핑되며, 형태는 가우시안 분포입니다. 직관적으로, 다변량 가우시안 분포를 한 축에 따라 잘라서 단면의 분포는 여전히 가우시안 분포를 따릅니다. 상세한 유도과정은 참고문헌 [2]에서 확인하실 수 있습니다.

특징 2의 경우, 동일한 D차원 다변량 가우시안 분포와 두 구역으로 나눈 입력 데이터 x₁과 x₂를 사용합니다. 다변량 가우시안 분포의 조건부 확률은 다음과 같이 쓸 수 있습니다:

![Formula 2](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_4.png)

<div class="content-ad"></div>

그림에서는 2차원 가우시안 분포(등고선) 및 조건부 가우시안 분포(점선)를 보여줍니다. 가우시안 분포의 모양은 각 조건에서 다릅니다. 자세한 유도 내용은 참조 [2]에서 확인할 수 있습니다.

## 1. 선형 회귀 모형 및 차원의 저주

가우시안 프로세스로 들어가기 전에 선형 회귀 모형과 그 단점, 즉 차원의 저주에 대해 명확히 해보고 싶습니다. 가우시안 프로세스는 이 개념과 밀접한 관련이 있으며 그 단점을 극복할 수 있습니다. 익숙하다면 이 챕터를 건너뛰셔도 됩니다.

선형 회귀 모형을 다시 복습해봅시다. 선형 회귀 모형은 기저 함수 𝝓(x)를 사용하여 데이터를 유연하게 표현할 수 있습니다.

<div class="content-ad"></div>

마법의 타로가 여러분을 초대합니다!

![마법의 타로](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_5.png)

기저 함수로는 다항식 항이나 코사인 함수와 같은 비선형 함수를 사용할 수 있습니다. 따라서 선형 회귀 모델은 x에 비선형 기저 함수를 적용함으로써 비선형 관계를 파악할 수 있습니다. 아래 그래프는 방사 기저 함수를 사용하는 예시를 보여줍니다.

![마법의 타로](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_6.png)

알 수 있듯이 복잡한 데이터 구조를 파악할 수 있습니다. 여전히 매개변수 w의 관점에서는 선형 관계를 유지하기 때문에 이를 선형 회귀 모델이라고 합니다. 매개변수에 비선형 표현이 없는지 확인할 수 있습니다. 점검해보세요!

<div class="content-ad"></div>

이러한 파라미터는 다중 선형 회귀와 동일한 방식으로 유도할 수 있습니다. 다음 방정식들은 선형 회귀 모델의 행렬 및 선형 대수 형태입니다. 우리는 N개의 데이터 포인트와 p+1개의 파라미터를 가정합니다.

![Equation 1](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_7.png)

![Equation 2](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_8.png)

입력 데이터에 기저 함수를 적용한 후 행렬 𝚽의 값들이 상수가 되는 것을 주목하세요. 이것은 다중 선형 회귀와 유사하지 않나요? 실제로, 파라미터의 해석적 미분은 같습니다.

<div class="content-ad"></div>

![MathematicalunderstandingofGaussianProcess_9](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_9.png)

안녕하세요! 선형 회귀 모델은 공식(4)에서 독립 변수가 하나라고 가정하는 함정이 있습니다. 따라서 입력 데이터 차원이 증가할수록 매개 변수의 수가 기하 급수적으로 증가합니다. 기저 함수의 수를 추가하면 모델의 유연성을 높일 수 있지만, 계산량이 현실적이지 않게 증가합니다. 이 문제를 차원의 저주라고 합니다. 계산량을 증가시키지 않으면서 모델의 유연성을 향상시킬 수 있는 방법이 있을까요? 네, 가우시안 프로세스 이론을 적용해 볼 수 있습니다. 다음 섹션으로 넘어가서 가우시안 프로세스에 대해 알아보도록 하겠습니다.

## 3. 가우시안 프로세스의 수학적 배경

모델의 매개 변수 수가 증가할 때 선형 회귀 모델이 직면하는 차원의 저주에 대한 문제를 살펴보았습니다. 이 문제의 해결책은 매개 변수의 기대값을 취하고 매개 변수를 계산할 필요가 없는 상황을 만드는 것입니다. 이것이 무엇을 의미할까요? 선형 회귀 모델의 공식을 기억해보세요.

<div class="content-ad"></div>

![Mathematical understanding of Gaussian Process 10](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_10.png)

Now, if we assume that the parameter w follows a Gaussian distribution, the output y will also follow a Gaussian distribution since the matrix 𝚽 is just a constant value matrix. We assume the parameter follows the Gaussian distribution as shown below:

![Mathematical understanding of Gaussian Process 11](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_11.png)

From this assumption, we can calculate the parameters of the output distribution as follows:

<div class="content-ad"></div>

위에서 볼 수 있듯이, 기대값을 취함으로써 매개변수 계산을 제거할 수 있습니다. 따라서 무한한 매개변수가 있더라도 계산에 영향을 미치지 않습니다. 이 x와 y 사이의 관계는 가우시안 프로세스를 따릅니다. 가우시안 프로세스의 정의는 다음과 같습니다:

![가우시안 프로세스](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_14.png)

<div class="content-ad"></div>

직관적으로 이해하면, 가우시안 프로세스는 무한 개수의 매개변수를 갖는 다변량 가우시안 분포입니다. 여기서 나오는 공식 (7)은 주어진 데이터로부터 가우시안 프로세스의 주변 가우시안 분포를 의미합니다. 주변 다변량 가우시안 분포가 여전히 가우시안 분포를 따른다는 특징에서 비롯됩니다. 따라서 가우스 과정을 잘 활용함으로써 무한 차원의 매개변수를 고려하여 모델을 구축할 수 있습니다.

또 다른 문제는 매트릭스 𝚽를 어떻게 선택해야 하는지입니다. 위의 공식에서 공분산 매트릭스 부분에 집중하고 이를 K로 설정할 때, 각 요소는 다음과 같이 쓸 수 있습니다:

![수식](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_15.png)

공식 (9)에 따르면, 공분산 매트릭스의 각 요소는 𝟇(xᵢ)와 𝟇(xⱼ)의 내적의 곱으로 됩니다. 내적은 코사인 유사성과 유사하므로, 공식 (9)의 값은 xᵢ와 xⱼ가 유사할수록 값이 커집니다.

<div class="content-ad"></div>

특이값 분해의 특성인 대칭이고 양의 정부호인 공분산 행렬을 충족하고 역행렬을 가지려면 𝟇(x)를 적절히 선택해야 합니다. 이를 달성하기 위해 우리는 𝟇(x)에 대해서 커널 함수를 활용합니다.

![image](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_16.png)

커널 함수를 사용하는 장점 중 하나는 커널 함수를 통해 𝟇(x)의 내적을 명시적으로 계산하지 않고 얻을 수 있다는 것입니다. 이 기술을 커널 트릭이라고 합니다. 커널 트릭에 대한 자세한 설명은 [3] 블로그에서 확인할 수 있습니다. 가장 많이 사용되는 커널 함수는 아래와 같습니다.

- 가우시안 커널

<div class="content-ad"></div>


![Linear kernel](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_17.png)

- Linear kernel

![Periodic kernel](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_18.png)

- Periodic kernel


<div class="content-ad"></div>

![이미지](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_19.png)

시각화된 내용에서 각 커널을 사용하여 일차원 가우시안 프로세스를 샘플링하는 것을 보여줍니다. 커널의 특징들을 볼 수 있어요.

이제, 가우시안 프로세스를 다시 살펴봅시다. 커널 함수를 사용하여 정의를 다시 작성할 수 있어요:

![이미지 ](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_20.png)

<div class="content-ad"></div>

When each element of \(f\) follows a Gaussian process, which represents an infinite Gaussian distribution, the joint probability of \(f\) follows a multivariate Gaussian distribution.

Moving forward, we will explore the practical application of the Gaussian process, specifically the Gaussian process regression model.

## 4. Applying the Gaussian Process: Gaussian Process Regression

In this last section, we will demonstrate how the Gaussian process is applied to regression analysis. Stay tuned for the exciting topics ahead!

<div class="content-ad"></div>

- 가우시안 프로세스 모델을 피팅하고 추론하는 방법
- 1차원 데이터를 위한 가우시안 프로세스 모델 예시
- 다차원 데이터를 위한 가우시안 프로세스 모델 예시

## 가우시안 프로세스 모델 피팅과 추론하는 방법

우리에게는 N개의 입력 데이터 x와 해당하는 출력 데이터 y가 있습니다.

![Mathematical understanding of Gaussian Process_21](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_21.png)

<div class="content-ad"></div>

간편함을 위해 전처리를 위해 입력 데이터 x에 정규화를 적용합니다. 이는 x의 평균이 0이라는 것을 의미합니다. x와 y 사이에 다음 관계가 있다고 가정하고, f가 가우시안 프로세스를 따른다고 가정합니다.

![image](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_22.png)

그러므로 출력 y는 다음 다변량 가우시안 분포를 따릅니다.

![image](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_23.png)

<div class="content-ad"></div>

적합성을 위해 커널 함수를 통해 공분산 행렬을 계산한 후, 출력 y 분포의 매개변수는 정확히 하나로 결정됩니다. 따라서 가우시안 프로세스는 커널 함수의 하이퍼파라미터 이외에는 훈련 단계가 없습니다.

추론 단계는 어떻습니까? 가우시안 프로세스는 선형 회귀 모델과 같이 가중치 매개변수가 없기 때문에 새 데이터를 포함하여 다시 적합해야 합니다. 하지만, 다변량 가우스 분포의 특징을 이용하여 계산량을 줄일 수 있습니다.

새 데이터 포인트 m개를 가정해봅시다.

![image](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_24.png)

<div class="content-ad"></div>

In this case, the distribution, including new data points, also follows the Gaussian distribution, so we can describe it as follows:

![Gaussian Process](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_25.png)

Don't forget about formula (2), the parameters of the conditional multivariate Gaussian distribution.

![Gaussian Process Parameters](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_26.png)

<div class="content-ad"></div>

이 공식을 식(11)에 적용하면 매개변수는 다음과 같습니다:

![MathematicalunderstandingofGaussianProcess_27](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_27.png)

이것은 가우시안 프로세스 회귀 모델의 업데이트 공식입니다. 이를 표본이 취하고 싶을 때는 촐레스키 분해에서 유도된 하삼각 행렬을 사용합니다.

![MathematicalunderstandingofGaussianProcess_28](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_28.png)

<div class="content-ad"></div>

[4]에서 디테일한 이론적 설명을 확인할 수 있어요. 하지만 실제 상황에서는 파이썬에 이미 좋은 라이브러리들이 있기 때문에 가우시안 프로세스 회귀를 처음부터 구현할 필요가 없어요.

아래 작은 섹션들에서 우리는 Gpy 라이브러리를 사용하여 가우시안 프로세스를 어떻게 구현하는지 배울 거예요. pip를 통해 쉽게 설치할 수 있어요.

## 예시: 일차원 데이터용 가우시안 프로세스 모델

우리는 가우시안 노이즈와 함께 신 기능에서 생성된 장난감 예제 데이터를 사용할 거예요.

<div class="content-ad"></div>


# 무작위 샘플 생성

X = np.linspace(start=0, stop=10, num=100).reshape(-1, 1)
y = np.squeeze(X * np.sin(X)) + np.random.randn(X.shape[0])
y = y.reshape(-1, 1)


![Image](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_29.png)

우리는 RBF 커널을 사용하는 이유는 매우 유연하고 사용하기 쉽기 때문입니다. GPy 덕분에 몇 줄 만으로 RBF 커널과 가우시안 프로세스 회귀 모델을 선언할 수 있습니다.


# RBF 커널
kernel = GPy.kern.RBF(input_dim=1, variance=1., lengthscale=1.)

# RBF 커널을 사용한 가우시안 프로세스 회귀
m = GPy.models.GPRegression(X, y, kernel)


<div class="content-ad"></div>

![Gaussian Process](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_30.png)

When we look at this image, we can observe the x point, which represents the input data. The blue curve illustrates the expected values of the Gaussian process regression model at that point. The light blue shaded region indicates the 95% confidence interval.

It's interesting to note that the confidence interval is shallower in areas with many data points and broader in areas with only a few points. For engineers in the manufacturing industry, this can offer insights into where more data is needed, and where the data region (or experimental condition) is likely to have target values.

## Example: Using a Gaussian process model for data in multiple dimensions

<div class="content-ad"></div>

스킥런 샘플 데이터셋에서 당뇨병 데이터셋을 사용할 것입니다.

```python
# 당뇨병 데이터셋 불러오고 데이터프레임 생성
diabetes = datasets.load_diabetes()
df = pd.DataFrame(diabetes.data, columns=diabetes.feature_names)
```

해당 데이터셋은 이미 전처리(정규화)되어 있으므로 가우시안 프로세스 회귀 모델을 직접 구현할 수 있습니다. 여러 입력에 대해 같은 클래스를 사용할 수도 있습니다.

```python
# 데이터셋
X = df[['age', 'sex', 'bmi', 'bp']].values
y = df[['target']].values

# 커널 설정
kernel = GPy.kern.RBF(input_dim=4, variance=1.0, lengthscale=1.0, ARD=True)

# GPR 모델 생성
m = GPy.models.GPRegression(X, y, kernel=kernel)
```

<div class="content-ad"></div>

The result is displayed in the table below.

![The result table](/assets/img/2024-07-08-MathematicalunderstandingofGaussianProcess_31.png)

As you can see, there is plenty of room for improvement. The simplest way to enhance the prediction result is by collecting more data. If that's not feasible, you could consider altering the kernel selection or optimizing the hyperparameters. I'll share the code I utilized later, so feel free to modify and experiment with it!

You can recreate the visualizations and GPy experiments using the code provided below.

<div class="content-ad"></div>

우리는 가우스 과정의 수학적 정리와 실제 구현에 대해 이야기했습니다. 이 기술은 데이터가 적을 때 유용합니다. 하지만, 계산 양이 데이터 양에 따라 달라지기 때문에 대용량 데이터에 적합하지 않다는 점을 기억해 주세요. 읽어주셔서 감사합니다!

## 참고 문헌

[1] Mochihashi, D., Oba, S., ガウス過程と機械学習, 講談社

[2]https://www.khoury.northeastern.edu/home/vip/teach/MLcourse/3_generative_models/lecture_notes/Marginal and conditional distributions of multivariate normal distribution.pdf

<div class="content-ad"></div>

There is a fascinating article by Hui in Medium about Kernels for Machine Learning. You can also check out this resource at [rpubs](https://rpubs.com/binhho660/922614). Enjoy exploring these insightful reads!