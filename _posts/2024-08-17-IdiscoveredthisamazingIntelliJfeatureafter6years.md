---
title: "알면 퇴근시간이 빨라지는 IntelliJ 활용 방법"
description: ""
coverImage: "/assets/img/2024-08-17-IdiscoveredthisamazingIntelliJfeatureafter6years_0.png"
date: 2024-08-17 01:01
ogImage:
  url: /assets/img/2024-08-17-IdiscoveredthisamazingIntelliJfeatureafter6years_0.png
tag: Tech
originalTitle: "I discovered this amazing IntelliJ feature after 6 years"
link: "https://medium.com/@mayank.sharma2796/i-discovered-this-amazing-intellij-feature-after-6-years-72eeb58f1349"
isUpdated: true
updatedAt: 1723863725938
---

![이미지](/assets/img/2024-08-17-IdiscoveredthisamazingIntelliJfeatureafter6years_0.png)

2018년에 처음으로 IntelliJ IDEA를 사용했어요. 대학 시절에는 eclipse에 대해 들어본 적만 있었는데, 처음에는 조금 힘들었어요.

6년이 흘렀는데, 이제는 거의 매일 IntelliJ IDEA를 이용하며 일하고, Java 코드를 작성하고 디버깅하고 멋진 것들을 만들어내고 있어요.

최근에 문제를 디버깅하려고 했는데, 로그가 Athena에서만 보였어요. 문제를 이해하려고 노력하던 중에 동료가 내 책상에 와서 IntelliJ에서 스택 트레이스를 분석하지 않느냐고 물어보았어요.

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

그분이 무슨 말을 하는지 이해하지 못했는데, 그 다음에 한 행동이 정말 놀라웠어요.

그는 그냥 스택 추적을 복사했어요. Code→ Analyse Stack Trace 또는 Thread Dump로 이동해서 거기서 스택 추적을 복사했어요.

![이미지](/assets/img/2024-08-17-IdiscoveredthisamazingIntelliJfeatureafter6years_1.png)

스택 추적을 해석하고 코드 베이스를 쉽게 탐색할 수 있도록 해줬어요.

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

다음에 이메일, 슬랙 또는 디버깅 중에 얻은 로그를 처리해야 할 때 다음 단계를 따라주세요.

- 주 메뉴에서 Code | Analyze Stack Trace 또는 Thread Dump로 이동합니다.
- 열리는 Analyze Stack Trace 대화 상자에서 외부 스택 추적이나 스레드 덤프를 Put a stack trace or a complete thread dump here: 텍스트 영역에 붙여넣습니다.
- 스택 추적을 해독하려면, Unscramble stack trace 확인란을 선택하고 원하는 해독기와 로그 파일을 선택하세요.
- 스택 추적 텍스트가 손상된 경우(줄이 잘리거나 줄바꿈되거나 너무 긴 경우 등) 일부 소프트웨어(예: 버그 추적기 또는 메일 클라이언트)로 처리한 후 Normalize를 클릭하세요.
- OK를 클릭합니다. 스택 추적이 Run 도구 창에 표시됩니다.
- 문제를 일으킨 코드로 이동하려면 필요한 스택 추적 라인으로 스크롤하고 소스 파일로 이어지는 링크를 클릭하세요. 파일이 편집기에서 열립니다.

![이미지1](/assets/img/2024-08-17-IdiscoveredthisamazingIntelliJfeatureafter6years_2.png)

![이미지2](/assets/img/2024-08-17-IdiscoveredthisamazingIntelliJfeatureafter6years_3.png)

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

# 왜 이 기능이 이렇게 중요한가요?

- 시간 절약: 파일 안에서 수동으로 검색할 필요가 없습니다. 이 도구는 코드에서 정확한 위치를 즉시 강조힙니다.
- 생산성 향상: 문제를 찾는 데 시간을 들이기보다 해결하는 데 더 많은 시간을 투자하세요.
- 오류 추적: 버그를 빨리 이해하고 재현하는 데 도움이 되어 디버깅 과정이 훨씬 원활해집니다.

이 게시물이 마음에 드셨다면 저의 프로필에서 다른 게시물을 확인해보세요.

제 프로필에서 꼭 가상 시스템 디자인 인터뷰 일정을 예약해보세요: [링크](https://www.meetapro.com/provider/listing/160769)

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

링크드인: [마양크 샤르마 프로필](https://www.linkedin.com/in/mayank-sharma-2002bb10b/)

[Buy Me a Coffee](www.buymeacoffee.com)

내 웹사이트: [imayanks.com](imayanks.com)
