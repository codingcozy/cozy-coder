---
title: "클라이언트 측 데이터-저니 분석기로 스마트 디버깅 하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-SmartdebuggingusingData-JourneyAnalyzersontheclientside_0.png"
date: 2024-08-03 18:24
ogImage: 
  url: /assets/img/2024-08-03-SmartdebuggingusingData-JourneyAnalyzersontheclientside_0.png
tag: Tech
originalTitle: "Smart debugging using Data-Journey Analyzers on the client side"
link: "https://medium.com/@ethicalhacker365/smart-debugging-using-data-journey-analyzers-on-client-side-d66edbe8dd94"
---


당신의 앱이 새벽 3시에 데이터베이스 마이그레이션을 예약하거나 정기적인 간격으로 동기화를 수행하는 상황에서 QA 팀이 일부 사용자가 데이터가 올바르게 마이그레이션되지 않는 심각한 버그를 겪고 있다고 보고했습니다. 사용자의 기기에서 이렇게 발생할 수 있는 방법에 대해 전혀 감이 없습니다. 이 버그나 잘못된 상태를 일으킬 수 있는 레이스 조건을 재현할 수있는 가능성은 90%입니다. 이를 어떻게 디버깅할 것인가요?

한 가지 직감은 일부 내부 분석 이벤트를 서버에 푸시하여 특정 흐름 체크포인트에서 분석하는 것일 수 있습니다. 이것은 옳을 수 있지만 만약 이러한 "특정 체크포인트"가 많다면 어떨까요?

예를 들어 설명해보겠습니다. 다음과 같이 캐싱 전략을 구현하고 있는 상황입니다:

- Work manager가 매일 실행됨.
- 캐시 크기 및 TTL을 확인합니다.
- 캐시 크기가 TTL에 따라 빈 값 또는 잘못된 경우, 서버에서 데이터를 가져오기 시작합니다.
- 이제 데이터는 3회의 재시도 후 또는 한 번에 가져왔을 수 있습니다.
- 데이터를 성공적으로 가져온 후, 새 데이터를 위한 공간을 확보하고 그곳에 두기 위해 캐시를 정리합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-SmartdebuggingusingData-JourneyAnalyzersontheclientside_0.png)

지금 문제는 어떤 레이스 조건이 발생하는지입니다. 어떤 이벤트를 분석에 로깅할까요? 버그가 발생할 수 있는 5-10개의 단순한 체크포인트들을 볼 수 있습니다. 모든 라인을 로깅하고 서버로 푸시하는 것은 멋져 보이지 않습니다.

그렇다면 어떻게 멋져 보일까요? 데이터 Journey 분석기.

이름 그대로 시스템 내 데이터의 각 Journey를 추적하고, 예상된 흐름에 따라 유효성을 검사하고 의심이 생기면 서버로 오류를 푸시합니다.


<div class="content-ad"></div>

캐싱 예시에서 알고리즘은 다음과 같이 작동합니다:

- 발생한 모든 이벤트를 푸시할 목록을 만듭니다.
- 워크 매니저가 시작될 때 키가 WORK_MANAGER_STARTED인 이벤트를 푸시합니다.
- 이와 유사하게, 다른 키는 CACHE_EMPTY, CACHE_INVALID, FETCHING_FROM_NETWORK, RETRYING, SLOW_INTERNET, DATA_FETCHED, CACHE_EVICTED, PUTTING_INTO_CACHE일 것입니다.
- 이러한 모든 키가 발생한 순서대로 목록에 푸시됩니다.
- 전문가 팁: 이러한 이벤트를 스냅샷이라고 부릅니다. 각각의 이벤트에서 앱의 관련 상태를 스냅샷을 찍는 것처럼 모두 푸시하세요. 타임스탬프도 꼭 푸시하세요.

```js
private val KEY_WORK_MANAGER_STARTED = 0
private val KEY_CACHE_EMPTY = 1
private val KEY_CACHE_INVALID = 2
private val KEY_FETCHING_FROM_NETWORK = 3
private val KEY_RETRYING = 4
private val KEY_SLOW_INTERNET = 5
private val KEY_DATA_FETCHED = 6
private val KEY_CACHE_EVICTED = 7
private val KEY_PUTTING_INTO_CACHE = 8

data class EventSnapshot(
  val key: Int,
  val userId: Long,
  val bytesDownloadedFromServer: Int,
  val isDownloadInProgress: Boolean,
  val cacheSize: Int,
  val timestamp: Long,
  val isInDozeMode: Boolean,
  val appStandbyBucket: Int,
  val networkConnectivity: Int
  // etc...
)

private val eventJourney = mutableListOf<EventSnapshot>()
```

여정 지도가 채워지기 시작하면 핫스팟 이벤트를 찾아야 합니다. 이 경우에는 캐시에 데이터를 넣는 것이 핫스팟 이벤트일 것입니다 (사용 사례에 따라 다름).

<div class="content-ad"></div>

이 블로그의 흥미로운 부분인 이벤트 접근 방식을 분석해 보겠습니다.

앱의 사용 사례에 따라 달라지겠지만, 한 가지 분석 알고리즘은 다음과 같을 수 있습니다: 

- KEY_PUTTING_INTO_CACHE 키를 가진 이벤트가 데이터 구조에 푸시되면 해당 인덱스를 표시합니다.
- 더 많은 이벤트가 발생할 때까지 기다린 후 복사 목록을 만듭니다 (이렇게 하면 데이터 구조에서 발생하는 다른 작업을 동기화할 필요가 없습니다).
- 해당 복사 목록을 분석기에 전달하고 분석 알고리즘을 시작합니다. 여기서 몇 가지 유효성 검사 조건을 확인합니다.
- 예를 들어 KEY_PUTTING_INTO_CACHE 이벤트 전에 KEY_DATA_FETCHED 이벤트가 있어야 합니다.
- 캐시 크기가 가득 차 있지 않아야 합니다.
- 여러분이 고려해야 할 다양한 이러한 유효성 검사가 있을 수 있습니다.
- 이상한 동작을 확인한 후 서버에 이벤트 접근을 기록할 수 있습니다.
- 실제로 뭔가 이상한 버그가 발생하는 경우에만 이벤트를 기록하는 데 도움이 됩니다.
- 참고: OS가 컴포넌트를 메모리를 확보하기 위해 종료하는 경우를 대비해 이러한 이벤트를 영속적으로 만드려고 노력해보세요.

Signal은 데이터베이스 마이그레이션을 실행한 후 비슷한 마이그레이션 검사기를 실행하고 이상한 점이 있는 경우 이를 로그로 남깁니다. 텔레그램은 단편 스택이 비어 있는지 또는 화면이 비어 있는지 확인하고 이에 따라 서버로 로그를 푸시합니다.

<div class="content-ad"></div>

이 분석기들은 프로덕션 환경에서 레이스 조건을 디버그하려고 할 때 자신만으로는 재현할 수 없을 때 정말 유용합니다. 이를 반복해서 사용하여 사용자의 이용자 경로를 보다 잘 이해할 수 있도록 클릭 히트 맵을 만들어낼 수 있습니다.

더 많은 최신 블로그를 보려면 팔로우해주세요. LinkedIn에서 저를 만나보세요: https://www.linkedin.com/in/sidsharma2002/