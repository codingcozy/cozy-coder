---
title: "패션 GAN 최신 AI 기술로 패션 트렌드 생성하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-FashionGAN_0.png"
date: 2024-07-07 23:57
ogImage: 
  url: /assets/img/2024-07-07-FashionGAN_0.png
tag: Tech
originalTitle: "Fashion GAN"
link: "https://medium.com/@ahtashamahsan988/fashion-gan-a96a94e5d8c5"
---


이 글은 Fashion MNIST 데이터셋을 기반으로 이미지를 생성하기 위해 TensorFlow를 사용하여 생성 적 적대 신경망(GAN)을 구현합니다. 데이터 전처리, 생성자 및 구분자 모델 정의, 훈련을 위한 사용자 지정 GAN 클래스 및 생성된 이미지의 시각화가 포함되어 있습니다.

코드를 살펴보겠습니다.

```python
import tensorflow as tf
gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
  tf.config.experimental.set_memory_growth(gpu, True)
```

이 코드 스니펫은 TensorFlow를 사용할 수 있는 GPU를 효율적으로 활용하기 위해 설정합니다. 다음은 이 코드가 단계별로 수행하는 작업입니다:

<div class="content-ad"></div>

1. TensorFlow 라이브러리를 불러오는 코드입니다.

2. 'GPU'로 지정된 물리적 GPU 장치를 모두 나열합니다. 이는 GPU 장치들의 목록을 반환합니다.

3. 각 GPU 장치를 순회하면서 메모리 성장 옵션을 `True`로 설정합니다. TensorFlow가 필요한 만큼만 GPU 메모리를 할당하도록 설정함으로써, 사용 가능한 모든 메모리를 미리 예약하는 것보다 더 효율적으로 GPU를 공유할 수 있으며, 메모리 부족 오류를 미연에 방지할 수 있습니다.

이 구성은 GPU 메모리를 동적으로 관리해야 하는 환경에서 특히 유용합니다.

<div class="content-ad"></div>

## 라이브러리 및 데이터셋 불러오기

텐서플로우로부터 데이터셋을 불러올 거에요.

```python
import tensorflow_datasets as tfds
from matplotlib import pyplot as plt
import numpy as np

ds = tfds.load('fashion_mnist', split='train')
dataiterator = ds.as_numpy_iterator()
```

## 데이터 시각화

<div class="content-ad"></div>

```python
fig, ax = plt.subplots(ncols=4, nrows=4, figsize=(8,8))
index = 0
for x in range(4):
  for y in range(4):
    ax[x,y].imshow(dataiterator.next()['image'])
    ax[x,y].title.set_text(dataiterator.next()['label'])
```

이 코드 스니펫은 서브플롯 그리드를 생성하고 Fashion MNIST 데이터셋에서 이미지를 시각화합니다. 다음은 단계별 설명입니다:

1. fig, ax = plt.subplots(ncols=4, nrows=4, figsize=(8,8)):
   - figure (`fig`) 및 4x4 서브플롯(`ax`)을 생성합니다.
   - ncols=4와 nrows=4는 서브플롯의 열과 행 수를 지정합니다.
   - figsize=(8,8)은 전체 그림 크기를 8x8 인치로 설정합니다.

2. for x in range(4): for y in range(4):
   - 중첩된 루프는 4x4 그리드를 반복합니다. 바깥쪽 루프(`x`)는 행을 반복하고 안쪽 루프(`y`)는 열을 반복합니다.


<div class="content-ad"></div>

3. ax\[x,y\].imshow(dataiterator.next\(\)['image'\]):  
→ `dataiterator`에서 다음 이미지를 가져와서 위치 `(x, y)`에 있는 subplot에 표시합니다.  
→ `dataiterator.next()`은 데이터셋 이터레이터에서 다음 배치를 가져오는데, 이는 'image'와 'label'을 포함한 사전입니다.  
→ `dataiterator.next()['image']`은 가져온 배치에서 이미지를 추출합니다.

4. ax\[x,y\].title.set_text\(dataiterator.next\(\)['label'\]):  
→ 위치 `(x, y)`에 있는 subplot의 제목을 해당 이미지의 레이블로 설정합니다.  
→ `dataiterator.next()['label']`은 가져온 배치에서 레이블을 추출합니다.

\[이미지\]\(https://yourwebsite.com/assets/img/2024-07-07-FashionGAN_0.png\)

```python
def scale_image(data):
  image = data['image']
  return image/255

ds = ds.map(scale_image)
ds = ds.cache()
ds = ds.shuffle(60000)
ds = ds.batch(128)
ds = ds.prefetch(64)
```

<div class="content-ad"></div>

이러한 변환은 데이터셋을 스케일링하고 캐싱하고 셔플링하며 배치 처리하고 데이터를 미리 가져오는 등의 작업을 수행하여 모델을 효율적이고 효과적으로 학습할 수 있도록 전처리합니다.

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Dense, Flatten, Reshape, LeakyReLU, Dropout, UpSampling2D

def build_generator():
  model = Sequential()
  model.add(Dense(7*7*128, input_dim=128))
  model.add(LeakyReLU(0.2))
  model.add(Reshape((7,7,128)))

  model.add(UpSampling2D())
  model.add(Conv2D(128, 5, padding='same'))
  model.add(LeakyReLU(0.2))

  model.add(UpSampling2D())
  model.add(Conv2D(128, 5, padding='same'))
  model.add(LeakyReLU(0.2))

  model.add(Conv2D(128, 5, padding='same'))
  model.add(LeakyReLU(0.2))

  model.add(Conv2D(128, 5, padding='same'))
  model.add(LeakyReLU(0.2))

  model.add(Conv2D(1, 4, activation='sigmoid', padding='same'))

  return model

generator = build_generator()
generator.summary()
```

## 1. 입력 레이어:

- Dense(7*7*128, input_dim=128): 생성기는 128차원의 잡음 벡터(input_dim=128)를 입력으로 받는 완전 연결(Dense) 레이어로 시작합니다. 이 레이어는 (7*7*128,) 모양의 텐서를 출력하며, 그 후에 재구성됩니다.

<div class="content-ad"></div>

## 2. 형태 변환 레이어:

- Reshape((7,7,128)): 밀집 레이어의 출력을 (7, 7, 128) 모양의 3D 텐서로 변형합니다. 이렇게 하면 후속 컨볼루션 레이어를 위해 데이터가 준비됩니다.

## 3. 업샘플링 및 컨볼루션 레이어:

- UpSampling2D(): 이웃 점 최근 이웃 업샘플링을 사용하여 데이터의 공간 차원(너비와 높이)을 증가시킵니다.
- Conv2D(128, 5, padding=`same`): 128개의 5x5 필터를 사용한 컨볼루션 레이어로, `same` 패딩을 사용하여 공간 차원을 유지합니다.
- LeakyReLU(0.2): 각 컨볼루션 레이어 후에 기울기가 0.2인 Leaky ReLU 활성화 함수를 적용하여 비선형성을 도입합니다.

<div class="content-ad"></div>

이러한 레이어(UpSampling2D 다음에 LeakyReLU를 사용한 Conv2D)가 두 번 반복되며, 점진적으로 공간 해상도와 피처 깊이가 증가합니다.

## 4. 최종 합성곱 레이어:

- Conv2D(1, 4, activation=`sigmoid`, padding=`same`): 최종 합성곱 레이어는 채널이 1개인 이미지를 출력합니다 (output_dim=1 이므로) 4x4 커널 크기와 시그모이드 활성화 함수를 사용합니다. 이 레이어는 픽셀 값이 [0, 1] 범위에 있는 회색조 강도를 나타내는 출력 이미지를 생성합니다.

# 생성자 모델 요약:

<div class="content-ad"></div>

**generator.summary()** 함수의 출력은 생성자 모델의 구조를 요약합니다.

## Summary에서 주요 포인트:

- 레이어 유형 및 매개변수: 각 레이어 유형(Dense, Reshape, UpSampling2D, Conv2D, LeakyReLU)은 출력 형태 및 학습 가능한 매개변수 수와 함께 나열됩니다.
- 출력 형태: 마지막 Conv2D 레이어는 (28, 28, 1) 형태의 이미지를 출력하며, 이는 크기가 28x28인 단일 채널(그레이스케일) 이미지에 해당합니다.
- 총 매개변수: 모델은 총 2,450,049개의 매개변수를 가지며, 이는 훈련 중에 학습됩니다.

<div class="content-ad"></div>

```python
def build_discriminator():
  model = Sequential()
  model.add(Conv2D(32, 5, input_shape=(28,28,1)))
  model.add(LeakyReLU(0.2))
  model.add(Dropout(0.4))

  model.add(Conv2D(64, 5))
  model.add(LeakyReLU(0.2))
  model.add(Dropout(0.4))

  model.add(Conv2D(128, 5))
  model.add(LeakyReLU(0.2))
  model.add(Dropout(0.4))

  model.add(Conv2D(256, 5))
  model.add(LeakyReLU(0.2))
  model.add(Dropout(0.4))

  model.add(Flatten())
  model.add(Dropout(0.4))
  model.add(Dense(1, activation='sigmoid'))

  return model

discriminator = build_discriminator()
discriminator.summary()
```

The discriminator model is designed to classify whether an input image is real (from the Fashion MNIST dataset) or generated by the generator model. Its architecture includes convolutional layers followed by dropout for regularization and ends with a dense layer for binary classification. This setup is crucial for the adversarial training process in GANs, where the generator learns to generate increasingly realistic images based on the feedback from the discriminator.

```python
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.losses import BinaryCrossentropy

g_opt = Adam(learning_rate=0.0001)
d_opt = Adam(learning_rate=0.00001)

g_loss = BinaryCrossentropy()
d_loss = BinaryCrossentropy()
```

- Optimizers: Adam optimizers are chosen for both the generator and discriminator due to their effectiveness in training deep neural networks.
- Learning Rates: Typically, the generator’s learning rate is higher than the discriminator’s to facilitate more rapid learning in the generator, while the discriminator’s learning rate is lower to stabilize its training.
- Loss Functions: Binary crossentropy is well-suited for GANs as it measures the difference between probability distributions of real and generated images, guiding both the generator and discriminator towards their respective objectives.

<div class="content-ad"></div>

이러한 구성 요소들은 GAN의 교육 과정에서 중요한 역할을 합니다. 여기서 생성자(generator)와 식별자(discriminator)가 적대적으로 교육되며 서로의 성능을 지속적으로 향상시킵니다. 균형이 달성될 때까지 생성자가 현실적인 이미지를 생성하고 식별자가 진짜 이미지와 구별하지 못하는 수준에 이를 때까지 계속 발전합니다.

```python
class FashionGAN(Model):
  def __init__(self, generator, discriminator, *args, **kwargs):
    super().__init__(*args, **kwargs)
    self.generator = generator
    self.discriminator = discriminator

  def compile(self, g_opt, d_opt, g_loss, d_loss, *args, **kwargs):
    super().compile(*args, **kwargs)
    self.g_opt = g_opt
    self.d_opt = d_opt
    self.g_loss = g_loss
    self.d_loss = d_loss

  def train_step(self, batch):
    real_images = batch
    fake_images = self.generator(tf.random.normal((128, 128, 1)), training=False)

    with tf.GradientTape() as d_tape:
      yhat_real = self.discriminator(real_images, training=True)
      yhat_fake = self.discriminator(fake_images, training=True)
      yhat_realfake = tf.concat([yhat_real, yhat_fake], axis=0)

      y_realfake = tf.concat([tf.zeros_like(yhat_real), tf.ones_like(yhat_fake)], axis=0)

      noise_real = 0.15*tf.random.uniform(tf.shape(yhat_real))
      noise_fake = -0.15*tf.random.uniform(tf.shape(yhat_fake))
      y_realfake = tf.concat([noise_real, noise_fake], axis=0)

      total_d_loss = self.d_loss(y_realfake, yhat_realfake)

    dgrad = d_tape.gradient(total_d_loss, self.discriminator.trainable_variables)
    self.d_opt.apply_gradients(zip(dgrad, self.discriminator.trainable_variables))

    with tf.GradientTape() as g_tape:
      gen_images = self.generator(tf.random.normal((128, 128, 1)), training=True)
      predicted_labels = self.discriminator(gen_images, training=False)
      total_g_loss = self.g_loss(tf.zeros_like(predicted_labels), predicted_labels)

    ggrad = g_tape.gradient(total_g_loss, self.generator.trainable_variables)
    self.g_opt.apply_gradients(zip(ggrad, self.generator.trainable_variables))

    return {'d_loss': total_d_loss, 'g_loss': total_g_loss}
```

- 생성자 훈련: 생성자(self.generator)는 g_loss를 최소화하여 식별자(self.discriminator)가 진짜로 분류하는 이미지를 생성하도록 장려합니다.
- 식별자 훈련: 식별자(self.discriminator)는 실제와 가짜 이미지를 구별하도록 훈련되며 d_loss를 최소화합니다.
- 노이즈 추가: 식별자의 레이블인 y_realfake에 노이즈를 추가하여 GAN 교육 과정의 견고성을 향상시킵니다.
- 최적화: Adam 최적화기(self.g_opt는 생성자용, self.d_opt는 식별자용)가 계산된 그래디언트를 기반으로 각 모델 가중치를 업데이트하는 데 사용됩니다.

```python
import os
from keras.preprocessing.image import array_to_img
from tensorflow.keras.callbacks import Callback

class GANMonitor(Callback):
  def __init__(self, num_img=3, latent_dim=128):
    self.num_img = num_img
    self.latent_dim = latent_dim

  def on_epoch_end(self, epoch, logs=None):
    random_latent_vectors = tf.random.normal(shape=(self.num_img, self.latent_dim, 1))
    generated_images = self.model.generator(random_latent_vectors)
    generated_images *= 255
    generated_images.numpy()

    for i in range(self.num_img):
      img = array_to_img(generated_images[i])
      img.save(os.path.join(f'generated_img_{epoch}_{i}.png'))

hist = fashgan.fit(ds, epochs=50, callbacks=[GANMonitor()]) #2000 recommended
```

<div class="content-ad"></div>

`GANMonitor` 클래스는 GAN 훈련 중 각 에포크 끝에 이미지를 생성하고 저장하는 사용자 지정 Keras callback입니다. 이는 생성기 모델을 사용하여 무작위 잠재 벡터에서 지정된 수의 이미지를 생성하고 이러한 이미지를 디스크에 저장합니다. 이를 통해 훈련 에포크에 걸쳐 생성기의 진행 상황을 시각적으로 모니터링할 수 있습니다.

```js
plt.suptitle('Loss')
plt.plot(hist.history['d_loss'], label='d_loss')
plt.plot(hist.history['g_loss'], label='g_loss')
plt.legend()
plt.show()
```

![Loss Plot](/assets/img/2024-07-07-FashionGAN_1.png)

이 그래프는 각 에포크별 감별자 손실(d_loss)과 생성기 손실(g_loss)을 보여줍니다. 이 그래프가 나타내는 바는 다음과 같습니다:

<div class="content-ad"></div>

디스크리미네이터 손실 (d_loss):

- 디스크리미네이터 손실은 대략 -0.5 주변의 값 주변에서 변동합니다.
- 이것은 디스크리미네이터가 어느 정도 실제와 가짜 이미지를 올바르게 식별하고 있음을 나타냅니다. 그러나 음의 손실 값은 손실 함수를 조정해야 할 수도 있다는 것을 시사합니다. 일반적으로 손실 값은 양수여야 합니다.

생성기 손실 (g_loss):

- 생성기 손실이 제로에 일정하게 유지됩니다.
- 이는 예상치 못한 행동입니다. 일반적으로 생성기 손실은 에포크별로 변경되어야 하며, 생성기가 디스크리미네이터를 속이기 위해 현실적인 이미지를 생성할 능력을 향상시킬 때 변해야 합니다. 제로의 일정한 손실은 손실 계산, 옵티마이저 또는 손실 업데이트 방법에 문제가 있을 수 있음을 시사합니다.

<div class="content-ad"></div>

다음은 문제를 진단하고 해결할 때 확인할 수 있는 몇 가지 사항입니다:
- 손실 계산: 손실 함수가 올바르게 정의되고 적용되는지 확인하세요. train_step 메서드에서 total_d_loss 및 total_g_loss 계산을 검토하세요.
- 옵티마이저 적용: 그레이디언트가 올바르게 계산되고 모델 가중치에 적용되는지 확인하세요. apply_gradients 호출을 확인하세요.
- 손실 계산을 위한 레이블: 손실을 계산할 때 사용되는 레이블이 올바른지 확인하세요. 예를 들어, 생성자의 손실은 가짜 이미지에 대한 판별자의 출력을 실제로 분류할 목표 레이블 1과 비교해야 합니다 (생성자는 판별자가 실제로로 분류할 이미지를 생성하기 위해 노력합니다).
- 학습률: 옵티마이저의 학습률을 검토하세요. 때로는 너무 높거나 너무 낮은 학습률은 수렴하지 않을 수 있습니다.
- 모델 구조: 생성자와 판별자 모델의 아키텍처가 올바른지 확인하고 레이어 구성에 문제가 없는지 확인하세요.

아래는 생성자와 판별자를 저장하는 코드입니다:


generator.save('generatormodel.h5')
discriminator.save('discriminatormodel.h5')


최종적으로, 생성자와 판별자를 저장하세요.

<div class="content-ad"></div>

풀 코드는 Github에서 확인하실 수 있습니다.

GANs에 대해 더 많은 정보 보기

감사합니다