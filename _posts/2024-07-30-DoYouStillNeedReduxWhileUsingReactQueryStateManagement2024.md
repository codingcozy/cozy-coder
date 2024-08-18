---
title: "2024년 상태 관리 트렌트인 React Query 개념 및 정리"
description: ""
coverImage: "/assets/img/2024-07-30-DoYouStillNeedReduxWhileUsingReactQueryStateManagement2024_0.png"
date: 2024-07-30 17:25
ogImage:
  url: /assets/img/2024-07-30-DoYouStillNeedReduxWhileUsingReactQueryStateManagement2024_0.png
tag: Tech
originalTitle: "Do You Still Need Redux While Using React Query  State Management 2024"
link: "https://medium.com/javascript-in-plain-english/do-you-still-need-redux-while-using-react-query-state-management-2024-f4d0ce881946"
isUpdated: true
---

![이미지](/assets/img/2024-07-30-DoYouStillNeedReduxWhileUsingReactQueryStateManagement2024_0.png)

리액트에 처음 뛰어들 때에는 Redux가 상태 관리 라이브러리로 가장 인기 있었습니다. 매 자습서와 블로그 글이 그의 가치를 높게 평가하며 송술하고 있었죠.

Redux의 예측 가능한 상태 컨테이너와 강력한 생태계는 몇 년 동안 리액트 세계에서 필수적인 요소가 되어 왔습니다. ReduxSaga와 ReduxThunk은 모두 비동기 요청을 처리하는 아름다운 방법들이었지만, 시대가 변하고 있습니다.

그러나 프론트엔드 생태계가 발전함에 따라 우리의 도구와 방법론도 발전하고 있습니다. 여기에 React Query가 등장했는데, 이 강력한 데이터 가져오기 라이브러리는 상당한 인기를 얻고 있습니다.

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

2024년, React Query를 사용하는 동안 Redux를 여전히 필요로 할까요?

# React Query 이해하기

Tanner Linsley가 만든 React Query는 React 애플리케이션에서 서버 측 상태를 관리하기 위해 설계된 라이브러리입니다. Redux가 일반적인 상태 관리 도구인 반면, React Query는 비동기 데이터 가져오기와 캐싱에 중점을 둔 라이브러리입니다.

![이미지](/assets/img/2024-07-30-DoYouStillNeedReduxWhileUsingReactQueryStateManagement2024_1.png)

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

서버 데이터를 관리하는 복잡성을 간소화하여 데이터 가져오기, 캐싱, 동기화 및 서버 상태 업데이트에 대한 후크를 제공합니다.

# React Query의 장점

- 단순화된 데이터 가져오기: React Query는 데이터 가져오기와 관련된 보일러플레이트 작업을 추상화하여 로딩 상태, 오류 및 재시도 관리 등을 간소화합니다. useQuery 및 useMutation과 같은 후크를 사용하면 데이터 가져오기가 직관적이고 간결해집니다.
- 자동 캐싱 및 백그라운드 업데이트: React Query는 기본 캐싱 및 stal-while-revalidate 전략을 제공합니다. 이는 데이터가 캐시되어 만료된 경우 자동으로 백그라운드에서 다시 가져와 화면이 항상 최신 상태를 유지할 수 있도록 합니다.
- 낙관적 업데이트: React Query를 사용하면 UI가 즉시 업데이트되어 사용자 경험이 개선되는 낙관적 업데이트를 간단히 구현할 수 있습니다.
- 개발 도구: React Query 개발 도구는 쿼리와 상태에 대한 상세한 정보를 제공하여 디버깅을 쉽게 할 수 있게 해줍니다.
- 서버 상태 관리: React Query는 서버 상태에만 집중하여 응용 프로그램의 상태 관리를 간소화할 수 있습니다.

![이미지](/assets/img/2024-07-30-DoYouStillNeedReduxWhileUsingReactQueryStateManagement2024_2.png)

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

# React Query의 단점

- 서버 상태에 제한됨: React Query는 클라이언트 상태(로컬 상태)를 관리하기 위해 설계되지 않았습니다. 복잡한 클라이언트 측 상태 관리를 위해 다른 상태 관리 솔루션이 필요할 수 있습니다.
- 학습 곡선: Redux를 사용하여 서버 상태를 관리하는 것보다 쉽지만, React Query는 자체적인 개념과 훅을 도입하여 학습이 필요합니다.
- 잠재적인 오버헤드: 매우 간단한 애플리케이션의 경우, 추가적인 추상화 계층이 과도할 수 있습니다.

# Redux: 전투에서 검증된 베테랑

반면에 Redux는 클라이언트 측 및 서버 측 상태를 모두 관리할 수 있는 강력하고 유연한 상태 관리 도구입니다. 단방향 데이터 흐름, 미들웨어 생태계 및 시간 여행 디버깅을 통해 대규모 애플리케이션에 대한 견고한 선택지입니다.

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

# Redux의 장점

- 전역 상태 관리: Redux는 전역 상태를 효과적으로 관리하여 응용 프로그램에 대한 단일 진실의 원천을 제공합니다.
- 미들웨어 및 생태계: Redux의 생태계는 방대하며, Redux Thunk 및 Redux Saga와 같은 미들웨어가 부가 효과 및 비동기 논리 처리에 강력한 도구를 제공합니다.
- 예측 가능한 상태 업데이트: Redux의 엄격한 상태 업데이트 규칙은 예측 가능하고 유지 보수가 쉬운 코드를 유발하며, 특히 대규모 애플리케이션에서 유용합니다.
- 커뮤니티 및 지원: Redux는 큰 커뮤니티와 방대한 문서를 갖고 있어 지원 및 자료를 쉽게 찾을 수 있습니다.

# Redux의 단점

- 보일러플레이트 코드: Redux는 액션 생성자부터 리듀서까지 많은 보일러플레이트 코드가 필요한데, 이는 입문자에게는 혼란스럽고 소규모 프로젝트에서는 불편할 수 있습니다.
- 간단한 사용 사례의 복잡성: 간단한 상태 관리가 필요한 애플리케이션의 경우, Redux는 과도한 엔지니어링 느낌을 줄 수 있습니다.

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

# React Query와 Redux를 언제 사용해야 할까요?

![이미지](/assets/img/2024-07-30-DoYouStillNeedReduxWhileUsingReactQueryStateManagement2024_3.png)

## React Query 사용 시 주의사항:

- 앱이 데이터 중심일 때: 애플리케이션이 서버 데이터에 많이 의존하고 데이터 검색 요구 사항이 복잡한 경우, React Query의 캐싱, 백그라운드 업데이트 및 쿼리 무효화 기능이 여러분의 삶을 간단하게 만들어줄 것입니다.
- 데이터 검색 단순화를 원할 때: React Query의 직관적인 후크(Hooks)는 서버 데이터 주변의 데이터 검색 및 상태 관리를 단순화합니다.
- 전역 클라이언트 상태 관리가 필요 없을 때: 앱의 클라이언트 측 상태 관리가 최소화되고 React의 내장 상태 또는 Context API로 처리할 수 있는 경우, React Query는 훌륭한 선택입니다.

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

## 리덕스와 함께 사용할 때:

- 포괄적인 상태 관리가 필요할 때: 애플리케이션이 서버 사이드 상태에 추가로 클라이언트 사이드 상태 관리가 필요한 경우, 리덕스는 통합된 방식을 제공합니다.
- 미들웨어에 의존하는 경우: 애플리케이션이 사이드 이펙트와 복잡한 비동기 플로우에 크게 의존하는 경우, 리덕스 미들웨어인 Thunk나 Saga와 같은 것이 유용할 것입니다.
- 기존의 리덕스 인프라가 있을 때: 이미 리덕스를 사용하고 있는 프로젝트의 경우, React Query를 도입하면 명확한 혜택이 없는 한 불필요한 복잡성을 추가할 수 있습니다.

# 2024년 실제 사용 사례 및 인기

2024년에는 React Query가 React 애플리케이션에서 데이터 가져오기를 위한 인기 있는 선택지로 자리를 굳혔습니다.

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

Netflix, Walmart, 그리고 GitHub와 같은 기업들이 React Query를 도입하여 데이터 관리 프로세스를 간소화했습니다. 예를 들어 Netflix에서는 React Query가 복잡한 데이터 가져오기 요구사항을 처리하는 데 중요한 역할을 하여 UI의 장황함을 줄이고 성능을 향상시키는 데 도움이 되었습니다.

그러나 Redux는 아직 구식이 아닙니다. 광범위하고 복잡한 상태 관리가 필요한 대규모 기업들은 여전히 Redux의 유연성과 생태계에 의존하고 있습니다.

선택은 종종 프로젝트의 특정 요구 사항과 팀이 도구에 대한 친숙도에 달려 있습니다.

# 결론

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

프런트엔드 개발의 변화하는 풍경에서 React Query를 사용할 때 Redux가 필요한지는 애플리케이션의 특정 요구 사항에 따라 다릅니다. React Query는 서버 측 상태를 관리하고 데이터 가져오기를 간편하게 하며 보일러플레이트를 줄이는 데 빛을 발합니다.

Redux는 특히 복잡한 클라이언트 측 상태를 가진 대규모 애플리케이션에서 튼튼한 선택지로 남아 있습니다.

어떤 도구든 프로젝트와 팀의 요구 사항을 평가하는 것이 중요합니다. Redux와 React Query는 각자의 장점을 가지고 있으며 필요에 따라 동일한 애플리케이션에서 공존할 수 있습니다.

핵심은 각각의 강점을 활용하여 유지보수 가능하고 성능이 우수하며 확장 가능한 애플리케이션을 구축하는 데 있습니다.

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

React Query를 사용하면서 Redux가 여전히 필요한가요? 때에 따라 다릅니다. 모두 당신의 애플리케이션의 고유한 요구 사항에 달려 있어요. 즐거운 코딩하세요!

만세!

# 간단하고 명료한 언어로 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 계속 읽기 전에:

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

- 작가의 글에 박수를 보내고 팔로우해 주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠는 PlainEnglish.io에서 확인하세요
