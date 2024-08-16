---
title: "MVVM 클린 아키텍처에서 Kotlin Flow와 Firebase Realtime Remote Config 사용 방법"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-09 10:41
ogImage: 
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Using Kotlin Flow and Firebase Realtime Remote Config in an MVVM Clean Architecture"
link: "https://medium.com/@jaafoura.kais/using-kotlin-flow-and-firebase-realtime-remote-config-in-an-mvvm-clean-architecture-e9934f4b76ba"
isUpdated: true
---





# 소개

휴대 전화 앱 개발의 항상 변화하는 세계에서는 사용자 요구에 유연하고 민감하게 대응하는 것이 중요합니다. 자주 업데이트와 재배포가 필요하지 않은 원격 애플리케이션 사용자 정의 능력은 이 유연성을 달성하는 데 중요한 구성 요소입니다. Firebase Realtime Remote Config는 이 문제에 강력한 해결책을 제공합니다.

이 글에서는 Firebase Realtime Remote Config를 안드로이드 애플리케이션에 통합하는 과정을 MVVM(Model-View-ViewModel) 아키텍처 패턴의 원칙을 준수하면서 Kotlin Flow의 적응성을 활용합니다. 이 가이드의 끝에는 깔끔한 아키텍처를 위해 코드를 작성하는 방법과 원격 데이터를 쉽게 검색하고 적용하는 방법에 대해 강력한 아이디어를 갖게 될 것입니다.

# 준비물

<div class="content-ad"></div>

**GCP에서 실시간 원격 구성 활성화하기**

안녕하세요! 이번 가이드를 따라가기 전에 몇 가지 필수 조건을 알려드릴게요. 파이어베이스 원격 구성 실시간 API를 활성화시켜두셨는지 확인해주세요.

1. 구글 클라우드 플랫폼 콘솔을 열어주세요.
2. 파이어베이스 프로젝트를 선택해주세요.
3. API 및 서비스 섹션으로 이동해주세요.
4. "파이어베이스 원격 구성 실시간 API"를 검색하고 활성화해주세요.

이제 코드로 넘어가기 전에 위 사항을 확인해주세요. 문제가 발생하거나 궁금한 점이 있으면 언제든지 질문해주세요!

<div class="content-ad"></div>

**중요 사항:** 이 API는 이미 활성화되어 있어야 하지만, 두 번 확인하는 것이 좋습니다.

# Clean Architecture: 견고한 기반

Clean Architecture는 우리의 구현을 위한 견고한 구조를 제공합니다:

- 데이터 레이어: Firebase Remote Config와 상호 작용하여 초기 값 가져오기 및 실시간 업데이트를 위한 리스너 설정.
- 도메인 레이어: config 매개변수와 관련된 비즈니스 로직을 캡슐화하여 관심사의 깔끔한 분리를 보장합니다.
- 프레젠테이션 레이어: config 변경에 반응하여 MVVM 및 Jetpack Compose를 사용하여 실시간으로 UI를 업데이트합니다.

<div class="content-ad"></div>

# 구현: 실시간 업데이트의 작동 방식

- 프로젝트 설정:

- 필요한 Firebase 종속성이 있는지 확인하십시오:

```js
dependencies {
    // Firebase 플랫폼용 BoM 가져오기
    implementation(platform("com.google.firebase:firebase-bom:32.8.0"))

    // 원격 구성 및 분석 라이브러리에 대한 종속성 추가
    // BoM을 사용할 때 Firebase 라이브러리 종속성에 버전을 지정하지 않습니다
    implementation("com.google.firebase:firebase-config")
    implementation("com.google.firebase:firebase-analytics")
}
```

<div class="content-ad"></div>

- 당신의 애플리케이션에서 Firebase를 초기화하세요.

FirebaseRemoteConfig의 addOnConfigUpdateListener 메소드와 callbackFlow를 사용하여 Remote Config의 Repository를 구현하세요.

callbackFlow 함수는 우리의 경우에는 문자열 Flow의 인스턴스를 생성합니다.

업데이트가 발생하면 새 값이 지정된 키에 대해 trySend를 사용하여 전송하려고 노력합니다. awaitClose는 Flow가 취소될 때 정리 작업을 정의하는 데 사용됩니다.

<div class="content-ad"></div>

Using awaitClose in your code is essential to avoid any lingering resources that may cause a decrease in app performance or unexpected issues over time.

```kotlin
class FirebaseRealtimeRemoteConfigRepository @Inject constructor(private val firebaseRemoteConfig: FirebaseRemoteConfig) :
  RealtimeRemoteConfigRepository {
  override fun getConfig(key: String): Flow<String> = callbackFlow {
    val listener = FirebaseRemoteConfig.OnConfigUpdateListener {
      trySend(firebaseRemoteConfig.getString(key))
    }
    firebaseRemoteConfig.addOnConfigUpdateListener(listener)
    awaitClose {
      firebaseRemoteConfig.removeOnConfigUpdateListener(listener)
    }
  }
}
```

- Define an interface for real-time configuration interaction in the Domain Layer:

```kotlin
interface RealtimeRemoteConfigRepository {
    fun getConfig(key: String): Flow<String>
}
```

<div class="content-ad"></div>

- 이제 Hilt 모듈을 구현하여 인터페이스를 데이터 레이어의 구현과 연결할 수 있습니다.

```kotlin
@Module
abstract class RealTimeRemoteConfigModule {
    @Binds
    abstract fun providesRealTimeRemoteConfig(repository: FirebaseRealtimeRemoteConfigRepository): RealtimeRemoteConfigRepository
}
```  

- 구성값을 가져와 처리하는 로직을 캡슐화하기 위한 유스케이스를 생성하세요:

```kotlin
class GetRemoteConfigUseCase @Inject constructor(
    private val repository: RealtimeRemoteConfigRepository
) {
    operator fun invoke(key: String): Flow<String> = repository.getConfig(key)
}
```  

<div class="content-ad"></div>

Presentation Layer (ViewModel):

- Use case를 주입하고 설정 값을 Flow로 노출해주세요:

```kotlin
@HiltViewModel
class MainViewModel @Inject constructor(
    getRemoteConfigUseCase: GetRemoteConfigUseCase
) : ViewModel() {
    val featureEnabled = getRemoteConfigUseCase("enable_new_feature")
        .map { it.toBoolean() } // 변환 예시
        .stateIn(viewModelScope, SharingStarted.Eagerly, false)
}
```

Jetpack Compose (UI):

<div class="content-ad"></div>

- 내 적응 가능과 UI를 반응적으로 업데이트하세요:

```kotlin
@Composable
fun MyScreen(viewModel: MainViewModel = hiltViewModel()) {
    val featureEnabled by viewModel.featureEnabled.collectAsState()
    // ... featureEnabled을 사용하여 UI 요소를 조건부로 렌더링합니다.
}
```

**결론**

실시간 원격 구성은 클래식 원격 구성 시스템에 비해 상당한 이점을 제공합니다. 일반적으로 업데이트가 전파되기까지 최소 12분이 소요되는 전통적인 방식과 달리, 실시간 원격 구성은 변경 사항을 즉시 전달합니다. 이는 앱 구성의 수정 사항이 사용자 화면에서 즉시 반영되어 수동 새로고침이나 긴 대기 기간이 필요하지 않다는 것을 의미합니다.
이 실시간 기능은 앱의 반응성을 향상시키며 사용자 경험, 기능 또는 콘텐츠에 대해 더 동적이고 적시에 조정할 수 있도록 합니다. 개발자들에게 앱 구성을 관리하는 더 큰 통제력과 유연성을 제공하여 더 빠른 반복 및 사용자 요구사항 또는 비즈니스 요구에 대한 더 유연한 대응이 가능합니다. 이 재정립은 핵심 메시지를 유지하면서 실시간 원격 구성의 이점을 더욱 세련되고 포괄적으로 설명합니다.