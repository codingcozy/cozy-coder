---
title: "iOS 면접 가이드 Swift에서 URLSession 작업의 오류 처리 방법"
description: ""
coverImage: "/assets/img/2024-08-18-iOSInterviewGuideErrorHandlingwithURLSessionTasksinSwift_0.png"
date: 2024-08-18 10:51
ogImage: 
  url: /assets/img/2024-08-18-iOSInterviewGuideErrorHandlingwithURLSessionTasksinSwift_0.png
tag: Tech
originalTitle: "iOS Interview Guide Error Handling with URLSession Tasks in Swift"
link: "https://medium.com/swiftable/ios-interview-guide-error-handling-with-urlsession-tasks-in-swift-0055a377ab23"
isUpdated: false
---



![image](/assets/img/2024-08-18-iOSInterviewGuideErrorHandlingwithURLSessionTasksinSwift_0.png)

Level: 중급, 우선순위: 중

## Q. URLSession 작업에서 오류와 응답을 어떻게 처리하나요?

URLSession 작업에서 오류와 응답을 처리하는 것은 견고한 네트워킹 코드를 위해 중요합니다. URLSession은 성공적인 응답과 오류를 우아하게 처리할 수 있는 메커니즘을 제공합니다.


<div class="content-ad"></div>

데이터 작업을 다룰 때는 일반적으로 완료 핸들러(completion handler)를 사용하여 데이터와 오류를 처리합니다. Result 타입을 사용하는 completionHandler 접근 방식은 비동기 작업에 대한 완료 핸들러를 사용하면서 오류 처리에 대한 유연성과 명확성을 제공합니다. 이 접근 방식을 통해 오류를 보다 구조화되고 간결하게 처리할 수 있습니다.

URLSession을 사용하여 네트워크 요청을 수행하고 완료 핸들러를 Result 타입으로 사용하여 응답을 처리하는 방법을 살펴봅시다. 아래에서 한 단계씩 쪼개어 보겠습니다:

## 네트워크 오류 관리

NetworkError 열거형을 정의하여 다음과 같이 Error 프로토콜을 준수하는 것이 좋은 방법입니다:

<div class="content-ad"></div>

```swift
enum NetworkError: Error {
    case invalidData
    case invalidJSON
    case invalidResponse
}
```

네트워크 작업 중 발생할 수 있는 다양한 유형의 오류에 대한 경우를 포함하고 있습니다: invalidData, invalidJSON 및 invalidResponse입니다. 이러한 경우들은 오류를 분류하고 효율적으로 처리하는데 도움이 됩니다. 또한 필요에 따라 더 많은 오류 케이스를 추가할 수 있습니다.

오류 메시지의 경우, 특정 경우와 연결된 값을 사용하여 enum의 경우를 정의하여 해당 경우의 오류 메시지를 제공할 수 있습니다.

## 요청 실행하기

<div class="content-ad"></div>

URLRequest와 옵셔널 완료 핸들러를 파라미터로 사용하는 함수를 가정해봅시다. 이 함수는 URLSession을 사용하여 네트워크 요청을 수행하는 데이터 작업을 생성합니다. 예를 들어:

```js
func executeRequest(request: URLRequest, completion: ((Result<[String: Any], NetworkError>) -> ())?) {
    let dataTask = URLSession.shared.dataTask(with: request) { (data, response, error) in

        guard let data = data else {
            completion?(.failure(.invalidData))
            return
        }

        do {
            let responseJSON = try JSONSerialization.jsonObject(with: data, options: .allowFragments)
            if let responseData = responseJSON as? [String : Any] {
                completion?(.success(responseData))
            } else {
                completion?(.failure(.invalidResponse))
            }
        } catch let error {
            completion?(.failure(.invalidJSON))
        }
    }
    dataTask.resume()
}
```

데이터 작업의 완료 핸들러 내에서 잠재적인 오류를 확인합니다:

- 데이터가 받아지지 않은 경우 (data가 nil인 경우), .invalidData 에러 케이스가 포함된 실패 결과를 가진 완료 핸들러를 호출합니다.
- 데이터가 있는 경우, JSONSerialization을 사용하여 JSON을 직렬화하려고 시도합니다.
- 직렬화가 성공하고 JSON 데이터가 예상 형식 ([String: Any])으로 있는 경우, JSON 데이터가 포함된 성공 결과를 가진 완료 핸들러를 호출합니다.
- JSON 데이터가 예상 형식으로 되어 있지 않는 경우, .invalidResponse 에러 케이스가 포함된 실패 결과를 가진 완료 핸들러를 호출합니다.
- JSON 직렬화 중에 오류가 발생한 경우, .invalidJSON 에러 케이스가 포함된 실패 결과를 가진 완료 핸들러를 호출합니다.

<div class="content-ad"></div>

참고: 완료 핸들러 내에서 오류 및 응답 처리는 API의 구조에 따라 다를 수 있습니다.

이 함수를 실행하여 요청을 보내려면 다음과 같이 호출할 수 있습니다:

```js
if let url = URL(string: "https://www.example.com/sample_data") {
    let urlRequest = URLRequest(url: url)
    executeRequest(request: urlRequest) { result in
        switch result {
            case .success(let response): print("성공 응답")
            case .failure(let error): print("문제가 발생했습니다: \(error)")
        }
    }
}
```

클로저 내에서 받은 결과에 대해 switch 문을 사용합니다:

<div class="content-ad"></div>

- 성공한 경우 결과를 처리하여 추가 작업을 수행합니다.
- 실패한 경우 에러를 처리하여 해당 경우를 관리합니다.

이 방법을 사용하면 URLSession 작업에서 오류와 응답을 효과적으로 처리하여 앱이 다양한 네트워킹 시나리오에서 적절하게 동작하도록 할 수 있습니다. 오류를 정상적으로 처리하여 사용자 경험을 향상시키고 개발 중 효과적으로 문제 해결할 수 있도록 합니다.

오늘 바로 내용을 확인하고 iOS 인터뷰 마스터로의 여정에 떠나보세요! iOS Interview Handbook (할인 코드: SWIFTABLE_HANDBOOK).

계속 배우고, 계속 준비해보세요!

<div class="content-ad"></div>

안녕하세요, Nitin A님!