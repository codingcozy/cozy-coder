---
title: "파이썬에서 gRPC Interceptor 활용하기"
description: ""
coverImage: "/assets/img/2024-08-19-gRPCInterceptorsinPython_0.png"
date: 2024-08-19 02:00
ogImage: 
  url: /assets/img/2024-08-19-gRPCInterceptorsinPython_0.png
tag: Tech
originalTitle: "gRPC Interceptors in Python"
link: "https://medium.com/taranis-ag/grpc-interceptors-in-python-267402b5e77e"
isUpdated: false
---



![그림](/assets/img/2024-08-19-gRPCInterceptorsinPython_0.png)

# 간단한 소개 (건너뛸 수 있음)

다른 많은 스타트업들과 마찬가지로, 규모가 커지면서 단일체(monolith)에서 마이크로서비스 아키텍처로 전환해야 했습니다. 우리는 gRPC를 주요 프로토콜로 선택했는데, 높은 성능, 강력한 유형의 계약, 여러 언어 지원, 비동기 지원, 그리고 양방향 스트리밍을 좋아해서 선택했습니다 (사실 파이썬의 특정 성능 문제 때문에 이후에 양방향 스트리밍을 사용하지 않았지만... 이에 대해서는 나중에 다시 언급하겠습니다).

파이썬을 주요 코딩 언어로 삼은 우리는 서로 다른 gRPC 서비스 간의 내부 통신을 형식화하고, 모든 RPC 메서드에 적용될 동작을 구현해야 했습니다. 이 때 우리는 gRPC 인터셉터를 마이크로서비스 생태계에 도입했습니다.


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

이 글에서는 gRPC 인터셉터가 무엇이며 간단한 예제를 사용하여 동기 및 비동기 구현으로 어떻게 구현하는지에 대해 살펴볼 것입니다. 이 글의 끝에는 사용하고 코드를 확인할 수 있는 모니터링 인터셉터의 실제 예제도 얻을 수 있을 것입니다.

# 인터셉터란?

아마도 이미 미들웨어의 개념에 익숙할 것입니다. 두 지점 사이에 사용자 정의 기능의 "다리"를 추가하는 것입니다.

gRPC 인터셉터는 단지 그것뿐이지만 gRPC를 위해입니다. 클라이언트와 서버에서 요청과 응답을 가로채고 추가 동작 및 조작을 추가할 수 있습니다. 여러 인터셉터를 구성할 수 있으며 순서대로 실행됩니다. 인터셉터의 일반적인 사용 사례는 인증, 권한 부여, 로깅, 오류 처리 및 메트릭스가 있습니다.

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

gRPC에는 2가지 구현 방식이 있습니다. 동기(sync)와 비동기(async - asyncio 사용). 그에 따라 인터셉터도 클라이언트 및 서버의 동기 및 비동기 클라이언트 및 서버를 위해 2가지 다른 구현 방식이 있습니다.

# 파이썬에서 인터셉터 구현하기

인터셉터에는 채널(Channel)과 서버(Server) 두 가지 유형이 있습니다. 이 두 유형은 구현 위치에 따라 다릅니다:

- 채널 인터셉터는 클라이언트 측에 있으며 클라이언트가 수행하는 각 호출을 가로챕니다.
- 서버 인터셉터는 서버 측에 있으며 서버가 받는 각 호출을 가로챕니다.

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

다음은 요청과 응답의 경로에 있는 채널 및 서버 인터셉터의 배치를 대략적으로 보여주는 다이어그램이 있어요:

![gRPC Interceptors in Python](/assets/img/2024-08-19-gRPCInterceptorsinPython_1.png)

이제 구현하는 방법을 살펴볼까요?

간단한 재시도 인터셉터를 구현할 건데요, gRPC 오류가 발생했을 때 지수적으로 대기하고 지정된 회수 이후에 중지하는 방식입니다. 대기 시간을 관리하기 위해 다음 코드를 사용할 거예요:

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

```js
import asyncio
import grpc
import time


class WaitExponential:
    def __init__(self, multiplier, min, max):
        self.multiplier = multiplier
        self.min = min
        self.max = max

    def calculate_exponential_waiting_time(self, attempt: int) -> int:
        exp = 2 ** attempt
        result = self.multiplier * exp

        return max(0, self.min, min(result, self.max))

    def wait(self, seconds: int):
        time.sleep(seconds)

    async def wait_async(self, seconds: int):
        await asyncio.sleep(seconds)


class InterceptorRetry:
    def __init__(self, wait: WaitExponential, stop: int):
        self.wait = wait
        self.stop = stop
```

WaitExponential 클래스의 calculate_exponential_waiting_time은 시도 횟수에 따라 기다려야 하는 시간을 반환하며, wait 함수는 실제 대기를 수행하고, wait_async는 비동기 구현을 위해 동일한 작업을 수행합니다. InterceptorRetry 클래스는 인터셉터 초기화를 돕습니다.

## 채널 인터셉터

인터셉터에 대한 완전한 코드는 여기에 있습니다. 걱정 마세요, 이것이 무엇을 하는지 곧 설명하겠습니다:

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

```python
import grpc
import InterceptorRetry

class RetryOnErrorClientInterceptor(grpc.UnaryUnaryClientInterceptor,
                                    grpc.StreamUnaryClientInterceptor,
                                    grpc.UnaryStreamClientInterceptor,
                                    grpc.StreamStreamClientInterceptor):
    def __init__(self, interceptor_retry: InterceptorRetry):
        self.waiting = interceptor_retry.wait
        self.stop = interceptor_retry.stop

    def _intercept_call(self, continuation, client_call_details, request_or_iterator):
        response = continuation(client_call_details, request_or_iterator)

        for retry_attempt in range(0, self.stop - 1):
            if isinstance(response, Exception):
                waiting_time = self.waiting.calculate_exponential_waiting_time(
                    retry_attempt)

                self.waiting.wait(seconds=waiting_time)

                response = continuation(client_call_details, 
                    request_or_iterator)

        return response

    def intercept_unary_unary(self, continuation, client_call_details, request):
        return self._intercept_call(continuation, client_call_details, request)
    
    def intercept_stream_unary(self, continuation, client_call_details, request_iterator):
        return self._intercept_call(continuation, client_call_details, request_iterator)

    def intercept_unary_stream(self, continuation, client_call_details, request):
        return self._intercept_call(continuation, client_call_details, request)

    def intercept_stream_stream(self, continuation, client_call_details, request_iterator):
        return self._intercept_call(continuation, client_call_details, request_iterator)
```

채널 인터셉터를 사용하면 가능한 옵션 중 원하는 호출 유형을 명시적으로 인터셉트해야 합니다: unary-unary, stream-unary, unary-stream, stream-stream. 

저희의 인터셉터는 모든 가능한 호출을 인터셉트하길 원하기 때문에 RetryOnErrorClientInterceptor는 네 개의 gRPC 클라이언트 인터셉터 인터페이스(UnaryUnaryClientInterceptor, StreamUnaryClientInterceptor, UnaryStreamClientInterceptor, StreamStreamClientInterceptor)를 모두 구현합니다. 네 개의 인터페이스를 통해 구현해야 할 네 가지 인터셉터 메서드인 intercept_unary_unary, intercept_stream_unary, intercept_unary_stream, intercept_stream_stream이 제공됩니다. 

각 인터셉터 메서드는 다음과 같은 인자를 받습니다:

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

- continuation — RPC 호출을 계속하고 서버로 호출을 합니다.
- client_call_details — RPC 호출에 대한 메타데이터를 보유하며, 이는 서버로 보내기 전에 검사하거나 조작할 수 있습니다.
- request/request_iterator — 클라이언트로부터 받은 요청으로, 클라이언트의 요청이 Unary인지 Streaming인지에 따라 달라집니다.

RetryOnErrorClientInterceptor는 초기화될 때 interceptor_retry에 InterceptorRetry 유형의 재시도 구성을 가져옵니다.

각 Interceptor 메서드에서 _intercept_call을 호출하는데, 이는 호출을 가로채는 데 사용하는 구현을 보유합니다. 먼저, continuation을 사용하여 서버 호출을 수행하고 응답을 받은 후, interceptor_retry의 stop count 범위에서 루프로 감싸서 응답이 예외인지 확인하고, 그 경우 시도에 따라 대기 시간을 계산하고 호출을 다시 시도합니다. 호출이 성공하거나 모든 시도가 실패할 때까지, 응답은 Interceptor에서 반환됩니다.

채널 가로채기:

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

```js
interceptor_retry = InterceptorRetry(wait=WaitExponential(multiplier=0.5, min=0.5, max=3),
                                     stop=5)
interceptor = RetryOnRpcErrorClientInterceptor(interceptor_retry)

channel = grpc.insecure_channel('localhost:50051')
intercept_channel = grpc.intercept_channel(channel, interceptor)
```

우리는 InterceptorRetry를 사용하여 RetryOnRPCErrorClientInterceptor를 초기화하고 gRPC 채널을 받은 후 intercept_channel을 호출하여 interceptor로 채널을 중간에 가로챕니다.

비동기 interceptor 버전:

```js
import grpc
from grpc import aio
import InterceptorRetry


class RetryOnErrorClientInterceptorAsync(aio.UnaryUnaryClientInterceptor,
                                         aio.StreamUnaryClientInterceptor,
                                         aio.UnaryStreamClientInterceptor,
                                         aio.StreamStreamClientInterceptor):
    def __init__(self, interceptor_retry: InterceptorRetry):
        self.waiting = interceptor_retry.wait
        self.stop = interceptor_retry.stop

    async def _intercept_call(self, continuation, client_call_details, request_or_iterator):
        response = await continuation(client_call_details, request_or_iterator)

        for retry_attempt in range(self.stop):
            try:
                await response.wait_for_connection()
                return response
            except grpc.RpcError as error:
                waiting_time = self.waiting.calculate_exponential_waiting_time(
                    retry_attempt)

                await self.waiting.wait_async(seconds=waiting_time)

                response = await continuation(client_call_details, request_or_iterator)

        return response
    
    def intercept_unary_unary(self, continuation, client_call_details, request):
        return self._intercept_call(continuation, client_call_details, request)

    def intercept_stream_unary(self, continuation, client_call_details, request_iterator):
        return self._intercept_call(continuation, client_call_details, request_iterator)

    def intercept_unary_stream(self, continuation, client_call_details, request):
        return self._intercept_call(continuation, client_call_details, request)

    def intercept_stream_stream(self, continuation, client_call_details, request_iterator):
        return self._intercept_call(continuation, client_call_details, request_iterator)
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

RetryOnErrorClientInterceptor 이후에 이 내용은 익숙할 것입니다. 개념은 같습니다. RetryOnErrorClientInterceptorAsync는 네 개의 gRPC 클라이언트 인터셉터 인터페이스를 구현합니다. 다만, 이번에는 grpc.aio에서 가져옵니다.

_intercept_call도 약간 다릅니다. 이제는 continuation에서 응답을 기다려야 하며, response.wait_for_connection()을 호출하여 RPC가 성공적으로 연결되었는지 확인해야 합니다. 그렇지 않은 경우 예외가 발생하며, 이를 response 유형을 확인하는 대신 except로 캐치할 수 있습니다. 또한 InterceptorRetry의 self.waiting.wait_async로 비동기적으로 대기합니다.

채널을 가로지르는 비동기 버전:

```js
channel = grpc.aio.insecure_channel('localhost:50051', interceptors=[interceptor])
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

인터셉터 초기화는 단항 인터셉터와 동일합니다. 차이는 채널 생성 방법에 있습니다. 비동기 채널에서는 grpc.aio.insecure_channel로 채널을 만들 때 인터셉터를 구성할 수 있습니다.

## 서버 인터셉터

서버 인터셉터 구현은 더 간단합니다. 저희 인터셉터는 ServerInterceptor 인터페이스를 구현하고 intercept_service 메서드를 구현해야 합니다. 호출이 가로챌 수 있도록 handler_call_details와 함께 continuation을 호출하는 것으로 RPC 서버 호출이 이루어집니다. 이미 재시도 코드를 래핑하는 방법을 알고 계시므로 중요한 부분만 보여드리겠습니다:

```js
import grpc
from grpc import ServerInterceptor

class RetryOnErrorServerInterceptor(ServerInterceptor):
    def __init__(self, interceptor_retry: InterceptorRetry):
        ...

    async def intercept_service(self, continuation, handler_call_details):
        # 재시도하는 중...
        response = continuation(handler_call_details)
        # 재시도하는 중...

        return response
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

서버를 가로채보겠습니다:

```js
interceptor_retry = InterceptorRetry(wait=WaitExponential(multiplier=0.5, min=0.5, max=3),
                                     stop=5)
interceptor = RetryOnRpcErrorClientInterceptor(interceptor_retry)

server = grpc.server(futures.ThreadPoolExecutor(),interceptors=(interceptor))
```

비동기 서버 인터셉터:

비동기에서는 grpc.aio에서 ServerInterceptor를 얻어서 다음과 같이 사용할 것입니다:

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

```js
response = await continuation(handler_call_details)
```

비동기 서버를 가로채기:

```js
interceptor_retry = InterceptorRetry(wait=WaitExponential(multiplier=0.5, min=0.5, max=3),
                                     stop=5)
interceptor = RetryOnRpcErrorClientInterceptor(interceptor_retry)

server = grpc.aio.server(interceptors=[interceptor])
```

# 실제 예제


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

간단한 예제는 배우는 데 좋지만, 실제로 운영 가능한 코드를 볼 수 있는 실제 사례가 있으면 더 좋죠. 걱정 마세요! 저희는 Python async gRPC 서비스를 모니터링하는 인터셉터를 Prometheus를 사용하여 만들었습니다. 여기에서 확인하실 수 있어요: https://github.com/taranisag/py-async-grpc-prometheus. 또한 이 인터셉터를 서비스에 사용하는 방법에 대한 글도 작성했어요.

간단한 재시도 예제와 함께 동기 및 비동기 채널 및 서버 인터셉터를 구현하는 방법과, 모니터링을 위한 복잡한 gRPC 서비스 인터셉터의 실제 사용 사례를 보았습니다. 이를 확인하고 활용해보세요.

읽어 주셔서 감사합니다. 본문이 gRPC 작업에 유용하게 사용되었으면 좋겠습니다!