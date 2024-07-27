---
title: "2024년 iOS 및 Android용 React Native 스플래시 스크린 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_0.png"
date: 2024-07-10 01:23
ogImage: 
  url: /assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_0.png
tag: Tech
originalTitle: "Set up Splash screen in React Native for iOS and Android | 2024"
link: "https://medium.com/@svbala99/set-up-splash-screen-in-react-native-for-ios-and-android-2023-dbedb87fe75e"
---


안녕하세요! 여러분, 잘 지내시죠? 오늘은 리액트 네이티브 앱에서 안드로이드와 iOS용 스플래시 화면을 설정하는 경험을 공유하려고 해요. 함께 알아보도록 하죠..

![Splash Screen](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_0.png)

이를 위해 라이브러리가 필요한데요. 많이 사용되는 https://www.npmjs.com/package/react-native-splash-screen 를 사용할 거에요. 다운로드 양과 활발한 활동성 때문에 이 라이브러리를 선택했어요.

스플래시 화면은 사용자가 앱의 다른 기능을 이용하기 전에 가장 먼저 나타나는 화면이에요. 모바일 어플리케이션의 브랜드 이름과 아이콘이 사용자에게 기억되도록 만드는 가장 좋은 방법 중 하나라고 할 수 있어요. 더 자세한 내용은 여기를 참고해주세요: https://docs.expo.dev/develop/user-interface/splash-screen/

<div class="content-ad"></div>

React Native에서 스플래시 화면을 만드는 것에는 많은 이점이 있습니다. 예를 들어, API에서 데이터를 로드하는 시나리오를 생각해보세요. 사용자가 기다릴 때 로더를 표시하는 것은 좋은 사용자 경험이 될 수 있습니다. 앱이 시작될 때 로더를 표시하면 사용자가 앱이 준비될 때까지 기다릴 동안 조직적이고 잘 디자인된 화면을 제공할 수 있습니다.

이 react-native-splash-screen 데모에서 우리는 안드로이드와 iOS용 플래시 화면을 만들 것입니다. 이 튜토리얼은 적절한 이미지 사이즈를 준비하는 방법, 필요한 파일을 업데이트하는 방법, 그리고 앱이 로드되면 스플래시 화면을 숨기는 방법을 안내합니다. 완성된 앱은 아래 스크린샷처럼 보일 것입니다:

![스플래시 스크린 설정](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_1.png)

![스플래시 스크린 설정](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_2.png)

<div class="content-ad"></div>

모바일 애플리케이션을 위한 스플래시 화면을 만드는 것은 꽤 복잡할 수 있습니다. 스플래시 화면 해상도의 일관성 부족으로 일부 기기에서 디스플레이 문제가 발생하길 원치 않으시겠죠. 예를 들어, 안드로이드 기기의 요구 사항은 iOS와 완전히 다릅니다. 대부분의 경험이 풍부한 디자이너들은 두 기기용 필요한 스플래시 화면 해상도를 처음부터 만들어낼 수 있습니다.

하지만 Android와 iOS 모두를 위한 스플래시 화면을 만들 수 있는 다양한 타사 도구들이 많이 있습니다. 이번 튜토리얼에서는 Android와 iOS 앱을 위한 아이콘과 이미지를 생성하는 온라인 플랫폼인 'App Icon Generator'를 사용할 것입니다.

먼저, Appicon 사이트로 이동해주세요. 이미지를 상자에 끌어다 놓고, 기본 크기로 4배를 선택해주세요. iOS와 Android를 확인하고, '생성하기'를 클릭해주세요:

![스플래시 화면 설정](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_3.png)

<div class="content-ad"></div>

그 다음에, 다운로드한 파일을 압축 해제하고 iOS 및 Android 폴더를 복사하여 시작 프로젝트를 복제한 assets 디렉토리의 assets 폴더에 붙여넣으세요:

![이미지](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_4.png)

```js
// 의존성 패키지를 설치하려면 다음 명령을 실행하세요
/* npm */
npm i react-native-splash-screen --save

/* yarn */
yarn add react-native-splash-screen
```

# iOS용 스플래시 화면 만들기

<div class="content-ad"></div>


다음은 AppDelegate.mm 파일로 이동하여 다음 코드를 업데이트하십시오. 코드 #import "RNSplashScreen"을 추가하고 기본적으로 [RNSplashScreen show]로 대기 화면을 표시하도록 설정하십시오.

```js
#import "AppDelegate.h"
#import "RNSplashScreen.h"

#import <React/RCTBundleURLProvider.h>

@implementation AppDelegate
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  self.moduleName = @"Aptster";
  // 별도의 초기 속성을 아래 사전에 추가할 수 있습니다.
  // React Native에서 사용하는 ViewController로 전달될 것입니다.
  self.initialProps = @{};
  BOOL ret = [super application:application didFinishLaunchingWithOptions:launchOptions];
  if (ret == YES)
  { 
    [RNSplashScreen show];
  }
  return ret;
}

...............
...
...
```

다음으로, Xcode에서 프로젝트 워크스페이스를 열고 이미지 클릭하여 Image 아래 어디든 마우스 오른쪽 버튼을 클릭하고 New Image Set을 선택합니다. 이미지 이름을 "splash"로 설정하고 에셋 폴더를 열어서 iOS 폴더로 이동합니다. iOS의 세 개의 이미지를 1x, 2x 및 3x로 명명된 Xcode의 세 개 상자 위에 끌어다 놓으십시오:


<div class="content-ad"></div>

Markdown 형식으로 변경하세요.

다음으로, LaunchScreen.storyboard를 선택합니다. View Controller Scene 'View Controller' View를 선택하고 SplashScreen 및 Powered by React Native 레이블을 클릭한 후 키보드에서 Delete 키를 누릅니다.

다음으로, View를 선택하고 Xcode의 오른쪽 상단 부분에있는 ruler 아이콘을 클릭합니다. Safe Area Layout Guide 옵션을 선택 해제하고, 객체 검색 입력 필드에 "image view"를 입력한 후, "image view"를 View 캔버스로 드래그합니다:

![image](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_6.png)

<div class="content-ad"></div>

이미지 뷰가 설정되었으니, 이미지 속성 아이콘을 클릭하고 이미지를 "splash"로 변경해주세요. 아래와 같이 콘텐츠 모드를 "aspect fit"으로 설정해주세요:

\!\[Splash Image\]\(/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_7.png\)

# 안드로이드용 스플래시 화면 만들기

MainActivity.kt 파일에 다음 줄을 추가하세요: -

<div class="content-ad"></div>


안녕하세요! 요청하신 내용을 한 번 확인해보세요.

```js
package com.techoedgecorp.aptster
import android.os.Bundle; // add this
import com.facebook.react.ReactActivity
import com.facebook.react.ReactActivityDelegate
import com.facebook.react.defaults.DefaultNewArchitectureEntryPoint.fabricEnabled
import com.facebook.react.defaults.DefaultReactActivityDelegate
import org.devio.rn.splashscreen.SplashScreen;

class MainActivity : ReactActivity() {

   override fun onCreate(savedInstanceState: Bundle?) {
    SplashScreen.show(this);  // add this
    super.onCreate(null)
  }
```

그리고, app/src/main/res/layout에 launch_screen.xml 파일을 생성하세요. 만약 layout 폴더가 없다면 새로 생성해주세요.

```js
<!-- launch_screen.xml -->

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ImageView android:layout_width="match_parent" android:layout_height="match_parent" android:src="@drawable/launch_screen" android:scaleType="centerCrop" />
</RelativeLayout>
```

![이미지](/assets/img/2024-07-10-SetupSplashscreeninReactNativeforiOSandAndroid2024_8.png)


<div class="content-ad"></div>

위 이미지를 Markdown 형식으로 나타내어 launch_screen.png 이미지를 적절한 폴더에 추가하세요.

메인 `styles.xml`에 다음 줄을 추가하여 안드로이드 시스템에서 제공하는 기본 스플래시 스크린을 투명하게 만드세요. 안드로이드 12부터는 사용자가 앱 아이콘을 클릭할 때 안드로이드 시스템 앱 런처가 기본 스플래시 스크린을 제공합니다. 그렇지 않으면 우리의 스플래시 스크린보다 먼저 안드로이드 기본 스플래시 스크린이 나타날 것입니다.

```js
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.DayNight.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="android:editTextBackground">@drawable/rn_edit_text_material</item>
        <item name="android:windowIsTranslucent">true</item> 
            <!-- 위 줄을 추가하세요. -->
    </style>

</resources>
```

# 앱이로드된 후 스플래시 스크린 감추기

<div class="content-ad"></div>


### 해결 방법 — 2023년 6월 23일, React Native 버전 “0.72.0”에서 스플래시 화면이 숨겨지지 않는 문제가 발생했습니다. 그래서 우리는 iOS에서 다음 코드를 작성했습니다. 그렇지 않으면 [RNSplashScreen show]; 만으로도 충분했을 것입니다.

```js
BOOL ret = [super application:application didFinishLaunchingWithOptions:launchOptions];
if (ret == YES)
{ 
  [RNSplashScreen show];
}
return ret;
```

# 결론


<div class="content-ad"></div>

그것이에요. 우리가 iOS와 Android용으로 스플래시 애셋과 스플래시 화면을 만들었어요. 고마워요.