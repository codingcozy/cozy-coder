---
title: "API 사용 중단 제대로 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-23-HowToDeprecateAPIstheRightWay_0.png"
date: 2024-07-23 21:19
ogImage: 
  url: /assets/img/2024-07-23-HowToDeprecateAPIstheRightWay_0.png
tag: Tech
originalTitle: "How To Deprecate APIs the Right Way"
link: "https://medium.com/better-programming/how-to-deprecate-apis-the-right-way-371c1cbf1723"
---



![이미지](/assets/img/2024-07-23-HowToDeprecateAPIstheRightWay_0.png)

소프트웨어 개발에서 유일한 확실한 것은 요구 사항이 변한다는 것입니다.

이런 일이 몇 번이나 일어났나요? 프로젝트를 시작하고 요구 사항을 충족하는 앱을 개발하여 릴리스하고 모두가 만족해합니다. 제품 매니저가 돌아와서 새로운 기능을 추가해 달라고 요청합니다. 갑자기 추가적인 매개변수를 전달하거나 속성의 이름을 변경해야 합니다. 변경을 수행하면 앱의 다른 부분도 오류가 발생하기 시작하며, 때로는 다른 팀이 처리한 부분도 포함됩니다.

라이브러리, 프레임워크 또는 앱의 모듈 중 하나를 작업하고 있다면 가능한 모든 사용자를 고려해야 합니다. 파괴적인 변경을 도입하면 사용자가 코드를 조정할 수 있는 시간과 도구를 제공해야 합니다.


<div class="content-ad"></div>

오늘은 무엇이 파괴적인 변경인지, 어떤 변경이 파괴적으로 간주될 수 있는지, 그리고 해당 변경 사항을 안전하게 전파하는 데 사용할 수 있는 기술에 대해 탐색하고 싶어요.

# 파괴적인 변경이란 무엇인가요?

파괴적인 변경은 코드 내에서 시스템이 계속 작동하려면 다른 부분의 코드를 변경해야 하는 변경입니다.

가장 간단한 예는 공개 API의 속성 이름을 변경할 때입니다: 해당 속성을 사용하는 모든 다른 구성 요소는 새로운 이름으로 업데이트해야 합니다.

<div class="content-ad"></div>

다양한 수준에서 중요한 변경 사항이 발생할 수 있습니다:

- 코드 변경 사항: 일부 공개 또는 보호된 API의 시그니처가 변경됩니다. 이러한 경우 컴파일러에서 오류를 일으킵니다.
- 동작 변경 사항: 코드 동작 방식에 변경 사항이 있습니다. 이러한 경우 코드가 오류 없이 계속 빌드되지만 최종 결과물이 이전 버전과 다를 수 있습니다. 예를 들어, Apple이 Swift 버전을 다른 것으로 바꾼 경우, Dictionary.key 속성에서 반환되는 키의 순서가 변경되었습니다. 이는 이전 시스템 버전으로 참조 테스트를 얻었을 때 일부 스냅샷 테스트가 실패하게 만들 수 있습니다.
- 빌드 시스템 변경 사항: 파일이 작성되는 방식이나 컴파일러에 전달해야 하는 플래그에 대한 변경 사항이 있습니다. 한 버전에서 다른 버전으로 앱을 구성해야 하는 방식이 변경되면, 이전 앱 버전은 새로운 빌드 시스템 버전에서 빌드되지 않을 수 있습니다.

본 문서에서는 첫 번째 유형의 변경인 코드 변경 사항에 초점을 맞추고자 합니다. 변경 사항을 도입할 때마다 주의 깊게 고려하고 적절히 대응해야 합니다.

# Breaking Change가 아닌 것은 무엇인가요?

<div class="content-ad"></div>

일부 코드 변경은 파괴적인 것이 아닙니다.

API를 변경하지 않고 관련 동작을 수정하는 버그 수정은 파괴적이지 않습니다.

추가적인 변경(예: 클래스에 새로운 함수나 속성 추가)은 파괴적이지 않습니다: 새로운 기능이 있는 모듈 버전을 사용하는 시스템은 여전히 예상대로 작동합니다. 단지 새로운 기능을 사용하지 않을 뿐입니다.

함수에 기본값이 있는 매개변수를 추가하는 것은 파괴적인 변경처럼 보일 수 있습니다. 그러나 기본 매개변수는 함수가 사용되는 호출 위치가 변경되지 않도록 보장합니다. 따라서 이 변경은 파괴적이지 않습니다.

<div class="content-ad"></div>

예를 들어, 다음과 같은 add 함수를 고려해보세요:

```js
func add(a: Int, b: Int) -> Int {
  return a + b
}
```

다양한 진법을 지원해야 한다는 요구사항이 들어오면, 다음과 같이 수정할 수 있습니다:

```js
enum Base {
  case decimal
  case octal
  
  func convert(value: Int) -> Int {
    switch self {
    case .decimal:
      return Int(String(value, radix: 10))!
    case .octal:
      return Int(String(value, radix: 8))!
    }
  }
}

func add(a: Int, b: Int, base: Base = .decimal) -> Int {
  let result = a + b
  return base.convert(value: result)
}
```

<div class="content-ad"></div>

여기서는 지원되는 기본값을 정의하는 enum을 만들고, 숫자를 한 베이스에서 다른 베이스로 변환하는 convert 메소드를 구현할 수 있습니다.

기본값으로 .decimal을 전달함으로써, base 매개변수 없이 사용되는 이전 버전의 add에 대한 사용자들은 문제없이 사용할 수 있습니다.

private 또는 fileprivate 키워드에 대한 변경 사항은 파괴적인 변경으로 이어지지 않습니다. 해당 키워드로 보호되는 메소드와 프로퍼티들을 외부에서 간섭할 수 없기 때문에, 그 안에서 발생하는 변경 사항은 보통 안전합니다.

내부 프로퍼티와 메소드에 대한 변경 사항은 조금 더 까다롭습니다. 라이브러리, 프레임워크 또는 제대로 모듈화된 앱의 별도 패키지에서 작업하고 있다면, 내부 인터페이스의 변경은 안전합니다. 외부에서 해당 메소드와 프로퍼티를 사용할 수 없기 때문입니다. 그러나 비공식 모듈(즉, 모듈이 아닌 폴더 기반 구조로 되어 있는 앱)에서 작업 중인 경우, 내부인 것을 변경한다면 파괴적인 변경이 될 수 있습니다. 다른 폴더에 있는 코드가 변경되어야 할 수 있기 때문입니다.

<div class="content-ad"></div>

# Breaking Changes Types and Solutions

저는 사용자의 코드를 깨뜨리지 않으려는 보편적인 방식의 변경사항 세트를 공유하고 싶어요.

끝없이 깨뜨릴 수 없다는 것이 목표는 아닙니다. 요구 사항이 변경되고 코드는 그 문제에 대한 가장 간단한 해결책이어야 합니다.

목표는 사용자가 사전에 해당 변경 사항에 대비할 수 있도록 하는 것입니다. 사용자가 경고 없이 어느 날 갑자기 라이브러리를 깨뜨린다면 사용자는 행복하지 않을 것입니다. 그들의 작업 흐름이 몇 일 동안 깨져있을 수 있습니다.

<div class="content-ad"></div>

목표는 사용자들이 새로운 API로 원활하게 전환할 수 있는 프로세스를 만드는 것입니다. 변경 사항에 누구도 놀라지 않고 새로운 기능의 채택이 계획될 수 있도록 그리고 변화가 원활하게 이루어질 수 있도록합니다.

## 속성 변경

속성을 변경하는 것은 브레이크가 발생할 수 있는 가장 쉬운 곳일 것입니다. 다음 클래스를 예로 들어보겠습니다:

```js
class Stack<T> {
  private var content: [T] = []

  public init() {}
  
  public var count: Int {
    return content.count
  }
  
  public func push(_ value: T) {
    content.append(value)
  }
  
  public func pop() -> T? {
    guard !content.isEmpty else { return nil }
    return content.removeLast()
  }
}
```

<div class="content-ad"></div>

이 클래스는 push와 pop 연산을 가진 전통적인 스택을 나타냅니다. 또한 스택에 있는 항목 수를 반환하는 count 속성이 있습니다.

## 이름 변경

속성 이름을 변경하는 것은 중단 변경입니다. count 변수를 사용하던 사용자들은 이제 더 이상 사용할 수 없을 것입니다. 더 이상 존재하지 않기 때문입니다.

중단되지 않는 방식으로 이름을 변경하려면 다음 단계를 따라주세요:

<div class="content-ad"></div>

- 적절한 이름으로 새 속성을 추가하세요. 예를 들어 size 속성을 추가하십시오.
- count 속성이 내부적으로 size를 사용하도록 하세요.
- count 속성에는 사용이 중지된 애너테이션을 추가하세요.

```js
class Stack<T> {
  private var content: [T] = []

  public init() {}
  
  // 1. 새로운 속성
  public var size: Int {
    return content.count
  }
  
  // 3. 사용이 중지된 애너테이션 추가
  @available(*, deprecated, renamed: "size")
  public var count: Int {
    // 2. count 속성이 size 속성을 사용하도록 변경
    return size
  }
  // 스택의 나머지 코드
}
```

이렇게 하면 API 사용자가 이전 count 속성을 계속 사용할 수 있지만, 사용 시 다음과 같은 메시지가 표시됩니다:

<img src="/assets/img/2024-07-23-HowToDeprecateAPIstheRightWay_1.png" />

<div class="content-ad"></div>

이 버전의 @available 주석을 사용하면 Xcode의 기능을 활용하여 변수의 이름을 자동으로 변경할 수 있습니다. 삼각형을 클릭하면 경고 메시지가 확장되어 다음과 같이 표시됩니다:

![이미지](/assets/img/2024-07-23-HowToDeprecateAPIstheRightWay_2.png)

그리고 "수정"을 클릭하면 Xcode가 호출 위치를 자동으로 업데이트합니다.

경고 메시지의 중요한 부분은 실질적 조치가 가능해야 합니다: 사용자들에게 어떻게 해야 하는지 알려주지 않고 속성을 폐기하는 것으로 끝이라면 사용자들이 방황할 수 있으며, 그렇게 느껴지면 사용자를 잃기를 원하지 않으실 것입니다.

<div class="content-ad"></div>

위 코드가 준비되면 두 단계의 릴리스 접근 방식을 따를 수 있습니다:

- 사용 중단을 나타내는 새로운 버전의 속성을 릴리스합니다. 이전 프레임워크의 버전이 N이라면 버전 N+1을 릴리스해야 합니다.
- 이전 속성과 사용 중단 어노테이션을 삭제하고 버전 N+2를 릴리스합니다.

## 속성 유형 변경

또 다른 종류의 변경은 속성 유형을 변경하는 것입니다.

<div class="content-ad"></div>

이제 count가 사용자 정의 StackSize 타입을 반환할 것이라고 가정해봅시다:

```js
enum StackSize {
  case none
  case some(value: Int)
}

class Stack<T> {
  // ...
  public var count: StackSize {
    return content.isEmpty ? .none : .some(value: content.count)
  }
  // ...
}
```

이 변경은 사용자들이 count API로 숫자를 기대하지만 다른 객체를 받게 되므로 파괴적인 변화입니다.

이 문제를 해결하기 위해 두 단계 접근법이 필요한데, 먼저 다음 단계를 따릅니다:

<div class="content-ad"></div>


- 적절한 유형의 새 변수 newCount를 소개합니다.
- 기존의 count가 내부적으로 newCount를 사용하도록 만듭니다.
- 사용자에게 newCount 대신 count를 사용하도록 하는 사용 방법을 추가합니다.

```js
// PHASE 1
class Stack<T> {
  // ...
  
  public var newCount: StackSize {
    return content.isEmpty ? .none : .some(value: content.count)
  }
  
  @available(*, deprecated, message: "please use newCount")
  public var count: Int {
    switch self.newCount {
    case .none:
      return 0
    case .some(let value):
      return value
    }
  }
  // ...
}

// PHASE 2
class Stack<T> {
  // ...
  
  @available(*, deprecated, message: "please use count")
  public var newCount: StackSize {
    return count
  }
  
  public var count: StackSize {
    return content.isEmpty ? .none : .some(value: content.count)
  }
  // ...
}
```

이 시점에서 deprecation을 포함한 N+1 버전을 릴리스할 수 있습니다.

그런 다음, 변경된 이름의 프로퍼티 브레이킹 변경 전략을 적용하여 newCount를 count로 이름을 변경합니다.


<div class="content-ad"></div>

그러면 해당하는 마지막 API 버전으로 이전할 시간을 사용자들에게 제공하지만 필요한 변경 사항을 적용하는 데 세 가지 버전이 걸린다는 것을 알아두세요.

모든 속성 유형 변경이 기술적으로 필히 증상이 나타나겠지 않다는 것을 감안하세요. 만약, 속성이 클래스를 반환한다면, 진정한 기존 변경을 일으키지 않고 하위 클래스를 반환할 수 있습니다.

다음을 고려해보세요:

<div class="content-ad"></div>

```js
 클래스 PetFactory {
   var pet: Pet { return Dog() }
}

클래스 Pet {}
클래스 Dog: Pet {}
```

그리고 이렇게 pet 속성을 변경해보세요:

```js
클래스 PetFactory {
  var pet: Dog { return Dog() }
}
```

기술적으로는 이것이 중단 변경이 아닙니다. Dog도 Pet이기 때문에 다음과 같은 코드도 작동합니다:


<div class="content-ad"></div>

```js
Let p1 = PetFactory().pet
Let p2: Pet = PetFactory().pet
```

항상 작동될 것입니다.

## 속성 제거하기

공개 속성을 제거하는 것은 다른 파괴적 변경입니다. 이는 어떤 대체가 필요하지 않지만, 일반적으로 버전 N+1에서 속성을 주석 처리하는 것이 더 좋습니다. 사용자에게 곧 제거될 것임을 경고하기 위해 속성을 주석 처리하고 버전 N+2에서 그것을 제거하는 것이 좋습니다.

<div class="content-ad"></div>


```js
class Stack<T> {
  // ...
  @available(*, deprecated, message: "you should not check how many items a Stack contains")
  public var count: Int {
  // ...
}
```

# Methods Changes

The second common entity where breaking changes happen is methods.

You can apply the same strategies described for properties for some of the changes to methods. For example:


<div class="content-ad"></div>

- 메서드 이름 변경: 속성 이름 변경과 같은 전략을 사용합니다.
- 반환 유형 변경: 속성 유형 변경과 같은 전략을 사용합니다.
- 메서드 제거: 속성 제거와 같은 전략을 사용합니다.

메서드와 관련된 몇 가지 추가 변경 사항은 살펴볼 가치가 있습니다.

## 매개변수 이름 변경

매개변수 이름을 변경할 때는 중요한 변경 사항입니다. 메서드의 시그니처가 변경되었으므로 모든 호출지가 업데이트되어야 합니다.

<div class="content-ad"></div>

코드를 변경하지 않고 기술을 변경하는 기술은 메서드 이름을 바꾸는 기술과 비슷합니다. 매개변수 이름은 메서드 시그니처의 일부이며, 이는 메서드의 공식적인 이름입니다.

따라서 따라야 할 단계는 다음과 같습니다:

- 새 서명을 갖는 새 메서드를 추가합니다.
- 이전 메서드를 첫 번째 메서드를 호출하도록 변경하여 로직을 구현합니다.
- 이전 메서드에 새 이름으로 변경된 사용 금지 주석을 추가합니다.

실제로는 Stack 예제를 사용할 때 다음과 같이 보일 것입니다:

<div class="content-ad"></div>

```js
class Stack<t> {
  // ...

  // 1. 새로운 메서드 추가하기
  public func push(val: T) {
    content.append(val)
  }

  // 3. 이전 메서드에는 사용이 중단된 메시지를 추가합니다
  @available(*, deprecated, renamed: "push(val:)")
  public func push(_ value: T) {
    // 2. 이전 메서드가 새 메서드를 호출하도록 변경합니다
    push(val: value)
  }
}
```

또한, 이 경우에는 renamed 매개변수를 사용하여 Xcode가 사용자의 모든 호출 위치에서 문제를 자동으로 수정할 수 있습니다.

## 매개변수 유형 변경 또는 매개변수 제거하기

매개변수 유형을 변경하거나 매개변수를 제거하는 것은 매개변수 이름 변경 사례와 매우 유사합니다. 유일한 차이점은 @available 주석의 renamed 버전을 사용할 수 없다는 것입니다. Xcode가 호출 위치를 자동으로 업데이트할 수 없기 때문입니다.  

<div class="content-ad"></div>

그래서, 단계는 다음과 같습니다:

- 새로운 시그니처를 가진 새 메소드를 추가합니다.
- 이전 메소드가 로직을 구현하기 위해 첫 번째 메소드를 호출하도록 수정합니다.
- 이전 메소드에는 deprecated 주석을 추가합니다.

이전 메시지가 실행 가능하도록 만들어야 합니다. 고객이 코드를 사용하는 데 도움이 필요합니다. 예를 들어 Contacts 객체를 고려해 보세요:

```js
class Contacts {
  private var list: [String: (String, String)] = [:]

  public func add(contact: (name: String, number: String)) {
    list[contact.name] = contact
  }
}
```

<div class="content-ad"></div>

contacts 객체는 튜플을 사용하여 연락처를 추가할 수 있게 합니다. 그런 다음, 튜플 대신 Contact struct를 사용하기로 결정했습니다:

```js
struct Contact {
  let name: String
  let number: String
}
```

Contacts 코드를 안전하게 변경하기 위해 다음 규칙을 따르십시오:

```js
class Contacts {
  private var list: [String: Contact] = [:]

  public func add(contact: Contact) {
    list[contact.name] = contact
  }
  
  @available(*, deprecated, message: "This version of add is deprecated. Please call add with a Contact object")
  public func add(contact: (name: String, number: String)) {
    let newContact = Contact(
      name: contact.name,
      number: contact.number
    )
    add(contact: newContact)
  }
}
```

<div class="content-ad"></div>

이전 add 함수가 새로운 함수를 호출하도록 변경해야 합니다. 이를 위해 튜플을 새 객체로 변환한 다음 이전 메서드를 호출합니다.

파라미터를 제거해야 하는 경우에도 동일한 방식을 사용할 수 있지만, 더 간단합니다. 이전 메서드는 제거된 파라미터를 전달하지 않고 단순히 새 메서드를 호출합니다. 사용해서는 안 되는 추가 함수를 제거하고 코드베이스를 간소화하는 것이 최종 목표이므로 이전 방법이 계속 사용되어야 합니다.

## 파라미터 추가

파라미터를 추가할 때 발생하는 마지막 유형의 파손 변경 사항은 파라미터를 추가하는 경우입니다. "파괴적 변경사항이 아닌 것은?" 섹션에서 이를 해결하는 한 가지 방법을 보여드렸습니다. 새로운 파라미터에 대한 합리적인 기본값을 제공할 수 있다면 변경 사항이 파손적이 아닙니다.

<div class="content-ad"></div>

그게 불가능할 때, 해결책은 다음과 같습니다:

- 이전 기능을 한 번 더 버전에 대해 지원하기.
- 새로운 매개변수를 생성하고 새로운 방법을 사용하는 방법을 제안하는 폐지 메시지가 있는 메소드에 주석 달기.

폐지 메시지에서 제안하는 조치에는 사용자가 새로운 메소드를 사용하는 데 필요한 추가 매개변수를 제공하는 문서 링크가 포함될 수 있습니다.

경고를 표시할 때 Xcode는 URL을 클릭할 수 있게 만들어 사용자가 새 API를 사용하는 데 필요한 조치를 알 수 있도록 도와줍니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-23-HowToDeprecateAPIstheRightWay_3.png)

# 결론

오늘의 글에서 API 수준에서 발생할 수 있는 일반적인 파괴적인 변경 유형을 탐구했습니다. 우리는 속성과 메서드에 초점을 맞췄지만, 클래스, 구조체, 프로토콜 또는 열거형의 이름을 바꿀 때도 파괴적인 변경이 발생할 수 있습니다. 이러한 변경 사항을 작업하는 기술은 유사합니다:

- 새로 도입하고 싶은 새 유형을 추가합니다.
- 가능하다면 이전 유형을 새 유형에 준수하거나 확장합니다. 그렇지 않으면 다음 버전에 두 유형을 모두 지원합니다.
- 이전 유형의 맨 위에 폐지 메시지를 추가합니다.


<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변환해 주세요.

<div class="content-ad"></div>

이 기사에서 논의된 기법을 꾸준히 적용하면 제품을 계속 개선하면서 충분한 시간과 정보를 제공하여 고객들이 새로운 API 버전으로 이전할 수 있습니다. 경우에 따라 사용자를 위해 일부 작업을 Xcode를 활용할 수도 있습니다. 이보다 더 좋은 것이 무엇이죠?