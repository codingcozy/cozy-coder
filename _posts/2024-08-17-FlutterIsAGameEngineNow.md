---
title: "Flutter를 게임 엔진으로 활용하는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-FlutterIsAGameEngineNow_0.png"
date: 2024-08-17 00:18
ogImage:
  url: /assets/img/2024-08-17-FlutterIsAGameEngineNow_0.png
tag: Tech
originalTitle: "Flutter Is A Game Engine Now"
link: "https://medium.com/@impure/flutter-is-a-game-engine-now-db6b1bbce721"
isUpdated: true
updatedAt: 1723863627535
---

![Flutter 3.24](/assets/img/2024-08-17-FlutterIsAGameEngineNow_0.png)

플러터 3.24가 출시되었어요. 실제로 플러터 3.19가 세 번의 핫 픽스 릴리스만 받은 것에 조금 놀랐어요(그래서 플러터 3.19.3까지만 릴리스되었어요). 보통 적어도 X.Y.9까지 릴리스되는데 말이에요. 아마 이것은 플러터가 더 안정화되고 있다는 신호인가봐요.

이번 릴리스에는 몇 가지 큰 새로운 기능이 있어요. 먼저, 새로운 위젯과 새로운 슬리버들이 생겼어요. iOS와 같은 슬리버 애니메이션을 보여주었어요:

![Sliver Animation](/assets/img/2024-08-17-FlutterIsAGameEngineNow_1.png)

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

비록 내 것이 훨씬 멋지긴 해요.

![image](https://miro.medium.com/v2/resize:fit:1200/0*rmL3jEHskVTCgbiv.gif)

그냥 레이아웃 빌더일 뿐이에요. 요렇게 해보고 있어요.

곧 더 많은 광고 제공 업체가 생길 거에요. 이전에는 구글 모바일 광고만 있었는데, 이제 웹 지원도 추가될지도 몰라요. 🤞

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

거기에 Swift Package manager가 있습니다. Cocoapods는 한동안 작동하지 않았어요. 심지어 제가 문제를 해결하는 방법에 대해 블로그 글을 썼고, 아직도 독자들이 보고 있어요. 문제를 해결할 수 있는 답이 있지만 너무 복잡해서 지쳤기 때문에 글을 썼죠.

iOS/Mac에 빌드하는 것을 두려워하는 사람들도 보았어요. 그렇게 복잡하지 않아요, 하나의 명령어면 됩니다: pod update

그 글을 쓴 후에 또 다른 문제가 있음을 깨달았어요: Cocoapods가 Apple Silicon에 대해 업데이트되지 않았다는 거죠. 업데이트 될 것이라고 희망했지만, 2024년 8월 기준으로, Apple Silicon이 데뷔한 지 거의 4년이 지났음에도 아직 업데이트되지 않았어요. Rosetta 호환성을 사용해야 합니다.

이 문제를 조사해보았어요. 핵심 문제는 Cocoapods가 — 사라지지는 않았지만 — 예전과 다르다는 점이네요. 대부분의 사람들은 Swift Package Manager로 이동하고 있는 것 같아요. 그리고 Flutter도 이제 이동하려고 합니다.

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

이것이 Flutter 3.24에서의 모든 큰 발표 사항입니다. 그런데 또 뭔가 눈에 띄는 게 있었어요...

저는 Flutter Medium 블로그를 구독하고 있습니다. 그리고 Flutter 3.24 게시물이 게시된 동시에 두 개의 추가 게시물이 있었습니다: 먼저 Flutter 및 3.24 그리고 Dart 3.5 개요였죠. 유명한 올림픽 때문에 뉴스에 실린 프랑스 철도 SNCF도 있다고 하네요. 짜증을 내는 누군가가 그것을 망쳤다는 소문이 있었죠. 그리고 Wolt, Whirlpool, Monta의 이야기들도 있었습니다.

그리고 Flutter GPU에 관한 이 게시물이 게시되었습니다. 이것은 GPU와 상호 작용하는 낮은 수준의 방법이라고 하네요. 이것은 그래픽 API(Vulcan/OpenGL for Android, Metal for iOS) 위에 위치한 어떤 종류의 추상화층인데, 엄밀히 말해, 그래픽 API 위에 위치한 Impeller 위에 있다고 하네요.

정말 강력하군요. 이 이미지를 멈출 수 없어요.

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

<img src="/assets/img/2024-08-17-FlutterIsAGameEngineNow_2.png" />

작년 Flutter Forward 이후 사람들이 3D 데모에 대해 열광하더라고. 그런데 나는 생각했어:

당연히 농담이었어, 실제로 플러터가 그런 것들을 구현할 것이라고는 기대하지 않았어. 그런데 이제 그 헬멧을 보여줬어:

텍스처와 실시간 반영. 말 그대로 헬멧이 빛을 받지 않아서 전체적으로 밝기가 동일하지만 여전히 놀라운 데모야. 아마 심지어 Flutter Forward의 3D보다 더 인상적일지도 몰라.

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

2024년이 플러터에게 큰 해가 될 것이라는 내 생각을 더욱 확고히 했어요. 게임 산업에서 발견한 몇 가지 걱정스러운 트렌드를 지적한 글이었죠. Unity가 미친 듯한 설치 수수료를 도입하려고 시도하고, 언리얼도 비슷한 일을 하지만 게임 개발자를 위한 것은 아니라서 괜찮다고 판단했으며, 고돗은 사용자 친화성이 부족하다는 점을 지적했어요.

재미있는 점은 이 글을 쓰는 동안 이 영상이 나왔다는 거에요:

나는 이 문제의 해결책으로 플러터를 제안했어요. 하지만 그때는 충분히 크게 생각하지 못했어요. 2D 모바일 게임을 대체할 것으로 생각했었죠.

플러터의 3D 지원에 그다지 감명받지 않았기 때문에 그랬던데요. Flutter를 사용하여 3D를 활용한 게임잼 게임 몇 개를 시도해봤는데 하드웨어 가속화가 되지 않았기 때문에 성능이 좋지 않았어요.

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

플러터 포워드에서 임펠러가 문제를 해결하고 플러터에 GPU 가속 그래픽을 제공할 수 있다는 것이 보여졌어요. 하지만 언제 준비가 될지 확신하지 못했죠. 그리고 제게는 여전히 기술 데모처럼 보였어요: 텍스처, 그림자, 물리 효과가 없었습니다. 그렇다면 지금은 어떤가요?

![Image](https://miro.medium.com/v2/resize:fit:1012/0*rFXLv0aW41NpMOGF.gif)

대쉬가 점프했어요. 점프했어요! 물론, 상당히 부드러운 점프인데요, 이는 게임 개발에서 흔한 문제입니다. 이 데모는 게임 개발이 플러터로 올 것을 보여줍니다. 2D뿐만 아니라 3D 게임 개발도 말이에요. 그리고 생각한 것보다 일찍 도래할 수도 있습니다.

비디오 게임에서 불필요하게 어려운 것이 있을 때, 바로 UI라는 것이죠. 그리고 플러터는 그것을 쉽게 처리할 수 있습니다. 저는 플러터가 성공할 것이라는 의심이 들지 않아요.

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

하지만, 이게 맞는 결정인가요?

요즘에 이것에 대해 생각을 많이 해봤는데, 제 생각에는 맞는 것 같아요. 프로젝트는 더 개발될수록 자연스레 느려지는데, 개선할 부분이 줄어들기 때문이에요. 제 생각에는 Flutter가 그 지점에 이르렀다고 생각해요. Flutter 1.x와 2.x 시절에는 할 일이 많았지만, 이제는 그렇지 않아요. 그래서 게임 개발로 전환한다고 해서 UI 측면이 반드시 영향을 받을 필요는 없을 것 같아요.

사실 게임 개발은 모두 성능에 관한 것이라서 오히려 도움이 될 수 있어요. 만약 게임 개발자를 만나면 그들이 이야기하는 것은 항상 캐시 미스, 점진적 가비지 수집기, 셰이더 해킹 등이에요. 게임 개발자들은 가능한 최상의 성능이 필요하기 때문에 게임 개발자를 위해 최적화하면 사실 모두를 위한 최적화를 하게 되는 것이죠.

실은 Flutter/Dart 가비지 수집기에 대한 또 다른 포스트를 작업 중인데, 저는 그것이 별로 좋지 않다고 생각해요. also여기에 이 게임 개발 각도가 추가되면 Flutter가 마침내 그 문제에 대처할 수밖에 없게 될지도 모르겠죠. 그런데 그런 일이 벌어진다면 큰 뉴스가 될 거 같아요.

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

_혹시 과장일 수도 있어요_
