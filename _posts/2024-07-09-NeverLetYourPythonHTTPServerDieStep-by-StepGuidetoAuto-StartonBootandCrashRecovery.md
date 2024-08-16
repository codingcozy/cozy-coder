---
title: "Python HTTP 서버 절대 멈추지 않게 하기 부팅 시 자동 시작과 크래시 복구 단계를 체계적으로 설명"
description: ""
coverImage: "/assets/img/2024-07-09-NeverLetYourPythonHTTPServerDieStep-by-StepGuidetoAuto-StartonBootandCrashRecovery_0.png"
date: 2024-07-09 22:58
ogImage: 
  url: /assets/img/2024-07-09-NeverLetYourPythonHTTPServerDieStep-by-StepGuidetoAuto-StartonBootandCrashRecovery_0.png
tag: Tech
originalTitle: "Never Let Your Python HTTP Server Die: Step-by-Step Guide to Auto-Start on Boot and Crash Recovery"
link: "https://medium.com/@ponnala/never-let-your-python-http-server-die-step-by-step-guide-to-auto-start-on-boot-and-crash-recovery-1f7b0f94401e"
---


<img src="/assets/img/2024-07-09-NeverLetYourPythonHTTPServerDieStep-by-StepGuidetoAuto-StartonBootandCrashRecovery_0.png" />

이 기사에서는 부팅 시 자동으로 시작하고 충돌 시 다시 시작하는 Python HTTP 서버를 설정하는 과정을 안내해 드리겠습니다. 이것은 정적 HTML 사이트를 제공하고 24/7 온라인 상태를 유지하는 데 완벽합니다. 이 간단한 단계를 따르면 수동으로 서버를 시작할 필요가 없어집니다!

## Python의 HTTP 서버를 사용하는 이유

Python의 내장 HTTP 서버는 정적 파일을 빠르고 쉽게 제공하는 방법입니다. 개발 및 소규모 프로젝트에 적합합니다. 그러나 기본적으로 시스템 재부팅이나 서버 충돌 시 자동으로 시작하지 않습니다. 이것이 이 가이드가 유용한 이유입니다.

<div class="content-ad"></div>

## 배울 내용:

- Python HTTP 서버를 관리하기 위한 systemd 서비스를 생성하는 방법
- 서버가 부팅될 때 서버가 시작되도록 하는 방법
- 서버가 충돌할 경우 자동으로 다시 시작하는 방법

## 선행 요구 사항:

- Linux 시스템(이 예제에서는 Ubuntu 사용)
- 명령 줄에 대한 기본 지식
- 시스템에 Python이 설치되어 있어야 함(Python 3 권장)

<div class="content-ad"></div>

# 단계 1: 정적 사이트 구성하기

우선, 정적 사이트 파일을 정리해 봅시다. 디렉토리를 만들고 HTML, CSS, JavaScript 및 기타 에셋을 넣어주세요.

```js
mkdir my_static_site
cd my_static_site
touch index.html styles.css script.js
```

index.html에 내용을 추가하여 테스트해보세요:

<div class="content-ad"></div>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Static Site</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Welcome to My Static Site</h1>
    <p>This is a simple static site served with Python HTTP server.</p>
    <script src="script.js"></script>
</body>
</html>


# 단계 2: Python HTTP 서버 시작하기

사이트 디렉토리로 이동하여 다음 명령어로 서버를 시작하세요:

```bash
cd /path/to/my_static_site
python3 -m http.server 8000
``` 


<div class="content-ad"></div>

브라우저를 열고 http://localhost:8000 로 이동하세요. 여러분의 사이트가 표시될 겁니다!

# 단계 3: systemd 서비스 생성

이제 시스템디 서비스를 만들어서 서버를 관리해보겠습니다. 이를 통해 서버가 부팅 시 시작되고, 문제가 발생할 경우 다시 시작될 수 있습니다.

새 서비스 파일을 생성하세요:

<div class="content-ad"></div>

```bash
sudo nano /etc/systemd/system/static-site.service
```

파일에 다음 내용을 추가하세요:

```bash
[Unit]
Description=정적 사이트용 Python HTTP 서버
After=network.target

[Service]
ExecStart=/usr/bin/python3 -m http.server 8000 --directory /path/to/your/static/site
WorkingDirectory=/path/to/your/static/site
Restart=always
RestartSec=10
User=your-username
Group=your-group

[Install]
WantedBy=multi-user.target
```

`/path/to/your/static/site`을 실제 사이트 디렉토리 경로로 바꿔주세요. `your-username` 및 `your-group`을 실제 사용자 이름 및 그룹으로 바꿔주세요.

<div class="content-ad"></div>

# 단계 4: systemd 다시 로드

새 서비스를 인식하도록 systemd를 다시 로드합니다:

```js
sudo systemctl daemon-reload
```

# 단계 5: 서비스 활성화

<div class="content-ad"></div>

부팅 시 서비스를 시작하도록 설정하세요:

```js
sudo systemctl enable static-site.service
```

# 단계 6: 서비스 시작

서비스를 즉시 시작하세요:

<div class="content-ad"></div>

```js
sudo systemctl start static-site.service
```

# 단계 7: 서비스 확인

서비스의 상태를 확인하여 정상적으로 실행 중인지 확인하세요:

```js
sudo systemctl status static-site.service
```

<div class="content-ad"></div>

당신은 개발자이십니다. 제가 도와드리겠습니다.

```js
● static-site.service - 정적 사이트용 Python HTTP 서버
   로드됨: /etc/systemd/system/static-site.service에서 로드됨 (활성화됨; 벤더 프리셋: 활성화됨)
   활성화: [날짜]; [시간] 이후로 활성화됨 (실행 중)

# 결론

축하합니다! 부팅 시 자동으로 시작되고, 중단되면 다시 시작하는 Python HTTP 서버를 설정했습니다. 이 설정은 수동 개입없이 정적 사이트가 온라인 상태로 유지되도록 합니다. 작은 프로젝트와 개발 환경에 완벽합니다!
```