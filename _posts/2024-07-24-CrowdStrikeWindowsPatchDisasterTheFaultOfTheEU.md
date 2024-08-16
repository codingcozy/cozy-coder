---
title: "CrowdStrike 윈도우 패치 대참사, EU 때문"
description: ""
coverImage: "/assets/img/2024-07-24-CrowdStrikeWindowsPatchDisasterTheFaultOfTheEU_0.png"
date: 2024-07-24 11:56
ogImage: 
  url: /assets/img/2024-07-24-CrowdStrikeWindowsPatchDisasterTheFaultOfTheEU_0.png
tag: Tech
originalTitle: "CrowdStrike Windows Patch Disaster The Fault Of  The EU"
link: "https://medium.com/@tsecretdeveloper/crowdstrike-windows-patch-disaster-the-fault-of-the-eu-5b2addd464fb"
---


지난 금요일 일어난 CrowdStrike-Windows 장애를 기억해 보세요. 전 세계적으로 윈도우 기기에 큰 피해가 발생했죠.

항공사는 이착륙을 할 수 없었어요. 병원은 수술을 연기했고 기차도 늦었어요.

기기들이 회복 루프에 갇혀있는 동안 누구 탓인지 다툼이 시작되었어요. 심지어 IT 지원 직원들도 아직 키보드에 눈물을 흘리고 있을 때였죠.

<div class="content-ad"></div>

이건 정말이지요. 마이크로소프트는 이 문제로 인한 압박을 원치 않고 다른 누굴 탓하려는 것 같아요. 그들은 오늘의 편한 희생양인 EU를 탓하고 있어요. 

# 커널에서의 못나 행동쟁이

Crowdstrike의 Falcon 소프트웨어는 기계를 사이버 공격과 악성 소프트웨어로부터 보호하기 위해 설계되었어요. 이는 실제로 커널 모듈로 작동하며 메모리 관리부터 장치 관리까지 운영 체계의 핵심 기능에 접근할 수 있으며 장치 드라이버처럼 작동합니다.

만약 누군가 *크라우드스트라이크*가 빈 (음, 제로로 가득 찬) sys 파일을 업데이트에 작성하고 오류를 기술적으로 처리하지 않았다면 어떨까요. 이는 불량 파일로부터 읽힌 올바르지 않은 포인터, 잘못된 접근 및 블루 스크린으로 이어졌어요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-24-CrowdStrikeWindowsPatchDisasterTheFaultOfTheEU_1.png" />

이 논증을 따라주셔서 고마워요.

만약 따라오기 어렵다면, 간단히 이해해주세요. EU에 언급되지 않았으며 (논란의 여지가 있는) 여기에서 관련이 없다고 할 수 있습니다. 그래서 아마도 마이크로소프트가 비난을 완전히 유럽으로 돌려 놓은 이유일지도 모릅니다.

# 희생양 설정

<div class="content-ad"></div>

월스트리트 저널에 발표한 Microsoft의 진술에서는 유럽 위원회에 명확하게 책임을 뒀습니다.

Microsoft가 제3자 앱에 커널 액세스를 제공하여 경쟁과 상호 운용성을 증진해야 한다는 것이 사실일지도 모릅니다.

그러나 이것은 사용자 보호를 위해 그들의 프로세스를 보장하는 책임을 면제시키지는 않습니다. 특히 이러한 주요 시스템 오류에 대한 책임을 유럽 연합에 뒤집어 씌우는 것은 공정하지 않습니다.

확실한 것은 보안과 개방성 사이에 섬세한 균형이 필요하다는 점입니다. Microsoft는 정말 더 나은 일을 해야 하며, 그들이 다른 단체에 책임을 뒤집어 씌우는 것은 공정하거나 올바르지 않습니다.

<div class="content-ad"></div>

# 문제 해결 된 걸까요?

Microsoft은 관리자들이 기계를 복구하는 데 도움을 주는 도구를 출시했어요. 적어도 Microsoft가 그 문제를 해결하기 위해 장치를 껐다 켰다 하는 것을 제안했던 것보다 조금 더 나은 방법 같네요.

아마도 Microsoft는 이런 특별한 제안을 하는 데 대해 유럽연합을 탓할 것 같아요.

솔직히 말해서, 저는 여기서 문제가 Microsoft의 솔루션 품질에 있는 거라고 생각해요. 다른 이들을 문제의 책임자로 삼고 싶어하는 사실은 그들의 일과 시장에서 믿을 만한 정도에 대해 많은 이야기를 해요.

<div class="content-ad"></div>

# 결론

마이크로소프트는 CrowdStrike 재앙에서 자신들의 책임을 인정해야 합니다. 이 회사는 안전하고 견고한 운영 체제를 제공하는 책임이 있습니다. 그렇지 않다면, 그들이 왜 거기 있나요? 그들은 무엇을 하고 있나요?

이것들은 마이크로소프트가 답해야 할 질문들입니다.

# 저자에 대하여

<div class="content-ad"></div>

프로 개발자 "The Secret Developer"는 Twitter에서 @TheSDeveloper로 찾을 수 있으며 Medium.com을 통해 정기적으로 기사를 게시합니다.

The Secret Developer는 코드 버그를 유럽 연합을 탓하려고 생각 중입니다.