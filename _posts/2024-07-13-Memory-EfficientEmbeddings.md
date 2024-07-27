---
title: "메모리를 절약하는 임베딩 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-13-Memory-EfficientEmbeddings_0.png"
date: 2024-07-13 23:00
ogImage: 
  url: /assets/img/2024-07-13-Memory-EfficientEmbeddings_0.png
tag: Tech
originalTitle: "Memory-Efficient Embeddings"
link: "https://medium.com/towards-data-science/memory-efficient-embeddings-d637cba7f006"
---


![Memory-EfficientEmbeddings_0](/assets/img/2024-07-13-Memory-EfficientEmbeddings_0.png)

When it comes to dealing with categorical data, many beginners opt for one-hot encoding. While this method works fine in most cases, handling thousands or even millions of categories can make it impractical. Here's why:

- Increased Dimensionality: Each category adds a new feature, leading to higher dimensionality. This can result in the curse of dimensionality, making the data sparse and potentially causing computational complexity issues and reduced model performance.
- Loss of Semantics: One-hot encoding treats each category as a separate feature, overlooking any semantic relationships between them. This means losing valuable insights present in the original categorical variable.

These challenges often arise in natural language processing tasks, where we deal with numerous words, or in recommendation systems, where there are many customers and articles. To address this, embeddings can be a great solution. However, having many embeddings can significantly increase the memory requirements of your model, sometimes reaching several gigabytes.

<div class="content-ad"></div>

이 글에서는 이 메모리 풋프린트를 줄이는 몇 가지 방법을 소개하고 싶어요. 이 중 하나는 Shi 등의 흥미로운 논문 `Compositional Embeddings Using Complementary Partitions for Memory-Efficient Recommendation Systems` 에서 나온 방법입니다. 우리는 또한 이러한 방법이 등급 예측 작업에서 어떻게 성과를 내는지 실험해 볼 거에요.

# 임베딩

간략히 말하면, 우리는 긴 희소 벡터 대신 길이가 d인 조밀한 벡터, 즉 우리의 임베딩을 원해요. 임베딩 차원 d는 우리가 자유롭게 선택할 수 있는 하이퍼파라미터에요.

임베딩은 짧고 카테고리의 의미를 어느 정도 포착해야 하는 실수 벡터에요. 실수는 훈련을 통해 배우게 돼요. TensorFlow에서의 구체적인 구현은 제 다른 글을 확인해주세요:

<div class="content-ad"></div>

## 모형 크기

임베딩은 훌륭하며, 한 번 사용하면 다시 돌아가고 싶지 않을 정도입니다. 그러나 많은 카테고리가 있다면 각각에 대해 많은 실수를 저장해야 합니다. 6개 카테고리에 대한 차원이 4인 작은 임베딩 테이블을 생각해 볼 수 있습니다:

![Embedding Table](/assets/img/2024-07-13-Memory-EfficientEmbeddings_1.png)

일반적으로 N 개의 카테고리와 차원 d에 대해 테이블 크기는 N ⋅ d이며, 이것이 메모리 요구 사항의 근원입니다.

<div class="content-ad"></div>

이 표의 크기는 매우 쉽게 커질 수 있습니다: 제가 하는 일 중 하나는 100만 명 이상의 고객에게 서비스하는 임베딩 기반 추천 시스템을 만들었습니다. 추천 시스템의 크기는 종종 700 MB 정도이며, 이는 모델을 배포하기 어렵게 만들었습니다. 예를 들어, BigQuery에서 모델의 최대 크기가 450 MB로 제한되어 있다는 것을 알 수 있습니다. 그래서, 이 모델의 크기를 대략 반으로 줄일 수 있는 방법을 살펴보겠습니다!

# 임베딩 테이블 크기를 줄이는 간단한 팁

이 표를 좀 더 작게 만드는 여러 가지 방법이 있지만, 보통은 어떤 적대환경이 따릅니다:

그래도, 우리는 메모리와 작업 성능 사이의 최적의 균형을 찾아볼 수 있습니다 - 어쩌면 정확도가 93%인 10 GB 모델보다 91% 정확도를 달성하는 50 MB 모델이 더 나을지도 모릅니다. 이를 어떻게 달성할 수 있는지 살펴보겠습니다.

<div class="content-ad"></div>

## 차원 축소

가장 간단한 메모리 축소 방법은 차원 d를 축소하는 것입니다.

![Memory EfficientEmbeddings](/assets/img/2024-07-13-Memory-EfficientEmbeddings_2.png)

이 방법은 잘 동작할 수 있지만, 차원이 너무 낮아지면 모델이 더 이상 표현력이 부족해질 수 있습니다. 예를 들어 차원을 d = 1로 줄인다고 상상해보세요. 수백만 개의 범주와 작업하고 있다면 이 경우 많은 정보를 잃을 수 있습니다. 그럼에도 몇몇 사람들은 이 극단적 압축을 target encoding이라는 이름으로 하는 것을 좋아합니다.

<div class="content-ad"></div>

## 해시 트릭

다른 방법은 범주 N의 수를 감소시키는 것입니다. 예를 들어, 매우 유사하다고 생각되는 여러 범주를 함께 그룹화할 수 있습니다. 이 작업은 많은 노력이 필요할 수 있기 때문에 사람들은 종종 해시 함수를 사용하여 무작위 그룹화를 찾습니다.

![Memory Efficient Embeddings](/assets/img/2024-07-13-Memory-EfficientEmbeddings_3.png)

두 경우 모두 다른 범주가 동일한 임베딩을 받을 수 있습니다. 이로 인해 성질이 근본적으로 다른 범주를 혼동할 수 있어서 좋은 임베딩을 학습하기가 더 어려울 수 있습니다.

<div class="content-ad"></div>

![Memory-EfficientEmbeddings](/assets/img/2024-07-13-Memory-EfficientEmbeddings_4.png)

## 부동 소수점 정밀도 낮추기

지금까지 놓친 한 가지는 부동 소수점 자체의 크기입니다. Float32 대신에 Embeddings를 float16로 다운샘플링하거나, 심지어 float8로도 낮출 수 있습니다. 테이블 크기는 동일하지만 셀당 비트 수가 줄어듭니다. Float16을 사용하면 50%의 축소율을, Float8을 사용하면 75%의 축소율을 얻을 수 있습니다.

모든 float32를 단순히 float16으로 캐스팅 하는 것은 권장되지 않습니다. 훈련 중에 NaN의 발생을 유발할 수 있어 훈련 불안정성을 초래할 수 있습니다.

<div class="content-ad"></div>

# 구성 별은

이제 Shi 등이 쓴 논문 "Compositional Embeddings Using Complementary Partitions for Memory-Efficient Recommendation Systems"로 넘어가 봅시다. 그들의 논문에서는 해싱 기법보다 나은 메모리 저장 방법을 설명하고 있습니다.

이 작동 방식에 대해 들어가기 전에, 우리가 평소 임베딩을 어떻게 사용하는지 간단히 복습해 봅시다. N개의 카테고리가 0부터 N-1까지 번호가 매겨져 있다고 가정해 봅시다. 카테고리 i의 임베딩을 검색하려면, 행렬의 i번째 행을 찾아 해당 벡터를 임베딩으로 출력합니다. 끝.  

## 아이디어

<div class="content-ad"></div>

그들은 자신들의 방법의 여러 다른 버전을 제시하지만, 그들의 이른바 나눗셈-나머지 트릭부터 시작해봅시다. 여기서, 그들은 차원 d의 포함을 가진 두 개의 작은 표를 만듭니다. 한 표가 크기 M이고 다른 하나가 크기 N / M이라고 가정해 봅시다. 숫자 예시로, N = 100 개의 카테고리가 있다고 가정하고 M = 20으로 설정합시다. 그러면 크기가 20인 포함 표 하나와 크기가 5인 포함 표 하나가 생깁니다.

![Memory-EfficientEmbeddings_5](/assets/img/2024-07-13-Memory-EfficientEmbeddings_5.png)

우리의 N 개 카테고리가 0부터 N-1까지 번호가 매겨져 있다고 다시 가정하고, 카테고리 i에 대한 포함을 얻고 싶다고 합시다. 이 아이디어는 이 카테고리에 대한 두 개의 합성 포함을 찾아보고 두 개에서 최종 포함을 조합하는 것입니다 - 각 표에서 하나씩 - 예를 들어 이들을 구성 요소별로 더하거나 곱해 최종 포함을 만든다는 것입니다. 저자들은 이들을 곱하는 것을 제안합니다.

따라서, 우리는 두 개의 훨씬 작은 표를 사용하여 포함을 만들었습니다. 총합으로는 M⋅d + (N/M)⋅d = (M + N/M)⋅d 개의 숫자를 저장해야 하므로, 이는 항상 N⋅d보다 작게 됩니다.

<div class="content-ad"></div>

## Tarot Reading for Their Algorithm

안녕하세요! 이 텍스트를 한국어로 번역해드릴게요. 타로 전문가로서 유저 님에게 친철하게 알려드릴게요.

## 그들의 알고리즘

작은 테이블에서 어떤 임베딩을 찾아야 하는지 어떻게 알 수 있을까요? 그건 나머지와 함께 나눗셈을 하면 알 수 있어요! 어떤 숫자 i의 임베딩이 필요하다고 가정해보죠. 여러분이 학창 시절을 기억한다면, 우리는 i = q ⋅ M + r 형태로 나타낼 수 있다는 것을 알겠네요. 여기서 q는 몫(quotient)이고, r은 나머지(remainder)를 나타냅니다.

예를 들어, i = 77인 경우를 생각해봅시다. 77을 3 ⋅ 20 + 17로 나타낼 수 있고, 몫이 3이고 나머지가 17인 경우, 작은 테이블에서 복합 임베딩을 찾을 인덱스입니다.

일반적으로, 다음과 같은 알고리즘을 얻게 됩니다:

<div class="content-ad"></div>

![Memory-EfficientEmbeddings_6](/assets/img/2024-07-13-Memory-EfficientEmbeddings_6.png)

j = 57에 대해 어떻게 생각하시나요? 문제 없어요, 2⋅20 + 17. 여기서 i와 j 두 카테고리가 동일한 나머지 임베딩 17을 공유하고 있음을 볼 수 있는데, 이는 i = 77과 j = 57을 연관시킵니다. 그러나 그들의 몫 임베딩은 다르며, 따라서 최종 임베딩도 다릅니다.

## 감소 이득

i를 M으로 나눈 몫과 나머지를 쉽게 구할 수 있어요. i // M 및 i % M입니다. 저는 이 아이디어를 좋아해요. 이해하고 구현하기 쉽기 때문이죠. 이 방법을 사용하여 메모리를 최소화하려면 M + N / M을 M에 대해 최소화해야 해요. M이 정수임을 고려하지 않는다면, f의 도함수를 취할 수 있어요. 그리고 f(M) = 1 - N / M² 이 되며, f'(M) = 0을 풀면 M = √N 가 됩니다. 이 경우에는 두 테이블의 길이가 거의 동일한데요.

<div class="content-ad"></div>

이것은 이전의 N⋅d 값 대신 약 √N⋅d만 저장하면 된다는 것을 의미합니다. 이것은 엄청난 이득입니다. 다시 숫자를 사용하고 싶다면, 100⋅d 값을 2⋅√100⋅d = 20⋅d 값으로 낮출 수 있습니다. 즉, d와 무관하게 80% 감소입니다. 그리고 이것이 많다고 생각한다면, N에 대해 큰 값들을 대입해보세요. 일반적으로 embedding을 사용할 때는 N = 10,000의 경우 이미 98%의 감소량이 있습니다!

그러나 이 수준의 압축은 뚜렷하게 성능이 나빠질 수 있다는 것을 염두에 두세요. 기억 공간을 최적화하는 것이 항상 최선의 결정은 아닙니다. 모델 성능적인 측면에서, 납득할만한 분할인 M ≠ √N이 더 나을 수 있습니다.

## 일반화

이 아이디어는 쉽게 일반화할 수 있습니다. 주어진 인덱스 i에 대해, 그것을 어떻게든 두 개 더 작은 수로 분할하세요. 그리고 왜 두 개만 하는 거죠? 여러 개로도 분할할 수 있습니다. 예를 들어 k로 말이죠. 중국인의 나머지 정리를 활용하는 한 가지 방법이 있습니다. 이것은 말하는 것보다 복잡하게 들릴 수 있지만, 실제로는 간단합니다. 기본적으로, M₁, M₂, …, Mₙ(모듈라라고 불리는)이라고 부르는 수들을 선택하십시오.

<div class="content-ad"></div>

각각의 두 수가 공통 약수가 없으며,
두 수의 곱이 N보다 커야 합니다.

이는 두 가지 다른 범주 i와 j가 나눗셈-나머지 경우와 마찬가지로 다른 합성 임베딩을 얻을 수 있도록 하는 기술적 요구사항입니다.

그럼, 단순히 i % M₁, i % M₂, …, i % Mₙ을 계산하고 여기에는 n개의 색인이 있습니다. 이를 n개의 크기가 각각 M₁, M₂, …, Mₙ인 작은 테이블에서 찾아볼 수 있습니다. 메모리 요구 사항은 (M₁ + M₂ + … + Mₙ) ⋅ d가 되며, 모듈러가 모두 ⁿ√N의 크기 정도인 경우에 n ⋅ ⁿ√N ⋅ d로 매우 낮을 수 있습니다. 숫자 예로 들면, N = 100이고 n = 5를 갖는 경우, 이 경우 약 87%의 감소가 일어납니다. N = 10,000이고 n = 5인 경우, 메모리 요구 사항은 약 99.7% 감소합니다.

다시 한 번, 이것이 많지 않은지 주의 깊게 살펴보십시오! 아마도 숫자를 다르게 선택해야 할 것이며, 이는 감소를 줄이지만 모델의 성능을 향상시킬 수 있습니다.

<div class="content-ad"></div>

# 텐서플로우에서의 구현

텐서플로우에서 이 논리를 구현하기 전에 모두 동일한 페이지에서 시작합시다. 텐서플로우에서 임베딩 레이어를 사용하는 방법을 간단히 되짚어 봅시다:

```python
import tensorflow as tf

N = 100
d = 64

embedding_layer = tf.keras.layers.Embedding(N, d)
X = tf.constant([1, 2, 3])

print("출력 형태:", embedding_layer(X).shape)
print("임베딩 테이블 형태:", embedding_layer.get_weights()[0].shape)

# 출력:
# 출력 형태: (3, 64)
# 임베딩 테이블 형태: (100, 64)
```

## 나누기-나머지 레이어

<div class="content-ad"></div>

지금까지 쉬웠죠? 이제, 몫-나머지 트릭을 구현해 봅시다. 아래 코드는 간단하게 이해되어야 합니다. X는 많은 정수 숫자가 들어 있는 텐서(배열)입니다. 호출 메서드에서는 몫과 나머지를 계산하고, compositional tables인 quotient_embedding과 remainder_embedding에서 해당 값들을 찾습니다.

```js
class DivisionCompositionEmbedding(tf.keras.layers.Layer):
    def __init__(self, input_dim, output_dim, divisor, **embedding_layer_kwargs):
        super().__init__()
        self.input_dim = input_dim
        self.output_dim = output_dim
        self.divisor = divisor
        
        self.quotient_embedding = tf.keras.layers.Embedding(input_dim // divisor, output_dim, **embedding_layer_kwargs)
        self.remainder_embedding = tf.keras.layers.Embedding(divisor, output_dim, **embedding_layer_kwargs)
        
    def call(self, X):
        quotient_embedding = self.quotient_embedding(X // self.divisor)
        remainder_embedding = self.remainder_embedding(X % self.divisor)
        
        return quotient_embedding * remainder_embedding
```

이 레이어를 다른 레이어처럼 사용할 수 있습니다.

```js
N = 100
d = 64
M = 20

embedding_layer = DivisionCompositionEmbedding(N, d, M)
X = tf.constant([1, 2, 3])
print("Output shape:", embedding_layer(X).shape)
print("First embedding table shape:", embedding_layer.get_weights()[0].shape)
print("Second embedding table shape:", embedding_layer.get_weights()[1].shape)

# 결과:
# Output shape: (3, 64)
# First embedding table shape: (5, 64)
# Second embedding table shape: (20, 64)
```

<div class="content-ad"></div>

## 중국인의 나머지 계층

여기서 좀 쉽네요. 이렇게 보세요:

```js
class ChineseRemainderCompositionEmbedding(tf.keras.layers.Layer):
    def __init__(self, input_dim, output_dim, moduli, **embedding_layer_kwargs):
        super().__init__()
        self.input_dim = input_dim
        self.output_dim = output_dim
        self.moduli = moduli

        self.embeddings = [tf.keras.layers.Embedding(modulus, output_dim, **embedding_layer_kwargs) for modulus in moduli]
        self.multiply = tf.keras.layers.Multiply()

    def call(self, X):
        embeddings = [embedding(X % modulus) for embedding, modulus in zip(self.embeddings, self.moduli)]

        return self.multiply(embeddings)
```

사용법을 다시 한 번 확인해봅시다:

<div class="content-ad"></div>

```python
N = 100
d = 64
Ms = (7, 11, 13)

embedding_layer = ChineseRemainderCompositionEmbedding(N, d, Ms)
X = tf.constant([1, 2, 3])
print("Output shape:", embedding_layer(X).shape)
print("First embedding table shape:", embedding_layer.get_weights()[0].shape)
print("Second embedding table shape:", embedding_layer.get_weights()[1].shape)
print("Third embedding table shape:", embedding_layer.get_weights()[2].shape)

# Output:
# Output shape: (3, 64)
# First embedding table shape: (7, 64)
# Second embedding table shape: (11, 64)
# Third embedding table shape: (13, 64)
```

## 성능 결과

저자들은 클릭 스루유율 예측을 위해 Criteo Ad Kaggle Competition 데이터 세트를 사용하여 모델을 테스트했습니다. 이 데이터 세트에는 13개의 밀집 특징과 26개의 범주형 특징이 포함되어 있습니다. 저자들은 DCM과 DLRM 두 가지 다른 신경망 아키텍처를 사용하여 방법을 테스트했습니다. 이들 중 한 가지 결과는 다음과 같습니다:

![Memory-Efficient Embeddings](/assets/img/2024-07-13-Memory-EfficientEmbeddings_7.png)


<div class="content-ad"></div>

해싱 트릭과 몫 나머지 트릭을 적용하면 훨씬 작은 모델이 생성되는 것을 확인할 수 있지만, 몫 나먘지 트릭의 압축 방법이 어떤 식으로든 더 나은 것 같습니다. 이 특정 데이터셋과 모델에 대해서는 해싱 트릭의 임베딩이 몫 나먘지 트릭의 임베딩보다 모델의 성능을 약간 더 나빠지게 합니다. 논문에 더 많은 그래프가 있으니 꼭 확인해보세요!

안타깝게도, 저자들은 새로운 임베딩 레이어를 간단한 방법(차원 축소 또는 정밀도 감소)과 비교하는 것을 잊어버렸습니다. 예를 들어 d를 80% 줄이는 경우에 무슨 일이 일어나는지 확인하는 것이 흥미로웠을 것 같습니다.

## 나의 실험

저도 MovieLens 20M 데이터셋을 사용해 간단한 추천 시스템을 만들어보았어요. 이 데이터셋은 사용자들이 영화에 매긴 별점(1부터 5까지)으로 이루어진 영화 평점 데이터가 가득합니다. regression 모델을 구축하여

<div class="content-ad"></div>

- 사용자를 임베드하고,
- 영화를 임베드하며,
- 이러한 임베딩을 연결하고,
- 선형 레이어를 사용하여 단일 숫자를 출력합니다.

다음은 코드입니다:

```js
import tensorflow as tf


class Model(tf.keras.Model):
    def __init__(self, user_model, movie_model):
        super().__init__()
        self.user_model = user_model
        self.movie_model = movie_model
        self.concatenate = tf.keras.layers.Concatenate()
        self.linear = tf.keras.layers.Dense(1, activation="sigmoid")

    def call(self, X):
        concat = self.concatenate([self.user_model(X[:, 0]), self.movie_model(X[:, 1])])
        return 4.5 * self.linear(concat) + 0.5
```

모델에 임베딩 레이어를 전달하여 일반적인 (평범한) 임베딩 레이어를 시도할 수 있지만, 해싱, 몫과 나머지, 그리고 중국인의 나머지 트릭도 사용해볼 수 있습니다.

<div class="content-ad"></div>

이렇게하면 Polars를 사용하여 데이터를 로드할 수 있어요.

```python
# !pip install polars

import polars as pl

TRAIN_SET_SIZE = 19_000_000
EMBEDDING_DIM = 8
BATCH_SIZE = 1024

ratings = (
    pl.scan_csv(
        "ml-20m/ratings.csv",
    )
    .with_columns(pl.col("userId").rank(method="dense").alias("userId_encoded"), pl.col("movieId").rank(method="dense").alias("movieId_encoded"))
    .collect()
    .sample(fraction=1.0, shuffle=True, seed=0)
)

train = ratings.head(TRAIN_SET_SIZE)
evaluation = ratings.tail(-TRAIN_SET_SIZE)

X_train = train.select(["userId_encoded", "movieId_encoded"]).cast(pl.Int32).to_numpy()
y_train = train.select(["rating"]).to_numpy()
X_evaluation = evaluation.select(["userId_encoded", "movieId_encoded"]).cast(pl.Int32).to_numpy()
y_evaluation = evaluation.select(["rating"]).to_numpy()

train_ds = tf.data.Dataset.from_tensor_slices((X_train, y_train)).batch(BATCH_SIZE).cache().prefetch(tf.data.AUTOTUNE)
evaluation_ds = tf.data.Dataset.from_tensor_slices((X_evaluation, y_evaluation)).batch(BATCH_SIZE).cache().prefetch(tf.data.AUTOTUNE)
```

8개의 임베딩 차원을 사용했기 때문에 성능을 더 낮추기가 어려우니 참고해 주세요. 최대 메모리 절약률이 87.5%에 달하는 1차원으로 계속 감소시킬 수도 있어요.

작은 학습 함수를 만들었어요.

<div class="content-ad"></div>

```python
def train(user_model, movie_model):
    model = Model(user_model, movie_model)
    model.compile(loss="mse", optimizer="adam")
    model.fit(train_ds, epochs=3, validation_data=evaluation_ds, callbacks=[tf.keras.callbacks.EarlyStopping()])
    print(model.summary())

    return model
```

My results:

![Memory-EfficientEmbeddings_8](/assets/img/2024-07-13-Memory-EfficientEmbeddings_8.png)

I could replicate that the authors’ proposed methods are definitely better than the simple hashing trick. But the biggest surprise was that even reducing the dimension to 1 yields a model that is on par with the original model. We got an 87% reduction for free, in terms of model performance and code changes. Wow.

<div class="content-ad"></div>

간단한 차원 축소가 저자들에게도 도움이 되었을지는 모르겠지만, 그들의 방법과 비교해 볼만한 흥미로운 결과가 있을 것이라고 생각해요. 저는 그들에게 메시지를 보내었고, 답변을 받으면 기사를 업데이트할 거에요.

참고: float32 대신 float16을 사용해 봤는데, 언급한 숫자적 문제에 부딪혔고, 학습이 완료되지 못했어요.

# 결론 및 토론

이 기사에서는 임베딩이 범주형 데이터를 인코딩하는데 유용한 도구임을 확인했어요. 그러나 이러한 임베딩은 대규모 모델을 야기할 수 있어서, 임베딩 테이블 크기를 줄일 수 있는 방법을 알고 있는 것이 좋아요.

<div class="content-ad"></div>

먼저, 우리는 몇 가지 쉬운 방법들을 논의했어요.

- 테이블의 너비 줄이기 (d 축소)
- 테이블의 높이 줄이기 (N 축소, 해싱 트릭)
- 셀 메모리 줄이기 (소수 부동소수점 정밀도 감소)

그 다음으로 Shi 등의 새로운 아이디어를 알려드렸어요. 하나의 큰 테이블을 가지는 대신 여러 개의 작은 테이블을 가지고 그들로부터 우리가 원하는 결과물을 조립해내보자는 거예요. 이것은 기본적으로 다른 것이에요.

- 카테고리마다 임베딩을 가지는 대신에
- 임의로 카테고리를 섞어서 동일한 임베딩을 제공하는 것(hashing)

<div class="content-ad"></div>

카테고리의 접근 방식 중 하나는 여러 조합 임베딩을 가질 수 있고, 예를 들어 최종 임베딩을 얻기 위해 곱할 수 있습니다. 이러한 조합 임베딩 각각은 여러 카테고리에서 사용되는데, 이는 해싱 트릭과 유사합니다. 그래도 저자들이 만든 방법은 서로 다른 두 카테고리에 대한 최종 임베딩이 동일하지 않도록 보장합니다. 이는 전체 해싱 테이블에서 하는 것과 유사합니다.

오늘 새롭고 흥미로운 가치 있는 것을 배워주셨기를 바랍니다. 읽어 주셔서 감사합니다!

알고리즘의 세계로 더 깊이 파고들고 싶다면, 새로운 저의 출간물 '모든 것 알고리즘'을 살펴보세요! 아직도 작가를 찾고 있습니다!