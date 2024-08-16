---
title: "리눅스 자가 삭제 파일 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-LinuxHowtocreateselfdeletingfiles_0.png"
date: 2024-08-03 20:14
ogImage: 
  url: /assets/img/2024-08-03-LinuxHowtocreateselfdeletingfiles_0.png
tag: Tech
originalTitle: "Linux How to create self deleting files"
link: "https://medium.com/@kpatronas/linux-how-to-create-self-deleting-files-ab0b07396cce"
---


<img src="/assets/img/2024-08-03-LinuxHowtocreateselfdeletingfiles_0.png" />

이 기사에서는 자가 삭제 파일을 만드는 방법을 보여드리겠습니다! 일정한 시간이 지난 후나 특정 날짜에 자동으로 삭제되는 파일입니다! 이게 왜 필요한지 궁금할 수도 있겠죠? 제 경우에는 시스템에서 이벤트와 함께 이메일을 보내는 스크립트가 마지막 이메일을 보낸 지 1시간 이내이면 다시 이벤트 이메일을 보내지 않도록 하는 메커니즘을 만들고 싶었기 때문입니다! 그러나 당신도 각별한 경우에 유용하게 쓸 수 있을 겁니다!

## 명령 생성하기

Linux에는 자가 삭제 파일을 만드는 데 사용되는 내장 명령이 없기 때문에 우리가 직접 만들어야 합니다! 생각보다 훨씬 쉽습니다 :)

<div class="content-ad"></div>

아래 스크립트를 작성한 후 sdelete.sh로 저장하세요.

```js
#!/bin/bash

# 올바른 인수의 개수가 제공되었는지 확인
if [ "$#" -ne 2 ]; then
    echo "사용법: $0 <파일이름> <시간>"
    echo "예시: $0 /경로/파일명 '지금부터 5분 후'"
    echo "예시: $0 /경로/파일명 '오후 12시 30분 내일'"
    exit 1
fi

# 인수를 변수에 할당
filename=$1
time=$2

# 파일이름이 전체 경로인지 확인
if [[ "$filename" != /* ]]; then
    echo "오류: 파일이름은 전체 경로여야 합니다."
    exit 1
fi

# 파일 생성
touch "$filename"
if [ $? -eq 0 ]; then
    echo "파일 '$filename'이 생성되었습니다."
else
    echo "오류: 파일 '$filename'을 생성하는 데 실패했습니다."
    exit 1
fi

# 'at' 명령어를 사용하여 파일 삭제 일정 예약
echo "rm \"$filename\"" | at "$time"
if [ $? -eq 0 ]; then
    echo "파일 '$filename'이 $time에 삭제 예약되었습니다."
else
    echo "'at' 명령으로 삭제 일정 예약을 실패했습니다."
    exit 1
fi
```

chmod 명령어로 실행 가능하도록 설정하세요.

```js
chmod +x sdelete.sh
```

<div class="content-ad"></div>

## 테스트

이제 스크립트가 작동하는지 확인하기 위해 몇 가지 테스트를 진행해봅시다.

일정 시간이 지난 후 파일 생성 및 삭제

```js
$ ./sdelete.sh ~/hello.txt 'now +1 minute'
파일 '/home/administrator/hello.txt'이(가) 생성되었습니다.
경고: 명령은 /bin/sh를 사용하여 실행됩니다.
작업 6이 2024년 7월 25일 목요일 17:18:00에 예약되었습니다.
파일 '/home/administrator/hello.txt'이(가) now +1 minute 후에 삭제 예정입니다.

$ ls -l | grep -i hello
-rw-rw-r-- 1 administrator administrator          0 Jul 25 17:17 hello.txt
```

<div class="content-ad"></div>

일 분 후에 확인하면 hello.txt 파일이 존재하지 않는 것을 확인할 수 있습니다.

특정한 타임 스탬프에서 파일 생성 및 삭제

또한 파일이 삭제될 타임스탬프를 매개변수로 정의할 수 있습니다. 이 경우에는 17:50입니다.

```js
$ ./sdelete.sh ~/hello.txt '17:50'
File '/home/administrator/hello.txt'가 생성되었습니다.
경고: 명령이 /bin/sh를 사용하여 실행됩니다.
작업 7이 2024년 7월 25일 목요일 17:50:00에 예약되었습니다.
'/home/administrator/hello.txt' 파일이 17:50에 삭제되도록 예정되었습니다.
```

<div class="content-ad"></div>

## 어떻게 작동하나요?

이 스크립트는 at 명령어에 의존합니다. 리눅스의 at 명령어는 나중 시간에 실행될 명령어를 예약하는 데 사용됩니다. 표준 입력 또는 지정된 파일에서 명령을 읽고 지정된 시간에 실행합니다.

다음은 at 명령어에 대한 몇 가지 간단한 예제입니다.

미래에 작업 예약하기

<div class="content-ad"></div>

```js
echo "ls -l /tmp" | at now + 1 minute
```

특정 시간에 명령을 스케줄링하십시오

```js
echo "echo 'Hello, World!'" | at 10:30 AM
```

특정 날짜와 시간에 명령 스케줄링하기

<div class="content-ad"></div>

```js
echo "shutdown -h now" | at 11:59 PM 12/31/2024
```

## 결론

음.. 파일 자체 삭제하는 것은 그렇게 유용하지 않을지도 모르지만, 이 짧은 글을 즐겁게 보셨으면 좋겠어요!