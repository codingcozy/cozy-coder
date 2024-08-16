---
title: "리눅스  CPU 사용률 높을 때 문제 해결하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-LinuxHowtoTroubleshootHighCPUUsage_0.png"
date: 2024-07-12 22:50
ogImage: 
  url: /assets/img/2024-07-12-LinuxHowtoTroubleshootHighCPUUsage_0.png
tag: Tech
originalTitle: "Linux — How to Troubleshoot High CPU Usage"
link: "https://medium.com/@tonylixu/linux-how-to-troubleshoot-high-cpu-usage-70a441802422"
---



![이미지](/assets/img/2024-07-12-LinuxHowtoTroubleshootHighCPUUsage_0.png)

데브옵스 엔지니어에게는 시스템 성능, 확장성 및 비용 효율성에 영향을 미치는 여러 가지 요인으로 높은 CPU 사용량을 해결하는 기술을 숙달하는 것이 매우 중요합니다.

CPU 병목 현상을 식별하고 해결함으로써 엔지니어들은 성능 저하의 위험을 줄이고 최적의 자원 활용을 유지할 수 있습니다. 효율적인 CPU 관리는 확장성을 위해 필수적이며, 응용 프로그램이 성능에 영향을 미치지 않고 증가하는 작업 부하를 원활하게 처리할 수 있도록 합니다.

적극적인 문제 해결과 최적화를 통해 엔지니어들은 변화하는 요구에 적응할 수 있는 확장 가능한 아키텍처를 설계할 수 있으며, 결국 클라우드 환경에서 인프라 비용을 줄이고 비용 효율성을 향상시킬 수 있습니다.


<div class="content-ad"></div>

# CPU 사용률이란

CPU 사용률은 시스템이나 특정 프로세스에 의해 현재 사용되는 중앙 처리 장치(CPU)의 처리 용량의 백분율을 의미합니다.

이는 CPU가 애플리케이션을 실행하거나 시스템 프로세스를 처리하고 입력/출력 작업을 처리하는 등 다양한 작업을 위해 명령을 실행하는 시간을 나타냅니다. CPU 사용률은 일반적으로 특정 기간 동안 백분율로 측정되며 높은 백분율은 CPU에 더 많은 작업 부하가 있다는 것을 나타냅니다.

가장 일반적으로 사용되고 익숙한 CPU 측정 항목 중 하나인 CPU 사용률이 어떻게 계산되는지 말해 줄 수 있나요? 또한 %user, %nice, %system, %iowait, %steal 등의 의미를 알고 계신가요? 이들 사이의 차이를 설명해 주실 수 있나요?

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-12-LinuxHowtoTroubleshootHighCPUUsage_1.png)

이번에 언급한 대로 Linux는 각 CPU의 시간을 매우 짧은 시간 조각으로 나누고, 스케줄러를 통해 각 작업에 시간 조각을 할당하여 동시에 실행되는 것처럼 보이도록합니다.

CPU 시간을 얻기 위해 Linux는 미리 정의된 비트 속도(HZ로 표시)를 통해 시간 인터럽트를 트리거하고, 부팅 이후 비트 수를 기록하기 위해 전역 변수 jiffies를 사용합니다. 시간 인터럽트가 발생할 때마다 jiffies 값이 1씩 증가합니다.

비트 속도 HZ는 커널에 대해 구성 가능한 옵션으로 100, 250, 1000 등으로 설정할 수 있습니다. 서로 다른 시스템은 다른 값을 설정할 수 있으며, /boot/config 커널 옵션을 조회하여 구성 값을 확인할 수 있습니다. 예를 들어, 내 CONFIG_HZ는 다음과 같습니다:


<div class="content-ad"></div>

```js
$ grep 'CONFIG_HZ=' /boot/config-$(uname -r)
CONFIG_HZ_250=y
CONFIG_HZ=250
```

CONFIG_HZ=250은 초당 250번의 인터럽트가 발생한다는 것을 의미합니다 (1 인터럽트는 4 밀리초마다 발생). 시간 인터럽트를 받으면, 커널은 모든 작업을 중지하고 다양한 스케줄링 및 시간 관련 작업을 수행합니다.

틱 레이트 HZ는 커널 옵션이므로 사용자 공간 프로그램이 직접 액세스할 수 없습니다. 사용자 공간 프로그램의 편의를 위해 커널은 항상 100으로 고정된 사용자 공간 틱 레이트 USER_HZ를 제공합니다. 이렇게 하면 사용자 프로그램이 커널에서 HZ가 어떻게 설정되었는지 신경 쓸 필요가 없습니다.

# 자세한 CPU 사용량


<div class="content-ad"></div>

리눅스에서 시스템의 내부 상태는 /proc 가상 파일 시스템을 통해 사용자 공간에서 접근할 수 있습니다. 여러 파일 중에서 /proc/stat은 시스템의 CPU 및 작업 통계를 중요하게 제공합니다. 예를 들어:

```js
$ cat /proc/stat | grep ^cpu
cpu  280580 7407 286084 172900810 83602 0 583 0 0 0
cpu0 144745 4181 176701 86423902 52076 0 301 0 0 0
cpu1 135834 3226 109383 86476907 31525 0 282 0 0 0
```

위 명령어 'cat /proc/stat | grep ^cpu'는 리눅스 커널의 /proc 가상 파일 시스템에서 CPU 사용 통계에 관한 정보를 검색하고 표시합니다. 결과를 살펴보겠습니다:

- 열 1 — CPU: 이 행은 모든 CPU 코어에 대한 집계 통계를 제공합니다. 값은 다양한 메트릭을 나타냅니다:
- 열 2 — 사용자 모드: (일반적으로 us로 약자 사용) 사용자 모드 CPU 시간을 나타냅니다. 여기서는 nice time이 포함되지 않지만 guest time은 포함됩니다.
- 열 3 — Nice 모드: (보통 ni로 약자 사용)은 낮은 우선순위의 사용자 모드 CPU 시간을 나타냅니다. 즉, 프로세스의 nice 값이 1에서 19 사이로 조정될 때의 CPU 시간을 의미합니다. 여기서 nice 값의 범위는 -20부터 19까지입니다. 값이 클수록 우선 순위가 낮습니다.
- 열 4 — 시스템 모드: (일반적으로 sys로 약자 사용) 커널 모드 CPU 시간을 나타냅니다.
- 열 5 — 아이들: (일반적으로 id로 약자 사용) 아이들 시간을 나타냅니다. 여기서는 I/O를 기다리는 시간(iowait)은 포함되지 않습니다.
- 열 6 — I/O 대기: (일반적으로 wa로 약자 사용) I/O를 기다리는 CPU 시간을 나타냅니다.
- 열 7 — IRQ: (일반적으로 hi로 약자 사용) 하드 인터럽트 처리를 위한 CPU 시간을 나타냅니다.
- 열 8 — SoftIRQ: (일반적으로 si로 약자 사용) 소프트 인터럽트를 처리하는 CPU 시간을 나타냅니다.
- 열 9 — Steal: (일반적으로 st로 약자 사용) 다른 가상 머신에 의해 사용된 CPU 시간을 나타냅니다.
- 열 10 — Guest: (일반적으로 guest로 약자 사용) 가상화를 통해 다른 운영 체제를 실행하는 데 소요된 시간을 나타냅니다. 즉, 가상 머신을 실행하는 CPU 시간을 의미합니다.
- 열 11 — Guest_nice: (일반적으로 gnice로 약자 사용) 낮은 우선순위로 가상 머신을 실행하는 데 소요된 시간을 나타냅니다.

<div class="content-ad"></div>

## CPU 사용량 계산

"CPU 사용량"은 다음 공식을 사용하여 계산될 수 있다고 생각할 수 있습니다:

```js
CPU 사용량 = 1 - (CPU 유휴 시간/CPU 총 시간)
```

이 공식에 따르면, /proc/stat의 데이터를 사용하여 CPU 사용량을 쉽게 계산할 수 있습니다. 그러나 /proc/stat의 값은 부팅 이후의 누적 값이므로 /proc/stat에서 직접 계산하는 것은 매우 정확하지 않습니다. 실제로 대부분의 성능 도구는 다음과 같은 공식을 사용합니다.

<div class="content-ad"></div>

```js
avg_cpu_usage = 1 - (new_cpu_idle - old_cpu_idle)/(new_cpu_total - old_cpu_total)
```

다음은 Python에서의 예제 계산입니다:

```js
# 예제 값
new_cpu_idle = 200000
old_cpu_idle = 100000
new_cpu_total = 400000
old_cpu_total = 300000

# 평균 CPU 사용량 계산
avg_cpu_usage = 1 - (new_cpu_idle - old_cpu_idle) / (new_cpu_total - old_cpu_total)

# 결과 출력
print("평균 CPU 사용량:", avg_cpu_usage)
```

이제 시스템 CPU 사용량이 어떻게 계산되는지 알았으니, 프로세스는 어떨까요? 시스템 메트릭과 유사하게, Linux는 각 프로세스에 대한 실행 통계를 제공합니다. 이는 `/proc/`pid`/stat`에 있습니다.

<div class="content-ad"></div>

예를 들어 top 및 ps 도구가 보고하는 CPU 사용량을 비교하면, 기본 결과가 다를 수 있습니다. top은 기본적으로 3초 간격을 사용하고, ps는 프로세스의 전체 수명을 사용합니다.

# CPU 사용량 확인 방법

CPU 사용량의 의미를 이해한 후에 이제 CPU 사용량을 확인하는 방법을 살펴봅시다. 가장 일반적으로 사용되는 두 도구는 top과 ps입니다:

- top은 시스템 전체의 CPU 및 메모리 사용량뿐만 아니라 개별 프로세스의 자원 사용량을 보여줍니다.
- ps는 각 프로세스의 자원 사용량만 표시합니다.

<div class="content-ad"></div>

top 명령어 예시:

```js
top - 09:45:21 up 10 days,  2:36,  3 users,  load average: 0.75, 0.83, 0.90
Tasks: 256 total,   2 running, 254 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.0 us,  0.5 sy,  0.0 ni, 97.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  15825.9 total,   4451.8 free,   4425.0 used,   5949.1 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.  10677.3 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    1 root      20   0  169408   7380   5176 S   0.0   0.0   0:01.22 systemd
    2 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kthreadd
    4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:+
```

이 출력에서 세 번째 줄 %Cpu는 시스템의 CPU 사용률을 의미합니다. 각 열의 의미는 이전 섹션에 언급되어 있으며, 각 CPU의 사용률을 보려면 1 키를 눌러야 합니다.

빈 줄 이후에는 프로세스의 실시간 정보가 표시됩니다. 각 프로세스에는 %CPU 열이 있어 해당 프로세스의 CPU 사용률을 나타냅니다. 이 %CPU 값은 사용자 모드와 커널 모드 CPU 사용량의 합입니다.

<div class="content-ad"></div>

그래서 말씀드린대로 top은 프로세스의 사용자 모드 CPU와 커널 모드 CPU를 세분화하지 않습니다. 그렇다면 각 프로세스의 세부 정보는 어떻게 확인할까요? pidstat 명령어를 사용할 수 있습니다.

pidstat 명령어 출력

```js
# 5번 총 출력, 1초 간격
$ pidstat 1 5
15:56:02      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
15:56:03        0     15006    0.00    0.99    0.00    0.00    0.99     1  dockerd
...
평균:      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
평균:        0     15006    0.00    0.99    0.00    0.00    0.99     -  dockerd
```

위 출력에서 1초 간격으로 5개 그룹의 프로세스 CPU 사용량이 표시됩니다.

<div class="content-ad"></div>

- 사용자 모드 CPU 사용량 (%usr);
- 커널 모드 CPU 사용량 (%system);
- 가상 머신 CPU 사용량 (%guest);
- CPU 대기 시간 (%wait);
- 그리고 전체 CPU 사용량 (%CPU).

마지막으로, 5 세트의 데이터 평균도 계산됩니다.

# 고 CPU 사용량 문제 해결

![High CPU Usage](/assets/img/2024-07-12-LinuxHowtoTroubleshootHighCPUUsage_2.png)

<div class="content-ad"></div>

고 CPU 사용량을 해결할 때, top, ps, pidstat과 같은 도구를 사용하여 높은 CPU 사용량(예: 100%)을 가진 프로세스를 쉽게 찾을 수 있어요. 그런 다음, 코드에서 CPU를 점유하는 기능이 무엇인지 알고 싶을 거예요. 프로그램 중 어떤 부분이 높은 CPU 사용량을 유발하는지 알고 싶다면 perf 명령어를 사용할 수 있어요.

perf는 Linux 2.6.31 이후의 내장 성능 분석 도구예요. 성능 이벤트 샘플링을 기반으로 시스템의 다양한 이벤트와 커널 성능을 분석할 수 있을 뿐만 아니라 지정된 애플리케이션의 성능 문제를 분석할 수도 있어요.

## perf top

perf top은 top과 유사해요. 실시간으로 가장 많은 CPU 클럭을 차지하는 함수나 명령어를 표시하여 핫 함수를 찾을 수 있어요. 인터페이스는 다음과 같아요:

<div class="content-ad"></div>

```js
$ perf top
샘플: 833, 이벤트 'cpu-clock'의 횟수 (약): 97742399
오버헤드  공유 오브젝트       심볼
   7.28%  perf                [.] 0x00000000001f78a4
   4.72%  [kernel]            [k] vsnprintf
   4.32%  [kernel]            [k] module_get_kallsym
   3.65%  [kernel]            [k] _raw_spin_unlock_irqrestore
...

위 출력의 각 섹션이 무엇을 나타내는지 알아봅시다:

- 샘플: 이 라인은 수집된 샘플 수와 모니터링 중인 이벤트를 나타냅니다. 이 경우 cpu-clock 이벤트에 대해 833개의 샘플이 수집되었습니다.
- 이벤트 횟수 (약): 이 라인은 샘플링 기간 동안 발생한 총 이벤트 횟수의 대략적인 개수를 제공합니다. 이 경우 대략 97,742,399의 cpu-clock 이벤트가 발생했습니다.
- 오버헤드: 이 열은 특정 함수나 심볼에서 샘플된 전체 CPU 시간에 대한 CPU 시간의 백분율을 보여줍니다. 예를 들어, 첫 번째 행은 'perf' 바이너리 내부의 주소 0x00000000001f78a4에있는 함수에서 CPU 시간의 7.28%가 소비되었음을 나타냅니다.
- 공유 오브젝트: 이 열은 해당 심볼이 위치한 공유 오브젝트나 실행 파일의 이름을 표시합니다. 예를 들어, '[kernel]'은 해당 심볼이 커널 내에 위치함을 나타냅니다.
- 심볼: 이 열은 CPU 시간이 소비된 함수나 심볼의 이름이나 주소를 제공합니다. 예를 들어, vsnprintf 및 module_get_kallsym은 커널 함수이며, _raw_spin_unlock_irqrestore는 스핀 락에 사용되는 커널 매크로입니다.

위 출력을 예로 들어, perf 도구 자체가 가장 많은 CPU 클럭을 소비하지만 그 비율은 7.28%로, 시스템에 CPU 성능 문제가 없음을 나타냅니다. perf top의 사용에 대해 매우 명확히 알고 있어야 합니다.
``` 

<div class="content-ad"></div>

예를 들어:

```js
$ perf record # 샘플링을 중지하려면 Ctrl+C를 누르세요
[ perf record: 데이터를 기록하도록 1회 깨어남 ]
[ perf record: 0.452 MB perf.data(6093개의 샘플)를 캡처하고 기록함 ]
$ perf report # 보고서 표시
```

# 결론

CPU 사용량은 가장 직관적이고 흔히 사용되는 시스템 성능 지표이며, 성능 문제 해결 시 주로 집중하는 첫 번째 지표입니다.

<div class="content-ad"></div>

이해해야할 것들은 사용자 모드 (%user), 좋음 모드 (%nice), 시스템 모드 (%system), I/O 대기 (%iowait), 인터럽트(%irq) 및 소프트 인터럽트(%softirq)의 사용법입니다. 예를 들어:

- 사용자 CPU와 좋음 CPU가 높을 경우, 사용자 모드 프로세스가 더 많은 CPU를 차지하므로 프로세스의 성능에 집중해야합니다.
- 시스템 CPU가 높을 경우, 커널 모드가 더 많은 CPU를 차지하므로 커널 스레드나 시스템 호출의 성능을 확인해야합니다.
- CPU가 I/O 대기 중인 경우, I/O 대기 시간이 상대적으로 길다는 것을 나타내므로 시스템 스토리지에 I/O 문제가 있는지 확인해야합니다.
- 소프트 인터럽트와 하드 인터럽트가 높을 경우, 소프트 인터럽트 또는 하드 인터럽트의 처리 프로그램이 더 많은 CPU를 차지하므로 커널의 인터럽트 서비스 프로그램을 확인해야합니다.