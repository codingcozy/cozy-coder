---
title: "Swift-iOS 면접 질문 베스트 8가지"
description: ""
coverImage: "/assets/img/2024-07-14-Swift-iOSInterviewQuestions-Part8_0.png"
date: 2024-07-14 00:18
ogImage: 
  url: /assets/img/2024-07-14-Swift-iOSInterviewQuestions-Part8_0.png
tag: Tech
originalTitle: "Swift-iOS Interview Questions-Part 8"
link: "https://medium.com/swift-interview-preparations/swift-ios-interview-questions-part-8-124723f72c08"
---


스위프트, iOS, Xcode에 관한 인터뷰 질문

![image](/assets/img/2024-07-14-Swift-iOSInterviewQuestions-Part8_0.png)

- GCD에서 UI 작업은 어디에서 수행해야 합니까?

메인 큐는 UI 업데이트와 이미지 가져오기에 완벽하며, 다른 무거운 작업은 백그라운드 스레드에서 실행되어야 합니다.

<div class="content-ad"></div>

```swift
DispatchQueue.global(qos: .background).async {
    // 여기에 데이터를 로드합니다
    if let url = URL(string: "https://example.com/data.json"),
       let data = try? Data(contentsOf: url) {
        do {
            // 데이터 파싱 또는 처리를 수행합니다
            if let json = try JSONSerialization.jsonObject(with: data, options: []) as? [String: Any] {
                // 주 스레드에서 UI를 업데이트합니다
                DispatchQueue.main.async {
                    // 여기서 UI를 업데이트합니다
                    self.textView.text = json.description
                }
            }
        } catch {
            // 에러 처리
            print("JSON 파싱 에러: \(error.localizedDescription)")
        }
    } else {
        // 에러 처리
        print("데이터를 불러오는 중 에러 발생.")
    }
}

이 Swift 예시에서:

- DispatchQueue.global(qos: .background).async는 백그라운드에서 데이터 로딩을 수행합니다.
- URL 및 데이터 로딩이 안전하게 해제됩니다.
- JSON 데이터는 오류를 처리하는 do-catch 블록에서 파싱됩니다.
- 데이터가 성공적으로 로드되고 파싱된 후에는 DispatchQueue.main.async가 사용되어 UI를 주 스레드에서 업데이트합니다.

2. 빠른 열거(fast enumeration)란 무엇인가요?

<div class="content-ad"></div>

코코아 프레임워크의 여러 클래스, 특히 컬렉션 클래스들은 NSFastEnumeration 프로토콜을 채택합니다. 이 프로토콜을 사용하여 해당 인스턴스에 의해 보유된 요소들을 표준 C for 루프와 유사한 구문을 사용해 검색할 수 있습니다. 다음 예시에서 설명된 것처럼요: 

let anArray: [Any] = ["one", "two", "three"]
for element in anArray {
    print(element)
}

Swift에서, 배열의 요소 유형을 알고 있다면 더 정확하게 지정할 수 있습니다. 예를 들어, 배열이 문자열만 포함한다면 [String]을 사용할 수 있습니다:

let anArray: [String] = ["one", "two", "three"]
for element in anArray {
    print(element)
}

<div class="content-ad"></div>

Instance(인스턴스) 메서드는 특정 클래스, 구조체 또는 열거형의 인스턴스에 속하는 함수입니다. 이러한 인스턴스의 기능을 지원하며 인스턴스의 속성에 액세스하고 수정하는 방법을 제공하거나 인스턴스의 목적과 관련된 기능을 제공함으로써 기여합니다.

클래스(클래스) 메서드는 타입 자체에서 호출됩니다. 또한 타입 메서드라고도합니다. 클래스의 타입 메소드를 나타내려면 메서드의 func 키워드 앞에 class 키워드를 작성하여 클래스 내에서, 그리고 구조체와 열거형 내에서는 메서드의 func 키워드 앞에 static 키워드를 작성합니다.

서브클래스는 클래스 메서드를 재정의할 수 있습니다. 왜냐하면 이것들은 동적으로 디스패치 되기 때문입니다. static메서드는 재정의할 수 없습니다. 클래스 속성은 이론적으로 동일한 방식으로 작동할 것으로 예상됩니다(서브클래스는 재정의할 수 있음), 그러나 현재 Swift에서는 불가능합니다. static은 "Class final"로 간주될 수 있습니다.

<div class="content-ad"></div>

4. UIButton은 UIControl을 상속받고, UIControl은 UIView를 상속받고, UIView는 UIResponder를 상속받으며, UIResponder은 루트 클래스인 NSObject를 상속받습니다.

NSObject
   |
UIResponder
   |
UIView
   |
UIControl
   |
UIButton

5. 레이어 객체란 무엇인가요?

<div class="content-ad"></div>

레이어 객체는 시각적 콘텐츠를 나타내는 데이터 객체로, 뷰에서 이 콘텐츠를 렌더링하는 데 사용됩니다. 사용자 정의 레이어 객체를 추가하여 복잡한 애니메이션 및 고급 시각적 효과를 구현할 수도 있습니다.

6. 애플리케이션의 UI 요소를 사용하는 테스트 스크립트를 작성할 때 어떤 API를 사용해야 합니까?

UI Automation API를 사용하여 테스트 프로시저를 자동화합니다. JavaScript 테스트 스크립트는 애플리케이션과의 사용자 상호작용을 시뮬레이션하고 로그 정보를 호스트 컴퓨터에 반환합니다.

7. UIKit 클래스를 사용해야 하는 애플리케이션 스레드는 무엇입니까?

<div class="content-ad"></div>

앱의 주 스레드에서 UIKit 클래스를 사용할 수 있어요.

8. 자식 클래스에서 super 키워드를 설명해 드릴까요?

우리는 자식 클래스의 저장 프로퍼티를 설정한 후, 부모 클래스 이니셜라이저를 호출하기 위해 super 키워드를 사용해요.

9. iOS에서 말하는 딥 링킹이란 무엇인가요?

<div class="content-ad"></div>

깊은 링크(Deep links)란 URL이나 유니버설 링크를 사용하여 웹사이트나 스토어 대신 앱으로 바로 이동시키는 링크입니다. URL 스키마는 깊은 링크를 만드는 잘 알려진 방법 중 하나이지만, 유니버설 링크는 애플의 새로운 방식으로 웹페이지와 앱을 하나의 링크 아래로 연결하는 방법입니다. 깊은 링크를 사용하면 앱을 열 수 있는 클릭 가능한 링크와 필요한 리소스로 이동하는 스마트한 링크를 생성할 수 있습니다. 이러한 링크를 사용하면 사용자들은 본인이 직접 페이지를 보는 수고와 시간을 아낄 수 있으며, 그 결과 사용자 경험을 크게 향상시킬 수 있습니다.

iOS 애플리케이션의 사용자 인터페이스를 구축하는 데 사용되는 프레임워크는 무엇인가요?

파운데이션 프레임워크는 iOS 및 OS X 개발에 대한 클래스, 프로토콜 및 함수를 정의하며, UIKit은 특히 iOS 개발을 위해 설계되었습니다. iOS에서 UIKit을 사용하여 애플리케이션의 사용자 인터페이스 및 그래픽 인프라를 개발합니다. 이에는 다음이 포함됩니다:

- 앱 구조(시스템과 사용자 간의 상호작용 관리)
- 이벤트 처리(입력 제스처, 멀티터치 제스처 등 다양한 제스처 처리)
- 사용자 인터페이스(사용자 상호작용, 텍스트 및 콘텐츠 공유, 이미지 선택, 비디오 편집, 파일 인쇄 등)
- 그래픽, 그리기 및 인쇄

<div class="content-ad"></div>

11. KVC와 KVO의 차이점을 설명해 드리겠습니다.

KVC (Key-value-coding)

KVC는 객체 속성을 개발 중에 정적으로 속성 이름을 알 필요 없이 문자열을 사용하여 런타임에서 액세스할 수 있게 합니다.

KVO (Key-Value Observing)

<div class="content-ad"></div>

KVO는 Objective-C와 Swift에서 프로그램 상태 변경을 관찰하는 방법 중 하나입니다. 만약 객체가 인스턴스 변수를 가지고 있다면, KVO를 사용하여 다른 객체들이 해당 변수의 변경사항을 관찰할 수 있습니다.

iOS 아키텍처는 어떻게 생겼을까요?

iOS 아키텍처는 네 개의 레이어로 구성되어 있습니다. 각 레이어는 하드웨어 위에서 작동하는 애플리케이션을 위한 프로그래밍 프레임워크를 제공합니다. 상위 레벨 레이어는 그래픽 및 인터페이스 관련 서비스를 제공하고, 하위 레벨 레이어는 모든 애플리케이션이 필요로 하는 서비스를 제공합니다. 응용 프로그램과 하드웨어 레이어 사이의 레이어는 통신을 강화합니다.

다양한 레이어는 다음과 같습니다:

<div class="content-ad"></div>

- 핵심 OS/응용 프로그램 레이어: 핵심 OS 레이어는 가장 낮은 수준의 레이어로서 기기 하드웨어 위에 위치합니다. 이 레이어는 메모리, 파일 시스템, 쓰레드, 그리고 낮은 수준의 네트워킹, 외부 액세서리 접근 등을 관리합니다.
- 서비스 레이어: 서비스 레이어는 상위 레이어와 사용자들이 요구하는 서비스를 설계합니다. 이 레이어는 객체 블록, Grand Central Dispatch, 인앱 구매, iCloud 저장소 등을 처리합니다.
- 미디어 레이어: 미디어 레이어는 비디오, 오디오, 그래픽과 같은 미디어를 처리하고 가능하게 합니다.
- 코코아 터치 레이어: 이것은 응용 프로그램 레이어로, 여러분이 프레임워크를 만드는 곳입니다. 이것은 또한 iOS 사용자들이 운영 체제와 상호 작용하는 인터페이스로도 작동합니다.

13. 얕은 복사와 깊은 복사의 차이점은 무엇인가요? 얕은 복사는 주소 복사로도 알려져 있습니다. 이 과정에서 실제 데이터가 아닌 주소만 복사됩니다. 그에 비해 깊은 복사는 데이터가 복사됩니다. 두 개의 객체 A와 B가 있다고 가정합시다. A는 다른 배열을 가리키고 있고, B는 다른 배열을 가리키고 있습니다. 이제 나는 얕은 복사를 수행하기 위해 다음과 같이 할 것입니다. 
Char *A = '‘a’,’b’,’c’';
Char *B = '‘x’,’y’,’z’';
B = A;
이제 B는 A 포인터가 가리키는 위치와 같은 위치를 가리킵니다. 이 경우 A와 B는 동일한 데이터를 공유합니다. 만일 변경 사항이 생기면 두 개의 값이 변경됩니다. 장점은 복사 과정이 매우 빠르며 배열의 크기에 독립적입니다.

깊은 복사에서는 데이터도 복사됩니다. 이 과정은 느리지만 A와 B는 각자의 복사본을 갖게 되며, 한 복사본에 변경을 가해도 다른 복사본에는 영향을 미치지 않습니다.

14. Swift에서 프로퍼티를 선택적으로 만들 수 있는 방법은 무엇인가요?

<div class="content-ad"></div>

코드에서 물음표 ??’를 선언하여 속성을 선택 사항으로 만들어야 합니다. 속성에 값이 없으면 기호 ? 를 사용하여 런타임 오류를 피할 수 있습니다.

**15. 스위프트에서의 반 열린 범위 연산자란 무엇인가요?**

스위프트는 여러 유형의 연산자를 지원합니다. 그 중 하나가 반 열린 범위 연산자입니다. 반 열린 범위 연산자는 a와 b( a`b) 두 값 사이의 범위를 지정합니다. 이때 b는 포함되지 않습니다. 이 연산자는 첫 번째 값만을 포함하고 마지막 값은 포함하지 않기 때문에 반 열린 범위 연산자로 알려져 있습니다.

## 반 열린 범위 연산자를 루프에서 사용하기

<div class="content-ad"></div>

let range = 1..<5
for number in range {
    print(number)
}

Output:

1
2
3
4

16. Swift에서 함수는 무엇인가요?
```

<div class="content-ad"></div>

하수는 특정 작업을 수행하는 코드 세트입니다. Swift 프로그래밍 언어에서 함수는 함수 호출 내부에서 로컬 및 글로벌 매개변수 값을 전달하는 데 사용됩니다.

Swift에서 함수는 두 가지 유형으로 분류할 수 있습니다:

- 사용자 정의 함수
- 내장 함수 (라이브러리 함수)

17. Swift에서 중첩 함수란 무엇입니까?

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 바꾸세요.

<div class="content-ad"></div>

이넘(Enum)은 Swift 열거형으로도 알려져 있어요. 이넘은 관련 있는 값들의 집합을 담고 있는 데이터 타입이에요. 클래스 안에서 선언되며, 이넘의 값들은 해당 클래스의 인스턴스 멤버들을 통해 접근될 수 있어요.

구문:

```js
enum enum_name
{
// 여기에 값들을 설명합니다
}
```

정규 표현식(Regular Expression)과 응답자 체인(Responder Chain)은 Swift에서 무엇인가요?

<div class="content-ad"></div>

정규 표현식: 정규 표현식은 문자열 내에서 검색을 수행하는 방법을 지정하는 특별한 문자열 패턴들입니다.

응답 체인: 응답 체인은 이벤트에 응답할 수 있는 객체들의 계층 구조입니다.

20. Swift에서 사전(Dictionary)을 설명하세요.

Swift의 사전(Dictionary)은 key-value 쌍을 저장하고 키를 사용하여 값을 액세스하는 데 사용됩니다. 다른 프로그래밍 언어의 해시 테이블과 유사합니다.

<div class="content-ad"></div>

Swift 프로그래밍 언어에서는 키-값 쌍을 기반으로 값을 얻고 싶을 때 언제든지 사전(딕셔너리) 개념을 활용할 수 있어요.

Swift 사전의 구문:

다음은 Swift 프로그래밍 언어에서 사전을 정의하는 구문이에요.

```swift
Dictionary<Key, Value>()
또는
[Key: Value]()
```

<div class="content-ad"></div>

21. **프로세서 관리는 무엇인가요?**

프로세서 관리는 비즈니스 프로세스를 분석, 정의, 최적화 및 제어하기 위한 도구와 자원을 제공합니다. 이를 통해 더 나은 성능을 얻을 수 있습니다.

22. **iOS의 프로세스 관리는 무엇인가요?**

iOS의 각 프로세스는 하나 이상의 스레드로 구성됩니다. 각 스레드는 단일 실행 경로로 작동합니다. iOS의 각 애플리케이션은 주요 기능을 실행하는 단일 스레드로 시작됩니다.

<div class="content-ad"></div>

스위프트에서 부동 소수점 수란 무엇인가요? 스위프트에서 다양한 부동 소수점 수는 무엇인가요?

소수점을 포함하거나 분수 구성 요소가 있는 숫자를 부동 소수점 수라고 합니다. 예를 들어, 1.34는 부동 소수점 수입니다. 부동 소수점 유형은 정수 유형보다 더 넓은 값의 범위를 표현할 수 있습니다. 스위프트에서는 두 가지 부호가 있는 부동 소수점 숫자가 있습니다:

더블(Double): 64비트 부동 소수점 수를 나타냅니다. 부동 소수점 값이 매우 큰 경우에 사용됩니다.

플로트(Float): 32비트 부동 소수점 수를 나타냅니다. 부동 소수점 값이 64비트 정밀도가 필요하지 않은 경우 사용됩니다.

<div class="content-ad"></div>

24. Swift에서 주석을 작성하는 방법은 무엇인가요?

Swift 프로그래밍 언어에서는 한 줄 주석을 작성할 때 이중 슬래시 (//)를 사용합니다.

예를 들어:

```swift
// 이것은 한 줄 주석입니다.
```

<div class="content-ad"></div>

/* 영어를 한국어로 번역해주셔서 감사합니다! 트럼프 전문가의 의견을 들려드릴게요. */

스위치 문은 스위프트(Swift) 언어에서 어떻게 사용되나요? 

스위치 문은 조건에 따라 코드를 분기 처리하는 데 사용됩니다. 특정 변수나 값에 대해 여러 경우의 수를 고려할 때 유용해요. case 별로 다른 실행 블록을 정의하여 각 case에 해당하는 코드를 실행할 수 있습니다. 이를 통해 코드의 논리를 정리하고 가독성을 높일 수 있어요.

위의 예시처럼 스위치 문은 변수나 값이 다양한 경우에 따라 다른 실행 흐름을 제어할 수 있어서 매우 유용한 기능이에요! 만약 스위프트에서 스위치 문을 활용하는 방법이 궁금하시다면 더 질문해주세요. 함께 알아보아요! 🌟

<div class="content-ad"></div>

- `switch` 문은 긴 if-else-if 문의 대안으로 사용됩니다.
- `switch` 문은 모든 종류의 데이터를 지원하며, 이를 동기화하고 동등성을 확인합니다.
- `switch` 문에는 fall through가 없기 때문에 `break`는 필요하지 않습니다.
- `switch` 문은 변수의 모든 가능한 값에 대해 처리해야 합니다.

**26. Swift 언어에서 `break` 문을 사용하는 목적**

`break` 문은 즉시 문을 중단해야 하는 반복문 내에서 사용됩니다. 또한 `switch` 문에서 case를 종료하는 데에도 사용됩니다.

**27. Swift 루프에서 `continue` 문의 사용 목적**

<div class="content-ad"></div>

The `continue` statement in Swift loops is a powerful tool to alter the flow of execution. When encountered, it immediately halts the current iteration and proceeds directly to the start of the next cycle within the loop.

### Swift Collections Overview:

In Swift, there are two fundamental collection types:

- **Array**: Arrays in Swift can store multiple values of the same or different data types.

<div class="content-ad"></div>

딕셔너리: Swift에서 딕셔너리는 다른 프로그래밍 언어의 해시 테이블과 유사합니다. 딕셔너리에는 키-값 쌍을 저장하고 키를 사용하여 값을 액세스할 수 있습니다.

스위프트에서의 상속이란 무엇인가요?

상속은 한 클래스가 다른 클래스로부터 속성, 메서드 및 다른 특성을 상속받는 과정입니다. Swift 프로그래밍 언어에서는 상속이 지원됩니다. Swift에서 상속에는 두 가지 유형의 클래스가 있습니다:

하위 클래스: 다른 클래스로부터 속성을 상속받는 클래스를 자식 클래스 또는 하위 클래스라고 합니다.

<div class="content-ad"></div>

슈퍼 클래스: 서브 클래스가 속성을 상속하는 주요 클래스를 부모 클래스 또는 슈퍼 클래스라고 합니다.

30. Swift 프로그램이 배포되는 방법을 설명해주세요.

Swift 프로그램은 기본적으로 Tomcat 설치에 응용 프로그램을 배포합니다. 배포 스크립트는 클라이언트 코드를 JavaScript로 번들링하고, 필요한 모든 서버 측 클래스를 패키지 파일인 Hello.war로 모읍니다. 이 파일은 GWT jar 및 Swift 런타임 jar와 함께 Tomcat 설치 위치로 복사됩니다. CATALINA_HOME이 설정되어 있지 않으면 이 파일들을 수동으로 복사해야 합니다.

31. NSUserDefaults가 무엇인지 설명하고, NSUserDefaults에서 지원하는 타입은 무엇인가요?

<div class="content-ad"></div>

NSUserDefaults는 키-값 쌍을 사용하여 데이터를 저장하는 가장 쉬운 방법이에요. NSUserDefaults는 데이터를 작은 양만 저장합니다. 대부분의 경우 NSUserDefaults는 사용자 설정 및 중요하지 않은 데이터를 저장하는 데 가장 적합합니다. NSUserDefaults가 지원하는 유형은 다음과 같아요:

- NSString
- NSNumber
- NSDate
- NSDictionary
- NSData

장점

- 데이터를 저장하고 검색하기 비교적 쉬워요
- 작은 크기의 데이터를 유지하는 데 완벽합니다 (예: 사용자 설정)
- 배우고 구현하기 쉬워요

<div class="content-ad"></div>

단점

- 대량의 데이터를 수집하기에 적합하지 않음
- 대량의 데이터를 저장할 때 성능이 저하될 수 있음
- 민감한 데이터를 저장하기에는 이상적이지 않음

32. UI를 작성할 때 어떤 것을 선호하나요? Xib 파일, 스토리보드, 또는 프로그래밍 방식의 UIView?

iOS 인터뷰를 준비할 때 UI를 작성하는 전략을 알고 있어야 합니다. 스토리보드와 Xib는 디자인 사양과 잘 맞는 빠른 UI를 만들 때 좋습니다. 제품 관리자가 화면 진행 상황을 쉽게 파악할 수도 있습니다. 스토리보드는 응용 프로그램을 통한 흐름을 잘 표현하고 전체 응용 프로그램의 고수준 시각화를 제공하는 데도 좋습니다. 스토리보드의 단점은 팀 환경에서 협업하기 어려우며, 단일 파일이기 때문에 병합을 제어하기 어려울 수 있습니다. 스토리보드와 Xib 파일은 중복되어 업데이트하기 복잡해질 수 있습니다. 예를 들어, 모든 버튼이 동일하게 보이도록 하고 갑자기 색상을 변경해야하는 경우 스토리보드와 Xib 전체에서 이를 수행하는 것이 오랜 시간/어려운 프로세스가 될 수 있습니다. 프로그래밍 방식으로 UIView를 만드는 것은 비효율적이고 지루할 수 있지만 더 많은 제어와 쉬운 분리를 허용할 수 있습니다. 또한 코드를 공유할 수 있습니다. 그리고 아주 쉽게 테스트할 수도 있습니다.

<div class="content-ad"></div>

33. UIView 요소의 레이아웃을 식별하는 다양한 방법이 무엇인가요?

UIView의 요소 레이아웃을 지정하는 몇 가지 일반적인 방법은 다음과 같습니다:

- InterfaceBuilder를 사용하면 프로젝트에 XIB 파일을 추가하여 레이아웃 요소를 포함한 후, XIB 파일을 응용 프로그램 코드에서 로드할 수 있습니다. InterfaceBuilder를 사용하여 응용 프로그램을 위한 스토리보드를 만들 수 있습니다.
- NSLayoutConstraints를 사용하여 코드를 작성하여 요소를 자동 레이아웃으로 정렬할 수 있습니다.
- 각 요소의 정확한 좌표를 설명하는 CGRect를 생성하고 이를 UIView에 전달할 수 있습니다.

34. Size Classes의 목적은 무엇인가요?

<div class="content-ad"></div>

사이즈 클래스는 앱에서 추가적인 레이아웃 구성을 할 수 있는 기능으로, 여러 기기에서 UI가 잘 작동하도록 도와줍니다. 예를 들어, 스택 뷰는 일반적인 조건에서는 가로로 뷰를 정렬하지만 제약 조건이 있는 경우 세로로 정렬될 수 있습니다.

35. Swift에서 물음표 ? 는 무엇을 의미할까요?

물음표 ? 는 속성을 선언할 때 사용되며 해당 속성이 옵셔널임을 컴파일러에게 알려줍니다. 이 속성은 값을 가질 수도, 가지지 않을 수도 있는데, 후자의 경우에는 그 속성에 접근할 때 런타임 오류를 피하기 위해 ? 를 사용할 수 있습니다. 이것은 옵셔널 체이닝(아래 참조)에서 유용하며, 이 예시의 변형은 조건문에서 사용될 수 있습니다.

```swift
var optionalName : String? = "Baljit"
if optionalName != nil {
    print("Your name is \(optionalName!)")
}
```

<div class="content-ad"></div>


36. 이중 물음표의 사용 목적은?

변수에 기본값을 제공하기 위해 사용됩니다.

```js
let missingName: String? = nil
let realName: String? = "Baljit Kaur"
let existentName: String = missingName ?? realName
```

37. Swift에서 타입 별칭(Type aliasing)이란 무엇인가요?

<div class="content-ad"></div>

이것은 C/C++에서 가져온 많은 내용을 빌려온 것입니다. 이 것은 타입에 별칭을 부여할 수 있게 해주는데, 이는 특정한 맥락에서 유용할 수 있습니다.

이것은 코드를 더 가독성 있게 만들어주고 이해하기 쉽게 만들어줄 것입니다.

```js
typealias AudioSample = UInt16
```

또한 스위프트에서 함수와 메서드의 차이는 무엇인가요? 🌟

<div class="content-ad"></div>

이 두 가지는 보통 프로그래머가 알아야 하는 용어로 설명할 수 있어요. 즉, 특정 작업을 수행하기 위해 이상적으로 설정된 자체 포괄적인 코드 블록입니다. 함수는 전역 범위에 존재하는 반면 메소드는 특정 유형에 속합니다.

39. Swift의 optional binding과 optional chaining은 무엇인가요?

답: Optional binding 또는 chaining은 선택적으로 선언된 속성들과 함께 유용합니다. 다음 예제를 보세요:

```swift
class Student { var courses: [Course]? }
let student = Student()
```

<div class="content-ad"></div>

40. 옵셔널 체이닝

만약 느낌표(!)를 통해 courses 속성에 접근하려고 한다면, 이 속성이 아직 초기화되지 않았기 때문에 런타임 오류가 발생할 것입니다. 옵셔널 체이닝은 이 값을 안전하게 언래핑할 수 있게 해줍니다. 속성 뒤에 물음표(?)를 놓음으로써 사용하며, nil을 포함할 수 있는 옵셔널에 대해 속성과 메소드를 질의하는 방법입니다. 이는 강제 언래핑의 대안으로 간주될 수 있습니다.

41. 옵셔널 바인딩

옵셔널 바인딩은 if나 while 블록의 첫 번째 절에서 옵셔널에서 임시 변수를 할당할 때 사용되는 용어입니다. 아래 코드 블록을 고려해보세요. 만약 속성 courses가 아직 초기화되지 않은 상태라면 런타임 오류 대신 블록이 정상적으로 실행되는 것을 확인할 수 있습니다.

<div class="content-ad"></div>

위의 코드는 학생의 코스를 나타내는 배열을 초기화하지 않아 계속될 것입니다. 간단히 아래 코드를 추가해보세요:

```swift
init() { 
   courses = [Course]() 
}
```

그러면 “Yep, courses we have”가 출력됩니다.

<div class="content-ad"></div>

외부 매개변수의 구문은 로컬 매개변수 이름 앞에 위치합니다.

```js
func yourFunction(externalParameterName localParameterName :Type, …) { … }
```

이를 구체적인 예시로 들어보겠습니다:

<div class="content-ad"></div>

43. Swift에서 에러를 처리하는 방법은 어떻게 해야 할까요?

Swift에서의 에러 처리 방법은 Objective-C와 다소 다릅니다. Swift에서는 함수가 에러를 throw할 수 있다고 선언하는 것이 가능합니다. 따라서, 에러를 처리하거나 전파하는 것은 호출하는 쪽의 책임입니다. 이는 Java에서 상황을 다루는 방식과 유사합니다.

에러를 throw할 수 있는 함수는 함수 이름 뒤에 throws 키워드를 추가하여 선언하면 됩니다. 이러한 메소드를 호출하는 모든 함수는 try 블록에서 호출해야 합니다.

<div class="content-ad"></div>

func canThrowErrors() throws -` String // 에러를 던지는 메서드를 호출하는 방법try canThrowErrors() //Optional로 지정할 수도 있음let maybe = try? canThrowErrors()`

44. Swift에서 Guard문은 무엇인가요?

Guard문은 수비적인 프로그래밍 스타일을 선호하는 사람들에게 좋은 제어 흐름 문장입니다. 이 문장은 불리언 조건을 평가하고, 평가 결과가 참이면 프로그램 실행을 계속합니다. Guard문은 항상 else 절을 가지며, 해당 부분에 도달하면 코드 블록을 나가야 합니다.

```swift
guard let courses = student.courses else { return }
```

<div class="content-ad"></div>

45. 다음 코드를 고려해 보십시오:

```js
var array1 = [1, 2, 3, 4, 5]
var array2 = array1
array2.append(6)
var len = array1.count
```

변수 len의 값은 무엇이며, 그 이유는 무엇인가요?

len 변수의 값은 5입니다. 이는 array1에 5개의 요소가 있음을 나타내며, array2에는 6개의 요소가 있음을 의미합니다.

<div class="content-ad"></div>

```js
array1 = [1, 2, 3, 4, 5]

array2 = [1, 2, 3, 4, 5, 6]

array1이 array2에 할당될 때, array1의 복사본이 실제로 생성되어 할당됩니다.

이유는 스위프트 배열이 값 타입(구조체로 구현됨)이기 때문입니다. 참조 타입(즉, 클래스)이 아니기 때문에 변수에 값 타입이 할당되거나 함수나 메소드에 인자로 전달되거나 움직일 때 실제로 복사본이 생성되어 할당되거나 전달됩니다. 스위프트 딕셔너리 또한 값 타입으로 구조체로 구현되어 있습니다.
```

<div class="content-ad"></div>

스위프트의 값 타입에는 다음이 있어요:

- 구조체(배열 및 딕셔너리 포함)
- 열거형
- 기본 데이터 타입(부울린, 정수, 실수 등)

46. 다음 코드를 고려해보세요:

```js
let op1: Int = 1
let op2: UInt = 2
let op3: Double = 3.34
var result = op1 + op2 + op3
```

<div class="content-ad"></div>

에러는 어디에 있고 왜 발생하는 걸까요? 이를 어떻게 고치면 될까요?

답변: Swift는 데이터 유형 간 암시적 캐스트를 정의하지 않습니다. 심지어 개념적으로 거의 동일한 경우에도 (예: UInt 및 Int).

에러를 수정하려면 캐스트 대신 명시적 변환이 필요합니다. 샘플 코드에서 모든 표현식 피연산자는 Double로 변환되어야 합니다. 이 경우 모두 같은 유형으로 변환되어야 합니다:

```js
var result = Double(op1) + Double(op2) + op3
```

<div class="content-ad"></div>

The String struct in Swift doesn't have a count or length property to directly get the number of characters it contains. Instead, you can use the global `countElements` function. When applied to strings, the complexity of the `countElements` function is O(n).

Swift strings are designed to support extended grapheme clusters. Each character in a string is a sequence of one or more unicode scalars that combine to form a single human-readable character. Due to the variable memory requirements of different characters and the fact that accessing an extended grapheme cluster requires sequential traversal to determine the character it represents, the number of characters in a string cannot be known without traversing the entire string. This is why the complexity of the `countElements` function is O(n).

<div class="content-ad"></div>

스위프트 열거형에서 로우 값(raw values)과 연관 값(associated values)의 차이점은 무엇일까요?

로우 값은 열거형 케이스에 상수(문자 상수) 값을 연결하는 데 사용됩니다. 값의 유형은 열거형 유형의 일부이며, 각 열거형 케이스는 고유한 로우 값(중복 값은 허용되지 않음)을 지정해야 합니다.

다음 예시는 Int 타입의 로우 값으로 열거형을 보여줍니다:

```swift
enum IntEnum : Int {
  case ONE = 1
  case TWO = 2
  case THREE = 3
}
```

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 변경하면 좋아요:


An enum value can be converted to its raw value by using the rawValue property:

var enumVar: IntEnum = IntEnum.TWO
var rawValue: Int = enumVar.rawValue


A raw value can be converted to an enum instance by using a dedicated initializer:


var enumVar: IntEnum? = IntEnum(rawValue: 1)


<div class="content-ad"></div>

연관 값은 특정 enum 케이스에 임의의 데이터를 연결하는 데 사용됩니다. 각 enum 케이스는 케이스 정의에서 튜플로 선언된 영 개 이상의 연관 값이 있을 수 있습니다.

```swift
enum AssociatedEnum {
  case EMPTY
  case WITH_INT(value: Int)
  case WITH_TUPLE(value: Int, text: String, data: [Float])
}
```

케이스에 연관된 타입은 enum 선언의 일부인 반면, 연관 값은 인스턴스별로 지정되므로 enum 케이스는 서로 다른 연관 값을 가질 수 있습니다.

49. 다음 코드 스니펫은 컴파일 시간 오류가 발생합니다.

<div class="content-ad"></div>

```swift
struct IntStack {
  var items = [Int]()
  mutating func add(x: Int) {
    items.append(x) // No more compile time error.
  }
}
```

그 부분을 수정하면 에러가 사라집니다. 간단하죠? :)

<div class="content-ad"></div>

**IntStack 구조체:**

```js
struct IntStack {
    var items = [Int]()
    mutating func add(x: Int) {
        items.append(x) // 모두 좋아요!
    }
}
```

**50. NSMutableArray를 Swift Array로 변환하는 방법**

```js
var myMutableArray = NSMutableArray(array: ["xcode", "eclipse", "net bins"])
var myswiftArray = myMutableArray as NSArray as! [String]
```

이 글이 마음에 드셨다면 공유하고 박수를 보내주셔서 다른 사람들도 찾을 수 있게 해주세요👏🏻👏🏻👏🏻👏🏻👏🏻

<div class="content-ad"></div>

당신은 📲로도 저와 연락할 수 있어요.

이쪽 아래에서 제 코드를 확인해볼 수 있어요👇🏻:

[GitHub](GitHub)

<div class="content-ad"></div>

# 재밌게 보셨나요?

의견, 질문 또는 추천사항이 있으면 아래 댓글 섹션에 자유롭게 남겨주세요💬

💁🏻‍♀️ 즐거운 코딩하세요!

감사합니다😊

<div class="content-ad"></div>

더 많은 업데이트를 보시려면 Swiftfy를 팔로우해 주세요!

추가 정보는 다음을 참조해 주세요: