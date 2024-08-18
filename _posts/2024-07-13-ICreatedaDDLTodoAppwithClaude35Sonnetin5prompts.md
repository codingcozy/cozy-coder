---
title: "Claude 35의 Sonnet을 사용해 5번의 프롬프트로 DDLTodo 앱 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_0.png"
date: 2024-07-13 23:05
ogImage:
  url: /assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_0.png
tag: Tech
originalTitle: "I Created a DDLTodo App with Claude 3.5 Sonnet in 5 prompts"
link: "https://medium.com/generative-ai/i-created-a-ddltodo-app-with-claude-3-5-sonnet-in-5-prompts-4814bc0f3673"
isUpdated: true
---

![Desk Image](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_0.png)

# 배경

지금 당신의 책상이 마치 복잡하고 기능이 많이 들어간 할 일 앱처럼 생겼다고 상상해보세요. 다음 주에 트윗할 것을 상기시키는 알림이 있고, 항공권을 예약해야 한다는 또 다른 알림이 있으며, SubStack 콘텐츠 업데이트를 검색해야 한다는 메모가 있습니다. 각기 다른 프로젝트가 엮이고, 정돈된 그룹화 시스템에 의해 다양한 크기의 수많은 저장함이 만들어졌습니다. 책상 속의 한 서브-프로젝트 안에 감춰진 이벤트 티켓 예약에 관한 작은 상기 알림을 놓치기는 생각보다 쉬울 수 있습니다. 이 실수는 오랫동안 계획해온 여행이 맛을 잃게 할 수도 있습니다.

이제, 자신이 모든 것을 청소하고 오직 가장 긴요한 사항들에만 신경 쓸 수 있는 깨끗하고 새롭게 정리된 책상을 마련하기로 결정한 것처럼 상상해보세요. 바로 이 앱이 제가 만들고자 했던 것입니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이 환상은 독특한 도전을 불러일으켰어요: Claude 3.5 Sonnet의 다섯 가지 질문만을 사용하여 iOS 앱을 만들고 출시하는 것! 이 아이디어는 Claude 3.5의 새로운 "Artifacts" 기능에서 영감을 받았어요. 이 기능은 코드 스니펫, 문서, 그리고 웹사이트 디자인까지 생성할 수 있어요. SwiftUI 미리보기를 아직 지원하지 않는 Artifacts의 빠른 속도와 능력에 정말 감명받았죠.

하루에 다섯 번의 무료 이용 기회만 있는 이 제한은 제 창의력을 자극했어요: 이 다섯 번의 기회 안에 앱 스토어에 성공적으로 앱을 출시할 수 있을까요?

처음 컨셉은 간단했어요. 지저분한 책상 위 모든 것을 털어버리고 중요한 일을 공백의 종이에 적어두는 행위를 반영했거든요. 그래서 원래 앱 이름을 '10ThingsTODO'로 지었어요. 하지만 우리 모두 마감 기한에 압박을 받게 되죠. 그래서 제출 기한이 있는 작업들을 가장 중요한 위치에 두었어요. 지속적인 최적화 끝에, 이 앱은 마침내 'DDLTodo'라는 이름으로 앱 스토어에서 성공적으로 출시되었어요.

# 시작 프롬프트

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![Link to image 1](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_1.png)

안녕하세요! AI를 활용한 개발에 뛰어들기 전에 앱의 비전을 스케치했어요. 처음 설계한 것은 간단했어요: 최대 10가지 할 일 항목이 나열되고, 각 항목에는 마감일과 알림 설정 옵션이 포함되어 있었어요. 앱은 각 작업의 남은 시간을 보여주고, 제목은 현재 작업 수를 동적으로 업데이트했어요.

![Link to image 2](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_2.png)
이 스케치를 가지고 클로드 3.5 소네트를 위한 초기 프롬프트를 작성했어요:

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

당신은 훌륭한 iOS 개발자입니다.
작업이 있습니다. 표준을 충족하고 앱 스토어에 업로드하고 런칭할 수 있도록 5가지의 질문과 응답 프롬프트를 제공하는 완벽한 SwiftUI iOS 앱 파일 세트를 만들어야 합니다.
즉, 한꺼번에 모든 코드를 제공하여 실행할 수 있어야 합니다. 매우 간단하고 사용하기 쉬워야 합니다.
앱의 이름은: 10ThingsTODO 입니다.
이것은 사용자가 최대 10가지 할 일 목록을 만들 수 있는 한 페이지 앱입니다.
사용자는 알림을 설정하고 해당 할 일의 남은 시간을 보여줄 수 있습니다.
제목은 자동으로 x개의 항목을 보여줍니다. 여기서 x는 할 일 항목의 개수를 의미합니다.
또한 사용자가 특정 마감일(사람들이 비워둘 수 있음)로 새 항목을 만들 수 있는 addthingview를 보여줍니다.
앱은 얼마나 많은 날이 남았는지 및 (1일 이하 또는 1시간 이하일 경우) 얼마나 많은 시간 또는 분이 남아 있는지를 계산할 것입니다.
앱은 Core Data를 사용해야 합니다.
제발 정보 설명도 제공해주세요.
앱 스토어에 런칭하기 위해 필요한 모든 것들을 설명, 해시태그 등도 작성해주세요.
내가 craft.io 페이지에 올릴 개인정보 보호 페이지의 기본 단어도 작성해 주세요.
로고 프롬프트도 작성해주세요. 단계별로 만들어 가며 가장 포괄적인 코드를 출력해봅시다.

# 핵심 기능

DDLTodo를 독특하게 만드는 요소에 대해 살펴보겠습니다:

# 1. 더 집중된 디자인

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![DDLTodo App Interface](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_3.png)

화면에서 보듯이, DDLTodo는 간결함을 중시합니다. 많은 할 일 앱이 복잡한 태그와 그룹을 사용하는 데 반해 DDLTodo는 "마감 기한"이 있는 작업을 위한 "DDL"과 특정 시간 제약이 없는 작업을 위한 "FREE" 두 가지 주요 섹션만 가지고 있습니다. 이 깔끔하고 혼란이 없는 디자인은 사용자가 한눈에 모든 작업을 볼 수 있도록 하여 인지 부하를 줄이고 진정으로 중요한 것에 집중할 수 있게 도와줍니다.

# 2. 순서화된 작업 관리와 비순서화된 작업 관리의 균형

이 앱의 디자인은 인간의 에너지가 제한적이며 모든 작업이 동등하지 않다는 점을 인정합니다. 마감 기한이 있는 작업(DDL)은 자동으로 맨 위에 배치되어 사용자가 즉시 긴급한 항목을 볼 수 있게 합니다. 이미지에서 "편지 쓰기"와 그 마감 기한 및 카운트다운이 눈에 띕니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

"FREE" 섹션에 마감 기한이 없는 작업들의 경우, 사용자들은 쉽게 드래그 앤 드롭으로 순서를 변경할 수 있습니다. 이 유연성은 현재 필요성과 에너지 수준에 기반하여 빠르게 우선순위를 정하는 데 도움이 됩니다.

### 3. 회한 메커니즘 통합

제공된 이미지에서는 보이지 않지만, DDLTodo에는 독특한 "회한 메커니즘"이 포함되어 있습니다. 첫 번째 버전에서는 사용자가 작업을 완료로 표시했을 때, 진행률 표시 막대와 진동 피드백이 주어져 사용자가 잠시 다시 고려할 수 있도록 했습니다. 완료된 작업은 보관되어 필요 시 복구할 수 있었습니다.

그러나 이 디자인이 다소 번거로운 것 같아요. 향후 업데이트에서 사용자가 마음을 바꿔 작업 완료를 되돌리기 위해 2~3초간 단순한 취소 버튼을 구현할 계획입니다."

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 4. 간단하지만 강력한 작업 생성

![task app screenshot](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_4.png)

"새로운 할 일 추가" 화면은 기능성을 희생하지 않고 간단함에 대한 앱의 헌신을 보여줍니다. 사용자들은 다음과 같이 할 일을 추가할 수 있습니다:

- 작업 이름 입력
- 마감일을 설정할지 여부 선택
- 마감일을 위한 특정 날짜와 시간 선택
- 리마인더 설정 (선택 사항으로 보입니다)

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이 단순화된 프로세스를 통해 새 작업을 손쉽게 추가할 수 있고 불필요한 세부 사항에 갇히지 않게 됩니다.

## 가장 큰 도전: CloudKit 통합

GPT-4와 Claude 3.5의 지침을 따라 데이터 동기화를 위해 CloudKit을 구성하고 사용하려고 노력했습니다. 그러나 CloudKit 디버깅이 예상보다 훨씬 복잡하다는 것을 알게 되었습니다. 이 프로세스는 너무 많은 로그 출력을 생성하여 문제를 정확히 파악하는 것을 어렵게 만들었습니다.

그 결과, 한 발 물러나기로 결정했습니다. 현재 계획은 간단한 독립형 CloudKit 데모를 작성하여 해당 기술을 탐색하고 디버깅하는 것입니다. 이 통제된 환경에서 클라우드 동기화를 성공적으로 구현한 후 이를 DDLTodo에 통합할 것입니다. 처음에 기대했던 것보다 더 많은 시간이 걸리지만, 안정적인 클라우드 동기화 기능을 최종적으로 달성하기 위한 최선의 방법이라고 생각합니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 최종 생각

DDLTodo는 앱 개발에서 흥미로운 실험을 대표하는 존재입니다. 그 만들어지는 프로세스에 인공지능이 사용되고, 그 디자인 철학에서도 그렇습니다. ToDo 앱은 독립적인 개발자들을 위한 세 가지 초기 프로젝트 중 하나로 여겨지고 있습니다(경비 추적 앱 및 일기앱과 함께), 그리고 그 세 가운데 가장 간단한 것으로 여겨지고 있습니다. 그런데 새로운 ToDo 앱들이 시장을 계속 덮치는 상황은 누구나 자기만의 연기와 생각 조직에 대한 유일무이한 고민을 갖고 있다는 것을 보여줍니다.

내 초기 의도는 간단했습니다. 더럽힌 책상을 정리하고 깨끗한 종이에 가장 중요한 작업들만 적는 행위를 모방하는 앱을 만드는 것이었습니다. 그래서 이 앱은 원래 10ThingsTODO라고 불렸죠. 그러나 우리 머리 위에 늘 떨어지는 마감 기한의 칼을 인식하면서, 시간에 쫒기는 작업은 항상 우선순위를 갖도록 했습니다.

![이미지](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_5.png)

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이야기가 Generative AI에 게재되었습니다. LinkedIn에서 저희와 연락하고 Zeniteq를 팔로우하여 최신 AI 이야기를 읽어보세요.

최신 뉴스와 AI 업데이트를 받으시려면 저희 뉴스레터를 구독해주세요. 함께 AI의 미래를 함께 만들어봐요!

![image](/assets/img/2024-07-13-ICreatedaDDLTodoAppwithClaude35Sonnetin5prompts_6.png)
