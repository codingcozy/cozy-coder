---
title: "한눈에 알 수 있는 비대칭 데이터 시각화 방법 5가지"
description: ""
coverImage: "/code-tower.github.io/assets/no-image.jpg"
date: 2024-07-06 03:46
ogImage: 
  url: /code-tower.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Golden Methods to Visualize Skewed Data"
link: "https://medium.com/@yuanbo.faith/golden-methods-to-visualize-data-with-skewed-distribution-13f0b2a0db99"
isUpdated: true
---





튀어나온 데이터는 매우 불균형한 분포를 갖는 데이터를 의미합니다. 한 가지 변수의 데이터가 히스토그램으로 나타낼 때, 대부분의 데이터 포인트가 분포의 왼쪽에 군집되어 있고 긴 꼬리가 오른쪽 쪽으로 뻗어 있는 경우 (오른쪽으로 편향), 또는 다른 방식으로도 될 수 있습니다 (왼쪽으로 편향) 또는 더 복잡한 편향 패턴일 수 있습니다. 긴 꼬리(희소한 데이터)가 그래픽 공간을 지배하며 대다수의 데이터 포인트가 플롯 모서리로 압축되어 있어 군집된 데이터 주요 부분의 기본 패턴을 파악하기 어렵게 만듭니다. 

이 글에서는 편향된 분포를 가진 데이터를 시각화하는 강력한 일곱 가지 전략을 요약했습니다.

### 투명도, 열린 원, 색상, 그리고 주변 분포를 활용하여 군집된 데이터 패턴을 드러내기

왼쪽의 다음 산점도는 주택 판매와 가격 사이의 관계를 보여줍니다. 상당히 높은 판매량을 보여주는 희소한 데이터 포인트들이 주요 데이터를 왼쪽으로 압축합니다…