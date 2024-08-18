---
title: "React 앱과 Nodejs 서버를 Vercel에 배포하는 방법 정리"
description: ""
coverImage: "/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_0.png"
date: 2024-08-04 18:46
ogImage:
  url: /assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_0.png
tag: Tech
originalTitle: "Steps for Deploying React app and Nodejs Server in Vercel And Render"
link: "https://medium.com/@kmahesh18001/steps-for-deploying-react-app-and-nodejs-server-in-vercel-and-render-494737535b5a"
isUpdated: true
---

<img src="/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_0.png" />

## 목차 : -

## Render에 Node.js 서버(백엔드) 코드를 배포하는 단계 :-

- MONGO_URL, PORT, JWT_TOKEN 및 개인 키와 같은 필수 링크가 .env 파일에 포함되어 있는지 확인해주세요.
- 백엔드 코드를 만든 저장소에 푸시하세요 (저장소가 없는 경우 백엔드용으로 새로운 저장소를 만드세요).
- Render.com 웹 사이트로 이동하여 Github 계정을 사용하여 등록하십시오 (선호됨) - 이 방법을 사용하면 옵션에서 모든 저장소가 표시되므로 배포하려는 저장소를 선택할 수 있습니다.
- 로그인 후 New 버튼을 클릭하고 Web Service를 선택하세요.
- 배포할 저장소 이름을 선택하고 연결을 클릭하세요.

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

![Step 1](/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_1.png)

![Step 2](/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_2.png)

6. 빌드 및 배포에는 시간이 걸릴 수 있습니다. 성공적으로 배포된 후 로그에 "Your service is live" 메시지가 표시됩니다. 오류가 발생하면 로그를 확인하고 문제를 해결하세요.

![Step 3](/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_3.png)

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

![이미지](/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_4.png)

## Vercel에 React 앱을 배포하는 단계:-

7. 위 링크를 복사하고 해당 URL을 "localhost:8000" 대신 프론트엔드 파일에 붙여넣고, 아래 이미지 형식으로 파일을 생성하고 파일을 import합니다. 이렇게하면 링크 localhost:8000을 수동으로 추가하지 않아도 됩니다.

![이미지](/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_5.png)

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

<img src="/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_6.png" />

8. 이제, Atlas로 이동하여 `Security` Network Access `Add IP Address` Allow access ` Confirm을 클릭하세요.

9. 프론트엔드에서 cmd를 엽니다.

<img src="/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_7.png" />

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

<img src="/assets/img/2024-08-04-StepsforDeployingReactappandNodejsServerinVercelAndRender_8.png" />

감사합니다,
마헤시쿠마르 카스트리.
