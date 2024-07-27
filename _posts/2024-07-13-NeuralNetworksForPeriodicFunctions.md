---
title: "주기 함수에 대한 신경망 활용 방법"
description: ""
coverImage: "/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_0.png"
date: 2024-07-13 22:32
ogImage: 
  url: /assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_0.png
tag: Tech
originalTitle: "Neural Networks For Periodic Functions"
link: "https://medium.com/towards-data-science/neural-networks-for-periodic-functions-648cfc940437"
---


이미지 태그를 Markdown 형식으로 변경해주세요.

Neural networks are known to be great approximators for any function — at least whenever we don’t move too far away from our dataset. Let us see what that means. Here is some data:

![Data Example](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_1.png)

It does not only look like a sine wave, it actually is, with some noise added. We can now train a normal feed-forward neural network having 1 hidden layer with 1000 neurons and ReLU activation. We get the following fit:

<div class="content-ad"></div>

![Neural Networks For Periodic Functions 2](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_2.png)

Well, the model appears to be quite decent, except for some issues at the edges. One way to address this could be by increasing the number of neurons in the hidden layer, following Cybenko's universal approximation theorem. However, I'd like to draw your attention to something else:

![Neural Networks For Periodic Functions 3](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_3.png)

Now, we might argue that the extrapolation behavior is problematic when assuming the wave pattern extends beyond the observed range. Without additional domain knowledge or more data, it would simply remain an assumption.

<div class="content-ad"></div>

하지만, 이 글의 나머지 부분에서는 데이터 내에서 우리가 포착할 수 있는 주기적인 패턴이 외부에서도 계속된다고 가정할 것입니다. 시간 시리즈 모델링을 할 때 이것은 흔한 가정입니다. 우리는 자연스럽게 앞으로 추정하고 싶기 때문입니다. 훈련 데이터에서 관측된 계절성이 계속되리라고 가정합니다. 추가 정보 없이 무슨 말을 할 수 있겠습니까? 이 글에서는 삼각함수 기반의 활성화 함수를 사용하는 방법이 모델에 이 가정을 내포하는 데 어떻게 도움이 되는지 보여드리고자 합니다.

하지만 그 전에, 우리는 ReLU 기반의 신경망이 일반적으로 외삽하는 방식과 왜 우리가 시계열 예측에 그대로 사용하면 안 되는지에 대해 간단히 더 깊게 파헤쳐보겠습니다.

## 신경망의 외삽

우리는 알았듯이 ReLU 네트워크의 출력은 데이터셋에서 더 멀어질수록 선으로 변합니다. 이겢은 김지인 등이 Neural Networks Fail to Learn Periodic Functions and How to Fix It 논문에서 보여준 것과 같은 일반적인 패턴입니다. 그들은 증명했습니다:

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_4.png)

To make it simpler, you can interpret it as f(z·u)≈z·W·u+b for large z. This means that in a ReLU neural network, if you input extremely large values - far from 0 - the network will act like a linear function.

In the case where input and output are one-dimensional, like in our example, if we set u=1, we get f(z)≈W·z+b for large z, where W is a 1×1 matrix, essentially a real number. By setting u=-1, we get f(-z)≈W’·z+b for large z and some other constant, W’. This correlates with what we observed at the boundaries.

## Sigmoid- or tanh-based networks

<div class="content-ad"></div>

Bounded activation functions like sigmoid or tanh have a similar effect:

The authors describe it as follows:

![Neural Networks for Periodic Functions](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_5.png)

This indicates that our model acts like a constant function f(z·u) ≈ v when moving away from 0, as opposed to a more versatile linear function in the ReLU case.

<div class="content-ad"></div>

이 모든 것은 흥미롭지만 주기적인 행동을 모델링할 수있는 활성화 함수에 대해 이야기해 봅시다.

### 사인 기반 활성화 함수

여기서부터 진행할 수 있는 여러 가지 방법이 있습니다. 가장 쉬운 것은 활성화 함수로 sin(x)를 사용하여 결과를 확인하는 것입니다. Ziyin 등은 더 고심하고 그들이 이를 제안하는 '스네이크 함수'라 불리는 Snake(x) = x + sin²(x)를 사용할 것을 제안합니다. 또는 파라미터화하려면 Snakeₐ(x) = x + sin²(ax) / a를 사용할 수도 있습니다.

### 순수한 사인

<div class="content-ad"></div>

한번 숨을 쉬어보세요! 이렇게 타로의 마법을 사용하여 숨을 들이마시고 내쉬게 될 것입니다. 오직 사인 함수를 활성화 함수로 사용하는 숨겨진 노트 하나만으로 모델을 구축해보려고 합니다. TensorFlow를 사용하여 구현할 거지만, 실제로는 어떤 딥러닝 프레임워크를 사용해도 따라 할 수 있을 만큽입니다.

```js
import tensorflow as tf

model = tf.keras.Sequential([
    tf.keras.layers.Dense(1, activation=tf.math.sin),
    tf.keras.layers.Dense(1)
])

model.compile(
    loss="mse",
    optimizer=tf.keras.optimizers.AdamW() # AdamW는 Adam의 개선 버전입니다. 논문은 여기에: https://arxiv.org/abs/1711.05101
) 
```

이제 모델을 훈련해볼 수 있습니다:

```js
X = tf.random.uniform((100000, 1), minval=-6, maxval=6)
y = tf.math.sin(X) + 0.1 * np.random.randn(100000, 1)

model.fit(
    X,
    y,
    validation_split=0.1,
    epochs=100,
    callbacks=[
        tf.keras.callbacks.EarlyStopping(patience=2, restore_best_weights=True), # validation mse가 두 에포크 동안 개선되지 않으면 멈춥니다.
        tf.keras.callbacks.ReduceLROnPlateau(patience=0) # validation mse가 개선되지 않으면 학습률을 낮춥니다.
    ]
)
```

<div class="content-ad"></div>

우리가 받은 예측은 다음과 같아요:

![Neural Networks For Periodic Functions](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_6.png)

시계열 예측을 할 때 이런 것을 보는 건 참 좋아요! 왼쪽(과거)이 중요하지 않은 경우도 많지만, 거기서 합리적인 추정을 무료로 얻을 수 있다면 더 좋겠죠.

조심해야 할 점이 있어요. 너무 많은 숨겨진 노드를 넣지 않아야 해요. 그렇지 않으면 모델이 데이터에 없는 계절성을 잡아내게 될 수도 있어요. 과하게 적용해서 오버핏팅을 일으킬 수 있거든요. 100개의 숨겨진 노드를 사용했을 때 어떤 일이 벌어지는지 한 번 봐봐요:

<div class="content-ad"></div>

![Neural Networks For Periodic Functions](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_7.png)

어느 단기 계절성 이외에도, 모델은 학습 시점에서 하강하는 큰 계절성을 감지합니다. 그러나 우리의 데이터에 대해서는 이는 사실이 아닙니다.

## 뱀 함수

현재 단순한 데이터에 대해 뱀 함수를 발명한 것이 아닙니다. 이 함수는 모델에 추세를 부과하기 때문에 우리의 데이터와는 잘 맞지 않을 것입니다.

<div class="content-ad"></div>

```python
def snake(x):
    return tf.math.sin(x)**2 + x

model = tf.keras.Sequential([
    tf.keras.layers.Dense(5, activation=snake),
    tf.keras.layers.Dense(1)
])
```

이 모델을 훈련시키면 이와 같은 결과가 나올 것입니다:

![Neural Networks For Periodic Functions](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_8.png)

갑자기 나타나는 상승 추세를 볼 수 있습니다. 하지만 이는 뱀 함수가 설계된 방식, 즉 "+ x" 항의 결과입니다. 그러나 추세가 있는 함수나 시계열의 경우 뱀 함수가 빛을 발할 수도 있습니다.

<div class="content-ad"></div>

저자들은 보편적인 외삽정리를 제시합니다:

![](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_9.png)

여기서 사용하는 것은 순수한 사인과 코사인파가 아닌 스네이크 함수이기 때문에 푸리에 근사 정리와 유사하지만 같지는 않습니다.

# 추세가 있는 시계열

<div class="content-ad"></div>

이제 시계열 예측을 하고자 합니다. 여러 계절성을 가지고 있는 경우도 있습니다. 시간당, 매달, 매년 또는 쉽게 설명할 수 없는 주기가 포함될 수 있습니다. 그리고 추세가 있을 수 있습니다.

이 문제를 해결하기 위한 고전적인 통계적 방법이 많이 있습니다. SARIMA나 Holt-Winters로도 알려진 삼중 지수 평활 방법과 같이요. 뉴럴 네트워크 쪽에서는 순환 신경망, 컨볼루션, 또는 트랜스포머를 사용할 수 있습니다.

그러나 저는 이러한 방법들의 세부사항에 대해서는 다루지 않고 주요 목표에 집중할 것입니다: 간단한 삼각함수를 기반으로 하는 피드포워드 신경망을 구축하는 것이죠. 이러한 방식은 앞서 언급한 많은 방법들과는 달리 재귀식을 다루지 않아도 된다는 장점이 있습니다. 일단 훈련할 데이터를 만들어봅시다.

<div class="content-ad"></div>


X = tf.cast(tf.range(50)[:, tf.newaxis], tf.float32)
y = 3*tf.math.sin(X) + 2*tf.math.sin(2*X) - tf.math.sin(3*X) + X + 0.1 * tf.random.normal((50, 1))

The data looks like this:

![Image](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_10.png)

## Additive trend + seasonality model


<div class="content-ad"></div>

이렇게 Keras에서 구현할 수 있어요:

```python
class AdditiveTrendAndSeasonalityModel(tf.keras.Model):
    def __init__(self, trend_model, seasonality_model):
        super().__init__()
        self.trend_model = trend_model
        self.seasonality_model = seasonality_model
        
    def call(self, x):
        return self.trend_model(x) + self.seasonality_model(x)
    
trend_model = tf.keras.Sequential([
    tf.keras.layers.Dense(5, activation="relu"),
    tf.keras.layers.Dense(1)
])

seasonality_model = tf.keras.Sequential([
    tf.keras.layers.Dense(5, activation=tf.math.sin),
    tf.keras.layers.Dense(1, use_bias=False)
])

model = AdditiveTrendAndSeasonalityModel(trend_model, seasonality_model)
```

이제 모델을 컴파일하고 학습시킬 수 있어요.

```python
model.compile(
    loss="mse",
    optimizer=tf.keras.optimizers.AdamW(learning_rate=0.0005)
)

model.fit(
    X,
    y,
    epochs=3000,
    callbacks=[tf.keras.callbacks.EarlyStopping(monitor="loss", patience=5)]
)
```

<div class="content-ad"></div>

우리 모델을 이제 외삽에 사용할 수 있어요.

![Image](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_11.png)

저희 모델의 외삽 행동이 멋져 보여요! 추세는 좋고 선형적이에요. 하지만 계절성 패턴은 좀 더 정확해져야 할 거 같아요. 모델이 원래 맞는 패턴을 학습할 기회가 있었는데도 여전히 너무 단순한 패턴을 제시했어요. 사인 활성화 노드 5개를 줬어도 3개만 있어도 충분했었죠.

또한 사인 기반 신경망에서 초기화가 매우 중요하다는 것을 발견했어요. 때로는 위 이미지처럼 좋고 빠른 수렴을 얻을 수 있어요. 그러나 때로는 매우 나쁜 국부 최솟값에 갇히기도 해요. 좀 운에 맡긴다고 할까요.

<div class="content-ad"></div>

그러나 여전히 우리의 모델은 trend_model과 seasonality_model로 구성되어 있기 때문에 시계열을 트렌드와 계절성 부분으로 쉽게 분해할 수 있어요:

![이미지](/assets/img/2024-07-13-NeuralNetworksForPeriodicFunctions_12.png)

멋지네요! 그리고 만약에 곱셈 트렌드를 원한다면, 모델 클래스에서 +를 *로 바꿔주기만 하면 돼요.

# 결론

<div class="content-ad"></div>

이 기사에서는 신경망의 추정 능력이 활성화 함수에 따라 결정된다는 사실을 많은 사람들이 인식하지 못한다는 점을 보았습니다.

또한 시계열 모델링을 위해 추세 및 계절성 부분으로 구성된 간단한 신경망을 만드는 방법을 살펴보았습니다. 이 두 부분을 더한 후 우리에게 멋진 분해를 제공합니다. 시계열을 다루고 있기 때문에 추정 능력은 중요합니다. 따라서 어떤 활성화 함수를 선택하는지 신중해야 합니다. 저희 네트워크의 계절성 부분에서는 주기적인 신경망 부분을 만들기 위해 사인 활성화 함수를 사용했습니다.

Ziyin 등이 제안한 뱀 함수를 사용할 수도 있지만, 그러면 시계열을 추세와 계절성 구성 요소로 분해할 수 있는 능력이 상실됩니다. 하지만, 이게 중요하지 않다면 시도해 볼 가치가 있습니다. 대신 예측 성능이 중요하다면요.

오늘 새롭고 흥미로운, 가치 있는 것을 배우셨으면 좋겠습니다. 읽어 주셔서 감사합니다!

<div class="content-ad"></div>

그리고 알고리즘의 세계에 더 깊이 파고들고 싶다면, 새로운 저작물 '모든 것이 알고리즘'을 읽어보세요! 아직 글쓰기를 원하는 작가들을 찾고 있어요!