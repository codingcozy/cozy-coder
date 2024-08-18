---
title: "핵심 리눅스 터미널 해킹 요령 효율성을 위한  파트 2"
description: ""
coverImage: "/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_0.png"
date: 2024-06-19 08:35
ogImage:
  url: /assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_0.png
tag: Tech
originalTitle: "Essential Linux Terminal Hacks for efficiency — Part 2"
link: "https://medium.com/faun/essential-linux-terminal-hacks-for-efficiency-part-2-84c1036d8258"
isUpdated: true
---

## 파트 2: 팁과 꿀팁 - 역 cmd 검색, 히스토리에서 날짜와 시간 등.

![이미지](/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_0.png)

참고: 일부는 기본 명령어일 뿐입니다.

## 1. 역 검색 cmds:

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

명령을 재사용하려면 Ctrl + R을 누르고 일치하는 키워드 몇 개를 입력하여 최근에 사용된 명령을 확인할 수 있습니다. 검색은 명령 히스토리를 기반으로 합니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*8d6OCQLH_WCzvQCcUpCfcQ.gif)

## 2. 히스토리에서 특정 명령 사용하기

이전에 사용된 모든 명령을 나열하려면 history 명령을 사용하고 “!`숫자`”를 사용하여 해당 명령을 재사용하세요.

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

![image](/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_1.png)

## 3. Show Date and time in History:

To view the date and time in the command history, use HISTTIMEFORMAT and select how you want the date to be displayed.

```js
HISTTIMEFORMAT="%d-%m-%y %r" history

%d - Day
%m - Month
%y - Year
%T / %r - Time in 24/12 hour format
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

영구적으로 설정하려면 .bashrc 파일에 아래 코드를 추가하세요.

```js
export HISTTIMEFORMAT="%d-%m-%y %r "
```

<img src="/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_2.png" />

참고: 현재 일자가 기록되지 않았기 때문에 현재 날짜가 모든 날짜에 표시될 수 있지만, 이후에는 정확한 날짜가 표시됩니다.

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

## 4. 한 번에 여러 명령 실행하기:

여러 명령을 동시에 실행하려면 3가지 옵션을 사용하여 함께 연결할 수 있습니다.

- `;` — 명령을 순차적으로 실행합니다.

- `&&` — 하나의 명령이 실패하면 다음 명령이 실행되지 않습니다.

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

|| — Command runs only if the previous cmd fails.

![Screenshot](/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_3.png)

## 5. View content with Less:

Viewing a file with large data using “cat” spams the whole window, using less shows a chunk of content, and then you can scroll up or down.

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

다른 기능들은 다음과 같습니다:

- 검색: 파일 내에서 특정 용어를 /`검색 용어`를 사용하여 검색합니다.
- 탐색: 파일 끝으로 이동하려면 G를 입력하세요, 1G(1행으로 이동), N(이전 검색 반복) 등이 있습니다.
- 옵션: -N(행 번호 표시), -i(검색 시 대소문자 구분 없음), -S(텍스트 자동 줄 바꿈 비활성화) 등과 같이 동작을 사용자 정의할 수 있는 여러 옵션이 있습니다.

## 6. 열(Column):

column 명령어를 사용하여 텍스트 파일이나 명령어 출력을 더 읽기 쉬운 형식으로 볼 수 있습니다.

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
# -t 옵션을 사용하여 표 형식으로 보여줍니다. -s 옵션은 구분자를 지정합니다. column 명령어로 실행하거나 다른 명령어의 출력을 column으로 파이핑하세요.

column -s ',' -t data.csv # 파일로부터 구분된 값으로 표시

cat /etc/passwd | column -s ':' -t
```

<img src="/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_4.png" />

## 6. 파일 자르기

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

파일을 열지 않고 내용을 지우려면 truncate를 사용하세요. -s (--size)로 크기를 지정할 수 있습니다. 0은 파일을 비우는 것이고 다른 숫자는 해당 크기로 줄이는 것을 의미합니다.

```js
truncate -s 0 filename.txt -- 모든 데이터 삭제
truncate -s 100 filename.txt
```

<img src="/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_5.png" />

## 7. Head and Tail:

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

이름 그대로 말하듯이, head는 파일의 처음 몇 줄을 보여주고, tail은 마지막 몇 줄을 보여줍니다.

표시할 줄 수도 지정할 수 있습니다.

```js
head/tail -n 20 <file>
```

`-f` 옵션을 사용한 tail은 로그와 같이 변경되는 파일을 볼 때 유용합니다.

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
tail -f <file>
```

![Image](/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_6.png)

## 8. View exit code:

Exit codes show the results of execution, typically useful for shell scripts. Use `echo $?` to view the exit code of the previous command.

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

<img src="/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_7.png" />

## 9. 중첩 디렉토리 만들기:

중첩 디렉토리를 만들어야 할 때는 /를 사용하여 하위 디렉토리를 정의하세요.

```js
mkdir -p dir/{dir1/subdir1,dir2,dir3/subdir3}
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

<img src="/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_8.png" />

## 10. 파일 명령어:

file 명령어를 사용하여 어떤 파일의 유형을 확인할 수 있습니다. 특히 확장자가 없는 파일이나 diff 파일로 위장된 파일을 식별하는 데 유용합니다.

예시: #!에 파이썬 인터프리터가 있는 .sh 파일.

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

오늘은 여기까지입니다.

part 1은 여기에서 확인하세요:

<img src="/assets/img/2024-06-19-EssentialLinuxTerminalHacksforefficiencyPart2_10.png" />

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

## 👋 만약 이 내용이 도움이 되었다면, 아래 👏 버튼을 몇 번 클릭하여 작성자에 대한 지원을 보여주세요 👇

## 🚀 FAUN 개발자 커뮤니티에 가입하고 매주 이메일로 비슷한 이야기를 받아보세요
