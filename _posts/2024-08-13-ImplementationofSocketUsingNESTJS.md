---
title: "NestJS에서 소켓 기능 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-08-13-ImplementationofSocketUsingNESTJS_0.png"
date: 2024-08-13 12:14
ogImage:
  url: /assets/img/2024-08-13-ImplementationofSocketUsingNESTJS_0.png
tag: Tech
originalTitle: "Implementation of Socket Using NEST JS"
link: "https://medium.com/@stephin0398/implementation-of-socket-using-nest-js-3f16e232f8a6"
isUpdated: true
updatedAt: 1723863094661
---

# 소켓은 무엇이며 어디에 사용되나요?

소켓은 네트워크에서 실행 중인 두 프로그램 간의 양방향 통신 링크의 한 지점입니다. 소켓 메커니즘은 통신이 발생하는 명명된 연락점을 설정하여 프로세스 간 통신(IPC)을 제공합니다. 'Pipe'이 파이프를 만들 때 사용되는 것과 마찬가지로 소켓은 '소켓' 시스템 호출을 사용하여 생성됩니다. 소켓은 네트워크 상에서 양방향 FIFO 통신 기능을 제공합니다.

예시: 세 번째로 상금 결제 서비스를 시스템에 통합한다고 가정해봅시다. 결제 프로세스에는 시간이 걸리므로 서비스는 완료되면 웹훅을 통해 우리에게 결제 상태를 알립니다. 이 웹훅을 받은 후에는 즉시 결제 상태를 클라이언트에게 업데이트해야 합니다. 이를 위해 WebSocket 통신을 사용하여 실시간 업데이트를 클라이언트에게 전송합니다.

이제 코드로 들어가 보겠습니다,

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

기존 시스템에서 네스트 명령을 사용하여 모듈을 생성하는 중이에요.

```js
nest g mo <모듈-이름>
//예시: nest g mo gateway
```

모듈 내에서 서비스를 생성해보세요.

```js
nest generate service <서비스-이름>
//예시: nest generate service gateway
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

그래요. 폴더는 이렇게 생성됩니다:

![Screenshot](/assets/img/2024-08-13-ImplementationofSocketUsingNESTJS_0.png)

gateway.service.ts 파일에 코드를 추가해주세요.

```javascript
import { Injectable } from '@nestjs/common';
import {
  WebSocketGateway,
  SubscribeMessage,
  MessageBody,
  WebSocketServer,
} from '@nestjs/websockets';
import { Server } from 'socket.io';

@Injectable()
// Nest에서 웹 소켓을 위해 사용하는 데코레이터입니다
@WebSocketGateway({
  namespace: 'xyz',
  cors: {
    origin: '*',
  },
})
// 소켓 모듈을 초기화하는 데 사용되는 함수입니다
onModuleInit() {
  setTimeout(() => {
    if (this.server) {
      this.server.on('connection', (socket) => {
        console.log(socket.id);
        console.log('Connected');
      });
    } else {
      console.error('WebSocketServer is not initialized.');
    }
  }, 5000);
}
// 메시지를 구독하고 데이터를 DB에 추가합니다
// 여기서는 소켓이 연결되어 있는지 확인하기 위해 이 함수를 사용합니다
@SubscribeMessage('Ping')
onNewMessage(@MessageBody() body: any) {
  console.log(body);
  console.log('Welcome to xyz');
  this.server.emit('Pong', {
    msg: 'Ping',
    Body: {
      Message: 'Both end connected 시작합시다',
    },
  });
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

위 코드를 살펴보도록 하죠,

@Injectable(): 이 데코레이터는 해당 클래스를 다른 클래스에 주입할 수 있는 서비스로 표시합니다. 이는 NestJS에서 의존성 주입의 중요한 부분입니다.

@WebSocketGateway(): 이 데코레이터는 WebSocket 게이트웨이를 설정하는 데 사용됩니다. 게이트웨이는 WebSocket 연결과 통신을 처리합니다.

Options:

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

- namespace: `xyz`: 이 WebSocket 게이트웨이가 xyz 네임스페이스 하에 있음을 지정합니다. 클라이언트들은 이 네임스페이스에 연결할 것입니다.
- cors: ' origin: `*` ': 이는 모든 출처(`*`)에서의 교차 출처 요청을 허용합니다. 이는 서로 다른 출처의 클라이언트들이 WebSocket 서버에 연결해야 하는 경우 중요합니다.

# onModuleInit 함수:

- 목적: onModuleInit() 메서드는 모듈이 초기화될 때 호출됩니다. WebSocket 서버를 설정하여 새로운 연결을 듣기 시작합니다.
- 타임아웃:
- setTimeout은 실행을 5초 지연시키는 데 사용됩니다. 이는 연결을 처리하기 전에 시스템의 모든 부분(WebSocket 서버 포함)이 완전히 초기화되도록 하는 데 사용될 수 있습니다.
- 연결 처리:
- server.on(`connection`, (socket) =` '...') 부분은 WebSocket 서버에 새로운 클라이언트가 연결될 때 트리거되는 이벤트 리스너를 추가합니다. 연결된 클라이언트의 소켓 ID 및 "연결됨" 메시지를 로그에 남깁니다.

# 메시지 구독:

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

- @SubscribeMessage('Ping'): 이 데코레이터는 'Ping' 이벤트 이름을 가진 메시지를 수신 대기합니다. 서버가 이 이벤트 이름을 포함한 메시지를 수신하면 onNewMessage() 메소드를 트리거합니다.
- onNewMessage(body: any): 'Ping' 메시지를 수신하면 이 함수가 호출됩니다. 메시지 본문을 콘솔에 로그로 남기고 환영 메시지도 로그로 남깁니다.
- 응답 발신:
- 들어온 메시지를 로깅한 후, 서버는 this.server.emit()를 사용하여 모든 연결된 클라이언트에게 'Pong' 메시지를 보냅니다. 메시지에는 다음이 포함됩니다:
- msg: 'Ping': 초기 이벤트 이름을 동일하게 반복합니다.
- Body: ' Message: `Lets start Both end connected` ': 클라이언트와 서버가 성공적으로 연결되었음을 나타내는 메시지를 보냅니다.

이 구조는 서버와 클라이언트 간의 실시간 상호작용이 필요한 채팅 애플리케이션, 알림, 또는 라이브 업데이트와 같은 애플리케이션에서 기본 WebSocket 통신 채널을 설정하는 데 유용합니다.

또한 emit 함수를 공통으로 작성하여 어디서든 사용할 수 있습니다:

```js
  //이 함수는 리포지토리의 어디서든 이벤트를 발신하는 데 도움이 됩니다.
  async emitSocketEventNotification(request: {message: string, eventName: string, data: object}) {
    try {
      await this.server.emit(request.eventName, {
        message: request.message,
        body: request.data,
      });
      return '전송 성공';
    } catch (error) {
      console.log(error);
    }
  }
}
```
