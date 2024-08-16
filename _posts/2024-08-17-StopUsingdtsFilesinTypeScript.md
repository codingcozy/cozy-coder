---
title: "TypeScript에서 더 이상 dts 파일을 사용하지 말아야 하는 이유"
description: ""
coverImage: "/assets/img/2024-08-17-StopUsingdtsFilesinTypeScript_0.png"
date: 2024-08-17 00:40
ogImage: 
  url: /assets/img/2024-08-17-StopUsingdtsFilesinTypeScript_0.png
tag: Tech
originalTitle: "Stop Using dts Files in TypeScript"
link: "https://medium.com/gitconnected/stop-using-d-ts-files-in-typescript-da01e4a71b26"
isUpdated: false
---


![image](/assets/img/2024-08-17-StopUsingdtsFilesinTypeScript_0.png)

맷 포콕이 TypeScript 프로젝트에서 영원히 .d.ts 파일 사용을 중단하라고 모두에게 말하고 있습니다. 그의 입장을 강화하는 트윗이 여기 있습니다:

기억하세요, 이건 TypeScript 마법사 맷 포콕의 의견이기 때문에 이 글은 진지하게 받아들여져야 합니다.

하지만, 그가 맞을까요? 여러분이 모든 .d.ts 파일을 열어서 .ts로 교체해야 할까요?

<div class="content-ad"></div>

이 글에서는 .d.ts 파일의 목적을 설명하여 맷이 옳은 이유와 .d.ts 코드를 다시 작성해서는 안 되는 이유를 증명하겠습니다. 그러니 말이 많았죠. 
# .d.ts 파일의 목적

먼저 .d.ts 파일이 쓸모없는 것이 아님을 명확하게 하겠습니다.

<div class="content-ad"></div>

.d.ts 파일은 즉 엄격한 청사진으로, 소스 코드에서 사용할 수 있는 타입을 나타냅니다.

앞서 .d.ts 파일은 선언만 포함할 수 있다고 언급했으니, 함수를 사용하여 두 숫자를 더하는 선언과 구현의 차이를 살펴보겠습니다:

```js
// 선언 (.d.ts)
export function add(num1: number, num2): number

// 구현 (.ts)
export function add(num1: number, num2): number {
  return num1 + num2
}
```

add 함수의 구현에서는 실제 덧셈이 수행되고 결과가 반환되지만, 선언에서는 그렇지 않음을 알 수 있습니다.

<div class="content-ad"></div>

이제 .d.ts 파일을 실제로 어떻게 사용해야 할까요?

예를 들어 add 함수의 경우를 살펴보고, 선언을 저장하는 add.d.ts 파일과 구현을 저장하는 add.js 파일이 있다고 상상해 봅시다.

이제 add 함수를 실제로 사용할 index.js 파일을 만들어 보겠습니다.

```js
import { add } from "./x"

const result = add(1, 4)

console.log(result) // 출력: 5
```

<div class="content-ad"></div>

이 add 함수 내부에 타입 선언이 주석으로 추가되었기 때문에 이 JS 파일 내에서 타입 안전성이 보장됩니다.

# .ts 파일이 더 좋은 이유

그래서 .d.ts 파일이 어떻게 작동하고 유용한지 살펴보았는데, 그럼에도 불구하고 Matt가 옳은 이유는 무엇일까요?

그 이유는 단일 파일 내에서 주석으로 타입을 선언하고 구현할 수 있는 .ts 파일을 직접 생성할 수 있기 때문입니다.

<div class="content-ad"></div>

즉, 선언과 구현이 모두 포함된 단일 add.ts 파일을 가지는 것은 add.d.ts 및 add.js 파일을 별도로 정의하는 것과 동일합니다.

이것은 선언 파일을 해당 구현 파일과 조직하는 것에 대해 걱정할 필요가 없다는 것을 의미하며, 이는 개발자 경험을 향상시키는 데 좋습니다.

그러나 .d.ts가 "더 나은" 것으로 여겨지는 인기있는 주장은 라이브러리 코드 배포에 필요하기 때문이라는 것입니다.

컴파일된 JavaScript 소스 코드와 함께 .d.ts 파일을 사용하는 것이 .ts 파일을 저장하는 것보다 효율적이며, 사용자가 라이브러리를 사용할 때 유형 안정성을 갖는 데 실제로 필요한 것은 유형 선언만 있기 때문입니다.

<div class="content-ad"></div>

모든 내용이 사실이지만, .ts 파일에서 몇 가지 설정을 변경하여 package.json 및 tsconfig.json 파일을 조정하면 .d.ts 파일을 자동으로 생성할 수 있습니다.

- tsconfig.json: .d.ts 파일 생성을 지원하도록 "declaration": true를 추가해야 합니다.

```json
{
  "compilerOptions": {
    "declaration": true,
    "target": "ES6",
    "module": "commonjs",
    "outDir": "./dist",
    "strict": true
  },
  "include": ["src/**/*"]
}
```

- package.json: 컴파일된 소스 코드 옆에 생성된 .d.ts 파일에 대한 "types" 속성을 설정해야 합니다.

<div class="content-ad"></div>

```json
{
  "name": "d.ts 사용 중지",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc"
  }
}
```

자동으로 .d.ts 파일을 생성하면 직접 작성하지 않고도 모든 이점을 누릴 수 있습니다.

# 결정타를 물리는 순간

.d.ts 파일에서 할 수 있는 모든 것은 .ts 파일 내에서도 할 수 있습니다.


<div class="content-ad"></div>

이것은 .ts 파일 내에서 '' declare global '' 구문을 사용하여 달성할 수 있습니다.

중괄호 내부의 모든 것은 전역 앰비언트 선언으로 처리되며, 이것이 .d.ts 파일이 작동하는 핵심입니다.

그래서 결론은, .ts 파일은 앰비언트 선언을 저장할 수 있는 능력이 있으므로 .d.ts 파일이 없어도 여전히 전역적으로 접근 가능한 유형을 가질 수 있습니다.

더 나쁜 일은 대부분의 프로젝트가 tsconfig 옵션인 skipLibCheck를 true로 설정한다는 것입니다. 이렇게 하면 외부 .d.ts 파일뿐만 아니라 자체 파일도 타입 안전성이 제거됩니다.

<div class="content-ad"></div>

이것은 .d.ts 파일에 정의된 유형이 잘못될 수 있고 이에 대해 심지어 인식하지 못할 수 있다는 것을 의미합니다!

그러므로 .d.ts 파일을 작성하는 데 정말 별 도움이 안 되기 때문에, 99%의 경우 .ts 파일을 사용하는 것이 더 나은 선택입니다. 이는 라이브러리 코드의 유형 오류 가능성을 줄이고 개발자 경험을 향상시키기 위함입니다.

그러므로 결론은, 맞아요, 매트가 맞습니다. .d.ts 파일은 절대 직접 작성해서는 안 되며 자동 생성해야 합니다.

그리고 댓글 섹션에서 당신의 반박을 듣고 싶습니다.