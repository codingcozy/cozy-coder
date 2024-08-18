---
title: "YoloV10 아키텍처로 비디오 분석 그래프 시각화로 객체 수 세기, 속도 및 거리 추정 방법"
description: ""
coverImage: "/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_0.png"
date: 2024-07-09 23:30
ogImage:
  url: /assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_0.png
tag: Tech
originalTitle: "Video Analytics: Counting, Speed, Distance Estimation with Graph Visualization , YoloV10 Architecture"
link: "https://medium.com/@VK_Venkatkumar/video-analytics-counting-speed-distance-estimation-with-graph-visualization-yolov10-da1c24f7f245"
isUpdated: true
---

디지털 이미지나 비디오로부터 컴퓨터가 고수준의 이해를 얻는 데 관여하는 컴퓨터 비전은 상호 교차하는 과학 분야입니다. 공학적 관점에서는 인간 시각 시스템이 수행할 수 있는 작업을 이해하고 자동화하려고 합니다.

![image](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_0.png)

요즘에는 작업 분류, 객체 감지, 분할, 그리고 키포인트 검출이 메인 실시간 컴퓨터 비전 응용 프로그램입니다. 그래서, 모두가 이것이 어떻게 발전했는지 어떻게 생각하시나요?

먼저, 컴퓨터 비전의 주요 문제에 대해 간단히 논의하겠습니다.

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

![Image](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_1.png)

당신은 위의 그림을 살펴본 후 컴퓨터 비전 과제와 관련된 모든 주요 용어를 명확하게 이해한 것 같아요.

이것은 내 의견으로는 심각한 문제이지만 모든 것이 문제 설명에 달렸습니다. 그러나 이 어려움은 일반적으로 모든 2D 컴퓨터 비전 엔지니어가 마주치는 것입니다.

6년 전에 이미지 처리로 시작하여 컴퓨터 비전으로 발전했고, 도전에 따라 많은 기초적인 것을 배우고 딥 러닝 기반 모델을 익힌 경험이 있어서 잘 알고 있어요. 예를 들어, resnet과 vgg19, unet, efficientnet 등과 같은 많은 수학 개념을 자세히 다룰 것이고 많은 독자적인 내용을 살펴볼 거에요. 그러나 요즘 많은 학생들과 이른 컴퓨터 비전에 대해 이야기할 때, 그들은 모두 "YOLO"라 말할 뿐이에요. 게다가 대다수 사람들은 YOLO의 백엔드 작동 방식을 알지 못했습니다. YOLO의 백엔드 아키텍처에는 VGG 요소가 포함되어 있습니다. YOLO의 높은 인기와 다른 모델과 비교했을 때 매우 뛰어난 정확성 때문에 그것이 진정 중요한 것입니다.

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

![2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_2.png](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_2.png)

여러분 모두가 원하는 건 이미 준비된 솔루션인데요. 패키지를 설치하고 프로세스가 일어나게 한다음에 백엔드 작업을 이해할 필요 없이 바로 사용할 수 있는 것이겠지요. 해결책이 있습니다. YOLO (Ultralytics)이 바로 그견동물을 세는 알고리즘으로 만들어졌습니다.

![2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_3.png](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_3.png)

저는 Yolov10에 대해 일반적인 것들에 대해 이야기할 예정이며, 비즈니스 관점에서 그래프 시각화를 통해 객체의 수, 속도 및 거리를 추정하는 비디오 분석 데모 프로젝트를 소개할 거에요.

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

YOLO는 무엇인가요?

CNN 기반 객체 탐지기는 주로 추천 시스템에 적합합니다. YOLO (You Only Look Once) 모델은 높은 성능으로 객체를 탐지하는 데 사용됩니다. YOLO는 이미지를 그리드 시스템으로 나누고, 각 그리드는 그 안에 있는 객체를 탐지합니다. 실시간 객체 감지를 위해 데이터 스트림을 기반으로 사용할 수 있습니다. 매우 적은 계산 리소스를 필요로 합니다.

현재 YOLOv10이 출시되었습니다! YOLOV10의 주요 기능은 무엇인가요?

실시간 객체 감지는 최소의 지연을 가지고 이미지 내 객체의 카테고리와 위치를 정확하게 예측하는 것을 목표로 합니다. YOLO 시리즈는 성능과 효율성을 균형 있게 유지하는 데 뛰어나지만, NMS에 대한 의존 및 구조적인 비효율성과 같은 문제점이 남아 있습니다. YOLOv10은 이러한 문제에 대처하기 위해 NMS 없는 훈련 및 효율성 및 정확도에 중점을 둔 모델 디자인 전략을 도입하고 있습니다.

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

**Architecture**

YOLOv10은 이전 YOLO 모델에 몇 가지 혁신을 더했습니다:

- **Backbone**: 더 나은 그래디언트 흐름과 계산의 중복을 줄이기 위해 개선된 CSPNet을 사용합니다.
- **Neck**: 다양한 스케일의 특징을 집적시켜 효과적으로 멀티스케일 특징 융합을 위해 PAN 레이어를 통합합니다.
- **One-to-Many Head**: 훈련 중에 객체당 여러 예측을 생성하여 감독 신호를 풍부하게 만들고 학습 정확도를 향상시킵니다.
- **One-to-One Head**: 추론 중에 객체당 최상의 단일 예측을 출력하여 NMS가 필요 없게 하고 대기 시간을 줄입니다.

**주요 기능**

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

- NMS 없이 적용되는 일관된 이중 할당을 통해 NMS의 필요성을 제거하며 추론 지연 시간을 줄입니다.
- 전체적인 모델 디자인: 경량 분류 헤드, 공간-채널 분리 다운 샘플링, 랭크 안내 블록 디자인 등을 통해 효율성과 정확성을 모두 최적화합니다.
- 향상된 모델 능력: 대커널 컨볼루션 및 부분 자기 주의 모듈을 활용하여 성능을 향상시키고 중요한 계산 비용을 필요로 하지 않습니다.

![image](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_4.png)

❓️ YOLOv10의 특징 및 중요성

YOLOv10은 실시간 물체 감지에 혁신적인 방법론을 소개하여 NMS의 필요성을 없애고 모델 구성 요소를 최적화하여 우수한 성능을 달성합니다. 일관된 이중 할당 및 효율성-정확성 중심의 종합적인 모델 디자인을 활용하여, YOLOv10은 최신의 정확성을 제공하면서 계산 오버헤드를 줄입니다. 그 구조에는 향상된 백본 및 넥 구성 요소뿐만 아니라 혁신적인 one-to-many 및 one-to-one 헤드도 포함되어 있습니다. 서로 다른 응용 프로그램 요구에 맞는 모델 변형을 제공하여, YOLOv10은 정확성과 효율성에서 이전 YOLO 버전 및 다른 동시 대 물체 감지기를 능가하는 새로운 기준을 세웁니다. 예를 들어, YOLOv10-S는 COCO 데이터셋에서 RT-DETR-R18보다 비슷한 AP를 가지고 1.8배 빠르며, YOLOv10-B는 YOLOv9-C와 동일한 성능을 유지하면서 46%의 지연 시간 절감 및 25%의 파라미터 감소를 달성합니다.

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

![Image](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_5.png)

만일 YOLOV8에 대해 상세히 알고 싶다면, 아래 기사를 읽어보세요.

Video Analytics: Counting, Speed, Distance Estimation with Graph Visualization

이 프로젝트에서는 특정 객체의 개수, 속도 및 거리 추정을 즉시 얻을 수 있는 시스템을 개발했습니다. 이 기능은 업체에게 즉각적이고 실행 가능한 통찰력을 제공하여 상당한 혜택을 제공합니다.

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

Ultralytics의 모델인 YOLOv8s와 YOLOv8n을 활용했어요. 이 두 모델은 물체 감지에서 높은 정확성, 효율성, 낮은 대기 시간으로 유명해요. 이러한 모델들은 최종 단계 분석을 간단하게 만들어 전반적인 프로세스를 더욱 원활하고 효과적으로 만들어줘요.

개발 경험을 통해 이러한 솔루션들이 고급 기술을 통해 비즈니스 문제에 대처하는 데 상당한 잠재력을 보여준다는 것을 즐겁게 체험했어요. YoloV8와 YoloV10 모델 역시 좋은 결과를 얻을 수 있지만, YoloV10은 정확성과 대기 시간 면에서 뛰어난 결과를 보여줘요.

메인 함수:

비디오 분석 특정 객체
(물체 수, 속도, 거리 측정..)

원하는 클래스를 입력하세요 (horse: 17, person: 0, car: 2, van: 8, bus: 5, tree: 62)
사용자로부터 원하는 클래스 및 ID를 입력할 수 있는 기능이에요. 예를 들어 "person: 0, car: 2, horse: 17"와 같은 형식으로 입력할 수 있어요.

"horse:17, person: 0, car: 2, van: 8, bus: 5, tree: 62"와 같이 입력할 수 있어요.
원하는 클래스: horse: 17, person: 0, car: 2

총합{total_counts} 을 보여줘요.

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

![image](https://miro.medium.com/v2/resize:fit:1400/1*gHubTKSSjt5jD0bdZMQTaA.gif)

In my humble opinion, this model is absolutely superb for item recognition and can be effortlessly customized to suit specific objects. Even for those not well-versed in AI or deep learning, this software is incredibly user-friendly and widely utilized.

A burning question that frequently emerges is why AI, deep learning, and computer vision engineers are so enthusiastic about developing such ventures. The answer is multifaceted. These projects present valuable opportunities to push the boundaries of the field and tackle real-world issues head-on. Personally, I highly recommend diving into this endeavor without delay. While progress in this realm may be a shared journey for all, there are still a multitude of hurdles to overcome in object identification and segmentation undertakings.

Some of these challenges include:

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

**프로젝트에 중요한 고려 사항:**

1. **조명의 변화:** 광 조건의 변화는 물체 감지의 정확도에 큰 영향을 줄 수 있습니다.
2. **환경:** 다양한 배경 및 설정은 인식 과정을 복잡하게 만들 수 있습니다.

**3. 문제 정의:** 올바른 문제를 식별하고 해결하는 것은 이러한 프로젝트의 성공에 중요합니다.

**4. 현장 작업:** 실제 환경에서의 실현과 테스트는 꼭 필요하지만 관리하기 어려울 수 있습니다.

![이미지](/assets/img/2024-07-09-VideoAnalyticsCountingSpeedDistanceEstimationwithGraphVisualizationYoloV10Architecture_6.png)

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

Only engineers and developers with field knowledge can resolve problems of this nature.

I appreciate you being here, folks. Feel free to drop a comment if you have any questions or spot any errors in this article. Let's also connect on Kaggle Conversation or LinkedIn!

Reference:

- [VK-Ant Github](https://github.com/VK-Ant/Computervision_Exploration)
- [Ultralytics Analytics Guide](https://docs.ultralytics.com/guides/analytics/)
- [Yolov10 Model Design](https://docs.ultralytics.com/models/yolov10/#holistic-efficiency-accuracy-driven-model-design)

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

소셜 네트워크: linkedin, Kaggle, Github
