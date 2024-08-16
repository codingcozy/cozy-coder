---
title: "iOS에서 Swift 59로 게이지 뷰 그리는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-DrawingaGaugeViewwithSwift59oniOS_0.png"
date: 2024-08-03 19:56
ogImage: 
  url: /assets/img/2024-08-03-DrawingaGaugeViewwithSwift59oniOS_0.png
tag: Tech
originalTitle: "Drawing a Gauge View with Swift 59 on iOS"
link: "https://medium.com/@egzonpllana/drawing-a-gauge-view-with-swift-5-9-on-ios-6480e382e366"
---


# 소개

이 글에서는 수학 함수를 소프트웨어 공학에 구현하는 방법을 살펴보고, GaugeViewXK라는 사용자 지정 UIKit 뷰로 변환할 것입니다. 이 뷰는 속도계나 다른 측정 기기에서 찾을 수 있는 게이지를 표시하는 데 사용됩니다. 코드를 분해하고 그 요소들과 논리를 간단한 용어로 설명할 것입니다. 이 안내서는 Swift에서 그리기 API에 대해 더 알고 싶어하는 중/시니어 개발자를 대상으로 합니다.

이 구성 요소를 구축하는 동안, 우리의 목표는 완벽한 구성 요소를 만드는 것이 아니라, 이를 사용해 API 및 Swift-iOS에서 뷰를 그리는 방법을 탐구하는 것입니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*t6RaEGaFpxRyrgEMzCdQ2Q.gif)

<div class="content-ad"></div>

# 목적

GaugeViewXK는 게이지 위에 값의 범위를 시각적으로 나타내기 위해 만들어졌습니다. 그라데이션 색상, 다양한 값에 대한 레이블, 원활하게 목표 값을 향해 움직이는 동적 인디케이터와 같은 기능이 포함되어 있습니다. 목표는 정보를 제공하고 시각적으로 매력적인 게이지 뷰를 만드는 것입니다.

# 구현 방법

- CAShapeLayer 및 CAGradientLayer: 이러한 Core Animation 레이어는 게이지 및 그 구성 요소를 그리고 애니메이션하는 데 사용됩니다.
- CADisplayLink: 게이지 인디케이터를 부드럽게 애니메이션하기 위해 사용됩니다.

<div class="content-ad"></div>

# 도전적인 부분

- 지표 애니메이션:

- 도전점: 지표를 부드럽고 정확하게 대상 값으로 이동시키는 것은 정확한 타이밍과 화면 새로고침 속도와의 동기화를 필요로 합니다. 도전점은 특히 대상 값이 자주 변경될 때 애니메이션이 부자연스럽거나 일관성이 없어지지 않도록 하는 데 있습니다.
- 우리가 한 방법: CADisplayLink를 사용하여 화면의 새로고침 속도와 동기화하는 디스플레이 링크를 만들었습니다. 이를 통해 지표가 위치를 부드럽고 일관되게 업데이트할 수 있도록 했습니다. 현재 값과 대상 값 사이의 차이를 계산하고 지표의 위치를 점진적으로 업데이트하여 부드러운 애니메이션을 구현했습니다.

2. 게이지와 레이블 그리기:

<div class="content-ad"></div>

- 왜 도전적인가: 게이지를 그리는 것은 값의 범위를 나타내는 정확한 호를 만드는 것을 포함합니다. 또한, 이 호 위의 레이블을 정확하게 배치하려면 각도와 위치를 올바르게 계산해야 합니다. 도전 과제는 레이블이 고르게 간격을 두고 게이지에 올바르게 정렬되도록 하는 것입니다.
- 우리가 한 방법: 우리는 삼각법을 사용하여 게이지 호 위의 레이블 위치를 계산했습니다. 각도를 라디안으로 변환하고 사인 및 코사인 함수를 사용하여 각 레이블의 (x, y) 좌표를 결정했습니다. 이를 통해 레이블이 고르게 분포되고 게이지의 해당 값과 정렬되도록 보장했습니다. 또한 CAShapeLayer 및 CAGradientLayer를 활용하여 게이지를 그리고 적절한 색상 또는 그라데이션을 적용했습니다.

# Display Link란 무엇인가요?

CADisplayLink는 앱이 그림을 디스플레이의 새로 고침 속도에 동기화할 수 있게 해주는 타이머 객체입니다. 디스플레이의 새로 고침 속도에 따라 메소드를 호출하여 부드러운 애니메이션을 보장합니다.

# 게이지 그리기

<div class="content-ad"></div>

게이지는 CAShapeLayer와 CAGradientLayer를 사용하여 값을 나타내는 다채로운 호를 그립니다.

- 호 경로: UIBezierPath를 사용하여 중심, 반지름, 시작 각도 및 종료 각도를 지정하여 호 경로를 생성합니다.
- 모양 레이어: CAShapeLayer는 호 경로를 그리는 데 사용됩니다. 게이지 색상 구성에 따라 단일 색상을 적용하거나 CAGradientLayer를 사용하여 그라데이션을 생성합니다.

게이지의 모양은 다양한 색상 또는 그라데이션으로 사용자 정의할 수 있으며 두께는 gaugeWidth 속성으로 제어됩니다.

# 레이블 그리기

<div class="content-ad"></div>

게이지 주변에는 특정 값들을 나타내는 레이블이 그려집니다.

- 레이블 생성: 각 값에 대해 UILabel이 생성되고 계산된 좌표에 배치됩니다. 레이블은 labelFont 및 labelColor 속성에 따라 중앙 정렬되고 스타일이 적용됩니다.
- 레이블 위치 지정: 레이블의 위치를 게이지의 호를 따라 계산합니다. 이를 위해 각 레이블의 각도를 결정하고 삼각함수를 사용하여 호 위의 (x, y) 좌표를 찾습니다.

이로써 레이블이 균등하게 분포되고 게이지상 해당 값과 정렬되도록 보장됩니다.

# 지시자 그리기

<div class="content-ad"></div>

지시자 바늘은 게이지의 현재 값으로 향합니다.

- 지시자 레이어: CAShapeLayer를 사용하여 지시자 바늘을 그립니다. 바늘의 위치는 현재 값에 기반하여 게이지의 각도로 매핑되도록 계산됩니다.
- 지시자 애니메이션: CADisplayLink를 사용하여 바늘을 현재 위치에서 대상 값까지 부드럽게 애니메이션합니다. 이 과정은 바늘의 위치를 점진적으로 업데이트하고 각 단계마다 다시 그리는 것을 포함합니다.

# 그라디언트 레이어 만들기

- gradientLayer.type = .conic
어떤 종류의 페인트를 사용할지 결정합니다. 여기서는 무지개 원 형태인 코닉 그라디언트를 선택합니다.
- gradientLayer.startPoint = CGPoint(x: 0.5, y: 0.5)
그림을 시작할 캔버스의 중심을 선택합니다. 이는 사각형의 중앙점입니다.
- gradientLayer.endPoint = CGPoint(x: 0.23, y: 2)
페인트가 끝날 곳을 설정합니다. 이 지점은 중앙 밖에 약간 있어서 그라디언트에 방향성을 부여합니다.
- gradientLayer.locations = calculateGradientLocations(for: colors)
각 색상이 그라디언트에 어디에 배치될지 계산합니다. 특정 지점에 스티커를 붙이는 것과 같습니다.
- gradientLayer.frame = rect
캔버스에 얼마나 큰 공간을 할당해야 하는지 지정합니다. 우리가 얻은 사각형의 크기입니다.
- gradientLayer.colors = processGradientColors(colors: colors).map { $0.cgColor }
그리기에 색상을 준비합니다. 각 색상을 캔버스가 이해하는 형식으로 변환하고 순서대로 넣습니다.
- gradientLayer.mask = shapeLayer
캔버스에 스텐실을 놓습니다. 모양 레이어는 마스크처럼 작용하여 우리가 특정 영역에만 그릴 수 있도록 합니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-08-03-DrawingaGaugeViewwithSwift59oniOS_0.png)

## UIBezierPath이 무엇인가요?

UIBezierPath의 init(arcCenter:radius:startAngle:endAngle:clockwise:) 메서드는 원의 아크를 나타내는 새로운 베지에 경로 객체를 생성합니다. 이 메서드는 사용자 정의 뷰에서 아크를 그리는 데 유용합니다.

## 매개변수


<div class="content-ad"></div>

- arcCenter: 아크의 중심을 정의하는 CGPoint입니다. 이는 아크가 그려질 중심점을 나타냅니다.
- radius: 아크의 반지름을 지정하는 CGFloat입니다. 중심점부터 아크의 어떤 점까지의 거리를 결정합니다.
- startAngle: 아크의 시작 각도를 나타내는 CGFloat로, 라디안으로 표시됩니다. 각도는 단위 원에서 양의 x축을 기준으로 측정됩니다.
- endAngle: 아크의 끝 각도를 나타내는 CGFloat으로, startAngle과 마찬가지로 양의 x축을 기준으로 측정됩니다.
- clockwise: 아크가 그려지는 방향을 나타내는 Bool입니다. True이면 시계 방향으로 아크가 그려집니다.

# CGPoint 계산 설명

```js
let endPoint = CGPoint(
    x: center.x + radius * cos(endAngle - .pi / 2),
    y: center.y + radius * sin(endAngle - .pi / 2)
)
```

- 중심 점
center: 이것은 지시자 선의 시작점으로, 일반적으로 게이지의 중앙에 있습니다 (indicatorWidth로 조정됨).
- 반지름
radius: 이것은 지시자 선의 길이로, 게이지 폭의 절반에서 게이지 폭 및 일부 간격을 뺀 것으로 계산됩니다.
- 끝 각도
endAngle: 이 값은 현재 속도를 기반으로 지시자가 향하는 각도(라디안)입니다. 이 각도는 메서드 내에서 이전에 계산됩니다.
- 삼각함수
cos (코사인)와 sin (사인)은 각도를 x 및 y 좌표로 변환하는 데 도움이 되는 삼각함수입니다.
삼각법에서 각도는 일반적으로 양의 x축(0도)에서 시계 반대 방향으로 시작합니다. 그러나 화면에 그리기 위해 각도는 일반적으로 양의 y축(90도)에서 시작하여 시계 방향으로 이동합니다. 이 차이를 보정하기 위해 endAngle에서 π/2(90도)를 뺍니다.
- x 및 y 좌표 계산

<div class="content-ad"></div>

- endPoint의 x 좌표는 다음과 같이 계산됩니다:

```js
center.x + radius * cos(endAngle - .pi / 2)
center.y + radius * sin(endAngle - .pi / 2)
```

- 이는 중심의 x 좌표에서 시작하여, 각도의 코사인에 반지름을 곱한 값에 따라 수평으로 이동한다는 것을 의미합니다.

# 시각화

<div class="content-ad"></div>

- 원을 가정해보세요. 중심이 center에 있는 원이에요.
- 반지름은 중심부터 원의 가장자리까지의 길이에요.
- endAngle은 지시기가 가리키는 방향을 결정해요.
- cos와 sin을 사용하여 이 각을 실제 x와 y 거리로 변환하여 endPoint를 얻어요.

# 실제 예시

만약 (50, 50)에 중심을 두고 반지름이 40인 게이지가 있고, endAngle이 바로 위쪽을 가리키는 것에 해당한다면 (90도 또는 π/2 라디안), 이 계산은 다음과 같을 거예요:

```js
x: 50 + 40 * cos(π/2 - π/2) = 50 + 40 * cos(0) = 50 + 40 * 1 = 90
y: 50 + 40 * sin(π/2 - π/2) = 50 + 40 * sin(0) = 50 + 40 * 0 = 50
```

<div class="content-ad"></div>

그래서, endPoint는 중심에서 오른쪽으로 직접 이동하여 (90, 50)이 되며, 이는 90도 각을 나타냅니다.

이 계산은 속도 값에 따라 지시자가 올바르게 표시되도록 하여 게이지에서 정확한 시각적 표현을 제공합니다.

![image](/assets/img/2024-08-03-DrawingaGaugeViewwithSwift59oniOS_1.png)

# 게이지 뷰를 만드는 것이 재미있고 도전적인 이유

<div class="content-ad"></div>

게이지뷰(GaugeView)와 같은 커스텀 뷰를 만드는 것은 여러 가지 이유로 보상적인 경험이 됩니다:

- 핵심 애니메이션(Core Animation) 학습: CAShapeLayer, CAGradientLayer 및 CADisplayLink와 같은 도구를 사용하여 복잡한 애니메이션과 iOS에서 사용자 정의 그림을 생성하는 실용적인 경험을 얻을 수 있습니다.
- 수학적 응용: 삼각 함수를 적용하여 위치와 각도를 계산하면 추상적인 수학과 실용적인 코딩 간의 간극을 좁힐 수 있습니다.
- 문제 해결: 부드러운 애니메이션과 정확한 레이블 위치 설정과 같은 문제를 해결함으로써 문제 해결 능력과 세부 사항에 대한 주의력을 향상시킬 수 있습니다.
- 시각적 피드백: 부드러운 애니메이션과 동적 업데이트를 통해 보면서 사용자 정의 게이지가 살아 움직이는 모습을 볼 수 있어 즉각적이고 만족스러운 시각적 피드백을 제공합니다.

총론적으로, 게이지뷰를 만드는 것은 고급 Swift 및 iOS 개발 기술을 학습하고 연습하는 데 훌륭한 프로젝트로, 재미있는 교육적인 도전이 될 것입니다.

# 자료


<div class="content-ad"></div>

이 문서에서 다루는 주제에 대한 자세한 정보를 원하시면 다음 자료를 살펴보실 수 있습니다:

- Drawing on iOS (https://developer.apple.com/documentation/uikit/uiview/1622529-draw)
- CADisplayLink (https://developer.apple.com/documentation/quartzcore/cadisplaylink)
- CADisplayLink 및 그 응용 (https://medium.com/@dmitryivanov_54099/cadisplaylink-and-its-applications-bfafb760d738)
- CAShapeLayer (https://developer.apple.com/documentation/quartzcore/cashapelayer)
- CAGradientLayer (https://developer.apple.com/documentation/quartzcore/cagradientlayer)
- UIBezierPath (https://developer.apple.com/documentation/uikit/uibezierpath)
- 삼각함수 (https://www.mathsisfun.com/algebra/trigonometry.html)

소프트웨어 엔지니어링에서 수학 함수의 구현에 관한 이 글을 읽어 주셔서 감사합니다. 이로써 여러분의 궁금증을 자극하고 이 중요한 요소의 복잡성에 대해 관심을 갖게 되었으면 좋겠습니다. 다음 글에서 더 도전적이고 재미있는 주제로 만나 보도록 하겠습니다!

# GitHub

<div class="content-ad"></div>

GitHub 저장소에 전체 구현을 찾아보세요:  
[https://github.com/egzonpllana/SwiftAndMathematicalChallenges/tree/main/GaugeViewXK](https://github.com/egzonpllana/SwiftAndMathematicalChallenges/tree/main/GaugeViewXK)

# 함께 연락해요

- LinkedIn: [https://www.linkedin.com/in/egzon-pllana](https://www.linkedin.com/in/egzon-pllana)
- GitHub: [https://github.com/egzonpllana](https://github.com/egzonpllana)