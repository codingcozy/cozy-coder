---
title: "라즈베리 파이 시작하기 첫 번째 프로젝트 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-01-GettingStartedwithRaspberryPiBuildingYourFirstProject_0.png"
date: 2024-07-01 17:45
ogImage: 
  url: /assets/img/2024-07-01-GettingStartedwithRaspberryPiBuildingYourFirstProject_0.png
tag: Tech
originalTitle: "Getting Started with Raspberry Pi: Building Your First Project"
link: "https://medium.com/@thisistamim/getting-started-with-raspberry-pi-building-your-first-project-fe2291f96ab3"
---


![Getting Started with Raspberry Pi](/assets/img/2024-07-01-GettingStartedwithRaspberryPiBuildingYourFirstProject_0.png)

라즈베리 파이는 작고 저렴한 컴퓨터로, 프로그래밍 학습부터 복잡한 전자 제품까지 다양한 프로젝트에 사용할 수 있습니다. 초보자든 경험 많은 개발자든, 라즈베리 파이는 컴퓨팅과 전자공학의 세계로 입문하기 쉽고 재미있는 방법을 제공합니다. 이 기사에서는 라즈베리 파이 설정부터 간단한 프로젝트 완성까지 안내해드리겠습니다: 기본 LED 깜박임 회로 만들기.

## 준비물

시작하려면 다음 구성품이 필요합니다:

<div class="content-ad"></div>

- Raspberry Pi (모델 3 이상 권장)
- Raspberry Pi OS가 설치된 MicroSD 카드 (적어도 8GB)
- 전원 공급 장치 (5V, 2.5A)
- HDMI 케이블 (모니터에 연결)
- USB 키보드와 마우스
- 브레드보드
- LED (어떤 색상이든 상관 없음)
- 저항기 (220옴)
- 점퍼 와이어

## 단계 1: Raspberry Pi 설정

- Raspberry Pi OS 설치: MicroSD 카드에 OS가 이미 설치되어 있지 않은 경우 Raspberry Pi 웹사이트에서 다운로드할 수 있습니다. Rufus와 같은 도구를 사용하여 OS 이미지를 MicroSD 카드에 플래시합니다.
- MicroSD 카드 삽입: MicroSD 카드를 Raspberry Pi의 슬롯에 넣습니다.
- 주변 기기 연결: HDMI 케이블을 Raspberry Pi와 모니터에 연결하십시오. USB 키보드와 마우스를 Raspberry Pi의 USB 포트에 연결합니다.
- 전원 공급: 전원 공급 장치를 꽂아 Raspberry Pi를 켭니다. Raspberry Pi OS 데스크톱이 표시될 것입니다.

## 단계 2: LED 깜박임 프로그램 작성

<div class="content-ad"></div>

- 터미널 열기: 작업 표시 줄에서 터미널 아이콘을 클릭하여 명령줄 인터페이스를 엽니다.
- GPIO 라이브러리 설치: 다음 명령을 입력하여 GPIO 라이브러리가 설치되어 있는지 확인합니다:

```js
sudo apt-get update
sudo apt-get install python3-rpi.gpio
```

3. 파이썬 스크립트 만들기: 텍스트 편집기(예: Thonny Python IDE)를 열고 다음 코드를 작성하여 LED를 깜박이게 만듭니다:

```js
import RPi.GPIO as GPIO
import time

LED_PIN = 18

GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)

try:
    while True:
        GPIO.output(LED_PIN, GPIO.HIGH)
        time.sleep(1)
        GPIO.output(LED_PIN, GPIO.LOW)
        time.sleep(1)
except KeyboardInterrupt:
    pass
finally:
    GPIO.cleanup()
```

<div class="content-ad"></div>

4. 스크립트 저장: 스크립트를 blink.py로 저장해 주세요.

## 단계 3: 회로 만들기

- LED를 브레드보드에 놓기: LED를 브레드보드에 삽입하세요. 긴 다리(양극)가 긍정(+), 짧은 다리(음극)가 부정(-)임을 유의해 주세요.
- 저항 연결: 저항의 한쪽 끝을 LED의 짧은 다리(음극)에 연결하고 다른 쪽을 브레드보드의 그라운드 레일에 연결하세요.
- 점퍼 와이어 연결:

  - 라즈베리 파이의 GPIO 핀 18번에서 LED의 양극에 점퍼 와이어를 연결하세요.
  - 브레드보드의 그라운드 레일에서 라즈베리 파이의 GND 핀에 접지 핀용 점퍼 와이어를 연결하세요.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-01-GettingStartedwithRaspberryPiBuildingYourFirstProject_1.png)

## 단계 4: 프로그램 실행

- Python 스크립트 실행: 터미널에서 스크립트를 저장한 디렉토리로 이동하여 다음을 실행합니다:

```bash
python3 blink.py
```

<div class="content-ad"></div>

2. LED 깜박임 확인: 만약 모든 것이 제대로 연결되어 있다면 LED가 1초마다 켜졌다가 꺼졌다가 할 것입니다.

이 간단한 LED 깜박임 프로젝트는 Raspberry Pi에서 GPIO (일반용도 입력/출력) 프로그래밍의 기본을 소개합니다. 여기서부터는 날씨 관측소를 만들거나 홈 자동화 시스템을 구축하거나 자신만의 레트로 게임 콘솔을 개발하는 등 더 복잡한 프로젝트를 탐험할 수 있습니다. 가능성은 무궁무진하며, 방대한 커뮤니티와 자료들이 제공되므로 Raspberry Pi로 무엇을 할지 아이디어가 절대 바닥나지 않을 것입니다. 아래 댓글에 경험과 통찰을 공유해 주세요! 즐거운 만들기!!