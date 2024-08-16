---
title: "Electron JS 크로스 플랫폼 데스크탑 앱 제작 종합 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-ElectronJSAComprehensiveGuidetoBuildingCross-PlatformDesktopApps_0.png"
date: 2024-07-07 23:03
ogImage: 
  url: /assets/img/2024-07-07-ElectronJSAComprehensiveGuidetoBuildingCross-PlatformDesktopApps_0.png
tag: Tech
originalTitle: "Electron JS: A Comprehensive Guide to Building Cross-Platform Desktop Apps"
link: "https://medium.com/@chandantechie/electron-js-a-comprehensive-guide-to-building-cross-platform-desktop-apps-923b5f3d5030"
isUpdated: true
---





```html
<img src="/assets/img/2024-07-07-ElectronJSAComprehensiveGuidetoBuildingCross-PlatformDesktopApps_0.png" />

# 소개

Electron JS는 HTML, CSS 및 JavaScript 등의 웹 기술을 사용하여 크로스 플랫폼 데스크톱 애플리케이션을 구축하기 위한 오픈 소스 프레임워크입니다. 개발자들은 웹 개발에 사용하는 기술과 동일한 기술을 사용하여 데스크톱 앱을 만들 수 있습니다. 이로 인해 Electron JS는 데스크톱 애플리케이션을 구축하는 인기 있는 선택지 중 하나가 되었습니다. 이 안내서에서는 Electron JS의 기본 개념, 기능 및 Electron JS 프로젝트를 생성하고 작업하는 방법을 안내합니다.

# 기본 개념
```

<div class="content-ad"></div>

Electron JS를 시작하기 전에 기본 개념을 이해하는 것이 중요합니다:

- 웹 기술: HTML, CSS 및 JavaScript가 앱의 사용자 인터페이스와 기능을 구축하는데 사용됩니다.
- Node JS: Electron은 데스크톱에서 JavaScript를 실행하기 위해 Node JS를 사용하며, 파일 시스템, 네트워크 및 기타 시스템 자원에 액세스할 수 있습니다.
- Chromium: Electron은 앱의 사용자 인터페이스를 렌더링하기 위해 Chromium 엔진을 사용하여 익숙한 웹과 유사한 경험을 제공합니다.

# Electron JS 설정하기

Electron JS를 시작하려면:

<div class="content-ad"></div>

- Node JS 설치: 공식 웹사이트에서 Node JS를 다운로드하고 설치하세요.
- Electron CLI 설치: `npm install electron-cli -g` 명령어를 실행하여 Electron CLI를 설치하세요.
- 새 프로젝트 생성: `electron-cli new my-app` 명령어를 실행하여 새 Electron JS 프로젝트를 만드세요.

# 프로젝트 구조

Electron JS 프로젝트는 다음과 같이 구성됩니다:

- main.js: 앱 창을 생성하고 시스템 이벤트를 처리하는 주 프로세스 파일입니다.
- renderer.js: 앱의 사용자 인터페이스를 렌더링하는 렌더러 프로세스 파일입니다.
- index.html: 앱의 사용자 인터페이스를 포함하는 주 HTML 파일입니다.
- package.json: 종속성 및 스크립트를 포함하는 프로젝트 구성 파일입니다.

<div class="content-ad"></div>

# 앱 만들기

앱을 만들기 위해 다음을 수행해야 합니다:

- 코드 작성: JavaScript로 메인 및 렌더러 프로세스 코드를 작성합니다.
- 사용자 인터페이스 생성: HTML 및 CSS를 사용하여 사용자 인터페이스를 만듭니다.
- 앱 실행: 앱을 시작하려면 `electron .` 명령을 실행합니다.

# Electron.js로 데모를 설정하려면 다음 단계를 따라주세요:

<div class="content-ad"></div>

Node.js와 npm 설치하기

- 먼저, 컴퓨터에 Node.js와 npm이 설치되어 있는지 확인해주세요. Node.js 웹사이트에서 다운로드하여 설치할 수 있습니다.

프로젝트 초기화하기

```js
mkdir electron-demo
cd electron-demo
```

<div class="content-ad"></div>

새로운 npm 프로젝트를 초기화하세요:

```js
npm init -y
```

<div class="content-ad"></div>

3. 일렉트론 설치

일렉트론을 개발 의존성으로 설치하세요:

```js

<div class="content-ad"></div>

네예~ 아래와 같이 명령어를 실행하시면 됩니다.

npm install electron --save-dev

<div class="content-ad"></div>

다음 내용으로 main.js 파일을 만들어주세요:

const { app, BrowserWindow } = require('electron');

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  });

  win.loadFile('index.html');
}

app.whenReady().then(createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow();
  }
});

index.html

다음 내용으로 index.html 파일을 만들어주세요:

<div class="content-ad"></div>

<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Hello Electron</title>
</head>
<body>
  <h1>Hello, Electron!</h1>
  <p>This is a simple Electron application.</p>
</body>
</html>

패키지.json 업데이트하기

패키지.json 파일을 열고 Electron 애플리케이션을 실행할 start 스크립트를 추가하세요:

"scripts": {
  "start": "electron ."
}

<div class="content-ad"></div>

6. 일렉트론 앱 실행

다음 명령어를 사용하여 일렉트론 애플리케이션을 시작합니다:

npm start

"Hello, Electron!" 메시지가 표시된 새 창이 열리는 것을 확인할 수 있어요.

<div class="content-ad"></div>

프로젝트를 위한 새 디렉토리를 만들고 해당 디렉토리로 이동하세요:

# 일렉트론 JS의 특징

일렉트론 JS는 다음과 같은 여러 가지 기능을 제공합니다:

- 크로스 플랫폼: Windows, macOS 및 Linux에서 실행되는 앱을 만듭니다.
- 웹 기술 사용: 데스크톱 앱을 만들 때 웹 기술을 활용합니다.
- Node JS 통합: 시스템 리소스 및 Node JS 모듈에 액세스합니다.
- 크로미움 엔진: 렌더링 및 성능을 위해 크로미움 엔진을 사용합니다.

<div class="content-ad"></div>

# 고급 주제

일렉트론 JS에 대한 기본적인 이해가 있으면 다음과 같은 고급 주제를 살펴볼 수 있습니다:

- 일렉트론 API: 일렉트론 API를 사용하여 시스템 자원과 기능에 액세스할 수 있습니다.
- Node JS 모듈: Node JS 모듈을 사용하여 앱의 기능을 확장할 수 있습니다.
- 성능 최적화: 다양한 기술을 사용하여 앱의 성능을 최적화할 수 있습니다.

# 결론

<div class="content-ad"></div>

일렉트론 JS는 웹 기술을 사용하여 크로스 플랫폼 데스크톱 애플리케이션을 구축하기 위한 강력한 프레임워크입니다. 유연하고 확장 가능한 아키텍처를 갖춘 일렉트론 JS는 데스크톱 앱을 구축하기 위한 견고한 플랫폼을 제공합니다. 이 안내서를 따라가면 기초부터 고급까지 일렉트론 JS 프로젝트를 만들고 작업할 수 있습니다. 즐거운 코딩 되세요!

감사합니다.

찬단