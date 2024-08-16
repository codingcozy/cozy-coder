---
title: "React Native APK 생성 방법  디버그 APK와 릴리즈 APK 만들기"
description: ""
coverImage: "/assets/img/2024-07-07-ReactNativeGenerateAPKDebugandReleaseAPK_0.png"
date: 2024-07-07 23:16
ogImage: 
  url: /assets/img/2024-07-07-ReactNativeGenerateAPKDebugandReleaseAPK_0.png
tag: Tech
originalTitle: "React Native Generate APK — Debug and Release APK"
link: "https://medium.com/geekculture/react-native-generate-apk-debug-and-release-apk-4e9981a2ea51"
isUpdated: true
---





![image](/assets/img/2024-07-07-ReactNativeGenerateAPKDebugandReleaseAPK_0.png)

안녕하세요! 오늘은 안드로이드 Package Kit (APK)에 관해 이야기를 해볼게요. 안드로이드 운영체제에서 사용되는 APK는 휴대폰 앱의 배포 및 설치를 위한 파일 형식이에요. 윈도우 OS의 .exe 파일과 유사하게 작동하며, 안드로이드 기기에서 사용하는 파일 형식이에요.

# Debug APK

## 어떻게 활용할 수 있을까요?

<div class="content-ad"></div>

**디버그 .apk 파일은 앱을 앱 스토어에 발행하기 전에 설치하고 테스트할 수 있는 기회를 제공합니다. 다만, 발행 준비가 완료되지 않았고, 발행 전에 처리해야 할 몇 가지 작업이 남아 있습니다. 그래도 초기 배포와 테스트에 유용할 것입니다.**

**이 .apk 파일을 실행하려면 휴대폰에서 디버깅 옵션을 활성화해야 합니다.**

### 요구 사항:

- 리액트 네이티브 버전 `0.58`

<div class="content-ad"></div>

## 3 단계로 만드는 방법은?

**단계 1:** 터미널에서 프로젝트 루트로 가서 아래 명령을 실행하세요:


react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res


**단계 2:** android 디렉토리로 이동하세요:

<div class="content-ad"></div>

**안녕하세요!**

안드로이드 폴더에 들어와 주셨군요. 이제 이 안드로이드 폴더 내에서 다음 명령어를 실행해 주세요. 


./gradlew assembleDebug


그러면요! 다음 경로에 apk 파일을 찾으실 수 있습니다: `yourProject/android/app/build/outputs/apk/debug/app-debug.apk` 

언제든지 질문이 있으시면 말씀해주세요!

<div class="content-ad"></div>

# APK 릴리스

자바로 생성된 서명 키가 필요하며, 이는 안드로이드용 React Native 실행 가능 이진 파일을 생성하는 데 사용되는 키스토어 파일입니다. 다음 명령어를 사용하여 터미널에서 keytool을 사용하여 하나를 생성할 수 있습니다.


keytool -genkey -v -keystore your_key_name.keystore -alias your_key_alias -keyalg RSA -keysize 2048 -validity 10000


keytool 유틸리티를 실행하면 암호를 입력하라는 메시지가 표시됩니다. *암호를 기억해야 합니다!*

<div class="content-ad"></div>

**친구들아, 안녕하세요!** 🌟

여러분은 "your_key_name"을 자신이 원하는 이름으로 바꿀 수 있습니다. 또한 "your_key_alias"를 사용할 수 있습니다. 이 키는 보안 상의 이유로 기본 1024 대신에 2048 크기의 키를 사용합니다.

먼저, 리액트 네이티브 프로젝트 폴더의 android/app 디렉터리 아래에 your_key_name.keystore 파일을 복사하여 붙여넣어야 합니다. 

터미널에서:

```bash
mv my-release-key.keystore /android/app
```

이렇게 하면 파일을 이동시킬 수 있어요! 😉✨

팁: "my-release-key.keystore" 부분도 원하는 이름으로 변경해 보세요! 🌈

언제든지 질문이 있으시면 아주 기꺼이 도와드릴게요! 🌼🔮

<div class="content-ad"></div>

안녕하세요! 🌟

안드로이드 앱의 build.gradle 파일을 열어서 keystore 구성을 추가해야 합니다. 프로젝트를 keystore로 구성하는 두 가지 방법이 있습니다. 첫 번째는 일반적이고 안전하지 않은 방법입니다.

이 방법은 보안 문제가 있습니다. 왜냐하면 암호를 평문으로 저장하기 때문이죠. .gradle 파일에 keystore 암호를 저장하는 대신 빌드 프로세스를 명령줄에서 빌드할 때 이러한 암호를 입력하도록 지정할 수 있습니다.

Gradle 빌드 파일로 비밀번호를 요청하려면, 위의 구성을 다음과 같이 변경하시면 됩니다:

터미널 디렉토리를 android로 이동해주세요:

cd android


혹시 더 궁금한 점이 있거나 도움이 필요하시다면 언제든지 물어주세요! 🌙✨

<div class="content-ad"></div>

블로그에서 자주 언급되는 방법인데요, 윈도우에서는 


gradlew assembleRelease


리눅스와 맥 운영체제에서는


./gradlew assembleRelease


를 입력하시면 APK 생성 과정이 끝나게 됩니다. 만들어진 APK 파일은 `android/app/build/outputs/apk/app-release.apk` 경로에서 확인하실 수 있습니다. 이 APK는 실제 앱으로, 스마트폰으로 보내거나 구글 플레이 스토어에 업로드할 수 있습니다. 축하드립니다, 여러분은 리액트 네이티브 릴리스 빌드 APK를 성공적으로 생성하셨어요. 🌟