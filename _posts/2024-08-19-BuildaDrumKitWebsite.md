---
title: "자바스크립트로 만드는 멋진 드럼 킷 웹사이트 만들기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 02:41
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Build a Drum Kit Website"
link: "https://dev.to/abhishekgurjar/build-a-drum-kit-website-8hh"
isUpdated: false
---


## 소개

안녕하세요, 개발자 여러분! 제가 최근에 개발한 '드럼 킷' 프로젝트를 공유할 기회가 있어 정말 기쁩니다. 이 프로젝트는 JavaScript를 연습하기에 재미있고 상호작용적인 방법을 제공합니다. 특히 사용자 입력 및 오디오 재생에 대해 다룹니다. JavaScript에 입문하려는 초보자이거나 음악과 코딩을 좋아하는 사람이라면 이 프로젝트가 딱입니다.

## 프로젝트 개요

'드럼 킷'은 드럼 세트를 시뮬레이션하는 상호작용적인 웹 애플리케이션입니다. 사용자는 드럼 버튼을 클릭하거나 키보드에서 해당 키를 눌러 소리를 연주할 수 있습니다. 이 프로젝트는 이벤트, 오디오 및 CSS 애니메이션을 어떻게 사용하여 반응성이 있고 매력적인 사용자 경험을 만드는지 보여줍니다.

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

## 기능

- 상호 작용 드럼 버튼: 다른 드럼 소리를 재생하는 클릭 가능한 버튼입니다.
- 키보드 제어: 특정 키를 눌러 드럼 소리를 발생시킵니다.
- 시각적 피드백: 버튼을 누를 때 애니메이션이 표시되어 즉각적인 시각적 피드백을 제공합니다.
- 반응형 디자인: 레이아웃이 다른 화면 크기에 맞춰 조정되어 장치 간 일관된 경험을 보장합니다.

## 사용된 기술

- HTML: 드럼 킷 인터페이스의 구조를 제공합니다.
- CSS: 버튼 애니메이션 및 전반적인 레이아웃을 포함한 드럼 킷 디자인을 스타일링합니다.
- JavaScript: 사용자 상호 작용, 소리 재생 및 애니메이션을 처리합니다.

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

## 프로젝트 구조

프로젝트 구조를 간단히 살펴보겠습니다:

```js
Drum-Kit/
├── index.html
├── styles.css
└── index.js
```

- index.html: 드럼 키트의 HTML 구조를 포함합니다.
- styles.css: 드럼 키트와 애니메이션을 위한 CSS 스타일이 포함됩니다.
- index.js: 사용자 상호작용, 사운드 효과 및 애니메이션을 관리합니다.

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

## 설치

프로젝트를 시작하려면 다음 단계를 따라주세요:

- 저장소를 복제합니다:
```bash
git clone https://github.com/abhishekgurjar-in/Drum-Kit.git
```
- 프로젝트 디렉토리를 엽니다:
```bash
cd Drum-Kit
```
- 프로젝트를 실행합니다: 
웹 브라우저에서 index.html 파일을 열어 드럼 킷을 확인하세요.
- 인덱스.html 파일을 웹 브라우저에서 열어 드럼 킷을 확인하세요.

## 사용법

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

- 웹 브라우저에서 웹 사이트를 열어주세요.
- 드럼 버튼을 클릭하거나 해당하는 키(w, a, s, d, j, k, l)를 눌러 다른 드럼 소리를 연주해보세요.
- 눌렀을 때 버튼 애니메이션을 확인하여 시각적 피드백을 받아보세요.

## 코드 설명

### HTML

index.html 파일은 드럼 킷의 구조를 설정하고, 각 드럼 소리를 연주하는 버튼 및 바닥글을 포함하고 있습니다. 아래는 일부 코드 예시입니다:

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
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8" />
    <title>Drum Kit</title>
    <link rel="stylesheet" href="styles.css" />
    <link href="https://fonts.googleapis.com/css?family=Arvo" rel="stylesheet" />
  </head>
  <body>
    <h1 id="title">Drum 🥁 Kit</h1>
    <div class="set">
      <button class="w drum">w</button>
      <button class="a drum">a</button>
      <button class="s drum">s</button>
      <button class="d drum">d</button>
      <button class="j drum">j</button>
      <button class="k drum">k</button>
      <button class="l drum">l</button>
    </div>
    <script src="index.js" charset="utf-8"></script>
    <footer>Made with ❤️ by Abhishek Gurjar</footer>
  </body>
</html>
```

### CSS

The styles.css file styles the drum kit, including the drum buttons and animations. Here are some key styles:

```js
body {
  text-align: center;
  background-color: #283149;
}

h1 {
  font-size: 5rem;
  color: #DBEDF3;
  font-family: "Arvo", cursive;
  text-shadow: 3px 0 #DA0463;
}

footer {
  color: #DBEDF3;
  font-family: sans-serif;
}

.w { background-image: url("images/tom1.png"); }
.a { background-image: url("images/tom2.png"); }
.s { background-image: url("images/tom3.png"); }
.d { background-image: url("images/tom4.png"); }
.j { background-image: url("images/snare.png"); }
.k { background-image: url("images/crash.png"); }
.l { background-image: url("images/kick.png"); }

.set {
  margin: 10% auto;
}

.pressed {
  box-shadow: 0 3px 4px 0 #DBEDF3;
  opacity: 0.5;
}

.drum {
  outline: none;
  border: 10px solid #404B69;
  font-size: 5rem;
  font-family: 'Arvo', cursive;
  line-height: 2;
  font-weight: 900;
  color: #DA0463;
  text-shadow: 3px 0 #DBEDF3;
  border-radius: 15px;
  display: inline-block;
  width: 150px;
  height: 150px;
  text-align: center;
  margin: 10px;
  background-color: white;
}
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

### JavaScript

index.js 파일은 드럼 키트의 기능을 제어하며, 소리 재생과 버튼 애니메이션을 포함합니다. 아래는 장표입니다:

```js
const numberOfDrumButtons = document.querySelectorAll(".drum").length;

for (let i = 0; i < numberOfDrumButtons; i++) {
  document.querySelectorAll(".drum")[i].addEventListener("click", function () {
    const buttonInnerHTML = this.innerHTML;

    makeSound(buttonInnerHTML);
    buttonAnimation(buttonInnerHTML);
  });
}

document.addEventListener("keypress", function (event) {
  makeSound(event.key);
  buttonAnimation(event.key);
});

function makeSound(key) {
  switch (key) {
    case "w":
      const tom1 = new Audio("sounds/tom-1.mp3");
      tom1.play();
      break;
    case "a":
      const tom2 = new Audio("sounds/tom-2.mp3");
      tom2.play();
      break;
    case "s":
      const tom3 = new Audio("sounds/tom-3.mp3");
      tom3.play();
      break;
    case "d":
      const tom4 = new Audio("sounds/tom-4.mp3");
      tom4.play();
      break;
    case "j":
      const snare = new Audio("sounds/snare.mp3");
      snare.play();
      break;
    case "k":
      const crash = new Audio("sounds/crash.mp3");
      crash.play();
      break;
    case "l":
      const kick = new Audio("sounds/kick-bass.mp3");
      kick.play();
      break;
    default:
      console.log(key);
  }
}

function buttonAnimation(currentKey) {
  const activeButton = document.querySelector("." + currentKey);
  activeButton.classList.add("pressed");

  setTimeout(function () {
    activeButton.classList.remove("pressed");
  }, 100);
}
```

## 데모 보기

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

여기서 Drum Kit의 라이브 데모를 확인할 수 있어요.

## 결론

Drum Kit을 만들면서 JavaScript의 이벤트 처리와 오디오 기능에 몰두해 볼 수 있어서 정말 멋진 경험이었어요. 이 프로젝트가 여러분에게 상호작용하는 웹 애플리케이션을 시도하고 자신만의 재미있고 매력적인 프로젝트를 만들도록 영감을 줄 수 있기를 바라요. 코드를 탐험하고 원하면 사용자 정의하여 자신의 작업에 활용해보세요. 즐거운 코딩되세요!

## 크레딧

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

이 프로젝트는 JavaScript의 상호 작용 웹 요소를 만드는 데 잠재력을 보여주기 위해 만들어졌어요.

## 저자

- Abhishek Gurjar
GitHub 프로필
- GitHub 프로필