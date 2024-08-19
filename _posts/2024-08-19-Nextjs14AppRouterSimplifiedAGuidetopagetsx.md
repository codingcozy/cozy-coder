---
title: "Nextjs 14 App Router page.tsx "
description: ""
coverImage: "/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_0.png"
date: 2024-08-19 02:28
ogImage: 
  url: /assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_0.png
tag: Tech
originalTitle: "Nextjs 14 App Router Simplified A Guide to pagetsx"
link: "https://medium.com/@sagarpatil23399/next-js-14-app-router-simplified-a-guide-to-page-tsx-233131ec039b"
isUpdated: true
updatedAt: 1724034721819
---


Next.js는 파일 시스템 기반 라우터를 사용합니다. 이 라우터를 사용하면 중첩된 경로에서 특정 동작을 가진 UI를 생성할 수 있습니다:

- layout : 세그먼트와 해당 자식들을 위한 공유 UI
- page : 경로의 고유 UI 및 공개 경로 생성
- loading : 세그먼트와 해당 자식들을 위한 로딩 UI
- not-found : 세그먼트와 해당 자식들을 위한 찾을 수 없음 UI
- error : 세그먼트와 해당 자식들을 위한 오류 UI
- global-error : 전역 오류 UI
- route : 서버 측 API 엔드포인트
- template : 특수 재랜더링된 레이아웃 UI
- default : 병렬 라우트에 대한 대체 UI

참고: 특수 파일에는 .js, .jsx 또는 .tsx 파일 확장자를 사용할 수 있습니다.

![이미지](/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_0.png)

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

이 글에서는 Next.js (App Router)의 페이지 파일 역할을 이해하는 데 깊이 파고들어 볼 것입니다.

페이지는 경로에 고유한 UI입니다. 페이지를 정의하는 방법은 page.js 파일에서 컴포넌트를 기본 내보내기하는 것입니다.
해설:
웹 앱에서 "페이지"를 방문하는 특정 주소(또는 "경로")를 방문할 때 보는 특정 화면 또는 뷰로 생각해보세요. 예를 들어, 사이트의 홈페이지, "회사 소개" 페이지 또는 "연락처" 페이지는 모두 다른 "페이지"입니다.

간단히 말하면, 이러한 페이지 중 하나를 만들고 싶다면 프로젝트에서 page.js(또는 TypeScript를 사용하는 경우 page.tsx)라는 파일을 만들면 됩니다. 이 파일 안에 페이지가 어떻게 보일지 설명하는 코드를 작성한 다음, 이 코드를 내보냅니다. 방문자가 해당 경로를 방문할 때 표시되는 것이 이 내보낸 코드입니다.

예를 들어, 인덱스 페이지를 만들려면 앱 디렉토리 안에 page.js 파일을 추가하십시오:

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

<img src="/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_1.png" />

<img src="/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_2.png" />

그리고 더 많은 페이지를 만들려면 새 폴더를 만들고 해당 폴더 안에 page.js 파일을 추가하세요. 예를 들어, /dashboard 경로에 대한 페이지를 만들고 싶다면 dashboard라는 새 폴더를 만들고 그 안에 page.js 파일을 추가하세요:

참고:
1. 페이지는 .js, .jsx 또는 .tsx 파일 확장자를 사용할 수 있습니다.
2. 페이지는 항상 루트 서브트리의 말단입니다.
3. 말단: 자식이 없는 서브트리의 노드로, URL 경로의 마지막 세그먼트와 같은 것입니다.

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


![이미지1](/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_3.png)

4. 루트 세그먼트를 공개로 만들려면 page.js 파일이 필요합니다.
5. 페이지는 기본적으로 서버 컴포넌트이지만, 파일 상단에 "use client" 지시문을 사용하여 클라이언트 컴포넌트로 설정할 수 있습니다. 이 때, import 문 위에 위치해야 합니다.

![이미지2](/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_4.png)

![이미지3](/assets/img/2024-08-19-Nextjs14AppRouterSimplifiedAGuidetopagetsx_5.png)


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

6. 페이지에서 데이터를 가져올 수 있습니다.