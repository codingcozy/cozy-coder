---
title: "갤러리 앱 만들기 Flutter에서 GridView 이해하기"
description: ""
coverImage: "/assets/img/2024-07-10-LetsbuildaGalleryAppUnderstandingGridViewinFlutter_0.png"
date: 2024-07-10 01:10
ogImage: 
  url: /assets/img/2024-07-10-LetsbuildaGalleryAppUnderstandingGridViewinFlutter_0.png
tag: Tech
originalTitle: "Let’s build a Gallery App: Understanding GridView in Flutter."
link: "https://medium.com/flutter-simplified/lets-build-a-gallery-app-understanding-gridview-in-flutter-48c9681ee439"
---


이미지:
![이미지](/assets/img/2024-07-10-LetsbuildaGalleryAppUnderstandingGridViewinFlutter_0.png)

GridView는 아이템을 행과 열로 배열하는 2차원 레이아웃입니다.

디자인적으로, 그리드 레이아웃은 레이아웃의 모든 요소가 동등한 우선 순위를 갖고 있으며 각 요소가 사용 가능한 공간을 충분히 활용할 만큼 많은 컨텐츠를 갖지 않을 때 널리 사용됩니다. 이 패턴은 이미지 갤러리와 제품 카탈로그 등에서 관찰할 수 있습니다...

## Flutter에서의 GridView

<div class="content-ad"></div>

어떤 것을 배우는 가장 좋은 방법은 실제로 만들어 보는 것입니다. 그래서, Grid View를 사용한 간단한 이미지 갤러리를 만들어 봅시다.

```js
GridView.count(
      crossAxisCount: 4,
      crossAxisSpacing: 24.0,
      mainAxisSpacing: 24.0,
      padding: const EdgeInsets.all(40.0),
      scrollDirection: Axis.horizontal,
      children: const [
        ImageWidget(index: 0),
        ImageWidget(index: 1),
        ImageWidget(index: 2),
        ImageWidget(index: 3),
        ImageWidget(index: 4),
        ImageWidget(index: 5),
        ImageWidget(index: 6),
     ],
);
```

이 간단한 코드 스니펫은 아름다운 이미지 갤러리를 만들어 줍니다. 전체 코드에 대한 소스는 GitHub 저장소에서 확인할 수 있어요.

Gridview.count()에는 많은 프로퍼티가 있습니다. 이 중에서 다섯 가지 가장 중요한 것들을 알아보도록 하죠.

<div class="content-ad"></div>

- `crossAxisCount`: This property takes a double as an input and determines how many elements to display before moving to a new line.
- `crossAxisSpacing` and `mainAxisSpacing`: These properties also accept a double as input. `crossAxisSpacing` defines the space between elements in the same line, while `mainAxisSpacing` defines the space between lines.
- `scrollDirection`: This property determines the scroll directions by controlling the main and cross axes. It accepts `Axis` as an input; `Axis.horizontal` sets the mainAxis to a horizontal direction, and `Axis.vertical` sets the mainAxis to a vertical direction.
- `children`: This property accepts a list of widgets that will be displayed in the `GridView` cells.

This might seem simple and everything seems to be working smoothly.

But wait... what if we need to render 1000 elements on the screen?

Well, just using the normal count method won't cut it! It might lead to rendering issues and even crash your app.

<div class="content-ad"></div>

## GridView.builder

플러터는 모든 레이아웃과 마찬가지로 GridView에 대한 빌더를 가지고 있습니다. 이 방법은 필요에 따라 셀을 렌더링하여 이 문제를 해결합니다. 해당 요소는 뷰에 진입할 때까지로딩됩니다. 갤러리 앱을 위한 builder 방법을 사용한 샘플 코드는 다음과 같습니다:

```js
GridView.builder(
      itemCount: 1000,
      padding: const EdgeInsets.all(40.0),
      gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 4,
        childAspectRatio: 2 / 3,
        crossAxisSpacing: 24.0,
        mainAxisSpacing: 24.0,
      ),
      scrollDirection: Axis.vertical,
      itemBuilder: (_, index) => ImageWidget(
        index: index,
      ),
)
```

builder 방법은 count 방법과 매우 유사하지만 몇 가지 차이점이 있습니다.

<div class="content-ad"></div>

### itemCount
Count 메소드와는 달리, 아이템 개수를 수동으로 제공하여 itemBuilder 메소드가 반복됩니다.

### itemBuilder
위젯을 반환하는 메소드를 받습니다. 이 메소드에서는 아이템의 컨텍스트와 인덱스를 얻습니다.

### gridDelegate
어떻게 자식 요소들이 배열되고 크기가 어떻게 정해지는지를 지배하는 가장 중요한 매개변수입니다. Flutter는 몇 가지 내장된 델리게이트를 제공하며 우리가 직접 만들 수도 있습니다. SliverGridDelegateWithFixedCrossAxisCount: 이 델리게이트는 고정된 열(또는 행) 수의 그리드를 만드는 데 완벽하며 각 항목에 대해 동일한 또는 사용자 정의 너비/높이를 지정할 수 있습니다. SliverGridDelegateWithMaxCrossAxisExtent: 이 델리게이트는 최대 너비/높이를 가진 레이아웃에 이상적입니다. 항목이 다음 줄로 넘어갈 수 있는 더 동적인 그리드를 제공합니다.

## 결론

그리드 뷰는 Flutter의 기본 개념이지만 사용 사례는 무한합니다. 이 이야기를 쓰기 시작할 때 전체 그리드 뷰에 대한 단일 리소스 소스로 만들 계획이었습니다. 그러나 간단하지 않다는 것을 깨달았고 개념을 더 잘 이해하기 위해 더 작은 부분으로 나누기로 결정했습니다.