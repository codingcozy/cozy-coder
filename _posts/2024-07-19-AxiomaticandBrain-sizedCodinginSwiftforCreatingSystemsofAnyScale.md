---
title: "모든 규모의 시스템을 구축하기 위한 Swift 코딩 방법 원칙 및 간결한 코드 작성법"
description: ""
coverImage: "/assets/img/2024-07-19-AxiomaticandBrain-sizedCodinginSwiftforCreatingSystemsofAnyScale_0.png"
date: 2024-07-19 11:26
ogImage: 
  url: /assets/img/2024-07-19-AxiomaticandBrain-sizedCodinginSwiftforCreatingSystemsofAnyScale_0.png
tag: Tech
originalTitle: "Axiomatic and Brain-sized Coding in Swift for Creating Systems of Any Scale"
link: "https://medium.com/better-programming/axiomatic-and-brain-sized-coding-in-swift-for-creating-systems-of-any-scale-ec352e7d87a9"
---


<img src="/assets/img/2024-07-19-AxiomaticandBrain-sizedCodinginSwiftforCreatingSystemsofAnyScale_0.png" />

이 글에서는 “Axiomatic Coding”이라고 하는 것을 소개하려고 합니다. 이것은 이해하기 쉬운 코드를 만들어내며 작은 게임부터 여러 기능이 포함된 완전한 앱까지 어떤 규모에서도 쉽게 확장할 수 있는 스타일을 사용합니다.

저는 Swift를 사용할 것이며, Swift는 말하는 영어와 닮은 코드를 작성할 수 있게 해주는 타입 시스템과 타입 추론을 사용합니다. 그리고 무엇보다도: 이러한 종류의 코드는 빠르게 작성할 수 있습니다.

# “Axiomatic”이란 무엇을 의미합니까?

<div class="content-ad"></div>

수학과 논리에서, 공리는 더 이상 증명하지 않고도 참으로 간주할 수 있는 명제입니다. 2 + 2 = 4가 그러한 공리입니다. 또한 모든 자연수 n에는 다음 자연수 n+1이 있음도 공리입니다.

"Axiomatic Codes"에는 그와 같은 명제가 포함되어 있습니다. 모든 사용자 상호작용이나 다른 변경 사항은 이러한 명제 중 하나 이상을 거칩니다. 다음은 이를 달성하는 한 가지 방법입니다:

이것은 "뱀"이라는 게임입니다. 주요 데이터 구조는 Change enum에서 가능한 상호작용을 정의합니다: 오른쪽으로, 왼쪽으로 이동하거나 앞으로 나아가거나 성장합니다.

Facing enum은 주어진 순간 어떤 방향을 향하고 있는지를 정의합니다. 북쪽, 남쪽, 동쪽, 서쪽

<div class="content-ad"></div>

```js
구조체 Snake {
    init (head:Coordinate) { self.init(head, [], .north) }
    
    enum Change {
        case move(Move); enum Move { 
            case forward, right, left 
        }
        case grow
    }
    
    enum Facing { case north, east, south, west }
    
    let head  : Coordinate
    let tail  : [Coordinate]
    let facing: Facing
    
    func body() -> [Coordinate] {  return [head] + tail }

    func alter(_ c:Change) -> Self {
        switch c {
        case let .move(d): return move(d)
        case     .grow   : return grow( )
        }
    }
}
```

<img src="https://miro.medium.com/v2/resize:fit:1200/1*tjeqTfSWVBUfUjO1-HjQPw.gif" />

The axiomatic statements are usually found in the alter method. Here I use helper methods move() and grow(), but as these are only called from alter, this is still true.

```js
확장 프로퍼티는 개인적으로 Snake {
    init(_ h:Coordinate,_ t:[Coordinate],_ f:Facing) { head = h; tail = t; facing = f }
    
    func move(_ move:Change.Move) -> Self {
        var newTail: [Coordinate] { Array(body().prefix(tail.count)) }
        
        switch (facing, move) {
        case (.north,.forward): return Self(Coordinate(x:head.x    ,y:head.y - 1),newTail,.north)
        case (.east ,.forward): return Self(Coordinate(x:head.x + 1,y:head.y    ),newTail,.east )
        case (.south,.forward): return Self(Coordinate(x:head.x    ,y:head.y + 1),newTail,.south)
        case (.west ,.forward): return Self(Coordinate(x:head.x - 1,y:head.y    ),newTail,.west )
        case (.north,   .left): return Self(Coordinate(x:head.x - 1,y:head.y    ),newTail,.west )
        case (.east ,   .left): return Self(Coordinate(x:head.x    ,y:head.y - 1),newTail,.north)
        case (.south,   .left): return Self(Coordinate(x:head.x + 1,y:head.y    ),newTail,.east )
        case (.west ,   .left): return Self(Coordinate(x:head.x    ,y:head.y + 1),newTail,.south)
        case (.north,  .right): return Self(Coordinate(x:head.x + 1,y:head.y    ),newTail,.east )
        case (.east ,  .right): return Self(Coordinate(x:head.x    ,y:head.y + 1),newTail,.south)
        case (.south,  .right): return Self(Coordinate(x:head.x - 1,y:head.y    ),newTail,.west )
        case (.west ,  .right): return Self(Coordinate(x:head.x    ,y:head.y - 1),newTail,.north)
        }
    }
    
    func grow() -> Self {
        //  |head|<----------------- tail ----------------->|facing|
        Self(head,!tail.isEmpty ? tail+[tail.last!] : [head],facing)
    }
}
```

<div class="content-ad"></div>

In move() 함수에서는 switch 문을 사용하여 뱀이 향하는 방향과 여기서 어느 방향으로 회전할지 패턴 매칭을 통해 평가합니다.
예를 들어:

```js
case (.north, .left): return Self(Coordinate(x: head.x-1, y: head.y), newTail, .west)
```

현재 뱀은 북쪽을 향하고 왼쪽으로 움직이고 있습니다. x 좌표가 하나 감소할 것이고 (head.x-1) 머리가 서쪽을 향하게 될 것을 쉽게 이해할 수 있습니다.

```js
case (.east, .right): return Self(Coordinate(x: head.x, y: head.y+1), newTail, .south)
```

<div class="content-ad"></div>

뱀은 동쪽을 향하고 오른쪽으로 회전합니다. 물론, 이제 남쪽을 향하게 되어 머리가 한 칸 아래로 이동합니다 (head.y +1).

이러한 case 문의 왼쪽 부분은 작업을 정의하고 오른쪽 부분은 작업을 실행합니다.

뱀을 이동시키는 데 12가지 가능한 조합이 있습니다: 뱀이 향하는 방향에 따라 4가지, 이동 방향에 따라 3가지입니다. 4 x 3 = 12. 결과 문장은 매우 간단하여 쉽게 이해하고 확인할 수 있습니다. 공리적입니다.

다음은 스마트 조명 시스템을 위한 Light를 모델링한 것입니다. Change enum은 조명과 상호 작용하는 여러 명령을 정의합니다.

<div class="content-ad"></div>

- 이름을 변경합니다: .renaming(.it(to:”kitchen”))
- 켜고 끕니다: .turning(.it(.on)) .turning(.it(.off))
- 조명 모드를 추가합니다: .adding(.mode(.hsb))
- 디스플레이를 전환합니다: .toggling(.display(to:.slider))
- 설정
색조: .setting(.hue(to:0.8))
채도: .setting(0.7)
밝기: .setting(.brightness(to:0.6))
온도: .setting(.temperature(to:.mirek(166))) — 조명의 색상 값.

```js
구조체 Light:Codable, Equatable, Identifiable {
    열거형 변경 {
        case renaming(_RenameIt); enum _RenameIt { case it     (to:String)    }
        case turning (_TurnIt  ); enum _TurnIt   { case it     (Turn)         }
        case adding  (_Add     ); enum _Add      { case mode   (Light.Mode)   }
        case toggling(_Toggle  ); enum _Toggle   { case display(to:Interface) }
        case setting (_Set     ); enum _Set      {
            case hue        (to:Double     )
            case saturation (to:Double     )
            case brightness (to:Double     )
            case temperature(to:Temperature)
        }
    }
    init(id:String, name:String) { self.init(id, name, .off, 0, 0, 0, 0, .slider, [], .unset) }
    
    열거형 Mode:Codable { case unset, hsb, ct }
    열거형 Temperature { case mirek(Int) }
    
    let id        : String
    let name      : String
    let isOn      : Turn
    
    let hue       : Double
    let saturation: Double
    let brightness: Double
    let ct        : Int
    
    let modes       : [Mode]
    let selectedMode: Mode
    let display     : Interface
    
    func alter(by changes:Change...) -> Self { changes.reduce(self) { $0.alter($1) } }
}
```

위의 공리적 명제들은 다시 개인적으로 alter 메소드에서 찾을 수 있습니다.

```js
확장 Light {
    개인적 init(_ x:String,_ n:String,_ o:Turn,_ b:Double,_ s:Double,_ h:Double,_ t:Int,_ d:Interface,_ m:[Mode],_ y:Mode) { id = x;name = n;isOn = o;hue = h;saturation = s;brightness = b;ct = t;modes = m;selectedMode = y;display = d }
    개인적 func alter(_ change:Change) -> Self {
        switch change {
        case let .renaming(         .it(to:n         )): return .init(id, n   , isOn, brightness, saturation,   hue,   ct,    display, modes,          selectedMode)
        case     .turning (         .it(.on          )): return .init(id, name, .on,  brightness, saturation,   hue,   ct,    display, modes,          selectedMode)
        case     .turning (         .it(.off         )): return .init(id, name, .off, brightness, saturation,   hue,   ct,    display, modes,          selectedMode)
        case let .setting ( .brightness(to:b         )): return .init(id, name, isOn, b         , saturation,   hue,   ct,    display, modes,          selectedMode)
        case let .setting ( .saturation(to:s         )): return .init(id, name, isOn, brightness, s         ,   hue,   ct,    display, modes,          .hsb        )
        case let .setting (        .hue(to:h         )): return .init(id, name, isOn, brightness, saturation,   h  ,   ct,    display, modes,          .hsb        )
        case let .setting (.temperature(to:.mirek(t) )): return .init(id, name, isOn, brightness, saturation,   hue,    t,    display, modes,          .ct         )
        case     .toggling(    .display(to:.slider   )): return .init(id, name, isOn, brightness, saturation,   hue,   ct,    .slider, modes,          selectedMode)
        case     .toggling(    .display(to:.scrubbing)): return .init(id, name, isOn, brightness, saturation,   hue,   ct, .scrubbing, modes,          selectedMode)
        case let .adding  (       .mode(mode         )): return .init(id, name, isOn, brightness, saturation,   hue,   ct,    display, modes + [mode], selectedMode)
        }
    }
}
```

<div class="content-ad"></div>

패턴 매칭을 통해 Change 명령에서 새 값 대신 이전 값에 새 값을 덮어씌워 새 빛 인스턴스를 생성합니다. 여기서는 밝기 값을 사용합니다:

```js
case let .setting(.brightness(to:b)): return .init(id,name,isOn,b ,saturation,hue,ct,display,modes,selectedMode)
```

공리적 문항은 연쇄적으로 연결할 수 있습니다:

```js
light.alter(
    by:
        .renaming(.it(to:"Turingzaal 1")),
        .setting (.brightness (to:0.5   )),
        .setting (.hue        (to:0.5   )),
        .setting (.saturation (to:0.5   )),
        .setting (.temperature(to:200.mk)),
        .turning (.it         (   .on   ))
    )
```

<div class="content-ad"></div>

음, 이번엔 Table 태그를 Markdown 형식으로 변환해보죠.

이 정도면 말하는 영어와 꽤 비슷해 보이죠? axiomatic 코딩을 데이터 유형에 어떻게 사용할 수 있는지 살펴봤으니, 이제 어떻게 동일한 기술을 사용하여 모든 규모의 시스템을 구축하는지 알아보겠습니다.

# 데이터 구조를 넘어서

우리는 제로부터 시작하는 것이 아니라, 로버트 C. 마틴이 제안한 "Clean Architecture"를 구현합니다. 마틴의 말처럼, 우리는 UseCases를 주요 구성 요소로 사용하지만, 이를 Feature로 그룹화하기도 합니다.

<div class="content-ad"></div>


테이블 태그를 Markdown 형식으로 변경하겠습니다.

A UseCase는 요청을 수신하고 해당 작업을 수행한 다음 응답을 보내는 프로토콜입니다. 두 개의 열거형을 사용하여 UseCases DSL을 설명할 수 있습니다.

UseCase 프로토콜의 구현은 조명 앱에서 가져온 이 예제와 비슷할 수 있습니다.

```js
struct LightValueSetter: UseCase {
    typealias RequestType  = Request
    typealias ResponseType = Response
    
    enum Request  { case apply   (Apply, on: Light)          }
    enum Response { case applying(Apply, on: Light, Outcome) }
    
    private let lightsStack : LightsStack
    private let store       : AppStore
    private let respond     : (Response) -> ()
    
    init(lightsStack:LightsStack, store:AppStore, responder:@escaping (Response) -> ()) {
        self.lightsStack = lightsStack
        self.store       = store
        self.respond     = responder
    }
    
    func request(_ request: Request) {
        switch request {
        case let .apply(.values(v),on:l): lightsStack.set(values:v,on:l) {
            switch $0 {
             case .success(let l): store.change(.by(.replacing(.light(with:l)))); respond(.applying(.values(v),on:l,.succeeded))
            case .failure(let e):                                                respond(.applying(.values(v),on:l,.failed(e)))
            }
        }
        }
    }
}

extension Light  {
    enum Value {
        case hue, saturation, brightness, colortemp
    }
    enum Values { 
        case hsb(Double, Double, Double), 
             ct(Int, Double), 
             bri(Double) 
    }
}
enum Apply { case values(Light.Values) }
enum Outcome { case succeeded, failed(Error?) }
```

<div class="content-ad"></div>

LightValueSetter는 LightStack 종속성을 갖고 생성되었습니다. 이는 네트워킹을 수행하고 조명에 연결할 것이며, 변경 사항을 저장할 AppStore, 호출자에게 UseCases 응답을 전달할 콜백 메서드가 포함되어 있습니다.

LightValueSetter 요청 enum은 Apply enum과 Light struct를 사용하는 하나의 case를 가지고 있습니다.

```js
setter.request(.apply(.values(.ct(220, 0.5)), on: light0)) // 색온도 설정
setter.request(.apply(.values(.bri(0.75)), on: light1))    // 밝기 설정
```

콜백을 통해 반환되는 응답은 다음과 같습니다:

<div class="content-ad"></div>

```js
.applying(.values(.ct(220, 0.5)), on: light0, .succeeded)
.applying(.values(.bri(0.75)), on: light1, .failed(e))
```

하나의 UseCase 표면적은 하나의 메소드인 요청(request)이고, 생성 시에는 콜백이 제공됩니다.

요청은 하나의 값만 허용하며, 그 값은 주로 Request 열거형이 생성을 허용하는 값입니다. 이는 보통 손에 꽤 들어오며, 오류 가능성을 제한하고 테스트하기가 쉽습니다: 요청을 보내고 적절한 응답이 돌아오는지 확인합니다.

Dimmer UseCase는 다음과 같을 수 있습니다. 이를 통해 주어진 값에 따라 색조, 채도, 밝기 또는 색온도를 감소시키거나 증가시킬 수 있습니다.

<div class="content-ad"></div>

```js
.dimming(.increase(.brightness, by: 10.pt, on: light0, .failed(e)))
.dimming(.decrease(.brightness, by: 10.pt, on: light1, .succeeded))
```

```js
struct Dimmer: UseCase {
    typealias RequestType = Request
    typealias ResponseType = Response
    
    private let lightsStack: LightsStack
    private let store      : AppStore
    private let respond    : (Response) -> ()
    
    enum Request  { 
        case increase(Light.Value, by: Light.Value.Increment, on: Light)
        case decrease(Light.Value, by: Light.Value.Increment, on: Light) 
    }
    enum Response { 
        case increase(Light.Value, by: Light.Value.Increment, on: Light, Outcome)
        case decrease(Light.Value, by: Light.Value.Increment, on: Light, Outcome)
    }
    init(lightsStack: LightsStack, store: AppStore, responder: @escaping (Response) -> ()) {
        self.lightsStack = lightsStack
        self.store = store
        self.respond = responder
    }

    func request(_ request: Request) {
        switch request{
        case let .increase(.brightness, by: .pt(increase), on: light):
            lightsStack.set(values: .bri(min(1.00, brightnessValue(of: light) + (increase / 100.0)), on: light) { result in
                switch result {
                case .success(let light): store.change(.by(.replacing(.light(with: light))); respond(.increase(.brightness, by: .pt(increase), on: light, .succeeded))
                case .failure(let error): respond(.increase(.brightness, by: .pt(increase), on: light, .failed(error)))
                }
            }
        case let .decrease(.brightness, by: .pt(decrease), on: light):
            lightsStack.set(values: .bri(max(0.01, brightnessValue(of: light) - (decrease / 100.0)), on: light) { result in
                switch result {
                case .success(let light): store.change(.by(.replacing(.light(with: light))); respond(.decrease(.brightness, by: .pt(decrease), on: light, .succeeded))
                case .failure(let error): respond(.decrease(.brightness, by: .pt(decrease), on: light, .failed(error)))
                }
            }
        case let .increase(.saturation, by: .pt(increase), on: light):
            lightsStack.set(values: .hsb(hueValue(of: light), min(1.00, saturationValue(of: light) + (increase / 100.0), brightnessValue(of: light)), on: light) { result in
                switch result {
                case .success(let light): store.change(.by(.replacing(.light(with: light))); respond(.increase(.saturation, by: .pt(increase), on: light, .succeeded))
                case .failure(let error): respond(.increase(.saturation, by: .pt(increase), on: light, .failed(error)))
                }
            }
        case let .decrease(.saturation, by: .pt(decrease), on: light):
            lightsStack.set(values: .hsb(hueValue(of: light), max(0.01, saturationValue(of: light) - (decrease / 100.0), brightnessValue(of: light)), on: light) { result in
                switch result {
                case .success(let light): store.change(.by(.replacing(.light(with: light))); respond(.decrease(.saturation, by: .pt(decrease), on: light, .succeeded))
                case .failure(let error): respond(.decrease(.saturation, by: .pt(decrease), on: light, .failed(error)))
                }
            }
        //...
    }
}

extension Light {
    enum Value {
        case hue, saturation, brightness, colortemp
        enum Increment { case pt(Double) }
    }
    enum Values { case hsb(Double, Double, Double), ct(Int, Double), bri(Double) }
}
```  

<div class="content-ad"></div>

아래는 각 사용 사례가 요청 및 응답 DSL을 가지고 있는 것을 볼 수 있습니다. DSL은 각 UseCase마다 고유하며 "관심사의 분리"를 효과적으로 구현합니다.

한편 기능은 단일 DSL인 Message를 공유합니다. 이로써 기능끼리 통신할 수 있고 Message는 UI에서도 사용됩니다.

이제 좀 더 심층적으로 살펴보는 시간입니다:

```js
//MARK: - 메시지
enum Message {
    case lighting(_Lighting)
    case dashboard(_Dashboard)
    case logging(_Logging)
}

//MARK: - 공통 조명 DSL 요소들
enum Outcome { case succeeded, failed(Error?)        }
enum Items   { case lights, rooms                    }
enum Loaded  { case lights([Light]), rooms([Room])   }
enum Turn    { case on, off                          }
enum Apply   { case values(Light.Values)             }
enum Alter   { case name(String)                     }

extension Light  {
    enum Value {
        case hue, saturation, brightness, colortemp
        public enum Increment { case pt(Double) }
    }
    enum Values { case hsb(Double, Double, Double), ct(Int, Double), bri(Double) }
}
//MARK: - 조명 메시지
extension Message {
    enum _Lighting { // 조명 기능
    //       |------------------------ 명령어 ----------------------|  |---------------------------- 응답 ----------------------------|
        case load    (Items                                          ), loading   (Loaded,                                          Outcome)
        case turn    (Light,    Turn                                 ), turning   (Light,    Turn,                                  Outcome)
        case apply   (Apply,                                 on:Light), applying  (Apply,                                 on:Light, Outcome)
        case change  (Alter,                                 on:Light), changing  (Alter,                                 on:Light, Outcome)
        case increase(Light.Value, by:Light.Value.Increment, on:Light), increasing(Light.Value, by:Light.Value.Increment, on:Light, Outcome)
        case decrease(Light.Value, by:Light.Value.Increment, on:Light), decreasing(Light.Value, by:Light.Value.Increment, on:Light, Outcome)
    }
}
```

<div class="content-ad"></div>

The Message enum에는 Lighting, Dashboard 및 Logging 기능을 나타내는 세 가지 상위 케이스가 있습니다.

이 문서에서는 후자를 무시하고 Lighting만 살펴보겠습니다.

Message._Lighting은 다음 명령어를 허용합니다:

- 조명 및 객실을 로드(load)하는 방법
.lighting(.load(.lights))
.lighting(.load(.rooms))
- 조명을 켜고 끄는 방법
.lighting(.turn(aLight, .on))
.lighting(.turn(aLight, .off))
- 조명 이름 변경하는 방법
.lighting(.change(.name(newValue), on:aLight))
- 주어진 증가 비율로 색조, 채도, 밝기 및 색온도를 증가 및 감소시키는 방법
.lighting(.increase(.brightness, by: .pt(10), on:aLight))
.lighting(.decrease(.brightness, by: .pt(10), on:aLight))
.lighting(.increase(.saturation, by: .pt(10), on:aLight))
.lighting(.decrease(.saturation, by: .pt(10), on:aLight))
...

<div class="content-ad"></div>

메시지는 기능이 청취하는 전역 이벤트입니다. 메시지가 특정 기능에 관심이 있으면 해당 기능은 패턴 일치를 통해 명령을 해독하고 사용 사례를 위해 번역할 것입니다.

사용 사례가 작업을 수행한 후(동기적 또는 비동기적으로), 기능에 응답을 반환합니다. 기능은 다시 모든 기능 및 UI로 전달할 수 있는 메시지로 다시 번역할 것입니다.

# 라이브러리로도 작동하는 아키텍처

앱 도메인에는 다양한 기능이 포함될 수 있으며 각 기능에는 임의의 개수의 사용 사례가 포함될 수 있습니다. 따라서 풍부한 앱을 구축하는 것이 가능합니다. 그러나 모든 표면 영역이 항상 함수일 뿐이므로 이를 통해 앱 도메인은 더 큰 프로젝트 내에서도 라이브러리로서 작동할 수 있습니다. 이는 전체 앱을 정의하는 데 적합하며 기존 코드에서 라이브러리로도 사용할 수 있습니다.

<div class="content-ad"></div>

앱 도메인은 다시 말해서 참조를 통해 유지하는 일부 적용된 함수일 뿐입니다. 기능과 같은 방식으로 말이죠.

```js
enum StackKind { case hue, mock }

func createAppDomain(
    store      : AppStore,
    receivers  : [Input],
    stackKind  : StackKind = .mock,
    rootHandler: @escaping Output) -> Input
{
    let stack = stack(of: stackKind)
    let features: [Input] = [
        createLightingFeature (store:store, lightsStack:stack, output:rootHandler),
        createDashboardFeature(store:store,                    output:rootHandler),
        createLoggingFeature()
    ]
    return { msg in
        (receivers + features).forEach { $0(msg) }
    }
}

fileprivate func stack(of kind:StackKind) -> LightsStack {
    switch kind {
    case .hue : return LightsStack(client: HueClient(ip:"192.168.178.37", user:"0cpSVXBH47AspTz8yz6xzPRH4e4DHIktuE18IUhd"))
    case .mock: return LightsStack(client: MockClient(with: thelights, rooms: []                                         ))
    }
}
```

# 확장 방법

두뇌 사이즈로 유지하기 위해 한 기능에는 10개 이상의 사용 사례가 없어야하며 앱 도메인에는 10개 이상의 기능이 없어야 합니다. 이는 최대 100개의 사용 사례를 갖는 대규모 앱을 구축할 수 있음을 의미합니다.

<div class="content-ad"></div>

어떤 프로젝트를 한 적이 없지만 실제로 도움이 되지 않았던 프로젝트는 없었어요. 하지만 이것이 끝이 아닙니다.

더 많은 사용 사례가 필요하다면 같은 트릭을 반복할 수 있어요: 하나의 앱 도메인 대신 여러 개의 도메인을 가질 수 있어요. 이제 각 도메인은 자체 Message 유형을 갖고 있고 앱 도메인 상위 단계에서는 도메인 간 메시지 유형을 공유합니다. 만약 앱 도메인에 대한 이 메시지가 흥미로운 경우, 특정 도메인 메시지 유형 값으로 매핑되며 이 값이 도메인 함수로 전달됩니다. 여전히 10가지가 뇌 크기임을 가정하면, 각각 최대 100가지 사용 사례를 가지는 10개의 앱 도메인을 보유할 수 있어요: 이제 전체 시스템은 1000가지 UseCases를 유지할 수 있어야 합니다 — 이건 충분히 될 거예요.

# 왜 신경 써야 할까요?

내게만큼이나 이러한 종류의 코딩이 지난 2십년 동안 접해 온 다른 스타일보다 몇 배 빠르다는 사실이 증명되었어요. 간단한 게임을 만드는 데 몇 분이면 충분해요. Lighting 앱은 하루만에 완료되었어요. 이러한 종류의 코딩을 배우는 데 코더들이 매우 짧은 시간이 걸렸다는 피드백을 받았어요 — 딱 한 두 시간이면 충분했대요.

<div class="content-ad"></div>

이렇게 작성된 코드는 거의 불변형입니다. 이는 좋은 점이죠: 그 목적대로 변경할 수 없다면 우연히 변경되지도 않습니다. 오류의 크기는 더 이상 발생할 수 없습니다.

이 기사에서 시스템 설계를 심도있게 다루지는 않았지만 ("Swift에서 깨끗한 아키텍처의 구현인 Khipu 소개"를 읽어서 더 자세히 알아보세요), 우리는 오직 한 방향으로만 흐르는 아키텍처를 만들었어요. 이는 뇌 크기에게 큰 승리이면서 오류가 없는 코드에 좋다는 것을 의미해요.

# 리소스

- The Ultimate Domain Language: 선언적 Swift
- Swift의 유형 시스템을 사용한 동작 모델링
- 선언적 Swift에서 외교
- Khipu: 선언적 도메인 패러다임을 통한 신속하고 지속 가능한 Swift 소프트웨어 생성
- 조명 앱: https://gitlab.com/vikingosegundo/brighter-hue
- 뱀 앱: https://gitlab.com/vikingosegundo/declarative-snake
- 더 많은 예제: https://gitlab.com/vikingosegundo