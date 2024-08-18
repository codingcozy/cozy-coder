---
title: "Crowdin을 이용해 5분만에 Nextjs 앱 번역하는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 02:49
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How to Translate Your Nextjs App in 5 Minutes With Crowdin"
link: "https://dev.to/zerodays/how-to-translate-your-nextjs-app-in-5-minutes-with-crowdin-1pe0"
isUpdated: false
---


대부분의 웹사이트는 Webflow나 WordPress와 같은 콘텐츠 관리 시스템을 사용하므로 로컬라이제이션이 큰 문제가 되지 않습니다. 시스템을 통해 비교적 쉽게 처리할 수 있기 때문이죠.

하지만 Next.js나 순수 React를 사용한 맞춤형 웹 애플리케이션과 같은 사용자 정의 솔루션의 경우, 이 작업이 상당히 까다로울 수 있습니다.

고객이 제품을 다른 언어로 제공하기를 원할 때, 개발자 중 한 명이 코드를 열어 문자열을 수동으로 번역해야 하는 경우가 많습니다. 번역 과정이 종종 아웃소싱하기에는 기술적으로 너무 복잡하므로 완전히 번역자에 의해 수행될 수 없을 때입니다.

이제 웹 앱 전체를 하나 이상의 언어로 번역해야 한다고 상상해보세요. 개발자들이 할 시간이 없는 일들의 산더미라고 생각하시면 됩니다.

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

저희는 시장에서 이 문제에 대한 해결책이 있는지 확인하기로 결정하고 조사를 좀 한 후에 Crowdin을 시도하기로 결정했고, 정말 멋지다고 생각해요! Crowdin은 다음을 제공합니다:

- 거의 모든 언어 지원
- 600개 이상의 앱과 통합(깃허브 통합 포함)
- 문자열 번역을 위한 쉬운 UI

우리의 요구에 맞게 Crowdin 주변에 로컬라이제이션 파이프라인을 개발했고, 여기에 대해 공유하고 싶어요.

## 로컬라이제이션 파이프라인

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

우리 애플리케이션에서 번역 가능한 문자열은 특정 번역 파일에 저장됩니다 (일반적으로 JSON 형식으로 저장됩니다). 번역 파일에 번역 가능한 문자열을 저장하는 것 외에도, 우리는 보통 일종의 국제화 프레임워크를 사용합니다 (예를 들어 Next.js 애플리케이션에서는 next-i18next를 사용합니다).

폴더 구조 예시:

```js
/project-root 
  /public 
    /locales 
      /en-INTL 
        common.json
      /sl-SI 
        common.json
```

예시 번역 파일 common.json:

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

```json
{
  "welcome_message": "저희 앱을 이용해 주셔서 환영합니다!",
  "login_button": "로그인",
  "logout_button": "로그아웃"
}
```

번역 파일을 최신 상태로 유지하기 위해 소스 파일을 업로드하고 번역을 다운로드하기 위해 Crowdin CLI를 사용합니다.

문자열이 Crowdin에 제공되면 저희 고객 또는 전용 번역자들이 Crowdin 웹 플랫폼 내에서 해당 언어로 문자열을 번역합니다.

번역이 완료되면 Crowdin은 GitHub과 연동하여 자동으로 다음 단계를 수행합니다. Crowdin은 해당 프로젝트의 GitHub 저장소에 자동으로 pull request를 생성합니다.


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

## 코드 예시

이 섹션에서는 이전 섹션에서 파이프라인을 구현하는 단계별 가이드를 살펴볼 것입니다.

- Crowdin 웹사이트에서 계정 및 프로젝트 생성
- Crowdin CLI 다운로드. macOS를 사용하는 경우 Homebrew를 사용하면 쉽게 설치할 수 있습니다:

```js
brew install crowdin@4
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

### 설정 파일

설치 후에는 CLI를 Crowdin 프로젝트와 함께 사용하도록 구성해야 합니다.

프로젝트의 루트 디렉토리에 구성 파일(crowdin.yml)을 만들어야 합니다. 다음 명령을 사용하여 구성 파일을 만들 수 있습니다:

```js
crowdin init
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

구성 파일을 만든 후에는 Crowdin 자격 증명을 설정하십시오. 이 자격 증명은 Crowdin 웹 UI에서 찾을 수 있습니다.

```js
'project_id_env': 'CROWDIN_PROJECT_ID'
'api_token_env': 'CROWDIN_PERSONAL_TOKEN'
'base_path_env': 'CROWDIN_BASE_PATH'
'base_url_env': 'CROWDIN_BASE_URL'
```

preserve_hierarchy 속성은 true로 설정해야 합니다. 이를 통해 소스 파일의 디렉터리 구조가 Crowdin에 유지됩니다.

구성 파일의 files 섹션에서는 Crowdin과 동기화해야 하는 파일을 지정합니다. 소스 파일의 경로와 번역 파일의 해당 위치가 포함됩니다. 소스 언어가 en-US이고 번역 파일이 /public/locales/`locale`/에 위치한다면, files 섹션은 다음과 같이 보여야 합니다:

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

```js
- source: '/public/locales/en-US/*.json'
  translation: '/public/locales/%locale%/%original_file_name%'
  dest: '/%original_file_name%'
```

이렇게 해석할게요:

- source는 번역을 위해 업로드할 파일 패턴을 지정합니다.
- translation은 번역된 파일을 로컬에 어디에 배치할지를 지정합니다. %locale%은 대상 언어 코드를 나타내는 자리 표시자입니다.
- dest는 번역 파일의 파일 이름을 나타냅니다. 

### Crowdin에서 번역 업로드/다운로드하기


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

번역 파일을 Crowdin과 쉽게 관리하기 위해 두 가지 사용자 정의 명령어인 push-i18n과 pull-i18n을 사용합니다.

프로젝트의 package.json 파일에 이러한 명령어를 정의하여 번역 파일을 업로드하고 다운로드하는 프로세스를 자동화할 수 있습니다.

```js
"push-i18n": "crowdin upload sources && crowdin upload translations"
```

push-i18n 명령어는 소스 파일과 기존 번역 파일을 Crowdin에 업로드하는 역할을 합니다. 이 명령어는 보통 프로젝트의 로컬라이제이션 파일의 최신 버전을 Crowdin에 업데이트하고 싶을 때 실행됩니다.

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

```js
"pull-i18n": "crowdin download sources && crowdin download"
```

pull-i18n 명령은 Crowdin에서 파일을 다운로드하여 로컬 프로젝트에 적용하는 프로세스를 처리합니다. 이를 통해 프로젝트에서 가장 최신의 번역을 사용할 수 있게 됩니다.

이러한 명령을 편리하게 사용하려면 package.json 파일의 scripts 섹션에 추가하면 됩니다.

새롭게 업데이트된 문자열을 업로드하려면 push-i18n을 사용하세요. 다른 번역자가 수정한 내용을 적용하려면 pull-i18n을 실행하세요.

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

## 결론

Crowdin은 웹 앱 로컬라이제이션의 고통스러운 문제에 대한 훌륭한 솔루션을 제공합니다. 개발자가 작업을 유지하는 대신, 비기술 팀원들이 직관적인 대시보드를 통해 전체 번역 과정을 수행할 수 있습니다. 다양한 통합 및 언어 지원도 큰 장점입니다!

다른 로컬라이제이션 도구를 추천하시나요? 댓글로 알려주세요!

본 블로그와 그 하부 연구는 zerodays.dev의 멋진 팀이 만들었습니다.