---
title: "안드로이드 개발에서 배운 10가지 고급 Jetpack Compose 테크닉 "
description: ""
coverImage: "/assets/img/2024-07-10-Top10advancedJetpackComposetechniquesIvepickedupafteryearsofAndroiddevelopment_0.png"
date: 2024-07-10 01:29
ogImage: 
  url: /assets/img/2024-07-10-Top10advancedJetpackComposetechniquesIvepickedupafteryearsofAndroiddevelopment_0.png
tag: Tech
originalTitle: "Top 10 advanced Jetpack Compose techniques 🦊I've picked up after years of Android development."
link: "https://medium.com/@niranjanky14/top-10-advanced-jetpack-compose-techniques-ive-picked-up-after-years-of-android-development-7b9081a8e762"
isUpdated: true
---





🔮안녕하세요! 오늘은 'State Hoisting'에 대해 이야기해볼게요.

'**State Hoisting**'는 여러 컴포저블 사이에서 상태를 공유해야 할 때 사용되는 기술이에요. 이때 우리는 상태를 컴포저블 트리의 더 상위 수준으로 이동시켜요. 어렵게 생각하지 마세요! 함께 배워나가면서 더 많은 지식을 쌓아가요. 🌟✨

<div class="content-ad"></div>

위 코드 예제에서 보듯이 showBootcampScreen 상태는 공유되며, 컴포저 트리의 최상위에 유지됩니다.

## State Hoisting을 사용하는 이유

![이미지1](/assets/img/2024-07-10-Top10advancedJetpackComposetechniquesIvepickedupafteryearsofAndroiddevelopment_1.png)

![이미지2](/assets/img/2024-07-10-Top10advancedJetpackComposetechniquesIvepickedupafteryearsofAndroiddevelopment_2.png)

<div class="content-ad"></div>

# 2. Modifier.then()을 사용한 사용자 정의 레이아웃:

Modifier를 연결하여 복잡한 UI를 만들어보세요. 아래는 간격이 있는 그리드 레이아웃을 만드는 방법입니다: