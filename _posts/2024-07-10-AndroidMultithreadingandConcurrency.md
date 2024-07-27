---
title: "안드로이드 멀티스레딩과 동시성 쉽게 이해하기"
description: ""
coverImage: "/assets/img/2024-07-10-AndroidMultithreadingandConcurrency_0.png"
date: 2024-07-10 01:08
ogImage: 
  url: /assets/img/2024-07-10-AndroidMultithreadingandConcurrency_0.png
tag: Tech
originalTitle: "Android Multithreading and Concurrency"
link: "https://medium.com/@dilip2882/android-multithreading-and-concurrency-c50bd1a1b9e0"
---


# 소개

# 쓰레드

![Android Multithreading and Concurrency](/assets/img/2024-07-10-AndroidMultithreadingandConcurrency_0.png)

한 프로세서에서 실행되는 두 쓰레드의 프로세스

<div class="content-ad"></div>

- 스레드는 프로세스 내에서 실행 단위의 가장 작은 단위입니다.
- 안드로이드 애플리케이션은 기본적으로 하나의 프로세스에서 실행되지만 여러 스레드를 가질 수 있습니다.
- 주 스레드(또는 UI 스레드로도 알려짐)는 사용자 인터페이스와 상호작용을 처리합니다.
- 긴 시간이 걸리는 작업(예: 네트워크 요청이나 데이터베이스 쿼리)은 UI를 멈추게하는 것을 피하기 위해 주 스레드에서 수행되어서는 안 됩니다.

## 안드로이드의 스레드 유형

1. 주 스레드 (UI 스레드)

- UI 업데이트, 사용자 상호작용 및 뷰 그리기를 처리하는 역할
- 시간이 많이 걸리는 작업을 수행하지 않아야 UI가 멈추는 것을 방지해야 합니다.

<div class="content-ad"></div>

### 2. Background Threads

- Used for non-UI tasks such as network requests, file I/O, and computations.
- Developers create them explicitly to handle work off the main thread.
- Examples include AsyncTask, HandlerThread, and IntentService.

### 3. Worker Threads

- Managed by the system through mechanisms like thread pool and ThreadPoolExecutor.
- Designed for executing independent tasks in parallel.
- Perfect for tasks like image processing and data synchronization.

<div class="content-ad"></div>

# Handlers and Loopers

핸들러(Handler)는 스레드 간 통신을 가능하게 합니다. 핸들러는 메시지 큐를 관리하는 루퍼(Looper)와 관련이 있습니다. 핸들러를 사용하여 메시지나 런러블을 메인 스레드나 다른 워커 스레드로 전달할 수 있습니다.

루퍼(Looper)란 무엇인가요?

- 루퍼는 큐에 있는 메시지(런러블)를 실행하는 클래스입니다.
- 한 번 실행되고 종료되는 단순한 스레드와 달리, 루퍼는 현재 스레드에서 메시지 루프를 생성합니다.
- 루퍼는 명시적으로 중지될 때까지 큐에서 메시지를 계속 처리합니다.

<div class="content-ad"></div>

한들러(Handler)란 무엇인가?

- 한들러는 단일 스레드의 메시지 큐와 관련이 있습니다.
- 루퍼(Looper)의 큐에 Runnable 객체나 메시지를 추가할 수 있도록 해줍니다.
- 새 한들러 인스턴스를 생성할 때 특정 루퍼에 결합됩니다.

루퍼와 한들러를 사용해 메시지 루프를 생성하는 방법:

- 루퍼를 준비합니다:

<div class="content-ad"></div>

**1. Looper 준비하기:**

- `Looper.prepare()`를 호출하여 메시지 처리를 위해 Looper를 준비합니다.
- 이 단계는 현재 스레드와 관련된 메시지 큐를 초기화합니다.

**2. Handler 생성하기:**

- Looper와 관련된 Handler를 인스턴스화합니다.
- Handler는 작업(메시지 또는 Runnable)를 Looper의 큐에 넣을 수 있습니다.

**3. 루프 실행:**

<div class="content-ad"></div>

- 메시지 루프를 시작하려면 Looper.loop()을 호출하세요.
- 루퍼는 순서대로 대기열에 있는 메시지를 처리하며 명시적으로 루프를 종료할 때까지 계속 실행됩니다.

코틀린에서 핸들러 및 루퍼 사용하기:

```kotlin
class MyLooperThread : Thread() {
    private lateinit var mainHandler: Handler

    override fun run() {
        // 루퍼를 준비합니다
        Looper.prepare()

        // 이 루퍼와 연결된 핸들러를 생성합니다
        mainHandler = Handler()

        // 대기열에 작업 추가 시뮬레이션
        mainHandler.postDelayed({
            println("작업 1 실행됨")
        }, 1000)

        mainHandler.postDelayed({
            println("작업 2 실행됨")
        }, 2000)

        // 메시지 루프 시작
        Looper.loop()
    }
}

fun main() {
    val looperThread = MyLooperThread()
    looperThread.start()
}
```

# AsyncTask

<div class="content-ad"></div>

- 역사적으로 안드로이드 개발을 이해하는 데 여전히 유효한 내용이지만 사용은 권장되지 않습니다.
- UI 스레드에서 백그라운드 작업을 관리하는 것을 간단화합니다.
- onPreExecute, doInBackground 및 onPostExecute 메서드로 구성됩니다.
- 이미지 다운로드 또는 서버에서 데이터 가져오는 것과 같은 짧은 수명 작업에 유용합니다.

### AsyncTask가 왜 사용 중지되었는가?

대안을 살펴보기 전에 AsyncTask가 사용 중지된 이유를 간단히 이해해봅시다:

- 복잡성: AsyncTask는 하위 클래스화 및 여러 메서드(onPreExecute, doInBackground, onProgressUpdate 및 onPostExecute) 관리가 필요했습니다. 이 복잡성으로 인해 잠재적인 문제와 오용이 발생했습니다.
- 구성 변경: AsyncTask는 호출 구성요소(일반적으로 Activity)의 수명주기와 결합되어 있었습니다. 구성 변경(화면 회전과 같은)이 메모리 누수나 예기치 않은 동작을 일으킬 수 있었습니다.
- 단일 실행: AsyncTask 인스턴스는 한 번만 실행할 수 있었습니다. 다시 실행하려고 하면 예외가 발생합니다.

<div class="content-ad"></div>

에러가 발생된 것 같아요. 에러 해결을 위해 AsyncTask 클래스를 사용하는 방법을 알려드릴게요!

```js
class FactorialAsyncTask(private val inputNumber: Int) : AsyncTask<Void, Void, Int>() {

    override fun onPreExecute() {
        // 백그라운드 작업이 시작되기 전 UI 스레드에서 실행됩니다
        // UI 업데이트 (예: 진행 상황 표시)
    }

    override fun doInBackground(vararg params: Void?): Int {
        // 백그라운드 스레드에서 실행됩니다
        // 무거운 계산 수행 (팩토리얼 계산)
        return calculateFactorial(inputNumber)
    }

    override fun onPostExecute(result: Int) {
        // 백그라운드 처리가 완료된 후 UI 스레드에서 실행됩니다
        // 결과로 UI 업데이트 (예: 팩토리얼 표시)
    }
}
```

# Loader

로더는 안드로이드 구성 요소 중 하나로, FragmentActivity 또는 Fragment에서 콘텐츠 제공자나 다른 데이터 원본에서 데이터를로드할 수 있게 해줍니다. 데이터 로드와 관련된 몇 가지 일반적인 문제를 해결해줘요!

<div class="content-ad"></div>

- 또한 사용이 중단되었지만 언급할 가치가 있습니다.
- 로더는 별도의 스레드에서 실행되어 느린 또는 응답하지 않는 UI를 방지합니다.
- 데이터를 비동기적으로 로드하고 구성 변경(예: 화면 회전)을 통해 유지합니다.
- 로더는 기본 데이터 소스의 변화를 모니터링하기 위해 옵저버를 구현할 수 있습니다.
- 주로 데이터베이스에서 데이터를 로드하기 위해 CursorLoader와 함께 사용됩니다.

## 로더 API

로더를 사용하는 데 관련된 주요 클래스 및 인터페이스를 살펴보겠습니다:

- LoaderManager: FragmentActivity 또는 Fragment와 관련된 추상 클래스로, 하나 이상의 로더 인스턴스를 관리합니다. 활동 또는 프래그먼트 당 하나의 LoaderManager만 있지만 여러 로더를 관리할 수 있습니다. LoaderManager를 가져오려면 활동 또는 프래그먼트에서 getSupportLoaderManager()을 호출하면 됩니다. 로더에서 데이터를로드 시작하려면 initLoader() 또는 restartLoader()를 사용하십시오. 시스템은 자동으로 동일한 정수 ID를 가진 로더가 이미 있는지 확인하고 새로운 로더를 만들거나 기존 로더를 재사용합니다.
- LoaderManager.LoaderCallbacks: 활동 또는 프래그먼트에서 로더 이벤트에 관련된 콜백을 받기위해 구현해야 하는 인터페이스입니다. onCreateLoader(), onLoadFinished(), onLoaderReset()와 같은 메소드가 포함됩니다.

<div class="content-ad"></div>

예시:

```kotlin
class MyLoaderActivity : AppCompatActivity(), LoaderManager.LoaderCallbacks<Cursor> {

    private val LOADER_ID = 42

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_my_loader)

        // Loader 초기화
        supportLoaderManager.initLoader(LOADER_ID, null, this)
    }

    override fun onCreateLoader(id: Int, args: Bundle?): Loader<Cursor> {
        // CursorLoader를 생성하고 반환
        val uri = Uri.parse("content://my_provider/data")
        return CursorLoader(this, uri, null, null, null, null)
    }

    override fun onLoadFinished(loader: Loader<Cursor>, data: Cursor?) {
        // 로드된 데이터 처리 (UI 업데이트 등)
    }

    override fun onLoaderReset(loader: Loader<Cursor>) {
        // 리소스 정리 (예: 커서 닫기)
    }
}
```

# AsyncTaskLoader

AsyncTaskLoader는 Loader의 하위 클래스로, 화면 회전과 같은 구성 변경을 처리하면서 백그라운드에서 데이터를 비동기적으로 로드할 수 있게 해줍니다. 특히 콘텐츠 제공자, 데이터베이스 또는 기타 데이터 소스에서 데이터를 로드해야 할 때 유용합니다.

<div class="content-ad"></div>

![Example:](https://example.com/image.jpg)  
  
- 커스텀 로더 클래스 작성:

```kotlin
class ContactsLoader(context: Context) : AsyncTaskLoader<List<String>>(context) {

    override fun loadInBackground(): List<String>? {
        // 연락처 불러오기를 시뮬레이션합니다 (실제 데이터 검색으로 대체)
        return listOf("Alice", "Bob", "Charlie")
    }
}  
```

2. 액티비티나 프래그먼트에서 로더를 초기화하세요:

<div class="content-ad"></div>

```kotlin
class MainActivity : AppCompatActivity() {

    private val LOADER_ID = 42

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Initialize the loader
        supportLoaderManager.initLoader(LOADER_ID, null, contactsLoaderCallbacks)
    }

    private val contactsLoaderCallbacks = object : LoaderManager.LoaderCallbacks<List<String>> {
        override fun onCreateLoader(id: Int, args: Bundle?): Loader<List<String>> {
            return ContactsLoader(this@MainActivity)
        }

        override fun onLoadFinished(loader: Loader<List<String>>, data: List<String>?) {
            // Handle the loaded data (update UI, etc.)
            data?.let { contacts ->
                // Display contacts in a RecyclerView or ListView
                // Example: adapter.submitList(contacts)
            }
        }

        override fun onLoaderReset(loader: Loader<List<String>>) {
            // Clean up resources (e.g., close cursor)
        }
    }
}
```

**AsyncTaskLoader 대안:**

- **ViewModel:** 구성 변경을 버티고 UI 관련 데이터를 관리하는 라이프사이클을 고려한 방법을 제공합니다.
- **LiveData:** 데이터 변경을 관찰하고 데이터가 변경될 때 자동으로 UI를 업데이트합니다.

**ThreadPoolExecutor**


<div class="content-ad"></div>

![ThreadPoolExecutor](/assets/img/2024-07-10-AndroidMultithreadingandConcurrency_1.png)

ThreadPoolExecutor은 Java 동시성 프레임워크의 클래스로, 일꾼 스레드 풀을 유연하게 관리하는 방법을 제공합니다. 스레드 수, 대기열 크기 및 다른 매개변수를 제어하여 자원 사용과 작업 실행을 최적화할 수 있습니다.

- 동시에 작업을 실행하는 데 사용하는 일꾼 스레드 풀을 제공합니다.
- 스레드 풀 크기, 대기열 관리 및 작업 실행에 대한 세밀한 제어를 허용합니다.
- 독립적인 작업(예: 이미지 처리)을 병렬화하는 데 유용합니다.

전형적인 ThreadPoolExecutor에는 다음과 같은 여러 구성 요소가 포함되어 있습니다:

<div class="content-ad"></div>


작업자 풀 값: ExecutorService = ThreadPoolExecutor(
    corePoolSize,
    maximumPoolSize,
    keepAliveTime,
    TimeUnit.SECONDS,
    workQueue,
    threadFactory,
    handler
)


- corePoolSize: 풀 내 스레드의 초기 수. 이러한 스레드는 작업 큐가 비어있지 않은 경우 즉시 생성됩니다.
- maximumPoolSize: 풀 내 스레드의 최대 수. 이 한계에 도달할 때까지 스레드가 생성됩니다.
- keepAliveTime: 대기 중인 스레드(작업을 실행하지 않는)가 이 기간 후에 종료됩니다.
- workQueue: 보류 중인 작업이 저장되는 큐입니다. 바운드되었거나 언바운드될 수 있습니다.
- threadFactory: 풀을 위해 새 스레드를 생성합니다.
- handler: 작업 큐가 오버플로우할 때의 상황을 다룹니다(예: 작업 거부).

## 스레드 풀 생성

- 고정 스레드 풀


<div class="content-ad"></div>

고정 스레드 풀은 일정한 수의 스레드를 유지합니다. 작업이 일정하게 흐를 때 유용하며 대략적으로 동일한 수준에 있을 때 유용합니다.

Markdown 형식을 사용하여 이미지 태그를 변경하였습니다:

```js
val workerPool: ExecutorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors())
```

싱글 스레드 풀은 작업을 하나의 스레드에서 순차적으로 실행합니다. 드물거나 성능에 크게 영향을 주지 않는 작업을 병렬로 실행하기에 적합합니다:

<div class="content-ad"></div>

```kotlin
val workerPool: ExecutorService = Executors.newSingleThreadExecutor()
// 예시 사용법:
workerPool.submit {
    // CPU 집약적인 작업 수행
    println("열심히 일하고 있어요")
}
```

## 예시:

병렬로 이미지를 다운로드하고 싶다고 가정해봅시다. 다음은 쓰레드 풀을 사용하는 방법입니다:

```kotlin
val imageUrls = listOf("url1", "url2", "url3")

val workerPool: ExecutorService = Executors.newFixedThreadPool(imageUrls.size)

for (url in imageUrls) {
    workerPool.submit {
        // URL에서 이미지 다운로드
        println("$url 주소에서 이미지 다운로드 완료")
    }
}

// 자원 정리
workerPool.shutdown()
workerPool.awaitTermination(1, TimeUnit.HOURS)
println("이미지 다운로드 완료")
```

<div class="content-ad"></div>

# 코루틴

코루틴은 안드로이드에서 동시성 디자인 패턴을 간단하게 만든 코틀린 기능입니다. 

- 안드로이드에서 동시성을 다루는 현대적인 방법
- 코루틴은 가벼운 스레드로, 비동기 코드를 순차적이고 자연스럽게 작성할 수 있게 해줍니다.
- 코틀린 언어 기능을 활용하여 작성됨
- 가벼우며 구조화되어 사용하기 쉽습니다.
- 콜백과 중첩된 스레드를 순차적인 코드로 대체합니다.
- 비동기 작업을 위한 중단 가능한 함수를 지원합니다.

## 코루틴 범위

<div class="content-ad"></div>

CoroutineScope를 만드는 가장 간단한 방법은 runBlocking 함수를 사용하는 것입니다. 이 함수는 람다 본문 내의 모든 코루틴이 완료될 때까지 현재 스레드를 차단합니다. 여기 예시가 있습니다:

```kotlin
runBlocking {
  // 이 코드는 메인 스레드에서 실행되며 코루틴이 완료될 때까지 차단됩니다.
  launch {
    // 이 코드는 별도의 코루틴에서 실행됩니다.
  }
}
```

## 코루틴 시작하기: launch 함수

CoroutineScope 내에서 새로운 코루틴을 시작하기 위해 launch 함수를 사용합니다. 이 함수는 비동기적으로 실행할 코드가 포함된 람다를 인수로 취합니다. launch 함수는 코루틴의 상태(실행 중, 완료, 취소)에 대한 정보를 제공하고 관리 작업을 할 수 있도록 하는 Job 객체를 반환합니다.

<div class="content-ad"></div>


runBlocking {
  launch {
    delay(1000) // Suspends the coroutine for 1 second
    println("Coroutine finished!")
  }
}


이 예에서 launch 함수는 람다 내부의 코드를 실행하는 새로운 코루틴을 생성합니다. delay 함수는 메시지를 인쇄하기 전에 코루틴을 1초간 일시 정지시킵니다. runBlocking이 사용되었기 때문에 메인 스레드는 코루틴이 완료될 때까지 대기합니다.

## 코루틴 디스패처

- 이것은 코루틴이 실행될 스레드를 지정합니다. UI 업데이트용 Dispatchers.Main 및 네트워크 호출용 Dispatchers.IO와 같은 내장 디스패처가 쉽게 사용할 수 있습니다.


<div class="content-ad"></div>

예시:

```kotlin
// CoroutineScope에 대해 지정하기
runBlocking(Dispatchers.Default) {
    
}

// 또는

// 특정 코루틴에 대해 지정하기
runBlocking {
        launch(Dispatchers.IO) {
                // IO 스레드에서 실행됨
        }
        launch(Dispatchers.Default) {
                // Default 스레드에서 실행됨
        }       
}
```