---
title: "효율적인 LLAMA3 조정을 위한 ORPO 및 표현 세분화 결합 방법"
description: ""
coverImage: "/code-tower.github.io/assets/no-image.jpg"
date: 2024-07-08 00:10
ogImage: 
  url: /code-tower.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Combining ORPO and Representation Fine-Tuning for Efficient LLAMA3 Alignment"
link: "https://medium.com/towards-data-science/combining-orpo-and-representation-fine-tuning-for-efficient-llama3-alignment-77f6a2e3af8c"
isUpdated: true
---





파인튜닝은 특정 작업에 언어 모델을 적응시키는 가장 인기 있는 기술 중 하나입니다.

하지만 대부분의 경우, 이를 위해서는 많은 양의 컴퓨팅 파워와 자원이 필요합니다.

최근의 발전 중 PeFT, 즉 파라미터 효율적 파인튜닝과 같은 기술들은 Low-Rank 적응 방법, 표현 파인튜닝, 그리고 ORPO (Odds Ratio Preference Optimization)와 같은 방법들은 파인튜닝을 보다 효율적으로 만들기 위해 노력하고 있습니다. 이러한 방법들은 많은 컴퓨팅 자원과 훈련 시간을 절약하면서 최첨단 수준이나 뛰어난 성능을 달성합니다.

이제 우리는 이러한 방법을 도입하여 최적화 경계를 얼마나 더 늘릴 수 있을까요? (이 글의 전문을 읽으려면 여기를 클릭해 주세요. 작가들을 지원하려면 Medium 멤버십 고려해 주세요!)

<div class="content-ad"></div>

이 게시물에서는 최신 기법 중 두 가지를 조합하는 방법에 대해 이야기하겠습니다: LLAMA3 모델의 최적 선호 조정을 위한 표현 파인튜닝과 ORPO의 중요성에 대해 설명하겠습니다.

먼저, 언어 모델의 선호도 훈련의 중요성을 설명하고 기존의 선호 조정 기술을 개요로 살펴보겠습니다. 그런 다음에는…