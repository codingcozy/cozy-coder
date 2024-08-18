---
title: "Angular 17의 새로운 기능들"
description: ""
coverImage: "/assets/img/2024-07-06-Angular17Newfeatures_0.png"
date: 2024-07-06 11:06
ogImage:
  url: /assets/img/2024-07-06-Angular17Newfeatures_0.png
tag: Tech
originalTitle: "Angular 17 New features"
link: "https://medium.com/@harshitmangla96/angular-17-new-features-2d395ebf1429"
isUpdated: true
---

저는 새 프로젝트로 Angular 17을 사용하고 있어요. 그래서 그 기능을 활용하고 프로젝트에 적용하려고 하고 있어요.
지금까지 배운 것들을 담은 기사를 쓰기로 결정했습니다.

![](/assets/img/2024-07-06-Angular17Newfeatures_0.png)

1. 독립된 컴포넌트
   독립된 컴포넌트는 NgModule에서 선언하지 않고도 컴포넌트를 만들 수 있는 새로운 기능입니다. 이것은 컴포넌트 선언 프로세스를 간소화하고 보일러플레이트 코드를 줄입니다. 이제 app.module.ts 파일에서 모든 컴포넌트를 import할 필요가 없어요. standalone 매개변수를 true로 설정하면 선언할 필요가 없어집니다.

## 예시:

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
import { Component } from "@angular/core";

@Component({
  selector: "app-root",
  standalone: true,
  template: `<h1>Angular 17에 오신 것을 환영합니다!</h1>`,
  styleUrls: ["./app.component.css"],
})
export class AppComponent {}
```

Angular의 Directive Composition API를 사용하면 여러 디렉티브를 한 개의 디렉티브로 구성할 수 있습니다. 이 새로운 기능은 복잡한 동작을 관리하기가 간단해지고 여러 디렉티브의 기능을 결합하여 코드 재사용성을 높이는 것을 가능하게 합니다.

## 2. Directive Composition API를 사용하는 이유

- 복잡한 동작 단순화: 요소에 여러 디렉티브를 적용하는 대신, 이를 하나의 디렉티브로 구성할 수 있습니다.
- 코드 재사용성 향상: 자주 사용되는 디렉티브 조합을 하나의 재사용 가능한 디렉티브로 구성할 수 있습니다.
- 코드 가독성 향상: 템플릿에서 요소에 직접 적용되는 디렉티브의 수를 줄여 템플릿을 깨끗하게 유지할 수 있습니다.

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

## 단계 1: 개별 지시문 생성하기

```js
// highlight.directive.ts
import { Directive, ElementRef, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {
    this.renderer.setStyle(this.el.nativeElement, 'backgroundColor', 'yellow');
  }
}

// bold.directive.ts
import { Directive, ElementRef, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appBold]'
})
export class BoldDirective {
  constructor(private el: ElementRef, private renderer: Renderer2) {
    this.renderer.setStyle(this.el.nativeElement, 'fontWeight', 'bold');
  }
}
```

## 단계 2: 병합 지시문 생성하기

다음으로, hostDirectives 속성을 사용하여 개별 지시문을 조합하는 새로운 지시문을 만드세요.

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

```typescript
// combined.directive.ts
import { Directive } from "@angular/core";
import { HighlightDirective } from "./highlight.directive";
import { BoldDirective } from "./bold.directive";

@Directive({
  selector: "[appCombined]",
  standalone: true,
  hostDirectives: [HighlightDirective, BoldDirective],
})
export class CombinedDirective {}
```

## 단계 3: 구성 지시자를 컴포넌트에서 사용

마지막으로, 컴포넌트 템플릿의 요소에 구성 지시자를 적용하십시오.

```typescript
// app.component.ts
import { Component } from "@angular/core";
import { CombinedDirective } from "./combined.directive";

@Component({
  selector: "app-root",
  standalone: true,
  imports: [CombinedDirective],
  template: ` <div appCombined>이 텍스트는 굵고 강조되어 표시됩니다!</div> `,
  styleUrls: ["./app.component.css"],
})
export class AppComponent {}
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

# 3. 앵귤러 시그널

앵귤러 17에서는 앵귤러 시그널을 소개합니다. 이는 세밀한 반응성을 위한 반응형 기본 요소로, 이 기능을 통해 개별 구성 요소 및 요소의 반응성을 더욱 잘 제어할 수 있습니다.

```js
// counter.component.ts
import { Component, signal } from "@angular/core";

@Component({
  selector: "app-counter",
  standalone: true,
  template: `
    <div>
      <p>Counter: { counter() }</p>
      <button (click)="increment()">Increment</button>
    </div>
  `,
})
export class CounterComponent {
  counter = signal(0);

  increment() {
    this.counter.update((count) => count + 1);
  }
}
```

# 4. 향상된 수분화

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

앵귤러 17에는 성능을 향상시키는 새로운 수분화 기능이 포함되어 있습니다. 이 기능은 서버 측 렌더링 애플리케이션의 성능을 향상시키고 클라이언트 측 애플리케이션의 수분화를 빠르고 효율적으로 할 수 있게 합니다.

수분화는 현대 웹 애플리케이션에서 중요한 프로세스로, 서버 렌더링 정적 콘텐츠와 클라이언트 측 상호 작용 사이의 간극을 좁히는 역할을 합니다. 개선된 수분화 기능을 통해 앵귤러와 같은 프레임워크는 더 빠르고 효율적이며 SEO에 유리한 웹 애플리케이션을 제공할 수 있습니다.

# 5. 개선된 개발 경험

앵귤러 17은 개선된 에러 메시지, 빠른 빌드 시간 및 향상된 TypeScript 지원을 통해 전체적인 개발자 경험을 향상시키는 데 초점을 맞추고 있습니다.

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

아주 큰 업데이트는 아니지만, Angular의 17 버전에서 공정한 업데이트가 있어요. 여전히 14-15 버전으로 SPA를 만들 수 있어요. 성능은 분명히 더 좋을 거에요, 하지만 주요 기능 면에서는 큰 업데이트가 없어요.
