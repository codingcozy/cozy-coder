---
title: "JavaScript에서 consolelog할 때 줄 바꿈 안되게 하는 방법 "
description: ""
coverImage: "/assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_0.png"
date: 2024-07-29 14:05
ogImage:
  url: /assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_0.png
tag: Tech
originalTitle: "How to consolelog without newline in JavaScript"
link: "https://medium.com/coding-beauty/javascript-console-log-without-newline-20e7e63cca36"
isUpdated: true
---

<img src="/assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_0.png" />

많은 개발자들이 사용해보지 않은 새로운 방법으로 콘솔에 새 줄 없이 로그를 남길 수 있습니다.

다음과 같이 반복문에서 로그를 남겨야 할 때:

<img src="/assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_1.png" />

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

일반 console.log는 작동하나요?

아니요:

![console.log without newline](/assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_2.png)

그러면 어떻게 해야 하죠?

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

우리가 하는 일은 `process.stdout.write`입니다.

![예시 이미지 1](/assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_3.png)

`process.stdout.write`는 새 줄 없이 콘솔에 출력합니다.

![예시 이미지 2](/assets/img/2024-07-29-HowtoconsolelogwithoutnewlineinJavaScript_4.png)

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

# process.stdout.write와 console.log의 차이점은 무엇인가요?

먼저, console.log은 사실 process.stdout.write와 같습니다!
