---
title: "Angular 애플리케이션 실행 방법 5가지"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-09 10:34
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Different ways to run Angular application"
link: "https://medium.com/@balramchavan/different-ways-to-run-angular-application-4f5626308542"
---


일단 응용 프로그램이 컴파일되면 출력 로그에 응용 프로그램을 시작할 URL이 표시됩니다. 'ng serve' 명령을 사용하여 대부분의 사용자가 응용 프로그램을 시작합니다.

<div class="content-ad"></div>

브라우저를 열고 싶을 때는 `--open` 매개변수를 전달하면 됩니다.

```js
ng serve --open
```

## 2. `ng s`

serve라는 단어를 전부 입력하기 귀찮다면, s만 사용하여 ng s 명령어로 애플리케이션을 시작할 수 있어요.

<div class="content-ad"></div>

빌드가 완료되면 브라우저를 바로 열고 싶다면, -o 플래그를 사용하여 브라우저를 실행할 수 있습니다.

```js
ng s -o
```

## 3. npm run start

Node.js, Angular, React 등 다양한 JavaScript 프레임워크를 자주 전환하는 사람이라면 이 명령어를 쉽게 기억할 수 있습니다. 이 명령은 package.json 파일에서 해당 명령을 실행합니다.

<div class="content-ad"></div>

```js
npm run start
```

```js
{
  "scripts": {
    "start": "ng serve --configuration development",
  }
}
```

## 4. npm start

게으른 개발자들을 위한 보너스 팁(나와 같은), npm은 기본적으로 start 단어를 선택하므로 run 단어를 생략하여 애플리케이션을 시작하려면 npm start로 입력하면 됩니다.

<div class="content-ad"></div>

```json
npm start
```