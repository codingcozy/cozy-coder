---
title: "TypeScript로 매개변수가 있는 데코레이터 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-CreatingaDecoratorwithParametersinTypeScript_0.png"
date: 2024-08-03 18:29
ogImage: 
  url: /assets/img/2024-08-03-CreatingaDecoratorwithParametersinTypeScript_0.png
tag: Tech
originalTitle: "Creating a Decorator with Parameters in TypeScript"
link: "https://medium.com/@dlyusko/creating-a-decorator-with-parameters-in-typescript-db78873242dd"
isUpdated: true
---





<img src="/assets/img/2024-08-03-CreatingaDecoratorwithParametersinTypeScript_0.png" />

TypeScript의 데코레이터는 클래스와 메서드에 추가 기능을 제공하는 강력한 방법을 제공합니다.

이 글에서는 메서드 실행을 기록하고 선택적으로 반환 값을 보고서에 첨부할 수 있는 매개변수화된 데코레이터를 생성하는 방법을 살펴볼 것입니다.

# 데코레이터를 사용하는 이유?

<div class="content-ad"></div>

데코레이터는 다양한 시나리오에서 유용할 수 있어요:

- 메소드 호출 로깅.
- 성능 감사.
- 원본 코드를 수정하지 않고 메소드 동작을 변경하는 것.

저희 예시에서는 로깅에 사용되는 단계 데코레이터를 만들 것이에요. 필요하다면 반환 값을 첨부할 수도 있어요.

# 데코레이터 정의

<div class="content-ad"></div>

아래는 우리 데코레이터의 모습입니다:

```js
// Class method decorator context type
export type ClassMethodDecoratorContextType = {
  kind: 'method';
  name: string | symbol;
  access: { get(): unknown };
  static: boolean;
  private: boolean;
  addInitializer(initializer: () => void): void;
};
```

```js
// 스텝 용어를 바꾸는 데 도움이 되는 함수
export const replaceArgs = (strValue: string, ...params: Array<string | number>): string => {
  let str = strValue;
  params.forEach((param, index) => (str = str.replace(`{${index}`, String(param))));
  return str;
};
```

```js
import { ClassMethodDecoratorContextType } from '@interfaces/decorators/decorator';
import { report } from '@services/reporter';
import { replaceArgs } from '@helpers/string.helper';

// 데코레이터
export function step(wording: string, logReturnValue: boolean = false): any {
  return (target: Function, context: ClassMethodDecoratorContextType) => {
    if (context.kind !== 'method') {
      throw new Error('The "step" decorator can only be applied to method');
    }
    return function (...args: Array<any>) {
      return report.step(replaceArgs(wording, ...args), async () => {
        const res = await target.apply(this, args);
        if (logReturnValue) {
          await report.attach(res, 'Attachment');
        }
        return res;
      });
    };
  };
}
```

<div class="content-ad"></div>

# 데코레이터 매개변수

- wording: string — 보고서에서 단계를 설명하는 데 사용되는 문자열입니다.
- logReturnValue: boolean = false — 메소드의 반환 값 로깅 여부를 나타내는 매개변수입니다.

# 데코레이터 컨텍스트

컨텍스트 객체는 데코레이터가 적용된 메소드에 대한 정보를 제공합니다. 우리의 경우에는 context.kind가 `method`와 동일한지 확인하여 데코레이터가 메소드에만 적용되도록 합니다.

<div class="content-ad"></div>

# Core Logic

함수 내부에서는 다음 단계를 수행합니다:

- 변경된 인수를 전달하여 report.step()를 호출합니다.
- target.apply(this, args)를 사용하여 원래 메서드를 호출합니다.
- logReturnValue가 true인 경우, 반환 값을 report.attachJson(res, `Attachment`)를 사용하여 보고서에 첨부합니다.

# 데코레이터 사용 예시

<div class="content-ad"></div>

이제 우리의 데코레이터를 클래스에서 어떻게 사용하는지 살펴보겠습니다:

```js
class UserService {
  @step('Create user with name = "{0}"', true)
  async createUser(name: string) {
    // 사용자 생성을 나타내는 비동기 호출 모의
    const user = await service.createUser(name)
    return user;
  }
}

// 메소드를 호출하는 예제
const userService = new UserService();
await userService.createUser("Jack")
```

# 예제 설명

- createUser 메소드에 @step 데코레이터를 적용하고, 문자열을 전달하여 로깅을 설정하고 logReturnValue를 true로 설정합니다.
- createUser("Jack")을 호출할 때, 데코레이터는 먼저 실행을 로깅한 후 메소드를 수행하고 리턴 값을 보고서에 첨부합니다.
- 마지막으로 다음 단계 문구가 생성됩니다: Create user with name = "Jack"

<div class="content-ad"></div>

# 결론

TypeScript에서 매개변수화된 데코레이터를 만드는 것은 코드의 구조와 기능을 향상시키는 강력한 방법입니다.

단계 데코레이터를 사용하여 메서드에 로깅 및 보고 기능을 쉽게 추가할 수 있어 코드를 더 읽기 쉽고 유지 관리하기 쉽게 만들 수 있습니다.

이제 여러 작업을 위해 자체 데코레이터를 만들 준비가 되었습니다!