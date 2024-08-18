---
title: "Nodejs V225에서 무슨 새로운 기능들이 있는지 알아보세요"
description: ""
coverImage: "/assets/img/2024-08-19-WhatisnewinNodejsV225_0.png"
date: 2024-08-19 02:29
ogImage: 
  url: /assets/img/2024-08-19-WhatisnewinNodejsV225_0.png
tag: Tech
originalTitle: "What is new in Nodejs V225"
link: "https://medium.com/@nairihar/what-is-new-in-node-js-v22-5-8899620ddebf"
isUpdated: false
---


## 새 도구에 관한 간단한 소개

![이미지](/assets/img/2024-08-19-WhatisnewinNodejsV225_0.png)

이 글에서는 새로운 Node.js 버전에 도입된 주목할만한 변경 사항을 살펴보겠습니다:

- add node:sqlite module (Colin Ihrig) #53752
- add matchesGlob method (Aviv Keller) #52881
- add postMessageToThread (Paolo Insogna) #53682

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

시작해 봅시다!

# node:sqlite

```js
// index.js

const { DatabaseSync } = require('node:sqlite');
const database = new DatabaseSync(':memory:');

// 문자열을 통해 SQL 문을 실행합니다.
database.exec(`
  CREATE TABLE data(
    id INTEGER PRIMARY KEY,
    name TEXT
  );
`);

// 데이터베이스에 데이터를 삽입하는 준비된 문을 작성합니다.
const insert = database.prepare('INSERT INTO data (id, name) VALUES (?, ?)');

// 바인딩된 값으로 준비된 문을 실행합니다.
insert.run(1, 'Bob');
insert.run(2, 'John');

// 데이터베이스에서 데이터를 읽는 준비된 문을 작성합니다.
const query = database.prepare('SELECT * FROM data ORDER BY id');

// 준비된 문을 실행하고 결과 세트를 로그에 기록합니다.
console.log(query.all());

// [ { id: 1, name: 'Bob' }, { id: 2, name: 'John' } ]
```

위 예제는 node:sqlite 모듈의 기본 사용법을 보여줍니다. 인-메모리 데이터베이스를 열고 데이터를 작성한 다음 데이터를 다시 읽어오는 과정을 보여줍니다.

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

--experimental-sqlite 플래그를 사용할 때 이 내장 라이브러리를 사용할 수 있습니다.

```js
node --experimental-sqlite index.js
```

우리 개발자들은 Node.js 기본 모듈과 함께 서버를 완전히 기능적으로 만들기 위해 많은 외부 모듈을 사용합니다. Node.js가 이러한 중요한 도구를 직접 포함하려고 노력하는 것은 훌륭합니다. 예를 들어, 최근에 테스트 러너의 네이티브 지원이 추가되어 jest와 mocha에서 네이티브 라이브러리로 전환했습니다. 이제는 데이터베이스와 ORM의 차례입니다.

현재 이것은 실험적인 모듈이며, 안정화되고 더 많은 메소드를 갖추기까지 시간이 필요할 것입니다.

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

Node.js Core 팀의 노력을 함께 감사하면서, 언젠가는 대부분의 주요 모듈이 기본적으로 제공되어 외부 라이브러리를 거의 사용하지 않게 될 것입니다.

# matchesGlob

이것은 다른 실험적인 메소드로, 경로가 패턴과 일치하는지를 결정합니다.

```js
path.matchesGlob('/foo/bar', '/foo/*'); // true
path.matchesGlob('/foo/bar*', 'foo/bird'); // false
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

# postMessageToThread

마지막으로, 다른 worker에 값을 보내는 방법입니다. 이 worker는 thread ID로 식별됩니다.

이전에는 Message Channels를 사용하여 worker 스레드와 통신할 수 있었지만 이제는 스레드 간 통신이 훨씬 간단해졌습니다.

다음은 스레드 ID를 가져오는 방법입니다:

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
const { threadId } = require('node:worker_threads');
```

그리고 이것이 서로 통신하는 방법입니다:

```js
postMessageToThread(id, { message: 'pong' });
```

메시지를 먼저 받기 위해서는 리스너를 설정해야 합니다:

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
process.on('workerMessage', (value, source) => {
    console.log(`${source} -> ${threadId}:`, value);
});
```

송신자의 스레드 ID가 소스입니다. 따라서 메시지를 되돌리고 싶다면 다음과 같이 할 수 있습니다.

```js
process.on('workerMessage', (value, source) => {
  console.log(`${source} -> ${threadId}:`, value);
  postMessageToThread(source, { message: 'pong' });
});
```

메인 스레드와 통신하려면 메인 스레드 ID가 0이므로 그냥 0을 사용하면 됩니다.

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
postMessageToThread(0, { message: 'ping' });
```

완벽한 예제가 여기 있어요.

저의 요약을 즐겨주시기를 바랍니다. 제가 정보를 신속하게 전달하고 가장 중요한 업데이트를 강조하기 위해 노력했습니다.

그리고 제 JavaScript 뉴스레터를 Telegram에서 팔로우해주세요: @javascript