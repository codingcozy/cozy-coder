---
title: "개발자가 테스트를 했다면, 전 세계적인 대혼란을 막을 수 있었을까"
description: ""
coverImage: "/assets/img/2024-07-26-IfDevelopersTestedaGlobalMeltdownMightHaveBeenAverted_0.png"
date: 2024-07-26 11:53
ogImage: 
  url: /assets/img/2024-07-26-IfDevelopersTestedaGlobalMeltdownMightHaveBeenAverted_0.png
tag: Tech
originalTitle: "If Developers Tested, a Global Meltdown Might Have Been Averted"
link: "https://medium.com/@tsecretdeveloper/if-developers-tested-a-global-meltdown-might-have-been-averted-d9cbbfa40cc4"
---



![Image](/assets/img/2024-07-26-IfDevelopersTestedaGlobalMeltdownMightHaveBeenAverted_0.png)

소프트웨어 개발은 예측할 수 없는 삶으로 이끌어 줄 수 있어요.

새로운 업데이트를 위해 즐겁게 코드를 작성한 하루는 실수를 고치느라 밤을 새우는 것으로 이어질 수도 있어요. 소프트웨어 엔지니어가 아닌 사람들은 행복에서 비애로의 급격한 전환을 상상할 수 없을 겁니다. 하지만 그것이 우리가 살아가는 삶이에요.

대부분의 경우, 우리에게 일어나는 피해는 잃어버린 하룻밤의 수면일 뿐이에요. 코드를 제대로 테스트하지 않았다는 조금의 창피함. 그리고 그 후에는 삶을 계속해 나가게 되죠.


<div class="content-ad"></div>

그렇다면, CrowdStrike가 비행을 중단하고 작업을 중단하며 전 세계적으로 블루 스크린 오브 데스가 기본 화면보호기로 설정될 정도로 재앙적인 업데이트를 어떻게 전달했는지 알고 계신가요?

그들은 사전 사건 검토에서 모든 것을 말했고, 결과가 귀하를 깜짝 놀라게 할 수 있습니다.

# 결함이 있는 업데이트

2024년 7월 19일, CrowdStrike는 Falcon Windows 센서용 'Rapid Response Content' (콘텐츠 구성 업데이트)를 발표했습니다. 새로운 위협에 대한 텔레메트리를 수집하여 엔드포인트 및 ID 보호를 향상시키는 대신 Windows를 크래시시키는 결과를 초래했습니다.

<div class="content-ad"></div>

설링크테이블을 사용하면 좀 더 보기 좋은 형식으로 표를 만들어볼 수 있어요. 이렇게:

| header1 | header2 |
|---------|---------|
| data1   | data2   |