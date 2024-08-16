---
title: "iOS 앱에 커스텀 팝오버 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-07-22-HowtoAddCustomPopoverstoYouriOSApp_0.png"
date: 2024-07-22 11:27
ogImage: 
  url: /assets/img/2024-07-22-HowtoAddCustomPopoverstoYouriOSApp_0.png
tag: Tech
originalTitle: "How to Add Custom Popovers to Your iOS App"
link: "https://medium.com/better-programming/how-to-add-custom-popovers-to-your-swiftui-ios-app-814bdfad73d0"
---


<img src="/assets/img/2024-07-22-HowtoAddCustomPopoverstoYouriOSApp_0.png" />

현재 팝오버를 표시하는 방법은 3가지가 있습니다. 하나는 UIPopoverPresentationController를 사용하는 것인데, 이는 iPad에서만 작동합니다. 다른 방법으로 UIMenu를 사용할 수 있지만, 이는 iOS 14 이상에서만 사용할 수 있습니다. 마지막으로 UIAlertController를 사용할 수도 있습니다. 이게 구현하기 가장 쉬울 것입니다. 그러나 라벨, 버튼 및 텍스트 필드만 지원하기 때문에 가장 제한적일 수도 있습니다.

<img src="/assets/img/2024-07-22-HowtoAddCustomPopoverstoYouriOSApp_1.png" />

그래서... 만약 앱 내 알림이 필요하거나 PIP(화면 안 팝업 비디오)가 필요하거나 온보딩 팝업이 필요하다면 어떻게 해야 할까요? 여기서 맞춤형 팝오버가 필요해집니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-22-HowtoAddCustomPopoverstoYouriOSApp_2.png)

Popovers 라이브러리를 사용할 것입니다. 솔직히 말해서, 저는 이 라이브러리의 개발자입니다. 하지만 이 라이브러리는 100% 오픈 소스이며 MIT 라이센스 하에 제공되므로 자유롭게 사용하셔도 됩니다.

먼저, 프로젝트에 라이브러리를 추가해야 합니다. Popovers는 Swift Package Manager를 지원합니다. Xcode에서는 파일 → 패키지 추가... 로 이동한 후 다음 URL을 입력하세요:

```js
https://github.com/aheze/Popovers
```

<div class="content-ad"></div>

그럼 시작해 봅시다!

# 기본 팝오버🔗

간단한 팝오버를 추가하려면 .popover() 수정자를 사용하면 됩니다 . 팝오버는 SwiftUI를 기반으로 하므로 원하는 뷰를 전달하고 표시할 수 있습니다.

<img src="https://miro.medium.com/v2/resize:fit:400/1*jHRE0isenb7Rr-8A5wo9Jg.gif" />

<div class="content-ad"></div>

알았어요. 만약 버튼의 상단에 팝오버를 붙이고 싶다면 어떨까요? 조금의 간격도 활용해볼 수 있어요. 굉장히 간단하게 할 수 있답니다:

![Popover to Button's Top](https://miro.medium.com/v2/resize:fit:520/1*_-1HVvLCv3vOmH3Wky8H7g.gif)

멋져요! 이제 애니메이션을 추가하고 고무줄 효과를 원하는 대로 맞춤 설정해봅시다.

![Customize Animation](https://miro.medium.com/v2/resize:fit:520/1*uoKsIJA5zQgnE81cDov0bg.gif)

<div class="content-ad"></div>

이제 바운시 팝오버가  생겼어요! 하지만 여기서 할 수 있는 것은 그저 표면일 뿐입니다. 아래에 모든 커스터마이징할 수 있는 속성들의 목록이 있어요. 이제 좀 더 고급 기능들을 살펴봅시다.

# 고급 팝오버🔗

내가 가장 좋아하는 SwiftUI의 기능 중 하나는 전환(transition)입니다. 뷰 구조가 동일한 한도 내에서 시각적으로 다른 뷰 사이를 애니메이션으로 전환할 수 있어요. 팝오버는 .popover(selection:) 수정자를 통해 이를 활용하여 다중 팝오버 사이를 부드럽게 전환할 수 있어요.

<div class="content-ad"></div>

좋아요! 그런데 또 하나 해볼 게요! Popovers를 만들 때 가능한 한 맞춤화할 수 있도록 만들었어요. 만약 Popover를 기존 뷰에 연결하고 싶다면, 배경을 추가하고 PopoverReader를 넣어보세요. PopoverReader은 GeometryReader처럼 작동하지만 더 멋져요!

![image](https://miro.medium.com/v2/resize:fit:520/1*UjMC5JS8DVR6eVFZWriTfQ.gif)

마지막으로 UIMenus의 모습이 정말 마음에 들었는데, iOS 14+에서만 사용할 수 있다는 게 아쉬웠어요. 그래서 Popovers를 사용하여 iOS 13로 백포트했어요!

![image](https://miro.medium.com/v2/resize:fit:520/1*JoHMaWvR4opLKCFzTq0FAQ.gif)

<div class="content-ad"></div>

메뉴 템플릿과 함께 소스에 포함된 다른 유틸리티 뷰와 메소드들이 있어요. 무엇보다도 더 멋진 것을 만들어보는 건 여러분에게 맡기겠어요.

# 결론🔗

시스템 구성 요소들은 정말 멋져요. 하지만 맞춤성을 희생해야 한다는 게 숨겨진 단점이기도 해요. 모든 앱이 같은 구성 요소를 사용하기 때문에 사용자 경험은 플랫폼 전체에서 매우 일관되어집니다. 하지만 조금이라도 제어를 원한다면 꽤 껄끄러운 상황이 될 수 있어요.

제 경우 내 앱에 그런 종류의 제어가 필요해서 팝오버를 만들었어요. 언젠가 유용하게 활용해 보실 수 있기를 바래요. 읽어 주셔서 감사합니다!