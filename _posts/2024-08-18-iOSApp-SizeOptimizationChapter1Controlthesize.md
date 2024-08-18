---
title: "iOS 앱 사이즈 최적화 하는 방법"
description: ""
coverImage: "/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_0.png"
date: 2024-08-18 10:49
ogImage: 
  url: /assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_0.png
tag: Tech
originalTitle: "iOS App-Size Optimization Chapter 1  Control the size"
link: "https://medium.com/kerege/ios-app-size-optimization-chapter-1-control-the-size-a74fd59f0fb5"
isUpdated: true
updatedAt: 1723951896991
---

앱(.ipa) 내부에 무엇이 있는지 고려해 본 적이 있나요?

이 기사들은 사용자에게 해를 끼치지 않으면서 iOS 앱을 작게 만드는 방법에 대해 다루고 있습니다. 많은 개발자들이 작은 앱이 더 좋을 수 있다는 것을 기억해야 합니다. 작은 앱은 더 빨리 다운로드되고 더 잘 작동하며, 특히 인터넷 속도가 느린 곳에서도 더 많은 사람들이 사용할 수 있습니다.

이 기사는 앱의 크기를 제어하고 내부를 이해하는 데 초점을 맞추고 있습니다. 무언가를 변경하거나 개선하기 전에 측정 도구를 가지고 있는 것이 좋아요.

시작하기 전에, Xcode 빌드 시스템이 어떻게 작동하는지 이해하기 위해 이 기사를 읽어보는 것을 추천합니다.

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

아래는 Xcode 빌드 시스템의 개요입니다:

![Xcode Build System Overview](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_0.png)

빌드 프로세스의 결과로 'Product' 폴더가 생기는데, 여기에는 모든 컴파일된 파일과 리소스가 들어있습니다.

찾으려면 Xcode를 열고 - `설정 -` 위치 -` 파생 데이터로 이동하세요.

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

![image](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_1.png)

Here, you can find the DerivedData, Products, and Archives folders. This means you don’t need to archive and export your app every time you want to see changes.

For this article, I will use my app with an .ipa file as an example.

![image](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_2.png)

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

먼저 .ipa 파일을 .zip 확장자로 바꿔주세요. 그냥 이름을 바꾸시면 됩니다.

![Step 3](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_3.png)

파일을 열어봅니다.

![Step 4](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_4.png)

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

선택한 애플리케이션 파일을 선택하고 '패키지 콘텐츠 보기'를 클릭하세요.

![image](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_5.png)

'Size'로 정렬하면 사용자에게 제공하는 내용을 정확히 확인할 수 있습니다!

내 프로젝트에서는 많은 mp3, scn(3D 오브젝트), json(Lottie) 파일이 있는데, 정말 필요한 걸까요? 이 시리즈 기사의 다음 장에서 이에 대해 다룰 예정입니다.

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

Assets.car 파일은 무엇인가요?

저는 Xcode 빌드 시스템에 관한 이전 기사 'Xcode 빌드 시스템: 준비'에서 설명했습니다.

간단히 말하자면, Assets.car은 효율적인 런타임 사용과 앱 성능 향상을 위해 설계된 에셋 카탈로그의 컴파일된 최적화된 형태를 나타냅니다.

다음으로 주목해야 할 중요한 폴더는 'Frameworks'와 'PlugIns'입니다.

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

<img src="/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_6.png" />

- 'Frameworks' 폴더는 이름에서 알 수 있듯이 컴파일된 프레임워크를 포함합니다. 프레임워크를 추가하는 경우, 내부 구성 요소를 이해하는 것이 중요합니다. 이 프레임워크가 제공하는 모든 기능을 갖춘 이 무거운 기능이 필요한가요? 이것은 최적화를 고려할 좋은 기회입니다. 약속한 대로, 다음 장에서 최적화 전략과 기술을 설명하겠습니다.
- 'PlugIns' 폴더에는 확장 프로그램이 포함되어 있습니다. 이 .appex 파일이므로 '패키지 내용 보기' 옵션을 사용하여 패키지 내용을 확인해 보겠습니다.

<img src="/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_7.png" />

알아차리셨겠지만, Assets.car, .scn, 그리고 컴파일된 Swift 파일들이 보입니다.

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

앱과 확장 프로그램 간의 모든 리소스를 공유하는 라이브러리를 공유하는 것이 좋겠죠? .scn과 Assets.car과 같은 파일들의 중복을 피할 수 있어요.

주 앱과 확장 프로그램 간의 파일을 비교하면 어떤 파일들이 중복되는지 확인할 수 있어요. 예를 들어, MoonScene.scn (일반적으로 파일 크기를 확인해요).

이 글에서 논의하고 싶은 다음 파일은 .bundle 파일입니다.

![image](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_8.png)

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

‘패키지 내용 표시’를 클릭해서 내부를 확인해보세요.

![이미지](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_9.png)

.bundle 확장자를 가진 파일에는 Assets.car, 이미지, 로컬라이제이션 파일, .plist 파일 및 일반적으로 글꼴을 포함한 리소스가 포함되어 있습니다. 리소스로 처리되는 모든 것이 들어 있어요. 개발자들은 이 파일들을 거기에 넣어두죠 ;) 저도 다음 챕터에서 이것을 설명할게요.

이 방법이 쉽고 편리해서 정말 좋아해요. 하지만 물론 더 편리하고 자동화된 다른 방법들도 있을 거에요. 예를 들어, emergetools가 그 중 하나입니다. 내가 알기로 Spotify가 이것을 자동화 도구로 사용한다고 하더라구요.

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

아래는 Markdown 형식으로 변환한 것입니다.

Let’s try it and see if it reveals anything interesting.

![image1](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_10.png)

The design is really good, featuring interactive elements. The best aspect is the color coding of the boxes.

![image2](/assets/img/2024-08-18-iOSApp-SizeOptimizationChapter1Controlthesize_11.png)

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

빨간 상자는 (평소와 같이) 나쁜 일이 발생했음을 나타냅니다. 이러한 빨간 상자는 이미 정의된 'MoonScene.scn'과 같이 중복된 파일을 나타냅니다.

이 서비스는 앱을 최적화하는 옵션도 제공하지만, 이러한 옵션은 불완전합니다. 최적화는 앱마다 다르며 앞에서 언급했듯이 앱 크기를 관리하는 것은 여러분의 책임입니다.

Substack에서 저희를 팔로우해보세요: https://kerege.substack.com/

LinkedIn에서 저와 함께하세요: https://www.linkedin.com/in/salgara/

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

제게 커피를 사주실래요? https://ko-fi.com/salgara
