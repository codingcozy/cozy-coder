---
title: "팩토리 패턴 여러 모듈 등록하는 방법"
description: ""
coverImage: "/assets/img/2024-07-23-FactoryMultipleModuleRegistration_0.png"
date: 2024-07-23 21:24
ogImage: 
  url: /assets/img/2024-07-23-FactoryMultipleModuleRegistration_0.png
tag: Tech
originalTitle: "Factory Multiple Module Registration"
link: "https://medium.com/better-programming/factory-multiple-module-registration-f9d19721a31d"
---



![Factory with multiple modules](/assets/img/2024-07-23-FactoryMultipleModuleRegistration_0.png)

팩토리와 같은 다중 모듈을 사용하는 의존성 주입 시스템을 사용하려면 종종 "누가 먼저인가"의 딜레마에 직면하게 됩니다.

ModuleP가 abstractAccountLoading 프로토콜을 지정했다고 가정해 봅시다.



<div class="content-ad"></div>

그럼 다음은 회계 모듈인 ModuleA가 있습니다. 회계 모듈은 계정을 표시하지만 그것들을 불러오기 위해 로더 중 하나가 필요합니다.

마지막으로, 또 다른 모듈이 있습니다. 이를 ModuleB라고 부르겠습니다. ModuleB는 필요한 모든 종류의 로더를 만드는 방법을 알고 있습니다.

ModuleA와 ModuleB는 독립적입니다. 둘 다 서로에 대해 알지 못하지만 둘 다 모델과 프로토콜을 다루는 ModuleP에 직접적인 의존성을 갖고 있습니다.

이것은 클래식한 모듈 계약 패턴입니다.

<div class="content-ad"></div>

하지만 우리는 만들어야 할 애플리케이션이 있어요. ModuleA가 ModuleB에 대해 아무것도 모른 채로 계정 로더의 인스턴스를 어떻게 얻을 수 있나요?

음, 이 글은 의존성 주입에 관한 글이니 그 방향으로 이야기를 이어가 보죠.

## Resolver

이전 의존성 주입 시스템인 Resolver에서는 이런 문제가 실제로는 문제가 되지 않았어요. 두 모듈 모두 Resolver를 import하고, ModuleA의 어딘가에서 필요할 때 AccountLoading의 인스턴스를 요청하면 되었거든요.

<div class="content-ad"></div>

여기에 Injected 프로퍼티 래퍼를 사용한 예제가 있습니다.

```js
public class ViewModel: ObservableObject {
    @Injected var loader: AccountLoading
    @Published var accounts: [Account] = []
    func load() {
        accounts = loader.load()
    }
}
```

그리고 이게 다에요. 글쎄, 아니면 누군가가 Injected 프로퍼티 래퍼가 필요로 하는 인스턴스를 요청하기 전에 이 뷰 모델이 생성되기 전에 AccountLoading의 인스턴스를 등록해 놓은 상태라면 문제 없을 거예요.

만약 그런게 없다면... 음... 크래시 하게 될 거예요. 이것은 분명한 단점이지만, 대체로 코드를 실행하고 테스트해 보는 첫 번째 시도 때 등록을 잊는 것은 상당히 명백하기 때문에 문제는 없을 거예요.

<div class="content-ad"></div>

하지만 더 나은 방법을 찾기 위해 Factory를 만들었어요.

## 컴파일 시 안전

Factory는 컴파일 시 안전성을 보장합니다.

어떻게요? Factory는 원하는 것을 제공하기 위한 Factory가 존재함을 보증함으로써 이를 달성합니다. 이를 위해 요청한 것의 인스턴스를 제공하는 팩토리 클로저를 제공해야 하는 컨테이너에 적절한 Factory 객체를 추가합니다.

<div class="content-ad"></div>

일반 애플리케이션에서 Factory 등록은 보통 다음과 같이 보입니다.

```js
extension Container {
    public static let accountLoader = Factory<AccountLoading> { AccountLoader() }
}
```

Container가 존재합니다. AccountLoading 타입의 Factory가 존재합니다. 그리고 해당 타입의 인스턴스를 제공하는 클로저가 존재합니다. 그 결과, 컴파일 타임과 런타임에서 안전할 것입니다. 코드가 그렇지 않으면 간단히 컴파일되지 않습니다.

이러한 등록 방식은 모듈 애플리케이션에서도 동작할 수 있습니다. 모듈에서 프로토콜을 정의하고 구현하는 경우에도 해당됩니다. 프로토콜 유형과 Factory가 모두 공개되고, 구현 유형인 AccountLoader는 모듈 내부에서 정의되어 안전하게 숨겨집니다.

<div class="content-ad"></div>

그러나 우리의 교차 모듈 애플리케이션에서는 그렇게 할 수 없어요.

프로토콜은 ModuleP에서 정의되어 있어요. 구체적인 타입의 AccountLoader는 ModuleB에 존재하는데... 하지만 ModuleA는 이것을 모르죠. 알 수 없어요.

그러나 ModuleA의 코드는 Factory를 볼 수 있어야 합니다. 그리고 그 Factory에는 정의가 있어야 해요.

첫 번째는 누구일까요?

<div class="content-ad"></div>

## 선택적 등록

공장에서 이러한 문제에 대한 일반적인 해결책은 그냥 Factory가 선택적 타입을 제공하도록 하는 것입니다. 그래서 ModuleA에서는 이렇게 지정할 것입니다.

```js
extension Container {
    public static let accountLoader = Factory<AccountLoading?> { nil }
}
```

그리고 해당 모듈의 코드에서는 계정 로더를 요청하기 위해 이 인스턴스를 사용하게 됩니다. 이는 즉, Factory 기반의 뷰 모델 코드가 이제 다음과 같이 보일 것입니다.

<div class="content-ad"></div>

```javascript
class ViewModel: ObservableObject {
    @Injected(Container.accountLoader) var loader
    @Published var accounts: [Account] = []
    func load() {
        guard let loader else { return }
        accounts = loader.load()
    }
}
```

로드 함수에서 추가적으로 사용된 guard에 유의해 주세요. 이를 통해 우리가 필요한 로더가 있는지 확인합니다. 더 중요한 점은 옵셔널 타입의 사용으로 애플리케이션이 컴파일 시 안전하다는 것을 확신할 수 있다는 점입니다. 만약 누군가가 등록을 잊어서 nil 값이 반환된다면 애플리케이션이 올바르게 동작하지 않을 수 있지만 적어도 크래시는 발생하지 않습니다.

그렇다면 어떻게 작업을 수행할까요? 다시 말해, ModelA에는 Factory가 있습니다. 하지만 ModuleB에는 어카운트 로더를 만드는 방법을 알고 있는 네트워킹 코드가 포함되어 있습니다.

해결 방법은 비교적 간단합니다. 주 애플리케이션 어디선가 두 모듈을 연결하는 코드를 조금 작성해야 합니다.


<div class="content-ad"></div>

```js
import ModuleA
import ModuleB

extension Container {
    static func setupModules() {
        accountLoader.register { AccountLoader() }
    }
}
```

새로운 factory closure를 accountLoader에 등록함으로써, 이제 요청 시 새로 등록한 서비스를 반환하고 원래의 nil 값이 아님을 보장했습니다.

거의 다 왔습니다. 이제 할 일은 우리의 애플리케이션이 시작되기 전에 등록을 설정하는 함수를 호출하도록 하는 것만 남았습니다. 아마도 이렇게 하면 될 것 같네요.

```js
@main
struct AccountingApp: App {
    init() {
        Container.setupModules()
    }
    var body: some Scene {
        WindowGroup {
            NavigationView {
                ContentView()
            }
            .navigationViewStyle(StackNavigationViewStyle())
        }
    }
}
```

<div class="content-ad"></div>

그럼 여기까지입니다!

하지만 한 가지 주의할 점이 있어요: 어떤 똑똑한 사람이 선택적 처리를 피하거나 로더를 명시적으로 해제하거나 Factory 정의 자체를 명시적으로 해제할 것이라고 결정할 것 같은데, 제발 이런 일들은 하지 말아 주세요. 이러한 행동은 사실 Factory의 기본 원리를 완전히 무시하는 것과 같아요.

## 추가 문제

끝났나요? 정말로요?

<div class="content-ad"></div>

안녕하세요! 몇 가지 잠재적인 문제가 더 있습니다.

첫 번째 문제는 설정 함수를 호출하는 것을 기억해야 한다는 것입니다. 문제가 될 수는 있지만, 그 자체로는 큰 문제는 아닙니다.

하지만 두 번째 문제는 조금 더 은밀합니다. 다음을 고려해 보세요.

```js
@main
struct AccountingApp: App {
    let viewModel = ModuleA.ViewModel()
    init() {
        Container.setupModules()
    }
    var body: some Scene {
        WindowGroup {
            NavigationView {
                ContentView()
            }
            .environmentObject(ViewModel)
            .navigationViewStyle(StackNavigationViewStyle())
        }
    }
}
```

<div class="content-ad"></div>

ModuleA의 view model을 환경에 넣어야 할 때, 그냥 하나를 속성으로 생성하면 됩니다.

그런데 문제가 있어요.

시스템이 AccountingApp의 인스턴스를 만들려고 하면 struct의 속성이 초기화된 다음에 initializer가 발동합니다. 이는 ViewModel이 생성되고 Injected 속성 래퍼가 Factory에 계정 로더를 요청하기 전에 설정 함수가 호출되지 않는다는 것을 의미합니다.

그래서 우리는 아무것도 얻지 못합니다. 정말 아무것도요.

<div class="content-ad"></div>

이 예시는 다소 인위적이지만, 다중 모듈 환경에서 실제로는 생각하는 것보다 훨씬 더 자주 발생합니다.

## AutoRegistering

Resolver는 첫 해결이 발생하기 전에 자동으로 설정 함수를 호출함으로써 이 문제를 해결했고, 우연스럽게도 Factory에도 동일한 방법론의 버전을 추가했습니다.

여기에 우리가 수정한 설정 함수가 있습니다. 함수 이름이 registerAllServices로 변경되었으며, 이제 확장이 AutoRegistering을 준수함에 유의하십시오.

<div class="content-ad"></div>

```js
import ModuleA
import ModuleB

extension Container: AutoRegistering {
    static func registerAllServices {
        accountLoader.register { AccountLoader() }
    }
}
```

이제는 초기화 함수를 사용하지 않아도 됩니다.

```js
@main
struct AccountingApp: App {
    let viewModel = ModuleA.ViewModel()
    var body: some Scene {
        WindowGroup {
            NavigationView {
                ContentView()
            }
            .environmentObject(ViewModel)
            .navigationViewStyle(StackNavigationViewStyle())
        }
    }
}
```

팩토리는 이제 Factory 인스턴스가 처음 생성되기 전에 한 번만 registerAllServices를 자동으로 호출합니다.

<div class="content-ad"></div>

## 내부 동작

만약 당신이 이것을 아직 읽고 있다면 뭔가를 알고 싶어하는 사람일 것입니다. 내부 동작은 꽤 흥미로울 거야.

프로토콜은 충분히 간단해.

```js
public protocol AutoRegistering {
    static func registerAllServices()
}
```

<div class="content-ad"></div>

그리고 이전에 어떻게 사용하는지 보았습니다. 그러나 어떻게 Container 클래스가 해당 프로토콜을 준수하는지 확인할까요? 그리고 함수가 한 번만 호출되도록 어떻게 보장할까요?

```js
extension Container {
    fileprivate static var autoRegistrationCheck: Void = {
        (Container.self as? AutoRegistering.Type)?.registerAllServices()
    }()
}
```

먼저 한 번만 호출되는 문제에 대해 해결해 보겠습니다. 예전에는 dispatch_once를 사용했겠지만, Swift에서는 해당 함수가 사용되지 않도록 하고 있으며 대신 정적 변수를 사용하는 것을 고려해야 한다고 합니다.

그래서 autoRegistrationCheck를 만들었습니다. 그리고 약속한 대로, Swift 자체가 정적 변수 이니셜라이저가 한 번만 호출되도록 보장합니다.

<div class="content-ad"></div>

함수 내부의 `(Container.self as? AutoRegistering.Type)` 코드는 약간 이상해 보일 수 있지만, 기본적으로는 단지 누군가가 Container 타입을 AutoRegistering 타입으로 확장했는지 확인하는 것입니다.

캐스트된 타입이 nil이 아닌 경우, 해당 클래스 타입에서 `registerAllServices`를 호출합니다. 간단하죠.

Factory에 남은 것은 첫 번째 해결 전에 호출하는 것뿐입니다.

```js
func resolve(_ params: P) -> T {
     let _ = Container.autoRegistrationCheck
     // 남은 코드
}
```

<div class="content-ad"></div>

만약 Container가 AutoRegistering을 준수한다면, registerAllServices가 부작용으로 호출되며 반환된 Void가 폐기됩니다. 다음 호출에서는 무조건 폐기할 Void를 요청하고 있죠... 결과적으로 아주 적은 코드가 실행됩니다. (이 부분의 SIL 코드는 흥미로울 것 같아요.)

Resolver의 내부 확인 작업은 이것보다 훨씬 깔끔하지 않았지만, 부디 코드를 검토해보세요.

## 완료 블록

여기까지입니다. Factory와 함께 사용할 수 있는 여러 모듈 등록 및 해결 전략 그리고 마법이 일어나는 과정에 대한 간략한 살펴보기가 있습니다.

<div class="content-ad"></div>

당신도 알고 있겠지요. 질문이나 의견이 있으시면 아래에 적어주시고, 더 많은 내용을 보고 싶으시면 좋아요 버튼을 한참 눌러주세요.

진짜로요. Medium은 프로모션과 지불 알고리즘을 변경했기 때문에 박수를 보내고 댓글을 남기는 것이 엄청난 차이를 만들어냅니다.

다음에 또 뵙겠습니다.

이 기사는 스위프트 의존성 주입 시리즈의 일부입니다.