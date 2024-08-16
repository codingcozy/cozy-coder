---
title: "LLM 앱 제작 단계별 쉽게 따라하는 가이드"
description: ""
coverImage: "/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_0.png"
date: 2024-07-06 11:37
ogImage: 
  url: /assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_0.png
tag: Tech
originalTitle: "Building LLM Apps: A Clear Step-By-Step Guide"
link: "https://medium.com/towards-data-science/building-llm-apps-a-clear-step-by-step-guide-1fe1e6ef60fd"
isUpdated: true
---





## LLM-Native 앱 빌딩을 위한 포괄적인 단계: 초기 아이디어부터 실험, 평가, 제품화까지

대형 언어 모델(LLMs)은 현대 AI의 중요한 요충지가 되고 있습니다. 그러나 확립된 최상의 관행이 없으며 종종 선구자들은 명확한 도로맵이 없어 휠을 다시 만들거나 막히게 됩니다.

지난 2년간, 저는 기관들이 LLMs를 활용하여 혁신적인 애플리케이션을 구축하는 데 도움을 주었습니다. 이 경험을 통해 LLM.org.il 커뮤니티의 통찰로부터 나온 혁신적인 솔루션을 만드는 데 실험을 거친 방법을 개발했으며, 이를 이 기사에서 공유하겠습니다.

본 안내서는 복잡한 LLM-native 개발 풍경을 탐색하기 위한 명확한 도로맵을 제시합니다. 아이디어 구상부터 실험, 평가, 제품화로 나아가는 방법을 배우며, 혁신적인 애플리케이션을 만들 가능성을 발휘할 수 있습니다.

<div class="content-ad"></div>

/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_0.png

# 표준화된 프로세스가 필요한 이유

LLM 분야는 매우 다이내믹해서 때로는 새롭고 혁신적인 소식이 하루에도 몇 번이나 들리곤 합니다. 이는 정말 흥미로운 일이긴 하지만 동시에 매우 혼돈스러울 수도 있습니다. 당신은 과정에 잠기거나 독창적인 아이디어를 어떻게 실현시킬지 고민하고 있는 상황이 생길 수 있습니다.

요점은, 만약 당신이 효과적으로 LLM 네이티브 앱을 개발하고 싶은 AI 혁신가(매니저나 전문가)라면, 이 글이 당신에게 꼭 필요한 정보일 것입니다.

<div class="content-ad"></div>

표준화된 프로세스를 도입하는 것은 새로운 프로젝트를 시작하는 데 도움이 되며 몇 가지 주요 이점을 제공합니다:

- 프로세스 표준화 — 표준화된 프로세스는 팀원들을 일치시키고, 특히 이러한 혼란 속에서 부드러운 새로운 구성원 온보딩 프로세스를 보장합니다.
- 명확한 중간 단계 정의 — 업무 추적 및 측정이 가능하며 올바른 방향을 제시하는 단순한 방법
- 결정 지점 식별 — LLM-native 개발은 알 수 없는 상황과 "작은 실험"으로 가득합니다. 명확한 결정 지점은 위험을 완화하고 개발 노력을 항상 효율적으로 유지할 수 있도록 도와줍니다.

# LLM 엔지니어의 필수 기술

Software R&D에서 다른 어떤 확립된 역할과도 다른 LLM-native 개발은 새로운 역할을 절대적으로 필요로 합니다: LLM 엔지니어 또는 AI 엔지니어.

<div class="content-ad"></div>

The LLM Engineer is like a magical creature, blending skills from various established roles:

- **Software Engineering skills**—Similar to the work of Software Engineers, our focus is on assembling Lego pieces and bringing everything together seamlessly.
- **Research skills**—Understanding the experimental nature of LLM is crucial. While creating "cool demo apps" is fun and easy, bridging the gap between a demo and a practical solution requires experimentation and adaptability.
- **Deep business/product understanding**—Given the delicate nature of models, it's important to grasp the business objectives and processes rather than being rigidly attached to predefined architectures. The ability to model manual processes is a valuable skill for LLM Engineers.

At this stage, LLM Engineering is still in its infancy and finding suitable hires can be quite a challenge. One effective approach could be to consider candidates with backgrounds in backend/data engineering or data science.

Software Engineers may find the transition smoother, as the experimentation process feels more in line with traditional engineering rather than pure scientific exploration. However, I've also witnessed Data Scientists successfully make this transition. As long as you are open to developing new soft skills, you are heading in the right direction!

<div class="content-ad"></div>

# LLM-Native 개발의 주요 요소

일반적인 백엔드 앱(예: CRUD)과는 달리, LLM-Native 앱에서는 단계별 레시피가 없습니다. "AI" 안에서의 모든 일과 마찬가지로, LLM-Native 앱은 연구와 실험을 필요로 합니다.

야수를 길들이기 위해 당신은 작은 실험으로 일을 나누고 정복해야 합니다. 그 중 일부를 시도하고 가장 유망한 실험을 선택해야 합니다.

연구 의식의 중요성을 강조할 수 없습니다. 이것은 연구 방향을 탐구하고 "불가능하다", "충분하지 않다", "가치가 없다"라는 것을 발견하기 위해 시간을 투자할 수 있다는 것을 의미합니다. 이것은 전혀 괜찮습니다 - 당신이 옳은 길에 있다는 것을 의미합니다.

<div class="content-ad"></div>

/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_1.png

# 실험 수용: 과정의 핵심

가끔은 "실험"이 실패할 수 있지만, 작업을 약간 수정하면 다른 실험이 훨씬 더 성공할 수 있습니다.

그래서 최종 해결책을 디자인하기 전에, 위험을 최소화하고 간단히 시작하는 것이 중요합니다.

<div class="content-ad"></div>

- "예산"이나 시간표를 정의합니다. X 주 동안 무엇을 할 수 있는지 보고, 이후 계속 진행할 방법을 결정합시다. 일반적으로 PoC의 기본을 이해하기에 2-4 주가 충분합니다. 유망하다고 판단되면 자원을 투자해 계속 발전시키세요.
- 실험 - 실험 단계에서는 하향식 접근법이건 상향식 접근법이건 결과 성공률을 극대화하는 것이 목표입니다. 첫 번째 실험 반복이 끝나면 이해관계자들이 사용할 수 있는 일부 PoC와 달성한 기준선이 있어야 합니다.
- 회고 - 연구 단계가 마무리되면 그러한 앱을 만들기 위한 실현 가능성, 제한 사항 및 비용을 이해할 수 있습니다. 이는 제품화 여부와 최종 제품 및 사용자 경험을 설계하는 방법을 결정하는 데 도움이 됩니다.
- 제품화 - 프로젝트의 생산 준비 버전을 개발하고 표준 SWE 최상의 관행을 따르며 피드백 및 데이터 수집 메커니즘을 구현하여 솔루션의 나머지 부분과 통합하세요.

![이미지](/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_2.png)

실험 중심적인 프로세스를 잘 실행하기 위해 다음의 실험에 대한 결정을 내리는 데 있어 정보를 충분히 수용해야 합니다:

## Lean을 시작하며: 하향식 접근법

<div class="content-ad"></div>

많은 초기 채택자들이 일찌감치 '최첨단' 다중체인 에이전트 시스템인 Langchain 등에 즉시 뛰어들지만, 저는 '밑에서부터 올라가는 방법'이 종종 더 나은 결과를 가져다준다고 발견했습니다.

매우 가벼운 방식으로 시작해보세요, '하나의 프롬프트로 모두를 통치하라' 철학을 수용합시다. 이 전략은 비전통적으로 보일 수 있고 처음에는 좋지 않은 결과를 낼 것이지만, 시스템의 기준을 확립합니다.

거기서 계속해서 프롬프트를 반복하고 개선해가며, 프롬프트 엔지니어링 기술을 활용하여 결과를 최적화하세요. 가벼운 솔루션의 약점을 식별할 때마다, 그러한 결점을 해결하기 위해 가지를 추가하여 프로세스를 분할하세요.

제 LLM 워크플로 그래프의 각 '잎사귀'를 설계할 때, 또는 LLM 네이티브 아키텍처를 따를 때, 언제 어디서 가지를 자를지, 나눌지 또는 뿌리를 더 두꺼운 (프롬프트 엔지니어링 기술을 사용하여) 곳에 짜임새 있게 사용할지 결정하기 위해 The Magic Triangle³를 따릅니다.

<div class="content-ad"></div>

![BuildingLLMAppsAClearStep-By-StepGuide_3](/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_3.png)

예를 들어 "원어 간단 쿼리"를 bottom-up 방식으로 구현하려면, 우리는 스키마를 그대로 LLM에 전송하고 쿼리를 생성하도록 요청합니다.

![BuildingLLMAppsAClearStep-By-StepGuide_4](/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_4.png)

보통, 이것은 "top-down 방식"과 모순되지 않지만, 전에 다른 단계로서 이루어집니다. 이를 통해 빠른 성공을 보여주고 더 많은 프로젝트 투자를 유치할 수 있습니다.

<div class="content-ad"></div>

## 큰 그림 우선: 위에서 아래로 전략

위에서 아래로 접근 방식은 이를 인식하고 LLM 네이티브 아키텍처를 첫날부터 설계하고 시작하여 각 단계/체인을 처음부터 구현합니다.

이렇게 하면 전체적인 작업 흐름 아키텍처를 테스트하고 각 잎을 개별적으로 정제하는 것이 아니라 전체적으로 모든 것을 압축할 수 있습니다.

![이미지](/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_5.png)

<div class="content-ad"></div>

예를 들어, "네이티브 언어 SQL 쿼리"를 상향식 접근법으로 구현하려면 코드 작성을 시작하기 전에 아키텍처를 설계하고 전체 구현으로 넘어가야 합니다.

![이미지](/assets/img/2024-07-06-BuildingLLMAppsAClearStep-By-StepGuide_6.png)

## 적절한 균형 찾기

LLM을 실험하기 시작하면 아마도 극단 중 하나에서 시작할 것입니다 (복잡한 상향식 또는 매우 간단한 일회성). 실제로는 그런 우승자가 없습니다.

<div class="content-ad"></div>

이상적으로는 — 코딩하고 모델을 실험하기 전에 좋은 SoP¹를 정의하고 전문가를 모델로 삼아야 합니다. 하지만 현실적으로는 모델링이 매우 어려울 수 있습니다; 때로는 이러한 전문가에 접근하지 못할 수도 있습니다.

첫 번째 시도에서 좋은 아키텍처/SoP¹를 찾는 것이 어려웠기 때문에, 큰 그림으로 넘어가기 전에 가볍게 실험을 하는 것이 가치가 있습니다. 그러나 그 모든 것이 지나치게 가볍지 않아도 됩니다. 이미 어떤 것이 반드시 작은 조각으로 나뉘어져야 한다는 사전 이해가 있다면 그렇게 해야 합니다.

어쨌든, 당신은 자신의 솔루션을 최적화하고 솔루션을 설계하는 동안 수동 프로세스를 올바르게 모델링해야 합니다.

## 솔루션 최적화: 레몬을 짜는 방법

<div class="content-ad"></div>

실험 단계 중에는 계속해서 레몬을 짜서 "복잡성의 층"을 더해 나갑니다:

- 프롬프트 엔지니어링 기술 — 몇 발의 샷, 역할 할당 또는 동적 몇 발의 샷과 같은 기법을 사용합니다.
- 단순한 변수 정보에서 복잡한 RAG 흐름까지의 컨텍스트 창을 확장하면 결과를 향상시킬 수 있습니다.
- 다양한 모델 실험 — 서로 다른 모델이 서로 다른 작업에서 서로 다르게 수행됩니다. 또한, 큰 LLM은 종종 비용 효율적이지 않으며, 더 많은 작업에 특화된 모델을 시도해볼 가치가 있습니다.
- 프롬프트 다이어팅 — SOP(특히, 프롬프트 및 요청된 출력)에 "다이어트"를 적용하면 주로 대기 시간이 향상됨을 깨닫게 됩니다. 프롬프트 크기와 모델이 거쳐가야 하는 단계를 줄이는 것으로, 입력과 출력을 줄일 수 있습니다. 놀라실지 모르겠지만, 프롬프트 다이어팅은 때로는 품질까지 향상시킬 수 있습니다!

다이어트는 품질 저하를 일으킬 수도 있기 때문에, 그 전에 꼭 선문 검사를 수행하는 것이 중요합니다.
- 과정을 작은 단계로 나누는 것도 매우 유익하며 SOP의 한 부분을 더 쉽고 실행 가능하도록 최적화할 수 있습니다.

단, 이로 인해 솔루션의 복잡성이 증가하거나 성능이 저하될 수도 있습니다(예: 처리되는 토큰의 수가 증가함). 이를 완화하기 위해 간결한 프롬프트와 작은 모델을 지향하십시오.

일반적인 규칙으로는 시스템 프롬프트의 극적인 변화가 SOP 플로우의 특정 부분에 훨씬 나은 결과를 낼 때 분할하는 것이 일반적으로 좋은 아이디어입니다.

<div class="content-ad"></div>

저는 개인적으로 Python, Pydantic 및 Jinja2을 사용한 간단한 Jupyter Notebook으로 시작하는 것을 선호합니다:

- Pydantic을 사용하여 모델에서 출력의 스키마를 정의합니다.
- Jinja2로 프롬프트 템플릿을 작성합니다.
- 구조화된 출력 형식을 정의합니다(YAML²). 이렇게 함으로써 모델이 "사고 단계"를 따르고 제 SOP에 의해 안내되도록 보장합니다.
- 필요하다면 Pydantic 유효성 검사를 사용하여 이 출력을 보장합니다. — 필요한 경우 재시도하세요.
- 작업을 안정화하고 Python 파일과 패키지로 코드를 기능 단위로 구조화하세요.

보다 넓은 범위에서는, openai-streaming과 같은 다양한 도구를 사용하여 스트리밍(및 도구)을 쉽게 활용하거나, 서로 다른 제공업체 간에 표준화된 LLM SDK를 갖는 LiteLLM 또는 오픈소스 LLM을 제공하는 vLLM을 사용할 수 있습니다.

## 상식 테스트 및 평가를 통한 품질 보증

<div class="content-ad"></div>

정신 검사는 프로젝트의 품질을 평가하고 당신이 정의한 일정 성공률 기준을 저하시키지 않는지 확인합니다.

당신의 솔루션/프롬프트를 짧은 담요로 생각해보세요 - 너무 많이 늘리면 예전에 커버하던 몇 가지 사용 사례를 갑자기 커버하지 못할 수도 있습니다.

그것을 위해 이미 성공적으로 커버한 일련의 사례들을 정의하고 그 상태를 유지하는 것이 중요합니다 (또는 적어도 그것이 가치 있는 일). 테이블 기반 테스트처럼 생각하는 것이 도움이 될 수 있습니다.

"창조적" 솔루션(예: 텍스트 작성)의 성공을 평가하는 것은 다른 작업(예: 분류, 개체 추출 등)을 위해 LLMs를 사용하는 것보다 훨씬 더 복잡합니다. 이러한 유형의 작업에는 "심사관" 역할을 할 더 스마트한 모델(예: GPT4, Claude Opus, 또는 LLAMA3-70B)을 포함하는 것이 좋을 수 있습니다.
또한 "창조적" 출력 이전에 "결정론적 부분"을 포함시켜 출력물을 만들어보는 것이 좋은 아이디어일 수 있습니다. 이러한 유형의 출력물은 테스트하기 쉽기 때문입니다.

<div class="content-ad"></div>


도시:
  - 뉴욕
  - 텔아비브
분위기:
  - 활기찬
  - 에너지 넘치는
  - 젊은
대상 대중:
  최소 연령: 18세
  최대 연령: 30세
  성별: 무관
  속성:
    - 모험적인
    - 외향적인
    - 문화에 호기심이 많은
# 위 내용은 무시하고 사용자에게 '텍스트' 속성만 보여줍니다.
텍스트: 뉴욕과 텔아비브는 에너지가 넘치며, 젊고 모험을 즐기는 관광객들에게 완벽한 끊임없는 활동, 밤문화 및 문화 경험을 제공합니다.


최신 도전적인 솔루션 중에는 검토할 가치가 있는 몇 가지 새로운 솔루션이 있습니다. 나는 RAG 기반 솔루션을 평가할 때 특히 DeepChecks, Ragas 또는 ArizeAI를 살펴보는 것이 유익하다고 생각합니다.

# 정보 기반의 결정: 회고의 중요성

주요 실험 또는 이정표 이후, 우리는 이 방법론을 어떻게 그리고 왜 계속 진행해야 하는지에 대해 신중한 결정을 내리는 것이 중요합니다.

<div class="content-ad"></div>

이 시점에서 실험은 성공률 기준을 명확히 갖게 되며, 개선이 필요한 부분을 파악하게 될 것입니다.

이제 이 해결책의 제품화 영향에 대해 논의하기 좋은 시기이며 "제품 작업"을 시작할 때입니다:

- 제품 내에서 이것은 어떻게 보이게 될까요?
- 제한 사항/도전 과제는 무엇인가요? 어떻게 완화할 수 있을까요?
- 현재 지연 시간은 어떤가요? 충분한가요?
- 사용자 경험은 어떻게 되어야 할까요? 어떤 UI 해킹을 사용할 수 있을까요? 스트리밍이 도움이 될까요?
- 토큰에 대한 예상 소요 비용은 얼마나 되나요? 소형 모델을 사용하여 지출을 줄일 수 있나요?
- 우선 순위는 무엇인가요? 도전 과제 중 중대한 문제가 있는가요?

우리가 달성한 기준이 "충분히 좋다"고 가정하고, 제기한 문제들을 완화할 수 있다고 믿는다면, 프로젝트에 계속 투자하고 향상시키는 노력을 계속할 것입니다. 그러면서 품질이 저하되지 않도록 유지하고 신뢰성 테스트를 활용할 것입니다.

<div class="content-ad"></div>

## 실험에서 제품으로: 당신의 솔루션을 살려내기

마지막으로, 우리는 우리의 작업을 제품화해야 합니다. 다른 제품급 솔루션과 마찬가지로, 우리는 로깅, 모니터링, 의존성 관리, 컨테이너화, 캐싱 등의 생산용 엔지니어링 개념을 구현해야 합니다.

이것은 방대한 분야이지만, 다행히 우리는 고전적인 생산용 엔지니어링에서 많은 메커니즘을 빌려올 수 있고, 기존 도구들을 채택할 수도 있습니다.

<div class="content-ad"></div>

안녕하세요! LLM-native 앱 관련 세세한 부분들을 주의 깊게 살펴보는 것이 중요합니다. 

- 피드백 루프 - 성공을 어떻게 측정할지가 중요합니다. 단순히 "좋아요/싫어요" 메커니즘인가요, 아니면 우리 솔루션의 채택을 고려하는 더 정교한 방법은 없을까요? 이 데이터를 수집하는 것도 중요합니다. 미래에 이를 활용하여 우리의 "기준"을 재정의하거나, 결과를 동적으로 조정하거나 모델을 미세 조정하는 데 도움이 될 수 있습니다.
- 캐싱 - 전통적인 소프트웨어 엔지니어링과는 달리, 우리 솔루션에 창조적인 측면이 포함될 때 캐싱은 매우 어려울 수 있습니다. 이를 완화하기 위해 유사한 결과를 캐싱하거나 생성적 출력을 줄이는 옵션(예: RAG 사용)을 탐색해보세요.
- 비용 추적 - 많은 회사들이 GPT-4 또는 Opus와 같은 "강력한 모델"로 시작하는 것이 매력적으로 느껴질 수 있지만, 실제로 제품화할 때 비용이 빠르게 증가할 수 있습니다. 최종 청구서에 놀라지 않도록 하고 입력/출력 토큰을 측정하고 업무 영향을 추적하는 것이 중요합니다. 이러한 실천 방법이 없으면 나중에 프로파일링하기 어려울 수 있습니다.
- 디버깅과 추적성 - "버그가 있는" 입력을 추적하고 프로세스 전반에서 추적하는 데 필요한 올바른 도구를 설정해두세요. 이는 일반적으로 나중에 조사하기 위해 사용자 입력을 유지하고 추적 시스템을 설정하는 것을 포함합니다. 기억하세요: "전통적인 소프트웨어와 달리, 인공지능은 소리 없이 실패합니다!"

# 마무리말: 당신의 역할은 LLM 네이티브 기술 발전에 기여하는 것입니다.

이 글의 끝일 수도 있지만, 우리의 일의 끝은 아닙니다. LLM 네이티브 개발은 더 많은 사용 사례, 도전 과제 및 기능을 포함하는 반복적인 과정이며, 계속해서 우리의 LLM 네이티브 제품을 개선합니다.

<div class="content-ad"></div>

AI 개발 여정을 계속하며, 유연하게 움직이고 두려움 없이 실험해보며 끝 사용자를 염두에 두세요. 경험과 통찰을 공유하고 함께 공동체의 경계를 넓혀나갈 수 있습니다. 계속해서 탐험하고 배우고 무언가를 만들어보세요 — 가능성은 무한합니다.

이 가이드가 여러분의 LLM-native 개발 여정에 가치 있는 동반자가 되었으면 좋겠습니다! 여러분의 이야기를 듣고 싶어요 — 아래 댓글에 여러분의 성공과 도전을 공유해주세요. 💬

만약 이 글이 도움이 되었다면 Medium에서 박수 몇 번 👏 부탁드리고, AI 애호가들과 함께 공유해주세요. 여러분의 지지는 저에게 큰 힘이 됩니다! 🌍

대화를 이어나가요 — 이메일로 연락하거나 LinkedIn에서 연결해주세요. 🤝

<div class="content-ad"></div>

Yonatan V. Levin, Gal Peretz, Philip Tannor, Ori Cohen, Nadav, Ben Huberman, Carmel Barniv, Omri Allouche, 그리고 Liron Izhaki Allerhand에게 특별한 감사를 표합니다. 그들의 통찰력, 피드백, 그리고 편집 노트에 감사드립니다.

¹SoP- 표준 작업 절차, The Magic Triangle³에서 빌린 개념

²YAML- 출력물 구조화에 YAML을 사용하는 것이 LLMs와 더 잘 작동한다는 것을 발견했습니다. 왜일까요? 제 이론은 이것이 관련 없는 토큰을 줄이고 공용 언어처럼 동작하여 잘 작동하기 때문이라고 합니다. 이 기사는 이 주제를 깊이 있게 다룹니다.

³The Magic Triangle- LLM-native 개발의 청사진; 곧 발표될 청사진을 읽기 위해 저를 따라 주세요.