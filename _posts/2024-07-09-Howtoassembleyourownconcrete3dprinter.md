---
title: "자체 제작한 콘크리트 3D 프린터 조립 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_0.png"
date: 2024-07-09 23:09
ogImage: 
  url: /assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_0.png
tag: Tech
originalTitle: "How to assemble your own concrete 3d printer."
link: "https://medium.com/@Nik_chen/how-to-assemble-your-own-concrete-3d-printer-4e6da490b6e6"
isUpdated: true
---





자신만의 콘크리트 3D 프린터를 만들기 위해서는, 게시된 가이드를 참고하시거나, 다리, 카테시안 또는 직교 좌표계 시스템을 사용하면 쉽다는 것을 알아두세요. 다리, 카테시안 또는 직교 로봇은 X-Y-Z 카테시안 좌표계에서 작동하는 세 개의 선형 관절으로 설계됩니다. 현재 시장의 콘크리트 3D 프린터 중 95%가 다리, 카테시안 또는 직교 로봇입니다. 그 이유가 있습니다.

첫 번째 파트에서는 CNC 라우터를 조립할 것입니다. 네 맞아요; CNC 라우터를 조립하고 콘크리트 3D 프린터를 만들 것입니다. 왜냐하면 콘크리트 3D 프린터는 95%가 CNC 라우터이기 때문입니다. CNC 라우터를 구입하고, 스핀들 대신 콘크리트 호스를 설치하기만 하면 됩니다.

건설 현장에서 집을 인쇄하는 데 CNC 라우터를 건설 프린터로 사용하려는 기업들을 지원하는 입장은 아닙니다. 이러한 유형의 장비는 그 작업에 적합하지 않습니다. 불행히도 투자자들은 아직까지 그 미묘한 차이를 인식하지 못하고 있으며, 스타트업은 계속해서 CNC 라우터를 개발하고 많은 금액을 조달하고 있는데, 이는 불행히도 이 산업에 해를 끼치는 결과를 낳습니다.

왜 CNC 라우터를 만드는 것이 쉽고 저렴한지 이해해 봅시다.
여기에 몇 가지 이유가 있습니다:
- 유튜브에 공개 도메인에서 어떻게 CNC 라우터를 만드는 비디오가 수천 개가 있습니다.
- 거의 모든 크기의 CNC 라우터를 만들기 위한 하드웨어 키트가 수백 개 이상 구매할 수 있습니다.
- 무료 또는 저렴한 CNC 라우터 제어 소프트웨어를 다운로드할 수 있습니다.
- 아두이노 또는 라즈베리 파이에 대한 수백 개의 오픈 소스 소프트웨어 프로젝트를 찾을 수 있습니다.

<div class="content-ad"></div>

그럼 시작해 봅시다. 필요한 준비물은 다음과 같아요:

- 모터와 리니어 가이드가 달린 프레임이 준비되어 있어야 해요
- CNC 제어 보드

<div class="content-ad"></div>


![Image 1](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_1.png)

Software

![Image 2](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_2.png)

Motor drivers — 4pc


<div class="content-ad"></div>


![Power supply for motors 48VDC](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_3.png)

![Power supply for logic (controller) 24VDC; 5A](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_4.png)


<div class="content-ad"></div>

![이미지](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_5.png)

모든 것을 연결하기 위해 일부 전선이 필요합니다.
다음 지침을 따르세요:

부품 1
부품 2
부품 3
부품 4
부품 5

CNC 라우터 부분은 CNC 라우터를 콘크리트 3D 프린터로 바꾸기 위한 종료 부분입니다. 조절 가능한 속도의 석회고 펌프를 구입하고 펌프에서 나오는 호스를 스핀들이 있어야 할 위치 뒤쪽에 연결해야 합니다.

<div class="content-ad"></div>


![A concrete 3D printer](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_6.png)

And voilà, you get a concrete 3D printer.

The progressive cavity pump is the best type for material feed.

This pump does not pulsate like a piston pump and handles very viscous materials well.


<div class="content-ad"></div>

프로그레시브 캐빈티 펌프에 대해 더 알고 싶다면, 이 비디오를 꼭 시청하세요.

이 펌프를 추천합니다:

![3D 프린터](/assets/img/2024-07-09-Howtoassembleyourownconcrete3dprinter_7.png)

내장된 VFD(가변 주파수 드라이브)가 이미 탑재돼 있어서 작은 3D 프린터를 제작하는 데 완벽합니다.

<div class="content-ad"></div>

그로 인해, 소재의 압출 속도를 수동으로 조절할 수 있습니다.

예산:

- 소프트웨어 Mach3 — $165
- CNC 제어 보드 — $89.99
- 모터 및 가이드가 장착된 프레임 (1500mm x 1500mm) — $677
- 모터 드라이버 4개 — $99.56
- 모터용 전원 공급 장치 — $78.59
- 로직용 전원 공급 장치 (컨트롤러) — $17.88
- 몰탄 펌프 — $4500 (대략적인 값)
- 기타 (전선 도구, 전기 케이스 등) — $1500

총액: $7128.02