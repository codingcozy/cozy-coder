---
title: "Kotlin 멀티플랫폼으로 Android와 iOS에서 동작하는 카운트다운 타이머 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ImplementingaCross-PlatformCountdownTimerinKotlinMultiplatformforAndroidandiOS_0.png"
date: 2024-07-07 13:41
ogImage: 
  url: /assets/img/2024-07-07-ImplementingaCross-PlatformCountdownTimerinKotlinMultiplatformforAndroidandiOS_0.png
tag: Tech
originalTitle: "Implementing a Cross-Platform Countdown Timer in Kotlin Multiplatform for Android and iOS"
link: "https://medium.com/@adman.shadman/implementing-a-cross-platform-countdown-timer-in-kotlin-multiplatform-for-android-and-ios-6f3f41695607"
---


![Image](/assets/img/2024-07-07-ImplementingaCross-PlatformCountdownTimerinKotlinMultiplatformforAndroidandiOS_0.png)

안녕하세요!

안드로이드와 iOS 플랫폼 모두에서 매끄럽게 작동하는 카운트다운 타이머를 만들어보는 것은 도전적이면서 보람이 있는 일일 수 있습니다. Kotlin Multiplatform을 사용하면 두 플랫폼 간의 공통 코드를 공유하면서도 플랫폼별 기능에 액세스할 수 있습니다. 이 글에서는 Kotlin Multiplatform을 사용하여 안드로이드와 iOS용으로 맞춤화된 카운트다운 타이머를 구현하는 방법을 안내해 드리겠습니다.

# 소개

Kotlin Multiplatform을 사용하면 여러 플랫폼에서 실행할 수 있는 코드를 작성하여 중복을 줄이고 일관성을 유지할 수 있습니다. 이 예제에서는 지정된 간격으로 사용자에게 남은 날, 시간, 분과 초를 업데이트하는 카운트다운 타이머를 구현할 것입니다. 안드로이드와 iOS에 대해 플랫폼별 구현을 제공할 것입니다.

<div class="content-ad"></div>

# 공통 코드

먼저, 공유 모듈에서 카운트다운 타이머를 위한 공통 인터페이스를 정의해 봅시다.

```js
expect class CountDownTimer(
    initialMillis: Long,
    intervalMillis: Long,
    onTick: (days: Long, hours: Long, minutes: Long, seconds: Long) -> Unit,
    onFinish: () -> Unit
) {
    fun start()
    fun cancel()
}
```

# 안드로이드 구현

<div class="content-ad"></div>

안녕하세요! 안드로이드 구현부분에 대해서 이야기를 해보도록 할게요.

```kotlin
actual class CountDownTimer actual constructor(
    private val initialMillis: Long,
    private val intervalMillis: Long,
    private val onTick: (days: Long, hours: Long, minutes: Long, seconds: Long) -> Unit,
    private val onFinish: () -> Unit
) {
    private var androidCountDownTimer: android.os.CountDownTimer? = null

    actual fun start() {
        androidCountDownTimer = object : android.os.CountDownTimer(initialMillis, intervalMillis) {
            override fun onTick(millisUntilFinished: Long) {
                val days = millisUntilFinished / (24 * 60 * 60 * 1000)
                val hours = (millisUntilFinished % (24 * 60 * 60 * 1000)) / (60 * 60 * 1000)
                val minutes = (millisUntilFinished % (60 * 60 * 1000)) / (60 * 1000)
                val seconds = (millisUntilFinished % (60 * 1000)) / 1000
                onTick(days, hours, minutes, seconds)
            }

            override fun onFinish() {
                onFinish()
            }
        }.start()
    }

    actual fun cancel() {
        androidCountDownTimer?.cancel()
    }
}
```

iOS 구현부분은 NSTimer를 사용합니다.

<div class="content-ad"></div>

```kotlin
class CountDownTimer constructor(
    private val initialMillis: Long,
    private val intervalMillis: Long,
    private val onTick: (days: Long, hours: Long, minutes: Long, seconds: Long) -> Unit,
    private val onFinish: () -> Unit
) {
    private var iosTimer: NSTimer? = null
    private var endTimeMillis: Long = initialMillis

    fun onTimerTick(timer: NSTimer) {
        val currentTimeMillis = (NSDate().timeIntervalSince1970 * 1000).toLong()
        val millisUntilFinished = endTimeMillis - currentTimeMillis

        if (millisUntilFinished <= 0) {
            onTimerFinish(timer)
        } else {
            val days = millisUntilFinished / (24 * 60 * 60 * 1000)
            val hours = (millisUntilFinished % (24 * 60 * 60 * 1000)) / (60 * 60 * 1000)
            val minutes = (millisUntilFinished % (60 * 60 * 1000)) / (60 * 1000)
            val seconds = (millisUntilFinished % (60 * 1000)) / 1000
            onTick(days, hours, minutes, seconds)
        }
    }

    fun onTimerFinish(timer: NSTimer) {
        iosTimer?.invalidate()
        iosTimer = null
        onFinish()
    }

    fun start() {
        endTimeMillis = (NSDate().timeIntervalSince1970 * 1000).toLong() + initialMillis
        iosTimer = NSTimer.scheduledTimerWithTimeInterval(
            interval = intervalMillis / 1000.0,
            repeats = true
        ) { timer ->
            onTimerTick(timer)
        }
    }

    fun cancel() {
        iosTimer?.invalidate()
        iosTimer = null
    }
}
```

# 사용 예시

카타고리즘 멍키펑키 깃헙 수도 코드의 카운트다운 타이머에 대한 간단한 사용 예시입니다.

```kotlin
val countDownTimer = CountDownTimer(
    initialMillis = 3600000, // 1시간
    intervalMillis = 1000, // 1초
    onTick = { days, hours, minutes, seconds ->
        println("남은 시간: $days 일 $hours 시간 $minutes 분 $seconds 초")
    },
    onFinish = {
        println("카운트다운 완료!")
    }
)

countDownTimer.start()
```

<div class="content-ad"></div>

# 결론

카를린 멀티플랫폼을 활용하면 안드로이드와 iOS 양쪽에서 최소한의 코드 중복으로 작동하는 카운트다운 타이머를 만들 수 있습니다. 이 방식은 개발 시간을 절약할 뿐만 아니라 플랫폼 간 일관된 경험을 보장합니다. 카를린 멀티플랫폼을 사용하면 다양한 플랫폼 간 비즈니스 로직을 공유하는 것이 쉽고, 사용자 경험을 중점으로 더 멋진 앱을 만들 수 있습니다.