---
title: "예외를 사용하지 마세요 대신 이것을 시도해 보세요 "
description: ""
coverImage: "/assets/img/2024-07-24-StopUsingExceptionsTryThisInstead_0.png"
date: 2024-07-24 11:57
ogImage: 
  url: /assets/img/2024-07-24-StopUsingExceptionsTryThisInstead_0.png
tag: Tech
originalTitle: "Stop Using Exceptions Try This Instead "
link: "https://medium.com/@kmorpex/stop-using-exceptions-try-this-instead-a35c0f4d30ad"
---


## 개발 팁

![image](/assets/img/2024-07-24-StopUsingExceptionsTryThisInstead_0.png)

# 문제 설명

예외는 일반적으로 앱이 회복할 수 없는 상태에 직면했을 때와 같은 예상치 못한 상황을 처리하기 위한 것입니다. 그러나 개발자가 프로그램 흐름을 제어하기 위해 사용할 때 발생하는 문제는 성능에 심각한 영향을 미칠 수 있습니다. 예외의 잘못된 사용은 코드를 복잡하게 만들고 앱의 실행 속도를 늦출 수 있습니다.

<div class="content-ad"></div>

예외를 흐름 제어에 사용하는 것이 나쁜 이유에 대해 이야기해봐요:

- 성능 저하: 예외 처리는 많은 리소스를 사용하기 때문에 실행 시간을 늦춥니다.
- 코드 가독성: 예외를 너무 많이 사용하면 코드를 이해하고 유지하기 어렵게 만들 수 있어요.
- 오류 처리 복잡성: 흐름 제어에 예외를 사용하면 오류 처리가 더 복잡해지며 디버깅 및 오류 관리가 어려워져요.

구독 서비스에서의 예시를 보여드릴게요:

![SubscriptionService 예시](/assets/img/2024-07-24-StopUsingExceptionsTryThisInstead_1.png)

<div class="content-ad"></div>

# 해결 방법

이 문제를 해결하기 위해, 우리 앱에서 에러를 처리하는 더 나은 방법을 찾아야 합니다. 좋은 옵션 중 하나는 결과 패턴(Result Pattern)인데, 이를 통해 에러를 더 부드럽고 효과적으로 관리할 수 있습니다. 이 방법은 예외를 던지는 대신에 성공 또는 에러가 될 수 있는 결과를 가진 객체를 반환하는 것을 포함합니다.

## 결과 패턴 구현

먼저 해야 할 일은 애플리케이션 에러를 처리하는 Error 클래스입니다.
코드 — 앱에서 에러의 고유한 이름.
설명 — 개발자 친화적인 에러 정보.

<div class="content-ad"></div>

```csharp
public sealed record Error(string Code, string Name)
{
    public static readonly Error None = new(string.Empty, string.Empty);
}
```

그런 다음, 값과 오류를 포장하여 실패를 설명하는 Result`T` 클래스를 만들 수 있습니다. 이것은 꽤 간단한 구현이지만 다양한 추가 기능을 추가할 수 있습니다.

```csharp
public class Result
{
    public Error Error { get; }
    public bool IsSuccess { get; }
    public bool IsFailure => !IsSuccess;

    private Result(bool isSuccess, Error error)
    {
        if (isSuccess && error != Error.None) 
            throw new InvalidOperationException();
        if (!isSuccess && error == Error.None) 
            throw new InvalidOperationException();

        IsSuccess = isSuccess;
        Error = error;
    }

    public static Result Success() 
        => new Result(true, Error.None);

    public static Result Failure(Error error) 
        => new Result(false, error);
}
```

Result 인스턴스를 만들려면 정적 메서드를 사용해야 합니다:
Success - 성공적인 결과 생성
Failure - 지정된 Error로 실패 결과 생성하기


<div class="content-ad"></div>

만약 직접 Result 클래스를 만드는 것이 귀찮다면 FluentResults 라이브러리를 확인해보세요.

## 결과 패턴 적용하기

이제 Result 클래스가 준비되었으니, 실제 상황에서 어떻게 사용할 수 있는지 살펴봅시다.

SubscriptionService의 리팩토링된 버전이 여기 있습니다. 몇 가지 사항을 주목해 보세요:

<div class="content-ad"></div>

- 더 이상 예외를 던지지 않습니다
- 결과 반환 형식이 명시적입니다
- 메서드가 반환하는 오류가 명확합니다

오류 처리에 Result 패턴을 사용하는 또 다른 장점은 테스트를 수월하게 할 수 있다는 것입니다.

![이미지](/assets/img/2024-07-24-StopUsingExceptionsTryThisInstead_2.png)

## 앱 오류 문서화

<div class="content-ad"></div>

앱에서 발생 가능한 모든 오류를 문서화하고자 한다면 Error 클래스를 사용할 수 있습니다. 한 가지 방법은 static Errors 클래스를 생성하는 것입니다. 이 클래스에는 특정 오류 유형을 가진 중첩 클래스가 포함될 것입니다.

사용 방법은 Errors.Subscriptions.PrivateChannel과 같이 될 것입니다. 그러나 저는 다른 접근 방식을 선호하여 오류에 대한 전용 클래스를 만들었습니다.

다음은 Subscription 엔티티에 대한 가능한 오류를 문서화하는 SubscriptionErrors 클래스입니다:

```js
public static class SubscriptionErrors
{
    public static readonly Error PrivateChannel = new Error(
        "Subscriptions.PrivateChannel", "Can't subscribe private channel");

    public static readonly Error AlreadySubscribing = new Error(
        "Subscriptions.AlreadySubscribing", "Already subscribing");
}
```

<div class="content-ad"></div>

정적 필드를 사용하는 대신에 오류를 반환하는 정적 메서드를 사용할 수도 있습니다. 이러한 메서드를 사용하려면 오류 인스턴스를 얻기 위해 특정 인수를 전달해야 합니다.

```js
public static class SubscriptionErrors
{
    public static Error NotFound(Guid id) => new Error(
        "Subscriptions.NotFound", $"The subscription with Id '{id}' was not found");
}
```

## API 응답으로 결과 변환하기

결과 객체는 최종적으로 ASP.NET Core의 Minimal API(또는 컨트롤러)에 도달할 것입니다. Minimal API는 IResult 응답을 반환하고, 컨트롤러는 IActionResult 응답을 반환합니다. 그러나 어떤 경우에도 결과 객체를 적절한 API 응답으로 변환해야 합니다.

<div class="content-ad"></div>

더 효율적인 접근법을 취하는 좋은 방법이 있어요. 각 결과 상태에 도달할 때 콜백을 호출하도록 Match 확장 메서드를 사용할 수 있어요. 그러면 Match 메서드가 관련 있는 콜백을 실행하고 결과를 제공할 거예요.

```js
public static class ResultExtensions
{
    public static T Match<T>(
        this Result result,
        Func<T> onSuccess,
        Func<Error, T> onFailure)
    {
        return result.IsSuccess ? onSuccess() : onFailure(result.Error);
    }
}
```

그리고 이렇게 Match 메서드를 최소한으로 사용하는 API 엔드포인트를 보여줄게요:

```js
app.MapPost(
    "users/{userId}/subscribe/{channelId}",
    (Guid userId, Guid channelId, SubscriptionService subscriptionService) =>
    {
        var result = await subscriptionService.StartSubscribingAsync(
            userId,
            channelId);

        return result.Match(
            onSuccess: () => Results.NoContent(),
            onFailure: error => Results.BadRequest(error));
    });
```

<div class="content-ad"></div>

# 결론

예외 처리는 확실히 비용이 많이 들며, 제어 흐름에 사용하면 주요 성능 문제를 일으킬 수 있습니다. 그러나 결과 패턴을 사용하면 오류를 더 효과적으로 처리하고 코드를 더 명확하게 만들며 앱의 성능을 향상시킬 수 있습니다. 이것이 소프트웨어 개발의 현대적 접근 방식입니다. 이를 통해 앱을 보다 견고하고 빠르게 만들 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/0*lKq48DXVBjSwagaB.gif)

👏 만약 이 콘텐츠가 도움이 되었다면, 클랩(clap) 버튼을 눌러주세요 (버튼을 누른 채로 유지하면 여러 번 클릭할 수 있습니다). 또한, 여러분의 생각과 제안을 댓글에 남겨주시기를 권장합니다. 함께 이 주제를 논의해 나가도록 합시다.

<div class="content-ad"></div>

안녕하세요! 읽어주셔서 감사합니다.