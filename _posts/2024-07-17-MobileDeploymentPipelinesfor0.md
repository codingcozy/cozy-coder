---
title: "제로 비용으로 모바일 배포 파이프라인 구축하기"
description: ""
coverImage: "/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_0.png"
date: 2024-07-17 23:56
ogImage: 
  url: /assets/img/2024-07-17-MobileDeploymentPipelinesfor0_0.png
tag: Tech
originalTitle: "Mobile Deployment Pipelines for 0"
link: "https://medium.com/gitconnected/mobile-deployment-pipelines-for-0-f0ec86b2269d"
---



![이미지](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_0.png)

지속적 통합(CI)은 시니어 개발자로 성장하기 위한 중요한 요소 중 하나에요.

만약 당신이 초록색 프로젝트에 전념하고 있다면, 효과적인 빌드, 테스트 및 배포 파이프라인을 설정하는 여정은 흔한 것이죠. 하지만 만약 당신이 하루에 성숙한 프로젝트에 종사하고 있다면 어떨까요?

CI 파이프라인을 처음으로 구축하는 경우, 이는 새로운 프로젝트에 대한 필수 조건이 되어 당신이 엔지니어로서 불리함을 가져올 수 있어요. 오늘은 사이드 프로젝트에 CI를 설정하는 방법에 대해 보여 드릴게요. 무료로요!


<div class="content-ad"></div>

- 부분 I: 패스트레인
- 부분 II: 앱스토어 커넥트
- 부분 III: GitHub 액션

나는 내 신뢰할 수 있는 오픈소스 프로젝트인 Bev에 대한 CI를 구현 중이지만, 당신의 코드베이스에 동일한 결과를 얻기 위해 나의 안내를 따르세요.

# 부분 I: 패스트레인

패스트레인은 빌드 및 배포를 자동화하는 Ruby 스크립트 모음인 오픈소스입니다. 본질적으로 xcodebuild 또는 Gradle 명령어 위에 사용자 친화적인 래퍼로, 표준 워크플로우를 자동화하는 데 도움이 되는 기능입니다.

<div class="content-ad"></div>

Fastlane을 설정하려면 문서를 참조해주세요.

테스트용으로 두 가지 아주 간단한 레인을 설정해보겠습니다 (각 PR에서 실행되는 레인과 main 브랜치로 병합할 때 실행되는 레인).

## fastlane test

Fastfile에서 구성된 테스트 레인은 더 간단할 수 없습니다.

<div class="content-ad"></div>

```js
desc "각 PR에서 테스트 실행"
lane :test do
    scan(device: "iPhone 15 Pro",
         scheme: "BevTests")
end
```

scan은 앱의 주어진 scheme에 대한 테스트를 실행합니다 — 우리는 여기서 포괄적인 단위 및 UI 테스트 스위트가 포함된 테스트 계획을 설정했습니다. 우리는 로컬에서 빠르게 fastlane test를 실행하고 첫 성공을 빠르게 확인할 수 있습니다.

<img src="/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_1.png" />

## fastlane deploy


<div class="content-ad"></div>

배포 레인은 조금 더 복잡합니다. 인증서를 관리하고 앱을 아카이브하고 App Store Connect에 제출해야 합니다. 다음은 약간 간소화된 형식입니다.

```js
desc  "App Store Connect로 앱 배포"
lane :deploy do 
    match
    gym
    api_key = app_store_connect_api_key()
    deliver(api_key: api_key)
end 
```

거꾸로 진행하면...

- deliver는 서명된 .ipa 파일을 App Store Connect에 업로드합니다.
- app_store_connect_api_key는 다른 Fastlane 명령어와 함께 사용할 수 있는 형식으로 API 토큰을 불러옵니다.
- gym은 앱의 릴리스 변형을 빌드하고 .ipa를 패키지화합니다.
- match는 인증서와 프로비저닝 프로파일을 설치합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_2.png)

처음으로 시도한 건 아니겠지만, 이게 처음부터 바로 작동했다고 하면 놀랄 일은 아니에요. 실제로 많은 설정 작업을 거쳐야만 했죠.

match는 우리의 지속적 통합에서 첫 번째 주요 어려움을 겪은 부분이에요.

## fastlane match


<div class="content-ad"></div>

match 명령어는 팀의 코드 서명 인증서와 프로필의 생성, 저장 및 관리를 자동화하고 통일하는 데 도움을 줍니다. 무엇보다 중요한 것은 CI 머신이 릴리스에 서명할 수 있는 능력을 제공하면서 Git에 자격 증명을 저장할 필요가 없게 합니다.

이것은 주니어 개발자들이 CI를 보호하는 것을 좋아하는 주요 이유 중 하나입니다: match는 이해해야 하는 필수 요소이며 익숙해지기에는 약간 무서울 수 있습니다.

설정하려면 fastlane match init를 실행하고 CLI에서 단계별 가이드를 따라 인증서와 프로비저닝 프로필을 안전하게 저장하세요.

<img src="/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_3.png" />

<div class="content-ad"></div>

개인적으로 Google Cloud 저장 모드를 선호합니다. 그 이유는 (1) 비밀을 관리하기 매우 간단하고, (2) 대규모 무료 티어가 제공되며, (3) 대부분의 모바일 개발자가 이미 Firebase를 통해 Google 클라우드 계정을 보유하고 있기 때문이며, (4) 프로젝트 설정이 매우 쉽기 때문입니다.


![이미지](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_4.png)


fastlane match를 작동시키려면 gc_keys.json 파일을 프로젝트 폴더에 넣어두어야 합니다. 이 파일이 소스 관리에서 제외되도록 .gitignore 파일에 추가하는 것이 중요하니 즉시 처리해주세요. 곧이어 이러한 키들을 시스템 CI에 시크릿으로 추가할 것입니다.

이제 fastlane match appstore를 실행하고 App Store Connect 자격 증명을 입력하고 키 저장소에 배포 프로필을 설정할 수 있습니다. fastlane match는 인증서 및 프로필을 생성하고 방금 설정한 Google 클라우드 키 저장소에 저장합니다.

<div class="content-ad"></div>

## 전체 배포 파이프라인

이제 코드 서명을 설정했으니, 나머지 배포 파이프라인을 작성할 수 있습니다:

```js
lane :deploy do 
  match(readonly: true)
  api_key = app_store_connect_api_key(
    key_id: "V4D62Q8UQB",
    issuer_id: "69a6de92-2bb4-47e3-e053-5b8c7c11a4d1",
    key_content: $APP_STORE_CONNECT_API_KEY_KEY,
    is_key_content_base64: true
  )
  increment_build_number({
    build_number: latest_testflight_build_number(api_key: api_key) + 1
  })
  gym(export_options: "./fastlane/ExportOptions.plist")
  deliver(
    api_key: api_key,
    force: true,
    skip_screenshots: true,
    precheck_include_in_app_purchases: false
  )
end
```

이전 간소화된 버전에 몇 가지 개선 사항이 있습니다.

<div class="content-ad"></div>

- match는 읽기 전용 모드에서 실행 중이며 새 인증서 및 프로필을 생성하지 않아도 되므로 CI 시스템에 권장됩니다.
- api_key는 App Store Connect API 키를 가져와 배포 단계를 자동화할 수 있게 해줍니다. 다음 섹션에서 모든 것을 설명하겠습니다.
- increment_build_number은 빌드 번호를 자동으로 증가시켜주어 추적할 필요가 없게 하며, TestFlight에서 최신 정보를 가져옵니다.
- gym은 이제 앱을 패키징하는 방법을 xcodebuild에 알려주는 로컬 파일에서 내보낼 옵션을 가져옵니다.
- deliver는 몇 가지 기본값을 재정의하여 업로드 속도를 빠르게 만듭니다.

이제 로컬 파이프라인이 마련되어 작동하고 있으므로 인프라를 구축할 차례입니다: App Store 및 GitHub Actions 러너.

# Part II: App Store Connect

앱을 실제로 배포할 수 있도록 보장하려면 App Store Connect로 이동해야 합니다. 이미 등록되지 않은 경우 지금 추가해주세요.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_5.png)

## App Store Connect API 키 생성하기

앱 스토어 커넥트 API 키를 설정하기 위해 사용자 및 액세스로 이동하세요. 나중에 배포를 시작할 때 이 키가 필요합니다!

![Image](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_6.png)


<div class="content-ad"></div>

파일을 .p8 형식으로 다운로드하고 이 파일을 안전한 곳에 저장해주세요.

파일은 어렵지만 문자열은 쉽습니다. 그래서 저는 Fastlane에서 사용할 키를 base64로 인코딩하는 것을 좋아합니다. 키를 포함하고 있는 폴더 안에서 다음 터미널 명령을 사용해주세요:

```js
cat AuthKey_A4D72Q2UQC.p8 | base64
```

이 인코딩된 API 키를 가져와 App Store Connect 사용자 및 액세스 페이지에 나열된 key_id 및 issuer_id와 함께 Fastlane 스크립트에 추가할 수 있습니다.

<div class="content-ad"></div>

```js
api_key = app_store_connect_api_key(
  key_id: "V4D62Q8UQB",
  issuer_id: "69a6de92-2bb4-47e3-e053-5b8c7c11a4d1",
  key_content: $APP_STORE_CONNECT_API_KEY_KEY,
  is_key_content_base64: true
)
```

key_content에 들어간 APP_STORE_CONNECT_API_KEY_KEY는 무엇일까요?

다음에 공개할 비밀입니다.

이 API 키는 앱을 제출하는 데 사용되므로 보안을 위해 보관하고 소스 제어에서 멀리해야 합니다. 일단 안전한 장소에 base-64로 인코딩된 키를 저장해 두세요. GitHub Actions 설정 시에 비밀을 처리할 때에는 섹션 III에서 다룰 것입니다.

<div class="content-ad"></div>

로컬에서 테스트하려면, base-64로 인코딩된 API 키를 하드 코딩하여 fastlane deploy를 실행해 보세요. 이렇게 하면 레인을 올바르게 실행하고 App Store Connect에 업로드된 앱을 볼 수 있을 거에요. 다만 API 키를 포함한 fastfile을 커밋하지 않도록 주의하세요.

## 앱별 비밀번호

일부 Fastlane 서비스에는 API 키만으로 충분하지 않은 경우가 있습니다.

모든 가능성을 고려하기 위해 Fastlane 서비스용 앱별 비밀번호를 생성할 수도 있습니다. 이렇게 하면 자동화된 프로세스가 귀하의 App Store Connect 계정으로 인증되게 되죠.

<div class="content-ad"></div>

appleid.apple.com 에 가서 "로그인 및 보안"을 선택한 다음, "앱별 비밀번호"를 선택하여 하나를 추가하세요.

![이미지](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_7.png)

이 비밀번호를 GitHub Actions secrets에 곧 추가할 때까지 안전한 곳에 기록해 두세요.

이제 본격적으로 시작할 준비가 되었습니다.

<div class="content-ad"></div>

# 파트 III: GitHub Actions

파스트레인 스크립트를 좋은 설정으로 준비했습니다. 앱을 앱 스토어 커넥트에 설정하고 Google Cloud 키스토어를 만들었습니다. 마지막으로 자동화 인프라의 마지막 조각을 설정할 수 있게 되었어요: GitHub Actions.

## 비밀 안전하게 저장하기

우선 하드코딩된 비밀 값을 제거하고 GitHub Actions의 Settings → Secrets and variables → Actions → Repository Secrets에 base-64로 인코딩된 $APP_STORE_CONNECT_API_KEY_KEY를 저장해봐요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_8.png" />

깃허브 액션에서 실행 중인 경우 Fastlane 스크립트를 실행하는 워크플로 파일(자세한 내용은 아래 참조)은 환경 프로퍼티로 시크릿에 액세스할 수 있습니다.

```js
APP_STORE_CONNECT_API_KEY_KEY: ${secrets.APP_STORE_CONNECT_API_KEY_KEY}
```

Google Cloud Storage 키에 대해 동일한 작업을 수행해 보죠 - gc_keys.json을 시크릿으로 추가하여 CI가 안전하게 소스 제어에서 사용하지 않으면서 액세스할 수 있도록 합니다.

<div class="content-ad"></div>

만약 이것을 잃어버린다고 해도 걱정하지 마세요 — 다시 fastlane match를 실행하여 키, 인증서 및 프로필을 다시 생성할 수 있습니다.

모두를 정리해보면 일반적으로 다음과 같은 4가지 비밀이 있을 것입니다:
- Base-64로 인코딩된 App Store Connect API 키
- Apple 앱별 비밀번호
- Google Cloud Storage 버킷 액세스 키
- Fastlane 세션 자격 증명*

이제 모든 비밀이 행복하게 저장되었으므로 시작할 수 있습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-17-MobileDeploymentPipelinesfor0_9.png)

## Self-hosted runners

안타깝지만, GitHub Actions의 클라우드 호스팅된 MacOS 러너는 Linux 러너보다 분당 10배 비용이 듭니다. 공개 저장소는 매달 200분의 Mac 러너 시간이 부여되지만, 비공개나 특히 활발한 저장소에게는 비용이 많이 들 수 있습니다.

하지만 저는 여러분에게 CI 비용이 $0임을 약속했으니 걱정하지 마세요.


<div class="content-ad"></div>

로컬 머신을 설정할 수 있어요 — 심지어 표준 개발용 랩톱도 가능합니다! — 자체 호스팅 러너로 설정할 수 있어요. GitHub에서 잘 문서화된 간단한 프로세스에 따라 설정할 수 있어요. 참고로, 공개 repo에서 자체 호스팅 러너를 실행하는 것은 위험할 수 있어요.

모든 설정을 마쳤다면, workflow 파일에서 runs-on 속성을 self-hosted로 설정하여 머신(또는 선택한 태그)을 가리킬 수 있어요. 마지막으로 action-runner/run.sh 쉘 스크립트를 실행하여 리스너를 시작하세요.

## 테스트 workflow

저희의 자동화 스크립트는 repo의 .github/workflows 폴더에 있어요. 형식은 기존의 .yaml로, 언제 어디서 무엇을 실행할지 정의하고 있어요.

<div class="content-ad"></div>

```yaml
name: Test
on:
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: self-hosted
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v3

      - uses: maxim-lobanov/setup-xcode@v1
        with:
          xcode-version: 15.2

      - name: Run Unit Tests
        run: fastlane test
```

위의 코드에서 **on:** 매개변수는 workflow를 언제 트리거할지 정의합니다: 이 경우 PR이 main 브랜치로 작성될 때마다 테스트 스위트를 실행하고 싶습니다.

**jobs:** 매개변수는 실행하려는 작업 중 하나인 테스트 스위트를 표시합니다.

위에서 언급했듯이, **runs-on:** 매개변수는 pipeline을 실행할 기계(또는 기계)를 선택할 수 있도록 합니다. self-hosted는 지역 장치를 사용하도록 지정하지만 여기에 태그를 추가하거나 macos-13(또는 다른 버전 번호)를 입력하여 클라우드 호스팅 러너를 요청할 수도 있습니다.

<div class="content-ad"></div>

이제 우리는 언제(예: on: 트리거)와 어디서(runs-on:)를 알게 되었으니, 이제 마침내 무엇을 정의할 수 있습니다. 다음은 테스트 파이프라인에서 실행해야 할 작업을 정의합니다:

- actions/checkout은 소스 코드 저장소를 체크아웃하는 표준 내장 작업입니다.
- setup-xcode는 올바른 Xcode 버전 및 해당 빌드 도구 체인이 사용되도록 보장하는 필수 작업입니다. 로컬에서 실행할 때, 정의된 버전이 설치되어 있어야 합니다.
- fastlane test은 마침내 우리의 Fastlane 스크립트를 실행합니다.

이제 워크플로에 모든 것을 설정했으므로 PR을 생성하고 소스 저장소의 Actions 탭에서 실행되는 것을 볼 수 있습니다.

이 테스트 워크플로를 설정했으므로, 풀 리퀘스트를 생성할 때마다 테스트를 파괴하는 회귀를 만들었는지 여부를 알 수 있습니다.

<div class="content-ad"></div>

하지만 더 나아가서 고려해볼 사항이 있습니다: 저장소 설정에서 Branches로 이동하여 Branch protection rule을 설정할 수 있습니다. main 브랜치에 병합하기 전에 테스트 동작이 통과되도록 하는 규칙을 강제할 수 있습니다.

이제 PR 워크플로우가 끝내주게 잘 구성되었으니, 마지막 보스에 도전해 볼 시간입니다: 배포 워크플로우.

## 배포 워크플로우

테스트 워크플로우에서 단일 Fastlane 명령을 실행하는 것이 잘 작동했을 것입니다, 특히 자체 호스팅 러너를 사용했다면.

<div class="content-ad"></div>

배포 워크플로우는 전혀 다른 녀석이에요.

프로비젼, 인증, 빌드 과정 간에 잘못될 수 있는 것들의 범위는 훨씬 더 커요. 특히 클라우드 호스팅 러너를 선택하고 모든 것을 버전별로 처리해야 할 때, 이 문제는 더욱 커져요.

이것이 마지막 주요 어려운 부분이랍니다. 이 고통을 이겨내는 것이 진짜 전문가와 아마추어를 나누는 요요의 순간이에요.

시니어들은 CI의 게이트를 막아둔 것이 아니랬어요.

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경했습니다.

우리는 너에게 고통을 덜 주고 싶었어.

우리의 최종, 전투 검증된 배포 워크플로우가 완성되었어:

```js
name: Deploy
env:
  FASTLANE_SESSION: ${ secrets.FASTLANE_SESSION }  
  GOOGLE_CLOUD_KEYS: ${ secrets.GOOGLE_CLOUD_KEYS }
  APP_STORE_CONNECT_API_KEY_KEY: ${ secrets.APP_STORE_CONNECT_API_KEY_KEY }
  FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD: ${ secrets.FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD }
  
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: self-hosted
    timeout-minutes: 10
    steps:    
      - uses: actions/checkout@v3

      - uses: maxim-lobanov/setup-xcode@v1
        with:
          xcode-version: 15.2

      - name: Create gc_Keys
        run: echo $GOOGLE_CLOUD_KEYS >> gc_keys.json

      - name: App Store Deploy
        run: fastlane deploy
```

이 워크플로우는 매우 유사해 보이며, 많은 동일한 단계가 있습니다:

<div class="content-ad"></div>

- 환경을 설정하고 모든 비밀을 가져와서 워크플로우에서 환경 변수로 접근할 수 있도록 했습니다. 이렇게 하면 Fastlane 스크립트에서 비밀을 볼 수 있습니다.
- 트리거는 이제 push to main으로 설정되어 있어요. 이는 위 워크플로우에서 PR이 병합되면 실행됩니다.
- 마찬가지로 저장소를 체크아웃하고 setup-xcode를 준비합니다.
- 그 다음으로, GOOGLE_CLOUD_KEYS 비밀을 지역 .json 파일에 작성할 수 있는 매우 간단한 스크립트를 실행합니다. 이 파일은 Fastlane 스크립트에서 접근 가능합니다.
- 마지막으로, 미리 설정한 Fastlane 스크립트를 토글하는 fastlane deploy를 실행합니다.

한 번 더 강조하겠습니다. (즉, 사용자 고유의 기기)는 꽤 잘 작동하는 경우가 많습니다. GitHub Actions의 클라우드 호스트 리터들은 종종 최신 OS와 SDK를 사용할 때 이해할 수 없는 오류를 일으킬 수 있습니다. (SwiftData는 전혀 작동이 안 되는 경우도 있습니다).

저는 이곳에서 고통을 대충 훑어보았지만, 클라우드 호스트 리터가 작동하도록 여러 차례 시도한 것을 확인하실 수 있습니다. 솔직히 말해서, 사용자 고유의 리터가 훨씬 쉽습니다!

하지만 앞서 말한대로, 이 튜토리얼은 CI 워크플로우를 $0으로 설정하는 데 중점을 두었고, 그 목표를 달성했습니다.

<div class="content-ad"></div>

수고가 많았어요! 자체 호스팅 러너에서 배포 워크플로우가 잘 작동 중이에요!

# 결론

지속적인 통합 파이프라인을 설정하여 프로젝트를 테스트, 빌드 및 배포하는 것은 익숙하지 않은 경우에는 어려울 수 있지만 고통스럽지 않아도 됩니다.

Fastlane 및 GitHub Actions를 사용하면 이러한 자동화 기술을 연습할 수 있는 모의실을 제로 달러의 비용으로 만들 수 있어요. 무언가를 배우셨으면 좋겠고, CI를 설정한 적이 없다면 새로운 프로젝트에 적용해보시기를 강력히 추천드려요!

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.