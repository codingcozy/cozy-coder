---
title: "RAG vs Long-Context LLM 품질 평가 방법 및 결과 분석"
description: ""
coverImage: "/assets/img/2024-07-09-EvaluatingTheQualityOfRAGLong-ContextLLMOutput_0.png"
date: 2024-07-09 11:33
ogImage:
  url: /assets/img/2024-07-09-EvaluatingTheQualityOfRAGLong-ContextLLMOutput_0.png
tag: Tech
originalTitle: "Evaluating The Quality Of RAG , Long-Context LLM Output"
link: "https://medium.com/@cobusgreyling/evaluating-the-quality-of-rag-long-context-llm-output-89bc1a56a5d8"
isUpdated: true
---

![img](/assets/img/2024-07-09-EvaluatingTheQualityOfRAGLong-ContextLLMOutput_0.png)

# 소개

Long-Context LLM 출력물과 RAG 결과의 품질을 어떻게 측정할까요? SalesForce는 생성된 출력물의 정확성을 측정하기 위한 데이터셋과 프레임워크를 만들었습니다.

SalesForce는 반복되는 통찰력이나 신호가 포함된 문서 "건초더미(Haystacks)"를 생성하는 절차를 설계했습니다. "건초더미 요약(SummHay)" 작업은 시스템이 관련 통찰력을 식별하고 소스 문서를 인용하는 요약을 생성하도록 요구합니다.

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

정확한 통찰과 인용에 대한 지식을 가지고, Salesforce는 Coverage와 Citation에 대한 자동 평가 점수 요약을 구현했습니다.

Salesforce는 대화와 뉴스 도메인에서 Haystacks를 만들었고, 10개의 LLM과 50개의 RAG 시스템을 평가했습니다. 그 결과에 따르면, 최고 시스템들조차 인간의 성능(56%)에 10 포인트 이상 뒤쳐지는 것으로 나타났습니다.

# RAG 및 Long-Context 윈도우

SummHay는 기업 RAG 시스템과 장기 문맥 모델의 위치 편향을 연구하는 데에도 사용될 수 있습니다. Salesforce는 미래에 시스템이 SummHay에서 인간의 성능을 맞고 능가할 것으로 기대하고 있습니다.

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

이미지 태그를 Markdown 형식으로 변경합니다.

최근, RAG 및 긴 문맥 LLM은 대량의 텍스트 코퍼스에 대한 쿼리 응답 문제를 해결하려고 노력합니다. 그러나 공통 작업에 대한 직접적인 비교가 부족하여 평가가 어려운 상황입니다.

최근의 테스트에서 모델은 대형 문서에서 소량의 정보를 찾아야 하는 것이 요구됩니다. 그러나 이러한 작업들은 최신 대형 언어 모델의 성능을 식별하는 데 필요한 복잡성이 부족합니다. 왜냐하면 많은 최첨단 모델들이 거의 완벽한 성능을 달성하기 때문입니다.

# 요약

Salesforce는 요약 작업을 장문 맥락 모델 및 RAG 시스템을 평가하는 테스트베드로 활용하기를 제안합니다.

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

요약은 긴 맥락을 이해하고 내용의 상대적 중요성을 신중히 이해해야 합니다.

## 발견된 문제:

특히 요약의 관련성을 평가하는 이전 연구는 단일 문서 요약이나 입력 내용이 1,000~2,000 토큰 정도인 작업에 집중해 왔습니다.

하지만 대화식 대화와 다문서 뉴스 요약은 여전히 약 10,000 토큰 정도로 제한되어 있습니다.

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

요약 평가에서의 주요 문제 중 하나는 저품질의 참조 요약과 인간 판단과 잘 관련되지 않는 자동 측정기에 의존한다는 점입니다.

전통적인 평가는 후보 요약을 골드 표준 참조와 비교하여 오버랩이 높을수록 더 좋은 품질을 나타낸다고 가정합니다. 이 방법은 특히 고품질 참조가 얻기 어려운 장문 맥락 설정에서 신뢰할 수 없습니다. 심지어 컨텐츠 커버리지에 대한 최고의 자동 측정도 종종 인간 판단과 잘 관련되지 못합니다.

이러한 문제를 해결하기 위해 Salesforce가 합성 데이터 생성을 사용하고 있습니다.

아래 이미지를 고려해보면, Salesforce의 접근 방식은 특정 주제에 대한 대량의 문서('헤이스택')를 생성하고, 문서 전체에 일정 신호가 반복되도록 하는 것을 포함합니다.

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

Salesforce는 어떤 통찰이 어떤 문서에 나타나는지를 제어하여 검색 쿼리에 관련 통찰을 자동으로 결정할 수 있습니다. SummHay 작업은 이러한 통찰을 요약하고 출처를 인용하는 시스템의 작업을 필요로 합니다. 요약은 예상된 통찰의 범위와 소스 문서를 인용한 정확성을 기반으로 평가됩니다.

# Haystack 생성 절차

Haystack는 대화와 뉴스 기사 두 가지 도메인에서 생성됩니다.

일반적으로 Haystack는 약 100개의 문서를 포함하며, 총 약 10만 개의 토큰이 사용됩니다. Salesforce는 주제별로 총 10개의 Haystack을 생성하며, 각각 대략 10개의 쿼리가 함께 제공되어 총 92개의 SummHay 작업이 이뤄집니다. 이 파이프라인은 확장 가능하며 다른 도메인에도 적용될 수 있습니다.

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

# 평가 방법

SummHay 평가 방법은 주로 시스템 출력물이 참조 통찰력을 얼마나 잘 다루는지, 그리고 인용의 품질을 평가하는 데 초점을 맞춥니다. 숙련된 주석가들 사이에서 프로토콜의 일관성이 뛰어나다는 것이 수동 주석을 통해 확인되었습니다 (0.77 상관 관계).

Salesforce는 이후 LLM 기반 평가를 실험하여 상관 관계 수준이 약간 낮아졌지만(0.71), 평가 비용은 거의 50배로 줄어든다는 것을 발견했습니다.

# 인간의 성능 평가 추정

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

Salesforce가 SummHay에서 인간의 성능 추정치를 수립하고 50개의 RAG 시스템 및 10개의 장기적 문맥 LLM을 대규모로 평가했습니다.

## 그들의 연구 결과는 다음과 같습니다:

- 모든 평가된 시스템에게 SummHay가 어려운 과제임을 나타내며, 어떠한 모델도 인간 수준의 성능을 달성하지 못했습니다. 이는 문서의 관련성에 대한 완벽한 지표인 오라클 시그널을 제공해도 마찬가지입니다.
- 이 우위를 갖고 있음에도 불구하고, 모델들은 여전히 인간의 퍼포먼스에 크게 미치지 못하며 통찰을 요약하고 출처를 정확하게 인용하는 데 어려움을 겪고 있습니다.
- RAG (검색 보완 생성) 파이프라인과 장기적 문맥 LLM (대규모 언어 모델) 중에 선택할 때 고려해야 할 중요한 트레이드오프가 있습니다.
- RAG 시스템은 일반적으로 더 나은 인용 품질을 제공하며, 특정 문서나 출처를 더 정확하게 참조할 수 있습니다.
- 그러나 이는 종종 모든 관련 정보를 포괄적으로 포착하고 요약하는 능력인 통찰력 커버리지를 희생해야 한다는 것을 의미합니다.
- 반면, 장기적 문맥 LLM은 통찰을 더 철저하게 다룰 수도 있지만 정확한 인용에 어려움을 겪을 수 있습니다.
- 고급 RAG 구성요소 (예: 재순위)를 사용하면 작업의 종단 간 성능 향상을 이끌 수 있으며, SummHay가 전체적인 RAG 평가에 대한 유망한 선택지임을 확인합니다.
- SummHay에 대한 위치 편향 실험은 중간에서의 손실 현상을 확인하여 대부분의 LLM이 문맥 창의 상단이나 하단의 정보에 편향된다는 것을 입증했습니다.

➡️ LinkedIn에서 언어 및 인공지능 교차점에 대한 업데이트를 받으려면 팔로우해 주세요.

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

**이미지1**
저는 현재 코레 AI의 최고 선구자입니다. AI와 언어가 교차하는 모든 것에 대해 탐구하고 쓰고 있습니다. LLM, 챗봇, 보이스봇, 개발 프레임워크, 데이터 중심의 잠재 공간 등 다양한 주제를 다룹니다.

**이미지2**

**이미지3**

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

![Click Here](/assets/img/2024-07-09-EvaluatingTheQualityOfRAGLong-ContextLLMOutput_4.png)
