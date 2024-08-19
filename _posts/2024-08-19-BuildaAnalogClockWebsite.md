---
title: " HTML, CSS, JavaScript로 아날로그 시계 웹사이트 만들기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 02:42
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Build a Analog Clock Website"
link: "https://dev.to/abhishekgurjar/build-a-analog-clock-website-5eia"
isUpdated: true
updatedAt: 1724033100191
---


## 소개

안녕하세요, 개발자 여러분! 오늘은 최근에 완료한 프로젝트인 아날로그 시계를 공유할 기회가 너무 반가워요. 이 프로젝트는 전통적인 아날로그 시계 모습을 활용하여 시간을 표시하는 시각적으로 매력적이고 상호작용적인 방법입니다. 특히 애니메이션, DOM 조작 및 시간 기반 기능과 같은 JavaScript, CSS 및 HTML 기술을 연마하기에 탁월한 프로젝트입니다. 초보자이거나 클래식 시계 인터페이스를 만들고 싶어하는 경험 많은 개발자라면 이 프로젝트를 선택하는 것이 좋습니다.

## 프로젝트 개요

아날로그 시계는 전통적인 아날로그 시계의 외관과 기능을 모방하는 실시간 시계입니다. 시계는 매 초마다 동적으로 업데이트되며 시, 분, 초 단위의 바늘이 부드럽게 회전하여 현재 시간을 반영합니다. 이 프로젝트는 동적이고 시각적으로 매력적인 웹 애플리케이션을 구축하는 연습을 원하는 개발자들에게 이상적입니다.

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

- 실시간 시계: 시계는 매 초마다 업데이트되며, 움직이는 시, 분, 초 바늘로 현재 시간을 표시합니다.
- 부드러운 애니메이션: 시계 바늘이 부드럽게 회전하여 현실적인 아날로그 시계 효과를 만듭니다.
- 반응형 디자인: 시계는 반응형으로 디자인되어 다양한 기기와 화면 크기에서 멋지게 보이도록 제작되었습니다.
- 미니멀한 디자인: 기능성과 우아함에 중점을 두고 깔끔하고 간결한 디자인을 갖추고 있습니다.

## 사용된 기술

- HTML: 웹페이지와 시계 레이아웃을 구성하는 데 사용되었습니다.
- CSS: 시계를 스타일링하기 위해 적용되었으며, 바늘의 위치 조정 및 부드러운 애니메이션 추가에 사용되었습니다.
- JavaScript: 시계의 시간 계산을 처리하고 DOM을 업데이트하며 바늘의 회전을 관리하기 위해 구현되었습니다.

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
Analog-Clock/
├── index.html
├── style.css
└── script.js
```

- index.html: 웹페이지의 HTML 구조를 포함합니다.
- style.css: 애니메이션 및 반응형 디자인을 포함한 CSS 스타일을 관리합니다.
- script.js: 자바스크립트를 사용하여 시계의 동적 측면을 관리합니다.

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

프로젝트를 시작하려면 다음 단계를 따르세요:

- 저장소 복제:
git clone https://github.com/abhishekgurjar-in/Analog-Clock.git
- 프로젝트 디렉토리 열기:
cd Analog-Clock
- 프로젝트 실행:
로컬 서버에서 실행하거나 웹 브라우저에서 index.html 파일을 열 수 있습니다.
- 로컬 서버에서 실행하거나 웹 브라우저에서 index.html 파일을 열 수 있습니다.

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
- 시계를 보며 시, 분, 초 침의 부드러운 애니메이션으로 현재 시간을 표시합니다.

## 코드 설명

### HTML

index.html 파일은 웹페이지의 구조를 설정하며, 시계 컨테이너와 헤더를 포함합니다. 아래는 HTML 코드 일부입니다:

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
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Analog Clock</title>
    <link rel="stylesheet" href="style.css" />
    <script src="script.js" defer></script>
  </head>
  <body>
    <div class="header">
      <h1>Analog Clock</h1>
    </div>
    <div id="clockContainer">
      <div id="hour"></div>
      <div id="minute"></div>
      <div id="second"></div>
    </div>
    <div class="footer">
      <p>Made with ❤️ by Abhishek Gurjar</p>
    </div>
  </body>
</html>
```

### CSS

style.css 파일은 시계의 컨테이너와 시침을 스타일링하여 시간을 정확히 표시할 수 있도록 보장합니다. 주요 스타일은 다음과 같습니다:

```js
#clockContainer {
  position: relative;
  margin: auto;
  height: 30vw;
  width: 30vw;
  background: url(clock.png) no-repeat;
  background-size: 100%;
}

#hour,
#minute,
#second {
  position: absolute;
  background: black;
  border-radius: 10px;
  transform-origin: bottom;
}

#hour {
  width: 1.8%;
  height: 25%;
  top: 25%;
  left: 48.85%;
  opacity: 0.8;
}

#minute {
  width: 1.6%;
  height: 30%;
  top: 19%;
  left: 48.9%;
  opacity: 0.8;
}

#second {
  width: 1%;
  height: 40%;
  top: 9%;
  left: 49.25%;
  opacity: 0.8;
}

.header {
  margin: 80px;
  text-align: center;
}

.footer {
  margin: 50px;
  text-align: center;
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

스크립트.js 파일은 현재 시간을 계산하고 시계의 시침을 업데이트하는 역할을 합니다. 아래는 JavaScript 코드 일부입니다:

```js
setInterval(() => {
  const date = new Date();
  const hourTime = date.getHours();
  const minuteTime = date.getMinutes();
  const secondTime = date.getSeconds();

  const hourRotation = 30 * hourTime + minuteTime / 2;
  const minuteRotation = 6 * minuteTime;
  const secondRotation = 6 * secondTime;

  const hour = document.getElementById('hour');
  const minute = document.getElementById('minute');
  const second = document.getElementById('second');

  hour.style.transform = `rotate(${hourRotation}deg)`;
  minute.style.transform = `rotate(${minuteRotation}deg)`;
  second.style.transform = `rotate(${secondRotation}deg)`;
}, 1000);
```

## 라이브 데모

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

이곳에서 아날로그 시계의 라이브 데모를 확인할 수 있어요.

## 결론

이 아날로그 시계를 구축하는 것은 JavaScript 애니메이션과 DOM 조작에 대해 더 심층적으로 파고들 수 있는 보람찬 경험이었습니다. 이 프로젝트가 여러분에게 자신만의 상호작용 및 시각적으로 매력적인 애플리케이션을 만들도록 영감을 줄 수 있기를 바라요. 코드를 탐험하고 원하는 대로 사용자 정의하며 자신의 프로젝트에 활용하는 것을 자유롭게 해보세요. 즐거운 코딩 되세요!

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

이 프로젝트는 아날로그 시계의 클래식 디자인에서 영감을 받았으며 간단한 실시간 시간 표시 도구가 필요한 경우를 고려하여 개발되었습니다.

## 저자

- Abhishek Gurjar
GitHub 프로필
- GitHub 프로필