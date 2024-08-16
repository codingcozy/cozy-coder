---
title: "변화의 시작 AWS에서 조용히 서비스 종료하는 2024년 동향"
description: ""
coverImage: "/assets/img/2024-08-03-DisruptionAheadAWSQuietlyAxingServices_0.png"
date: 2024-08-03 20:05
ogImage: 
  url: /assets/img/2024-08-03-DisruptionAheadAWSQuietlyAxingServices_0.png
tag: Tech
originalTitle: "Disruption Ahead AWS Quietly Axing Services"
link: "https://medium.com/@horovits/disruption-ahead-aws-quietly-axing-services-033e7518eefb"
---


# 아마존이 조용히 Cloud9, SimpleDB, CodeCommit 및 기타 서비스를 폐기하고 있습니다.

사용자들이 CloudCommit, Code9 및 기타 서비스가 새로운 사용자에게 차단되었다고 보고한 것으로 시작되었습니다.

![이미지](/assets/img/2024-08-03-DisruptionAheadAWSQuietlyAxingServices_0.png)

그런 다음 AWS의 주요 전도사 인 Jeff Barr가 트위터에서 간단한 노트를 트윗하여 "AWS CodeCommit를 포함한 일부 서비스에 대한 새 액세스를 중단하기로 결정했습니다"라고 밝혔습니다.

<div class="content-ad"></div>

Jeff Barr의 트윗에는 CodeCommit 이외의 다른 서비스 이름을 언급하지 않았으며 공지나 문서에 대한 추가 정보나 링크를 제공하지 않았습니다.

![이미지](/assets/img/2024-08-03-DisruptionAheadAWSQuietlyAxingServices_1.png)

쓰레드에서 나온 질문에 대해 Barr은 참고할 AWS 서비스 목록을 다음과 같이 알려주었습니다: "S3 Select, CloudSearch, Cloud9, SimpleDB, Forecast, Data Pipeline 및 CodeCommit."

![이미지](/assets/img/2024-08-03-DisruptionAheadAWSQuietlyAxingServices_2.png)

<div class="content-ad"></div>

이번 서비스 축소의 파도에 포함된 유일한 서비스들인가요? 그렇지 않아요. 지난 몇 달 동안 다른 서비스들도 목록에서 제외되었는데, Snowmobile, Workdocs(문서 공유), DevOps용 OpsWorks 요소, 그리고 금융 서비스용 Finspace 등이 포함됐다고 합니다. 이 정보는 SummitRoute의 aws_breaking_changes 추적기에 따라 확인됐어요.

이러한 변경으로 AWS 사용자뿐만 아니라 이러한 서비스로 방향을 향했던 파트너들에게도 큰 타격이 될 것으로 예상됩니다. 그러나 AWS는 시작부터 서비스를 실험하고는 폐기해왔다. 그렇다면 새로운 일이 무엇일까요?

이곳에는 더 큰 의미가 있습니다. AWS는 중요한 깨달음에 도달한 것 같아요. 아마도 우리 모두에게 다가와야 할 깨달음일지도 모르겠네요.

한 곳에서 모든 것을 이루려는 클라우드 쇼핑몰 접근 방식이 실패했다는 것 같아요. AWS는 모든 사람에게 모든 것을 할 수 없어요. AWS는 인프라 서비스에 대해서는 잘 해왔지만, 인프라에서 개발로 나가는 과정은 아직 긴 여정이 남아 있어요.

<div class="content-ad"></div>

Dev 자체는 하나의 것조차 아닙니다. 백엔드, 프론트엔드, 모바일, IoT 또는 Web3는 산업별로는 물론 금융 서비스나 의료와 같은 전문 영역입니다. 어떤 대 기업이건 모든 것에 전문화하고 뛰어나기는 불가능합니다. 이것들은 서로 다른 인격입니다.

예를 들어 Cloud9를 살펴보겠습니다. 이는 개발자들이 브라우저에서 애플리케이션 코드를 작성, 실행 및 디버깅할 수 있는 멋진 웹 통합 개발 환경(IDE)을 제공하는 멋진 스타트업이었습니다. 그것은 훌륭한 도구였고, 이를 사용하여 고객 워크샵을 열었습니다. 아마존이 Google과 Microsoft의 개발 도구 경쟁에서 대응하기 위해 인수했지만, AWS에게는 적합한 장소가 아니었습니다. 그것은 그들의 전문 분야가 아니었습니다.

이 깨달음은 아마존 웹 서비스가 기반 기술에서 가장 잘하는 일에 주력하고, 전문 기업의 부가 서비스를 통합하여 기술 동맹과 파트너 생태계에 더 많은 투자하는 데 도움이 되길 바랍니다. 이것이 품질 높은 개발자 경험 및 DevOps, SRE, 시스템 관리자, 데이터 과학자 및 기타 인격의 경험에 이르는 올바른 접근 방식입니다.