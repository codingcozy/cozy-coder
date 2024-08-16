---
title: "인공 신경망과 지표를 사용한 MASI 예측 향상 방법"
description: ""
coverImage: "/code-tower.github.io/assets/no-image.jpg"
date: 2024-07-09 11:33
ogImage: 
  url: /code-tower.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Enhancing MASI Predictions with Artificial Neural Networks and Indicators"
link: "https://medium.com/@redeaddiscolll/enhancing-masi-predictions-with-artificial-neural-networks-and-indicators-40ec0fba327c"
isUpdated: true
---





# 소개

재정 시장을 이해하기 위해서는 미래 추세를 예측하는 데 정확한 도구가 필요합니다. 본 글은 모로코 증시를 나타내는 모로코 전체 지수(MASI)를 예측하기 위한 고급 기술을 살펴봅니다. 기술적 지표가 이러한 예측의 정확도 향상에 어떤 영향을 미치는지를 확인하고자 합니다.

연구자들은 MASI 지수의 일일 종가를 수집했습니다. 그들은 이 데이터를 사용하여 가격 데이터에서 유도된 26가지 기술적 지표를 계산했습니다. 이러한 지표는 미래 가격을 예측하는 데 도움이 됩니다.

본 글에서는 이 데이터셋으로 훈련된 인공 신경망(ANN)을 사용합니다. 이 ANN은 딥 러닝 모델로, 이 모델들의 성능은 여러 메트릭을 사용하여 평가됩니다.

<div class="content-ad"></div>

**Mean absolute error (MAE)**은 예측 오차의 평균 크기를 나타내며, 오차가 너무 높은지 낮은지는 고려하지 않습니다. 

**Mean squared error (MSE)**는 오차를 제곱하여 평균한 것으로, 큰 오차에 더 많은 가중치가 부여됩니다.

**Mean absolute percentage error (MAPE)**는 오차를 실제 값의 백분율로 표시합니다.