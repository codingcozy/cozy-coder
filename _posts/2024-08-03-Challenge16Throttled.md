---
title: "챌린지 16 Throttled 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-Challenge16Throttled_0.png"
date: 2024-08-03 20:14
ogImage: 
  url: /assets/img/2024-08-03-Challenge16Throttled_0.png
tag: Tech
originalTitle: "Challenge 16 Throttled"
link: "https://medium.com/@danielepolencic/challenge-16-throttled-93133f8fd0ad"
---


쿠버네티스에서 CPU 제한은 항상 명확하지 않을 수 있으며 지연 증가로 이어질 수 있습니다.

그 이유를 이해하려면 작동 방식을 기억하는 것이 중요합니다.

CPU 제한을 정의할 때, 프로세스에 대한 CPU/시간 할당량을 정의합니다.

예를 들어, 1vCPU 제한은 100ms마다 전체 코어를 의미합니다.

<div class="content-ad"></div>

만약 0.5 vCPU만 사용하면, 나머지가 낭비됩니다. 그러나 1.5 vCPU를 사용할 수는 없어요. 시도하면, 커널이 전체 1 vCPU를 사용할 수 있게 해줄 거에요, 그러나 나머지 0.5 vCPU는 다음 주기(즉, 다음 100ms)에서 사용해야 합니다.

여기서 끝나지 않아요. 첫 번째 몇 밀리초에서 CPU 할당량을 모두 사용할 수 있지만, 더 많은 CPU가 필요하면 다음 주기까지 기다려야 해요.

스레드를 사용할 때는 조금 더 복잡해요. 각각이 CPU 시간이 필요하기 때문이죠.

![Challenge16Throttled_0.png](/assets/img/2024-08-03-Challenge16Throttled_0.png)

<div class="content-ad"></div>

가정하에 10개의 스레드를 가진 프로세스가 있고, 각각이 작동하는 데 0.2vCPU가 필요합니다. 이 프로세스는 단일 CPU로 제한됩니다. 다음 중 몇 가지 문장이 진실입니까:

- 프로세스는 쓰로틀되지 않습니다.
- 각 스레드는 쓰로틀되기 전에 0.1 vCPU를 사용할 수 있습니다.
- 스레드는 1vCPU의 제한을 공유합니다. 총량이 1vCPU를 초과하면 모두 쓰로틀됩니다.
- 위의 어느 것도 아닙니다.

해당 솔루션은 3번 옵션입니다: 스레드는 1vCPU의 제한을 공유합니다. 총량이 1vCPU를 초과하면 모두 쓰로틀됩니다.

<div class="content-ad"></div>

모든 스레드가 동일한 프로세스를 사용하여 시작되기 때문에 cgroup이 적용된 제한은 프로세스에 적용됩니다.

첫 10ms에서 한 스레드가 1 vCPU의 모든 사용 가능한 CPU 할당량을 사용한다면, 전체 예산을 사용할 뿐만 아니라:

- 다른 스레드가 실행되지 않으며, 다음 기간(100ms) 동안 쓰로틀링 됩니다.
- 현재 스레드도 90ms 동안 쓰로틀링됩니다(100ms - 10ms).

다음 간격에서 운이 나쁘다면, 다른 스레드가 CPU 할당량을 소비하고 남은 스레드가 굶주릴 수도 있습니다.

<div class="content-ad"></div>

이 문제를 어떻게 해결할 수 있을까요?

CPU 한도를 올바르게 설정하는 것은 복잡합니다. 대부분의 경우, CPU 한도를 설정하지 않는 것이 더 좋을 수 있습니다. CPU 한도를 설정하지 않으면 여분의 CPU 자원이 필요한 프로세스에 의해 사용될 수 있습니다.

CPU 한도를 설정하는 데 관한 두 가지 인기 있는 글이 있습니다:

- 쿠버네티스에서 CPU 한도 설정 사용을 중지하십시오
- 쿠버네티스에서 CPU 한도 설정 사용을 계속해야 하는 이유

<div class="content-ad"></div>

만약 이 글이 마음에 드셨다면 아래 내용도 좋아하실 수 있어요:

- Learnk8s에서 운영하는 Kubernetes 코스들을 확인해보세요.
- 저는 매주 Learn Kubernetes Weekly 뉴스레터를 발행하고 있어요.
- 저가 20주 동안 발행한 20개의 Kubernetes 개념 시리즈도 있답니다.