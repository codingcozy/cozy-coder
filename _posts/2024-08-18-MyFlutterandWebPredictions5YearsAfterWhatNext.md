---
title: "2024년 플러터와 웹 개발 현황과 앞으로의 미래"
description: ""
coverImage: "/assets/img/2024-08-18-MyFlutterandWebPredictions5YearsAfterWhatNext_0.png"
date: 2024-08-18 10:39
ogImage: 
  url: /assets/img/2024-08-18-MyFlutterandWebPredictions5YearsAfterWhatNext_0.png
tag: Tech
originalTitle: "My Flutter and Web Predictions 5 Years After What Next"
link: "https://medium.com/@slavaolenin/my-flutter-and-web-predictions-5-years-after-what-next-113b5c36de35"
isUpdated: true
updatedAt: 1723951692677
---

2019년에 Flutter가 웹에 혁명을 가져올 것이라고 예언했었죠. 그리고 제가 그것에 대해 옳았다는 걸 알아버렸습니다!

![이미지](/assets/img/2024-08-18-MyFlutterandWebPredictions5YearsAfterWhatNext_0.png)

다섯 년 전인 2019년, "왜 Flutter가 웹에 새로운 혁명을 가져올 것인가"라는 제목의 기사를 썼었어요. 그 글에서 Flutter가 웹 개발에서 일어날 혁명의 첫 발이라고 언급했죠. 간단히 말해서 다음과 같은 것들을 말했습니다:

- Flutter는 다른 것들이 이후에 이어 나올 시작점일 뿐이에요.
- 다른 언어들도 WASM을 통해 웹을 타겟팅할 것이에요.
- 브라우저들은 자바스크립트/HTML 외의 기능에 더 많은 노력을 기울일 거예요.
- HTML 태그에 국한되지 않는 SEO 최적화에 더 많은 작업이 기대될 것이에요.

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

내가 한 모든 예측에 관심이 있다면, 5년 전의 글을 확인해 보세요. 그런데 정말 유혹적이게 "봤어! 봤어! 내가 말했던 대로 정확히 그렇군!" 이렇게 말하고 싶네요. 여기에 이유가 있습니다:

## 5년 후 우리가 가진 것은 무엇인가요?

우리의 예측들을 개별적으로 검토하고 5년 후 현재 어디에 있는지 살펴봅시다.

## 플러터(Flutter)는 막 시작에 불과합니다.

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

말씀드렸던 대로, Flutter가 웹을 대상으로 처음 등장했습니다. 기본적인 문제를 해결한 후에는 텍스트 선택, 페이지 내 검색, 접근성과 같은 네이티브 브라우저 기능을 처리해야 합니다. 이러한 쟁점들을 정복하면 새로운 플레이어들이 시장에 진입하리라 기대할 수 있습니다. 새로운 프레임워크들을 처음부터 개발해야 하는 것은 아닙니다. 이미 많은 프레임워크가 존재했지만 순수한 성능과 거대한 번들 크기 때문에 브라우저를 대상으로 지향하지 않았던 것이다. 그리고 이제 우리는 선택할 수 있는 다양한 옵션들이 있습니다:

- Leptos나 Dioxus와 같은 러스트 프레임워크는 상대적으로 작은 WASM 번들을 생성하며 훌룡한 성능을 제공하며 UI를 수동으로 렌더링함으로써 순수한 HTML 페이지를 빌드할 수 있습니다.
- Slint와 같은 프레임워크는 임베디드 장치부터 WEB 브라우저까지 다양한 플랫폼에서 매우 성능이 우수하고 아름다운 UI를 구축할 수 있게 합니다.
- Google과 JetBrains는 Kotlin Compose Multiplatform을 통해 웹 지원을 발표했습니다.
- MAUI, Avalonia, Uno는 C# 개발자들에게 웹 지원을 기본으로 제공합니다.

제 이전 기사에서 언급한 대로, 대부분은 HTML을 사용하여 콘텐츠를 렌더링하지 않습니다. 대중적인 채택을 위해 이러한 솔루션들을 더욱 다듬어야 할 작업이 많이 남아 있지만, 이러한 트렌드는 분명합니다.

# 더 많은 언어들이 WASM을 통해 웹을 타겟팅할 것입니다.

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

지금 또 다른 혁명이 일어나고 있습니다:

- 모든 주요 브라우저에서 WasmGC 대출이 이루어지면서 Java, Kotlin, Python 또는 Ruby와 같은 보다 고수준 언어가 WASM 지원을 제공하고 있습니다. 여기서 제가 말하는 지원이란 경쟁력 있는 성능 및 번들 크기를 의미합니다. 이러한 언어들은 JavaScript 문자열을 사용하고 JavaScript 래퍼 없이 DOM을 직접 조작할 수 있게 될 것입니다.
- 한 번 WASM 구성 요소가 안정화되면 다양한 언어로 작성된 완벽하게 상호 운용 가능한 재사용 가능한 WASM 모듈의 생태계가 형성될 것입니다.

# 브라우저는 JavaScript/HTML 기능 이외에도 훨씬 더 많은 노력을 기울일 것입니다.

우리는 여기서도 많은 진전을 이루었습니다:

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

- 대부분의 프레임워크에서 사용되는 렌더링 프레임워크 Skia는 Kotlin Compose Multiplatform, Uno, Avalonia, Flutter 등에서 사용되며 WebGPU 지원이 진행 중입니다.
- Flutter의 임펠러가 모든 모바일 플랫폼에 배포되면 웹으로 이주될 가능성이 높습니다.
- WebGPU 자체는 모든 주요 플랫폼을 지원하는 크로스 플랫폼 솔루션이라고 강조되며, 이는 모든 이러한 프레임워크가 특정 플랫폼에 포대되어 사용할 수 있는 단일 렌더러를 보유할 수 있다는 것을 의미합니다.

# HTML 태그에 꽉 매여있지 않은 SEO 최적화 작업에 더욱 기대할 수 있습니다.

이 부분은 아직 미숙하지만, Flutter 팀은 이미 진행 중임을 발표했습니다.

Rust의 Leptos는 이미 우수한 서버 사이드 기능을 제공하고 있으며, Dioxus도 따라가고 있습니다. 물론, 이들은 HTML에 더 중점을 둔다. 그러나 특정 구현보다는 동향에 대해 이야기하고 있습니다.

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

# 그건 내 자랑 뿐만이 아니에요.

이 기사를 쓸 때 내 예측이 얼마나 훌륭한지 자랑하려고 한다고 생각할 수도 있겠지만, 사실이 아니에요. 아마 조금은 있을지도 모르겠지만, 그건 제게는 자기 성취감을 느끼게 해주는 것과 비슷해요. 그저 제가 찾고 있는 것이며, 그것이 일어나는 걸 보는 건 정말 설레고 기대되는 일이에요.

이 산업은 오랫동안 레거시 오류에 갇혀 있어 왔어요. 자바스크립트는 그 길을 따라가기 위해 설계된 게 아니었는데, 수천 명의 높은 재능을 가진 개발자들이 지난 30년 동안 그 문제를 고치느라 애를 썼어요.

지금 일어나고 있는 일은 post-JavaScript로의 매우 부드러운 이주, 자바스크립트는 그대로 남아 있지만 또 다른 WASM 지원 언어로 존재할 거에요.

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

# 다음은 무엇인가요?

다음 5년 이상 동안 어떤 일이 일어날 것으로 예상하고 있는지 알고 싶으신가요?

## 작은 WASM 번들

산업은 계속해서 WASM 번들 크기를 줄이는 데 초점을 맞출 것입니다. 최소한 최소화된 JavaScript 기반 번들과 비교할 수 있는 수준으로. 그것은 다양한 형태로 나타날 수 있지만, 뚜렷한 변화로 나타날 것입니다:

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

- WASM은 더 많은 내장 기능을 노출할 것입니다. 예를 들어, WasmGC를 통해 GC 기반 언어는 자체 GC를 번들로 제공할 필요가 없게 됩니다. 새로운 공유 문자열 제안이 진행 중에 있습니다. 기본적으로 브라우저들은 Windows가 DLL로 수행하는 것처럼 많은 기능들을 기본 제공할 것입니다.
- WASM 구성 요소 — 독립적인 WASM 모듈이 서로 통신할 수 있는 경우, 가장 일반적인 WASM 모듈은 CDNs에서 호스팅되어 대부분의 사용자 브라우저 캐시에 저장될 것입니다. 그렇지 않더라도, WASM 모듈을 분리함으로써 거대한 빌드를 더 작은 모듈로 분할하여 필요할 때 필요에 따라 지연로드할 수 있게 될 것입니다.

## 더 많은 기능

이것은 직관적입니다 — JavaScript에서 제공하는 모든 기능은 WASM에서도 사용할 수 있을 것입니다.

## 더 나은 도구

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

WasmGC가 소개되면서 Google, Microsoft, JetBrains와 같은 주요 기업들이 다양한 언어를 웹에서 적극적으로 홍보하는 것을 볼 수 있습니다.

Kotlin/WASM은 알파 단계에 있으며, Dart는 이미 안정화되었으며, 마이크로소프트는 .NET에 대한 공식 WASI 지원을 발표하고 있습니다.

이러한 기업들은 자사의 IDE를 계속 향상시켜 WASM을 쉽게 지원하고 있습니다. 커뮤니티도 해당 분야에서 놀라운 진전을 이루고 있습니다. 어제만 해도 flutter_rust_pridge 예제 앱을 시도해봤습니다. Rust/Flutter 앱을 만들고 iOS, Android, MacOS 및 웹에서 실행하는 것이 얼마나 쉬웠는지 놀랍습니다. 약간의 마법같은 느낌이 듭니다. 실제로 JavaScript/WASM 애플리케이션을 만들어보고 소요 시간을 비교해보세요.

# 최종 추측

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

자바스크립트 엔진은 자바스크립트를 빠르게 컴파일하고 최적화하여 성능을 향상시키기 위해 필요한 작업량 때문에 현재 브라우저에서 가장 복잡한 시스템 중 하나입니다. 그래서 WASM이 자바스크립트와 기능적으로 호환되면, 자바스크립트 컴파일러를 V8과 같은 런타임으로부터 분리하여 순수하게 WASM 전용 런타임으로 변경하는 것이 합리적입니다.

이렇게 하면 자바스크립트는 WASM으로 컴파일할 수 있는 또 다른 언어가 될 것입니다. 한동안 (아주 오랜 기간 동안), 브라우저는 기존 웹 사이트를 위해 자바스크립트 컴파일러를 번들로 제공할 것이지만, 내부적으로는 JS 파일을 WASM 모듈로 컴파일할 것입니다.

브라우저 공급업체들은 브라우저에서의 복잡성을 상당 부분 컴파일러와 개발자에게 오프로딩하여 분리할 수 있습니다. 생산성을 높이고 성능이 걱정되지 않는다면 루비를 사용하세요. 그렇지 않으면 러스트나 스위프트로 구축하세요.

저의 주장은 순수한 추측에 불과하지만, 미래 어느 날에는 최적화된 WASM을 생성할 수 있는 널리 채택된 JS의 LLVM 아날로그가 등장할 가능성이 매우 높습니다. 그리고 이는 더 간단하고 가벼운 순수 WASM 런타임을 모든 주요 브라우저에 함께 제공할 것입니다.

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

너무나도 믿기 어려울 수도 있겠지만! JIT와 그에 따른 모든 최적화에 대해 어떻게 생각하시나요? 이미 React Native 세계에서 그런 변화가 일어나고 있다는 것을 알 수 있어요. React Native가 JIT 지원이 없는 JavaScript 런타임인 Hermes로 이동한 후에 성능 문제가 발생한 것을 몇 명이나 알고 있었나요?

# 여러분은 무엇을 생각하시나요?

저는 미래에 대해 여러분의 생각을 알고 싶어해요. 동의하시는 부분이 있나요? 특히 저의 최종 추측에 대해 궁금하네요. 너무나도 믿기 어려울 것 같다고 생각하시나요?

건배해요!
