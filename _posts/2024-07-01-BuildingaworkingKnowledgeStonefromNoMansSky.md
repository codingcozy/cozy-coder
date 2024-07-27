---
title: "No Mans Sky에서 작동하는 지식의 돌 만들기 방법"
description: ""
coverImage: "/assets/img/2024-07-01-BuildingaworkingKnowledgeStonefromNoMansSky_0.png"
date: 2024-07-01 17:41
ogImage: 
  url: /assets/img/2024-07-01-BuildingaworkingKnowledgeStonefromNoMansSky_0.png
tag: Tech
originalTitle: "Building a working Knowledge Stone from No Man’s Sky"
link: "https://medium.com/@kgabrieldev/building-a-working-knowledge-stone-from-no-mans-sky-417d25dd6fce"
---


게임 No Man’s Sky에서는 지식의 돌과 상호 작용하여 언어의 단어를 배웁니다. 이 돌은 하나의 단어를 캐릭터의 두뇌로 직접 전달하는 지식을 전달합니다. 하지만 실제 세계에서 언어를 배우는 것은 그렇게 간단하지 않습니다. 그럼에도 불구하고, 당신을 가르치는 지식의 돌을 만들어 봅시다.

이것은 전자 장치 작업에 대한 완전한 자습서가 아닙니다. 이 프로젝트를 사용하거나 작업하는 동안 발생하는 모든 손상에 대해 책임지지 않습니다.

작동하는 지식의 돌을 만들기 위해 다음 부품을 사용합니다:

- 아두이노 나노
- WS2812B LED 스트립
- 모든 것을 전원 공급하기 위한 USB 케이블
- SW-420 충격 센서
- NFC 스티커
- 아두이노를 내 PC에 프로그래밍하기 위해 사용하는 다른 USB 케이블

<div class="content-ad"></div>

지식의 돌 자체를 손에 넣으려면 3D 프린터가 필요합니다. 지식의 돌 파일은 Printables와 MakerWorld에서 찾을 수 있습니다. 모든 전자 기기를 담을 상자 파일도 Printables와 MakerWorld에 있습니다. 제가 돌의 크기를 150%로, 전자 기기를 담을 상자는 원래 크기의 125%로 확대했습니다.

부품을 모두 프린트하신 후에 전자 기기 조립을 시작할 수 있습니다.

다음은 전자 기기를 연결하는 방법을 보여주는 도표입니다.

<div class="content-ad"></div>

!! 중요 !!

아두이노에 LED 스트립을 바로 연결하지 마십시오. 꼭 스위치를 사용하십시오. 아두이노의 전원을 켤 때마다 스위치를 끄십시오 (LED 스트립도 꺼짐). 이렇게 함으로써 USB 포트를 통해 아두이노 (그리고 LED 스트립)를 전원을 공급할 때 아두이노로부터 너무 많은 전력을 받아올리지 못하게 하여 파괴되는 것을 방지할 수 있습니다.

비디오에서는 지식 돌이 두 가지 다른 소스 코드에서 작동하는 것을 보여드렸습니다. 하나는 휴대폰이 닿으면 돌이 밝아지는 것이었고, 다른 하나는 LED 스트립의 색 스펙트럼을 사용하여 돌을 모든 색상으로 밝게 했습니다.

다음은 터치할 때 돌이 밝아지는 첫 번째 코드입니다.

<div class="content-ad"></div>

```js
#include <FastLED.h>

const int vibration_sensor_pin = 2;
const int led_data_pin = 13;
const int leds_number = 15; // 스트립이 가진 LED 수

const int brightness = 180; // 최대 밝기, 0 = 어두움, 255 = 전체 밝기
boolean animated = false; // LED 스트립이 현재 애니메이션을 수행 중인가요?

CRGB leds[leds_number];

void setup() {
  pinMode(vibration_sensor_pin, INPUT);

  // 진동 센서가 신호를 감지할 경우 애니메이션 메서드를 실행합니다.
  attachInterrupt(digitalPinToInterrupt(vibration_sensor_pin), animation, RISING);

  FastLED.addLeds<WS2812B, led_data_pin, GRB>(leds, leds_number);
  
  for(int dot = 0; dot < leds_number; dot++) { 
    leds[dot] = CRGB::Black;
  }
  FastLED.show();

  // LED 스트립이 작동하는지 확인하기 위해 RGB로 밝혀보세요.
  // 그 후 꺼져요.
  fill_solid(leds, leds_number, CRGB::Red);
  FastLED.show();
  delay(300);
  fill_solid(leds, leds_number, CRGB::Green);
  FastLED.show();
  delay(300);
  fill_solid(leds, leds_number, CRGB::Blue);
  FastLED.show();
  delay(300);
  fill_solid(leds, leds_number, CRGB::Black);
  FastLED.show();
}

void loop() {
}

void animation() {
  // 시간 측정
  unsigned long current_time = millis();
  static unsigned long last_interrupt_time = current_time - 6000;

  // 신호 튕김 방지
  // 마지막 신호가 5초 이내에 발생했으면 무시합니다.
  if(current_time - last_interrupt_time > 5000 && !animated) {
    // LED 스트립에서 애니메이션 실행
    animated = true;

    // 1.5초 동안 점점 더 밝게
    for(int curr_brightness = 0; curr_brightness <= brightness; curr_brightness++) {
      fill_solid(leds, leds_number, CRGB(0, 180, 255));
      FastLED.setBrightness(curr_brightness);
      FastLED.show();
      delay(1500 / brightness);
    }

    // 10초간 유지
    delay(10000);

    // 1.5초 동안 점점 어둡게
    for(int curr_brightness = brightness; curr_brightness >= 0; curr_brightness--) {
      fill_solid(leds, leds_number, CRGB(0, 180, 255));
      FastLED.setBrightness(curr_brightness);
      FastLED.show();
      delay(1500 / brightness);
    }

    animated = false;
  }

  // 다음 바운스를 감지하기 위해 시간 업데이트
  last_interrupt_time = current_time;
}
```

지식의 돌이 모든 색상을 순환하는 코드는 여기에서 확인할 수 있습니다.

```js
#include <FastLED.h>

const int led_data_pin = 13;
const int leds_number = 15; // 스트립이 가진 LED 수

const int brightness = 180; // 최대 밝기, 0 = 어두움, 255 = 전체 밝기

CRGB leds[leds_number];

void setup() {
  FastLED.addLeds<WS2812B, led_data_pin>(leds, leds_number);
  
  // LED 스트립 초기화
  for(int dot = 0; dot < leds_number; dot++) { 
    leds[dot] = CRGB::Black;
  }
  FastLED.show();
}

void loop() {
  static uint8_t hue = 0; // 색을 나타내는 hue

  // 현재 색상 표시하고 hue 값 증가
  FastLED.showColor(CHSV(hue++, 255, brightness));

  // 다음 색상 표시 전에 20밀리초 대기
  delay(20);
}
```

PC를 아두이노 나노에 USB 포트를 통해 연결할 때 LED 스트립을 연결 해제하는 것이 중요합니다. 이전에 언급한 대로 LED는 아두이노 일부를 파괴할 정도의 전원을 끌어낼 수 있습니다.


<div class="content-ad"></div>

이제 충격 센서를 흔들어 빛 효과를 활성화한 후, 5초 이상 휴식을 주도록 설정했어요. 충격 센서가 너무 민감하거나 반대로 민감하지 않다고 생각되면 시계방향 또는 반시계방향으로 나사를 돌려 민감도를 조절할 수 있어요.

스마트폰으로 돌을 사용해 웹사이트를 열 수 있도록 하려면 NFC 스티커에 웹사이트 URL을 써야 해요. 스마트폰과 NFC Tools 앱(구글 플레이 / 앱 스토어)을 사용해 URL https://nms-words.kgabriel.dev를 데이터로 추가한 후, 스마트폰에 스티커를 대고 앱에서 "쓰기" 탭으로 전환해 이를 스티커에 쓰면 돼요.

이제 모두 함께 모아놓기 전 마지막 단계로 NFC 스티커를 지식의 돌의 내부 상단에 붙이는 거예요.

소띠하고 프로그래밍을 마치면, 조립한 상자 안에 전자 부품을 조심스럽게 넣으면 돼요. LED 스트립은 바깥쪽에 두고 상자 측면의 평평한 표면에 붙일 수 있어요. LED 스트립을 더 고정하기 위해 저는 집게로 고정했어요.

<div class="content-ad"></div>

🎉이제 작동하는 지식의 돌이 생겼어요!🎉

튜토리얼을 즐기셨길 바랍니다! 만약 아직 보지 않았다면, 이 지식의 돌을 보여준 영상도 확인해보는 것을 잊지 마세요. 정말로 중요한 일이에요.