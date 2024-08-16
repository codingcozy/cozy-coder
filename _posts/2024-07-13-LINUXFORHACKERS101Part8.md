---
title: "해커를 위한 리눅스 기초 강좌 Part 8"
description: ""
coverImage: "/assets/img/2024-07-13-LINUXFORHACKERS101Part8_0.png"
date: 2024-07-13 22:04
ogImage: 
  url: /assets/img/2024-07-13-LINUXFORHACKERS101Part8_0.png
tag: Tech
originalTitle: "LINUX FOR HACKERS 101 Part 8"
link: "https://medium.com/@agapehearts/linux-for-hackers-101-part-8-83fdd5760143"
---



![이미지](/assets/img/2024-07-13-LINUXFORHACKERS101Part8_0.png)

# 문제 이해하기

그래픽 인터페이스에서 명령 줄로 전환할 때, 파일 및 디렉토리 이름에 있는 공백은 예상치 못한 문제를 일으킬 수 있습니다. 문제를 직접 확인하기 위해 예제를 살펴보겠습니다.

시나리오:


<div class="content-ad"></div>

스페이스라는 디렉토리 안에는 이름에 공백이 있는 파일과 디렉토리가 있습니다:

- file name.txt (파일)
- my dir (디렉토리)

셸은 파일과 디렉토리 모두를 단일 따옴표로 표시하여 file name.txt가 하나의 엔티티라는 것을 나타냅니다.

일반적인 오류:

<div class="content-ad"></div>

- **cat** 명령어 사용:

![2024-07-13-LINUXFORHACKERS101Part8_1](/assets/img/2024-07-13-LINUXFORHACKERS101Part8_1.png)

- 이는 쉘이 file과 name.txt를 두 개의 별개 인자로 해석하여 오류가 발생합니다. cat 명령어는 file 또는 name.txt 라는 파일을 찾을 수 없습니다.

2. **cd** 명령어 사용:

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-13-LINUXFORHACKERS101Part8_2.png)

- shell은 my 및 dir을 두 개의 별개 인수로 볼 수 있기 때문에 cd 명령은 인수가 너무 많다고 에러 메시지를 표시합니다.

# 이러한 문제가 발생하는 이유

bash 쉘은 명령 행 인수의 구분 기호로 공백을 사용합니다. 이로 인해 cat 및 cd와 같은 명령은 하나가 아닌 두 개의 인수로 인식됩니다.


<div class="content-ad"></div>

# 파일 및 디렉터리 이름에 공백 처리하는 해결책

이러한 문제를 피하려면 파일 및 디렉터리 이름에 공백을 사용하지 않는 것이 가장 좋은 방법입니다. 대신 단어 구분자로 대시(-) 또는 밑줄(_)을 사용하세요. 그러나 공백을 피할 수 없는 경우 문제를 해결하는 두 가지 방법이 있습니다:

## 방법 1: 공백 이스케이핑

공백을 피하기 위해 백슬래시(\)를 사용하세요. 이를 통해 셸에 공백을 인수의 일부로 취급하고 구분자로 취급하지 않도록 알릴 수 있습니다.

<div class="content-ad"></div>

예시:

![이미지](/assets/img/2024-07-13-LINUXFORHACKERS101Part8_3.png)

## 방법 2: 이름 인용하기

전체 이름을 따옴표로 감싸세요. 작은 따옴표도 되지만, 특수 문자를 해석하지 않고 단순히 다루도록 하는 것을 원한다면 큰 따옴표를 추천합니다.

<div class="content-ad"></div>

예시:

![이미지](/assets/img/2024-07-13-LINUXFORHACKERS101Part8_4.png)

# 결론

명령줄에서 파일 및 디렉토리 이름에 공백 문제를 피하는 가장 쉬운 방법은 공백을 전혀 사용하지 않는 것입니다. 공백을 피할 수 없다면 백슬래시로 공백을 이스케이프하거나 이름을 이중 따옴표로 묶는 것이 문제를 효과적으로 해결할 수 있습니다. 이러한 기술을 이해하고 적용함으로써 명령줄 환경에서 파일 및 디렉토리 이름에 공백이 포함된 작업을 원할하게 처리할 수 있습니다.