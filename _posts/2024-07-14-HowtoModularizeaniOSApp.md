---
title: "iOS 앱 모듈화 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-HowtoModularizeaniOSApp_0.png"
date: 2024-07-14 00:05
ogImage: 
  url: /assets/img/2024-07-14-HowtoModularizeaniOSApp_0.png
tag: Tech
originalTitle: "How to Modularize an iOS App"
link: "https://medium.com/gitconnected/how-to-modularize-an-ios-app-a8d5bdab8398"
---


이 글에서는 iOS 애플리케이션을 더 작은 모듈로 분할하는 방법에 대해 깊이 알아보겠습니다. XcodeGen, Clean Architecture 원칙, Xcode 템플릿 등을 활용할 것입니다. 사용자 인터페이스에는 SwiftUI를 사용하고, 네비게이션에는 UIKit를 사용할 것입니다.

간단히 말해서, 이 글을 읽은 후에 배우게 될 내용은 다음과 같습니다:

- 깨끗한 아키텍처 접근 방식에 따라 iOS 애플리케이션을 계층으로 나누는 방법.
- 환형 종속성 방지하는 방법.
- 앱 모듈화를 위해 XcodeGen 사용하는 방법.
- 인터페이스 모듈 접근 방식이 무엇이며 모듈화된 앱에서 네비게이션을 어떻게 구현하는지.
- GraphViz를 사용하여 간단한 명령줄 호출로 의존성 다이어그램을 생성하는 방법.
- 기타 iOS 모듈화 도구들의 사용 가능 여부.

<div class="content-ad"></div>

**데모 앱**

지금 모듈화할 준비가 된 데모 앱을 확인해보세요:

![Demo App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_1.png)

<div class="content-ad"></div>

안녕하세요! 이 프로젝트에서는 Sign In, Quotes, Quote Detail 및 Settings의 네 개의 화면을 갖고 있습니다. 각각을 별도의 기능으로 만들 계획입니다.

플로우는 다음과 같습니다:

- "Sign In" 화면은 사용자가 아직 로그인하지 않은 경우에 표시됩니다. 그렇지 않으면 "Quotes" 화면이 표시됩니다.
- "Sign In" 버튼을 탭하면 사용자가 인증되고 "Quotes" 화면이 표시됩니다.
- 특정 인용구를 탭하면 "Quote Detail" 화면으로 이동하며, "Delete quote" 버튼을 탭하여 인용구를 삭제할 수 있습니다.
- 인용구가 삭제되면, 삭제된 인용구가 표시되지 않는 "Quotes" 화면으로 자동으로 이동됩니다. 이 기능을 통해 두 개의 격리된 기능이 서로 통신하는 방법을 보여줍니다.
- 또한 탭 바가 있으므로 "Settings" 화면으로 이동하여 "Sign Out" 버튼을 탭하여 로그아웃할 수 있습니다.
- 로그아웃하면 계층 구조가 파괴되고 다시 "Sign In" 화면이 표시됩니다.

우리는 "The Simpsons"에서 명언, 인물 및 이미지 URL을 불러올 The Simpsons Quote API를 사용할 것입니다.

<div class="content-ad"></div>

> 참고: The Simpsons Quote API는 무료 플랫폼에 공개되어 있어, 네트워크 요청을 너무 자주 사용하면 400 에러를 받을 수 있습니다. 요청을 스팸으로 사용하지 않도록 주의해주세요. 그렇지 않으면 잠시 기다려야 할 수도 있습니다. API가 다시 이용 가능해질 때까지:

![How to Modularize an iOS App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_2.png)

이제 우리가 구축할 모든 기능을 알게 되었으니, 모듈화의 이점과 단점을 알아봅시다.

# 모듈화의 이점

<div class="content-ad"></div>

큰 Xcode 프로젝트를 다룰 때 한 파일을 조금 수정하면 긴 빌드 시간이 걸리는 것을 자주 느낍니다. 이는 모든 .swift 파일을 포함하는 하나의 타겟이 있기 때문에 발생합니다. 어떤 것을 변경할 때마다 Xcode가 전체 타겟을 다시 빌드하기 때문입니다. 프로젝트가 클수록 컴파일하고 처리할 코드와 자원이 많아져 전체 빌드 시간이 늘어납니다.

큰 Xcode 프로젝트를 모듈로 나누면 독립적인 모듈이나 프레임워크로 나눌 수 있습니다. 각 모듈이나 프레임워크는 별도의 타겟을 가지고 있어 다른 모듈과 독립적으로 빌드하고 컴파일할 수 있습니다. 특정 모듈을 변경할 때 해당 모듈만 재빌드하면 되므로 전체 빌드 시간을 단축할 수 있습니다.

모듈화를 사용하면 다음과 같은 이점을 누릴 수 있습니다:

- 증분 빌드. Xcode는 변경된 모듈만 다시 빌드하며 전체 프로젝트를 다시 빌드하지 않습니다.
- 병렬 빌드. Xcode는 다중 코어 프로세서의 성능을 활용하여 여러 모듈을 병렬로 빌드할 수 있습니다.
- 캡슐화. 모듈화를 통해 원하지 않는 의존성을 도입하기가 어려워지며, 구성 파일에서 명시적으로 정의해야 합니다.
- 재사용성. 특정 모듈은 다른 프로젝트에서 재사용할 수 있어 중복을 제거할 수 있습니다.

<div class="content-ad"></div>

# 모듈화의 단점

모듈화는 다수의 장점을 제공하지만, 잠재적인 단점을 고려하는 것이 중요합니다.

- 복잡성: 의존성을 성공적으로 처리하고 모듈 간에 효율적으로 커뮤니케이션하며 명확한 모듈 경계를 설정하기 위해서는 주의 깊게 계획하고 아키텍처를 신중하게 설계해야 합니다.
- 학습 곡선: 개발 팀이 특정 모듈화 도구에 익숙하지 않은 경우, 모듈화는 개발팀에게 학습 곡선을 도입할 수 있습니다. 예를 들어 XcodeGen의 경우, 팀은 project.yml 파일을 어떻게 관리해야 하는지 배워야 합니다. Tuist와 같이 Swift를 사용하는 도구라도 배우는 데 시간이 필요할 수 있습니다.
- 오용: 완벽하게 모듈화된 앱을 만들려는 시도에서 개발자는 여전히 더 긴 빌드 시간 문제를 겪을 수 있습니다. 이는 기능 모듈이 다른 기능 모듈에 의존할 때 발생할 수 있으며, 종속성 그래프를 수직으로 만들어버릴 수 있습니다. Feature A가 Feature B에 의존하고, Feature B가 Feature C에 의존한다면, Feature C를 변경할 때 다른 모든 기능이 다시 빌드될 수 있습니다. 이를 극복하기 위해 기능을 격리시켜야 합니다. 이것이 이번 튜토리얼에서 해볼 것입니다.

# 시작합시다.

<div class="content-ad"></div>

Let's start creating an Xcode project named SimpsonsQuotes. Opt for Storyboard for Interface now; however, we'll eventually switch to SwiftUI for the user interface:

![project structure](/assets/img/2024-07-14-HowtoModularizeaniOSApp_3.png)

Afterward, remove the Main.storyboard file, as well as ViewController.swift and Info.plist. The deletion of Info.plist is necessary because we'll handle it through XcodeGen in the future.

Now, the project's layout appears as follows:

<div class="content-ad"></div>

이제 AppDelegate.swift로 이동하여 다음과 같이 수정해봅시다:

configurationForConnecting 메서드를 수정하여 AppDelegate에 SceneDelegate 클래스에 대해 알려줍니다. 기본적으로 Info.plist에서 처리되지만, 너무 많은 세부 사항을 담아 정의를 혼란스럽게 만들고 싶지 않습니다.

게다가, 이 튜토리얼에서 필요 없는 메서드를 제거하여 SceneDelegate를 리팩토링해봅시다:

<div class="content-ad"></div>

To finish the project setup, let’s create a `.gitignore` file which we will fill as follows:

In your own projects, this file will likely have more ignored files, but for the sake of simplicity, we keep it short in our case. The goal is to get rid of the `.xcodeproj` and generate the project based on a configuration file of XcodeGen.

With that being done, we can start working with XcodeGen.

# XcodeGen Setup

<div class="content-ad"></div>

만약 XcodeGen을 설치하지 않았다면 Homebrew를 통해 설치할 수 있어요:

```js
brew install xcodegen
```

이미 설치했다면, 먼저 다음 명령어로 버전을 확인해 보세요:

```js
brew info xcodegen
```

<div class="content-ad"></div>

이 글은 Xcode 14.3.1을 사용하고 있으며, 해당 버전은 XcodeGen 2.36.1부터 지원합니다. 그러나 최신 버전을 설치할 수도 있습니다. 저의 경우, 2.37.0 버전을 사용 중입니다. 

![How to Modularize an iOS App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_5.png)

이에 XcodeGen의 이전 버전을 사용 중이라면 다음과 같이 업데이트할 수 있습니다:

```js
brew upgrade xcodegen
```

<div class="content-ad"></div>

이제 XcodeGen을 마쳤으니 모듈 구조를 만드는 데 사용해봅시다.

# XcodeGen 사용하기

우리는 project.yml 파일을 사용하여 프로젝트를 관리할 것입니다. .xcodeproj 파일이 있는 폴더로 이동하여 다음과 같이 파일을 만들어주세요:

![How to Modularize an iOS App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_6.png)

<div class="content-ad"></div>

이제 .xcodeproj 파일을 삭제하고 생성된 project.yml 파일을 열어봅시다:

![HowtoModularizeaniOSApp_7](/assets/img/2024-07-14-HowtoModularizeaniOSApp_7.png)

이 파일에 다음을 추가합시다:

- 프로젝트 이름을 "SimpsonsQuotes"로 지정합니다.
- 번들 ID 접두어를 com.zafar.simpsons-quotes로 설정합니다. 이 필드는 제품 번들 ID와 다릅니다. 나중에 추가되는 각 프레임워크에 이 접두어 ID가 자동으로 설정됩니다.
- 이후에 필요한 createIntermediateGroups 옵션을 추가합니다. true로 설정된 이 옵션은 생성된 Xcode 프로젝트가 Finder에 표시된 폴더 구조와 동일하게 포함되도록 합니다. 예를 들어 Application/Presentation/MyModule에 모듈이 있을 경우, Xcode 프로젝트에도 동일한 계층 구조가 포함됩니다. 이 속성을 true로 설정하지 않으면 생성된 프로젝트에는 MyModule 폴더만 표시됩니다.
- iOS 15를 타겟팅하므로 deploymentTarget을 해당 버전에 맞게 설정합니다.

<div class="content-ad"></div>

지금은 앱을 실행하는 데 사용될 메인 Application target을 생성할 것입니다 (이전에 추가한 코드를 반복하지 않고 "..." 표기법을 사용했습니다):

- Application이라는 새 target을 생성합니다.
- 유형은 application으로 설정됩니다. 나중에 각각의 모듈에 대해 프레임워크 유형을 사용할 것입니다.
- 플랫폼을 iOS로 설정합니다.
- info 키는 Info.plist에 필요한 항목을 지정하는 데 사용됩니다. 먼저 파일이 저장될 경로와 해당 속성을 지정합니다. 우리의 경우 Info.plist가 아직 존재하지 않는 경우 자동으로 생성될 것입니다.
- 사용할 LaunchScreen 파일 이름을 설정하여 실행 화면, 표시된 앱 이름, 버전, 화면 방향은 세로 고정만, 비-HTTPS 요청 허용을 설정합니다.
- 설정에서 TARGETED_DEVICE_FAMILY를 1로 설정하여 iPhones에서만 앱이 실행되도록 합니다. PRODUCT_BUNDLE_IDENTIFIER는 com.zafar.simpsons-quotes로 설정됩니다.
- 마지막으로, sources를 설정하여 target에 포함될 파일을 지정합니다.

이렇게 한 다음에는 생성된 target을 위한 폴더를 추가하고 파일을 그곳으로 이동할 것입니다.

먼저, AppDelegate.swift 및 기타 파일이 있는 폴더로 이동해 주세요:

<div class="content-ad"></div>

**안녕하세요!**

요청하신 이미지 태그를 Markdown 형식으로 변경해 드리겠습니다.

![HowtoModularizeaniOSApp_8](/assets/img/2024-07-14-HowtoModularizeaniOSApp_8.png)

이어서, 어플리케이션 폴더를 생성해 주세요:

![HowtoModularizeaniOSApp_9](/assets/img/2024-07-14-HowtoModularizeaniOSApp_9.png)

이제 모든 파일을 어플리케이션 폴더로 옮겨주세요.

<div class="content-ad"></div>

이제 우리는 다음 모듈을 위한 기초가 될 기본 구조로 프로젝트를 생성할 수 있습니다. project.yml이 있는 폴더로 이동하여 xcodegen을 실행하세요:

![image_10](/assets/img/2024-07-14-HowtoModularizeaniOSApp_10.png)

프로젝트가 생성되었음을 확인할 수 있으며, 열어보면 다음과 같은 구조를 볼 수 있습니다:

<div class="content-ad"></div>

![image](/assets/img/2024-07-14-HowtoModularizeaniOSApp_12.png)

우리는 첫 번째 모듈인 Application을 성공적으로 만들었습니다. 이제 개별 기능을 생성하고 모듈로 묶을 수 있습니다.

자산을 각각의 모듈로 이동하는 것으로 시작하겠습니다.

# 앱 모듈화하기

<div class="content-ad"></div>

저희는 Clean Architecture 접근 방식에 따라 애플리케이션을 구조화하고 전체 앱을 다음과 같은 계층으로 나눌 것입니다:

- Application — 우리의 경우에는 AppDelegate, SceneDelegate, LaunchScreen 및 애플리케이션의 Info.plist가 포함될 것입니다.
- Presentation — 자산, 기능 (Quotes, QuoteDetail 등), 탐색 구성 요소 및 UI에 사용될 모델을 포함할 것입니다.
- Data — 특정 비즈니스 로직을 다루는 서비스가 포함될 것입니다.
- Domain — 엔티티(Codable 모델로 받는 백엔드 데이터) 및 네트워커 구성 요소가 포함될 것입니다.

Presentation 레이어로 Assets를 옮길 것이므로 다음과 같이 Presentation 및 Assets 폴더를 생성해봅시다:

![이미지](/assets/img/2024-07-14-HowtoModularizeaniOSApp_13.png)

<div class="content-ad"></div>

The -p flag is like magic; it will conjure up parent directories if they're missing.

Now, let's transport the Assets.xcassets to the Assets folder:

![Check out the image here for guidance](/assets/img/2024-07-14-HowtoModularizeaniOSApp_14.png)

Before we can mention the Assets module within the project.yml file, let's craft a standard template for modules:

<div class="content-ad"></div>

- targetTemplates 키를 사용하여 새 템플릿을 만듭니다.
- 템플릿 이름은 Module로 지정합니다.
- 유형은 framework로, 플랫폼은 iOS이고 배포 대상은 iOS 15.0으로 지정합니다.
- GENERATE_INFOPLIST_FILE 키는 모듈을 위해 Info.plist가 자동으로 생성되어야 하는지 여부를 결정합니다. 우리는 수동으로 만들기 싫으므로 true로 설정합니다.
- TARGETED_DEVICE_FAMILY는 다시 iPhones만을 대상으로 하므로 1로 설정합니다.
- SUPPORTS_MACCATALYST는 Mac에서 앱을 실행하는 것에 관심이 없기 때문에 false로 설정합니다.

템플릿을 완료했으니 이제 Assets 모듈을 만들어 봅시다:

Assets.xcassets에 AppIcon이 포함되어 있고 Xcode가 앱 아이콘이 애플리케이션 자체의 대상에 포함되어 있기를 기대하기 때문에 Application 대상을 다음과 같이 수정할 것입니다:

- SimpsonsQuotes/Presentation/Assets/Assets.xcassets 경로를 Application의 소스에 추가했습니다.
- 애플리케이션은 이제 Assets 모듈에 의존합니다.

<div class="content-ad"></div>

지금 xcodegen을 실행하고 프로젝트를 열면 생성된 Assets 모듈을 볼 수 있을 거에요.

![Assets module](/assets/img/2024-07-14-HowtoModularizeaniOSApp_15.png)

파편화 기능에 들어가기 전에 먼저 더 작은 구성 요소를 처리해보죠. Data 및 Domain 레이어 내의 모듈들을 다루게 될 거에요:

- Endpoints (Data)
- Services (Data)
- Networker (Domain)
- Entities (Domain)

<div class="content-ad"></div>

그들 간의 관계는 다음과 같습니다:

Endpoints는 에디티티를 가져오기 위해 서비스에서 사용되는 특정 REST API 엔드포인트를 나타냅니다. 서비스는 실제로 Networker에 종속적이거나 아닐 수 있습니다. 우리의 경우, quotes 엔드포인트에서 배열로 인용구를 가져올 것입니다. QuotesService가 이 비즈니스 로직을 담당할 것입니다. 게다가, 인증 작업을 처리할 AuthService를 생성할 것입니다.

Domain 레이어부터 시작하여 Networker와 Entities를 위한 모듈을 생성하겠습니다.

# Domain Layer 모듈화

<div class="content-ad"></div>

First, let's create Domain and Networker folders like this:

![folder structure](/assets/img/2024-07-14-HowtoModularizeaniOSApp_16.png)

Next, move to the project.yml and define the Networker module:

Now, follow the same steps for Entities:

<div class="content-ad"></div>

이미지를 확인해 주세요: 

![How to Modularize an iOS App - Step 17](/assets/img/2024-07-14-HowtoModularizeaniOSApp_17.png)

다음으로 xcodegen을 다시 실행하면 성공적으로 대상이 생성된 것을 확인할 수 있습니다:

![How to Modularize an iOS App - Step 18](/assets/img/2024-07-14-HowtoModularizeaniOSApp_18.png)

이제 Networker.swift 파일을 생성해 보겠습니다. (대상을 Application이 아닌 Networker로 설정했음에 주의하세요)

<div class="content-ad"></div>

![Networker implementation](/assets/img/2024-07-14-HowtoModularizeaniOSApp_19.png)

네트워커는 다음과 같이 구현됩니다.

네트워커의 구현 세부 정보에 주의를 기울이지 않을 것입니다. 우리가 알아야 할 것은 그 작업이 응답에서 특정 객체를 가져오는 것입니다.

다음으로 Quote 엔티티를 생성해 봅시다 (대상이 Entities로 설정되어 있음을 유의하세요):

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-14-HowtoModularizeaniOSApp_20.png)

앱의 도메인 레이어가 완료되었으니 이제 데이터 레이어를 처리해 봅시다.

## 데이터 레이어 모듈화

우리는 데이터 및 엔드포인트 폴더를 만들고 project.yml 파일을 수정하는 것으로 시작합니다.

<div class="content-ad"></div>

**이제, 서비스 모듈에 대해 동일한 작업을 수행합니다:**

![Services module](/assets/img/2024-07-14-HowtoModularizeaniOSApp_22.png)

이번에는 서비스가 엔드포인트, 엔티티 및 네트워커에 의존한다는 점을 명심하세요.

<div class="content-ad"></div>

마찬가지로 도메인 레이어와 같이, xcodegen을 실행하고 변경 사항을 검토해 봅시다:

![How to Modularize an iOS App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_23.png)

여기서 볼 수 있듯이, Endpoints와 Services는 이제 독립적인 모듈입니다.

다음으로, Endpoints 타겟을 위한 Endpoint.swift 파일을 생성하면서 계속 진행합니다:

<div class="content-ad"></div>

이 간단한 구조는 특정 REST API 엔드포인트를 나타낼 것입니다.

다음은 Endpoint+Base.swift 파일을 추가합니다.

여기서 모든 다가오는 SimpsonsQuotes 엔드포인트를 위한 기본 URL과 헤더를 지정합니다 (간단하게 두 개를 사용할 것이지만, 실제 프로젝트에서는 많은 엔드포인트가 있을 것입니다).

이제 Quotes 엔드포인트를 만들어봅시다.

<div class="content-ad"></div>

본문을 보면 우리는 카운트 매개변수가 있는 두 개의 엔드포인트를 가지고 있다는 것을 알 수 있습니다. 우리 프로젝트에는 전자를 사용할 것입니다.

이 3개의 파일은 Application이 아니라 Endpoints 대상으로 작성되어야 합니다.

마지막으로, Services 대상으로 AuthService와 QuotesService를 생성함으로써 Data 레이어를 마무리짓겠습니다.

AuthService는 다음과 같이 구현되어 있습니다.

<div class="content-ad"></div>

간단히 말씀드리면, 인증 상태를 처리하기 위해 내부적으로 UserDefaults를 사용할 것입니다. 요청은 결과를 반환하기 전에 2초간 대기하도록 모의로 구현되어 있습니다.

마지막으로, QuotesService를 구현할 것입니다:

여기서는 Endpoints 및 Networker 모듈을 가져와 사용한다는 것을 볼 수 있습니다.

이제 최종 레이어, 즉 Presentation을 모듈화할 차례입니다.

<div class="content-ad"></div>

# 프레젠테이션 계층 모듈화

피쳐에 대한 모듈을 생성하기 전에 UI 아키텍처 접근 방식을 살펴봅시다. SwiftUI를 사용하여 사용자 인터페이스를 처리하고, UIKit를 사용하여 탐색을 처리할 것입니다. 각 피쳐를 서로 분리하고 응용 프로그램 아키텍처의 더 높은 수준에서 연결하기 원하기 때문입니다.

일반적으로 우리가 사용할 MVVM 접근 방식은 이렇습니다:

![image](/assets/img/2024-07-14-HowtoModularizeaniOSApp_24.png)

<div class="content-ad"></div>

- 뷰(View) — ViewModel에 종속성을 가진 기본 SwiftUI 뷰입니다.
- ViewModel — 특정 뷰(View)의 상태를 나타내는 ObservableObject 클래스입니다.
- 모델(Model) — 뷰(View)에 적용하기 편리한 프레젠테이션 모델입니다.
- 워커(Worker) — 서비스 및 데이터 레이어의 비즈니스 로직 컴포넌트에 종속성이 있는 선택적인 구성 요소입니다.
- 내비게이션(Navigation) — 특정 기능에 대한 내비게이션을 설명하는 프로토콜입니다. 아래에서 이를 어떻게 사용할지 설명하겠습니다.
- 어셈블리(Assembly) — 다이어그램에 표시되지 않는 구성 요소로, 정적 메서드가 있는 단순한 열거형(enum)입니다. 내부적으로 모든 종속성을 연결하고 소비자에게 사용 준비가 된 뷰(View)를 생성할 것입니다. 특정 기능의 특정 뷰(View)를 초기화하려면 해당 기능의 어셈블리를 활용할 것입니다.

# 내비게이션 구현

Quotes와 Settings 화면이 있다고 가정해봅시다. Quotes에서 Settings로 이동하려면 Settings 모듈을 Quotes 모듈에 가져와야 합니다. 하지만, 이는 Quotes가 Settings를 가져오고 그 반대도 마찬가지인 순환 종속성 문제를 만들 수 있습니다. 이를 피하기 위해, 기능들은 완전히 서로 격리되어야 합니다. 다시 말해, 기능 모듈은 다른 기능 모듈을 가져오지 않아야 합니다.

그렇다면 남아있는 질문은, 다른 기능으로 이동하는 방법이지만 실제로 그것에 종속성이 없는 방법은 무엇인가? 인터페이스 모듈 접근 방식을 따르거나, 기능 자체보다 한 수준 위로 내비게이션 로직을 올리는 것이 답입니다.

<div class="content-ad"></div>

인터페이스 모듈 접근 방식은 각 피처 모듈에 대해 해당 피처를 설명하는 간단한 프로토콜 모듈을 하나 더 생성하는 것을 의미합니다. 따라서 하나의 구체적인 피처 A는 피처 B 인터페이스 모듈에 의존하며, 피처 B는 피처 B 인터페이스를 구현할 것입니다.

![image](/assets/img/2024-07-14-HowtoModularizeaniOSApp_25.png)

이 방식은 피처 간을 분리하는 유효한 방법이며, 인터페이스는 구현 세부 사항보다 덜 자주 변경되기 때문에 빌드 시간을 줄이는 데 도움이 됩니다. 따라서 피처 B에서 무언가를 변경해도 피처 A를 다시 빌드할 필요가 없습니다. 왜냐하면 피처 B 인터페이스가 변경되지 않았기 때문입니다.

그러나 이 방식은 생성되는 모듈의 수가 두 배로 되고, 대부분의 대규모 응용 프로그램에는 과도한 것으로 여겨집니다. 저희의 경우, 대신 네비게이션 부분을 기능보다 한 단계 위로 올릴 것입니다. 위의 MVVM 다이어그램에서는 Navigation 프로토콜이 보입니다. 이 프로토콜을 각 ViewModel 파일에 정의하고 ViewModel이 이에 의존하도록 할 것입니다. 어셈블리를 통해 기능을 생성할 때 그 네비게이션은 매개변수로 전달될 것입니다. 따라서 본질적으로 우리는 피처 위에 네비게이터 구성 요소를 두게 될 것입니다.

<div class="content-ad"></div>

![How to Modularize an iOS App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_26.png)

위에서 볼 수 있듯이, 기능 간에 화살표가 없어서 표현 계층을 가능한 한 수평으로 만듭니다. 저는 위 다이어그램을 완성된 프로젝트에서 Graph.viz를 사용해 생성했는데, 이에 대해 글의 끝에서 설명하겠습니다.

이제 우리의 Xcode 프로젝트로 돌아가 Presentation 안에 Navigator라는 새 폴더를 추가해봅시다:

![How to Modularize an iOS App](/assets/img/2024-07-14-HowtoModularizeaniOSApp_27.png)

<div class="content-ad"></div>

And now, let's dive into creating a module for it:

After running `xcodegen` again, you can confirm that the Navigator target has been effectively created. 

Next, we'll include a NavigatorProtocol file that will be utilized by specific Navigators in the future:

This file consists of a `start()` method designed for navigating to the feature. For instance, with the StartNavigator, the `start()` method will guide you from the current view to the StartView.

<div class="content-ad"></div>

# 기능 구현하기

개별 기능을 더 빠르게 생성하는 방법은 Xcode 템플릿을 활용하는 것입니다. 완성된 데모 프로젝트의 저장소에 포함된 Xcode 템플릿을 활용할 수 있습니다. 이를 Xcode에 통합하려면 SwiftUI MVVM Screen.xctemplate 폴더를 Xcode/Templates 폴더로 복사하여 붙여넣으세요.

이제 우리는 기능 생성을 시작할 준비가 되었습니다. 이번에 생성할 기능들은 다음과 같습니다:

- LaunchScreen과 동일한 외관을 갖는 Start 기능. 인증 상태에 따라 사용자는 자동으로 Sign In 또는 Main 기능으로 이동됩니다.
- 사용자가 탭하면 간단한 "로그인" 버튼이 있는 Sign In 기능. 사용자가 인증되고 앱이 Main 기능으로 이동됩니다.
- 지정된 경로를 가진 UITabBarController가 있는 Main 기능. 이 기능의 Navigator는 개별 기능을 생성하고 UITabBarController와 연결할 것입니다. 전반적으로 Main 기능은 다른 기능들을 위한 컨테이너 역할을 합니다.
- 명언 목록을 보여주는 Quotes 기능. 명언을 탭하면 사용자가 명언 세부 정보 기능으로 이동합니다.
- 심슨 캐릭터, 명언 및 "명언 삭제" 버튼을 표시하는 Quote Detail 기능. 이 버튼을 통해 분리된 기능끼리 어떻게 대화를 나눌 수 있는지 보여줍니다. "명언 삭제" 버튼을 탭하면 Quotes 기능에서 더 이상 해당 명언을 표시하지 않습니다.
- "로그아웃" 버튼을 표시하는 Settings 기능. 이 버튼을 탭하면 인증 상태가 재설정되고 사용자는 Start 기능으로 이동합니다.

<div class="content-ad"></div>

UI 및 비즈니스 로직 유당를 만드는 방법에 대해 심층적으로 다루지는 않을 거에요. 이 글의 초점은 아니니까요. Start 기능이 어떻게 구현되었는지 자세히 살펴보고, Main 모듈이 컨테이너로 사용되며, 마지막으로 Quote Detail이 Quotes 기능과 어떻게 소통하는지 확인해볼 거예요.

## Start 기능

먼저, 해당 기능에 대한 폴더를 만들어요:

![HowtoModularizeaniOSApp_28](/assets/img/2024-07-14-HowtoModularizeaniOSApp_28.png)

<div class="content-ad"></div>

다음으로 `project.yml` 파일을 생성합니다 (서비스를 종속성으로 지정한다는 점을 주의하세요):

`xcodegen`을 실행한 후에 `Start` 타겟이 생성되었습니다.

이제 파일을 모두 생성해 봅시다. "새 파일 생성"을 탭하고 "템플릿" 헤더 아래에서 "SwiftUI MVVM Screen" 템플릿을 찾습니다:

![이미지](/assets/img/2024-07-14-HowtoModularizeaniOSApp_29.png)

<div class="content-ad"></div>

**스타트** 모듈을 **시작**이라고 명명하고:

![시작 모듈](/assets/img/2024-07-14-HowtoModularizeaniOSApp_30.png)

또한 당연히 **스타트** 타겟만 선택하세요:

![스타트 타겟](/assets/img/2024-07-14-HowtoModularizeaniOSApp_31.png)

<div class="content-ad"></div>

결과적으로, 생성된 네 개의 파일을 확인할 수 있습니다:

![Worker](/assets/img/2024-07-14-HowtoModularizeaniOSApp_32.png)

먼저, Worker부터 시작하겠습니다:

인증된 사용자인지를 결정하는 프록시로 작동하는 것을 확인할 수 있습니다.

<div class="content-ad"></div>

다음으로 ViewModel을 다룹니다:

- 우리는 나중에 Navigator에 의해 구현될 performRoute(_route:) 메서드가 포함 된 StartNavigation 프로토콜이 있습니다.
- 우리는 signIn과 main 두 개의 라우트를 정의했습니다.
- ViewModel은 Worker 및 Navigation에 종속됩니다.
- Input 표시는 View에서 호출되는 메서드를 포함합니다. onViewAppear()는 인증 확인을 트리거하고 그에 따라 내비게이션이 수행됩니다.

뷰:

- 우리는 StartViewModel에 의존합니다.
- .onAppear 클로저 내에서 해당 ViewModel 메서드를 호출합니다.
- 미리보기를 위해 우리는 내비게이션 매개 변수를 null로 조립합니다, 왜냐하면 우리는 여기서 내비게이션을 테스트하지 않기 때문입니다.

<div class="content-ad"></div>

어셈블리를 가져왔습니다.

이전에 말했듯이, 이는 기능의 구성 요소를 결합하는 간단한 열거형입니다. 네비게이션 매개변수는 StartNavigator 내부에서 채워질 것입니다.

이제 StartNavigator를 만드는 시간입니다. Presentation/Navigator 안에 "Feature Navigators"라는 폴더를 추가하고 aStartNavigator.swift 파일을 만듭니다.

- 우리는 start() 메서드를 제공하는 기본 NavigatorProtocol을 준수하며, StartView에서 라우트를 처리하기 위해 StartNavigation을 사용합니다.
- Start가 초기 기능이므로 UIWindow에 의존합니다. startViewController는 StartViewModel이 StartNavigator를 강하게 참조하기 때문에 약한 변수로 정의됩니다.
- start() 메서드 내에서 StartAssembly를 사용하여 SwiftUI View를 생성하고, 그 후 UIHostingController로 감싸서 UIWindow의 rootViewController로 설정합니다.
- performRoute(_ route:) 메서드는 SignIn 기능 또는 Main 기능으로 이동합니다.

<div class="content-ad"></div>

자, 이제는 StartNavigator를 SceneDelegate와 연결해야 할 시간이에요. SceneDelegate를 다음과 같이 수정해봐요:

- Navigator 모듈을 import 해줍니다.
- NavigatorProtocol에 의존성을 선언해줍니다.
- UIWindow를 이용하여 navigator를 초기화해줍니다.

이제 project.yml 파일을 수정하여 Navigator 모듈을 Application의 의존성으로 선언하고 xcodegen을 다시 실행해봐요:

현재 Main과 SignIn과 그 구성요소들이 존재하지 않아서 컴파일 타임 에러가 발생하고 있는 건 아시죠.

<div class="content-ad"></div>

우리는 관심이 없는 Sign In 기능 생성을 다루지 않을 거예요. 하지만 여기서 코드를 확인하실 수 있어요. 

# 컨테이너로 동작하는 주요 기능

Start와 유사하게 Main 모듈을 생성한 후, 두 개 파일로 채워넣습니다.

먼저 MainTabBarController를 생성해요:

<div class="content-ad"></div>

- "Start(시작)"와 같이 MainNavigation 프로토콜을 선언합니다.
- 두 개의 탭을 가지고 있으므로 Routes enum에는 quotes와 settings 경우가 포함되어 있습니다.
- MainNavigation에 의존성을 선언합니다.
- viewWillAppear(_animated:)에서 setupRoutes(_routes:) 메서드를 호출하여 탭을 설정합니다.

이제 MainAssembly를 생성해 봅시다:

여기에서는 MainTabBarController를 간단히 반환합니다.

기능을 위한 MainNavigator는 아래와 같이 생성됩니다:

<div class="content-ad"></div>

- Main 기능을 표시할 때 sourceViewController가 필요합니다.
- 순환 참조를 피하기 위해 view controllers을 unowned 및 weak으로 표시했습니다. sourceViewController는 MainNavigator보다 오래 살아남기 때문에 unowned로 표시할 수 있습니다.
- start() 메소드 내에서 MainTabBarController를 초기화하고 표시합니다.
- setupRoutes(_routes:) 메소드에서 QuotesNavigator 및 SettingsNavigator를 사용하여 MainTabBarController의 탭으로 두 기능을 만듭니다.

메인 컨테이너가 완료되었으니 이제 두 기능이 서로 대화하는 방법을 알아보겠습니다.

# 기능간 통신

기능간의 상호작용을 위해 Navigators에 의존할 것입니다. 예를 들어, 인용구 삭제를 처리하기 위해 QuoteDetailNavigator에 QuoteDetailNavigatorDelegate 프로토콜을 정의합니다:

<div class="content-ad"></div>

- 프로토콜에는 didDeleteQuote(_quote:) 메서드가 있습니다.
- QuoteDetailNavigator를 초기화할 때 델리게이트를 제공합니다. 우리의 경우에는 델리게이트가 QuotesNavigator가 될 것입니다.
- public didDeleteQuote 메서드는 QuoteDetailNavigation에서 나온 것으로, 여기서는 단순히 델리게이트의 메서드를 호출합니다.

Navigator의 메서드를 어떻게 호출하는지 알아보기 위해 QuoteDetailViewModel을 살펴보겠습니다:

- 어떤 ViewModel이든 마찬가지로, 여기에서는 QuoteDetailNavigation 프로토콜을 정의하고 QuoteDetailNavigator가 준수합니다.
- QuoteDetailView에는 버튼이 있고, 해당 버튼을 탭하면 onDeleteTap() 메서드가 호출됩니다. 이 메서드는 적절한 네비게이션 메서드를 호출합니다.

QuoteDetail을 명확하게 한 후에는 이제 Quotes가 변경에 응답하는 방법을 살펴봅시다. QuotesViewModel은 다음과 같습니다:

<div class="content-ad"></div>

- 이니셜 라이저 내부에서는 bindQuoteDeletion() 메서드를 호출하는데, 이는 3단계에서 생성될 것입니다.
- 우리는 QuoteModel을 삭제할 onQuoteDeletion subject를 정의합니다.
- bindQuoteDeletion() 메서드 내부에서는 subject의 요소를 청취하고 로컬에 발행된 quotes 속성을 수정합니다.
- QuotesView에서 리스트 셀을 보여주기 위해 quotes 속성을 사용합니다. 속성이 변경되면 뷰가 다시 그려집니다.

마지막으로, QuotesNavigator가 모든 것을 다루는 방법을 살펴봅시다:

- 우리는 QuoteDetailNavigatorDelegate를 준수하며, didDeleteQuote(_quote:) 메서드를 QuotesNavigator에 제공합니다.
- QuotesViewModel은 약한 종속성으로 정의됩니다.
- QuoteDetail로 이동할 때, QuoteDetailNavigator의 대리인으로서 자신을 전달합니다.
- 마지막으로 didDeleteQuote(_quote:) 메서드에서 뷰 모델의 onQuoteDeleteSubject에 이벤트를 보내어 뷰에서 사용되는 항목 목록을 업데이트합니다.

# 최종 기술

<div class="content-ad"></div>

만약 Xcode 프로젝트를 확인하면 폴더들이 무작위로 나열되어 있습니다. 특정한 순서를 유지하기 위해 project.yml 파일을 수정합니다:

options 키 아래에서 groupOrdering 키를 추가하여 해당 패턴에 대한 특정 순서를 제공할 수 있습니다. 저희는 다음과 같은 순서를 유지하고 싶습니다:

루트 폴더:

- Application
- Presentation
- Data
- Domain

<div class="content-ad"></div>

프레젠테이션 폴더:

- 자산,
- 네비게이터,
- 기능,
- 모델

기능 폴더: 앱에서의 프레젠테이션을 기준으로 기능을 정렬하세요.

xcodegen을 실행한 후, Xcode가 폴더를 올바르게 정렬해 줄 것입니다.

<div class="content-ad"></div>

이전 글에서 프로젝트를 위해 자동으로 생성할 수 있는 의존성 다이어그램을 보여드렸습니다:

![Dependency Diagram](/assets/img/2024-07-14-HowtoModularizeaniOSApp_33.png)

이러한 다이어그램을 생성하기 위해 Homebrew를 통해 설치할 수 있는 GraphViz를 사용합니다:

```js
brew install graphviz
```

<div class="content-ad"></div>

성공적으로 설치한 후에는 project.yml 파일이 있는 루트 폴더로 이동하여 다음 명령을 실행합니다:

```js
xcodegen dump --type graphviz --file Graph.viz
```

이 명령은 Graph.viz 파일을 생성하는데요, 이를 적절한 형식으로 변환할 것입니다. .png 형식으로 생성하려면 이 명령을 실행하면 됩니다:

```js
dot -Tpng Graph.viz -o graph.png
```

<div class="content-ad"></div>

따라서 서로 의존하는 관계들을 나타내는 다이어그램을 얻게 될 것입니다.

# 마무리

우리는 어떻게 XcodeGen을 앱 모듈화에 활용하며 Clean Architecture 접근 방식을 함께 사용할 수 있는지 보았습니다. 해당 아키텍처에서 레이어는 다음과 같이 연결되어 있습니다:

- 애플리케이션 레이어는 프레젠테이션 레이어에 의존하지만 그 역은 성립하지 않습니다.
- 프레젠테이션 레이어는 데이터 레이어에 의존하며, 데이터 레이어는 프레젠테이션 레이어를 인지하고 있지 않습니다.
- 도메인 레이어는 가장 낮은 레이어로 다른 레이어와 독립적이며 데이터 레이어에서 사용됩니다.

<div class="content-ad"></div>

비록 이 기사는 XcodeGen에 초점을 맞췄지만, 앱을 구조화하는 접근법은 사용하는 도구와는 독립적입니다. 다른 대안에 관심이 있다면 아래를 확인해보세요:

- Tuist
- Swift Package Manager. 이 비디오는 도움이 될 수 있습니다.

읽어주셔서 감사합니다!