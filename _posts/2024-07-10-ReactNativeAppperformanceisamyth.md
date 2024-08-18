---
title: "React Native 앱 성능은 과장인가"
description: ""
coverImage: "/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_0.png"
date: 2024-07-10 01:34
ogImage:
  url: /assets/img/2024-07-10-ReactNativeAppperformanceisamyth_0.png
tag: Tech
originalTitle: "React Native App performance is a myth?"
link: "https://medium.com/@anil-gudigar/react-native-app-performance-is-a-myth-dfe7b141b812"
isUpdated: true
---

`<img src="/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_0.png" />`

JavaScript와 React를 사용하여 크로스 플랫폼 모바일 앱을 구축하기 위해 React Native는 인기 있는 프레임워크 중 하나입니다. 네이티브 UI 구성 요소와 API를 활용하여 개발자들은 플랫폼 간에 코드를 간소화할 수 있습니다. 그러나 React Native는 성능과 개발자 참여에 영향을 미치는 다양한 도전에 직면하고 있습니다.

여기에서는 최신 아키텍처가 이러한 문제를 해결하는 방법에 대해 살펴보겠습니다.

# 구식 아키텍처: 브릿지와 일괄 브릿지

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

React Native의 이전 아키텍처는 JavaScript와 네이티브 환경을 연결하는 브릿지 메커니즘을 활용하여 데이터 직렬화, 스레딩 및 동기화를 관리했습니다. 그러나 이 브릿지는 성능과 개발자 경험에 영향을 미치는 단점과 병목 현상을 도입했습니다.

![image](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_1.png)

● JavaScript 스레드: JavaScript 코드 번들이 JavaScript 엔진 (JSC, Hermes 등)의 도움을 받아 이 스레드에서 실행됩니다.

● 네이티브 스레드: 네이티브 UI 렌더링 및 사용자 이벤트 처리 (클릭, 스와이프 등)가 네이티브 스레드에서 실행됩니다.

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

**이것이 리액트 네이티브 브릿지입니다.**

두 개의 서로 다른 세계에서 별도로 작동하는 이 2개의 스레드를 연결하는 방법이 필요하며 서로의 기술을 모릅니다. 전체 시스템이 작동하려면 서로 대화할 수 있는 메커니즘이 있어야 합니다. 이 두 개의 별도 스레드를 통신하는 데 도움이 되는 메커니즘을 브릿지라고 합니다.

리액트 네이티브의 핵심 장점은 모바일 플랫폼에서 네이티브 UI 뷰를 호출한다는 것입니다. 브릿지 기술 덕분에 응용 프로그램 로직을 네이티브 측 UIView 매니저와 연결하는 것이 가능해집니다.

![React Native Bridge](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_2.png)

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

React Native 브릿지는 데이터를 JSON 형식으로 변환합니다. 사용자가 모바일 화면에서 작업을 수행하면 UI 스레드가 주 JavaScript 스레드로 보고합니다. 데이터를 JSON 메시지로 묶어서 JS 스레드로 전송합니다.

네이티브 및 자바스크립트 스레드는 어떻게 통신할까요?

![React Native App Performance](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_3.png)

JavaScriptCore + ReactJS + 브릿지가 합쳐져 React Native이 됩니다.

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

위 다이어그램은 JavaScript 모듈이 호출되거나 실행되는 방식을 보여줍니다.

- JavaScriptCore: JS 코드 해석 및 실행을 담당합니다.
- ReactJS: VirtualDom의 설명 및 관리, 네이티브 컴포넌트를 그리고 업데이트하는 방향을 지시하며, 많은 계산 논리가 JS에서도 수행됩니다. ReactJS 자체적으로 UI를 직접 그리지 않습니다. UI 그리기는 매우 시간이 많이 소요되는 작업이며, 네이티브 컴포넌트가 이 작업에 가장 적합합니다.
- Bridges: ReactJS 그리기 지시를 네이티브 컴포넌트에 대한 그리기 지시로 번역하는 데 사용되며, 동시에 네이티브 컴포넌트가 수신한 사용자 이벤트를 ReactJS에 피드백합니다.

- BridgeNative 코드는 네이티브 모듈을 관리하고 JS 코드가 호출할 수 있는 해당 JS 모듈 정보를 생성합니다. 각각의 기능적 JS 레이어 캡슐화는 주로 ReactJS에 적응되어 네이티브 모듈의 기능이 ReactJS를 사용하여 호출되기 쉽도록 합니다. MessageQueue.js는 JS 레이어에서 Bridge의 프록시이며, 모든 JS2N 및 N2JS 호출은 이를 통해 전달됩니다. JS와 Native 간의 포인터 전달은 없으며, 모든 매개변수는 문자열로 전달됩니다. 모든 인스턴스는 JS와 Native 양쪽에서 번호가 매겨지고, 매핑이 수행된 후 번호/문자열 번호가 교차 개체를 찾는 기준으로 사용되어 경계를 넘나드는 개체를 찾습니다.

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

The graph visualizes a snapshot of JavaScript execution mid-process.

![Graph](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_5.png)

The sequence usually begins from native code\* calling into JS. As it proceeds, methods are called on NativeModules, prompting the queuing of calls for execution on the native side. After JS completes its part, the native side processes the enqueued calls. During this phase, callbacks and bridge calls (utilizing the \_bridge instance from a native module to perform enqueueJSCall:args:) are employed to trigger calls back into JS.

From JavaScript to Java calls

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

**MessageQueue.js**  
여기서 두 곳 사이의 모든 통신이 처리됩니다. 메시지는 JSON-RPC와 비슷한 형식을 가지며, JavaScript에서 네이티브로의 메서드 호출 및 네이티브에서 JavaScript로의 콜백 전달을 처리합니다. 이것이 JavaScript 컨텍스트와 네이티브 컨텍스트 사이의 유일한 연결입니다. 모든 것이 MessageQueue를 통해 전달되며, 모든 네트워크 요청, 네트워크 응답, 레이아웃 측정, 렌더 요청, 사용자 상호작용, 애니메이션 순서 지시, 네이티브 모듈 호출, I/O 작업 등이 해당됩니다. MessageQueue가 혼잡하지 않도록 유지하는 것은 앱이 원활하게 작동하는 것을 보장하는 데 중요합니다.

**이미지 태그 Markdown 형식으로 변환**  
![이미지](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_7.png)

사용자가 클릭한 내용이 네이티브에서 어떻게 처리되는지에 대한 전체 시각화입니다.

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

# 성능 문제와 제한 사항은 무엇인가요?

과거의 아키텍처에는 몇 가지 제한 사항이 있습니다:

- 직렬화 및 역직렬화 오버헤드: JSON, Java 객체 및 Objective-C 객체와 같은 형식 간 데이터 변환은 CPU 시간과 메모리 사용량을 증가시키며, 특히 대규모 또는 복잡한 데이터 구조의 경우 그 영향이 커집니다.
- 비동기성과 지연: 배치 모드로 작동하는 경우 브릿지는 일정 간격으로 메시지를 보내거나 큐가 최대 용량에 도달했을 때 메시지를 보냅니다. 이로 인해 메시지를 보내고 받는 사이에 지연이 발생하며, 특히 애니메이션 및 제스처와 같은 대화형 기능에 반응성이 영향을 받습니다.
- 네이티브 모듈의 한계: 브릿지는 문자열, 숫자, 부울, 배열 및 맵과 같은 일부 네이티브 기능 및 유형만 지원합니다. 결과적으로 네이티브 모듈은 이러한 제약 조건과 API를 조정해야 하며, 이미지, 비디오 및 스트림과 같이 더 복잡하거나 사용자 정의 유형이 필요한 기능에 대해 기능이나 성능이 저하될 수 있습니다.
- 비동기적: 한 쪽에서 데이터를 브릿지로 제출하고 다른 쪽이 그것을 처리할 때까지 비동기적으로 “대기”했습니다.
- 단일 스레드: 자바스크립트 쪽은 단일 스레드에서 작동하며, 따라서 그 세계에서의 계산은 그 단일 스레드에서 수행되어야 했습니다.
- 추가 오버헤드 부과: 한 쪽이 다른 쪽을 사용해야 할 때마다 데이터 정보를 직렬화해야 했습니다. 다른 쪽은 데이터를 역직렬화해야 했습니다. 선택한 형식은 간단하고 사람이 읽을 수 있는 JSON 형식이었지만, 가벼움에도 불구하고 비용이 발생했습니다.
- Android APK에 번들로 포함된 JSC: “JavaScriptCore” 엔진이 iOS 기기에만 존재하기 때문에 Android APK 번들에 JSC 엔진을 포함해야 합니다.
- 브릿지가 병목현상을 초래: 브릿지는 빠른 통신을 위한 병목구간입니다.
- 네이티브 모듈은 시작 시로드해야 함: 양쪽 모두 서로와 그들의 객체 유형을 모르기 때문에 새로운 종류의 문제가 발생했으며, JavaScript는 필요할 때 모듈을 시작할 수 없으므로 응용 프로그램은 응용 프로그램 시작 시 모든 네이티브 모듈을 로드해야 합니다.

# 새로운 아키텍처: JSI, Fabric 및 Turbo Modules

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

## 선결 조건

- React Native 0.69 - 이 버전은 새로운 아키텍처를 소개합니다.
- TypeScript 앱 - Codegen을 사용하기 위해 타입을 사용해야 합니다.

![React Native App Performance](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_8.png)

## JSI (JavaScript Interface)

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

JSI (JavaScript Interface)는 C++로 작성되었습니다. 이는 구식 아키텍처에서의 브릿지를 대체하고 JavaScript 객체 및 함수에 대한 직접적인 네이티브 인터페이스를 제공했습니다.

이 브릿지는 JavaScript 인터페이스라는 모듈로 대체될 예정입니다. JavaScript 엔진이 네이티브 영역에서 메서드를 직접 호출할 수 있도록하는 C++로 작성된 가벼운 범용 레이어입니다.

![Click here for the image](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_9.png)

JSI를 통해 네이티브 메서드는 C++ 호스트 객체를 통해 JavaScript에 노출될 것입니다. JavaScript는 이러한 객체에 대한 참조를 유지할 수 있습니다. 그리고 해당 참조를 사용하여 메서드를 직접 호출할 수 있습니다. 이는 JavaScript 코드가 웹에서 DOM 요소에 대한 참조를 보유하고 해당 요소에서 메서드를 호출할 수 있는 것과 비슷합니다. 예를 들어: 다음과 같이 작성하면:

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

여기서 컨테이너는 JavaScript 변수이지만 아마도 C++에서 초기화된 DOM 요소에 대한 참조를 보유하고 있습니다. "컨테이너" 변수에 대해 어떤 메소드를 호출하면, DOM 요소에서 해당 메소드를 호출하게 될 것입니다. JSI는 비슷한 방식으로 작동합니다.

브리지와 달리, JSI는 JavaScript 코드가 Native 모듈에 대한 참조를 보유할 수 있게 합니다. JSI를 통해 JavaScript는 이 참조에 직접적으로 메소드를 호출할 수 있습니다.

![Turbo Modules](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_10.png)

# Turbo Modules

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

Turbo Modules는 이전 네이티브 모듈 시스템을 대체한 네이티브 모듈 시스템입니다.

새로운 네이티브 모듈 시스템은 "터보 모듈"로 불리며 JSON 데이터 직렬화 대신 JSI 기술을 사용합니다. JSI(JavaScript Interface)는 C++로 작성되었습니다.

![이미지](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_11.png)

구(舊) 아키텍처에서, JavaScript가 사용하는 모든 네이티브 모듈(예: 블루투스, 위치 정보, 파일 저장소, 카메라 등)는 앱이 여는 동안 초기화되어야 합니다. 이 말은 즉, 사용자가 특정 모듈이 필요하지 않더라도 시작할 때 초기화되어야 한다는 것을 의미합니다.

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

터보 모듈은 예전 네이티브 모듈들을 향상시킨 것입니다. 지난 글에서 보신 것처럼 이제부터 자바스크립트는 이러한 모듈에 대한 참조를 가질 수 있게 되었습니다. 이는 JS 코드가 각 모듈이 필요할 때만 로드할 수 있게 해줍니다. 이는 리액트 네이티브 앱의 시작 시간을 크게 개선할 것입니다.

어플리케이션이 4번째 화면에서 카메라 모듈이 필요한 경우, 그때 필요한 시점에 카메라 모듈을 로드할 수 있습니다. 이처럼 네이티브 모듈의 on-demand 로딩은 어플리케이션의 상호작용 시간(TTI)에 많은 도움이 됩니다.

- 터보 모듈은 리액트 네이티브에서 "네이티브 모듈"을 구현하는 새로운 방법입니다. JSI 및 "코드 생성기에 의해 생성된 네이티브 코드"를 활용하여 네이티브 모듈을 구현합니다.
- 새로운 아키텍처에서는 터보 모듈이 사용자가 앱을 시작할 때 블루투스, 지오로케이션, 파일 저장소와 같은 네이티브 모듈을 지연로딩하는 방식을 도입했습니다. 예전 아키텍처에서는 앱에 사용된 모든 네이티브 모듈이 시작할 때 초기화되어야 했지만, 이제는 필요한 모듈만 초기화됩니다.

# 패브릭 (새로운 렌더링 엔진)

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

Fabric은 디바이스에서 UI를 렌더링하는 데 책임을지는 UIManager입니다. 지금의 차이는 Fabric이 브릿지를 통해 JavaScript와 통신하는 대신, Fabric이 JavaScript를 통해 자체 기능을 노출하여 JS 측과 Native 측이 ref 함수를 통해 직접 통신할 수 있다는 것입니다. 양쪽 간에 데이터를 전송하는 것이 뛰어날 것입니다.

React Native 렌더러는 React 로직을 호스트(예: iOS, Android) 플랫폼에 렌더링하기 위해 일련의 작업을 거칩니다. 이 작업 시퀀스를 렌더 파이프 라인이라고하며 UI 상태의 초기 렌더링 및 업데이트에 대해 발생합니다.

![Fabric Image](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_12.png)

- Fabric은 브릿지를 사용하지 않고 Hermes 및 네이티브 코드와 통신하기 위해 JSI를 사용합니다.
- Fabric은 React Native의 새로운 렌더링 시스템으로, 프레임워크의 호스트 플랫폼(네이티브 측 플랫폼인 Android 또는 iOS 같은 플랫폼)과의 상호 운용성을 향상시키고 JavaScript와 네이티브 스레드 간 통신을 개선하고자 합니다.

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

렌더 파이프라인은 일반적으로 세 가지 단계로 나눌 수 있습니다:

# 코드 생성 (네이티브 코드 생성기)

그래서, C++은 강력한 타입 지정 언어이고 JavaScript/TypeScript는 강력한 타입 지정 언어가 아닙니다. JavaScript와 C++ 간에 통신을 위해 인터페이스가 있어야 합니다. “CodeGen”은 JavaScript/TypeScript에서 인터페이스를 생성하는 데 도움이 되는 도구입니다. 생성된 인터페이스는 C++ 세계와 JavaScript 세계 간에 통신을 돕습니다.

“CodeGen”은 적절한 기둥은 아니지만 많은 반복 코드를 작성하는 것을 피할 수 있는 도구입니다. “CodeGen”을 사용하는 것은 강제적이지 않습니다. 그것으로 생성된 모든 코드는 수동으로 작성할 수도 있습니다. 그러나 많은 시간을 절약해 줄 수 있는 프레임워크 코드를 생성합니다. “CodeGen”은 iOS 또는 Android 앱을 빌드할 때 React Native에 의해 자동으로 호출됩니다.

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

![React Native Performance](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_13.png)

안녕하세요! 오늘은 React Native의 새 아키텍처에서 Codegen이 하는 역할 중 2가지에 대해 이야기해보려고 해요.

첫 번째는 정적 타입 체크인데요:

- 자바스크립트는 동적 타입(변수의 유형을 정의할 필요가 없음) 언어이고, JSI는 C++로 작성되어 정적 타입(변수의 유형을 정의해야 함) 언어에요. 따라서 두 언어 간의 통신 문제가 발생할 수 있어요(Javascript와 C++ 사이).
- 그래서 새 아키텍처에는 JavaScript와 C++ 간의 타입 관련 통신 문제를 해결하는 CodeGen이 포함되어 있답니다.

이렇게 코드를 번역해보았어요! 다른 궁금한 거 있으면 언제든 물어보세요. 😉

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

"네이티브 코드"를 생성합니다:

코드 생성은 새 아키텍처의 두 가지 주요 구성 요소에 대한 네이티브 코드를 생성하는 역할을 합니다. 이 두 가지는:

![이미지](/assets/img/2024-07-10-ReactNativeAppperformanceisamyth_14.png)

# 새 아키텍처의 장점은 무엇인가요?

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

- 성능 개선: JSI(Javascript Interface)는 직렬화 및 역직렬화의 부담을 제거하여 CPU 시간과 메모리 사용량을 줄입니다. Fabric은 동시 렌더링을 가능하게 하여 React가 주 스레드를 차단하지 않고 여러 구성 요소를 동시에 렌더링할 수 있습니다. Turbo Modules는 동기식 네이티브 호출을 가능하게하여 대기 시간을 줄이고 응답 시간을 개선합니다.
- 개발자 경험 향상: JSI를 통해 개발자들은 Hermes 또는 JSC와 같은 원하는 JavaScript 엔진을 사용할 수 있습니다. Fabric은 Suspense, Concurrent Mode, Server Components 등 React 18의 모든 기능을 지원합니다. Turbo Modules는 추가 래퍼나 어댑터가 필요 없이 이미지, 비디오, 스트림 등과 같은 더 많은 네이티브 기능과 유형을 지원합니다.
- 호환성 향상: JSI는 어떤 JavaScript 엔진이나 네이티브 플랫폼과 상호 작용하기 위한 통일된 인터페이스를 제공합니다. Fabric은 React DOM 또는 React Native Web과 동일한 원칙과 API를 따릅니다. Turbo Modules는 JavaScript 모듈 또는 ES6 모듈과 같은 규칙과 패턴을 따릅니다.
- 동기 실행: 이제 처음부터 비동기적이어서 말이 안 되었던 기능을 동기적으로 실행할 수 있습니다.
- 동시성: JavaScript에서 서로 다른 스레드에서 실행되는 함수들을 호출할 수 있습니다.
- 오버헤드 감소: 새로운 아키텍처는 더 이상 데이터를 직렬화/역직렬화할 필요가 없으므로 직렬화 세금을 지불할 필요가 없습니다.
- 유형 안정성: JS가 C++ 객체의 메서드를 올바르게 호출하고 그 반대에 대한 호출을 제대로 처리할 수 있도록 자동 생성된 코드 레이어가 추가되었습니다. 이 코드는 Flow 또는 TypeScript를 통해 유형을 지정해야 합니다.

# 새 아키텍처로 마이그레이션하는 방법

React Native의 새로운 아키텍처는 여전히 실험적이며 개발 중입니다. 그러나 React Native 0.68 이상에서 새 앱 템플릿을 선택함으로써 시도해 볼 수 있습니다. 새 앱 템플릿에는 새 아키텍처를 시작하는 데 도움이 되는 상세한 주석과 문서가 포함되어 있습니다.

기존 앱이나 라이브러리를 새 아키텍처를 사용하도록 마이그레이션하려면 React Native 웹사이트의 마이그레이션 가이드를 따를 수 있습니다. 이 마이그레이션 가이드에는 Android 및 iOS 프로젝트 전체에 새 아키텍처를 적용하기 위해 따라야 할 단계가 개요로 제공됩니다. 또한 사용자 정의 Fabric 구성 요소나 Turbo Modules을 생성하는 데 대한 예제와 모범 사례를 제공합니다.
