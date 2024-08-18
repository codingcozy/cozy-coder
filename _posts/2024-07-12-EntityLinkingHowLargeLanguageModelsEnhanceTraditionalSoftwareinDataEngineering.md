---
title: "엔티티 링크 대형 언어 모델이 전통적인 데이터 엔지니어링 소프트웨어를 향상시키는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-EntityLinkingHowLargeLanguageModelsEnhanceTraditionalSoftwareinDataEngineering_0.png"
date: 2024-07-12 23:23
ogImage:
  url: /assets/img/2024-07-12-EntityLinkingHowLargeLanguageModelsEnhanceTraditionalSoftwareinDataEngineering_0.png
tag: Tech
originalTitle: "Entity Linking: How Large Language Models Enhance Traditional Software in Data Engineering"
link: "https://medium.com/@alcarazanthony1/entity-linking-how-large-language-models-enhance-traditional-software-in-data-engineering-656364b7968c"
isUpdated: true
---

데이터 엔지니어링은 거인과 마주합니다. 구조화되지 않은 방대하고 혼돈스러운 데이터는 우리가 그것을 이해하는 능력에 도전을 제기합니다. 그 핵심에는 Entity Linking(EL)이 있습니다. 이는 텍스트 언급을 지식 베이스 항목에 연결하는 중요한 작업입니다. "워싱턴"에 대해 읽고 있다고 상상해보세요. 어떤 "워싱턴"인가요? 수도? 주 이름? 대통령 이름?

Entity Linking은 이러한 질문에 대답합니다. 이는 검색 엔진과 추천 시스템의 핵심입니다. 그러나 전통적인 방법들은 고전하고 있습니다. 데이터가 커지면서 도전도 커지고 있는 것입니다.

거대 언어 모델(LLM)이 등장합니다. 이 인공지능 시스템은 기존 소프트웨어 시스템을 위한 Entity Linking에서 혁명을 약속하고 있습니다. 특히 장꼬롤된 엔티티, 즉 종종 간과되는 드문한 이름에 대해 전례없는 정확성과 효율성을 제공합니다.

그러나 LLM만으로는 해결책이 아닙니다. 진정한 마법은 우리가 이들을 기존 소프트웨어와 결합할 때 일어납니다. 이 퓨전은 기존 방법이 부족한 곳에 그들의 능력을 통합합니다. 이는 전문 지식과 넓은 이해의 시너지입니다. 🌟

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

LLM은 어떻게 이런 성과를 이룰까요? 그들은 방대한 지식과 깊은 맥락 이해를 활용합니다. 데이터 엔지니어들과 연구자들은 이 힘을 이용하여 기존 방법을 능가하는 혁신적인 접근법을 개발하고 있습니다. 그 중 하나가 LLMAEL — LLM-증강 Entity Linking입니다.

LLMAEL은 LLM을 이용하여 entity 언급을 위한 풍부한 맥락적 설명을 생성합니다. 이러한 설명은 기존 EL 모델에 투입됩니다. 결과는 뭘까요? 특히 까다로운 long-tail entities의 경우, 정확도가 향상됩니다.

결과는 인상적입니다. 연구에 따르면, LLM-증강 entity linking이 기존 방법을 크게 능가합니다. 표준 벤치마크에서 LLMAEL은 모든 데이터셋에서 state-of-the-art 결과를 달성합니다. 이전 최고 결과보다 최대 3%까지 중의적 구별 정확도를 향상시킵니다. Long-tail entities의 경우, 이득은 더욱 두드러집니다 — 최대 5% 향상 가능합니다.

하지만 숫자만큼 중요한 것은 아닙니다. 이 LLM-소프트웨어 콤보는 워크플로우를 변화시킵니다. 프로세스를 가속화합니다. 데이터 엔지니어링에서 새로운 가능성을 개척합니다. 정보 검색부터 지식 그래프 구축에 이르는 실제 응용 사례에서, 그 영향력은 상당합니다. 더 정확한 entity linking은 더 나은 검색 결과, 더 포괄적인 지식 베이스, 그리고 더 정교한 추천 시스템으로 이어집니다.

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

`![Entity Linking 0](/assets/img/2024-07-12-EntityLinkingHowLargeLanguageModelsEnhanceTraditionalSoftwareinDataEngineering_0.png)`

`![Entity Linking 1](/assets/img/2024-07-12-EntityLinkingHowLargeLanguageModelsEnhanceTraditionalSoftwareinDataEngineering_1.png)`

# I. 전통 개체 링킹 방법의 한계

개체 링킹은 중요합니다. 자연어 처리와 정보 검색의 중심 요소입니다. 전통적인 방법은 잘 작동했습니다. 그러나 새로운 도전에 직면하고 있습니다. 현대의 데이터 환경은 방대하고 복잡합니다.

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

타로 카드 전문가시군요. 활동 프로세스를 알아봅시다:

- 개체명 인식 (NER): 텍스트에서 개체 언급 식별하기.
- 후보 생성: 지식 베이스에서 잠재적인 개체 목록 만들기.
- 개체 모호성 해소: 맥락을 기반으로 가장 가능성 있는 개체 선택하기.

이 방법론은 제약이 있습니다. 모호성은 큰 장벽이죠. 예를 들어 "Washington"이라는 단어를 생각해보세요. 이것은 많은 의미를 갖을 수 있습니다. 수도? 주 이름? 사람 이름? 기존의 전통적인 방법은 이러한 모호함에 어려움을 겪을 수 있습니다. 표면적인 특징과 제한된 맥락에 의존하기 때문에 세밀한 상황에서 오류가 발생할 수 있습니다.

희귀 개체들은 또 다른 중요한 도전을 제기합니다. 이들은 훈련 데이터나 지식 베이스에 빈번하지 않은 개체들입니다. 이들은 새로운 스타트업, 드문 과학 용어 또는 그냥 인기 없는 개체들일 수 있습니다. 전통적인 모델은 이러한 상황에서 종종 어려움을 겪습니다. 그 이유는 무엇일까요? 훈련 데이터의 부족, 지식 베이스 엔트리의 희소성, 그리고 독특한 맥락이 모두 역할을 합니다.

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

렌더링(Limited scalability and the need for efficiency are important considerations in data engineering. As knowledge bases expand, the process of pairwise comparisons between references and potential matches can become too computationally demanding. Additionally, keeping systems up-to-date can be quite a task given the constant evolution of knowledge bases. This often requires frequent system retraining or manual updates.

Traditional methods have their limitations when it comes to world knowledge. They heavily rely on training data and associated knowledge bases, but achieving human-level understanding often requires a broader range of cultural knowledge. For example, correctly linking the term "Big Apple" to refer to New York City necessitates an understanding of cultural context that traditional systems may lack.

Challenges also arise in the realms of cross-lingual and domain-specific linking. Effective cross-lingual linkage is crucial for global information sharing, but traditional methods may struggle with languages that have fewer resources. Domain-specific linking in fields such as biomedicine or law demands specialized knowledge, and adapting general-purpose systems to these areas can be resource-intensive.

Given these limitations, there is a need for more sophisticated approaches that can handle ambiguity and a wide array of entities. These methods should be able to scale effectively, integrate comprehensive world knowledge, and be adaptable to diverse linguistic and domain-specific contexts. Large Language Models show considerable promise in addressing these challenges and have the potential to transform entity linking in the field of data engineering.)

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

# II. 대형 언어 모델: Entity Linking을 위한 게임 체인저

대형 언어 모델(LLMs)이 Entity Linking을 혁신하고 있습니다. 자연어 처리에서 패러다임 변화를 나타내고 있습니다. GPT-3와 BERT와 같은 모델들이 게임을 바꾸고 있습니다. 방대한 양의 텍스트 데이터로 훈련된 이 신경망들은 놀라운 성능을 보여줍니다.

LLMs가 무엇을 할 수 있을까요? 이들은 문맥을 깊이 있게 이해합니다. 인간과 같은 텍스트를 손쉽게 생성합니다. 최소한의 파인 튜닝으로 다양한 언어 작업을 수행합니다. 그렇다면, 어떻게 LLMs가 Entity Linking을 변화시키는 걸까요?

LLMs는 트랜스포머 구조에 기반을 두고 있습니다. 수십억 개의 단어로 훈련됩니다. 이러한 방대한 훈련은 그들에게 중요한 능력을 부여합니다:

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

- 맥락 이해력: 그들은 미묘한 맥락과 장기간의 의존성을 파악합니다.

- 세계적인 지식: LLM은 방대한 암묵적인 지식을 축적합니다.

- 소럽트 학습: 그들은 최소한의 훈련으로 새로운 작업에 적응합니다.

- 다국어 능력: 많은 LLM은 여러 언어를 이해합니다.

이러한 능력은 전통적인 entity linking의 한계를 직면한 채로 해결합니다. 그 방법은 무엇인가요? LLM은 기존 방법을 여러 가지 방식으로 보완합니다.

그들은 맥락을 강화합니다. LLM은 Entity에 대한 풍부하고 관련성 있는 설명을 생성합니다. 이는 특히 모호한 경우에 대한 명확성을 향상시킵니다. 또한 후보 생성을 개선합니다. 넓은 지식을 통해 잠재적인 entity를 식별하는 데 도움이 되며, 특히 Long-tail entity의 경우에 특히 유용합니다. Entity 표현도 더 좋아집니다. LLM은 동적이고 맥락을 인식하는 표현을 생성합니다. 이는 전통적인 시스템에서 사용되는 정적임베딩을 능가합니다.

하지만 진정한 마법은 LLMAEL로 일어납니다. 이 접근 방식은 LLM을 전통적인 EL 모델과 결합합니다. 작동 방식은 다음과 같습니다:

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

먼저, LLM은 각 엔티티 언급에 대한 추가적인 맥락을 생성합니다. 그런 다음, 이 LLM이 생성한 맥락은 원본 텍스트와 융합됩니다. 마지막으로, 이 보강된 입력에 대해 세밀하게 조정된 향상된 EL 모델이 링킹을 수행합니다.

LLM을 활용한 Entity Linking 및 Augmentation (LLMAEL)은 핵심적인 문제에 도전합니다. 추가적인 맥락을 통해 모호성을 해결합니다. 정보성 높은 설명을 생성함으로써 장기적인 엔티티를 처리합니다. LLM에서 맥락을 보강자로 사용하여 확장성을 유지합니다. 그리고 생성된 맥락을 통해 세계적인 지식을 암시적으로 통합합니다.

## IV. 실용적인 응용

LLM으로 보강된 엔티티 링킹은 게임 체인저입니다. 그 영향은 다양한 분야에 걸쳐 넓고 깊게 미치고 있습니다. 함께 탐험해 보겠습니다:

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

- 정보 검색: 검색 엔진들이 더 똑똑해지고 있어요. 더 정확하고 맥락에 맞는 결과물을 제공하죠. 모호한 질문들? 문제없어요. 알 수 없는 개체들도 손쉽게 처리돼요.
- 지식 그래프 구축: 지식 그래프를 만들고 업데이트하는 일이 쉬워졌어요. 장거리 엔터티들도 잘 자리를 찾아가요. 과학 연구부터 비즈니스 인텔리전스까지, 응용 분야는 무궁무진해요.
- 콘텐츠 추천: 콘텐츠 이해가 한 단계 높아졌어요. 추천 시스템이 진화해요. 전자상거래, 미디어 스트리밍, 디지털 마케팅 — 모두 이 향상된 이해력에서 혜택을 봐요.
- 자연어 이해: 챗봇이 더 자연스럽게 대화해요. 가상 비서들이 더 똑똑해져요. 고객 서비스 자동화가 크게 향상돼요. 인간과 기계의 상호작용을 위한 새로운 시대죠.
- 다국어 정보 처리: 언어 장벽이 사라져요. 다국어 뉴스 집계가 번성하죠. 글로벌 비즈니스 인텔리전스 시스템이 번창해요. 세상이 더 작고 더 연결되어가요.
- 전문 분야 응용: 생명과학, 법률 연구, 기술 문서 — 모두 변화됐어요. 전문 용어들이 정확하게 연결돼요. 정보 처리가 전례없는 효율성에 이를 정도로 발전했어요.

실용적인 영향은 상당합니다. 비즈니스 인텔리전스가 감춰진 추세를 발굴해내죠. 놀랍도록 정확하게 잠재적인 파트너를 식별해내요. 과학 연구가 크게 발전하고, 문헌 리뷰가 보다 포괄적해져요. 지식 발견이 가속화되죠.

하지만 여전히 도전 과제들이 남아 있죠. 함께 살펴보죠:

- 계산 자원: LLM은 자원 투입이 많이 필요한 존재들이에요. 하지만 LLMAEL은 해결책을 제시해요. LLM을 맥락 생성에만 적절히 사용하죠.
- 개인정보 보호와 데이터 보안: 민감한 정보는 주의해서 다뤄져야 해요. 온프레미스나 조정된 작은 모델이 답일 수 있어요.
- 편향과 공정성: LLM이 기존 편향을 확대시킬 수 있어요. 경계가 필요해요. 특히 보호받아야 할 특성들에 대해 모니터링과 완화 전략이 중요해요.
- 설명 가능성: LLM은 성능을 향상시키지만 해석 가능성에 어느 비용이 드는 걸까요? 설명 방법을 개발하는 것이 시급한 연구 과제가 되었어요.

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

비록 이러한 장애가 있지만 미래는 밝습니다. LLM-강화 개체 링킹은 엄청난 잠재력을 보여줍니다. 데이터 엔지니어링의 경계를 넓혀나가고 있습니다. 앞으로의 여정은 흥미로우며 도전적이며 가능성이 가득합니다.

## 참고 자료:

[1] Amy Xin, Yunjia Qi, Zijun Yao, Fangwei Zhu, Kaisheng Zeng, Bin Xu, Lei Hou, Juanzi Li. “LLMAEL: 대형 언어 모델은 개체 링킹을 위한 좋은 문맥 보강자입니다.” arXiv 사전 인쇄 arXiv:2307.03928 (2023).
