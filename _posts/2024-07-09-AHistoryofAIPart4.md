---
title: "AI의 역사 파트 4 2024 최신 동향과 미래 전망"
description: ""
coverImage: "/assets/img/2024-07-09-AHistoryofAIPart4_0.png"
date: 2024-07-09 10:18
ogImage:
  url: /assets/img/2024-07-09-AHistoryofAIPart4_0.png
tag: Tech
originalTitle: "A History of AI (Part 4)"
link: "https://medium.com/on-technology/a-history-of-ai-part-4-a68590f1ecd2"
isUpdated: true
---

## 인공지능 연구 논문

안녕하세요! 이 글은 인공지능의 역사를 살펴보기 위해 그 분야에서 가장 중요한 연구 논문들을 리뷰하는 일련의 글 중 네 번째로 준비했습니다.

#AIHistory #DeepLearning #MachineLearning #AIPapers #TechInnovation

# Batch Normalization

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

**Batch Normalization: Deep Network Training Acceleration through Internal Covariate Shift Reduction**

In 2015, Sergey Ioffe and Christian Szegedy published a groundbreaking research paper entitled "Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift."

This study unveiled a novel approach to enhancing the speed and effectiveness of training deep learning models, commonly utilized in tasks such as image recognition. The researchers discovered that by normalizing the input data to each layer of the model during the training process, they could considerably expedite and optimize the training process.

The introduction of Batch Normalization not only accelerated the training time but also enhanced the accuracy of the models, sometimes eliminating the necessity for additional techniques like Dropout. The positive outcomes obtained through this method surpassed expectations, showcasing results that even outperformed human accuracy in certain scenarios.

This research paper made a significant impact by introducing Batch Normalization, a methodology that greatly augmented the efficiency and performance of training deep neural networks. By addressing a key issue in deep learning known as internal covariate shift, this technique enabled faster training speeds, higher learning rates, and improved overall accuracy. This groundbreaking innovation simplified the process of training deep learning models and contributed significantly to the rapid progress in AI applications.

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

- 깊은 신경망: 인공 지능 모델의 한 유형으로, 사진 및 음성 인식과 같은 작업에 사용되며 인간 두뇌의 신경망을 모방합니다.
- 내부 공변량 변화: 훈련 중에 신경망 각 층에 입력의 분포가 변화하는 현상으로, 학습 과정을 느리게 만들 수 있습니다.
- 정규화: 서로 다른 척도에서 측정된 값을 공통 척도로 조정하는 과정으로, 안정화시키고 학습 속도를 높이는데 도움이 됩니다.
- 미니배치: 각 훈련 반복에서 모델 파라미터를 업데이트하는 데 사용되는 훈련 데이터의 작은 하위 집합입니다.
- 학습률: 훈련 중 손실 기울기에 따라 모델 파라미터를 얼마나 조정할지 제어하는 매개변수입니다.
- 드롭아웃: 훈련 중에 임의로 선택된 뉴런을 무시하여 과적합을 방지하는 정규화 기법입니다.
- 상위 5 테스트 오류: 분류 작업에서 사용되는 측정 항목으로, 모델의 예측이 참 레이블이 상위 5개 예측 레이블 중 하나인 경우 정확하게 판단됩니다.
- 이미지넷: 시각적 객체 인식 소프트웨어 연구에 사용되는 대규모 데이터베이스로, 수백만 개의 레이블이 붙은 이미지가 포함되어 있습니다. 두 번째 부분도 참고하세요.

# 인셉션

Convolutions을 통해 더 깊게 들어가다, Christian Szegedy, Wei Liu, Yangqing Jia, Pierre Sermanet, Scott Reed, Dragomir Anguelov, Dumitru Erhan, Vincent Vanhoucke, Andrew Rabinovich에 의해 (2015)

이 연구는 이미지 인식 및 분류에 매우 효과적인 새로운 딥러닝 모델인 '인셉션'을 소개합니다. 이 디자인은 컴퓨팅 자원의 사용을 최대한으로 활용하여 네트워크를 더 깊고 넓게 만들어도 더 많은 계산 능력이 필요하지 않게 합니다. 이 모델의 특정 버전인 '구글넷'은 여러 스케일에서 동시에 이미지를 처리하여 주요 이미지 인식 대회에서 우수한 성능을 발휘했습니다.

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

이 연구는 새로운 신경망 구조를 소개하여 이미지 분류 및 탐지 성능을 크게 향상시킨 것으로 영향력이 큽니다. 인셉션 모델은 계산 자원을 효율적으로 사용하고 다양한 척도에서 정보를 처리할 수 있는 능력으로 신경망 설계의 새로운 기준을 세우며 AI 및 딥 러닝 분야의 후속 발전에 영향을 미쳤습니다.

- 딥 컨볼루션 신경망(CNN): 이미지와 같은 격자 구조의 데이터를 처리하기 위해 특별히 설계된 신경망 유형입니다. 여러 층을 사용하여 공간적 계층의 특징을 자동적으로 학습하고 적응적으로 학습합니다. Part 1도 참고하세요.
- ImageNet 대규모 시각 인식 대회(ILSVRC2014): 수백만 개의 레이블이 지정된 이미지 데이터셋을 사용하여 대규모 객체 탐지 및 이미지 분류 알고리즘을 평가하는 매년 개최되는 대회입니다.
- 계산 자원: 신경망 모델을 실행하는 데 할당된 계산 자원(처리 능력 및 메모리)의 양입니다.
- 헤비안 원칙: 뉴런이 동시에 활성화되면 시냅스의 효과성이 증가한다는 신경과학 이론입니다. 신경망의 맥락에서는 시냅스의 효과성이 전 단순 전위 및 후위 뉴런이 동시에 활성화될 때 증가한다는 개념을 의미합니다.
- 다중 척도 처리: 시스템이 정보를 여러 세부 수준이나 척도에서 분석하는 이미지 처리 접근 방식입니다. 이는 이미지에서 크기와 상관없이 개체를 인식하는 데 도움이 됩니다.
- 구글넷: 인셉션 아키텍처의 구체적 구현으로, 22층으로 구성되어 이미지 분류 및 탐지 작업에서 최첨단 결과를 달성하는 데 사용되었습니다.

# 심도 깊은 Q

볼로디미르 민이, 코레이 카부쿠오글루, 데이비드 실버, 안드레이 A. 루수, 조엘 베네스, 마크 G. 벨레메, 알렉스 그레이브스, 마틴 리드밀러, 안드레아스 K. 피들랜드, 그레그 오스트로브스키, 스티그 피터슨, 찰스 비티, 아미르 사딕, 요아 닌스 안토노글루, 헬렌 킹, 다샨 쿠마란, 단 비어스트라, 셰인 렉 및 데미스 하새비스(2015)의 깊은 강화 학습을 통한 인간 수준 제어

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

해당 연구는 인간이 하는 것과 비슷하게 게임 화면과 점수를 보고 다양한 Atari 2600 비디오 게임을 배우고 탁월한 성과를 내는 컴퓨터 프로그램을 만드는 과정을 설명합니다. 이 프로그램은 인공지능의 고급 기술을 활용하여 게임 환경을 이해하고 이기기 위한 전략을 개발하여 프로 플레이어 수준의 성능을 달성합니다.

이 논문은 깊은 학습과 강화 학습을 결합하여 원시 감각 입력(예: 비디오 게임 픽셀)을 직접 학습하여 복잡한 작업을 수행할 수 있는 지능적 에이전트를 만드는 잠재력을 증명했기 때문에 혁신적입니다. 이 논문은 손으로 디자인된 특징을 필요로하지 않고 다양한 작업에서 인간 수준의 성과를 달성할 수 있는 인공지능이 가능하다는 것을 보여주었으며, 보다 일반적이고 적응 가능한 AI 시스템을 위한 길을 열었습니다.

- 강화 학습(RL): 에이전트가 행동을 수행하고 보상이나 벌점을 받음으로써 결정을 학습하는 기계 학습 유형.
- 고차원 감각 입력: 이미지나 소리와 같이 많은 특징을 가진 복잡하고 상세한 입력으로 컴퓨터가 직접 처리하기 어려운 것.
- 계층적 감각 처리 시스템: 간단한 것부터 복잡한 것까지 단계적으로 감각 정보를 처리하는 생물학적 시스템인 인간 뇌와 같은 것.
- 상전보 신호: 종종 뇌의 보상 처리와 관련된 신경세포의 빠른 활동 폭발.
- 시간차 강화 학습: 에이전트가 시간이 지남에 따라 예측된 보상을 실제로 받은 보상과 비교함으로써 학습하는 RL 알고리즘.
- 심층 Q-네트워크(DQN): 이미지와 같은 고차원 입력에서 직접 강화 학습 작업의 최적 행동을 학습하기 위해 특별히 설계된 심층 신경 네트워크 유형.
- 엔드-투-엔드 강화 학습: 중간 특징 추출이나 수동 조정이 필요하지 않고 원시 입력부터 작업까지 AI 시스템을 학습시키는 것.
- 클래식 Atari 2600 게임: 이 연구에서 AI의 테스트용으로 사용된 초기 비디오 게임으로, 그래픽은 간단하지만 게임플레이는 복잡한 것으로 유명.
- 정책: 상황에서 AI 에이전트가 행동을 결정하기 위해 따르는 전략이나 규칙 세트.
- 하이퍼파라미터: AI 모델의 학습 프로세스를 제어하는 설정 또는 구성요소, 예를 들어 학습 속도나 네트워크 아키텍처.

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

**빠른 R-CNN: 지역 제안 네트워크를 활용한 실시간 객체 탐지 방식, Shaoqing Ren, Kaiming He, Ross Girshick, Jian Sun (2015)**

이 연구는 이미지 내 객체 탐지를 획기적으로 가속화시키는 방법을 제시하며, 지역 제안과 객체 탐지를 효율적인 단일 시스템으로 통합합니다. 동일한 합성곱층을 두 작업에 공유함으로써 해당 방법은 빠르게 지역 제안을 생성하고 높은 정확도로 객체를 탐지할 수 있습니다. 이는 표준 벤치마크에서 놀라운 결과를 달성하였습니다.

이 논문은 AI 분야에서 영향력을 끼쳤으며, 객체 탐지 시스템의 속도와 정확도를 현저히 향상시켰습니다. 지역 제안 생성과 객체 탐지를 하나의 네트워크로 결합함으로써, 실시간 객체 탐지 응용 프로그램의 길을 열었으며 컴퓨터 비전 및 딥 러닝 분야에서 많은 후속 개발에 영향을 미쳤습니다.

- **지역 제안 알고리즘**: 이미지 내에 객체가 있을 것으로 추정되는 부분을 식별하는 기술.
- **R-CNN (지역 기반 합성곱 신경망)**: 이미지 내 후보 지역을 먼저 제안하고 이후 이러한 지역을 분류하고 정제하여 객체를 식별하는 방법.
- **SPPnet (공간 피라미드 풀링 네트워크)**: 이미지 크기를 다르게 받아들일 수 있는 방식으로 객체 탐지의 효율성을 개선하는 네트워크.
- **Fast R-CNN**: 원래 R-CNN의 속도와 정확도를 향상시킨 고급 객체 탐지 모델.
- **지역 제안 네트워크 (RPN)**: 이미지에서 객체 위치와 점수를 예측하여 지역 제안을 생성하는 네트워크.
- **완전 컨볼루션 네트워크**: 오직 컨볼루션층만 사용하는 신경망 유형으로, 이미지 인식과 같은 작업에 효율적입니다.
- **객체 경계**: 이미지 내 객체 주변의 직사각형을 정의하는 좌표.
- **객체성 점수**: 지역이 객체를 포함할 가능성을 나타내는 점수.
- **End-to-End 훈련**: 각 부분을 별도로 훈련시키는 대신 시스템 전체를 동시에 훈련시키는 처리 방식.
- **교대 최적화**: 네트워크의 다른 부분을 교대로 훈련시켜 시스템을 최적화하는 훈련 전략.
- **VGG-16 모델**: 이미지 인식 작업에서 뛰어난 성능으로 알려진 16층 깊이의 심층 컨볼루션 네트워크.
- **프레임 속도**: 시스템이 이미지를 처리하는 속도로, 초당 처리하는 프레임 수(fps)로 측정됩니다.
- **mAP (평균 평균 정밀도)**: 객체 탐지 모델의 정확도를 평가하는 지표.
- **PASCAL VOC**: 객체 탐지 알고리즘의 성능을 벤치마크로 사용하는 표준 데이터셋.

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

# U-Net

U-Net: Convolutional Networks for Biomedical Image Segmentation, by Olaf Ronneberger, Philipp Fischer, Thomas Brox (2015)

안녕하세요! 오늘은 Olaf Ronneberger, Philipp Fischer, Thomas Brox가 2015년에 발표한 "U-Net: Convolutional Networks for Biomedical Image Segmentation" 연구에 대해 이야기해보려고 해요.

이 연구는 의료 이미지를 분석하는 새로운 방법인 U-Net을 소개했어요. U-Net은 세포나 뉴런과 같은 이미지 내 다양한 구조물을 식별하고 윤곽을 그릴 수 있는 인공 지능의 한 종류에요. 제한된 예제를 똑똑하게 활용하고 데이터 증강을 통해 향상시킴으로써, 이 방법은 적은 원본 이미지라도 매우 잘 수행할 수 있도록 훈련될 수 있어요. 빠르고 정확하기 때문에 의학 연구에서 세포를 추적하는 것과 같은 작업에 이상적이에요.

이 논문은 이미지 분할을 수행하는 혁신적이고 효율적인 방법을 제안했기 때문에 영향력을 미쳤어요. U-Net 아키텍처의 정확한 결과를 제한된 데이터로 제공할 수 있는 능력과 빠른 처리 속도는 의료 영상 및 그 이상에서 딥러닝 기술 개발에서 중요한 역할을 했어요. 다양한 도전을 성공적으로 수행한 성과는 U-Net의 실용적인 효과를 입증하며 해당 분야의 새로운 표준을 설정했어요.

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

- 데이터 증강: 이미지 회전 또는 크기 조정과 같은 무작위 변형을 적용하여 기존 데이터에서 새로운 훈련 데이터를 만드는 기술로, AI 모델이 더 잘 학습하도록 돕는다.
- 수축 경로: 이미지 크기를 단계별로 줄여가며 중요한 특징을 포착하는 U-Net의 일부분.
- 확장 경로: 이미지 크기를 원래 크기로 다시 확대하여 특징을 정확하게 지역화할 수 있게 하는 보완적인 부분.
- 슬라이딩 윈도우 컨볼루션 신경망: 이미지를 분류하기 위해 작은 창이 이미지 상에서 이동하는 구식 방법으로, U-Net에 비해 효율성이 낮다.
- ISBI 챌린지: 연구자들이 생명과학 이미지 분할 작업에 자신의 방법을 시험하는 경쟁적인 이벤트.
- 투과광 현미경: 샘플을 통과하는 빛을 사용하여 이미지를 생성하는 기술로, 생물학 및 의학 연구에 중요하다.
- 위상강조 및 DIC: 투명한 시료에서 대조를 강화하는 특정 유형의 광 현미경 기술.
- GPU(그래픽 처리 장치): 빠른 연산을 수행하는 강력한 프로세서로, 딥 러닝 모델의 교육 및 실행에 필수적이다.

# 잔여 학습

Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun(2015)에 의해 Image Recognition을 위한 Deep Residual Learning

이 연구는 장마크 신경망을 훈련하는 새로운 방법, 잔여 학습을 소개하며, 많은 층을 가진 네트워크를 더 쉽고 효과적으로 훈련할 수 있게 한다. 이 기술은 희미한 기울기 문제를 해결함으로써 정확도를 향상시키며, 보다 깊은 네트워크 는 일반적으로 최적화하기가 점점 어려워지는 문제를 해결한다. 이 방법을 사용하여 저자들은 이미지 인식 대회에서 획기적인 결과를 달성했으며, 어려운 데이터셋에 대한 성능을 크게 향상시켰다.

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

이 논문은 이전에 최적화하기 어려웠던 심층 신경망을 훈련하는 실용적인 해결책을 보여줘, 매우 영향력이 크다. 더 깊은 신경망을 훈련할 수 있게 해서, 다양한 시각 인식 작업에서 더 발전된 정확한 AI 모델을 위한 길을 열었다. 잔차 신경망(ResNets)의 성공은 깊은 학습 모델의 개발에서 주요 아키텍처로 자리매김하게 해서, 후속 연구와 AI 응용에 영향을 끼쳤다.

- 잔차 학습: 신경망 내 레이어가 전체 출력을 직접 학습하기보다 실제 출력과 입력 간 차이인 잔차 함수를 학습하여 훈련을 더 효율적으로 만드는 기술.
- 사라지는 그래디언트: 깊은 신경망을 훈련할 때 발생하는 그래디언트(모델을 업데이트하는 데 사용되는 값)가 너무 작아져 훈련이 중단되는 문제.
- 레이어: 계산이 수행되는 신경망의 여러 단계로, 각 레이어가 이전 레이어에 기반을 두고 있다.
- VGG 네트: 간단하고 깊은 구조로 유명한 합성곱 신경망 아키텍처로, 옥스퍼드 대학교 시각 기하 그룹에서 개발되었다.
- COCO: 객체 탐지, 세분화 및 캡션 작업에 사용되는 데이터셋.
- ILSVRC: 컴퓨터 비전 분야에서 권위 있는 대규모 시각 인식 챌린지.
- CIFAR-10: 10가지 클래스의 32x32 컬러 이미지 60,000개로 이루어진 기계 학습 및 컴퓨터 비전 알고리즘 훈련용 데이터셋.
- 객체 탐지: 이미지 내 객체를 식별하고 위치를 찾는 컴퓨터 비전 작업.
- 위치 지정: 이미지 내 객체를 감지하는 것뿐만 아니라, 그들의 정확한 위치를 결정하는 작업.
- 세분화: 이미지를 여러 세그먼트나 영역으로 분할하여 분석을 단순화하는 과정.

# YOLO

Joseph Redmon, Santosh Divvala, Ross Girshick, Ali Farhadi가 저술한 "한 번에 본다: 통합형, 실시간 객체 탐지" (2016)

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

이 연구는 이미지에서 물체를 빠르고 정확하게 탐지하는 새로운 방법을 제안합니다. 이전에는 물체를 식별하기 위해 복잡한 단계를 사용했지만, 이 방법은 단일하고 신속한 신경망을 사용해 작업을 단순한 예측 문제로 다룹니다. 이를 통해 실시간으로 이미지를 분석하여 거의 즉시 물체를 식별할 수 있습니다. 이 시스템은 다양한 상황에서 물체를 인식할 수 있으며, 배경에 있는 물체를 잘못 식별하는 것을 줄일 수 있습니다.

이 논문은 이전 기술보다 더 빠르고 효율적인 물체 탐지 방법을 도입하여 영향력을 끼쳤습니다. 연구자들이 문제에 접근하는 방식을 변화시키며, 자율 주행, 보안 시스템, 증강 현실과 같은 실시간 응용 프로그램에서 발전을 이끌었습니다.

- YOLO (You Only Look Once): 실시간 물체 탐지를 위한 신경망 모델로, 간단한 예측 작업으로 탐지를 처리하여 이미지를 빠르게 처리합니다.
- 회귀 문제: 이 문맥에서 물체의 위치와 해당 클래스 확률을 연속 값으로 예측하는 것.
- 경계 상자: 이미지에서 물체 주변에 그려진 직사각형으로 위치를 나타냅니다.
- 클래스 확률: 감지된 물체가 특정 카테고리에 속할 가능성.
- mAP (Mean Average Precision): 물체 감지 시스템의 정확도 측정값.
- DPM (Deformable Parts Model): 물체 탐지를 위한 이전 방법으로, 물체를 나타내기 위해 일련의 부분을 사용합니다.

읽어 주셔서 감사합니다! 피드백을 환영합니다! 특히 중요한 연구를 놓친 것 같다고 생각하시면 알려주세요!

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

![Tarot Image](/assets/img/2024-07-09-AHistoryofAIPart4_0.png)
