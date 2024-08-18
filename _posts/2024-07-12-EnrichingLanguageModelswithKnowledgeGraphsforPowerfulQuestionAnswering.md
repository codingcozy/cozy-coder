---
title: "강력한 질문 응답을 위한 지식 그래프와 언어 모델 통합 방법"
description: ""
coverImage: "/assets/img/2024-07-12-EnrichingLanguageModelswithKnowledgeGraphsforPowerfulQuestionAnswering_0.png"
date: 2024-07-12 23:36
ogImage:
  url: /assets/img/2024-07-12-EnrichingLanguageModelswithKnowledgeGraphsforPowerfulQuestionAnswering_0.png
tag: Tech
originalTitle: "Enriching Language Models with Knowledge Graphs for Powerful Question Answering"
link: "https://medium.com/ai-in-plain-english/enriching-language-models-with-knowledge-graphs-for-powerful-question-answering-3c08b3b8020f"
isUpdated: true
---

인공 지능 소프트웨어가 이 글의 문법, 흐름 및 가독성을 강화하는 데 사용되었습니다.

검색 보강 생성(Retrieval-augmented generation, RAG)은 대규모 언어 모델(Large Language Models, LLMs)을 향상시키는 핵심 기술로 부상했습니다. 외부 맥락을 제공함으로써 RAG는 LLM이 생성한 텍스트를 실제 사실 정보에 근거하여 지원되지 않는 환각을 위험하게 하는 대신에 뿌리를 내립니다. 이러한 맥락은 일반적으로 벡터 유사성 검색을 사용하여 색인된 데이터베이스에서 관련 텍스트 세그먼트를 검색함으로써 제공됩니다.

그러나 이러한 전통적인 벡터 인덱스 검색 접근 방식에는 한계가 있습니다. 단순한 단어 벡터 유사성은 종종 복잡한 현실 세계 데이터 내의 미묘한 연결과 관계를 완전히 포착하지 못할 수 있습니다. 따라서 단독의 텍스트 조각들은 LLM의 이해와 추론을 제한하는 좁고 표면적인 맥락을 제공하는 경향이 있습니다.

최근 혁신적인 방법들은 RAG 프레임워크에서의 문맥 보강 소스로 원시 텍스트 세그먼트 대신 지식 그래프 사용을 탐구했습니다. 지식 그래프는 엔티티, 그들의 속성 및 그들 사이의 라벨이 지정된 관계의 구조화된 표현을 제공합니다. 출처 말뭉치에서 구축된 이 그래프는 핵심 의미 개념과 종속성의 고수준 추상화를 인코딩합니다.

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

LLM 텍스트 생성 알고리즘에 정교하게 조정된 서브그래프를 제공하여 쿼리를 문맥화하면, 더 깊은 추론, 설명, 사실적으로 부정확한 환각 감소를 필요로 하는 작업에서 상당한 향상이 나타납니다. 그래프 형식의 풍부한 메타데이터는 고립된 텍스트 세그먼트가 제공할 수 있는 것보다 더 강력한 문맥적 연결을 노출시킵니다.

이 기사에서는 Microsoft의 GraphRAG와 Xiaoxin He 및 동료의 G-Retriever에서 지식 그래프 중심 접근법을 취하는 최근 두 가지 기술을 분석합니다.

이를 통해 이러한 방법을 연결하여 컨텍스트에 민감한 신경 기반 텍스트 생성을 더 발전시킬 수있는 기회를 제안합니다.

이 기사에서는 기초 텍스트 단편에서 제공하지 못하는 관계적 관점을 인코딩하는 지식 그래프가 어떻게 문맥을 향상시켜주는지 탐구하며, 이로 인해 뛰어난 증강이 제공된다고 제안합니다.

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

카드 건축, 검색 및 신경 통합 구성 요소에 대한 자세한 내용은이 기술이 구조화 된 데이터를 활용하여 LLM을 실제로 어떻게 향상시키는지를 분석할 것입니다.

마지막으로 GraphRAG, G-Retriever 및 반복적인 RAG의 잠재적인 조합에 대해 더 정교한 맥락적 추론을 더 많이하는 방향으로 논의됩니다.

![이미지](/assets/img/2024-07-12-EnrichingLanguageModelswithKnowledgeGraphsforPowerfulQuestionAnswering_0.png)

# 지식 그래프에 들어가세요

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

지식 그래프는 개체를 노드로 코딩하고 해당 노드 내의 속성을 자세히 표현하며, 노드를 연결하는 라벨이 지정된 엣지로 개체 사이의 관계를 나타내는 구조화된 표현입니다. 관련된 요소들을 연결함으로써 중요한 의미적 개념과 종속성을 추상화합니다.

지식 그래프를 구축하는 과정은 일반적으로 특정 도메인의 소스 문서 집합에서 개체와 관계를 추출하여 상호 연결된 네트워크로 표현하는 것을 포함합니다. 예를 들어, 의료 출판물로부터 질병, 증상, 치료법을 노드로 하는 의료 지식 그래프를 구축할 수 있으며, 상호 관계(상관 관계, 원인, 치료법 등)를 나타내는 엣지가 연결됩니다.

이 그래프 기반 표현은 고립된 텍스트 단편보다 여러 가지 이점을 제공합니다:

- 전역적 관점 — 네트워크 구조는 지역적 단편이 아닌 문서 전체에 걸친 연결의 더 완전한 새로운 관점을 제공합니다.
- 명시적 관계 — 개체 간의 종속성은 관계 유형이 지정된 엣지로 직접 캡처됩니다. 이러한 수준의 형식화는 비정형 텍스트에서는 불가능합니다.
- 개념의 추상화 — 핵심 의학적 개념은 노드가 되어 표면적 용어의 변형과 상이함을 초월합니다.
- 다중 단계 탐색 — 그래프로 연결된 엣지 의미에 의해 쿼리에 답변하기 위해 여러 단계를 연결할 수 있습니다. 단일 단편을 넘어서는 것입니다.
- 확장 검색 — 하위 그래프는 가장 중요한 개체를 중심으로 연결된 구성요소를 선택적으로 검색할 수 있습니다.

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

언어 모델에 지식 그래프 컨텍스트를 적용하여 각각의 텍스트 세그먼트가 아닌 쿼리에 맞게 조정하면 추가적인 전체적 시각, 형식적 개념 모델, 관계 표현이 뉴럴 생성기가 더 깊은 추론을 보여주는 결과물을 생산하는 데 더 잘 갖추어지게 됩니다.

지식 그래프가 포착하는 연결은 고립된 텍스트 조각에서 이용할 수 없는 것으로, LLM(Large Language Models)에 외부에 대한 우수한 이해력을 제공하여 생성을 구조화된 현실에 기반으로 하게 됩니다. 이는 좁고 표면적인 텍스트 세그먼트가 부과하는 제한을 해제합니다.

그래프 형태로 포착된 풍부한 메타데이터 — 엔티티, 관계, 의미 클러스터 — 는 다중 홉 설명적 추론에서 이야기 요약에 이르기까지 다양한 언어 모델 능력을 향상시키는 컨텍스트를 해제합니다. 지식은 LLM을 기본 텍스트 유사성을 넘어 성장시킵니다.

# GraphRAG: 그래프 클러스터링으로 주요 주제 요약하기

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

GraphRAG은 지식 그래프를 검색 보강에 적용함으로써 언어 모델이 전체 데이터셋을 대상으로 추론할 수 있게 해준다. 지역적인 조각뿐만 아니라.

이 방법은 대규모 언어 모델 자체를 활용하여 비공개 말뭉치에서 지식 그래프를 구축하고, entity, attribute 및 relationship을 상호 연결된 의미 네트워크로 추출하는 것을 포함한다. LLM 기반의 그래프 추출을 통해 새로운 데이터셋에 적응이 가능하다.

강력한 그래프 클러스터링 알고리즘을 활용하여 이러한 데이터 요소들을 의미론적 클러스터로 그룹화하여 잠재적 주제 및 내러티브를 반영한다. 이 클러스터는 문서를 거치는 연상을 식별한다.

그런 다음 GraphRAG가 가져오는 주요 혁신은 LLM 그래프에서 생성된 말뭉치 전체 의미 클러스터를 상위 레벨 쿼리에 답할 때 검색 소스로 사용하는 것이다. 데이터셋에서 주요 주제를 요약하는 질문 등을 고려하십시오.

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

일반적인 벡터 유사성만큼 실패하지 않는 GraphRAG는 데이터 집합 전체에서 서로 연결된 엔티티 및 관계의 하위 그룹 내에서 이야기를 반영하는 일관된 요약을 제공하기 위해 그래프 클러스터링 기술을 활용합니다.

게다가, 지식 그래프 구조는 각 클러스터에 기여한 소스 레코드로의 직접적인 링크를 유지합니다. 이를 통해 증거로서 지원 스니펫을 요약된 주제와 함께 표시할 수 있는 출처 추적을 가능하게 합니다.

문서 집합 전체를 연결된 의미 네트워크로 다루는 것은 전통적으로 격리된 텍스트 세그먼트를 문맥으로 하는 언어 모델에 대해 어렵습니다. 그러나 LLM이 구축한 전역적인 상호 연결된, 관계 중심 표현에 의해 GraphRAG는 전체 데이터셋의 잠재 주제에 대한 인식을 끌어낼 수 있습니다.

이 "전체 데이터셋 이해"는 LLM을 질의에 대한 관련 텍스트 스니펫을 좁게 검색하는 것에서 대신 의미 그래프 그룹화를 반영하는 계층화된 요약을 활용하여 전체적으로 키 테마에 대해 종합적으로 추론하는 방식으로 나아가게 합니다.

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

GraphRAG는 LLM(어반 언어 모델) 지식 그래프를 구축하고 클러스터링하여 기본 텍스트 유사성에서 기업체 간의 중요 주제 및 이야기에 대한 구조화된 추론으로 진보할 수 있도록 합니다. 고립된 텍스트에서 쉽게 감지할 수 없는 전역 잠재 주제를 검색 가능한 개념으로 변환합니다.

# G-Retriever: 텍스트 그래프 질의 응답을 위한 구조화된 검색

G-Retriever는 잠재적으로 관련 노드를 초기 식별하기 위한 벡터 유사성 검색과 그래프 신경망을 결합합니다. 이 하이브리드 방식은 그래프 인식력과 확장성을 균형 있게 유지합니다.

공통 벡터와 그래프 임베딩을 기반으로 한 Prize-Collecting Steiner Tree 알고리즘은 높은 관련성 대 크기 비율에 맞게 맞춤화되어 질문에 대답하는 데 중요한 중심 노드 및 관련성에 집중된 소규모 연결된 지식 부분 그래프를 검색합니다.

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

이렇게 전체 소스 지식 그래프에서 직접 검색한 구조화된 그래픽 컨텍스트를 제공하는 것이 텍스트 조각들보다 더 풍부한 정보를 제공하여 LLM에 지역적 관점을 제공할 수 있습니다. 이는 진정한 그래프에 근간을 둔 구조적 추론을 통해 정확한 답변을 생성하는 데 도움이 됩니다.

게다가, 그래프 컨텍스트를 직접 검색함으로써, 기존의 LLM 접근 방식이 얼마나 많은 텍스트 생성에만 의존하는지의 위험과 상식을 넘어선 개연성없는 주장을 줄입니다. 검색된 데이터를 기반으로 출력물을 근거로 가져옴으로써 사실적 신뢰성을 강화할 수 있습니다.

요컨대, G-Retriever는 정보 검색을 적절한 서브그래프를 통해 확장 가능성을 균형 있게 잡아 원래의 구조화된 데이터에 밀접하게 결합하여 LLM 생성을 규제하고 환각 위험을 줄이며 대형 실제 세계 네트워크에서 지역적 그래프 기반 추론을 가능하게 합니다.

이 혼합 방법론은 일반 코퍼스 접근법으로 인한 RAG의 실패를 보완하기 위해 규모에 맞게 텍스트 지식 그래프 상에서 직접 그래프 질문에 대한 검색을 조정합니다.

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

# RAG + 지식 그래프 연결하기 = 강력한 QA

RAG와 G-Retriever는 언어 모델을 강화하기 위해 지식 그래프를 활용하는 유망한 진전을 보여주고 있지만, 각각이 개선할 여지가 있습니다. GraphRAG는 개인 기관을 중점으로 하며, 그래프를 처음부터 구축하는 과정이 비용이 많이 들어갑니다. G-Retriever는 공개 데이터를 처리하지만 잠재적인 주제 인식이 부족합니다.

서로 보완적인 강점을 결합함으로써 더 깊이있는 발전을 이룰 수 있습니다:

주제를 포함한 그래프 임베딩을 풍부하게 하기

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

G-리트리버의 상-수집 슈타이너 트리 알고리즘이 그래프RAG로부터의 잠재적 주제 클러스터를 포함하여 업그레이드되어, 노드/엣지에 추가적인 메타 수준 설명을 부여할 수 있습니다. 이는 다중 해상도 이해를 위해 세부적인 개체 수준과 고수준 주제 수준에서의 쿼리를 가능하게 할 것입니다.

구조화된 검색으로 요약 강화

반면에, 그래프RAG의 문서 데이터 집합 요약은 정확성과 설명 가능성을 위해 G-리트리버를 활용할 수 있습니다. 넓은 주제 내의 특정 개체/관계에 관한 세부 질문은 선택적 하위 그래프 검색을 유발함으로써, 참 그래프에 LLM 답변을 근거지을 수 있습니다.

그래프 위의 반복 체인드 추론

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

더 나아가서, 플랫폼은 반복적인 RAG를 수행할 수 있을 것입니다. 하나의 검색+LLM 사이클에서 출력 그래프 문맥을 가져와 추가적인 추론 사이클을 일으키는 입력으로 사용할 수 있습니다. 이러한 단계적인 그래프 탐험을 통한 질문에 대한 해답은 명백하지 않은 통찰을 발견할 수 있을 것입니다.

자동 생성된 그래프

마지막으로, 말뭉치로부터 자동 생성된 그래프의 발전은 두 기술에 모두 이식될 수 있습니다. 이를 통해 GraphRAG가 처음부터 구축할 필요 없이 G-Retriever의 그래프를 최신 상태로 유지할 수 있을 것입니다.

지식 그래프 생성이 자동화되고 LLM이 성숙해짐에 따라, 구조화된 지식 검색을 활용한 견고한 시스템은 상황인식 언어 생성을 해제하는 데 점점 더 중요해질 것입니다. 그래픽 및 신경망을 밀접하게 통합함으로써 정확한 의미론적 검색 알고리즘을 용이하게 할 수 있습니다. 이는 특정한 관련 있는 서브그래프를 찾아내어 사실적이고 근거 있는 추론을 촉진할 수 있습니다.

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

\*\*RAG 반복을 체인으로 연결하는 것은 복잡한 단계별 추론을 위한 메커니즘을 제공하여 AI 발전을 가속화할 수 있는 새로운 패러다임을 제시합니다. 이는 구성적으로 증진된 모델들이 고립된 학습을 대체함으로써 AI 발전을 가속화할 가능성이 큽니다.

상호 보완적인 그래프와 신경 방법의 융합은 추론, 설명 및 추론이 가능한 언어 모델을 도울 것입니다. 그뿐만 아니라 검색뿐 아니라 추론할 수 있는, 능력 있는 대화형 AI의 다음 시대를 주도할 것입니다.

# 간단해 보자면 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

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

- 쓴 글은 꼭 박수를 치고 작가를 팔로우해주세요! 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: Stackademic | CoFeed | Venture
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요.
