---
title: "Android와 React Native 간의 통신 방법"
description: ""
coverImage: "/assets/img/2024-07-10-HowtocommunicatebetweenAndroidandReactNative_0.png"
date: 2024-07-10 01:07
ogImage: 
  url: /assets/img/2024-07-10-HowtocommunicatebetweenAndroidandReactNative_0.png
tag: Tech
originalTitle: "How to communicate between Android and React Native"
link: "https://medium.com/@sinanomik/how-to-communicate-between-android-and-react-native-1e12c5a73b03"
---


![React Native Bridging](/assets/img/2024-07-10-HowtocommunicatebetweenAndroidandReactNative_0.png)

React Native 브리징은 앱의 JavaScript 부분과 네이티브(Android 또는 iOS) 부분 간의 연결을 구축하는 것과 같습니다. 이 가이드에서는 간단한 단계로 어떻게 수행되는지 보여드릴 거에요.

브리징 메커니즘을 통해 React Native 애플리케이션은 JavaScript에서 바로 사용할 수 없는 플랫폼별 API 및 기능에 액세스할 수 있습니다. 이를 통해 React Native 개발자는 네이티브 모듈을 통합하여 애플리케이션의 기능을 확장할 수 있습니다. 이러한 네이티브 모듈은 기본적으로 특정 기능을 JavaScript 레이어에 노출하는 네이티브 코드 패키지입니다. 

이것이 작동하는 방식을 살펴보겠습니다:

<div class="content-ad"></div>

1. 자바스크립트 쪽: React Native 앱에서는 자바스크립트로 코드를 작성합니다. Android 또는 iOS에 특화된 기능을 사용하려면 해당 기능을 나타내는 모듈을 가져와 사용합니다.

2. 네이티브 쪽: 장치 쪽(Android 또는 iOS)에서는 플랫폼의 네이티브 언어(예: Android용 Java/Kotlin 또는 iOS용 Swift)로 코드를 작성합니다. 이 코드는 카메라, 센서 또는 기타 장치별 기능에 액세스할 수 있게 합니다.

3. 브릿지: React Native에는 자바스크립트와 네이티브 코드 간 통신을 돕는 특별한 시스템인 "브릿지"가 있습니다. 자바스크립트에서 네이티브 모듈의 함수를 호출하면, 브릿지가 해당 요청을 네이티브 코드로 보내고 그 반대도 마찬가지입니다.

4. 네이티브 모듈: 네이티브 모듈은 자바스크립트와 네이티브 코드 사이의 번역기와 같습니다. 플랫폼의 언어로 작성된 코드 조각으로, 자바스크립트에서의 요청을 처리하고 장치에서 작업을 수행하는 방법을 알고 있습니다.

<div class="content-ad"></div>

5. 등록: 네이티브 모듈을 JavaScript에서 사용할 수 있게 만들기 위해서는 React Native에 등록해야 합니다. 일반적으로 안드로이드의 "MainApplication.java" 또는 iOS의 "AppDelegate.m"과 같은 특정 파일에 설정 코드를 추가해야 합니다.

새로운 React Native 프로젝트를 설정해보겠습니다. 터미널에서 다음 명령어를 실행해주세요:

```js
npx react-native@latest init ReactProject
cd ReactProject
```

프로젝트가 초기화되면, 프로젝트 디렉토리로 이동하여 응용 프로그램을 시작해보세요.

<div class="content-ad"></div>


npm start


안드로이드 기기에서 애플리케이션을 실행하려면 다음 명령을 실행하세요:


npx react-native run-android


이제 브릿지 모듈을 프로젝트에 통합해 보겠습니다.

<div class="content-ad"></div>

첫 번째로, "App.js" 파일을 열고 다음 코드 조각을 포함하세요:

```js
import { NativeModules } from 'react-native';
const { Module } = NativeModules;
```

이 코드 조각은 React Native에서 "NativeModules" 패키지를 가져와서 "Module" 객체에 할당합니다. "Module"이 모듈의 이름과 일치하는지 확인하세요.

그다음으로 브릿지 모듈용 Kotlin 클래스를 작성해봅시다. 주로 메인 액티비티가 있는 위치로 이동하세요. 일반적으로 "com.yourproject" 폴더 하위에 위치합니다.

<div class="content-ad"></div>

새 Kotlin 클래스 "Module"을 생성해 보세요:

```kotlin
class Module(reactContext: ReactApplicationContext) : ReactContextBaseJavaModule(reactContext) {

    private var successCallback: Callback? = null
    private var errorCallback: Callback? = null

    override fun getName(): String {
        return "Module"
    }

    @RequiresApi(api = Build.VERSION_CODES.M)
    @ReactMethod
    fun logText(text: String, successCallback: Callback, errorCallback: Callback) {
        try {
            this.successCallback = successCallback
            this.errorCallback = errorCallback

            successCallback?.invoke("Native Android에서 트리거됨: $text")
        } catch (e: Exception) {
            errorCallback(e.toString())
        }
    }
}
```

이 Kotlin 클래스는 React Native에서 제공하는 "ReactContextBaseJavaModule" 클래스를 확장합니다. `getName()` 메서드는 모듈의 이름을 반환하며, 이 이름은 JavaScript 코드에서 사용하는 이름과 일치해야 합니다.

모듈을 생성한 후에는 React Native와 등록해야 합니다. 이를 위해 패키지를 생성할 것입니다.

<div class="content-ad"></div>

"MainApplication.kt" 파일에서 "getPackages()" 메서드를 찾아 다음과 같은 코드 조각을 추가해 주세요:

```kotlin
override fun getPackages(): List<ReactPackage> =
   PackageList(this).packages.apply {
       add(Package())
   }
```

반드시 "getPackages()" 메서드가 반환하는 패키지 목록에 귀하의 패키지를 추가하고, "Package.kt" 파일은 다음과 같이 구성되어 있어야 하며, 이 파일은 "com.yourproject" 폴더 아래에 있어야 합니다.

```kotlin
class Package (): ReactPackage {
    override fun createNativeModules(reactContext: ReactApplicationContext): MutableList<NativeModule> {
        val modules = ArrayList<NativeModule>()
        modules.add(Module(reactContext))
        return modules
    }

    override fun createViewManagers(reactContext: ReactApplicationContext): List<ViewManager<*, *>> {
        return Collections.emptyList()
    }

}
```

<div class="content-ad"></div>

이제 브릿지 모듈이 등록되어 React Native 애플리케이션 내에서 사용할 준비가 되었습니다.

간단한 코드를 확인해보세요;

React에서 Android 쪽으로 값을 전송해보기

```js
const textJS = 'This Text Sent From JS';
const LogText = () => {
   Platform.OS === 'android' && Module.logText(textJS,(res: string) => {console.log(res);}, (err: string) => {console.log(err);})
};
```

<div class="content-ad"></div>

안녕하세요! 이런 경우 안드로이드에서 React Native으로 값들을 전달할 때 이 listener를 사용하실 수 있어요.

```js
    useEffect(() => {
       const eventEmitter = new NativeEventEmitter(NativeModules.ToastExample);
       let eventListener = eventEmitter.addListener('EventReminder', event => {
         console.log(event.eventProperty) // "someValue"
       });

       // Removes the listener once unmounted
       return () => {
         eventListener.remove();
       };
     }, []);
```

결론

React Native 브리징은 안드로이드와 iOS 기기에서 플랫폼별 기능에 접근하는 것을 가능케 하는 중요한 메커니즘으로 작용합니다. 자바스크립트와 네이티브 코드 간의 통신 다리를 구축함으로써, 개발자들은 네이티브 프로그래밍 언어로 처리되는 카메라, 센서 또는 기기 기능과 같은 기능을 쉽게 통합할 수 있습니다.

<div class="content-ad"></div>

이 자세한 단계들이 React Native 프로젝트에서 브릿지 모듈을 설정하는 데 도움이 될거에요. 추가 질문이나 명확화가 필요하면 언제든지 물어봐 주세요!