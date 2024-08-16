---
title: "Nodejs에서 파일 경로 다루기 Path 모듈 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-WorkingwithFilePathsinNodejsAGuidetothePathModule_0.png"
date: 2024-07-07 23:12
ogImage: 
  url: /assets/img/2024-07-07-WorkingwithFilePathsinNodejsAGuidetothePathModule_0.png
tag: Tech
originalTitle: "Working with File Paths in Node.js: A Guide to the Path Module"
link: "https://medium.com/@louistrinh/working-with-file-paths-in-node-js-a-guide-to-the-path-module-829f384aac5f"
isUpdated: true
---






![이미지](/assets/img/2024-07-07-WorkingwithFilePathsinNodejsAGuidetothePathModule_0.png)

## NodeJS의 Path 모듈

Path 모듈은 NodeJS에 내장된 모듈로, 파일 경로를 다루는 작업을 간편하게 만들어 줍니다. 파일 시스템 경로를 효과적으로 처리하기 위한 다양한 기능을 제공합니다. 여기에는 몇 가지 기능과 예제가 있습니다:

1. 디렉토리 경로 가져오기:


<div class="content-ad"></div>

```js
const path = require('path');
const dirPath = path.dirname('/home/user/project/file.js');
console.log(dirPath); // '/home/user/project'
```

이 코드는 주어진 파일 경로에서 디렉토리 경로를 검색합니다. 이 경우에는 " /home/user/project/file.js"에서 " /home/user/project"를 추출합니다.

2. 파일 확장자 가져 오기:

JavaScript

<div class="content-ad"></div>

```js
const path = require('path');
const ext = path.extname('/home/user/project/file.js');
console.log(ext); // '.js'
```

위 코드는 예시에서 파일 확장자인 "js"를 추출합니다.

3. 절대 경로 확인:

```js
const path = require('path');
const isAbsolute = path.isAbsolute('/home/user/project/file.js');
console.log(isAbsolute); // true
```

<div class="content-ad"></div>

이 코드는 경로가 절대 경로인지 확인합니다 (루트 디렉토리에서 시작되는 경우, 예: "/home/user/project/file.js"). 이 경우에는 true를 반환합니다.