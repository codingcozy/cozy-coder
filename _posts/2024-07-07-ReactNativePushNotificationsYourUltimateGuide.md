---
title: "React Native 푸시 알림 궁극적인 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_0.png"
date: 2024-07-07 23:27
ogImage: 
  url: /assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_0.png
tag: Tech
originalTitle: "React Native Push Notifications: Your Ultimate Guide"
link: "https://medium.com/@abbasradio1234.ar/react-native-push-notifications-your-ultimate-guide-44d11009bb76"
isUpdated: true
---





안녕하세요, 지니어스 분들!! 현대 모바일 앱 개발의 빠른 세계에서 푸시 알림은 사용자들과 시기적절한 업데이트를 전달하는 데 중요한 역할을 합니다. 이 포스트에서는 React Native 앱에서 react-native-push-notification 모듈을 사용하여 푸시 알림을 설정하는 방법에 대해 알아보겠습니다. Firebase 콘솔을 활용하여 알림을 전송하고 앱 내에서 이를 수신하는 방법을 살펴볼 것입니다. 또한 React Native 애플리케이션에서 푸시 알림을 관리하는 과정을 깊이 파헤쳐볼 것입니다. 이는 앱 내에서 푸시 알림을 통해 전송된 데이터에 액세스하고, 앱이 비활성 상태이거나 종료된 경우 앱을 활성화하는 것을 포함합니다.

# 푸시 알림이란

푸시 알림은 모바일 애플리케이션에서 사용자의 장치로 전송되는 메시지 또는 알림으로, 앱이 활성 상태가 아닐 때에도 전송됩니다. 이러한 알림에는 업데이트, 알림, 프로모션 또는 개인화된 메시지와 같은 다양한 정보가 포함될 수 있습니다. 이러한 알림은 장치의 홈 화면, 잠금 화면 또는 사용자 설정에 따라 알림 센터에서 배너, 알림 또는 배지로 나타납니다.

<div class="content-ad"></div>

![React Native Push Notifications Guide](/assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_1.png)

## 리액트 네이티브 앱을 위한 푸시 알림 설정하기

이 튜토리얼에서는 안드로이드 기기를 위한 리액트 네이티브 애플리케이션에서 푸시 알림 수신을 설정하는 데 중점을 둘 것입니다. 제공된 코드는 안드로이드와 iOS 플랫폼 모두와 호환되지만 iOS의 경우 추가 구성 단계가 필요합니다. iOS 설정에 대해 다루는 별도의 튜토리얼에서 해당 단계를 다룰 것입니다.

새로운 리액트 네이티브 앱을 만들려면 터미널에서 다음 명령을 사용할 수 있습니다:

<div class="content-ad"></div>

모래시계

Android 애플리케이션을 실행할 수 있어요. 다음 코드 스니펫을 사용해 보세요!

```bash
npm run android
```

푸시 알림 Dependency를 만드세요.

<div class="content-ad"></div>

이 설정에서는 React Native 애플리케이션 내에서 푸시 알림 기능을 처리하기 위해 특별히 설계된 'react-native-push-notification' 패키지를 활용합니다. 이 패키지는 Node Package Manager (npm)을 통해 설치할 수 있습니다.

```js
npm install --save react-native-push-notification
 or
yarn add react-native-push-notification
```

# Firebase와 우리 애플리케이션 등록

Firebase 대시보드에서 Android 또는 iOS 애플리케이션을 등록하여 푸시 알림을 활성화해야 합니다. 이번 데모에서는 Android 앱에 대한 푸시 알림 구현에 중점을 두겠습니다.

<div class="content-ad"></div>

안녕하세요! 오늘은 Firebase 대시보드에 Android 애플리케이션을 등록하는 방법에 대해 이야기할 거예요.

1. Android 애플리케이션을 등록하고 이름을 지어줍니다.
2. 이미 프로젝트를 선택했으면 해당 프로젝트를 선택하거나 새로 만듭니다.
3. "앱 추가" 버튼을 클릭하여 프로젝트에 새로운 플랫폼을 추가합니다.
4. Android 아이콘을 클릭하여 Android 플랫폼을 선택합니다.
5. Android 앱의 패키지 이름을 입력합니다. 이것은 일반적으로 React Native 프로젝트의 "android/app/build.gradle" 파일에서 찾을 수 있어요.
6. (선택사항) 앱 별명 및 디버그 서명 인증서 SHA-1과 같은 다른 선택적 세부 사항을 입력할 수 있어요.
7. "앱 등록" 버튼을 클릭하여 등록 프로세스를 완료합니다.

아래는 서명 보고서를 생성하는 명령어에요.

```bash
cd android

./gradlew signingReport
```

<div class="content-ad"></div>

![React Native Push Notifications](/assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_3.png)

Hey there! After you've downloaded the google-services.json file from the Firebase console, make sure you place it in the root directory of your application module.

Next, you'll want to add the necessary code provided by Firebase documentation into the project-level build.gradle file (located at **`project`/build.gradle**). This code snippet will configure your project to work seamlessly with Firebase services:

```js
buildscript {
    ext {
        ...
        buildToolsVersion = "34.0.0"
        minSdkVersion = 21
        compileSdkVersion = 34
        targetSdkVersion = 34
        ndkVersion = "25.1.8937393"
        kotlinVersion = "1.8.0"
        ...
    }
    
    repositories {
        ...
        google() // Remember to add this line
        mavenCentral()
        ...
    }
    
    dependencies {
        ...
        classpath("com.google.gms:google-services:4.4.0") // Google Service Plugin
        ...
    }
}

apply plugin: "com.facebook.react.rootproject"
```

Hope this helps enhance your Firebase integration! Feel free to reach out if you have any more questions. 😉

<div class="content-ad"></div>

프로젝트 수준의 구성을 완료한 후, `project`/`app-module`/build.gradle에 있는 애플리케이션 수준의 build.gradle 파일을 생성하십시오.

```js
...
apply plugin: "com.google.gms.google-services"
...

dependencies {
    ...
    implementation(platform("com.google.firebase:firebase-bom:32.7.1"))
    ...
}
```

React Native 안드로이드 애플리케이션에 푸시 알림을 통합하려면, 다음 코드 조각을 AndroidManifest.xml 파일에 대체하십시오.

```js
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <!-- 알림 액세스 요청을 위한 권한 -->
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
    <application
        android:name=".MainApplication"
        android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:allowBackup="false"
        android:theme="@style/AppTheme">
        <meta-data android:name="com.dieam.reactnativepushnotification.notification_foreground"
            android:value="true" />
        <meta-data android:name="com.dieam.reactnativepushnotification.channel_create_default"
            android:value="true" />
        <meta-data android:name="com.dieam.reactnativepushnotification.notification_color"
            android:resource="@color/white" />
        <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationActions"
            android:exported="true" />
        <receiver
            android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationPublisher"
            android:exported="true" />
        <receiver
            android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationBootEventReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.intent.action.QUICKBOOT_POWERON" />
                <action android:name="com.htc.intent.action.QUICKBOOT_POWERON" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|screenSize|smallestScreenSize|uiMode"
            android:launchMode="singleTask"
            android:windowSoftInputMode="adjustResize"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

<div class="content-ad"></div>

마지막으로 android/app/src/res/values/colors.xml으로 이동하세요. 해당 파일이 존재하지 않는다면 만들어주세요. 이 파일은 Android 장치에서 통지의 색상을 결정합니다.

```js
<resources>
    <color name="white">#FFF</color>
</resources>
```

```js
// 프로젝트 폴더로 이동하고 다음 단계를 따라주세요
// android 폴더로 이동
cd android

// 다음 명령어를 사용하여 오류 또는 종속성 문제를 확인하세요
./gradlew clean
```

현재까지 모든 종속성이 추가되었고, 이제 다음 단계인 알림 수신으로 넘어갈 예정입니다.

<div class="content-ad"></div>

# 로컬 알림 설정

당신의 React Native 프로젝트에서 App.tsx 파일을 열거나 루트 디렉토리에 새 파일을 생성하세요. react-native-push-notification에서 PushNotification 모듈을 가져와 초기화하세요.

```js
import PushNotification from 'react-native-push-notification'

const LocalNotification = () => {
    const key = Date.now().toString(); // 키는 매번 고유해야 합니다
    PushNotification.createChannel(
        {
            channelId: key, // (필수)
            channelName: "로컬 메시지", // (필수)
            channelDescription: "로컬 메시지용 알림", // (선택 사항) 기본값: 정의되지 않음.
            importance: 4, // (선택 사항) 기본값: 4. Android 알림 중요도의 정수 값
            vibrate: true, // (선택 사항) 기본값: true. true이면 기본 진동 패턴을 생성합니다.
        },
        (created) => console.log(`createChannel returned '${created}'`) // (선택 사항) 채널이 생성된 경우를 반환합니다. false는 이미 존재함을 의미합니다.
    );
    PushNotification.localNotification({
        channelId: key, // 이것은 createchannel 내의 channelid와 동일해야 합니다.
        title: '로컬 메시지',
        message: '로컬 메시지!!',
    })
};

export default LocalNotification
```

이제 위 파일을 App.tsx에 가져와서 사용하세요

<div class="content-ad"></div>


JSX
import React from 'react';
import {
  Button,
  SafeAreaView,
  ScrollView,
  StatusBar,
  Text,
  useColorScheme,
  View,
} from 'react-native';
import {
  Colors,
} from 'react-native/Libraries/NewAppScreen';
import LocalNotification from './Notification';

function App(): React.JSX.Element {
  const isDarkMode = useColorScheme() === 'dark';

  const backgroundStyle = {
    backgroundColor: isDarkMode ? Colors.darker : Colors.lighter,
  };

  return (
    <SafeAreaView style={backgroundStyle}>
      <StatusBar
        barStyle={isDarkMode ? 'light-content' : 'dark-content'}
        backgroundColor={backgroundStyle.backgroundColor}
      />
      <ScrollView
        contentInsetAdjustmentBehavior="automatic"
        style={backgroundStyle}>
        <View
          style={{
            backgroundColor: isDarkMode ? Colors.black : Colors.white,
            flex: 1,
            justifyContent: 'center',
            alignItems: 'center',
            alignContent: 'center',
          }}>
          <Text> Push Notification!! </Text>
          <Button title={'Click Here'} onPress={LocalNotification} />
        </View>
      </ScrollView>
    </SafeAreaView>
  );
}
export default App;


Clicking on the 'Click Here' button will render a local notification successfully.

Now, let's move on to setting up Remote Notifications.

Add the following lines to `PermissionsAndroid.js` in `node_modules/react-native/Libraries/PermissionsAndroid/PermissionsAndroid.js`:


<div class="content-ad"></div>

```js
...
export type Rationale = {
  ...
  POST_NOTIFICATIONS?: string,
};

const PERMISSION_REQUEST_RESULT = Object.freeze({
  ...
  POST_NOTIFICATIONS: 'android.permission.POST_NOTIFICATIONS',
});
```

현재 프로젝트에서는 AndroidManifest.xml 파일에서 이미 이를 초기화했습니다.

RemoteNotification.tsx 파일을 만들어서 원격 알림을 구성하기 위한 다음 코드 라인을 추가해주세요.

```js
import { useEffect } from 'react';
import { PermissionsAndroid, Platform } from 'react-native';
import PushNotification from 'react-native-push-notification';

const checkApplicationPermission = async () => {
    if (Platform.OS === 'android') {
        try {
            await PermissionsAndroid.request(
                PermissionsAndroid.PERMISSIONS.POST_NOTIFICATIONS,
            );
        } catch (error) {
            console.error(error)
        }
    }
};

const RemoteNotification = () => {
    useEffect(() => {
        checkApplicationPermission();
        // 이 함수를 사용하여 로컬 알림을 렌더링하므로 동일한 알림에 대해 여러 번 알림을 받지 않습니다.
        // 모든 FCM 테스트 서버 알림에 대해 동일한 채널 ID를 사용합니다.
        
        PushNotification.getChannels(function (channel_ids) {
            channel_ids.forEach((id) => {
                PushNotification.deleteChannel(id)
            })
        });
        
        PushNotification.configure({
            // (optional) 토큰이 생성될 때 호출됨 (iOS 및 Android)
            onRegister: function (token) {
                console.log('TOKEN:', token);
            },

            // (필수) 원격 또는 로컬 알림이 열리거나 수신될 때 호출됨
            onNotification: function (notification) {
                const { message, title, id } = notification;
                let strTitle: string = JSON.stringify(title).split('"').join('');
                let strBody: string = JSON.stringify(message).split('"').join('');
                const key: string = JSON.stringify(id).split('"').join('');
                
                PushNotification.createChannel(
                {
                    channelId: key, // (필수 및 고유해야 함)
                    channelName: "원격 메시지", // (필수)
                    channelDescription: "원격 메시지를 위한 알림", // (선택 사항) 기본값: 정의되지 않음
                    importance: 4, // (선택 사항) 기본값: 4. Android 알림 중요도의 정수 값
                    vibrate: true, // (선택 사항) 기본값: true. 참이면 기본 진동 패턴을 생성
                },
                (created) => console.log(`createChannel returned '${created}'`) // (선택 사항) 콜백은 채널이 생성되었는지 여부를 반환하며, false는 이미 존재한다는 것을 의미
                );
                
                PushNotification.localNotification({
                    channelId: key, // 이는 createchannel에서의 channelId과 동일해야합니다.
                    title: strTitle,
                    message: strBody,
                });
                
                console.log('원격 알림 ==>', title, message, id, notification);
                // 알림 처리는 여기서 진행
            },
            
            // Android 전용: GCM 또는 FCM 발신자 ID
            senderID: '1234567890',
            popInitialNotification: true,
            requestPermissions: true,
        });
    }, []);
    return null;
};
export default RemoteNotification;
```

<div class="content-ad"></div>

이 파일에서는 checkApplicationPermission이라는 함수를 만들었습니다. 이 함수는 일반적으로 Android 버전 13 이상에서 설치된 새 애플리케이션에 기본적으로 제공되지 않는 알림에 대한 액세스 권한을 요청합니다.

알림에 대한 액세스를 얻은 후에는 PushNotification.configure를 구성합니다. 여기서 우리는 먼저 디바이스 토큰을 생성하여 디바이스가 원격 알림을 받기 위해 필요합니다.

우리가 알림을 받을 때 onNotification 함수가 렌더링됩니다. 특히 우리 프로젝트에서는 원격 알림을 받을 때 로컬 알림을 렌더링합니다. 때로는 Foreground 상태에서 팝업 알림을받지 못할 수 있기 때문에 이러한 경우에는 여기서 해당 기능을 렌더링합니다.

원격 알림 수신

<div class="content-ad"></div>

우리의 원격 알림을 받기 위해서 몇 가지 추가 변경이 필요합니다. 앱 활동에는 세 가지 상태가 있습니다:

- Foreground State
- Background State
- Quit State

따라서 이러한 상태에 대해 App.tsx에서 변경 사항을 적용할 것입니다.

App.tsx에서 RemoteNotification을 import하고 렌더링할 것입니다.

<div class="content-ad"></div>

이제 모든 수정 사항을 적용했으니 원격 알림을 받을 준비가 되었습니다.

이제 FCM 콘솔로 이동하여 알림을 보내겠습니다.

![React Native Push Notifications Your Ultimate Guide](/assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_4.png)

<div class="content-ad"></div>

푸시 알림을 설정한 후, 원격 알림을 성공적으로 받게 됩니다.

![image 5](/assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_5.png)

![image 6](/assets/img/2024-07-07-ReactNativePushNotificationsYourUltimateGuide_6.png)

본 강좌에서는 react-native-push-notification 패키지를 사용하여 React Native 앱에 푸시 알림을 통합하는 필수 단계를 다뤘습니다. 알림 서비스를 위해 Firebase를 설정하는 것부터 로컬 알림을 구성하는 것까지, 이제 프로젝트에 효과적으로 푸시 알림을 구현할 튼튼한 기반이 마련되었습니다.

<div class="content-ad"></div>

React Native 개발의 무한한 가능성을 탐험하신다면, 푸시 알림은 사용자와 사용자 경험을 향상시키는 강력한 도구라는 것을 기억해 주세요.

이 튜토리얼이 React Native 프로젝트에 푸시 알림을 통합하는 데 유용한 통찰과 실용적인 가이드를 제공했기를 바라며, 만약 중간에 질문이 생기거나 어려움을 겪게 된다면, 지원이 필요할 때 언제든지 저에게 연락해주세요.

즐거운 코딩하세요! 당신의 앱이 푸시 알림의 힘으로 번창하기를 기원합니다! 🌟