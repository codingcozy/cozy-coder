---
title: "Openssh 96p1 업그레이드 과정 완벽 가이드"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-07-09 10:00
ogImage:
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Openssh Upgrade process to 9.6p1"
link: "https://medium.com/@eren.c.uysal/openssh-upgrade-process-to-9-6p1-4d71ca4cd424"
---

먼저, 패키지 관리자를 통해 gcc 패키지를 검색하고 설치하세요.

```bash
sudo yum install gcc
```

Zlib 라이브러리 설치: 먼저 시스템에 zlib 라이브러리를 설치해야 합니다.

```bash
sudo yum install zlib-devel
```

<div class="content-ad"></div>

OpenSSL 라이브러리 설치: 먼저 시스템에 OpenSSL 라이브러리를 설치해야 합니다.

```js
sudo yum install openssl-devel
```

OpenSSH 버전 확인: 현재 OpenSSH 버전을 확인하려면 터미널을 열고 다음 명령어를 실행하세요:

```js
ssh - V;
```

<div class="content-ad"></div>

OpenSSH 버전 9.6p1 또는 9.6 다운로드: OpenSSH의 9.6p1 또는 9.6 버전을 공식 웹사이트나 신뢰할 만한 출처에서 다운로드해보세요. 예를 들어, OpenSSH 웹사이트에서 다운로드하려면:

```js
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.6p1.tar.gz
```

다운로드한 파일 해제하기: 다운로드한 파일을 해제하려면 다음 명령어를 실행하세요:

```js
tar -zxvf openssh-9.6p1.tar.gz
```

<div class="content-ad"></div>

컴파일 및 설치: 컴파일 및 설치를 위해 다음 단계를 따라주세요:

```js
cd openssh-9.6p1
./configure
make
sudo make install
```

설치 확인: 설치가 성공적으로 완료되었는지 확인하려면 다음 명령을 실행하세요:

```js
ssh - V;
```

<div class="content-ad"></div>

필요한 경우 구성 변경: 설치 후 필요에 따라 OpenSSH 구성 파일을 업데이트할 수 있습니다. 예를 들어, /etc/ssh/sshd_config 파일을 확인하고 필요한 설정을 업데이트할 수 있습니다.
