---
title: "Bash 스크립트에서 ctrlc 처리하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-Howtohandlectrlcinbashscripts_0.png"
date: 2024-07-13 22:02
ogImage: 
  url: /assets/img/2024-07-13-Howtohandlectrlcinbashscripts_0.png
tag: Tech
originalTitle: "How to handle ctrl+c in bash scripts"
link: "https://medium.com/@kpatronas/how-to-handle-ctrl-c-in-bash-scripts-d7085e7d3d47"
---



![How to Handle Ctrl+C in Bash Scripts](/assets/img/2024-07-13-Howtohandlectrlcinbashscripts_0.png)

가정해보세요. Ctrl+C가 눌릴 때까지 영원히 실행되는 스크립트를 만들고 싶다면요.. 이미 알고 계실지도 모릅니다만, 노력 없이도 이것을 할 수 있습니다. 언제든지 Ctrl+C를 누르면 스크립트가 종료됩니다. 그러나 이 스크립트가 하는 일은 스크립트를 우아하게 종료하지 않는 것입니다. 이는 스크립트가 필요한 모든 정리 작업을 수행하지 않고 그냥 종료된다는 것을 의미합니다. 이 기사에서는 Ctrl+C와 같은 신호를 처리하고 Ctrl+C를 누른 후 몇 가지 정리 기능을 수행하는 방법을 보여드리겠습니다.

## 영원히 반복하기!

이 간단한 예제에서는 Ctrl+C가 눌릴 때까지 루프에 진입하는 간단한 bash 스크립트가 있습니다. 이 루프에서는 test.tmp가 1초마다 생성되고 삭제됩니다. Ctrl+C가 눌리면 test.tmp가 삭제되지 않을 수 있는 여러 경우가 있습니다. Ctrl+C를 누른 후에도 test.tmp가 삭제되도록 어떻게 할 수 있을까요?


<div class="content-ad"></div>

```bash
#!/bin/bash

while true; do
  touch test.tmp
  sleep 1
  echo "Running..."
  rm -f test.tmp
done
```

## It's a trap!

Bash has the trap command, which allows you to catch signals (ctrl+c is the SIGINT signal) and execute a command upon capturing. The trap syntax is as follows:

```bash
trap function_command_to_execute SIGNAL
```

<div class="content-ad"></div>

매우 간단하고 멋지죠! 그렇죠? 이제 함께 원래 스크립트에 `trap`을 적용해 보겠습니다!

```js
#!/bin/bash

cleanup(){
  echo ""
  echo "CTRL+C를 눌러 종료하기 전 정리 작업 중..."
  rm -rf touch.tmp 2>/dev/null
  exit 1
}

# SIGINT 신호(Ctrl+C)를 받으면 trap 실행
trap cleanup SIGINT

while true; do
  touch test.tmp
  sleep 1
  echo "실행 중..."
  rm -f test.tmp
done
```

스크립트를 실행하고 ctrl+c를 누르면 다음과 같은 결과가 생성됩니다:

```js
./hello.sh
실행 중...
실행 중...
실행 중...
^C
CTRL+C를 눌러 종료하기 전 정리 작업 중...
```

<div class="content-ad"></div>

'ctrl+c'를 눌러 생성된 신호가 잡히고 cleanup 함수가 실행됐어요! 멋져요! 지금 몇 가지 질문이 있을 수 있어요

왜 echo를 두 번 사용했나요?

```js
echo ""
echo "CTRL+C pressed, clean up things before exiting..."
```

그 이유는 ctrl+c를 눌러 줄바꿈 없이 누르면 출력이 꼬일 수 있기 때문이에요

<div class="content-ad"></div>

```js
❯ ./hello.sh
실행 중...
실행 중...
실행 중...
실행 중...
^CCTRL+C가 눌렸습니다. 종료 전 정리 중 ...

왜 stderr를 /dev/null로 리다이렉션했나요?

rm -rf touch.tmp 2>/dev/null

/dev/null은 검은 구멍 역할을 하는 특수 장치 이름입니다! 거기에 던지는 모든 것이 사라집니다! 따라서 이미 삭제된 파일을 삭제하려고 시도할 때 오류가 화면에 표시되지 않으므로 사용자를 혼란스럽게 만들지 않는다는 것입니다!
```

<div class="content-ad"></div>

스크립트를 exit 1로 끝낸 이유가 뭐에요?

exit 코드를 사용하면 매우 유용해요! exit 코드가 0이라는 건 프로그램 또는 스크립트가 정상적으로 완료됐음을 나타내고, 그 외의 값은 비정상적인 방법으로 끝났거나 오류 때문일 수 있어요. 이를 통해 문제 해결 작업을 더 쉽게 할 수 있지만, 제어 흐름을 제공하기도 해요. 스크립트나 프로그램의 종료 상태를 $? 변수를 사용하여 확인할 수 있어요. 이를 통해 bash 스크립트에서 다른 스크립트나 프로그램이 비정상적으로 종료된 경우에 대해 어떻게 처리할지 결정할 수 있어요.

```js
❯ ./hello.sh
Running...
Running...
Running...
^C
CTRL+C가 눌렸습니다. 종료 전 정리작업을 처리합니다...
❯
❯ echo $?
1
```

## 결론

<div class="content-ad"></div>

시그널을 처리하는 방법을 알면 ctrl+c를 누르면 스크립트가 종료되기 전에 정리 작업을 수행할 수 있어요! 이 글이 마음에 드셨나요? 댓글을 남겨주세요!