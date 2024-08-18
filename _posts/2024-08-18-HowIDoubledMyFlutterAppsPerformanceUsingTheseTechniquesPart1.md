---
title: "플러터 앱 성능 2배 향상시키는 방법 - 파트 1"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-18 10:40
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "How I Doubled My Flutter Apps Performance Using These Techniques  Part 1"
link: "https://medium.com/@raghavthegreat1/how-i-doubled-my-flutter-apps-performance-using-these-techniques-part-1-48ac41d553e9"
isUpdated: false
---


2020년 완전히 잠겨 있다가 플러터 여행을 시작했을 때 놀라웠어요. 아름다운 크로스 플랫폼 앱을 손쉽게 만들 수 있다는 것에 매료되었죠. 그러나 앱이 점점 복잡해지면서 성능이 점차 떨어지는 것을 알게 되었어요. 가장 불편한 순간에 앱이 느리거나 충돌하는 것을 본 적이 있다면, 얼마나 짜증나는지 알 거예요.

4년간의 플러터 여정 동안 개인 및 클라이언트 프로젝트를 많이 만들었는데, 플러터는 기본적으로 성능이 우수한 프레임워크임을 깨달았어요. 주요 성능 문제는 우리 쪽에서 발생하며, 이러한 문제는 프로젝트에 쉽게 통합할 수 있습니다. 사실, 이러한 성능 문제는 플러터의 작고 중요한 개념을 간과할 경우 발생합니다.

# 1. 위젯 빌드 최적화: 덜은게 더 좋다!

플러터는 모든 것이 위젯이며, 구축하는 모든 위젯에는 비용이 들어갑니다. 앱이 느릴 때는 주로 위젯이 불필요하게 다시 구축되기 때문입니다. 따라서 빠르고 간단한 수정으로 앱의 성능을 더욱 향상시킬 수 있습니다.

<div class="content-ad"></div>

## 해결책: const 위젯과 키 사용하기

위젯을 const로 표시함으로써, 이 위젯이 변경되지 않을 것이라는 것을 Flutter에게 알려줍니다. 이 간단한 변경으로 불필요한 다시 빌드를 방지할 수 있습니다.

```js
const Text('Hello, Flutter!');
```

키를 사용하는 것 또한 한 가지 요령입니다. 키를 사용하면 Flutter가 다시 빌드하는 과정에서 위젯을 추적할 수 있으므로 혼란을 피하고 모든 위젯을 다시 빌드하지 않습니다.

<div class="content-ad"></div>

```javascript
ListView.builder(
  key: const Key('my-list-view'),
  itemBuilder: (context, index) {
    return Text('Item $index');
  },
);
```

## 퀵 팁:

만약 VSCode를 사용 중이라면, 상수를 추가하는 일련의 수고스러운 과정을 간단한 코드로 대신할 수 있어요.

파일을 저장할 때마다, VSCode는 해당하는 곳에 const 키워드를 자동으로 추가해 줍니다. 이를 위해 다음 단계를 따라해 보세요:

<div class="content-ad"></div>

- VSCode에서 (Ctrl + P)를 눌러 Command Palette를 열고 settings.json 파일을 검색합니다.
- 그곳에 다음 코드를 추가하세요

```js
"editor.codeActionsOnSave": {      
  "source.fixAll": true  
}
```

# 2. RepaintBoundary: 손상 포함

성능을 향상시키는 것은 Flutter 위젯의 다시 그리기 방법을 고려하지 못할 수도 있는 잠복적인 주범 중 하나입니다. 위젯이 변경될 때마다 Flutter는 해당 위젯을 다시 그리는데, 위젯 트리가 깊거나 영향 받는 영역이 큰 경우 비용이 많이 들 수 있습니다. 이때 RepaintBoundary가 구원의 역할을 합니다.

<div class="content-ad"></div>

RepaintBoundary는 플러터에게 이 위젯과 그 안에 있는 모든 것을 별도의 레이어로 취급해야 한다고 말하는 위젯이에요. 이렇게 하면 경계 안의 내용이 변경될 때 화면의 해당 부분만 다시 그려지고 전체 위젯 트리가 다시 그려지지 않아요. 이는 특히 애니메이션, 이미지 또는 자주 업데이트되는 모든 위젯에 유용해요.

```js
RepaintBoundary(
  child: Container(
    width: 100,
    height: 100,
    color: Colors.blue,
    child: Center(
      child: Text('I only repaint this!'),
    ),
  ),
)
```

이 예에서 RepaintBoundary 내의 Container만이 변경될 때 다시 그려지며 전체 상위 위젯이 다시 그려지지 않아요.

또한, 불필요한 위젯을 너무 많이 겹쳐 사용하는 것을 피하세요. 예를 들어 여러 개의 Padding, Align 및 DecoratedBox 위젯을 중첩하는 대신 단일 Container를 사용하면 처리 시간을 많이 절약할 수 있어요.

<div class="content-ad"></div>

## 전문 팁: Repaint Rainbow으로 디버깅하기

화면을 다시 그리는 지점을 시각적으로 디버깅하려면 플러터의 성능 오버레이에서 "Repaint Rainbow"을 활성화할 수 있습니다. 이 도구는 화면에서 다시 그려지는 영역을 색으로 표시합니다. 화면의 큰 부분이 잘못 그려질 때 활성화되면, RepaintBoundary가 필요할 수 있음을 나타냅니다.

```js
import 'package:flutter/rendering.dart';
void main() {
  debugRepaintRainbowEnabled = true;
  runApp(MyApp());
}
```

## ⚠️ 주의: RepaintBoundary를 과도하게 사용하지 마세요

<div class="content-ad"></div>

RepaintBoundary은 강력하지만 과용하면 역효과가 발생할 수 있어요. 모든 것을 RepaintBoundary로 감싸면 복잡도가 증가하고 메모리 사용량이 더 높아질 수 있어요. 선택적으로 사용하여 실제로 격리된 다시 그리기에서 혜택을 받는 앱 부분에만 사용하세요.

# 3. 효율적인 ListView: 더 이상의 외압 스크롤

소셜 미디어 피드나 온라인 상점과 같이 스크롤이 많은 앱의 경우, ListView 최적화가 중요해요.

## 해결책: ListView.builder 및 IndexedStack

<div class="content-ad"></div>

ListView.builder는 대량의 목록을 처리할 때 가장 좋은 친구입니다. 화면에 현재 보이는 요소만 빌드하므로 메모리와 CPU 사용량을 절약할 수 있어요.

```js
ListView.builder(
  itemCount: 1000,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text('아이템 $index'),
    );
  },
);
```

하나씩만 표시해야 하는 복잡한 UI 요소가 있는 화면의 경우 IndexedStack이 좋은 해결책입니다. 모든 위젯을 메모리에 유지하지만 한 번에 하나씩만 표시하여 리빌드를 줄일 수 있습니다.

```js
IndexedStack(
  index: 1, // 활성 자식 인덱스
  children: [
    Container(color: Colors.red, height: 100, width: 100),
    Container(color: Colors.green, height: 100, width: 100),
    Container(color: Colors.blue, height: 100, width: 100),
  ],
)
```

<div class="content-ad"></div>

# 4. 빠른 로딩을 위해 이미지 캐싱하기

성능에 영향을 미치는 가장 큰 요인은 이미지일 수 있습니다. 앱이 동일한 이미지를 반복해서 로드하는 경우 캐싱을 고려해보는 것이 좋습니다.

## 해결책: CachedNetworkImage 사용

cached_network_image 패키지는 반드시 필수입니다. 처음로드한 후 이미지를 로컬로 캐싱하여 앱이 동일한 이미지를 반복해서 가져오는 시간을 낭비하지 않게 합니다.

<div class="content-ad"></div>

```js
CachedNetworkImage(
  imageUrl: "https://example.com/image.jpg",
  placeholder: (context, url) => CircularProgressIndicator(),
  errorWidget: (context, url, error) => Icon(Icons.error),
);
```

# 5. async 작업의 숨겨진 비용

Dart에서 비동기 함수를 작성할 때, 필요하지 않은 경우 async를 사용하지 않도록 하세요. 미래를 다루는 함수에 대해 async를 사용하는 것은 유혹적일 수 있지만, 함수의 동작에 영향을 미치지 않는 경우 async를 제외하는 것이 가장 좋습니다. 불필요하게 async 키워드를 사용하면 계산 부하가 큰 함수의 성능이 급격하게 저하됩니다.

여기에 제가 언급한 사실을 입증하는 Rei의 멋진 Medium 기사가 있습니다.


<div class="content-ad"></div>

알맞은 형식:

```js
Future<int> fastestBranch(Future<int> left, Future<int> right) {
  return Future.any([left, right]);
}
```

불필요한 async 사용:

```js
Future<int> fastestBranch(Future<int> left, Future<int> right) async {
  return Future.any([left, right]);
}
```  

<div class="content-ad"></div>

## 언제 async를 사용해야 할까요?

- 함수 내에서 await을 사용하고 있을 때.
- 에러를 비동기적으로 throw할 때.
- 값을 자동으로 Future로 래핑하고 싶을 때.

예시:

```js
Future<void> usesAwait(Future<String> later) async {
  print(await later);
}

Future<void> asyncError() async {
  throw '에러 발생!';
}

Future<String> asyncValue() async => '값';
```

<div class="content-ad"></div>

간단히 말해서, 함수에 가치를 더할 때만 async를 사용하세요!

# 결론

여기서 본 시리즈의 맨 처음 부분에서, 우리는 플러터 앱의 성능을 두 배로 높일 수 있는 다섯 가지 간단하면서도 강력한 기술을 발견했습니다. 이러한 조정은 내 앱이 더 부드럽게 실행되는 것뿐만 아니라 개발자로서의 자신감도 높였습니다. 기억하세요, 최적화는 속도뿐만이 아니라 사용자에게 원활한 경험을 제공하는 것입니다.

즐거운 코딩 하시고, 여러분의 플러터 앱들이 항상 빠르고 반응성이 좋기를 바랍니다!

<div class="content-ad"></div>

# 유용한 링크

이 글에 50번 박수를 칠 수 있다는 것을 아시나요? 박수 버튼을 누르고 누르고 있어보세요! 😙

감사합니다!