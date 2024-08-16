---
title: "Linux 시스템에서 Softirq로 인한 높은 CPU 사용 해결 방법"
description: ""
coverImage: "/assets/img/2024-07-07-HowToAddressHighCPUUsageFromSoftirqsinLinuxSystems_0.png"
date: 2024-07-07 22:31
ogImage: 
  url: /assets/img/2024-07-07-HowToAddressHighCPUUsageFromSoftirqsinLinuxSystems_0.png
tag: Tech
originalTitle: "How To Address High CPU Usage From Softirqs in Linux Systems"
link: "https://medium.com/@cstoppgmr/how-to-address-high-cpu-usage-from-softirqs-in-linux-systems-8216ce1f9755"
---


인터럽트는 시스템의 동시성을 향상시키기 위해 설계된 비동기 이벤트 처리 메커니즘입니다. 인터럽트 이벤트가 발생하면 인터럽트 핸들러의 실행을 트리거합니다. 이 핸들러는 상단 하프(top half)와 하단 하프(bottom half)로 나뉩니다.

- 상단 하프(하드 인터럽트): 인터럽트를 빠르게 처리합니다.
- 하단 하프(소프트 인터럽트): 상단 하프에서 완료되지 않은 작업을 비동기적으로 처리합니다.

Linux에서는 네트워크 수신 및 송신, 타이머, 스케줄링, RCU 잠금 등 다양한 유형의 소프트 인터럽트가 포함됩니다. proc 파일시스템의 /proc/softirqs를 확인하여 소프트 인터럽트의 상태를 확인할 수 있습니다.

<div class="content-ad"></div>

리눅스의 각 CPU에는 해당 CPU 번호를 가진 ksoftirqd라는 소프트 인터럽트 커널 스레드가 있습니다. 소프트 인터럽트 이벤트의 발생 빈도가 너무 높은 경우, 이 커널 스레드는 높은 CPU 사용량을 유발할 수 있어 네트워크 수신 및 송신에 지연이 발생하거나 스케줄링이 느려질 수 있습니다.

소프트 인터럽트에서 발생하는 높은 CPU 사용량은 일반적인 성능 문제입니다. 오늘은 인기 있는 리버스 프록시 서버 Nginx를 예시로 하여 이 상황을 분석하는 방법을 알려드리겠습니다.

# 사례 분석

이 사례 연구에서는 두 가상 머신을 사용하고, 그들의 관계를 설명하는 다이어그램을 작성했습니다.