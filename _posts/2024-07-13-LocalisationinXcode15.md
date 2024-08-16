---
title: "Xcode 15에서 로컬라이제이션Localisation 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-LocalisationinXcode15_0.png"
date: 2024-07-13 01:10
ogImage: 
  url: /assets/img/2024-07-13-LocalisationinXcode15_0.png
tag: Tech
originalTitle: "Localisation in Xcode 15"
link: "https://medium.com/better-programming/localisation-in-xcode-15-5be52e97fff0"
isUpdated: true
---





![Screenshot](/assets/img/2024-07-13-LocalisationinXcode15_0.png)

String Catalogs는 WWDC 2023에서 소개된 멋진 새로운 기능이에요. 

.strings 파일 (더 나아가 .stringsdict 파일까지)은 Apple 개발 환경에서 Objective-C 시대의 남은 잔해 중 하나였어요.

하지만 이제 그것도 더 이상은 아니에요.

<div class="content-ad"></div>

안녕하세요, Tarot 전문가입니다. 

나의 신뢰할 수 있는, 즐거운 사이드 프로젝트 Bev을 iOS 17로 업그레이드 중이에요. 그 과정에서 WWDC23의 Discover String Catalogs의 가르침들을 적용 중이에요.

여정에 함께해 주세요!

# The Bad Old World™

Xcode 15 이전에는 대부분의 문자열이 .strings 파일에 정의되어 있었어요.

<div class="content-ad"></div>


// 홈 화면
"home_screen_title"="홈";
"home_screen_button_add_friend"="친구 추가";
"home_screen_button_settings"="설정";
"home_screen_friends_list"="친구 목록";

세미콜론 하나 빠졌네요?

안타깝게도, 모듈의 모든 문자열이 깨져서, 실제 복사본 대신 home_screen_title 또는 home_screen_button_add_friend가 표시됩니다.

.stringsdict 파일은 더 이해하기 어렵습니다. 서로 다른 언어는 복수화를 처리할 때 서로 다른 문법 규칙이 적용되기 때문에, "3명의 친구"와 같이 간단한 내용을 말하기 위해 심도 있는 내용으로 문자열을 정의해야 합니다.


<div class="content-ad"></div>


# 문자열 카탈로그 만들기

Bev를 처음 작성할 때 문자열을 다국화하지 않았습니다.

API가 데이터를 영어로만 반환한다든지 또는 앱을 App Store에서 접근할 수 있는 175개 국가에 출시할 계획이 없었다는 것을 하루 종일 논쟁해도 됩니다.


<div class="content-ad"></div>

그러나 솔직히 말해서, 그냥 서투른 일이었어요.

정말 부끄럽네요.

하지만 긍정적인 면에서, 이것은 우리가 깨끗한 출발점을 갖게 해줍니다.

## 앱에는 어떤 종류의 텍스트가 있나요?

<div class="content-ad"></div>

Bev 앱에는 2개의 화면이 있어요:

- 맥주 목록
- 특정 맥주의 상세 정보.

우리는 국제화할 UI 구성 요소 몇 가지 — 그리고 문자열 몇 가지 종류를 사용할 거예요.

```js
// 단일 문자열
//
NavigationStack {
    beerListView
        .navigationTitle("Bev")
}
Alert(title: Text("Error"), /* ... */ )
Section("이것과 함께 마십시오") { /* ... */ }

// 내부에 변수 텍스트가 있는 문자열
//
Text("처음 양조된 날짜: \(beer.firstBrewed)") // 예: "처음 양조된 날짜: 2020"

// 형식화된 숫자가 있는 문자열
//
Text(String(format: "%.2f", beer.abv) + "% 도수") // 예: "도수 4.05%"

// 복수로 표현된 문자열
//
Text("\(viewModel.beers.count) 종류의 맥주") // 예: "100 종류의 맥주"
```

<div class="content-ad"></div>

## 끈 카탈로그

먼저 해야 할 일을 하겠습니다.

우리 Resources/ 폴더에 새 파일을 생성하고, 앱 타겟에 추가할 새로운 끈 카탈로그 파일 유형을 선택해주세요.

![2024-07-13-LocalisationinXcode15_1.png](/assets/img/2024-07-13-LocalisationinXcode15_1.png)

<div class="content-ad"></div>

우선 String Catalog은 꽤 평범합니다. 왜 그리 크게 중요하다는 걸까요?


![Localisation in Xcode](/assets/img/2024-07-13-LocalisationinXcode15_2.png)


코드를 실행하면 마법이 일어납니다. Xcode가 이 파일을 컴파일 시 자동으로 채워줍니다. 이 기능이 오래된 프로젝트에서는 자동으로 실행되지 않을 수 있으니, Xcode 빌드 설정에서 Use Compiler to Extract Swift Strings를 설정하는지 확인하세요.


![Localisation in Xcode](/assets/img/2024-07-13-LocalisationinXcode15_3.png)


<div class="content-ad"></div>

와우! 🤩

갑자기 내 모든 원시 문자열이 우리의 String Catalog, 즉 Localizable 파일로 흡입되었어요.

## 암시적 LocalizedStringKeys

이 "마법"이 일어나는 이유는, SwiftUI 기본 요소들이 문자열 리터럴을 제공받을 때 자동으로 LocalizedStringKey를 찾기 때문이에요. 이러한 암시적 LocalizedStringKey는 컴파일러에 의해 추출되어 우리의 String Catalog인 Localizable.xcstrings에 채워지게 됩니다.

<div class="content-ad"></div>

# Bev의 국제화

이제 이론을 배우고 초기 String Catalog를 채웠으니, 앱 내의 여러 문자열을 최종적으로 국제화할 수 있습니다.

## 간단한 독립형 문자열

이것들은 가장 간단한 경우입니다. SwiftUI 기본 요소 안에 단순한 텍스트 조각이 있는 경우입니다.

<div class="content-ad"></div>

**Yeast 섹션**

우리의 String Catalog는 이미 작동되고 있으므로 LocalizedStringKey가 암시적으로 생성됩니다:

![LocalisationinXcode15_4](/assets/img/2024-07-13-LocalisationinXcode15_4.png)

하지만, 우리의 문자열 키를 화면과 기능에 따라 구성하는 것이 좋은 방법이므로 Section.FeatureName.Screen.String과 같이 네임스페이스를 지정해줍니다.

<div class="content-ad"></div>

"Feature.Beer.DetailView.Yeast" 섹션을 확인해주세요.

내가 말하고자 하는 것을 언어 카달로그에 추가하는 것은 다음에 번역자가 번역할 때 도움이 돼서 아주 예의 바르다구요. 변수 텍스트가 포함된 문자열입니다. 
Markdown 형식으로 이미지를 추가하겠습니다:

![LocalisationinXcode15_5](/assets/img/2024-07-13-LocalisationinXcode15_5.png)

<div class="content-ad"></div>

많은 경우, 변수나 속성에 기반한 문자열 보간이 포함된 텍스트를 표시하고 싶어질 것입니다.

```js
Text("처음 양조된 맥주는 \(beer.firstBrewed)년 입니다.")
```

번역 된 텍스트라도 다른 언어에서 작동하지 않을 수 있습니다. 많은 언어의 문법에서는 연도를 먼저 표시하고 싶을 수 있기 때문입니다.

String Catalog은 이를 똑똑하게 처리합니다.

<div class="content-ad"></div>

When we refine the implicit LocalizedStringKey, we express it more precisely with an inline argument:

```js
Text("Feature.Beer.DetailView.FirstBrewed \(beer.firstBrewed)")
```

In the String Catalog, the string is created with the interpolated value, using %@ as the placeholder:

![Localisation in Xcode](/assets/img/2024-07-13-LocalisationinXcode15_6.png)

<div class="content-ad"></div>

## 형식화된 숫자를 포함한 문자열

문자열 삽입과 유사하게, 종종 문자열 내부에 숫자를 넣고 싶어할 것입니다.

맥주의 알코올 함유량을 표시하는 것은 이를 흔히 사용하는 사례입니다. 저는 확실히 대부분의 사람들이 우리 앱에서 이것을 합니다.

```swift
Text("\(String(format: "%.2f", beer.abv))% abv")
```

<div class="content-ad"></div>

이번에 우리가 String Catalog에서 가져온 LocalizedStringKey는 조금 혼란스러워요.

![Localized Key Image](/assets/img/2024-07-13-LocalisationinXcode15_7.png)

실제로 문자열의 일부로 이스케이프된 보간 기호 %@와 실제 %를 함께 사용하고 있어서 조금 이상해 보이죠. 이를 이스케이프 처리하는 데 또 다른 % 기호가 필요하다면, 이를 위해서요.

하지만 이 합성된 버전은 문제가 있어요. 왜냐하면 문자열의 십진수 형식을 처리하는 로직을 SwiftUI 뷰 기본 요소에 String(format: "%.2f", beer.abv) 형태로 넘겨주기 때문이에요.

<div class="content-ad"></div>

운 좋게도, String Catalog를 사용하면 이를 적절한 위치에 놓을 수 있습니다. 먼저, 우리의 implicitLocalizedStringKey를 만들어봅시다:

```js
Text("Feature.Beer.DetailView.ABV \(beer.abv)")
```

다음으로, 코드를 작성하고 String Catalog의 출력을 수정할 수 있습니다. 컴파일러는 Beer 구조체의 abv 속성의 타입을 해결하여 이미 Double 형식의 문자열 내삽을 %lf로 표시합니다. 이제 원하는 2소수 자릿수 형식을 String Catalog에서 %.2f로 설정해봅시다.

![Localization in Xcode](/assets/img/2024-07-13-LocalisationinXcode15_8.png)

<div class="content-ad"></div>

## 복수화된 문자열

이게 바로 진짜 중요한 거야.

.stringsdict 파일은 언제나 관리하기 어려운 문제였는데, String Catalog의 킬러 앱은 마침내 이 파일을 처리하는 것이지.

처음에 왜 이런 파일이 존재하는지 확신이 안 서면, 제가 할 수 있는 것보다 이 WWDC 슬라이드가 더 잘 설명해줄 거야:

<div class="content-ad"></div>

Bev를 업그레이드하여 "325종의 맥주 검색"과 같은 검색 바로 변화시켰어요. 이런 작업을 너무 순진하게 처리한다면, 간단하게 이렇게 작성할 수 있을거에요:

```js
.searchable(text: $searchText, 
            placement: .navigationBarDrawer,
            prompt: "Search \(viewModel.beers.count) beers")
```

좀 더 신중하게 접근하고 있지만, .stringsdict 파일이 무엇인지 듣지 못했다면 (저도 부끄럽게도 2022년까지 몰랐었어요), 이렇게 작성할 수 있어요.

<div class="content-ad"></div>


.searchable(text: $searchText, 
            placement: .navigationBarDrawer,
            prompt: "Search \(viewModel.beers.count) beer\(viewModel.beers.count == 1 ? "" : "s")")


이건 영어로 사용할 때 잘 작동하지만 객관적으로 매우 지저분한 (그리고 비효율적인!) 코드야.

처음의 순진한 버전으로 돌아가 보면, 컴파일 시간에 String Catalog이 생성하는 것을 볼 수 있어. 행을 마우스 오른쪽으로 클릭하고 Vary by Plural을 선택할 수 있어.

![LocalisationinXcode15_10](/assets/img/2024-07-13-LocalisationinXcode15_10.png)


<div class="content-ad"></div>

이제 문자열은 우리 언어에 따라 복수화 옵션으로 간단히 분리됩니다. 영어로는 "One"과 "Other"만 있습니다. 다음에 뭘 해야 할 지 제가 알려줄 필요는 없겠죠?

![Image](/assets/img/2024-07-13-LocalisationinXcode15_11.png)

이것은 무서운 사전이나 제가 처음 시작한 소프트웨어에 대한 범죄보다 쉽습니다!

마지막으로, 물론, 문자열 키를 조직 체계에 맞게 업데이트할 수 있습니다:

<div class="content-ad"></div>


.검색가능하다(텍스트: $searchText,
            배치: .navigationBarDrawer,
            프롬프트: "Feature.Beer.ListView.SearchBarPrompt \(viewModel.beers.count)")


## 뷰 모델 내의 문자열

마지막으로, 때로는 뷰에 직접적으로 문자열을 설정하지 않을 때가 있습니다 — 모든 것이 SwiftUI 프리미티브 내부에 있지는 않으며, 특히 ForEach로 래핑된 것은 문자열 리터럴이 아닐 수도 있습니다.

Apple은 우리가 이렇게 하려고 할 때 LocalizedStringResource를 사용하도록 권장합니다 — 단, iOS 16+에서만 사용할 수 있다는 점을 유념하세요.


<div class="content-ad"></div>

여기서는 단순히 뷰 모델에 문자열 리터럴을 사용하여 LocalizedStringResource를 인스턴스화할 수 있습니다:

```js
private let string = LocalizedStringResource("Feature.Beer.List.String")
```

그리고 이 문자열은 다른 키들과 마찬가지로 문자열 카탈로그에 채워집니다!

이 방법이 선호되는 접근 방식인 이유는 LocalizedStringResource 초기화 프로그램이 주석, 테이블 이름 및 문자열에 대한 기본 값을 지원하기 때문입니다.

<div class="content-ad"></div>

# Translating Bev

로컬라이제이션에 대한 기사는 실제 애플리케이션의 로컬라이제이션 없이는 완벽하지 않을까요?

분명히 말씀드리자면, 문자 카탈로그를 통한 로컬라이제이션에 대해 이야기하는데 이를 실제 적용하지 않는다면 그는 국제화(i18n)만을 연습하는 사기꾼일 뿐이죠.

언어에 대해 이야기할 때, 의미론적 측면을 놓치지 말아야 합니다, 블로그 게시물에서도요!

<div class="content-ad"></div>

내 String Catalog의 raw JSON을 활용하여 OpenAI의 친구와 함께, 번역된 앱을 만들어 보려고 합니다.(*조금 부끄러워함*) prompt-engineer 방식으로 진행할 거에요.

결과에 대해서는 이야기할 여지는 없을 거예요.

## 프랑스어

## 스페인어

<div class="content-ad"></div>

## German

# 결론

많은 보일러플레이트를 관리하는 데 따르는 고통이 줄어들길 바랍니다.

그럼 이만큼 하겠습니다!

<div class="content-ad"></div>

어쩌면, 꿈을 꾸어봐도 좋을 것 같아요. 안드로이드에서는 이미 수 년간 로컬라이제이션을 잘 해오고 있어서 우리도 그들에 합류할 수 있을지도요.