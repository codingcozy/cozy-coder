---
title: "안드로이드 WorkManager 완벽 가이드 처음부터 고수가 되는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-FromZerotoHerowithWorkManagerinAndroid_0.png"
date: 2024-07-07 20:02
ogImage: 
  url: /assets/img/2024-07-07-FromZerotoHerowithWorkManagerinAndroid_0.png
tag: Tech
originalTitle: "From Zero to Hero with WorkManager in Android"
link: "https://medium.com/nerd-for-tech/from-zero-to-hero-with-workmanager-in-android-22cf6a185791"
---


안녕하세요, 카드 전문가입니다.

안드로이드에서의 백그라운드 작업 스케줄링은 어려울 수 있습니다. WorkManager는 강력하고 유연한 솔루션을 제공하여 이 프로세스를 간소화합니다. 이 안내서는 개발자가 직면하는 일반적인 문제를 다루고 WorkManager가 강력한 기능을 통해 이러한 문제를 해결하는 방법에 대해 설명합니다.

**Q1: WorkManager가 무엇이며 왜 사용해야 하는가?**
WorkManager는 안드로이드 Jetpack에서 제공하는 API로, 예약 가능하고 보장된 백그라운드 작업을 스케줄링하는 데 사용됩니다. 모든 안드로이드 버전에서 일관된 API를 제공하여 작업을 실행하는 가장 좋은 방법을 자동으로 선택함으로써 작업 스케줄링을 간편화합니다.

**Q2: WorkManager에 적합한 작업 유형은 무엇인가?**
WorkManager는 보장된 실행이 필요한 작업에 이상적입니다. 예를 들어 다음과 같은 작업에 적합합니다:
- 주기적인 데이터 동기화
- 로그 또는 분석 업로드
- 알림 전송
- 로컬 데이터 정리

행복한 하루 되세요!

<div class="content-ad"></div>

실시간 또는 즉시 실행이 필요한 작업에는 적합하지 않아요.

Q3: 내 프로젝트에서 WorkManager를 설정하는 방법은 무엇인가요?
A: 먼저 build.gradle 파일에 WorkManager 종속성을 추가하세요. 그리고 Worker를 확장하고 doWork() 메서드를 구현하여 작업을 정의할 수 있어요.

마지막으로 WorkManager를 사용하여 작업을 예약하세요.

Q4: WorkManager는 작업 실행 전에 가져야하는 작업 제약 조건을 어떻게 처리하나요?
A: WorkManager를 사용하면 네트워크 가용성, 디바이스 충전 상태, 디바이스 대기 상태, 배터리 레벨과 같은 적절한 조건 하에서 작업이 실행되도록 제약 조건을 설정할 수 있어요.

<div class="content-ad"></div>

요구사항 설정하는 예시를 보여드릴게요:

Q5: WorkManager로 주기적 작업을 예약할 수 있을까요?
A: 네, WorkManager는 주기적 작업을 지원합니다. `PeriodicWorkRequestBuilder`를 사용하여 일정 간격으로 반복되는 작업을 예약할 수 있어요.

Q6: WorkManager는 작업 완료를 어떻게 보장할까요?
A: WorkManager는 작업 실행을 보장하기 위해 다음과 같은 방법으로 작업 완료를 보장합니다:
— 앱 재시작 및 시스템 재부팅 시 작업 유지
— 실패한 작업을 자동으로 재시도하여 백오프 정책에 따라
— 다양한 결과 상태 제공 (성공, 실패, 재시도)

Q7: WorkManager를 사용하여 작업을 연결하는 방법은 무엇인가요?
A: beginWith() 및 then() 메서드를 사용하여 여러 작업을 연속적으로 실행할 수 있어요.

<div class="content-ad"></div>

A8: 나의 작업 상태를 어떻게 관찰할 수 있을까요?
A: WorkInfo 옵저버를 연결하여 작업 상태를 관찰할 수 있습니다.

A9: Worker에 데이터를 전달하고 받을 수 있을까요?
A: 네, Data 객체를 사용하여 Worker에 입력 데이터를 전달하고 비슷한 방식으로 출력 데이터를 검색할 수 있습니다.

A10: WorkManager에서 작업 재시도를 어떻게 처리할까요?
A: 실패한 작업에 대한 재시도를 처리하기 위해 백오프 정책을 지정할 수 있습니다. 이는 WorkManager가 재시도하기 전에 기다리는 시간을 결정합니다.

A11: PeriodicWorkPolicy는 무엇이며 주기적 작업에 어떤 영향을 미치나요?
A: `PeriodicWorkPolicy`는 동일한 고유 이름을 가진 새 주기적 작업이 큐에 들어올 때 기존 주기적 작업의 동작을 결정합니다. 사용할 수 있는 두 가지 정책이 있습니다:
- KEEP: 동일한 이름을 가진 기존 주기적 작업이 있는 경우 새 요청은 무시됩니다.
- REPLACE: 기존 주기적 작업이 취소되고 새 요청으로 대체됩니다.

<div class="content-ad"></div>

여기 `PeriodicWorkPolicy`를 사용한 예제가 있습니다:

이 예제에서, 이미 "uniqueWorkName"이라는 이름의 주기적 작업이 있는 경우 유지되고, 새 요청은 무시됩니다. 기존 작업을 대체하려면 `ExistingPeriodicWorkPolicy.REPLACE`를 사용하십시오.

이것이 WorkManager에 관한 전부입니다. 자유롭게 시도해보세요.