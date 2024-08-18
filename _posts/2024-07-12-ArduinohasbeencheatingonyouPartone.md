---
title: "아두이노가 당신을 속여왔다 파트 1"
description: ""
coverImage: "/assets/img/2024-07-12-ArduinohasbeencheatingonyouPartone_0.png"
date: 2024-07-12 23:05
ogImage:
  url: /assets/img/2024-07-12-ArduinohasbeencheatingonyouPartone_0.png
tag: Tech
originalTitle: "Arduino has been cheating on you. Part one"
link: "https://medium.com/@carlosaldea/arduino-has-been-cheating-on-you-ecab5ed5cd22"
isUpdated: true
---

<img src="/assets/img/2024-07-12-ArduinohasbeencheatingonyouPartone_0.png" />

안녕하세요. 그 사실을 받아들이는 게 어려운 거 알아요. 당신은 거짓말을 하고, 불편한 진실을 숨기던 누군가를 사랑해왔던 거죠.

# 진지하게 얘기해봐요

화가 나잖아 싶을 수도 있지만, 이번 시리즈의 주인공은 아두이노에 대해 이야기할 거예요.

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

그리고 간결하게 말하자면, 아두이노가 당신에게 숨겨둔 모든 비밀을 폭로하겠습니다.

그리고 임베디드 소프트웨어와 펌웨어에 대한 지식을 향상시키고, 임베디드 소프트웨어 개발자가 되고 싶다면, 이러한 모든 것에 관심을 가져야 합니다.

# 하지만, 아두이노에 문제가 있나요?

실제로는 아무 문제가 없습니다. 아두이노는 보드와 소프트웨어 레이어로 이루어진 생태계로, 프로그래밍 지식이 부족한 사람들이 자신만의 하드웨어/소프트웨어 프로젝트를 만들 수 있도록 도와주는 것입니다.

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

하지만, 하지만... 큰 막연한 '하지만'.

조금 복잡한 작업을 하려고 하거나 '펌웨어 및 임베디드 시스템' 세계에서 더 깊이 파고들어야 할 때, Arduino가 많은 것을 숨겼다는 것을 깨닫게 됩니다.

매우 중요한 것들을 많이요.

임베디드 시스템 분야에서 경력을 쌓고 싶거나, 단순한 Arduino 메이커 이상이 되고 싶다면 알아야 할 것들이죠.

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

이 시리즈의 기사에서는 Arduino가 당신에게 숨겨 놓은 것들을 묘사하겠습니다.

## CPU (모두의 보스)

자, 이제 손 더러운 일을 시작해봅시다.

이 첫 번째 기사에서는 임베디드 시스템의 핵심인 CPU에 대해 이야기하겠습니다.

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

임베디드 시스템과 소프트웨어 개발 프로세스의 많은 다른 부분에 제약을 설정하는 CPU(MCU 또는 SoC)에 관해 얘기해보겠습니다.

예를 들어, 이러한 것들을 결정할 것입니다:

- 사용하는 컴파일러
- 주변장치에 액세스하기 위한 메모리 맵핑
- 인터럽트 모델
- 주변장치 처리 방법
- 보유한 HW 기능 (DMA, 암호 모듈 등)

그리고 신중히 생각해보면 Arduino 보드를 사면서 사용하는 마이크로컨트롤러조차 거의 모릅니다(적어도 필요하지 않을 것입니다).

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

아래는 예시 몇 개를 드릴게요:

- Arduino UNO는 ATmega328P (Atmel)를 사용합니다.
- Arduino UNO R4 Minima는 RA4M1 (Renesas)를 사용합니다.
- Arduino GIGA R1 WiFi는 STM32H747XI (ST Microelectronics)를 사용합니다.

이 세 가지 다른 MCU는 서로 다른 아키텍처 (Atmel 및 ARM), 기능 및 제조사를 가지고 있습니다. 따라서 처리해야 할 서로 다른 종류의 매뉴얼을 가지고 있습니다.

## 다음 챕터에서 계속됩니다...

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

다음 장에서는 아두이노가 숨기고 있는 다른(매우) 중요한 것들에 대해 이야기할 거에요.

곧 뵙겠습니다!

5개 무료 기사에 빨리 도달하는 데 지쳤나요?

Medium에 가입하여 수많은 놀라운 이야기를 즐기는 것이 좋을지도 모릅니다(아마 이것은 아니겠지만, 정말 매력적인 이야기들이 많다는 것을 확신합니다).

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

저의 링크를 통해 Medium을 구독하면 저도 일부 수익을 얻을 수 있어요. 그 돈으로 글쓰기 코치를 고용해서 마침내 좋은 글을 쓸지도 몰라요 😋.
