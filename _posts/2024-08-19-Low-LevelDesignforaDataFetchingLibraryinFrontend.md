---
title: "프론트엔드 데이터 가져오기 라이브러리의 로우 레벨 디자인"
description: ""
coverImage: "/assets/img/2024-08-19-Low-LevelDesignforaDataFetchingLibraryinFrontend_0.png"
date: 2024-08-19 02:17
ogImage: 
  url: /assets/img/2024-08-19-Low-LevelDesignforaDataFetchingLibraryinFrontend_0.png
tag: Tech
originalTitle: "Low-Level Design for a Data Fetching Library in Frontend"
link: "https://medium.com/@web.hardikhere/low-level-design-for-a-data-fetching-library-in-frontend-56b789a2edf5"
isUpdated: true
updatedAt: 1724034682939
---


데이터 쿼리 시스템을 구축하는 클래스 기반 접근 방식입니다.

![Link to image](/assets/img/2024-08-19-Low-LevelDesignforaDataFetchingLibraryinFrontend_0.png)

## 왜 라이브러리가 필요한가요?

지난 주 방갈로르에 오픈한 키티 카페에 다녀왔어요. 맛있는 "키티 카레"를 요리하는 키티들을 봤는데, 아직은 레스토랑을 운영하는 법을 모르는 걸로 보였어요. 그곳에서 문제가 있음을 눈치챘죠.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

문제: "키티의 카레"에 대한 여러 주문이 접수되어, 주방에서는 각 주문을 독립적으로 준비하기 시작할 수 있습니다. 이로 인해 각 주문이 필요 이상으로 더 많은 재료와 조리 시간을 사용하는 중복 준비 과정이 발생할 수 있습니다.

영향: 중복 된 재료 낭비 및 불필요한 노력 중복으로 인한 요리 비용 상승.

해결책: 데이터 검색 시스템은 중복 제거 로직을 구현하여 동일한 요리에 대한 여러 주문이 들어오면 이를 단일 조리 과정으로 통합할 수 있습니다.

혜택: 이렇게 함으로써 중복 준비를 줄이고 자원 사용을 최적화할 수 있습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이제 음식 주문을 UI 구성 요소로 고려할 때 서버에서 데이터를 요청하여 렌더링하면서, 독립적으로 동일한 종류의 데이터를 요청할 수 있습니다.

## 이를 달성하는 데 도움이 되는 라이브러리는 무엇인가요?

- useSWR: 가장 가벼우면서 가장 간단한 라이브러리입니다. (평가: 4/5)
- ReactQuery: 가장 인기 있고 강력한 라이브러리입니다. (평가: 5/5)
- RTK Query: Redux와 더불어 강하게 결합되어 있다면, 그러나 개발 경험 측면에서 지나치게 복잡하다고 느꼈습니다. (평가: 3/5)

대부분의 이러한 라이브러리는 React용으로 제작되었으며 구현에 대해 다양한 방법을 사용합니다. 이 블로그에서는 개념을 객체 지향적으로 설명하기 위해 클래스를 사용해 설명할 것이며 함수형 접근 방식을 선호하는 경우 댓글에서 알려 주시면 앞으로의 블로그 포스트에서 다루어 드릴 것이니 참고하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 기능 요구 사항

- 핵심 기능: 다양한 HTTP 메소드 지원, 사용자 정의 구성 및 포괄적인 오류 처리를 통해 효율적인 데이터 검색 가능.
- 캐싱 및 중복 제거: 만료 전략을 사용한 인메모리 캐싱 구현 및 중복 요청 피하며 성능 개선.
- 상태 관리: 로딩, 데이터 및 오류 상태 추적 및 노출하면서 키 라이프사이클 이벤트에 대한 업데이트 이벤트 제공하는 동안 시작 또는 완료된 가져오기와 같은 핵심 라이프사이클 이벤트에 대한 이벤트 제공.
- Stale-While-Revalidate: 백그라운드에서 유효성 재확인하면서 배출 데이터 제공할 수 있고, 구성 가능한 간격에서 자동 데이터 갱신을 지원.

## 핵심 클래스

- DataFetcher: 데이터 검색, 캐시 관리, 로딩, 오류 등 다양한 상태 처리하는 주요 클래스.
- Cache: 무효화 및 사전 검색을 위한 메서드를 포함한 인메모리 캐싱 관리를 위한 유틸리티 클래스.
- EventEmitter: 업데이트, 오류 및 캐시 변경과 같은 이벤트 처리를 위한 유틸리티 클래스.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<img src="/assets/img/2024-08-19-Low-LevelDesignforaDataFetchingLibraryinFrontend_1.png" />

## DataFetcher Class

이것은 우리가 가지고 있는 핵심적이고 가장 중요한 클래스입니다. 이 클래스는 핵심 로직을 관리할 것입니다. 데이터 가져오기, 캐싱 그리고 상태 관리를 포함합니다.

속성:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- fetcher: 데이터를 가져오는 데 사용되는 함수 참조입니다. 어떤 사람들은 fetch를 사용할 수 있고, 어떤 사람들은 axios를 사용할 수 있습니다.
- cache: Cache 클래스의 인스턴스입니다.
- isFetching: 데이터가 현재 캐시에서 가져오는 중인지 여부를 추적하는 부울 값입니다.
- error: 데이터를 가져오는 중에 발생한 모든 오류를 저장합니다.
- data: 가져온 데이터를 보유합니다.
- listeners: EventEmitter 클래스의 인스턴스입니다.
- options: fetcher에 대한 구성 옵션입니다. 예: refreshInterval, dedupingInterval 등.

메소드:

- fetch(key): 주어진 키에 기반하여 데이터를 가져옵니다.
- shouldRevalidate(timestamp): 데이터가 타임스탬프에 기반하여 다시 유효성을 검사해야 하는지를 결정합니다.
- startAutoRefresh(): 데이터를 자동으로 새로 고침할 수 있도록 간격을 시작합니다.
- on(event, listener): 이벤트 리스너를 등록합니다.
- notifyListeners(event, payload): 모든 청취자들에게 이벤트를 알립니다.
- invalidateCache(key): 특정 키에 대한 캐시를 무효화합니다. 원격으로 작업 선호도를 변경했을 때 API가 무효화되고 새로운 데이터로 새로 고침되어야 하는 경우와 같은 여러 경우에 도움이 됩니다.

## Cache Class

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

속성:

- store: 캐시된 데이터와 해당 메타데이터(예: 타임스탬프)를 보유하는 객체입니다.

메서드:

- get(key): 키를 기반으로 캐시에서 데이터를 검색합니다.
- set(key, data): 키와 현재 타임스탬프로 캐시에 데이터를 저장합니다.
- invalidate(key): 캐시에서 데이터를 제거합니다.
- prefetch(key, fetcher): 필요한 시점에 데이터를 가져와 캐시에 저장합니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## EventEmitter Class

속성:

- events: 이벤트 이름과 해당 리스너를 저장하는 객체입니다.

메서드:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- on(event, listener): 이벤트에 대한 리스너를 등록합니다. 주로 on-"fetching" 및 on-"data"라는 두 가지 이벤트를 등록할 것입니다.
- emit(event, payload): 제공된 페이로드로 이벤트에 등록된 모든 리스너를 호출합니다.

## 사용법:

```js
const fetcher = (key) => fetch(`https://your-api-endpoint.com/${key}`).then(res => res.json());

const dataFetcher = new DataFetcher(fetcher, {
  key: 'task-1',
  refreshInterval: 10000,
});

dataFetcher.on('fetching', (isFetching) => {
  console.log('Fetching:', isFetching);
});

dataFetcher.on('data', (data) => {
  console.log('Data received:', data);
});

dataFetcher.fetch('task-1');


dataFetcher.invalidateCache('task-1');


dataFetcher.cache.prefetch('task-2', fetcher);
```

DataFetcher, EventEmitter 및 Cache 클래스를 구현하는 데 도움이 되기를 바랍니다. 코딩하는 동안 Observer 패턴(이벤트 에미터에서 사용), Singleton(캐시의 경우, 그렇게 설계된 경우) 또는 Strategy(다양한 가져오기 전략이 구현된 경우, 예: 롱 폴링)과 같은 디자인 패턴을 익힐 수 있습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

많은 추가 기능을 덧붙일 수 있어요. 예를 들어 페이지별 API 캐싱, 이를 단일 캐시로 병합하고 무효화 태그를 사용하여 한 API가 POST API를 실행할 때 자동으로 무효화되도록 할 수 있어요. 예를 들어, 직업 선호도를 변경하는 API를 실행하면 해당 선호도를 가져오는 API도 자동으로 무효화 태그를 통해 무효화되어야 해요. 현재 우리가 사용하는 방식으로는 수동으로 무효화하고 있어요.

최근 면접에서 실수를 해서 LLD를 시작했어요. 더 공부하면서 더 많은 것을 쓸 거고, 인터뷰 세계에서 틱택토와 같이 독특하고 일반적이지 않은 것들에 대해 배울 거예요.

![Low-Level Design for a Data Fetching Library in Frontend](/assets/img/2024-08-19-Low-LevelDesignforaDataFetchingLibraryinFrontend_2.png)

## 참고문헌과 추가 읽을 거리:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- React Query를 탐험해보세요: [링크](https://medium.com/@dengafred/introduction-to-react-query-for-data-fetching-4173e38710e8)
- LLD 인터뷰 준비를 위해 이 로드맵을 따르고 있어요: [링크](https://medium.com/@sandeep.kumar.ece16/low-level-design-roadmap-7581688d96fa)
- JS에서 클래스 작성 기초: [링크](https://javascript.info/classes)
- 뮤타 고양이는 누구일까요?: [링크](https://www.imdb.com/title/tt0113824/)