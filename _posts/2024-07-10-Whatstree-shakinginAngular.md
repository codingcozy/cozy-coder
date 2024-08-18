---
title: "Angular에서 트리 쉐이킹이란 무엇인가요"
description: ""
coverImage: "/assets/img/2024-07-10-Whatstree-shakinginAngular_0.png"
date: 2024-07-10 00:56
ogImage:
  url: /assets/img/2024-07-10-Whatstree-shakinginAngular_0.png
tag: Tech
originalTitle: "What’s tree-shaking in Angular?"
link: "https://medium.com/angular-training/whats-tree-shaking-in-angular-da95c346114c"
isUpdated: true
---

![이미지](/assets/img/2024-07-10-Whatstree-shakinginAngular_0.png)

앵귤러 프로젝트 전체의 크기를 조사하고 앵귤러에 의해 컴파일된 코드( dist 폴더 내 빌드 출력물)와 비교해 본 적이 있나요?

차이는 거의 믿기 어렵습니다. 일반적으로 앵귤러 앱의 전체 프로젝트 폴더는 약 600MB 이상이며 이는 코드뿐만 아니라 모든 종속성, Angular CLI, TypeScript 컴파일러 등을 포함한 여러 가지를 포함합니다.

![이미지](/assets/img/2024-07-10-Whatstree-shakinginAngular_1.png)

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

ng build를 실행한 후에는 dist 폴더가 600MB보다 훨씬 작아지는 것을 확인할 수 있습니다. 보통 몇 KB 또는 MB 정도로, 어플리케이션의 크기에 따라 다릅니다.

이것은 트리 쉐이킹이라는 기술 덕분입니다.

우리는 우리 코드가 프로젝트 폴더에 있는 것 중 일부에만 의존한다는 것을 알고 있습니다. 실수로 다운로드한 의존성이지만 사용되지 않거나 사용하지 않는 Angular 모듈이 있는 경우가 있습니다. 아래는 샘플 앱이 사용할 수 있는 예시입니다:

![What is tree-shaking in Angular](/assets/img/2024-07-10-Whatstree-shakinginAngular_2.png)

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

또한, 컴파일러와 단위 테스트와 관련된 모든 코드는 프로덕션에서는 필요하지 않습니다. 따라서 빌드 프로세스는 그것들을 모두 제거하는 방법을 알고 있습니다.

![이미지](/assets/img/2024-07-10-Whatstree-shakinginAngular_3.png)

트리 쉐이킹은 우리 애플리케이션이 필요로 하는 최소한의 코드만 유지하는 것을 의미합니다. 다른 말로, 빌드 프로세스는 우리 코드 트리를 흔들어, 우리 코드 베이스의 가지에 "묶여 있지 않은" 코드는 나무의 잎처럼 떨어지게 만듭니다:

![이미지](/assets/img/2024-07-10-Whatstree-shakinginAngular_4.png)

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

우리의 빌드 출력 크기가 이미 상당히 줄어들었습니다. 하지만 Angular의 최신 기능들은 트리 쉐이킹을 더욱 발전시키는 데 도움이 됩니다. 과거에는 앱 내에서 전체 모듈을 가져와서 사용할 서브셋만 사용하는 경우에도 앱 크기가 커지는 문제가 있었습니다.

예를 들어, 100개의 컴포넌트 및 디렉티브를 포함하는 컴포넌트 라이브러리를 종속성으로 지정하고 5개 또는 6개만 사용할 수도 있습니다. 우리는 라이브러리의 모든 기능을 100% 활용하는 경우는 드뭅니다. 대신, 라이브러리의 일부 함수나 클래스를 필요로 할 뿐입니다.

이것이 독립적인 구성요소의 강점입니다. 독립 기능을 사용하면, 사용하지 않을 기능이 포함된 전체 모듈을 가져오는 대신에 개별 컴포넌트, 디렉티브, 파이프를 가져와서 Angular의 트리 쉐이킹을 효과적으로 할 수 있습니다:

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

<img src="/assets/img/2024-07-10-Whatstree-shakinginAngular_6.png" />

<img src="/assets/img/2024-07-10-Whatstree-shakinginAngular_7.png" />

# Angular 트리 쉐이킹을 개선할 수 있을까요?

Angular 앱에서 import 구문을 사용할 때마다 컴파일러에게 말하는 것입니다: 이 종속성이 이 컴포넌트/파이프/디렉티브/서비스를 실행하는 데 필요하다. 사용되지 않는 import를 정리하면 트리 쉐이킹이 제품에서 필요없는 모든 코드를 제거하는지 확인하는 쉬운 방법이 될 수 있습니다.

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

웹에서는 사이즈가 중요합니다: 사용자가 다운로드해야 하는 코드가 많을수록 응용 프로그램은 느려지는데, 이는 코드를 다운로드하고 구문 분석하고 실행한 후에 HTML 응용 프로그램이 렌더링되기 때문입니다.

또한 응용 프로그램 크기를 제어하기 위해 예산을 활용할 수 있으며, 응용 프로그램의 크기를 급격하게 증가시키는 일부 종속성이 발생할 때 빌드 시간에 경고 또는 심지어 오류를 받을 수 있습니다.
