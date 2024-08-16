---
title: "2,000개의 Import들 SwiftPM 모듈을 효율적으로 정리하는 방법"
description: ""
coverImage: "/assets/img/2024-07-22-2000ImportsOrganizingMyAppsSwiftPMModules_0.png"
date: 2024-07-22 11:26
ogImage: 
  url: /assets/img/2024-07-22-2000ImportsOrganizingMyAppsSwiftPMModules_0.png
tag: Tech
originalTitle: "2,000 Imports Organizing My Apps SwiftPM Modules"
link: "https://medium.com/better-programming/2-000-imports-organizing-my-apps-swiftpm-modules-f7c68c22ebd2"
---


<img src="/assets/img/2024-07-22-2000ImportsOrganizingMyAppsSwiftPMModules_0.png" />

# 문제

최근 RemafoX의 역사상 가장 큰 기능을 작업하기로 결정했습니다. 시작해야 할 위치를 생각하던 중 1년 미만인 프로젝트에서 70개가 넘는 타겟에 빠져 버리는 것을 발견했습니다. 제 앱을 명확한 코드 분리를 위해 모듈화하고, 빠른 빌드 시간 (= 빠른 SwiftUI 미리보기, 테스트 및 기타)을 위해 Point-Free에서 소개한 바닐라 SwiftPM 방법을 사용하여 개발 중입니다.

앞으로 몇 년 동안 이 앱을 계속 작업할 계획이라면 (현재 온라인에 27가지의 기능이 있지만 이것은 빙산의 일각일 뿐이며, 더 많은 아이디어가 있습니다), 먼저 이 난장판을 정리하기로 결정했습니다. 결국, 기능 간 리팩터링은 코드 베이스를 깔끔하게 유지하고 개발자를 행복하게 합니다! 😇

<div class="content-ad"></div>

Swift Evolution 쓰레드를 읽다가 _@exported_ 어트리뷰트를 발견했다고 기억해요. 제가 관련된 뉴스레터의 이슈를 준비하며 하나를 준비할 때 이 어트리뷔트를 발견했어요. 언더스코어(_)로 시작하는 API를 사용하는 것은 권장되지 않지만 그들의 동작이 바뀔 수도 있거나 완전히 삭제될 수도 있기 때문에, 정리되지 않은 여러 타겟을 정리하는 것이 목적이 었기 때문에 다른 대안이 부족했습니다.

정확히 이러한 이유 때문에, 이 어트리뷰트가 완전히 삭제되는 가능성이 비교적 낮을 것이라고 납득시켰어요. 오히려, 나는 관련된 제안이 언젠가 채택되어 공식 Swift로 들어올 것이라고 믿고 있습니다. 따라서 _@exported_를 그때 무엇으로 명명될지 바꿀 수 있을 것이라고 생각해요. 또한, 이 어트리뷰트를 나의 앱이 이미 많이 의존하고 있는 _The Composable Architecture_ 프레임워크에서 _Point-Free_가 의존하고 있음을 알게 되었어요. 그렇다면 왜 이것에 전력 투자하지 않을까요?

간략히 말하면, 이 어트리뷰트가 도와주는 것은 다음과 같아요: 10개의 피처 모듈과 5개의 헬퍼 모듈이 있다고 상상해봐요. 10개의 피처 중의 각 파일에서는 이 5개의 헬퍼 모듈을 모두(또는 대부분) 임포트하곤 해요. 이런식으로 얻는 결과물은 다음과 같을 거에요:


import Assets
import Analytics
import ComposableArchitecture
import Constants
import Defaults
import HandySwift
import HelpfulErrorUI
import ReusableUI
import SFSafeSymbols
import SwiftUI
import Utility


<div class="content-ad"></div>

그리고 이런 작업을 계속해서 반복하게 됩니다. 괜찮아요, 모든 파일에서 모두 필요한 것은 아니지만, 사실은 Xcode가 모듈 중 하나의 파일만 가져오더라도 전체 모듈을 연결한다는 것이기 때문에 모든 파일에서 모두 가져오는 것이 빌드 시간에 영향을 주지 않을 것입니다(내가 아는 한).

@_exported import을 사용하여 모든 이러한 import를 CoreDependencies와 같은 새로운 대상을 만들어 하나로 합칠 수 있습니다. 그리고 다음 내용과 같이 해당 내용이 있는 새로운 Swift 파일을 만듭니다:

```js
@_exported import Assets
@_exported import Analytics
@_exported import ComposableArchitecture
@_exported import Constants
@_exported import Defaults
@_exported import HandySwift
@_exported import HelpfulErrorUI
@_exported import ReusableUI
@_exported import SFSafeSymbols
@_exported import SwiftUI
@_exported import Utility
```

이제 CoreDependencies를 가져오면 다른 모든 모듈을 가져오게 됩니다!

<div class="content-ad"></div>

하지만 모든 것을 CoreDependencies라는 하나의 그룹으로 묶는 것이 정말 올바른 해결책일까요? 너무 자주 import를 반복해야 하는 문제 외에도, 현재 70개의 모듈을 알파벳 순으로 정렬하고 있는데, 이는 다른 종류의 그룹화나 구조가 없어서입니다. 이러한 그룹화의 부족은 정확한 모듈의 이름을 기억하지 못하더라도 어느 정도 찾고 있는 모듈을 찾기가 어렵게 만들 뿐만 아니라, 가능한 많은 코드를 재사용하려고 기능을 개발하면서 순환 종속성(circular dependencies)이 발생할 수도 있습니다. 코드를 재사용하면서 컴파일러 오류를 방지하기 위해 어디에 어떤 코드를 배치해야 하는지 전략적으로 계획해야 합니다.

# 해결책

문제에 실용적인 해결책을 찾는 가장 좋은 방법은 실제 예제를 살펴보는 것입니다. 그래서, RemafoX에서 실제로 사용하는 몇 가지 모듈을 선택했습니다:

```js
Analytics
Assets
BetterCodable
CommandLineSetup
ComposableArchitecture
Constants
FilesSearch
Foundation
HandySwift
HelpfulErrorUI
MachineTranslation
Paywall
ProjectsBrowser
ReusableUI
SFSafeSymbols
Settings
SwiftUI
Utility
```

<div class="content-ad"></div>

네, 위의 목록에 Foundation과 SwiftUI도 추가했습니다. 왜냐하면 결국에는 다른 종속성들처럼 가져와야 하는 종속성이기 때문이죠. 저는 이들을 내장된 외부 종속성으로 보고 있어요. Swift 파일에서 Foundation을 적어도 가져와야 한다고 익숙하실 수도 있지만 사실 Foundation 없이 Swift 코드를 작성할 수도 있어요. Foundation 없이 기초적인 Swift 기능만 가지고 모든 Swift 표준 라이브러리에 포함된 모든 내용을 포함한 코드를 작성할 수 있어요. 완전히 가능한 일이죠!

그리고 사실 우리가 자주 사용하는 이 두 가지 import는 Apple 내부 기능/도우미의 집합체를 나타내요: Apple은 Foundation과 SwiftUI 또는 UIKit/AppKit을 뒤에 숨겨놓은 많은 기능들을 하나의 그룹으로 묶어 두었어요. 결정적인 요소는 UI를 나타내는 것이나 UI와 직접적으로 관련이 있는 모든 것이 하나의 그룹에 속하고, UI를 나타내지 않거나 UI와 직접적으로 관련이 없는 모든 것이 다른 그룹에 속해 있다는 것 같아요. 그래서 우리가 할 수 있는 가장 자연스러운 일은 그들의 흔적을 따르는 것이겠지요, 심지어 Foundation을 비-UI 그룹에 사용하고(모두에게 SwiftUI와 UIKit에 나타나는 UI를 추가) UI 그룹에 사용하여 이름짓기를 복사할 수도 있어요. 우리 그룹은 앱의 도메인에 구체적이므로 결과적으로 우리 그룹의 이름은 다음과 같을 거에요: AppFoundation과 AppUI.

위의 모듈 목록에 이를 적용해봅시다:


// AppFoundation
Analytics
BetterCodable
CommandLineSetup
Constants
FilesSearch
Foundation
HandySwift
MachineTranslation
Utility

// AppUI
Assets
ComposableArchitecture
HelpfulErrorUI
Paywall
ProjectsBrowser
SFSafeSymbols
ReusableUI
Settings
SwiftUI


<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 바꿔보면 좋을 것 같아요.

<div class="content-ad"></div>


// (전역적으로 유용한) Helpers
Analytics
Assets
BetterCodable
ComposableArchitecture
Constants
Foundation
HandySwift
HelpfulErrorUI
ReusableUI
SFSafeSymbols
SwiftUI
Utility

// (도메인별) 기능
CommandLineSetup
FilesSearch
MachineTranslation
Paywall
ProjectsBrowser
Settings


만약 두 가지 분리 요소를 결합한다면, 4분기가 있는 다음과 같은 그래프가 나올 것입니다. 그래프 사이에는 의존성이 있습니다:

![이미지](/assets/img/2024-07-22-2000ImportsOrganizingMyAppsSwiftPMModules_1.png)

중요한 몇 가지 사항은 다음과 같습니다:

<div class="content-ad"></div>

- ⬆️ 상단 “Feature” 절반은 하단 “Helpers” 절반 위에 구축되어 있습니다. 따라서:
- ↖️ 초록색 “UI 없는 기능” 모듈은 AppFoundation을 가져올 수 있습니다.
- ↗️ 빨간색 “UI 기능” 모듈은 AppFoundation 및 AppUI를 모두 가져올 수 있습니다.
- 그룹 내에서 모듈은 서로 의존할 수 있습니다 (순환을 방지하세요!).
- ➡️ “UI” 모듈은 ⬅️ “UI 없는” 모듈 또는 AppFoundation에 의존할 수 있습니다.
- ⬇️ 하단 “Helpers”는 위의 “Features”에서 가져올 수 없습니다!
- 외부 모듈도 “Features”일 수 있습니다 (↖️ 참조, 현재 ↗️에는 없음)

이 구조를 적용하기 위해 AppFoundation이라는 새 모듈과 해당 콘텐츠가 포함된 새 Swift 파일 (AppFoundation.swift)을 만들었습니다:

```js
// 시스템
@_exported import Foundation

// 내부
@_exported import Analytics
@_exported import Constants
@_exported import Utility

// 외부
@_exported import BetterCodable
@_exported import HandySwift
```

또한 Swift 파일에 다음 내용이 포함된 AppUI 모듈을 만들었습니다:

<div class="content-ad"></div>

```swift
// 시스템
@_exported import SwiftUI

// 내부
@_exported import Assets
@_exported import HelpfulErrorUI
@_exported import ReusableUI

// 외부
@_exported import ComposableArchitecture
@_exported import SFSafeSymbols
```

이제 이 기사 초반의 초기 예제에서 11개의 가져온 것을 UI 기능 모듈 내의 파일에서 가져온 이 2줄로 간단히 대체할 수 있습니다:

```swift
import AppFoundation
import AppUI
```

11개의 가져온 것이 @_exported 속성 덕분에 단지 2개로 줄었습니다.

<div class="content-ad"></div>

아래는 제가 마지막으로 한 작업입니다! 제 제품, 의존성 및 타깃을 Package.swift 파일에서 4분기 기준으로 그룹화했습니다. 이를 위해 모든 섹션에 // MARK: - Non-UI Features와 같은 프라그마 마크를 추가하고 관련 문장을 알파벳 순서대로 넣었습니다. 내 결과물 매니페스트는 이렇게 생겼어요:


```js
- 위의 테이블 태그를 Markdown 형식으로 변경하였습니다. 해당 코드를 보시려면 라이브러리를 사용하실 수 있어요!


```

<div class="content-ad"></div>

모든 관련된 import를 AppFoundation/AppUI로 교체하는 방법을 알려드릴게요:

- 먼저 Xcode의 Find & Replace를 이용하여 @_exported 라이브러리의 모든 import를 AppFoundation으로 대체했어요. 그러면 AppFoundation을 여러 번 import한 여러 파일이 많이 생겼죠.
- 그 다음으로, SwiftLint rule 중 duplicate_imports를 사용했어요. 이 rule은 자동 수정을 지원해요. brew install swiftlint로 설치하고, 다음 3줄 코드를 실행하세요:

```js
echo "only_rules: [duplicate_imports]" > temp_swiftlint.yml
swiftlint lint --config temp_swiftlint.yml --path Sources --autocorrect
rm temp_swiftlint.yml
```

--path에 전달된 매개변수가 Sources가 아닌 경우 수정해주세요.

<div class="content-ad"></div>

3. 마지막으로, AppFoundation의 일부인 모듈 내에 있는 파일들 및 AppFoundation.swift 자체를 사용하여 Git을 사용하여 변경 사항을 되돌렸어요.

4. 그런 다음, AppUI에 대해 위의 단계를 반복했어요. 모든 과정은 10분 이하로 끝났어요!

그게 다에요!
정리하기 전에 프로젝트에 약 2,000개의 import가 있었던 것을 볼 수 있어요:

![이미지](/assets/img/2024-07-22-2000ImportsOrganizingMyAppsSwiftPMModules_2.png)

<div class="content-ad"></div>

그리고 지금은 수입품이 1,200개 밖에 없어요. 이전보다 대략 40%가량 줄었어요!

![이미지](/assets/img/2024-07-22-2000ImportsOrganizingMyAppsSwiftPMModules_3.png)

또한, 제 Package.swift 매니페스트 파일이 상당히 짦아졌어요. 827줄에서 575줄로 줄었어요. 대략 1/3이 줄었다고 할 수 있어요. 그리고 모든 것이 더 구조화되었어요. 기뻐요! 😍

# 결론

<div class="content-ad"></div>

감사해요! @_exported import과 모듈을 "UI 관련" 또는 "UI 비관련", 그리고 더 "전역적으로 유용한" 것 또는 더 "도메인 특정한" 것으로 네 가지 그룹으로 분리함으로써, 이제 "Helper" 모듈을 "Feature" 모듈에 한두 줄만으로 무한하게 가져올 수 있어요! 더불어, 이러한 그룹과 그들의 import 규칙은 회로 의존성을 방지하여 코드를 올바른 모듈에 쉽게 배치할 수 있는 가이드 역할도 해줘요.

결과: 작성할 코드가 줄어들고 빌드 오류 가능성도 줄어들었어요 — 양쪽 다 이득이죠!