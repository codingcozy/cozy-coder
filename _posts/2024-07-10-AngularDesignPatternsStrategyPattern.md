---
title: "Angular 디자인 패턴 전략 패턴 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-AngularDesignPatternsStrategyPattern_0.png"
date: 2024-07-10 00:54
ogImage: 
  url: /assets/img/2024-07-10-AngularDesignPatternsStrategyPattern_0.png
tag: Tech
originalTitle: "Angular Design Patterns: Strategy Pattern"
link: "https://medium.com/@vugar-005/angular-design-patterns-strategy-pattern-ace359ae77b3"
isUpdated: true
---






![Image](/assets/img/2024-07-10-AngularDesignPatternsStrategyPattern_0.png)

# Intro

The Strategy pattern is a behavioral design pattern that provides a mechanism to select an algorithm at runtime from a family of algorithms, and make them interchangeable. In context of Angular, you can think algorithms as services here but it can also be used for components and classes.

🗎 Source Code


<div class="content-ad"></div>

➡️ 권장 사전 요구 사항

💎향상된 마크다운 버전

# 주요 이점

- 목적: 상호 교환 가능한 알고리즘 패밀리를 정의하고 클라이언트가 실행 중에 어떤 것을 사용할지 동적으로 선택할 수 있도록 합니다. 이는 유연성을 제공합니다.
- 캡슐화: 각 전략은 별도의 클래스로 캡슐화되어 있어 코드를 깨끗하고 조직화된 상태로 유지하는 데 도움이 됩니다. 클라이언트 코드는 각 전략이 어떻게 구현되었는지의 세부 사항을 알지 못하며, 공통 인터페이스와만 상호 작용합니다.
- 상속보다 합성: 전략 패턴은 상속을 의존하는 대신에 행위 재사용을 달성하기 위해 합성에 기반을 둔 것입니다. 이는 유연한 디자인을 이끌어 냅니다.

<div class="content-ad"></div>

# 유용한 시나리오 💎

- 한 문제를 해결하는 데 서로 교차로 사용할 수 있는 여러 알고리즘이나 접근 방식이 있는 경우.

# 클래식 시나리오

내비게이션 앱: 내비게이션은 차량, 보행자 또는 자전거 탑승자를 위해 다른 경로 지정 전략을 사용할 수 있습니다.

<div class="content-ad"></div>

정렬 알고리즘: 다양한 정렬 알고리즘(퀵 정렬, 버블 정렬, 병합 정렬)은 전략으로 구현될 수 있어서 실행 중에 가장 적합한 것을 선택할 수 있게 해줍니다.

# 용어집 🌍

1. 컨텍스트:

컨텍스트는 구체적인 전략 중 하나에 대한 참조를 가지고 있으며 이는 다른 메서드에서 사용됩니다. 이를 달성하기 위해 컨텍스트 클래스에 일반적으로 클라이언트가 설정하는 setStategy(stategy)라는 공개 메서드가 있습니다. 이 참조는 전략 인터페이스 유형입니다. 컨텍스트는 사용하는 전략을 알지 못합니다. 중요한 점은 전략이 전략 인터페이스를 구현해야 한다는 것입니다.

<div class="content-ad"></div>

2. 전략 인터페이스:

모든 구체적인 전략에 공통적입니다. 구체적인 전략에 의해 구현되는 블루프린트를 정의합니다.

3. 구체적인 전략:

구체적인 전략은 클라이언트가 사용하고 컨텍스트에서 사용하는 알고리즘의 구현입니다. 전략 인터페이스를 구현합니다.

<div class="content-ad"></div>

4. 클라이언트: 특정 전략 객체를 초기화하고 setStategy(stategy)를 통해 컨텍스트에 전달합니다.

# 예시

# 예시 1: 배송 앱

전자 상거래 애플리케이션을 가정해보세요. 클라이언트의 선호에 따라 배송 정보를 제공하고 싶습니다.

<div class="content-ad"></div>

이 게시물의 맥락에서는 선택할 수 있는 교환 가능한 전략을 제공하고 싶습니다.

![image](https://miro.medium.com/v2/resize:fit:1376/1*OopJo1oxEQsj93XLZhZGDw.gif)

## 문제

먼저 무식/나이브한 접근 방식을 살펴보겠습니다: ⬇️

<div class="content-ad"></div>

소스 코드 v1

```js
export class ShippingV1Component {
  public readonly shippingOptions = ['EXPRESS', 'ECONOMY'];
  public selectedOption!: string;
  public type?: string;
  public cost?: string;
  public estimatedTime?: string;

  constructor(
    private readonly expressShippping: ExpressShippingService,
    private readonly economyShipping: EconomyShippingService
  ) {}

  public onStrategyChange(option: string): void {
    this.selectedOption = option;
    this.getData(option);
  }

  private getData(option: string): void {
    if (option === 'EXPRESS') {
      this.type = this.expressShippping.getType();
      this.cost = this.expressShippping.getCost();
      this.estimatedTime = this.expressShippping.getEstimatedTime();
    } else if (option === 'ECONOMY') {
      this.type = this.economyShipping.getType();
      this.cost = this.economyShipping.getCost();
      this.estimatedTime = this.economyShipping.getEstimatedTime();
    }
  }
}
```

getData 메서드에서 이미 불필요한 조건을 볼 수 있습니다. 이는 더 복잡해질 수 있습니다. 만약 새로운 배송 서비스인 해상 운송 등을 도입한다면 어떻게 될까요? 더 많은 복잡성이 생길 것입니다. 😕

## 해결책: 🛠

<div class="content-ad"></div>

아래 다이어그램은 최종 구현 결과물이어야 하는 모습을 보여줍니다:

![다이어그램](/assets/img/2024-07-10-AngularDesignPatternsStrategyPattern_1.png)

단계 1: 구체적인 전략을 위한 전략 인터페이스 정의

배송 전략 인터페이스는 모든 전략의 변형에 공통적입니다.

<div class="content-ad"></div>

```js
export interface IShippingStrategy {
    getType: () => string;
    getCost: () => string;
    getEstimatedTime: () => string;
}
```

다음 단계: IShippingStrategy를 구현하는 구체적인 전략을 만듭니다.

```js
@Injectable({
  providedIn: 'root',
})
export class EconomyShippingService implements IShippingStrategy {
  public getType(): string {
    return 'ECONOMY';
  }

  public getCost(): string {
    return '15$';
  }

  public getEstimatedTime(): string {
    return '5-12 days';
  }
}
```

그리고

<div class="content-ad"></div>

```js
@Injectable({
  providedIn: 'root'
})
export class ExpressShippingService implements IShippingStrategy {
  public getType(): string {
    return 'EXPRESS';
  }

  public getCost(): string {
    return '100$';
  };

  public getEstimatedTime(): string {
    return '1-2 days';
  };
}
```

Step 3: 구체적인 전략에 대한 참조를 가진 컨텍스트 서비스를 만드세요.

```js
@Injectable({
  providedIn: 'root',
})
export class ShippingContextService implements IShippingStrategy {
  private strategy!: IShippingStrategy;

  public hasChosenStrategy(): boolean {
    return !!this.strategy;
  }

  public setStrategy(strategy: IShippingStrategy): void {
    this.strategy = strategy;
  }

  public getType(): string {
    return this.strategy.getType();
  }

  public getCost(): string {
    return this.strategy.getCost();
  }

  public getEstimatedTime(): string {
    return this.strategy.getEstimatedTime();
  }
}
```

클라이언트에서 setStrategy 메서드를 호출합니다.

<div class="content-ad"></div>

Step 4: 클라이언트가 선호하는 전략을 선택할 수 있도록 합니다.

우리의 새로운 ShippingV2Component:

소스 코드 v2

```js
export class ShippingV2Component {
  public readonly shippingOptions = ['EXPRESS', 'ECONOMY'];
  public selectedOption!: string;
  public type?: string;
  public cost?: string;
  public estimatedTime?: string;

  constructor(
    private readonly injector: Injector,
    private readonly shippingContext: ShippingContextService
  ) {}

  public onStrategyChange(option: string): void {
    this.selectedOption = option;

    switch (option) {
      case 'EXPRESS': {
        const strategy = this.injector.get(ExpressShippingService);
        this.shippingContext.setStrategy(strategy);
        break;
      }

      case 'ECONOMY': {
        const strategy = this.injector.get(EconomyShippingService);
        this.shippingContext.setStrategy(strategy);
        break;
      }
    }
    this.getData();
  }

  private getData(): void {
    if (!this.shippingContext.hasChosenStrategy) {
      return;
    }
    this.type = this.shippingContext.getType();
    this.cost = this.shippingContext.getCost();
    this.estimatedTime = this.shippingContext.getEstimatedTime();
  }
}
```

<div class="content-ad"></div>

알아차리셨겠지만, 저희는 조건문을 한 번만 사용합니다. 그것은 고객이 배송 옵션을 선택할 때 onStrategyChange에서 발생합니다.

그게 다에요 🙂

## 예제 2: 팩토리 패턴을 통한 노트 앱 개선

이 예제에 대해서는 이전 팩토리 패턴 게시물을 읽어주세요.

<div class="content-ad"></div>

제가 팩토리 패턴 게시물에 이 경고를 작성했어요:

그런 다음 전략 패턴을 통해 이 문제에 대한 업데이트된 해결책을 제공했습니다.

이게 다에요. 즐겁게 즐겼길 바라며 유용하게 찾아보셨길 바랍니다. 읽어 주셔서 감사합니다.🍍

참고문헌:

<div class="content-ad"></div>

https://refactoring.guru/design-patterns/strategy

AI 도구