---
title: "개발자들이 switch 문보다 if-else 문을 좋아하는 이유"
description: ""
coverImage: "/assets/img/2024-07-27-WhyDoMostProgrammersPreferif-elseThanswitch_0.png"
date: 2024-07-27 14:05
ogImage:
  url: /assets/img/2024-07-27-WhyDoMostProgrammersPreferif-elseThanswitch_0.png
tag: Tech
originalTitle: "Why Do Most Programmers Prefer if-else Than switch"
link: "https://medium.com/frontend-canteen/why-do-most-programmers-prefer-if-else-than-switch-718f397af4ec"
isUpdated: true
---

<img src="/assets/img/2024-07-27-WhyDoMostProgrammersPreferif-elseThanswitch_0.png" />

최근에 코드를 검토했는데, 재미있는 사실을 발견했어요. 대부분의 프로그래머들이 switch 대신에 if-else를 선호하는 것으로 보여요!

일반적으로, else-if 분기가 3개 이상이면 switch를 사용하는 것을 고려해야 합니다. 10개 이상의 조건 분기가 있는 경우, 설정 변수나 파일을 사용하고 해당 설정에 대한 특정 함수를 작성하는 것을 고려해야 합니다. 매핑 로직이 복잡하고 빈번하게 사용될 경우, 전용 규칙 엔진이나 DSL을 만들어서 이를 처리하는 것도 고려해 볼 수 있어요.

Switch가 더 적합한 명백한 부분이 많은데, 개발자들이 if-else를 사용한다는 것이 제 궁금증을 자극했어요. 그래서 이 문제에 대해 연구하기로 결정했어요.

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

# 개발 프로세스

많은 경우, 코드가 한 번에 개발되는 것이 아니며, 한 조각의 코드는 종종 여러 명의 개발자들에 의해 서로 다른 시기에 개발 및 수정됩니다.

여러 개의 가지를 가진 판단문은 종종 한 번에 작성되지 않고, 새로운 요구사항이 추가될 때마다 새로운 가지가 추가됩니다. 각각의 가지를 추가하는 사람들은 전체 코드를 리팩토링할 책임을 느끼지 않습니다. 왜냐하면 그들은 원하는 일을 가장 낮은 비용으로 끝내기를 원하기 때문에 코드 품질이 계속해서 낮아지게 됩니다.
