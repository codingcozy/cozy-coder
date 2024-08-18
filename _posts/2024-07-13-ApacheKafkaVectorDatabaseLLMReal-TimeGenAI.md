---
title: "Apache Kafka  Vector Database  대규모 언어 모델LLM  실시간 생성 AI 만들기"
description: ""
coverImage: "/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_0.png"
date: 2024-07-13 02:43
ogImage:
  url: /assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_0.png
tag: Tech
originalTitle: "Apache Kafka + Vector Database + LLM = Real-Time GenAI"
link: "https://medium.com/@kai-waehner/apache-kafka-vector-database-llm-real-time-genai-4b5b6e687d85"
isUpdated: true
---

**GenAI(Generative AI)**는 고급 AI 사례와 혁신을 가능하게 하지만 기업 아키텍처가 어떻게 변화하는지를 바꿉니다. **Large Language Models (LLM)**, 벡터 데이터베이스, Retrial Augmentation Generation (RAG)은 새로운 데이터 통합 패턴과 데이터 엔지니어링 Best Practice를 필요로 합니다. **Apache Kafka**와 **Apache Flink**를 활용한 데이터 스트리밍은 대규모 실시간 데이터의 수집과 정제에 핵심적인 역할을 합니다. 이를 통해 다양한 데이터베이스와 분석 플랫폼을 연결하고 독립적인 비즈니스 부서와 데이터 제품을 분리하는데 도움을 줍니다. 이 블로그 게시물은 이벤트 스트리밍과 전통적인 요청-응답 API 및 데이터베이스 사이의 가능한 아키텍처, 예시, 그리고 trade-offs에 대해 탐구합니다.

[Link to the blog post](https://kaiwaehner.github.io/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_0.png)

(카이 베너의 블로그에서 원본 포스팅: "Apache Kafka + Vector Database + LLM = Real-Time GenAI"... 데이터 스트리밍 커뮤니티에 가입하고 새 블로그 포스트에 대한 최신 정보를 구독하세요)

# Apache Kafka와 GenAI를 활용한 사용 사례

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

Generative AI (GenAI)은 자연 언어 처리(NLP), 이미지 생성, 코드 최적화 및 기타 작업을 위한 차세대 AI 엔진입니다. 실제 세계에서 많은 프로젝트들을 돕는데, 서비스 데스크 자동화, 챗봇을 통한 고객 대화, 소셜 네트워크에서의 콘텐츠 관리 등에 사용됩니다.

아파치 카프카는 다양한 데이터 원본을 통합하고 규모화된 처리, 실시간 모델 추론을 위한 기계 학습 플랫폼에서 주도적인 조정 계층이 되었습니다.

카프카와의 데이터 스트리밍은 이미 많은 GenAI 인프라 및 소프트웨어 제품을 지원하고 있습니다. 다양한 시나리오들이 가능합니다:

- 전체 기계 학습 인프라의 데이터 패브릭으로서의 데이터 스트리밍
- 실시간 예측을 위한 스트림 처리를 통한 모델 스코어링 및 콘텐츠 생성
- 입력 텍스트, 음성 또는 이미지로 구성된 스트리밍 데이터 파이프라인 생성
- 대형 언어 모델의 실시간 온라인 훈련

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

이러한 사용 사례를 탐색했습니다. Expedia, BMW 및 Tinder와 같은 실제 예시를 포함하여 "GenAI를 위한 미션 크리티컬 데이터 퍼브릭으로 Apache Kafka"라는 블로그 게시물에서 다루었습니다.

다음은 대형 언어 모델 (LLM), 검색 보강 생성 (RAG)과 벡터 데이터베이스 및 시맨틱 검색을 결합한 구체적인 아키텍처와 Apache Kafka 및 Flink를 사용한 데이터 스트리밍에 대해 살펴봅니다.

# 생성적 AI가 전통적인 기계 학습 아키텍처와 어떻게 다른가요?

기계 학습 (ML)은 컴퓨터가 숨겨진 통찰을 찾도록 허용하며 어디를 살펴볼지 프로그래밍되지 않았습니다. 이를 모델 교육이라고하며, 대용량 데이터 세트를 분석하는 일괄 처리 과정입니다. 결과는 이진 파일이며, 분석 모델입니다.

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

새로 들어오는 이벤트에 이러한 모델을 적용하여 예측을 하는 것을 말합니다. 이를 모델 스코어링이라고 하며, 실시간으로 발생할 수도 있고 모델을 애플리케이션에 삽입하거나 모델 서버로 요청-응답 API 호출을 통해 배치로도 실행될 수 있습니다.

그러나 LLMs와 GenAI는 전통적인 머신러닝 프로세스와 달리 다른 요구사항과 패턴을 갖고 있습니다. 제 전 동료인 Michael Drogalis가 두 가지 간단하고 명확한 다이어그램으로 설명한 것처럼요.

## 복잡한 데이터 엔지니어링이 필요한 전통적인 예측적 머신러닝

예측 인공지능은 예측을 수행합니다. 목적에 맞게 설계된 모델. 오프라인 훈련. 이것이 지난 10년 동안 우리가 머신러닝을 수행한 방식입니다.

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

전통적인 ML에서는 데이터 엔지니어링 작업의 대부분이 모델 생성 시간에 발생합니다. 특성 엔지니어링과 모델 훈련에는 많은 전문 지식과 노력이 필요합니다.

![image](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_1.png)

새로운 사용 사례는 데이터 엔지니어 및 데이터 과학자에 의해 구축된 새로운 모델을 필요로 합니다.

## 대형 언어 모델 (LLM)을 활용한 생성적 AI로 AI의 민주화

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

Generative Artificial Intelligence (GenAI)는 콘텐츠를 생성합니다. 재사용 가능한 모델들이에요. 문맥을 파악하며 학습하는 거죠.

그런데 대형 언어 모델을 사용하면 매번 쿼리할 때마다 데이터 엔지니어링이 발생해요. 여러 응용 프로그램이 같은 모델을 재사용하기 때문에 문제가 발생할 수 있어요.

![이미지](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_2.png)

# GenAI 사용 사례에서의 대규모 언어 모델의 과제들

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

Large Language Models (LLMs)는 재사용이 가능합니다. 이는 AI의 민주화를 가능하게 합니다. 왜냐하면 모든 팀이 AI 전문 지식을 요구하지 않기 때문이죠. 대신, 낮은 AI 전문 지식만 있어도 기존 LLM을 사용할 수 있습니다.

그러나 LLM의 몇 가지 큰 희생이 존재합니다:

- 비싼 훈련 비용: ChatGPT와 같은 LLM은 컴퓨팅 리소스로 수백만 달러가 듭니다 (이것은 모델을 구축하는 데 필요한 전문 지식 비용을 포함하지 않습니다)
- 정적 데이터: LLM은 "시대에 갇혀" 있어서 모델이 최신 정보를 갖고 있지 않다는 의미입니다.
- 도메인 지식의 부족: LLM은 일반 데이터 세트에서 학습하는 경우가 많습니다. 따라서 데이터 엔지니어들은 전 세계 웹에서 정보를 가져와 모델 훈련에 공급합니다. 그러나 기업은 비즈니스 가치를 제공하기 위해 자신의 맥락에서 LMM을 사용해야 합니다.
- 멍청함: LLM은 인간처럼 지능적이지 않습니다. 예를 들어, ChatGPT는 사용자가 제시한 문장의 단어 수를조차 세어 주지 못합니다.

이러한 도전은 이른바 환각을 일으킵니다...

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

## 환각을 피하고 신뢰할 만한 답변 얻기

환각, 즉 최상의 추측적인 답변은 결과물입니다. 그리고 LLM은 이것이 만들어진 것이라고 알려주지 않습니다. 환각은 인공지능 모델이 실제 데이터나 정보를 기반으로 하지 않고, 완전히 가공되거나 현실적이지 않은 결과물을 생성하는 현상입니다. 환각은 텍스트나 이미지 생성기와 같은 생성 모델이, 입력 데이터나 맥락과 무관하거나 코호런트하지 않은, 사실적이지 않거나 관련이 없는 콘텐츠를 생성할 때 발생할 수 있습니다. 이러한 환각은 모델에 의해 완전히 공상된 텍스트, 이미지 또는 기타 유형의 콘텐츠로 나타날 수 있습니다.

환각은 생성 적 인공지능에서 문제가 될 수 있습니다. 왜냐하면 그것들은 오인도 나거나 거짓 정보의 생성으로 이어질 수 있기 때문입니다.

이러한 이유로 새로운 디자인 패턴이 생성 적 인공지능을 위해 등장했습니다: 검색 증강 생성(RAG). 이 새로운 모법을 먼저 살펴보고, 그런 다음 Apache Kafka, Flink와 같은 기술로 데이터 스트리밍을 수행하는 것이 GenAI 엔터프라이즈 아키텍처에 대한 기본 요구 사항인 이유를 살펴봅시다.

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

# Semantic Search and Retrieval Augmented Generation (RAG)

카드: **흐름이 힘을 받다**

GenAI를 활용하는 많은 애플리케이션들이 내용이 정확하고 최신인 인플루언서를 함께 사용해서 검색증강생성(Retrieval Augmented Generation, RAG)의 디자인 패턴을 따라갑니다. Pinecone 팀은 완전 관리되는 벡터 데이터베이스로 이 용도에 적합한 아름답게 설명한 다이어그램을 가지고 있습니다:

![Pinecone Explanation](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_3.png)

출처: Pinecone

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

높은 수준에서, RAG는 일반적으로 두 단계로 구성됩니다. 첫 번째는 데이터 증강 단계로, 종종 구조화되지 않은 운영 데이터를 청크로 나눈 다음 임베딩 모델을 사용하여 임베딩을 생성합니다. 이 임베딩은 벡터 데이터베이스에 색인화됩니다. 벡터 데이터베이스는 정확한 키워드 일치가 필요하지 않은 프롬프트에 관련 컨텍스트를 찾기 위한 의미론적 검색을 가능하게 하는 도구입니다.

두 번째는 추론 단계인데, GenAI 모델이 질문과 컨텍스트를 받아 신뢰할 수 있는 답변을 생성합니다(환각 없이). RAG는 임베딩을 업데이트하지 않지만, 관련 정보를 검색하여 프롬프트와 함께 LLM으로 전송합니다.

# 임베딩을 사용한 의미론적 검색을 위한 벡터 데이터베이스

벡터 데이터베이스, 또는 벡터 저장소 또는 벡터 인덱스로도 알려진 것은 벡터 데이터를 효율적으로 저장하고 검색하기 위해 특별히 설계된 데이터베이스 유형입니다. 이 맥락에서 벡터 데이터란 숫자 벡터의 모음을 의미하며, 텍스트, 이미지, 오디오 또는 기타 구조화되지 않은 데이터의 임베딩과 같은 다양한 데이터 유형을 표현할 수 있습니다. 벡터 데이터베이스는 기계 학습, 데이터 검색, 추천 시스템, 유사성 검색 등과 관련된 응용 프로그램에서 유용합니다.

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

벡터 데이터베이스는 유사도 검색, 종종 의미 검색이라고도 불리는 작업에서 우수한 성과를 보여줍니다. 코사인 유사도나 유클리드 거리와 같은 다양한 유사성 측정 기준을 기반으로 주어진 쿼리 벡터와 유사하거나 가까운 벡터를 빠르게 찾아낼 수 있습니다.

벡터 데이터베이스는 (필수적으로) 별도의 데이터베이스 범주가 아닙니다. Gradient Flow는 '검색 증강 생성을 위한 최선의 모범 사례'에서 다음을 설명합니다:

"벡터 검색은 더 이상 벡터 데이터베이스에만 제한되지 않습니다. 많은 데이터 관리 시스템 — PostgreSQL을 비롯한 — 이제 벡터 검색을 지원합니다. 특정 응용 프로그램에 따라, 개별 요구 사항을 충족하는 시스템을 찾을 수 있습니다. 실시간 또는 스트리밍이 우선 사항인가요? Rockset의 옵션을 확인해보세요. 이미 지식 그래프를 사용하고 계신가요? Neo4j의 벡터 검색 지원은 RAG 결과의 설명과 시각화를 더욱 간단하게 만들어줍니다."

또 다른 구체적인 예로, MongoDB의 "Atlas Vector Search 및 오픈 소스 모델의 파워 활용: MongoDB를 활용한 생성적 AI 애플리케이션 구축" 튜토리얼을 살펴보세요. Apache Kafka와 벡터 데이터베이스를 결합하는 다양한 옵션이 있습니다. 다음은 이벤트 중심 환경에서 가능한 아키텍처입니다.

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

# 이벤트 주도 아키텍처: 데이터 스트리밍 + 벡터 데이터베이스 + LLM

이벤트 주도 애플리케이션은 검색 보강 생성 (RAG) 단계의 데이터 보강 및 모델 추론을 더 효과적으로 구현할 수 있습니다. 아파치 카프카와 아파치 플링크를 이용한 데이터 스트리밍은 데이터의 일관된 동기화를 가능하게 하며, 규모에 관계없이 데이터의 실시간 처리를 지원하며 데이터 큐레이션 (= 스트리밍 ETL)을 수행합니다.

다음 다이어그램은 이벤트 주도 데이터 스트리밍을 활용한 기업 아키텍처를 보여주며, GenAI 파이프라인 전체에서 데이터 수집 및 처리에 활용됩니다:

\[ ![다이어그램](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_4.png) \]

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

이 예시는 실시간으로 항공편 예약 및 변경 정보를 Kafka의 이벤트 스토어에 실시간으로 수집하여 GenAI 기술을 사용하여 후속 처리를 위해 데이터를 처리합니다. Flink는 데이터를 사전 처리한 후 벡터 데이터베이스를 위한 임베딩 모델을 호출합니다. 동시에 Python으로 구축된 실시간 고객 서비스 응용 프로그램은 모든 관련 문맥 데이터(예: 항공편 데이터, 고객 데이터, 임베딩 등)를 사용하여 대규모 언어 모델을 유도합니다. LLM은 신뢰할 수 있는 예측을 생성하며, 예를 들어 승객을 다른 항공편으로 재예약할 것을 권장합니다.

대부분의 기업 시나리오에서 모든 처리는 기업 방화벽 뒤에서 보안 및 데이터 개인 정보 보호 이유로 작동합니다. LLM은 예약 엔진과 같은 트랜잭션 시스템과 통합되어 재예약을 실행하고 결과를 관련 응용 프로그램 및 데이터베이스에 피드합니다.

# API 대 이벤트 기반 데이터 스트리밍에 대한 요청-응답

이상적인 세계에서는 모든 것이 이벤트 기반 및 데이터 스트리밍됩니다. 현실세계는 다릅니다. 따라서 기업 아키텍처의 일부에서는 요청-응답 형식의 API 호출 (HTTP/REST 또는 SQL)이 완전히 적합합니다. Kafka가 시스템을 완전히 분리하기 때문에 각 응용 프로그램은 자체 통신 패러다임과 처리 속도를 선택합니다. 따라서 HTTP/REST API와 Apache Kafka 사이의 Trade-offs를 이해하는 것이 중요합니다.

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

아파치 카프카에서 Request-Response를 사용해야 하는 시기는 언제일까요? 이 결정은 주로 대기 시간, 결합도, 또는 보안과 같은 트레이드오프에 따라 내려집니다. 그러나 대규모 LLM(Large Language Models)의 경우 상황은 달라집니다. LLM을 훈련하는 데 매우 비용이 많이 드는만큼 기존 LLM의 재사용성이 중요합니다. 그리고 LLM을 Kafka Streams나 Flink 애플리케이션에 포함하는 것은 의사결정 트리, 클러스터링 또는 작은 신경망과 같은 다른 모델과는 다르게 별로 타당하지 않습니다.

비슷하게, 보강 모델은 보통 RPC/API 호출을 통해 통합됩니다. 이를 Kafka Streams 마이크로서비스나 Flink 작업에 포함시킨다면 보강 모델은 긴밀하게 결합됩니다. 그리고 오늘날 전문가들이 많은 이유 중 하나는 이들을 운영하고 최적화하는 것이 간단하지 않기 때문입니다.

LLM 및 보강 모델을 호스팅하는 솔루션은 일반적으로 HTTP와 같은 RPC 인터페이스만을 제공합니다. 앞으로 스트리밍 데이터에 대해 request-response가 안티패턴이기 때문에 이것은 아마도 변경될 것입니다. 모델 서버의 진화에 대한 훌륭한 예시로는 Seldon이 있습니다. 한편, Kafka 네이티브 인터페이스를 제공하고 있습니다. Kafka 네이티브 모델 배포를 통해 Streaming Machine Learning에 대한 request-response 대 스트리밍 모델 서빙을 읽어보세요.

# LLM과 기업 전체의 직접적 통합?

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

이 기사를 쓰는 동안 OpenAI가 GPTs를 발표했습니다. 이제 사용자 정의 버전의 ChatGPT를 만들 수 있게 되었는데, 이는 지침, 추가 지식, 그리고 다양한 스킬을 결합할 수 있습니다. 기업용으로 가장 흥미로운 기능은 개발자들이 OpenAI의 GPT를 실제 세계, 즉 다른 소프트웨어 응용프로그램, 데이터베이스 및 클라우드 서비스에 연결할 수 있다는 것입니다.

“내장된 기능을 사용하는 것 외에도, 하나 이상의 API를 GPT에 제공하여 사용자 정의 작업을 정의할 수도 있습니다. 플러그인과 같이, 작업은 GPT가 외부 데이터를 통합하거나 실제 세계와 상호 작용할 수 있게 합니다. GPT를 데이터베이스에 연결하거나 이메일에 통합하거나 쇼핑 어시스턴트로 활용할 수 있습니다. 예를 들어, 여행 목록 데이터베이스를 통합하거나 사용자 이메일 수신함을 연결하거나 전자상거래 주문을 처리할 수 있습니다.”

직접 통합을 사용하는 것의 단점은 강한 결합과 점대점 통신입니다. 이미 Kafka를 사용 중이라면, 도메인 주도 설계와 진정한 분리의 가치를 이해하고 있을 것입니다.

![Apache Kafka Vector Database](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_5.png)

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

To learn more about the importance and value of true decoupling in a microservice or data mesh architecture, take a look at these insightful articles:

- _Apache Kafka vs. Enterprise Service Bus (ESB) — Friends, Enemies or Frenemies?_
- _Why Data Streaming differs from Data Integration with integration Platform as a Service (iPaaS) Solutions_

And don't forget, when it comes to Public GenAI APIs and LLMs, it's important to note that they may have a weak security and governance strategy. As AI data needs grow and the number of integrations increase, challenges related to data access, lineage, and security can become more complex.

Exciting times ahead as we delve into the world of _Data Streaming with Kafka, Flink and GenAI in Practice_! Let's embrace the journey together.

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

이론을 많이 살펴본 후 실제 예제, 데모 및 GenAI와 데이터 스트리밍을 결합한 실제 사례를 살펴보겠습니다.

- 예시: Flink SQL + OpenAI API
- 데모: ChatGPT 4 + Confluent Cloud + RAG 및 벡터 검색을 위한 MongoDB Atlas
- 성공 사례: Elemental Cognition — Confluent Cloud를 통해 구동되는 실시간 AI 플랫폼

# 예시: Flink SQL + OpenAI API

카프카와 Flink를 사용한 스트림 처리는 실시간 및 과거 데이터의 데이터 상관관계 분석을 가능하게 합니다. 특히 생성적 AI에 대해 우수한 예시는 맥락별 고객 서비스입니다. 여기서는 항공사 예제와 항공편 취소에 머무릅니다.

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

한 가지 주목해야 할 점은 상태 유지 형 스트림 프로세서가 CRM, 충성도 플랫폼 및 기타 응용 프로그램에서 기존 고객 정보를 가져와 고객이 채팅 봇에서 쿼리한 정보와 연관 지어 LLM에 RPC 호출을 수행한다는 것입니다.

아래 다이어그램은 Apache Flink와 Flink SQL 사용자 정의 함수(UDF)를 사용합니다. SQL 쿼리가 전처리된 데이터를 OpenAI API로 전달하여 신뢰할 수 있는 답변을 받습니다. 이 답변은 다른 Kafka 주제로 전송되어 하위 응용 프로그램이 해당 정보를 활용할 수 있으며, 예를 들어 티켓 재예약, 충성도 플랫폼 업데이트, 데이터를 데이터 레이크에 저장하여 나중에 일괄 처리 및 분석에 활용합니다.

**데모: ChatGPT 4 + Confluent Cloud + MongoDB Atlas를 사용한 RAG 및 벡터 검색**

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

내 동료인 브릿튼 라로치는 카프카를 사용한 데이터 통합 및 처리, 몽고디비를 사용한 저장 및 의미 벡터 검색의 조합을 보여주는 환상적인 소매 데모를 구축했습니다. D-ID는 AI 비디오 생성 플랫폼으로서 명령 줄 인터페이스 대신 시각적 AI 아바타로 더 아름답게 만들어줍니다.

![2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_7.png](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_7.png)

Confluent Cloud와 MongoDB Atlas와 같은 완전 관리되고 깊게 통합된 서비스를 통해 비즈니스 로직 구축에 집중할 수 있습니다.

이 아키텍처는 내가 위에서 소개한 이벤트 기반 스트리밍 예제와 다릅니다. 여전히 응용 프로그램을 완전히 분리하기 위해 핵심은 카프카입니다. 대부분의 서비스는 요청-응답 API를 통합합니다. 이 방법은 간단하며 이해하기 쉬우며 종종 충분합니다. 그리고 나중에 Python Kafka API, Kafka의 변경 데이터 캡처 (CDC), LangChain Python UDF를 Apache Flink에 삽입하거나 AsyncAPI와 같은 비동기 인터페이스를 사용하여 이벤트 기반 패턴으로 쉽게 마이그레이션할 수 있습니다.

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

다음은 MongoDB Atlas를 사용하여 RAG 및 의미 검색을 통해 실시간 GenAI 플랫폼인 Elemental Cognition의 데모를 안내하는 짧은 다섯 분 데모입니다. 이 데모에서는 통합 허브로서 Confluent 및 최종 사용자와의 통신 인터페이스로 D-ID를 활용합니다.

# 성공 사례: Elemental Cognition — 카프카와 Confluent Cloud를 기반으로 한 실시간 GenAI 플랫폼

2015년 IBM의 혁신적인 왓슨 기술의 발명가이자 유명한 AI 연구자인 David Ferrucci 박사가 Elemental Cognition을 설립했습니다. 이 회사는 신뢰, 정확도 및 투명성이 중요한 결정을 가속화하고 개선하기 위해 GenAI를 활용합니다.

Elemental Cognition 기술은 다양한 산업 및 사용 사례에 활용될 수 있습니다. 주요 대상은 의료 / 생명 과학, 투자 관리, 정보 수집, 물류 및 일정 관리, 그리고 고객 센터 등입니다.

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

AI 플랫폼은 문제 해결을 돕고 이해하고 신뢰할 수 있는 전문 지식을 제공하는 책임 있는 투명한 AI를 개발합니다.

Elemental Cognition의 방식은 혁신적인 아키텍처를 통해 다양한 AI 전략을 결합하여 인간이 이해할 수 있는 지식을 획득하고 추론하여 문제를 협력적으로, 동적으로 해결합니다. 그 결과로 전문 문제 해결 지능이 대화 및 탐색 애플리케이션으로 더 투명하고 비용 효율적으로 전달됩니다.

Confluent Cloud는 AI 플랫폼을 지원하여 확장 가능한 실시간 데이터 및 데이터 통합 사례가 가능합니다. 다양하고 인상적인 사용 사례에서 배울 수 있는 웹사이트를 살펴보시기를 추천합니다.

![Kafka](/assets/img/2024-07-13-ApacheKafkaVectorDatabaseLLMReal-TimeGenAI_8.png)

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

# 아파치 카프카: 젠AI 기업 아키텍처의 중추신경계

젠AI(Generative AI)는 AI/ML 기업 아키텍처에 변화를 요구합니다. 증가 모델, LLMs, RAG 및 벡터 데이터베이스와 의미 검색을 위해서는 데이터 통합, 상관 및 분리가 필요합니다. 카프카와 플링크를 활용한 데이터 스트리밍이 여기 도움이 됩니다.

많은 애플리케이션 및 데이터베이스는 REST/HTTP, SQL 또는 다른 인터페이스를 사용한 요청-응답 통신을 하고 있습니다. 이것은 전혀 문제가 되지 않습니다. 데이터 제품 및 애플리케이션을 위해 데이터 일관성을 유지하면서 적절한 기술 및 통합 계층을 선택하세요.

아파치 카프카와 아파치 플링크를 활용한 데이터 스트리밍은 서로 다른 도메인을 완전히 분리함으로써 개발자와 데이터 엔지니어가 비즈니스 문제에 집중할 수 있게 도와줍니다. HTTP, 카프카 API, AsyncAPI, 데이터베이스로부터의 CDC, SaaS 인터페이스 및 기타 다양한 옵션을 통해 카프카와의 통합이 가능합니다.

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

카프카는 어떤 통신 패러다임으로도 시스템을 연결하는 것을 가능케 합니다. 이벤트 저장소는 데이터를 밀리초 단위로 공유하지만 데이터를 천천히 처리해야 하는 하위 응용 프로그램과 과거 데이터를 다시 재생하는 데도 데이터를 영속화합니다. 데이터 메시의 핵심은 실시간으로 운용되어야 합니다. 어떠한 훌륭한 기업 아키텍처에도 해당하는 내용이죠. GenAI도 마찬가지입니다.

아파치 카프카를 활용하여 대화형 AI, 챗봇 및 다른 GenAI 애플리케이션을 어떻게 구축하는지 궁금하지 않으신가요? RAG를 Flink와 벡터 데이터베이스로 실시간으로 구축하여 LLM에 적절한 컨텍스트를 제공했나요? LinkedIn에서 연결해 상의해보는 건 어떨까요? 뉴스레터를 구독하여 새로운 블로그 글에 대한 업데이트를 받아보세요!
