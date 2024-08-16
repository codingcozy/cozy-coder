---
title: "깊이 배우기 일러스트 5부 - 장단기 메모리 LSTM 쉽게 이해하기"
description: ""
coverImage: "/it-bada.github.io/assets/no-image.jpg"
date: 2024-07-06 10:58
ogImage: 
  url: /it-bada.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Deep Learning Illustrated, Part 5: Long Short-Term Memory (LSTM)"
link: "https://medium.com/towards-data-science/deep-learning-illustrated-part-5-long-short-term-memory-lstm-d379fbbc9bc6"
isUpdated: true
---





저희의 딥러닝 일러스트 여행 5부에 오신 것을 환영합니다!

오늘은 이전 글에서 다뤘던 일반적인 순환 신경망(RNN)을 업그레이드한 Long Short-Term Memory(LSTM) 네트워크에 대해 이야기해볼 거예요. 우리는 RNN이 순차적 문제를 해결하는 데 사용되지만 멀리 떨어진 정보를 보존하는 데 어려움을 겪어 단기 기억 문제를 야기한다는 것을 알았어요. 여기서 LSTM이 등장해 문제를 해결합니다. 그들은 RNN의 반복 측면을 활용하지만 독특한 방법으로 접근하는 거죠. 그래서 이것이 어떻게 이루어지는지 알아보도록 할게요.

작은 팁 - 이 글은 내가 쓴 것 중에서 가장 좋아하는 글 중 하나라서, 여러분을 이 여정에 안내해드리는 것을 기다리지 못할 만큼 설레이고 있어요!

먼저 우리가 이전에 보았던 RNN에서 무슨 일이 일어나고 있는지 살펴봅시다. 우리는 입력 x, 하나의 tanh 활성화 함수를 가진 하나의 뉴런으로 구성된 은닉 레이어, 그리고 시그모이드 활성화 함수를 갖는 출력 뉴런을 가진 신경망을 가졌었죠. 그래서 RNN의 첫 번째 단계는 다음과 같았습니다: