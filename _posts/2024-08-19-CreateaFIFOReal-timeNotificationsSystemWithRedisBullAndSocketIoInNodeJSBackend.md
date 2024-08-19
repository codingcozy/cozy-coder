---
title: "Nodejs 백엔드에서 Redis, Bull 및 Socket IO를 사용한 실시간 FIFO 알림 시스템 구현하기"
description: ""
coverImage: "/assets/img/2024-08-19-CreateaFIFOReal-timeNotificationsSystemWithRedisBullAndSocketIoInNodeJSBackend_0.png"
date: 2024-08-19 02:30
ogImage: 
  url: /assets/img/2024-08-19-CreateaFIFOReal-timeNotificationsSystemWithRedisBullAndSocketIoInNodeJSBackend_0.png
tag: Tech
originalTitle: "Create a FIFO , Real-time Notifications System With Redis, Bull And Socket Io In Node JS Backend"
link: "https://medium.com/@laskar.ksatria12/how-to-create-a-fifo-real-time-notifications-system-with-redis-bull-and-socket-io-in-node-js-6b5c96bffb23"
isUpdated: true
updatedAt: 1724033201313
---


![이미지](/assets/img/2024-08-19-CreateaFIFOReal-timeNotificationsSystemWithRedisBullAndSocketIoInNodeJSBackend_0.png)

백엔드 시스템 개발에서 때로는 간단한 비동기 접근만으로 충분하지 않을 때가 있습니다. 티켓 판매나 제한된 재고 캠페인과 같이 구매 요청을 처리하기 전에 상품 재고를 확인해야 하는 시스템에서는 FIFO(선입선출) 시스템이 필수적입니다. 이러한 경우 사용자에게 보류 중인 응답을 보내주고, 상태 변경에 대한 실시간 알림을 제공해야 합니다. 상태가 허용되었는지 아니면 재고 소진과 같은 이유로 거부되었는지 알려줘야 합니다.

알림을 위한 다양한 방법이 있지만, 이 경우에는 Socket IO를 사용하여 프론트엔드 또는 사용자에게 실시간 업데이트를 보냅니다. 대기열을 관리하기 위해 Redis와 Bull을 사용하고 있습니다.

그럼, 시작해봅시다!

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

서버 설정

먼저 터미널을 열고 다음 명령을 사용하여 nodejs-queue라는 폴더(또는 원하는 다른 이름)를 만드세요:

```js
mkdir nodejs-queue
```

다음 명령을 사용하여 package.json 파일을 만드세요:

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
npm init -y
```

그런 다음 몇 가지 패키지를 설치하세요.

```js
npm install --save express redis cors socket.io bull
```

개발 환경에서는 Nodemon을 사용하여 개발 프로세스를 간단하게 만들어요. 백엔드에서는 업데이트를 하면 자동으로 다시 시작해요.

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


npm install -D nodemon


루트 폴더에 app.js 파일을 생성한 후 package.json 스크립트에 다음을 추가하세요.
“dev”: “nodemon app.js”, 그리고 package.json에도 type: module을 추가해주세요.

다음은 변경된 package.json 파일입니다.

```json
{
  "name": "nodejs-queue",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "dev": "nodemon app.js"
  },
  "type": "module",
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "nodemon": "^3.1.4"
  },
  "dependencies": {
    "bull": "^4.16.0",
    "cors": "^2.8.5",
    "express": "^4.19.2",
    "redis": "^4.7.0",
    "socket.io": "^4.7.5"
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

app.js에 서버를 설정하세요.

```js
import express from "express";
import cors from "cors";
import http from "http";

const app = express();

const server = http.createServer(app);
const PORT = 3000;

const MyServer = async () => {
  try {
    app.use(cors());
    app.use(express.urlencoded({ extended: false }));
    app.use(express.json());
    server.listen(PORT, () => console.log(`서버가 포트 ${PORT}에서 실행 중입니다.`));
  } catch (error) {
    throw Error(error);
  }
};

MyServer();
```

그런 다음 터미널에서 "npm run dev"를 실행하세요. 올바르게 실행하면 이렇게 보일 것입니다.

![이미지](/assets/img/2024-08-19-CreateaFIFOReal-timeNotificationsSystemWithRedisBullAndSocketIoInNodeJSBackend_1.png)

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

FIFO 시스템 생성하기

이제 FIFO 기반의 구매 시스템을 만들어보겠습니다. 판매되는 상품은 재고가 5개뿐입니다. 사용자가 구매를 하면 먼저 요청이 처리 중이라는 메시지를 받게 됩니다. 재고가 아직 있는 경우, 요청이 수락된 것을 알리는 알림을 보내줍니다. 그러나 재고가 소진된 경우, 품절 상태인 것을 알리는 알림을 전송합니다.

우선은 Bull & Redis를 설정해야 합니다. queue.js라는 파일을 생성하고 다음 코드를 추가해주세요.

```js
import Queue from "bull";

export const MyQueue = new Queue("my-queue", {
  redis: {
    host: "127.0.0.1",
    port: 6379,
  },
});

function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

let stock = 5;

MyQueue.process(async (job) => {
  await delay(5000);
  console.log(job.data);
});
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

제품 데이터베이스의 수량을 시뮬레이션하는 재고 변수를 만들었습니다. 또한 비동기 프로세스를 시뮬레이션하는 지연 함수도 만들었습니다.

이제 코드를 테스트할 준비가 되었습니다. app.js에 MyQueue를 import하세요.

```js
import { MyQueue } from "./queue.js";
```

MyServer 함수에 다음 코드를 추가하세요.

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
app.get("/test", async (_, res) => {
  MyQueue.add({ message: "hello world" });
  res.status(200).json({ message: "Your order is in queue." });
});
```

Postman이나 브라우저를 열고 이 URL을 복사하세요.

```js
http://localhost:4000/buy
```

터미널에서 이 것을 보면 제대로 작성한 것입니다.

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

아래는 마크다운 형식으로 표로 변환한 내용입니다.


![이미지](/assets/img/2024-08-19-CreateaFIFOReal-timeNotificationsSystemWithRedisBullAndSocketIoInNodeJSBackend_2.png)

이제 사용자에게 요청 상태에 대한 정보를 제공하기 위해 Socket.io를 설정해야 합니다.

app.js에 Socket Io를 가져와 다음 코드를 추가하세요.

```js
import { Server } from "socket.io";

export const Io = new Server(server, {
  cors: {
    origin: "*",
  },
});
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

다음으로, queue.js 파일에 Io 변수를 가져오세요.

```js
import { Io } from "./app.js";
```

이제 app.js에 POST 라우트를 만들어서 이메일을 body로 받는 로직을 추가합니다.

```js
app.post("/buy", async (req, res) => {
  let email = req.body.email;
  if (!email) throw Error("Required Email");
  MyQueue.add({ data: { email } });
  res.status(200).json({ message: "주문이 처리되었습니다" });
});
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

나중에 특정 사용자에게 메시지를 보내는 '키'로이 이메일도 사용할 것입니다.

게다가, 재고를 검증하는 미들웨어를 생성할 수도 있습니다. 따라서 재고가 소진되면 대기열 처리를 계속하지 않을 것입니다.

아래 코드를 app.js에 추가해주세요.

```js
const ValidateStock = (_, res, next) => {
  if (stock > 1) return next();
  else res.status(400).json({ message: "죄송합니다, 재고가 소진되었습니다" });
};
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

"Buy" 엔드포인트에 그 미들웨어를 추가해주세요.

```js
app.post("/buy", ValidateStock, async (req, res) => {
  let email = req.body.email;
  if (!email) throw Error("Required Email");
  MyQueue.add({ data: { email } });
  res.status(200).json({ message: "주문이 처리되었습니다" });
});
```

이제 이를 프론트엔드에서 테스트하여 소켓 IO가 올바르게 실행되었는지 확인합니다. 이 경우, 저는 Remix Js를 사용하고 있습니다. 만약 원하시면 Next Js나 React Js를 사용해도 됩니다.

프론트엔드에 Socket.IO 클라이언트를 설치해주세요.

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
npm install socket.io-client@4.7.2
```

그런 다음 다음 코드를 추가하십시오.

```js
import {useEffect, useState} from 'react';
import io from 'socket.io-client';

let Io;

const backendUrl = 'http://localhost:4000';

export default function SamplePage() {
  const [email, setEmail] = useState('laskar@mail.com');
  const [showText, setShowText] = useState('');

  const handleClick = async () => {
    try {
      if (!email) return alert('이메일을 입력해주세요');
      const response = await fetch(`${backendUrl}/buy`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({email}),
      });
      const data: any = await response.json();
      setShowText(data?.message);
    } catch (error) {
      console.log(error);
    }
  };
  useEffect(() => {
    Io = io(backendUrl);
    Io.on(`${email}-send`, (data) => {
      setShowText(data);
    });
    return () => {
      Io = io(backendUrl);
      Io.off(`${email}-send`);
    };
  }, [email]);
  return (
    <div className="flex items-center justify-center w-screen h-screen">
      <div>
        <div className="mb-5">
          <input
            className="p-2"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
        </div>
        <div className="px-2 py-2 mb-3">
          <button onClick={() => handleClick()}>구매</button>
        </div>
        <p className="font-semibold">{showText}</p>
      </div>
    </div>
  );
}
```

그런 다음 이메일을 입력하고 '구매'를 클릭하십시오. 그러면 주문이 처리되었다는 통지 텍스트가 표시됩니다. 정확하게 수행한 경우, 5초 후에 브라우저를 새로 고치지 않아도 "주문이 승인되었습니다"로 텍스트가 변경됩니다.


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


![image](/assets/img/2024-08-19-CreateaFIFOReal-timeNotificationsSystemWithRedisBullAndSocketIoInNodeJSBackend_3.png)

그리고 Redis와 Bull을 사용하여 FIFO 시스템을 구현하는 간단한 예제였습니다. 이 기사가 도움이 되었으면 좋겠어요.

전체 Node.js 코드를 확인하려면 제 깃허브를 방문해주세요.

https://github.com/laskar-ksatria/nodejs-fifo
