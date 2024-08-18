---
title: "1,500만원 들이지 말고 직접 해보세요"
description: ""
coverImage: "/assets/img/2024-07-06-Whyspend15000ifyoucandoityourself_0.png"
date: 2024-07-06 02:54
ogImage:
  url: /assets/img/2024-07-06-Whyspend15000ifyoucandoityourself_0.png
tag: Tech
originalTitle: "Why spend 150,00€ if you can do it yourself?"
link: "https://medium.com/@francescocassano945/why-spend-200-00-if-you-can-do-it-yourself-1f8e346f46eb"
isUpdated: true
---

몇 주 전에 심슨 쇼를 보다가 홈럴이 그릴에서 고기의 익힘을 확인하는 장면을 보다가, 아마존에서 블루투스 온도계를 찾기 시작했어요.

첫 번째 모델: 100€. 음, 계속해봐야 겠네요.

두 번째 모델: 150,00€. 너무 비싸요.

150,00€을 지출하고 싶지도 못하고 할 수도 없어요. 그래서 무엇을 할까요? 저는 개발자이자 전자 기기 애호가이기 때문에 스스로 만들 수 있겠죠!

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

- 첫 번째 단계: 작년에 샀던 초보용 온도 조절 장치를 분해한 후 온도 센서를 살펴보았습니다. 이런 종류의 온도 조절 장치는 NTC 센서(음온도 계수)를 사용했습니다. 온라인 조사를 거친 후, 매우 간단한 전기 스키마를 발견했습니다. 제가 필요한 것은 센서, 저항기(10 kOhm) 그리고 전원을 제공하기 위한 배터리 뿐입니다. 하지만 이 시점에서 온도 값을 읽고 전송하기 위해 무엇을 사용해야 할까요? 계속해서 두 번째 단계로 이어갑시다.
- 두 번째 단계: 개발자에게 가장 좋은 선택은 아두이노나 더 나아가 ESP32입니다. ESP32는 내장된 BLE를 사용할 수 있어 정말 멋집니다! 시작해봅시다.
- 세 번째 단계: 사용자 인터페이스를 개발해야 합니다. 아직 언급하지 않았지만, 저는 안드로이드 개발자이기 때문에 해결책은 자명하게 제시되었습니다: ESP32와 BLE를 통해 연결되는 Android 앱입니다.

구성품:

- ESP32
- NTC 센서
- 저항기
- MP1584EN (배터리의 9V 전압을 5V로 조정하여 ESP에 공급하기 위한)

불행히도, 아직 기사를 쓸 계획은 없었고, 사진도 없습니다. 하지만 시작점은 이렇습니다:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

/assets/img/2024-07-06-Whyspend15000ifyoucandoityourself_0.png

NFC 센서는 열악재 직전에 위치해 있습니다. 이것은 고급 역공학입니다! :D

센서를 분해한 후에 회로를 제작하기 시작했고 최종 결과물은 이겁니다:

/assets/img/2024-07-06-Whyspend15000ifyoucandoityourself_1.png

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

GPIO34에 센서가 연결되어 있고 이제 온도 값을 가져올 수 있습니다:

```js
// 여러 번 샘플링하여 잡음을 평균화합니다; 더 고급 알고리즘을 사용할 수도 있습니다.
for (int i = 0; i < numSamples; i++) {
  int adcValue = analogRead(ntcPin);
  adcValueSum += adcValue;
  delay(10);
}

int adcValue = adcValueSum / numSamples;

// NTC 열 전도체를 통한 전압 계산
float voltage = adcValue * 3.3 / 4095.0;
// 온도 값을 가져옵니다
float temperature = voltage * 100;

if (deviceConnected) {
  // BLE를 통해 안드로이드 앱으로 값 전송
}
```

중요: ESP32에 연결된 NTC 센서를 보호하기 위해 유리섬유로 된 외피를 사용했습니다.

Jetpack Compose로 간단한 안드로이드 앱을 개발했습니다. 주요 기능은 ESP32와 통신하기 위한 BLE 브리지이며, 장치 검색 중에 특정 장치를 검색하도록 필터를 설정하여 ESP32-CassyMeat(네, Cassy는 나에요)와 같은 장치를 가져옵니다. 장치가 연결되면 사용자 정의 구성 요소에서 온도를 표시할 수 있습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

한 페이지가 4개의 섹션으로 나뉘어 있어요.

- 실시간으로 온도를 보여주는 사용자 지정 게이지입니다. 초록색과 빨간색 선으로 된 두 가지 한계를 추가했어요. 이 한계는 최적의 익힘 범위를 나타냅니다.
  통합 라이브러리: https://github.com/yamin8000/Gauge
- 익힘 상태를 알려주는 메시지입니다.
- 선택된 고기의 모든 다양한 온도 범위입니다.
- 사용자가 고기 종류를 선택할 수 있도록 하는 스피너입니다.

<img src="https://miro.medium.com/v2/resize:fit:1400/1*1nImVisT5pnvwxrdhzxITA.gif" />

마지막 단계로, Blender를 사용해 회로와 배터리를 넣을 케이스를 디자인하고, 3D 프린터로 출력했어요.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

/assets/img/2024-07-06-Whyspend15000ifyoucandoityourself_2.png
