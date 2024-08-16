---
title: "리눅스 정규 표현식 활용 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-12-LinuxRegularExpression_0.png"
date: 2024-07-12 22:54
ogImage: 
  url: /assets/img/2024-07-12-LinuxRegularExpression_0.png
tag: Tech
originalTitle: "Linux Regular Expression"
link: "https://medium.com/@adnanturgayaydin/linux-regular-expression-0158f7491905"
---


아래는 마크다운 형식으로 표로 변환해보겠습니다.


![](/assets/img/2024-07-12-LinuxRegularExpression_0.png)

이 실습 교육의 목적은 Linux에서 정규 표현식을 사용하는 방법을 가르치는 것입니다.

## 학습 목표

이 실습 교육을 마치면 다음을 할 수 있게 될 것입니다;


<div class="content-ad"></div>

- Linux에서 정규 표현식을 사용하세요.

# 개요

- 파트 1 — Linux 정규 표현식

# 파트 1 — Linux 정규 표현식

<div class="content-ad"></div>

리눅스 정규 표현식은 데이터를 검색하고 복잡한 패턴을 일치시키는 데 도움이 되는 특수 문자 또는 문자 집합입니다. 정규 표현식은 ‘regexp’ 또는 ‘regex’로도 불립니다. 이들은 sed, awk, grep 등 많은 다른 리눅스 명령어에서 사용됩니다.

- 다양한 종류의 Regex를 사용하는 방법을 이해해 봅시다.
- 기본 정규 표현식:

기호설명.replaces any character^matches start of string$matches end of string*matches up zero or more times the preceding character\특수 문자를 나타냅니다()정규 표현식 그룹?정확히 한 문자와 일치합니다

- 파일을 만들어 이름을 fruits.txt로 지정하세요.

<div class="content-ad"></div>

```json
{
  "apple": "사과",
  "watermelon": "수박",
  "orange": "오렌지",
  "strawberry": "딸기",
  "blueberry": "블루베리",
  "lemon": "레몬",
  "blackberry": "블랙베리",
  "raspberry": "라즈베리",
  "cranberry": "크랜베리",
  "kak": "칵",
  "kek": "켁",
  "kik": "킉",
  "kbk": "큱",
  "kdk": "크끇",
  "kCk": "크럅",
  "k5k": "칭",
  "kalk": "캍"
}
```

<div class="content-ad"></div>

```js
cat fruits.txt | grep k.k
```

# ^ symbol

```js
cat fruits.txt | grep ^b
```

# $ symbol

<div class="content-ad"></div>

```js
cat fruits.txt | grep n$
```

# [ ] 사용법

- [ab] a 또는 b와 일치
- [a-z] 소문자와 일치
- [0-9] 숫자와 일치

```js
cat fruits.txt | grep k[adb]k
cat fruits.txt | grep k[a-z]k
cat fruits.txt | grep k[A-Z]k
cat fruits.txt | grep k[a-zA-Z]k
cat fruits.txt | grep k[0-9]k
cat fruits.txt | grep k[a-zA-Z0-9]k
```

<div class="content-ad"></div>

# 'n' 사용법

- 'n'은 이전 문자가 'n'번 정확히 나타날 때 일치

```js
cat fruits.txt | grep -E "p{2}"
```

# +는 하나 이상, *는 영 또는 그 이상, ?는 영 또는 일회성.

<div class="content-ad"></div>

- "match.txt" 파일을 만들어주세요.

```js
aa
axa
axxa
axxxa
axxxxa
axxxxxa
axxxxxxa
```

```js
cat match.txt | grep -E "ax{2}a"
```

```js
cat match.txt | grep -E "ax+a"  # '+'는 하나 이상을 나타냅니다.
```

<div class="content-ad"></div>

```js
cat match.txt | grep -E "ax*a"  # '*'는 0회 이상
```

```js
cat match.txt | grep -E "ax?a"  # '?'는 0회 또는 1회
```