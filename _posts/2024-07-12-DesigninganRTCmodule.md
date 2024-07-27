---
title: "RTC 모듈 설계 방법"
description: ""
coverImage: "/assets/img/2024-07-12-DesigninganRTCmodule_0.png"
date: 2024-07-12 23:03
ogImage: 
  url: /assets/img/2024-07-12-DesigninganRTCmodule_0.png
tag: Tech
originalTitle: "Designing an RTC module"
link: "https://medium.com/gitconnected/designing-an-rtc-module-25a6c6f2353f"
---


아두이노, ESP32, STM32 등을 사용한 많은 홈 자동화/DIY/교육 프로젝트는 실시간 시계(RTC)와 백업 전원원을 필요로 합니다. 그렇게 하면 매번 시간을 재설정할 필요가 없어요. 시장에서 DS3231과 DS1307 모듈과 같은 것들을 많이 찾을 수 있어요. 오늘은 그런 간단한 모듈을 언가-붕가하는 여행에 여러분을 초대하고 싶어합니다. 준비되셨나요? 함께 떠나요!

# 최종 목표

이 실험의 결과물로써, 제가 원하는 것은:

- RTC 모듈의 회로도
- 이 모듈의 PCB 레이아웃
- 제조 파일 (Gerber 파일)

<div class="content-ad"></div>

내가 운동하고 싶은 것:

- RTC 칩의 데이터 시트 읽기
- 회로도 작성
- PCB 레이아웃 만들기
- 구성 요소, 패키지 선택 등

# 고지사항

분명히 하기 위해, 이것은 포괄적인 전문적인 안내서가 아닙니다 — 나는 완전한 초보자이다. PCB 디자인을 한 지 15년이 넘었거나 관련된 것을 만든 지 오랜 시간이 지났습니다. 그러니까 모든 것을 거리낌 없이 받아들이세요. 길을 가면서 배우고 있으며, 나와 같은 사람들에게 도움이 되고, 경험이 풍부한 사람들로부터 통찰력/권장 사항을 받을 수 있기를 희망합니다.

<div class="content-ad"></div>

# 회로 설계

저는 회로 설계부터 시작했어요. 필요한 모든 기능을 갖추고 있고 오픈 소스인 KiCad을 선택했어요.

## 구성 요소

이 모듈의 핵심으로 DS3231M SOP8을 선택했어요:

<div class="content-ad"></div>

- 느슨한 핀이 없습니다.
- I2C를 지원합니다.
- 2개의 알람이 있습니다.
- 높은 정확도를 가지고 있습니다.
- 배터리 백업 핀이 있습니다.
- 외부 크리스탈 발진기가 필요하지 않습니다.

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_0.png)

데이터 시트에 따르면, DS3231M은 전형적으로 3.3V인 2.3V와 5.5V 사이의 DC 전력이 필요합니다. 배터리의 경우, 3V의 전압에 2.3V에서 5.5V까지의 DC 전압을 기대합니다.

전형적인 배터리 전압을 고려하여, 3V을 공급하는 CR2032 코인 배터리를 사용하기로 결정했습니다. 이는 가장 작은 배터리는 아니지만, 널리 사용되며 매우 풍부합니다. 가격도 꽤 낮습니다. 6달러에 10개를 찾아, 배터리 당 60센트입니다. 물론 더 저렴한 옵션도 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-12-DesigninganRTCmodule_1.png" />

DS3231M 데이터 시트에 따르면, 몇 가지 추가 수동 부품이 필요했습니다:

- 32kHz, INT/SQW, SDA 및 SCL 핀용 4개의 풀업 저항기. 저는 이를 위해 4.7k 옴을 선택했습니다.
- 디커플링 커패시터 2개 (하나는 Vbat에 대해서 선택 사항이지만, 혹시나 하여 거기에 넣기로 결정했습니다). 저는 1uF를 사용했습니다.

## 회로

<div class="content-ad"></div>

KiCad에서 깨끗한 새 프로젝트를 생성하고 회로도 편집기로 이동했어요.

먼저, 데크에 필요한 구성 요소를 배치하고 싶었어요:

- DS3231MZ (8핀 패키지 풋프린트임을 확인했어요)
- 내 CR2032를 위한 배터리 셀
- VCC 및 Vbat 디커플링을 위한 비극성 콘덴서 2개
- 병렬 토폴로지의 4개의 저항 네트워크(모든 풀업 저항을 하나의 패키지로 그룹화)
- 리셋을 위한 푸시 버튼
- 재밌게 만들기 위한 저항이 달린 전원 LED
- 소비자를 위한 6개 핀 커넥터

그 다음으로, 각 구성 요소에 값을 부여하러 나갔어요:

<div class="content-ad"></div>

- 디커플링 캐패시터용으로 1uF
- 배터리용으로 3V
- 저항 네트워크용으로 4.7k
- 레드 LED 및 470 옴 저항기

![RTC 모듈 설계](/assets/img/2024-07-12-DesigninganRTCmodule_2.png)

다음 단계로, 모든 것을 적절하게 연결해야 했습니다. 전원 넷부터 시작했습니다. 커넥터의 핀 1을 접지 핀으로 사용하기로 결정했고, 핀 6을 VCC로 사용했습니다.

![RTC 모듈 설계](/assets/img/2024-07-12-DesigninganRTCmodule_3.png)

<div class="content-ad"></div>

전원 및 분리 커패시터를 처리한 후, 전원 LED를 설계하기로 했습니다. 전원 LED를 본 스키마와 분리하여 보다 가독성있게 만들었습니다. KiCad는 넷 이름으로 연결을 이해합니다.

![전원 LED](/assets/img/2024-07-12-DesigninganRTCmodule_4.png)

이어서 리셋 버튼입니다. 데이터시트에 따르면 액티브-로우이며 작동시키려면 로우로 당겨야 합니다. DS3231에 이미 포함된 저항으로 인해 추가 저항이 필요하지 않습니다.

![리셋 버튼](/assets/img/2024-07-12-DesigninganRTCmodule_5.png)

<div class="content-ad"></div>

남은 건 데이터 핀들 뿐이에요. 저는 LED와 같은 방식으로 통신 이름별로 나누어 놓았고, 다음에 커넥터 옆에 놓아서 다이어그램이 많은 겹침이 없게 했어요.

![RTCmodule_6 이미지](/assets/img/2024-07-12-DesigninganRTCmodule_6.png)

![RTCmodule_7 이미지](/assets/img/2024-07-12-DesigninganRTCmodule_7.png)

당연히 PCB 설계 단계에서 여러 번 커넥터 핀을 섞었어요. 트레이스의 겹침을 줄이려고 하지만 일단 여기까지에요! 조금 손질하고 레이블을 붙이면 앞으로 나아갈 준비가 완료돼요.

<div class="content-ad"></div>

하지만 그 전에, 이 스키마가 유효한지 확인하고 싶었어요. 그래서 KiCad에서 전기 규칙 확인을 제공해요. 첫 번째 실행 후 몇 가지 문제를 보았어요.

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_8.png)

3개의 유사한 문제가 있었고, VCC, Vbat 및 그라운드를 가리켰어요. 구글링을 해본 결과, 이 네트가 특별한 심볼 PWR_FLAG로 전원 넷임을 지정해야 했어요. 이러한 플래그를 넣고 다시 확인을 실행하니 정리되었어요!

# PCB 디자인

<div class="content-ad"></div>

## 풋프린트 할당하기

각 구성 요소는 PCB에 풋프린트라 불리는 것이 있어야 합니다. 간단하게 말해, 이는 구성 요소의 패키지를 나타내고 PCB에 어떻게 납땜될 것인지를 보여주는 것입니다. PCB 디자인으로 넘어가기 전에 배치하고 싶은 모든 구성 요소에 풋프린트를 지정해야 했습니다. KiCad에는 각 구성 요소를 수동으로 변경할 필요가 없도록 이를위한 특정 창이 있습니다.

![풋프린트 지정](/assets/img/2024-07-12-DesigninganRTCmodule_9.png)

지금까지 지정한 유일한 풋프린트는 DS3231MZ입니다. 이는 8핀 패키지에 대해 사전 정의된 기본 풋프린트가 있었기 때문입니다. 따라서 콘덴서부터 시작하여 그것을 탐구하기 시작했습니다.

<div class="content-ad"></div>

다양한 구성 요소에 사용할 패키지를 결정하기 위해 https://www.digikey.com/에 들어가서 원하는 구성 요소를 검색했어요. 제가 찾고 싶었던 1uF, SMD, 6.3V 캐패시터를 검색하고 인기 순으로 정렬했어요. 첫 번째 매치는 0201(0603 metric) 패키지의 GRM033R60J104KE19D 캐패시터였어요. 제게 좋아 보여요!

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_10.png)

그래서, 이 패키지를 KiCad 창에서 찾아서 조금 더 큰 패드를 제공하는 HandSolder 옵션을 선택했어요.

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_11.png)

<div class="content-ad"></div>

다른 모든 부품에 대해도 같은 연습을 진행했어요:

- 470 옴 저항: RC0402FR-07470RL, 0402 (1005 미터) 패키지
- 4.7k 옴 저항 어레이, YC164-JR-074K7L, 1206 (3216 미터) - 0603 * 4
- 빨간색 LED, BND0603JKRS001, 0603 패키지
- 리셋 버튼, PTS810 SJK 250 SMTR LFS
- 배터리 홀더, 23-BHCC-1 또는 20mm 간격의 유사한 제품.

커넥터로 2.54mm 핀 헤더를 선택했어요.

![RTC 모듈 설계](/assets/img/2024-07-12-DesigninganRTCmodule_12.png)

<div class="content-ad"></div>

지금 PCB 설계로 넘어갑시다.

## 부품 배치

PCB Editor 창으로 전환하여 회로도에서 PCB를 업데이트하니 발판이 쌓여 있었어요.

![PCB footprints](/assets/img/2024-07-12-DesigninganRTCmodule_13.png)

<div class="content-ad"></div>

PCB에 대한 내 생각은 레이어를 2개 만들어서, 상단 레이어는 신호/전원용으로, 하단은 그라운드 플레인으로 만드는 것이었습니다 (비아를 통해 신호가 필요한 경우를 위해 — 실제로 그랬죠).

백 사이드에 배터리를 놓고, 디커플링 캐패시터는 핀에 가능한 가까이 배치했으며, 나머지는 여기저기로 퍼뜨렸습니다. 이곳의 크기 제약은 배터리 홀더 + 커넥터였습니다.

팁: 선택한 요소를 뒷면으로 뒤집으려면 F 키를 누르세요.

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_14.png)

<div class="content-ad"></div>

그렇게 해서 나는 첫 번째 위치 설정을 마쳤어. 물론, 뭔가를 연결하기 시작하면서 바뀌기는 했지만, 전반적인 모양은 거의 그대로 유지됐어.

## 추적 준비

바닥층이 그라운드 플레인이어야 한다는 걸 알고 있어, 그래서 PCB 설정에서 전원 플레인으로 설정했어.

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_15.png)

<div class="content-ad"></div>

보드 설정 시 JLCPCB에서 몇 가지 제약 조건을 넣어보았어요.

![image](/assets/img/2024-07-12-DesigninganRTCmodule_16.png)

그리고 미리 정의된 트레이스와 비아 크기를 추가했어요 (트레이스는 0.2mm와 0.3mm, 비아는 제약 사항) 그리고 넷 클래스들을 설정했어요 (신호의 경우 0.2mm 트레이스로 기본값, 전원은 살짝 큰  0.3mm 트레이스로).

이제 모든 것이 준비되어 첫 번째 트레이스를 넣을 준비가 되었어요.

<div class="content-ad"></div>

## 부품 연결하기

우선 전원 추적부터 시작했어요. VCC 및 Vbat로 향하는 모든 것들에 연결했죠. 이를 위해 0.3mm의 추적선을 사용하기로 결정했습니다.

<img src="/assets/img/2024-07-12-DesigninganRTCmodule_17.png" />

그리고 이미 조금은 작업을 재배치해야 했어요: 4.7k 어레이를 왼쪽으로 옮겨 VCC 핀과 정렬하고, C2 디커플링 캐패시터를 조금 이동했죠. 이렇게 모든 양전력 추적부가 (아마도) 완료되었어요. 그라운드는 뒷면 전원 플레인으로 연결된 후에 다시 연결할 거예요.

<div class="content-ad"></div>

다음 단계는 신호 핀을 연결하고 크게 엉망을 벌이지 않도록 하는 겁니다. 그래서 진행되었어요.

![image](/assets/img/2024-07-12-DesigninganRTCmodule_18.png)

DS3231의 VCC 레인에 몇 가지 변경 사항이 있음을 알 수 있을 겁니다. 32kHz와 SQW 핀을 IC 아래로, 다시 커넥터까지 via를 통해 추적할 수 있도록 VCC 레인과 LED를 약간 이동하기로 결정했어요.

팁: 새로운 트레이스를 끌 때 V를 눌러 via를 놓을 수 있어요.

<div class="content-ad"></div>

가장 최근에 연결된 넷은 그라운드였어요. 그러기 위해 바닥 부분에 채워진 존을 추가하고 GND 네트에 연결했어요.

팁: 모양을 놓은 후 B를 눌러 평면을 만드세요.

그라운드 평면이 모양이 만들어지고 준비됐으면, 모든 그라운드 핀들을 Via를 사용해 연결했어요.

설계 규칙을 점검한 후, 모든 Via에 오류가 있다는 걸 알았어요. 규칙에 따르면 최소 연핑지지 지름이 0.13이어야 하지만, via 제한에는 0.5/0.3 지름/홀 크기로 나와서 0.1이 돼요. 그래서 지름을 0.06 올려야 했어요.

<div class="content-ad"></div>

또 다른 오류는 다이오드의 최소 간격과 관련이 있었습니다. 그 간격을 1온스 구리 한도인 0.127mm로 낮춰야 했어요. 어쨌든 2온스는 필요 없을 것 같아요.

마지막 오류는 PCB 아웃라인이 누락되었다는 것이었습니다. 그 문제를 해결하려고 엣지-컷 레이어로 전환하고 PCB의 모양을 그렸어요.

그럼 이게 바로 "최종" 결과랍니다.

![이미지](/assets/img/2024-07-12-DesigninganRTCmodule_19.png)

<div class="content-ad"></div>

제가 마지막으로 한 일은 핀을 표시하기 위해 보드 뒷면에 일부 라벨을 추가했어요.

또한 KiCad에는 정말 멋진 3D 뷰도 제공된답니다!

![Image 1](/assets/img/2024-07-12-DesigninganRTCmodule_20.png)

![Image 2](/assets/img/2024-07-12-DesigninganRTCmodule_21.png)

<div class="content-ad"></div>

# 결론

그래요, 프로젝트가 끝났어요 (적어도 제게는 지금까지). 전문가들이 따르는 진짜 프로세스에 얼마나 가까운지는 모르겠지만, 제가 얻은 결과에 매우 만족합니다.

솔직히 말해서, 이 일은 제가 기대했던 것보다 더 재미있고, 기대했던 것보다 더 도전적이었습니다. 하지만, 내가 한 실수만큼 많은 것을 배웠다고 말할 수 있어요 — 정말 많은 실수를 했거든요.

아마 언젠가는 이 보드를 제조해보고 실제로 폭발하는지 확인해볼지도 몰라요, 그건 정말 흥미로울 것 같아요. 그런데 지금은 — 끝났어요.

<div class="content-ad"></div>

# 자료

KiCad: [링크](https://www.kicad.org/)

기사의 프로젝트: [링크](https://github.com/paramoshkinandrew/DS3231M_Module_KiCad)

DS3231M 데이터시트: [링크](https://www.analog.com/media/en/technical-documentation/data-sheets/DS3231M.pdf)

<div class="content-ad"></div>

디지키: https://www.digikey.com/