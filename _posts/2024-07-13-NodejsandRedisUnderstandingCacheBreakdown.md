---
title: "Nodejs와 Redis 캐시 붕괴 이해하기"
description: ""
coverImage: "/assets/img/2024-07-13-NodejsandRedisUnderstandingCacheBreakdown_0.png"
date: 2024-07-13 00:19
ogImage: 
  url: /assets/img/2024-07-13-NodejsandRedisUnderstandingCacheBreakdown_0.png
tag: Tech
originalTitle: "Node.js and Redis: Understanding Cache Breakdown"
link: "https://medium.com/@louistrinh/node-js-and-redis-understanding-cache-breakdown-6ce021e8ec2b"
isUpdated: true
---






![Cache breakdown](/assets/img/2024-07-13-NodejsandRedisUnderstandingCacheBreakdown_0.png)

캐시 붕괴는 많은 동시 요청이 존재하지 않는 캐시 키에 액세스할 때 발생하는 현상입니다. 이는 여러 가지 문제를 유발합니다:

- 시스템 부하 증가: Redis는 누락된 키에 대한 요청을 계속 처리해야 하므로 응답 시간이 지연되고 전체 시스템 성능에 영향을 줍니다.
- 자원 낭비: Redis는 캐시된 데이터를 저장하기 위해 메모리를 사용합니다. 캐시 붕괴 시 메모리가 존재하지 않는 키에 낭비됩니다.
- 사용자 경험 저하: 사용자는 응답 대기 시간이 길어지면서 부정적인 사용자 경험을 겪게 됩니다.

캐시 붕괴의 일반적인 원인은 다음과 같습니다:


<div class="content-ad"></div>

- 소프트웨어 버그: 애플리케이션 코드의 버그는 존재하지 않는 캐시 키에 접근할 수 있습니다.
- 자주 변경되는 데이터: 데이터가 자주 변경되면 캐시가 시간 내에 업데이트되지 않아 키가 누락될 수 있습니다.
- 갑작스러운 트래픽 급증: 트래픽이 급증하면 캐시가 부하를 처리하지 못할 수 있어 고장이 발생할 수 있습니다.

캐시 고장을 해결하는 전략:

- 캐시 키에 만료 시간 설정: 더 이상 유효하지 않은 키를 위한 메모리를 해제합니다.
- 중첩 캐싱 구현: 여러 캐시 레벨에 데이터를 저장하여 자주 액세스되는 데이터에는 Redis를 사용하고 드물게 액세스되는 데이터에는 Memcached를 사용합니다.
- 쫓기 알고리즘 활용: 가장 적게 사용된 키를 캐시에서 제거하여 새로운 키에 공간을 확보합니다.
- 분산 캐싱 사용: 여러 개의 Redis 서버를 활용하여 캐시된 데이터를 저장하며 부하를 분산시키고 오류 허용성을 향상시킵니다.

서킷 브레이커 패턴:

<div class="content-ad"></div>

- 개념: 캐시 키에 대한 일정 수의 실패한 요청이 발생하면, 회로 차단기가 자동으로 캐시와의 연결을 차단하고 요청을 원본 데이터베이스로 직접 리디렉션합니다. 대기 기간 이후에는 회로 차단기가 캐시에 다시 연결을 시도합니다.
- 이점: 캐시와 주 데이터베이스에 대한 부하를 줄이면서 에러 전파를 방지합니다.

높은 가용성을 갖춘 Redis 캐시 클러스터 구축:

- Redis Sentinel 사용: Redis 클러스터 내에서 마스터/슬레이브 장애 조치를 모니터하고 자동으로 처리합니다.
- Redis 클러스터 구현: 데이터 샤딩 및 분배를 가능케 하여 장애 허용성과 확장성을 향상시킵니다.

더 많은 자료:

<div class="content-ad"></div>

- Redis 캐시 붕괴에 대한 문서: (링크 제공 불가 — URL 제공 불가)
- 회로 차단기 패턴: (링크 제공 불가 — URL 제공 불가)
- 고가용성 Redis 클러스터 구축: [Redis 공식 문서 - 고가용성 확장 관리](https://redis.io/docs/management/scaling/)

추가 사항:

- 캐시 붕괴를 해결하기 위한 최선의 방법은 귀하의 특정 애플리케이션 요구 사항에 따라 다릅니다.
- 고가용성 Redis 캐시 클러스터를 구축할 때 성능, 신뢰성, 비용과 같은 요소를 주의 깊게 고려하십시오.