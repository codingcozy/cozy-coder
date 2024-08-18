---
title: "하이브리드 추론을 향하여 구조를 서브심볼릭 시스템에 통합하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-TowardsHybridReasoningAssimilatingStructureintoSubsymbolicSystems_0.png"
date: 2024-07-13 22:29
ogImage:
  url: /assets/img/2024-07-13-TowardsHybridReasoningAssimilatingStructureintoSubsymbolicSystems_0.png
tag: Tech
originalTitle: "Towards Hybrid Reasoning: Assimilating Structure into Subsymbolic Systems"
link: "https://medium.com/the-modern-scientist/towards-hybrid-reasoning-assimilating-structure-into-subsymbolic-systems-05cf9d34d13d"
isUpdated: true
---

최근 대형 언어 모델의 발전은 텍스트 생성 시 놀라운 유창함과 적응력을 보여주었습니다. 몇 가지 예시에 노출된 이후, 이러한 모델은 다양한 주제에 대해 놀랍도록 일관된 연장을 제공할 수 있으며, 유연한 이해력과 추론 능력을 나타냅니다.

그러나 추가적인 분석 결과, 남는 기본적인 빈틈이 낙관론을 억제합니다. 본질적으로 대형 언어 모델은 대규모 교육 말뭉치에서 용어 간 상관 관계를 반영하는 통계적 패턴만 누적합니다. 이들은 개념, 관계 또는 규칙에 대한 구조화된 표현이나 명시적 모델링이 없습니다. 결과적으로, 그들은 종종 기본적인 논리적 오류를 일으키며, 교육 분포 이상의 체계적인 일반화에서 실패하며, 일관된 추론 체인을 유지하는 데 어려움을 겪습니다.

진실된 이해가 아닌 데이터 빈도에 의해 주도되는 패턴 인식기의 본질적 성격은 세밀한 추론, 원인 분석 또는 구성적 일반화가 필요한 쿼리의 경우 유연성을 상실시킵니다. 다단계 추론, 역학의 해독 또는 분리된 사실들의 통합을 요구하는 질문들은 이러한 한계를 쉽게 드러내곤 합니다.

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

반면, 지식 그래프는 네트워크 내에서 개념을 명시적으로 인코딩하여 상호 연결된 노드로 구조화된 상징적 표현을 제공합니다. 다양한 관계를 모델링하여 세부적인 의미론을 캡처하며, 이는 개체 간의 계층적, 논리적, 시간적 및 절차적 연결을 포함합니다. 이러한 확인된 사실과 제약 사항의 기반은 설명 가능한 구조화된 질의와 분석을 지원합니다.

그러나 정확한 형식적인 질의를 구성하는 것은 전문 지식이 필요하며, 지식 그래프는 또한 불완전함과 희소성에 시달립니다. 그들은 신경 모델이 보여주는 개방적 추론, 적응 또는 의미론적으로 근거 있는 일반화 능력을 가지고 있지 않습니다.

지식 그래프와 LLMs의 보완적인 강점과 약점은 각 표현의 본질적인 능력을 활용하는 조화된 접근을 장려합니다.

그러나 방대한 통계 모델과 구조화된 지식 네트워크 간의 효과적인 인터페이스를 개발하는 것은 일치, 추적 가능성, 효율성, 상호 강화에 대한 엄청난 도전을 가져옵니다.

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

미숙한 방법론은 표현을 완전히 활용하지 못합니다. 지식 그래프 사실을 단순히 검색하고 제시함으로써 LLM에 구조를 포기합니다.

반면에 질문을 형식적인 쿼리로 직접 번역하는 것은 종종 충분한 범위가 부족합니다.

대신, 동적인 다중 단계 방법론은 지식 형태의 능력을 실질적으로 증폭하는 데 필수적입니다.

이 글에서는 구조화된 지식 그래프와 유연한 신경 언어 모델 간 유동적 상호작용을 섬세하게 조율하는 이유와 기술을 탐구합니다.

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

주장은 이러한 이산적 표현들을 효과적으로 연결하기 위해 질문을 조정하고 요구를 분해하며 관련 서브그래프를 신속하게 검색하고 중간 결과를 단계별로 전파하며 검색을 재귀적으로 재계획하고 증거를 통합하고 설명을 평가하는 조정된 파이프라인이 필요하다는 것입니다. 이때 LLMs은 구조화된 지식과 사용자 요구 사이의 맥락적 접착제 역할을 합니다.

![이미지](/assets/img/2024-07-13-TowardsHybridReasoningAssimilatingStructureintoSubsymbolicSystems_0.png)

# 개요:

I. 합성의 장애

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

우리는 대형 언어 모델의 주요 한계인 구성 일반화에 대한 어려움부터 개요를 시작합니다. 인과 지식 단편을 모듈식으로 결합하는 취약성은 보다 심층적인 추론에 장애를 제공합니다.

전환: 내재적 능력 차이의 이 분석은 구조화된 지식 틀을 제공할 수 있는 시스템을 탐색할 동기를 부여합니다.

II. 벡터 검색 대비 지식 그래프의 장점

다음으로 우리는 언어 모델 추론을 강화하기 위한 지식 그래프의 강점을 강조합니다 — 보다 풍부한 관계 모델링, 설명 가능한 트레일, 집중적인 탐색 및 구조 학습.

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

제III장. 복합 지식 그래프의 문제점

- 규모, 노이즈, 불완전성 및 희소성과 같은 도전적인 측면은 현실 세계의 지식 그래프를 효과적으로 조회하여 신경 모델에 전달하는 데 장애물을 제시합니다.

다양한 이유로 복잡한 지식 그래프에 대한 도전적인 요소들을 알아보면서, 우리는 구조화된 지식에 접근하는 쿼리 및 확장의 고질적인 어려움을 분석합니다.

- 이러한 장애물들은 조정된 단계적 접근 방식의 필요성을 강조합니다.

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

#### IV. A Structured Assimilation Workflow

We suggest an ideal multi-phase workflow that emphasizes iterative analysis, modularization, parallel retrieval, and recursive enhancement when assimilating knowledge graphs into language models.

_Transition:_ This conceptual template presents an automation-focused vision. We delve into examples of enterprise applications.

#### V. Infusing Enterprise Operations

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

마침내, 통합 산업 지식 그래프에 의존하는 언어 모델이 기업 내 비즈니스 문제에 대한 추론 엔진으로 작용하는 것을 상상합니다.

# I. 복합성 도전 과제

LLM의 주요 제한 요소는 구성 일반화에 대한 고충입니다 - 모듈식으로 개별적으로 학습한 기술을 유연하게 결합하여 새로운 구성물을 형성하는 것. 이는 체계적인 인과 분석을 방해합니다.

a. 부서지기 쉬운 조합

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

LLMs(사용자 언어 모델)은 인과 주장과 추론을 구조화 가능한 단위로 구성하는 대신에 단일한 덩어리로 기억하는 경향이 있습니다. 예를 들어, "강우량이 적으면 작물 수확량이 감소한다"는 주장은 나눌 수 없는 단위로 기억됩니다.

이는 여러 학습된 인과 관계를 신선한 추론을 만들기 위해 신뢰할 수 없게 결합하는 데 어렵게 만듭니다. 인과적 링크들은 모듈식으로 분리되어 있지 않습니다. 이러한 결과로, 심지어 간단한 것들조차도 새로운 인과 주장의 조합이 종종 LLM들이 붕괴시키며, 교육 중 본 문장들을 정확히 보지 않으면 그들의 반응은 예측할 수 없게 됩니다.

예를 들어, 하나의 LLM은 다음과 같은 인과 주장을 각각 따로 접했을 수 있습니다:

"강우량이 적으면 작물 수확량이 감소한다"

"고온은 강우량을 낮춥니다"

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

하지만 이후 신뢰할 수 있는 방법으로 그것들을 결합하여 이러한 결론을 도출하지 못한다:

"고온은 수확량을 감소시킨다"

이 모델들은 인과 지식 단편들의 구조화된 조합을 허용하는 명시적 추상화가 부족하기 때문입니다.

b. 긴 인과 사슬

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

- 다단계 인과 관계 추론은 추론 단계 사이를 가로 질러 중간 결론을 유지해야 합니다.
- 그러나 LLMs는 현재까지 추론된 사실을 오래된 인과적 서술 중간에 유지하는 데 필요한 에피소드 메모리에 어려움을 겪습니다.
- 그 결과, 이들은 지속적인 결론 체인이 필요한 다단계 설명을 발전시킬 때 일관된 논리적 서술을 제시하지 못할 때가 많습니다.
- 추론된 사실을 신뢰성 있게 추적하지 못하는 것으로 인해, 그들은 특히 오랜 추론 순서가 포함된 체계적 인과 분석을 방해받습니다.

c. 인과적 이행

- 현실 세계의 인과 추론은 분리된 단편이 아니라 서로 연결된 체인 사이를 유연하게 이동합니다.
- 예를 들어, 폭우가 홍수를 일으키고, 그로 인해 대규모 대피와 이로 인한 경제적 손실이 발생할 수 있습니다.
- 모든 인과 체인 사이의 모든 가능한 전환을 이해하고 하드 코딩하는 것은 사실상 불가능합니다.
- 그러나 현재 LLMs는 인과적 체인을 넘나든 매끄럽게 전환하는 일반화를 신뢰할 수 없습니다. 그들의 지식은 연관된 웹이 아니라 조각난 것입니다.

d. 메커니즘 가설 설정

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

- 인간은 새로운 인과 메커니즘을 창의적으로 가정하여 관찰된 현상을 설명하는 설명 모델을 구축할 수 있습니다.
- 그러나 LLMs는 시나리오를 활발히 시뮬레이트하거나 가설을 테스트하기 위해 세계에 개입할 수 없습니다.
- 이는 이미 학습한 명제를 재구성하는 것 이상의 생산적 가설 형성 능력을 제한하는데 크게 기여합니다.
- 그들은 개념적 추상화와 모델 테스트를 포함하는 내재적 이론 구축 능력이 아닌 명시적 인과 메커니즘에 기초합니다.

인과성의 조합은 모듈식 지식, 지속적 에피소드 메모리, 부드러운 전환, 창의적 가정을 필요로 합니다. 이런 면들에서 LLMs는 모두 어려움을 겪고 있으며, 이는 인과적 지식을 더 풍부하게 이해하는 방향으로 유연하게 결합하는 것에서 체계적으로 취약한 것을 나타냅니다.

본 연구는 본질적인 능력 간격을 분석함으로써 구조화된 지식 지원 제도를 탐색하는 동기부여를 제공합니다.

# KG RAG 시스템이 벡터 데이터베이스보다 우수함의 원인

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

RAG(검색 증강 생성)은 확장 가능한 텍스트 말뭉치와 강력한 언어 모델을 결합하는 뚜렷한 기술로 등장했습니다. 이러한 파이프라인의 주요 요소는 의미적 임베딩을 통한 벡터 검색을 사용하여 모델 생성을 맥락화할 유용한 텍스트 단락을 식별하는 것입니다.

그러나 복잡한 추론이 요구될 때는, 단어 연상 주변의 느슨한 통계 신호에만 의존하는 시스템은 내재적 한계에 도달합니다. 개념들의 상호 연결된 성격을 인식하지 않은 채 벡터 기술은 일관된 추론을 돕는 사실 체인을 검색하는 데 어려움을 겪습니다.

여기서 명시적으로 구조화된 지식 그래프가 차세대 RAG 디자인을 변화시키는 요인이 됩니다. 개념을 노드로, 속성을 속성으로, 다차원 관계를 유형별 엣지로 캡슐화함으로써 지식 그래프는 검색의 수준을 끌어올릴 도면 구조를 제공합니다. 이제 각 쿼리에 대해 고차원 벡터 공간에서 유령을 추구하는 대신, 검증된 연결을 통한 추론 트레일이 데이터 기반 기계 학습의 추론을 돕는 설명 가능한 추론을 제공합니다.

본질적으로, 지식 그래프는 임상 출처, 온톨로지 및 유효성 주위에 투명한 의미론을 쓰고, 순수 통계 주도 검색의 맹점을 덮어씁니다. 연결성은 콘텐츠에 맥락을 제공합니다. 전역 그래프 토폴로지는 구조적 관련성을 지역 벡터 근접성과 연결시킵니다. 또한 상징적이지만 계산 가능한 요소 표현은 유동적으로 쿼리할 수 있는 상호 운용 가능한 기반을 생성합니다.

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

결과는 체계적인 상징적 기반 위에 안전하게 안정화된 유연한 신경 생성기입니다. 혹은 공학 용어로 말하자면, 견고한 운영 시스템에 의해 확고히 바탕을 둔 고수준 언어들입니다. 통계적 모델을 정직하게 유지하면서 설명 가능한 감사 트레일을 통해 구조화된 지식 검색은 검색 및 생성 간에 조화로운 증가를 약속하는 이 조화로운 하이브리드 아키텍처를 제공합니다.

더 풍부한 관계

지식 그래프는 엔티티 간 다중 상호 연결된 관계를 모델링합니다 — 분류, 논리, 절차, 인과, 시간적 관계 등을 포함합니다. 이러한 구조화된 지식 표현은 유사도 점수뿐만 아니라 더 체계적인 추론을 가능케 합니다.

예를 들어, 위치의 위도/경도 좌표를 알고 있다면 지리적 추론이 가능합니다. 또한 부분-전체 계층 구조는 유전 기반 추론을 가능하게 합니다. 사실을 시기별로 명확히 하면 시간적 추론도 가능해집니다.

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

이러한 다양한 신호들은 확인된 관계를 사용하여 회수된 사실들 간의 정확한 논리적 연쇄를 안내합니다.

그에 비해, 벡터 검색의 통로 임베딩은 구조화된 관계 개념이 없는 고립된 원자 벡터로 엔티티를 인코딩합니다. 가치 있는 관계 신호가 손실됩니다.

설명 가능한 추론 트레일

멀티-핫 인퍼런스 중에 엔티티 및 관계 위를 추적함으로써 KGRA 시스템은 전체 그래프 탐색 감사 트레일을 드러낼 수 있습니다. 이 투명성은 관련성 점수가 불투명한 신경 프로젝션인 벡터 검색과는 달리 근본적인 추론을 설명합니다.

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

**신뢰할 수 있는 지식 그래프의 영역**과 **의심스러운 연결**에 근거한 결론을 평가할 수 있습니다. 이러한 감사 기능은 설명 가능성이 중요한 구현에서 신뢰성을 향상시킵니다.

**집중 탐구**

관련 콘텐츠를 명시적으로 연결함으로써 정보 검색 시 핵심 개체와 개념 주변의 집중된 탐구를 허용합니다. 이를 통해 재귀적 벡터 유사성 체이닝 중에 발생할 수 있는 관련성이 옅은 콘텐츠로 이탈하는 것을 방지할 수 있습니다.

예를 들어, "파리"와 관련된 다른 단락을 직접 검색함으로써, 모든 프랑스 관련 내용을 가져오는 대신 추론을 중심으로 하게 됩니다.

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

모듈성

고유한 관계와 제약 조건을 갖는 하위 그래프는 맞춤형 추론 파이프라인으로 구성할 수 있는 모듈식 지식 구성 요소를 제공합니다. 이 레고와 같은 모듈성은 추론 도구를 플러그 앤 플레이 방식으로 섞고 매치할 수 있도록 가능케 합니다.

그에 반해, 벡터 검색은 모듈성이나 제약 관계 기반 추론을 위한 기본 메커니즘을 제공하지 않습니다.

구조 학습

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

통계적 패턴을 분석하여 그래프 연결성을 통해 새로운 사실적 지식과 신학적 제약 사항이 자동으로 드러날 수 있습니다. 이를 통해 자기개선을 통해 보다 포괄적이고 일관성 있는 검색이 가능해집니다.

벡터 검색은 이러한 구조 학습에 대한 본능적인 위상이 부족합니다. 새로운 지식을 통합하려면 외부 모델 재학습이 필요합니다.

다음은 대형 언어모델 (LLM) 추론을 향상시키는 데 있어 표준 벡터 검색 검색 보강 (V-RAG)과 지식그래프 보강 (KG-RAG) 시스템 사이의 주요 차이점을 강조한 비교 표입니다:

![Comparison Table](/assets/img/2024-07-13-TowardsHybridReasoningAssimilatingStructureintoSubsymbolicSystems_1.png)

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

복잡한 추론에 대한 종합적인 처리가 필요할 때:

- 벡터 검색은 원자적 단락 사이의 유사성 개념만을 제공합니다.
- 이는 추론 체인에서의 체계성과 설명 가능성을 제한합니다.

대조적으로:

- 지식 그래프는 다양한 유형화된, 계층적인, 측면적인, 위치적, 시간적 및 논리적인 구조화된 관계를 제공합니다.
- 설명 가능하고 집중되며 체온학적으로 기초를 둔 추론 트레일을 가능하게 합니다.

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

지식 그래프 확장은 더 투명하고 체계적인 추론의 구성을 지원합니다. LLM은 여전히 검색된 콘텐츠를 소화하여 응답을 생성합니다. 그러나 상류 검색은 명시적으로 인코딩된 세계 지식에서 크게 이익을 얻습니다.

전체 지식 그래프가 인코딩되는 규모와 편의성을 위해 선택적 범위의 규모 측면을 고려할 때 트레이드오프가 발생합니다. 그러나 고가의 복잡한 추론을 위해 체계적인 지식이 표면적 유사성을 능가합니다.

지식 그래프의 이점을 확립한 후, 쿼리와 구조화된 지식 액세스의 확장에 대한 내재적인 도전을 분석합니다.

# III. 복잡한 KG 쿼리의 도전

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

### a. 규모

- 실제 세계 지식 그래프는 십억 개 또는 조각의 사실을 담을 수 있는 거대한 크기로 확장됩니다.
- 전체 그래프를 처리하는 것은 지수적 복잡성으로 인해 많은 알고리즘에게 문제가 됩니다.
- 적절한 소규모 하위 그래프를 검색하는 것조차 거대한 규모에서 효율적인 도전과 지연 문제를 야기합니다.
- 다중 점프 추론은 거대하고 이질적인 그래프의 먼 지역을 가로지르는 대규모 교차 동일 이웃을 탐색해야합니다.

### b. 소음

- 자동으로 생성된 지식 그래프는 오류가 많은 정보 추출 파이프라인을 통해 추출된 사실로부터 소음을 겪습니다.
- 결과적으로 정확성이 충족되지 않아 쿼리 응답이 악화되고, 정확한 의존성에 민감한 복잡한 추론 체인이 방해를 받을 수 있습니다.
- 규모에 맞는 검증을 통해 노이즈를 정리하는 것은 여전히 극도로 어렵고 인력 집약적입니다.

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

c. 불완전함

- 대부분의 지식 그래프는 인류 지식의 전체 범위에 비해 매우 불완전합니다.
- 한정된 말뭉치에서 구축되는 과정에서 중요한 개념과 관계들이 규칙적으로 누락됩니다.
- 빈 공간을 채우는 것에 대한 강력한 추론은 알고리즘에서 이용할 수 없는 광범위한 상식적 배경을 필요로 합니다.
- 질의는 빈 엔티티나 관계에 의존할 수 있으며, 소수의 연결로 인해 답변할 수 없는 상태가 될 수 있습니다.

d. 희소성

- 대부분의 현실 세계 그래프는 긴 꼬리를 가진 멱 함수 분포를 갖습니다.
- 대부분의 엔티티는 몇 개의 직접 이웃과 듬성듬성한 연결만을 가질 수 있습니다.
- 이는 최소한의 링크로 연결된 희소한 지역을 통해 조회 및 추론이 제한되면서 불완전성 문제를 악화시킵니다.
- 산발적인 그래프와 사실의 섬들을 통해 초희소 지식 그래프에서 지수적으로 증가하는 달리 도달할 수 있는 도전을 가지고 있습니다.

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

### c. 쿼리 작성 난이도

- 다문화 질문을 SPARQL 또는 Cypher와 같은 형식적 그래프 쿼리 언어로 정확하게 매핑하는 것은 전문가들에게도 어려운 일입니다.
- 이는 질문 내의 정확한 의미론과 관계를 이해하여 엔티티와 관계를 통한 동등한 탐색으로 번역해야 한다는 것을 필요로 합니다.
- 그래프 쿼리 언어 자체가 어려운 학습 곡선을 가지고 있어 기술적이지 않은 영역 전문가들에게 접근성이 제한되고 있습니다.

대규모, 노이지, 불완전하며 빈번하게 이어져 있는 그래프를 통해 나타나는 복잡성의 증가는 정확한 고성능 쿼리 작성과 실행을 제한하는 지속적인 도전 과제가 됩니다.

이러한 장애물은 조정된 다단계 접근 방식의 필요성을 강조하고 있습니다.

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

# IV. 제안된 하이브리드 워크플로

대형 언어 모델(Large Language Models, LLMs)을 외부 지식 그래프로 효과적으로 보완하는 것은 주의 깊은 조율이 필요한 다단계 과정입니다.

## a. 주기적 그래프로 에이전트 시스템 활용하기 :

순환 및 비순환 방향 그래프 사이의 근본적인 차이는 복잡한 추론 시스템을 설계할 때 유용한 유사점을 강조합니다. 주기적 그래프는 노드가 자체 또는 이전에 방문한 노드에 연결되어 재귀, 피드백 루프 및 업데이트된 믿음을 바탕으로 반복적으로 쿼리하는 가능성을 제공합니다. 반면에 비순환 그래프는 고정된 토폴로지를 따르는 엄격한 피드포워드 정보 흐름을 갖습니다.

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

이러한 구분은 대규모 언어 모델(Large Language Models, LLMs)을 외부 지식 그래프와 통합하는 메커니즘을 설계하는 데 잘 매핑됩니다. 구조화된 지식을 LLM에 효과적으로 보강하기 위해서는 필요에 따른 분석, 관련 서브그래프 검색, 중간 신호 전파, 남은 간극 재평가, 추가 쿼리 반복적 실행, 증거 통합 및 최종 의미 추론이 필요합니다.

이 반복적인 프로세스는 활발하게 쿼리를 실행하는 에이전트 런타임의 주기적인 성질을 반영하며, 지식을 통합하고 업데이트된 이해에 기반한 모델을 재쿼리하는 방식으로 응답을 점진적으로 개선합니다. 이 주기는 외부 구조물을 첨가해 응답을 점진적으로 향상시키는 데 사용됩니다.

본질적으로, 지식 그래프를 LLM에 효과적으로 흡수하기 위해서는 관련 외부 컨텍스트를 반복적으로 묶고, 남은 요구 사항을 평가하며, 전략을 조정하고, 점진적으로 응답을 향상시키며 진화시키는 피드백 루프 전체에 걸친 조율이 필요합니다. 이러한 주기적인 워크플로우 아키텍처는 구조화된 지식을 LLM에 확장 가능하고 효율적이며 신뢰성 있게 보강하는 데 도움이 됩니다.

반면에 사전에 정의된 강력한 단계를 통해 정보를 전달하는 비주기적 사슬은 동적 지식 검색의 확장을 필요로 하는 복잡한 추론 작업에 대응하는 데 적응성이 적습니다. 신중하게 설계된 주기적 보강 전략은 구조화된 지식 그래프를 통합함으로써 LLM을 체계적으로 향상시키기 위해 필요한 유연성과 조절성을 제공합니다.

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

## b. 비동기성의 중요한 역할

외부 지식 그래프를 대규모 언어 모델(LLM)에 효과적으로 흡수하는 것은 쿼리, 증거 집계, 의미 파악 및 반복주기 동안 점진적으로 완성본 개선을 조정하는 작업을 포함합니다.

복잡한 추론 워크플로우에서 이러한 단계 중 많은 것들은 병렬 실행에 적합한 독립적인 하위 작업을 포함합니다. 예를 들어, 분리된 그래프 세그먼트 간에 동시에 쿼리를 실행하거나 여러 엔티티에 대해 벡터 임베딩을 동시에 요청하는 것 등이 있습니다.

이러한 비동기적 워크플로우를 처리하는 것이 효율적이고 확장 가능한 시스템 구조에 중요합니다. 주요 이점은 다음과 같습니다:

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

- 동시 작업을 허용하여 대기 시간을 단축시킴
- 병렬처리를 통해 계산 자원 활용 증가
- 독립적인 쿼리가 병렬로 실행되어 더 빠른 응답 시간 확보
- 복잡한 오케스트레이션 로직에 대한 더 많은 유연성

LangGraph은 순환 결정 그래프의 비동기 조정을 전문으로 하는 프레임워크입니다. 비동기 노드는 기본적으로 동시성을 제공하며, LangGraph는 전체적인 워크플로우를 처리합니다.

기타 비동기식 지식 흡수를 위한 아키텍처에는 Ray와 같은 미들웨어를 사용하거나 FastAPI와 같은 비동기 플랫폼에 직접 로직을 인코딩하는 방법이 있습니다.

비동기 기술을 통해 지식 통합에 중요한 복잡한 쿼리-생성-정제 주기가 대폭 가속화될 수 있습니다. 이는 대규모 실시간 통합을 가능하게 합니다.

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

## c. 상징적 및 부상징적 통합

우리는 LLM을 효과적으로 향상시키기 위해서는 지식 그래프와 같은 상징적 구조화된 지식 표현을 신경 모델 내의 부상징적 통계 학습과 조화롭게 결합해야 한다는 것을 확인했습니다.

언어 모델에 상징적 지식을 생산적으로 결합하고 상태화하는 핵심 전략은 다음과 같습니다:

## d. 공동 벡터 임베딩

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

**공유 의미 공간을 생성하고 명백한 체계론 요소들인 엔티티, 관계, 제약과 신경 텍스트 토큰 임베딩을 결합하세요.**

**이 공통 공간을 통해 구조화된 지식과 자유 텍스트를 모두 포함하여 유사성 쿼리를 할 수 있습니다. 임베딩은 결합 접착제 역할을 합니다.**

### c. 그래프 스키마 사전지식

**심볼 그래프 스키마 - 엔티티, 관계, 논리 규칙 - 을 직접 모델 아키텍처에 인코딩하여 LLM 아키텍처 설계 선택 사항을 제안하세요.**

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

The injection of symbolic biases can greatly influence the way we navigate our thoughts and reasoning.

## Differentiable Programming

In this approach, we encode program logic, such as knowledge graph traversals or queries, as differentiable operations.

By utilizing gradients, we can fine-tune the interaction between structured programs and neural models, achieving a more seamless integration.

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

## e. Reasoning Shortcuts

**추론의 단축키**

추론 작업을 우회할 수 있도록 추론 단축키 역할을 하는 상징적 규칙들을 정리한 저장소입니다.

예를 들어, 정의적 규칙은 멀티-홉 탐색을 요구하는 대신 개념을 직접적으로 연관시킵니다.

이 전략들의 선택은 LLMs를 구조화된 지식으로 생산적으로 조건을 부여하는 방법을 예시로 보여줍니다. 신경 해석과 상징적 조작을 통합하는 것은 보다 체계적이고 의미론적으로 근거를 갖춘 추론을 약속합니다.

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

# V. 이상화된 형태에서의 조정 오케스트레이션 :

독일 사회학자 막스 베버(Max Weber)는 ‘이상 유형(ideal type)’이라는 개념을 만들어, 경험적 현실과 비교하기 위한 분석적 틀을 제공했습니다. 이상 유형은 어떤 현상의 극단적 표현을 추상적으로 모델링하여 특정 요소들을 수사적으로 강조합니다.

예를 들어, 베버의 '이상적 관료제(ideal bureaucracy)'는 대규모 조직의 본질을 추출합니다 — 계층구조, 전문화, 표준화, 생산성 등. 실제 기관들은 이 개념적 순수성에 다양한 정도로 근사합니다.

마찬가지로, 우리는 지식 그래프와 대규모 언어 모델 사이의 제안된 오케스트레이션 순서를 이상 유형 작업 흐름으로 특징화할 수 있습니다. 정형화된 지식을 효과적으로 동화하기 위한 이상 유형 워크플로우입니다.

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

단계는 추론 요구 사항이 완전히 분석되고 모듈식 도구가 동시에 시작되며 중간 결과물이 효율적으로 전파되고, 재계획이 원활하게 이루어지며, 통합이 철저하며, 설명이 정확하게 평가된다는 이상적인 워크플로우를 제시합니다.

Weber의 이상적 인 관료제와 마찬가지로, 이 개념적 워크플로는 논리적 기준을 제공합니다. 실제 구현은 실제 적용상의 제약으로 인해 특정 측면을 부분적으로만 실현할 수 있습니다.

그러나 이상 타입을 설정함으로써 최적 시스템의 모습에 대한 가설을 제공합니다. 이는 기술적 개선을 동기부여하기 위한 모범적 결과를 제공합니다. 그리고 이상과 실현 사이의 잠재적 간극을 드러냅니다.

이상 타입으로서, 이 워크플로우는 구조화된 지식을 언어 모델로의 원활한 통합을 위한 염원적인 모델을 제공합니다. 병행적 증거 수집, 자동 재귀적 재계획 및 다중 채널 설명과 같은 측면은 가상의 효율 최고점을 묘사합니다.

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

비전은 상징적 및 부상징적 능력을 조화롭게 하는 추론 생태계에 대한 기능적 측면을 정의합니다. 전용 연구를 통해 이 개념적 템플릿이 보다 구체적으로 구현될 수 있습니다. 그러나 지금 당장은 구조화된 지식 주입을 통해 언어 모델을 증강하는 데 이상적인 청사진으로 기능합니다.

1. 추론 작업 분석

- 핵심 개념을 식별하고 그들 사이의 의존성을 결정해 용어의 다의성을 명확히 합니다.
- 복잡한 문제를 단순 정보 요구 사항으로 해체하여 답변을 구성하는 데 필요한 기본 정보 블록을 도출합니다.

이는 지식 검색 워크플로우의 기본 구성 요소를 제공합니다.

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

**2. 모듈식 도구로 분해하기**

- 특정 서브그래프 검색 작업, 그래프 알고리즘 및 벡터 유사성 쿼리를 처리하는 재사용 가능한 모듈식 도구로 캡슐화합니다. 이로써 하위 수준의 지식 그래프 복잡성을 처리할 수 있습니다.
- 노드 추천, 사실 확인, 시간적 순서 등 특정 추론 패턴을 다루는 구성요소 상자를 만듭니다.
- 도구 간의 상호 운용성을 위해 깔끔한 인터페이스와 데이터 형식을 정의합니다.

**3. 다각적인 쿼리 초기화**

- 동시에 모듈식 도구 쿼리를 구성하고 실행하여 동시에 증거를 수집합니다.
- 근사 벡터 검색을 활용하여 지식 그래프의 가장 관련성 높은 영역에 도구를 빠르게 집중시킵니다.
- 실행시 최적화 프로그램은 중간 신호를 기반으로 탐색 전략을 지속적으로 조정합니다.

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

**4. 중간 결과물 전파**

- 모듈식 도구들이 출력물을 생성할 때, 생으로 떨어뜨리는 것이 아니라 직접 집중된 상태 저장소에 넣어주세요.
- 통합을 위해 검색된 내용 전체에서 상호 참조 및 개체 해결해주세요.
- 인코더가 증거를 공유 벡터 공간에 흡수합니다.

**5. 재귀적 재계획**

- 지금까지 수집된 증거에 대한 업데이트된 통합된 견해를 바탕으로, 남은 개방적 추론 요구사항을 재귀적으로 재평가해주세요.
- 설명의 빈틈을 채우기 위해 필요할 때 동적으로 추가 도구 쿼리를 실행해주세요.
- 최적화 도구가 답을 완성하는 과정을 추적합니다.

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

## 6. LLM 흡수

- 로더 배치는 다양한 증거물을 해석, 명확히 하고 조직하는 대규모 언어 모델이 소화할 수 있도록 업데이트된 상태 다이제스트를 처리합니다.
- LLM은 무절제한 추측을 강조하면서 세부 정보를 일관된 이야기로 조율하는 추론 클루로 작용합니다.

## 7. 평가 및 설명

- 조립된 설명의 일치성을 원본 추론 쿼리와 평가 쿼리 및 사용자 피드백을 통해 평가합니다.
- 증거 지원 및 추론 체인을 명확하게 하는 자연어 응답을 작성하여 전체 답변 생성 시 따르는 추론 체인을 명확히 합니다.
- 익스포저는 주요 그래프 탐색 경로와 도구 출력을 해석 가능한 감사 추적으로 번역합니다.

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

우리의 제안은 고품질의 설명적 결과물에 수렴할 수 있도록 체계화된 지식과 생성언어 구성요소의 모듈화, 병렬성 및 공조를 활용한 통합된 파이프라인을 활용하는 통합된 워크플로를 포함하고 있습니다.

지식 그래프를 언어 모델로 흡수할 때 반복적 분석, 모듈화, 병렬 검색 및 재귀적 향상에 중점을 둔 이상적인 다상 프로세스 워크플로를 제안합니다.

# 요약하자면

우리는 구조화된 지식 그래프를 대규모 언어 모델에 효과적으로 결합하여 통합된 추론 시스템을 만들기 위한 다층적 접근 방식을 탐색했습니다.

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

이 방법론은 상징적 지식 표현과 신경 의미 능력의 보완적인 강점을 결합합니다. 분석, 검색, 전파, 재계획, 흡수 및 설명의 반복적 주기를 통해 외부 구조를 유동적 의미 공간에 효율적으로 주입할 수 있도록 조심스럽게 조정됩니다.

이 워크플로우는 병렬 쿼리, 공유 벡터 임베딩 및 컨트롤러 아키텍처를 활용하여 모달리티를 시너지적으로 결합합니다. 자동화된 증가를 지향하지만 해석, 평가 및 조정 루프를 핵심적으로 유지하여 투명성과 품질에 대한 인간 감독을 보장합니다.

재능을 융합함으로써 언어 모델을 패턴 인식을 넘어 전문가 수준의 추론이 가능하면서도 표현력 있는 자연어 연결성을 갖춘 강력한 추론 엔진으로 발전시킬 수 있습니다.

# 기업용 추론 엔진

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

앞으로 볼 때, 이러한 지식이 풍부한 대형 언어 모델들은 기업 운영 내에서 다재다능한 추론 엔진으로 작용할 수 있을 것입니다.

그들은 정리된 산업 통찰력, 내부 통계, 규정, 전례 및 프로젝트 기억을 조립하는 확장 지식 그래프를 흡수합니다. 이 구조화된 데이터를 통해 학습된 벡터 임베딩은 텍스트 말뭉치와 결합되어 순조로운 접근을 가능하게 합니다.

그런 다음 에이전트들은 향상된 모델을 활용하여 자연 언어 또는 그래픽 인터페이스를 통해 제기된 맞춤형 추론 과제를 해결합니다. 이러한 과제에는 시장 반응을 예측하는 것, 공급망 흐름을 최적화하는 것, 후보 혁신을 식별하는 것, 법률 전략과 제품 위치 결정에 관한 조언을 하는 것도 포함됩니다.

시간이 지남에 따라 구체화된 워크플로우는 팀들이 구조적 논리와 맥락적 적응성을 갖춘 증강 인공지능에 의해 자동 분석하기 위해 복잡한 문제를 구성할 수 있도록 해줍니다. 새로운 지식의 지속적인 흡수가 연속적인 개선을 가능하게 합니다.

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

## The Bigger Picture

Stepping back, the answer to augmenting neural networks lies not in ever-increasing scale or data volumes, but in carefully cultivating connectivity to diverse reasoning talent. Only through coordinating specialization can capability compound.

The ideal type workflow offers a conceptual roadmap for productively conjoining graphical precision with textual expression into integrated systems that solve multifaceted challenges. More cohesive computation awaits this harmonious hybridization of neural and symbolic.

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

**AI 최고 책임자 & 아키텍트: Fribl에서 Neuro-Symbolic AI 시스템을 만드는 중인 GenAI 개선자**

# 일상어로 말해보자 🚀

In Plain English 커뮤니티의 일원이 되어주셔서 감사합니다! 떠나시기 전에:

- 저자를 좋아하고 팔로우하는 것을 잊지 마세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문: Stackademic | CoFeed | Venture
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기
