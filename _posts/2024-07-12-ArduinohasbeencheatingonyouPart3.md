---
title: "아두이노 사용 시 주의할 점 파헤치기 Part 3"
description: ""
coverImage: "/assets/img/2024-07-12-ArduinohasbeencheatingonyouPart3_0.png"
date: 2024-07-12 23:00
ogImage:
  url: /assets/img/2024-07-12-ArduinohasbeencheatingonyouPart3_0.png
tag: Tech
originalTitle: "Arduino has been cheating on you. Part 3"
link: "https://medium.com/@carlosaldea/arduino-has-been-cheating-on-you-part-3-c1d566b009de"
isUpdated: true
---

프로그램이 보드로 전달되는 방법은 어떻게 되나요?

![이미지](/assets/img/2024-07-12-ArduinohasbeencheatingonyouPart3_0.png)

새로운 친구인 CPU와 컴파일러를 만난 후에, Arduino가 당신에게 숨겨둔 또 하나의 중요한 개념을 알아보는 시간입니다.

# 보드에 프로그램을 전송하는 방법

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

새로 작성한 스케치를 컴파일하고(.c 파일을 .ino로 이름을 변경한 것임을 기억해 주세요) 보드의 플래시 메모리에 해당 이진 파일을 쓰는 시간입니다. 이를 "플래싱"이라고 부릅니다.

다시 한 번, Arduino는 이 과정을 매우 간단하게 만들어 놨어요. 버튼을 누르면 끝이죠.

하지만 친구들아, 이전의 포스트(여기와 여기)를 읽어 보셨다면, 우리 눈에 보이지 않는 중요한 것이 있다는 걸 알고 계실 겁니다.

여기서 말하는 것은 부트로더입니다.

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

마이크로컨트롤러의 플래시 메모리에 애플리케이션을 작성하려면 일반적으로 "프로그래머"라고 불리는 외부 회로가 필요합니다 (JLink가 하나의 예일 수 있지만 그 외에도 몇 가지 더 있을 수 있습니다). 특히, 플래시 메모리가 비어 있는 경우(일반적으로 새로운 마이크로컨트롤러인 경우) 마이크로컨트롤러에 프로그래머가 필요합니다.

그러나 운이 좋게도, 우리의 마이크로컨트롤러의 플래시 메모리에 새로운 애플리케이션을 쓰는 또 다른 메커니즘이 있습니다.

이 "메커니즘"이란 무엇일까요? 음, 이것은 개념적으로 간단하면서도 많은 복잡성을 가지고 있는 무엇인데요. 그것이 바로 "부트로더"입니다.

## 부트로더란 무엇인가요?

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

이전에 언급한 대로, 이것은 간단한 것입니다. 단지 마이크로컨트롤러의 플래시에 작성하는 프로그램으로, 두 가지를 수행할 수 있게 해줍니다.

- 플래시 메모리의 다른 영역에 저장된 응용 프로그램을 실행합니다.
- 그 "다른 영역"에 새 응용 프로그램을 작성합니다.

이 부트로더는 USB를 통해 새 스케치를 보내고 플래시에 기록하는 것을 가능하게 합니다.

그러니까, 여러분이 응용 프로그램을 작성을 마치고 "플래시"를 클릭하면, 아두이노 보드 내부에서는 무슨 일이 일어나느냐면:

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

- 보드가 재설정되고 부트로더가 실행됩니다.
- 부트로더는 애플리케이션이 작성될 것을 감지합니다.
- USB 포트를 통해 애플리케이션을 받아 플래시 메모리에 작성합니다.
- 방금 기록한 애플리케이션을 실행 시작합니다.

# 마지막으로

새로운 애플리케이션을 아두이노 보드에 플래싱하는 것은 아두이노 IDE를 사용하는 것만으로는 알 수 없는 몇 가지 구성 요소와 단계가 포함되어 있음을 배웠습니다.

이 시리즈의 다음이자 마지막 장에서 아마도 당신에게 숨겨진 아두이노에서 가장 중요한 부분에 대해 이야기하겠습니다.

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

곧 다시 만나요!

무료 기사 5개에 빨리 도달하는 것이 지치셨나요?

아마도 Medium에 가입하여 굉장히 멋진 이야기들을 즐기는 것이 좋을 때가 되었을지도 모르겠네요 (아마도 이건 아니지만, 정말 좋은 이야기들이 많다고 확신합니다).

제 링크를 통해 Medium에 가입하시면 제가 일부 수익을 얻을 수 있고, 그 돈으로 글쓰기 코치를 고용해서 드디어 뭔가 괜찮은 것을 쓸지도 몰라요 😋.
