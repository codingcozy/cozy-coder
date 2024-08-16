---
title: "해커를 위한 리눅스 기초 101 Part 3"
description: ""
coverImage: "/assets/img/2024-07-07-LINUXFORHACKERS101Part3_0.png"
date: 2024-07-07 22:33
ogImage: 
  url: /assets/img/2024-07-07-LINUXFORHACKERS101Part3_0.png
tag: Tech
originalTitle: "LINUX FOR HACKERS 101 Part 3"
link: "https://medium.com/@agapehearts/linux-for-hackers-101-part-3-42b9988311f8"
---


![이미지](/assets/img/2024-07-07-LINUXFORHACKERS101Part3_0.png)

# Linux 저장 장치 및 파티션 이해하기

이전 블로그에서 언급했듯이 리눅스는 서로 다른 드라이브 파티션에 대해 별도의 드라이브 문자를 사용하지 않습니다. 모든 활성 저장 장치는 루트 디렉토리부터 시작하는 단일 파일 시스템의 일부입니다. 루트 디렉토리는 슬래시(/)로 표시됩니다. 이 동작 방식을 이해하려면 리눅스가 저장 장치와 해당 장치의 파티션을 어떻게 사용하는지 알아야 합니다. 함께 알아보겠습니다!

## 리눅스 시스템의 저장 장치

<div class="content-ad"></div>

리눅스는 특정 이름으로 저장 장치를 식별합니다. 예를 들어, 첫 번째 SATA 하드 드라이브는 dev/sda로 식별됩니다. 여기서 `s`는 SATA 드라이브임을 나타내고, `a`는 첫 번째 드라이브임을 나타냅니다. 두 번째 SATA 하드 드라이브는 dev/sdb가 될 것입니다.