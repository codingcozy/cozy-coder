---
title: "완벽한 Rust 기반 터미널 설정 가이드"
description: ""
coverImage: "/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_0.png"
date: 2024-08-03 20:17
ogImage: 
  url: /assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_0.png
tag: Tech
originalTitle: "The BEST Fully Rust-based Terminal Setup"
link: "https://medium.com/@nanthony007/the-best-fully-rust-based-terminal-setup-f6384ea3de1d"
---


터미널과 러스트 기반 도구만 사용한 개발 환경 전체적인 개요에 대해 간략히 살펴보겠습니다.

이 내용을 간단하게 유지하기 위해 도구들을 자세히 설명하거나 설치 방법에 대해 다루지 않겠습니다. 대신 각 도구의 웹사이트를 참조하면 유틸리티를 잘 보여주기 때문에 해당 사이트로 이동하도록 안내하겠습니다. 아직 러스트 기술에 열광하지 않은 경우에 대비하여 일반적으로 사용할 수 있는 대체 도구도 제시하겠습니다... 그러나 진심으로 말해, 러스트를 꼭 사용해야 합니다! 이 설정에서 러스트 기반이 아닌 것은 제 폰트뿐이며, 저는 Fira Code Retina를 사용하고 있는데, 이는 프로그래밍 언어로 폰트를 만들 수 없기 때문에 사용하는 것입니다... 혹시 가능할까요?

## 왜 Rust를 선택하고 왜 터미널만 사용하나요?

시작하기 전에 한 가지 주관적인 의견을 먼저 전하겠습니다. 왜 러스트 기반 도구를 선택한 이유일까요? 러스트는 C나 C++로 이전에 작성된 많은 도구를 혁신하고 있는 안전한 메모리 언어입니다. 이것에 대해 저를 믿어주세요.

<div class="content-ad"></div>


![Terminal](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_0.png)

Regarding the second question... using only the terminal might be challenging and require a serious commitment. While I believe it can make you a more versatile developer, nowadays we have tools like VS Code and JetBrains that handle this for us. However, using only the terminal can simplify your workflow by eliminating the need to manage additional applications, avoid memory-hungry IDEs (yes, JetBrains, I'm looking at you!), and forgo Markdown editors like iAWriter. It's just you and your terminal.

Alright, enough small talk, let's dive in! We'll start from the outside and move inward, offering some additional tools along the way, so stay tuned for those!

## The Terminal


<div class="content-ad"></div>

먼저 터미널 에뮬레이터가 필요합니다. 제 macOS에서 사용하기에는 이 선택이 꽤 명확해 보였어요. Warp. Warp는 놀라운 기능들이 많이 있어서 당신이 예상하지 못한 터미널의 기능이 정말 도움이 됩니다. 주목할 몇 가지는:

- 블록
- 코드 완성
- AI 생성 명령어
- 명령어 팔레트

![이미지](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_1.png)

Warp에서 제가 크게 좋아하지 않는 것은 하단 기반 인터페이스인 것 뿐… 단순히 말하자면 제 프롬프트를 위에 두고 이전에 실행된 셀들을 아래에 두는 것을 선호할 뿐이에요.

<div class="content-ad"></div>

만약 Mac을 사용하지 않거나 크로스 플랫폼 에뮬레이터를 선호한다면, 저는 Alacritty도 시도해보았고 꽤 좋아합니다.

만약 Rust 기반 도구에 관심이 없다면, Mac에서는 iTerm을 추천합니다. 메뉴 기반의 사용자 정의가 Alacritty의 파일 기반 구성보다 간단하며, 사용자 정의 옵션도 무궁무진합니다.

## 터미널 멀티플렉서

Zellij! Zellij은 Rust로 작성된 tmux 대안입니다. 제게 가장 큰 장점 중 하나는 화면 분할 및 탭 이동 버튼이 함께 제공된다는 것입니다. 이를 통해 화면 및 탭을 어떻게 이동할지 확인할 수 있습니다!

<div class="content-ad"></div>


![TheBESTFullyRust-basedTerminalSetup_2](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_2.png)

It also comes with built-in layout supports.

## The Shell

Nushell! This shell is new (get it?) on the market and blazing fast! It takes a structured data approach to the shell and brings modern technological expectations (including built-in fetch to the shell). Borrowing some ideas from Powershell, it consumes structured data and even allows you to open and interact with files as tables!


<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_3.png)

누쉘에 대한 유일한 대안으로 피쉬를 추천해요. 벌써 bash와 zsh에서 빠져나오세요. bash로 스크립트를 작성하세요. 하지만 대화형 터미널 사용은 좀 더 사용자 친화적이어야 합니다.

## 보너스 1: 누쉘 테이블

우리의 첫 번째 보너스에요! 누쉘 테이블은 실제로 데이터 테이블입니다! 만약 파이썬에 익숙하다면 이것들은 기본적으로 팬더스 테이블과 비슷합니다. 다만, 팬더스를 사용하는 대신 @polars를 사용해요. 이것은 Rust 기반 데이터 프레임 라이브러리에요! 그래서 (거의) 팬더스에서 할 수 있는 모든 것들을 누쉘 테이블에도 적용할 수 있어요!


<div class="content-ad"></div>

![이미지](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_4.png)

## 프롬프트

Starship! 이 프롬프트는 화려하게 빠르며, 많은 인기있는 쉘과 함께 작동하며, 매우 최소한이되거나 매우 정보성 있게 구성할 수 있습니다. 기본 설정으로 언어 버전, 패키지 버전 및 git 상태와 같은 좋은 정보를 제공합니다!

![이미지](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_5.png)

<div class="content-ad"></div>

대안으로 oh-my-zsh/fish를 추천드립니다. (다만 슬로우하고 부풀어 있는 점이 있어요) 쉘에 따라 다르지만, Starship을 사용하는 걸 권해드릴게요.

## 추가 정보 2: 더 나은 ls

Exa! Exa는 ls를 예쁜 아이콘과 그리드로 변경해줘요. 이 아이콘들에는 Hack Nerd 폰트를 사용해요.

![이미지](/assets/img/2024-08-03-TheBESTFullyRust-basedTerminalSetup_6.png)

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경해주세요.

<div class="content-ad"></div>

여기서 내가 추천하는 유일한 대안은 Lua 기반인 LunarVimf이고 꽤 멋지지만 Helix와 달리 플러그인으로 느려질 수 있어요.

## 보너스 빠른 라운드

보너스 빠른 라운드! 이 멋진 CLI 도구들은 모두 Rust로 제작되었으며 아마도 매일 사용하는 도구들을 개선해요!

@links to all

<div class="content-ad"></div>

- ripgrep: 더 나은, 빠른 grep
- ripgrep-all: 이진 파일용 ripgrep
- fd: 빠른 찾기 도구
- zoxide: 똑똑한 cd
- hyperfine: CLI 벤치마킹 도구

## 결론

이 모든 도구는 전반적으로 훌륭합니다. 유일한 단점은 그 중 일부가 상당히 새로운 도구들이라는 것입니다. 때로는 내가 원하는 방식으로 nutshell 색상을 작동시키는 데 어려움을 겪기도 하며, Zellij은 현재 Warp와 잘 맞지 않습니다. 내 Zellij 색상도 약간 개선이 필요할 것 같습니다.

이 스타일 기사가 마음에 들었으면하고 유익하게 여겼으면 좋겠습니다! 이 도구들 중 일부를 시도해보세요. 아마도 그 중 일부를 들어보지 못했을지도 모릅니다!

<div class="content-ad"></div>

만약 흥미로운 내용을 발견했거나 새로운 도구를 알게 되었다면 한 번 클랩을 해주시고 더 많은 이런 기사를 보려면 저를 팔로우해주세요!