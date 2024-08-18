---
title: "FastAPI 동기적 병목 현상 해결하기"
description: ""
coverImage: "/assets/img/2024-08-19-AsyncAwaitIsntEnoughSolvingSynchronousBottlenecksinFastAPI_0.png"
date: 2024-08-19 02:02
ogImage: 
  url: /assets/img/2024-08-19-AsyncAwaitIsntEnoughSolvingSynchronousBottlenecksinFastAPI_0.png
tag: Tech
originalTitle: "Async Await Isnt Enough Solving Synchronous Bottlenecks in FastAPI"
link: "https://medium.com/@aryanrot234/async-await-isnt-enough-solving-synchronous-bottlenecks-in-fastapi-6f9f152256a2"
isUpdated: false
---



![Async/await is great for handling I/O-bound operations, promising smoother and more responsive applications. But what happens when it doesn’t work as expected? In my FastAPI project, I ran into a situation where async/await wasn’t enough, causing delays and pauses in my services. Here’s how I fixed it.](/assets/img/2024-08-19-AsyncAwaitIsntEnoughSolvingSynchronousBottlenecksinFastAPI_0.png)

## Where Async Falls Short:

I had multiple APIs in my FastAPI application. Some of them required heavy processing, and despite making the main API functions async, the app still paused when these heavy tasks ran. The issue? While the main function was async, the helper functions were still synchronous, blocking other tasks from running.


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
async def main_api_function():
  helper_function_1()
  helper_function_2()

def helper_function_1():
  #heavy processing
  return

def helper_function_2():
  #heavy processing
  return
```

여기서 문제가 발생합니다:

비동기 함수를 호출할 때 보조 코드도 비동기여야 합니다. 이 요구 사항은 코드베이스 전반에 영향을 미치며, 해당 비동기 함수를 의존하는 모든 함수가 비동기여야 하며, 최상위 수준까지 계속됩니다. 그러나 모든 것을 비동기로 만들어도 함수 자체가 동기적으로 CPU 집중적인 작업을 수행한다면 도움이 되지 않습니다. 이는 병목 현상을 유발하여 응용 프로그램의 반응성을 떨어뜨릴 수 있습니다.

하지만 이 문서를 읽고 계신다면 이미 수많은 API 및 동기적으로 작동할 수도 있는 보조 함수에 빠져 계실텐데, 모든 주요 함수 및 보조 함수를 비동기로 변경하고 문제를 해결하는지 테스트하는 것이 부담스러울 수 있습니다.


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

## 해결책: ThreadPoolExecutor를 사용하여 무거운 작업 오프로딩

이 문제를 해결하기 위해, CPU 바인딩 작업을 주 스레드를 차단하지 않고 처리할 방법이 필요했습니다. 그 해결책은 무엇일까요? ThreadPoolExecutor를 사용하여 이러한 무거운 작업을 별도 스레드에서 실행하면서 주 이벤트 루프를 다른 작업에 대해 자유롭게 유지하는 것입니다.

여기에 내가 취한 방법입니다:

- 스레드 풀 생성: 먼저 ThreadPoolExecutor로 스레드 풀을 설정했습니다. 이는 병렬로 작업을 처리할 수 있는 작업자 그룹을 갖고 있는 것과 같습니다. 각 작업자는 무거운 작업을 수행할 수 있고, 이는 주 스레드를 다른 요청 처리를 계속할 수 있도록 해줍니다.

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
from concurrent.futures import ThreadPoolExecutor
import asyncio

executor = ThreadPoolExecutor(max_workers=10) # 서버 설정에 따라 변경 가능

```

2. 별도의 스레드에서 차단 코드 실행: 저는 run_blocking_code_in_executor라는 비동기 함수를 작성했습니다. 이 함수는 블로킹 작업을 실행하기 위해 loop.run_in_executor를 사용합니다. 이 함수는 블로킹 함수와 해당 인수를 가져와 별도의 스레드에서 실행하고 결과를 반환합니다.

```js
async def run_blocking_code_in_executor(blocking_function, *args):
    loop = asyncio.get_running_loop()
    return await loop.run_in_executor(executor, blocking_function, *args)
```

3. 솔루션 적용하기: CPU 집약 작업인 중량 계산과 같은 작업을 시뮬레이션하는 generate_report라는 함수가 있다고 가정해보겠습니다. 이를 오프로드하는 방법은 다음과 같습니다:

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
def generate_report(data):
    # CPU-bound 작업을 시뮬레이션합니다.
    new_report = create_report(data)
    return new_report

result = await run_blocking_code_in_executor(generate_report, data)
```

이 예제에서 generate_report는 메인 쓰레드를 차단하는 작업을 나타냅니다. 별도의 쓰레드로 이동하여 주요 API 및 다른 API 함수가 멈추지 않고 계속 실행되도록하면 응용 프로그램의 반응성이 향상됩니다.

즉, 동기 함수를 해당 매개변수와 함께 쓰레드에서 실행하기 위해 쓰레드 실행자 함수로 래핑하는 것과 같습니다.

## 결론:

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

비동기/대기(async/await)만으로는 항상 충분하지 않을 수 있습니다, 특히 CPU 집약 작업을 다룰 때. 비동기/대기(async/await)를 스레딩과 결합하면 FastAPI 애플리케이션을 원활하게 유지하고 동기화 병목 현상을 피할 수 있습니다. 비슷한 문제에 직면하고 있다면, 이 접근 방식을 사용하여 서비스를 반응적으로 유지하는 것을 고려해보세요.