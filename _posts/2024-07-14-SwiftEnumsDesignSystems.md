---
title: "디자인 시스템에서 Swift Enum을 활용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-SwiftEnumsDesignSystems_0.png"
date: 2024-07-14 00:22
ogImage:
  url: /assets/img/2024-07-14-SwiftEnumsDesignSystems_0.png
tag: Tech
originalTitle: "Swift Enums 🤝 Design Systems"
link: "https://medium.com/better-programming/swift-enums-design-systems-51d488e26d31"
isUpdated: true
---

만약 대규모 개발 프로젝트에 참여한 적이 있다면, 디자인 시스템을 마주한 적이 있을 것입니다. 디자인 시스템은 브랜딩을 재사용 가능한 구성 요소, 색상, 글꼴, 아이콘 및 모티프로 캡슐화하는 라이브러리로, 제품 전반에 걸쳐 일관된 스타일을 유지하는 데 도움이 됩니다.

간단히 말해, UI 디자인의 각 화면마다 바퀴를 다시 발명하는 것을 피하도록 해줍니다.

개인적으로, 팀원들의 일상을 더 쉽게 만들어주는 일을 좋아합니다. 깔끔하고 직관적인 방법으로 디자인 시스템을 구현하면, 팀원들이 매일 그것과 함께 작업하게 될 것이기 때문에 엄청난 영향력을 가집니다.

나의 경력에서, 앱에 다양한 디자인 시스템을 적용하고 iOS에서 여러 가지 다양한 접근 방식을 탐색해왔습니다.

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

- UIKit 스토리보드와 Nib
- 각 UIKit 하위 클래스에 대해 프로그래밍 방식으로 구성 요소 그리기
- 구성 요소 그리기 및 애니메이션을 가능하게 하는 프로토콜
- 선언적으로 구성 요소를 정의하기 위한 Enum

각 접근 방식에는 그 자리가 있지만, SwiftUI에서 가장 깔끔하고 사용하기 쉬운 접근 방식은 역시 열거형을 사용하여 구성 요소를 정의하는 것이라고 생각합니다.

본 글을 읽는 동안 많은 코드 조각을 추가했지만, 깃허브에서 제 코드를 확인하고 따라해도 좋습니다.

# 디자인 시스템

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

디자이너가 디자인을 보내 주었어요. 그것은 버튼 컴포넌트인 것 같아요. 보통 Figma 무료 계정이나 Sketch 라이선스만큼이나 값어치 있는 디자이너는 작은 스타일 세트와 브랜드 색상 팔레트, 여러 가지 사이즈, 아이콘, 정렬을 가지고 있는 편이에요. Enums가 여기에 강력한 접근법일 수 있다는 것을 깨달을 때, 머리 속에서 바퀴가 돌기 시작할 거예요.

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

# 당신의 버튼을 정의하세요

버튼을 정의하는 것부터 시작해봅시다. SwiftUI 영역에 있으니 View 구조체를 정의하는 것부터 시작할 수 있습니다.

```js
import SwiftUI

public struct MyButton: View {

    private let title: String
    private let action: () -> Void

    public init(title: String,
                action: @escaping () -> Void) {
        self.title = title
        self.action = action
    }

    public var body: some View {
        Button(action: action, label: {
            // 여기에 버튼 UI 추가
        })
    }
}
```

여기서 간단히 MyButton이라고 이름 지었지만 일반적으로는 회사 이름을 접두사로 붙입니다. 저는 DeloitteButton, CarbnButton 및 Gener8Button을 만들어 왔어요. 개인 프로젝트라면 [당신의 이름]Button으로 이름 붙이셔도 괜찮습니다!

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

시작하려면, 우리는 간단히 SwiftUI 버튼을 기본값으로 초기화하는 방법과 동일한 방식으로 버튼을 초기화합니다. 제목과 탭 액션으로 시작합니다.

# 당신의 첫번째 Enum

이제 기본 사항이 갖춰졌으므로, 흥미로운 내용을 만들어볼 수 있게 되었습니다.

디자인 시스템의 버튼의 가장 간단한 기능부터 시작하여 복잡성과 사용자 정의의 수준을 차례로 내려갈 수 있습니다. 그래서 처음으로 색상을 다루기로 합시다.

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

위의 디자인 시스템에는 3가지 주요 색상이 있어요:

- 기본 버튼 — 메인 브랜드 색상으로 파란색을 사용해요.
- 강조 버튼 — 보조 브랜드 색상인 연한 녹색을 사용해요.
- 오류 버튼 — 연한 빨강색이에요.

우리는 MyButtonColor 열거형을 사용하여 이를 깔끔하게 처리할 수 있어요:

```js
public struct MyButton: View {

    public enum MyButtonColor {

        case `default`
        case accent
        case error

        var mainColor: Color {
            switch self {
            case .`default`: return .blue
            case .accent: return .green.opacity(0.85)
            case .error: return .red.opacity(0.6)
            }
        }

        var detailColor: Color {
            switch self {
            case .`default`: return .white
            case .accent: return .white
            case .error: return .white
            }
              }
        }
```

그랬더니 정리되어 있고 귀엽지 않아요? 요렇게 하면 컴포넌트 색상을 쉽게 관리할 수 있답니다! 🌟

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

여기서는 Swift의 열거형의 놀라운 힘을 활용하고 있어요. mainColor 및 detailColor와 같은 계산된 속성을 enum에 추가할 때, case switches가 self를 넘기고 자체를 위해 정의된 값을 찾아요!

간단한 설명: default가 Swift 키워드이기 때문에, enum case 중 하나를 default로 지정하려면 back-ticks로 '이스케이프'해야 해요. 그렇지 않으면 컴파일러가 혼란스러워 할 거에요.

이제 MyButton의 body 속성을 수정하여 색 속성이 그려질 때 이러한 속성을 포함할 수 있어요:

```swift
public var body: some View {
    Button(action: action, label: {
        buttonWithColor
    })
}

private var buttonWithColor: some View {
    Text(title)
        .foregroundColor(type.detailColor)
        .background(
            Capsule()
                .fill(type.mainColor)
        )
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

이제 드디어 우리는 MyButton의 이니셜라이저를 수정하여 이 유형을 인수로 노출시킬 수 있습니다. .default 버튼 색상이 가장 일반적으로 사용되기 때문에, 이 초기화 프로그램에서 color에 대한 기본 인수로 사용해야 합니다.

```js
    private let color: MyButtonColor
    private let title: String
    private let action: () -> Void

    public init(color: MyButtonColor = .default,
                title: String,
                action: @escaping () -> Void) {

        self.color = color
        self.title = title
        self.action = action
    }
```

이제 팀원들은 MyButton을 기본 SwiftUI Button과 동일한 방식으로 사용하고, .default(파란색) 버튼을 그릴 수 있습니다. 또한 .accent(초록색) 또는 .error(빨간색) 변형에 대한 기본 색상을 무시하고 싶을 때 보다 복잡한 버전의 이니셜라이저를 사용할 수 있습니다.

# 이미지 추가하기

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

애플에 따르면, 점진적으로 공개되는 API 디자인의 중요한 측면은 나열할 수 있는 것보다 조합 가능성입니다. 이것은 말 그대로 "열거형을 사용하지 말라"는 것을 의미하는 것이 아니라, API 소비자들이 가능한 한 맞춤화할 수 있도록 자유를 부여해야 한다는 것을 의미합니다.

이 경우에는 새로운 열거형인 MyButtonIcon을 만들 것이며, enum 연관 값의 이점을 활용하여 원하는 Image를 버튼에 추가할 수 있도록 할 것입니다!

```js
public enum MyButtonIcon {
  case leading(_ icon: Image)
  case trailing(_ icon: Image)
}

private let color: MyButtonColor
private let icon: MyButtonIcon?
private let title: String
private let action: () -> Void

public init(color: MyButtonColor = .`default`,
            icon: MyButtonIcon? = nil,
            title: String,
            action: @escaping () -> Void) {

  self.color = color
  self.icon = icon
  self.title = title
  self.action = action
```

기본적으로 버튼에 아이콘을 추가하고 싶지 않습니다. 따라서 MyButtonIcon 속성을 옵셔널로 만들고 기본 값을 nil로 설정해야 합니다.

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

이제 우리는 MyButton의 본문에 이를 설정할 수 있습니다:

```js
    public var body: some View {
        Button(action: action, label: {
            buttonForColor
        })
    }

    private var buttonForColor: some View {
        buttonContent
            .foregroundColor(color.detailColor)
            .background(
                Capsule()
                    .fill(color.mainColor)
            )
    }

    private var buttonContent: some View {
        HStack(spacing: 16) {
            if case .leading(let image) = icon {
                iconView(for: image)
            }

            Text(title)

            if case .trailing(let image) = icon {
                iconView(for: image)
            }
        }
    }

    private func iconView(for image: Image) -> some View {
        image
            .renderingMode(.template)
            .resizable()
            .aspectRatio(contentMode: .fit)
            .frame(width: 20, height: 20)
    }
```

여기서, buttonForColor에 새로운 buttonContent 하위 뷰를 만들었습니다. 이 buttonContent에는 SwiftUI HStack이 포함되어 있습니다. 아이콘의 enum case에 따라, 원래 버튼 텍스트의 leading 또는 trailing 측면 중 선택적으로 값을 가져옵니다.

선택적인 패턴 매칭을 사용하여 이러한 이미지의 연관 값을 enum에서 가져와서 뷰의 위치에 배치할 수 있습니다.

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

새로운 iconView(for image: Image) 메서드에 전달하여 기본 SwiftUI 이미지 사용자 정의를 추가합니다. renderingMode를 .template으로 설정하여 MyButtonColor의 detailColor로 색상이 설정됩니다.

# 나머지 부분

![image](/assets/img/2024-07-14-SwiftEnumsDesignSystems_1.png)

사이즈와 스타일을 처리하는 더 많은 열거형을 구현한 후, 다음과 같이 보이는 이니셜 라이저가 남습니다:

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
    public init(type: MyButtonType = .primary,
                color: MyButtonColor = .default,
                size: MyButtonSize = .large,
                icon: MyButtonIcon? = nil,
                title: String,
                action: @escaping () -> Void) {

        self.type = type
        self.color = color
        self.size = size
        self.icon = icon
        self.title = title
        self.action = action
    }
```

Here in our design, we are following the concept of progressive disclosure:

- The simplicity at the point of use allows developers to interact with it like a regular SwiftUI Button, focusing on essential information like button title and action while offering flexibility for customization.
- By leveraging Xcode autocomplete with our enum cases, even newcomers to your project can easily grasp which properties to adjust for design consistency.
- Our initializer includes smart default values, so creating the most standard button (large, blue, filled, no icon) is straightforward and requires minimal effort.
- Users of our API can add their desired icon to the button by incorporating an associated value into the icon enum.

## Let's witness this approach in action.

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

When we use the button in our Views, we can simply call it without any customization:

```js
MyButton(title: "Press me!", action: { didPressButton() })
```

![Button Image](/assets/img/2024-07-14-SwiftEnumsDesignSystems_2.png)

Alternatively, we can leverage all the features we've implemented for a more personalized button:

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

오늘은 MyButton에 대한 전체 샘플 코드가 포함된 Github를 확인해보세요. 디자인 라이브러리의 모든 가능한 변형 사례를 보여주는 미니 프로젝트도 함께 있습니다.

# 결론

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

MyButton을 개발함으로써, 유용한 UI 구성 요소를 만들 뿐만 아니라 Swift 유형 시스템의 강력한 기능 중 많은 부분을 이해할 수 있었습니다:

- 특정 맞춤 옵션을 정의하기 위해 Enum 사용
- case 내부에서 계산된 속성이 있는 Enum 정의, case를 switch
- Enum case 내부의 연관 값은 완전한 맞춤을 가능하게 함
- MyButton 초기화기의 기본 매개변수는 점진적으로 정보를 공개함
- 선택적 속성을 사용하여 필요없는 기능을 회피할 수 있음

이것을 읽은 후, SwiftUI에서 디자인 시스템을 구현하기 위해 수많은 Enum을 활용하는 것에 대해 고려해보기를 바랍니다.
