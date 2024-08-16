---
title: "Swift에 첫 번째 패키지 명령 플러그인 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-24-CreateYourFirstSwiftPackageCommandPlugin_0.png"
date: 2024-07-24 11:55
ogImage: 
  url: /assets/img/2024-07-24-CreateYourFirstSwiftPackageCommandPlugin_0.png
tag: Tech
originalTitle: "Create Your First Swift Package Command Plugin"
link: "https://medium.com/better-programming/create-your-first-swift-package-command-plugin-3f918e2e8b8e"
---


올해 애플은 Swift Package Manager용 새로운 기능을 출시했습니다: Swift Package 플러그인. 이제 Xcode와 자동으로 통합되는 두 가지 종류의 플러그인을 작성할 수 있습니다:

- 빌드(및 미리 빌드) 플러그인.
- Command 플러그인.

이미 몇 개의 기사에서 빌드 플러그인에 대해 이야기했습니다: "첫 번째 Swift Package 빌드 플러그인 구현"과 "iOS 앱에서 Xcode 플러그인 사용 방법".

<div class="content-ad"></div>

오늘은 Command 플러그인을 만드는 데 필요한 단계를 공유하고 싶어요. 이러한 플러그인을 개발하는 경험은 좋지 않기 때문에, 디버그하는 기술도 함께 공유하고 싶어요.

# 플러그인

이 글에서는 JSON 명세서에서 Swift 코드를 생성하는 Command Plugin을 만들 거예요.

Command 플러그인을 만들려면 다양한 부품들이 필요해요:

<div class="content-ad"></div>

- Plugin의 구조를 정의하는 Package.swift 파일.
- 적절한 폴더 구조.
- CommandPlugin 프로토콜을 준수하는 Swift struct.
- 플러그인을 구현하기 위한 비즈니스 로직.
- 선택적으로, 플러그인을 테스트하기 위한 추가 Package.

## Package.swift

Swift Package 플러그인은 Swift Packages로 정의됩니다. 플러그인을 위한 새 패키지를 만들어야 합니다.

Command 플러그인을 위한 전형적인 패키지 구조는 다음과 같습니다:

<div class="content-ad"></div>

중요한 사항은 다음과 같습니다:

- 1번 라인의 swift-tool-version은 최소 5.7 이상이어야 합니다.
- 대상은 .plugin 유형이며 .command 기능을 갖습니다.

명령 플러그인을 완전히 정의하려면 몇 가지 속성을 더 추가해야 합니다: 의도(intent)와 권한(permission) 집합입니다.

의도는 플러그인이 존재하는 이유입니다. 동사와 설명으로 구성됩니다.

<div class="content-ad"></div>

- 커맨드 플러그인은 Swift 패키지 커맨드 라인 도구를 통해서도 호출될 수 있어요. 동사 속성은 이 커맨드 플러그인을 호출할 때 커맨드 라인에서 사용할 수 있는 인자입니다. 구문은 아래와 같아요:

```js
swift package plugin <verb> [args...]
```

- 설명 속성은 플러그인에 대한 사람이 읽을 수 있는 설명이에요.

권한 집합은 Package API에서 정의된 enum에서 옵니다. 이 enum에는 명시적인 케이스가 없지만 하나의 정적 함수인 writeToPackageDirectory를 제공해요. 이 함수는 Xcode에게 플러그인이 쓰기 액세스가 필요하다고 알려주며, IDE가 명령을 호출할 때 사용자에게 메시지를 표시해요. 이 프롬프트는 이 권한으로 플러그인이 어떤 작업을 수행할지 사용자에게 설명하는 사람이 읽을 수 있는 설명인 이유 문자열을 보여줄 거에요.

<div class="content-ad"></div>

## 폴더 구조

모든 Swift 패키지와 마찬가지로 플러그인은 적절한 폴더 구조를 준수해야 올바르게 빌드됩니다. Package.swift에서 path 속성을 사용하여 구조를 사용자 정의할 수 있지만, 기본 폴더 구조는 다음과 같습니다.


CodeGeneratorPlugin
├── Package.swift
└── Plugins
    └── CodeGenerator
        └── CodeGenerator.swift


CodeGeneratorPlugin은 현재 패키지를 포함하는 폴더입니다. Plugins 폴더는 Package.swift에서 정의된 모든 플러그인의 홈입니다. 각 플러그인의 코드는 플러그인과 동일한 이름을 가진 폴더에 있어야 합니다. 이 예시에서는 CodeGenerator 폴더입니다.

<div class="content-ad"></div>

CodeGenerator.swift 파일은 플러그인의 진입점이며 비즈니스 로직을 포함할 것입니다. 다른 폴더들과 달리 CodeGenerator로 명명할 필요는 없습니다. Swift 파일은 아무 이름이나 가질 수 있습니다.

## CodeGenerator.swift

이 구조체는 플러그인의 진입점입니다. 기본 구조는 다음과 같습니다:

중요한 조각들은:

<div class="content-ad"></div>

- PackagePlugin 문을 가져옵니다. 이 문은 새 API를 가진 framework를 가져옵니다.
- @main 어노테이션. 이는 플러그인의 진입점을 정의합니다.
- CommandPlugin을 준수합니다. 이는 해당 구조체를 올바른 Command 플러그인으로 표시하고 메서드를 구현하도록 강제합니다.
- performCommand 메서드. 이 메서드에는 플러그인의 논리가 포함되어 있습니다.

performCommand에는 두 개의 매개변수가 있습니다: context와 arguments입니다. context는 패키지에서 정보를 읽는 데 사용될 수 있습니다. arguments는 Xcode나 명령줄에서 명령에 전달할 수 있는 인수 목록입니다.

## 코드 생성 논리

이것은 플러그인을 만들기 위한 마지막 단계입니다. JSON 명세에서 시작하는 Swift 코드를 생성하는 코드를 작성해야 합니다. 이를 위해 일부 도우미 클래스와 몇 가지 함수가 필요할 수 있습니다.

<div class="content-ad"></div>

JSON 사양

먼저 JSON 엔티티를 나타내는 데이터를 정의해야 합니다. 이 예제에서는 몇 개의 Swift 구조체를 생성하려고 합니다. 이러한 데이터 구조체는 매우 간단합니다. 단지 let 속성만을 갖습니다.

파싱하려는 JSON은 다음과 같은 구조를 가지고 있습니다:

```js
{
  "fields": [{
    "label": "<variable name>",
    "type": "<variable type>",
    ["subtype": "<variable type>"]
  }]
}
```

<div class="content-ad"></div>

이 JSON 객체는 하나의 구조체를 나타냅니다. fields 속성은 완전한 Swift 프로퍼티를 정의하는 다른 객체를 포함합니다. label은 구조체 내의 프로퍼티 이름이고, type은 프로퍼티의 주요 유형입니다. 제네릭의 경우, 제네릭 유형을 지정하기 위해 subtype이 필요합니다.

구조체의 이름은 파일의 이름이 될 것입니다. 따라서 유효한 JSON은 다음과 같은 Person.json 파일일 것입니다:

```json
{
  "fields": [
    {
      "label": "name",
      "type": "String"
    },
    {
      "label": "surname",
      "type": "String"
    },
    {
      "label": "age",
      "type": "Int"
    },
    {
      "label": "family",
      "type": "Array",
      "subtype": "Person"
    }
  ]
}
```

이 Person 타입에는 이름, 성, 나이 및 다른 Person 타입의 목록 인 family가 있습니다. 플러그인 실행 후 다음과 같은 Swift 구조체를 얻을 것으로 예상됩니다:

<div class="content-ad"></div>

```json
struct Person {
  let name: String,
  let surname: String,
  let age: Int
  let family: [Person]
} 
```

**데이터 모델**

플러그인 로직에서 이 JSON을 올바르게 처리하려면 디코딩할 수 있도록 데이터 모델을 적절하게 구성해야 합니다.

이를 달성하기 위해 다음 두 구조체가 필요합니다:

<div class="content-ad"></div>

첫 번째 구조체는 필드 목록을 포함하는 래퍼입니다. 이는 최상위 JSON 객체를 나타냅니다.

필드 구조체는 내부 객체를 정의하는 데이터 모델입니다. 레이블을 위한 속성, 유형을 위한 속성, 그리고 경우에 따라 일반적으로 다루어야 할 하위 유형을 위한 선택적 속성이 있습니다.

로직

마지막으로 로직을 구현할 수 있습니다. 플러그인 내에서 여러 함수로 분할하여 간단하게 만들 수 있습니다.

<div class="content-ad"></div>

첫 번째 함수는 performCommand이며, 플러그인의 진입점입니다.

이를 플러그인의 구성 루트로 볼 수 있습니다: 문맥에서 모든 관련 데이터를 가져와 의존성을 인스턴스화하고, 이를 나머지 코드에 전달할 수 있습니다.

performCommand는 executeCommand를 호출합니다.

이 메서드는 drillDown 메서드를 사용하여 생성해야 하는 모든 구조체를 추출합니다. 구조체가 없는 경우 반환됩니다.

<div class="content-ad"></div>

그럼, 'Struct.swift'라는 파일에 구조체를 쓸 겁니다. 모든 구조체는 간편함을 위해 하나의 파일에 포함될 겁니다.

'drillDown' 메서드는 패키지의 폴더 구조를 크롤하여 JSON 스펙을 재귀적으로 찾는 책임이 있어요.

이 예제는 모든 JSON 스펙이 'Definitions'라는 폴더에 위치한다고 가정하고 작동합니다.

'drillDown' 메서드는 먼저 디렉터리 속성의 내용을 가져옵니다. 이 속성은 기본적으로 패키지의 주 폴더입니다. 그런 다음, 디렉터리의 마지막 경로 구성 요소가 'Definitions'인 경우, 해당 폴더에 포함된 각 항목의 전체 경로를 검색하고 각 항목에 대해 'createSwiftStruct' 함수를 호출합니다.

<div class="content-ad"></div>

그렇지 않으면, 트리를 탐색합니다. 현재 폴더의 각 항목에 대해 해당 항목이 폴더인지 아닌지 확인합니다. 폴더인 경우 해당 폴더로 진입하려고 시도하고 결과를 변수에 누적하여 재귀의 끝에 반환됩니다.

마지막 방법은 createSwiftStructure입니다.

이 방법은 매개변수로 전달된 파일의 내용을 읽습니다. 그런 다음 위에서 정의한 Data 모델을 사용하여 디코딩하려고 시도합니다.

그런 다음 파일 이름에서 구조체 이름을 추출하고 필드 목록을 생성합니다.

<div class="content-ad"></div>

드디어 유효한 Swift 구조체인 문자열을 반환합니다.

## 사용 방법

이제 다른 패키지에서 명령 플러그인을 시험해 볼 차례입니다.

먼저 Package.swift에서 새 패키지를 생성하세요. 이를 위해 Package.swift 파일에 .target를 추가하면 됩니다:

<div class="content-ad"></div>

해당 패키지는 적절한 폴더 구조도 필요합니다. 아래와 같은 구조여야 합니다:

```js
CodeGeneratorPlugin
├── Package.swift
└── Plugins
    └── CodeGenerator
        └── CodeGenerator.swift
└── Sources
    └── MyCode
        └── HelloWorld.swift
```

HelloWorld.swift은 빈 Swift 파일일 뿐입니다. 모든 Swift 패키지는 해당 폴더에 적어도 하나의 Swift 파일을 가져야 합니다.

이 시점에서 CodeGeneratorPlugin 프로젝트를 마우스 오른쪽 버튼으로 클릭하면, 이미 Xcode가 컨텍스트 메뉴에 CodeGenerator 사용자 정의 플러그인이 나타나는 것을 확인할 수 있습니다!

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-24-CreateYourFirstSwiftPackageCommandPlugin_1.png)

다음 단계는 위에서 설명한 thePerson.json 파일이 있는 Definitions 폴더를 추가하는 것입니다.

작업이 끝나면, Xcode에서 콘텍스트 메뉴의 CodeGenerator 메뉴 항목을 클릭하면 대화 상자가 표시되어 다음을 할 수 있습니다:

- 플러그인을 실행할 대상을 선택합니다.
- 필요한 경우 추가 인수를 전달합니다.


<div class="content-ad"></div>

아래와 같이 테이블 태그를 Markdown 형식으로 변경해주세요.


![CreateYourFirstSwiftPackageCommandPlugin_2](/assets/img/2024-07-24-CreateYourFirstSwiftPackageCommandPlugin_2.png)

이 경우에는 추가 인수가 필요하지 않으며 안전하게 실행할 수 있습니다.

이제 Xcode에서 명령을 실행할 수 있는지 묻습니다.

![CreateYourFirstSwiftPackageCommandPlugin_3](/assets/img/2024-07-24-CreateYourFirstSwiftPackageCommandPlugin_3.png)


<div class="content-ad"></div>

이 대화의 작성자로부터의 라인은 이 기사의 첫 번째 단계에서 Package.swift에 설정한 이 플러그인을 위한 이유를 보여줍니다.

파일 변경 명령 허용을 클릭하면 Xcode가 명령을 실행할 것입니다. 몇 초 후에 HelloWorld.swift 아래에 Structs.swift 파일이 나타날 것입니다.

새 파일에는 다음 내용이 포함되어 있어야 합니다:

축하합니다! 첫 번째 명령 플러그인을 만들고 Xcode를 사용하여 다른 대상에 적용했습니다.

<div class="content-ad"></div>

# 플러그인 디버깅

아쉽게도 조금의 시행착오 없이 완벽한 코드를 작성하는 사람은 없습니다. 이 플러그인을 개발하는 동안 자주 실행하여 작동 여부를 확인했지만, 개발자 경험은 매우 좋지 않았어요:

- 처음에 플러그인이 작동하지 않아서 어떤 출력물도 생성되지 않았습니다.
- Xcode에서 사용 가능한 에러가 없었어요.
- 디버거를 연결하여 무엇이 발생하고 있는지 확인할 수 없었습니다.
- Swift의 print 함수가 아무 곳에도 아무것도 쓰고 있지 않았습니다.

이 플러그인을 디버깅하는 내 해결책은 각 단계를 로그 파일에 기록하는 것이었어요. 그것을 달성하기 위해 다음 단계를 따랐습니다:

<div class="content-ad"></div>

- 전역 변수 `log`를 생성했어요. 이 변수는 [String] 형식이에요. 이 명령이 실행될 때마다 다시 생성되므로 프로세스 간 메모리 공유 문제가 없어요.
- `log(_ message: String)` 함수를 생성해서 메시지를 `log` 변수에 추가했어요.
- `printLog()` 함수를 만들어서 모든 로그를 결합하고 실행 후 검토할 수 있는 로그 파일에 기록했어요.
- 마지막으로, 코드에 `log(_:)` 함수를 호출하여 무슨 일이 일어나고 있는지 볼 수 있도록 설정했어요.

이 방법으로 다양한 오류를 로깅하고 플러그인을 성공적으로 구현할 수 있었어요.

# 결론

오늘의 글에서는 Swift 5.7용 커맨드 플러그인을 구성하는 방법을 배웠어요.

<div class="content-ad"></div>

패키지의 구조와 구현하는 기본 개념을 배웠습니다. 또한 Xcode에서 실행하는 방법도 배웠어요. 개발 경험이 부족했지만 다양한 실행 단계를 볼 수 있는 기본 로거 솔루션을 만드는 방법도 배웠습니다.

명령 플러그인은 매우 유용하지만 명령 줄에서 사용할 때 훨씬 더 유용할 것입니다. 예를 들어 사용자 정의 명령을 지속적 통합 환경에 통합할 수 있게 될 거예요.

이러한 새로운 강력한 도구로 커뮤니티가 무엇을 만들어낼지 기대가 되네요!