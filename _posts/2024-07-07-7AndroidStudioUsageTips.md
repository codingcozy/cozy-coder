---
title: "Android Studio 사용을 위한 7가지 팁"
description: ""
coverImage: "/assets/img/2024-07-07-7AndroidStudioUsageTips_0.png"
date: 2024-07-07 23:20
ogImage: 
  url: /assets/img/2024-07-07-7AndroidStudioUsageTips_0.png
tag: Tech
originalTitle: "7 Android Studio Usage Tips"
link: "https://medium.com/@domen.lanisnik/android-studio-usage-tips-4c6c47b716e8"
---


안드로이드 스튜디오는 사용하기 쉬운 강력한 IDE입니다. 그러나 명확하지 않지만 개발자 생산성을 향상시키고 앱 개발을 더 쉽게 만들어 주는 기능도 지원합니다.

다음은 아마도 유용하게 활용할 수 있는 안드로이드 스튜디오 사용 팁 몇 가지입니다.

![안드로이드 스튜디오 사용 팁](/assets/img/2024-07-07-7AndroidStudioUsageTips_0.png)

## 1. Logcat을 사용하여 스크린샷 찍기 및 화면 녹화하기

<div class="content-ad"></div>

로그캣 창 안에는 현재 연결된 장치의 화면을 쉽게 캡처하거나 녹화할 수 있는 버튼이 있습니다.

로그캣 창의 왼쪽 메뉴에 있는 카메라 버튼을 클릭하면 스크린샷을 쉽게 찍을 수 있습니다. 1초 정도 후에 캡처된 화면을 확인할 수 있습니다. 저장하기 전에 이미지의 크기를 조절하거나 잘라내거나 회전시킬 수 있습니다.

![Android Studio Tips](/assets/img/2024-07-07-7AndroidStudioUsageTips_1.png)

화면을 녹화하려면 스크린샷 아이콘 아래의 비디오 카메라 버튼을 클릭하십시오. 비트 속도와 해상도를 선택할 수 있는 다이얼로그가 나타납니다. 녹화 중에는 화면 미리보기가 표시되지 않습니다. 녹화를 중지하면 컴퓨터에 녹화한 파일을 저장할 위치를 선택하라는 메시지가 표시됩니다.

<div class="content-ad"></div>

![Screenshot of Android Studio Usage Tips](/assets/img/2024-07-07-7AndroidStudioUsageTips_2.png)

**2. 키보드 단축키 배우고 활용하기**

안드로이드 스튜디오는 네비게이션부터 리팩터링, 디버깅까지 필요한 모든 일반 작업에 대한 키보드 단축키를 제공합니다. 마우스 대신 키보드를 사용하는 방법을 익히는 것은 어렵게 느껴질 수 있지만, 장기적으로 생산성을 높이는 데 도움이 될 것입니다.

모든 단축키를 외우려고 하지 말고, Key Promoter X라는 IDE 플러그인을 설치할 수 있습니다. 이 플러그인은 IDE 내부 버튼에 마우스를 사용할 때 대신 사용해야 할 키보드 단축키를 보여줍니다. 그리고 자주 사용하는 동작에 대해 단축키가 없다면 해당 동작에 대한 단축키 생성을 제안해줍니다.

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 수정하세요.

플러그인을 설치하려면 설정으로 이동한 뒤 - ` 플러그인에서 Key Promoter X를 마켓플레이스에서 검색하고 설치를 클릭하세요.

이미지 태그를 Markdown 형식으로 수정하였습니다.

3. 실행 창 전환 비활성화

<div class="content-ad"></div>

최근의 안드로이드 스튜디오 버전은 앱을 배포한 후 자동으로 Logcat에서 Run 창으로 전환됩니다. 이것은 여러분이 앱을 실행하기 전 몇 초 전에 전환했더라도 수동으로 탭을 전환해야 해서 귀찮을 수 있습니다.

이러한 동작을 비활성화하려면 Run - Edit Configurations로 이동하고 General 탭 하단으로 스크롤하여 Activate tool window가 선택 해제되어 있는지 확인하십시오.

![Android Studio](/assets/img/2024-07-07-7AndroidStudioUsageTips_5.png)

## 4. 각 앱 실행 시 Logcat 자동으로 표시하기

<div class="content-ad"></div>

이전 설정과 관련하여, 앱을 배포할 때 자동으로 Logcat 탭이 열리도록 설정할 수 있습니다. 또한 로그를 지우는 옵션도 설정할 수 있습니다.

현재 하단 창 전체를 닫거나 다른 탭을 선택한 경우에도 Logcat이 열립니다.

각 프로젝트마다 이 작업을 수행해야 합니다. Run - Edit Configurations로 이동한 다음 Miscellaneous 탭을 선택합니다. 그런 다음 “Show logcat automatically” 및 “Clear log before launch” 옵션을 활성화할 수 있습니다.

[Android Studio Usage Tips](/assets/img/2024-07-07-7AndroidStudioUsageTips_6.png)

<div class="content-ad"></div>

## 5. 내장 Git 클라이언트 사용하기

Android Studio에는 다양한 기능을 갖춘 내장 Git GUI 클라이언트가 함께 제공됩니다. GitHub Desktop이나 SourceTree 같은 제3자 독립형 GUI 클라이언트를 사용하고 있다면 내장 클라이언트를 사용함으로써 작업 흐름을 단순화할 수 있습니다. 콘솔 사용자라면 아마도 직접 git 명령을 실행하는 것을 선호할 것입니다만, 여전히 이 기능이 유용하다고 느낄 수 있을 겁니다.

포함된 기능으로는 fetch, pull, push, 새 브랜치 만들기와 같은 기본적인 기능들뿐만 아니라, 강제 푸시(force-push), rebase, cherry-pick 등과 같은 더 고급 사용 사례들도 Android Studio에서 지원합니다.

병합 충돌 해결
양쪽 브랜치에서의 변경 사항과 최종 결과를 가운데에 표시해주는 훌륭한 대화식 병합 충돌 해결 도구도 제공됩니다. 수동 편집도 가능합니다.

<div class="content-ad"></div>

Shelving changes
또한, 안드로이드 스튜디오는 변경 내용을 보관하거나 감추는 기능을 지원합니다. 이는 아직 커밋하지 않은 보류 중인 변경 사항을 저장하고 필요할 때 복원할 수 있는 것을 의미합니다. 예를 들어, 다른 작업으로 전환해야 하는 경우 변경 사항을 나중에 작업하기 위해 옆으로 옮겨놓고 싶을 때 유용합니다.

![Android Studio Usage Tips](/assets/img/2024-07-07-7AndroidStudioUsageTips_7.png)

또한, 안드로이드 스튜디오는 Droidcon Italy에서 볼 수 있는 이 토크에서 더 많은 Git 기능을 지원합니다: [Droidcon Italy YouTube 링크](https://www.youtube.com/watch?v=XMUnUotuvGw).

## 6. ADB Idea 플러그인 설치하기

<div class="content-ad"></div>

Android Studio은 유용한 기능을 추가하는 서드파티 플러그인에 방대한 지원을 제공합니다. 그 중 하나는 ADB Idea 플러그인으로, 일반적인 수동 작업을 빠르게 실행할 수 있는 간편한 방법을 제공합니다.

기기 설정의 다양한 메뉴를 클릭해서 앱 데이터를 삭제하거나 권한을 취소하는 대신, Android Studio에서 한 번의 버튼 누르기로 할 수 있습니다.

![Android Studio Tips](https://yourwebsite.com/assets/img/2024-07-07-7AndroidStudioUsageTips_8.png)

플러그인을 설치하려면 설정 - 플러그인으로 이동하여 마켓플레이스에서 ADB Idea를 검색하고 설치 버튼을 클릭하면 됩니다.

<div class="content-ad"></div>

![Screenshot](/assets/img/2024-07-07-7AndroidStudioUsageTips_9.png)

## 7. 디버거 배우기

안드로이드 스튜디오의 디버거는 다양한 기능을 갖춘 강력한 도구입니다. 효과적으로 사용하면 디버깅 작업이 더 생산적이고 성공적일 수 있습니다.

앱을 다시 실행하는 대신 디버거를 연결하세요
디버깅을 시작하는 두 가지 방법이 있습니다. 하나는 실행 버튼 옆의 녹색 버그 버튼을 클릭하여 앱을 실행하는 것입니다. 기존에 실행 중인 경우 앱을 다시 시작하며 시간이 조금 더 소요될 수 있습니다.

<div class="content-ad"></div>

빠른 방법은 툴바에 있는 화살표가있는 버그 버튼을 클릭하여 이미 실행 중인 응용 프로그램에 디버거를 연결하는 것입니다. 이렇게 하면 새 창이 열리고 프로세스를 선택할 수 있습니다. 중단점 내에 체크 마크가 표시되면 디버깅을 시작할 수 있습니다.

![Android Studio Debugger](/assets/img/2024-07-07-7AndroidStudioUsageTips_10.png)

키보드 바로 가기키 사용하기
"Step into"이나 "Step over"와 같은 실행 버튼은 디버깅을 더 빠르게 할 수있는 키보드 바로 가기키가 있습니다. Key Promoter X를 사용하여 쉽게 배울 수 있습니다.

![Android Studio Keyboard Shortcuts](/assets/img/2024-07-07-7AndroidStudioUsageTips_11.png)

<div class="content-ad"></div>

조건부 중단점
조건부 중단점을 설정할 수 있습니다. 이는 지정한 조건이 참으로 평가될 때만 트리거되는 중단점을 설정하는 기능입니다. 이를 통해 디버깅을 가속화할 수 있으며 관심 없는 경우에는 중단점이 동작하지 않습니다.

조건부 중단점을 생성하는 한 가지 방법은 먼저 라인 번호를 클릭하여 일반 중단점을 설정하는 것입니다. 그런 다음 중단점을 마우스 오른쪽 버튼으로 클릭하여 팝업을 열고 조건을 작성하면 됩니다.

예를 들어, 아래 이미지의 중단점은 newsResourceId 매개변수가 google과 동일한 경우에만 트리거됩니다.

![이미지](/assets/img/2024-07-07-7AndroidStudioUsageTips_12.png)

<div class="content-ad"></div>

범례브레이크포인트를 생성하는 또 다른 방법은 디버그 창에서 중단점 창을 열어서 기존의 중단점을 선택하고 조건을 추가하는 것입니다.

![Breakpoint window in the debug window](/assets/img/2024-07-07-7AndroidStudioUsageTips_13.png)

조건식 평가하기
디버거를 사용할 때 런타임에서 특정 변수, 매개변수 또는 표현식의 값을 확인할 수 있습니다. 함수 매개변수의 경우 일반적으로 그 값이 바로 옆에 표시됩니다. 다른 표현식은 다음과 같이 평가할 수 있습니다:

- 표현식을 선택하고 "Watch 추가"를 클릭합니다.
- 표현식을 선택하고 디버깅 창으로 드래그하여 놓습니다.
- 표현식을 수동으로 표현식 입력란에 작성합니다.

<div class="content-ad"></div>

마지막 옵션은 변수의 값뿐만 아니라 추가 속성도 볼 수 있게 합니다.

![debugger](https://miro.medium.com/v2/resize:fit:1200/1*d8mhVZnc4Ey0H2xxzFTQHA.gif)

공식 문서에서 디버거의 기능에 대해 더 읽어보세요.

![android studio usage tips](/assets/img/2024-07-07-7AndroidStudioUsageTips_14.png)

<div class="content-ad"></div>

# 결론

안드로이드 스튜디오에는 많은 기능들이 숨어 있지만, 이를 통해 더 생산적이고 개발을 간소화할 수 있습니다. 이 기사에서는 안드로이드 스튜디오를 최대한 활용하는 몇 가지 팁을 다뤘습니다. 이를 유용하게 활용하시고 일상적인 개발 루틴에 반영해보시기를 바랍니다.

안드로이드 스튜디오의 많은 다른 유용한 기능들이 있습니다. 댓글에서 여러분이 좋아하는 안드로이드 스튜디오 생산성 팁을 알려주세요.

만약 토크를 통해 배우는 것을 선호하신다면, 여기 Droidcon Berlin의 안드로이드 스튜디오에서 더욱 효율적으로 일할 수 있는 방법에 대한 멋진 토크가 있습니다: [Droidcon Berlin - 안드로이드 스튜디오에서 전문가가 되는 방법](https://www.droidcon.com/ 2021/ 11/ 10/ become-a-pro-in-android-studio-dcbln21/)