---
title: "2024년 최신 안드로이드 개발 트렌드"
description: ""
coverImage: "/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_0.png"
date: 2024-07-07 23:24
ogImage:
  url: /assets/img/2024-07-07-ModernAndroidDevelopmentin2024_0.png
tag: Tech
originalTitle: "Modern Android Development in 2024"
link: "https://medium.com/@devjorgecastro/modern-android-development-in-2024-b70f194938bd"
isUpdated: true
---

**![Modern Android Development in 2024](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_0.png)**

안녕하세요 여러분 👋🏻, 새해를 맞아 새로운 시작이라는 소중한 기회를 놓치고 싶지 않아요. 지금은 2024년 버전의 Modern Android Development라는 제목의 글을 나누고 싶어해요. 🚀

이전 글을 놓친 경우, Modern Android Development in 2023을 살펴보시고 올해의 변화를 확인해보세요.

## **면책 조항**

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

🔮 이 글은 내 개인적인 의견과 전문적인 통찰을 반영한 것이며, 안드로이드 개발자 커뮤니티 내 다양한 의견을 고려하였습니다. 또한, 구글이 안드로이드를 위한 제시한 지침을 정기적으로 검토하고 있습니다.

⚠️ 중요한 점은 나는 특정 강력한 도구, 패턴 및 아키텍처를 명시적으로 언급하지 않을지라도, 이것이 안드로이드 애플리케이션 개발을 위한 가치 있는 대안으로서의 잠재력을 무효화하지는 않는다는 것을 강조해야 합니다.

# 코틀린을 어디서나 사용하기 ❤️

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_1.png)

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

Kotlin은 JetBrains에서 개발한 프로그래밍 언어입니다. Google에서 추천하며 2017년 5월 공식 발표했어요. Java와 호환되며 JVM에서 실행될 수 있는 현대적인 프로그래밍 언어로, 안드로이드 애플리케이션 개발에 빠르게 채택되고 있어요.

안드로이드에 익숙한지 여부와 상관없이 Kotlin을 처음 선택으로 고려해보세요. 흐름에 맞추는 게 좋아요 🏊🏻 😎, 구글은 2019년 Google I/O에서 이 접근 방식을 발표했죠. Kotlin을 사용하면 코루틴의 강력함과 안드로이드 생태계용으로 개발된 현대적인 라이브러리 활용 등 모든 현대 언어의 기능을 사용할 수 있어요.

공식 Kotlin 문서는 여기에 있습니다

Kotlin은 안드로이드 애플리케이션 개발뿐만 아니라 여러 용도로 사용할 수 있는 다목적 언어에요. 여러분이 직접 이 그래프에서 확인할 수 있듯이, 안드로이드 애플리케이션 개발과 관련해 많은 인기를 끌었죠.

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

## Kotlin 2.0이 도착했어요

다른 중요한 사항 중 하나는 곧 출시될 Kotlin 2.0입니다. 이 기사 작성 시점에서의 버전은 2.0.0-beta3입니다.

![Kotlin 2.0 Image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_2.png)

Kotlin 2.0에서의 새로운 K2 컴파일러도 매우 중요한 추가 기능 중 하나인데요. 이는 중요한 성능 향상을 가져오며, 새 언어 기능 개발을 가속화하고, Kotlin이 지원하는 모든 플랫폼을 통합하며, 멀티플랫폼 프로젝트에 더 나은 아키텍처를 제공할 것입니다.

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

**코틀린 컨프 23의 리뷰를 확인해보세요!**

# 코틀린 컨프 '23 요약 🚀

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_3.png)

젯팩 콤포즈는 안드로이드 젯팩 라이브러리의 일부로, Kotlin 프로그래밍 언어를 사용하여 네이티브 사용자 인터페이스를 쉽게 만들 수 있습니다. 또한 LiveData 및 ViewModel과 같은 다른 안드로이드 젯팩 라이브러리와 통합되어, 리액티브하고 유지보수 가능한 안드로이드 애플리케이션을 더 쉽게 구축할 수 있습니다.

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

Some key features of **Jetpack Compose** that you should know:

- **Declarative UI**: Design your user interface with ease.
- **Customizable widgets**: Tailor widgets to suit your style and needs.
- **Easy integration with existing code**: Seamlessly integrate with your old view system.
- **Live preview**: See your changes in real-time.
- **Improved performance**: Enjoy faster and smoother interactions.

Here are some useful resources to delve deeper into Jetpack Compose:

- [Jetpack Compose documentation](link to documentation)
- [Compose to Kotlin Compatibility Map](link to compatibility map)
- [Jetpack Compose Roadmap](link to roadmap)
- [Course](link to course)
- API Guidelines for @Composable components in Jetpack Compose

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

# 안드로이드 젯팩 ⚙️

![Android Jetpack](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_4.png)

일반적인 도구 중 일부는 다음과 같습니다:

- ViewModel
- Room
- DataStore
- WorkManager
- Navigation
- CameraX
- Compose
- Media3
- Glance

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

# 마테리얼 유 / 마테리얼 디자인 🥰

안드로이드 12에 소개된 새로운 맞춤화 기능인 마테리얼 유(Material You)는 사용자들이 자신의 개인적 취향과 일치하도록 운영 체제의 시각적 외관을 맞춤 설정할 수 있게 해줍니다. 이 혁신적인 추가 기능은 사용자 인터페이스 디자인의 최고 기준을 유지하기 위해 만들어진 가이드라인, 구성 요소 및 도구로 이루어진 적응형 시스템인 마테리얼 디자인을 보완합니다. 오픈 소스 코드로 지원되는 마테리얼 디자인은 디자이너와 개발자 간에 원활한 협업을 촉진하여 효율적으로 멋진 제품을 만들 수 있도록 지원합니다.

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_5.png)

현재 마테리얼 디자인의 마지막 버전은 3이며, 더 많은 정보를 보려면 [여기](링크)를 확인하세요. 또한, 애플리케이션의 테마를 정의하는 데 도움이 되는 마테리얼 테마 빌더(Material Theme Builder)를 활용할 수 있습니다.

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

## 코딜랩

매터리얼 3으로 코틀린 코루틴 사용하기

# 스플래시 화면 API

![Image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_6.png)

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

안녕하세요!
안드로이드의 SplashScreen API는 안드로이드 12 이후 버전에서 앱이 올바르게 표시되도록 하는 데 필수적입니다. 업데이트를 하지 않으면 애플리케이션 실행 경험에 영향을 줄 수 있습니다. 최신 버전의 운영 체제에서 일관된 사용자 경험을 위해 빠르게 이 API를 채택하는 것이 중요합니다.

# Clean Architecture

![Clean Architecture](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_7.png)

로버트 C. 마틴에 의해 소개된 Clean Architecture 개념은 소프트웨어를 계층으로 분리함으로써 책임을 분리하는 데 기반을 두고 있습니다.

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

특징

- 프레임워크와 독립적입니다.
- 테스트할 수 있습니다.
- UI와 독립적입니다.
- 데이터베이스와 독립적입니다.
- 외부 기관에 독립적입니다.

의존성 규칙

의존성 규칙은 작성자가 "The Clean Code Blog"에서 잘 설명하고 있습니다.

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

안녕하세요!

클린 아키텍처는 안드로이드 어플리케이션에서 중요한 역할을 합니다. Presentation 계층에는 Activities, Composables, Fragments, View Models 등 다양한 뷰 컴포넌트들이 포함됩니다. Domain 계층에는 Use Cases, Entities, Repositories와 같은 도메인 컴포넌트들이 있고, Data 계층에는 Repository 구현체, Mappers, DTO 등이 포함됩니다.

# Presentation Layer를 위한 아키텍처 패턴들

![Modern Android Development](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_8.png)

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

건축 패턴은 소프트웨어 아키텍처를 디자인하는 데 도움을 주는 고수준 전략이며, 공통 아키텍처 문제에 대한 재사용 가능한 프레임워크 내의 해결책으로 특징 지어집니다. 건축 패턴은 디자인 패턴과 유사하지만 규모가 크며 시스템의 전체 구조, 구성 요소 간의 관계, 데이터 관리 방식과 같은 더 전역적인 문제를 다룹니다.

프레젠테이션 레이어 내에는 MVVM과 MVI와 같은 몇 가지 건축 패턴이 있습니다. 각각을 설명하기보다는 이에 대해 너무 많은 정보를 인터넷에서 찾을 수 있다는 점을 강조하고 싶습니다. 😅

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

또한, 앱 아키텍처에 대한 가이드도 참고하실 수 있어요!

![Modern Android Development in 2024_9](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_9.png)

# 의존성 주입

![Modern Android Development in 2024_10](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_10.png)

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

의존성 주입은 클라이언트가 스스로 생성하는 대신 외부 소스에서 의존성을 가져오는 소프트웨어 디자인 패턴입니다. 객체와 그 의존성 간의 제어 역전(IoC)을 달성하는 기술입니다.

- 힐트 ❤️
- 다거
- 코인

# 모듈화

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_11.png)

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

모듈화는 소프트웨어 디자인 기술로, 응용 프로그램을 독립적인 모듈로 나누어 각각이 자체 기능과 책임을 갖도록 하는 것을 말해요.

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_12.png)

# 모듈화의 장점

재사용성: 독립적인 모듈을 가지고 있기 때문에, 이를 응용 프로그램의 다른 부분이나 다른 응용 프로그램에서 재사용할 수 있어요.

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

**엄격한 가시성 제어:** 모듈을 이용하면 코드베이스의 다른 부분에 노출시킬 내용을 쉽게 제어할 수 있습니다.

**사용자 정의 가능한 전달:** Play Feature Delivery는 앱 번들의 고급 기능을 활용하여, 앱의 특정 기능을 조건부로 또는 필요 시 전달할 수 있게 합니다.

**확장성:** 독립적인 모듈을 가지고 있어 기능을 추가하거나 제거해도 응용 프로그램의 다른 부분에 영향을 미치지 않습니다.

**유지보수의 용이성:** 응용 프로그램을 독립적인 모듈로 분할하여 각각의 기능과 책임을 갖게 함으로써, 코드를 이해하고 유지하는 것이 더 쉬워집니다.

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

#### 테스트 용이성:

독립 모듈을 가지고 있어 각각을 분리하여 테스트할 수 있어 오류를 쉽게 발견하고 수정할 수 있습니다.

#### 아키텍처 개선:

모듈화를 통해 애플리케이션 아키텍처를 개선하여 코드의 조직화와 구조화를 더 잘할 수 있습니다.

#### 협업 향상:

독립적인 모듈을 가지고 있어 개발자들이 동시에 서로 간섭받지 않고 애플리케이션의 다양한 부분에 작업할 수 있습니다.

#### 빌드 시간:

일부 Gradle 기능들인 증분 빌드(incremental build), 빌드 캐시(build cache) 또는 병렬 빌드(parallel build)는 모듈성을 활용하여 빌드 성능을 향상시킬 수 있습니다.

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

공식 문서에서 더 많은 정보를 확인해보세요.

## 네트워크

![Network Image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_13.png)

- OkHttp
- Retrofit
- Ktor

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

# 일련화

![image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_14.png)

이 섹션에서는 제 의견으로 중요한 두 가지 도구를 언급하고 싶습니다: Retrofit과 널리 사용되는 Moshi 그리고 JetBrains의 Kotlin 팀의 선택, Kotlin Serialization입니다.

- Moshi
- Kotlin Serialization

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

🌟 **Moshi와 Kotlin Serialization 소개**

Moshi와 Kotlin Serialization은 Kotlin과 Java를 위한 직렬화/역직렬화 라이브러리로, 객체를 JSON 또는 다른 직렬화 형식으로 변환하고 그 반대로 변환할 수 있습니다. 모바일 및 데스크톱 애플리케이션에서 최적화된 사용자 친화적 인터페이스를 제공합니다. Moshi는 주로 JSON 직렬화에 중점을 두고 있으며, Kotlin Serialization은 JSON을 포함한 다양한 직렬화 형식을 지원합니다.

## 이미지 로딩

![Modern Android Development in 2024](https://www.example.com/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_15.png)

- Coil
- Glide

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

# 반응성 / 스레드 관리

리액티브 프로그래밍과 비동기 프로세스에 관련해 말하자면, Kotlin 코루틴은 서스펜션 함수와 Flow로 높은 성능을 보여줍니다. 하지만 안드로이드 애플리케이션 개발에서 RxJava의 가치를 인정하는 것이 중요합니다. 코루틴과 Flow의 채택이 증가하고 있지만, 여러 프로젝트에서 RxJava는 여전히 강력하고 인기 있는 선택지입니다.

새로운 프로젝트에는 언제나 Kotlin 코루틴을 선택하세요 ❤️. 몇 가지 Kotlin 코루틴 개념을 탐험해보세요.

# 로컬 저장소

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

모바일 애플리케이션을 개발할 때 중요한 점은 세션 데이터나 캐시 데이터와 같이 로컬로 데이터를 유지하는 능력을 갖추는 것입니다. 애플리케이션의 요구 사항에 따라 적절한 저장 옵션을 선택하는 것이 중요합니다. 키-값과 같은 비구조화된 데이터 또는 데이터베이스와 같은 구조화된 데이터를 저장할 수 있습니다. 이 글에서 언급한 것은 사용 가능한 로컬 저장 타입의 전부가 아니며(파일 저장소와 같은 다른 유형도 있음), 단지 데이터를 저장할 수 있는 도구만 언급한 것입니다.

![image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_16.png)

제안 사항:

- ~~SharedPreferences~~
- DataStore
- EncryptedSharedPreferences

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

# Testing 🕵🏼

![Image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_17.png)

When it comes to software development, testing is paramount to guarantee a high-quality product. It not only identifies errors but also validates requirements and ensures customer satisfaction. Here are some widely-used tools for this purpose:

- JUnit 5
- Mockk
- Kotest
- Kluent
- Turbine
- Espresso
- Robolectric
- Maestro

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

툴 문서의 테스트 섹션입니다

## 스크린샷 테스트 📸

![Screenshot](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_18.png)

안드로이드에서의 스크린샷 테스트는 애플리케이션의 다양한 UI 요소의 스크린샷을 자동으로 캡처하고 이를 베이스라인 이미지와 비교하여 의도하지 않은 시각적 변경을 감지하는 것을 포함합니다. 이는 앱의 버전 및 구성의 일관된 UI 외관을 보장하고 개발 프로세스 초기에 시각적 리그레션을 미리 감지하는 데 도움이 됩니다.

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

- Paparazzi,
- Roborazzi

# R8 최적화

R8는 프로젝트의 Java 바이트 코드를 Android 플랫폼에서 실행되는 DEX 형식으로 변환하는 기본 컴파일러입니다. 이 도구는 우리 애플리케이션의 코드를 난독화하고 축소시켜주며 클래스와 프로퍼티의 이름을 줄여줌으로써 프로젝트 내의 사용되지 않는 코드와 리소스를 제거합니다. 더 자세한 내용을 보려면 Android 문서에서 앱을 축소화하고 난독화하며 최적화하는 방법을 확인하세요. 또한 ProGuard 규칙 파일을 통해 R8의 동작을 비활성화하거나 사용자 정의할 수도 있습니다.

![image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_19.png)

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

- 코드 축소
- 리소스 축소
- 난독화
- 최적화

### 제3자 도구

- DexGuard

## Play 기능 제공

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

# 적응형 레이아웃

![ModernAndroidDevelopmentin2024_20](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_20.png)

모바일 기기의 다양한 디자인 요소를 갖는 사용량이 증가함에 따라, 안드로이드 애플리케이션을 다양한 화면에 맞추어 작업할 수 있는 도구가 필요합니다. 이것이 바로 안드로이드에서 제공하는 창 크기 클래스입니다. 간단히 말하면, 세 가지 대형 화면 형식 그룹이며, 디자인을 개발하는 데 중요한 지점을 나타냅니다. 많은 화면 디자인을 생각할 필요 없이, 우리의 가능성을 3개 그룹인 Compat, Medium 및 Expanded로 줄여줍니다.

## 창 크기 클래스

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

![이미지1](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_21.png)

![이미지2](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_22.png)

다양한 화면 크기 지원

우리가 가지고 있는 또 다른 중요한 자원은 Canonical Layouts입니다. 이는 대부분의 Android 애플리케이션 시나리오에 사용할 수 있는 미리 정의된 화면 디자인으로, 대형 화면에 적응하는 방법을 안내해줍니다.

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

![Form Factor](https://miro.medium.com/v2/resize:fit:1200/0*PTOp6eefW7HhjhtR.gif)

## 기타 관련 자료

- Google I/O 2022에서 알아야 할 Form Factors 3가지
- 재생 목록: Google I/O 2022의 Form Factors

Form Factor 트레이닝

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

# Localization 🌎

![Localization](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_23.png)

Localization involves adapting a product to meet the needs of diverse audiences in different regions. This includes translating text, adjusting formats, and considering cultural aspects. Its advantages include access to global markets, enhanced user experience, increased customer satisfaction, competitiveness in the global market, and compliance with local regulations.

Note: BCP 47 is a standard used by Android for internationalization

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

## 참고 자료

- 다양한 언어와 문화 지원
- 앱 지역화
- 앱별 언어 설정
- 앱별 언어 설정 — 파트 1
- 앱별 언어 설정 — 파트 2

# 성능 🔋⚙️

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_24.png)

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

안녕하세요, 안드로이드 애플리케이션을 개발하는 동안, 앱이 실행되는 시작부터 끝까지 사용자 경험이 개선되어야 합니다. 이를 위해 앱 성능에 영향을 미칠 수 있는 사항을 예방적으로 분석하고 지속적으로 모니터링할 수 있는 도구들이 필요합니다. 그래서 여기에는 이 목적을 위해 도움이 될 수 있는 도구들의 목록을 준비했습니다:

- Benchmark
- Baseline Profiles
- App Startup
- Firebase Performance Monitoring
- JankStats library

# 앱 내 업데이트

![](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_25.png)

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

# 앱 리뷰

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_26.png)

# 가시성 👀

![이미지](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_27.png)

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

안녕하세요!

앱 시장이 더욱 경쟁적으로 변화함에 따라 좋은 사용자 경험을 구현하는 것은 버그 없는 앱을 보장하는 것에서 시작됩니다. 앱이 버그 없는 것을 확인하는 가장 좋은 방법 중 하나는 문제가 발생할 때 즉시 감지하고 어떻게 대처할지 알아내는 것입니다. Android Vitals를 활용하여 앱 내에서 가장 많은 충돌과 반응성 문제가 발생하는 영역을 식별할 수 있습니다. 그런 다음 Firebase Crashlytics의 사용자 정의 충돌 보고서를 활용하여 문제의 근본 원인에 대한 더 많은 세부 정보를 얻어 효과적으로 문제를 해결할 수 있습니다.

## 도구

- Google Analytics
- Android Vitals
- Firebase Crashlytics
- Firebase Performance Monitoring

# 접근성

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

![Image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_28.png)

접근성은 소프트웨어의 설계 및 구축에서 중요한 기능으로, 접근성 요구사항을 갖춘 사람들이 응용프로그램을 사용할 수 있도록 제공함으로써 사용자 경험을 개선합니다. 이 개념이 향상시키려는 장애에 대한 일부 예는 시각 문제, 색맹, 청각 문제, 손가락 움직임 문제 및 인지 장애가 있습니다.

고려할 사항:

- 텍스트 가시성 향상 (색대비, 크기 조정 가능한 텍스트)
- 큰, 간단한 컨트롤 사용
- 각 UI 요소 설명하기

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

안녕하세요! 타로 전문가님! 😊

오늘은 안드로이드 문서에서 “보안”에 대해 이야기해보려고 해요.

**보안 🔐**

![ModernAndroidDevelopmentin2024_29](https://yourwebsite.com/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_29.png)

앱을 개발할 때 우리가 고려해야 할 가장 중요한 측면 중 하나가 보안입니다. 기기의 무결성, 데이터의 안전 및 사용자의 신뢰를 보호하는 애플리케이션을 개발할 때는 보안이 매우 중요하답니다. 이 목적을 달성하기 위해 아래에 제시된 팁들이 도움이 될 거예요. 🌟

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

- 사용자 자격 증명을 자격 증명 관리자로 저장하세요: 자격 증명 관리자는 여러 가지 로그인 방법을 지원하는 Jetpack API입니다. 사용자 이름 및 비밀번호, 패스키, Google 로그인과 같은 연합 로그인 솔루션을 한 곳에서 제공하여 개발자들에게 통합을 단순화합니다.
- 민감한 데이터와 파일을 암호화하세요: EncryptedSharedPreferences와 EncryptedFile을 사용하세요.
- 서명 기반 권한 적용: 제어할 수 있는 앱 간 데이터 공유 시 서명 기반 권한을 사용하세요.

```js
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <permission android:name="my_custom_permission_name"
                android:protectionLevel="signature" />
```

- 앱 구성에 필요한 키, 토큰 또는 민감한 데이터를 프로젝트 저장소에 있는 파일이나 클래스에 직접 넣지 마세요. 대신 local.properties를 사용하세요.
- SSL Pinning 구현: SSL Pinning을 사용하여 애플리케이션과 원격 서버 간 통신을 더욱 안전하게 만드세요. 이는 중간자 공격을 방지하고 특정 SSL 인증서를 갖춘 신뢰할 수 있는 서버와의 통신만이 발생하도록 합니다.

res/xml/network_security_config.xml

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

- 런타임 애플리케이션 자가 보호 (RASP)를 구현하세요: 이것은 애플리케이션을 공격과 취약점으로부터 런타임에서 보호하는 보안 기술입니다. RASP는 애플리케이션의 동작을 모니터링하고 공격을 나타낼 수 있는 의심스러운 활동을 감지함으로써 작동합니다. RASP가 제공하는 몇 가지 이점은 다음과 같습니다:

- 코드 난독화.
- 루팅 탐지.
- 변조/앱 후크 탐지.
- 역 공학 공격 방지.
- 안티 디버깅 기술.
- 가상 환경 탐지.
- 앱 동작의 런타임 분석.

더 많은 정보를 원하신다면 이 기사를 참조하세요: 안드로이드 앱에서의 런타임 애플리케이션 자가 보호 기술(RASP). 또한 안드로이드의 일부 보안 지침

# 버전 카탈로그

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

![2024-07-07-ModernAndroidDevelopmentin2024_30.png](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_30.png)

안녕하세요! 오늘은 Gradle의 버전 카탈로그에 대해 이야기해 보려고 해요.

Gradle은 프로젝트 의존성을 중앙에서 효율적으로 관리할 수 있는 표준 방법을 제공합니다. 버전 카탈로그는 7.0 버전에서 실험적으로 도입되었고 7.4 버전에서 공식적으로 출시되었습니다.

이점은 다음과 같아요:

- 각 카탈로그에 대해 Gradle은 타입 안전한 액세서를 생성하여 IDE에서 자동 완성을 통해 쉽게 의존성을 추가할 수 있어요.
- 모든 프로젝트에 대해 카탈로그가 공유되어, 의존성의 버전을 선언하고 그 버전 변경이 모든 하위 프로젝트에 적용되도록 할 수 있어요.
- 카탈로그는 함께 자주 사용되는 "의존성 그룹”인 의존성 번들을 선언할 수 있어요.
- 카탈로그는 의존성의 그룹과 이름을 해당 실제 버전에서 분리하고, 여러 의존성 사이에 버전 선언을 공유할 수 있는 버전 참조를 사용할 수 있어요.

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

안녕하세요! 타로 전문가 여러분!

보다 자세한 내용을 확인하세요.

# Secrets Gradle Plugin

![Secrets Gradle Plugin](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_31.png)

Google은 API 키를 버전 관리 시스템에 체크인하지 말 것을 강력하게 권장합니다. 대신 프로젝트의 루트 디렉토리에 위치한 버전 관리에서 제외된 로컬 secrets.properties 파일에 저장한 후 안드로이드용 Secrets Gradle Plugin을 사용하여 API 키를 읽도록 권장합니다.

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

# 로거

![Logger](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_32.png)

로거는 프로그램 실행에 관한 정보를 기록하는 소프트웨어 도구입니다. 중요 이벤트, 오류 디버그 메시지 및 프로그램 작동 방식을 진단하거나 이해하는 데 유용한 다른 정보가 포함됩니다. 로거는 로그 파일, 콘솔, 데이터베이스 또는 로깅 서버로 메시지를 보내는 것과 같은 다양한 위치에 메시지를 작성하도록 구성할 수 있습니다.

- Klogging
- Timber

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

# 린터 / 정적 코드 분석기

![ModernAndroidDevelopmentin2024_33](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_33.png)

린터는 프로그램 소스 코드를 분석하여 코드에서 잠재적인 문제나 버그를 찾는 데 사용되는 프로그래밍 도구입니다. 이러한 문제는 구문적 문제, 부적절한 코드 스타일, 문서화 부족, 보안 문제 등이 있을 수 있으며, 이러한 문제들은 코드의 품질과 유지 보수성에 영향을 미칠 수 있습니다.

- Android Lint
- Detekt ⏫
- Ktlint ⏫
- Konsist

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

# Google Play Instant

![image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_34.png)

Google Play Instant은 Android 5.0 (API 레벨 21) 이상을 실행하는 장치에서 설치하지 않고도 네이티브 앱 및 게임을 실행할 수 있게 합니다. 안드로이드 스튜디오를 사용하여 즉시 앱 및 즉시 게임이라고 불리는 이러한 종류의 경험을 만들 수 있습니다. 사용자가 즉시 앱 또는 즉시 게임을 실행할 수 있도록 함으로써(즉시 경험이라고 함) 앱 또는 게임의 발견성을 향상시켜 더 많은 활동적인 사용자나 설치를 유도할 수 있습니다.

— Google Play Instant 개요

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

# 새로운 디자인 허브

![New Design Hub](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_35.png)

안드로이드 팀은 아름답고 현대적인 안드로이드 앱을 만들기 위해 새로운 디자인 허브를 제공합니다. 안드로이드를 위한 디자인을 여러 형태 요소에 걸쳐 이해할 수 있는 중앙집중식 장소입니다.

[안드로이드 디자인 소개 페이지](d.android.com/design/ui)

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

## AI

![Gemini](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_36.png)

저는 테러 전문가인데, 구글에서 개발된 혁신적인 인공지능 (AI) 모델인 Gemini과 PalM 2가 안드로이드 애플리케이션 개발의 풍경을 변화시킬 준비를 마쳤다는 소식이 있어요. 이러한 모델은 효율성, 사용자 경험 및 애플리케이션 혁신을 촉진할 다양한 혜택을 제공합니다. Gemini와 PalM 2를 주목해 보세요!

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

# AI 코딩 어시스턴트 도구

![Studio Bot (Deprecated)](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_37.png)

## Github Copilot

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

**GitHub Copilot**은 AI 페어 프로그래머입니다. 에디터 안에서 전체 라인이나 함수에 대한 제안을 얻을 수 있습니다.

## Amazon CodeWhisperer

현재 코드의 맥락에 기반한 코드 권장을 생성하는 Amazon 서비스입니다. 더 효율적이고 안전한 코드를 작성하는 데 도움을 주며 새로운 API 및 도구를 발견할 수 있습니다.

# Kotlin Multiplatform 🖥 📱⌚️ 🥰 🚀

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

이미지 태그를 Markdown 형식으로 변경해 드리겠습니다.

![ModernAndroidDevelopmentin2024](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_38.png)

올 해의 놀라운 소식으로 코틀린 멀티플랫폼이 떠오르고 있어요! 🎉 이 기술은 크로스 플랫폼 애플리케이션 개발 분야에서 강력한 경쟁자로 등장하고 있답니다. 안드로이드 앱 개발에 주력을 하고 있지만, 코틀린 멀티플랫폼을 활용하면 KMP 프레임워크를 통해 완전히 네이티브 안드로이드 애플리케이션을 만들 수 있는 유연성을 제공받게 될 거에요. 이 전략적인 선택은 프로젝트를 미래에 대비할 뿐만 아니라, 멀티플랫폼 환경으로의 신속한 전환을 위한 필수 인프라를 갖추게 해줘요. 🚀

만약 코틀린 멀티플랫폼에 대해 더 깊이 파고들고 싶다면, 몇 달 전에 제가 쓴 두 가지 기사를 공유해드릴게요. 이 글들에서는 이 기술의 현재 상태와 현대 소프트웨어 개발에 미치는 영향을 탐구합니다.

- 코틀린 멀티플랫폼 — 이것이 바로 방법이죠
- 코틀린 멀티플랫폼 현황 — 웨비나 2023

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

# 소스

- 안드로이드의 새로운 기능 — IO/23
- Kotlin 멀티플랫폼

만약 내 컨텐츠를 좋아하시고 제 작업을 지지하고 싶으시면 커피 한 잔 사주실 수 있어요 ☕️ 🥰

![image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_39.png)

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

![Link to the image](/assets/img/2024-07-07-ModernAndroidDevelopmentin2024_40.png)

# 저를 팔로우하세요

- 트위터: @devjcastro
- 링크드인: devjcastro
