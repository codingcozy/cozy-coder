---
title: "리눅스 배우기  네트워킹 구성 7단계"
description: ""
coverImage: "/assets/img/2024-07-07-LearningLinux7ConfigruingNetworking_0.png"
date: 2024-07-07 19:42
ogImage: 
  url: /assets/img/2024-07-07-LearningLinux7ConfigruingNetworking_0.png
tag: Tech
originalTitle: "Learning Linux — 7.Configruing Networking"
link: "https://medium.com/@bradyxu831/learning-linux-7-configruing-networking-8ad0f1401246"
---


## 리눅스 학습 여정을 공유하는 곳

![이미지](/assets/img/2024-07-07-LearningLinux7ConfigruingNetworking_0.png)

# 1. 인터페이스 및 연결

1) 장치 및 연결

<div class="content-ad"></div>

- 장치는 네트워크 인터페이스 카드입니다.
- 연결은 장치가 네트워크에 연결하는 방식을 정의하는 구성입니다.

2) 네트워크 인터페이스 카드의 기본 이름은 펌웨어, 장치 위상 및 장치 유형을 기반으로 합니다. 이름은 세 부분으로 구성됩니다:

- 첫 번째 부분
  - 이더넷 인터페이스는 en으로 시작합니다.
  - WLAN 인터페이스는 wl로 시작합니다.
  - WWAN 인터페이스는 ww로 시작합니다.
- 두 번째 부분은 어댑터 유형을 나타냅니다.
  - 내장 어댑터는 o를 사용합니다.
  - 핫플러그 슬롯(슬롯에 장착하는 장치)은 s로 표시됩니다.
  - PCI 위치에 있는 장치는 p로 표시됩니다.
  - 관리자는 또한 네트워크 카드의 MAC 주소를 기반으로 장치 이름을 만들기 위해 x를 사용할 수 있습니다.
- 세 번째 부분은 인덱스, ID 또는 포트를 표시하는 숫자입니다.
- 기본 이름을 결정할 수 없는 경우, 전통적인 이름이 사용됩니다.
- 전통적인 이름(eth0, eth1 등) 또는 BIOS 장치를 기반으로 한 이름과 같이 다른 유형의 이름이 있습니다.

## 2. 네트워크 구성 유효성 검사

<div class="content-ad"></div>

1) 명령어 — ip

- ip addr show: IP 주소에 대한 정보를 표시합니다.
- ip route show: 라우팅 테이블에 대한 정보를 표시합니다.
- ip link show: 모든 네트워크 인터페이스에 대한 기본 정보를 표시합니다.
- ip -s link show: 모든 네트워크 인터페이스에 대한 상세 통계를 표시합니다.
- ip link set dev `장치_이름` `상태`: 네트워크 인터페이스의 상태를 변경합니다. 예: ip link set dev eth0 up 또는 ip link set dev eth1 down
- 최신 Linux 배포판에서는 ifconfig 유틸리티를 사용하지 마세요.

2) 명령어 — netstat 및 ss

- netstat -tuln: 수신 대기 중인 TCP 및 UDP 포트를 표시합니다.
- netstat -a: 모든 연결을 표시합니다.
- netstat -r: 라우팅 테이블을 표시합니다.
- ss -tuln: 수신 대기 중인 TCP 및 UDP 소켓을 표시합니다.
- ss -a: 모든 소켓을 표시합니다.
- ss -r: 주소를 해결합니다.

<div class="content-ad"></div>

3) ip, netstat, 그리고 ss에 대해 명확히

- ip 명령을 사용한 구성은 비영속적이므로 구성을 영속화하려면 nmcli 또는 nmtui를 사용해야 합니다.
- netstat와 ss 명령은 주로 모니터링 및 표시에 사용되며 구성에는 사용되지 않습니다.

4) 루프백 주소, 모든 주소, 그리고 임의의 포트

- 127.0.0.1 - IPv4 루프백 주소로, 로컬 기기의 프로세스 간 통신에 사용됩니다.
- [::1] - IPv6 루프백 주소로, 로컬 기기의 프로세스 간 통신에 사용됩니다.
- 0.0.0.0 - 모든 IPv4 주소를 나타냅니다.
- [::] - 모든 IPv6 주소를 나타냅니다.
- * - 임의의 포트를 나타냅니다.
- 0.0.0.0:* - 모든 IPv4 주소 및 임의의 포트에서 수신 대기합니다.
- [::]:* - 모든 IPv6 주소 및 임의의 포트에서 수신 대기합니다.

<div class="content-ad"></div>

# 3. nmtui와 nmcli를 사용하여 네트워크 구성하기

1) NetworkManager 서비스

- NetworkManager 서비스는 RHEL에서 네트워킹을 담당합니다.
- NetworkManager가 시작되면 /etc/NetworkManager/system-connections에서 구성 파일을 읽습니다.
- 구성 파일에는 ens160.nmconnection과 같은 네트워크 인터페이스 이름과 대응하는 이름이 있습니다. 연결을 생성할 때마다 구성 파일이 생성되어 /etc/NetworkManager/system-connections 디렉토리에 저장됩니다.
- systemctl status NetworkManager: NetworkManager의 현재 상태를 확인합니다.
- 일반 사용자는 ssh를 제외한 네트워크 구성을 변경할 수 있습니다.
- nmcli general permissions: NetworkManager 작업과 관련된 현재 사용자 권한을 확인합니다.

2) nmcli

<div class="content-ad"></div>

1) **nmcli con show**: 모든 네트워크 연결을 나열합니다.
2) **nmcli dev status**: 모든 네트워크 장치를 나열합니다.
3) **nmcli con show `connection_name`**: 특정 연결의 모든 속성을 보여줍니다.
4) **nmcli dev show `device_name`**: 특정 네트워크 장치의 모든 속성을 보여줍니다.
5) **nmcli con add `…`**: 새로운 네트워크 연결을 추가합니다.
6) **nmcli con mod `…`**: 기존의 네트워크 연결을 수정합니다.
7) **nmcli con down `connection_name`**: 연결을 비활성화합니다.
8) **nmcli con up `connection_name`**: 연결을 활성화합니다.
9) **man 5 nm-settings** 또는 **man nmcli-examples**: 유용한 문서와 예시입니다.

3) **nmtui**

- nmtui 도구에는 세 가지 메뉴가 있습니다.
  - 연결 편집
  - 연결 활성화
  - 시스템 호스트명 설정

4) **nm-connection-editor**

<div class="content-ad"></div>

- `nm-connection-editor` 명령을 사용하면 연결을 편리하게 구성할 수 있는 인터페이스가 열립니다.

## 4. 호스트 이름 및 이름 해결

1) 호스트 이름

- 완전한 호스트 이름을 FQDN (Fully Qualified Domain Name)이라고 합니다.
- 완전한 호스트 이름 = 호스트 이름 + DNS 도메인입니다.
- 예: myserver.example.com, myserver는 호스트 이름이고 example.com은 DNS 도메인입니다.

<div class="content-ad"></div>

2) 호스트 이름 변경

- hostnamectl status 명령어: 현재 호스트 이름을 보여줍니다.
- 호스트 이름을 변경하는 세 가지 방법:
  - nmtuil 도구를 사용합니다.
  - hostnamectl set-hostname `new-hostname`
  - /etc/hostname 파일을 수정하여 기존 호스트 이름을 새 호스트 이름으로 대체합니다.

3) 호스트 이름 해결

- 호스트 이름 해결을 위한 두 가지 방법:
  - /etc/hosts 파일 구성: IpAddress Hostname1 Hostname2
  - DNS 서버 사용
- /etc/nsswitch.conf 파일의 "hosts: files dns myhostname" 라인으로 인해, 호스트 이름을 해결하기 위해 DNS보다 /etc/host 파일의 우선순위가 높습니다.
- 인터넷 상의 호스트와 통신하기 위해서는 /etc/hosts만으로는 충분하지 않고 DNS가 필요합니다.
- NetworkManager는 DNS 설정을 /etc/sysconfig/network-scripts에 저장하고 이름 해결을 위해 `/etc/resolv.conf`에 푸시합니다. 다음 재부팅 시 NetworkManager에 의해 덮어쓰이지 않도록 `/etc/resolv.conf`를 직접 편집하는 것은 피해야 합니다.
- 어떤 DNS 서버에 연락할지를 지정하는 세 가지 방법:
  - nmtuil 도구 사용
  - DHCP 서버를 사용하여 DNS 서버 IP 주소를 설정합니다.
  - nmcli con mod `connection-id` [+]ipv4.dns `ip-of-dns`: nmcli 도구 사용. 여러 IP 주소를 추가할 때 +를 사용합니다. 예: nmcli con mod MyConnection ipv4.dns 8.8.8.8, nmcli con mod MyConnection +ipv4.dns 8.8.8.8 8.8.4.4
- getent hosts `servername` 명령어: /etc/hosts와 DNS에서 검색하여 호스트 이름 해결을 확인합니다.