---
title: "부분최소제곱법PLS으로 다변량 분석의 격차 메우기"
description: ""
coverImage: "/assets/img/2024-07-12-PartialLeastSquaresBridgingtheGapinMultivariateAnalysis_0.png"
date: 2024-07-12 23:28
ogImage:
  url: /assets/img/2024-07-12-PartialLeastSquaresBridgingtheGapinMultivariateAnalysis_0.png
tag: Tech
originalTitle: "Partial Least Squares: Bridging the Gap in Multivariate Analysis"
link: "https://medium.com/ai-mind-labs/partial-least-squares-bridging-the-gap-in-multivariate-analysis-5376df6baaa0"
isUpdated: true
---

## 소개

통계 분석의 복잡한 세계에서는 기술의 발전으로 데이터의 탐구와 해석이 점점 더 복잡해지는데, 부분 최소 제곱법(PLS)이 중요한 방법론으로 떠오르고 있습니다. 이 에세이는 PLS의 본질에 대해 탐구하며, 다변량 분석 영역에서 제공하는 독특한 장점과 의의, 그리고 응용 분야를 명료하게 다루고 있습니다.

![PLS Image](/assets/img/2024-07-12-PartialLeastSquaresBridgingtheGapinMultivariateAnalysis_0.png)

## PLS의 탄생과 발전

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

20세기 후반에 탄생한 PLS(부분 최소 제곱)는 초기에 전통적인 통계적 방법이 높은 상관성 데이터나 예측 변수가 관측치보다 많은 시나리오와 같은 제약에 부딪힐 때 개발되었습니다. 이 방법은 차원 축소와 잠재 변수 모델링에 대한 뛰어난 접근 방식을 통해 초기 응용을 초월하며, 화학측정학, 감각분석, 금융 및 사회과학 분야를 포함한 다양한 학문 분야에서 유용성을 발견했습니다.

## PLS의 방법론적 핵심

PLS의 핵심은 예측 변수 집합(X)과 하나 이상의 반응 변수(Y) 사이의 관계를 잠재 변수를 추출하여 모델링하는 것입니다. 이 잠재 변수는 예측 변수의 선형 결합으로, 응답을 예측하는 데 필요한 최대의 관련 정보를 포착하기 위해 설계됩니다. 이 과정은 고차원 데이터의 복잡성을 단순화뿐만 아니라, 예측 변수의 가장 정보가 풍부한 측면에 초점을 맞춤으로써 모델의 예측 정확도를 향상시킵니다.

## 차원 축소: PLS의 핵심

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

PLS의 특징 중 하나는 차원 축소에 대한 강력한 접근 방식이다. 주성분 분석(PCA)이 예측자 행렬에만 주로 초점을 맞춘 반면, PLS는 잠재 변수를 최적화하여 예측자 및 응답 행렬의 분산을 설명한다. 이 이중 포커스는 추출된 구성 요소가 데이터 구조를 대표하는 것뿐만 아니라 결과 변수를 매우 예측하기에도 효과적인지를 보장한다.

## 응용 분야: 다재다능함을 증명하는 유형

PLS의 다재다능함은 다양한 응용 분야에서 나타난다. 화학 측정학에서 PLS는 분광 데이터를 분석하는 데 필수적이며, 화학자들이 스펙트럼 데이터를 바탕으로 화학적 특성을 효율적으로 예측할 수 있게 도와준다. 마케팅 분야에서는 PLS가 소비자 행동과 제품 특성간의 관계를 모델링하여 복잡한 소비자 선호도를 해독하는 데 도움을 준다. 게다가 유전체학 분야에서 PLS는 유전자 표지자와 형질 특성 간의 관계를 이해하는 데 기여하여 매우 복잡하고 다차원 데이터셋을 처리할 수 있는 능력을 보여준다.

## 전통적인 방법에 대한 장점

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

PLS(부분최소제곱)는 기존의 다변량 방법에 비해 여러 가지 장점을 제공합니다. 다변량 예측변수들 간에 높은 상관관계가 있을 때에도 사전 변수 선택을 필요로 하지 않고 다룰 수 있는 능력은 많은 상황에서 선호되는 선택지로 만들었습니다. 더불어, 예측변수의 수가 관측값의 수를 초과하는 경우에도 강건성을 유지하여 대규모 데이터셋이 특징인 현대 데이터 분석에서 상당한 이점을 제공합니다.

## 도전과 고려사항

그러나 강점이 있음에도 불구하고, PLS에도 도전 과제가 있습니다. 잠재변수의 해석은 원래 변수들에 비해 보다 직관적이지 않을 수 있으며, 신중한 고려와 도메인 지식이 필요합니다. 또한 추출할 잠재변수의 수를 선택하는 것은 중요한 결정으로 남아 있으며, 종종 교차 검증 기법을 활용하여 모형의 성능을 최적화하고 오버피팅을 방지합니다.

## Code

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

파이썬에서 합성 데이터셋을 사용하여 Partial Least Squares (PLS) 회귀를 설명하기 위해, numpy 라이브러리를 사용하여 데이터를 생성하고 scikit-learn 라이브러리로 PLS 모델을 적용할 것입니다. 이 예제에서는 합성 데이터셋을 만들고 PLS 회귀를 적용하며, 결과를 matplotlib로 시각화하는 과정을 안내하겠습니다.

```js
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cross_decomposition import PLSRegression
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.model_selection import train_test_split

np.random.seed(0)  # 재현성을 위한 시드 설정
n = 1000  # 샘플 수
p = 10    # 특성 수

# 예측 변수 (X) 생성
X = np.random.normal(0, 1, (n, p))
# 일부 노이즈를 추가한 반응 변수 (Y) 생성
Y = X[:, 0] + 2 * X[:, 1] + 0.5 * np.random.normal(0, 1, n) + 5

# 데이터셋을 훈련 세트와 테스트 세트로 분할
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=1)

# PLS 회귀 모델 정의
pls = PLSRegression(n_components=2)
# 모델 적합
pls.fit(X_train, Y_train)
# 테스트 세트에 대해 예측
Y_pred = pls.predict(X_test)

mse = mean_squared_error(Y_test, Y_pred)
r2 = r2_score(Y_test, Y_pred)

print(f'평균 제곱 오차: {mse:.2f}')
print(f'R^2 점수: {r2:.2f}')

plt.scatter(Y_test, Y_pred, edgecolors=(0, 0, 0))
plt.plot([Y_test.min(), Y_test.max()], [Y_test.min(), Y_test.max()], 'k--', lw=4)
plt.xlabel('실제 값')
plt.ylabel('예측 값')
plt.title('PLS 회귀: 실제 vs 예측')
plt.show()
```

![PLS Regression](/assets/img/2024-07-12-PartialLeastSquaresBridgingtheGapinMultivariateAnalysis_1.png)

```js
평균 제곱 오차: 0.22
R^2 점수: 0.95
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

이 완벽한 코드 스니펫은 파이썬에서 합성 데이터셋을 사용한 PLS 회귀의 기본적인 사용법을 보여줍니다. PLS 모델의 구성 요소(n_components)를 조정하여 모델의 성능에 어떤 영향을 미치는지 확인할 수 있습니다. 시각화를 통해 모델이 만들어낸 예측의 정확성을 이해하는 데 도움이 됩니다.

## 결론

부분 최소 제곱 회귀는 데이터의 점점 더 커지는 복잡성에 대응하기 위해 통계적 방법론이 발전한 증거로 남아 있습니다. 전통적인 회귀 기술과 현대 데이터 분석의 요구 사항 사이의 간극을 메워 주는 PLS는 다재다능하고 강력한 도구로 자리매김했습니다. 복잡한 데이터셋 내의 잠재 구조를 발견하는 능력과 다양한 학문 분야에 적용 가능한 특성은 통계 도구 상자 속에서의 중요성을 강조합니다. 데이터 중심 시대로 더 나아가면서 PLS의 원리와 적용은 의심할 여지없이 확장되어 현명하고 데이터 기반 의사결정으로 이끌어주는 길을 밝힐 것입니다.

## AI Mind로부터의 메시지

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

![Tarot](https://miro.medium.com/v2/resize:fit:500/0*5Wm7sOfTpe5DEbhg.gif)

우리 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 👏 이야기에 박수를 보내고 저자를 팔로우하세요 👉
- 📰 AI Mind 출판물에서 더 많은 컨텐츠를 확인해 보세요
- 🧠 AI 프롬프트를 쉽고 무료로 개선해보세요
- 🧰 직관적인 AI 도구를 발견하세요
