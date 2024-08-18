---
title: "안드로이드 의존성 주입 Hilt에서 Koin으로 마이그레이션하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-AndroidDependencyInjectionMigratingfromHilttoKoin_0.png"
date: 2024-07-13 00:46
ogImage:
  url: /assets/img/2024-07-13-AndroidDependencyInjectionMigratingfromHilttoKoin_0.png
tag: Tech
originalTitle: "Android Dependency Injection: Migrating from Hilt to Koin"
link: "https://medium.com/gitconnected/android-dependency-injection-migrating-from-hilt-to-koin-cd2e2a103b9c"
isUpdated: true
---

최근에 안드로이드 프로젝트에서 의존성 주입(Dependency Injection, DI) 도구를 변경하기로 결정했습니다. Dagger의 Hilt에서 더 간결한 Koin 라이브러리로 옮겨가는 것이 아이디어입니다. 이 변경을 결정한 이유는 개인 프로젝트를 KMM 애플리케이션으로 지속적으로 전환하는 작업을 더 잘 지원하는 라이브러리를 사용하고자 했기 때문입니다. Koin은 KMM을 바로 지원하므로 iOS에 대한 대체 솔루션을 찾을 필요가 없습니다.

이전에 Hilt 및 Koin 라이브러리 간의 가벼운 비교를 작성한 적이 있지만, 이번에는 어떻게 한 라이브러리에서 다른 라이브러리로 마이그레이션하는지를 보여줄 것입니다. 이 가이드는 이론적으로 대부분 반대로 마이그레이션하는 데도 적용될 것으로 예상되지만, 마지막 주장에 대해서는 보장할 수 없습니다!

시작부터 Koin 의존성을 가져오는 것으로 시작하겠습니다. 그런 다음 마이그레이션 가이드를 단계별로 진행하고, 이 글을 마무리하여 남아 있는 Hilt 코드와 해당 의존성 선언을 모두 제거할 것입니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 시작해 봅시다

어쨌든, 우리의 현재 조사를 시작해 봅시다. 첫 번째 단계는 Koin 라이브러리를 프로젝트에 포함하는 것입니다. 현대적인 안드로이드 프로젝트에서는 이를 실행하는 두 가지 주요 방법이 있습니다: build.gradle.kts 파일 내에서 간단히 의존성을 도입하는 전통적인 방법과 version catalog .toml 파일을 사용하는 현대적인 방법입니다.

## 전통적인 방법

만약 프로젝트가 version catalog을 사용하지 않는다면, build.gradle 또는 build.gradle.kts 파일을 열고 다음과 같은 라인을 프로젝트에 추가해 주세요:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```kotlin
// build.gradle.kts
dependencies {
  ...
  implementation(platform("io.insert-koin:koin-bom:$koin_version"))
  implementation("io.insert-koin:koin-android")
  // optional
  implementation("io.insert-koin:koin-test")
  implementation("io.insert-koin:koin-test-junit4")
  implementation("io.insert-koin:koin-androidx-compose")
}
```

공식 Koin 문서에서는 동일한 제공 업체로부터 다양한 종속성을 쉽게 사용할 수 있도록 BoM 의존성을 사용하는 것을 권장합니다.

## Version Catalog 방법

대신, 최근 Gradle이 도입한 버전 카탈로그 패턴을 사용하도록 프로젝트를 마이그레이션했다면, 그것을 실행하려면 더 많은 변경이 필요할 것입니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

먼저 libs.versions.toml 파일을 업데이트해야 해요. 모든 새로운 의존성을 추가해요:

```js
// 최신 버전을 잊지마세요!
koin-bom = "3.5.0"
...
koin-bom = { module = "io.insert-koin:koin-bom", version.ref = "koin-bom" }
koin-android = { module = "io.insert-koin:koin-android" }
// 옵션
koin-test = { module = "io.insert-koin:koin-test" }
koin-test-junit4 = { module = "io.insert-koin:koin-test-junit4" }
koin-android-compose = { module = "io.insert-koin:koin-androidx-compose" }
```

다음으로 build.gradle 또는 build.gradle.kts 파일에 새로운 의존성을 직접 추가할게요:

```js
// build.gradle.kts
dependencies {
    ...
  implementation(platform(libs.koin.bom))
  implementation(libs.koin.android)
  // 옵션
  implementation(libs.koin.test)
  implementation(libs.koin.test.junit)
  implementation(libs.koin.android.compose)
}
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 코인 시작하기

코인이 프로젝트에 완전히 통합되었다면, 다음 단계는 라이브러리를 시작하는 것입니다. 힐트와 비슷하게, 이 작업은 일반적으로 Application 클래스를 확장한 클래스에서 수행됩니다.

## 힐트 제거

그러나 코인을 설정하기 전에, 먼저 Application을 확장한 클래스에서 모든 힐트 호출을 제거해야 합니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

**HiltAndroidApp 어노테이션 제거**

새로운 버전의 Hilt에서는 `@HiltAndroidApp` 어노테이션을 사용하지 않아도 됩니다. 이 어노테이션을 제거하고 `App` 클래스를 수정해주세요.

```java
open class App : Application() {
  ...
}
```

**Koin 추가**

Koin을 추가하는 것은 매우 간단합니다. 다음과 같이 하나의 함수 호출로 가능합니다:

```java
open class App : Application() {
  ...
  startKoin {
    androidContext(this@App)
    modules(AppModule)
  }
}
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이번 모듈 선언에 대해 이야기해보겠습니다. 다음 섹션에서는 AppModule 구현을 살펴볼 것입니다. 그러나 다음으로 진행하기 전에, Koin이 여러분이 설정할 수 있는 몇 가지 옵션이 더 있다는 점을 알아두는 것이 좋습니다. 공식 문서에서 설명한대로 androidLogger() 및 androidFileProperties() 호출을 함께 살펴보고 현명하게 선택하세요.

# AppModule 구현

지난 섹션에서 보았듯이, startKoin 호출은 모듈 선언을 정의해야 합니다. 이 AppModule 필드의 목적은 앱이 작동해야 하는 모든 주입 가능한 모듈을 정의하는 것입니다.

공식 문서에서 설명한대로, 다음 몇 개 하위 섹션에서 살펴볼 세 가지 주요 모듈 유형이 있습니다. 독자가 숙지해야 할 몇 가지 고급 기능도 있습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 팩토리 모듈

먼저, AppModule 필드를 자체 파일에 정의하거나 App 클래스와 동일한 공간에 추가하고 단일 팩토리 모듈을 추가할 수 있습니다:

```kotlin
val AppModule = module {
  factory<CoroutineScope> {
    CoroutineScope(SupervisorJob() + Dispatchers.IO)
  }
}
```

팩토리 모듈은 정의된 클래스의 새 인스턴스가 주입마다 생성되어야 함을 나타냅니다. 이 경우에는 CoroutineScope를 예로 사용했지만, 우리의 DI 구현에 추가하고자 하는 어떤 종류의 클래스라도 사용할 수 있습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 싱글 모듈

싱글 모듈, 또는 싱글톤 모듈로도 알려진 싱글 모듈은 주어진 클래스의 단일 인스턴스가 배포될 것을 나타냅니다.

```kotlin
val AppModule = module {
  ...
  single {
    Preferences(
      context = get()
    )
  }
  single {
    Analytics(
      context = get(),
      preferences = get()
    )
  }
}
```

위 예제에서는 single 키워드를 사용하여 단일 인스턴스 종속성을 설정하는 것뿐만 아니라, get() 선언을 사용하여 다른 종속성을 주입하고 있습니다. Preferences 종속성은 get() 호출을 사용하여 앞에서 startKoin 호출 내에서 설정한 Context를 획득하기 위해 사용됩니다. 비슷하게, Analytics 종속성은 올바르게 작동하려면 Context 및 Preferences 종속성 모두를 요청합니다. 더 명시적이고 싶다면 get() 호출 내에서 클래스 선언을 직접 추가할 수 있습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```kotlin
val AppModule = module {
  ...
  single {
    Preferences(
      context = get(Context::class)
    )
  }
  single {
    Analytics(
      context = get(Context::class),
      preferences = get(Preferences::class)
    )
  }
}
```

## 뷰 모델 모듈

이 마이그레이션 가이드에서 볼 수 있는 마지막 모듈 선언은 뷰 모델 종속성 선언입니다. 이 선언은 이전 두 가지와 동일한 방식으로 작동하지만, 다음 섹션에서 살펴볼 프로바이더 구조 내부에 뷰 모델을 저장하는 추가 기능이 있습니다.

```kotlin
val AppModule = module {
  ...
  viewModel {
    MainViewModel(get())
  }
  viewModel {
    WelcomeViewModel(get(), get())
  }
  viewModel {
    MapViewModel(get(), get(), get())
  }
}
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# View Models 이동하기

View Models에 관한 주제를 따라가면, 처음으로 Koin을 통합하는 새로운 앱에 대한 해결책으로 여러분의 모듈 내에 viewModel 선언을 추가하는 것이 좋습니다. 그러나 우리의 현재 조사에 대해서는, 뒤처지지 않기 위해 이 논문을 명확하고 집중된 상태로 유지하기 위해 남아있는 Hilt 코드를 정리해야 합니다.

## Hilt 제거

Hilt는 일반적으로 뷰 모델에 포함되어야 하는 두 가지 서로 다른 코드 조각을 필요로 합니다: @HiltViewModel 주석과 클래스 서명 자체에 있는 @Inject constructor(..) 선언입니다. 이 모든 것들이 Koin에 필요하지 않기 때문에, 이를 모두 삭제하는 것으로 진행하겠습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

**Koin 추가**

한편, Koin은 이전 섹션에서 이미 추가한 코드 이상의 추가 코드 없이 구현할 수 있습니다. 따라서 View Model의 클래스 선언이 이전보다 깔끔해 보일 것입니다.

```java
// 추가할 내용 없음
class MainViewModel(
  val context: Context
) { ... }
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## Tarot Insight

지금은 전통적인 **ViewModelProvider**에 의해 View Models이 처리되지 않기 때문에 Composables를 업데이트해야 합니다. Compose를 사용 중이지 않다면 이 섹션을 건너뛰세요. 그렇지 않으면 기존의 **viewModel()** 호출에 단순한 변경을 해주면 충분히 잘 작동할 것입니다:

```kotlin
@Composable
fun MapScreen(
  viewModel: MapViewModel = koinViewModel(), // 변경 전: viewModel()
) { ... }
```

# View Holders 이동하기 (Fragment + Activity)

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

여기까지 오면 대부분의 데이터는 팩토리 및 단일 모듈의 도입, 그리고 모든 viewModels에 의해 처리되었어야 합니다. 그러나 액티비티와 프래그먼트와 같은 view holder에도 의존성을 주입해주어야 합니다. 공식 문서에서 설명한 대로, Hilt는 안드로이드 클래스에 @AndroidEntryPoint 주석을 추가하도록 요구하여 라이브러리가 의존성 주입이 필요할 수 있다는 것을 알 수 있도록 합니다. Hilt와 달리 Koin은 주입해야 할 의존성마다 inject() 호출 외에 추가 설정이 필요하지 않습니다.

더불어, 우리는 위에서 Compose로 한 것과 유사하게, 일반적인 것 대신 Koin의 View Model 제공자를 사용해야 합니다. 이 변경은 글자 변경과 다른 import 뿐이 필요합니다!

## Hilt 제거

이미 언급했듯이, 예전 Hilt 구현에서 정리해야 할 코드가 있습니다. 특히 @AndroidEntryPoint 주석입니다. 또한, 전체적으로 Hilt의 것 대신 Koin 전용 호출을 사용하도록 주입 호출을 변경해주어야 합니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

// Activity나 Fragment 중 하나를 제거하세요
@AndroidEntryPoint
class WelcomeFragment : Fragment() {
...
@Inject
lateinit var anayltics: Analytics
@Inject
lateinit var prefs: Prerferences

// 더 이상 일반 view model provider를 사용하지 않습니다
private val viewModel: WelcomeViewModel by viewModels()
}

## Koin 추가

의존성을 주입 요청하기 위해 Inject() 애노테이션을 사용하는 대신, Koin은 코드 베이스 전체에 분산된 inline inject() 확장을 사용합니다:

// Activity나 Fragment 중 하나를 추가하세요
class WelcomeFragment : Fragment() {
...
val anayltics: Analytics by inject()
val prefs: Prerferences by inject()

// Koin의 view model provider를 사용하여 주입
private val viewModel: WelcomeViewModel by viewModel()
}

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 다른 클래스들 이동하기

Hilt는 거의 모든 다른 컴포넌트들에 대해 Module 클래스를 만드는 것을 권장합니다. 이미 보았듯이 @Inject() 어노테이션을 통합하는 방법과 반복되고 테스트에 비용이 많이 드는 모든 보일러플레이트 코드에 대해 만들어보시라고 합니다.

다행히, Koin은 그런 모든 것의 절반도 필요로 하지 않으며, 대신 초기 AppModule 필드의 정의에 의존하여 필요한 모든 종속성을 주변에 주입할 수 있습니다. 따라서 이 마지막 단계에서 우리는 더는 필요하지 않은 많은 이전 힐트 코드를 삭제하고 이미 구현된 Koin 통합과의 가장 큰 차이점을 강조해보려고 합니다.

## Hilt 제거

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

먼저, Hilt 프로젝트 전반에 걸쳐 @Module 주석으로 식별된 Module 클래스를 모두 제거할 것입니다:

```kotlin
// 모두 삭제
@InstallIn(SingletonComponent::class)
@Module
object PreferencesModule {
    private lateinit var instance: Preferences
    @Singleton
    @Provides
    fun providesPreferences(
        @ApplicationContext context: Context
    ): Preferences {
        if (this::instance.isInitialized.not()) {
            instance = Preferences(context)
        }
        return instance
    }
}
```

다음으로, 기존 클래스의 서명을 업데이트하여 남은 Hilt 코드를 삭제해야 합니다:

```kotlin
// 어노테이션 제거
@Singleton
class Preferences @Inject constructor(
    private val context: Context
) { ... }
```

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 코인 추가

코인 측면에서는 위 예제의 종속성을 선언하기 위해 추가 코드가 필요하지 않습니다. 이미 설정한 것 이외에. 따라서 Preferences 클래스는 더 깔끔하고 멋져 보여야 합니다.

```js
// 추가할 것이 없음
class Preferences(
  private val context: Context
) { ... }
```

## 리본 매듭 짓기

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

크고 중요한 부분을 처리했으니, 이제 남은 주요 작업은 프로젝트에서 모든 Hilt 종속성을 제거하는 것입니다. 버전 카탈로그를 사용하든 아니든 각각의 Gradle 파일로 이동하여 Hilt 라이브러리와 관련된 종속성 선언을 모두 제거하세요. 참고: 플러그인도 제거하는 것을 잊지 마세요!

마지막으로, 'hilt'라는 단어를 Global Find(Shift + Command + F)로 찾는 것을 추천합니다. 이렇게 하면 아무 것도 놓치지 않도록 할 수 있습니다. 그러나 종속성이 제대로 제거되었다면 어떤 잔해가 남아 있더라도 빌드가 실패해야 합니다!

## 사후 판단

이 글에서는 의존성 주입 라이브러리를 Hilt에서 Koin으로 마이그레이션하는 완전히 단순화된 프로세스를 살펴보았습니다. 이를 하는 이유는 빠르게 KMM을 도입하기 위해 프로젝트를 준비하기 위해서였지만, Hilt에서 이동하는 데에는 다른 이유들도 많습니다. Google이 강력히 권장하지만, Kotlin-first 라이브러리를 사용하면 Dagger의 납작한 변종을 사용하는 것보다 자체적으로 많은 이점을 가져올 수 있습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Koin은 Hilt보다 훨씬 가벼운 편이에요. PR 차이에서도 확인할 수 있죠:

![이미지](/assets/img/2024-07-13-AndroidDependencyInjectionMigratingfromHilttoKoin_1.png)

읽는 분들이 유용하게 사용하셨으면 좋겠네요. 다른 멋진 Android 팁을 찾고 계시다면 제 다른 작품도 확인해보세요! 링크는 아래에 있어요 👇

👾 제 프로필 👾 | 🔗 모든 다른 링크 🔗

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![Image](/assets/img/2024-07-13-AndroidDependencyInjectionMigratingfromHilttoKoin_2.png)

# Resources

- [Dagger Hilt](https://dagger.dev/hilt/)
- [Koin](https://insert-koin.io/)
- [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html)
- [Kotlin Dependency Injection: Koin vs Hilt](https://blog.logrocket.com/kotlin-dependency-injection-koin-vs-hilt/)
