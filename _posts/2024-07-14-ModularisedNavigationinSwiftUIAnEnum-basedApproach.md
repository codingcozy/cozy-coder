---
title: "SwiftUI에서 모듈화된 네비게이션  Enum 기반 접근법"
description: ""
coverImage: "/assets/img/2024-07-14-ModularisedNavigationinSwiftUIAnEnum-basedApproach_0.png"
date: 2024-07-14 00:21
ogImage: 
  url: /assets/img/2024-07-14-ModularisedNavigationinSwiftUIAnEnum-basedApproach_0.png
tag: Tech
originalTitle: "Modularised Navigation in SwiftUI — An Enum-based Approach"
link: "https://medium.com/better-programming/modularised-navigation-in-swiftui-an-enum-based-approach-13028acd01ae"
isUpdated: true
---





iOS 16에서, SwiftUI가 새롭게 나타났어! 이제 네비게이션을 완전히 캡슐화할 수 있게 되었어. 이제 더 이상 많은 바인딩을 전달하거나 UI에 직접 NavigationLink를 사용할 필요가 없어졌어.

그러나 항상 새로운 도전이 되어버린 것 중 하나는 모듈화야.

최상의 상황에서는 각 모듈이 독립적으로 기능하고 다른 모듈(동일한 레이어에 있는)에 의존성이 없는 게 최고야.

<div class="content-ad"></div>

그 가능성은 있지만 항상 한 모듈에서 다른 모듈로 이동해야 하는 필요성이 있습니다.

이 문제를 우아하게 해결하는 한 가지 방법은 내부 및 외부 네비게이션에 대한 enum의 사용입니다.

# 컨셉

앱 (또는 그 일부)은 하나의 라우터 내에 번들로 포장됩니다. 진입점 (App/Root View/TabViews)은 NavigationPathis를 생성하고 보유한 후 NavigationStack에 전달합니다. NavigationPathis는 참조 유형이므로 우리는 그와 동일한 경로를 가진 주 라우터를 만들고 전달할 수 있습니다. 이제 주 라우터가 필요로 하는 것은 몇 가지 뷰를 push하고 pop하는 작업일 뿐입니다.

<div class="content-ad"></div>

모듈에 관해서는 각 모듈이 내부 경로를 처리하는 사용자 정의 라우터를 포함하고 있습니다. 이러한 라우터들은 주 라우터에 대한 참조를 받아 초기 NavigationPath를 수정할 수 있도록 합니다.

뷰 스택을 수정하려면, 각 모듈에는 모든 가능한 경로를 포함한 enum이 있습니다. 이 enum은 view.navigationDestination()에 의해 처리됩니다.

각 모듈은 내부 enum을 통해 내비게이션을 관리하며, 이는 view.navigationDestination()에 의해 매핑됩니다. 모든 모듈의 라우터는 여전히 주 라우터로 호출되므로, 라우터가 경로 자체에 대한 실제 참조를 가지고 있지는 않습니다.

마지막으로 모듈 간에 내비게이션하는 방법은 좀 까다롭습니다. 실제로 모듈들이 그렇게 직접적으로 하는 것은 아닙니다. 대신, 내부적으로 이동하는 것처럼 주 라우터에서 동일한 메서드를 호출하지만, 이번에는 다른 enum 값을 전달합니다. 이 enum은 모듈의 종료 지점을 처리합니다. 기술적으로, 구별 및 모듈 간 데이터 전송을 쉽게 하기 위해 관련 값이 다른 여러 종료점이 있을 수 있습니다.

<div class="content-ad"></div>

실제 앱은 모든 모듈에 대한 각 출구 Enum을 view.navigationDestination()를 통해 매핑하고 있습니다. 이번에는 네비게이션이 앱 레벨에서 발생하기 때문에 어떤 모듈이 어느 모듈로 이동하는지 알 수 없어 모듈들이 완전히 독립적입니다.

# 구현

일반적인 아이디어를 개요로 설명했으니 이제 실제 구현으로 넘어가 보겠습니다.

## 앱 레벨

<div class="content-ad"></div>

첫 번째로 해야 할 일은 모든 모듈이 상호 작용하는 전역 라우터입니다.

AppRouter:

```swift
public class AppRouter: ObservableObject {
    // 제한 사항: NavigationStack이 바인딩될 수 있도록 private이어야 합니다.
    @Published var path: NavigationPath

    init(with path: NavigationPath) {
        self.path = path
    }

    func navigate(to destination: AnyHashable) {
        path.append(destination)
    }

    func pop() {
        path.removeLast()
    }
}
```

## 모듈 레벨

<div class="content-ad"></div>

메인 라우터를 정의했으니, 이제 모듈로 넘어갈 수 있습니다. 각 모듈마다 자체 라우터가 있지만, 모듈 라우터에 대한 베이스 타입도 있으면 좋겠죠.

Markdown 형식으로 나타낸 Module Router입니다:

```swift
protocol ModuleRouter {
    var appRouter: AppRouter { get }
}
```

예시 모듈 라우터 구현:

```swift
// 내부 경로를 위한 Enum
enum DashboardRoute: Hashable {
    case details
}

// 외부 경로를 위한 Enum (모듈에서 나가는 경로)
enum DashboardExit: Hashable {
    case logout
    case settings
}

// 모듈 라우터
class DashboardRouter: ModuleRouter {
    var appRouter: AppRouter

    init(with appRouter: AppRouter) {
        self.appRouter = appRouter
    }

    func navigate(to target: DashboardRoute) {
        appRouter.navigate(to: target)
    }

    func pop() {
        appRouter.pop()
    }
}
```

<div class="content-ad"></div>

우리의 주 앱이 모듈의 경로를 결정하는 어떤 결정도 내리지 않도록 하기 위해 view의 확장을 정의합니다. 이 확장은 모듈의 라우트 enum을 특정 대상 뷰로 매핑합니다.

본 modifier는 메인 앱에서 손쉽게 호출할 수 있으며, 실제 네비게이션과 뷰에 대한 세부 정보를 알 필요가 없습니다.

```swift
// 이 모듈의 경로를 위한 사용자 정의 view modifier
public extension View {
    public func withDashboardRoutes() -> some View {
        self.navigationDestination(for: DashboardRoute.self) { destination in
            switch destination {
                // 이 모듈의 각 경로에 대한 네비게이션 로직 처리
                case .details:
                    DetailView()
                ...
            }
        }
    }
}
```

이제 모듈을 설정했으므로, 실제 앱에서 이들의 네비게이션을 바인딩할 수 있습니다.

<div class="content-ad"></div>


## 모듈 간 네비게이션

이제 각 모듈이 내부적으로 네비게이션할 수 있고 모든 네비게이션은 응용 프로그램에 바인딩되어 있으므로 남은 것은 모듈 간 네비게이션입니다. 이를 위해 각 모듈에 대한 추가적인 navigationDestinations를 간단히 추가하면 됩니다.

```js
import Dashboard
import OtherModule
import EvenAnotherModule

@main
struct Application: App {
    @ObservedObject var router: AppRouter
    
    init() {
        //실제 앱에서는 DI 컨테이너에서 가져와야 합니다.
        self.router = AppRouter()
    }
    
    var body: some Scene {
        WindowGroup {
            NavigationStack(path: $router.path) {
                // 다시 말하지만, 실제 앱에서는 DI 컨테이너에서 가져와야 합니다.
                DashboardView(router: DashboardRouter(with: router)) 
                    .withDashboardRoutes()
                    .withOtherModuleRoutes()
                    .withEvenAnotherModuleRoutes()
                    .navigationDestination(for: DashboardExit.self) { destination in 
                        switch destination {
                            case .logout:
                                LoginView()
                            case .settings:
                                SettingsView()
                        }
                    }
            }
        }
    }
}
```

<div class="content-ad"></div>

# 생각

NavigationStack 및 NavigationPath 덕분에 SwiftUI 네비게이션이 훨씬 개선되었습니다. Enum은 새로운 네비게이션을 사용하는 훌륭한 방법을 제공하지만, 모듈 간 네비게이션은 어떤 상충점을 가지고 있습니다. 이 개념은 내 의견대로 잘 작동하지만 당연히 유일한 방법은 아닙니다. 물론 sheets, modals 등이 빠진 몇 가지 부분이 있지만 대부분의 경우를 위한 뼈대 라우팅은 확실히 견고해야 합니다.

## 여기서부터 어떻게 해야 할까요?

- 모듈이 어떻게 `Routing entries that only return some View` - 앱의 뷰를 완전히 숨길 수 있을까요?
- 이 아키텍처에 sheets 및 모듈을 어떻게 도입할 수 있을까요?

<div class="content-ad"></div>

읽어 주셔서 감사합니다!