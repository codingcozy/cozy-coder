---
title: "Shadowfax 안드로이드 앱을 40 더 빠르게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_0.png"
date: 2024-07-07 23:30
ogImage:
  url: /assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_0.png
tag: Tech
originalTitle: "Making Shadowfax Android App 40% faster"
link: "https://medium.com/shadowfax-newsroom/making-shadowfax-android-app-40-faster-995cd36b6e5e"
isUpdated: true
---

**이미지 첨부**

# 1. 목표 설정

휴대폰 앱 성능 관련해서는 모든 밀리초가 중요합니다. 앱이 빠르게 로드될수록 사용자가 머물 가능성이 높아집니다.

100,000명 이상의 DAU를 가진 Shadowfax Rider 앱은 시작하는 데 약 3.5초가 걸려 문제에 직면했습니다!

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

이번에는 시간을 줄이는 것이 목표였어요:

- 90번째 백분위에 대해 2초
- 중위 사용자들에 대해서 800밀리초

## 2. 앱 시작 시간 측정

Firebase에 따르면, 앱 시작 시간은 앱이 런처에서 시작되어 첫 활동의 `onResume()` 메서드가 호출될 때까지의 지속 시간입니다. 이 지속 시간은 다음과 같이 logcat에도 보고됩니다:

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

여기에서 더 많은 정보를 확인할 수 있습니다. 우리에게는 Firebase Startup 시간이 진실의 근원이었습니다.

만약 onResume이 호출된 이후에 앱이 완전히 로드된 것으로 간주한다면(예를 들어 맵이 완전히 그려진 후), 그 시간을 시스템 및 Firebase에 Activity.reportFullyDrawn()을 사용하여 보고할 수 있습니다.

만약 Perfetto를 사용 중이라면, 나중에 더 자세히 설명할 것입니다.

# 3. 자세히 들여다보기

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

앱을 시작 시간별로 세분화하기 위해, Firebase 성능 라이브러리에서 @trace 어노테이션을 앱 클래스의 onCreate() 함수와 BaseActivity & MainActivity의 onCreate() 및 onStart()에 추가했습니다. 기본적으로 모든 것을 최상위 수준에서 측정하여 주요 요인을 파악하고 거기서부터 자세히 들여다 볼 수 있게 했어요.

![이미지](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_1.png)

메인 및 베이스 액티비티 외에도 앱 클래스가 앱 시작 시간의 30%를 차지했기 때문에 두 가지 작업을 수행했어요:

# 3.1 라이브러리 및 콘텐츠 제공자의 지연 로딩 (-10% 시작 시간)

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

애플리케이션 클래스는 보통 많은 라이브러리를 초기화합니다. 우리는 앱 시작 시 바로 필요하지 않은 라이브러리들을 백그라운드에서 초기화하도록 변경했습니다. 만약 컨텐츠 제공 업체가 있다면 Startup 라이브러리를 사용하여 지연 로딩할 수도 있습니다.

다음은 몇 가지 SDK를 백그라운드 스레드에서 초기화하는 방법입니다:

```kotlin
class MyApp : Application() {
    @AddTrace(name = "backgroundInitializationsTrace")
    private fun performBackgroundInitializations() {
        runWithLooper {
            // init SDKs here that do NOT require init on main thread
        }
    }
}

object MyUtils {
    // util fun for init on background thread
    fun runWithLooper(runnable: Runnable) {
        val threadHandler = HandlerThread("Thread${System.currentTimeMillis()}")
        try {
            threadHandler.start()
            val handler = Handler(threadHandler.looper)
            handler.post {
                runnable.run()
            }
        } catch (e: OutOfMemoryError) {
            firebaseCrashlytics.recordException(e)
            runWithinMainLooper(runnable)
        }
    }
    // fallback
    fun runWithinMainLooper(mainThread: Runnable) {
        val handler = Handler(Looper.getMainLooper())
        handler.post {
            mainThread.run()
        }
    }
}
```

# 3.2 베이스라인 프로필 (-7% 시작 시간)

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

구글에서는 첫 번째 앱 시작 시간을 개선하기 위해 기준 프로필 설정을 권장합니다. 우리는 총 앱 시작 시간이 7% 향상되었다는 것을 발견했어요. 여러분의 경우에는 달라질 수 있지만, 꼭 시도해 보세요.

그래서 이것은 좋은 시작이었지만 더 깊이 파야 했어요.

# 4. Perfetto 사용하기

우리는 안드로이드 스튜디오에서 시스템 추적을 실행한 다음 앱을 시작하고 추적 데이터를 Perfetto 시각화 도구로 로드하여 각 함수의 실행 시간을 측정할 수 있어요.

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

Android Studio의 내장 프로파일러를 사용할 수 있지만, Perfetto가 더 나은 네비게이션과 세부 정보를 제공합니다.

시스템 추적 방법은 여기에 있으나 앱 실행 시간을 프로파일링하기 위해 앱을 일반적으로 실행하지 마세요. 다음과 같이 진행하세요:

- 정확한 결과를 얻기 위해 앱의 릴리스 빌드 변형을 선택하세요(debug 빌드 대신)
- Android Studio에서 실행 버튼 근처의 3점 메뉴를 클릭하세요
- 그런 다음 앱을 시작하기 위해 "최소한의 오버헤드로 앱 프로파일링"을 선택하세요
- 이제 앱이 완전히 로드된 후 녹화를 중지하세요
- 마지막으로 Profiler에서 저장 아이콘을 사용하여 추적을 내보내고, Perfetto 웹 UI로 가져오세요

![Image](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_2.png)

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

퍼페토로 추적을 불러오면 화면에 수많은 색이 나타나서 당황스러울 수 있습니다. 하지만 걱정하지 마세요. 우리는 이 모든 것에 신경 쓸 필요가 없어요.

### 퍼페토에서 시작 시간 지표 찾기

![퍼페토에서 시작 시간 지표 찾기](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_3.png)

키보드에서 Ctrl/cmd+F를 눌러 "startup"을 찾아보세요. 앱 시작 시간의 시각화가 나타날 것입니다. 그 녹색 막대를 클릭한 다음 키보드에서 'M'을 눌러보세요. 이렇게 하면 그 막대의 시작과 끝을 표시할 수 있습니다. 이게 지금 우리가 신경 쓸 대상이에요. 다른 것들은 그저 잡음뿐이에요.

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

# 4.2 원인을 찾아라

이제 시작 시간 막대 아래에서 패키지 이름을 찾아 확장하세요. 그래프의 표시된 영역에만 집중하고 주요 스레드를 확인하세요. X축에는 각 함수의 지속 시간이 표시되며, y축에는 중첩 함수가 있습니다.

가로로 긴 막대부터 먼저 검토하세요. 이들이 가장 많은 시간을 소비했기 때문입니다. 예상보다 더 많은 시간이 걸린 함수들 중 어떤 것들이 있는지 살펴보고 나열해보세요. 이제 상위 4-5개의 주요 요인을 파악한 후 최적화할 수 있습니다.

![그래프](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_4.png)

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

프레임이 16밀리초 내에 렌더링되어야 60프레임을 달성할 수 있어요!

팁: WASD 키를 사용하여 이동하세요. perfetto UI에 대해 더 알아보기 바랍니다. 문서가 약간 오래됐지만 핵심 원칙은 동일합니다.

# 5. 토끼굴로 들어가기

장기간 실행되는 함수들이 있을 수 있지만 결과물이 보이지 않을지도 몰라요.

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

우리가 해야 할 일은 이렇게요.

![MakingShadowfaxAndroidApp40faster](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_5.png)

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

트레이싱을 더 많이 추가해야 합니다. 자바 및 코틀린에서 사용할 수 있는 트레이싱 라이브러리가 있어요. 자세한 내용은 여기 있지만, 일단 스니펫을 살펴봅시다:

```js
class MyClass {
  fun foo(pika: String) {
    trace("MyClass.foo") {
    // 기존의 fun 로직을 여기에 추가하시면 됩니다...
    }
  }
}
```

이제 MyClass.foo는 Perfetto에 표시되기 시작할 거에요. 디버깅 중인 함수 내부의 모든 중첩 함수에 트레이싱을 추가하고, 그런 다음 트레이싱을 다시 녹화하여 Perfetto에서 분석해보세요. 각 라이프사이클 함수에 대해 반복하세요.

![MakingShadowfaxAndroidApp](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_6.png)

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

# 6. 우리의 해결책

이 연구를 통해 뿌리 원인과 목표가 매우 명확해졌습니다. 이제 이러한 원인들을 하나씩 해결할 때가 왔습니다. Perfetto와 SysTrace에서 얻은 관측 결과를 바탕으로 다음과 같은 다양한 최적화가 실행되었습니다:

## 6.1. 홈 화면 레이아웃 최적화 (-15% 앱 구동 시간)

ConstraintLayout을 사용하여 뷰의 중첩을 피하고, 사용자 상호작용 후에만 표시되는 숨겨진 뷰 대신 viewstubs를 사용함으로써 최대 600ms까지 감소시켰습니다. 이 방법은 기본적으로 숨겨진 뷰의 측정과 확장을 방지하여 사용자 상호작용 후에만 표시되는 뷰를 효율적으로 처리합니다. view stubs에 대해 더 자세히 읽어보세요.

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

200–300 ms 시간이 소요되었던 mapView가 onResume()이 호출된 이후에만 init으로 이동하여 핵심 UI에 주문 수 및 상태가 배치된 후에 로드되도록 변경되었습니다.

```kotlin
private fun HomeFragment.lazyLoadMap() {
  Handler(Looper.getMainLooper()).post {
    // 이러다이러한 작업은 메인 스레드가 HomeFragment를 인플레이트한 후에만 실행됩니다
    initMap()
  }
}
```

# 6.2 MainActivity 최적화하기 (-5% 시작 시간)

반복된 추적 테스트 결과, MainActivity에서는 상위 수준 레이아웃인 LinearLayout이 ConstraintLayout보다 성능이 우수했습니다. MainActivity는 3–4개 이상의 뷰를 포함하는 Fragment 컨테이너일 뿐이었기 때문에 실제로 LinearLayout으로 변경하면 MainActivity의 시작 속도가 2배 빨라지며, 특히 웜 스타트 때 더욱 효과적입니다.

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

# 6.3 메인 액티비티 SDK의 지연 로딩 (-10% 시간)

퍼페토는 메인 액티비티에서 초기화되는 단일 3rd 파티 SDK가 onCreate() 지속 시간의 70%를 차지한다는 것을 보여줬어요. 그래서 우리는 해당 SDK가 앱 시작 시 즉시 필요하지 않았기 때문에 백그라운드에서 지연 로딩을 시작했어요.

# 7. 야생에서의 실제 성능

이러한 해결책을 여러 릴리스를 통해 출시한 후, 시작 시간 90백분위가 3.5초에서 겨우 2초 미만으로 점차 감소하는 것을 보았어요. 이는 놀라운 42%의 감소에 해당합니다.

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

![image](/assets/img/2024-07-07-MakingShadowfaxAndroidApp40faster_7.png)

🌟 We're constantly searching for bottlenecks and enhancing the speed of the app to boost productivity for our partners.

🔍 Identifying bottlenecks with Perfetto was essential for this project. It provided us with the certainty to address them, knowing the performance gains we would get from fixing each issue.

🙌 Shoutout to Burhan and Vishnu for their help in enhancing the app's startup time!
