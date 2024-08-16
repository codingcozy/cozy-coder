---
title: "정확한 라벨링으로 머신러닝 성능 높이는 방법 Confident Learning 활용법"
description: ""
coverImage: "/it-bada.github.io/assets/no-image.jpg"
date: 2024-07-09 10:16
ogImage: 
  url: /it-bada.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Confident Learning: Harnessing the Power of Label Accuracy in Machine Learning"
link: "https://medium.com/dev-genius/confident-learning-harnessing-the-power-of-label-accuracy-in-machine-learning-39961e51bbfe"
isUpdated: true
---





# Abstract

안녕하세요! 오늘은 기계 학습에서 증명된 정보의 질이 모델 성능에 중요하다는 것에 대해 이야기해보려고 해요. 노이즈가 있는 레이블은 예측 모델의 정확도와 신뢰성을 현저히 저하시킬 수 있어요.

문제는, 노이즈가 있는 레이블을 다루기 위한 전통적인 방법은 종종 수작업 검사나 간단한 휴리스틱을 활용하며, 이는 시간이 많이 소요되고 오류 발생 가능성이 높아져 모델 성능이 최적화되지 않을 수 있다는 것이에요.

이 글에서는 자신감 있는 학습에 대해 탐구해보려고 해요. 이는 모델의 예측을 활용하여 훈련 데이터의 정확성을 향상시키고 레이블 오류를 체계적으로 식별하고 수정하는 접근 방식을 의미해요. 합성 데이터셋을 활용하여 이 방법론을 설명하고, 특성 엔지니어링, 하이퍼파라미터 조정, 교차 검증 및 평가에 대해 살펴볼 거예요.

<div class="content-ad"></div>

**Results:** The confident learning classifier delivered enhanced performance results with an accuracy of 78%. By conducting feature importance analysis, we identified crucial variables that significantly influence the model's decision-making process. Furthermore, the confusion matrix demonstrated a well-balanced precision and recall, indicating the model's effectiveness.

**Conclusions:** Confident learning proves to be a successful strategy in reducing the impact of noisy labels, thereby creating more dependable and resilient machine learning models. This approach plays a vital role in scenarios where label accuracy is of utmost importance, ultimately improving the overall accuracy and reliability of AI systems.

**Keywords:** Confident Learning, Noisy Labels, Machine Learning Data Quality...