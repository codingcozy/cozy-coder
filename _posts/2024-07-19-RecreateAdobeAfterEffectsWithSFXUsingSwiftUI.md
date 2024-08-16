---
title: "SwiftUI로 Adobe AfterEffects 효과 재현하는 방법 SFX 추가하기"
description: ""
coverImage: "/assets/img/2024-07-19-RecreateAdobeAfterEffectsWithSFXUsingSwiftUI_0.png"
date: 2024-07-19 13:24
ogImage: 
  url: /assets/img/2024-07-19-RecreateAdobeAfterEffectsWithSFXUsingSwiftUI_0.png
tag: Tech
originalTitle: "Recreate Adobe AfterEffects With SFX Using SwiftUI"
link: "https://medium.com/better-programming/sfx-using-swiftui-2a3e4079f4f2"
---


![image](https://miro.medium.com/v2/resize:fit:1400/1*adaUpDI77HWM13BjZ0LjCA.gif)

몇 회사만이 동사를 발명했다고 주장할 수 있거나 적어도 하나를 다시 활용했습니다. 모든 언어에서 동일한 동사입니다. 아마도 가장 유명한 것은 "Google"일 것입니다. 당신은 뭔가를 Google한다는 의미로 찾습니다.

그것보다 더 악의적인 것이 하나 더 있습니다. 그것은 거의 원래 소유자로부터 분리되어버릴 정도로 이르렀습니다. 즉, "Photoshop"이라는 이름입니다. 사진은 "Photoshop" 앱을 사용하여 변경되지 않았더라도 "photoshopped"로 설명됩니다. 그러나 Adobe는 무료 광고일 뿐이라고 생각하겠지만 뭐니 뭐니 해도 신경 쓰지 않을 것입니다.

물론 오늘날에는 Adobe가 가장 다양한 크리에이티브 앱 포트폴리오를 보유하고 있으며 Adobe Illustrator 및 Adobe After Effects와 같은 다른 앱과 함께 시장을 조용히 장악했습니다. 후자 패키지는 우주적 비디오에 특수 효과를 생성하는 데 효과적입니다. SwiftUI를 사용하여 속성 문자열, 애니메이션 및 약간의 Swift 마법을 활용하여 일부 SFX를 재생성하는 방법을 보고 싶으시다면 여기서 저와 함께하세요.

<div class="content-ad"></div>

# 연구

Adobe의 소속이거나 상당히 가까운 친척일 것입니다. "After Effects" SFX 템플릿을 자랑하는 무수히 많은 템플릿이 표시됩니다. 많은 템플릿은 단어를 한 글자씩 나열하고 거의 모든 글자와 단어를 페이지 상에서 움직입니다. 그들은 불투명도와 색상을 자주 변경하고 각도를 수정합니다.

## 시작

프로젝트를 시작할 때, 글자 간격을 다시 살펴보았습니다. 똑똑한 코드를 개발하여 캡처해야 할 것이라고 생각했지만, 다행히도 NSString이 그것을 대신해 줄 수 있다는 것을 발견하여 그럴 필요가 없었습니다.

<div class="content-ad"></div>

```js
for w in 0..<words.count {
    let word = words[w]
    if let font = UIFont(name: "Neuton-Regular", size: 48) {
        let fontAttributes = [NSAttributedString.Key.font: font]
        let text = "\(word)"
        let boxValue = (text as NSString).size(withAttributes: fontAttributes)
        let boundingBox = (text as NSString).boundingRect(with: boxValue, context: nil)
        boxes.append(boxValue)
        bounding.append(boundingBox)
    }
}
if boxes.count == words.count {
    isReady = true
}

bounding box 값이 표준 너비로 나타나므로 글꼴 문자가 표시됩니다. 그런 다음 문자열을 표시하기 위해 HStack을 사용하여 뷰를 만들었습니다. 다음은 코드입니다:

struct WordView: View {
    @EnvironmentObject var paras: Paras
    @Binding var boxes: [CGSize]
    @Binding var isReady: Bool
    var body: some View {
        HStack(spacing: paras.spaces) {
            if isReady {
                ForEach(0..<words.count, id: \.self) { dix in
                    LetterView(boxes: $boxes, letter2D: words[dix], index2D: dix)
                }
            }
        }
    }
}

뷰 안에 다른 뷰를 포함하는 뷰입니다.
```

<div class="content-ad"></div>

```swift
// 해당 코드에서 참조된 환경 객체는 매개 변수의 컬렉션입니다. 변경할 수 있는 값들은 표시된 문자열의 각 문자의 일부 측면에 영향을줍니다.

class Paras: ObservableObject {
    static var counts = words.count
    @Published var spaces = -4.0
    @Published var zoomer:[Double] = Array(repeating: 1, count: counts)
    @Published var angles:[Double] = Array(repeating: 0, count: counts)
    @Published var offsets:[CGSize] = Array(repeating: CGSize.zero, count: counts)
    @Published var axis:[SIMD3<Int>] = Array(repeating: SIMD3<Int>(x: 0, y: 1, z: 0), count: counts)
    @Published var alphas:[Double] = Array(repeating: 1, count: counts)
    @Published var colours:[Color] = Array(repeating: Color.white, count: counts)
}

let screenSize: CGRect = UIScreen.main.bounds
let screenWidth: CGFloat = UIScreen.main.bounds.width
let screenHeight: CGFloat = UIScreen.main.bounds.height

메뉴 버튼을 사용하여 표시된 문자열의 각 문자의 불투명도, 각도, 위치 및 크기를 개별적으로 변경하고 심지어 3D 변환을 적용할 수도 있습니다.
```

<div class="content-ad"></div>

## 더 많은 정보

하지만 더 중요한 것은 해당 문자열의 색상에 대한 최대한의 유연성을 확보하고 싶었습니다. 그래서 나는 이를 마스크로 구현하기로 결정했습니다. 그 뒤에는 이 웹 사이트에서 다운로드한 배경 영화 파일을 재생했습니다. 다음 코드로 이를 성취했습니다:

```js
struct AVPlayerControllerRepresented : UIViewControllerRepresentable {
    var player : AVPlayer
    
    func makeUIViewController(context: Context) -> AVPlayerViewController {
        let controller = AVPlayerViewController()
        controller.player = player
        controller.showsPlaybackControls = false
        NotificationCenter.default.addObserver(forName: .AVPlayerItemDidPlayToEndTime, object: self.player.currentItem, queue: .main) { [self] _ in
            self.player.seek(to: CMTime.zero)
            self.player.play()
        }
        return controller
    }
    
    func updateUIViewController(_ uiViewController: AVPlayerViewController, context: Context) {
        
    }
}
```

저는 이 비디오를 루프하는 유용한 팁을 스택 오버플로우에서 찾았습니다. 플레이어의 버전이 이 작성한 코드와는 다르게 작동할 것입니다.

<div class="content-ad"></div>

그리고 마지막으로, 모든 것을 함께 가져와서 여기에 메인 ContentView가 있습니다.

```js
struct ContentView: View {
    @StateObject var paras = Paras()
    
    @State var boxes:[CGSize] = []
    @State var bounding:[CGRect] = []
    @State var isReady = false
    @State var id = 0
    @State var bitPattern = "0"
    
    let player = AVPlayer(url: URL(fileURLWithPath: Bundle.main.path(forResource: "fire", ofType: "mp4")!))
    var body: some View {
        ZStack {
          // 박스 마스크의 초기화는 여기에 들어갑니다
          VStack {
                AVPlayerControllerRepresented(player: player)
                    .onAppear {
                        player.play()
                        player.actionAtItemEnd = .none
                    }
                    .scaleEffect(4.0)
                    .offset(y: -312)
                    .mask(WordView(boxes: $boxes, isReady: $isReady))
                    .font(Fonts.neutonRegular(size: 312))
                Actions(bitPattern: $bitPattern)
            }
        }.environmentObject(paras)
    }
}
```

인터페이스에는 버튼이 반 다섯 개 정도 있어서, 다양한 SFX 효과를 시작할 수 있는 훅이 주어졌습니다. 모든 액션은 다음과 같은 템플릿을 따릅니다:

```js
Text("숨기기")
    .font(Fonts.neutonRegular(size: 24))
    .foregroundColor(Color.red)
    .onTapGesture(count: 2) {
        for i in stride(from: words.count - 1, through: 0, by: -1) {
            DispatchQueue.main.asyncAfter(deadline: .now() + Double(i)) {
                paras.angles[i] = 90.0
            }
        }
    }
    .onTapGesture(count: 1) {
        for i in stride(from: words.count - 1, through: 0, by: -1) {
            paras.angles[i] = 0
        }
    }.padding()
```

<div class="content-ad"></div>

모두를 고려하면, 여기 이 설정으로 얻을 수 있는 두 번째 SFX 몽타주가 있습니다. 이 기사를 이끌기 위해 동일한 코드를 사용했지만 다른 비디오를 사용했습니다.

![SFX](https://miro.medium.com/v2/resize:fit:1400/1*t-65oBltnDb0bNqYdn0IRQ.gif)

# 순수 SwiftUI

곧바로 떠오르시겠지만, 문자 간격 크기 코드는 무슨 일이 있었는지 묻습니다. 말 그대로, 의미가 무엇이었을까요? 결국 아직 사용하지 않았기 때문입니다. 플레이어 코드를 주석 처리하고 이 구조체를 추가하세요:

<div class="content-ad"></div>

```js
struct BackView: View {
    @EnvironmentObject var paras:Paras
    @Binding var boxes:[CGSize]
    @Binding var isReady:Bool
    var body: some View {
        HStack(spacing: paras.spaces) {
                if isReady {
                    let max = Double(words.count)
                    ForEach(0..<words.count, id: \.self) { dix in
                        let min = Double(dix + 1)
                        let hue = min / max
                        let color = Color(hue: hue, saturation: 1.0, brightness: 1.0)
                        let mode = LinearGradient(gradient: Gradient(colors: [.black,color]), startPoint: .top, endPoint: .bottom)
                        RecView(index2D: dix, letter2D: words[dix], color2D: mode, boxes: $boxes)
                    }
                }
        }
    }
}
```

여기에 필요한 코드입니다:

```js
struct RecView: View {
    @EnvironmentObject var paras:Paras
    @State var index2D:Int
    @State var letter2D:String
    @State var color2D:LinearGradient
    @Binding var boxes:[CGSize]
   
    var body: some View {
        HStack(spacing: paras.spaces) {
                Rectangle()
                    .fill(color2D)
                    .offset(paras.offsets[index2D])
                    .frame(width: boxes[index2D].width, height: boxes[index2D].height * 3)
                    .padding(.trailing,0)
                    .rotation3DEffect(.degrees(paras.angles[index2D]), axis: (x:paras.axis[index2D].x.cg, y: paras.axis[index2D].y.cg,z:paras.axis[index2D].z.cg), anchor: .center, anchorZ: 0, perspective: 2)
   
                    .animation(.easeOut(duration: 4), value: paras.offsets)
                    .scaleEffect(paras.zoomer[index2D])
        }
    }
}
```

마지막으로, ContentView 주석 처리 된 섹션에 BackView 코드를 추가해주세요.

<div class="content-ad"></div>


<img src="/assets/img/2024-07-19-RecreateAdobeAfterEffectsWithSFXUsingSwiftUI_0.png" />

이제 실행해보세요. 단어를 바꿨다고 가정하면 다음과 같은 결과가 보일 것입니다:

<img src="https://miro.medium.com/v2/resize:fit:1400/1*A2MTdypy7JtszC-rby4mmQ.gif" />

제가 글자에 색상을 입히기 위해 LinearGradient을 사용했습니다. 이 그라데이션을 흑백으로 변경하는 불 버튼을 이용했어요. 이 효과는 RecView()에 추가한 "combine publisher"를 사용해서 만들었습니다.


<div class="content-ad"></div>

```swift
.onReceive(colorPublisher, perform: { message in
    let (color,index) = message
    if index == index2D {
        let color2R = color2D
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.25) {
            color2D = color
            DispatchQueue.main.asyncAfter(deadline: .now() + 0.25) {
                color2D = color2R
            }
        }
    }
})
```

물론, 이전 예제에서 사용한 다른 모든 SFX도 모두 있으므로 텍스트를 한 글자씩 사라지게 만들거나 회전시키거나 어느 방향으로든 뒤집을 수 있습니다.

다음은 이 코드를 사용하여 마스크 뒤로 지나가는 스캔 라인을 생성하는 TimeLineView를 사용한 또 다른 대안입니다.

```swift
TimelineView(.periodic(from: .now, by: 0.010)) { timeline in
    Rectangle()
        .fill(LinearGradient(colors: [.black,.red], startPoint: .leading, endPoint: .trailing))
        .frame(width: 16, height: 256)
        .position(x: CGFloat(xPos), y: 200)
        .onChange(of: timeline.date, perform: { newValue in
            let max = Double(screenWidth)
            let min = Double(xPos + 1)
            let hue = min / max
            paras.colours[0] = Color(hue: hue, saturation: 1.0, brightness: 1.0)
            xPos = xPos < screenWidth ? xPos:0
            xPos += 4
        })
        .mask(WordView(boxes: $boxes, isReady: $isReady))
        .scaleEffect(y:2)
}
```

<div class="content-ad"></div>

여기 위 코드로 결과를 만들어냅니다. 단어를 읽을 수 있나요?

![image](https://miro.medium.com/v2/resize:fit:1400/1*-15ZjbVyemnyOX34xS-bsA.gif)

# SceneKit

마스크 뒤에 더 반응적인 환경이 있는 것이 더 의미가 있습니다. 이 코드로 배경을 바꿔보세요. 이렇게 하면 간단한 장면을 설정하고, 상자를 생성하는 subscriber를 만들어 땅으로 떨어뜨릴 것입니다.

<div class="content-ad"></div>

```swift
let wedgePublisher = PassthroughSubject<Color, Never>()
var wedgeSubscriber: AnyCancellable!

class GameScene: SKScene {
    override func didMove(to view: SKView) {
        physicsBody = SKPhysicsBody(edgeLoopFrom: frame)
        
        wedgeSubscriber = wedgePublisher
            .sink(receiveValue: { [self] color in
                let newCord = CGPoint(x: Double.random(in: 0..<screenWidth), y: screenHeight)
                let box = SKSpriteNode(color: UIColor(color), size: CGSize(width: 64, height: 64))
                box.position = newCord
                box.physicsBody = SKPhysicsBody(rectangleOf: CGSize(width: 64, height: 64))
                addChild(box)
            })
    }

}

struct GameView: View {
    var scene: SKScene {
        let scene = GameScene()
        scene.size = CGSize(width: screenWidth, height: screenWidth)
        scene.scaleMode = .fill
        return scene
    }

    var body: some View {
        SpriteView(scene: scene)
            .frame(width: screenWidth, height: screenHeight)
            .ignoresSafeArea()
    }

}
```

번역을 마쳤다면, 뷰에 추가할 액션 버튼을 만들어 보세요. 이 버튼은 눌리면 wedgePublisher를 128번 호출할 것입니다.

```swift
struct Actions3: View {
    var body: some View {
        Text("발사")
            .font(Fonts.neutonRegular(size: 24))
            .foregroundColor(Color.red)
            .onTapGesture(count: 2) {
                let max = Double(128)
                for i in 0..<128 {
                    DispatchQueue.main.asyncAfter(deadline: .now() + Double(i) * 0.1) {
                        let min = Double(i + 1)
                        let hue = min / max
                        let color = Color(hue: hue, saturation: 1.0, brightness: 1.0)
                        wedgePublisher.send(color)
                    }
                }
            }
    }
}
```

마지막으로, 위의 코드로 새로운 뷰를 메인 ContentView에 추가하세요.

<div class="content-ad"></div>

![image](/assets/img/2024-07-19-RecreateAdobeAfterEffectsWithSFXUsingSwiftUI_1.png)

추가한 후에 실행해보세요. 이렇게 나타날 거에요. 이 비디오에서는 SKShapeObject를 사용했고, title에 projectionEffect를 적용하여 보통의 글꼴을 기울임체로 만들었어요. 스택 오버플로우에서 훌륭한 설명을 찾을 수 있어요.

```js
.projectionEffect(ProjectionTransform(CATransform3DMakeRotation(-10 * (.pi / 180), 0.0, 0.0, 1.0)))
```

![image](https://miro.medium.com/v2/resize:fit:1400/1*gPnu41TzBgkxSEQpETw4yw.gif)

<div class="content-ad"></div>

마침내, 저는 다루지 않은 또 다른 태그인 shadow입니다. 그러나 이것은 세 가지 기본 색상인 빨강, 녹색, 파랑으로 그림자를 분할하는 일반적인 효과가 아닙니다.

```js
struct NewView: View {
    
    @State private var timeRemaining = 0.0
    
    let timer = Timer.publish(every: 1, on: .main, in: .common).autoconnect()
    let shadowRange: CGFloat = 4
    
    @State var redShadow = Color.clear
    @State var greenShadow = Color.clear
    @State var blueShadow = Color.clear
    
    @State var redSX: CGFloat = 8
    @State var redSY: CGFloat = 8
    @State var blueSX: CGFloat = 8
    @State var blueSY: CGFloat = -8
    @State var greenSX: CGFloat = -8
    @State var greenSY: CGFloat = -8
    
    @State var offsets = CGSize.zero
    @State private var offset = CGSize.zero
    
    var body: some View {
        
        Text("shadows")
            .font(Fonts.neutonRegular(size: 128))
            .foregroundColor(Color.white)
            .offset(offsets)
            .onTapGesture {
                timeRemaining = 1.0
            }
            .shadow(color: greenShadow, radius: 0.5, x: blueSX, y: blueSY)
            .shadow(color: redShadow, radius: 0.5, x: redSX, y: redSY)
            .shadow(color: blueShadow, radius: 0.5, x: greenSX, y: greenSY)
            .onReceive(timer) { time in
                if timeRemaining > 0 {
                    greenSX = offset.width > 0 ? 4.0 : -4.0
                    greenSY = offset.height > 0 ? 4.0 : -4.0
                    redSX = offset.width > 0 ? -4.0 : 4.0
                    redSY = offset.height > 0 ? 4.0 : -4.0
                    blueSX = offset.width > 0 ? -4.0 : 4.0
                    blueSY = offset.height > 0 ? -4.0 : 4.0
                    withAnimation(.linear(duration: 0.5)) {
                        redShadow = Color.clear
                        greenShadow = Color.clear
                        blueShadow = Color.clear
                    }
                }
            }
            .gesture(
                DragGesture()
                    .onChanged { gesture in
                        offset = gesture.translation
                        withAnimation(.linear(duration: 0.25)) {
                            redShadow = Color.red
                            greenShadow = Color.green
                            blueShadow = Color.blue
                        }
                    }
                    .onEnded { _ in
                        if abs(offset.height) > 10 || abs(offset.width) > 10 {
                            offsets = offset
                        } else {
                            offsets = .zero
                        }
                        timeRemaining = 1
                    }
            )
    }
}
```

움직임을 보여주기 위해 약간 과장된 RGB 효과 버전을 생성하는 코드 조각입니다. 화면 주위를 움직이는 텍스트 전체를 드래그 제스처를 사용하여 이동하고 있습니다. 이 제스처로 애니메이션이 트리거됩니다.

<img src="https://miro.medium.com/v2/resize:fit:1400/1*uIcWBVllxhskBGinI2Nayg.gif" />


<div class="content-ad"></div>

조금 놀아보세요; 그림자의 시간과 오프셋을 변경해보세요.

이 모든 것이 이 글의 끝으로 이어지는데, 이를 읽으며 여러분의 제목에 대한 아이디어를 얻었으면 좋겠습니다.