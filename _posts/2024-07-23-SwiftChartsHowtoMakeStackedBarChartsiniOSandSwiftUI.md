---
title: "iOS와 SwiftUI에서 스택 막대 차트 만드는 방법 - Swift Charts 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_0.png"
date: 2024-07-23 21:41
ogImage: 
  url: /assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_0.png
tag: Tech
originalTitle: "Swift Charts How to Make Stacked Bar Charts in iOS and SwiftUI"
link: "https://medium.com/better-ios-development/swift-charts-how-to-make-stacked-bar-charts-in-ios-and-swiftui-f4cd5b08a081"
---


<img src="/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_0.png" />

만약 일반적인 수직 및 수평 막대 차트만으로는 충분하지 않고 데이터 시각화에 또 다른 차원을 추가하고 싶다면 행운이 따를 겁니다. 스택된 막대 차트를 사용하면 미적 감각이나 사용자 경험을 해치지 않으면서 추가 데이터를 쉽게 표현할 수 있습니다. 오늘은 스택된 막대 차트에 대해 모두 배우게 될 겁니다.

Swift Charts와 관련된 경험이 있으면 더 좋지만, 수직 막대 차트가 Swift 및 SwiftUI에서 어떻게 작동하는지 이미 알고 있다면 이 기사를 이해하는 데 더 수월할 겁니다. 기사를 끝까지 읽으면 회사의 부서별 수익 데이터를 보여주는 스택된 막대 차트가 있는 iOS 앱을 갖게 될 것입니다. 시작해 봅시다!

목차:

<div class="content-ad"></div>

- 쌓인 막대 차트 데이터 모델
- 언제 쌓인 막대 차트를 선택해야 할까요?
- SwiftUI에서 쌓인 막대 차트를 만드는 방법
- 스타일링 101: 데이터 시각화를 새로운 수준으로 끌어올리기
- 보너스: 수직에서 수평 쌓인 막대 차트로 변경하는 방법
- 요약

# 쌓인 막대 차트 데이터 모델

우리는 빈 Xcode 프로젝트에서 시작합니다 - 앱의 유일한 뷰를 표시하는 ContentView.swift 파일만 있습니다. 여기에 어떻게 보이는지 알아보세요:

![이미지](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_1.png)

<div class="content-ad"></div>

실제 앱을 개발한다면 데이터 모델을 별도의 파일로 분리해야 하지만, 편의를 위해 우리는 모든 것을 여기에 작성할 거에요.

스택된 막대 차트는 다른 부서들 사이에서 만들어진 회사의 분기별 수익을 보여줄 거야. 데이터 모델(struct Revenue)은 Identifiable 프로토콜을 준수해야 하므로 쉽게 수익 데이터 컬렉션을 반복할 수 있도록 해야 해. 우리는 id 값을 UUID()의 새 인스턴스로 설정하고 기간, 부서, 수익 금액에 대한 세 가지 다른 속성을 선언할 거에요.

여기가 지금까지 당신의 프로젝트가 어떻게 보이는지에 대한 정보야:

![image](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_2.png)

<div class="content-ad"></div>

데이터 모델이 처리되었으니, 이제 좋은 부분에 몰두해 봅시다.

# 스택된 막대 차트를 선택해야 하는 시기

따라서 스택된 막대 차트는 두 개 이상의 정보를 표시해야 할 때에만 유용합니다. 우리의 경우, 일반 막대 차트는 분기별 수익을 표시할 수 있습니다. 그러나 부서 정보를 포함할 방법이 없습니다.

우리의 데이터 배열은 부서별 분기별 수익을 보여 주므로, 이는 총 3개의 정보 조각이 됩니다. 세 번째 데이터 차원을 일반 수평 또는 수직 막대 차트에 쑤셔 넣을 수 없습니다.

<div class="content-ad"></div>

여기 예시가 있습니다 - 아래 코드 스니펫을 보면 X 축에 시간 기간을, Y 축에 수익 금액을 보여주는 수직 막대 차트를 만드는 방법을 보여줍니다:

다음과 같이 차트를 볼 수 있습니다:

![차트](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_3.png)

BarMark()에 다른 변수를 표시할 수 있는 추가적인 매개변수가 없으므로 우리의 사용 사례에 너무 제한적인 일반적인 수직 막대 차트를 선언할 수는 없습니다.

<div class="content-ad"></div>

아래는 테이블 태그를 Markdown 형식으로 바꾼 예시입니다.

# SwiftUI에서 쌓인 막대 차트를 만드는 방법

추가 데이터를 스택으로 표시하는 방법을 알아봅시다.

스택 구성 요소를 추가하려면 foregroundStyle 수정자를 지정하면 됩니다. by 인수를 사용하는 초기화 함수를 사용하면 스택을 위해 사용될 추가 기능을 지정할 수 있습니다.

다음은 코드 예시입니다:

<div class="content-ad"></div>

아래에 표가 있습니다:

![표](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_4.png)

이렇게 바 섹션을 쌓는 것은 정말 쉽습니다. 그러나 현재 바 섹션 색상이 그렇게 매력적이지는 않죠. 그 다음 섹션에서 이 문제에 대해 다루도록 하겠습니다.

# 스타일링 101: 데이터 시각화를 새로운 수준으로 완성하기

<div class="content-ad"></div>

Swift 및 SwiftUI에서 쌓인 막대 차트를 스타일링하는 것은 처음에 생각한 것보다 더 어려울 수 있습니다. 전체 차트나 개별 막대에 일부 스타일을 적용할 수 없기 때문에 더 많은 작업이 필요합니다. 바 세그먼트와 작업해야 합니다.

바 세그먼트 색상을 변경하는 데 필요한 작업을 살펴보겠습니다.

## 바 세그먼트 색상 변경

가장 먼저 해야 할 일은 색상 배열을 가져와야 합니다. 아래 예시는 아름다운 단색 파란색 팔레트를 사용합니다.

<div class="content-ad"></div>

색상을 변경하려면 전체 Chart 요소에 chartForegroundStyleScale 수정자를 적용해야 합니다. 이 수정자는 도메인 - 독립적인 범주의 비-선택 사항 값 배열 및 range - 색상 배열에 대한 값을 예상합니다.

다음은 생성된 차트의 모습입니다:

![image](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_5.png)

색상은 놀라울 정도로 멋지지만 이제 범례에 문제가 있는 것으로 보입니다. 어떻게 해결하는지 살펴보겠습니다.

<div class="content-ad"></div>

## 사용자 정의 범례: 사용자 정의 색상을 제공할 때 범례 문제 해결하기

가장 좋은 해결책은 범례의 내용을 완전히 제어하는 것입니다.

LegendItem 구조체는 단일 범례 항목을 만드는 데 사용됩니다. 이는 변수 색상 원과 변수 텍스트를 가진 수평 스택일 뿐입니다. 그런 다음 이 구조체는 chartLegend 내에서 세 번 사용됩니다.

간단한 해결책은 아니지만 최종적으로 차트 범례를 완전히 제어할 수 있게 해줍니다. 원 대신 직사각형을 표시하거나 텍스트 크기를 조정하고 싶을 수도 있습니다. 그렇다면, 이제 가능합니다.

<div class="content-ad"></div>

여기 전체 코드 스니펫이에요:

이렇게 해서 범례 문제가 해결되었어요:

![이미지](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_6.png)

다른 조정할 수 있는 사항들을 진행해봐요.

<div class="content-ad"></div>

## 막대의 모서리 반지름 추가하기

막대의 모서리 반지름을 다른 UI 요소와 마찬가지로 변경할 수 있습니다. cornerRadius 수정자를 추가하고 숫자 값을 전달하면 됩니다. 숫자 값이 높을수록 막대가 더 둥글어집니다:

차트가 훨씬 현대적으로 보입니다:

![이미지](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_7.png)

<div class="content-ad"></div>

데이터 시각화를 더욱 발전시키고 싶다면 꼭 주석을 추가해야 합니다. 다음에는 이에 대해 다뤄보겠습니다.

## 막대 세그먼트에 주석 추가하기

스택된 막대 차트에 주석을 추가할 수도 있습니다. 스크립트는 모든 막대 세그먼트에 주석을 자동으로 배치해야 한다고 감지할 수 있습니다.

하지만, 스택된 막대 차트에서 가장 좋은 방법은 주석 텍스트를 막대 세그먼트 안에 넣는 것입니다. 이는 주석 수정자 호출 내부에서 오버레이 위치를 지정하여 수행됩니다:

<div class="content-ad"></div>

표 태그를 Markdown 형식으로 변경해보세요.

<div class="content-ad"></div>

위의 텍스트는 다음과 같이 보입니다:

![image](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_9.png)

적절한 텍스트 각도를 스스로 판단해보세요. 이 글의 나머지 부분은 이 각도로 유지하기로 했습니다. 다음은 차트 크기를 변경하는 방법을 살펴봅시다.

## 차트 크기 변경

<div class="content-ad"></div>

SwiftUI 요소처럼 Chart에 프레임 수정자를 지정할 수 있습니다. 모바일 장치에 가장 적합한 전체 폭 데이터 시각화를 위해 높이에만 신경 쓰면 됩니다.

다음은 코드 조각입니다:

차트가 더 이상 그렇게 키지 않습니다:


![Chart](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_10.png)


<div class="content-ad"></div>

그리고 마지막으로, 누락된 유일한 것을 추가해 봅시다 — 제목입니다.

## 제목 추가

차트 제목은 일반적으로 차트 상단에 표시되며, 왼쪽 가장자리에서 시작합니다. 이미 VStack으로 묶인 차트였으니, 간단히 Text() 요소를 그 위에 추가할 수 있습니다.

이렇게 하면 제목이 중앙에 정렬되지만, 감싸고 있는 VStack의 정렬 속성을 수정하여 위치를 변경할 수 있습니다:

<div class="content-ad"></div>

다음은 우리가 완성한 차트입니다:

![차트](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_11.png)

지금까지 대단한 작업했어요! 추가로 보너스 섹션이 준비되어 있어요.

# 보너스: 수직에서 수평으로 스택된 막대 차트로 전환하기

<div class="content-ad"></div>

상상하겠지만, 쌓인 막대 차트를 수평으로 만드는 데 방해할 누군가는 없습니다. 많은 막대를 표시해야 하고 작은 수평 공간에 막대를 촘촘히 배치하고 싶지 않은 경우가 가장 좋은 방법이지요.

첫 번째 할 일은 BarMark() 호출에서 X와 Y 축의 값을 교체하는 것입니다. 이것이 수직 막대 차트에서 수평 막대 차트로 전환하는 데 도움이 될 겁니다.

그 다음 단계는 주석의 회전 효과를 제거하는 것입니다. 이제 막대 세그먼트가 수평이므로 주석을 표시하는 데 훨씬 더 많은 공간이 마련되어 있으므로 주석을 각도로 표시할 필요가 없아요.

다음은 코드입니다:

<div class="content-ad"></div>

보여드린 표입니다:

![표](/assets/img/2024-07-23-SwiftChartsHowtoMakeStackedBarChartsiniOSandSwiftUI_12.png)

주석을 읽기 쉽게 표시했지만, 막대가 네 개뿐이므로 수평 레이아웃이 필요하지는 않습니다. 어느 쪽이 더 좋은지는 개인 취향에 따라 다를 수 있습니다. 

# 요약

<div class="content-ad"></div>

이 기사를 통해 Swift Charts 라이브러리의 막대 차트 부분을 마무리합니다. 몇 가지 다른 변형이 가능하지만, 그 사용 사례가 상당히 특정하여 전체 기사를 할애하는 가치가 없습니다. 이제 iOS 앱에 포함하고 싶은 대부분의 데이터 시각화를 위해 수평, 수직 및 쌓인 막대 차트를 만드는 방법을 알게 되었습니다.

다음 시간에는 시계열 데이터를 표시할 때 가장 좋은 옵션인 선 그래프에 대해 이야기하겠습니다. 이 주제에 관심이 있다면 계속해서 블로그를 주시하십시오.

Swift Charts에 대한 의견은 어떠신가요? iOS 앱에서 사용하고 계신가요, 아니면 여전히 서드파티 라이브러리를 선호하시나요? 아래 댓글 섹션에서 제게 알려주세요.

이 기사를 좋아하셨나요? 제한 없이 계속 학습하려면 Medium 회원이 되어주십시오. 아래 링크를 사용하시면 회원 자격을 획득하며, 추가 비용은 없습니다. 제게 회원비의 일부가 지급됩니다.

<div class="content-ad"></div>

2022년 11월 13일에 https://betteriosdevelopment.com에서 원래 게시된 내용입니다.