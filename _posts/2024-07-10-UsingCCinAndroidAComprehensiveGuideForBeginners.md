---
title: "초보자를 위한 안드로이드에서 C와 C 사용하기 종합 가이드"
description: ""
coverImage: "/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_0.png"
date: 2024-07-10 01:20
ogImage: 
  url: /assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_0.png
tag: Tech
originalTitle: "Using C C++ in Android: A Comprehensive Guide For Beginners"
link: "https://medium.com/proandroiddev/using-c-c-in-android-a-comprehensive-guide-for-beginners-8a870cf3dba6"
isUpdated: true
---





## 안드로이드 개발

![UsingCCinAndroidAComprehensiveGuideForBeginners_0.png](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_0.png)

안드로이드 개발자로서, 우리는 모두 Java 또는 Kotlin을 사용하여 아름다운 UI와 기능을 구축해 왔습니다. 앱 개발부터 실행까지의 안드로이드 스택 전체는 JVM 및 Java와 유사한 기능을 중심으로 하며, C로 작성된 커널(리눅스 커널)을 제외합니다.

프로그래밍 언어로서의 Java는 앱 개발을 위한 최상의 선택으로 만드는 많은 우수한 기능을 갖추고 있습니다. 가상 머신 실행으로 플랫폼에 독립적이며, JIT 컴파일된 결과물, 멀티스레딩 지원 및 프로그래머에게 표현력이 풍부하고 간단한 구문을 제공합니다. 플랫폼에 대한 의존성이 없는 특성으로 Java 패키지는 CPU 아키텍처 간 이동이 쉽고, 이로 인해 라이브러리 개발이 쉬워지며 플러그인, 빌드 도구 및 유틸리티 패키지 등의 전체 생태계를 향상시키게 됩니다.

<div class="content-ad"></div>

특성의 수 대비 성능 사이에는 교환 관계가 있습니다. 어셈블리 같은 언어는 메모리 및 실행 부하가 가장 적지만 프로그래머 관점에서 특성 수도 가장 적습니다. 계층 구조를 따라가면 C 및 C++ 같은 언어는 하드웨어에 더 가깝지만 다양한 기능을 제공합니다. 그 위에는 Java 및 Python 같은 언어가 있습니다. 이들은 가상 머신을 사용하여 플랫폼 의존성을 완전히 제거합니다. 이들 언어로 작성된 프로그램은 엄청난 부담이 있지만 개발자들에게는 낙원입니다.

## 안드로이드 프로젝트에서 C/C++ 지원이 필요한 이유는 무엇인가요?

위의 논의에서, 우리 시스템에서 성능이 개발 편의성보다 중요한 경우, '네이티브 언어' (C/C++)에 초점을 맞춥니다. Java/Kotlin에서부터 네이티브 언어인 C/C++로 이동합니다. 우리는 네이티브 코드의 역할과 성능 향상을 이해할 수 있는 몇 가지 예제를 살펴보겠습니다.

- 그래픽, 렌더링 및 상호작용: 사용자 인터페이스를 개발하고 꾸미는 것은 Jetpack Compose와 같은 고수준 프레임워크에서는 간단해 보일 수 있습니다. 하지만 픽셀 수준에서는 그림자의 강도, 조명 모드 및 객체의 질감을 계산하기 위해 수천 개의 계산이 이루어집니다. 이러한 계산에는 벡터 및 행렬과 그와 관련된 연산 등 선형 대수 구조물의 적극적인 활용이 필요합니다. 터치 상호작용을 처리하기 위해서는 모바일 화면의 터치 센서에서의 원시 좌표를 처리하고 클릭, 더블 클릭, 드래그 또는 스와이프 제스처를 구분해야 합니다. 이러한 계산은 하드웨어에 더 가까운 언어에서 수행하는 것이 더 나은데, 여기서 추가적인 최적화가 가능합니다.
- 머신 러닝: C/C++의 역할은 PyTorch 및 TensorFlow와 같은 인기있는 프레임워크가 상당 부분의 코드베이스를 C/C++로 작성했다는 사실로 쉽게 이해됩니다. TensorFlow는 C++로 작성된 연산을 사용할 수 있도록 래퍼(인터페이스)를 제공하며, 이러한 연산을 Python 코드에서 사용할 수 있게 합니다. C++를 채택하는 것은 당연한데, 선형 대수 연산과 CUDA(병렬 처리에 사용되는)용 코드베이스는 수년 전에 작성되었으며 많은 해 동안 검증되었기 때문입니다. TensorFlow를 위한 인터페이스 중 하나로 Python이 사용되지만, 이는 C/C++ 코드를 깔끔하고 사용하기 쉽도록 만드는 데 도움이 됩니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_1.png)

다수의 시스템들은 가독성 등을 희생하면서도 성능을 유지합니다. 다음으로, 명령 집합 구조(ISA)에 대해 간단히 논의하고, CPU 아키텍처 변경으로 프로그램 실행이 어떻게 변하는지 알아보겠습니다.

# C++가 Android 앱에 통합되는 방법 이해하기

![Image](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_2.png)


<div class="content-ad"></div>

위의 그림처럼 안드로이드에서 C/C++ 코드를 사용하는 방법이 나와있습니다. C/C++ 코드 및 Java/Kotlin 코드를 위한 두 개의 독립적인 빌드 프로세스가 존재합니다. 본 블로그에서는 C/C++ 코드 빌드 프로세스에 초점을 맞추어 코드가 JVM과 함수 호출을 어떻게 통신하는지 살펴볼 것입니다.

우선 C/C++ 및 Java 프로그램이 어떻게 컴파일되는지에 대한 간략한 개요부터 시작하여, 주로 C/C++ 컴파일의 플랫폼별 특성을 강조합니다. 그 다음으로 C/C++와 Java 코드 사이를 연결해주는 역할을 하는 JNI에 대해 설명합니다. 빌드 프로세스의 가장 아래에 위치한 CMake, shared 라이브러리 및 ABI에 대한 토론으로 마무리합니다.

자, 출발해봅시다 🚀

## 1. C++ 및 Java 프로그램의 컴파일하기

<div class="content-ad"></div>

➡️ C++은 컴파일된 언어로, 소스 코드가 실행 파일의 바이너리 코드로 변환됩니다. 실행 파일에는 소스 프로그램의 바이너리 버전, 필요한 경우 상수 및 라이브러리 코드가 포함됩니다.

➡️ 이 실행 파일은 운영 체제의 구성 요소인 로더에 의해 구문 분석되며, 이는 프로그램의 실행을 위해 메모리를 할당하고 실행 파일에서 명령을 읽습니다. 예를 들어, Ubuntu에서 사용할 수 있는 g++로 컴파일된 hello-world C++ 프로그램은 x86 또는 x86_64 명령 세트를 이해할 수 있다면 다른 Linux 배포판에서도 실행됩니다.

➡️ 모바일 장치는 arm 또는 arm64 명령 세트에서 작동하므로, x86용으로 컴파일된 프로그램은 실행되지 않습니다. 두 실행 파일 모두 로더가 보는 것과 같이 완전히 다른 언어로 작성되어 있기 때문입니다.

![image](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_3.png)

<div class="content-ad"></div>


![Image](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_4.png)

Android devices can primarily run on four architectures — arm64-v8a, armeabi-v7a, x86, and x86_64. The arm- architectures are used for ARM based processors in most Android mobile phones, while x86-based architectures are utilized on Intel or AMD processors, for example, in Windows emulators and Chromebooks.

## Java

➡️ If you’ve learned Java at some point, you might have come across the remarkable feature of platform independence, often highlighted in videos and blogs as "build once, run anywhere." Instead of converting the source code to a machine-dependent executable format, Java converts the code to an intermediate representation (IR).


<div class="content-ad"></div>

➡️ IR(Intermediate Representation)은 플랫폼에 대한 구애를 받지 않는다는 의미이며, x86 또는 ARM 플랫폼에서 생성된 IR은 명령어 집합의 차이에 관계없이 동일합니다. IR은 Java 가상 머신이라 불리는 플랫폼 의존적인 구성 요소에 의해 구문 분석되며 이를 통해 명령어를 읽고 밑바닥에있는 CPU에서 실행합니다. JVM은 IR을 한 손에, 그리고 기계의 CPU에 다른 손을 두고 있기 때문에 플랫폼에 대한 구애를 받지 않습니다.

➡️ JVM은 거의 모든 CPU 아키텍처에서 실행되며 생성된 IR이 플랫폼에 구애받지 않기 때문에 모든 플랫폼에서 작성된 Java 코드를 실행할 수 있습니다. 유일한 의존성은 대상 기계에 JVM이 설치되어 있어야 한다는 것입니다.

## Android ART와 DEX 바이트 코드

안드로이드 운영 체제는 Java 코드를 실행하기 위해 표준 JVM을 사용하지 않습니다. 패키지화된 애플리케이션인 APK는 DEX 파일(.class 파일과 유사)뿐만 아니라 네이티브 코드와 리소스를 포함합니다. DEX 파일은 OS에 의해 실행 전에 네이티브 실행 코드로 컴파일되어 사용자가 앱을 열 때 빠르게 인스턴스화될 수 있습니다.

<div class="content-ad"></div>

# 2. JNI을 사용하여 C++ 소스 코드 감싸기

안녕하세요! JNI 또는 Java Native Interface는 JVM과 네이티브 코드(C, C++ 또는 어셈블리 코드) 간의 통신을 용이하게 해주는 프레임워크입니다. 일반적으로 한 언어로 작성된 코드가 다른 언어로 작성된 코드와 통신할 수 있도록 하는 외부 함수 인터페이스(FFI)를 제공합니다. Java 소스 코드는 C++ 모듈에 존재하는 함수 정의를 찾아 JVM이 사용할 수 있도록 플래그 지정합니다.

JNI에는 jclass, jobject, jfloat, jstring 등과 같이 C++에서 각각의 Java 기본 유형(class, Object, float 및 String)을 나타내는 클래스가 포함되어 있습니다. 예를 들어 C++에서 정의된 JNI 함수는 다음과 같습니다.

```js
// C++ 소스 파일
extern "C" JNIEXPORT jstring JNICALL
Java_com_projects_ml_samplecppdemo_MainActivity_compute(
        JNIEnv* env,
        jobject instance ,
        jstring message , 
        jlong length
) {
    // 메소드 블록이 여기에 위치합니다
}
```

<div class="content-ad"></div>

당신이 MainActivity에서 계산할 Kotlin 함수에 대한 등가 Kotlin 함수가 있을 것입니다,

```js
// Kotlin 소스 파일
external fun compute( message: String , length: Long ): String
```

MainActivity.kt를 컴파일하는 동안 JVM은 코드에서 선언한 compute 함수에 대한 정의를 찾아야 합니다. 우리는 정의가 C++ 소스 파일에 포함되어 있는 것을 알고 있기 때문에, Java 프로그램에 그것을 제공하는 방법은 무엇일까요? 우리는 C++ 코드를 컴파일하고 JVM이 JNI 함수의 정의를 찾을 공유 라이브러리로 패키징합니다.

# 3. CMake과 Android NDK

<div class="content-ad"></div>

Markdown 형식으로 변경해 보겠습니다.

![이미지](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_5.png)

## 안드로이드 NDK 및 툴체인

Windows, macOS 또는 Linux 기반 운영 체제에서 안드로이드 앱을 개발합니다. 이러한 대부분의 시스템은 안드로이드 특정 ARM 아키텍처를 갖고 있지 않으며, 안드로이드 기기에서 코드를 컴파일하는 것은 불가능합니다. 그렇다면 휴대전화에서 사용하는 안드로이드 특정 ARM 아키텍처용 코드를 어떻게 컴파일할까요?

![이미지](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_6.png)

<div class="content-ad"></div>

안녕하세요! 안드로이드 NDK(Android Native Development Kit)를 사용하고 있습니다. 이 킷은 Android-ARM 라이브러리 및 실행 파일을 x86 또는 다른 arm 기기(애플 실리콘 또는 라즈베리 파이)에서 빌드하기 위한 컴파일러와 링커를 제공합니다. 이러한 다른 대상(예: Android-ARM)을 위해 코드를 작성하고 다른 대상(예: x86_64)에서 실행하는 과정을 크로스 컴파일이라고 합니다. 그러므로 Windows 기기에서 Android NDK의 컴파일러를 사용하여 앱을 위한 공유 라이브러리를 빌드할 수 있어서, 이것은 모바일 장치 즉, ARM 기기에서 완벽히 실행됩니다.

Android NDK에는 CMAKE_TOOLCHAIN_FILE이 존재하는데, 이는 CMake에 어떤 컴파일러를 사용할지 알려줍니다. 위키피디아에 따르면, 툴체인은 복잡한 소프트웨어 개발 작업을 수행하거나 소프트웨어 제품을 만드는 데 사용되는 프로그래밍 도구 세트입니다. Android NDK는 서로 다른 Android API 수준을 위해 다양한 툴체인을 제공하여 C/C++ 프로그램을 빌드하고 컴파일할 수 있습니다.

## CMake란 무엇인가요?

간단한 C++ hello-world 프로그램을 컴파일한다면, 대부분의 Linux 배포판에 사전 설치된 GNU의 g++ 컴파일러를 사용했을 것입니다.

<div class="content-ad"></div>


➡️ 단일 소스 파일 main.cpp의 경우, 한 줄의 명령어로 작업을 수행할 수 있습니다. 더 큰 코드베이스는 여러 모듈과 많은 C/C++ 소스 파일이 있을 수 있는데, 이를 컴파일하거나 공유/정적 라이브러리로 빌드해야 합니다. 이러한 코드베이스의 종속성인 다른 C++ 프로젝트도 잘 통합되어야 합니다. 이렇게 방대한 코드베이스는 컴파일에 많은 시간이 소요될 것입니다.

➡️ 이러한 문제들을 극복하기 위해 GNU의 Make 도구를 활용할 수 있습니다. 이 도구는 여러 대상을 관리하고, 점진적 빌드를 제공하며, 헤더 파일을 포함하는 능력을 지원합니다. 따라서 컴파일을 위해 여러 명령어를 실행하는 대신, 단일 Make 스크립트가 효율적으로 컴파일을 수행합니다.



➡️ CMake 최소 요구 버전을 3.22.1로 설정합니다.

project("samplecppdemo")

# CMake에게 주어진 소스 파일 native-lib.cpp을 위한 공유 라이브러리(.so)를 빌드하도록 지시합니다.
# native-lib.cpp에는 JNI 함수도 포함되어 있습니다.
add_library(
        ${CMAKE_PROJECT_NAME} 
        SHARED
        native-lib.cpp)

# CMake은 또한 현재 빌드에 다른 라이브러리를 링크할 수 있습니다.
# android 및 log는 안드로이드 특정 루틴 및 로깅을 제공합니다.
target_link_libraries(
        ${CMAKE_PROJECT_NAME}
        android
        log)


<div class="content-ad"></div>

➡️ CMake은 컴파일러에 독립적으로 Make 스크립트를 생성할 수 있으며, 의존성, 헤더 및 컴파일 시에 링크해야 하는 기타 라이브러리를 추가할 수 있는 고유의 구문을 가지고 있습니다. CMake은 Gradle과 유사하며 둘 다 빌드 시스템입니다.

빠르게 읽으시려면 이 StackOverflow 답변을 참조하세요: 코드를 컴파일할 때 Makefile과 CMake를 사용하는 차이점은 무엇인가요?

# 4. 공유 라이브러리와 ABI

➡️ C/C++ 코드의 컴파일은 실행 파일 또는 라이브러리 중 하나로 결과를 가져올 수 있으며, 이 둘 다 소스 코드의 이진 표현을 포함합니다. 실행 파일에는 실행이 시작되는 main 함수의 주소와 ELF 형식을 준수하는 추가 세부 정보가 포함되어 있습니다. 라이브러리는 다른 프로그램에서 호출할 수 있는 함수를 제공하며, 프로그램의 오브젝트 코드와 라이브러리를 링크하여 사용됩니다.

<div class="content-ad"></div>

![Using C/C++ in Android: A Comprehensive Guide for Beginners](/assets/img/2024-07-10-UsingCCinAndroidAComprehensiveGuideForBeginners_7.png)

🔍 안드로이드에서 C/C++ 파일은 .so(공유 개체) 확장자로 끝나는 공유 라이브러리로 컴파일됩니다. 이러한 라이브러리들은 (2)에서 extern으로 표시된 JNI 함수들을 노출시킵니다. JVM은 .so 파일의 코드를 참고하여 해당 기능의 바이너리 코드를 장치에서 실행하는 데 사용할 수 있습니다.

🔍 소스 코드와 라이브러리 코드 간의 해당 이진 수준의 상호작용은 일반적으로 애플리케이션 이진 인터페이스 (ABI)를 통해 발생합니다. 한편, 응용 프로그램 프로그래밍 인터페이스 (API)는 일반적으로 컴파일 이전에 소스 코드 수준에서 이러한 상호작용을 용이하게 합니다.

ABIs에 대한 직관적인 설명을 보려면 LinkedIn 게시물을 확인해보세요 — 두 개의 소프트웨어 요소가 소스 코드에서 통신해야 할 때, 우리는 API를 사용합니다. 그렇다면 두 이진 모듈이 통신하려면 어떻게 해야할까요?

<div class="content-ad"></div>

# 결론

이제 JVM은 공유 라이브러리에 노출된 함수에 액세스할 수 있으며, OS는 필요할 때 이를 실행합니다.

이 기사가 흥미로웠고 새로운 것을 배우셨기를 바랍니다. 궁금한 사항이나 제안 사항은 아래 댓글에 공유해 주세요. 즐거운 하루 되세요!