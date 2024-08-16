---
title: "리눅스에서 Plank 설치 및 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-06-InstallingandConfiguringPlankonLinux_0.png"
date: 2024-07-06 10:49
ogImage:
  url: /assets/img/2024-07-06-InstallingandConfiguringPlankonLinux_0.png
tag: Tech
originalTitle: "Installing and Configuring Plank on Linux"
link: "https://medium.com/@huzaifaqureshi037/installing-and-configuring-plank-on-linux-498731d2477c"
---

![Installing and Configuring Plank on Linux Lite](/assets/img/2024-07-06-InstallingandConfiguringPlankonLinux_0.png)

# Installing and Configuring Plank on Linux Lite

![Plank on Linux Lite](/assets/img/2024-07-06-InstallingandConfiguringPlankonLinux_1.png)

If you're looking to enhance your Linux Lite desktop experience with a sleek and lightweight dock, Plank is a great choice. In this guide, we'll walk you through the steps to install and configure Plank on Linux Lite, ensuring it runs on autostart in the background every time you log in.

<div class="content-ad"></div>

# 단계 1: Plank 설치하기

먼저, Plank를 설치해야 합니다. 터미널을 열고 다음 명령을 실행하세요:

Debian/Ubuntu 기반 Linux용 :

sudo apt-get update
sudo apt-get install plank

<div class="content-ad"></div>

Arch 기반 시스템에서 :

```bash
sudo pacman -S plank
```

Fedora/CentOS/RHEL에서 :

```bash
sudo dnf install plank
```

<div class="content-ad"></div>

# 단계 2: 설치 확인

Plank이 올바르게 설치되었는지 확인하려면 터미널에서 실행해볼 수 있어요:

plank

화면 하단에 Plank 독이 나타나는 것을 확인하실 수 있어요.

<div class="content-ad"></div>

/assets/img/2024-07-06-InstallingandConfiguringPlankonLinux_2.png

# 단계 3: 자동 시작을 위한 데스크톱 엔트리 생성

Plank를 자동 시작하도록 만들기 위해 데스크톱 엔트리를 생성해야 합니다.

# 자동 시작 디렉토리 생성하기 (존재하지 않을 경우)

<div class="content-ad"></div>

mkdir -p ~/.config/autostart

# 데스크톱 엔트리 파일 생성하기

nano ~/.config/autostart/plank.desktop

# 파일에 다음 내용 추가하기

<div class="content-ad"></div>

[데스크톱 엔트리]
타입=어플리케이션

이름=플랭크

실행=plank

X-GNOME-Autostart-enabled=true

<div class="content-ad"></div>

아래 Markdown 형식으로 테이블 태그를 변경해주세요.

| NoDisplay | false |
| Comment | Start Plank at login |
| Save the file and exit the editor (Ctrl+X, then Y, then Enter if you are using nano). |
| # Make the Desktop Entry Executable |

<div class="content-ad"></div>

```bash
chmod +x ~/.config/autostart/plank.desktop

# 단계 4: Plank 사용자 정의하기

Plank은 매우 사용자 정의가 가능합니다. 독을 마우스 오른쪽 버튼으로 클릭하고 "환경 설정"을 선택하여 설정에 액세스할 수 있습니다. 여기서 테마를 변경하거나 아이콘 크기를 조절하고 기타 설정을 구성하여 사용자 설정에 맞게 변경할 수 있습니다.

# 단계 5: 자동 시작 확인
```

<div class="content-ad"></div>

로그아웃한 후 다시 로그인하거나 컴퓨터를 재시작하여 Plank가 자동으로 시작되도록 해보세요. 로그인 후 화면 하단에 Plank 독이 나타날 것입니다.

# 단계 6: 문제 해결

Plank가 자동으로 시작되지 않는 경우, 로그 파일을 검사하여 발생한 문제를 확인해보세요. 또한 아래 명령어를 터미널에서 실행하여 시작을 수동으로 테스트할 수도 있습니다:

plank &

<div class="content-ad"></div>

# 결론

위 단계를 따르면 Linux Lite 데스크톱을 멋진 가벼운 Plank 독으로 향상시킬 수 있으며, 로그인할 때마다 자동으로 시작되도록 설정할 수 있습니다. Plank는 데스크톱에 현대적인 느낌을 더할뿐만 아니라 즐겨찾는 애플리케이션에 빠르게 액세스할 수 있도록하여 전반적인 생산성을 향상시킵니다.

커스터마이징을 즐기세요!

저는 프리랜서 웹 및 앱 개발자입니다. 프로젝트가 있을 경우 developerhuzaifa@gmail.com으로 연락주세요.

<div class="content-ad"></div>

또한, 제 포트폴리오 웹사이트를 확인해보세요: [https://www.developerhuzaifa.site](https://www.developerhuzaifa.site)
