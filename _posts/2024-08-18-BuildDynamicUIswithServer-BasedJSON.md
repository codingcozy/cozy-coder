---
title: "서버 기반 JSON을 활용한 동적 UI 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-18-BuildDynamicUIswithServer-BasedJSON_0.png"
date: 2024-08-18 10:41
ogImage: 
  url: /assets/img/2024-08-18-BuildDynamicUIswithServer-BasedJSON_0.png
tag: Tech
originalTitle: "Build Dynamic UIs with Server-Based JSON"
link: "https://medium.com/@nocode_71647/build-dynamic-uis-with-server-based-json-fa7ed6c4e3bd"
isUpdated: true
updatedAt: 1723951013565
---

<img src="/assets/img/2024-08-18-BuildDynamicUIswithServer-BasedJSON_0.png" />

앱의 전체 모양과 느낌을 업데이트를 요청하지 않고 바꿀 수 있다고 상상해보세요. 이것은 서버 기반 UI로 가능합니다. 여러분의 앱 인터페이스는 서버의 데이터로 제어됩니다. 이 안내서에서는 Flutter를 사용하여 서버에서 가져온 JSON 데이터를 사용해 동적 UI를 생성하는 방법을 보여드립니다. 이를 통해 여러분의 앱을 더 유연하고 강력하게 만들 수 있습니다.

# 단계 1: 서버에서 UI 정의하기

먼저, 여러분은 앱 인터페이스가 어떻게 보이길 원하는지 결정해야 합니다. 앱에 직접 코딩하는 대신 서버의 JSON 데이터로 이를 정의할 것입니다.

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

- JSON은 데이터를 설명하는 방법입니다. 예를 들어, 앱에서 버튼과 텍스트를 원한다면, JSON 형식으로 그들을 설명합니다.

## 예시 JSON 구조:

```js
{
  "type": "Column",
  "children": [
    {
      "type": "Text",
      "value": "Welcome to My App!"
    },
    {
      "type": "Button",
      "label": "Click Me",
      "onPressed": "/doSomething"
    }
  ]
}
```

이 예시에서:

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

- type: UI 요소의 종류를 설명합니다 (예: 텍스트 또는 버튼).
- children: 다른 요소 내부에 포함된 요소를 나열합니다. (예: 버튼 내의 텍스트).
- value: 나타나야 하는 텍스트입니다.
- onPressed: 버튼을 클릭했을 때 어떻게 동작해야 하는지를 나타냅니다 (다른 화면으로 이동하는 등).

# 단계 2: JSON 데이터를 전송하는 API 생성

다음으로, 이 JSON 데이터를 앱에 전송할 방법이 필요합니다. 여기서 API(앱과 서버 간에 대화하는 방법)가 필요합니다.

- Node.js, Firebase 또는 Python과 같은 도구를 사용하여 API를 생성하여 앱이 요청 시 JSON 데이터를 보낼 수 있습니다.

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

# 단계 3: 플러터 앱에서 JSON 데이터 가져오기

지금, 당신의 플러터 앱에서 서버로부터 JSON 데이터를 가져와야 합니다. 앱이 서버와 통신할 수 있게 해주는 http라는 패키지를 사용하여 이 작업을 수행할 것입니다.

다음은 JSON을 가져오기 위한 간단한 코드입니다:

```js
import 'package:flutter/material.dart';
import 'dart:convert';  // JSON 디코딩을 위한
import 'package:http/http.dart' as http;

class DynamicUI extends StatelessWidget {
  final String apiUrl;

  DynamicUI(this.apiUrl);

  Future<Map<String, dynamic>> fetchUI() async {
    final response = await http.get(Uri.parse(apiUrl));
    if (response.statusCode == 200) {
      return json.decode(response.body);
    } else {
      throw Exception('UI 불러오기 실패');
    }
  }

  @override
  Widget build(BuildContext context) {
    return FutureBuilder(
      future: fetchUI(),
      builder: (context, snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return Center(child: CircularProgressIndicator());
        } else if (snapshot.hasError) {
          return Center(child: Text('오류: ${snapshot.error}'));
        } else {
          return buildWidget(snapshot.data as Map<String, dynamic>);
        }
      },
    );
  }
}
```

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

# 단계 4: JSON 데이터를 Flutter 위젯으로 변환하기

JSON 데이터를 가져온 후, 이를 실제 UI 요소로 변환해야 합니다. 이는 JSON 설명을 버튼, 텍스트 및 다른 위젯으로 변환하는 것을 의미합니다.

## 예시 위젯 빌더:

```js
Widget buildWidget(Map<String, dynamic> json) {
  switch (json['type']) {
    case 'Column':
      return Column(
        children: (json['children'] as List)
            .map((childJson) => buildWidget(childJson))
            .toList(),
      );
    case 'Text':
      return Text(json['value']);
    case 'Button':
      return ElevatedButton(
        onPressed: () {
          print("Button Pressed");  // 여기에 동작을 추가하세요
        },
        child: Text(json['label']),
      );
    default:
      return SizedBox.shrink();  // 타입을 알 수 없을 때 빈 위젯
  }
}
```

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

이 함수는 JSON에서 유형을 확인한 후 (Column, Text 또는 Button과 같은) 해당하는 플러터 위젯을 생성합니다.

# 단계 5: 사용자 상호작용 처리하기

동적 UI에서는 사용자 작업 (예: 클릭)에 응답하는 버튼이나 다른 요소를 원할 수 있습니다. JSON에 작업을 포함시킬 수 있습니다. 예를 들어 버튼이 눌렸을 때 어떤 일이 일어나야 하는지와 같은 내용을 JSON에 포함시킬 수 있습니다.

예시:

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

```js
{
  "type": "Button",
  "label": "Click Me",
  "onPressed": "/doSomething"
}
```

플러터 코드에서 onPressed가 트리거될 때 앱에게 무엇을 할지 알려줄 거예요:

```js
case 'Button':
  return ElevatedButton(
    onPressed: () {
      Navigator.pushNamed(context, json['onPressed']);  // JSON에 따라 탐색
    },
    child: Text(json['label']),
  );
```

# 단계 6: 동적 UI의 이점을 누리세요

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

서버 기반 UI를 사용하면 다음과 같은 작업을 쉽게 할 수 있어요:

- 앱 업데이트: 앱 스토어에 업데이트를 푸시하지 않고 앱의 외관을 쉽게 변경할 수 있어요.
- 다양한 UI 테스트: 서로 다른 버전의 UI를 서로 다른 사용자에게 제공해 가장 잘 작동하는 것을 확인할 수 있어요.
- 경험 개인화: 사용자의 선호도나 위치에 따라 다양한 레이아웃이나 콘텐츠를 보여줄 수 있어요.

# 고려해야 할 도전과제

- 성능: JSON을 가져오고 파싱하는 것이 앱을 느리게 만들 수 있으니 효율적으로 유지하세요.
- 보안: JSON 데이터가 안전하고 앱에만 접근할 수 있도록 보장해야 해요.
- 오류 처리: JSON 데이터를 사용할 수 없거나 잘못된 경우를 대비해 계획하세요.

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

# 결론: 앱 개발의 미래가 여기에 있습니다

서버 기반 UI를 사용하면 플러터 앱을 매우 유연하고 쉽게 관리할 수 있습니다. 이 방법을 통해 사용자 요구사항과 시장 변화에 빠르게 대응할 수 있도록 앱의 인터페이스를 이전보다 더 잘 제어할 수 있습니다.

앱 개발을 더 나은 수준으로 끌어올리려면 준비가 되셨나요? 오늘부터 플러터에서 동적인 서버 기반 UI를 구축해보고 앱 디자인의 미래를 경험해보세요.
