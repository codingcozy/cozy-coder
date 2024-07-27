---
title: "Android 13에서 onBackPressed 폐지 - 기존 앱에 필요한 주요 변경 사항"
description: ""
coverImage: "/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_0.png"
date: 2024-07-13 00:33
ogImage: 
  url: /assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_0.png
tag: Tech
originalTitle: "The onBackPressed Is Now Deprecated in Android 13 and Might Need Major Changes in Existing Apps"
link: "https://medium.com/mobile-app-development-publication/migrate-to-android-13-predictive-back-soon-before-its-too-late-e1e1723f392"
---


## 안드로이드 개발 배우기

![Image](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_0.png)

안드로이드 13에서 소개된 기능 중 하나인 Push Notification 권한 이외에 가장 두드러진 기능 중 하나는 예측형 백(Back) 기능입니다. 이 기능은 기본적으로 활성화되어 있지 않지만, 안드로이드 13부터는 안드로이드의 미래 방향이 될 것입니다.

Google은 경고를 발표했습니다.

<div class="content-ad"></div>

만약 우리 앱의 백 버튼 동작을 사용자 정의한다면, Activitity의 onBackPressed를 재정의하는 방식으로, 이 변경 사항을 주의 깊게 살펴야 합니다. 최신 동작으로 업그레이드하는 것은 안드로이드에서 뒤로 가기를 사용자 정의하는 방식에 꽤 많은 패러다임 전환을 요구하며, 이는 상당히 많은 코드 변경으로 이어질 수 있습니다.

따라서 고급 사용자 정의 onBackPressed를 갖춘 대규모 레거시 앱이 있다면, 가능한 한 빨리 이 이주 작업에 착수하는 것이 중요합니다.

## 이 변경 사항이 필요한 이유

Google은 이 변경 사항에 대해 문서와 YouTube를 공유했지만, 널리 발표되지는 않았습니다.

<div class="content-ad"></div>

이건 우연히 onBackPressed가 더 이상 사용되지 않는다는 것을 알게 되었어요.

이 변경 경고는 코틀린 클래스에서만 표시되는데, 자바 클래스에서는 표시되지 않는군요. 그래서 자바로 안드로이드 코드를 작성하는 경우(어플리케이션의 일부 구식 클래스들이 있을 수도 있겠죠?), 이런 변화에 관해 놓치고 있을 수도 있어요!

![이미지](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_1.png)

현재 동작을 변경하는 것은 단지 몇 가지 매개변수를 변경하거나 API 호출만 하는 것이 아닙니다. 예측된 뒤로가기를 가능하게 하려면 돌연변이적인 전환을 필요로 합니다.

<div class="content-ad"></div>

따라서 예기치 않은 상황에 대비하기 위해 'Predictive back'을 이해하고 빨리 적용하는 방법을 알아보는 것이 중요합니다.

# '뒤로' 동작 이력 안내

변화의 이유를 이해하기 위해서 안드로이드가 '뒤로' 처리를 어떻게 발전시켰는지 알아야 합니다. 이를 통해 코드 변경 의도를 쉽게 이해할 뿐만 아니라 더 잘 받아들일 수 있습니다.

안드로이드 기기들은 초기에 주로 홈, 최근, 그리고 뒤로 버튼이 3개 있는 것이 일반적이었습니다. 아래 예시는 삼성 S2의 이 버튼들을 보여줍니다.

<div class="content-ad"></div>

여러 해 동안 하드웨어 버튼은 이제 아래의 Nexus 5 장치에 표시된 것처럼 소프트웨어 버튼으로 진화했습니다. 이로 인해 버튼이 전체 화면에 통합되어 더욱 조화롭게 보입니다.

기기에서 애플리케이션에 대한 더 많은 실제 공간을 확보하기 위한 일환으로 버튼이 더 이상 표시되지 않습니다. 그럼에도 불구하고 사용자는 스와이프 제스처로 이러한 동작을 수행할 수 있습니다.

## 뒤로 가기용 스와이프 제스처

뒤로 가기용 제스처는 단순히 기기에서 더 많은 화면 공간을 제공하는 것뿐만 아니라 사용자가 실제로 뒤로 이동하고 싶은지를 고려할 수 있도록 합니다.

<div class="content-ad"></div>

![Tarot Image](https://miro.medium.com/v2/resize:fit:640/1*oUQs4mqkLjmWz2GZe6GNrA.gif)

With this in place, Google thought perhaps if it was aware that a back button would exit the app, Google could pre-warn the user by giving some slight animation to the app.

This will help one distinguish if the back button is really just an app internal back (to some previous screen), or really exiting the app.

In Android 13, this is made possible, if

<div class="content-ad"></div>

- 앱은 Predictive Back을 채택했습니다 (코드에서만 사용하고 더 이상 onBackPressed()를 사용하지 않음)
- AndroidManifest.xml에서 애플리케이션 섹션에 android:enableOnBackInvokedCallback="true"을 선언하여 Predictive Back에 동의합니다.
- 개발자 옵션에서 Predictive back animations 기능을 활성화하십시오.

![Predictive Back](https://miro.medium.com/v2/resize:fit:640/1*pLjip7gaacZwFy0sSfmLaA.gif)

# 코딩 패러다임의 변화

![Deprecated in Android 13](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_2.png)

<div class="content-ad"></div>

위의 그림처럼, 나는 onBackPressed 커스터마이징을 3단계로 분할하고 있어.

## 1. 안드로이드 T 이전

안드로이드가 처음 소개된 이후로, 우리는 activity 클래스의 onBackPressed() 함수를 override하여 시스템 onBackPressed를 호출하기 전에 필요한 동작을 커스터마이즈할 수 있었어.

이것의 전형적인 사용 사례는 중첩된 fragment가 있는 activity에서 사용자가 뒤로 가기를 클릭할 때, activity를 종료하는 대신에 먼저 중첩된 fragments를 팝하고 싶은 경우야.

<div class="content-ad"></div>

아래와 같이 Fragment에 onBackPressed()가 없으므로 activity의 onBackPressed()에서 완전히 처리합니다.

```kotlin
override fun onBackPressed() {
    supportFragmentManager.fragments.forEach { fragment ->
        if (fragment != null && fragment.isVisible) {
            with(fragment.childFragmentManager) {
                if (backStackEntryCount > 0) {
                    popBackStack()
                    return
                }
            }
        }
    }
    super.onBackPressed()
}
```

## 2. Android T (이후) onBackPressed 이전 버전으로 이동하기

<div class="content-ad"></div>

In Android T today, we can still use onBackPressed() just like before, and it will work the same way as it did in the past.

But it's a good idea to start thinking about using the onBackPressedDispatcher callback instead.

![Android Back Button](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_4.png)

For better backward compatibility, make sure to add the latest version of AndroidX activity (1.6 or newer) to the dependencies section of your app’s build.gradle.

<div class="content-ad"></div>

그건 어렵지 않게 들릴 수 있지만, onBackPressed() 코드를 onBackPressedDispatcher 콜백으로 이동하면 되겠지 않을까요? 아래와 같이 활동 onCreate()에서 보여주는 것처럼요

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // 다른 코드

    onBackPressedDispatcher.addCallback(this) {
        supportFragmentManager.fragments.forEach { fragment ->
            if (fragment != null && fragment.isVisible) {
                with(fragment.childFragmentManager) {
                    if (backStackEntryCount > 0) {
                        popBackStack()
                        return@addCallback
                    }
                }
            }
        }

        // 우리의 콜백을 제거하고 다음 Back 콜백을 호출해요
        this@addCallback.remove()
        onBackPressedDispatcher.onBackPressed()
    }
}
```

우리의 경우에는 이로 인해 다른 동작이 나타날 거예요. 이에 대해 자세히 설명한 부분이 있어요

<div class="content-ad"></div>

## 3. 안드로이드 T(이후 버전)에서 android:enableOnBackInvokedCallback를 켠 경우

`android:enableOnBackInvokedCallback="true"`을 켜면 잠재적으로 새로운 안드로이드 SDK 동작을 흉내 내는 데 사용됩니다 (Google이 소개할 수도 있는).

그렇게 되면 사용자 백 제스처가 더 이상 액티비티의 `onBackPressed()` 함수를 호출하지 않을 것입니다. 따라서 거기에 사용자 지정 코드가 있으면 그것이 트리거되지 않을 수 있고 사용자 백 제스처 때 앱에서 이상한 동작이 발생할 수 있습니다.

![이미지](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_5.png)

<div class="content-ad"></div>

따라서, 새로운 onBackPressed Dispatcher 콜백 플로우로 조속히 이동하는 것이 중요합니다.

## onBackPressed Dispatcher Callback 작동 방식 이해하기

새로운 백 액션 사용자 정의는 백 콜백 스택에 의해 처리됩니다. 이들을 onCreate()나 fragment의 onViewCreated()에 추가할 수 있습니다. 이들은 나중에 백 인터셉션을 위한 준비로 라이프사이클 초반에 추가됩니다.

매개변수는 다음과 같습니다.

<div class="content-ad"></div>

```js
fun OnBackPressedDispatcher.addCallback(
    owner: LifecycleOwner? = null,
    enabled: Boolean = true,
    onBackPressed: OnBackPressedCallback.() -> Unit
): OnBackPressedCallback
```

- **onBackPressed** — 이곳에서 특정 액티비티나 프래그먼트의 뒤로 가기 동작을 맞춤 설정합니다.
- **owner** — 이것은 이 호출을 소유한 사람(프래그먼트 뷰 또는 액티비티)을 나타냅니다. 예: (viewLifecycleOwner 또는 activity) 따라서 소유자가 종료되면 콜백도 제거됩니다(메모리 누수나 충돌을 방지하기 위해).
- **enabled** — 이것은 콜백을 활성화 또는 비활성화하는 데 사용됩니다. 예: 사용자가 뷰를 종료하려는지 정말로 확인하기 위해 어떤 조치를 취할 때만 활성화할 수 있습니다.

## onBackPressed 콜백이 실행되는 방법을 설명하는 간단한 예제 앱 흐름

뒤로 가기 콜백이 작동하는 방식을 이해하기 위해, 한 가지 디자인을 사용하여 설명해보겠습니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_6.png)

앱이 여기에 로드된 상태에서 가정하는 것은 4단계가 있다고 가정합니다.

1. 앱을 로드할 때, 첫 번째 '뒤로'는 앱을 종료하는 '뒤로'입니다.
2. 활동이 로드될 때, 우리는 사용자 정의된 onBackPressed 콜백을 추가합니다.
3. 사용자가 홈 탭에서 세 번째 탭인 영화 탭으로 전환합니다. 이것은 바텀 바이므로 앱을 종료하기 전에 홈 탭으로 돌아가야 합니다.
4. 세 번째 탭이 로드될 때, 사용자 정의된 onBackPressed 콜백을 가진 프래그먼트도 로드됩니다.

1, 2, 3, 4에서보다 상세히 설명된 것처럼 onBackPressed 콜백이 추가됩니다.

<div class="content-ad"></div>

**Back 동작에 대한 내용입니다. 각 백 제스처는 해당 콜백(반대로 실행되는 형태)을 실행하고, 이전 콜백이 실행되기 전에 제거되어야 합니다(다음 백 제스처가 트리거되었을 때).

- 5. 첫 번째 백 제스쳐가 트리거 되면, Fragment에 추가된 사용자 지정 백이 호출되어 Fragment가 팝됩니다. Fragment가 팝되고 나면 해당 콜백이 스택에서 제거됩니다.
- 6. 다음 백 제스쳐가 트리거되면, 시스템에서 추가된 백 액션이 Home 탭으로 돌아가는 트리거입니다.
- 7. 다음 백 제스쳐가 트리거되면, 액티비티 사용자 지정 콜백이 트리거됩니다. 이후 해당 콜백은 해당 스택에서 제거되어야 합니다(callback.remove()를 사용하여 구현 가능) 또는 비활성화되어야 합니다(예: callback.enabled = false). 이후 액티비티가 여전히 활성 상태이고 해당 콜백을 더 이상 트리거하고 싶지 않은 경우 사용할 수 있습니다.
- 8. 마지막으로 발생하는 백은 앱을 종료하는 것입니다. 스택을 기반으로 시스템이 이것이 마지막 콜백임을 알 수 있고, 사용자에게 예측된 백 애니메이션을 제공할 수 있습니다.

**콜백 사용 활성화**

onBackPressedDispatcher 스택에 콜백을 추가하면 활성화될 때까지 트리거되지 않습니다.**

<div class="content-ad"></div>

이 문구는 사용자의 상태에 따라 앱을 나가거나 (예: 사용자에게 정말로 앱을 나가겠냐고 묻는 대화 상자를 표시하는) 사용자 지정 작업을 수행할 수 있도록 하는 경우에 매우 편리합니다.

![Image 1](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_7.png)

![Image 2](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_8.png)

# 이주 예시 디자인

<div class="content-ad"></div>

위에서 공유된 onBackPressedDispatcher 콜백 스택 작동 방식에 대해 공유했었는데, 코드 관점에서 백(뒤로) 사용자 정의를 어떻게 이동할 수 있는지 살펴보는 것이 좋을 수도 있겠죠.

이전에 언급했던대로, 해당 내용을 이전해도 onBackPressed()에서 사용자 정의 코드를 onBackPressedDispatcher의 콜백으로 이동하는 것만큼 단순한 일은 아닙니다.

상황을 설명하기 위해 Bottom Tab Bar로 이루어진 Fragments 그리고 내부에 중첩된 Child Fragments가 있는 디자인을 사용하겠습니다. 이 디자인은 이 기사에서 가져왔지만, 전체적인 설명을 위해 여기서 다시 설명하겠습니다.

이 디자인에서, 마이그레이션을 설명하기 위해 4가지 샘플이 있습니다.

<div class="content-ad"></div>


![Image](https://miro.medium.com/v2/resize:fit:640/1*FYreUmTcDWhbx3V6pK5dEA.gif)

## 원본 디자인 (onBackPressed를 사용한 Activity)

앱 플로우의 원래 동작에서는 뒤로 가기 제스처 동작을 설명하겠습니다. 마이그레이션을 테스트하기 위해 사용할 수 있는 내용입니다. 뒤로 가기 제스처를 통해 탭 전환, Fragment 및 ChildFragment가 팝될 때의 동작을 보여드리겠습니다.


<div class="content-ad"></div>

더 자세한 설명을 위해 아래의 플로우를 보여드립니다.

![Flow](https://yourwebsite.com/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_10.png)

백 제스처를 커스터마이즈하여 프래그먼트를 팝업하는 고전적인 방법은 다음과 같습니다. 액티비티의 `onBackPressed()`에서 백 제스처를 가로채는 것입니다.

```kotlin
override fun onBackPressed() {
    supportFragmentManager.fragments.forEach { fragment ->
        if (fragment != null && fragment.isVisible) {
            with(fragment.childFragmentManager) {
                if (backStackEntryCount > 0) {
                    popBackStack()
                    return
                }
            }
        }
    }
    super.onBackPressed()
}
```

<div class="content-ad"></div>

## onBackPressed() 함수와 enableOnBackInvokedCallback을 사용한 활동

만약 디자인에서 android:enableOnBackInvokedCallback="true"를 켜면, onBackPressed()는 호출되지 않게 됩니다.

그 결과로, fragment를 팝하는 사용자 지정 back 동작이 트리거되지 않게 됩니다.

이상한 동작은 아래에서 확인할 수 있습니다. 백이 2개 빠져버린 상황이 나타납니다.

<div class="content-ad"></div>

이를 더 잘 설명하기 위해 사용자 정의 제스처 back 콜백이 추가되지 않은 경우 시스템에서 추가한 두 가지 케이스가 있습니다:

- 홈 탭으로 돌아가는 스위치
- 애플리케이션을 종료하는 백

![image](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_12.png)

<div class="content-ad"></div>

따라서 뒷닫힘 동작 시에는 그 두 개의 뒤에만 작동되도록 트리거되었습니다.

[2014-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_13.png]

## Activity onBackPressedDispatcher Callback Setup

onBackPressed() 함수가 더 이상 작동하지 않는다는 점을 감안해, activity onCreate()에서 onBackPressedDispatcher를 사용하도록 마이그레이션할 수 있을 것으로 생각되어 콜백을 추가했습니다.

<div class="content-ad"></div>

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // 다른 코드

    onBackPressedDispatcher.addCallback(this) {
        supportFragmentManager.fragments.forEach { fragment ->
            if (fragment != null && fragment.isVisible) {
                with(fragment.childFragmentManager) {
                    if (backStackEntryCount > 0) {
                        popBackStack()
                        return@addCallback
                    }
                }
            }
        }

        // 우리의 콜백을 제거하고 다음 백 콜백을 호출합니다
        this@addCallback.remove()
        onBackPressedDispatcher.onBackPressed()
    }
}
```

이 코드에서는 아래와 같이 명시적으로 추가해야 합니다 (좋지 않은 코드 향입니다)

```kotlin
        // 우리의 콜백을 제거하고 다음 백 콜백을 호출합니다
        this@addCallback.remove()
        onBackPressedDispatcher.onBackPressed()
```

이 코드가 없으면 모든 프래그먼트가 팝됐어도 계속해서 모든 백키 제스처를 가로채는 콜백이 영원히 남아있어서, 메인 액티비티에서 벗어날 수 없고 애플리케이션을 종료할 수 없게 됩니다.


<div class="content-ad"></div>

이 문제를 해결하려면

- 먼저, 우리 자신의 콜백을 제거하여 다음 백 콜백이 호출될 수 있도록 합니다.
- 다음 콜백을 수동으로 호출하여 응용 프로그램을 종료할 수 있도록 합니다. 그렇게 하지 않으면 응용 프로그램이 버그가 있는 것처럼 느껴질 때까지 추가 제스처를 기다려야 합니다.

위와 같이 설정하면 아래와 같이 응용 프로그램이 작동합니다. 여전히 예상한 제스처 백이 누락되어 있는 것을 볼 수 있습니다.

![이미지 설명](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_14.png)

<div class="content-ad"></div>

이를 더 잘 설명하기 위해 사용자 정의한 제스처 백 콜백이 액티비티의 onCreate()에 추가되었기 때문에 후에 삽입되는 탭 변경 콜백보다 앞에 삽입됩니다. 따라서 콜백들(추가된 순서대로)은 아래와 같습니다.

- 홈 탭으로 전환하기
- 모든 프래그먼트를 팝업하는 액티비티에 추가된 사용자 정의 콜백
- 애플리케이션을 종료하는 백

![이미지](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_15.png)

이러한 콜백이 설정된 상태에서 제스처 백을 하면 먼저 홈 탭으로의 네비게이션 백이 트리거됩니다. 홈 탭 네비게이션 이후의 하위 프래그먼트들은 팝될 기회조차 없게 됩니다.

<div class="content-ad"></div>

안녕하세요! 🌟

위 이미지 태그를 아래 Markdown 형식에 맞게 바꿔보세요.


![이미지 설명](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_16.png)


그리고 사용자의 경우, 맞춤 백이 마지막으로 트리거될 때 다음 백 액션을 수동으로 트리거해야 합니다. 이 자체가 코드 스멜이 되는군요.

## Fragment onBackPressedDispatcher Callback 설정

액티비티 onCreate()에 onBackPressedDispatcher를 추가하는 것이 우리 상황에서 도움이 되지 않는다면, 아마도 컨테이너 프래그먼트의 onViewCreated()에 onBackPressedDispatcher 콜백을 추가할 수 있을 것 같아요.

<div class="content-ad"></div>

```kotlin
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        // 기타 코드

        requireActivity().onBackPressedDispatcher.addCallback(
            owner = viewLifecycleOwner
        ) {
            requireActivity().supportFragmentManager.fragments.forEach { fragment ->
                if (fragment != null && fragment.isVisible) {
                    with(fragment.childFragmentManager) {
                        if (backStackEntryCount > 0) {
                            popBackStack()
                            return@addCallback
                        }
                    }
                }
            }

            // 우리의 콜백을 제거하고 다음 백 버튼 콜백을 호출합니다.
            this@addCallback.remove()
            requireActivity().onBackPressedDispatcher.onBackPressed()
        }
    }
```

이렇게하면 각 컨테이너 프래그먼트가 생성될 때마다 새로운 콜백이 만들어지므로 이전 방식(우리 경우 활동에 콜백 추가)보다 훨씬 나은 방식입니다.

그러나 이 코드에서 명시적으로 아래 코드도 추가해야 합니다(이는 코드 냄새가 납니다).

```kotlin
// 우리의 콜백을 제거하고 다음 백 버튼 콜백을 호출합니다.
this@addCallback.remove()
requireActivity().onBackPressedDispatcher.onBackPressed()
```

<div class="content-ad"></div>

하지만, 이 방식에서는 이주 이전의 원래 설계 동작을 나타내는 것처럼 행동하는 것을 볼 수 있어요.

![image](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_17.png)

더 확실하게 설명하기 위해, 각 컨테이너 Fragment에 추가된 사용자 정의 제스처 back 콜백이 onViewCreated()에서 추가되기 때문에 Tab 변경 콜백 전후에 삽입됩니다. 따라서 callback (추가된 순서로)는 다음과 같습니다.

- 모든 Fragment를 팝하는 Container Fragment에 추가된 사용자 지정 콜백
- 홈 탭으로 돌아가는 작업
- 모든 Fragment를 팝하는 Container Fragment에 추가된 사용자 지정 콜백
- 애플리케이션을 종료하는 back

<div class="content-ad"></div>

![Image 1](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_18.png)

With these callbacks in place, when we gesture back, it first triggers the popping of the latest container fragment, and then navigates back to the home tab. Upon reaching the home tab, it again triggers the fragment popping callback. And, finally, it ends with the termination of the app.

![Image 2](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_19.png)

From a normal behavior point of view, this already reflects the original design that utilizes the onBackPressed() function.

<div class="content-ad"></div>

이에 대한 문제는 다음과 같습니다:

- 사용자 정의 콜백이 수동으로 제거(비활성화)됨 — Container Fragment가 해당 제스처 백이 발생하지 않기 때문입니다.
- 다음 시스템 콜백을 수동으로 호출해야 함.

이는 코드에 냄새를 발생시키며 예측적인 백 제스처를 방해합니다.

최종 백 동작은 다음과 같이 나타납니다. 앱을 나가는 예측적인 백이 표시되지 않음 (최종 제스처 백에 2개의 콜백이 있기 때문입니다).

<div class="content-ad"></div>

![Child Fragment onBackPressedDispatcher Callback Setup](https://miro.medium.com/v2/resize:fit:640/1*oUQs4mqkLjmWz2GZe6GNrA.gif)

## Child Fragment onBackPressedDispatcher Callback Setup

컨테이너 프래그먼트의 onViewCreated()에서 onBackPressedDispatcher 콜백을 추가하면 현재 콜백을 수동으로 제거하고 다음 콜백을 수동으로 호출해야 하는 필요가 생깁니다. 아마도 onBackPressedDispatcher 콜백을 해당 자식 프래그먼트의 onViewCreated()로 옮기는 것이 좋을 것 같습니다.

이것이 가장 합리적인 것으로 보이며, 각 백 제스처는 자식 프래그먼트 자체를 제거하는 것을 의도하고 있습니다.

<div class="content-ad"></div>

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // 기타 코드

    requireActivity().onBackPressedDispatcher.addCallback(
        owner = viewLifecycleOwner
    ) {
        parentFragment?.childFragmentManager?.popBackStack()
    }
}
```

여기서 onBackPressedDispatcher 콜백을 간단하게 추가하는 것을 주목해 주세요. 아래와 같은 작업이 필요하지 않습니다

- 수동으로 콜백 제거(비활성화) — 콜백은 소유한 프래그먼트가 팝되면 자동으로 제거됩니다.
- 마지막 백 액션에서 콜백이 제거된 후의 시스템 콜백을 수동으로 호출

행동 흐름은 원래 디자인과 동일하며, 올바른 동작입니다.


<div class="content-ad"></div>

내부적으로는, 콜백(callback)이 구성되는 방식이 이전 방식과 다릅니다.

- 하위 프래그먼트가 생성될 때만 추가됩니다 (따라서 해당 하위 프래그먼트를 팝하려는 백(Back) 제스처 의도가 있을 때만 불필요한 콜백이 추가되지 않습니다)
- 생성된 각 하위 프래그먼트는 자체 팝하는 콜백을 갖게 됩니다. 자신을 팝하면 콜백도 자동으로 제거됩니다. (컨테이너 프래그먼트 내에서 여러 개의 하위 프래그먼트가 생성된 경우, 이 접근 방식에서는 여러 개의 콜백이 추가됩니다. 이전 방식에서는 모든 하위 프래그먼트 팝을 처리할 하나의 콜백만 있었습니다)

![이미지](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_21.png)

<div class="content-ad"></div>

백 행동을 수행할 때마다 해당 백 동작은 자체 콜백을 호출하고 하위 프래그먼트를 팝합니다. 팝될 때마다 해당 콜백은 자동으로 제거됩니다. 따라서 시스템 콜백을 수동으로 호출하는 것은 더 이상 필요하지 않습니다.

![image](/assets/img/2024-07-13-TheonBackPressedIsNowDeprecatedinAndroid13andMightNeedMajorChangesinExistingApps_22.png)

이렇게 함으로써, 하위 프래그먼트의 마지막 팝 시에는 onBackPressedDispatcher 콜백 스택에 남아 있는 시스템 콜백만 사용할 수 있습니다. 사용자가 다시 뒤로 가려고 할 때 시스템은 애플리케이션을 종료하게 될 것임을 명확히 인식하고, 따라서 사용자에게 사전 경고 애니메이션을 제공할 수 있습니다.

![image](https://miro.medium.com/v2/resize:fit:640/1*pLjip7gaacZwFy0sSfmLaA.gif)

<div class="content-ad"></div>

이제 이는 onBackPressed()에서 onBackPressedDispatcher로의 완전한 동작 이동을 충족합니다.

# 결론

디자인에서 공유된 대로 onBackPressed()에서 onBackPressedDispatcher로의 이주는 단순한 매개변수 변경 뿐만 아니라 API 호출 변경뿐만 아니라 코드가 이동해야하는 전반적인 흐름 역시 포함됩니다.

<div class="content-ad"></div>

오리지널 맞춤화된 백 버튼 동작이 얼마나 복잡한지에 따라, 백 버튼 동작을 이해하고 변경해야 할 수도 있습니다. 따라서 이전 이관 작업은 적기에 올바르게 수행되도록 시작돼야 합니다.

올바르게 이전된 경우, 우리 예제에서는 활동의 복잡한 onBackPressed() 코드를 훨씬 간단한 onBackPressedDispatcher 콜백으로 변환할 수 있습니다.

```kotlin
override fun onBackPressed() {
    super.onBackPressed()
}
```

<div class="content-ad"></div>

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // Other code

    requireActivity().onBackPressedDispatcher.addCallback(
        owner = viewLifecycleOwner
    ) {
        parentFragment?.childFragmentManager?.popBackStack()
    }
}
```

이 예측적인 백 기능 때문에 activity에만 존재하던 예전의 onBackPressed()보다 훨씬 좋습니다. Fragment가 자체 백 제스처를 처리할 책임이 있기 때문입니다.

안드로이드 13부터 변경된 사항은 Google이 사용자들에게 더 빨리 전환하도록 독려하면서 앞으로 더 큰 백 제스처 변경의 시작에 불과하다고 믿습니다.

# 참고자료


<div class="content-ad"></div>

- Systems Back에 대한 기본 사항 - Google의 YouTube 채널에서 확인하세요
- 사용자 정의 백 네비게이션을 제공하는 방법 - Google 문서에서 찾아보세요
- 예측 백 제스처를 지원하는 방법 추가하기 - Google 문서에서 확인하세요
- 제스처 네비게이션과의 호환성 보장하기 - Google 문서에서 확인하세요