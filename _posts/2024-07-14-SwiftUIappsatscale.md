---
title: "대규모 SwiftUI 앱 개발 방법"
description: ""
coverImage: "/assets/img/2024-07-14-SwiftUIappsatscale_0.png"
date: 2024-07-14 00:24
ogImage:
  url: /assets/img/2024-07-14-SwiftUIappsatscale_0.png
tag: Tech
originalTitle: "SwiftUI apps at scale"
link: "https://medium.com/better-programming/swiftui-apps-at-scale-19b7886384f7"
isUpdated: true
---

오늘은 3개의 스타트업에서의 SwiftUI 여정 이야기를 전하고, SwiftUI를 풀타임으로 사용하는 데 방해 요인이 되는 중대한 결함에 대한 해결책을 설명하려고 해요.

![SwiftUI](/assets/img/2024-07-14-SwiftUIappsatscale_0.png)

## 수년간의 SwiftUI

저는 SwiftUI 초기부터 주로 사용하게 되어 굉장히 운이 좋았어요.

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

2019년에 베타 버전이 출시되자마자, Patcher라는 새로운 사이드 프로젝트 및 창업 프로젝트에 종사하고 있었죠. 이는 자동차 수리를 위한 우버처럼 생각할 수 있는 서비스였답니다. 우리는 모든 일진 엔지니어들이 하는 대로, 첫 시도에서 처음 고객을 확보하기 전에 모든 것을 구축하기로 결정했습니다.

이 앱은 모든 것을 보유하고 있었습니다. 지도, 온보딩, 프로필, 작업 요청, 결제, 수리 진행 흐름 및 기술자를 위한 두 번째 동반 앱까지. 이는 상당히 복잡한 시스템이었고, 늦은 2019년과 초기 2020년의 SwiftUI가 준비되어 있지 않았다고 말할 수 있습니다.

초기 UIKit 상호 운용성은 조잡했으며 상태를 전달하기가 어려웠죠. 이는 두 가지 주요 기능이 구글 맵스 SDK(뷰가 존재하지 않았습니다!)와 카메라를 사용했을 때 문제가 되었습니다. 무엇보다도, SwiftUI 1.0에서 네비게이션이 완전히 정상 작동하지 않았습니다. 결국 저희는 대부분의 앱 내 탐색에 대부분 모달을 사용하게 되었습니다.

저는 2020년 말에 Carbn을 공동 창업했습니다. 이는 더욱 친환경적 습관을 개발하고 탄소 발자국을 줄이는 데 도움을 주는 스타트업이었습니다. 이때 iOS 14가 출시되었고 SwiftUI가 쇼타임을 위해 준비된 상태였습니다. 우리는 @StateObject, 지연 스택, ScrollViewReader 및 제가 제일 좋아하는 matchedGeometryEffect 등을 얻었지만, 무엇보다 중요한 것은 사람들이 SwiftUI를 올바르게 앱 구조화하는 방법을 찾기 시작했다는 것이었습니다.

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

오늘은 Gener8에서 모바일 엔지니어링 리드로 일하고 있어요. 여기서는 여러분의 데이터를 제어하고 가치를 끌어내는 데 도와드리고 있답니다. 우리는 iOS 15+에 집중하고 있어서, Materialeffects, Refreshable, TaskModifier, Markdown 렌더링 등에 접근할 수 있어 행운이에요!

SwiftUI가 지금은 정말 매끄럽게 작동하고 있다고 말해도 될 것 같아요. 지난 4년 동안 어느 순간, 실험적인 장난감에서 진지한 비즈니스를 구축할 수 있는 완전한 UI 프레임워크가 되었어요.

# ...하지만, 제대로 사용해도 괜찮을까요?

음, 제대로 사용하면 그것이 제대로 사용 가능한 것이란 말이에요!

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

Before Swift got ABI stability, there was a decent-sized group of Objective-C enthusiasts who were hesitant to embrace Swift as a mature language. It seems like we may encounter similar voices advocating for UIKit even in the year 2030 (although it's rare to find anyone championing Storyboards these days).

Let's delve into the strengths, weaknesses, and the factors that might be holding back engineers from fully embracing SwiftUI.

# SwiftUI

## ...Pros

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

너에게 SwiftUI를 전파할 필요는 없겠지만, 간단히 말할게. SwiftUI는 빠르고 선언적이며 그 어떤 좋은 기능도 갖추고 있어. 리액티브하고, 상자 밖의 데이터 흐름을 제공해줘. 미래야.

## ...좋지만

물론 SwiftUI도 완벽하지 않아. 네비게이션은 항상 부자연스럽다. AV, 카메라 및 지도와 같은 옛날 스타일의 UIKit 기반 프레임워크에 대한 지원이 제한적이다(최소한 iOS 17 이전까지). 아직 성숙한 프레임워크가 아니기 때문에 종종 UIKit에 얽매이는 부분이 있어. 마지막으로, 렌더링 엔진은 CoreAnimation, UIKit 및 Metal의 지혜로운 조합을 사용하며, 명령형 프레임워크보다 '마법'이 조금 더 사용된다. 이는 비트별 성능 최적화를 더 어렵게 만든다.

## ...그러나 위와 같은 단점도 있지만

(Note: This is a creative translation inspired by Tarot blogs. Enjoy!)

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

Apple은 현재 싱글 뷰나 마스터-디테일 앱 이외의 것은 존재하지 않는다고 주장하고 있습니다. 모든 젊은 프레임워크와 마찬가지로, OS 버전은 계속 변화하며, 최신 킷을 사용하려면 코드 전체에 불편한 만큼의 if #available이 필요합니다. 마지막으로, 기존의 UIKit 코드가 많은 상황에서 SwiftUI를 원할하게 작동시키는 것이 어려울 수 있습니다 — 상태 관리가 매우 다르기 때문입니다.

SwiftUI는 환상적이지만 완벽하지는 않습니다. 이러한 불완전함은 4년 된 신생 프레임워크에서 예상할 수 있는 것이지만, 하나의 문제가 광범위한 채택을 막고 있습니다:

# Navigation에 집중해보죠

Navigation은 SwiftUI로 완전히 전환하기를 막는 가장 큰 문제입니다. 대규모, 복잡한 앱에 대해 자연스러운 옵션이 실제로 제공되지 않습니다. 앱 데이터의 상태를 통해 네비게이션을 정의하는 것은 장난감 앱에 대해 잘 작동하지만, 규모를 확장하는 것은 지치게 됩니다. 화면을 제공해야 하는지 이유를 생각하려면 앱의 전체 상태를 머릿속에 담고 있어서는 안 되는 일입니다.

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

정말, 근본적인 문제는...

## ...애플이 거대한 뷰 컨트롤러 문제를 부활시켰다.

iOS 16에서 애플이 업그레이드한 SwiftUI에서 제공되는 네비게이션 패러다임을 고려해 보세요:

NavigationStack, NavigationLink, .navigationDestination, .sheet 및 NavigationSplitView.

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

SwiftUI 보기로 구성된 것들 중에는 사용자 상호 작용이나 앱 상태에 따라 어떤 상호 작용이 어떤 하위 보기로 이어질지를 결정할 수 있도록 합니다.

이는 보기와 네비게이션 로직이 강하게 결합되어야 한다는 것을 의미합니다. 이는 테스트 가능성에 반하는 것이며, 엔지니어링의 최상의 실천 방법에 반하는 것으로, (솔직히) iOS 엔지니어들이 비교적 최근에야 회복한 습관입니다. 문자열형 시그웨이, viewDidLoad()에 인라인으로 포함된 URLSession 호출, 전체 앱 Main.storyboard 파일의 공포는 우리가 막 깨어난 지 얼마 안 된 나쁜 여행 같습니다.

# 네비게이션 로직 추상화

그래서 우리가 발견한 주요 문제를 알았습니다. SwiftUI는 네비게이션 로직을 추상화할 수 없게 만들기 때문에, 성숙한 엔지니어링 팀이 복잡한 제품을 만들기 위해 함께 협력하기가 어려워집니다.

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

하지만 해결책이 있어요! 계속 무심한 곳에 있었답니다.

![image](/assets/img/2024-07-14-SwiftUIappsatscale_1.png)

# 조정자 패턴

이것을 라우터 패턴 또는 내비게이터 패턴이라고도 할 수 있어요. 하지만 핵심 아이디어는 동일해요: 내비게이션 로직을 캡슐화하세요.

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

제가 이를 구현하는 방식은 AppCoordinator를 사용하는 것입니다. 이 AppCoordinator는 앱의 코드 흐름을 담당하며 (AppDelegate/SceneDelegate에서 설정됨) 여러 개의 자식 코디네이터를 소유하고 있습니다. 이 자식 코디네이터들은 각자의 하위 흐름을 위한 자식들을 가질 수 있습니다.

![Image](/assets/img/2024-07-14-SwiftUIappsatscale_2.png)

코디네이터를 위한 프로토콜은 꽤 간단합니다:

```js
public protocol Coordinator: AnyObject {
    func navigate(to route: Route)
}

public protocol Route { }
```

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

일반적으로 Route는 Coordinator의 네비게이션 흐름에서 가능한 화면을 나타내는 열거형일 뿐이며, 앱의 모든 네비게이션 로직은 Coordinator의 navigate(to:) 메서드 안에 격리됩니다.

이것은 UIKit에서 익숙할 수 있는 일반적인 접근 방식이지만, SwiftUI와 완전히 상호 운용이 가능하도록 수정하는 방법을 설명해 드리겠습니다. 더 필요한 구성 요소는 2개뿐입니다:

## UIHostingController

이는 SwiftUI 컨텍스트를 UIViewController 안에 래핑하여 두 패러다임 간의 다리 역할을 하는 것입니다.

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

## 네비게이션 프로토콜

요즘은 UINavigationController와 UITabBarController의 기능을 추상화하기 위해 2가지 프로토콜을 소개할 거에요. 이 두 가지가 복잡한 UIKit 앱의 필수 빌딩 블록이죠.

결과적으로 앱의 네비게이션 구조는 UIKit을 사용해본 사람들에게 매우 익숙할 거에요:

![이미지](/assets/img/2024-07-14-SwiftUIappsatscale_3.png)

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

## NavigationContext

이 프로토콜은 우리의 위시리스트이며, SwiftUI 뷰를 보여주고 숨기는 내비게이션 기능을 정의합니다. 프레젠테이션 로직은 뷰와 별도로 코딩되어 있습니다.

```swift
public protocol NavigationContext {
    func setInitialView<T: View>(view: T)
    func push<T: View>(view: T, animated: Bool)
    func pop(animated: Bool)
    func present<T: View>(view: T, animated: Bool)
    func dismiss(animated: Bool)
}
```

이 프레젠테이션 함수들은 제네릭 뷰를 받고, 우리는 네비게이션의 중요한 요소들을 모두 구현합니다: push와 pop; present와 dismiss; 또한, 기능의 루트에 초기 뷰를 생성하는 것이죠.

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

프로토콜의 구체적인 구현은 UINavigationController의 서브클래스를 통해 동작합니다:

```js
public class MyNavController: UINavigationController, NavigationContext {

    public init() {
        super.init(nibName: nil, bundle: nil)
    }

    public required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    public func setInitialView<T: View>(view: T) {
        let vc = UIHostingController(rootView: view)
        viewControllers = [vc]
    }

    public func push<T: View>(view: T, animated: Bool) {
        let vc = UIHostingController(rootView:: view)
        pushViewController(vc, animated: animated)
    }

    public func present<T: View>(view: T, animated: Bool) {
        let vc = UIHostingController(rootView: view)
        vc.modalTransitionStyle = .coverVertical
        vc.modalPresentationStyle = .automatic
        present(vc, animated: animated)
    }
}
```

여기서 보듯이 구현 방식은 표준 UIKit 네비게이션 메서드와 매우 유사하지만, SwiftUI 뷰를 UIHostingController로 래핑하는 추가 단계가 있습니다.

이제 첫 번째 코디네이터의 navigate(to:) 메서드를 쉽게 채워 넣을 수 있습니다:

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

```swift
public func navigate(to route: Route) {
    switch route {
    case .homeRoute:
        navigationContext.setInitialView(view: HomeView())

    case .feedRoute:
        navigationContext.push(view: FeedView(), animated: true)

    case .photoRoute:
        navigationContext.present(view: PhotoView(), animated: true)
    }
}
```

## NavigationRoot

This protocol abstracts over the functionality of a UITabBarController we want to use. Usually, you’ll just have one of these owned by your AppCoordinator.

```swift
public protocol NavigationRoot {
    func setTabs(to navigationContexts: [NavigationContext])
    func switchTo(tab: Int)
    func present(navContext: NavigationContext, animated: Bool)
}
```

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

여기에는 SwiftUI가 없어요. 몇 개의 네비게이션 컨텍스트를 소유하고, 탭을 통해 그 사이를 전환할 수 있습니다.

탭 바 컨트롤러 위에 컨텍스트를 모달로 표시하는 한 가지 방법도 있어요. 이는 인증 처리 시 흔히 사용되는 방법입니다.

# 더 많은 기능 해제하기

이제 주요 구성 요소들을 모두 모았고, 16년된 Cocoa Touch API의 견고한 기반을 사용해 SwiftUI 앱을 개발할 수 있게 되었어요. 하지만 UIHostingController를 서브클래싱함으로써 이들에서 더 많은 성능을 뽑아낼 수 있어요. 이를 통해 다음과 같은 작업을 수행할 수 있습니다:

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

- UIAppearance API를 사용하지 않고 각 화면의 커스텀 스타일을 더 많이 활용할 수 있습니다.
- UIViewController 라이프사이클 이벤트를 활용하여 NavigationContexts에서 서로 다른 뷰 간의 상태를 전달할 수 있습니다.

다음은 기본 구현입니다:

```swift
final class MyHostingController<ContentView>:
    UIHostingController<ContentView> where ContentView: View {

    var onViewIsAppearing: (() -> Void)?
    var onViewDidDisappear: (() -> Void)?

    convenience init(rootView: ContentView,
                     onViewIsAppearing: (() -> Void)?,
                     onViewDidDisappear: (() -> Void)?) {
        self.init(rootView: rootView)
        self.onViewIsAppearing = onViewIsAppearing
        self.onViewDidDisappear = onViewDidDisappear
    }
}
```

이를 통해 navigate(to:) 메서드의 좀 더 복잡한 구현을 만들어낼 수 있습니다.

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

```swift
// Swift

public func navigate(to route: Route) {
    switch route {

    // ...

    case .profileRoute:
        self.profileNavigationContext = MyNavigationController()
        profileNavigationContext.setInitialView(view: ProfileView(),
                                              onViewIsAppearing: { self.showOverlay() },
                                              onViewDidDisappear: { self.hideOverlay() })
        navigationContext.present(navContext: profileNavigationContext, animated: true)


    case .webLink(let url):
        let safariVC = SFSafariViewController(url: url)
        navigationContext.present(vc: safariVC, animated: true)
    }
}
```

여기에서는 이미 있는 프리젠테이션 코드를 사용하여 부모 뷰에 오버레이를 보여주고 숨기기 위해 뷰 라이프사이클 이벤트를 구현하고 있습니다. 마지막으로, 표준 코디네이터 패턴을 사용할 때 가능한 UIKit 상호 운용성의 원활함을 보여주기 위해 SFSafariViewController를 표시합니다.

# 결론

코디네이터는 복잡한 SwiftUI 앱을 구조화하는 유일한 방법은 아닙니다. iOS 16를 대상으로 한다면 새로운 네비게이션 API를 사용해도 되고, 구성 가능 아키텍처를 시도해 볼 수도 있습니다.

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

이 모든 대안들의 공통점은 최신 기술이 끊임없이 발전한다는 것입니다. coordinators를 사용하면 두 가지 세계의 장점을 모두 누릴 수 있습니다:

- SwiftUI로부터의 반응적이고 빠른 UI 생성
- UIKit의 성숙한 내비게이션 및 강력한 사용자 정의 기능

또한 즉시 사용 가능한 상호 운용성을 확보하고 상태에 대해 거의 고민할 필요가 없습니다. 아직 SwiftUI에 완전히 뛰어들지 않았다면 결코 시대에 뒤떨어지지 않는 16년 된 패러다임들을 파악하고 오늘 바로 시작해 보세요.
