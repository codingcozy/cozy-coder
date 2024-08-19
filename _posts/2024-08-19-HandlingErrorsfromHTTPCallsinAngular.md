---
title: "Angular에서 HTTP 호출로 발생하는 오류 처리 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 02:08
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Handling Errors from HTTP Calls in Angular"
link: "https://medium.com/@paul-chesa/handling-errors-from-http-calls-in-angular-4dbc7f6b26ca"
isUpdated: true
updatedAt: 1724033290935
---



![Image](https://miro.medium.com/v2/resize:fit:996/1*YgzNyRixtIqcJTCD0XYFLg.gif)

Angular 애플리케이션을 개발할 때, HTTP 요청을 사용해 백엔드 서버와 통신하는 것이 중요합니다. 그러나 때때로 문제가 발생할 수 있습니다. 마치 배송 서비스를 운영하는 것과 비슷하게요. 고객에게 소포를 보내는 책임을 맡아보세요. 때로는 소포가 고객에게 도착하지 않거나 손상되어 도착하기도 합니다. 이는 HTTP 요청이 실패하는 경우와 비슷합니다. Angular에서 이러한 오류를 어떻게 처리하는지 배달 서비스 비유를 통해 알아봅시다. 🚀

![Image](https://miro.medium.com/v2/resize:fit:398/1*pLQ9lW0LfXYbWVx9j8Tj1w.gif)

# 두 가지 유형의 배송 문제


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

배송 서비스와 마찬가지로, Angular 앱이 HTTP 요청을 보낼 때 두 가지 주요 문제가 발생할 수 있어요:

창고에서 상품이 나오지 않음 (네트워크나 연결 오류🛑):

가끔 배송 트럭이 고장이 나거나 교통체증으로 인해 상품이 창고를 떠날 수 없는 상황이 발생할 수 있어요. 이것은 HTTP 요청이 서버에 도달하지 않는 네트워크 오류처럼 동작해요. Angular에서 이러한 오류는 상태 코드가 0이며, 아무것도 전달되지 않았다는 것처럼 빈 영수증을 받는 것과 같아요.

상품은 배송되었지만 문제가 발생함 (백엔드 오류) ⚠️:

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

그 외에도 때로는 소포가 고객에게 도착하지만 손상되었거나 잘못된 상품일 수 있습니다. 이것은 서버가 요청을 받았지만 처리 중에 문제가 발생한 상황과 유사합니다. 서버는 특정 상태 코드가 포함된 오류 메시지를 보내는데, 예를 들어 "404" (아이템을 찾을 수 없음) 또는 "500" (서버 오류)와 같이 특정 상태 코드가 있습니다.

# 이러한 문제 해결 방법 💼

배송 서비스에서 이러한 문제를 해결하는 다양한 전략이 있을 것입니다. 마찬가지로 Angular는 HTTP 요청 오류를 효과적으로 처리할 수 있는 도구를 제공합니다.

## 1. 손님에게 도달하기 전 오류 잡기 (catchError) 🎣

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

품질 관리 팀이 각 패키지를 발송 전에 확인하는 상황을 상상해보세요. 문제가 발견되면 고객에게 보내지 않고 문제를 해결하거나 사과 메모를 보냅니다. Angular에서는 RxJS의 catchError 연산자를 사용하는 것과 비슷합니다.

다음은 작동 방식입니다:

```js
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

this.httpClient.get('https://api.example.com/data')
  .pipe(
    catchError(error => {
      // 품질 관리가 문제를 감지합니다
      console.error('배송 문제 발생:', error);
      // 사과 메모를 보내거나 문제를 해결합니다
      return throwError(() => new Error('이런! 문제가 발생했습니다. 나중에 다시 시도해주세요.'));
    })
  )
  .subscribe(data => {
    // 성공적인 배송
  });
```

요청 중에 문제가 발생하면(clobbered package와 같은), catchError 연산자가 이를 잡아냅니다. 고객이 손상된 패키지를 받는 대신, "죄송합니다, 문제가 발생했습니다!" 라는 친절한 메시지를 받게 됩니다. 😅

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

## 2. 배송 재시도 (retry) 🔄

가끔 배송 트럭이 교통 체증으로 막혀 있을 수 있지만, 다시 보내면 고객에게 잘 도착할 수도 있습니다. Angular에서는 실패한 경우 자동으로 요청을 다시 보내려면 retry 연산자를 사용할 수 있습니다.

다음은 실행 방법입니다:

```js
import { retry } from 'rxjs/operators';

this.httpClient.get('https://api.example.com/data')
  .pipe(
    retry(3), // 배송을 최대 3회 시도
    catchError(error => {
      console.error('재시도 후 배송 문제 발생:', error);
      return throwError(() => new Error('죄송합니다, 요청을 완료할 수 없습니다. 나중에 다시 시도해주세요.'));
    })
  )
  .subscribe(data => {
    // 배송 성공
  });
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

만약 요청이 일시적인 문제(예: 교통 🚦) 때문에 실패한다면, 재시도가 되면 "배송 트럭"을 다시 보냅니다. 몇 번의 시도 끝에도 전달되지 못하면 사과를 보냅니다. 📧

# 마무리하기 🎁

Angular에서 HTTP 오류를 처리하는 것은 성공적인 배송 서비스 운영과 유사합니다. 패키지가 분실되었을 때(네트워크 오류)나 손상되어 도착했을 때(백엔드 오류)도 대비할 준비를 해야 합니다. catchError와 retry와 같은 도구를 사용하여 "고객" (사용자)이 계획대로 진행되지 않을 때에도 원활한 경험을 제공할 수 있습니다. 좋은 배송 서비스처럼 데이터가 안전하고 제 시간에 도착하도록 또는 뭔가 잘못되면 탄탄한 백업 계획이 있는지 확인하고 싶습니다. 🌟

이러한 전략을 이해하고 구현함으로써, Angular 앱을 원활하게 유지하고 매번 훌륭한 사용자 경험을 제공할 수 있습니다! 🚀