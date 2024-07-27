---
title: "시계열 예측을 위해 현재 딥러닝이 필요하지 않은 이유"
description: ""
coverImage: "/assets/img/2024-07-06-WhyYouCurrentlyDoNotNeedDeepLearningforTimeSeriesForecasting_0.png"
date: 2024-07-06 10:59
ogImage: 
  url: /assets/img/2024-07-06-WhyYouCurrentlyDoNotNeedDeepLearningforTimeSeriesForecasting_0.png
tag: Tech
originalTitle: "Why You (Currently) Do Not Need Deep Learning for Time Series Forecasting"
link: "https://medium.com/towards-data-science/why-you-currently-do-not-need-deep-learning-for-time-series-forecasting-0de57f2bc0ed"
---


/assets/img/2024-07-06-WhyYouCurrentlyDoNotNeedDeepLearningforTimeSeriesForecasting_0.png

타임 시리즈 예측을 위한 딥 러닝이 많은 관심을 받고 있습니다. 많은 기사와 과학 논문이 최신 딥 러닝 모델에 대해 다루고 있으며, 이 모델이 모든 머신 러닝이나 통계 모델보다 우수하다고 언급합니다. 이러한 내용은 딥 러닝이 타임 시리즈 예측의 모든 문제를 해결해 줄 것으로 인식되고 있습니다. 특히 이 분야에 입문한 신입자들에게 더 그럴싸하게 보입니다.

그러나 저의 경험상, 딥 러닝이 필수적인 것은 아닙니다. 타임 시리즈 예측에 더 중요하고 효과적인 방법이 있습니다.

따라서, 이 글에서는 어떤 것이 작동하는지 보여 드리고 싶습니다. 많은 상황에서 효과를 입증한 방법들을 소개하겠습니다. Makridakis M5 대회 및 Kaggle AI 보고서 2023의 결과를 활용하여 제 경험과 비교해 보겠습니다.

<div class="content-ad"></div>

The Makridakis competitions, 실제 데이터셋에서 예측 방법을 비교합니다. 실전에서 무엇이 효과적인지 보여줍니다. 거의 40년 전에 시작된 대회 이후로 결과들이 변화했습니다. 처음 세 대회인 M1부터 M3에서는 통계 모델이 주를 이뤘습니다. M4 대회에서는 머신 러닝 모델이 잠재력을 보이기 시작했습니다.