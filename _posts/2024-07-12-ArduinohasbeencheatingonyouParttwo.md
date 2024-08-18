---
title: "아두이노가 당신을 속였다 파트 2"
description: ""
coverImage: "/assets/img/2024-07-12-ArduinohasbeencheatingonyouParttwo_0.png"
date: 2024-07-12 23:01
ogImage:
  url: /assets/img/2024-07-12-ArduinohasbeencheatingonyouParttwo_0.png
tag: Tech
originalTitle: "Arduino has been cheating on you. Part two"
link: "https://medium.com/@carlosaldea/arduino-has-been-cheating-on-you-6cca2568c6ca"
isUpdated: true
---

"번역기"

<img src="/assets/img/2024-07-12-ArduinohasbeencheatingonyouParttwo_0.png" />

두 번째 장에서는 아두이노가 숨기고 있는 모든 것을 계속 묘사하겠습니다(그리고 여러분을 "전문" 펌웨어 개발자로부터 떼어버리는 것들).

# 컴파일러

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

당신이 손에 쥔 CPU를 알게 되었다면(1장에서 보았던 것처럼), 다음 단계는 그 CPU에서 SW를 빌드하는 데 필요한 도구를 구하는 것입니다.

가장 중요한 도구는 다음과 같습니다: 컴파일러.
실제로, "크로스 컴파일러"라는 새로운 친구를 만나게 될 것입니다.

하지만 이에 대해 말하기 전에 아두이노로 돌아가 봅시다.

# "빌드"를 클릭하세요

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

그것이 바로 당신의 컴파일된 이진 파일을 플래시 메모리에 올릴 준비가 된 유일한 것입니다.

하지만 아두이노는 당신에게 후드 아래에서 무슨 일이 벌어지고 있는지 감추고 있습니다.

그것은 단순히 .c(말 그대로 .ino) 파일을 모두 .o 파일로 변환하고, 그것들을 모두 함께 넣어 "링크"하는 것을 말하는 작업을 수행하는 크로스 컴파일러(크로스 컴파일러)를 호출하는 것뿐입니다. 그렇게 하여, 최종적으로 보드에서 실행될 이진 파일을 얻을 수 있습니다.

# 크로스 및 비크로스 컴파일러

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

지금까지 "컴파일러"와 "크로스컴파일러"에 대해 이야기했습니다. 이 두 개녀은 간단한 용어로 설명하겠습니다.

컴파일러는 특정 기계(예: 인텔 x86 또는 amd64 아키텍처 CPU가 장착된 노트북)에서 실행되는 애플리케이션입니다. 이러한 애플리케이션은 소스 파일(사람이 읽을 수 있는 코드가 포함된 파일)을 오브젝트 파일(해당 CPU가 이해할 수 있는 기계 코드가 포함된 파일)로 변환할 수 있습니다.

그러나 이 간단한 정의에는 알아두어야 할 두 가지 세부 사항이 있습니다.

a) 컴파일러가 실행되는 CPU(이 예시에서는 인텔 CPU)

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

b) 프로그램이 컴파일러에 의해 생성될 CPU(이 경우, 같음)는 동일합니다. 컴파일러가 생성하는 프로그램은 동일한 기계에서 실행됩니다.

그래서 "일반" 컴파일러는 한 기계 내에서 실행되고 해당 기계용 코드를 생성합니다.

## 그러면 크로스 컴파일러는 어떨까요?

"일반" 컴파일러가 어떻게 작동하는지 설명한 논리를 따르면, 크로스 컴파일러가 무엇을 하는지 이미 알고 계실 것입니다.

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

자, 아두이노 보드로 돌아가 봅시다. Atmel CPU를 장착한 납작한 녀석이죠. 저희가 사용할 모든 도구(아두이노 IDE, 컴파일러 등)는 우리 멋진 노트북(x86 또는 amd64)에 설치되어 있습니다.

그러니까, 노트북에서 실행되지만 Atmel CPU에서 실행될 이진 파일을 생성하는 컴파일러가 필요합니다.

바로 '크로스 컴파일러'입니다. 다음과 같은 특징을 가진 컴파일러죠:
a) 특정 CPU(아키텍처)에서 실행

b) 다른 CPU(아키텍처)를 위한 코드 생성

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

# 마지막으로

이제 비밀을 알았네요. 아두이노는 컴파일러의 개념과 더 중요한 것인 "크로스 컴파일러"의 개념을 숨겨놓고 있습니다.

다음 장에서는 아두이노가 여러분에게 숨겨놓은 또 다른 중요한 사항에 대해 이야기할 것입니다.

계속 주목해 주세요.

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

5개의 무료 기사에 빨리 다다르는 것에 지쳤나요?

아마도 Medium에 구독해보는 것이 좋을 때가 되었을지도 모릅니다. 거기에는 놀라운 이야기들이 많이 있어요 (아마도 이런 내용은 아니겠지만, 정말 좋은 이야기들이 많다는 것을 확안합니다).

여기에서 제 링크를 통해 Medium을 구독하면 됩니다. 그러면 일부 수익을 얻을 수 있고, 그 돈으로 글쓰기 코치를 고용하고 마침내 어떤 훌륭한 것을 쓸 수 있을지도 모릅니다 😋.
