---
title: "스마트한 NgRX 사용 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-09 10:30
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "SmartNgRX"
link: "https://medium.com/@davembush_78433/smartngrx-d4fd2fb2382a"
isUpdated: true
---





## 보일러플레이트를 제거하고 SmartNgRX가 대부분의 작업을 대신하도록 합시다! (원문: smart-ngrx 게시물에서 발췌)

상상해보세요. NgRX를 사용 중인 줄도 모르실 정도로 거의 NgRX를 사용하지 않는 세계가 어떤 것인지. 리듀서, 액션, 이펙트를 작성할 필요가 없는 세계. store.dispatch()를 호출할 필요가 없는 세계. 작업한 데이터가 자동으로 서버에 지속적으로 유지되는 세계. 데이터가 필요할 때 서버에서 검색되고 사용이 끝나면 메모리에서 제거되거나 원하는 경우 영구히 보존되는 세계. 데이터가 자동으로 서버에서 새로 고침되는 세계, 또는 웹소켓 메시지를 사용하여 데이터를 새로 고침하며 그 경우에도 코드가 현재 사용하는 데이터만 새로 고침되는 세계. 낙관적 UI가 프레임워크에 내장된 세계.

여러분, SmartNgRX를 소개합니다. 제가 1년 넘게 개발한 프레임워크로, 상기한 작업뿐만 아니라 더 많은 작업을 수행합니다. NgRX 기반으로 작동하며 기존 NgRX 코드와 함께 사용할 수 있는 프레임워크입니다.

전체 문서는 SmartNgRX 문서 에서 확인하실 수 있지만, 작동 방식에 대해 간단히 설명드리겠습니다.

<div class="content-ad"></div>

# 간단한 개요

먼저, 응용 프로그램에 추가해야 할 두 개의 공급자가 있습니다. 첫 번째는 AppModule의 providers 배열에 추가되는 provideSmartNgRX()입니다. 두 번째는 사용할 위치에서 가장 가까운 모듈 또는 라우트에 추가되는 provideSmartNgRX()입니다.

이 두 공급자는 SmartNgRX가 작동하는 데 필요한 구성 정보를 설정합니다. 데이터를 새로 고침할 빈도, 사용하지 않는 데이터를 메모리에서 제거할 시기, NgRX 조각에 대한 데이터를 검색하기 위해 호출할 서비스, 특정 데이터 조각에 대한 플레이스홀더 행이 어떻게 보이는지 등을 제어합니다.

SmartNgRX 효과에서 호출할 서비스를 정의하려면 EffectService를 만들어야 합니다. 이 서비스는 CRUD 작업이 서버와 상호 작용하는 방식을 제어하는 곳입니다. 이것과 셀렉터가 작성해야 할 유일한 코드입니다.

<div class="content-ad"></div>

이제 셀렉터에 대해 이야기해보겠습니다. NgRX의 createSelector 함수를 사용하는 대신 SmartNgRX의 createSmartNgRXSelector 함수를 사용할 겁니다. 이 함수는 부모 셀렉터와 해당 셀렉터가 가질 수 있는 모든 자식들 간의 관계를 정의합니다.

그게 전부에요. 보통 작성하던 모든 다른 코드는 SmartNgRX에서 처리됩니다.

한번 시도해보고 어떻게 생각하는지 알려주세요.