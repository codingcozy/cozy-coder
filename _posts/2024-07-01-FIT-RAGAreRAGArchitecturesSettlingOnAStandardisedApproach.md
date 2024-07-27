---
title: "FIT-RAG RAG 아키텍처가 표준화된 접근 방식을 채택하고 있는가"
description: ""
coverImage: "/assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_0.png"
date: 2024-07-01 00:01
ogImage: 
  url: /assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_0.png
tag: Tech
originalTitle: "FIT-RAG: Are RAG Architectures Settling On A Standardised Approach?"
link: "https://medium.com/@cobusgreyling/fit-rag-are-rag-architectures-settling-on-a-standardised-approach-47775acab1b1"
---


이미지 경로: [2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_0.png](/assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_0.png)

# 소개

기술이 발전함에 따라 흥미로운 점은 대부분의 사람들이 좋은 디자인으로 판단되는 것에 수렴한다는 것입니다.

예를 들어 프롬프트 엔지니어링의 경우, 프롬프트가 템플릿으로 진화되어 변수가 주입될 수 있는 자리 표시자가 있는 것을 볼 수 있습니다.

<div class="content-ad"></div>

이제 이것은 프롬프트 체이닝으로 진전되었고, 결국 여러 도구를 가진 자율 에이전트로 발전했습니다.

그래서 RAG는 매우 비슷한 궤적을 통과하고 있어요...초반에는 RAG 그 자체로 충분하다고 여겨졌어요. 그러나 이제 RAG 스택에 추가적인 지식이 추가되고, RAG 아키텍처의 일부를 형성하는 여러 요소들과 함께 구성될 예정이에요.

# 네 가지 초기 고려 사항

먼저, 아래 기사에서 보시는 대로 프롬프트 구조가 RAG 아키텍처에서 점점 더 중요해지고 있으며, 연상 기술로 Chain-of-Thought와 같은 프롬프팅 기술이 도입되고 있어요.

<div class="content-ad"></div>

최근에는 단순히 상황 참조 데이터를 주입하는 것만으로는 충분하지 않습니다. 텍스트를 최적화하기 위해 문구가 활용되고 있습니다.

또한 RAG가 두 가지 측면에서 정적임을 인식하고 있습니다. 첫 번째로, RAG가 대화의 맥락을 고려하지 않거나 최소한 다수의 대화 턴에 걸친 맥락을 고려하지 않을 수 있다는 것입니다. 이에 더해, 데이터를 검색할지 말지 결정하는 것은 종종 유연성이 없는 일련의 정적 규칙에 기반하고 있습니다.

세 번째로, 불필요한 오버헤드를 고려하고 있으며, 불필요하고 최적화되지 않은 검색, 추가 텍스트가 추가되어 원치 않는 비용 및 추론 지연을 초래할 수 있습니다.

네 번째로, 최적의 응답을 선택하기 위한 다단계 접근법과 분류기를 사용하거나 다수의 데이터 스토어를 활용하거나 단순히 사용자 요청을 분류하는 데 사용됩니다. 이러한 분류기는 주로 이 특수화된 작업을 위해 모델을 훈련하는 데 사용되는 주석이 달린 데이터에 의존합니다.

<div class="content-ad"></div>

그리고 이전에 언급한 대로, RAG는 라마인덱스가 에이젼틱 RAG로 지칭하는 상태로 나아가고 있습니다. 여기서 RAG 기반 요소는 여러 하위 에이전트나 도구를 활용하여 데이터 검색을 관리하는 데 사용됩니다.

## FIT-RAG

FIT-RAG 연구는 LLM과 사실 데이터를 고려할 때 두 가지 문제를 식별합니다...

사실 데이터 부족: LLM의 원하는 문서에는 특정 쿼리에 필요한 사실 정보가 부족할 수 있으며, 이는 검색기를 오도하고 블랙박스 RAG의 효과를 약화시킬 수 있습니다.

<div class="content-ad"></div>

Token Overload: 모든 검색된 문서를 무차별적으로 병합하면 LLM에 사용된 토큰이 과도하게 많아져서 블랙박스 RAG의 효율성이 저하됩니다.

FIT-RAG는 사실적 정보를 고려하여 이중 레이블 문서 점수 판별기를 고안하는 것으로, 이 점수 판별기는 사실적 정보와 LLM 선호도를 구분된 레이블로 통합합니다.

또한 FIT-RAG는 자기-인식 인지기와 하위 문서 수준 토큰 축소기를 포함한 토큰 축소 전략을 구현합니다. 이러한 혁신들은 불필요한 증강을 최소화하고 증강 토큰을 크게 감소시켜 FIT-RAG의 효율성을 향상시키기 위해 설계되었습니다.

# FIT-RAG의 구성요소

<div class="content-ad"></div>

아래 이미지를 고려하면 FIT-RAG는 다섯 가지의 핵심 요소로 구성되어 있습니다:

- 유사성 기반 검색기,
- 이중 레이블 문서 평가자,
- 이중 면 성 자기 인식 기능,
- 하위 문서 수준 토큰 축소기,
- 프롬프트 구성 모듈.

특히, 이중 레이블 문서 평가자는 LLM 기호와 사실적 정보 모두와의 조화를 능숙하게 포착하기 위해 설계되었으며, 사실적 무지의 위험을 경감시킵니다.

또한, 이중 면 성 자기 인식기와 하위 문서 수준 토큰 축소기는 입력 토큰 최소화에 중요한 역할을 하여 토큰 낭비를 방지합니다.

<div class="content-ad"></div>

The bi-label document scorer is trained using bi-label learning, which involves two labels:

- Factual information (Has_Answer) and
- LLM preference (LLM_Prefer).

The "Has_Answer" label tells us if the document contains the answer to the question, while "LLM_Prefer" indicates if the document helps the LLM generate an accurate response.

Yet, there's a noticeable disparity in data distribution between these labels, potentially affecting the efficiency of bi-label learning. To tackle this issue, the paper introduces a data-imbalance-aware bi-label learning technique.

<div class="content-ad"></div>

이 방법은 데이터에 서로 다른 가중치를 할당하며, 하이퍼 그레이디언트 디센트를 사용하여 자동으로 학습합니다. 이 접근 방식은 데이터 불균형 문제를 효과적으로 다루어서, 바이-라벨 문서 점수기가 검색된 문서를 종합적으로 평가할 수 있도록 합니다.

바이-파셋트 셀프-지식 인식기는 LLM이 외부 지식이 필요한지를 평가하여 두 가지 측면을 고려합니다: 질문이 장기적이거나 오래된 정보에 관련되는지 여부, 그리고 질문의 가장 가까운 대응이 자체 지식을 갖고 있는지 여부.

한편, 서브-문서 레벨의 토큰 축소기는 중복되는 서브-문서를 제거함으로써, 검색된 문서들 중 서브-문서가 적지만 여전히 LLM의 정확한 답변 제공 능력을 향상시킬 수 있는 조합들을 선택합니다.

# FIT-RAG 프롬프트하기

<div class="content-ad"></div>

아래 이미지는 프롬프트 문구가 최적화된 모습을 보여줍니다...

# 결론

RAG(견주-리더-생성자) 파이프라인에 주체적 능력을 통합하면 복잡한 질문과 추론 작업에 대처하는 능력이 크게 향상될 수 있습니다. 주체적 능력을 파이프라인에 추가함으로써, 더 넓은 범위의 복잡한 질문과 시나리오를 다룰 수 있게 됩니다.

그러나 주체적 능력을 가진 에이전트가 직면하는 중요한 도전 과제 중 하나는 의사 결정 프로세스에서 조작성 및 투명성이 부족하다는 것입니다. 사용자 쿼리에 직면했을 때, 에이전트는 생각의 연쇄나 계획 접근을 채택할 수 있으며, 이는 문제 공간을 효과적으로 탐색하기 위해 대형 언어 모델(Large Language Models, LLMs)과 반복 상호 작용이 필요할 수 있습니다.

<div class="content-ad"></div>

LLMs(대형 언어 모델)과의 반복 상호작용에 의존하는 것은 컴퓨팅 부담을 초래할 뿐만 아니라, 에이전트가 결정에 대해 명확한 설명을 제공하는 것을 방해합니다.

그 결과, 에이전트 시스템의 조종 가능성과 투명성을 향상시키는 메커니즘을 개발해야 하는 절박한 필요성이 제기됩니다. 이는 사용자가 그들의 행동을 더 잘 이해하고 영향을 미치도록 하는 것을 가능하게 할 것입니다.

이러한 고통을 해소함으로써, 에이전트 시스템의 효율성과 효과성을 향상시키는데 그치지 않고, 복잡한 작업 및 문제 해결 시나리오에 대한 사람과 AI 에이전트 간의 신뢰와 협력을 촉진할 것입니다.

⭐️ LinkedIn에서 대형 언어 모델의 업데이트를 받아보세요! ⭐️

<div class="content-ad"></div>

**이미지1**:  
![Image 1](/assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_1.png)

저는 현재 코어 AI의 최고 전도사입니다. 인공지능과 언어가 교차하는 모든 것에 대해 탐구하고 쓰고 있습니다. LLM, 챗봇, 음성 봇, 개발 프레임워크, 데이터 중심의 잠재 공간 등 다양한 주제를 다루고 있습니다.

**이미지2**:  
![Image 2](/assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_2.png)

**이미지3**:  
![Image 3](/assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_3.png)

<div class="content-ad"></div>

![Image](/assets/img/2024-07-01-FIT-RAGAreRAGArchitecturesSettlingOnAStandardisedApproach_4.png)