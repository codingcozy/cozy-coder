---
title: "성능 향상 Faire가 NextJS로 전환한 이유와 방법"
description: ""
coverImage: "/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_0.png"
date: 2024-08-03 18:37
ogImage:
  url: /assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_0.png
tag: Tech
originalTitle: "Boosting performance Faires transition to NextJS"
link: "https://medium.com/faire-the-craft/boosting-performance-faires-transition-to-nextjs-3967f092caaf"
isUpdated: true
---

## Faire가 성숙한 단일 페이지 애플리케이션에서 서버 측 NextJS 프레임워크로의 이관을 어떻게 처리했는지

![이미지](/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_0.png)

글쓴이: Luke Bjerring, Jude Gao, Chris Krogh

Faire에서는 속도가 중요합니다. 저희 제품은 번개처럼 빠르게 동작하여 고객이 자신의 목표를 빨리 달성할 수 있어야 합니다. 고객들이 마켓플레이스에 자신의 가게를 설정하거나 원하는 제품을 찾거나 다른 작업을 빨리 하고 사업을 운영할 수 있도록 도와야 합니다.

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

작년 동안 Faire 엔지니어링 팀은 고객 경험을 개선하기 위해 많은 속도 개선을 우선시했습니다. 더 큰 프로젝트 중 하나는 NextJS 프레임워크로의 이주였습니다. 이 기사에서는 변경 배경, 직면한 장벽, 지금까지의 영향, 그리고 우리가 그 과정에서 발견한 기회에 대해 공유할 것입니다. SPA와 SSR 간의 차이, 수렴된 코드베이스의 점진적 이주 처리 방법, 그리고 귀사에 특화된 도전 과제들도 소개할 것입니다 — 예를 들어 우리 서비스 아키텍처의 반전, NextJS 보정을 위한 스키퓌시 보호 및 즉석 로컬라이제이션, 그리고 우리만의 섬세하게 조정된 성능 측정 방법 등이 있습니다. 계속해서 읽어보세요.

# 이전 코드베이스 구조

faire.com에서 실행되는 웹 애플리케이션은 React를 사용하여 작성되었으며, 이는 웹 인터페이스에 널리 사용되는 라이브러리입니다. Faire의 제품 요구사항이 발전함에 따라 코드베이스의 복잡성이 증가하면서로드 성능에 영향을 미쳤습니다. 초기 웹 애플리케이션 아키텍처에서 우리는 API와 웹페이지를 동일한 단일 서비스에서 제공하는 방식으로 간단히 유지했습니다. 이는 Kotlin 서버를 통해 Mustache 템플릿을 사용하여 HTML 응답을 작성하고, SPA(단일 페이지 애플리케이션) JavaScript 번들을 특징으로했습니다.

SPA 웹페이지를 렌더링하려면 브라우저가 순차적으로 필요합니다:

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

- HTML을 요청하고 응답을 구문 분석합니다.
- JavaScript 번들을 가져 와서 파싱한 후 실행합니다.
- 뷰별 데이터를 가져 와서 DOM을 렌더링합니다.
- DOM에 있는 이미지를 가져옵니다.

<img src="/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_1.png" />

이 시퀀스는 웹 페이지 속도가 JavaScript 번들을 빨리 가져 와서 실행하는 데 종속되어 있음을 의미합니다. SPA의 핵심 JavaScript 번들 크기를 합리적으로 유지하기가 어려워졌습니다. 공급 업체 번들, 코드 분할 및 기타 최적화와 같은 기술을 사용하더라도 최종 사용자의 로딩 시간이 저하되는 것을 보았습니다. 더 현대적인 아키텍처를 도입할 때입니다.

일부 연구를 한 후, 렌더링을 NodeJS로 이동하고 SSR(서버 측 렌더링) 및 다른 더 현대적인 기술을 활용하고자 합니다. 이전을 NextJS와 같은 프레임워크를 활용하여 관리하기가 훨씬 더 쉬울 것입니다. 이는 페이지 로드 시간을 줄이는 데 필요한 기능 세트를 제공합니다. 여기서 질문은 다음과 같았습니다. "Kotlin과 Mustache로 제공되는 SPA를 전면적인 NextJS 서버로 어떻게 옮겨야 하며, 전체 앱을 다시 작성하지 않아도 되는가?"

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

# 왜 NextJS를 선택했나요?

우리는 기존 개발 스택과 가장 잘 어울리는 프레임워크를 찾기 위해 노력했습니다. Remix, NextJS, Fresh/Deno 등 여러 옵션을 고려해보았습니다. 속도가 최우선 사항이었으며 프레임워크의 성숙도, 커뮤니티 참여, 예상 이주 작업, 개발자 경험 등 다른 측면의 필요를 균형있게 고려한 결과, 우리는 NextJS를 선택했습니다.

우리의 싱글 페이지 애플리케이션은 React Router를 활용했는데, 이는 Remix를 선호하는 이유 중 하나였습니다 (Remix와 React Router는 서로 강력하게 일치하여 지금은 통합을 계획 중이었습니다). 하지만 NextJS는 React의 전체 스택 아키텍처 비전을 완전히 구현한 기능들이 있었고(Server Components, 그리고 Suspense를 통한 데이터 가져오기 등). NextJS와 React 간의 밀접한 협력은 프로젝트의 미래에 대한 좋은 징조였으며, 다른 규모의 대기업들도 NextJS를 채택한 것을 알아두었습니다 — 또 다른 긍정적인 신호라고 판단했습니다. Remix로 마이그레이션하는 것보다 NextJS로 마이그레이션하는 것이 더 많은 미지수를 동반할 수 있었지만, 장기적인 성공을 위해 우리를 잘 준비시키고 이후 대규모 마이그레이션을 피하기를 원했습니다.

또한, 위에서 설명한 대로 우리의 번들 크기가 몇 년 간 커져 초기 로딩 성능에 악영향을 미쳤습니다. 서버에서만 더 많은 컴포넌트를 렌더링하는 것이 브라우저에서 수행하는 작업량을 줄여줄 것이라고 확신했습니다.

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

# 성능 기준 설정

로드 성능의 영향을 더 잘 이해하기 위해, 우리는 자체적으로 “페이지 로드 타이밍” 이벤트를 측정했습니다. 이러한 이벤트는 다음과 같은 다양한 단계를 포함했습니다:

- 페이지 요청 시 HTML 요청
- HTML 헤드가 파싱될 때 HTML 초기화
- HTML 본문이 파싱될 때 HTML 완료
- DOMContentLoaded 이벤트가 발생할 때 리소스 완료
- 로드 이벤트가 발생할 때 로드 완료

이러한 이벤트를 통해 우리는 두 가지 중요한 결과를 도출했습니다:

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

## 1. 사용자들이 로드가 완료되기 전에 사이트를 떠나고 있어요

우리는 단순히 보고된 각 이벤트의 숫자를 조사하여 HTML 초기화 후 각 단계에서의 이탈 정도를 측정했어요. 데이터에 따르면 사용자들이 페이지가 로드될 기회조차 주어지기 전에 포기하고 브라우저 탭을 닫고 있었어요. 이는 산업 연구 결과와 일치했어요 (이 훌륭한 일러스트들의 모음을 확인해보세요). 저희의 대부분의 로드 시간이 10초 이상이었으며, 이는 5초가 넘는 산업 표준 목표에 크게 못 미치는 수준이었어요.

## 2. LCP와 로드 시간은 모두 완전한 측정치가 아니에요

LCP (가장 큰 콘텐츠 페인트)와 로드가 완료되는 시간 간의 큰 시간적 차이가 있어요 (특히 SSR을 나중에 구현한 후). 우리는 실무에서 두 지표 중 어느 것도 사용자가 페이지가 실제로 로딩이 완료되었다고 느낄지 정확하게 전달하지 못한다는 것을 발견했어요. LCP는 페이지가 로드된 것으로 보이지만 상호작용이 불가능하다는 것을 알려줄 뿐이에요. 반면에, "로드 완료"는 하단 컨텐츠에만 영향을 주는 사소한 에셋이나 데이터와 같은 중요하지 않은 네트워크 활동을 기다리는 데에 과다하게 영향을 받을 수 있어요.

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

우리의 요구에 맞는 메트릭도 충족시키지 못하여, 페이지 로드 타이밍 이벤트를 더 정제하기 위해 다시 빈 도면으로 돌아왔습니다.

# UPLT: 사용자 인식 로드 시간

UPLT(사용자 인식 로드 시간)는 페어의 내부 지연 시간 메트릭으로, 페이지 성능의 세부 사항을 고려하도록 설계되었습니다. UPLT를 구현하여 브라우저와 DOM 라이프사이클에서의 활동에 반응하는 보고 메커니즘을 만들어 여러 로딩 단계를 더 상세히 주석 처리하고 페이지가 "로드된 것으로 느껴지는" 순간을 명시적으로 표시했습니다. 이는 페어의 웹 애플리케이션에서 극명하게 다른 인터페이스마다 각각의 적절한 완료 지점을 명확히 정의함으로써 TTFB, FCP, LCP, 로드 시간 등 다양한 기본 메트릭의 가치 차이를 피하도록 했습니다.

예를 들어, 공백 페이지, 로딩 스켈레톤의 SSR, 자바스크립트 로딩 후 수화된 DOM, 제품 이미지 등의 로드 단계들을 가질 수 있습니다. 사용자는 제품 이미지가 모두 보여지고 페이지가 상호 작용할 수 있는 준비가 되었을 때까지 페이지가 로드된 것으로 느끼지 않을 것입니다.

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

<img src="/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_2.png" />

HTML 수명주기 이벤트와 마찬가지로, 우리는 지연이 어디서 발생하는지 이해하기 위해 UPLT 분석을 만들었습니다. 우리는 DOMContentLoaded와 로드 이벤트를 우리만의 UPLT 측정항목인 렌더 및 마운트, 데이터 가져오기, 이미지 가져오기 등의 단계로 세분화했습니다. 케이크 바닥을 지배하고 있는 두드러지는 단계가 있었습니다: HTML 및 JS 로드입니다.

# 코틀린에서 노드JS로 이동

더 현대적인 스택으로 나아가기 위한 길 중에서 첫 번째 중대한 변경 사항은 React SSR을 차단 해제하기 위해 문서를 노드JS 환경에서 렌더링해야 한다는 것이었습니다. 이를 위해 우리의 코드베이스가 노드 친화적이어야 했습니다.

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

저희 애플리케이션의 코드는 과거에 브라우저 수명주기 내에서 실행될 것으로 가정하고 작성되었습니다. 그에 따라 다양한 브라우저 API가 코드베이스 전체에 퍼져 있었습니다. 몇 가지 간단한 예시는 다음과 같습니다:

- window.location 변경과 같은 브라우저 네비게이션
- resize와 같은 이벤트 리스너
- fetch API(Node 18 이후로는 더 이상 문제가 되지 않음)

우리는 lint 규칙을 사용하여 브라우저 API의 직접 사용을 식별하고 금지하며, jsdom을 사용하여 서버 컨텍스트에서 그들을 스텁 처리했습니다.

# 렌더링 요청

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

우리의 React 애플리케이션 코드가 서버 친화적인 것으로 바뀌면서, 서버 아키텍처와 NodeJS 요청 프레임워크에 대한 결정을 내리게 되었습니다. HTTP 요청을 직접 처리해야 할 필요성을 느꼈지만, 인증 및 요청 제한과 같은 Kotlin 스택에만 존재하는 독점적인 요청 동작 목록이 있었습니다.

가장 간단한 해결책은 요청 흐름을 그대로 유지한 채 문서 HTML 렌더링 단계를 NodeJS로 위임하는 것이었습니다 (Mustache를 대체). Faire에서는 프로토버퍼를 광범위하게 활용하고 있어서, 백엔드 모놀리스와 통신하기 위해 gRPC를 사용하기로 결정하여, HTTP 요청 처리를 NodeJS로 처리하는 주요 아키텍처 변경을 미루기로 했습니다.

![이미지](/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_3.png)

우리는 gRPC 서버를 추가하고, NodeJS 이진 파일을 Docker로 묶어 클러스터에 배포하고 이를 백엔드 모놀리스 Kotlin 서비스와 함께 인접하게 두었습니다. 백엔드 모놀리스는 HTML을 위해 중첩된 요청을 NodeJS 서버에 전송하고, 응답을 클라이언트로 전달했습니다.

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

# 서버 측 렌더링

이제 우리는 NodeJS에서 HTML을 렌더링하고 React SSR을 활용할 수 있었습니다. 가능한 빨리 영향을 측정하고자 했었는데, 서버-dom API를 활용하는 것이 충분히 간단할 것으로 낙관적으로 생각하고 우리만의 구현에 도전했습니다.

우리는 애플리케이션 코드가 창(window)에서 기대하는 "전역 데이터"를 zone.js를 사용하여 분리된 "존"에서 가져오게끔 추상화했습니다. 서버 측 HTML 결과를 응답 본문(body)으로 보냈고, 기존 DOM을 수분화(hydrate)하도록 클라이언트 측 부트스트래핑 코드를 조정했습니다.

지금은 서버 측 렌더링을 일부 지원하면서, HTML을 서버에서 렌더링하고 클라이언트에서 수분화하여 LCP를 50% 이상 줄일 수 있었습니다. 🔥

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

<img src="/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_4.png" />

이게 멋있어 보였지만 아직 축하할 시간은 아니었습니다. 이것으로 LCP가 크게 향상되었지만, 우리의 사용자 정의 SSR 구현은 성능 저하를 초래했습니다. SSR 단계가 HTML 요청에 약 100ms의 오버헤드를 추가했습니다. 게다가, 우리의 사용자 정의 SSR 플로우는 초기 로드에만 영향을 주었고, 그 이후의 탐색에는 영향을 주지 않았습니다.

상충관계 외에도, 우리가 작성한 사내 구현은 유지 관리하기 어려웠으며, 메모리 누수에 취약했고 특정 화면에 적용하기 어렵다는 문제가 있었습니다. 전역 데이터의 복잡성으로 인해 수분화 오류가 발생했기 때문입니다. 최선을 다해도 브라우저 API에 대한 잘못된 가정으로 인해 코드의 긴 꼬리가 있었으며, 구현이 잘 작동하지 않았습니다.

우리는 손실을 줄이기로 결정했고, NextJS의 구현을 채택하기로 했습니다.

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

# 라우터 어댑터

NextJS를 활용하는 데 필요한 노력을 최소화하기 위해 팀은 재작성을 피하려는 목표를 가졌습니다. 이 목표는 React Router용 어댑터인 ReactRouterCompat을 사용하여 SPA의 역호환성을 구축함으로써 달성되었습니다.

![image](/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_5.png)

이 층은 NextJS의 App Router와 기존 SPA의 레거시 React Router 사이의 히스토리 라이프사이클을 전파했습니다. 어댑터 층이 구축되면 gRPC 핸들러를 변경하여 해당 HTTP 요청의 동등한 합성을 만들고 커스텀 NextJS 서버를 통해 플럼하여 HTML을 처리할 수 있었습니다. 와! 이제 SPA 또는 NextJS로 트래픽을 제공할 수 있습니다!

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

두 프레임워크를 관리하여 트래픽 수요를 충족시키기 위해서는 주의 깊은 유지보수가 필요합니다. 엔지니어들이 코드를 두 프레임워크에서 테스트할 것을 기대하는 것은 가능하지만, 단순히 규율에만 의존하는 것으로는 확장성이 부족합니다. 이에 대응하기 위해, 우리는 루트 정의가 별도의 폴더에 선언되어 있고 React Router와 NextJS Router와 독립적으로 이 공통 폴더에서 루트를 가져오도록 구현된 코드 패턴을 도입했습니다.

이 접근 방식은 루트 로직에 대한 모든 변경이 중앙 집중화되어 양쪽 프레임워크가 각 업데이트를 매끄럽게 채택할 수 있도록 보장합니다. 이 코딩 패턴은 끊임없는 수동 테스트가 필요 없이 루트 일관성을 유지하는 데 큰 도움이 되었습니다.

이와 함께, 우리는 제품 환경에서 첫 번째 무해 실험을 실행하여 성능과 비즈니스 지표를 신중하게 모니터링하여 경험의 악화 여부를 확인했습니다. 몇 가지 버그 수정과 재시작이 필요했습니다. 세 번째 시도에서 실험은 성공을 거두어 (충분한 데이터를 샘플링하여 오차 범위로 95% 자신할 수 있음) 어떤 것도 퇴보시키지 않은 채로 진행되었고, 얕은 NextJS 구현에 대한 처리를 배포할 수 있었습니다 — 우리의 첫 번째 주요 이정표입니다!

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

# NextJS-at-the-front

NextJS를 롤아웃하는 다음 단계는 아키텍처를 뒤집어서 NextJS가 초기 HTTP 요청을 수신하도록 하는 것이었습니다. 우리의 모놀리틱 백엔드가 아닌 NextJS가 초기 요청을 받도록 하기 위해 여전히 요청 스택에서 Kotlin 요소를 활용해야 했기 때문에 클라이언트와 NextJS 사이에 얇은 웹 프록시 서비스를 도입했습니다.

![이미지](/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_7.png)

그런 다음 NodeJS 서비스는 렌더링에 필요한 데이터를 가져오기 위해 서버 측 데이터 수집을 실행합니다. 이러한 아키텍처에는 응답 스트리밍을 포함한 몇 가지 주요 장점이 있었습니다. 그러나 다음으로 해치지 않는 실험을 롤아웃하면서 한 가지 문제가 발생했습니다 - 카나리 배포 프로세스가 NextJS에 하드 리로드를 발생시키고 있었습니다.

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

# 스큐 보호

서버와 클라이언트에서 실행 중인 코드 간의 드리프트를 피하기 위해 NextJS는 연이어 발생하는 요청 사이에서 빌드 ID가 변경되었을 때 이를 감지하는 메커니즘을 갖고 있습니다. ID가 변경되었다고 감지되면 클라이언트에서 강력한 리로드를 실행하여 새로운 NextJS 빌드가 로드되고 클라이언트와 서버 간의 드리프트가 없음을 보장합니다.

이 기능은 기존의 캐너리 배포 시나리오와 잘 맞지 않았습니다. 캐너리 배포는 Linkerd의 임시 트래픽 분할을 사용하여 트래픽을 안정 버전 또는 캐너리로 무작위로 보내는 방식을 채택했습니다. 롤아웃 중에는 우리의 서비스 아키텍처가 실제로 다음과 같이 보였습니다:

![이미지](/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_8.png)

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

카나리아 또는 안정버전을 받게 되는 것은 우연에 따라 결정됩니다. 따라서 50/50 카나리아에서는 각 요청마다 시작한 빌드와 다른 빌드에 50%의 확률로 접근하게 되므로 하드 리로드를 유발할 확률이 있습니다. 저희는 메트릭을 수집하기 위해 카나리아 상태를 1시간 이상 유지하기 때문에 이 부정적인 영향이 상당히 컸습니다.

현재는 이 버전 스큐 문제에 대한 해결책이 있지만, 우리는 가장 빠른 전진 방법은 초기 렌더의 카나리아 역할을 클라이언트에서 서버로 전파하고, 트래픽 분할을 "스티키"하게 만들어서 프록시된 요청을 직접 해당 카나리아 역할로 보내는 것이라고 결정했습니다.

# 지역화

저희 SPA와 NextJS 사이의 또 다른 중요한 차이점은 지역화 작업 방식입니다. 우리 SPA에서는 각 지역에 대한 고유한 JS 번들을 생성하도록 번역물을 인라인으로 처리하는 오픈 소스 커스텀 바벨 플러그인을 사용합니다. 이렇게 하면 번역을 가져오기 위한 데이터 요청이나 사용되지 않는 번역을 JavaScript에 번들링하는 번거로움을 피할 수 있습니다.

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

NextJS에서 로컬라이제이션을 위한 전략은 React Server Components에 의존하며, 번역된 콘텐츠를 클라이언트로 전송합니다. 이는 모든 것이 클라이언트 컴포넌트인 기존 코드베이스가 번역되지 않을 것을 의미했습니다. NextJS에서 로컬라이제이션 제한을 극복하기 위해 사용자 정의 Babel 플러그인을 사용하여 로케일별 JavaScript 파일을 생성하여 원본 빌드를 여러 로케일 버전으로 변환했습니다.

![이미지](/assets/img/2024-08-03-BoostingperformanceFairestransitiontoNextJS_9.png)

우리의 사용자 정의 Node.js 서버에서는 응답 스트림을 가로채서 사용자의 로케일에 기반하여 동적으로 JS 번들 CDN URL을 지역화된 파일로 보완하는 방식을 사용했습니다. 이 특별한 접근 방식은 공식 NextJS 자료에 문서화된 표준 방법을 뛰어넘어 속도와 효율성을 최적화하며 중요한 클라이언트 측 로직 변경 없이 해결했습니다.

# 영향

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

우리의 NextJS 여정은 아직 진행 중이지만, 우리는 전략에 대한 확신을 쌓고 작업의 영향을 정량화하기 위해 여정 동안 다양한 A/B 실험을 진행해왔습니다. 지금까지 찾아낸 결과는 다음과 같습니다:

- SSR 구현으로 50% 이상 빠른 LCP
- 새로운 NextJS 아키텍처로 이전해 20% 이상 빠른 FCP
- 서버 컴포넌트 리라이트로 인해 공통 랜딩 페이지의 고객 포기율이 40% 이상 줄어들었습니다.
- NextJS에서 `head` 태그를 스트리밍하여 50% 이상 빠른 TTFB

# Faire의 SSR 여정에서 다음은 무엇인가요?

우리는 NextJS와 함께 오는 가능성들에 매우 흥분하고 있습니다. 우리는 최신 기술들을 탐구할 수 있는 아키텍처를 출시했습니다.

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

- 응답 스트리밍: 클라이언트에 `head` 내용을 전송하고 문서 처리를 더 빨리 시작할 수 있으며, 자산을 가져오거나 서버 측 데이터 검색을 점진적으로 클라이언트로 전달할 수 있습니다.
- 서버 구성요소: NextJS는 서버 구성요소를 지원하며, 클라우드로 많은 구성 요소 렌더링 로직을 전송하고 렌더링된 UI를 클라이언트로 스트림으로 전송할 수 있습니다.
- 부분 사전 렌더링: 실험적이긴 하지만, 빌드 시간에 일부 정적 콘텐츠를 컴파일하여 변경 빈도가 낮은 데이터를 가져오는 필요성을 줄일 수 있는 기회가 있다고 확신합니다, 예를 들어 head 메타 태그 같은 데이터.

우리의 초기 NextJS 아키텍처 변경을 출시한 뒤, Faire는 이제 특정 표면에 대한 페이지 렌더링에 대한 혁신적인 방법을 탐색할 준비가 되었습니다. 이미 React 프레임워크 없이는 이룰 수 없었던 서버 측 데이터 검색에서 성공을 보았습니다.

곧 무엇이 다가올지 주목하세요! 우리는 NextJS를 활용할 수 있는 코드베이스를 업데이트하기 위해 수많은 작업이 남아 있습니다. mobx에서 hooks로의 이관을 완료하고, 서버 구성요소를 지원하도록 설계 시스템을 재구현하고, 서버 측 데이터 검색 및 로딩 상태처럼 NextJS 라이프사이클을 활용하도록 대부분의 뷰를 다시 작성해야 할 것입니다...

작업이 끝없이 이어지고 우리는 그에 흥분합니다! Faire는 엔지니어링 품질과 사용자 경험에 자부심을 가지며 빠른 로딩이 그 경험에 중요한 부분임을 강조합니다. 🏎️

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

Faire 전체 프론트엔드 팀에 대한 거대한 프로젝트였지만, 루크 비제링, 트로이 타오, 크리스 크로그, 쥬드 가오, 조슬린 스테릭커, 라즐로 판디, 그리고 블레어 맥알파인이 기여한 노고 없이는 성공적으로 이뤄지지 않았을 것입니다!

전 세계의 소상공인을 지원하는 흥미로운 작업을 하는 팀에 참여하고 싶으신가요? 오픈된 역할에 지원하세요: https://www.faire.com/careers
