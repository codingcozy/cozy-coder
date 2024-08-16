---
title: "Kotlin Multiplatform을 이용해 안드로이드와 iOS에서 크로스 플랫폼 비디오 플레이어 컴포넌트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-CreatingaCross-PlatformVideoPlayerComponentinKotlinMultiplatformAndroidIOS_0.png"
date: 2024-07-10 01:12
ogImage: 
  url: /assets/img/2024-07-10-CreatingaCross-PlatformVideoPlayerComponentinKotlinMultiplatformAndroidIOS_0.png
tag: Tech
originalTitle: "Creating a Cross-Platform Video Player Component in Kotlin Multiplatform Android IOS"
link: "https://medium.com/@adman.shadman/creating-a-cross-platform-video-player-component-in-kotlin-multiplatform-android-ios-9d79174aa2ca"
isUpdated: true
---





![Creating a Cross-Platform Video Player Component in Kotlin Multiplatform for Android and iOS](/assets/img/2024-07-10-CreatingaCross-PlatformVideoPlayerComponentinKotlinMultiplatformAndroidIOS_0.png)

Hey there, fellow Tarot enthusiasts! 🌟

In the world of modern app development, having a video player component that works seamlessly across different platforms like Android and iOS is super important. Thanks to Kotlin Multiplatform (KMP), developers can write shared code that can run on both platforms, utilizing platform-specific APIs when necessary. 

This blog post dives into the art of creating a versatile VideoPlayer component in Kotlin Multiplatform. We'll be exploring how to handle video playback and dynamic orientation changes for Android using Jetpack Compose and for iOS using SwiftUI. 📱✨

### Setting Up Kotlin Multiplatform 

Before we get into the platform-specific implementation magic, let's make sure you have your Kotlin Multiplatform project all set up. Make sure you've got a shared module where you can define your VideoPlayer component using expect and actual declarations. ✨🎬

Stay tuned for more tips and insights in our Tarot reading of Kotlin Multiplatform! 🔮✨

<div class="content-ad"></div>


## Android Implementation with Jetpack Compose

Implementing GoBackToPortraitMode Functionality

안드로이드에서는 화면 방향 변경을 관리하기 위해 Jetpack Compose를 사용하여 GoBackToPortraitMode 기능을 구현합니다.


<div class="content-ad"></div>

```js
// 안드로이드 전용 모듈: androidMain
import androidx.compose.runtime.Composable
import androidx.compose.runtime.LaunchedEffect
import androidx.compose.ui.platform.LocalContext
import android.content.pm.ActivityInfo
import androidx.activity.ComponentActivity

@Composable
실제 fun 세로 모드로 돌아가기(triggerEffect: Boolean) {
    val context = LocalContext.current
    val activity = context as? ComponentActivity

    LaunchedEffect(triggerEffect) {
        if (triggerEffect) {
            activity?.requestedOrientation = ActivityInfo.SCREEN_ORIENTATION_PORTRAIT
        }
    }
}
```

비디오 플레이어 컴포넌트 구현

안드로이드용 VideoPlayer 컴포넌트는 비디오 재생을 위해 VideoView를 사용하며 방향 변경을 동적으로 처리합니다.

```js
@Composable
실제 fun VideoPlayer(
    modifier: Modifier,
    url: String,
    isLandscape: Boolean,
    shouldStop: Boolean,
    미디어준비완료시: () -> Unit
) {
    val activity = LocalContext.current as? ComponentActivity

    if (isLandscape) {
        activity?.requestedOrientation = ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE
    }

    AndroidView(
        modifier = modifier.fillMaxSize(),
        factory = { context ->
            VideoView(context).apply {
                setVideoPath(url)
                val mediaController = MediaController(context)
                mediaController.setAnchorView(this)
                setMediaController(mediaController)
                setOnPreparedListener {
                    미디어준비완료시()
                    start()
                }
                setOnErrorListener { _, _, _ ->
                    미디어준비완료시()
                    true
                }
                layoutParams = FrameLayout.LayoutParams(
                    FrameLayout.LayoutParams.MATCH_PARENT,
                    FrameLayout.LayoutParams.MATCH_PARENT
                )
            }
        },
        update = { view ->
            if (shouldStop) {
                view.stopPlayback()
            } else {
                view.start()
            }
        }
    )
}
```

<div class="content-ad"></div>

## SwiftUI을 활용한 iOS 구현

세로 모드로 돌아가는 기능 구현하기 

iOS에서 SwiftUI의 UIDevice API를 사용하여 방향 변경 관리하기 

```kotlin
// iOS 전용 모듈: iosMain
import androidx.compose.runtime.Composable

@Composable
actual fun GoBackToPortraitMode(triggerEffect: Boolean) {
    if (triggerEffect) {
        UIDevice.current.setValue(UIInterfaceOrientation.portrait.rawValue, forKey: "orientation")
    }
}
```

<div class="content-ad"></div>

VideoPlayer 컴포넌트 구현

SwiftUI의 VideoPlayer 컴포넌트는 AVPlayer를 사용하며 UIDevice API를 활용해 방향 변경을 관리합니다.

```kotlin
@Composable
actual fun VideoPlayer(
    modifier: Modifier,
    url: String,
    isLandscape: Boolean,
    shouldStop: Boolean,
    onMediaReadyToPlay: () -> Unit
) {
    let player = AVPlayer(url: URL(string: url)!)
    let playerLayer = AVPlayerLayer(player: player)
    let avPlayerViewController = AVPlayerViewController()
    avPlayerViewController.player = player
    avPlayerViewController.showsPlaybackControls = true

    if isLandscape {
        UIDevice.current.setValue(UIInterfaceOrientation.landscapeRight.rawValue, forKey: "orientation")
        UIApplication.shared.setStatusBarOrientation(UIInterfaceOrientation.unknown, animated: true)
    }

    UIKitView(
        factory: {
            let playerContainer = UIView()
            playerContainer.addSubview(avPlayerViewController.view)
            return playerContainer
        },
        update: { view, context in
            if shouldStop {
                player.pause()
                avPlayerViewController.player?.play()
                player.seek(to: .zero)
            } else {
                player.play()
            }
        }
    )
}
```

## 사용 예시

<div class="content-ad"></div>

여러분, 이렇게 VideoPlayer와 GoBackToPortraitMode 기능을 코틀린 멀티플랫폼 앱에서 사용할 수 있습니다:

```kotlin
var shouldStop by remember { mutableStateOf(false) }
var shouldGoBackToPortraitMode by remember { mutableStateOf(false) }
var showLoading by remember { mutableStateOf(true) }

Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
    VideoPlayer(
        modifier = Modifier,
        url = contentUrl,
        isLandscape = true,
        shouldStop = shouldStop,
        onMediaReadyToPlay = { showLoading = false }
    )
    if (showLoading) {
        CircularProgressIndicator(modifier = Modifier.size(40.dp), color = AppInfoManager.getAccentColor())
    }
}
```

## 결론

코틀린 멀티플랫폼을 사용하면 VideoPlayer와 같은 플랫폼별 기능을 활용하면서 공유 로직을 유지할 수 있는 교차 플랫폼 구성 요소를 효율적으로 만들 수 있습니다. Android에서는 Jetpack Compose를 사용하고 iOS에서는 SwiftUI를 사용함으로써 앱은 다양한 기기와 플랫폼에서 일관되고 풍부한 멀티미디어 경험을 제공할 수 있습니다. 이 접근 방식은 개발 시간을 단축할 뿐만 아니라 Android와 iOS 간에 통일된 사용자 경험을 보장합니다.