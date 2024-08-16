---
title: "Arduino, PHP, MySQL로 실시간 모션 센서 데이터 수집 시스템 구축하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-BuildingaReal-TimeMotionSensorDataCollectionSystemwithArduinoPHPandMySQL_0.png"
date: 2024-08-03 20:12
ogImage: 
  url: /assets/img/2024-08-03-BuildingaReal-TimeMotionSensorDataCollectionSystemwithArduinoPHPandMySQL_0.png
tag: Tech
originalTitle: "Building a Real-Time Motion Sensor Data Collection System with Arduino, PHP, and MySQL"
link: "https://medium.com/stackademic/building-a-real-time-motion-sensor-data-collection-system-with-arduino-php-and-mysql-feaf3c6d29d2"
---


# TL;DR

이 기사는 Arduino, PHP 및 MySQL을 사용하여 실시간 모션 센서 데이터 수집 시스템을 구축하는 단계별 안내서를 제공합니다. 이 시스템은 자투리입니다. 이들 안전과 복지를 보장하기 위한 비침입적 방식을 제공합니다. 모션 감지와 실시간 데이터 전송 및 저장을 결합함으로써, 이 프로젝트는 착용 장치의 제약 사항에 대처하여 양로자와 보호자 모두에게 안심을 제공합니다.

## 소개

오늘날의 급속한 세상에서, 특히 홀로 사는 우리 노인 가족의 안전과 복지를 보장하는 것은 매우 중요합니다. 고령 인구가 계속 증가함에 따라 그들의 건강과 안전을 모니터링하기 위한 신뢰할 수 있고 효율적인 시스템에 대한 필요성도 증가합니다. 많은 모니터링 작업에 도움이되는 스마트 워치가 있지만 때때로 침실에서 사용하는 동안 사람들은 그것들을 착용하고 싶어하지 않습니다. 그들은 기기를 착용하는 느낌을 느끼고 싶지 않으며 이것이 우리 프로젝트가 중요한 역할을 하는 곳입니다.본문은 Arduino, PHP 및 MySQL을 사용하여 고령자가 홀로 살고 있는 침실에 거주하는 노인 개인을 모니터하는 데 도움이 되도록 고안된 실시간 모션 센서 데이터 수집 시스템을 구축하는 과정을 안내합니다.

<div class="content-ad"></div>

## 사용 사례: 혼자 사는 노인 개인 모니터링

혼자 살고 있는 노인을 생각해보십시오. 그들의 안전은 넘어짐, 갑작스러운 질병 또는 몇 시간 또는 몇 일 동안 알지 못할 사고로 인해 손상을 입을 수 있습니다. 전통적인 모니터링 시스템은 그들의 즉각적인 안녕을 보장하기에 필요한 실시간 데이터를 제공하지 못할 수 있습니다. 이것이 우리의 모션 센서 데이터 수집 시스템이 필요한 이유입니다.

침실에 모션 센서를 전략적으로 배치함으로써 우리는 노인 개인의 움직임을 추적하고 긴급한 안전 문제나 건강 문제를 나타낼 수 있는 장기간의 비활동과 같은 이상한 패턴을 감지할 수 있습니다. 게다가, 시스템은 노인이 비상 상황 발생 시 간병인이나 가족 구성원을 즉시 경고할 수 있는 비상 버튼을 포함하고 있습니다.

## 프로젝트 개요

<div class="content-ad"></div>

이 프로젝트는 Arduino의 움직임 감지 기능, ESP8266 Wi-Fi 모듈의 데이터 전송 기능, 서버 측 처리를 위한 PHP, 그리고 데이터 저장을 위한 MySQL을 활용합니다. 각 구성 요소가 함께 작동하여 원활한 모니터링 시스템을 구축하는 방법에 대한 간략한 개요를 제공합니다:

- PIR 센서가 장애인의 침실 내 움직임을 감지합니다. 움직임이 감지되면 Arduino가 이벤트를 기록하도록 트리거됩니다.
- Wi-Fi 모듈 (ESP8266): 기록된 데이터는 Wi-Fi를 통해 실시간으로 서버로 전송되어 가족이나 간별할 수 있습니다.
- PHP 서버: 서버에서 실행되는 PHP 스크립트가 데이터를 수신하고 처리하여 MySQL 데이터베이스에 저장합니다.
- MySQL 데이터베이스: 데이터베이스는 모든 움직임 감지 이벤트를 저장하여, 노인의 활동에서 패턴이나 비정상성을 식별하는 데 분석할 수있는 기록을 제공합니다.

## 시스템의 장점

- 실시간 모니터링: 즉각적인 움직임 감지와 로그 기록을 통해 비정상 활동이나 긴급 상황이 발생할 경우 신속하게 대응할 수 있습니다.
- 향상된 안전성: 긴급 버튼은 노인이 간별에 즉시 근무자를 통지할 수 있는 추가적인 안전 기능을 제공합니다.
- 데이터 분석: 과거 데이터를 분석하여 시간이 지남에 따른 움직임 감소 등의 경향을 식별할 수 있으며, 이는 건강 문제를 나타낼 수 있습니다.
- 마음의 평화: 가족 및 간별꾼은 자신들이 담담히 사랑하는 이의 복지를 원격으로 모니터링할 수 있음을 알고 마음의 안정을 느낄 수 있습니다.

<div class="content-ad"></div>

## 필요한 구성품 및 도구

이 시스템을 구축하기 위해서는 다음의 하드웨어 및 소프트웨어 구성요소가 필요합니다:

하드웨어 구성품

- 아두이노 보드 (예: 아두이노 우노)
- PIR 센서
- ESP8266 Wi-Fi 모듈
- 브레드보드 및 점퍼 와이어

<div class="content-ad"></div>

소프트웨어 도구

- 아두이노 IDE
- PHP
- MySQL
- 웹 서버 (예: Apache, XAMPP)
- 텍스트 편집기 (예: VSCode, Sublime Text)

## 하드웨어 설정

회로도


<div class="content-ad"></div>

다음과 같이 PIR 센서와 ESP8266 모듈을 아두이노에 연결해보세요:

PIR 센서:

- VCC를 5V에 연결합니다.
- GND를 GND에 연결합니다.
- OUT를 디지턀 핀 D1에 연결합니다.

ESP8266 Wi-Fi 모듈:

<div class="content-ad"></div>

- VCC를 3.3V로 연결해 주세요.
- GND를 GND에 연결해 주세요.
- TX를 RX에 연결해 주세요.
- RX를 TX에 연결해 주세요.

단계별 안내

- 빵판에 구성 요소를 조립해 주세요.
- 느슨한 연결을 방지하기 위해 모든 연결이 안전하게 되어 있는지 확인해 주세요.

## 동작 감지용 아두이노 코드

<div class="content-ad"></div>

다음 아두이노 코드는 PIR 센서를 사용하여 움직임을 감지하고 ESP8266 모듈을 통해 PHP 서버로 데이터를 전송합니다:

```js
#include <ESP8266WiFi.h>

// Wi-Fi 정보
const char* ssid = "여러분의_SSID";
const char* password = "여러분의_비밀번호";

// 서버 정보
const char* server = "여러분의_서버_주소"; // 예: "192.168.1.100"

// 핀 정의
int motionPin = D1; // PIR 센서용 GPIO 핀

// 간격 설정
unsigned long lastMotionTime = 0; // 마지막으로 움직임을 감지한 시간
const unsigned long interval = 5000; // 녹화 사이의 최소 간격(밀리초 단위, 5초)

void setup() {
  Serial.begin(115200);
  pinMode(motionPin, INPUT);

  // Wi-Fi 연결
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Wi-Fi에 연결 중...");
  }
  Serial.println("Wi-Fi에 연결되었습니다");
}

void loop() {
  unsigned long currentTime = millis();
  int motionState = digitalRead(motionPin);

  if (motionState == HIGH && (currentTime - lastMotionTime > interval)) {
    sendToServer("움직임 감지됨", "sensor_01");
    lastMotionTime = currentTime;
  }

  delay(100); // 100밀리초마다 센서 상태 확인
}

void sendToServer(String message, String sensor_id) {
  WiFiClient client;
  if (client.connect(server, 80)) {
    String postData = "sensor_id=" + sensor_id + "&data=" + message;
    client.println("POST /save_data.php HTTP/1.1");
    client.println("Host: " + String(server));
    client.println("Content-Type: application/x-www-form-urlencoded");
    client.print("Content-Length: ");
    client.println(postData.length());
    client.println();
    client.print(postData);
    client.stop();
  }
}
```

## MySQL 데이터베이스 설정

MySQL 데이터베이스를 생성하고 센서 데이터를 저장할 테이블을 만듭니다:


<div class="content-ad"></div>

```sql
CREATE DATABASE sensor_data_db;

USE sensor_data_db;

CREATE TABLE sensor_data (
  id INT AUTO_INCREMENT PRIMARY KEY,
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  sensor_id VARCHAR(50),
  data VARCHAR(255)
);
```

## PHP 스크립트 만들기

들어오는 데이터를 처리하고 MySQL 데이터베이스에 저장하는 PHP 스크립트(save_data.php)를 만듭니다.

PHP 스크립트 (save_data.php)

<div class="content-ad"></div>

```js
<?php
$servername = "localhost";
$username = "your_username";
$password = "your_password";
$dbname = "sensor_data_db";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $sensor_id = $_POST['sensor_id'];
  $data = $_POST['data'];

  $sql = "INSERT INTO sensor_data (sensor_id, data) VALUES ('$sensor_id', '$data')";

  if ($conn->query($sql) === TRUE) {
    echo "New record created successfully";
  } else {
    echo "Error: " . $sql . "<br>" . $conn->error;
  }
} else {
  echo "Invalid request method";
}

$conn->close();
?>
```

## 아두이노와 PHP 서버 통합하기

아두이노 코드를 업데이트하여 PHP 서버와 통신하도록 설정하세요. 아두이노 섹션에 제공된 코드는 이미 이 작업을 수행하며, 모션 감지 데이터를 PHP 스크립트(save_data.php)로 전송합니다.

## 테스트 및 문제 해결하기


<div class="content-ad"></div>

설정 테스트 절차

- 하드웨어 테스트: PIR 센서가 움직임을 감지하는지 확인하기 위해 아두이노 IDE의 시리얼 모니터 출력을 관찰합니다.
- Wi-Fi 연결 테스트: ESP8266 모듈이 Wi-Fi 네트워크에 성공적으로 연결되었는지 확인합니다.
- 데이터 전송 테스트: PHP 서버로 데이터를 보내는 아두이노가 PHP 스크립트의 출력을 관찰하여 확인합니다.
- 데이터베이스 테스트: MySQL 데이터베이스를 확인하여 데이터가 올바르게 저장되고 있는지 확인합니다.

일반적인 문제 및 문제 해결 팁

- Wi-Fi 연결 없음: SSID와 암호를 확인하고 ESP8266 모듈이 올바르게 연결되어 있는지 확인합니다.
- 데이터베이스에 데이터 없음: PHP 스크립트가 실행 중이고 데이터베이스 연결 세부 정보가 올바른지 확인합니다.
- 센서가 움직임을 감지하지 않음: PIR 센서가 올바르게 연결되어 있는지 확인하고 작동하는지 확인합니다.

<div class="content-ad"></div>

## 응용 및 추가 개선 사항

잠재적인 응용 분야

- 홈 보안: 제한된 지역으로의 무단 진입을 감시합니다.
- 노인 돌봄: 홀로 생활하는 노인들의 움직임과 움직임이 없음을 감시합니다.
- 산업 모니터링: 시설의 민감한 지역에서의 움직임을 추적합니다.

추가 기능 아이디어

<div class="content-ad"></div>

- 추가 센서: 온도, 습도 등의 센서를 더 추가하여 모니터링 능력을 향상시킵니다.
- 모바일 알림: 보호자나 가족 구성원에게 SMS나 앱 알림을 보낼 수 있는 시스템을 구현합니다.
- 데이터 시각화: 데이터를 시각화하고 활동 패턴에 대한 통찰을 제공하는 웹 대시보드를 생성합니다.

![이미지](/assets/img/2024-08-03-BuildingaReal-TimeMotionSensorDataCollectionSystemwithArduinoPHPandMySQL_0.png)

# 실제 행동 및 샘플 데이터 수집

실제 시나리오에서 시스템이 어떻게 작동하는지 살펴보기 위해 혼자 살고 있는 노인의 침실을 하루동안 모니터링하는 일상적인 상황을 고려해보겠습니다. 센서가 감지한 움직임과 비상 버튼 누름을 추적할 것입니다. 시스템이 하루 내내 데이터를 어떻게 캡처하고 저장할지 살펴봅니다.

<div class="content-ad"></div>

## 현실적인 상황

아침 활동:

- 어르신은 아침 7시에 일어나 침실 주변을 돌아다닙니다.
- 그들은 옷갈아입기, 침대 정리하기, 창문 열기 등의 루틴 활동을 수행합니다.

오후 활동:

<div class="content-ad"></div>

- 2:00 PM부터 3:00 PM까지 침대에서 휴식을 취하며 중요한 움직임이 감지되지 않았습니다.
- 휴식 후, 물을 가져 오고 가벼운 운동을 위해 움직입니다.

저녁 활동:

- 해당 개인은 8:00 PM에 몸이 불편하여 비상 버튼을 누릅니다.

# 수집된 샘플 데이터

<div class="content-ad"></div>

가정해 봅시다. PIR 센서가 레코딩 사이의 최소 간격이 5초로 구성되어 움직임을 감지하도록 되어 있다고 가정해 봅시다. 이 날 수집된 데이터의 샘플 로그는 다음과 같습니다:

![이미지](/assets/img/2024-08-03-BuildingaReal-TimeMotionSensorDataCollectionSystemwithArduinoPHPandMySQL_1.png)

# 샘플 데이터 설명

- 오전 7:00:05: 개인이 일어나면서 침실 주변을 움직이기 시작합니다. 시스템은 이 초기 움직임을 기록합니다.
- 오전 7:00:12: 아침 루틴을 계속하며 더 많은 움직임이 감지됩니다.
- 오전 7:15:00: 침실 주변을 움직이는 동안 계속된 활동이 기록됩니다.
- 오전 7:30:00: 아침 업무를 수행하는 동안 다른 움직임이 감지됩니다.
- 오전 7:45:00: 더 많은 움직임이 감지되며, 일상적인 활동을 나타낼 수 있습니다.
- 오후 2:00:05: 휴식에서 깨어나 개인이 움직임을 감지합니다.
- 오후 2:15:00: 물을 가져오거나 가벼운 운동을 하는 것으로 추정되는 가벼운 움직임이 감지됩니다.
- 오후 3:00:05: 개인이 오후 활동을 계속하면서 더 많은 움직임이 감지됩니다.
- 오후 3:15:00: 계속된 가벼운 운동 또는 주변을 이동하는 것으로 추정됩니다.
- 오후 8:00:00: 개인이 안정하지 않다고 느껴 비상 버튼을 누릅니다. 시스템은 즉시 이 이벤트를 기록합니다.
- 오후 8:05:00: 보다 편안한 자세를 취하거나 지원을 받기 위해 이동하는 것으로 추정되는 추가적인 움직임이 감지됩니다.
- 오후 8:15:00: 계속된 활동은 개인이 여전히 활동 중이거나 침대에 준비 중임을 나타냅니다.

<div class="content-ad"></div>

# 데이터 통합과 실생활 시나리오

수집된 데이터는 일상 생활 및 비정상적인 활동 또는 긴급 상황에 대한 소중한 통찰력을 제공합니다. 예를 들어:

- 정기적인 감지 모니터링: 규칙적인 움직임 감지 로그를 통해 요양사들은 고령 개인의 일상 생활을 이해하고 활발하고 건강한지 확인할 수 있습니다.
- 불규칙성 식별: 움직임 감지의 간격은 휴식 기간을 나타낼 수 있으나, 길게 지속되는 비활동은 주의가 필요한 잠재적 문제를 시사할 수 있습니다.
- 긴급 대응: 긴급 버튼 누름이 즉시 로깅되어 요양사가 빠르게 대응하여 개인의 안전을 보장할 수 있습니다.

# 결론

<div class="content-ad"></div>

이 프로젝트는 혼자 사는 노인을 모니터링하고, 필요할 때 적시에 개입할 수 있는 효율적이고 신뢰할 수 있는 시스템을 만드는 방법을 보여줍니다. 이 안내를 따라 Arduino, PHP 및 MySQL을 사용하여 실시간 모션 센서 데이터 수집 시스템을 구축하여 가족과 간병인에게 안심감을 제공할 수 있습니다.

## 부록

完整한 아두이노 코드

```js
#include <ESP8266WiFi.h>

// 와이파이 자격 증명
const char* ssid = "여러분의_SSID";
const char* password = "여러분의_비밀번호";

// 서버 세부 정보
const char* server = "여러분의_서버_주소"; // 예: "192.168.1.100"

// 핀 정의
int motionPin = D1; // PIR 센서용 GPIO 핀

// 간격 설정
unsigned long lastMotionTime = 0; // 마지막 모션 감지 시간
const unsigned long interval = 5000; // 레코딩 간의 최소 간격(밀리초) (5초)

void setup() {
  Serial.begin(115200);
  pinMode(motionPin, INPUT);

  // Wi-Fi에 연결
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Wi-Fi에 연결 중...");
  }
  Serial.println("Wi-Fi에 연결되었습니다");
}

void loop() {
  unsigned long currentTime = millis();
  int motionState = digitalRead(motionPin);

  if (motionState == HIGH && (currentTime - lastMotionTime > interval)) {
    sendToServer("모션 감지됨", "sensor_01");
    lastMotionTime = currentTime;
  }

  delay(100); // 100밀리초 마다 센서 상태 확인
}

void sendToServer(String message, String sensor_id) {
  WiFiClient client;
  if (client.connect(server, 80)) {
    String postData = "sensor_id=" + sensor_id + "&data=" + message;
    client.println("POST /save_data.php HTTP/1.1");
    client.println("Host: " + String(server));
    client.println("Content-Type: application/x-www-form-urlencoded");
    client.print("Content-Length: ");
    client.println(postData.length());
    client.println();
    client.print(postData);
    client.stop();
  }
}
```

<div class="content-ad"></div>

PHP 스크립트 전체

```js
<?php
$servername = "localhost";
$username = "your_username";
$password = "your_password";
$dbname = "sensor_data_db";

// 연결 생성
$conn = new mysqli($servername, $username, $password, $dbname);

// 연결 확인
if ($conn->connect_error) {
  die("연결 실패: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $sensor_id = $_POST['sensor_id'];
  $data = $_POST['data'];

  $sql = "INSERT INTO sensor_data (sensor_id, data) VALUES ('$sensor_id', '$data')";

  if ($conn->query($sql) === TRUE) {
    echo "새 레코드가 성공적으로 생성되었습니다";
  } else {
    echo "오류 발생: " . $sql . "<br>" . $conn->error;
  }
} else {
  echo "유효하지 않은 요청 방식";
}

$conn->close();
?>
```

데이터베이스 설정을 위한 SQL 스크립트

```js
CREATE DATABASE sensor_data_db;

USE sensor_data_db;

CREATE TABLE sensor_data (
  id INT AUTO_INCREMENT PRIMARY KEY,
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  sensor_id VARCHAR(50),
  data VARCHAR(255)
);
```

<div class="content-ad"></div>

ข้อความสรุปคือการพัฒนาระบบตรวจจับการเคลื่อนไหวแบบเรียลไทม์สำหรับผู้สูงอายุโดยใช้ Arduino, PHP, และ MySQL
Revision 1.0 / กรกฎาคม 2567 — ลอจิกเบเกอร์ และ Stackademic 🎓

<div class="content-ad"></div>

제공해주신 정보를 끝까지 읽어주셔서 감사합니다. 떠나시기 전에요:

- 작가를 응원하고 팔로우해주세요! 👏
- 다양한 플랫폼 소식 받으려면 X, LinkedIn, YouTube, Discord를 팔로우해주세요.
- 다른 플랫폼에서도 저희를 만나보세요: In Plain English, CoFeed, Differ.
- Stackademic.com에서 더 많은 콘텐츠를 만나보실 수 있습니다.