---
title: "2024년 안드로이드 개발자 로드맵"
description: ""
coverImage: "/assets/img/2024-08-17-AndroidDeveloperRoadmap2024_0.png"
date: 2024-08-17 00:28
ogImage: 
  url: /assets/img/2024-08-17-AndroidDeveloperRoadmap2024_0.png
tag: Tech
originalTitle: "Android Developer Roadmap 2024"
link: "https://medium.com/@daniel.atitienei/android-developer-roadmap-2024-c0f88b8d33cd"
isUpdated: false
---


커피 한 잔 마시고 ☕, 안드로이드 개발자가 되기 위해 배워야 하는 것을 살펴봐요.

나는 3년 이상의 안드로이드 애플리케이션 개발 경험이 있으니, 지금의 위치에 있기 위해 거쳐야 했던 것들이 이 안내서에 포함되어 있어.

![](/assets/img/2024-08-17-AndroidDeveloperRoadmap2024_0.png)

# 프로그래밍 언어

<div class="content-ad"></div>

내 의견으로는 Kotlin이 이미 잘 알려지고 대부분의 애플리케이션에서 사용되기 때문에 Java를 배우지 말아야 한다고 생각해요. 미래에는 Java가 선택 사항이 되지 않을 것이라고 생각해요. 여기서 두 언어를 비교한 내용을 읽을 수 있어요.

# Jetpack Compose vs XML

Jetpack Compose은 Google이 만든 선언적 프레임워크로 앱을 만드는 과정을 간단하게 만들기 위해 고안되었어요. 이해하기 쉽고 직관적인 학습 방식을 가지고 있어요. Compose의 큰 장점 중 하나는 XML과의 상호 운용성이에요. 이는 Compose 코드를 XML 앱에 추가하거나 코드를 쉽게 적응시킬 수 있다는 것을 의미해요.

XML은 앱을 구축하는 명령적인 방식이에요. XML 파일을 사용하여 UI를 구축한 다음 Fragment/Activity에 연결하는 방식으로 작동해요.

<div class="content-ad"></div>

XML에서 스크롤 가능한 목록을 만드는 방법의 예제입니다. 제공된 코드는 문서에서 가져온 것이며 여기에서 찾을 수 있습니다.

```js
class CustomAdapter(private val dataSet: Array<String>) :
        RecyclerView.Adapter<CustomAdapter.ViewHolder>() {

    /**
     * 사용 중인 뷰 유형에 대한 참조를 제공합니다
     * (사용자 지정 ViewHolder)
     */
    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val textView: TextView

        init {
            // ViewHolder의 View에 대한 클릭 리스너 정의
            textView = view.findViewById(R.id.textView)
        }
    }

    // 새 뷰 생성 (레이아웃 관리자에 의해 호출됨)
    override fun onCreateViewHolder(viewGroup: ViewGroup, viewType: Int): ViewHolder {
        // 목록 항목의 UI를 정의하는 새 뷰 생성
        val view = LayoutInflater.from(viewGroup.context)
                .inflate(R.layout.text_row_item, viewGroup, false)

        return ViewHolder(view)
    }

    // 뷰의 콘텐츠 교체 (레이아웃 관리자에 의해 호출됨)
    override fun onBindViewHolder(viewHolder: ViewHolder, position: Int) {

        // 데이터셋에서이 위치의 요소를 가져 와서
        // 뷰의 내용을 해당 요소로 교체합니다
        viewHolder.textView.text = dataSet[position]
    }

    // 데이터셋의 크기 반환 (레이아웃 관리자에 의해 호출됨)
    override fun getItemCount() = dataSet.size

}
```

이것은 Jetpack Compose를 사용하여 스크롤 가능한 목록을 생성하는 방법에 대한 코드 조각입니다.

```js
@Composable
fun MyList() {
    LazyColumn {
        items(10) {
            Text(text = "Hello World!")
        }
    }
}
```

<div class="content-ad"></div>

## 그래 그럼 어느 것이죠?

일들이 조금 복잡해 보이네요. Jetpack Compose 코드가 훨씬 깨끗한 것 같아도 몇몇 회사들은 아직 그것에 맞게 코드를 변경하지 않았습니다.

내 조언은 Jetpack Compose를 배우고 XML도 조금 배워서 그 작동 방식을 이해하는 것입니다. 이것은 좋은 방법이죠. 왜냐하면 회사를 가입하게 되면 그들이 코드베이스를 적응시키고자 할 때 이미 어떻게 하는지 알고 있기 때문입니다.

# 애플리케이션 수명주기

<div class="content-ad"></div>

## onCreate()

앱이 처음 생성될 때 호출됩니다. 여기서는 예를 들어 뷰 바인딩과 같은 모든 정적 작업을 수행합니다.

## onStart()

사용자에게 앱이 표시될 때 트리거됩니다. 예를 들어 앱을 다시 시작하면 onStart()가 트리거되고 onResume()으로 전달됩니다.

<div class="content-ad"></div>

## onResume()

이 이벤트는 앱이 사용자와 상호 작용을 시작할 때 트리거됩니다.

## onPause()

이 이벤트는 앱이 사용자에게 보이지 않을 때 트리거됩니다. 예를 들어, 앱을 백그라운드로 넣을 때 이벤트가 트리거되며 onStop()으로 전달됩니다.

<div class="content-ad"></div>

## onStop()

사용자에게 더 이상 보이지 않을 때 호출됩니다.

## onRestart()

앱이 중지되었고 시작할 준비가 된 경우 호출됩니다. 예를 들어, 백그라운드에 있던 앱으로 다시 들어갈 때 이 이벤트가 발생하며 onStart()에 의해 전달됩니다.

<div class="content-ad"></div>

## onDestroy()

앱이 종료되기 전에 트리거됩니다. 사용자가 최근 앱에서 앱을 제거할 때

# 코루틴

코루틴은 스레드와 비슷하지만 훨씬 가벼우며 일반적으로 더 효율적이고 사용하기 쉽습니다. Kotlin 코루틴 가이드가 여기 있습니다.

<div class="content-ad"></div>

# 인터넷에서 데이터 가져오기

대부분의 앱은 데이터를 가져 오기 위해 데이터베이스와 통신하기 때문에 이를하는 방법을 알아야합니다.

이를 위해 Retrofit 또는 Ktor을 강력히 추천합니다. 둘 다 네트워크 요청에 매우 유용합니다.

# 로컬 데이터베이스

<div class="content-ad"></div>

앱이 가지고 있는 또 다른 중요한 기능으로는 로컬 데이터베이스를 사용할 수 있다. 이를 위해 Room, Realm 또는 SQLite 데이터베이스를 사용하는 방법을 배울 수 있습니다. 이들은 데이터를 저장하고 네트워크가 사용 불가능할 때 앱에서 데이터를 불러오는 데 사용될 수 있습니다.

## SharedPreferences

저장할 키-값의 상대적으로 작은 컬렉션이 있는 경우 SharedPreferences를 사용할 수 있습니다. 사용자의 최고 점수를 여기에 저장하는 예시가 있습니다.

## 아키텍처

<div class="content-ad"></div>

안녕하세요! 안드로이드 개발 세계에서 매우 많이 사용되는 아키텍처 패턴이 있습니다. 여기에는 일반적으로 사용되는 몇 가지 패턴이 있습니다:

- MVVM (모델-뷰-뷰모델)
- MVI (모델-뷰-인터프리터)
- MVP (모델-뷰-프리젠터)
- MVC (모델-뷰-컨트롤러)

# 이미지 로딩

네트워크에서 이미지를 로드하는 것은 Coil, Glide 또는 Picasso와 같은 다른 종속성을 사용하여 수행할 수 있습니다.

<div class="content-ad"></div>

# 코드 분석

이를 위해서는 린터를 사용해야 합니다. 린터는 코드를 분석하여 잠재적인 오류, 보안 취약점 및 스타일 문제를 찾는 도구입니다.

다음은 몇 가지 린터들입니다: Ktlint, Detekt 그리고 Android Lint.

# 디버깅

<div class="content-ad"></div>

여기 앱에서 오류를 쉽게 감지할 수 있도록 도와주는 몇 가지 디버깅 의존성이 있습니다.

- Timber: 안드로이드 로깅을 간소화합니다.
- LeakCanary: 안드로이드 앱에서 메모리 누수를 감지합니다.
- Stetho: 안드로이드 앱을 위한 디버거로 네트워크 트래픽, 데이터베이스 등을 검사합니다.
- Chuck: 안드로이드를 위한 네트워크 인터셉터로 HTTP/HTTPS 요청과 응답을 검사합니다.

# 테스트

테스트는 앱이 다양한 시나리오에서 올바르게 작동하는지 확인할 수 있는 중요한 작업입니다. 유닛 테스트(Unit Testing)와 인스트루먼테이션 테스팅(Instrumented Testing) 두 가지 유형의 테스트가 있습니다.

<div class="content-ad"></div>

## 단위 테스트

이 유형의 테스트는 코드 조각이 제대로 작동하는지 확인하기 위해 에뮬레이터를 사용하지 않고 수행됩니다. 단위 테스트에 사용되는 일부 종속성은 다음과 같습니다:

- JUnit: 테스트를 작성하고 실행하기 위한 테스트 프레임워크입니다.
- Kluent: 가독성이 좋고 표현력 있는 테스트를 작성하기 위한 코틀린 전용 어서션 라이브러리입니다.
- MockK: 코틀린용 목 객체 프레임워크로, 목 클래스를 만드는 데 사용됩니다.

## Instrumented Testing

<div class="content-ad"></div>

이 유형의 테스트는 에뮬레이터에서 앱을 실행하여 수행됩니다. 앱의 특정 화면을 테스트하고 상호 작용하거나 전체 앱 플로우를 테스트하는 E2E(End-To-End)를 작성할 수 있습니다. 이곳에는 몇 가지 인스트루먼테이션 테스트 종속성이 있습니다.

- Robolectric: 에뮬레이터나 기기가 필요하지 않고 JVM에서 안드로이드 테스트를 실행합니다.
- Espresso: 안드로이드를 위한 UI 테스트 작성을 위한 프레임워크로 UI 요소와 상호 작용합니다.
- Kaspresso: Espresso를 기반으로 구축된 고수준 UI 테스트 프레임워크로 테스트 작성을 단순화하고 신뢰성을 향상시키기 위해 만들어졌습니다.

# CI/CD

이것은 지속적 통합(Continuous Integration) 및 지속적 전달/배포(Continuous Delivery/Deployment)를 의미합니다. CI는 코드 변경을 공유 소스 코드 저장소에 자동으로 통합하는 프로세스를 자동화합니다. CD는 코드 변경의 통합, 테스트 및 전달을 의미하는 프로세스입니다.

<div class="content-ad"></div>

## CI/CD 도구

- **Bitrise**: 모바일 앱 개발에 주로 초점을 맞춘 클라우드 기반 플랫폼으로, iOS 및 Android 앱을 빌드, 테스트 및 배포할 수 있습니다.
- **Jenkins**: 안드로이드 및 iOS를 포함한 다양한 프로젝트에 사용할 수 있는 범용 오픈소스 CI/CD 도구로, 보다 많은 설정 및 유지관리가 필요합니다.
- **CircleCI**: 모바일 및 웹 애플리케이션에 적합한 유연성과 사용 편의성을 제공하는 클라우드 기반 CI/CD 플랫폼입니다.