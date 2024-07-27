---
title: "Play Core 라이브러리에서 마이그레이션 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-MigratingfromPlayCoreLibrary_0.png"
date: 2024-07-07 03:13
ogImage: 
  url: /assets/img/2024-07-07-MigratingfromPlayCoreLibrary_0.png
tag: Tech
originalTitle: "Migrating from Play Core Library"
link: "https://medium.com/proandroiddev/migrating-from-play-core-library-0b4afd3fca1a"
---


![image](/assets/img/2024-07-07-MigratingfromPlayCoreLibrary_0.png)

안녕하세요! 구글 플레이 스토어로부터 최근 이메일을 받으셨을 수도 있습니다. 메일 내용에는 다음과 같은 내용이 포함되어 있습니다:

겁이 나실 수도 있겠죠?

걱정 마세요. 실제로 생각보다 쉬울 거예요. 

<div class="content-ad"></div>

## 실제 변경 내용은 무엇일까요?

요약하자면, 구글은 2022년 초에 플레이 코어 라이브러리의 새 버전을 릴리스하는 것을 중단했습니다.

![이미지](/assets/img/2024-07-07-MigratingfromPlayCoreLibrary_1.png)

또한 2022년 4월부터 원래의 플레이 코어 라이브러리를 네 개의 별도 라이브러리로 분리했습니다:

<div class="content-ad"></div>

- **Play Assets Delivery Library**  
- **Play Feature Delivery Library**  
- **Play In-App Reviews Library**  
- **Play In-App Updates Library**  

Each library has its own unique functionality and set of responsibilities. 

As the older core Play library only supports a specific API level, it is essential to transition your application to utilize the newer libraries that accommodate the latest API levels. 

In order to effectively migrate, you must identify the specific functionalities from the original core Play library that your application relies on and then integrate the corresponding new library. For instance, if your app includes logic to inform users about available updates, you should implement the Play In-App Updates Library.

<div class="content-ad"></div>

여기서 두 가지 사례를 제시하겠습니다:

- 네이티브 안드로이드 애플리케이션
- 플러터 애플리케이션

## Use Case - Android

만약 코틀린이나 자바로 작성된 네이티브 안드로이드 애플리케이션을 보유하고 계시다면, 다음을 수행해야 합니다:

<div class="content-ad"></div>

- 앱 레벨의 build.gradle 파일을 열어주세요.
- 아마도 dependencies 블록 아래에 다음 코드를 보실 수 있습니다:

```js
implementation 'com.google.android.play:core-ktx:1.8.1'
```

3. 해당 코드를 제거하고 이전 core 라이브러리에 맞게 교체해야 합니다.

4. Play In-App-Updates 라이브러리를 사용해야 하는 경우, dependencies 블록에 아래 내용을 추가해야 합니다.

<div class="content-ad"></div>


implementation 'com.google.android.play:app-update:2.1.0'
//If you are using Kotlin, add the following dependency to your application
implementation 'com.google.android.play:app-update-ktx:2.1.0'


5. Rebuild your application and make sure everything works properly.

## Use Case - Flutter

Flutter is a versatile framework that supports both Android and iOS platforms. When upgrading the core play library in your Flutter app, check the libraries listed in your pubspec.yaml file: 


<div class="content-ad"></div>

위에서 볼 수 있듯이 해당 애플리케이션은 새로운 버전의 애플리케이션이 사용 가능할 때 사용자에게 알리는데 관련된 in_app_update 라이브러리에 의존합니다. in_app_update의 pub.dev 변경 로그 페이지로 이동하면 다음과 같은 내용을 확인할 수 있습니다:

![image](/assets/img/2024-07-07-MigratingfromPlayCoreLibrary_2.png)

따라서 우리는 pubspec.yaml 파일을 해당 버전으로 업데이트해야 합니다 (최소한).

<div class="content-ad"></div>


dependencies:
  flutter:
    sdk: flutter
  ...
  in_app_update: ^4.1.0


Just run `Pub get` and you should be all set! 🌟✨