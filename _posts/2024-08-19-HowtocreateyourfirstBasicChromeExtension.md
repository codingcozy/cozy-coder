---
title: "처음 만드는 기본 Chrome 확장 프로그램 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-HowtocreateyourfirstBasicChromeExtension_0.png"
date: 2024-08-19 01:48
ogImage: 
  url: /assets/img/2024-08-19-HowtocreateyourfirstBasicChromeExtension_0.png
tag: Tech
originalTitle: "How to create your first Basic Chrome Extension"
link: "https://medium.com/@rehmat-sayany/how-to-create-your-first-baschrome-extension-e05f2d126ab5"
isUpdated: false
---


<img src="/assets/img/2024-08-19-HowtocreateyourfirstBasicChromeExtension_0.png" />

크롬 확장 프로그램을 만드는 것은 브라우저의 기능을 향상시키거나 다른 사람들과 아이디어를 공유하는 데 재미와 보상이 될 수 있습니다. 생산성 도구를 만들고 웹 경험을 향상시키거나 작업을 자동화하려는 경우, 크롬 확장 프로그램을 통해 목표를 달성할 수 있습니다. 이 안내서에서는 처음부터 간단한 크롬 확장 프로그램을 만드는 과정을 안내하고 차후 프로젝트에 영감을 줄 아이디어를 제공할 것입니다.

## 크롬 확장 프로그램이란?

크롬 확장 프로그램은 브라우징 경험을 사용자 정의하는 작은 소프트웨어 프로그램입니다. 크롬 브라우저의 동작이나 인터페이스를 수정하여 기능을 개선하거나 새로운 기능을 제공하거나 다른 웹 서비스와 통합할 수 있습니다.

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

# 단계 1: 개발 환경 설정하기

시작하기 전에 몇 가지 도구가 필요합니다:

- 텍스트 편집기: 원하는 텍스트 편집기를 사용할 수 있지만, 인기 있는 선택지로는 Visual Studio Code, Sublime Text, 또는 Atom이 있습니다.
- Chrome 브라우저: Chrome의 최신 버전이 설치되어 있는지 확인해주세요.
- HTML, CSS, JavaScript의 기본 지식: Chrome 확장 프로그램은 주로 이러한 웹 기술을 사용하여 구축됩니다.

# 단계 2: 매니페스트 파일 생성하기

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

모든 Chrome 확장 프로그램은 manifest.json 파일이 필요합니다. 이 파일은 Chrome에 확장 프로그램에 대한 정보를 알려줍니다. 예를 들어, 확장 프로그램의 이름, 버전 및 필요한 권한 등이 포함됩니다.

- 컴퓨터에 확장 프로그램 파일을 저장할 새 폴더를 만들어주세요. 이름을 MyFirstExtension과 같은 것으로 지정해보세요.
- 폴더 안에 manifest.json이라는 파일을 생성해주세요.
- 다음 코드를 manifest.json 파일에 추가해주세요:

```js
{
  "manifest_version": 3,
  "name": "My First Chrome Extension",
  "version": "1.0",
  "author": "Rehmat Ali Sayany",
  "description": "간단한 Chrome 확장 프로그램 예제입니다.",
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "images/icon16.png",
      "48": "images/icon48.png",
      "128": "images/icon128.png"
    }
  },
  "permissions": ["activeTab"]
}
```

# 단계 3: 사용자 인터페이스 생성하기

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

이번 단계에서 확장 프로그램에 대한 기본 사용자 인터페이스를 만들 것입니다. 이에는 확장 프로그램 아이콘을 클릭할 때 나타나는 팝업이 포함됩니다.

- manifest.json 파일과 동일한 폴더에 popup.html이라는 HTML 파일을 생성합니다.
- 다음 코드를 popup.html에 추가합니다:

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>나의 확장 프로그램</title>
  <style>
    body {
      width: 200px;
      font-family: Arial, sans-serif;
      padding: 10px;
    }
    button {
      width: 100%;
      padding: 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>환영합니다!</h1>
  <button id="clickMe">클릭</button>
  <script src="popup.js"></script>
</body>
</html>
```

3. 확장 프로그램을 위한 아이콘을 추가하세요. 확장 프로그램 폴더 내부에 images라는 폴더를 만들고 세 가지 아이콘 (icon16.png, icon48.png, icon128.png)을 추가하세요. 이 아이콘들은 이미지 편집기를 사용하여 만들 수 있습니다.

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

# 단계 4: 자바스크립트를 사용하여 기능 추가하기

이제 확장 프로그램에 JavaScript를 사용하여 상호 작용성을 추가해 보겠습니다.

- 같은 폴더에 popup.js라는 JavaScript 파일을 만듭니다.
- 다음 코드를 popup.js에 추가합니다:

```js
document.getElementById('clickMe').addEventListener('click', function() {
  alert('크롬 확장 프로그램에서 안녕하세요!');
});
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

3. 이 스크립트는 버튼을 클릭할 때 경고를 표시합니다.

# 단계 5: 크롬에서 확장 프로그램 로드

이제 확장 프로그램을 위한 파일을 만들었으니 크롬에 로드할 차례입니다.

- Chrome을 열고 chrome://extensions/로 이동합니다.
- 오른쪽 상단의 스위치를 토글하여 개발자 모드를 활성화합니다.
- "압축 해제된 확장 프로그램을 로드"를 클릭하고 확장 프로그램 폴더를 선택합니다.
- 이제 설치된 확장 프로그램 목록에 확장 프로그램이 표시되며 Chrome 툴바에 해당 아이콘이 표시됩니다.

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

# 단계 6: 확장 프로그램 테스트하기

Chrome 툴바에 있는 확장 프로그램 아이콘을 클릭하세요. 만든 팝업이 나타나고, 버튼을 클릭하면 경고가 표시됩니다.

# 단계 7: 확장 프로그램 개선 및 확장하기

이것은 시작에 불과합니다! Chrome 확장 프로그램은 팝업을 표시하는 것 이상의 작업을 수행할 수 있습니다. 가능한 일례:


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

- 웹 페이지 상호 작용: 콘텐츠 스크립트를 사용하여 웹 페이지를 수정합니다.
- 데이터 저장: Chrome의 저장소 API를 사용하여 사용자 환경 설정을 저장합니다.
- 서버와 통신: 백그라운드 스크립트를 사용하여 장기 실행 작업이나 네트워크 요청을 처리합니다.
- 더 많은 UI 구성 요소 추가: 옵션 페이지, 컨텍스트 메뉴 등을 만듭니다.

# 단계 8: 확장 프로그램 배포

확장 프로그램을 완성했다면 Chrome 웹 스토어에 게시할 수 있습니다:

- Chrome 웹 스토어에 개발자 계정을 만듭니다.
- 확장 프로그램 폴더를 압축하여 패키징합니다.
- 패키지를 Chrome 웹 스토어에 업로드하고 게시하기 위해 안내에 따릅니다.

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

# Chrome Extension 아이디어

참고용으로 몇 가지 Chrome 확장 프로그램 아이디어를 소개해 드릴게요:

- 웹에서 API로 이동: 미리 정의된 패턴을 기반으로 웹페이지에서 해당 API URL로 이동하는 버튼을 자동으로 생성하고 표시해 주는 확장 프로그램입니다. 개발자나 API 테스터들이 프론트엔드와 백엔드 환경을 빠르게 전환해야 할 때 유용합니다.
- 생산성을 위한 탭 관리자: 컨텍스트나 사용자 정의 카테고리에 따라 탭을 그룹화하여 정리해 주는 확장 프로그램으로, 집중력을 높여 주고 탭 혼란을 줄여 줍니다.
- 페이지 주석 및 메모 작성 도구: 웹페이지 상에 직접 텍스트를 강조할 수 있고 메모를 추가하고 주석을 저장할 수 있는 도구로, 연구자나 학생들에게 적합합니다.
- 자동 다크 모드: 네이티브 다크 모드가 없는 웹사이트도 사용자가 지정한 테마로 다크 모드로 표시되도록 만들어 주는 확장 프로그램으로, 사용자 정의 테마와 예약된 활성화가 가능합니다.
- 특정 텍스트 번역기: 웹페이지에서 특정 텍스트 선택 부분만 번역하고 나머지 콘텐츠에는 영향을 미치지 않는 기능으로, 언어 학습자나 다국어 콘텐츠를 만나는 모든 사용자에게 유용합니다.
- URL 및 주석이 포함된 스크린샷: URL 및 타임스탬프가 포함된 스크린샷을 찍고 확장 프로그램 내에서 직접 주석을 추가할 수 있어 문서 작업이 더 편리하고 정확해집니다.
- 집중을 돕는 콘텐츠 차단기: 작업이나 공부 중에 집중을 돕기 위해 특정 유형의 콘텐츠(예: 소셜 미디어 피드, 댓글 섹션)을 차단해주는 확장 프로그램입니다.

# 결론

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

크롬 익스텐션을 만드는 것은 웹 개발에 대해 배우고 브라우징 경험을 개선하는 좋은 방법입니다. 이 단계를 따라하면 원하는 대로 확장하고 사용자 정의할 수 있는 기본적인 크롬 익스텐션을 만들 수 있습니다. 제공된 추가 아이디어를 통해 더 발전된 또는 특정한 익스텐션을 고려하여 자신이나 다른 사람의 필요에 맞는 것을 고민하고 개발할 수 있습니다. 가능성은 무한하니 실험하고 더 발전된 기능을 탐험할 주저하지 마세요. 즐거운 코딩하세요!