---
title: "HTML,CSS,JS로 틱택토 게임 웹사이트 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 02:43
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Build a Tic Tac Toe Game Website"
link: "https://dev.to/abhishekgurjar/build-a-tic-tac-toe-game-website-4cl5"
isUpdated: true
updatedAt: 1724033126436
---


## 소개

안녕하세요, 여러분! 개발자 여러분, 반가워요! 최신 프로젝트인 클래식 틱택토 게임을 소개해 드리게 되어 기쁩니다. 이 프로젝트는 JavaScript 기술을 연습하기에 최적인 방법이죠, 특히 게임 로직 처리, DOM 조작 및 사용자 상호작용 부분이죠. JavaScript를 막 시작한 분이든 재미있는 도전을 찾고 계신 분이든, 틱택토 게임은 여러분의 기술을 향상시키기에 완벽한 프로젝트입니다.

## 프로젝트 개요

틱택토 게임은 인기 있는 두 명용 게임의 웹 기반 구현입니다. 이 프로젝트는 상호작용 요소를 생성하는 방법, 게임 상태 관리 및 간단한 AI 로직 구현을 보여줍니다. 게임은 완전히 반응형으로 설계되어 데스크톱 및 모바일 장치에서 플레이할 수 있도록 되어 있습니다.

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

- 두 명 플레이 모드: 동일한 장치에서 친구와 함께 플레이하세요.
- 게임 로직: 각 이동 후 승자 또는 무승부를 자동으로 확인합니다.
- 재설정 기능: 언제든지 게임을 쉽게 다시 시작할 수 있습니다.
- 반응형 디자인: 게임 레이아웃이 다양한 화면 크기에 맞춰져 모든 기기에서 일관된 경험을 제공합니다.

## 사용된 기술

- HTML: 게임 인터페이스를 구조화합니다.
- CSS: 게임 보드, 버튼 및 다른 UI 요소를 디자인합니다.
- JavaScript: 플레이어 턴, 승리 조건 및 게임 재설정을 포함한 게임 로직을 관리합니다.

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

프로젝트 구조를 간략히 살펴보겠습니다:

```js
Tic-Tac-Toe/
├── index.html
├── styles.css
└── script.js
```

- index.html: Tic-Tac-Toe 게임의 HTML 구조를 포함합니다.
- styles.css: 게임 보드 및 반응형 디자인을 위한 CSS 스타일을 포함합니다.
- script.js: 플레이어 턴 및 승리 조건을 포함한 게임 로직을 처리합니다.

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
```bash
git clone https://github.com/abhishekgurjar-in/Tic-Tac-Toe.git
```
- 프로젝트 디렉토리 열기:
```bash
cd Tic-Tac-Toe
```
- 프로젝트 실행:
웹 브라우저에서 index.html 파일을 열어 Tic-Tac-Toe 게임을 시작하세요.

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

- 웹 브라우저에서 웹 사이트를 엽니다.
- 그리드의 빈 셀을 클릭하여 게임을 시작합니다.
- 플레이어는 번갈아가며 셀에 자신의 표식(X 또는 O)을 놓습니다.
- 결과 확인: 게임은 승리 조합이 있는 경우 승자를 선언하거나 모든 셀이 채워진 경우 비긴 것으로 선언합니다.
- 게임 리셋: "게임 다시 시작" 버튼을 클릭하여 새 게임을 시작합니다.

## 코드 설명

### HTML

index.html 파일은 Tic-Tac-Toe 게임의 구조를 설정하며, 게임 보드 및 제어 버튼을 포함합니다. 다음은 일부 코드입니다:

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
    <title>Tic-Tac-Toe Game</title>
    <link rel="stylesheet" href="style.css" />
    <script src="script.js" defer></script>
  </head>
  <body>
    <div class="msg-container hide">
      <p id="msg">Winner</p>
      <button id="new-btn">New Game</button>
    </div>
    <main>
      <h1>Tic Tac Toe</h1>
      <div class="container">
        <div class="game">
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
        </div>
      </div>
      <button id="reset-btn">Reset Game</button>
    </main>
    <div class="footer">
      <p>Made with ❤️ by Abhishek Gurjar</p>
    </div>
  </body>
</html>
```

### CSS

styles.css 파일은 Tic-Tac-Toe 게임의 모양을 결정합니다. 그리드 레이아웃, 버튼, 그리고 반응형 디자인이 포함되어 있습니다. 여기에 몇 가지 중요한 스타일이 있습니다:

```js
* {
    margin: 0;
    padding: 0;
  }

  body {
    background-color: #548687;
    text-align: center;
  }

  .container {
    height: 70vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .game {
    height: 60vmin;
    width: 60vmin;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    gap: 1.5vmin;
  }

  .box {
    height: 18vmin;
    width: 18vmin;
    border-radius: 1rem;
    border: none;
    box-shadow: 0 0 1rem rgba(0, 0, 0, 0.3);
    font-size: 8vmin;
    color: #b0413e;
    background-color: #ffffc7;
  }

  #reset-btn {
    padding: 1rem;
    font-size: 1.25rem;
    background-color: #191913;
    color: #fff;
    border-radius: 1rem;
    border: none;
  }

  #new-btn {
    padding: 1rem;
    font-size: 1.25rem;
    background-color: #191913;
    color: #fff;
    border-radius: 1rem;
    border: none;
  }

  #msg {
    color: #ffffc7;
    font-size: 5vmin;
  }

  .msg-container {
    height: 100vmin;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 4rem;
  }

  .hide {
    display: none;
  }

  .footer {
    margin: 50px;
    text-align: center;
    color: white;
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

게임 로직을 관리하는 script.js 파일은 플레이어 턴 처리, 승자 확인 및 게임 재설정을 포함합니다. 여기에 스니펫이 있어요:

```js
let boxes = document.querySelectorAll(".box");
let resetBtn = document.querySelector("#reset-btn");
let newGameBtn = document.querySelector("#new-btn");
let msgContainer = document.querySelector(".msg-container");
let msg = document.querySelector("#msg");

let turnO = true; //playerX, playerO
let count = 0; //무승부 판단용

const winPatterns = [
  [0, 1, 2],
  [0, 3, 6],
  [0, 4, 8],
  [1, 4, 7],
  [2, 5, 8],
  [2, 4, 6],
  [3, 4, 5],
  [6, 7, 8],
];

const resetGame = () => {
  turnO = true;
  count = 0;
  enableBoxes();
  msgContainer.classList.add("hide");
};

boxes.forEach((box) => {
  box.addEventListener("click", () => {
    if (turnO) {
      //playerO
      box.innerText = "O";
      turnO = false;
    } else {
      //playerX
      box.innerText = "X";
      turnO = true;
    }
    box.disabled = true;
    count++;

    let isWinner = checkWinner();

    if (count === 9 && !isWinner) {
      gameDraw();
    }
  });
});

const gameDraw = () => {
  msg.innerText = `게임은 무승부입니다.`;
  msgContainer.classList.remove("hide");
  disableBoxes();
};

const disableBoxes = () => {
  for (let box of boxes) {
    box.disabled = true;
  }
};

const enableBoxes = () => {
  for (let box of boxes) {
    box.disabled = false;
    box.innerText = "";
  }
};

const showWinner = (winner) => {
  msg.innerText = `축하합니다, 승자는 ${winner} 입니다`;
  msgContainer.classList.remove("hide");
  disableBoxes();
};

const checkWinner = () => {
  for (let pattern of winPatterns) {
    let pos1Val = boxes[pattern[0]].innerText;
    let pos2Val = boxes[pattern[1]].innerText;
    let pos3Val = boxes[pattern[2]].innerText;

    if (pos1Val != "" && pos2Val != "" && pos3Val != "") {
      if (pos1Val === pos2Val && pos2Val === pos3Val) {
        showWinner(pos1Val);
        return true;
      }
    }
  }
};

newGameBtn.addEventListener("click", resetGame);
resetBtn.addEventListener("click", resetGame);
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

위의 텍스트를 친근한 톤으로 한국어로 번역해보겠습니다.

여기서 Tic-Tac-Toe 게임의 라이브 데모를 확인할 수 있어요.

## 결론

이 Tic-Tac-Toe 게임을 만드는 것은 JavaScript를 연습할 수 있는 즐거운 경험이었어요, 특히 상호작용 웹 애플리케이션을 만들 때입니다. 이 프로젝트가 여러분에게 자신만의 게임을 만들고 JavaScript의 가능성을 탐험하도록 영감을 줬으면 좋겠어요. 즐거운 코딩되세요!

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

이 프로젝트는 JavaScript 및 DOM 조작에 중점을 둔 웹 개발 기술을 향상시키기 위한 나의 지속적인 여정의 일환으로 개발되었습니다.

## 저자

- Abhishek Gurjar
GitHub 프로필