---
title: "Redis 설정 및 메모리 데이터베이스 구축 - Day 6 Redis 마스터 클래스"
description: ""
coverImage: "/assets/img/2024-07-07-SiloSettingupRedisformem-dbDay6_0.png"
date: 2024-07-07 20:01
ogImage: 
  url: /assets/img/2024-07-07-SiloSettingupRedisformem-dbDay6_0.png
tag: Tech
originalTitle: "Silo: Setting up Redis for mem-db — Day 6"
link: "https://medium.com/@javascriptBard/silo-setting-up-redis-for-mem-db-day-6-fa0d6453fd8c"
---


![이미지](/assets/img/2024-07-07-SiloSettingupRedisformem-dbDay6_0.png)

간단하게 말씀드리겠습니다.

실로(Silo)에서 Redis가 어떤 이유로 중요한지 설명하는 것이 목표가 아닙니다. 본문은 실제 구현이 어떻게 작동하는지에 중점을 두고 있습니다. Redis가 좋은 아이디어인 이유에 대한 많은 자료들이 있으니, 제 말에 귀 기울이기 전에 그런 자료들을 참고하시는 것이 훨씬 나을 것입니다.

실로는 세션 저장, 일부 캐싱, 그리고 발행/구독(pub/sub)과 같은 작업에 Redis를 사용할 것입니다.

<div class="content-ad"></div>

현재 구현은 간단하며 이전 포스트에서 Cassandra를 만든 것과 일치합니다.

클라이언트는 다음과 같이 생겼어요:

```js
import { createClient } from 'redis'

class RedisClient {
  constructor(options) {
    this.options = options
  }

  async connect() {
    this.client = await createClient(this.options)
      .on('error', err => console.log('Redis Client Error', err))
      .connect()
  }

  async get(key) {
    return this.client.get(key)
  }

  async set(key, value, options = {}) {
    return this.client.set(key, value, options)
  }

  async del(key) {
    return this.client.del(key)
  }

  async quit() {
    await this.client.quit()
  }
}

export default RedisClient
```

사용법은 이렇습니다:

<div class="content-ad"></div>

```js
// RedisClient 및 config.js 임포트...
const redisClient = new RedisClient(config.redis)
await redisClient.connect()

// redisClient.get...
// redisClient.set...
```

나는 Silo에서 객체에 대한 클래스 기반 구현을 향해 가는 것 같은 방향에 아직 망설이고 있어. 그래서 내가 다른 포스트를 통해 결정을 내릴 때마다 이리저리 돌아다닐 것이야.

또한 Cassandra와 마찬가지로, Redis 컨테이너에 대해 docker-compose를 사용할 거야. 기본 하드코딩 버전은 다음과 같이 보여:

```js
  redisDB:
    image: redis:latest
    ports:
      - "6379:6379"
    healthcheck:
      test: "redis-cli ping"
      interval: 30s
      timeout: 10s
      retries: 10
    environment:
      - username=redis
      - password=redis
    volumes:
      - redisDB-data:/data
```

<div class="content-ad"></div>

아직 해야 할 일이 많고 정리해야 할 것도 더 많지만, 오늘의 목표는 매우 기본적인 작업을 완료하는 것이었습니다.

카산드라처럼, Redis에 GUI가 필요합니다. 저는 조 페너의 훌륭한 Redis Commander를 선택했고, 이 역시도 Docker에서 작동합니다 (도커를 좋아한다는 걸 아실 겁니다).

우리는 약간의 가짜 데이터를 넣고...요술 같네요!

![image](/assets/img/2024-07-07-SiloSettingupRedisformem-dbDay6_1.png)

<div class="content-ad"></div>

실로에서 Redis가 정말로 영구 데이터 저장 옵션으로 취급되지는 않겠지만, 드라이브를 사용하여 저장하면 Redis 컨테이너를 완전히 다시 시작할 때에도 어느 정도의 데이터 지속성이 보장될 것입니다. 지금으로서는 충분합니다.

직접적인 종속성 측면에서, 기본적인 Redis 클라이언트에 추가해야 할 것이 하나 있으므로, 현재 3개가 됩니다.

다음으로 진행하는 합리적인 단계는 검색-DB(meilisearch)로 이동하고 같은 처리를 적용하는 것인데, 그러나 그것은 잠시 미루고 먼저 다른 작업에 집중하기로 했습니다. 무엇인지는 잘 모르겠지만, 아마도 cron 작업과 관련된 작업일 가능성이 높을 것입니다.