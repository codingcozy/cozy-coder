---
title: "Spring Boot 332 벤치마크 Web, Reactive, CDS, AOT, Virtual Threads, JVM, Native 비교 분석"
description: ""
coverImage: "/assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_0.png"
date: 2024-08-17 01:03
ogImage: 
  url: /assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_0.png
tag: Tech
originalTitle: "Spring Boot 332 Benchmark Web, Reactive, CDS, AOT, Virtual Threads, JVM, and Native"
link: "https://medium.com/itnext/spring-boot-3-3-2-benchmark-web-reactive-cds-aot-virtual-threads-jvm-and-native-42d3b704e88e"
isUpdated: true
updatedAt: 1723863794066
---


## 스프링 부트 | 웹 | 리액티브 | CDS | AOT | 가상 스레드 | JVM | 네이티브

![이미지](/assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_0.png)

본문(하단 링크 참조)에서는 스프링 부트의 다양한 설정(웹, 리액티브, CDS, AOT, 가상 스레드, JVM, 네이티브)에 따른 성능을 벤치마킹하는 목표를 개요로 설명합니다.

간략히 말하자면, 우리는 Spring Web과 Spring Reactive를 갖춘 두 개의 스프링 부트 애플리케이션을 구현했습니다. 그들의 이름은 각각 'spring-boot-greeting-api-web'과 'spring-boot-greetings-api-reactive'입니다. 또한, 다양한 설정에 대한 JVM과 네이티브 도커 이미지를 구축했습니다. 이 설정에는 가상 스레드, CDS, AOT 여부에 따른 옵션이 포함되어 있었습니다.

<div class="content-ad"></div>

ivangfr/web-reactive-jvm-native-cds-aot-virtual-threads GitHub 저장소에서 소스 코드를 확인하실 수 있습니다. 자세한 내용은 주요 문서를 참조해주세요.

각 구성에 대해 JVM 및 Native Docker 이미지를 생성하기 위해 Spring Boot의 최신 버전을 사용할 예정입니다.

벤치마킹은 각 구성별로 JVM과 Native Docker 이미지에 대한 로드 테스트를 포함할 것입니다 (즉, CDS, AOT 또는 가상 스레드를 활성화한 경우와 그렇지 않은 경우).

각 반복에 대해 우리는:

<div class="content-ad"></div>

- 도커 컨테이너를 시작하세요.
- 100, 300, 900 및 마지막으로 2700개의 동시 요청으로 OHA 테스트 진행
- 도커 컨테이너를 종료하세요.

시작해 봅시다.

# 사용된 Java 및 Spring Boot 버전

이 성능 벤치마크에는 Java 버전 21과 Spring Boot 버전 3.3.2를 사용했습니다.

<div class="content-ad"></div>

# Docker 이미지 및 컨테이너 이름

이 성능 테스트용으로 빌드된 Docker 이미지는 이 Docker Hub 링크에서 찾을 수 있어요.

여기 우리가 만든 Docker 이미지 설정들이 있어요:

## JVM 인사 API 웹

<div class="content-ad"></div>

Docker 이미지 이름: ivanfranchin/spring-boot-greetings-api-web:3.3.2-21-jvm;

해당 이미지를 사용하는 Docker 컨테이너들:

- sb-web-jvm;
- sb-web-jvm-vt (환경 변수 SPRING_THREADS_VIRTUAL_ENABLED를 true로 설정하여).

## JVM Greetings API Web with CDS enabled

<div class="content-ad"></div>

도커 이미지 이름: ivanfranchin/spring-boot-greetings-api-web:3.3.2-21-jvm-cds;

이 이미지를 사용하는 도커 컨테이너 이름:

- sb-web-jvm-cds;
- sb-web-jvm-cds-vt (환경 변수 SPRING_THREADS_VIRTUAL_ENABLED를 true로 설정하여).

## JVM Greetings API Web with CDS and AOT enabled

<div class="content-ad"></div>


도커 이미지 이름: ivanfranchin/spring-boot-greetings-api-web:3.3.2–21-jvm-cds-aot;

사용 중인 도커 컨테이너 이름: sb-web-jvm-cds-aot.

## JVM Greetings API Web with CDS, AOT and Virtual Threads enabled

도커 이미지 이름: ivanfranchin/spring-boot-greetings-api-web:3.3.2–21-jvm-cds-aot-vt;


<div class="content-ad"></div>

Docker 컨테이너 이름은 sb-web-jvm-cds-aot-vt를 사용하고 있어요.

## Native Greetings API Web

도커 이미지 이름: ivanfranchin/spring-boot-greetings-api-web:3.3.2-21-native;

Docker 컨테이너 이름: sb-web-native를 사용하고 있습니다.

<div class="content-ad"></div>

## 네이티브 그리팅 API 웹에 가상 스레드 기능이 활성화된 버전입니다

도커 이미지 이름: ivanfranchin/spring-boot-greetings-api-web:3.3.2–21-native-vt;

사용 중인 도커 컨테이너 이름: sb-web-native-vt.

## JVM 그리팅 API 반응형 웹

<div class="content-ad"></div>

다음은 Markdown 형식으로 표현된 내용입니다.


- Docker 이미지 이름: ivanfranchin/spring-boot-greetings-api-reactive:3.3.2–21-jvm;
- 해당 이미지를 사용하는 Docker 컨테이너 이름: sb-reactive-jvm.

## JVM Greetings API Reactive Web with CDS enabled

- Docker 이미지 이름: ivanfranchin/spring-boot-greetings-api-reactive:3.3.2–21-jvm-cds;


<div class="content-ad"></div>

아래에 있는 표를 Markdown 형식으로 바꿔주실 수 있나요?

<div class="content-ad"></div>

## 네이티브 인사 API 리액티브 웹

도커 이미지 이름: ivanfranchin/spring-boot-greetings-api-reactive:3.3.2–21-native;

이를 사용하는 도커 컨테이너 이름: sb-reactive-native.

# 도커 컨테이너 메모리 제한

<div class="content-ad"></div>

도커 컨테이너를 실행할 때마다 메모리 제한을 1024MB로 설정했습니다.

# 사용된 기계 구성

이 벤치마크는 다음과 같은 특성을 가진 MacBook Pro에서 실행될 것입니다:

- 프로세서: 1.7 GHz 쿼드코어 인텔 코어 i7
- 메모리: 16GB 2133MHz LPDDR3

<div class="content-ad"></div>

우리는 Docker Desktop v4.29.0을 사용하고 있어요. 사용 가능한 모든 CPU와 메모리가 할당되어 있어요.

# 벤치마크 결과

아래 이미지에서는 Docker 컨테이너를 OHA로 100, 300, 900 및 마지막으로 2700개의 동시 요청으로 로드 테스트한 후 얻은 전체 결과를 보여드리겠어요:

```js
              응용 프로그램 | 요청 수 | 동시성 | 성공률(%) | 전체 시간(초) | 가장 느린 시간(초) | 가장 빠른 시간(초) | 평균 시간(초) | 요청/초 |
------------------------- + -------- + ------ + ---------- + -------------- + ----------------- + ----------------- + ----------------- + ---------- |
               sb-web-jvm |      100 |    100 |     100.00 |        1.2907 |            1.2894 |            1.2709 |            1.2788 |    77.4773 |
               sb-web-jvm |      300 |    300 |     100.00 |        2.2941 |            2.2862 |            1.0328 |            1.4614 |   130.7725 |
               sb-web-jvm |      900 |    900 |     100.00 |        5.1505 |            5.1357 |            1.0319 |            2.8984 |   174.7403 |
               sb-web-jvm |     2700 |   2700 |     100.00 |       14.1995 |           14.1099 |            1.0465 |            7.4166 |   190.1470 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
           sb-web-jvm-cds |      100 |    100 |     100.00 |        1.3432 |            1.3410 |            1.3170 |            1.3277 |    74.4495 |
           sb-web-jvm-cds |      300 |    300 |     100.00 |        2.1476 |            2.1417 |            1.0330 |            1.4241 |   139.6898 |
           sb-web-jvm-cds |      900 |    900 |     100.00 |        5.2627 |            5.2499 |            1.0628 |            2.9746 |   171.0157 |
           sb-web-jvm-cds |     2700 |   2700 |     100.00 |       14.2169 |           14.1609 |            1.0365 |            7.4027 |   189.9154 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
       sb-web-jvm-cds-aot |      100 |    100 |     100.00 |        1.4257 |            1.4235 |            1.3886 |            1.4002 |    70.1419 |
       sb-web-jvm-cds-aot |      300 |    300 |     100.00 |        2.1917 |            2.1837 |            1.0333 |            1.4393 |   136.8788 |
       sb-web-jvm-cds-aot |      900 |    900 |     100.00 |        5.2048 |            5.1859 |            1.0508 |            2.9247 |   172.9163 |
       sb-web-jvm-cds-aot |     2700 |   2700 |     100.00 |       14.1609 |           14.1123 |            1.0324 |            7.3611 |   190.6659 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
            sb-web-native |      100 |    100 |     100.00 |        1.0699 |            1.0682 |            1.0234 |            1.0510 |    93.4661 |
            sb-web-native |      300 |    300 |     100.00 |        2.1622 |            2.1578 |            1.0301 |            1.5489 |   138.7488 |
            sb-web-native |      900 |    900 |     100.00 |        5.1408 |            5.1323 |            1.0537 |            2.8892 |   175.0701 |
            sb-web-native |     2700 |   2700 |     100.00 |       14.2305 |           14.1652 |            1.0319 |            7.3852 |   189.7332 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
            sb-web-jvm-vt |      100 |    100 |     100.00 |        1.2249 |            1.2224 |            1.1770 |            1.2046 |    81.6417 |
            sb-web-jvm-vt |      300 |    300 |     100.00 |        2.2549 |            2.2476 |            1.0418 |            1.4391 |   133.0465 |
            sb-web-jvm-vt |      900 |    900 |     100.00 |        2.5317 |            2.5191 |            1.0565 |            1.6788 |   355.4891 |
            sb-web-jvm-vt |     2700 |   2700 |     100.00 |        5.0524 |            5.0201 |            1.0419 |            2.7341 |   534.4014 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
        sb-web-jvm-cds-vt |      100 |    100 |     100.00 |        1.2734 |            1.2707 |            1.1835 |            1.2331 |    78.5322 |
        sb-web-jvm-cds-vt |      300 |    300 |     100.00 |        2.3037 |            2.2971 |            1.0374 |            1.4106 |   130.2228 |
        sb-web-jvm-cds-vt |      900 |    900 |     100.00 |        3.1914 |            3.1835 |            1.0738 |            1.7153 |   282.0038 |
        sb-web-jvm-cds-vt |     2700 |   2700 |     100.00 |        4.3888 |            4.3648 |            1.0633 |            2.4219 |   615.2065 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
    sb-web-jvm-cds-aot-vt |      100 |    100 |     100.00 |        1.3345 |            1.3330 |            1.2432 |            1.2881 |    74.9361 |
    sb-web-jvm-cds-aot-vt |      300 |    300 |     100.00 |        2.3051 |            2.2995 |            1.0439 |            1.4787 |   130.1471 |
    sb-web-jvm-cds-aot-vt |      900 |    900 |     100.00 |        2.6009 |            2.5899 |            1.0551 |            1.7021 |   346.0315 |
    sb-web-jvm-cds-aot-vt |     2700 |   2700 |     100.00 |        4.4972 |            4.4756 |            1.0676 |            2.6563 |   600.3699 |
......................... + ........ + ...... + .......... + .............. + ................. + ................. + ................. + ........... |
         sb-web-native-vt |      100 |    100 |     100.00 |        1.0352 |            1.0328 |

<div class="content-ad"></div>

다음은 시작 시간 및 최대 CPU 및 메모리 소비량을 보여주는 이미지입니다:

              Application | StartUpTime(sec) |  Max CPU(%) | Max Memory(MB) |
------------------------- | ----------------- | ----------- | -------------- |
               sb-web-jvm |           1.7780 |      103.80 |         281.10 |
           sb-web-jvm-cds |           1.1530 |      142.01 |         279.50 |
       sb-web-jvm-cds-aot |           0.8230 |      148.10 |         268.90 |
            sb-web-native |           0.0610 |       21.53 |         226.70 |
            sb-web-jvm-vt |           1.8080 |      369.82 |         389.80 |
        sb-web-jvm-cds-vt |           1.1240 |      305.53 |         457.10 |
    sb-web-jvm-cds-aot-vt |           0.8100 |      380.67 |         417.30 |
         sb-web-native-vt |           0.0800 |       74.18 |         289.40 |
          sb-reactive-jvm |           1.9030 |      490.41 |         239.10 |
      sb-reactive-jvm-cds |           1.1930 |      407.66 |         218.20 |
  sb-reactive-jvm-cds-aot |           0.8240 |      398.32 |         211.80 |
       sb-reactive-native |           0.0440 |       78.68 |         217.30 |

이 데이터를 몇 가지 차트로 살펴보겠습니다:

## 각 요청별 평균 응답 시간

<div class="content-ad"></div>

![Spring Boot Benchmark Results](/assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_1.png)

초반에 동시 요청이 100개 있을 때, 모든 응용 프로그램은 유사한 평균 응답 시간을 보여줍니다. 가상 스레드가 없는 sb-web-* 앱의 평균 응답 시간은 1.2644초이며, 가상 스레드를 사용하는 sb-web-*-vt 앱은 1.1884초의 평균을 달성했습니다. sb-reactive-* 앱은 요청당 1.2136초의 평균 응답 시간을 가졌습니다.

동시성이 300개 요청으로 증가할 때, 가상 스레드가 없는 sb-web-* 앱은 평균 응답 시간이 1.4684초로 약간 상승합니다. 우리는 Apache Tomcat을 사용하고 있으며, 서버.tomcat.max-threads의 기본값이 200으로 설정되어 있기 때문에 가상 스레드가 없는 sb-web-* 앱은 즉시 처리할 수 없는 요청을 대기열에 넣게 됩니다. 가상 스레드를 사용하는 sb-web-*-vt 앱은 평균 응답 시간이 1.4464초를 달성했습니다. 한편, sb-reactive-* 앱은 요청당 평균 응답 시간이 1.0898초로 상대적으로 안정적입니다.

900개와 2700개의 동시 요청에서, 가상 스레드가 없는 sb-web-* 앱은 확장성에 어려움을 겪으면서 평균 응답 시간이 각각 2.9217초와 7.3914초가 되었습니다. 가상 스레드를 사용하는 sb-web-*-vt 앱은 요청을 더 잘 처리하여 평균 응답 시간이 각각 1.6993초와 2.6023초가 되었습니다. 반면에, sb-reactive-* 앱은 더 효율적으로 높은 부하를 처리하며, 각각 1.2584초와 1.7051초의 평균 응답 시간을 유지했습니다.

<div class="content-ad"></div>

## 스타트업 시간

![이미지](/assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_2.png)

네이티브 이미지 sb-web-native, sb-web-native-vt 및 sb-reactive-native의 스타트업 시간은 평균 0.0616초로 인상적으로 빠릅니다.

CDS 및 CDS+AOT를 적용하면 웹 및 리액티브 앱의 스타트업 시간이 줄어드는 것도 데이터에서 확인할 수 있습니다. 예를 들어, sb-web-jvm은 1.778초가 소요됩니다. CDS를 적용하면 sb-web-jvm-cds는 1.153초에 시작하여 35.15% 빠릅니다. CDS 및 AOT를 모두 적용하면 sb-web-jvm-cds-aot는 0.823초에 시작하여 sb-web-jvm보다 53.71% 빠릅니다.

<div class="content-ad"></div>

가상 스레드를 활성화해도 시작 시간에는 영향을 미치지 않는다는 것을 확인할 수 있습니다. 이는 가상 스레드가 처리량을 향상시키는 데 의도된 것이기 때문에 이해할 만합니다.

## 최대 메모리 사용량

![이미지](/assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_3.png)

sb-web-* 앱을 분석한 결과, sb-web-native이 최소 메모리 소비량을 가집니다. CDS를 활성화한 경우 (sb-web-jvm-cds) 또는 CDS+AOT (sb-web-jvm-cds-aot)를 적용한 경우 sb-web-jvm에 비해 최대 메모리 사용량이 약간 감소합니다.

<div class="content-ad"></div>

가상 스레드가 웹 앱에서 활성화되면 최대 메모리 소비량이 평균적으로 약 50% 증가합니다. 예를 들어, sb-web-jvm-cds-vt는 sb-web-jvm-cds와 비교했을 때 64% 증가하며 최대 메모리 사용량이 가장 높습니다.

sb-reactive-* 앱의 경우, 모두 평균 최대 메모리 소비량이 약 221.6MB로, 이는 웹 앱과 비교하여 낮은 수준입니다. CDS를 활성화하는 경우(sb-reactive-jvm-cds) 또는 CDS+AOT를 활성화하는 경우(sb-reactive-jvm-cds-aot) 모두 sb-reactive-jvm과 비교했을 때 최대 메모리 소비량이 약간 감소합니다.

## 최대 CPU 사용량

![이미지](/assets/img/2024-08-17-SpringBoot332BenchmarkWebReactiveCDSAOTVirtualThreadsJVMandNative_4.png)

<div class="content-ad"></div>

네이티브 이미지 sb-web-native, sb-web-native-vt 및 sb-reactive-native은 CPU 소비량이 매우 낮습니다.

sb-web-jvm* 앱을 분석하면 CDS (sb-web-jvm-cds) 및 CDS+AOT (sb-web-jvm-cds-aot)를 활성화할 때 최대 CPU 사용량이 약간 증가하는 것을 볼 수 있습니다. 가상 스레드가 활성화된 경우, 특히 sb-web-jvm-vt, sb-web-jvm-cds-vt, sb-web-jvm-cds-aot-vt 앱에서 최대 CPU 사용량은 대략 두 배 또는 세 배 더 늘어납니다.

sb-reactive-jvm* 앱의 경우, sb-reactive-jvm이 이 비교에서 모든 앱 중 가장 높은 최대 CPU 사용량을 기록합니다. 그러나 웹 앱과 달리 리액티브 앱은 CDS (sb-reactive-jvm-cds) 또는 CDS+AOT (sb-reactive-jvm-cds-aot)를 활성화하면 최대 CPU 사용량이 줄어듭니다.

# 결론

<div class="content-ad"></div>

이 글에서는 Spring Web (Spring MVC with Apache Tomcat)을 사용한 Spring Boot 애플리케이션과 Spring Reactive Web (Spring WebFlux with Netty)을 사용한 다른 애플리케이션을 구현했습니다. 다양한 설정을 위해서 JVM 및 Native Docker 이미지를 모두 빌드하여 가상 쓰레드, CDS, AOT 최적화 옵션 등을 비교하고 각 방법의 장단점을 이해하였습니다.

200이라는 기본 server.tomcat.threads.max 값으로 설정된 전통적인 Spring Web 응용 프로그램은 이 한계를 넘어서는 동시 요청을 효과적으로 처리하지 못합니다. 너무 많은 요청이 대기열에 들어가며 동시성이 높아질수록 평균 응답 시간이 증가합니다.

Spring Reactive가 전반적으로 가장 좋은 옵션이었습니다. 프로그래밍 스타일에 익숙하다면 다른 방법들보다 모든 메트릭에서 뛰어났으며, 메모리 소비도 그중에서 우수했습니다. 하지만 CPU 사용량에서 가장 높은 피크를 보였습니다.

CDS 및 AOT 최적화를 활성화하면 Web 및 Reactive 애플리케이션 모두 시작 시간을 더 빨리 달성할 수 있습니다.

<div class="content-ad"></div>

만약 전통적인 Spring Web 애플리케이션을 사용하고 있고 서버.tomcat.threads.max 속성을 계속 조정하거나 Spring Reactive로 리팩토링하는 것이 싫다면, 이미 Java 21을 사용 중이 아니라면 업그레이드하고 애플리케이션 구성 파일에 한 줄 추가하여 가상 스레드를 활성화하는 것을 고려해보세요. 이렇게 하면 애플리케이션이 가상 스레드를 생성하여 더 많은 동시 요청을 처리하는 데 도움이 되어 평균 응답 시간을 예상 범위 내로 유지할 수 있습니다. 그러나 CPU 사용량이 증가하고 더 많은 메모리가 필요할 수 있습니다.

# 지원 및 참여

이 글을 즐겼고 지원하고 싶다면 다음과 같은 조치를 고려해보세요:

- 👏 클랩(clapping), 강조 및 이야기에 답글을 달아 참여해주세요. 궁금한 점이 있으면 언제든지 답변해 드릴게요;
- 🌐 나의 이야기를 소셜 미디어에 공유해주세요;
- 🔔 Medium | LinkedIn | Twitter | GitHub에서 저를 팔로우해주세요;
- ✉️ 최신 게시물을 놓치지 않도록 뉴스레터를 구독해주세요.