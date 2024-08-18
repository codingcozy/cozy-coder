---
title: "단일 이미지 깊이 추정의 기초 모델, Depth Anything 사용법"
description: ""
coverImage: "/assets/img/2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_0.png"
date: 2024-07-10 00:23
ogImage:
  url: /assets/img/2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_0.png
tag: Tech
originalTitle: "Depth Anything —A Foundation Model for Monocular Depth Estimation"
link: "https://medium.com/towards-data-science/depth-anything-a-foundation-model-for-monocular-depth-estimation-8a7920b5c9cc"
isUpdated: true
---

## 🚀SASCHA’S PAPER CLUB

단안 깊이 추정, 2D 이미지로부터 3D 공간에서의 거리를 예측합니다. "문제는 부정확하고 본질적으로 모호하다", 깊이 추정에 관한 모든 논문에서 언급되는 바와 같이, 컴퓨터 비전과 로봇학의 기본적인 문제입니다. 동시에, 딥 러닝 기반의 NLP와 컴퓨터 비전에서는 기반 모델이 주류를 이루고 있습니다. 그들의 성공을 깊이 추정에도 활용할 수 있다면 멋지지 않을까요?

오늘의 논문 살펴보기에서는 'Depth Anything' 라는 단안 깊이 추정을 위한 기반 모델에 대해 자세히 살펴보겠습니다. 이 모델의 아키텍처, 훈련에 사용된 요령 및 측정 단위 거리 추정에 사용하는 방법을 알아볼 것입니다.

# 개요

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

# 맥락과 배경

깊이는 왜 중요한 형태이며 그에 대해 딥러닝을 사용하는 이유는 무엇일까요?

![2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_0.png](/assets/img/2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_0.png)

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

간단히 말해서, 3D 공간을 탐색하기 위해서는 모든 물건의 위치와 거리를 알아야 합니다. 고전적인 응용 사례로는 충돌 회피, 주행 가능한 공간 감지, 가상 혹은 증강 현실에서 물체 배치, 3D 물체 생성, 로봇을 조종하여 물체를 집는 것 등이 있습니다.

다양한 측정 방식을 사용하는 여러 종류의 센서로 깊이를 측정할 수 있으며, 적응 및 수동 측정 원리, 그리고 다양한 개수의 센서를 선택할 수 있습니다. 문제는 각 센서마다 장단점이 있지만 결론적으로 정확한 깊이를 측정하는 것은 비용이 많이 듭니다!

그러므로, 비교적 저렴하며 복잡한 설정이 필요하지 않고 많은 시스템에서 이미 사용 중인 센서를 사용하면 좋지 않을까요? 그 센서는 카메라입니다. 단일 카메라를 사용하면 많은 문제를 해결할 수 있지만, 새로운 문제에 직면하게 됩니다. 2D 이미지에서 3D 정보를 예측하는 것은 잘못된 문제로, 깊이를 명백하게 예측하기에 충분한 정보가 없다는 것을 의미합니다. 물체가 작은 것인지 아니면 그냥 멀리 있는 것인지 어떻게 알 수 있을까요? 표면이 오목한지 볼록한지 어떻게 알 수 있을까요? 이것은 딥 러닝을 위한 일감처럼 들립니다, 우리의 일반적인 함수 근사기!

이 문제는 이전에 어떻게 다루어졌을까요?

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

수년 동안 많은 딥러닝 방법이 단일 이미지로부터 깊이를 추정하기 위해 조사되어 왔어요. 어떤 사람들은 깊이 값을 직접 예측하기 위해 회귀 기반 방법을 시도했고, 다른 사람들은 깊이를 이산화시키고 어느 깊이 구간에 픽셀이 속해야 하는지 예측하기 위해 분류를 수행했어요.

생성적 딥러닝의 부상과 함께 많은 연구자들은 GAN, VAE 또는 확산 기반 모델을 사용하기도 했어요, 그리고 저도요 🤓

많은 모델이 유망한 성능을 보였지만, 그들의 사용은 종종 매우 특정한 응용 분야에 국한되었어요: 예를 들어 실내와 실외 장면, 상대적인 깊이 예측 대 메트릭 깊이 예측, 희소 깊이 대 밀도 깊이 등.

사전 훈련된 기본 모델은 훌륭한 0-샷 성능을 보여주었고, 상대적으로 적은 데이터 샘플로 새로운 응용 분야에 쉽게 미세 조정될 수 있어요. 데이터에 굉장히 의존적이기 때문에, 기본 모델은 보통 인터넷에서 사용 가능한 모달리티 - 이미지와 텍스트를 포함하지만 깊이 데이터를 위한 것은 아니었어요. 그런데 이제는!

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

# 방법

깊이가 중요한 이유와 깊이 데이터에 대한 기반 모델을 가지는 것이 바람직하다는 이유에 대해 이야기한 후, Depth Anything의 저자들이 이를 어떻게 달성했는지 살펴봅시다.

## Depth Anything 요약

Depth Anything는 이미지를 입력하고 깊이 맵을 예측하는 오토인코더 모델입니다. 이 모델은 라벨이 달린 이미지와 라벨이 없는 이미지의 조합을 훈련합니다.

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

훈련 과정에는 최대 3 단계가 포함되어 있습니다:

- 레이블이 달린 이미지로 선생님 모델을 훈련합니다.
- 선생님을 사용하여 학생 모델 (Depth Anything 모델)을 사전 훈련하여 레이블이 없는 데이터에 대한 의사 레이블을 만듭니다.
- (선택 사항) 모델을 특정 작업이나 데이터셋에 대해 세밀하게 조정합니다.

조금 더 깊이 파고들면

레이블이 달린 대량의 데이터 수집은 비용이 많이 들고 시간이 많이 소요됩니다. 반면 레이블이 없는 이미지는 대량으로 존재하며 수집하기 쉽습니다. 그러므로 우리는 이 데이터를 심층 학습 모델 훈련에 활용하고자 합니다. 이전 연구에서 일부는 동일한 아이디어를 가지고 레이블이 없는 데이터를 사용하며 고전적인 컴퓨터 비전 알고리즘(예: 스테레오 보정 또는 구조로부터의 움직임)과 결합하여 깊이 레이블을 얻었지만, 이는 상당히 시간이 많이 걸립니다.

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

Depth Anything의 저자들은 1,500,000개의 라벨이 지정된 데이터셋을 사용해 강력한 사전 훈련된 DINOv2 인코더(의미론적으로 풍부한 표현으로 유명한 기본 모델)를 사용해 강력한 교사 모델을 훈련시켰습니다. 그리고 62,000,000개의 라벨이 지정되지 않은 샘플들에 대한 의사 라벨을 생성하고, 이를 사용해 학생을 훈련했습니다.

이 방법으로 학생은 감독 기술을 사용해 훈련되었으며 빠르고 쉽게 밀도 있는 깊이지도를 얻을 수 있었습니다. 그러나 의사 라벨이 완벽하지는 않으며 학생을 훈련시키기 위해서 추가적인 요령이 필요합니다. 학생은 교사와 동일한 아키텍처를 가지고 있지만 교사의 가중치를 사용하지는 않습니다. 학생의 인코더도 DINOv2로 초기화되지만 디코더는 무작위로 초기화됩니다.

특징 정렬에 대한 일부 내용

이전 연구에서는 모델을 추가적인 의미 분할 작업으로 훈련시킬 때 깊이 추정을 향상시킬 수 있다는 것이 밝혀졌습니다. 이에 대한 직관은, 세계에 대해 더 나은 이해(더 의미 있는 특징 임베딩)를 가지면 깊이 추정 작업에서 모호성을 더 잘 해결할 수 있다는 것입니다.

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

실패한 시도 속에서 'Depth Anything'의 저자들은 레이블이 지정되어 있지 않은 데이터에 대한 깊이 추정 작업과 의미 분할 작업을 위해 개별 디코더와 함께 공유 인코더를 교육하려고 시도했습니다. 그 미할당 이미지는 Foundation 모델의 조합을 사용하여 레이블이 지정되었습니다.

결국, 그들은 특징 정렬 제한을 구현했습니다. 학생 모델의 교육 중, 학생의 인코더는 체인 잠긴 DINOv2 인코더(의미론적으로 풍부한 표현으로 유명한 Foundation 모델)와 유사한 특징을 가져야 합니다. 이들의 유사성은 나중에 볼 것처럼 코사인 유사도에 의해 강제됩니다.

그러나 DINOv2는 물체의 하위 부분(예: 자동차의 전면과 후면)에 대해 매우 유사한 특징을 갖고 있기 때문에 깊이 추정 하위 부분은 매우 다른 깊이 값을 가질 수 있습니다. 때문에 이들은 유사성을 강제하는 마진을 도입하지만 여전히 그것에서 이득을 얻을 수 있습니다.

그리고 변조에 대해서는 어떻게 생각하십니까?

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

Adding only unlabeled data to the training pipeline is not enough to significantly boost performance beyond what is achieved with labeled data alone.

To enhance results, the authors introduced strong perturbations to the unlabeled data consumed by the student model. It's important to note that these perturbations are exclusively applied to the student input and not to the teacher or the DINOv2 encoder. Similar strategies have been utilized in other self-supervised frameworks such as DINO and BYOL.

These perturbations involve various augmentation techniques such as color jittering, image noise, and spatial distortions.

The underlying idea remains consistent: encourage the models to recognize similar features regardless of variations in image appearance. In essence, a car should be identified as a car irrespective of whether the image is blurry or horizontally flipped.

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

## 모델 훈련에 사용된 데이터

작가들은 실내와 실외 장면의 다양한 이미지로 구성된 150만 개의 레이블이 달린 이미지와 6200만 장의 레이블이 없는 이미지로 데이터 세트를 구성합니다.

레이블은 밀도가 높은 깊이 맵입니다. 각 픽셀에는 연관된 깊이가 있습니다 (이미지보다 맵이 작을 경우 보간이 필요할 수 있음).

![link](https://miro.medium.com/v2/resize:fit:1400/1*BUXPekBp_YUhrqxDR9c-Jg.gif)

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

이전에 언급했듯이 표시된 데이터는 교사 모델 및 학생 모델을 교육하는 데 사용됩니다. 표시되지 않은 데이터는 학생 모델을 교육하는 데만 사용됩니다.

## 모델을 교육하기 위한 손실 함수

학생 모델의 손실 함수는 다음 세 가지 손실 용어의 평균입니다:

- 학생 예측과 실제 값 사이의 라벨링된 이미지 손실.
- 학생 인코더와 DINOv2 간의 특징 정렬 손실.
- 교사가 표시되지 않은 데이터에서 생성한 가짜 라벨과 학생 예측 간의 라벨링되지 않은 이미지 손실.

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

라벨이 붙은 이미지부터 시작해보겠습니다. 여기서는 학생의 예측과 라벨된 데이터셋의 라벨 간의 간단한 평균 절대 오차, 즉 MAE를 의미합니다.

![MAE](https://miro.medium.com/v2/resize:fit:1256/1*58BkC61wTVK-3N2KED_oqQ.gif)

특성 정렬 손실은 학생 모델의 인코더 출력과 DINOv2 인코더 출력 간의 평균 코사인 유사도로 계산됩니다. 유사도를 패널티로 삼고 싶기 때문에 이 손실을 계산할 때 우리는 코사인 유사도를 1에서 뺍니다. 코사인 유사도는 두 벡터 간의 각도를 측정하며, 거리를 측정하는 것은 아닙니다.

![Feature Alignment Loss](https://miro.medium.com/v2/resize:fit:1400/1*1cfgk5PpXPXfRq_gHnJuTw.gif)

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

마침내 라벨이 붙어 있지 않은 데이터를 살펴보겠습니다. 단일 이미지에 강한 변형을 가했음에도, 저자들은 샘플의 50%에 대해 CutMix 증가를 구현합니다. 이는 결국 마스크를 사용하여 2개의 이미지를 결합하는 것을 의미합니다.

[이미지](https://miro.medium.com/v2/resize:fit:1400/1*9aJT-sLP3GArWeEhYiGpPg.gif)

결합된 이미지는 학생 모델로 보내지고, 개별 이미지는 선생님 모델로 보내져, 의사 라벨을 얻게됩니다. 그런 다음 평균 절대 오차가 계산됩니다. 손실은 마스크 M에 의해 결정된 각 이미지에 대해 한 번씩 계산됨을 유의하세요.

[이미지](https://miro.medium.com/v2/resize:fit:1400/1*Qg-BpVfdYuwGyo6H66pZDQ.gif)

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

마침내 개별 용어가 추가되고 모든 픽셀의 평균을 구합니다. 이미 위의 방정식에서 마스크로 곱셈을 다시 할 필요가 없는 것 같네요.

![image](https://miro.medium.com/v2/resize:fit:1400/1*ereAfjJ7mCClZT7NiuUjtQ.gif)

# 질적 결과

실험과 감소에 뛰어들기 전에 논문의 순서를 변경하고 먼저 일부 질적 결과를 검토해보겠습니다.

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

첫 번째 테스트에서는 모델이 실내와 실외 장면을 포함한 다양한 도메인의 보이지 않는 이미지로 추론되었습니다. 이 때서는 다양한 조명 조건이 포함되었습니다.

![Image 1](/assets/img/2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_1.png)

나아가, MiDaS v3.1과의 성능을 비교하여, 'Depth Anything' 모델이 일반적으로 더 많은 세부 사항을 포착할 뿐만 아니라 장면에 대한 더 나은 일반적인 이해력을 가지고 있어 모호성을 더 잘 해결할 수 있다는 것을 확인할 수 있었습니다.

![Image 2](/assets/img/2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_2.png)

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

최종 질적 테스트에서 ControlNet이라는 확산 기반 생성 모델을 사용하여 예측된 깊이 맵과 입력 프롬프트에 의해 조건화된 새 이미지를 생성했습니다.

![Image](https://miro.medium.com/v2/resize:fit:1400/1*oklLoF-KJMfjuB_hVGYyOg.gif)

# 실험과 미차제

이제 우리는 그들의 방법의 효과를 평가하기 위해 수행된 실험과 미차제에 더 자세히 살펴보겠습니다.

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

## 실험

요약하자면, 저자들은 자신들의 실험에서 보여준 바로는:

- Depth Anything 인코더가 이전 SOTA보다 우수하며 0-샷 상대적 깊이 추정 및 미세 조정된 메트릭 깊이 추정에서도 더 작은 백본으로 성능을 내었다.
- 특징 정렬 제약이 효과적이며, Depth Anything 인코더가 의미론적으로 풍부한 특징을 갖고 있어 세분화 모델로 미세 조정될 수 있다.

제로-샷, 상대적 깊이 추정

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

이 실험에서는 Depth Anything 모델이 저자들이 구성한 데이터셋에서 사전 훈련을 받고, 다른 데이터셋에서도 샘플을 보지 않고(0-shot) 평가됩니다. MiDaS v3.1의 최고 체크포인트와 비교됩니다.

![Image](https://miro.medium.com/v2/resize:fit:1400/1*stzju7r6dxRQrVgCabr2kw.gif)

미세 조정(도메인 내) 측정 깊이 추정

이 실험에서는 상대적 깊이 추정에 대해 사전 훈련 된 Depth Anything 모델이 측정 깊이 추정을 위해 미세 조정됩니다. 인코더-디코더 모델은 사전 훈련 된 Depth Anything 인코더 및 무작위로 초기화 된 디코더로 초기화됩니다. 그런 다음 주어진 데이터셋의 훈련 세트에서 ZoeDepth 프레임 워크를 사용하여 미세 조정되어 마지막으로 해당 데이터셋에서 평가되고 다른 모델과 비교됩니다.

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

![image](https://miro.medium.com/v2/resize:fit:1400/1*azaq7HPhwbHd4VIVUHWf5A.gif)

파인튜닝(제로샷) 메트릭 깊이 추정

여기서는 동일한 미리 학습된 Depth Anything 모델이 메트릭 깊이 추정을 위해 파인튜닝되었으나, 이번에는 테스트된 데이터셋과는 다른 데이터셋에서 평가되었습니다.

ZoeDepth 프레임워크는 두 개의 메트릭 깊이 추정 모델을 파인튜닝하는 데 사용됩니다. 첫 번째 모델은 엔코더로 MiDaS v3.1(표에 ZoeDepth로 표시됨)를 사용하고, 두 번째 모델은 Depth Anything 엔코더를 사용합니다.

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

![image](https://miro.medium.com/v2/resize:fit:1400/1*mlQrGfPDKbONdAQk-TEEVQ.gif)

세밀하게 조정된 의미 분할

이 실험에서 저자들은 Depth Anything 인코더의 의미 능력을 테스트합니다. 훈련 중에 기능 정렬 손실로 제약되어 있었으며 DINOv2 인코더와 유사한 특징을 가지도록 학습되었습니다. 이 DINOv2 인코더는 풍부한 의미 임베딩을 갖는 것으로 알려진 기반 모델입니다.

![image](/assets/img/2024-07-10-DepthAnythingAFoundationModelforMonocularDepthEstimation_3.png)

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

## Ablations

일반 소거 연구

일반적인 소거 연구에서, 저자들은 다양한 손실 용어 및 제약 조건 및 라벨이 없는 데이터에 대한 강력한 교란의 효과를 검증했습니다.

![image](https://miro.medium.com/v2/resize:fit:1400/1*tM87O5U5nYEu5rNXFJmkoQ.gif)

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

다운스트림 작업을 위한 다른 인코더

이 최종 타작 연구에서 저자들은 깊이 추정 및 의미 분할 작업에 맞게 튜닝된 경우 DINOv2와 MiDaS의 인코더보다 자신의 인코더가 우월함을 보여줍니다. 이는 '깊이 여러 가지(Depth Anything)' 인코더의 특성 공간이 더 많은 의미 정보를 포함한다고 제안합니다.

![image](https://miro.medium.com/v2/resize:fit:1400/1*Bmu1K8TRmgJGRlg91lJdkA.gif)

# 결론

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

**깊이 어떤 것(Depth Anything)**은 이미지나 텍스트 이외의 도메인에서의 기반 모델로 나아가는 좋은 한 걸음입니다. 그들은 강력하고 의미론적으로 풍부한 인코더 모델을 성공적으로 훈련시켜 0-샷 모드로 사용하거나 사용자 지정 데이터셋에 대해 더 세밀하게 조정할 수 있습니다.

# 더 많은 자료와 리소스

귀하가 깊이 어떤 것(Depth Anything)과 기반 모델에 대해 더 알고 싶다면 아래 링크들을 참고해주세요.

Hugging Face 데모로 깊이 어떤 것(Depth Anything)를 직접 사용해보세요:

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

종이로 만든 워크스루

다른 기본 모델을 다루는 제 다른 종이 워크스루도 좋아할 거에요:
