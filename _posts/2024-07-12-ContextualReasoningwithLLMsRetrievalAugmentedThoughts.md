---
title: "LLMs을 활용한 문맥적 추론 검색 보강 사고 방법"
description: ""
coverImage: "/assets/img/2024-07-12-ContextualReasoningwithLLMsRetrievalAugmentedThoughts_0.png"
date: 2024-07-12 23:19
ogImage: 
  url: /assets/img/2024-07-12-ContextualReasoningwithLLMsRetrievalAugmentedThoughts_0.png
tag: Tech
originalTitle: "Contextual Reasoning with LLMs: Retrieval Augmented Thoughts"
link: "https://medium.com/ai-in-plain-english/contextual-reasoning-with-llms-retrieval-augmented-thoughts-b3c59a90eb1f"
isUpdated: true
---





인공 지능 소프트웨어가 이 기사의 텍스트의 문법, 흐름 및 가독성을 향상시키는 데 사용되었습니다.

대형 언어 모델(LLM)은 다양한 응용 프로그램에서 인간과 유사한 텍스트를 이해하고 생성하는 데 놀라운 성공을 거두었습니다. 문맥을 파악하고 일관성 있고 유창한 응답을 생성하는 능력으로 많은 자연 언어 처리 분야를 변혁시켰습니다.

그러나 과제의 복잡성이 증가함에 따라 LLM은 종종 논리적 일관성, 사실적 정확성 및 강력한 추론을 여러 단계에 걸쳐 유지하기 어려워합니다. 이 한계는 모델이 환각을 일으키거나 사실적 지식과 일치하지 않는 결과물을 생성하는 경향에서 비롯되는데, 특히 다단계 추론, 긴 문맥 또는 전문 분야를 다룰 때 발생합니다.

이러한 문제를 완화하고 LLM의 전체 잠재력을 발휘하기 위해 연구자들은 모델의 본질적 능력을 외부 지식 검색 및 구조화된 추론 과정과 함께 보완하기 위한 기술을 탐색해 왔습니다. 그 중 하나가 희망이 붙어 있는 접근법은 검색 기반 강화된 생각(RAT)입니다.

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-12-ContextualReasoningwithLLMsRetrievalAugmentedThoughts_0.png)

## LLMs를 위한 생각의 개념

언어 모델의 맥락에서 "생각"은 주어진 문제나 과제를 해결하는 과정에서의 의미적 단위나 단계를 나타냅니다. 다양한 형태로 나타날 수 있는데요, 예를 들면:

- 추론 단락 안의 문장
- 글을 쓸 때의 계획이나 텍스트 일부
- 요약할 정보나 문서 세트
- 수학적 추론에서의 숫자나 작업 순서

<div class="content-ad"></div>

이러한 생각들은 서로 연결되어 있으며, 각 단계는 이전 단계를 기반으로하여 이해 구조나 흐름을 형성합니다.

이러한 이해 구조는 그래프로 모델화할 수 있으며, 여기서 노드는 개별적인 생각을 나타내고, 엣지는 그들 사이의 의존성이나 관련성을 나타냅니다.

**검색 보강된 생각의 힘**

RAT는 검색 보강 생성과 사고의 체인 (CoT) 추론 두 가지 핵심 요소를 협동하는 프레임워크를 소개합니다.

<div class="content-ad"></div>

핵심 아이디어는 LLM의 다단계 추론 프로세스를 강화하고 근거를 제공하기 위해 외부 지식 소스를 활용하는 것입니다. 이는 환각과 사실 불일치의 위험을 줄입니다.

RAT 프레임워크는 두 단계로 운영됩니다:

- 초기 사고 연쇄 생성: LLM은 주어진 작업 프롬프트를 기반으로 초기 사고 연쇄를 생성하여 문제를 해결하기 위한 제로샷 시도를 나타냅니다.
- 검색과 반복적 개선: 그런 다음 프레임워크는 각 사고 단계를 반복적으로 개선합니다:

  - 작업 프롬프트, 현재 사고 단계 및 이전 수정된 단계를 기반으로 쿼리를 작성
  - 구성된 쿼리를 사용하여 외부 지식 원본(예: 위키백과, 도메인별 데이터베이스)에서 관련 정보를 검색
  - 검색된 정보를 기반으로 현재 사고 단계를 개선하고 수정된 버전을 생성합니다.

<div class="content-ad"></div>

외부 지식 검색을 추론 프로세스에 통합함으로써 RAT는 LLM의 사고를 사실적인 정보에 바탕을 두어 환각 가능성을 줄이고 생성된 결과물의 전반적인 품질과 일관성을 향상시킵니다.

이 접근 방식은 정보 검색을 증진한 생성(사실적 맥락 제공)과 CoT 추론(사고과정을 구조화)의 강점을 결합하여 LLM의 다단계 추론 능력을 향상시키는 강력한 시너지를 얻습니다.

## 다단계 추론 기법과 RAT

LLM을 다단계 추론 프로세스를 안내하는 여러 기법이 개발되었습니다.

<div class="content-ad"></div>

Chain-of-Thought (CoT) Prompting: 이 접근 방식은 입력과 출력 사이에 명시적인 중간 "추론 단계"를 소개하여 LLM이 최종 해결책에 도달할 수 있도록 개략적인 사고 과정을 안내합니다.

Tree-of-Thought (ToT): ToT는 추론 과정에서 가지치기를 허용하여 LLM이 다중 잠재적 해결책이나 하위 문제를 동시에 탐색할 수 있도록 합니다. 트리의 각 노드는 부분 해결책 또는 "사고"를 나타내며, LLM은 이러한 사고들을 평가하여 추구할 가장 유망한 경로를 결정합니다.

Graph-of-Thought (GoT): GoT는 가장 복잡한 토폴로지를 나타내며, 사고 간 임의적인 추론 종속성을 가능하게 합니다. 이는 여러 추론 단계를 하나의 해결책으로 통합할 수 있어 비선형 및 다면적 문제 해결 방식을 수용합니다.

## RAT 및 Tree-of-Thought (ToT):

<div class="content-ad"></div>

RAT은 Tree-of-Thought (ToT) 방법론과 결합하여 동시에 여러 가지 추리 경로를 탐색할 수 있습니다.

RAT은 일련의 CoT를 정제하는 대신, ToT 구조 내 각 노드(사고)를 세밀화하여 관련 정보를 검색하고 해당 사고의 세부 버전을 생성할 수 있습니다.

이를 통해 LLM은 각 추리 경로를 외부 지식에 근거하여 넓은 범위의 잠재적인 해결책을 탐색할 수 있습니다.

## RAT과 Graph-of-Thought (GoT):

<div class="content-ad"></div>

The Graph-of-Thought (GoT) topology embodies a highly intricate network of interconnected thoughts, featuring unbounded relationships between different ideas.

When coupled with the Retrieval-Augmented Transformation (RAT) method, the GoT framework excels at fetching pertinent information for each thought node within the network, empowering the Logical Level Machine (LLM) to weave together diverse reasoning processes into a coherent and well-founded solution.

By engaging in the retrieval mechanism, potential conflicts or inconsistencies stemming from the non-linear progression of reasoning can be ironed out, ensuring that the ultimate solution remains grounded in facts and sound logic.

By melding the organized reasoning structures of techniques like CoT, ToT, and GoT with RAT's capacity for external knowledge retrieval, LLMs stand to gain from a collaborative approach that harnesses both internal expertise and external factual insights.

<div class="content-ad"></div>

**자가 발견과 함께 추론 과정을 증진시키기**

자가 발견 접근 방식은 검색 증가된 사상(RAT)과 매우 상호 보완적이며, 전반적인 프레임워크에 통합하여 더욱 높은 능력을 갖게 할 수 있습니다.

<div class="content-ad"></div>

이탈릭체있는TEXT를입력할수있습니다. RAT는 초기 사고 연결과정(CoT)을 생성한 다음 외부 지식 원본에서 검색된 정보를 사용하여 각 사고 단계를 반복적으로 정제하는 방식으로 작동됨을 기억해 주세요. 이 과정을 통해 LLM의 추론이 사실적인 정보에 근거하여 현실감을 유지하고 모순을 완화합니다.

SELF-DISCOVER는 이 과정에 적응성과 과제에 특화된 최적화 수준을 소개합니다. LLM이 스스로 발견하고 각 과제에 맞게 최적화된 추론 구조를 작성할 수 있도록 함으로써, SELF-DISCOVER는 RAT 프레임워크의 효율성과 효과성을 향상시킬 수 있습니다.

맞춤형 추론 구조: 선형 CoT나 미리 정의된 트리/그래프 구조와 같은 고정된 추론 토폴로지에 의존하는 대신, SELF-DISCOVER는 LLM이 현재 작업에 최적화된 추론 계획을 작성할 수 있도록 하여, 문제의 본질적 성격과 복잡성을 더 잘 포착할 수 있어 더 효율적이고 정확한 추론을 이끌어낼 수 있습니다.

적응형 추론 모듈: SELF-DISCOVER는 과제 요구에 따라 선택, 조정, 조합할 수 있는 일련의 원자적 추론 모듈(예: "하위 작업으로 분해", "비판적 사고 사용")을 활용합니다. 이 모듈은 LLM이 문제에 가장 적합한 방식으로 조립할 수 있는 구성 요소로 볼 수 있어, 고정된 추론 토폴로지에 비해 큰 유연성과 맞춤화를 제공합니다.

<div class="content-ad"></div>

발견된 이유 구조는 SELF-DISCOVER에 의해 발견되었으며 RAT 프레임워크에 통합될 수 있습니다. 미리 결정된 CoT 또는 트리/그래프 구조를 개선하는 대신, RAT는 SELF-DISCOVER가 구성한 맞춤형 이유 구조를 개선하며, 계획의 각 단계에서 검색된 정보를 통합할 수 있습니다.

상호보완적 추론과 검색: SELF-DISCOVER의 적응형 추론 구조와 RAT의 외부 지식 검색 능력을 결합함으로써, 전체 프레임워크는 상호보완적 접근법에서 혜택을 받을 수 있습니다. 검색된 정보는 추론 프로세스를 안내하고 개선할 수 있으며, 맞춤형 추론 구조는 검색 프로세스를 이끌어, 각 단계에서 가장 관련성 있는 정보를 검색하도록 보장할 수 있습니다.

이렇게 SELF-DISCOVER와 RAT의 통합은 더욱 종합적이고 효과적인 추론 시스템으로 이끌 수 있습니다. SELF-DISCOVER는 유연성과 과제별 최적화를 제공하며, RAT은 외부 지식 소스에서 사실적인 근거와 맥락을 제공합니다. 둘을 함께 사용하면 LLM의 복잡한 작업을 효율적이고 정확하게 처리하며, 일관성과 일치성이 더 높아질 수 있습니다.

게다가, SELF-DISCOVER에 의해 발견된 추론 구조의 해석 가능성은 LLM의 사고 과정을 이해하고 분석하는 데 도움이 되며, 인간-인공지능 협력을 촉진하고 더 투명하고 신뢰할 수 있는 추론 시스템을 가능하게 할 수 있습니다.

<div class="content-ad"></div>

## 효율적 추론을 위한 몬테카를로 트리 탐색

몬테카를로 트리 탐색 (MCTS)을 검색 증진된 생각 (RAT) 및 SELF-DISCOVER와 통합하면, LLMs를 복잡한 추론 작업을 안내하기 위한 강력하고 효율적인 프레임워크로 이끌 수 있습니다. MCTS는 잠재적 추론 경로의 방대한 공간을 탐색하는 원칙적 접근을 제공하여 RAT과 SELF-DISCOVER의 강점을 보완합니다.

검색 트리로서의 추론 토폴로지:

RAT과 SELF-DISCOVER 프레임워크에서, 추론 과정은 상호 연결된 생각들의 토폴로지(체인, 트리 또는 그래프)로 구성됩니다.

<div class="content-ad"></div>

**탐험을 이끄는 추론 경로:**

MCTS는 각 노드가 생각을 나타내고 간선이 생각 사이의 전이를 나타내는 탐색 트리로 취급할 수 있습니다.

MCTS는 새로운 경로를 탐험하면서 유망한 경로를 활용하는 것을 균형있게 제공하며 추론의 위상을 탐색하는 체계적이고 효율적인 방법을 제공합니다.

모든 가능한 추론 경로를 철저하게 평가하는 대신, MCTS는 시뮬레이션 결과 및 점수 기능을 기반으로 가장 유망한 궤적에 집중할 수 있습니다.

<div class="content-ad"></div>

**검색과 추론 통합**

MCTS의 선택 및 확장 단계는 RAT의 검색 프로세스에 의해 안내될 수 있습니다. 외부 지식을 검색하여 새로운 생각 생성에 도움을 주는 과정입니다.

검색된 정보는 다양한 추론 경로의 잠재력을 평가하는 데 사용될 수 있으며, MCTS가 내린 탐색 및 활용 결정에 영향을 줄 수 있습니다.

융통성 있는 추론 구조:

<div class="content-ad"></div>

**자가발견(Self-Discover)**은 MCTS 탐사 과정의 초석 역할을 하는데 활용될 수 있어요.

MCTS가 추론 공간을 탐색하면서 SELF-DISCOVER에 피드백을 제공하여, 시뮬레이션 결과에 기반해 추론 구조를 다듬고 수정할 수 있어요.

효율적인 추론과 채점:

MCTS는 평가 함수나 보상 모델을 활용하여 다양한 추론 경로의 품질과 잠재력을 효율적으로 평가할 수 있어요.

<div class="content-ad"></div>

**점수 기능은 이유 과정의 원하는 속성을 포착하기 위해 학습하거나 설계될 수 있습니다. 예를 들어 사실적 정확성, 논리 일관성 또는 작업별 목표 등이 있습니다.**

**병렬화와 확장성:**

**MCTS는 병렬화에 적합하여 여러 시뮬레이션이 동시에 진행될 수 있어 이유 공간을 효율적으로 탐색할 수 있습니다.**

**이 확장성은 복잡한 이유 작업이나 대규모 이유 토폴로지를 다룰 때 특히 유용할 수 있습니다.**

<div class="content-ad"></div>

MCTS와 RAT, 그리고 SELF-DISCOVER을 결합함으로써 얻어지는 프레임워크는 각 구성 요소의 강점을 활용할 수 있습니다:

- RAT은 외부 지식 검색을 통해 사실적인 기초와 맥락을 제공합니다.
- SELF-DISCOVER는 적응형이자 과제별 추론 구조를 구성할 수 있게 해줍니다.
- MCTS는 추론 공간을 효율적으로 탐색하도록 안내하며, 검색된 정보와 점수화 기능을 활용하여 탐사와 개척을 균형있게 조절합니다.

<div class="content-ad"></div>

이 상호작용적인 방식은 LLMs에 대한 더 효율적이고 정확하며 확장 가능한 추론 시스템을 이끌어냅니다. 이러한 시스템은 복잡한 작업에 대처하면서도 일관성, 사실적 정확성, 그리고 여러 단계에서의 논리 일관성을 유지할 수 있습니다.

## 지식 그래프: RAT의 이상적인 검색 소스

RAT 프레임워크는 다양한 외부 지식 소스를 활용할 수 있지만, 지식 그래프는 특히 이 작업에 적합하게 만들어진 독특한 장점을 제공합니다:

구조화된 지식 표현: 지식 그래프는 지식을 구조화되고 의미적으로 풍부하게 표현하여 엔티티, 그들의 속성, 그리고 이들 간의 관계를 인코딩합니다. 이 구조화된 형식은 비구조화된 텍스트 소스와 비교하여 더 정확하고 명확하게 검색할 수 있도록 지원합니다.

<div class="content-ad"></div>

논리 제약 조건과 추론: 그래프 구조와 명시적인 관계 표현은 논리적 질의와 제약 조건을 만들어낼 수 있습니다. 이 능력은 RAT 시스템이 특정 조건을 기반으로 논리적 추론과 검색을 수행하도록 허용하는데, 예를 들어 현재의 사고에 언급된 다른 개체들과의 특정 속성 또는 관계를 가진 개체를 찾는 등의 작업이 가능합니다.

효율적인 그래프 순회 알고리즘: 지식 그래프는 효율적인 순회와 검색을 위해 잘 알려진 그래프 알고리즘과 데이터 구조를 활용할 수 있습니다. 이러한 알고리즘은 명시된 논리 제약 조건과 일치하는 관련 개체 및 서브그래프를 빠르게 식별할 수 있어 필요한 정보에 접근하는 데 시간을 절약합니다.

의미론적 임베딩: 지식 그래프 임베딩은 밀집 벡터 공간에서 개체와 관계를 나타내어 의미적 관계를 포착하고 유사성 기반 검색을 가능하게 합니다. 이 능력은 RAT 시스템이 논리적으로 관련 있는 개체뿐만 아니라 의미적으로 관련된 개념을 검색하도록 허용하여 보다 넓은 맥락 정보를 통해 추론 프로세스를 보강할 수 있습니다.

지식 통합과 추론: 지식 그래프는 다양한 지식 원본을 통합하는 것을 용이하게 합니다. 이는 RAT 시스템이 포괄적이고 일관된 지식 기반에서 추론을 할 수 있게 해 줍니다. 이 통합은 여러 원본에서의 정보가 그래프 구조 내에서 합쳐지고 조화되기 때문에 보다 정확하고 일관된 추론을 이끌어낼 수 있습니다.

<div class="content-ad"></div>

**해석 가능성과 투명성**: 지식 그래프에서 지식을 명시적으로 표현함으로써, 추론 프로세스의 해석 가능성과 투명성을 향상시킬 수 있습니다. 검색된 엔티티와 관계를 추적함으로써, RAT 시스템은 추론 단계에 대해 인간이 해석 가능한 설명을 제공할 수 있으며, 신뢰성과 책임성을 높일 수 있습니다.

지식 그래프를 검색 원본으로 활용함으로써, RAT 시스템은 구조화되고 논리적으로 일관된, 의미론적으로 풍부한 정보로부터 이점을 얻을 수 있으며, 보다 정확하고 맥락을 이해할 수 있고 해석 가능한 추론을 가능하게 합니다. 지식 그래프 기술이 계속 발전하고 보다 널리 채택되면, RAT 프레임워크와의 통합은 다양한 도메인에서 복잡한 추론 작업의 AI 시스템 능력 향상을 위한 큰 약속을 가질 것입니다.

# 코딩 모의 실행:

여기에는 우리가 논의한 다양한 구성 요소들 - 검색 증강된 생각 (RAT), SELF-DISCOVER, 몬테 카를로 트리 탐색 (MCTS) 및 지식 그래프 검색 - 을 큰 언어 모델 (LLMs)을 위한 일관된 추론 프레임워크로 통합한 코딩 모의 실행이 있습니다:

<div class="content-ad"></div>


이 코딩 모의 구성은 다양한 구성 요소들의 통합을 보여줍니다:

- 지식 그래프: 논리적 검색을 위한 구조화된 지식 베이스를 나타내며, 그래프를 초기화하고 쿼리에 따라 관련한 엔티티를 검색하는 방법을 갖추고 있습니다.
- Thought Node: 추론 위상의 노드를 나타내며, 추론 텍스트를 포함하고 부모 및 자식 노드에 대한 링크를 포함합니다.
- Reasoning Topology: 추론 위상을 관리하고, LLM 및 지식 그래프 검색을 사용하여 노드를 확장하는 것을 처리합니다. 추론 공간을 탐색하는 Monte Carlo Tree Search (MCTS) 알고리즘의 구현도 포함됩니다.
- SELF-DISCOVER: 원자적 추론 모듈에서 추론 구조를 발견하기 위해 SELF-DISCOVER 프레임워크를 구현합니다. 모듈을 선택, 적응, 구현하는 방법을 포함합니다.
- 사용 예시: 다양한 구성 요소가 함께 사용되는 방법을 보여줍니다. SELF-DISCOVER 객체를 인스턴스화하고 주어진 작업에 대한 추론 구조를 발견한 후, MCTS를 사용하여 추론 위상을 탐색하고 가장 유망한 추론 노드를 찾습니다.

이 모의 구성은 제안된 추론 프레임워크 구현을 위한 기반으로 제공되며, RAT(지식 그래프 검색), SELF-DISCOVER(적응 추론 구조), MCTS(추론 공간의 효율적 탐색)의 강점을 통합합니다.


<div class="content-ad"></div>

# 결론

LLM의 다단계 추론 능력을 향상시키는 것은 일관된, 사실적이고 논리적 추론이 필요한 복잡한 작업에 대응하는 데 중요합니다.

RAT, SELF-DISCOVER 및 MCTS와 같은 기법들은 구조화된 추론 토폴로지, 외부 지식 검색 및 추론 영역의 효율적 탐색을 통해 LLM을 보완하는 유망한 접근 방식을 제공합니다.

이 분야의 연구가 계속되면, LLM의 장점을 활용하면서 그 한계를 완화하는 보다 진보된 기술들을 기대할 수 있으며, 이를 통해 복잡한 현실 세계 문제에 대처할 수 있는 강력하고 능력있는 AI 시스템이 걸음을 내딛게 될 것입니다.

<div class="content-ad"></div>

**In Plain English 🚀**

어서와! In Plain English 커뮤니티에 오신 것을 환영해! 떠나시기 전에:

- 작가를 clapping하고 팔로우하는 것을 잊지마세요! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: Stackademic | CoFeed | Venture | Cubed
- 더 많은 콘텐츠: PlainEnglish.io