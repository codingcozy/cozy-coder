---
title: "CPU 활용도, 메모리, 디스크 사용량을 확인하는 리눅스 명령어들 "
description: ""
coverImage: "/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_0.png"
date: 2024-07-09 22:58
ogImage: 
  url: /assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_0.png
tag: Tech
originalTitle: "Linux commands to check CPU Utilization ,memory and disk."
link: "https://medium.com/@The_Anshuman/linux-commands-to-check-cpu-utilization-memory-and-disk-c9daf8a53be8"
---


리눅스에서 CPU 사용률과 메모리를 확인하는 명령어를 알아야 합니다. 그래서 이번에는 이에 대해 이야기해볼게요.

# CPU 사용률 

리눅스에서 CPU 사용량을 확인하려면 다양한 명령어가 있습니다. 확인해보죠.

## 1. Top

<div class="content-ad"></div>

시스템 성능에 대한 실시간 정보를 제공하는데요, CPU 및 메모리 사용량을 포함합니다.

기본적으로 top 명령어는 데이터를 5초마다 업데이트합니다.

여러 옵션이 있는데, 예를 들어 —

P는 CPU 사용량에 따라 모든 실행 중인 프로세스를 정렬합니다.

<div class="content-ad"></div>

M로 실행 중인 모든 프로세스를 메모리 사용량으로 정렬합니다.

I로 모든 유휴 프로세스를 숨김니다.

S로 모든 프로세스를 실행된 지 얼마나 오래되었는지에 따라 정렬합니다.

U로 특정 사용자가 소유한 모든 프로세스를 볼 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_0.png" />

## 2. mpstat

이 명령어는 CPU 사용량 통계를 보여줍니다. CPU 사용량을 시간에 따라 모니터링할 간격과 반복 횟수를 지정할 수 있습니다.

이것을 사용하려면 먼저 sysstat 패키지를 확인해야 합니다.

<div class="content-ad"></div>


![](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_1.png)

To display the report of all processors

![](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_2.png)

## 3. sar


<div class="content-ad"></div>

이 명령은 CPU 및 메모리 사용량을 포함한 시스템 활동 정보를 수집, 보고 및 저장합니다.

-u 옵션으로 CPU 성능을 추적하고 뒤에 초 단위의 시간을 입력하십시오

![image](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_3.png)

## 4. iostat

<div class="content-ad"></div>

리눅스에서는 이 명령어를 사용하여 시스템의 입력/출력(I/O) 장치 통계를 보고하고 모니터링하며, 디스크 이용률과 성능에 대한 정보를 포함합니다.

![이미지](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_4.png)

-c 옵션을 사용하여 CPU 이용률을 사용자 프로세스, 시스템 프로세스, I/O 대기 및 유휴 시간으로 나눌 수 있습니다.

![이미지](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_5.png)

<div class="content-ad"></div>

## 5. vmstat

이 Linux 명령어는 시스템 가상 메모리 사용량 및 CPU 사용량, 디스크 I/O 및 시스템 프로세스와 같은 다른 시스템 통계에 대한 정보를 표시하는 데 사용됩니다.

![image](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_6.png)

# 메모리 및 디스크 활용

<div class="content-ad"></div>

To check memory in Linux, we have various commands. Let's take a look...

### 1. `free`

This command shows memory and swap space usage in kilobytes or megabytes.

![Image](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_7.png)

<div class="content-ad"></div>

## 2. df

이 명령은 마운트된 파일 시스템의 디스크 공간 사용량에 대한 정보를 표시하는 데 사용됩니다.

![이미지](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_8.png)

## 3. du

<div class="content-ad"></div>

이 명령은 파일 및 디렉터리 공간 사용량을 추정하는 데 사용됩니다.


![이미지](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_9.png)


가장 큰 파일 또는 폴더를 얻으려면 "du -s * | sort -n"을 사용하십시오.


![이미지](/assets/img/2024-07-09-LinuxcommandstocheckCPUUtilizationmemoryanddisk_10.png)


<div class="content-ad"></div>

그게 CPU 및 메모리 사용량 명령어에 관한 모든 내용이에요.

....

감사합니다!!

....

<div class="content-ad"></div>

….

….

더 많은 자료를 보시려면 아래 링크를 팔로우해주세요:

[https://medium.com/@The_CodeConductor](https://medium.com/@The_CodeConductor)