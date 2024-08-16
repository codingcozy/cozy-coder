---
title: "넷플릭스 아키텍처에서 배울 수 있는 시스템 설계 교훈 10가지"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-29 13:49
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "10 System Design Lessons from Netflixs Architecture"
link: "https://medium.com/javarevisited/10-system-design-lessons-from-netflixs-architecture-19ece2191a03"
---


<img src="https://miro.medium.com/v2/resize:fit:1400/1*PB7v7MmW9NpdNn-_mfTj_A.gif" />

안녕하세요 친구들, 최근 대규모 기술 기업들이 시스템 디자인에 더 많은 주목을 기울이면서, 모두가 기존 시스템의 아키텍처를 배우는 데 시간을 투자하고 있습니다. 실제로 좋은 점입니다. 왜냐하면 넷플릭스와 같이 현실 세계에서 대규모 앱을 연구하는 것보다 더 나은 학습 방법은 없기 때문이죠.

지난 금요일의 Microsoft와 Crowdstrike의 전 세계적인 서비스 중단은 900만 대 이상의 기기에 영향을 미쳤습니다. 이 일은 수천 대의 항공편 지연을 야기하며 수백만 명의 사람들에게 불편을 초래한 것으로 보여지네요.

넷플릭스는 2억 4천7백만 명을 구독자들을 보유하고 있으며, 원활한 스트리밍과 탁월한 사용자 경험을 보장하는 탁월한 시스템 아키텍처를 전시하고 있습니다. 그러나 그들은 이것을 어떻게 하는 걸까요?

<div class="content-ad"></div>

3주 전에 넷플릭스 아키텍처에 대해 배우기 시작했어요. 제 첫 번째 정착지는 유튜브였고, 많은 넷플릭스 아키텍처 관련 동영상들을 만났어요. 이 기사에서는 제가 배운 교훈들과 함께 해당 동영상도 함께 공유할 거에요.

제가 처음으로 봤던 동영상은 Exponent의 '넷플릭스 디자인 방법론'이에요. 시스템 디자인 면접 준비 중이라면 좋아하는 채널 중 하나예요.