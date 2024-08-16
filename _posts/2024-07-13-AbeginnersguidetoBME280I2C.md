---
title: "초보자를 위한 BME280 I2C 사용 가이드"
description: ""
coverImage: "/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_0.png"
date: 2024-07-13 22:17
ogImage: 
  url: /assets/img/2024-07-13-AbeginnersguidetoBME280I2C_0.png
tag: Tech
originalTitle: "A beginner’s guide to BME280 I2C"
link: "https://medium.com/gitconnected/a-beginners-guide-to-bme280-i2c-00bd4c6bcaad"
isUpdated: true
---





# 여기서 무엇을 할 것인가요?

음, 그겣은 상당히 간단하죠! 이 게시물에서는 새로운 반짝이는 BME280 센서 모듈을 아두이노 나노에 연결하고 외부 라이브러리를 사용하지 않고 데이터를 가져올 것입니다. 이를 위해 BME280 데이터 시트를 읽고, 아두이노와 BME280의 I2C 기능을 사용하는 방법을 배우고 센서를 구성하고 데이터를 보상하는 방법 등을 배울 것입니다 :)

# 어떻게 이곳에 오게 되었나요?

현대 시대의 소프트웨어 엔지니어로서, 저는 모든 것을 단순화하는 편리한 고수준 프로그래밍 언어를 사용하여 코드를 작성하는 데 많은 시간을 보냈습니다. 그러나 항상 하위 레벨에서 작동하는 방식에 대해 궁금했습니다. 그래서 아두이노에 착수하게 되었습니다 :)

<div class="content-ad"></div>

아무것도 모르는 초보자로서, 주변 장치들이 너무 많이 얽혀있어서 간단하게 하기로 결정했고, 아두이노를 사용하여(나의 경우에는 나노) 현재 온도, 기압 및 습도를 보여주는 작은 장치를 만들기로 했습니다.

당연히 센서가 필요했기 때문에, 내 목표와 잘 맞는 BME280를 발견했습니다.

연결하자마자, 진짜로 여러 라이브러리들을 찾아서 작동하게 했지만, "라이브러리 인스턴스를 생성하고 값을 사용하라"보다는 인터페이스를 조금 더 이해하고 싶었고, 그렇게 시작됐습니다.

# Wiring

<div class="content-ad"></div>

센서 모듈 자체를 자세히 살펴보고 올바르게 연결하는 방법을 이해해봅시다.

![이미지](/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_0.png)

이 모듈의 버전은 다음과 같이 4개의 핀을 가지고 있습니다:

- VIN: 양의 전원 핀입니다. 이 모듈은 5V 전원 입력을 기대합니다. BME280 센서의 공급 전압은 1.7V에서 3.6V 사이이며, 따라서 모듈 뒷면에는 662K 전압 레귤레이터가 있습니다. 이 레귤레이터는 우리의 5V를 BME280 센서에 더 적합한 3.3V로 변환해줍니다.
- GND: 음의 전원 핀으로, 접지에 연결해주세요.
- SCL: 시리얼 클럭 핀입니다. 아두이노의 해당 SCL 핀에 연결해주세요.
- SDA: 시리얼 데이터 핀입니다. 아두이노의 해당 SDA 핀에 연결해주세요.

<div class="content-ad"></div>

이제 BME280 모듈의 핀이 무엇인지 알았으니, 아두이노 나노에 연결해야 할 위치를 찾아봅시다. VIN 및 GND 핀은 직관적이며, 그들은 아두이노의 왼쪽 아래 모서리에 있을 것입니다. SDA 및 SCL은 일부 아두이노 보드에서 특별히 표시되지 않았기 때문에 찾기가 약간 까다로울 수 있습니다.

그러한 핀을 찾는 가장 좋은 방법은 당신의 보드의 공식 문서를 참조하는 것입니다. 내 경우, https://docs.arduino.cc/hardware/nano/ 는 아주 잘 작동했습니다.

이러한 핀을 연결하는 데 어떤 방법이든 사용할 수 있습니다. 나는 BME280 모듈에 핀 커넥터를 납땜하고 점퍼 와이어로 브레드보드에 연결했습니다.

<div class="content-ad"></div>

그거죠! 우리 하드웨어는 모두 연결되어 있으니, 이제 소프트웨어 부분에 집중할 차례에요.

## 초기 설정

제가 사용하고 있는 것은 Platform.io와 VSCode이며, Platform.io 확장 기능을 함께 사용하고 있어요. 하지만 Arduino IDE와 같은 다른 프로그램을 사용해도 괜찮아요.

새 프로젝트에는 자주 사용할 것으로 예상되는 2개의 기본 기능이 포함되어 있습니다.

<div class="content-ad"></div>

```js
#include <Arduino.h>

void setup() {
}

void loop() {
}
```

아두이노 프레임워크는 우리에게 I2C 장치와의 통신을 쉽게하기 위해 Wire 라이브러리를 친절히 제공합니다: https://www.arduino.cc/reference/en/language/functions/communication/wire/. 거기서 I2C 프로토콜에 대해 더 많이 읽을 수도 있습니다. SDA와 SCL 핀을 설정할 필요도 없습니다. Wire가 대신 수행해줄 것입니다:

```js
// pins_arduino.h
#define PIN_WIRE_SDA        (18)
#define PIN_WIRE_SCL        (19)

static const uint8_t SDA = PIN_WIRE_SDA;
static const uint8_t SCL = PIN_WIRE_SCL;
```

```js
// wire/src/utility/twi.c

/* 
 * Function twi_init
 * Desc     readys twi pins and sets twi bitrate
 * Input    none
 * Output   none
 */
void twi_init(void)
{
  /* ..... */
  
  // activate internal pullups for twi.
  digitalWrite(SDA, 1);
  digitalWrite(SCL, 1);

  /* ..... */
}
```

<div class="content-ad"></div>

I2C를 통해 장치와 작업을 시작하려면 Wire 라이브러리를 초기화해야 합니다.

```js
#include <Arduino.h>
#include <Wire.h>

void setup() {
  // Wire 라이브러리 초기화
  Wire.begin();
}

void loop() {
}
```

실제 메시지 전송을 시작하기 전에 해야 할 마지막 작업은 I2C 버스에서 BME280 모듈 주소를 찾는 것입니다. 일반적으로 0x76 또는 0x77입니다. 제 경우에는 0x76입니다.

BME280 데이터 시트에 따라:

<div class="content-ad"></div>

# 데이터 시트 읽기

센서로부터 실제 데이터를 읽는 흥미로운 부분에 매우 가까워지고 있어요.

하지만 먼저, 우리는 센서에 대한 가장 중요한 문서인 데이터 시트를 확인해야 합니다. 그 값을 실제로 받을 수 있는 곳과 센서 자체를 구성하는 방법을 이해하기 위해서죠. 약간의 구글링을 통해 PDF로 받을 수 있어요: https://www.mouser.com/datasheet/2/783/BST-BME280-DS002-1509607.pdf.

이 문서에는 센서, 기능, 조건, 전기 및 물리적 특성 등에 대한 많은 유용한 정보가 있어요. 알고 싶은 모든 것이 거기에 있어요.

<div class="content-ad"></div>

## 메모리 맵 및 사용 가능한 레지스터

데이터 시트에는 주소와 설명이 포함된 센서 메모리 맵이 설명되어 있습니다. 대부분을 필요로 하고 있고, 이것들을 4개의 섹션으로 그룹화해 놓았습니다.

- 녹색: 0xF7 — 0xFE — 센서 측정 값을 포함하는 8개의 연속 레지스터
- 0xF7–0xF9는 압력을 저장합니다.
- 0xFA-0xFC는 온도를 저장합니다.
- 0xFD — 0xFE는 습도를 저장합니다.
모든 값은 가장 중요한 바이트부터 가장 덜 중요한 바이트로 정렬되어 있습니다. 다음 섹션에서 전체 값의 읽는 방법을 살펴보겠습니다.
- 노랑: 0xF2, 0xF4, 0xF5 — 구성
- 0xF5는 대기 시간과 IIR 필터 설정을 조정합니다 (이번에는 마지막 비트를 무시합니다).
- 0xF4는 온도 및 압력 오버샘플링과 센서 모드를 설정합니다.
- 0xF2는 습도 오버샘플링을 설정합니다.
참고: ctrl_hum (0xF2)는 ctrl_meas (0xF4) 쓰기 작업 이후에만 유효합니다 (제 5장 제 4.3절)
- 회색: 0xD0 — 칩 ID를 저장하는 단일 바이트로, 이를 사용하여 I2C 버스에서 BME280의 존재를 확인합니다.
- 분홍: 0xE1 — 0xF0, 0x880xA1 — 보정 데이터
나중에 이 데이터를 살펴볼 것이지만, 기본적으로는 이러한 값들을 사용하여 제공된 공식을 사용해 사람이 읽을 수 있는 센서 값을 계산해야 합니다

<img src="/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_2.png" />

<div class="content-ad"></div>

데이터 시트의 4장은 레지스터와 그 안에 있는 데이터에 대한 많은 세부 정보를 제공하고 있습니다. 조금 더 깊게 파고들고 싶다면 참고하세요.

# I2C 읽기 / 쓰기

BME280 데이터 시트는 센서에 데이터를 읽거나 쓸 때 어떤 데이터를 어떤 순서로 보내야 하는지를 상세히 설명해주어 매우 친절합니다. 6장 2절을 살펴보겠습니다:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_4.png" />

간단히 말해서,

- 데이터를 쓰기:
- 쓰기 모드에서 주소를 통해 장치와 통신을 시작합니다.
- 레지스터 주소를 보냅니다.
- 레지스터에 새 값을 보냅니다.
- 전송을 중지합니다.
- 데이터를 읽기:
- 쓰기 모드에서 주소를 통해 장치와 통신을 시작합니다.
- 레지스터 주소를 보냅니다.
- 전송을 중지합니다.
- 다시 읽기 모드에서 장치와 통신을 시작합니다.
- 데이터를 읽습니다.
- 전송을 중지합니다.

# 코딩: 센서 데이터 읽기

<div class="content-ad"></div>

마침내 데이터를 읽고 구성을 작성할 준비가 되었어요!

## 연결 확인

우선, 센서가 연결되어 있고 응답하는지를 확인해봅시다:

```js
#include <Arduino.h>
#include <Wire.h>

#define BME280_ADDRESS 0x76

void setup() {
  // 일부 로그를 출력하기 위해 시리얼 설정
  Serial.begin(9600);

  // 와이어 라이브러리 초기화
  Wire.begin();

  /* ---- 센서 연결 여부 확인 ---- */
  // BME280로 전송 시작
  Wire.beginTransmission(BME280_ADDRESS);
  // 다음 요청할 레지스터 설정
  Wire.write(0xD0);
  // 전송 종료
  Wire.endTransmission();
  // BME280로부터 1바이트 요청, 이전에 설정한 0xD0부터 시작
  Wire.requestFrom(BME280_ADDRESS, 1);
  // 읽을 데이터가 있는 경우
  if (Wire.available()) {
    // I2C 버스에서 1바이트 읽기
    uint8_t chipID = Wire.read();
    // 기대값과 비교
    if (chipID != 0x60) {
      // BME280을 찾을 수 없음
      Serial.println("BME280을 찾을 수 없습니다!");
      while(1);
    } else {
      Serial.println("BME280가 준비되었습니다!");
    }
  } else {
      Serial.println("BME280를 찾을 수 없습니다!");
      while(1);
  }
}

void loop() {
}
```  

<div class="content-ad"></div>

노트: 만약 BME280를 찾을 수 없다는 메시지가 표시된다면 다음 사항을 확인해보세요:

- 연결 방식이 제대로 되었는지 확인하고, Arduino와 BME280 모듈 사이의 연결이 올바른 핀에 연결되었는지 확인하세요.
- 만약 0x76을 사용 중이라면 0x77로, 0x77을 사용 중이라면 0x76으로 주소를 변경해보세요.

성공 메시지를 받았으면 좋겠네요! 이제 I2C를 통해 기기에서 처음으로 데이터를 읽었습니다. 방금 0xD0 레지스터 값을 읽었고, 그 값을 확인했습니다. 잘 했어요!

## 센서 설정 쓰기

<div class="content-ad"></div>

다음 할 일은 우리가 원하는 설정으로 BME280을 설정하는 것입니다. 이 예제에서는 온도, 습도 및 압력에 대해 x1 오버샘플링, 1000ms 대기 시간, IIR 필터 없이 유지하겠습니다.

이를 위해 "노란 그룹"에서 3 개의 레지스터를 작성해야 합니다. 주소는 있지만 거기에 어떤 값을 넣어야 할까요?

데이터 시트의 5장 4.3, 4.5 및 4.6 섹션은 우리가 도와줍니다.

config 레지스터 0xF5에서 시작합시다.

<div class="content-ad"></div>

`<img src="/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_5.png" />`

우리는 1000ms 대기 시간과 필터 미사용을 원합니다. 상단 표에 따르면 비트 7, 6 및 5가 대기 시간을 나타내며 해당 값은 왼쪽 하단의 표에 있습니다. 1000ms는 101 조합입니다. 비트 4, 3 및 2는 필터 구성을 나타내며 끄려면 오른쪽 하단의 표에 나와 있는 a000 조합을 사용합니다. 마지막 비트는 사용하지 않을 SPI 인터페이스를 활성화합니다. 이를 0으로 설정합니다. 비트 1은 어디에도 설명되어 있지 않으므로 0으로 설정해 안전하게 무시할 수 있습니다.

따라서, 우리의 값은 101_000_0_0 (_는 가독성을 위한 것) 또는 16진수로 0xA0입니다.

다음으로, 온도 및 압력 오버샘플링 및 센서 모드, 레지스터 0xF4입니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_6.png)

구성과 마찬가지로 레지스터 값 조립해 봅시다:

- 7번 비트, 6번 비트, 5번 비트 — 온도 오버샘플링. x1을 위해 001
- 4번 비트, 3번 비트, 2번 비트 — 압력 오버샘플링. x1을 위해 001
- 1번 비트, 0번 비트 — 센서 모드. Normal을 위해 11

그러므로, 0xF4 값은 001_001_11 이 될 것이고, 이는 16진수로 0x27 입니다.


<div class="content-ad"></div>

마지막 레지스터는 습도 오버샘플링 0xF2 입니다.

![image](/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_7.png)

이것은 쉬운거에요. 바이트에서 마지막 3 비트만 필요하기 때문에 나머지는 모두 0으로 만들면 됩니다. x1 오버샘플링을 위해 00000_001, 즉 0x1으로 변환됩니다.

이제 설정에 대한 모든 값을 가졌으니 써 볼까요?

<div class="content-ad"></div>

```js
#include <Arduino.h>
#include <Wire.h>

#define BME280_ADDRESS 0x77

void writeRegister(uint8_t reg, uint8_t value);

void setup() {
  // 일부 로그를 위해 시리얼 설정
  Serial.begin(9600);

  // Wire 라이브러리 초기화
  Wire.begin();

  /* ---- 센서 존재 여부 확인 ---- */
  /* ... */

  /* ---- 센서 구성 ---- */
  // 설정 (1000ms 대기, IRR 필터 없음)
  writeRegister(0xF5, 0xA0);
  // 습도 오버샘플링 (x1) - 중요: 온도 및 압력 설정보다 먼저 가야 함
  writeRegister(0xF2, 0x1);
  // 온도 및 압력 오버샘플링 및 센서 모드 (x1/x1, 정상 모드)
  writeRegister(0xF4, 0x27);
}

void loop() {
}

// 레지스터에 값을 쓰는 도우미 함수
void writeRegister(uint8_t reg, uint8_t value) {
    Wire.beginTransmission(BME280_ADDRESS);
    Wire.write(reg);
    Wire.write(value);
    Wire.endTransmission();
}
```

습도 설정을 온도 및 압력 설정보다 먼저 해야 한다는 것을 잊지 마세요. 그렇지 않으면 데이터 시트에 명시된대로 적용되지 않습니다.

## 센서의 원시 데이터 읽기

센서 대기 시간을 1000ms로 설정했으므로, 1초마다 원시 센서 데이터를 읽기 위해 delay 함수를 사용하는 루프 함수를 만듭니다. 대부분의 로직을 중지시키는 딜레이는 최상의 방법은 아니지만, 예시로는 충분합니다. 

<div class="content-ad"></div>


![BME280 I2C Raw Values](/assets/img/2024-07-13-AbeginnersguidetoBME280I2C_8.png)

원시값을 읽으려면 레지스터 0xF7부터 0xFE까지 시작하는 8개의 연속 바이트를 가져와야합니다. 처음 3개는 압력을 나타내고, 두 번째 3개는 온도를 나타내며, 마지막 2개는 습도를 나타냅니다.

여기서 어려운 부분은 이러한 그룹을 올바른 순서로 "붙이는" 것입니다. 예를 들어 압력 설명에 따르면 0xF7이 가장 중요한 바이트(bits 19에서 12까지), 0xF8은 가장 덜 중요한 바이트(bits 11에서 4까지), 0xF9은 우리가 정말로 필요한 것은 최하위 바이트 X이며 이는 비트 4에서 0을 채우기 위한 상위 4비트뿐입니다.

따라서 코딩적인 관점으로 표현하면


<div class="content-ad"></div>

```js
uint8_t msb = 0x12;
uint8_t lsb = 0xF8;
uint8_t xlsb = 0xA3;

uint32_t result = ((uint32_t)msb << 12) | ((uint16_t)lsb << 4) | (lsxb >> 4);
```

만약 비트 연산자를 사용해 본 적이 없는 사람들에게는 이 코드가 조금 어려울 수 있으니, 2진수 관점에서 보면 시각화하기 쉬워질 것입니다.

```js
uint8_t msb = 0x12;  // 00010010
uint8_t lsb = 0xF8;  // 11111000
uint8_t xlsb = 0xA3; // 10100011

// msb를 32비트로 변환
// msb - 00000000 00000000 00000000 00010010
// msb를 12비트 왼쪽으로 이동
// msb - 00000000 00000001 00100000 00000000

// lsb를 16비트로 변환
// lsb -                   00000000 11111000
// lsb를 4비트 왼쪽으로 이동
// lsb -                   00001111 10000000

// xlsb를 4비트 오른쪽으로 이동 (마지막 4비트 제외)
// xlsb -        00001010

// 비트 OR 적용
/*
00000000 00000001 00100000 00000000  - MSB
                  00001111 10000000  - LSB
                           00001010  - XLSB

00000000 00000001 00101111 10001010
```

따라서 이동 전, 32비트인 msb는 00000000 00000000 00000000 00010010으로 보이며, 이동 후에는 00000000 00000001 00100000 00000000입니다. lsb도 마찬가지이며, xlsb는 오른쪽으로 이동하여 마지막 4비트가 손실됩니다.

<div class="content-ad"></div>

이제 데이터를 읽을 시간입니다.

```js
#include <Arduino.h>
#include <Wire.h>

#define BME280_ADDRESS 0x76

void setup() {
  /* ... */
}

void loop() {
    // 읽기를 시작할 레지스터를 설정합니다
    Wire.beginTransmission(BME280_ADDRESS);
    Wire.write(0xF7);
    Wire.endTransmission();

    // 8비트의 데이터를 요청합니다 (0xF7부터 0xFE까지)
    Wire.requestFrom(BME280_ADDRESS, 8);
    // 읽을 데이터가 있는지 확인합니다
    if (Wire.available()) {
        // 레지스터로부터 raw 데이터를 가져와서 나중에 보정하기 위해 signed long int를 사용합니다
        signed long int rawPressure = ((uint32_t)Wire.read() << 12) | ((uint16_t)Wire.read() << 4) | ((uint8_t)Wire.read() >> 4);
        signed long int rawTemperature = ((uint32_t)Wire.read() << 12) | ((uint16_t)Wire.read() << 4) | ((uint8_t)Wire.read() >> 4);
        signed long int rawHumidity = ((uint16_t)Wire.read() << 8) | Wire.read();

        Serial.print("> Raw pressure: ");
        Serial.println(rawPressure);
        Serial.print("> Raw temperature: ");
        Serial.println(rawTemperature);
        Serial.print("> Raw humidity: ");
        Serial.println(rawHumidity);
        Serial.println("----------------");
    }

    delay(1000);
}
```

시리얼에 연결하고 일부 읽기를 얻어보세요. 억만세!

```js
> Raw pressure: 361680
> Raw temperature: 531088
> Raw humidity: 26047
----------------
```

<div class="content-ad"></div>

하지만, 그들이 전혀 의미가 없다고 하죠? 네, 그 값들은 보상을 받아야 합니다. 본질적으로, BME280 데이터 시트에는 이러한 숫자들을 더 의미 있게 변환하기 위한 3가지 큰 공식이 있습니다.

## 보상

BME280 보상 공식은 4장 2.3절에서 찾을 수 있습니다. 이들은 매우 크고 복잡한 3가지 공식인데, 저는 정말로 읽기 어렵다고 생각해요.

그러나 우리가 알아야 할 것은, 이러한 공식들이 보정 값들이 필요하다는 것입니다. 다행히도, 이러한 보정 값들은 실제로 BME280 메모리(분홍색 그룹)에 저장되어 있습니다.

<div class="content-ad"></div>

우리의 측정을 보상하기 위해 보정값을 읽고 설정 단계에서 저장해 봅시다.

아래 코드에는 새로운 내용이 없습니다. 측정값과 동일한 방식으로 센서에서 값을 읽고 결합하는 것뿐입니다.

아래 이미지를 통해 주의해야 할 부분이 있습니다. 맨 아래에서 3번째 줄과 2번째 줄을 유심히 살펴보세요. 0xE5 값이 dig_H4와 dig_H5에서 모두 사용되는 점에 주목해야 합니다. 그러나 dig_H4는 3~0비트를 사용하고, dig_H5는 7~4비트를 사용합니다. 다른 값들은 간단한 비트 이동으로 결합됩니다. 순서를 유지하고 있으면 문제 없을 거에요.

<div class="content-ad"></div>

```js
#include <Arduino.h>
#include <Wire.h>

#define BME280_ADDRESS 0x76

unsigned short dig_T1, dig_P1;
short dig_T2, dig_T3, dig_P2, dig_P3, dig_P4, dig_P5, dig_P6, dig_P7, dig_P8, dig_P9, dig_H2, dig_H4, dig_H5;
unsigned char dig_H1, dig_H3;
char dig_H6;

void writeRegister(uint8_t reg, uint8_t value);

void setup() {
    // 일부 로그를 위해 시리얼 설정
    Serial.begin(9600);

    // 와이어 라이브러리 초기화
    Wire.begin();

    /* ---- 센서의 존재 확인 ---- */
    /* ... */

    /* ---- 센서 구성 ---- */
    /* ... */

    /* ---- 보정 값 읽기 ----*/
    // 보정 데이터 가져오기
    Wire.beginTransmission(BME280_ADDRESS);
    Wire.write(0x88);
    Wire.endTransmission();
    // 24바이트(12쌍)의 보정 데이터 읽기
    Wire.requestFrom(BME280_ADDRESS, 24);
    if (Wire.available()) {
        // 온도 보정
        dig_T1 = Wire.read() | ((unsigned short)Wire.read() << 8);
        dig_T2 = Wire.read() | ((short)Wire.read() << 8);
        dig_T3 = Wire.read() | ((short)Wire.read() << 8);
        // 압력 보정
        dig_P1 = Wire.read() | ((unsigned short)Wire.read() << 8);
        dig_P2 = Wire.read() | ((short)Wire.read() << 8);
        dig_P3 = Wire.read() | ((short)Wire.read() << 8);
        dig_P4 = Wire.read() | ((short)Wire.read() << 8);
        dig_P5 = Wire.read() | ((short)Wire.read() << 8);
        dig_P6 = Wire.read() | ((short)Wire.read() << 8);
        dig_P7 = Wire.read() | ((short)Wire.read() << 8);
        dig_P8 = Wire.read() | ((short)Wire.read() << 8);
        dig_P9 = Wire.read() | ((short)Wire.read() << 8);
    } else {
        Serial.println("24바이트 보정 데이터를 읽을 수 없습니다");
        while (1);
    }

    // 습도 보정
    Wire.beginTransmission(BME280_ADDRESS);
    Wire.write(0xA1);
    Wire.endTransmission();
    Wire.requestFrom(BME280_ADDRESS, 1);
    if (Wire.available()) {
        dig_H1 = (unsigned char)Wire.read();
    } else {
        Serial.println("dig_H1를 읽을 수 없습니다");
        while (1);
    }

    Wire.beginTransmission(BME280_ADDRESS);
    Wire.write(0xE1);
    Wire.endTransmission();
    Wire.requestFrom(BME280_ADDRESS, 6);
    if (Wire.available()) {
        dig_H2 = Wire.read() | ((short)Wire.read() << 8);
        dig_H3 = Wire.read();
        dig_H4 = (short)Wire.read() << 4;
        // 까다로운 부분, e5를 따로 읽습니다
        uint8_t e5 = Wire.read();
        // 그리고 마지막 4비트만 추가합니다
        // e5 & 0x0F는 첫 4비트를 0으로 설정합니다
        // 0x0F = 0b00001111
        // 예: (1111 1111) & (0000 1111) = 1111 0000
        dig_H4 |= e5 & 0x0F;
        // 그리고 H5에 대해, e5의 첫 4비트만 필요하므로 오른쪽으로 4비트 이동합니다
        dig_H5 = ((short)Wire.read() << 4) | (e5 >> 4);
        dig_H6 = Wire.read();
    } else {
        Serial.println("남은 습도 보정 값을 읽을 수 없습니다");
        while (1);
    }
}

void loop() {
    /* ... */

    delay(1000);
}

void writeRegister(uint8_t reg, uint8_t value) {
  /* ... */
}
``` 

그리고 이렇게 BME280에서 읽은 보정 값들이 많이 나왔어요! 

다음 단계는 데이터 시트에서 보정식을 복사하여 우리의 원시 측정값에 올바르게 적용하는 것만 하면 될 거예요.

```js
#include <Arduino.h>
#include <Wire.h>

#define BME280_ADDRESS 0x76

unsigned short dig_T1, dig_P1;
short dig_T2, dig_T3, dig_P2, dig_P3, dig_P4, dig_P5, dig_P6, dig_P7, dig_P8, dig_P9, dig_H2, dig_H4, dig_H5;
unsigned char dig_H1, dig_H3;
char dig_H6;

// 공유된 정밀 온도 값
signed long int t_fine;

void writeRegister(uint8_t reg, uint8_t value);

// 보상
float getTemperatureC(signed long int rawTemp);
float getPressurePA(signed long int rawPressure);
uint8_t getHumidity(signed long int rawHumidity);

void setup() {
    /* ... */
}

void loop() {
    // 읽기를 시작할 레지스터 설정
    Wire.beginTransmission(BME280_ADDRESS);
    Wire.write(0xF7);
    Wire.endTransmission();

    // 데이터 8비트 요청 (0xF7부터 0xFE까지)
    Wire.requestFrom(BME280_ADDRESS, 8);
    // 읽을 것이 있는지 확인
    if (Wire.available()) {
        // 레지스터에서 원시 데이터 가져오기, 나중에 보정하기 위해 signed long int를 사용합니다
        signed long int rawPressure = ((uint32_t)Wire.read() << 12) | ((uint16_t)Wire.read() << 4) | ((uint8_t)Wire.read() >> 4);
        signed long int rawTemperature = ((uint32_t)Wire.read() << 12) | ((uint16_t)Wire.read() << 4) | ((uint8_t)Wire.read() >> 4);
        signed long int rawHumidity = ((uint16_t)Wire.read() << 8) | Wire.read();
        
        Serial.print("> 압력 mmHg: ");
        Serial.println((int)(getPressurePA(rawPressure) * 0.00750062));
        Serial.print("> 온도 C: ");
        Serial.println(getTemperatureC(rawTemperature));
        Serial.print("> 습도 %: ");
        Serial.println(getHumidity(rawHumidity));
        Serial.println("----------------");
    }

    delay(1000);
}

/* ... */

float getTemperatureC(signed long int rawTemp) {
    signed long int var1, var2;
    var1 = ((((rawTemp >> 3) - (((signed long int)dig_T1) << 1))) * ((signed long int)dig_T2)) >> 11;
    var2 = (((rawTemp >> 4) - ((signed long int)dig_T1) * ((rawTemp >> 4) - ((signed long int)dig_T1))) >> 12) * (((signed long int)dig_T3) >> 14);
    t_fine = var1 + var2;
    return (float)((t_fine * 5 + 128) >> 8) / 100;
}

float getPressurePA(signed long int rawPressure) {
    signed long long int var1, var2, p;
    var1 = ((signed long long int)t_fine) - 128000;
    var2 = var1 * var1 * (signed long long int)dig_P6;
    var2 = var2 + ((var1 * (signed long long int)dig_P5) << 17);
    var2 = var2 + (((signed long long int)dig_P4) << 35);
    var1 = (((((signed long long int)1) << 47) + var1)) * ((signed long long int)dig_P1) >> 33;
    if (var1 == 0) {
        return 0;
    }
    p = 1048576 - rawPressure;
    p = (((p << 31) - var2) * 3125) / var1;
    var1 = (((signed long long int)dig_P9) * (p >> 13) * (p >> 13)) >> 25;
    var2 = (((signed long long int)dig_P8) * p) >> 19;
    p = ((p + var1 + var2) >> 8) + (((signed long long int)dig_P7) << 4);
    return (float)p / 256;
}

uint8_t getHumidity(signed long int rawHumidity) {
    signed long

<div class="content-ad"></div>

파일 하단에 3개의 매우 큰 보상 공식이 추가되었고, 모든 보정 값들을 사용합니다. 또한 측정치에 대한 시리얼 출력도 업데이트되어 사용합니다.

참고: getPressurePA 및 getHumidity 함수는 공유 전역 t_fine 값을 사용하므로 이들을 호출하기 전에 getTemperatureC를 호출해야 합니다.

여기에 있습니다! 사람이 읽기 쉽고 의미 있는 데이터입니다. 섭씨를 화씨로 변환하거나, 파스칼을 인치 수은柱로 변환하거나, 기호한 다른 단위로 변환할 수 있습니다.

> 압력 mmHg: 758
> 온도 C: 22.99
> 습도 %: 46
----------------

<div class="content-ad"></div>

# 결론

우리는 BME280을 사용하는 방법을 전혀 모르는 상태에서부터 온도, 습도 및 압력의 실시간 값을 읽을 수 있는 곳으로 매우 멀리 왔습니다. 더 중요한 것은 이제 데이터 시트를 읽고 해석하고, I2C 인터페이스를 사용하여 주변 장치에서 데이터를 읽고 쓰는 방법을 이해하는 데 더 많은 경험을 쌓았고, 이를 하는 과정에서 즐거움을 느낄 수 있었을 것입니다. 저는 그랬어요!

## 링크

- 최종 스케치: [링크](https://gist.github.com/paramoshkinandrew/aeacc6c0b9e6e8a330101cbdaf9342df)
- 이 기사를 위해 만든 BME280_Arduino_I2C 라이브러리: [링크](https://github.com/paramoshkinandrew/BME280_Arduino_I2C)
- BME280 데이터 시트: [링크](https://www.mouser.com/datasheet/2/783/BST-BME280-DS002-1509607.pdf)
- BME280 센서 모듈: [링크](https://www.aliexpress.us/item/3256805781410598.html)
- 아두이노 나노 3.0: [링크](https://www.aliexpress.us/item/3256805781329423.html)