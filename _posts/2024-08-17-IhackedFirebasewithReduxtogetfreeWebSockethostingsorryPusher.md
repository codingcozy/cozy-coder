---
title: "Firebase와 Redux를 이용해 Web Socket 무료 호스팅 뚫는 방법 Pusher 미안"
description: ""
coverImage: "/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_0.png"
date: 2024-08-17 00:46
ogImage: 
  url: /assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_0.png
tag: Tech
originalTitle: "I hacked Firebase with Redux to get free Web Socket hosting sorry Pusher"
link: "https://medium.com/coding-beauty/firebase-websocket-hack-e7c60fc5a79c"
isUpdated: false
---



<img src="/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_0.png" />

강력한 실시간 앱을 기획 중이었어요. 그래서 웹 소켓이 필수적이었죠.

하지만 찾아본 웹 소켓 호스팅 옵션은 모두 너무 비실거나 설정이 복잡했어요.

그래서 Redux의 혁신적인 트릭을 사용해 Firebase를 해킹해서 무료 웹 소켓을 사용했지요. Pusher 죄송해요.


<div class="content-ad"></div>

웹 소켓은 정말 좋아요.

우리가 흔히 아는 클래식 HTTP 요청-응답 방식과는 달리, 웹 소켓 서버는 요청이 필요하지 않고 실시간으로 여러 메시지를 다수의 연결된 클라이언트에게 보낼 수 있어요.

파이어베이스 Firestore는 무료이면서 기본적으로 이러한 강력한 실시간 기능을 제공해요. 하지만 큰 문제가 있었죠.

Firestore는 데이터 중심이자 클라이언트 중심이에요.

<div class="content-ad"></div>

하지만 웹 소켓은 행동 중심적이고 서버 중심적입니다.

웹 소켓에서 클라이언트로서 이벤트 메시지를 채널을 통해 보내고 서버가 이를 사용하여 데이터를 처리하는 방법을 결정합니다.

서버가 완전한 제어를 갖고 있으며 어떤 사용자로부터도 악의적인 조작의 여지가 없습니다.

![이미지](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_1.png)

<div class="content-ad"></div>

파이어스토어에서는 데이터를 데이터베이스에 덤프하고 끝납니다. 클라이언트는 원하는대로 저장할 수 있습니다. URL을 가지고 있다면 누구나 데이터베이스의 모든 것에 액세스할 수 있습니다.

![이미지](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_2.png)

물론, 특정 데이터 경로를 보호하기 위해 "보안 규칙"을 추가할 수 있습니다.

하지만 이는 Pusher와 같은 실제 웹 소켓 서버의 유연성과 원격 제어와는 비교할 수 없이 부족합니다.

<div class="content-ad"></div>

그리고 Pusher를 사용했지만 무료 동시 연결 수량이 적었습니다. 이 앱에서는 모든 사용자가 서버에 영구적으로 연결되어 있어야 했는데, 그러다가 앱을 닫을 때에도 포함입니다.

![이미지](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_3.png)

그리고 새 프로젝트를 작업할 때면 종종 환상을 부리고 언젠가 수천, 수백만 사용자를 보유할 것으로 기대합니다.

그래서 그 일이 언젠가 일어날 때, 상당히 많은 금액을 지불해야 할 것을 알고 있었습니다.

<div class="content-ad"></div>

하지만 Firebase Firestore를 실제 서버처럼 작동하도록 만들고 데이터를 완전히 제어할 수 있다면 어떨까요?

저는 넓은 무료 한도를 즐기고 100만 개의 동시 연결이 가능할 것입니다.

## 내가 한 일

Firestore를 데이터 중심에서 행동 중심으로 변환해야 했습니다.

<div class="content-ad"></div>

하지만 정확히 어떻게 해야 할까요? Firestore에 채널을 가져오고 데이터를 조절하는 데 필요한 모든 권한을 갖춘 종류의 "서버"를 만들 수 있을까요?

답은 Redux 입니다.

하지만 어떻게 일까요? Redux가 Firebase와 무슨 관련이 있는 걸까요?

음, 바로 Redux가 순수한 React를 데이터 중심으로 변형하는 데 도움이 되었습니다.

<div class="content-ad"></div>

아래와 같이 변경해주세요:


<img src="/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_4.png" />

액션 중심으로:

<img src="/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_5.png" />

이제 데이터를 수정하는 책임은 리듀서의 손에 있습니다. 마치 웹 소켓 또는 HTTP 서버와 같이요.


<div class="content-ad"></div>

- 작업: 클라이언트에서 서버로 채널에 실시간 메시지를 보내기
- 리듀서: 메시지를 처리하고 웹 소켓 서버의 데이터를 수정하기

그래서 어떤 방식으로든 작업과 리듀서를 Firestore로 가져와야 했습니다. 그리고 결국, 모든 것이 스키마로 이어졌다는 것을 깨달았어요.

# 작업

작업과 작업 디스패치를 복제하기 위해 서로 다른 주제의 채널에 대한 Firestore 콜렉션을 생성했습니다.

<div class="content-ad"></div>

모든 채널은 사용자가 실시간 메시지를 수신하기 위한 각각의 하위 컬렉션을 가진 Firestore 문서입니다.

채널을 통해 이벤트를 보내려면 클라이언트는 해당 이벤트를 채널 내 자체 하위 컬렉션에 추가하면 됩니다.

![이미지1](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_6.png)

![이미지2](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_7.png)

<div class="content-ad"></div>

이것을 함수로 추상화하여 재사용하기 쉽도록 만들 수 있습니다:

![image1](https://yourwebsite.com/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_8.png)

![image2](https://yourwebsite.com/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_9.png)

# 리듀서들

<div class="content-ad"></div>

이제 데이터 수정을 위한 작업 처리를 추가해야 했습니다.

새로운 액션을 추가할 때마다 클라이언트에서 트리거되는 Firebase 함수를 만들어 이 작업을 수행했습니다:

따라서 데이터는 Firestore DB의 동일한 액션 스트림 컬렉션과 함께 존재하게 됩니다:

![이미지](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_10.png)

<div class="content-ad"></div>

아무 사용자도이 데이터에 직접 액세스 할 수 없습니다. 보안 규칙에 따라 사용자는 채널 컬렉션의 하위 컬렉션을 통해 메시지를 전송할 수 있습니다.


![이미지](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_11.png)


# 서버로부터 실시간 메시지 수신

나는 클라이언트로부터 서버 이벤트 전용으로 각 채널 내에 특별한 하위 컬렉션을 생성합니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_12.png)

여기서 데이터를 저장한 후에 다른 사용자들에게 새로운 메시지를 중계합니다.

지금 서버에 클라우드 함수 트리거를 추가한 것처럼, 클라이언트 측 Firestore 리스너를 서버 하위 컬렉션에 추가합니다:

한 가지 주요 차이점은 이 클라이언트를 위해 의도된 메시지만 가져 오도록 targetIds로 필터링한 것입니다:


<div class="content-ad"></div>

아래는 Markdown 형식으로 표 태그를 변경한 것입니다.


![Image 1](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_13.png)

And I could also abstract this logic into a function to use it several times:

![Image 2](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_14.png)

![Image 3](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_15.png)


<div class="content-ad"></div>

그래서 이를 통해 돈을 들이지 않고 Firebase에서 실시간 서버 중심 웹 소켓 기능을 완벽하게 복제했습니다.

실시간 데이터베이스에서도 완벽하게 작동합니다.

# 자바스크립트가 하는 모든 미친 짓

모든 뉘앙스를 알고 있다고 생각했을 때.
자바스크립트가 하는 모든 미친 짓은 자바스크립트의 세밀한 함정과 잘 알려지지 않은 부분에 대한 매혹적인 안내서로, 귀찮은 버그를 피하고 소중한 시간을 절약하세요.

<div class="content-ad"></div>


Get a free copy here today.

![Image](/assets/img/2024-08-17-IhackedFirebasewithReduxtogetfreeWebSockethostingsorryPusher_16.png)
