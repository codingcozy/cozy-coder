---
title: "SwiftUI와 물리 효과로 만드는 텍스트 애니메이션 방법"
description: ""
coverImage: "/assets/img/2024-07-23-TextEffectsWithSwiftUIandPhysics_0.png"
date: 2024-07-23 11:26
ogImage: 
  url: /assets/img/2024-07-23-TextEffectsWithSwiftUIandPhysics_0.png
tag: Tech
originalTitle: "TextEffects With SwiftUI and Physics"
link: "https://medium.com/better-programming/texteffects-with-swiftui-and-physics-2bcd1f1efad5"
---


<img src="https://miro.medium.com/v2/resize:fit:1400/1*y1JaOSszoa3ZpKLOQSCPXw.gif" />

저는 SwiftUI의 팬입니다. 2019년 출시 이후로 계속 사용해왔어요. 이 다섯 년 동안 많은 업데이트와 발전을 거듭했지만, 준비가 덜 된 것으로 생각하는 사람들도 있어요.

SwiftUI에서 아직 지원되지 않는 주요 기능 중 하나는 iOS 물리 엔진인데요, 이는 UIKit에 오랫동안 존재해온 요소입니다. 이 부족한 점을 보완하고 재미를 느끼기 위해 만든 방법을 함께 살펴보세요. 제가 말씀드리고 싶은 건 SwiftUI에 스프링과 같은 애니메이션이 있지만, 볼 수 있듯이 물리 엔진에는 훨씬 더 많은 것이 있습니다.

# GameClass 템플릿

<div class="content-ad"></div>

프로젝트를 시작하기 위해, 나는 내가 가장 좋아하는 참고 사이트 중 하나인 Hacking with Swift, Paul Hudson의 사이트를 찾아 SwiftUI와 함께 SpriteKit를 사용하는 데 필요한 코드 템플릿을 얻었어. 조금만 손을 대고 코드를 조정한 후에 코드는 다음과 같이 보이지:

```js
class GameScene: SKScene {
    override func didMove(to view: SKView) {
        physicsBody = SKPhysicsBody(edgeLoopFrom: frame)
        physicsWorld.gravity = CGVector(dx: 0.0, dy: 0.0)
    }
}
```

```js
struct ContentView: View {
    var scene: SKScene {
        let scene = GameScene()
        scene.size = CGSize(width: screenWidth, height: screenHeight)
        scene.scaleMode = .fill
        return scene
    }
    var body: some View {
        SpriteView(scene: scene, debugOptions: .showsPhysics)
            .frame(width: screenWidth, height: screenHeight)
    }
}
```

이제 SwiftUI 창 안에서 SpriteKit 인스턴스가 실행 중이다. 그 다음으로, 나는 누나 문서에서 논의한 공통 클래스 코드를 추가했어, Position 클래스의 간소화된 버전이야.

<div class="content-ad"></div>

```swift
class Position: ObservableObject {
    static var shared = Position()
    @Published var pointsX:[CGPoint] = []
    @Published var pointsY:[CGPoint] = []
}
```

이 코드는 SKLabelNode에 물리 속성을 가진 노드를 만들고 그것을 SwiftUI 뷰에 위치 속성을 통해 연결하는 것입니다. 여기에 X 값들을 캡처하기 위한 첫 번째 세트와, Y 값을 캡처하기 위한 두 번째 세트가 있습니다. 이 두 세트는 다른 축은 변경되지 않도록 합니다.

그런 다음, GameScene의 didMove 메서드에서 단어를 만드는 데 사용한 문자 배열을 정의하고, 샘플 케이스에서 7개의 SKLabelNode를 추가했습니다. 각 노드의 좌표를 공통 Position 클래스에 저장했습니다. 7개의 노드가 있었던 이유는 사용한 단어 "JOURNEY"에 7개의 문자가 포함되어 있었기 때문입니다.

```swift
for word in words {
    nodes.append(addText(word))
    nodes[nodes.count - 1].position = CGPoint(x: frame.minX + wedge + 300, y: frame.midY)
    points.pointsX.append(nodes[nodes.count - 1].position)
    points.pointsY.append(nodes[nodes.count - 1].position)
    points.pointsR.append(nodes[nodes.count - 1].position)
    addChild(nodes[nodes.count - 1])
    wedge += 32
}
```

<div class="content-ad"></div>

지금 이 시점에서 앱을 실행하면 회색 글자와 물리 모양을 나타내는 파란 상자가 표시됩니다. 물리 모양은 힘과 인력을 사용할 때 글자들이 서로 충돌하도록 보장하기 위해 필요합니다.

![이미지](/assets/img/2024-07-23-TextEffectsWithSwiftUIandPhysics_0.png)

물리학적인 관점에서 그 모양으로 만들었기 때문에, 모든 글자가 서로 위에 쌓여있기를 원합니다. SwiftUI 뷰로 그런 식으로 쌓을 예정이기 때문에 그렇게 모양이 지어졌습니다.

# 문자 간격 설정

<div class="content-ad"></div>

그림자 노드를 생성한 후에, 처음에는 일곱 개의 SwiftUI 뷰를 만들어 이 뷰들의 위치를 그 일곱 노드와 직접 연결했습니다. 타이틀에서 더블 스페이싱이 만족스러웠고 수직 공간만 다뤄야 한다면 잘 작동했습니다. 

그러나 타이틀의 간격을 없애려면 케링(kerning)을 수정해야 했습니다. 수평 축을 다루고 싶다면 더 나은 수정이 필요했습니다. 일곱 개의 개별 텍스트 뷰를 사용했기 때문에 오히려 정확한 케링을 적용하지 못했던 것이었습니다.

문제를 보다 철저히 조사하면서 이를 수정하기 위해 여러 버전의 코드를 시도해 보았습니다. 처음에는 SpriteKit에서 프레임 크기를 확인하여 수정하고 싶었지만 작동하지 않았습니다. 폰트에서 케링을 직접 저장하는 코드를 발견했지만, 반환된 값은 "단일 글자"에 대한 것이었으므로 도움이 되지 않았습니다.

```js
extension String
{
   func sizeUsingFont(usingFont font: UIFont) -> CGSize
    {
        let fontAttributes = [NSAttributedString.Key.font: font]
        return self.size(withAttributes: fontAttributes)
    }
}
```

<div class="content-ad"></div>

해당 문자열을 글자별로 쌓아서 변화를 살펴보는 것도 시도해 봤었지만, 제 이론이 틀렸는지, 아니면 제 코드가 잘못되었는지는 확실하지 않아요. 어쨌든 그 방법은 작동하지 않았어요.

# SwiftUI

곤혹스러운 상황에 처해서, SwiftUI 뷰에서 케닝 정보를 추출해보기 위해 이전과 같이 7개의 뷰에서 환경 설정을 사용해보았어요.

```js
struct ViewSizeKey2: PreferenceKey {
    static var defaultValue: CGFloat = 0

    static func reduce(value: inout CGFloat, nextValue: () -> CGFloat) {
        value = nextValue()
    }
}

// 배경으로 ViewGeometry 사용
struct ViewGeometry: View {
    var body: some View {
        GeometryReader { geometry in
            Color.clear
                .preference(key: ViewSizeKey2.self, value: geometry.frame(in: .local).midX)
                
        }
    }
}

HStack {
    ForEach((0..<words.count), id: \.self) { dex in
        Text(words[dex])
            .foregroundColor(Color.yellow)
            .font(Fonts.neutonBold(size: 48))
            .background(ViewGeometry())
            .onPreferenceChange(ViewSizeKey2.self) {
                points.widths[words[dex]] = $0
            }
    }.offset(x:-46)
}
```

<div class="content-ad"></div>

하지만 결과는 SpriteKit과 똑같았어요. 더 많은 정보를 찾아보면서 *.ttf 글꼴 파일의 디코딩에 관한 몇 개의 게시물을 발견했어요. 비록 Python을 사용하긴 했지만요. 조금 복잡하기는 했지만, 최적의 해결책처럼 보였고, 별로 손이 많이 가지 않는 무언가를 원했어요.

# 유레카

드디어 GitHub에서 SKAdvancedLabels라는 프로젝트에서 영감을 얻었어요. 그 저자는 세 개의 라벨을 만들고 코드에서 그들 사이를 전환하는 방식으로 작업했어요. 그러나 이 프로젝트에서 특별한 점은 각 라벨의 완전한 복사본을 세 개 만들고 그들 사이를 전환하는 것이었어요. 각 라벨이 완전하기 때문에 캐릿값이 정확했어요.

이것을 생각해보면서, 나도 마찬가지로 할 것을 결정했어요. 단, 제가 SwiftUI Views를 사용하고 각 행의 각 글자를 제외하고는 감추기 위해 속성 문자열을 사용할 거예요.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-23-TextEffectsWithSwiftUIandPhysics_1.png)

이 작업을 수행하는 코드는 선호도와 비슷해서, 쉽게 함께 조합하기 어려우나 관리할 수 있습니다. SwiftUI를 사용하여 SKAdvanceLabels 클래스 버전을 만들기 위해 View Modifier와 View Builder를 사용했습니다.

```swift
enum Actions {
    case fade
    case show
}
```

```swift
let changer = PassthroughSubject<(Actions,Int),Never>()

struct TextView: View {
    @State var pfix = 0
    @State var sfix = 6
    @State var alpha = 1
    
    @ViewBuilder
    var body: some View {
        ForEach((0..<words.count), id: \.self) { dex in
            Text(words.joined()) { string in
                if let range = string.range(of: words.joined().prefix(pfix + dex)) {
                    string[range].foregroundColor = .clear
                }
                if let range = string.range(of: words.joined().suffix(sfix - dex)) {
                    string[range].foregroundColor = .clear
                }
            }
            .modifier(Bases(dex: dex))
        }
    }
}

struct Bases: ViewModifier {
    @ObservedObject var points = Position.shared
    @State var ready = false
    @State var visible = 1.0
    @State var dex:Int
    func body(content: Content) -> some View {
        content
            .foregroundStyle(
                    LinearGradient(
                        colors: [.blue, .yellow],
                        startPoint: .top,
                        endPoint: .bottom
                    )
                )
            .foregroundColor(Color.white)
            .font(Fonts.neutonBold(size: 96))
            .position(ready ? points.pointsZ[dex]:CGPoint.zero)
            .offset(x:-24)
            .onReceive(firePublisher) { _ in
                ready = true
            }
            .opacity(visible)
            .scaleEffect(modeZoom)
            .rotation3DEffect(.degrees(rotating), axis: (x: 1, y: 0, z: 0))
            .animation(.easeIn(duration: 4), value: visible)
            .onReceive(changer) { command in
                let (action,single) = command
                if single == dex {
                    switch action {
                        case .fade:
                            visible = 0
                        case .show:
                            visible = 1
                        default:
                            assert(false,"NeverHappens")
                    }
                }
            }
    }
}
```


<div class="content-ad"></div>

이번에는 문자열, 크기, 글꼴, 심지어 알파벳을 변경해도 작동한다는 것을 확신했어요.

## 링크

이제 제가 원한 SpriteKit 그림자 뷰와 SwiftUI 뷰를 "프로퍼티 클래스"를 통해 서로 연결하고 싶어했습니다.

## 이닛

<div class="content-ad"></div>

먼저 SwiftUI 텍스트 뷰들이 배치되어 있는 방식대로 모든 스프라이트를 쌓아야 했어요. 엄밀히 말하면 매치되었을 때 글 간격이 아직 미세하게 차이가 있었어요, 한 두 점 정도로요; 균형을 유지하는 작업이 필요했어요. 제가 각 라벨 주위에 테두리를 그려놓은 이미지에서 델타를 볼 수 있어요. 테두리를 보세요; 라벨이 거의 3D 판에 부착된 것처럼 보이죠.

![Image](https://miro.medium.com/v2/resize:fit:1400/1*t2cqbCDR7woUaOcyIVT3_w.gif)

물론, 이제 SpriteKit 레이블은 읽을 수 없지만, 중요하지 않아요; 여기서 포함했던 이유는 무엇이 일어나고 있는지 이해하실 수 있도록 한 것뿐이에요. 야생에서 배경과 동일한 색상이 될 거예요 — 테두리처럼 보이지 않을 거예요.

이 애니메이션의 코드는 이렇게 생겼지만, 세부 사항에 관해선 조금 더 얘기해야 할 게 있어요.

<div class="content-ad"></div>

```js
fileprivate func initNodes() {
    firePublisherX.send(0)
    for index in 0..<self.nodes.count {
        self.nodes[index].physicsBody?.allowsRotation = false
        self.nodes[index].physicsBody?.friction = 1.0
    }
    
    let foo = SKAction.applyForce(CGVector(dx: 0.4, dy: 0), duration: 1)
    let foo2 = SKAction.applyForce(CGVector(dx: -0.4, dy: 0), duration: 1)
    nodes[0].run(foo)
    nodes[nodes.count - 1].run(foo2)
    
    copyXcordinates()
}
```

여기서 표시된 publisher는 SwiftUI 인스턴스에게 X 또는 Y 좌표에 집중하고 싶다고 알립니다.

```js
.position(readyY ? points.pointsY[dex]:CGPoint(x: screenWidth/2, y: screenHeight/2))
            .position(readyX ? points.pointsX[dex]:CGPoint(x: screenWidth/2, y: screenHeight/2))
            
            .onReceive(firePublisherX) { _ in
                readyX = true
                readyY = false
            }
            .onReceive(firePublisherY) { _ in
                readyY = true
                readyX = false
            }
```

# 복제품


<div class="content-ad"></div>

이 작업에서 가장 어려웠던 것 중 하나는 SpriteKit 좌표를 실시간으로 복사하는 것이었습니다. 포지션 클래스에서 공개된 변수를 사용했었고 클래스 자체는 싱글톤이었지만, 이곳에서의 어려움은 해당 좌표를 복사해야 하는 시간을 추적하는 것이었습니다.

아래와 같이 시작한 루틴으로 잘 작동했지만, 일부분에서 작동하지 않았습니다. 문제는 이것이 약 10초 동안 좌표를 복사했지만 그 이상은 그만뒀다는 것이었습니다.

```js
fileprivate func copyYcordinates() {
    firePublisherY.send(0)
    let foo3 = SKAction.customAction(withDuration: 10) { [self] _, elapsedTime in
            for index in 0..<self.nodes.count {
                let newCord = CGPoint(x: self.points.pointsX[index].x, y: self.nodes[index].position.y.flipCoordinate())
                self.points.pointsY[index].y = newCord.y
                self.points.pointsX[index].y = newCord.y
            }
    }
    self.run(foo3)
}
```

내가 시도했던 각 애니메이션을 작동하는데 필요한 최소한의 시간이었습니다. 하지만 난 불편했습니다. Y축 상에서 애니메이션을 두 개 연속으로 실행해야 할까? 각 애니메이션 간에 최소한 10초가 보장되어야 했거나, 두 가지 다른 루틴에서 동시에 좌표를 복사해야 했습니다. 이는 언젠가 반드시 심각한 런타임 오류/크래시를 야기할 코드였습니다.

<div class="content-ad"></div>

Timer를 사용해보려고 했지만 물리 엔진 렌더링에 혼란을 주었어요.

결국, 각 문자 위치의 좌표를 120번 복사하고 각 복사 사이에 0.01초의 일시 정지가 있는 완료 구문이 있는 SKAction을 사용하기로 결정했어요. 그런 다음 아직 움직이는 좌표가 있는지 확인하고, 그렇다면 다시 실행해야 하는지 테스트했어요. 중요한 점은 노드 간의 충돌을 감지해줘서 노드가 멈추고 다시 시작하기 때문이에요.

```js
fileprivate func copyXcordinates() {
        let foo1 = SKAction.run { [self] in
            for index in 0..<self.nodes.count {
                    self.points.pointsX[index].x = self.nodes[index].position.x
                    self.points.pointsY[index].x = self.nodes[index].position.x
            }
        }
        let foo2 = SKAction.wait(forDuration: 0.01)
        let foo3 = SKAction.repeat(SKAction.sequence([foo2,foo1]), count: 120)
        nodes[0].run(foo3, completion: { [self] in
            var speed = 0
            for node in nodes {
                speed += Int((node.physicsBody?.velocity.dx)!)
                speed += Int((node.physicsBody?.velocity.dy)!)
            }
            print("action completed \(speed)")
            if speed != 0 {
                copyXcordinates()
            } else {
                print("stopped Copy")
            }
        })
    }
```

이전에는 이론적으로 X만 변경하고 있었지만 물리 엔진은 자신만의 생각을 가지고 있기 때문에 X와 Y 값의 속도를 확인했어요.

<div class="content-ad"></div>

# 확장 및 축소

기초를 마련한 후 이제 텍스트를 애니메이션화하는 다양한 방법을 고안해볼 차례입니다. 물론, 먼저 커닝이 올바른지 확인하고 싶어졌기 때문에 이를 분리하여 다시 함께 압축했습니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*7RibrvAaxznjz-3NkyczMA.gif)

이 두 루틴으로 만든 작업들과, 악명높은 copyXcordinate 함수를 사용하는 방법을 포함하여 여기에 만든 작업들이 나와 있습니다.

<div class="content-ad"></div>

```swift
fileprivate func expandeNodes() {
    print("expandeNodes")
    for i in 1..<nodes.count {
        let limit = SKPhysicsJointLimit.joint(withBodyA: nodes[i - 1].physicsBody!, bodyB: nodes[i].physicsBody!, anchorA: nodes[i - 1].position, anchorB: nodes[i].position)
        limit.maxLength = 48
        physicsWorld.add(limit)
        nodes[i].physicsBody?.restitution = 0
        nodes[i].physicsBody?.friction = 0.9
        nodes[i].physicsBody?.linearDamping = 0.9
    }
    
    let foo = SKAction.applyImpulse(CGVector(dx: -0.1, dy: 0), duration: 0.1)
    let foo2 = SKAction.applyImpulse(CGVector(dx: 0.1, dy: 0), duration: 0.1)
    
    nodes[0].run(foo)
    nodes[nodes.count - 1].run(foo2)
    
    copyXcordinates()
}
```

여기서는 applyForce, applyImpulse 및 move와 같은 여러 가지 SKActions을 시도해 보았는데, 실제로 게임을 바꾸는 것은 처음에 변경한 physicsBody 속성들이었습니다.

```swift
fileprivate func contractNodes() {
    print("contractNodes")
    physicsWorld.removeAllJoints() // 이 작업은 또한 physics body도 제거합니다
    for node in nodes {
        addBody(textObject: node)
    }
    firePublisherX.send(0)
    nodes[3].physicsBody?.isDynamic = false
    
    let foo = SKAction.applyImpulse(CGVector(dx: -1, dy: 0), duration: 12)
    let foo2 = SKAction.applyImpulse(CGVector(dx: 1, dy: 0), duration: 12)
    nodes[0].run(foo2)
    nodes[nodes.count - 1].run(foo)
    
    copyXcordinates()
    let waiting = SKAction.wait(forDuration: 10)
    let foo3 = SKAction.customAction(withDuration: 0) { _, elapsedTime in
        self.nodes[3].physicsBody?.isDynamic = true
    }
    nodes[3].run(SKAction.sequence([waiting, foo3]))
}
```

미래를 생각해보면, 이러한 메서드 이후에 physicsBody를 재설정하는 루틴이 필수적이라는 것을 노트해두어야 합니다.


<div class="content-ad"></div>

# 좌우로 슬라이드

다음으로, 텍스트를 좌측, 우측으로 움직이고 다시 원래 위치로 돌아오는 것을 살펴보았습니다. 아래는 애니메이션된 TextEffect의 모습입니다:

![GIF](https://miro.medium.com/v2/resize:fit:1400/1*wbzLPBwa4tqvgaHS39iXDA.gif)

이 코드로 생성한 작업은 다음과 같습니다. 임시로 추가한 라인은 텍스트가 화면 가장자리를 벗어나는 것을 방지하기 위해 있습니다.

<div class="content-ad"></div>

```swift
fileprivate func slideNodesLeft() {
    addSolidCore(xPos: frame.minX + 256)
    firePublisherX.send(0)
    let foo = SKAction.run {
        for node in self.nodes {
            node.physicsBody?.affectedByGravity = false
        }
    }
    self.run(foo)
    physicsWorld.gravity = CGVector(dx: -0.5, dy: 0)
    slideNode(nodeIndex: 0)
    copyXcordinates()
}

fileprivate func slideNodesRight() {
    addSolidCore(xPos: frame.maxX - 256)
    firePublisherX.send(0)
    let foo = SKAction.run {
        for node in self.nodes {
            node.physicsBody?.affectedByGravity = false
        }
    }
    self.run(foo)
    physicsWorld.gravity = CGVector(dx: 0.5, dy: 0)
    slideNode(nodeIndex: nodes.count - 1)
    copyXcordinates()
}

fileprivate func slideNode(nodeIndex: Int) {
    let wait = SKAction.wait(forDuration: 0.25)
    let action2D = SKAction.customAction(withDuration: 1) { node, elapsedTime in
        self.nodes[nodeIndex].physicsBody?.affectedByGravity = true
    }
    let nextAction = SKAction.run {
        if self.physicsWorld.gravity.dx < 0 {
            self.slideNode(nodeIndex: nodeIndex + 1)
        } else {
            self.slideNode(nodeIndex: nodeIndex - 1)
            print("nodeIndex \(nodeIndex)")
        }
    }
    if nodeIndex < nodes.count && nodeIndex > -1 {
        nodes[nodeIndex].run(SKAction.sequence([wait, action2D, nextAction]))
    }
}

fileprivate func centreNodes() {
    firePublisherX.send(0)
    for node in nodes {
        node.physicsBody?.affectedByGravity = false
    }
    for i in 0..<nodes.count {
        let foo = SKAction.wait(forDuration: Double(i))
        let foo1 = SKAction.run {
            self.nodes[i].physicsBody?.affectedByGravity = true
        }
        let foo2 = SKAction.sequence([foo, foo1])
        self.nodes[i].run(foo2)
    }
    addSolidCore(xPos: frame.midX)
    physicsWorld.gravity = CGVector(dx: -0.4, dy: 0)
    copyXcordinates()
}
```

첫 두 경우에 재귀를 사용하고, 마지막 경우에는 간단한 루프를 사용하여 안심하고 실행합니다.

# 드롭 다운

이 루틴에는 수십 가지 가능성이 있지만, 여기서는 단 하나만 보여주어 시작할 수 있도록 하겠습니다. 이 코드는 그 물리적 속성을 변경하고 그 후에 약간의 지연이 있는 상태로 중앙에서 왼쪽으로 중력을 켭니다.


<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:1400/1*NiOtqiTcNXAI1-UoLwBl1Q.gif)

또 다른 변화로는 먼저 간격을 넓힌 다음에 떨어뜨리고 중력을 다른 순서로 추가하는 것이었습니다. SKLabelNodes를 서로 끌어 당기지 못하게 중력에 영향을 받지 않도록 하기 위해 모두 합쳤습니다.

```js
fileprivate func dropNodes() {
        firePublisherY.send(0)
        physicsWorld.removeAllJoints()
        for i in 0..<nodes.count {
            let custom = SKAction.run {
                self.nodes[i].physicsBody?.linearDamping = Double(i)/10
                self.nodes[i].physicsBody?.affectedByGravity = true
                self.nodes[i].physicsBody?.friction = 0
                self.nodes[i].physicsBody?.restitution = 0.6
            }
            nodes[i].run(custom)
        }
        copyYcordinates()
        physicsWorld.gravity = CGVector(dx: 0.0, dy: -0.5)
    }
```

여기에는 텍스트를 확장하고 몇 가지 미세한 수정을 한 후에 동일한 루틴의 또 다른 예가 있습니다. X 및 Y 값을 두 개의 다른 값 집합으로 분할하는 장점을 볼 수 있습니다. 물론 떨어뜨린 후에 SpriteKit 텍스트가 엉망이 되었지만 X 좌표만 복사하기 때문에 중요하지 않습니다.


<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:1400/1*JD_pKgqOH_cbywU90UvAEA.gif)

# SwiftUI 효과

## 페이드 아웃/페이드 인

물론, 텍스트 "JOURNAL"은 SWIFTUI 뷰의 컬렉션이므로 개별 이동과 함께 그것으로도 다양한 작업을 수행할 수 있습니다.


<div class="content-ad"></div>

<img src="https://miro.medium.com/v2/resize:fit:1400/1*KU0ieIxybje9TdGYqxckMg.gif" />

아래에는 이 코드를 실행하여 캡처한 효과가 있습니다. Combine 프레임워크를 사용하여 각 TextView에 메시지를 보내서 불투명도를 켜고 끌 수 있고, 작업 시간을 관리하기 위해 SKActions를 사용했습니다.

```js
fileprivate func fadeOutNodes(order: Bool) {
        
        var ordered = Array(0..<nodes.count)
        let unordered = ordered.shuffled()
        let fadeOut = order ? ordered : unordered
        for index in 0..<self.nodes.count {
            let wait = SKAction.wait(forDuration: Double(index) * 2)
            let custom = SKAction.customAction(withDuration: 0) { _, _ in
                changer.send((.fade, fadeOut[index]))
            }
            let sequence = SKAction.sequence([wait, custom])
            nodes[0].run(sequence)
        }
    }
    
    fileprivate func fadeInNodes(order: Bool) {
        let ordered = Array(0..<nodes.count)
        var unordered = ordered.shuffled()
        let fadeIn = order ? ordered : unordered
        for index in 0..<self.nodes.count {
            let wait = SKAction.wait(forDuration: Double(index) * 2)
            let custom = SKAction.customAction(withDuration: 0) { _, _ in
                changer.send((.show, fadeIn[index]))
            }
            let sequence = SKAction.sequence([wait, custom])
            nodes[0].run(sequence)
        }
    }
```

여기에는 코드에 포함된 몇 가지 더 많은 SwiftUI 효과를 시도했는데, 이 메서드들은 거의 동일하기 때문에 너무 자세히 설명하는 것은 별 의미가 없습니다.

<div class="content-ad"></div>

# 일반적인 코드

내 코드를 살펴보면서 Swift에서 trailing closure를 사용하여 루프를 작성할 수 있다는 것을 깨달았어요. 예전에 작성한 이 기사를 빠르게 검토한 후에 이 코드를 만들어 냈어요:

```js
enum Follow {
     case forwards
     case backwards
 }

 fileprivate func fadeOutNodes2() {
     genericNodeWalker(.backwards) { iNode2D,iDelay in
         let wait = SKAction.wait(forDuration: Double(iDelay) * 2)
         let custom = SKAction.run {
             print("index2D \(iNode2D)")
             changer.send((.fade, iNode2D))
         }
         let sequence = SKAction.sequence([wait, custom])
         self.nodes[iNode2D].run(sequence)
     }
 }

 fileprivate func genericNodeWalker(_ direction: Follow, encapsulatedMethod: (_ iNode: Int, _ delay: Int) -> Void) {
     switch direction {
         case .forwards:
             for nodeIndex in 0..<self.nodes.count {
                 encapsulatedMethod(nodeIndex, 0)
             }
         case .backwards:
             for indexNode in (0..<self.nodes.count).reversed() {
                 encapsulatedMethod(indexNode, (nodes.count - 1) - indexNode)
             }
         }
 }
```

이제 여기 올린 코드의 많은 루프를 이 인라인 조각들로 대체할 수 있어요. 그러나 거꾸로 카운트하여 루프 인덱스를 혼합하려면 처음에는 헷갈리기도 했어요.

<div class="content-ad"></div>

생각해보고, 더 이해하기 쉬운 재귀 버전을 만들기로 결정해서 그렇게 했어요.

```js
fileprivate func recursiveNodeWalker(_ direction:Follow, encapsulatedMethod: (_ iNode:Int, _ direct:Follow) -> Void) {
        switch direction {
            case .forwards:
                encapsulatedMethod(0,direction)
            case .backwards:
                encapsulatedMethod((nodes.count - 1),direction)
            }
    }
    
    fileprivate func fadeOutNodes3() {
        recursiveNodeWalker(.backwards) { iNode2D,direct  in
            self.recursiveFade(iNode2D:iNode2D,direct: direct)
        }
    }
    
    fileprivate func recursiveFade(iNode2D:Int,direct:Follow) {
        let wait = SKAction.wait(forDuration: Double(iNode2D))
        let custom = SKAction.run {
            changer.send((.fade, iNode2D))
        }
        let custom2 = SKAction.run { [self] in
            if direct == .forwards {
                if iNode2D < nodes.count - 1 {
                    recursiveFade(iNode2D: iNode2D + 1, direct: direct)
                }
            } else {
                if iNode2D > 0 {
                    recursiveFade(iNode2D: iNode2D - 1, direct: direct)
                }
            }
        }
        let sequence = SKAction.sequence([wait,custom,custom2])
        self.nodes[iNode2D].run(sequence)
    }
```

이렇게 하면 타이밍 값들을 역으로 고려할 필요가 없어요. 어쨌든 이만 해야겠어요. 평소보다 세 배나 길어진 거 같아요. 제가 쓰는 것만큼 읽는 것이 즐거우셨으면 좋겠네요. 더 많은 텍스트 효과에 대해 더 알고 싶으시면 다음을 읽어보세요.