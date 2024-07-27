---
title: "Firebase Cloud Messaging으로 푸시 알림 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-PushNotificationsusingFirebaseCloudMessaging_0.png"
date: 2024-07-10 01:51
ogImage: 
  url: /assets/img/2024-07-10-PushNotificationsusingFirebaseCloudMessaging_0.png
tag: Tech
originalTitle: "Push Notifications using Firebase Cloud Messaging"
link: "https://medium.com/@gaffaryucel/push-notifications-using-firebase-cloud-messaging-53d3baf28683"
---


![Image](/assets/img/2024-07-10-PushNotificationsusingFirebaseCloudMessaging_0.png)

# 코틀린을 활용한 현대적 안드로이드 개발 시리즈 #6

푸시 알림은 현대 모바일 애플리케이션의 중요한 요소로, 사용자들과의 실시간 커뮤니케이션과 참여를 가능하게 합니다. Firebase Cloud Messaging (FCM)은 Android 기기로 알림을 보내는 프로세스를 간소화하여, 개발자에게 강력하고 확장 가능한 솔루션을 제공합니다. 이 튜토리얼에서는 Android 앱에 FCM을 통합하는 방법에 대해 알아보겠습니다.

이 가이드를 마치면 Firebase Cloud Messaging 설정, 수신 메시지 처리, 그리고 알림을 원활하게 표시하는 방법을 이해할 것입니다. 우리는 종속성을 효율적으로 관리하기 위해 Hilt를 활용할 것이며, 클린하고 유지보수가 용이한 코드를 보장할 것입니다. 사용자들에게 정보를 제공하고 참여도를 높이는 데 도움이 되는 푸시 알림을 구현하는 중요한 내용에 대해 깊이 파고들어 봅시다.

<div class="content-ad"></div>

# Firebase Cloud Messaging 설정하기

## 단계 1: Firebase 프로젝트 설정하기

안드로이드 앱에 Firebase Cloud Messaging (FCM)을 통합하기 전에 Firebase 프로젝트를 설정해야합니다:

## Firebase 프로젝트 만들기:

<div class="content-ad"></div>

- 파이어베이스 콘솔에 들어가주세요.
- 새로운 파이어베이스 프로젝트를 만들기 위해 설정 마법사를 따라가세요.
- 프로젝트를 만들면 google-services.json 파일을 다운로드하고 안드로이드 프로젝트의 앱 디렉토리에 넣으세요.
- Firebase 콘솔을 통해 프로젝트에 클라우드 메시징을 추가하세요.

![이미지](/assets/img/2024-07-10-PushNotificationsusingFirebaseCloudMessaging_1.png)

## Firebase 클라우드 메시징 의존성 추가:

- 앱 수준의 build.gradle 파일을 열어주세요.
- Firebase 클라우드 메시징 의존성을 추가하세요.

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 변경해주세요.


implementation 'com.google.firebase:firebase-messaging:23.0.0'


# AndroidManifest.xml 구성:

- AndroidManifest.xml 파일을 엽니다.
- 필요한 권한 및 서비스 선언을 추가하세요:


<service
    android:name=".MyFirebaseMessagingService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>


<div class="content-ad"></div>

위 코드를 사용해 Firebase 메시징 서비스를 구현하는 방법에 대해 살펴보겠습니다.

먼저, Firebase 메시징 서비스를 처리하고 종속성 주입을 위해 Hilt를 사용하는 MyFirebaseMessagingService를 만들어야 합니다. 새로운 메시지 및 등록 토큰 업데이트를 처리합니다.

```kotlin
import android.app.NotificationManager
import android.app.PendingIntent
import android.content.Context
import android.content.Intent
import android.graphics.BitmapFactory
import androidx.core.app.NotificationCompat
import com.google.firebase.messaging.FirebaseMessagingService
import com.google.firebase.messaging.RemoteMessage
import java.net.URL

class MyFirebaseMessagingService : FirebaseMessagingService() {

    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)

        // 메시지 데이터 추출
        val title = message.data["title"]
        val body = message.data["body"]
        val imageUrl = message.data["imageUrl"]
        val clickAction = message.data["click_action"]

        // 알림 클릭 동작을 위한 인텐트 생성
        val intent = Intent(clickAction).apply {
            addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
        }

        val pendingIntent = PendingIntent.getActivity(
            this, 0, intent, PendingIntent.FLAG_ONE_SHOT
        )

        // 알림 빌드
        val notificationBuilder = NotificationCompat.Builder(this, "channel_id")
            .setContentTitle(title)
            .setContentText(body)
            .setSmallIcon(android.R.drawable.ic_dialog_info)
            .setAutoCancel(true)
            .setContentIntent(pendingIntent)
            .setPriority(NotificationCompat.PRIORITY_HIGH)

        // 이미지 로드
        imageUrl?.let {
            val bitmap = BitmapFactory.decodeStream(URL(imageUrl).openConnection().getInputStream())
            notificationBuilder.setLargeIcon(bitmap)
            notificationBuilder.setStyle(NotificationCompat.BigPictureStyle().bigPicture(bitmap))
        }

        // 알림 표시
        val notificationManager =
            getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager

		notificationManager.notify(0, notificationBuilder.build())
    }
}
```

이제 당신만의 서비스 클래스 이름으로 변경하세요. 재미있는 프로젝트를 만드는 데 도움이 되길 바랄게요! 🌟

<div class="content-ad"></div>

## 푸시 알림 데이터 클래스 생성

푸시 알림에 전송될 데이터를 구조화하기 위해 PushNotification.kt라는 데이터 클래스를 정의하세요:

```kotlin
package com.yourpackage.name.model.notification

data class PushNotification(
    val data: NotificationData,
    val to: String // 알림을 수신하는 기기의 FCM 토큰이어야 합니다
)

data class NotificationData(
    val title: String,
    val message: String
)
```

## FCM API 통합을 위한 Retrofit

<div class="content-ad"></div>

FCM 서버와 상호 작용하기 위한 인터페이스를 정의하는 Retrofit을 사용해보세요:

```kotlin
package com.example.myapp.service

import com.example.myapp.model.PushNotification
import retrofit2.Response
import retrofit2.http.Body
import retrofit2.http.Headers
import retrofit2.http.POST

interface NotificationAPI {

    @Headers("Authorization: key=YOUR_SERVER_KEY", "Content-Type: application/json")
    @POST("fcm/send")
    suspend fun postNotification(
        @Body notification: PushNotification
    ): Response<Void>
}
```

## 앱에서 Hilt 설정하기:

사용자 지정 애플리케이션 클래스를 만들고 @HiltAndroidApp으로 주석을 달아보세요.

<div class="content-ad"></div>

```java
package com.example.myapp

import android.app.Application;
import dagger.hilt.android.HiltAndroidApp;

@HiltAndroidApp
public class MyApp extends Application {
}
```

힐트 통합을 위해서는 앱에 힐트를 설치하는 방법을 설명하는 내 기사를 확인해보세요. 여기서는 중복을 피하기 위해 전체 힐트 설치를 보여드리지 않겠습니다. 의존성 주입을 위해 힐트를 설정하는 자세한 지침은 내 기사를 참고해주세요.

## FirebaseRepository Interface 구현

FirebaseRepository.kt라는 인터페이스를 만들어, 알림을 보내는 메서드를 정의하세요.

<div class="content-ad"></div>

## FirebaseRepositoryImpl 구현하기

Retrofit을 사용하여 FirebaseRepository의 구현체인 FirebaseRepositoryImpl.kt를 만들어보겠습니다:

```kotlin
package com.yourpackage.name.repository

import com.yourpackage.name.model.notification.PushNotification
import com.yourpackage.name.service.NotificationAPI
import okhttp3.ResponseBody
import retrofit2.Response
import javax.inject.Inject

class FirebaseRepositoryImpl @Inject constructor(
    private val notificationAPI: NotificationAPI
) : FirebaseRepository {

    override suspend fun sendNotification(notification: PushNotification): Response<ResponseBody> {
        return notificationAPI.sendNotification(notification)
    }
}
```

<div class="content-ad"></div>

## 힐트 모듈 정의:

앱에 필요한 종속성을 제공하기 위한 모듈을 정의하세요. 예를 들어, AppModule을 만들어보세요:

```kotlin
package com.yourpackage.name.di

import com.yourpackage.name.repository.FirebaseRepository
import com.yourpackage.name.repository.FirebaseRepositoryImpl
import com.yourpackage.name.service.NotificationAPI
import dagger.Module
import dagger.Provides
import dagger.hilt.InstallIn
import dagger.hilt.components.SingletonComponent
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://fcm.googleapis.com/") // Firebase FCM base URL
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }

    @Provides
    fun provideNotificationAPI(retrofit: Retrofit): NotificationAPI {
        return retrofit.create(NotificationAPI::class.java)
    }

    @Provides
    fun provideFirebaseRepository(
        notificationAPI: NotificationAPI
    ): FirebaseRepository {
        return FirebaseRepositoryImpl(notificationAPI)
    }
}
```

## 설명

<div class="content-ad"></div>

## 파이어베이스 뷰모델 만들기

UI(프래그먼트)와 FirebaseRepository 간의 상호작용을 처리할 FirebaseViewModel.kt라는 ViewModel 클래스를 만들어보세요:

```kotlin
package com.yourpackage.name.viewmodel

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.yourpackage.name.model.notification.NotificationData
import com.yourpackage.name.model.notification.PushNotification
import com.yourpackage.name.repository.FirebaseRepository
import dagger.hilt.android.lifecycle.HiltViewModel
import kotlinx.coroutines.launch
import javax.inject.Inject

@HiltViewModel
class FirebaseViewModel @Inject constructor(
    private val repository: FirebaseRepository
) : ViewModel() {

    fun sendNotificationToDevice(
        token: String,
        title: String,
        message: String
    ) {
        val notification = PushNotification(
            NotificationData(title, message),
            token
        )
        viewModelScope.launch {
            repository.sendNotification(notification)
        }
    }
}
```

<div class="content-ad"></div>

## 파이어베이스 프래그먼트 구현

파이어베이스 알림을 발송할 수 있는 사용자 프래그먼트인 FirebaseFragment.kt를 생성하세요. 아래는 알림을 보내기 위한 버튼을 설정하는 기본 예시입니다:

```kotlin
package com.yourpackage.name.ui.fragment

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.fragment.app.viewModels
import com.yourpackage.name.databinding.FragmentFirebaseBinding
import com.yourpackage.name.viewmodel.FirebaseViewModel
import dagger.hilt.android.AndroidEntryPoint

@AndroidEntryPoint
class FirebaseFragment : Fragment() {

    private var _binding: FragmentFirebaseBinding? = null
    private val binding get() = _binding!!

    private val viewModel: FirebaseViewModel by viewModels()

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentFirebaseBinding.inflate(inflater, container, false)
        return binding.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        binding.btnSendNotification.setOnClickListener {
            val token = "FCM_DEVICE_TOKEN" // 알림을 보낼 기기의 FCM 토큰으로 대체해주세요
            val title = "알림 제목"
            val message = "이것은 알림 메시지입니다"
            viewModel.sendNotificationToDevice(token, title, message)
        }
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

해설

<div class="content-ad"></div>

- **FirebaseViewModel:** FirebaseRepository를 사용하여 알림을 보내는 비즈니스 로직을 처리하는 ViewModel입니다.
- **FirebaseFragment:** 사용자가 알림 전송 프로세스를 시작하기 위해 UI와 상호 작용하는 Fragment입니다.
- **sendNotificationToDevice:** FirebaseViewModel에 있는 함수로, FirebaseRepository를 사용하여 알림을 준비하고 보냅니다.
- **btnSendNotification:** Fragment의 레이아웃 (fragment_firebase.xml)에 있는 버튼으로, 알림 전송 프로세스를 시작합니다.

## Hilt 통합

- **@HiltViewModel:** Hilt를 사용하여 ViewModel을 주입하는 주석입니다.
- **@AndroidEntryPoint:** Hilt를 사용하여 Fragment를 주입하는 주석입니다.

프로젝트가 의존성 주입을 위해 Hilt로 올바르게 구성되어 있고 Firebase Cloud Messaging (FCM)이 앱과 올바르게 설정되어 있는지 확인하세요. 플레이스홀더 (FCM_DEVICE_TOKEN, 알림 제목 및 메시지)을 앱 로직에 적합한 실제 값으로 대체해야 합니다.

<div class="content-ad"></div>

# 결론

이러한 단계를 따르면 Firebase Cloud Messaging (FCM) 알림 전송 기능을 ViewModel 및 Fragment와 리파지토리 패턴 및 의존성 주입을 위한 Hilt를 사용하여 Android 앱에 통합할 수 있습니다. 이 설정은 관심사의 분리를 촉진하고 테스트를 용이하게하며 앱의 알림 전송 기능의 유지 보수성을 향상시킵니다. 구현을 구체적인 앱 요구 사항과 Firebase 설정에 맞게 조정하십시오.