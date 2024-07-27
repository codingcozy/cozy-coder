---
title: "Swift에서 Custom Optionals 구현하기 나만의 Optional 타입 만드는 방법"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-13 01:14
ogImage: 
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Custom Optionals in Swift: Implementing Your Own Optional Type"
link: "https://medium.com/@duruldalkanat/custom-optionals-in-swift-implementing-your-own-optional-type-7b44468deca4"
---


스위프트의 Optional 타입은 값의 존재 여부를 명시적으로 처리하여 더 안전한 코드를 작성하도록 도와주는 강력한 기능입니다. 스위프트는 Optionals에 대한 내장 지원을 제공합니다. 하지만 라이브러리나 프레임워크가 옵셔널에 대한 구문적 설탕(?)을 지원하지 않는 경우는 어떻게 해야 할까요?

옵셔널의 핵심 기능을 여전히 구현하는 방법은 무엇인가요?

우리는 사용자 정의 Optional 타입을 구현해야 합니다.

# 기본: 사용자 정의 Optional 타입 정의하기

<div class="content-ad"></div>

먼저, 우리는 enum을 사용하여 사용자 정의 Optional 타입을 정의해야 합니다:

```js
enum CustomOptional<Wrapped> {
    case some(Wrapped)
    case none
}
```

이 enum에는 두 가지 case가 있습니다: some은 wrapped 타입의 연관 값을 포함하고, none은 값이 없음을 나타냅니다. 일반적인 `Wrapped`는 우리의 CustomOptional이 어떤 타입과도 작동할 수 있도록 합니다.

# 값에 액세스하고 언래핑하기

<div class="content-ad"></div>

그 다음으로 값에 액세스하고 언래핑하는 메소드를 추가해 보겠습니다:

```swift
extension CustomOptional {
    // MARK: - Accessing and Unwrapping
    var value: Wrapped? {
        switch self {
        case .some(let wrapped):
            return wrapped
        case .none:
            return nil
        }
    }

    func unwrap() throws -> Wrapped {
        switch self {
        case .some(let wrapped):
            return wrapped
        case .none:
            throw CustomOptionalError.unwrappingNone
        }
    }

    enum CustomOptionalError: Error {
        case unwrappingNone
    }
}
```

`value` 프로퍼티는 래핑된 값을 Swift Optional로 반환하여 기존의 Swift 코드와 상호 작용할 수 있게 합니다. `unwrap()` 메소드는 값을 강제로 언래핑하여 없는 경우에는 오류를 throw합니다.

# Optional Chaining 구현하기

<div class="content-ad"></div>

간단한 Optional Chaining을 지원하기 위해 map와 flatMap의 간소화된 버전을 구현할 수 있습니다.

```swift
extension CustomOptional {
    func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> CustomOptional<U> {
        switch self {
        case .some(let wrapped):
            return .some(try transform(wrapped))
        case .none:
            return .none
        }
    }

    func flatMap<U>(_ transform: (Wrapped) throws -> CustomOptional<U>) rethrows -> CustomOptional<U> {
        switch self {
        case .some(let wrapped):
            return try transform(wrapped)
        case .none:
            return .none
        }
    }
}
```

map과 flatMap: 이 두 메서드는 옵셔널 체이닝의 간소화된 버전을 제공합니다:
- map: 제공된 변환 클로저를 사용하여 포장된 값을 변환합니다.
- flatMap: map과 유사하지만, 다른 CustomOptional을 반환하는 변환에 대해 특별히 설계되었습니다.

<div class="content-ad"></div>

# 우리의 사용자 정의 Optional 사용하기

```swift
let someValue: CustomOptional<Int> = .some(42)
let noValue: CustomOptional<Int> = .none

// 값에 액세스
print(someValue.value) // Optional(42)
print(noValue.value) // nil

// 값 추출
do {
    let unwrapped = try someValue.unwrap()
    print(unwrapped) // 42
} catch {
    print("Error unwrapping: \(error)")
}

// Optional 체이닝
let doubled = someValue.map { $0 * 2 }
print(doubled.value) // Optional(84)

let stringified = someValue.flatMap { value in
    CustomOptional<String>.some(String(value))
}
print(stringified.value) // Optional("42")
```

우리의 CustomOptional 유형을 구현하는 것은 Swift의 Optional이 어떻게 작동하는지에 대한 소중한 통찰력을 제공합니다. 이 구현은 Swift의 기본 Optional의 모든 기능(?? 연산자나 암시적 언래핑과 같은)을 포함하고 있지는 않지만, 핵심 기능을 다루며 Swift에서 대수적 데이터 유형의 강점을 보여줍니다.