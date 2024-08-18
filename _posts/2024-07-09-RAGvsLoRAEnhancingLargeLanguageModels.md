---
title: "대형 언어 모델을 향상시키는 방법 RAG vs LoRA 비교"
description: ""
coverImage: "/assets/img/2024-07-09-RAGvsLoRAEnhancingLargeLanguageModels_0.png"
date: 2024-07-09 10:14
ogImage:
  url: /assets/img/2024-07-09-RAGvsLoRAEnhancingLargeLanguageModels_0.png
tag: Tech
originalTitle: "RAG vs. LoRA: Enhancing Large Language Models"
link: "https://medium.com/@prglabs/rag-vs-lora-enhancing-large-language-models-ec1a5038e639"
isUpdated: true
---

![RAG](/assets/img/2024-07-09-RAGvsLoRAEnhancingLargeLanguageModels_0.png)

# 소개

대형 언어 모델(LLM)이 계속 발전함에 따라, 연구자들과 실무자들은 그 성능, 적응성 및 효율성을 향상시키는 방법을 지속적으로 탐구하고 있습니다. 주목받고 있는 두 가지 접근법은 소환-증강 생성(RAG)과 저랭크 적응(LoRA)입니다. 두 기술은 LLM 기능을 향상시키기 위해 노력하지만, 기본적으로 서로 다른 메커니즘을 통해 이루어집니다. 이 기사에서는 RAG와 LoRA 기술을 비교하여 그 방법론, 장단점, 한계 및 사용 사례를 탐색해보겠습니다.

# 소환-증강 생성(RAG)

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

**Retrieval-Augmented Generation (RAG)**은 외부 지식 검색을 통해 대형 언어 모델의 능력을 향상시키는 인기 있는 방법입니다. RAG에는 두 가지 주요 구성 요소가 있습니다: **Retriever**(검색기)는 지식 베이스에서 관련 정보를 검색하고 검색합니다. 그리고 **Generator**(생성기)는 입력 및 검색된 컨텍스트를 기반으로 텍스트를 생성하는 언어 모델입니다.

**주요 이점**

- 향상된 정확도: 답변을 사실적인 정보로 근거 지어줍니다.
- 최신 지식: 모델의 훈련 데이터 외의 최신 정보에 접근할 수 있습니다.
- 환상 감소: 거짓이나 미지원되는 정보 생성을 최소화합니다.
- 유연성: 지식 베이스를 업데이트할 수 있고 전체 모델을 재훈련하지 않아도 됩니다.

**지식 베이스 구축**

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

RAG 시스템은 다양한 아키텍처와 기술을 사용하여 설계됩니다. 이에는 문서 저장을 위한 데이터베이스 (예: Elasticsearch 또는 MongoDB), 임베딩 관리를 위한 벡터 데이터베이스 (Pinecone, Weaviate, 또는 Kinetica), 클라우드 저장소 (AWS S3 또는 Google Cloud Storage) 등이 포함됩니다. 텍스트 전처리 도구로는 언어 감지기, NLTK, spaCy, 그리고/또는 HuggingFace Tokenizers가 사용되며, 임베딩 모델(예: 트랜스포머), 밀집 검색을 위한 검색 시스템(FAISS, Annoy, HNSW), 희소 검색(BM25, TF-IDF) 또는 밀집 및 희소 방법을 결합한 하이브리드 접근 방식 등이 활용됩니다. 언어 생성 모델로는 트랜스포머 기반 모델(예: GPT 시리즈, T5, BART) 뿐만 아니라 LLaMA, BLOOM, OPT 등의 오픈 소스 모델과 OpenAI GPT, Anthropic Claude, Google PaLM과 같은 상용 API도 사용됩니다. 이와 같은 다양한 구성 요소들을 연결해주는 통합 프레임워크로는 LangChain, RAG 시스템을 구축하는 데 사용되는 종단 간 프레임워크인 Haystack, 또는 Python, TensorFlow 또는 PyTorch와 같은 커스텀 프레임워크 등이 있습니다. 마지막으로는 Lucene 기반(예: Elasticsearch, Apache Solr)이나 벡터 검색(Faiss, Annoy, ScaNN)과 같은 기술을 사용한 색인 및 검색 능력이 제공됩니다.

## 검색 및 검색

지식 베이스가 편성되고 색인화된 후에는 검색 및 검색에 초점을 맞춥니다. 검색 엔진을 구축하는 데 사용되는 일반적인 구성 요소에는 다음이 포함됩니다:

- 쿼리 처리: 명명된 엔티티 인식(NER), 의도 분류, 동의어, 관련 용어
- 재순위 결정: 세밀한 관련성 점수를 위한 Cross-encoders 및/또는 러닝 투 랭크(LTR) 모델
- 캐싱: Redis 또는 Memcached와 같은 인메모리 캐시 및 Hazelcast나 Apache Ignite와 같은 분산 캐싱 도구 사용
- 모니터링 및 로깅: Prometheus, Grafana 및 ELK 스택(Elasticsearch, Logstash, Kibana)을 비롯한 여러 지표 및 로깅 도구
- 배포 및 확장: Docker, Kubernetes, AWS, Google Cloud, Azure
- API 레이어: FastAPI, Flask 또는 Django를 사용하여 RESTful API를 생성하고, 고성능 인터서비스 통신을 위한 gRPC
- 데이터 파이프라인: 데이터 워크플로 관리를 위한 Apache Airflow, Luigi 및 메시지 큐인 Kafka 또는 RabbitMQ
- 평가 지표: 생성 품질에 대한 ROUGE, BLEU, 검색 품질에 대한 Mean Reciprocal Rank(MRR) 및 Normalized Discounted Cumulative Gain(NDCG)

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

공용 클라우드 제공업체인 Google Cloud Platform (GCP) 및 Amazon Web Services (AWS)는 이러한 아키텍처의 개발 및 관리를 간소화하는 일련의 1차 도구를 제공합니다.

![이미지](/assets/img/2024-07-09-RAGvsLoRAEnhancingLargeLanguageModels_1.png)

RAG는 특히 특정 또는 최신 정보에 액세스가 필요한 작업에 대한 언어 모델을 더 신뢰할 수 있고 유익하게 만드는 중요한 발전을 상징합니다. RAG는 외부 지식을 동적으로 통합할 수 있는 메커니즘을 제공함으로써 전통적인 언어 모델의 일부 제한 사항을 해결합니다.

# Low-Rank Adaptation (LoRA)

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

LoRA는 사전 훈련된 언어 모델을 특정 작업이나 도메인에 맞게 세밀하게 조정하는 매개 변수 효율적인 파인 튜닝 기술입니다. 이 기술은 먼저 사전 훈련된 모델의 가중치를 고정시킴으로써 작동합니다 (원래의 사전 훈련된 모델 매개 변수는 변경되지 않습니다). 그런 다음 다음을 도입합니다:

- 낮은 순위 분해: LoRA는 각 모델 레이어에 작은 학습 가능한 순위 분해 행렬을 도입합니다.
- 적응: 이러한 낮은 순위 행렬만이 파인 튜닝 중에 업데이트되며, 학습 가능한 매개 변수 수를 크게 줄입니다.

이 접근법은 여러 가지 이점을 제공합니다. 먼저, 파라미터 효율성이 증가하므로 전체 파인 튜닝에 필요한 매개 변수의 일부분만을 학습해야 합니다. 이로 인해 업데이트할 매개 변수의 수가 적기 때문에 메모리 풋프린트가 줄어들고 훈련 주기도 빨라집니다. 또한, 낮은 순위 적응의 주요 이점 중 하나는 모듈성입니다. 여러 LoRA 적응을 훈련시켜 다양한 작업에 교체할 수 있으며, 단일 기본 모델로부터 훈련된 매우 특화된 과제별 모델 모음이 가능해집니다.

# 대등한 비교로 이동하기

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

RAG은 추론 시 외부 정보를 제공하여 모델의 성능을 향상시킵니다. LoRA는 소수의 매개변수를 미세 조정하여 모델의 적응성을 향상시킵니다. RAG는 검색 메커니즘을 통해 동적 유연성을 제공하며, LoRA는 교체 가능한 저랭크 행렬을 통해 모듈식 적응성을 제공합니다. RAG는 검색 시스템에 대한 추가 계산 리소스와 저장 공간이 필요합니다. LoRA는 미세 조정을 위한 계산 및 저장 요구 사항을 크게 줄입니다. RAG는 지식 베이스를 업데이트함으로써 새로운 정보를 신속하게 통합할 수 있지만, LoRA는 새로운 도메인이나 작업에 적응하기 위해 재학습이 필요합니다(이 과정은 비교적 빠릅니다). RAG는 최신이나 특정 지식이 필요한 작업의 성능을 크게 향상시킬 수 있습니다. LoRA는 한정된 리소스로 완전한 미세 조정에 근접한 성능을 달성할 수 있습니다.

# 결론

RAG와 LoRA는 LLM을 향상시키기 위한 두 가지 다른 패러다임을 대표합니다. RAG는 외부 검색을 통해 모델의 지식을 확장하는 데 중점을 두지만, LoRA는 모델의 매개변수를 특정 작업이나 도메인에 효과적으로 적응시키는 데 초점을 두고 있습니다.

RAG와 LoRA 중 어떤 것을 선택할지는 구체적인 사용 사례에 따라 다릅니다:

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

- 외부 지식에 액세스해야 하는 애플리케이션의 경우, RAG가 종종 더 나은 선택입니다.
- 특정 도메인이나 작업에 효율적으로 적응해야 할 때, 특히 한정된 컴퓨팅 자원으로, LoRA는 뛰어난 해결책을 제공합니다.

실제로 이러한 접근 방식은 상호 배타적이 아닙니다. 미래 연구에서는 RAG와 LoRA의 장점을 결합하는 방법을 탐구할 수 있으며, 이는 더욱 강력하고 적응성 있는 언어 모델로 이어질 수 있습니다. AI 분야가 계속 발전함에 따라, RAG와 LoRA와 같은 기술들이 다양한 응용 분야에서 대규모 언어 모델을 더 실용적이고 효율적이며 효과적으로 만드는 데 중요한 역할을 한다고 믿습니다.

질문이 있으신가요? 여기에서 상담 일정을 예약하세요.
