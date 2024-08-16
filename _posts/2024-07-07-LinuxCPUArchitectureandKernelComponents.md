---
title: "리눅스  CPU 아키텍처와 커널 구성 요소 이해하기"
description: ""
coverImage: "/assets/img/2024-07-07-LinuxCPUArchitectureandKernelComponents_0.png"
date: 2024-07-07 22:32
ogImage: 
  url: /assets/img/2024-07-07-LinuxCPUArchitectureandKernelComponents_0.png
tag: Tech
originalTitle: "Linux — CPU Architecture and Kernel Components"
link: "https://medium.com/@tonylixu/linux-cpu-architecture-and-kernel-components-062e05c44614"
---


<img src="/assets/img/2024-07-07-LinuxCPUArchitectureandKernelComponents_0.png" />

전통적으로 UNIX 및 Linux 시스템은 부트스트래핑 프로세스에 Basic I/O System (BIOS)를 사용했습니다. Linux 노트북을 켤 때, 프로세스는 하드웨어에 의해 완전히 제어됩니다. 먼저, 하드웨어는 Power On Self Test (POST)를 실행하도록 설계되어 있으며 이는 BIOS의 일부입니다. POST는 RAM과 같은 하드웨어 구성 요소가 올바르게 작동하는지 확인합니다.

현대적인 설정에서 BIOS에서 이전에 수행되던 기능들은 대부분 Unified Extensible Firmware Interface (UEFI)에 의해 대체되었습니다. UEFI는 운영 체제와 플랫폼 펌웨어 간의 소프트웨어 인터페이스를 개요하는 공개 사양입니다. BIOS라는 용어가 여전히 문서와 기사에 나타날 수 있지만, 이를 UEFI로 대체하여 생각하는 것이 좋습니다.

# BIOS (Basic Input/Output System):

<div class="content-ad"></div>

BIOS는 컴퓨터가 부팅될 때 사용되는 미리 설치된 프로그램입니다. 컴퓨터 메인보드의 칩에 저장되어 있으며 전원이 켜질 때 가장 먼저 실행되는 소프트웨어입니다. BIOS는 시스템을 준비하고 하드웨어를 확인하며 운영 체제가 컴퓨터 하드웨어에서 실행되도록 합니다. 또한 시스템 시간 및 부팅 순서와 같은 다양한 설정을 관리할 수 있게 합니다. BIOS는 널리 사용되지만 부팅 시간이 느리고 하드 디스크 크기에 제한이 있는 등 특정 제한이 있습니다.