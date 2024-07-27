---
title: "애니메이션 API를 통해 살펴보는 애플의 변화"
description: ""
coverImage: "/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_0.png"
date: 2024-07-14 00:13
ogImage: 
  url: /assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_0.png
tag: Tech
originalTitle: "Through the Ages: Apple Animation APIs"
link: "https://medium.com/better-programming/through-the-ages-apple-animation-apis-2ab5925f546b"
---


최근 존 시라쿠사의 전설적인 맥 OS X 리뷰를 우연히 처음 접했어요.

만약 여러분이 이와 생소하다면, 이 리뷰들은 1999년부터 2014년까지의 애플 주요 맥 OS X 릴리스에 대한 놀랍도록 상세한 기술 리뷰였어요. 이 리뷰들은 발표된 곳인 아스 테크니카에 따르면, "새로운 OS X 릴리스는 스티브 잡스로 시작해서 존 시라쿠사로 끝났다"고 해요. 저는 편리한 독서 목록이 제 안내자로 함께 이 리뷰들을 열심히 살펴봤어요.

가장 흥미로운 점은 무엇일까요 — 그리고 이것이 제가 이 글을 쓰게 된 진정한 원동력이 되었던 부분은, 2007년 Core Animation에 대한 부분이었어요. 스위프트 새끼로, 스위프트 3.0의 대대적인 이름 바꾸기를 가물가물 기억하는 저에게, Core Animation은 iOS 자체만큼이나 근본적인 느낌을 주며 UIKit과 SwiftUI 모두를 지탱하고 있는 것 같아요.

<div class="content-ad"></div>

코어 애니메이션이 나오기 전 세계가 존재했다는 걸 듣는 건 마치 산타클로스가 실제로 없다는 것을 깨닫는 것과 같아요. 뭐라구요? 픽셀들이 마법으로 화면을 움직이지 않았다고요? 마치 불이 나오기 전, 우리가 음식을 손으로 원자를 재구성해 요리했던 것을 깨닳는 것 같아요. 사실, 초창기 GUI에서 픽셀 작업을 해야 할 때라면 그 정도 해야 했답니다.

이 글에서는 각 시대에 Apple 개발자들에게 제공된 것을 발견하고, 각 연대별 애니메이션 API 세대가 해결한 문제들을 설명하고, 실제 오래된 애니메이션 코드를 경험해볼 수 있는 기회를 드릴게요.

# 타임라인

1989 — NeXTSTEP 세대: 디스플레이 포스트스크립트

<div class="content-ad"></div>

2001 - "애플의 부활: Quartz & OpenGL"

2007 - "현대 시대: Core Animation"

2014 - "성능 시대: Metal"

2019 - "선언형 혁명: SwiftUI"

<div class="content-ad"></div>

# 1989 — NeXTSTEP Generation: Display PostScript

오늘의 애플 소프트웨어의 맥락을 이해하기 위해 역사 속으로 우선 점프해볼까요? 1985년 애플에서 내쫓겨난 후, 스티브 잡스가 경쟁사인 넥스트를 설립했습니다.

넥스트는 객체 지향 프로그래밍과 그래픽 사용자 인터페이스에 매우 영향을 주었으며, 결국 애플에 인수되었습니다. 넥스트가 개발한 넥스트스텝 운영 체제는 맥 OS와 통합되어 맥 OS X가 되었습니다.

넥스트스텝 운영 체제는 Adobe가 개발한 디스플레이 포스트스크립트를 사용했습니다. 이것은 이전 세대의 디스플레이 레이어에 비해 상당한 발전을 가져왔습니다. 이것은 텍스트와 그래픽을 동일하게 처리할 수 있도록 하고 높은 수준의 API와 페이지 설명 언어를 통해 기기에 독립적인 특성을 소개했습니다.

<div class="content-ad"></div>

그것이 어떤 느낌이었는지 전해드리기 위해 NeXT 컴퓨터 프로그래밍 매뉴얼에서 매력적인 단편을 가져왔어요:

```js
# Application Kit에서 직사각형은 NXRect 유형의 C 구조로 지정됩니다. 모든 좌표 값은 부동 소수점 숫자로 지정되어야 하므로 부동 소수점 숫자로 NXCoord를 정의합니다:

typedef float   NXCoord;

# 하나는 x 좌표를 위한 것이고 다른 하나는 y 좌표를 위한 것인 NXCoord 변수 쌍은 한 지점을 지정합니다:

typedef struct _NXPoint {
    NXCoord     x;
    NXCoord     y;
} NXPoint;

# NXCoord 변수 쌍도 직사각형의 크기를 지정합니다:

typedef struct _NXSize {
    NXCoord     width;
    NXCoord     height;
} NXSize;

# NeXT 헤더 파일 graphics.h (appkit 하위 디렉터리에 있음)은 NXPoint와 NXSize 구조체를 결합하여 직사각형 자체를 정의합니다:

typedef struct _NXRect {
    NXPoint     origin;
    NXSize      size;
} NXRect;
```

![이미지](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_1.png)

매뉴얼에는 간단한 코드 예제와 PostScript API도 제공됩니다. 어떨 때 어떠한지요…

<div class="content-ad"></div>

친구들! 한 번 환자로 입력해 보겠습니다.

직사각형을 인스턴스화하는 방법을 알아보죠:

```js
NXRect rect = {10.0, 10.0}, {50.0, 50.0};
```

화면에 직사각형을 강조 표시하는 방법은 이렇습니다:

```js
NXHighlightRect(&rect);
```

<div class="content-ad"></div>

두 개의 직사각형이 겹치는지 확인해 봅니다:

```js
let 겹침;
겹침 = NXIntersectRect(&rect1, &rect2);
```

겹치는 직사각형의 교차점을 나타내는 새 직사각형을 만들어 봅니다:

```js
let smallrect;
smallrect = NXIntersectionRect(&rect1, &rect2);
```

<div class="content-ad"></div>

두 직사각형을 포함하는 새로운 직사각형을 만들어 보겠습니다:

```js
NXRect *bigrect;
bigrect = NXUnionRect(&rect1, &rect2);
```

![Image](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_2.png)

이 명령어들은 우리가 익숙한 것과는 비교적 저수준이었지만, 1989년의 하드웨어 제약을 고려할 때 꽤 깔끔한 시스템을 구성할 수 있었습니다.

<div class="content-ad"></div>

![link](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_3.png)

코드 고고학을 하는 것은 즐겁지만, 나의 버터플라이 키보드에 먼지가 쌓이기 시작했어요. 타이핑을 할 수 없게 되기 전에, 오늘날의 하드웨어에서 실행할 수 있는 몇 가지 API로 넘어가보려고 해요!

# 2001년 - 애플의 르네상스: Quartz & OpenGL

1996년, Apple Computer는 NeXT, 스티브 잡스, 그리고 NeXTSTEP 운영 체제를 인수했어요. 이것이 2001년에 출시된 초창기 Mac OS X의 기초가 되었죠.

<div class="content-ad"></div>

맥 OS X(읽는 방법: OS-텐)는 애플의 미래를 위한 기본적인 기반을 마련했으며, 그 자체로 OS 디자인의 혁명이었습니다. 오늘날 우리가 당연하게 여기는 모든 것인데요. 애플의 시그니처 룩 앤 필(Aqua)부터 커널(Darwin)까지!

![이미지](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_4.png)

우리 오리지널 OS의 그래픽 레이어는 세 가지 구성 요소로 구성되어 있어요:

- 낮은 수준의 디스플레이 및 렌더링에는 Quartz가 사용됩니다. 그 안에는 창이 포함돼 있어요.
- 애니메이션 및 3D 렌더링에는 OpenGL이 사용돼요.
- 오디오 및 비디오에는 Quicktime이 사용돼요. (즉, 매우 범위에서 벗어난 주제인 거예요.)

<div class="content-ad"></div>

2001년도의 OS X 프로그래밍이 어땠는지 알아보기 위해 Quartz와 OpenGL에 대해 자세히 살펴보겠습니다.

## Quartz

Quartz는 현재는 Core Graphics로 더 일반적으로 알려져 있으며, Aqua에 대한 기초 디스플레이 레이어를 형성합니다. Aqua는 우리 아키텍처 다이어그램 상단의 디자인 언어로, 오늘날 우리가 알고 사랑하는 익숙한 Apple UX를 소개했습니다. 독의 마우스 오버 확장과 창을 열고 닫을 때 생기는 유명한 지니 애니메이션은 Quartz를 사용하여 만들어졌습니다.

![image](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_5.png)

<div class="content-ad"></div>

"Quartz와 Display PostScript는 둘 다 '세대별' 디스플레이 레이어였어요 — 그것은 화면에 그려진 모양들에 대한 정보를 유지하고 있었답니다. 세대별 레이어들은 또한 벡터를 이해할 수 있어서 화면 상의 객체들을 쉽게 변형할 수 있었어요. Quartz는 PostScript 대신 Adobe의 PDF를 기본 그래픽 표현 언어로 사용했어요. 이는 색상, 글꼴 및 상호작용 전반에 걸쳐 개선점을 가져왔던 더 고급화된, 개방된 표준이었죠.

## 함께 뭔가를 그려봐요!

저는 간단한 Objective-C 앱으로 일을 진행할 거에요 — 제 Swift 동료들 죄송해요, 하지만 그 언어는 아직 13년 후에 출시될 예정이라구요!

먼저, ATTAQuartzView.h라는 헤더 파일을 만들어 볼게요. 이 파일은 제 'Animation Through The Ages Quartz View'에 대한 것인데, Objective-C에서는 네임스페이싱이 중요하거든요!"

<div class="content-ad"></div>

우선, Cocoa 프레임워크의 NSView를 상속받는 ATTAQuartzView 클래스를 선언하고, ATTAQuartzView.m 파일에 그리기 로직을 구현합니다. 

먼저, ATTAQuartzView.h 파일을 확인하겠습니다.

```objc
#import <Cocoa/Cocoa.h>

@interface ATTAQuartzView : NSView

@end
```

다음으로, ATTAQuartzView.m 파일에 아래와 같이 그리기 메서드를 구현합니다.

```objc
#import "ATTAQuartzView.h"
#import <CoreGraphics/CoreGraphics.h>

@implementation ATTAQuartzView

- (void)drawRect:(NSRect)dirtyRect {
    [super drawRect:dirtyRect];
    CGContextRef context = (CGContextRef)[[NSGraphicsContext currentContext] graphicsPort]; 
    CGContextSetRGBFillColor(context, 1.0, 0.85, 0.35, 1.0);
    CGContextFillRect(context, CGRectMake(150, 150, 200, 200));
}

@end
```

위 코드에서는 Core Graphics 컨텍스트를 생성하고, 채우기 색상을 설정한 후 해당 색상으로 직사각형을 채우는 과정을 보여드리고 있습니다.

<div class="content-ad"></div>

최종적으로, 쉬운 삶을 위해 뷰 컨트롤러의 viewDidLoad 메서드에서 이 뷰를 인스턴스화합니다:

```js
- (void)viewDidLoad {
    [super viewDidLoad];
    ATTAQuartzView *quartzView = [[ATTAQuartzView alloc] initWithFrame:NSMakeRect(0, 0, 400, 300)];
    [self.view addSubview:quartzView];
}
```

결과는? 창에 렌더링된 화려한 황금색 정사각형입니다.

![이미지](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_6.png)

<div class="content-ad"></div>

## Quartz로 애니메이션 만들기

Quartz로 애니메이션을 만들고 싶을 때 선택지가 상당히 한정되어 있습니다.

Core Animation과 SwiftUI에서 왔다면, 여러분이 익숙해진 것보다 조금 더 직접적일 수도 있습니다. NSTimer에서 코드를 실행해 화면 상 모양의 프레임을 직접 업데이트할 것입니다.

먼저, ATTAQuartzView.h 헤더 파일을 새로운 side 속성으로 업데이트해주세요:

<div class="content-ad"></div>


안녕하세요! 이전에 작성했던 코드에 대해 더 추가해보겠습니다.

ATTAQuartzView.m 구현 파일을 업데이트하여...

...시간이 지남에 따라 직사각형의 변의 길이를 변화시키는 타이머를 설정할 수 있습니다:

```objc
@implementation ATTAQuartzView
{
    NSTimer *_timer;
    CGFloat _increment;
}

- (instancetype)initWithFrame:(NSRect)frameRect {
    self = [super initWithFrame:frameRect];
    if (self) {
        _side = 50.0;
        _increment = 3.0;
        _timer = [NSTimer scheduledTimerWithTimeInterval:1.0/60.0 target:self selector:@selector(tick:) userInfo:nil repeats:YES];
    }
    return self;
}

- (void)tick:(NSTimer *)timer {
    self.side += _increment;
    if (self.side > 200.0 || self.side < 10.0) {
        _increment *= -1.0;
    }
    [self setNeedsDisplay:YES];
}

//...

@end
```

이렇게 하면 변화하는 사각형을 무한히 볼 수 있게 될 거예요! 다들 이 코드를 잘 활용하시길 바랍니다. 좋은 하루 되세요!


<div class="content-ad"></div>

...및 이 업데이트된 변의 길이를 기반으로 도형을 다시 그려주세요:

```objc
@implementation ATTAQuartzView

//...

- (void)drawRect:(NSRect)dirtyRect {
    [super drawRect:dirtyRect];
    CGContextRef context = (CGContextRef)[[NSGraphicsContext currentContext] graphicsPort];
    CGContextSetRGBFillColor(context, 1.0, 0.85, 0.35, 1.0);
    CGContextFillRect(context, CGRectMake(250 - self.side / 2.0,
                                          200 - self.side / 2.0,
                                          self.side,
                                          self.side)
                      );
}

@end
```

![Animated square](https://miro.medium.com/v2/resize:fit:990/0*wk0vjrs4NdjFIt_f.gif)

우리의 노력들이 아름답게 애니메이션 된 정사각형의 형태로 이익을 냅니다.

<div class="content-ad"></div>

API는 기본적인 벡터 모양을 그리거나 크기를 조정할 때는 매우 직관적입니다. 하지만 화면에서 여러 항목을 이동하거나 레이어 및 투명도를 처리하거나 부드러운 애니메이션 타이밍 곡선을 구현하려면 얼마나 복잡해질 수 있는지 상상해보십시오.

## OpenGL

OpenGL은 OS X 그래픽 이야기의 두 번째 기둥을 형성했습니다. OpenGL은 2D 및 3D에서 벡터 그래픽을 렌더링하기 위한 오픈 소스이자 크로스 플랫폼 API였습니다. GPU에 작업을 분산하여 그래픽을 렌더링했다는 것은 하드웨어 가속을 사용했다는 것입니다. 이것은 OS X에서 고성능 렌더링을 위한 필수 장소로 만들었습니다 (적어도 2002년 Quartz Extreme이 CPU로부터 Quartz를 해방시킨 때까지).

OpenGL의 부드러운 2D 애니메이션 기능과 빠른 GPU 렌더링은 세기 전환 시기의 개발자들이 할머니를 당황케 할 수 있을만한 하드웨어에서 고성능 앱을 만들 수 있게 했습니다. OpenGL은 이전을 그리는 동안 다음 프레임을 그리면서 원활한 프레임 속도를 보장하는 더블 버퍼링과 같은 다양한 최신 최적화 기법을 사용했습니다.

<div class="content-ad"></div>

이제 단순한 Objective-C Mac App 프로젝트를 Xcode에서 설정했어요. 함께 따라하실 수 있어요! 2004년 애플의 이 튜토리얼은 박지주느냐면서 문서화에 대해 얼마나 충분한지를 보여준 보물 같았어요.

## OpenGL 그리기 만들기

먼저, Xcode 일반 프로젝트 설정의 Frameworks, Libraries, and Embedded Content에 OpenGL을 가져와 주세요:

![img](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_7.png)

<div class="content-ad"></div>

지금, 간단한 Obj-C 헤더 파일인 ATTAOpenGLView.h를 만들 수 있습니다:

```objc
#import <Cocoa/Cocoa.h> 

@interface ATTAOpenGLView : NSOpenGLView

- (void) drawRect: (NSRect) bounds;

@end
```

다음으로, 구현 파일인 ATTAOpenGLView.m을 작성합니다:

```objc
#include <OpenGL/gl.h>
#import "ATTAOpenGLView.h"

@implementation ATTAOpenGLView

- (void) drawRect : (NSRect) bounds
{
    glClearColor(0, 0, 0, 0);
    glClear(GL_COLOR_BUFFER_BIT);
    drawAnObject();
    glFlush();
}

static void drawAnObject(void)
{
    glColor3f(0.5f, 0.8f, 0.1f);
    glBegin(GL_TRIANGLES);
    { 
        glVertex3f( -0.5, -0.5, 0.0);
        glVertex3f(  0.0,  0.5, 0.0);
        glVertex3f(  0.5, -0.5, 0.0);
    }
    glEnd();
}

@end
```

<div class="content-ad"></div>

drawRect(bounds:) 메소드는 배경 색을 검정으로 설정하고 우리의 그리기 루틴을 실행한 후, 그리기 지시를 GPU로 보내기 위해 glFlush()를 호출합니다.

drawAnObject()는 녹색 RGB 색상을 설정하고 삼각형의 꼭지점을 그립니다. glBegin(GL_TRIANGLES)은 OpenGL에게 블록 안의 꼭지점을 세 개씩 그룹화해서 삼각형으로 화면에 그리라고 지시합니다. 세 개의 꼭지점을 제공했기 때문에 단일 삼각형이 렌더링됩니다.

마지막으로 화면에 보기를 표시하기 위해, 메인 뷰 컨트롤러의 뷰를 Quartz를 볼 때와 같이 ATTAOpenGLView의 새 인스턴스로 설정했습니다.

```objective-c
@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.view = [[ATTAOpenGLView alloc] init];
}
```

<div class="content-ad"></div>

마침내 화면에 아름다운 녹색 삼각형이 그려졌습니다.

![green triangle](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_8.png)

## 애니메이션을 추가해 봅시다!

우리는 이제 NSTimer를 사용하여 다시 그리기 로직을 트리거할 수 있지만, 더 성능이 좋은 방법은 CVDisplayLink를 사용하는 것입니다. 이 방법을 통해 화면 리프레시 속도에 업데이트를 동기화하여 화면에 나타나지 않는 프레임을 렌더링하여 시계 주기를 낭비하지 않습니다.

<div class="content-ad"></div>

안녕하세요! 오늘은 좀 더 기본적인 내용을 살펴볼 시간이에요! 😃

아래는 Swift 코드로 된 내용이에요:

```swift
import OpenGLES
import CoreVideo

class ATTAOpenGLView: UIView {
    
    var displayLink: CVDisplayLink?
    var rotation: Float = 0.0
    
    static var displayLinkCallback: CVDisplayLinkOutputCallback = { (displayLink, now, outputTime, inFlags, outFlags, context) in
        autoreleasepool {
            let view = Unmanaged<ATTAOpenGLView>.fromOpaque(context!).takeUnretainedValue()
            DispatchQueue.main.async {
                view.markForRedisplay()
            }
        }
        return kCVReturnSuccess
    }
    
    func markForRedisplay() {
        self.setNeedsDisplay()
    }
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        var swapInterval: GLint = 1
        self.openGLContext.setValues(&swapInterval, for: .swapInterval)
        
        CVDisplayLinkCreateWithActiveCGDisplays(&displayLink)
        CVDisplayLinkSetOutputCallback(displayLink!, ATTAOpenGLView.displayLinkCallback, Unmanaged.passUnretained(self).toOpaque())
        CVDisplayLinkStart(displayLink!)
    }
    
    deinit {
        CVDisplayLinkStop(displayLink!)
        CVDisplayLinkRelease(displayLink!)
    }
    
    // 여기서 실제 그리기 코드 작성...
}
```

간단히 말해서, 초기화 과정에서 두 가지를 처리하고 있어요:

- NSOpenGLView의 "스왑 간격"을 1로 설정하고 있어요. 이것은 OpenGL에게 버퍼 스왑(즉, 프레임 속도)을 화면의 새로 고침 속도(즉, "v-신크" 속도)와 동기화하도록 지시해요. 용어를 떠나서, 우리는 OpenGL에게 초당 몇 프레임을 계산할 지 알려주고 있어요.
- 또한 CoreVideo에게 화면의 새로 고침 시 마다 콜백을 보내도록 지시하고 있어요. 이 콜백은 메인 스레드에서 선택기를 실행하고, 뷰를 다시 그리도록 해요. 이는 화면이 계산 중에 새로 고침되는 것을 피하고(screen tearing이라는 시각적 효과를 일으킬 수 있음), 화면에 실제로 그리기가 올바르게 일어나도록 하는 역할을 해요.

<div class="content-ad"></div>

우리 구현의 나머지는 그리 어렵지 않습니다. setNeedsDisplay가 설정될 때마다 drawRect 메서드가 호출되고, 우리의 회전을 업데이트하고, 일부 멋진 회전 로직이 추가된 drawAnObject를 업그레이드합니다.

여기서 주요한 차이점은 glRotatef를 추가한 것인데, 이로써 3D 축을 따라 매우 멋진 90년대 스러운 회전이 가능해졌습니다. 마지막으로 glPushMatrix와 glPopMatrix는 변환 매트릭스를 스택에 추가하고 제거하는데 사용됩니다. 본질적으로는 화면에 있는 다른 요소에 영향을 주지 않도록 둘레에 울타리를 쳐서 포장하는 것입니다. 이 한 형태의 경우에는 정확히 중요하지는 않지만, 예의 바른 행동입니다.

우리의 노력에 대한 보상은 고급 3D 삼각형입니다.

<div class="content-ad"></div>


![Image](https://miro.medium.com/v2/resize:fit:1400/1*01ESZfs4Kb0k3qVPe7w7mA.gif)

# 2007 — The Modern Era: Core Animation

지금부터 2007년으로 뛰어봅시다.

![Image](https://miro.medium.com/v2/resize:fit:960/1*EQFTgnY4DYXGzPq5TEgAOA.gif)

<div class="content-ad"></div>

The iPhone has just been released, and there's no App Store yet for another year. Apple platform developers can only create applications for Mac OS X, but they encounter a challenge - animation is tough work.

<div class="content-ad"></div>

Quartz(Quartz 는 모양을 만들고 OpenGL은 만든 모양을 애니메이션화하기 위해 필요한 것들을 보셨죠. 독지니와 같은 수준의 애니메이션을 구현하기 위해서는 인디 개발자들이 겪어야 할 어려움에 대해 생각해보세요.

2007년, Mac OS X Leopard에는 모든 것을 바꾼 반짝거리는 새로운 프레임워크인 Core Animation이 포함되었습니다. 갑자기 Apple 플랫폼 개발자들은 원하는 최종 상태를 서술하는 선언적인 프레임워크를 갖게 되었고, 시스템은 각 프레임을 그 상태에 도달할 때까지 그려냈습니다.

이 프레임워크는 그 상태로 이동하는 경로를 최적화하며, 밀접하게 통합된 Mac 하드웨어 - 포함하여 GPU까지 - 를 활용하여 부드러운 전환을 보장했습니다. Core Animation의 합리적인 기본 설정은 모든 Apple 플랫폼 앱에서 일관된 외관과 느낌을 만들어냈습니다.

<div class="content-ad"></div>

일부는 QuartzCore로 불리는 Core Animation은 프레임, 테두리, 필터, 그림자, 불투명도 및 마스크 등 여러 속성을 사용하여 가벼운 레이어를 통해 작동했으며, 거의 노력없이 애니메이션을 쉽게 만들 수 있었습니다. 또한 Core Animation은 비디오, Quartz 및 OpenGL을 통합하여 그래픽 요소를 함께 작동할 수 있게 했으며, 모든 그래픽 요소가 갑자기 "그저 작동"했습니다.

이것은 "현대 시대"의 시작을 알리는 것인데, Core Animation은 오늘 우리가 작성하는 UI의 기초를 이룹니다. AppKit, UIKit 및 SwiftUI를 포함합니다.

## Core Animation으로 개발하기

이제 현대 Core Animation 레이어를 직접 다루고 있으므로 기본 Xcode 템플릿을 사용하기가 상당히 쉽습니다. 심지어 프로젝트 템플릿의 기본 NSViewController에서 CALayer를 그릴 수도 있습니다. 여기에서 따라오거나 샘플 코드를 복제해보세요.

<div class="content-ad"></div>

ViewController.m 파일의 맨 위에는 Core Animation을 import하는 것을 잊지 마세요. Mac과 iOS에서 Core Animation은 QuartzCore란 이름의 하나의 framework에 Core Image와 함께 번들로 제공됩니다.

```objective-c
#import <QuartzCore/QuartzCore.h>
```

이제 모든 작업은 우리의 뷰 컨트롤러의 `viewDidLoad` 메서드에 들어갈 것입니다. 전에 언급했듯이, NSView는 Core Animation보다 훨씬 오래되었습니다. 그러므로, NSView를 레이어 백드로 만들기 위해 다음을 호출하여 옵트인해야 합니다.

```objective-c
[self.view setWantsLayer:YES];
```

<div class="content-ad"></div>

만약 UIViewController에 익숙하시다면, 기본적으로 CALayer로 뒷받침된 것으로 인식하지 못할 수도 있습니다.

바로 시작해 봅시다!

먼저, 원래 CALayer인 간단한 원을 그려봅시다.

```objc
// Circle 모양의 layer
CAShapeLayer *circleLayer = [CAShapeLayer layer];
circleLayer.frame = CGRectMake(self.view.bounds.size.width/2, self.view.bounds.size.height/2, 150, 150);
circleLayer.cornerRadius = 75;
circleLayer.backgroundColor = [NSColor whiteColor].CGColor;
[self.view.layer addSublayer:circleLayer];
```

<div class="content-ad"></div>

![Animation](https://miro.medium.com/v2/resize:fit:1400/1*V2A0j0ds8fQiOpdVOmyTqQ.gif)

이제 간단한 크기 조절 애니메이션을 추가해 보겠습니다. 이전 세대의 그래픽 API에 있던 것과는 다르게, 최신 시대에서는 레이어의 시작과 끝 상태를 지정할 수 있어 Core Animation이 필요한 프레임을 자동으로 계산해 줍니다!

```js
// 크기 조절 애니메이션
CABasicAnimation *scaleAnimation = [CABasicAnimation animationWithKeyPath:@"transform.scale"];
scaleAnimation.fromValue = @1.0;
scaleAnimation.toValue = @1.5;
scaleAnimation.duration = 1.0;
scaleAnimation.autoreverses = YES;
scaleAnimation.repeatCount = HUGE_VALF;
[circleLayer addAnimation:scaleAnimation forKey:@"scaleAnimation"];
```

솔직히 말해서 여기서 설명해야 할 건 별로 없습니다. [CABasicAnimation animationWithKeyPath:@"transform.scale"]를 사용하여 Core Animation에 어떤 유형의 애니메이션을 만들고 싶은지 알립니다.

<div class="content-ad"></div>

시작 상태와 끝 상태를 설정했습니다. 우리는 Core Animation에게 애니메이션을 자동으로 되감는 것을 요청했어요 — 시작부터 끝까지 그리고 다시 되감는 것이죠. 그리고 HUGE_VALF 횟수만큼 반복하도록 했어요.

![Circle Animation](https://miro.medium.com/v2/resize:fit:1400/1*er_yFfg3PeDW_kPjE5MWgg.gif)

그리고 다음으로 y축을 따라 회전하는 애니메이션을 추가했어요. 이전 스케일 애니메이션과 결합해서, 이것은 화면에서 당신 쪽으로 움직이는 원의 3D 효과를 줍니다. 마치 뒤집힌 동전처럼요.

```objective-c
// 회전 애니메이션
CABasicAnimation *circleYRotation = [CABasicAnimation animationWithKeyPath:@"transform.rotation.y"];
circleYRotation.toValue = [NSNumber numberWithFloat:M_PI * 2.0];
circleYRotation.duration = 4.0;
circleYRotation.repeatCount = HUGE_VALF;
[circleLayer addAnimation:circleYRotation forKey:@"rotationAnimation"];
```

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:1400/1*Sp4EZ_X7ucwsyesIcuW5YQ.gif)

코어 애니메이션에서는 이 두 가지 애니메이션을 레이어에 적용할 수 있고, 매끄럽게 함께 조합할 수 있습니다.

다음으로, 그라데이션 효과를 주기 위해 하위 레이어를 추가하고 모양 레이어의 경계에 고정시킬 수 있습니다. 이 하위 레이어로 설정하면 부모 모양 레이어에 적용된 모든 애니메이션이 매끄럽게 적용됩니다.

```js
// 그라데이션 하위 레이어
CAGradientLayer *gradientLayer = [CAGradientLayer layer];
gradientLayer.frame = circleLayer.bounds;
gradientLayer.colors = @[(__bridge id)[NSColor redColor].CGColor, (__bridge id)[NSColor yellowColor].CGColor];
gradientLayer.startPoint = CGPointMake(0, 0.5);
gradientLayer.endPoint = CGPointMake(1, 0.5);
gradientLayer.cornerRadius = circleLayer.cornerRadius;
[circleLayer addSublayer:gradientLayer];
```

<div class="content-ad"></div>

<img src="https://miro.medium.com/v2/resize:fit:1400/1*uR4VoY3SKMODZWJdSRaKlA.gif" />

마침내, 서브레이어에서 그라데이션 색상을 변조하는 애니메이션을 추가할 수 있습니다.

```js
// 색상 변경 애니메이션
CABasicAnimation *colorChange = [CABasicAnimation animationWithKeyPath:@"colors"];
colorChange.toValue = @[(__bridge id)[NSColor blueColor].CGColor, (__bridge id)[NSColor greenColor].CGColor];
colorChange.duration = 2.0;
colorChange.autoreverses = YES;
colorChange.repeatCount = HUGE_VALF;
[gradientLayer addAnimation:colorChange forKey:@"colorChangeAnimation"];
```

<img src="https://miro.medium.com/v2/resize:fit:1400/1*s4y0-fwvJrDenBzF9u6qOA.gif" />

<div class="content-ad"></div>

몇 일 동안 Display PostScript, Quartz 및 OpenGL을 해독하려고 시간을 보내면서, 2019년에 SwiftUI를 처음 사용한 느낌과 같습니다. 그냥 CALayer에 무엇을 해야 하는지 알려주면... 그저 작동합니다 🥹.

# 2014 — 성능 시대: Metal

일곱 년이 또 지난 뒤.

스티브 잡스는 사망했습니다. iPhone은 세계를 점령했습니다.

<div class="content-ad"></div>

코어 애니메이션 - 그리고 그 위에 구축된 UI 프레임워크, UIKit - WhatsApp, Uber 및 Candy Crush와 같은 앱 거인들을 낳았습니다. 그러나 첨단 모바일 게임을 개발하는 엔지니어들에게는 서서히 발생하는 문제가 있었습니다.

## 코어 애니메이션의 문제점은 무엇인가요?

코어 애니메이션에서 제공하는 고수준 추상화는 쉽게 시작하고 훌륭한 것을 만들 수 있게 해 주지만, 추상화에는 비용이 따릅니다. 특히, 모든 시계 주기를 세밀하게 관리하지 않을 때에는 효율성 대신 편의성으로 대가를 치루게 됩니다.

코어 애니메이션은 UIButton의 불투명도를 애니메이션화하거나 UITableView 셀을 끌어다니게 하는 것과 같은 쿠키 커터 작업에 유용합니다. 그러나 스페이스 슈터, 급속한 레이싱 게임 또는 완전히 몰입할 수 있는 농장 시뮬레이터를 렌더링해야 하는 경우는 어떨까요?

<div class="content-ad"></div>

OpenGL ES (Open Graphics Library for Embedded Systems)은 iOS에서 고성능 그래픽 API의 표준이었습니다. 이는 화면에 정확히 무엇을 렌더링하는지에 대해 세밀한 제어를 제공하는 저수준 3D 그래픽 API였습니다.

## OpenGL ES는 멋져보이네요. 문제가 무엇인가요?

OpenGL ES는 제 3자 라이브러리였습니다. 이는 소프트웨어가 GPU를 통해 그래픽 하드웨어와 상호 작용하기 위해 드라이버를 통해 연결되었다는 의미였습니다. 드라이버는 하드웨어와의 인터페이스를 저수준 명령어를 통해 생성하는 소프트웨어입니다. 이러한 그래픽 라이브러리 드라이버 자체도 CPU에서 실행되어야했습니다.

2010년대 초반에는 그래픽 칩의 성능이 빠르게 향상되고 있었습니다. 그들의 성능은 마침내 CPU의 성능을 앞서가기 시작했습니다. 이는 느린 CPU 하드웨어에서 실행되는 OpenGL ES 드라이버가 GPU에서 실행되는 그래픽 코드의 성능을 제한하는 병목 현상으로 이어졌습니다.

<div class="content-ad"></div>

## 메탈의 주요 개념

코딩을 시작하기 전에, 메탈 프레임워크가 실제로 어떻게 작동하는지에 대한 간단한 개념적 개요를 함께 살펴봅시다. 아직 그래픽 파이프라인 작업을 한 적이 없다면 약간 압도당할 수도 있는데, 걱정하지 마세요.

- 쉐이더(Shaders)는 렌더링을 처리하는 CPU에서 실행되는 작은 함수들입니다. 이름은 역사적으로, 상, 중 OpenGL 시대에는 그들이 단순히 벡터 모양의 음영을 제어했기 때문에 그렇게 불렸습니다.
- 버텍스 쉐이더(Vertex shaders)는 3차원 좌표를 2차원 좌표로 변환하여, 평면과 같은 것에 잘 매핑될 수 있도록 합니다 — 예를 들어 당신의 화면.
- 프래그먼트 쉐이더(Fragment shaders), 일명 픽셀 쉐이더,는 모양이 래스터화 될 때 화면의 개별 픽셀에 색상을 입힙니다 — 즉, 2D 벡터 이미지가 화면 상의 점으로 변환되었을 때입니다.
- Metal Shading Language은 이러한 쉐이더를 작성하는 데 사용하는 언어입니다. 이는 C++ 2011을 기반으로 하며, 여러 언어 기능이 다운으로 되어 있고, 그래픽 중심 기능들이 섞여 있습니다.
- MTLDevice는 GPU 자체의 성연한 하드웨어를 나타내는 MetalKit의 추상화입니다.
- MTLCommandBuffer는 그려야 할 명령이 대량 병렬로 실행되기 전에 저장되는 장소입니다.
- MTLCommandQueue는 상기 명령 버퍼를 관리하고 실행 순서를 유지합니다.

<div class="content-ad"></div>

당신의 두통이 있나요?

좋아요, 그것은 당신이 배우고 있다는 것을 의미합니다!

## 코드를 작성해봅시다

드디어 ✨ 미래의 ✨ 에 도달했으니, Swift에서 실행되는 iOS 앱을 만들 수 있습니다. 저와 함께 코드를 작성해 보거나, 샘플 프로젝트를 직접 확인해보세요.

<div class="content-ad"></div>

이미 익숙하실 것 같지만, Xcode 프로젝트에 Metal 및 MetalKit 프레임워크를 추가하는 것부터 시작해 보겠습니다:

![Image](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_9.png)

이제 본 행사에 참석해 보겠습니다. 프로젝트에 Shaders.metal 파일을 추가해야 합니다. 이는 앞서 언급한 C++ 기반 Metal Shading Language입니다. 매우 간결하며 몇 줄에 많은 의미가 담겨 있습니다.

```js
#include <metal_stdlib>
using namespace metal;

vertex float4 vertex_main(constant float4* vertices [[buffer(0)]], uint vid [[vertex_id]]) {
    return vertices[vid];
}

fragment float4 fragment_main() {
    return float4(1, 0, 0, 1);
}
```

<div class="content-ad"></div>

- metal_stdlib은 셰이더를 쓸 때 유용한 도우미 함수와 타입을 포함하고 있어요.
- using namespace metal을 사용하면 네임스페이스를 설정하고 Metal 프레임워크 이름을 모든 메서드 앞에 붙이지 않아도 된답니다.
- vertex_main 함수는 vertex float4로 시작해서 3D 좌표를 화면에 매핑하고 4D 벡터(네 개의 숫자)를 반환하는 버텍스 셰이더로 표시돼 있어요. 이는 GPU에게 그리려는 벡터 데이터를 어디서 찾아야 하는지 알려주죠.
- fragment_main 함수는 화면의 픽셀을 색칠하는 프래그먼트 셰이더로, 여기서는 단순히 RGB 형식으로 빨간색을 반환합니다.

## Metal을 위한 Swift API들

조금 더 편한 마음으로 준비해주세요. 지금부터는 조금 더 익숙한 Swift API들이 돌아옵니다. 먼저, MetalView.swift를 설정해볼게요:

```swift
final class MetalView: MTKView {

    var commandQueue: MTLCommandQueue!
    var pipelineState: MTLRenderPipelineState!
    
    override init(frame frameRect: CGRect, device: MTLDevice?) {
        super.init(frame: frameRect, device: device)
        commandQueue = device?.makeCommandQueue()!
        createRenderPipelineState()
        clearColor = MTLClearColor(red: 0.8, green: 0.8, blue: 0.8, alpha: 1.0)
    }

    required init(coder: NSCoder) {
        fatalError()
    }

// ...
```

<div class="content-ad"></div>

MTKView은 간단한 직사각형 프레임과 MTLDevice로 초기화됩니다. 앞서 언급한 대로, 이는 GPU를 나타내는 객체입니다.

UIKit과 함께 작업하므로 이 뷰를 프로젝트 템플릿의 ViewController.swift에 추가하는 것이 아주 쉬워졌어요:

```swift
import UIKit
import MetalKit

final class ViewController: UIViewController {
    
    var metalView: MetalView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        guard let metalDevice = MTLCreateSystemDefaultDevice() else {
            fatalError("이 장치에서 Metal이 지원되지 않습니다")
        }

        metalView = MetalView(frame: view.bounds, device: metalDevice)
        view.addSubview(metalView)
    }
}
```

MetalKit의 MTLCreateSystemDefaultDevice 메서드는 프레임워크가 GPU 하드웨어를 찾아 해당 하드웨어를 나타내는 metalDevice 객체를 반환하게 합니다. 여기에 MetalView를 초기화하고 렌더링 파이프라인을 설정합니다.

<div class="content-ad"></div>

메탈 렌더링 작업을 시작하려면 MetalView 내의 두 가지 중요한 컴포넌트를 더 채워야 합니다. createRenderPipelineState() 및 draw(_ rect: CGRect) 함수입니다.

먼저, 렌더 파이프라인을 설정해보겠습니다:

```swift
func createRenderPipelineState() {
    let library = device?.makeDefaultLibrary()
    let vertexFunction = library?.makeFunction(name: "vertex_main")
    let fragmentFunction = library?.makeFunction(name: "fragment_main")
    
    let pipelineDescriptor = MTLRenderPipelineDescriptor()
    pipelineDescriptor.vertexFunction = vertexFunction
    pipelineDescriptor.fragmentFunction = fragmentFunction
    pipelineDescriptor.colorAttachments[0].pixelFormat = .bgra8Unorm
    
    pipelineState = try? device?.makeRenderPipelineState(descriptor: pipelineDescriptor)
}
```

여기에서는 GPU에 Shaders.metal 파일에서 사용할 정점 및 프래그먼트 셰이더 함수를 알려줍니다. 그런 다음 정점 및 색상을 가져와 화면 픽셀로 처리하는 렌더링 파이프라인을 설정합니다.

<div class="content-ad"></div>

이 문서에는 다룰 내용이 많지만, 이 예제는 꽤 간단합니다. 고급 Metal 게임 엔진은 텍스처, 조명, 테셀레이션, 모핑, 시각 효과, 안티 앨리어싱 등을 렌더링 파이프라인에서 처리합니다.

마지막으로, 벡터 그리기를 설정하기 위해 draw(_ rect: CGRect)를 오버라이드합니다.

```swift
override func draw(_ rect: CGRect) {
    guard let drawable = currentDrawable,
          let renderPassDescriptor = currentRenderPassDescriptor else { return }
    
    let commandBuffer = commandQueue.makeCommandBuffer()!
    
    let renderEncoder = commandBuffer.makeRenderCommandEncoder(descriptor: renderPassDescriptor)!
    renderEncoder.setRenderPipelineState(pipelineState)
    
    let vertices: [SIMD4<Float>] = [
        [-0.8, -0.4, 0.0, 1.0],
        [ 0.8, -0.4, 0.0, 1.0],
        [ 0.0,  0.4, 0.0, 1.0]
    ]
    
    renderEncoder.setVertexBytes(vertices, length: vertices.count * MemoryLayout<SIMD4<Float>>.size, index: 0)
    renderEncoder.drawPrimitives(type: .triangle, vertexStart: 0, vertexCount: 3)
    renderEncoder.endEncoding()
    commandBuffer.present(drawable)
    commandBuffer.commit()
}
```

이 스크립트는 몇 가지 일을 수행하지만, 주로 다음을 수행합니다:

<div class="content-ad"></div>

- 렌더링할 수 있는 화면 표면이 있는지 확인합니다.
- GPU에 명령을 배치할 새로운 명령 버퍼를 만듭니다.
- 삼각형의 꼭지점을 나타내는 4D 벡터 배열을 설정합니다. 이는 Shaders.metal 파일의 vertex shader 메서드 인자와 일치합니다.
- 주어진 꼭지점을 기반으로 삼각형을 그리도록 렌더에 지시합니다. 앞서 언급했던 것처럼, 이것은 glBegin(GL_TRIANGLES)를 사용하여 OpenGL 렌더링 프로세스와 매우 유사하게 작동합니다.
- 마지막으로, 우리는 버퍼에 작업을 마쳤다고 알리고 실행을 위해 명령을 GPU에 보내도록 합니다.

이 튜토리얼은 다소 깊이가 있을 수 있지만, 4,500 단어 후에 지금은 헌신한 상태입니다. 그리고 이런 놀라운 결과를 보세요!

![Tutorial Result](https://miro.medium.com/v2/resize:fit:1400/1*hzrcTgMzDCABoSng270pxw.gif)

맞아요, 여러분. 회색 배경 위에 빨간색 삼각형입니다.

<div class="content-ad"></div>

만약 우리가 추상화 관점에서 한 걸음 물러나는 느낌을 받는다면, 맞습니다. Metal은 Core Animation보다 하위 수준의 API이기 때문에 하드웨어에게 정확히 무엇을 하길 원하는지 좀 더 구체적으로 명시해야 합니다. 이는 하드웨어가 정확히 무엇을 수행하는지에 대해 구체적으로 명시하는 더 기본적이고 저수준의 명령을 의미합니다.

반면에 Core Animation의 선언적 구문은 우리가 무엇을 하길 원하는지 명시하고, 시스템에게 상태 변경을 어떻게 렌더링할지에 대한 권한을 줍니다.

## Metal에 속도를 내볼까요?

...그리고 애니메이션을 도입해 봅시다.

<div class="content-ad"></div>

먼저, Shaders.metal 파일에서 시간 뿐만 아니라 4D 버텍스를 다룰 수 있도록 shader 함수를 업데이트해봅시다. 개선된 버전은 일련의 동기화된 사인파에 따라 색상을 변조할 것입니다.

```swift
#include <metal_stdlib>
using namespace metal;

struct VertexOut {
    float4 position [[position]];
    float time;
};

vertex VertexOut vertex_main(constant float4* vertices [[buffer(0)]], uint vid [[vertex_id]], constant float &time [[buffer(1)]])
{
    VertexOut out;
    out.position = vertices[vid];
    out.time = time;
    return out;
}

fragment float4 fragment_main(VertexOut in [[stage_in]])
{
    float r = sin(in.time) * 0.5 + 0.5;
    float g = sin(in.time + 2.0) * 0.5 + 0.5;
    float b = sin(in.time + 4.0) * 0.5 + 0.5;
    return float4(r, g, b, 1.0);
}
```

우리의 MetalView.swift 파일도 이러한 쉐이더에 시간 인자를 보내도록 업데이트해봅시다. 속성과 초기화자를 업데이트하세요:

```swift
final class MetalView: MTKView {

    var commandQueue: MTLCommandQueue!
    var pipelineState: MTLRenderPipelineState!
    var displayLink: CADisplayLink!
    var startTime: CFTimeInterval?
    var time: Float = 0.0

    override init(frame frameRect: CGRect, device: MTLDevice?) {
        super.init(frame: frameRect, device: device)
        commandQueue = device?.makeCommandQueue()!
        createRenderPipelineState()
        clearColor = MTLClearColor(red: 0.8, green: 0.8, blue: 0.8, alpha: 1.0)
        displayLink = CADisplayLink(target: self, selector: #selector(update))
        displayLink.add(to: .current, forMode: .default)
    }

//...
```

<div class="content-ad"></div>

여기서 우리는 이전 2001년 친구인 CADisplayLink를 다시 만나게 되었는데, 이번에는 약간 더 귀여운 API와 함께입니다. 새로운 업데이트 메서드는 디스플레이가 각 화면을 새로 고칠 때마다 화면을 초기화하고 모양을 다시 그리도록 보장합니다:

```js
@objc func update(displayLink: CADisplayLink) {
    if startTime == nil {
        startTime = displayLink.timestamp
    }
    
    let elapsed = displayLink.timestamp - startTime!
    time = Float(elapsed)
    
    self.setNeedsDisplay()
}
```

createRenderPipelineState 메서드에서 렌더링 파이프라인을 설정하기 위해 작성한 보일러플레이트는 전혀 변경되지 않았고, 그리기 로직도 많은 작업이 필요하지 않습니다:

```js
override func draw(_ rect: CGRect) {
    guard let drawable = currentDrawable,
          let renderPassDescriptor = currentRenderPassDescriptor else { return }
    
    let commandBuffer = commandQueue.makeCommandBuffer()!
    
    let renderEncoder = commandBuffer.makeRenderCommandEncoder(descriptor: renderPassDescriptor)!
    renderEncoder.setRenderPipelineState(pipelineState)
    
    let vertices: [SIMD4<Float>] = [
        [-0.8, -0.4, 0.0, 1.0],
        [ 0.8, -0.4, 0.0, 1.0],
        [ 0.0,  0.4, 0.0, 1.0]
    ]
    
    renderEncoder.setVertexBytes(vertices, length: vertices.count * MemoryLayout<SIMD4<Float>>.size, index: 0)
    renderEncoder.setVertexBytes(&time, length: MemoryLayout<Float>.size, index: 1)
    renderEncoder.drawPrimitives(type: .triangle, vertexStart: 0, vertexCount: 3)
    renderEncoder.endEncoding()
    commandBuffer.present(drawable)
    commandBuffer.commit()
}
```

<div class="content-ad"></div>

여기서 유일한 차이점은 renderEncoder.setVertexBytes에 새로운 호출을 추가하면서 &time 인수를 추가했습니다. 이를 통해 프래그먼트 셰이더가 시간에 따라 색상을 변조할 수 있게 되었죠.

결과는? 진정한 방사성 도리토 입니다. GPU는 힘겹게 일하지 않아요.

![image](https://miro.medium.com/v2/resize:fit:1400/1*kk2B64g6jN0LJu0ipWGfEA.gif)

## 더 나아가기

<div class="content-ad"></div>

이제 MetalView에서 CADisplayLink 로직을 구성했으며, renderEncoder.setVertexBytes(&time, length: MemoryLayout`Float`.size, index: 1)를 통해 셰이더로 시간 참조를 보낼 수 있습니다. 또한 꼭짓점 셰이더의 코드를 업데이트함으로써 더 많은 애니메이션을 구현할 수 있습니다:

```js
float4x4 rotationMatrix(float angle, float3 axis) {
    float c = cos(angle);
    float s = sin(angle);
    float3 normalized = normalize(axis);
    float3 temp = (1.0 - c) * normalized;

    float4x4 rotation = {
        {c + temp.x * normalized.x, temp.x * normalized.y + s * normalized.z, temp.x * normalized.z - s * normalized.y, 0.0},
        {temp.y * normalized.x - s * normalized.z, c + temp.y * normalized.y, temp.y * normalized.z + s * normalized.x, 0.0},
        {temp.z * normalized.x + s * normalized.y, temp.z * normalized.y - s * normalized.x, c + temp.z * normalized.z, 0.0},
        {0.0, 0.0, 0.0, 1.0}
    };

    return rotation;
}

vertex VertexOut vertex_main(constant float4* vertices [[buffer(0)]], uint vid [[vertex_id]], constant float &time [[buffer(1)]]) {
    VertexOut out;
    float4x4 rotation = rotationMatrix(time, float3(0.0, 0.0, 1.0));
out.position = rotation * vertices[vid];
    out.time = time;
    return out;
}
```

여기서는 시간에 따라 변하는 회전 행렬을 만들어 z축 회전을 표시하고 있습니다. 삼각형의 크기는 화면 프레임을 기반으로 하기 때문에, 이는 놀랍도록 멋진 아시드 트립 결과를 냅니다.

![acid trip](https://miro.medium.com/v2/resize:fit:1400/1*L--cPy_fTtp9tAbEDDEPwg.gif)

<div class="content-ad"></div>

독자 여러분, 이제 Metal, OpenGL ES 또는 Vulkan과 같은 3D 그래픽 프레임워크 전문가가 되어야만 대단한 iOS 앱을 만들 수 있다는 것은 아닙니다. 이러한 기술은 3D 게임 개발에도 직접 사용되지 않습니다. Metal에 대한 신비한 지식은 대부분 게임 엔진 개발자들 (그리고 최근에는 머신 러닝 엔지니어들)이 보유하고 있습니다.

만약 Metal에 대해 익숙하지 않았다면, 이 부분의 심오한 성격은 Core Animation (그리고 Quartz의 일정 부분)의 성취에 대한 증거입니다. 낮은 수준의 그래픽 전문 지식과 앱 개발이 분리되어 있는 것입니다. 오늘날 놀라운 소프트웨어를 만들기 위해 해당 지식이 반드시 필요한 것은 아니지만, 당신은 이러한 도구 위에 무의식적으로 기반을 쌓고 있을 수 있습니다.

애플의 그래픽 및 애니메이션 API 역사를 통해 여행한 후, 앞으로를 바라보는 것이 적절할 것입니다. 당연히 말이죠…

## … SwiftUI에 관한 이야기.

<div class="content-ad"></div>

# 2019 — 선언적 혁명: SwiftUI

자, 마지막으로 현재로 달려가볼까요?

2013년에 React는 페이스북의 무게를 전면에 내세워 2010년대 초반의 계속 변화하는 혼돈 속으로 내던졌습니다. React는 UI를 상태의 함수로 취급하고, 고성능을 위해 가상 DOM을 도입했으며, 컴포넌트 기반의 구성형 렌더링을 사용했습니다.

![Image](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_10.png)

<div class="content-ad"></div>

Apple은 2000년대 웹에 의해 Microsoft Windows가 점차 약화되는 것을 목격했으며, iOS에서도 웹을 사용하는 것을 제한하기 위해 최선을 다하고 있었습니다.

그러나 성문 앞에야 야만인이 있었습니다!

페이스북의 React Native은 2015년에 표준 React 구문을 사용하여 크로스 플랫폼 앱 컴파일을 소개했습니다. 이후 2017년에는 구글이 Dart 언어를 사용하여 웹, 안드로이드 및 iOS에서 React 스타일 UI 개발을 가능케 하는 Flutter를 소개했습니다. 두 접근 방식 모두 UI의 핫 리로딩과 같은 개발자 친화적인 특징을 가지고 있었습니다.

![Image](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_11.png)

<div class="content-ad"></div>

이번 대회는 iPhone (UIKit), Mac (AppKit) 및 Apple Watch (WatchKit)에 걸친 불편한 플랫폼 분할과 결합되어, 신입 엔지니어들에게는 나쁜 장기적인 선택처럼 보였습니다.

2019년에 Apple 플랫폼 UI 개발의 미래로 발표된 SwiftUI는 생산 준비가 됐는지에 대한 몇 년 동안의 논쟁을 촉발했습니다. (준비가 됐습니다.)

SwiftUI는 모든 Apple 플랫폼에서 UI를 만들기 위한 선언적 프레임워크였습니다. Core Animation 및 Metal 기반으로 구축되었으며, 이전보다 애니메이션을 훨씬 쉽게 만들었습니다.

![이미지](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_12.png)

<div class="content-ad"></div>

잘 따라오고 계시다면, 이미 2040년대에 있는 것 같지는 않습니다. Vision Pro 프로젝트를 Generative AR로 렌더링하려 한다면 SwiftUI 앱을 설정하는 방법에 대해 말씀드릴 필요가 없을 것입니다.

바로 실습으로 넘어가 보죠.

언제나처럼, 이 미니 프로젝트의 샘플 코드는 GitHub에서 확인하실 수 있습니다.

## SwiftUI 1.0: 기본 애니메이션

<div class="content-ad"></div>

SFSymbols 카탈로그를 통해 영감을 얻어 이 숫자 dumbbell.fill을 발견했어요. 여기에 스포티한 주제가 잘 어울릴 것 같아요!

![Through the Ages Apple Animation APIs](/assets/img/2024-07-14-ThroughtheAgesAppleAnimationAPIs_13.png)

SwiftUI에서 애니메이션은 정말로 상태의 기능입니다. 상태 변경을 애니메이트할 때 상태에 의존하는 UI는 새로운 상태를 나타내기 위해 자연스럽게 애니메이션됩니다. 이 정도의 선언적 특성은 Core Animation조차 부끄러워할지도 모르겠어요!

ContentView.swift에서 화면 아래쪽에 간단한 덤벨 이미지를 넣어 시작해볼 수 있어요.

<div class="content-ad"></div>

```swift
import SwiftUI

struct ContentView: View {

    var body: some View {
        Image(systemName: "dumbbell.fill")
            .imageScale(.large)
            .foregroundColor(.accentColor)
            .frame(maxWidth: .infinity, maxHeight: .infinity, alignment: .bottom)
    }
}
```

한 쪽에는 철봉을 들고 있다가 다른 한 쪽에는 다음 철봉을 들고 있는 것처럼 번갈아가며 반복하는 애니메이션을 만들고 싶어요. 이걸 구현하기 위해 두 가지 구성 요소가 필요합니다:

- 수직 이동
- 이미지 확대/축소

scaleEffect 수정자를 사용하고 frame 수정자의 alignment 속성을 함께 사용하여 이를 구현할 수 있어요.


<div class="content-ad"></div>

```swift
struct ContentView: View {

    @State private var isAnimated: Bool = false
    
    var body: some View {
        Image(systemName: "dumbbell.fill")
            .imageScale(.large)
            .foregroundColor(.accentColor)
            .scaleEffect(isAnimated ? 4 : 1)
            .frame(maxWidth: .infinity,
                   maxHeight: .infinity,
                   alignment: isAnimated ? .top : .bottom)
            .animation(loopingAnimation, value: isAnimated)
            .onAppear {
                isAnimated = true
            }
    }
    
    private var loopingAnimation: Animation {
        .easeInOut(duration: 0.75)
        .repeatForever(autoreverses: true)
    }
}
```

여기서는 animation 수식자를 사용하여 암시적 애니메이션을 생성합니다. 이는 SwiftUI 렌더링 엔진에게 지정된 값이 업데이트될 때 상태 변경을 애니메이트하도록 알려줍니다. 여기서는 onAppear에서 설정하는 간단한 isAnimated 속성을 값으로 사용하고, Animation에서 repeatForever를 사용하여 루프를 보장합니다. 이제 깔끔한 끌어올리기 효과를 얻습니다.

![Lifting Animation](https://miro.medium.com/v2/resize:fit:1400/1*kObTsi8A0sdMIeaHXjdMLA.gif)

더 나아가, 이것을 더 멋지게 만들고 코드를 정리할 수 있습니다.


<div class="content-ad"></div>

만약 여러분이 저에 대해 알고 있다면, 제가 enum을 사용하는 것을 좋아한다는 것을 아실 겁니다. 여기서, Rep enum을 생성하여 각 state를 나타내고 각 state에 대한 시각적 효과를 저장합니다.

```swift
enum Rep {
    case left
    case right
    
    mutating func lift() {
        switch self {
        case .left: self = .right
        case .right: self = .left
        }
    }
    
    func scale(rep: Rep) -> Double {
        self == rep ? 4 : 1
    }

    func alignment(rep: Rep) -> Alignment {
        self == rep ? .top : .bottom
    }
}
```

제가 덤벨 두 개를 만들고 싶기 때문에, 효과는 제가 덤벨을 어느 쪽에서 들어올리느냐에 따라 다를 것입니다. scaleEffect와 alignment는 rep와 덤벨의 쪽이 일치할 때 더 크고 높게 될 것입니다.

여기가 최종 SwiftUI 뷰입니다:

<div class="content-ad"></div>

```swift
구조체 DumbbellView: View {

    @State private var rep: Rep?

    var body: some View {
        HStack {
            createDumbbell(side: .left)
            createDumbbell(side: .right)
        }
        .padding(40)
        .background(Color.indigo)
        .onAppear {
            if rep == nil {
                rep = .left
                rep?.lift()
            }
        }
    }

    private func createDumbbell(side: Rep) -> some View {
        Image(systemName: "dumbbell.fill")
            .imageScale(.large)
            .foregroundColor(.white)
            .scaleEffect(side.scale(rep: rep ?? .left))
            .frame(maxWidth: .infinity,
                   maxHeight: .infinity,
                   alignment: side.alignment(rep: rep ?? .left))
            .animation(loopingAnimation, value: rep)
    }

    private var loopingAnimation: Animation {
        .easeInOut(duration: 0.75)
        .repeatForever(autoreverses: true)
    }
}
```

To make two dumbbells, we've refactored our SwiftUI code to create a dumbbell view via a helper method, initializing an icon for each side. Since we use repeatForever in our Animation, we only need to change the state once, using onAppear, to cause the animated state change to continue forever.

Now we've produced a truly gainful animation.

![GIF](https://miro.medium.com/v2/resize:fit:1400/1*D5gKgtXs0EgAWVk5h-0CXg.gif)


<div class="content-ad"></div>

## SwiftUI 2.0: matchedGeometryEffect

안녕하세요! SwiftUI 1.0은 혁명적이면서도 많은 결함이 있었죠. iOS 월드에 UI를 상태의 함수로 소개했으며 아름다운 선언적 구문을 도입했지만, 네비게이션이 완전히 망가져 있었고 많은 중요한 기능이 빠져 있었습니다.

2020년 iOS 14의 출시로 이러한 문제 중 많은 부분이 개선되었고 몇 가지 훌륭한 새로운 기능이 소개되었는데, 제가 가장 좋아하는 것은 matchedGeometryEffect입니다. 이는 Keynote의 Magic Move와 유사한 애니메이션 효과를 만들어냅니다.

matchedGeometryEffect를 사용하려면 요소 간에 고유한 식별자를 설정해야 합니다. 이를 통해 SwiftUI가 다른 하위뷰 간에도 뷰의 식별성을 유지할 수 있게 됩니다. 렌더링 엔진은 지오메트리(즉, 위치와 크기)를 캡처하고 두 뷰 사이의 변경사항을 보간하는 부드러운 움직임을 만들어내며 전환 시 발생하는 변화를 보여줍니다.

<div class="content-ad"></div>

이렇게 컨트사이드 뷰에서 스포츠 팀이 하프타임에 측면을 바꾸는 예제 코드가 있어. 주요 부분을 보여줄 테니, 전체 코드는 내 GitHub에서 확인해봐.

```js
import SwiftUI

struct CourtsideView: View {
    
    @Namespace private var animation
    @State private var isFirstHalf: Bool = true
    
    var body: some View {
        VStack {
            HStack {
                if isFirstHalf {
                    team(name: "Red Team", color: .red, flipped: false)
                    Spacer()
                    team(name: "Green Team", color: .green, flipped: true)
                } else {
                    team(name: "Green Team", color: .green, flipped: false)
                    Spacer()
                    team(name: "Red Team", color: .red, flipped: true)
                }
            }
        }
    }
    
    private func team(name: String, color: Color, flipped: Bool) -> some View {
        Text(name)
            .foregroundColor(color)
            .font(.title)
            .fontWeight(.bold)
            .rotationEffect(.radians(flipped ? (.pi / 2) : (-.pi / 2)))
            .matchedGeometryEffect(id: name, in: animation)
    }
}
```

여기 몇 가지 중요한 개념이 있어:

- @Namespace private var animation은 SwiftUI에게 뷰를 식별할 수 있는 전역 네임스페이스를 설정해.
- 우리는 두 가지 배치를 만들어: 첫 번째 하프에는 빨간 팀이 HStack의 앞쪽에 있고, 초록 팀은 뒷쪽에 있어; 이건 isFirstHalf이 false로 토글될 때 바뀌어.
- team 도우미 함수는 팀 이름, 색상, 회전 배열을 보여주는 작은 뷰를 만들어줘.
- 마지막으로, 각 팀에서 matchedGeometryEffect(id: name, in: animation)를 사용해서 전역 네임스페이스에 뷰의 식별 정보를 알려주고, 하프가 바뀔 때 부드럽게 애니메이션될 수 있게 해.

<div class="content-ad"></div>

우리의 하프타임 버튼은 명시적인 스프링 애니메이션과 함께 @State 프로퍼티를 토글합니다. 이는 isFirstHalf 프로퍼티에 의존하는 뷰의 모든 것이 변경될 때 애니메이션 효과를 받는다는 것을 의미합니다.

```swift
private var halfTimeButton: some View {
    Button(action: {
        withAnimation(.spring()) {
            isFirstHalf.toggle()
        }
        
    }, label: {
        Label("하프타임", systemImage: "flag.filled.and.flag.crossed")
    })
}
```

여기서 우리는 팀의 면을 바꾸기 위해 하프타임 버튼을 활용하는 것을 볼 수 있습니다:

![하프타임 버튼](https://miro.medium.com/v2/resize:fit:1400/1*eDe940TT7brvzVUdATNw0A.gif)

<div class="content-ad"></div>

저는 여기서 matchedGeometryEffect의 힘을 조금만 경험했을 뿐이에요. matchedGeometryEffect의 진정한 잠재력은, 작은 이미지가 세부 화면을 볼 때 완벽하게 확장되어 전체 크기의 헤더로 변환되는 히어로 이미지 애니메이션을 만들 때 나타나요.

## SwiftUI 2023: 키프레임 애니메이션

최신 SwiftUI 릴리스는 애니메이션에 대한 가장 큰 변화에요. 주로 개발자와 UI 디자이너들에게 세밀한 제어를 제공하는 오랫동안 기다려온 키프레임 애니메이션을 소개했어요. 아직 Lottie를 완전히 대체할지에 대한 것은 보여지지 않았어요.

"키프레임"의 개념은 올드스쿨 디즈니 애니메이션 시대로 거슬러 올라가요. 주요 예술가들이 움직임의 주요 지점, 즉 "키프레임"을 그리고, 그 사이에 있는 많은 프레임을 초안으로 그리는 보조 애니메이터들이 채워 넣었어요.

<div class="content-ad"></div>

프로그램에서의 키프레임은 크게 다르지 않습니다. 타임라인 상에 특정 지점을 정의하며 위치, 크기, 불투명도와 같은 속성들의 값을 설정합니다. 소프트웨어는 신뢰할 만한 주니어 애니메이터 역할을 하면서, 이러한 키프레임의 값들 사이를 보간하고 그 사이에 렌더링할 모든 프레임을 계산합니다.

최종 예제로 데드리프트를 다루어 봅시다.

```swift
import SwiftUI

struct DeadliftView: View {
    
    struct Transformation {
        var yScale = 1.0
    }
    
    var body: some View {
        Image(systemName: "figure.strengthtraining.traditional")
            .keyframeAnimator(initialValue: Transformation(),
                              repeating: true,
                              content: { content, transformation in
                content
                    .scaleEffect(y: transformation.yScale, anchor: .bottom)
            },
                              keyframes: { _ in
                KeyframeTrack(\.yScale) {
                    LinearKeyframe(0.6, duration: 0.25)
                    LinearKeyframe(1.0, duration: 0.25)
                    LinearKeyframe(1.4, duration: 0.25)
                    LinearKeyframe(1.0, duration: 0.25)
                }
            })
    }
}
```

여기에 새로운 API가 많이 등장했으니, 의도적으로 애니메이션 자체를 최대한 지루하게 만들었습니다. 따라서 데드리프트 반복을 수행하는 동안 KeyframeTrack을 쉽게 따라 갈 수 있습니다:

<div class="content-ad"></div>

- 처음 0.25초 동안, Transformation 구조체의 yScale 값을 초기 값 1.0에서 0.6으로 변환하면서 서로 앉습니다. scaleEffect(y: transformation.yScale) 뷰 수정자가 콘텐츠에 적용되어 초기 하강 효과가 발생합니다.
- 그런 다음 아령을 든 채 일어서며, 0.6에서 1.0으로, 그리고 1.4로 0.5초 동안 변화합니다. 여기서는 두 개의 별도의 선형 키프레임을 사용했으며, 지속 시간이 모두 일치하도록 했지만 LinearKeyframe(1.4, duration: 0.5)를 사용하여 한 번에 처리할 수도 있습니다.
- 마지막으로 마지막 0.25초 동안에 우리의 원래 스케일인 1.0으로 돌아갑니다. 이로써 부드럽게 반복할 수 있게합니다.

![image](https://miro.medium.com/v2/resize:fit:1400/1*gc1K6s7N6QjzT22-UcZ1LQ.gif)

키프레임 애니메이터 수정자가 이곳에서 중요한 부분을 차지하며, 다음 인수로 전체 구조를 설정합니다:

- initialValue는 원하는 모든 속성을 담고 있는 제네릭 값(Value)을 가져와요. 이 경우 초기 값을 갖고있는 Transformation 구조체를 초기화했습니다.
- repeating: true로 키프레임 애니메이션을 계속 반복하도록 설정합니다.
- content는 ViewBuilder 클로저로, 보기(content)에 변형을 적용하고 키프레임 간 보간된 값(변환)을 사용합니다.
- keyframes는 변환 값이 시간이 지남에 따라 어떻게 변경될지 정의합니다. 여기서 우리는 원하는 속성을 통해 개별적으로 타임된 변환을 허용하는 KeyframeTracks를 설정합니다.

<div class="content-ad"></div>

We could enhance the animation by adding the `yTranslation` property to our Transformation struct:

```js
struct Transformation {
    var yScale = 1.0
    var yTranslation = 0.0
}
```

Then, we introduce an extra offset modifier to the content within our keyframeAnimator:

```js
content
    .scaleEffect(y: transformation.yScale, anchor: .bottom)
    .offset(y: transformation.yTranslation)
```

<div class="content-ad"></div>

그리고 마지막으로, 새로운 번역 보간 방법을 추가한 KeyframeTrack을 하나 더 추가해봅시다:

```js
KeyframeTrack(\.yTranslation) {
    LinearKeyframe(20, duration: 0.25)
    SpringKeyframe(-40, duration: 0.5, spring: .snappy)
    CubicKeyframe(0, duration: 0.25)
}
```

이제 저희 강건한 사나이는 다리를 운동하며 더 강해지고 있어요:

![Strongman Leg Workout](https://miro.medium.com/v2/resize:fit:1400/1*8Rjf7k6TE6gMrTLlJS345w.gif)

<div class="content-ad"></div>

WWDC 2023에서 소개된 고급 애니메이션 API 양이 놀라울 정도로 많아서 아직 표면만 긁은 것 같아요.

- CustomAnimation 프로토콜을 사용하여 키프레임 애니메이션 보간을 위한 고유한 타이밍 곡선 만들기.
- phaseAnimator를 사용하여 뷰가 이산적인 단계를 통해 애니메이션되도록 만들기.
- mapCameraKeyframeAnimation을 사용하여 MapKit 경로를 따라 고급 애니메이션 만들기.
- 가장 흥미로운 것은 SwiftUI에서 Metal 셰이더를 직접 사용할 수 있게 된 것인데, colorEffect, distortionEffect, visualEffect, layerEffect에 대한 새로운 수정자가 도입되었어요.

# 애니메이션의 미래

시간을 따라 온 우리 여정의 끝에 도달했어요. NeXTSTEP 시대의 Display Postscript를 시작으로, Quartz와 OpenGL을 핵심 그래픽 기둥으로 도입한 원래의 Mac OS X를 거쳐 왔어요. Core Animation은 초기 iPhone 시대에 상태 변경을 정의하는 선언적 접근 방식으로 애니메이션을 혁명화했고, Metal은 네이티브 그래픽 엔진의 성능 병목 현상을 극복하기 위해 만들어진 거예요.

<div class="content-ad"></div>

드디어 SwiftUI가 진화하여 놀라운 애니메이션을 이전보다 쉽게 만들 수 있게 되었어요. 모양 수준 조작에서 시작한 우리의 여정과는 아주 다른 모습이죠.

미래에 무엇이 일어날지 기대가 돼요 — SwiftUI는 애플이 주력으로 삼은 방향이며, 오늘날에도 아직 초창기입니다. Vision Pro는 큰 변수입니다 — 이틀 후 다양한 진화를 겪을 것으로 예상됩니다 (아이폰 2G를 기억하시나요? 저는 기억이 안 나네요!). 개발자들이 Spaces에서 새로운 몰입형 경험을 어떻게 만들어 나갈지 배우면서 우리의 패러다임은 계속 발전할 거예요.

오늘날 개발자들의 효율성은 AR 및 VR에서 3D 경험을 위한 모양 수준 조작의 OpenGL 시절에 더 가까운 것으로 느껴져요. 제 예상으로는, 아마도 WWDC 2026년까지 ARKit의 SwiftUI 버전이 등장하여 3D 경험을 명시적으로 정의하는 방식으로 소개될 것 같아요 — 시스템에 우리가 보고 싶은 것을 말하지만, 공간에서 오브젝트의 정확한 위치를 알려주진 않을 거예요. 또한 VisionOS에서 무료로 사용할 수 있는 오브젝트 카탈로그를 소개하는 SFSymbols의 3D 버전이 가까워질 것으로 예상돼요.

미래에서 만나요!

<div class="content-ad"></div>

자콥의 테크 타버른을 읽어 주셔서 감사합니다!