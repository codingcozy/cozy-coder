---
title: "데스티니 인터넷 고스트 - 인터넷 알림 기능 소개"
description: ""
coverImage: "/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_0.png"
date: 2024-08-03 20:10
ogImage: 
  url: /assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_0.png
tag: Tech
originalTitle: "Destiny Internet Ghost  Internet Notifier"
link: "https://medium.com/@diyfactory/destiny-internet-ghost-internet-notifier-49f0cf1a9049"
---


# Destiny Internet Ghost — 인터넷 알림 프로그램

안녕하세요!

인터넷은 우리 삶을 살아가는 방식을 변화시켰어요. 소통, 교육, 은행, 오락 및 쇼핑 등 다양한 분야에서, 인터넷은 우리 세계에 엄청난 영향을 미쳤어요. 
예전처럼 펜이 이 세상에서 가장 중요한 도구였던 시기는 지나고, 지금은 인터넷이 세상을 지배하고 있어요. 
인터넷 없이 우리 세상을 상상하는 것은 정말 어려운 일이에요.

2021년에는 NodeMCU를 사용하여 내 집 라우터를 재부팅하는 "인터넷 하드웨어 감시자"를 만들었어요. 이 장치는 내 라우터 옆에 있어서, 인터넷에 문제가 발생할 때마다 즐겁게 라우터를 관리해 줘요.
올해는 "인터넷 상태 알림 프로그램"을 추가하여 현재 인터넷 상태를 RGB LED를 사용하여 표시하고 싶었어요. 
인터넷이 작동 중일 때는 파란 빛을 표시하고, 인터넷이 끊겼을 때에는 빨간 빛으로 변해요. 동영상: [https://youtu.be/mWDEx0khpWM](https://youtu.be/mWDEx0khpWM)

참고하실 점: 120V 또는 240V AC 전원 배선과 같은 "교류 전원"을 다룰 때는 항상 적절한 장비와 안전 장비를 사용하고, 충분한 기술과 경험이 있는지 확인하거나 굴트역전기사와 상의해주세요. 이 프로젝트는 어린이가 사용하기 위한 것이 아닙니다.

<div class="content-ad"></div>

# 수상 내역

## Instructables에 피처된 비디오

![2024-08-03-DestinyInternetGhostInternetNotifier_0.png](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_0.png)

# 필요한 구성품

<div class="content-ad"></div>


![Destiny Internet Ghost Internet Notifier 1](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_1.png)

For this project, we will need:
- 1 x RGB LED
- 1 x 200 Ohm Resistor
- 1 x WeMos-D1 Mini/NodeMCU
- 1 x 220V to 5V Step Down Converter
- 1 x 2M Long Power Cord
- 1 x 220V Male Wall Plug
- 1 x 3D Printer

## Circuit Diagram

![Destiny Internet Ghost Internet Notifier 2](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_2.png)


<div class="content-ad"></div>

회로는 매우 간단합니다.
D1-mini의 5V 핀과 GND 핀을 스텝다운 컨버터에 연결하세요.
그런 다음 RGB LED의 공통 아노드를 220옴 저항을 통해 D1-mini의 3.3V 핀에 연결하세요.
그런 다음 Blue LED를 D6에, Red를 D7에, Green을 D8에 연결하세요.
그게 전부입니다. 정말 간단하죠.

# 논리

![Circuit Diagram](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_3.png)

장치를 켜면 "www.google.com"에 핑을 보내기 시작합니다.
성공적인 핑을 받으면, 파란색 LED가 켜지고 점점 점멸합니다.
핑이 끊기면 파란색 LED가 꺼지고, 빨간색 LED가 켜져 핑이 다시 성공적으로 받아질 때까지 점점 점멸합니다.

<div class="content-ad"></div>

보드를 몇 일 동안 브레드보드에서 실행한 후, D1-mini 보드가 가끔 멈추고 완전히 응답하지 않는 문제를 발견했어요. 이 상황을 피하기 위해, 매 시간 보드를 "재부팅"하는 로직을 추가했어요.

# 3D 모델

<img src="/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_4.png" />

인터넷을 둘러보는 중에 "Destiny Ghosts"를 우연히 발견했어요.
Ghost는 게임 "Destiny"에서의 AI 동반자에요. 그것은 "여행자"가 우주를 지키도록 가디언들을 돕기 위해 죽는 날 동안 만든 지능체에요.

<div class="content-ad"></div>

나는 Ghosts의 개념과 디자인에 바로 반해 버렸어. Ghost의 3D 모델을 만드느라 며칠을 보냈어. STL 파일은 www.cults3D.com에서 다운로드할 수 있어.

# 3D 프린팅

![image](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_5.png)

Ghosts는 인간 손보다 약간 큰 부유하는 기계 엔티티야. 하나의 빛나는 "눈"이 있는 구 형태의 코어와 "껍데기"로 이루어져 있어. 기본 Ghost 껍데기는 주황색 하이라이트가 있는 8개의 대략적으로 사체형의 하얀 세그먼트로 이루어져 있지만, Ghost들이 코스메틱 목적으로 채택할 수 있는 다양한 모양의 껍데기가 많이 있어.

<div class="content-ad"></div>

게임에서는 유령이 항해에 사용되며, 컴퓨터 시스템에 액세스하거나 어두운 지역을 비추는 데 사용되며, 스패로를 소환하거나 훨씬 더 많은 일을 할 수 있습니다.

3D 프린팅은 매력적인 취미입니다! 3D 프린터를 사용하여 할 수 있는 것들이 많습니다. 3D 모델을 설계하고 3D 프린터를 사용하여 모델을 출력하는 것은 이제 나의 새로운 취미가 되었습니다. 10살 때부터 "메이커"였으며, 항상 나만의 물건을 만들었습니다. 나에게 있어서 3D 프린팅은 축복입니다. 3D 프린팅에 빠져들었습니다.

3D 프린팅은 내 전자 공작실 생활을 영원히 바꿨습니다. 이전에 부품을 주문했을 때, 프로젝트 자원에 맞게 부품이 맞을지 항상 궁금했습니다... 하지만 3D 프린터를 구입한 후에는 전혀 문제가 되지 않았습니다. 맞지 않을 경우에도 스스로 디자인하고 인쇄할 수 있기 때문입니다. 3D 프린터는 분명히 내 전자 공작실에 있던 "빠진 조각"이었습니다.

![이미지](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_6.png)

<div class="content-ad"></div>

# 색칠

![image](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_7.png)

아크릴 색을 사용하여 유령의 몸체를 칠했습니다.

그 후에는 테트라헤들럴 화이트 세그먼트에 오렌지 강조를 슈퍼 접착제로 붙였습니다.
그 다음으로 전면 눈과 뒷면 뚜껑을 유령의 몸체에 슈퍼 접착제로 붙였습니다.
그 후에는 유령의 뒷부분에 스탠드를 슈퍼 접착제로 붙였습니다. 이 스탠드는 책상에 놓을 때 장치를 수직으로 유지합니다.

<div class="content-ad"></div>

# 전자기기 추가하기

![이미지](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_8.png)

회로가 매우 간단하기 때문에 이에 대한 PCB를 디자인하지 않았어요. 저는 D1-Mini와 다른 모든 부품을 일반용도의 퍼프 보드에 납땜했어요. 상단에는 D1-Mini와 RGB LED를, 하단에는 200V에서 5V로 강하 감내하는 변환기와 220옴 저항을 납땜했어요.

다음으로, 2M 긴 전원 코드를 납땜한 뒤, 퍼프 보드를 유닛의 뒷부분에 핫 접착제로 붙였어요.

<div class="content-ad"></div>

# 코드

이 코드는 모든 필요한 라이브러리를 포함하는 것으로 시작합니다. 그런 다음 WiFi 연결 설정에 필요한 모든 변수를 정의합니다.

다음으로 LED 핀을 정의하고 코드에서 사용된 모든 전역 변수 목록을 작성합니다.

setup() 섹션에서는 먼저 모든 핀 모드를 정의한 후 WiFi 연결을 설정합니다. D1-mini 보드에 정적 IP 주소를 할당했습니다. DHCP를 사용하려면 코드에서 이 세 줄을 삭제하십시오.

<div class="content-ad"></div>

루프() 섹션에서는 8.8.8.8 또는 Google.com을 핑하고 성공한 경우 파란 LED를 서서히 켜고 끕니다. 핑이 실패하면 빨간 LED가 서서히 켜졌다가 꺼집니다.

BrightnessController() 및 pingTest() 함수 모두 코드 하단에 정의되어 있습니다.

그런 다음, D1-mini가 멈추거나 응답하지 않게 될 것을 방지하기 위해 ESP.reset() 함수를 사용하여 디바이스를 매 시간 재설정합니다.

# 브레드보드 데모

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_9.png)

3D 프린팅 부품에 전자 부품을 추가하기 전에 브레드보드에서 빠른 테스트를 수행하여 모든 것이 예상대로 작동하는지 확인했습니다. 전원을 켜면 녹색 LED가 깜박입니다. 라우터와 연결이 설정되면 장치가 8.8.8.8을 핑하기 시작합니다. 

처음에는 핑 몇 개가 실패하고 빨간색 LED가 켜집니다. 성공적인 핑을 받으면 파란색 LED가 점점 사라지고 나타납니다. 완료!

# 최종 데모

![이미지](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_10.png)


<div class="content-ad"></div>

그래서, 제 최종 설정은 이렇게 생겼어요.

장치를 켜면 녹색 LED가 라우터와 연결할 때까지 깜박입니다. 그 후에 블루 LED가 서서히 사라지면서 www.Google.com에서 성공적인 핑을 수신했음을 알려줍니다. 
이 장치는 "인터넷 상태 알림기"로서 집 안 어디에나 설치하고 사용할 수 있어요.

# 감사합니다

제 글을 확인해 주셔서 다시 한 번 감사합니다. 도움이 되었으면 좋겣습니다.
제 YouTube 채널을 구독하고 싶다면 여기로 가세요: https://www.youtube.com/user/tarantula3

<div class="content-ad"></div>

비디오: 보기
전체 블로그 글: 보기

## 참고 자료

GitHub: 보기

Stl 파일: 다운로드
NodeMCU를 사용한 인터넷 하드웨어 감시 도구: 보기

<div class="content-ad"></div>

## 나의 작품 지원하기

- BTC: 1Hrr83W2zu2hmDcmYqZMhgPQ71oLj5b7v5
- LTC: LPh69qxUqaHKYuFPJVJsNQjpBHWK7hZ9TZ
- DOGE: DEU2Wz3TK95119HMNZv2kpU7PkWbGNs9K3
- ETH: 0xD64fb51C74E0206cB6702aB922C765c68B97dCD4
- BAT: 0x9D9E77cA360b53cD89cc01dC37A5314C0113FFc3
- LBC: bZ8ANEJFsd2MNFfpoxBhtFNPboh7PmD7M2
- COS: bnb136ns6lfw4zs5hg4n85vdthaad7hq5m4gtkgf23 Memo: 572187879
- BNB: 0xD64fb51C74E0206cB6702aB922C765c68B97dCD4
- MATIC: 0xD64fb51C74E0206cB6702aB922C765c68B97dCD4

내 다음 튜토리얼에서 다시 만나요.

![이미지](/assets/img/2024-08-03-DestinyInternetGhostInternetNotifier_11.png)