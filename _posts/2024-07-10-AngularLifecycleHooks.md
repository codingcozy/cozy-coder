---
title: "Angular  라이프사이클 훅 이해하기 및 사용 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 00:39
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Angular : Lifecycle Hooks"
link: "https://medium.com/@reachbyteridge/angular-lifecycle-hooks-babc45ebd4b0"
isUpdated: true
---





먼저 Angular가 없으면 이 블로그를 읽지 않겠지요… 수명주기 훅은 Angular의 일부로 버전 2부터 있었어요 (오래된 것 같죠 ❤). 수명주기 훅은 거의 5년 동안 Angular에 충실했고 평생 충실할 것을 약속합니다. 지금 실제 내용으로 넘어가 볼게요.

지시문(Directive) 및 구성요소(Component) 인스턴스는 Angular가 생성, 업데이트 및 파괴하는 과정에서 라이프사이클이 있어요. 개발자는 Angular 코어 라이브러리에서 라이프사이클 훅 인터페이스 중 하나 이상을 구현함으로써 해당 라이프사이클의 중요한 순간에 개입할 수 있어요. 각 인터페이스에는 인터페이스 이름 앞에 ng 접두사가 붙은 단일 훅 메소드가 있어요.

Angular 프로젝트가 초기화되면 Angular은 루트 구성요소를 생성하고 표시해요. 설계되고 후손을 생성해요. 어플리케이션 개발 중에 로드된 구성 요소들에 대해 Angular는 데이터 바인딩 속성이 변경되고 업데이트될 때 계속 확인해요. 구성 요소가 더 이상 사용되지 않으면 사멸 단계로 접근해서 DOM에서 사라지게 돼요.

계속... https://www.byteridge.com/expert-opinions/angular-lifecycle-hooks/