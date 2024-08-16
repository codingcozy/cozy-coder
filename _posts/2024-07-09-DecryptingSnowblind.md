---
title: "눈부심의 수수께끼 풀기 비밀 코드 해독 방법"
description: ""
coverImage: "/assets/img/2024-07-09-DecryptingSnowblind_0.png"
date: 2024-07-09 10:45
ogImage: 
  url: /assets/img/2024-07-09-DecryptingSnowblind_0.png
tag: Tech
originalTitle: "Decrypting: Snowblind"
link: "https://medium.com/@kr1pt7c/decrypting-snowblind-06a70901ac57"
isUpdated: true
---





2024년 초에 등장한 안드로이드 기기를 대상으로 한 신작 악성 소프트웨어인 Snowblind은 동남아시아 지역에서 활동 중인 강력한 데이터 스틸러입니다. 이 악성 소프트웨어는 은행 트로이jan 형태로 나타나며, 고유한 공격 벡터를 갖추고 있습니다. Snowblind은 안드로이드의 강력한 보안 메커니즘 중 하나인 seccomp(secure computing)를 활용하여 설계된 애플리케이션을 상대로 공격을 수행합니다. 

안드로이드에 도입된 seccomp는 애플리케이션이 샌드박싱을 구현하고 공격 표면을 줄이기 위해 시스템 호출을 제한하는 방법으로 소개되었습니다. 프로세스들은 시스템 호출이 이뤄질 때 확인되는 정책을 지정하는 데 사용할 수 있습니다. Snowblind는 이러한 기능을 활용하여 대상 애플리케이션에서 발견된 안티 탐지 및 안티 리패키징 메커니즘을 우회합니다.

<div class="content-ad"></div>

예방 탐지 및 재패키징 메커니즘의 작동 방식은 디스크의 APK 파일을 확인하여 변조되었는지 여부를 확인하는 것입니다. 이 과정에서 open() 시스템 호출을 사용합니다. Snowblind은 대상 애플리케이션에 사용자 지정 seccomp 필터를 포함한 네이티브 라이브러리를 삽입하면서 변조를 가합니다. 이 필터는 예방 탐지 메커니즘보다 먼저 실행되어 디스크의 변조된 APK 파일로의 open() 시스템 호출을 차단합니다. 그런 다음 Snowblind는 호출을 디스크의 합법적인 APK의 버전으로 리다이렉션하여 예방 탐지 메커니즘을 실행할 수 있도록 합니다. 또한 공격 속도를 높이는 메커니즘이 포함되어 있어 시스템 호출이 예방 탐지 메커니즘을 구현하는 라이브러리에서 온 경우에만 필터를 실행합니다.

Snowblind은 다른 목적을 위한 여러 공격 벡터를 사용하지만 이는 이를 시장의 다른 악성 소프트웨어와 구분 짓는 요인입니다.

이런 종류의 내용을 좋아하신다면 이 게시물에 박수를 보내주시거나 댓글을 달아주시고 더 많은 내용을 받아보세요.

안전하게 이용하세요.

<div class="content-ad"></div>

다음에 또 만나요. 이제 그만 할게요. 🌟✨