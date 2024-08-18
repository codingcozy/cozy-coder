---
title: "Typescript에서 꼭 알아야 할 Promise 유틸리티 5가지"
description: ""
coverImage: "/assets/img/2024-07-02-EssentialPromiseUtilitiesforTypescript_0.png"
date: 2024-07-02 23:03
ogImage:
  url: /assets/img/2024-07-02-EssentialPromiseUtilitiesforTypescript_0.png
tag: Tech
originalTitle: "Essential Promise Utilities for Typescript"
link: "https://medium.com/itnext/essential-promise-utilities-for-javascript-9234cd9d3fdb"
isUpdated: true
---

![2024-07-02-EssentialPromiseUtilitiesforTypescript_0](/assets/img/2024-07-02-EssentialPromiseUtilitiesforTypescript_0.png)

ES6 표준이 처음 발표되었을 때부터 시간이 좀 지났어요 - 정확히 9년이에요. 이 업데이트는 매우 유용한 기능을 많이 가져왔죠. 그 중에서도 가장 멋진 추가사항 중 하나가 비동기 작업을 처리하는 새로운 방법인 Promises예요.

모든 것은 악명높은 콜백 지옥에서 시작했어요. 그것은 .then/.catch API에 의해 약간 완화되었죠. 이것은 결국 async/await 문법으로 발전했고, 그 결과 JavaScript가 더 읽기 쉬운 언어가 되었어요. 하지만 저에게는 이게 충분하지 않았어요.

![GIF](https://miro.medium.com/v2/resize:fit:960/1*riNysDtxnyOBqWw5HQ8yjA.gif)

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

큰 개선이 있었지만 이 코드는 여전히 꽤 추해요:

```js
let result;
try {
  result = await someWork();
} catch (err) {
  // 에러 처리
}

// 결과값을 이용해 무언가를 수행합니다.
```

특히 이 코드를 추하게 만드는 두 가지 이유가 있어요:

- 나중에 사용하기 위해 try/catch 블록 밖에서 변수를 정의해야 합니다.
- try/catch 블록은 여전히 불필요한 들여쓰기를 만들어내요.

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

저희가 처음 작성한 코드는 각 promise 호출 주변에 다섯 줄이 더 추가되어 있었고, 이에 대해 무언가 해야 했습니다.

그 즈음에 Golang이 나와서, 그것의 에러 처리 방식에 영감을 받아 JavaScript에서 이를 복제하기 위한 간단한 함수를 만들었습니다. 저는 이 함수를 JavaScript에서 사용해 오랜 시간이 지났습니다. 이제 그 모든 것을 하나의 패키지로 모아서 공개하기로 결정했습니다.

### 한 줄로 구성된 Promise 처리

왜 배열 튜플로 구조 분해하고 러스트 스타일의 객체로 구조 분해하지 않는 거죠? 배열 튜플은 반환 값을 원하는대로 이름을 지정할 수 있으며, 내 생각에는 객체 키를 이름을 바꾸는 것이 미묘하다고 생각하기 때문입니다.

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
import { to } from "@mrspartak/promises";

const [error, result] = await to(somePromise);
```

<img src="https://miro.medium.com/v2/resize:fit:960/1*xTTHonxGh_ItwQcAx-HTAg.gif" />

## Using the `to` Function Has Its Perks!

- Automatically determines the correct return types.
- Avoids throwing errors.
- Empowers you to use custom variable names through array destructuring.

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

백엔드와 프론트엔드에서 모두 작동하며, 보너스로 finally API도 지원합니다.

```js
import { to } from "@mrspartak/promises";

let isLoading = true;
const [error, result] = await to(fetchData(), () => {
  isLoading = false;
});
```

# 프로미스 실행 시간 초과

많은 경우에는 특정 시간 내에 프로미스가 완료되도록 하고 싶을 수 있습니다. 이 기능은 네트워크 요청이나 기타 비동기 작업이 무한히 지연될 수 있는 경우에 특히 유용합니다. 이를 해결하기 위해 저희 유틸리티 라이브러리에는 프로미스가 지정된 시간 내에 해결되도록 보장하는 타임아웃 함수가 포함되어 있습니다.

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

## 작동 방식

타임아웃 함수는 실행하고 싶은 프로미스, 기다릴 최대 시간(밀리초) 및 시간 초과 시 던질 선택적 오류 메시지를 세 가지 인수로 사용합니다. 지정된 시간 내에 프로미스가 해결되지 않으면 함수가 오류를 throw하여 적절히 처리할 수 있습니다.

```js
import { timeout } from "@mrspartak/promises";
import { api } from "./api";

// 레이스 조건으로 사용할 수 있음
const [error, user] = await timeout(api.getUser(), 1000, "시간 초과");
if (error) {
  // 오류는 타임아웃 오류 또는 API에서 발생한 오류 중 하나일 수 있습니다.
}
```

타임아웃 함수는 튜플을 반환하는 함수의 API를 준수합니다.

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

## 타임아웃 기능을 사용하는 이점

- 작업 멈춤 방지: 프라미스가 해결될 때까지 무한정 기다리지 않고 애플리케이션이 멈추는 것을 방지합니다.
- 향상된 사용자 경험: 작업이 예상보다 오래 걸릴 때 사용자에게 적시에 피드백을 제공합니다.

# 실행 지연

이것은 여러분의 코드베이스에 있는 가장 인기 있는 원라이너 중 하나로, 모든 프로젝트에서 복사하여 붙여넣기를 계속하는 기능이라고 생각합니다. 실행을 지연시키는 delay 함수를 소개합니다. 이것은 주어진 시간 동안 코드 실행을 일시 중지하는 간단한 유틸리티입니다. 이는 속도 제한, 재시도 메커니즘 또는 테스트 목적으로 인위적인 지연을 만드는 등 다양한 시나리오에 매우 유용합니다. 아직 네이밍에 대한 표준이 없기 때문에 delay와 완전히 동일한 것을 수행하는 sleep이라는 별칭을 갖고 있습니다.

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

## 작동 방식

delay 함수는 실행을 일시 중지할 밀리초 단위의 시간을 나타내는 하나의 인자를 받습니다. 지정된 지연 시간 후에 해결되는 프로미스를 반환하여, 비동기 함수 안에서 일시 중지를 만들기 위해 await와 함께 사용할 수 있게 합니다.

```js
import { delay, sleep } from "@mrspartak/promises";
import { parsePage } from "./parser";

for (let i = 0; i < 10; i++) {
  // 페이지를 구문 분석하고 다음 페이지를 구문 분석하기 전 1초를 기다립니다
  const 페이지데이터 = await parsePage(i);
  await delay(1000);
}

// delay 대신에 alias인 sleep를 사용할 수도 있습니다
await sleep(1000);
```

## delay 함수 사용의 장점

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

- 코드 재사용성: 여러 프로젝트에서 동일한 지연 로직을 반복해서 구현할 필요가 없습니다.
- 간편함: 복잡한 우회 방법을 사용하지 않고 코드에 지연을 도입하는 명확하고 간결한 방법을 제공합니다.
- 유연성: 재시도 메커니즘, 속도 제한 또는 네트워크 지연을 시뮬레이션하는 등 다양한 시나리오에서 사용할 수 있습니다.

# 실행 연기

많은 상황에서 프로미스를 생성하고 해당 해결이나 거부를 나중에 연기해야 할 수 있습니다. 이때 deferred 함수가 유용합니다. 이 함수는 프로미스의 해결과 거부를 수동으로 제어할 수 있는 방법을 제공하여 즉시 결과를 제공하지 않는 비동기 작업을 처리하는 강력한 도구로 작용합니다.

## 작동 방법

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

deferred 함수는 프라미스 및 해당 resolve와 reject 함수를 포함한 객체를 반환합니다. 이를 통해 미래의 어느 시점에서든 프라미스를 해결하거나 거절할 수 있어서 프라미스의 수명 주기를 완전히 제어할 수 있습니다.

## 참고사항

이것은 JS/TS 커뮤니티에서 매우 논란적이었지만 이미 표준의 일부이며 최신 런타임에서 지원되고 있습니다. 이것을 라이브러리에 추가하면 이전 런타임과의 하위 호환성을 확보할 수 있습니다. 코드 실행 흐름을 변경하면 코드 가독성이 감소하고 버그가 발생할 수도 있습니다. 따라서 이 유틸리티를 사용할 때 주의해야 합니다.

```js
import { deferred } from "@mrspartak/promises"
import { readStream } from "./stream"

//Deferred 프라미스 생성
const { promise, resolve, reject } = deferred<void>()

//스트림을 청크 단위로 읽은 다음 프라미스를 해결
const stream = await readStream()
let data = ''
stream.on('data', (chunk) => {
  data += chunk
})
stream.on('end', () => {
  resolve()
})

//데이터로 프라미스 해결
await promise
console.log(data) //데이터가 준비됨
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

## deferred 함수 사용의 이점

- 수동 제어: 언제든지 약속을 수동으로 해결하거나 거부할 수 있어 비동기 작업에 더욱 뛰어난 제어를 제공합니다.
- 향상된 유연성: 비동기 작업의 결과가 즉시 사용할 수 없는 상황에 유용합니다.

# 실행 다시 시도

비동기 프로그래밍 세계에서 가끔은 문제가 발생합니다. 네트워크 요청이 실패할 수 있고, API가 시간 초과될 수도 있으며, 다른 오류가 발생할 수 있습니다. 즉시 포기하는 대신, 마지막에 오류를 발생시키기 전에 몇 번의 시도를 해보는 편이 유익할 수 있습니다. 이때 retry 함수가 필요합니다. 이 함수를 사용하면 특정 횟수만큼 promise를 반환하는 함수를 다시 시도할 수 있고, 필요한 경우 시도 사이에 지연을 적용할 수 있습니다.

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

## 작동 방식

리트라이 함수는 세 가지 인수를 사용합니다: 다시 시도할 프로미스 반환 함수, 리트라이 시도 횟수 및 선택적인 시도 간 지연 시간입니다. 함수가 실패하면 지정된 지연 시간(제공된 경우)동안 재시도를 대기합니다. 이 과정은 함수가 성공하거나 최대 시도 횟수에 도달할 때까지 계속됩니다.

```js
import { retry } from "@mrspartak/promises";
import { apiCall } from "./api";

// API 호출을 최대 3회, 시도 간 지연 시간 1000밀리초로 재시도합니다
const [error, result] = await retry(() => apiCall(), 3, { delay: 1000 });
if (error) {
  // error는 항상 프로미스 거부로 인한 오류입니다
}
```

보통 리트라이에는 더 많은 옵션이 있으며, 백오프 알고리즘, 후크 또는 다른 기능이 필요한 경우 라이브러리에 쉽게 추가할 수 있습니다. 적절한 API를 만들기 위해 사용 사례에 대한 의견을 기꺼이 수집하겠습니다.

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

# 실행 시간

약속이 해결되거나 거부되기까지 얼마나 걸리는지 이해하는 것은 성능 모니터링 및 최적화에 매우 중요합니다. 'duration' 함수는 약속이 해결 및 거부되는 데 걸리는 시간을 측정하여 비동기 작업의 성능에 대한 소중한 통찰을 제공합니다.

## 작동 방식

'duration' 함수는 약속을 래핑하고 약속이 해결되거나 거부되기까지 소요된 시간을 기록합니다. 성공하는 경우 오류가 발생하거나 발생하지 않은 결과 및 밀리초 단위의 지속 시간을 포함하는 튜플을 반환합니다. 이 정보는 성능 병목 현상을 식별하고 코드를 최적화하는 데 도움이 될 수 있습니다.

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
import { duration } from "@mrspartak/promises";
import { migrate } from "./migrate";

const migratePromise = migrate();

// 마이그레이션 함수의 소요 시간 측정
const [error, result, duration] = await duration(migratePromise);

console.log(`마이그레이션이 ${duration}밀리초가 소요되었습니다.`);
```

보시다시피, 이것은 기본적으로 함수를 대체하는 것이며, 배열로 3개의 인수를 받는 장소이며, 그 중 마지막 것이 실행 시간(밀리초 단위)을 포함하고 있습니다.

# 다음은 무엇인가요?

<img src="https://miro.medium.com/v2/resize:fit:960/1*ewAvXIAsw_c7xbCR-izlzA.gif" />

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

이 중에 나는 사용한 모든 유틸리티들을 나열한 것이 아니지만, 거의 모든 프로젝트에서 사용한 기본적인 부분들입니다. 이들은 완전히 유형화되어 있고, 테스트로 완벽히 검증되었으며, 거의 6년 동안 운영환경에서 사용되었습니다. 물론, 점차적으로 추가할 수 있는 더 많은 유틸리티들이 있습니다. 또한 기여를 환영합니다. 요청을 통해 제공하거나 새로운 유틸리티를 제공해 주시면 감사하겠습니다.

추후 업데이트와 개선 사항을 기대해 주세요. 추가되었으면 하는 제안이나 유틸리티가 있으면 자유롭게 기여해 주세요. 함께 JavaScript와 TypeScript의 비동기 프로그래밍을 더욱 향상시키도록 합시다!

이전에 클릭하지 않았다면, 여기 Github 링크를 확인해 주세요: https://github.com/mrspartak/promises

즐거운 코딩하세요! 🚀
