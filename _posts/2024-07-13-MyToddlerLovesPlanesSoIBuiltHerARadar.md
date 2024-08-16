---
title: "비행기를 좋아하는 우리 아이를 위해 만든 레이더 시스템 구축 과정"
description: ""
coverImage: "/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_0.png"
date: 2024-07-13 01:28
ogImage: 
  url: /assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_0.png
tag: Tech
originalTitle: "My Toddler Loves Planes, So I Built Her A Radar"
link: "https://medium.com/gitconnected/my-toddler-loves-planes-so-i-built-her-a-radar-52f4f4534818"
isUpdated: true
---





이 글은 영감의 이야기이자 비전이 형태를 갖는 과정, 그리고 가장 성급한 평가자인 2세 소녀의 신속한 피드백을 담은 이야기입니다.

최종 제품으로 빨리 이동하고 싶으시다면 지금 바로 애플 앱 스토어에서 Aviator — Radar on your Phone 앱을 다운로드해보세요!

# 영감을 주는

<div class="content-ad"></div>

토드러를 이번 여름에 해외로 데리고 갔어요.

정말 기대했더라구요. 근데, 3시간이나 되는 비행을 버틸 수 있도록 하기 위해서 저희 부부는 비행기 여행을 엄청나게 기대하게 만들어줬어요. 그래서 우리집에서 곧바로 비행기에 탈 줄 알았던 딸아이가 공항까지 가는데 택시에 타야 한다는 것에 놀랐지요.

한 번 비행기에 타면, 정말 놀라운 일이 벌어졌어요 — 만약 승무원들이 비행기에 관심이 많은 귀여운 토드러와 함께 보면, 조종실을 구경할 수 있게 초대를 받을 수 있다는 거예요.

그래서 이 일로 딸아이는 비행기에 대한 집착이 더해졌어요. 하늘에서 비행기를 찾아달라고 귀엽게 요청하고, 제가 찾아주면 기뻐해서 환하게 웃는답니다.✈️

<div class="content-ad"></div>

### 문제 상황이 드러나다

지난 주, 저는 딸을 어깨에 업고 정원에서 한 시간을 보냈어요. 저희는 저녁 하늘에 반짝이는 비행기들을 발견했어요. 하나 또 하나.

딸과 놀기는 항상 좋지만, 더 효율적인 방법을 적용할 수 있다는 것을 알았어요.

저는 FlightRadar24를 찾았어요. 이 사이트는 지도 위에 비행기의 위치를 표시해줘요. 꽤 잘 작동했지만, 눈으로 찾아야 하는 비행기 위치를 확인하기가 조금 번거로웠어요.

<div class="content-ad"></div>

![Plane](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_1.png)

요세는 비행기를 찾는 게 꽤 어려워요. 런던시티공항에서 막 이륙한 에어버스와 4만 피트에서 비행 중인 리어젯이 그림자 형태로 모습을 드러내기 때문이죠. 하지만 실제 하늘에서는 큰 비행기를 찾기가 훨씬 수월해요.

마지막으로, 그리고 가장 중요한 건요, 제 아이는 지도가 뭔지 이해하거나 관심조차 없어요. 그녀가 원한 건 그냥 비행기 보는 거였어요.

그러니 우리는 여전히 문제를 안고 있어요.

<div class="content-ad"></div>

오리엔테이션.

사이징.

사용성.

아피지아 모바일 기술 리드로써, 제가 어린이에게 마음에 드는 앱을 만들어 주는 것을 막을 수 있는 것은 없었습니다. 하지만 우리 아이에게 말려하는 말대로, 제 딸을 위해 웅장한 앱을 만들기 시작하기에는 어디서부터 시작해야 할지도 모르겠다고 생각했었죠.

<div class="content-ad"></div>

## 비전이 형성되어 가는 중이에요

우리 앱에 대한 아이디어가 있었어요:

우리가 연구를 통해 만들어냈던 요구 사항을 유지하면서:

- 앱은 계속해서 올바른 방향으로 유지되어야 해요. 기기와 함께 회전해 비행기를 올바른 방향으로 나타내야 해요.
- 비행기를 비행 높이에 따라 크기가 커지거나 작아져야 해요.
- 앱은 재미있어야 하고, 진지한 비즈니스 앱보다는 오히려 레트로 아동용 장난감 같이 느껴져야 해요.

<div class="content-ad"></div>

이러한 요구 사항은 컨셉 증명의 일부를 형성하는 여러 이동 부분을 유발했습니다:

- 방향 유지는 기존 솔루션에 없는 핵심 차별화 제품 요구 사항입니다. 상세한 비행 정보를 다루는 일은 아니라서요. 우리는 멋진 레이더를 만들고 싶을 뿐입니다! iOS Core Location API가 제공하는 콜백 델리게이트를 통해 사용자가 장치를 다시 조정할 때마다 우리를 지원해줍니다.
- 물론 가장 중요한 구성 요소는 비행 데이터 API입니다. OpenSky Network가 필요한 바로 그것을 보유하고 있습니다. 비상업적 용도로 무료인 간단한 REST API가 지역 내 항공편의 실시간 데이터를 제공합니다. 현실적인 레이더 스윕을 위해 몇 초마다 이 엔드포인트에 ping을 보내고 싶을 것입니다.
- API를 호출하기 위해서는 위치 데이터가 필요합니다. Core Location은 다시 한 번 우리를 도와줍니다. 사용자의 위치에서 위도의 +/- 1도를 쿼리할 수 있어 사용자의 위치가 충분히 은폐되도록 하기 위해 0.1도 (약 10km)의 정밀도로 설정할 수 있습니다. 또한 세션 당 이 데이터를 한 번만 가져오면 됩니다.
- 마지막으로, 가장 어려운 부분으로, 비행 위치 데이터를 우리 자신의 위치와 지향된 좌표와 비교하기 위해 삼각법 기술을 다듬어야 합니다. 이렇게 하면 하늘에서 우리에게 상대적인 위치에 따라 화면에 인접 항공기를 올바른 위치에 그릴 수 있게 됩니다.

이 앱을 기반으로 비즈니스를 구축할 계획이 없으므로(다시 말해, OpenSky Network API는 비상업적 용도로만 제한됨), SwiftUI를 위한 간단한 MV 아키텍처를 사용할 것입니다. 비즈니스 로직은 약간의 뷰에 남겨두고, SwiftUI의 내장 API를 신뢰하며, API 및 위치와 같은 핵심 서비스를 분리할 것입니다.

컨셉을 증명한 후에는 정말 재미있는 부분인 멋진 레이더로 만들고 제 아이와 함께 테스트할 수 있는 작업에 착수할 수 있을 것입니다!

<div class="content-ad"></div>

# Concept Validation

먼저 중요한 것이 있습니다.

마스코트로는 귀여운 비행사 모자를 쓴 딸의 만화를 상상하고 있습니다. 그러니 우리 앱의 이름은 이미 정해졌군요: Aviator.

끝없는 의지력을 발휘해서, MVP가 완성될 때까지 앱 아이콘에 시간 낭비는 하지 않을 거예요. 하지만 시작할 프로젝트명은 이제 갖추게 되었습니다.

<div class="content-ad"></div>

## 오리엔테이션

내 주요 차별화 제품 요구사항 중 첫 번째는 오리엔테이션 유지입니다. 화면의 객체들이 실제 위치와 일치해야 유용하게 사용될 수 있습니다. 그래서 사용자가 회전할 때 화면 자체가 회전하여 북쪽을 계속 가리켜야 합니다.

지금은 AviatorApp 및 ContentView의 템플릿 파일을 무시하고, LocationManager 싱글톤을 만들고 CLLocationManagerDelegate의 didUpdateHeading 메서드를 연결합니다.

내 LocationManager는 초기 설정에서 위치 권한 요청, 델리게이트 설정, 그리고 Core Location에 오리엔테이션 정보 전송 시작을 처리합니다.

<div class="content-ad"></div>

```swift
final class LocationManager: CLLocationManager, CLLocationManagerDelegate {
    
    static let shared = LocationManager()
    
    private(set) var rotationAngleSubject = CurrentValueSubject<Double, Never>(0)
    
    override init() {
        super.init()
        requestWhenInUseAuthorization()
        delegate = self
        startUpdatingHeading()
    }
    
    func locationManager(_ manager: CLLocationManager, didUpdateHeading newHeading: CLHeading) {
        rotationAngleSubject.send(-newHeading.magneticHeading)
    }
}

SwiftUI 뷰에서 잘 동작하도록 하려면, Combine publisher를 통해 orientation 정보를 보내야 해요. rotationAngleSubject를 이용해서, 뷰 내에서 .onReceive로 반응적으로 처리하고, 로컬 @State 속성 rotationAngle을 설정할 수 있어요.  

뷰 내에서 멋진 나침반 효과를 얻기 위해, rotationAngle과 함께 변하는 직사각형들을 그리고 있어요.

@State private var rotationAngle: Angle = .degrees(0)

var body: some View {
    ZStack {
        ForEach(0..<36) {
            let angle = Angle.degrees(Double($0 * 10)) + rotationAngle
            Rectangle()
                .frame(width: $0 == 0 ? 16 : 8, height: $0 == 0 ? 3 : 2)
                .foregroundColor($0 == 0 ? .red : .blue)
                .rotationEffect(angle)
                .offset(x: 120 * cos(CGFloat(angle.radians)), y: 120 * sin(CGFloat(angle.radians)))
                .animation(.bouncy, value: rotationAngle)
        }
    }
    .onReceive(LocationManager.shared.rotationAngleSubject) { angle in
        rotationAngle = Angle.degrees(angle)
    }
}

<div class="content-ad"></div>

내 장치에서 테스트해본 결과 정말 좋아 보이고 실제 위치에 완벽하게 반응합니다!

![Alt text](https://miro.medium.com/v2/resize:fit:1200/1*0D7gEYaGnD8Y2gfDXOLE4Q.gif)

참 샬라한 시각적인 버그가 있는데, 애니메이션 로직이 0도와 360도를 별개의 숫자로 처리해 웃긴 일이 벌어졌어요. 진북쪽을 지나가면 모든 직사각형이 회전한다는 거죠. 하지만 이건 PoC에 대해 문제 없죠. (사실 이 UI를 실제로 유지할 일이 별로 없을 겁니다.)

## 비행 데이터 API

<div class="content-ad"></div>

이제 워밍업이 끝났어요.

이제 가장 중요한 부분이에요: OpenSky Network API에서 데이터를 분석하는 것.

해당 API를 통해 위도와 경도의 범위를 지정하고, 해당 범위 내의 항공편 배열을 반환받을 수 있습니다. 간단한 GET 요청을 통해 이를 확인할 수 있어요. 브라우저에 아래 내용을 붙여넣기만 하면 지금 저의 위쪽에서 무슨 항공편이 비행 중인지 확인할 수 있어요:

https://opensky-network.org/api/states/all?lamin=51.0&lamax=52.0&lomin=-0.5&lomax=0.5

<div class="content-ad"></div>

REST API는 잘 문서화되어 있지만, 키가 없는 구조로 되어 있어요. 데이터가 순차적으로 나열된 속성 목록으로 제공돼요.

![이미지](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_2.png)

이것을 디코드하기 위해 UnkeyedContainer를 사용해야 해요. 이는 JSON 응답에서 필드를 순서대로 파싱하기 위해 설계된 것이에요.

struct Flight: Decodable {

    let icao24: String 
    let callsign: String?
    let origin_country: String? 
    let time_position: Int?
    let last_contact: Int
    let longitude: Double
    let latitude: Double

    // ... 

    init(from decoder: Decoder) throws {
        var container = try decoder.unkeyedContainer()
        icao24 = try container.decode(String.self)
        callsign = try? container.decode(String?.self)
        origin_country = try container.decode(String.self)
        time_position = try? container.decode(Int?.self)
        last_contact = try container.decode(Int.self)
        longitude = try container.decode(Double.self)
        latitude = try container.decode(Double.self)

        // ...
    }
}

<div class="content-ad"></div>

사용자의 위치 좌표를 기반으로 하는 GET 요청을 수행하는 간단한 API를 작성할 수 있어요.

final class FlightAPI {
    
    func fetchLocalFlightData(coordinate: CLLocationCoordinate2D) async throws -> [Flight] {
        
        let lamin = String(format: "%.1f", coordinate.latitude - 0.25)
        let lamax = String(format: "%.1f", coordinate.latitude + 0.25)
        let lomin = String(format: "%.1f", coordinate.longitude - 0.5)
        let lomax = String(format: "%.1f", coordinate.longitude + 0.5)

        let url = URL(string: "https://opensky-network.org/api/states/all?lamin=\(lamin)&lamax=\(lamax)&lomin=\(lomin)&lomax=\(lomax)")!
        let data = try await URLSession.shared.data(from: url).0
        return try JSONDecoder().decode([Flight].self, from: data)
    }
}

이제 한 발 나아가고 있어요!

항공 데이터는 메모리 내 Flight 객체의 배열로 아주 잘 파싱되었어요. 이제 이를 간단하게 다룰 수 있죠.

<div class="content-ad"></div>

## 비행기 그리기

내 LocationManager를 수정하여 중요한 위치 변경을 감지하고 이러한 좌표를 퍼블리셔를 통해 전송하는 것은 매우 사소한 작업이죠.

다시 한 번, 순수한 MV 아키텍처 스타일로, 내 뷰는 .onReceive를 통해 좌표를 수신하고 이러한 좌표로 내 새로운 FlightAPI를 호출합니다. 결과는 무엇일까요? 당신의 지역 하늘에서 오르내리는 비행기에 관한 데이터죠.

이제, 초기 컨셉 증명에서 가장 어려운 부분에 도달했습니다: 실제로 비행기 아이콘을 나의 위치와 비교하여 올바른 위치에 표시하는 것입니다.

<div class="content-ad"></div>

나의 첫 번째 시도는 직선적인 방법이었어: 상대적인 위도와 경도를 화면에 표시된 점수 값으로 고정하여 곱했어.

@State private var coordinates: CLLocationCoordinate2D?
@State private var flights: [Flight] = []

var airplanes: some View {
    ForEach(flights, id: \.icao24) { flight in
        let latDiff = coordinate.latitude - (flight.latitude ?? 0)
        let lngDiff = coordinate.longitude - (flight.longitude ?? 0)
        Image(systemName: "airplane")
            .resizable()
            .frame(width: 20, height: 20)
            .rotationEffect(.degrees(flight.true_track ?? 0))
            .foregroundColor(.red)
            .offset(x: 250 * latDiff, y: 250 * lngDiff)
    }
}

이 방법으로는 정확하지 않을 거라는 건 당연한데, 왜냐하면 위도나 경도 한 단위의 절대 거리는 지리적 위치에 따라 달라지기 때문이지. 하지만 그래도 시작하기엔 좋은 곳이었어.

## 초기 결과

<div class="content-ad"></div>

비행기 그림의 정확성을 실제로 테스트하는 방법은 무엇인가요?

모든 것 아래에 지도를 그릴 수 있어요!

이제 AviatorView에는 3개의 레이어가 있어요: 맨 위에 나침반, 화면에 그려진 비행기들, 그리고 그 모든 것 아래에 있는 꾸밈이 없는 SwiftUI 지도 뷰가 있어요.

@State private var cameraPosition: MapCameraPosition = .camera(MapCamera(
        centerCoordinate: CLLocationCoordinate2D(latitude: 51.0, longitude: 0.0),
        distance: 100_000,
        heading: 0))

var body: some View {
    ZStack {
        Markdown(position: $cameraPosition) { } 
        airplanes
        compass
    }
}

<div class="content-ad"></div>

내 첫 심야 해커톤 결과물과 FlightRadar의 예측을 비교해봤어. 정말 흥미로운 걸 발견했어. 하늘에서 비행기의 수와 집합이 대체적으로 맞는 데 위치는 상당히 틀리네.

갑자기, 또 다른 영감의 백미. 너무 간단하지만, 전에는 생각을 못했네.

맵에 비행기를 주석을 이용해 그려야겠어!

<div class="content-ad"></div>

# The MVP

하루 종일 생각해온 아이디어가 있어요: 지도를 사용해 항공기 모양의 주석을 정확한 지리 위치에 그릴 거에요.

언젠가는 실제 지도를 숨기고, 레이더 위치에 마커로서 항공기만 표시하는 방법을 찾고 싶어요. 이렇게 하면 우리가 추구하는 멋진, 완전히 지향된 레이더 효과를 얻을 수 있을 거예요.

## 지도 주석

<div class="content-ad"></div>

iOS 17에 나오는 기능 중에는 맵에 주석을 그리는 것이 쉽다고 하네요. FlightMapView를 리팩토링해보는 건 어떨까요?

import MapKit
import SwiftUI

struct FlightMapView: View {
    
    @Binding var cameraPosition: MapCameraPosition
    
    let flights: [Flight]

    var body: some View {
        Map(position: $cameraPosition) {
            planeMapAnnotations
        }
        .mapStyle(.imagery)
        .allowsHitTesting(false)
    }
}

여기서 레이더 용도로 맵 위의 상호작용을 막고 싶다면 `.allowsHitTesting(false)`를 사용하면 된답니다. 이렇게 하면 사용자는 비행기와 위치만 보고 맵은 보이지 않게 됩니다.

## 비행기 크기 결정

<div class="content-ad"></div>

오리엔테이션 이후, 사이징은 기존 솔루션에서 처리되지 못한 핵심 문제였습니다.

비행 고도를 사용하여 맵 주석에 간단한 로그 스케일링을 추가하여 고도가 더 높은 항공기가 화면 상에서 더 크게 나타나도록 했습니다.

게다가 사용자의 Core Location에서 가져온 오리엔테이션과 항공기의 true_track 속성을 결합하여 항공기가 올바른 방향을 향하도록 했습니다.

@State private var rotationAngle: Angle = .degrees(0)

private var planeMapAnnotations: some MapContent {
    ForEach(flights, id: \.icao24) { flight in
        Annotation(flight.icao24, coordinate: flight.coordinate) {
            let rotation = rotationAngle.degrees + flight.true_track
            let scale = min(2, max(log10(height + 1), 0.5))
            Image(systemName: "airplane")
                .rotationEffect(.degrees(rotation))
                .scaleEffect(scale)
        }
        .tint(.white)
    }
}

<div class="content-ad"></div>

## 사용자 연구

지금은 제 MVP가 실제로 작동하는지 알아내는 궁극적인 시험할 때입니다.

저는 딸과 함께 비행기를 관찰하러 갈 것입니다.

우리는 실제 지도 주석을 갖고 있으며 지도상에서 사용자의 위치와 방향을 표시합니다.

<div class="content-ad"></div>

가장 중요한 것은 비행기를 정확하게 찾아낸다는 것이죠!

MVP는 대성공을 거뒀어요. 딸과 함께 앱에서 확인할 수 있는 비행기를 발견했답니다!

이번 초기 테스트에서는 두 가지 중요한 정보를 얻을 수 있었어요.

첫째, 제 스케일링 논리가 역으로 되어 있다는 것이에요. 런던 시티 공항에 있는 작은 비행기를 보세요. 앱의 목적이 하늘에서 비행기를 찾는 것이기 때문에 스케일링을 역전시켜야 해요. 높이에 있는 비행기는 우리 눈으로 발견하기 쉬워야 하니까 더 커 보여야 합니다.

<div class="content-ad"></div>

두 번째로, 내 유아는 지도에 관심을 두지 않고 비행기에만 관심이 있어요. 비행기를 찾는 데 주의를 집중하고 소음을 없애려면 지도를 제거해야 했죠. 그리고 레이더를 구축하기 시작했어요!

## 업데이트된 스케일링 로직

비행기의 스케일링 로직을 손쉽게 수정했어요.

화면에 잘 어울리고 크기가 적당하게 분포되는 것을 확인하기 위해 몇 가지 시도 끝에 이 스케일링을 선택했어요.

<div class="content-ad"></div>

제 지역의 공중 스캔에서 이러한 스케일링이 나왔어요:

- Scale: 1.0835408863965839
- Scale: 0.8330645861650874
- Scale: 1.095791123396205
- Scale: 1.1077242935783653
- Scale: 2.0
- Scale: 1.4864702267977097
- Scale: 0.7

이 분포는 상당히 잘 작동하는 것 같아요. NOx를 제외하고는, 항공 여행 중심지에서 살 때 꽤 유용하답니다. 🌬️✈️

<div class="content-ad"></div>

# 레이더 만들기

나는 내가 상상한 레이더를 건설하기 준비를 거의 했다. 그러나 문제가 있었다.

## API 견고성

오픈 소스인 OpenSky API는 타임아웃이 발생하며 502 Bad Gateway 오류를 반환하거나 때로는 단순히 null 데이터를 가진 200 응답을 반환했다.

<div class="content-ad"></div>

프랭키, 괜찮아요 — 이 애플리케이션은 기업 비즈니스 앱이 아니니까, 이 멋진 API는 무료로 제공됩니다. SLA가 없고 제가 그런 것을 요구하지도 않아요. 

클라이언트 측의 신뢰성을 높이기 위해 API 호출에서 기본적인 재시도 논리를 구현했어요.

private func fetchFlights(at coordinate: CLLocationCoordinate2D, retries: Int = 3) async {
    do {
        try await api.fetchLocalFlightData(coordinate: coordinate)

    } catch {
        if retries > 0 {
            try await fetchFlights(at: coordinate, retries: retries - 1)
        }
    }
}

다음 날, 그 API는 하루 종일 잘 작동했어요 — 특정 고트래픽 시간을 제외하고는 대부분 좋은 것으로 보여요.

<div class="content-ad"></div>

## 지도를 가리는 작업

가장 중요한 소음 감소 작업은 실제 지도를 보이지 않게 만드는 것입니다. 레이더는 이렇게 하지 않으면 작동하지 않습니다.

나는 이 작업을 평평한 색으로 된 MapPolygon을 사용하여 수행했습니다 — 이것은 겉으로 보기에는 지도의 섹션을 강조하기 위해 오버레이를 배치할 수 있는 것으로 설계된 것입니다. 하지만 나는 우리의 주석을 제외한 모든 것을 숨기고 싶었습니다.

struct FlightMapView: View {

    var body: some View {
        Map(position: $cameraPosition) {
            planeMapAnnotations
            MapPolygon(overlay(coordinate: coordinate))
        }
        .mapStyle(.imagery)
        .allowsHitTesting(false)
    }

    // ...
    
    private func rectangle(around coordinate: CLLocationCoordinate2D) -> [CLLocationCoordinate2D] {
        [
            CLLocationCoordinate2D(latitude: coordinate.latitude - 1, longitude: coordinate.longitude - 1),
            CLLocationCoordinate2D(latitude: coordinate.latitude - 1, longitude: coordinate.longitude + 1),
            CLLocationCoordinate2D(latitude: coordinate.latitude + 1, longitude: coordinate.longitude + 1),
            CLLocationCoordinate2D(latitude: coordinate.latitude + 1, longitude: coordinate.longitude - 1)
        ]
    }
    
    private func overlay(coordinate: CLLocationCoordinate2D) -> MKPolygon {
        let rectangle = rectangle(around: coordinate)
        return MKPolygon(coordinates: rectangle, count: rectangle.count)
    }
}

<div class="content-ad"></div>

내 소중한 행운을 소모하면서, 이 접근법이 아주 효과적으로 작동했어요! 이제 비행기를 볼 수 있지만 지도는 없고, 우리가 원하던대로예요!

중요한 건, 애플이 오버레이를 지도 위에 놓고 주석 아래에 배치한 디자인을 했다는 거예요. 다른 방법을 택했다면, 제 딸의 새 장난감은 마물을 당해야 했을 거예요.

## 레이더 그리기

핵심 요구 사항 중 마지막 조각이 레이더 화면이었어요.

<div class="content-ad"></div>

이것은 본질적으로 세트의 선, 동심원, 그리고 20도의 회전 각도였어요. 저같은 SwiftUI 애호가에게는 간단한 작업이었죠.

## 심야의 교차확인

우리가 얼마나 많이 성장했는지 한 번 봐봐요.

오늘의 핵심 시각적 변화 - 지도를 오버레이로 숨기고 레이더를위한 SwiftUI 뷰 몇 줄로 - 우리는 이제 원래의 비전에 빠르게 다가가고 있습니다.

<div class="content-ad"></div>

하늘을 가득 채우는 비행기와 레이더 UI를 비교해 볼 때, 우리는 상당히 잘 어울립니다.

## 사용자 테스트 #2

3일 동안 열심히 일한 끝에, 내 어린이는 내가 만든 장난감에 조금씩 흥미를 보이기 시작했습니다.

우리는 개념을 증명했고, 핵심 초기 목표를 달성하는 MVP를 완성했습니다.

<div class="content-ad"></div>

이제 앱 스토어에 올릴 준비를 시작해볼까요?

## MVP부터 제품으로

나는 스키오모포리즘을 정말 좋아합니다. 그래서 이 앱에 레트로, 장난감과 같은 퀄리티를 더하기 위해 모든 애니메이션 능력을 발휘하고 싶었어요.

### 레이더를 돋보이게 만들기

<div class="content-ad"></div>

제가 레이더에 만든 효과에 자랑스러웠어요. 

[하신 효과](https://miro.medium.com/v2/resize:fit:1152/1*jD2X69Yi6Glx-uV_0xDOeA.gif)는 마치 내가 하는 게 '멍청한 천재'라고 불렀다면서요.

처음에는 삼각법과 타이머를 사용하여, 선이 건드릴 때마다 맵 주석을 재색칠하고 서서히 사라지게 만드는 방법을 고안했어요.

<div class="content-ad"></div>

하지만 그때 내가 깨달았어. 내 라인은 단순히 초록에서 투명으로 이어지는 20도 폭의 각도 그라데이션일 뿐이라는 걸.

만약 360도 폭의 각도 그라데이션이라면 어떨까?

거기에 초록에서 투명으로, 다시 투명으로, 또 투명으로, 마지막으로 검정까지 이어지는 그라데이션이라면 어떨까?

private var radarLine: some View {
    Circle()
        .fill(
            AngularGradient(
                gradient: Gradient(colors: [
                    Color.black, Color.black, Color.black, Color.black,
                    Color.black.opacity(0.8), Color.black.opacity(0.6),
                    Color.black.opacity(0.4), Color.black.opacity(0.2),
                    Color.clear, Color.clear, Color.clear, Color.clear,
                    Color.clear, Color.clear, Color.clear, Color.clear,
                    Color.clear, Color.clear, Color.clear, Color.green]),
                center: .center,
                startAngle: .degrees(rotationDegree),
                endAngle: .degrees(rotationDegree + 360)
            )
        )
        .rotationEffect(Angle(degrees: rotationDegree))
        .animation(.linear(duration: 6).repeatForever(autoreverses: false), value: rotationDegree)
}

<div class="content-ad"></div>

자주 그러한 경우 그러한 해결책이 가장 잘 작동합니다.

기기를 너무 빨리 회전할 때 화면의 모서리에서 지도에서 비롯된 이상한 시각적 효과를 발견했습니다. 오버레이가 지도의 카메라 위치 외부를 게을리 렌더링하는 것으로 보입니다.

레이더 뷰의 검은 윤곽선을 역 마스크로 만들면 문제가 해결됩니다(즉, 레이더를 위한 원형 구멍이 있는 검은 직사각형).

## 레이더를 정말로 돋보이게 만들기

<div class="content-ad"></div>

UI가 지금 정리되어 보이지만 아직 레트로라고 말하기는 좀 이르네요.

제가 CRT 화면 효과를 추가하고 싶었어요. 텔레비전 스캔라인을 넣어서 앱이 정말 오래된 레이더 스캐너 위에 그려진 것처럼 보이게 하고 싶어요.

iOS 17에는 Metal 셰이더를 지원하는 colorEffect가 내장되어 있어요. 그래서 이 효과를 구현하는 게 이전보다 훨씬 쉬워졌어요.

#include <metal_stdlib>
using namespace metal;

[[ stitchable ]] half4 crtScreen(
    float2 position,
    half4 color,
    float time
) {
    
    if (all(abs(color.rgb - half3(0.0, 0.0, 0.0)) < half3(0.01, 0.01, 0.01))) {
        return color;
    }
    
    const half scanlineIntensity = 0.2;
    const half scanlineFrequency = 400.0;
    half scanlineValue = sin((position.y + time * 10.0) * scanlineFrequency * 3.14159h) * scanlineIntensity;
    return half4(color.rgb - scanlineValue, color.a);
}

<div class="content-ad"></div>

C++에 대해 더 깊게 다루는 것은 다른 기사에 남겨둘 수 있을 것 같아요. 무료로 가져다 쓰셔도 좋습니다 — 가장 중요한 것은, 우리가 원하는 어떤 뷰에도 CRT 효과를 적용할 수 있는 뷰 수정자를 만들었다는 거에요!

extension View {
    
    func crtScreenEffect(startTime: Date) -> some View {
        modifier(CRTScreen(startTime: startTime))
    }
}
    
struct CRTScreen: ViewModifier {
    
    let startTime: Date
    
    func body(content: Content) -> some View {
        content
            .colorEffect(
                ShaderLibrary.crtScreen(
                    .float(startTime.timeIntervalSinceNow)
                )
            )
    }
}

이 수정자와 섀이더 자체는 시간 매개변수를 입력으로 받아서 스캔 라인이 빨리 움직이게 하고 효과를 더 동적으로 만든다는 점을 유념해 주세요.

![CRT Effect](https://miro.medium.com/v2/resize:fit:1200/1*w2X46UKHmjIzpO8ROWlRrA.gif)

<div class="content-ad"></div>

## 비상업적 이용

오픈스카이 네트워크 웹사이트는 꽤 명확하지만, 제가 공손하게 하기 위해 메모를 보내어 어플스토어에 제가 올릴 것이 그들의 정책에 잘 부합하는지 확인했습니다.

그들은 매우 친절히 20분 내에 답장을 주셨어요! 

![이미지](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_3.png)

<div class="content-ad"></div>

## 시각 너머

레이다의 경험을 판매하고 접근성을 높이기 위해, 비행 상태가 업데이트 될 때마다 작은 비프-부프 시스템 사운드 효과를 추가했어요.

private func fetchFlights(coordinate: Coordinate, retries: Int = 2) async {
    do {
        let flights = try await api.fetchLocalFlightData(coordinate: coordinate)
        await MainActor.run {
            self.flights = flights
            AudioServicesPlaySystemSound(1052)
            hapticTrigger.toggle()
        }

    // ...

}

때로는 새로운 haptic를 위한 메인 뷰에 sensorFeedback 수식어도 함께 사용해보세요.

<div class="content-ad"></div>

```
.sensoryFeedback(.levelChange, trigger: hapticTrigger)


But you know what? As I was working on this code, a thought popped up in my mind. Not everyone might appreciate that beeping sound. So, I think it's time to introduce some customization options.

## Controls & Customization

Let's start with a silent mode, which should be the first on the list.

<div class="content-ad"></div>

하지만 @AppStorage를 사용하여 몇 가지 간단한 사용자 정의도 가능합니다.

```swift
@AppStorage("silent") var silentMode: Bool = false
@AppStorage("showMap") var showMap: Bool = false
@AppStorage("userColor") var userColor: Color = .green
```

이제 사용자들은 소리를 끌 수 있고, 심지어 레이더 오버레이를 끄어 지도를 볼 수도 있습니다.

그러나 가장 중요한 것은 제 아이를 위해 이것을 만들고 있기 때문에 SwiftUI 색상 선택기를 통해 레이더에 사용자 정의 색상을 선택하는 것이 절대적으로 필수적입니다.

<div class="content-ad"></div>

드디어, 한 쌍의 애니메이션 SFSymbol이 없는 삶이 어떤 것인지 생각해보세요!

```swift
private func toggleableIcon(state: Bool, iconTrue: String, iconFalse: String) -> some View {
    Image(systemName: state ? iconTrue : iconFalse)
        .contentTransition(.symbolEffect(.replace))
    // ...
}
```

저희 앱이 이제 본격적으로 준비된 것 같아요.

![Animated SFSymbol](https://miro.medium.com/v2/resize:fit:1200/1*wyixSZmnEn2o4QgbraNQ7w.gif)

<div class="content-ad"></div>

안녕하세요! 조금 리팩토링을 해야 하는데, 뷰를 각각의 파일로 옮겨야 할 것 같아요.

지금 현재 최상위 AviatorView는 이렇게 생겼어요:

```js
// @State properties ...

var body: some View {
    ZStack {
        if let coordinate = locationManager.coordinateSubject.value {
            FlightMapView(
                cameraPosition: $cameraPosition,
                flights: flights,
                rotationAngle: rotationAngle,
                coordinate: coordinate
            )
        }
    
        TimelineView(.animation) { context in
            RadarView()
                .crtScreenEffect()
                .negativeHighlight()
        }
    
        ControlsView(errorMessage: errorMessage)
    }

    // onRecieve modifiers ...
}
```

## The Aviator Mascot

<div class="content-ad"></div>

치즈! 지난 달에 Midjourney 비용 납부를 멈춰서, 무역용으로 무료 사용할 수 있는 제네크래프트의 생성기를 활용했어요. 다행히도, 독비 모자를 쓴 딸을 근사하게 재현했는데, 바로 원했던 느낌이더라구요! ![링크](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_4.png) 덕분에 트위터에서 제 가장 성공적인 트윗을 남겼답니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_5.png)

# 앱 스토어로!

나는 개인적으로 여러 해 동안 애플 개발자 프로그램에 비용을 지불한 적이 없어.
이 버려진 사이드 프로젝트 무덤을 봐.


<div class="content-ad"></div>

이미지 주소가 있기 때문에 마크다운 형식으로 변경해 주세요.


![Image 6](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_6.png)

Welp. I’m £79 down and ready to hit publish.

![Image 7](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_7.png)

## 최종 사용자 테스트


<div class="content-ad"></div>

일요일 오후까지 계속 기다리면서, 아이가 이제 UI 색상을 직접 선택할 수 있다는 사실에 완전히 기뻐하는 여동생과 함께 주말 사용자 테스트 몇 라운드 더 진행해 보겠습니다.

## 토요일 아침

## 토요일 저녁

## 일요일 오후

<div class="content-ad"></div>

## 이제 온라인 상태입니다!

![이미지](/assets/img/2024-07-13-MyToddlerLovesPlanesSoIBuiltHerARadar_8.png)

여러분도 앱을 다운로드하고 싶으신가요?

지금 바로 폰에 Aviator — Radar 앱을 다운로드 받아보세요 (평가도 하지 마세요)!

<div class="content-ad"></div>

# 다음 단계

일주일 동안 몇 개 저녁에 모아서 만든 게 꽤 마음에 든다. 사이드 프로젝트를 오랜만에 진행한 건데, 딸을 위한 재미난 장난감을 만드는 건 몇 년 동안 코딩으로 즐거움을 느낀 적이 없었어.

이 글을 마치고 난 뒤, 다음 릴리스를 위해 내 미니 로드맵에 다음과 같은 몇 가지 기능을 추가할 계획이 있어:

- 지도에 줌 레벨을 추가하여 레이더를 가까운 항공기로 제한합니다.
- OpenSky Network API의 고급 버전을 사용하여 헬기, 인공위성 및 비행기 크기 클래스를 표시합니다.
- 비행기의 출발지 및 목적지 국가 표시를 전환합니다.
- 보다 고급 Metal 쉐이더로 CRT 화면 효과를 개선합니다.
- 모든 컨트롤을 조절 가능한 점진적 공개식 풀아웃 모달로 재구성하고 디텐트를 추가합니다.
- 일부 거리 및 고도를 필터링하는 슬라이더 컨트롤을 구현합니다. 예를 들어, 아래에 있는 원격 항공기를 숨기기 위해 일부 거리 및 높이를 숨깁니다.
- 레이더에 UFO, 거대한 벌레, 외계인을 렌더링하는 "우주모드"를 구현합니다.

<div class="content-ad"></div>

피드백이나 당신만의 아이디어가 있다면, 댓글로 꼭 알려주세요! 

(진짜, 새로운 Medium 알고리즘에 따르면, 클랩과 댓글이 상당한 차이를 만들어냅니다!)