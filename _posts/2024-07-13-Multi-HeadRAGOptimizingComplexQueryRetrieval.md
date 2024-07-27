---
title: "복잡한 쿼리 검색 최적화 Multi-Head RAG2024 최신 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-13-Multi-HeadRAGOptimizingComplexQueryRetrieval_0.png"
date: 2024-07-13 03:12
ogImage: 
  url: /assets/img/2024-07-13-Multi-HeadRAGOptimizingComplexQueryRetrieval_0.png
tag: Tech
originalTitle: "Multi-Head RAG: Optimizing Complex Query Retrieval"
link: "https://medium.com/ai-advances/multi-head-rag-optimizing-complex-query-retrieval-68fdf173a1e1"
---


# 새로운 방식으로 다중 측면 정보 검색 도전 해결하기

![image](/assets/img/2024-07-13-Multi-HeadRAGOptimizingComplexQueryRetrieval_0.png)

저의 이전 글에서, 저는 검색 보강 생성(RAG)이 대형 언어 모델(LLMs)의 능력을 향상시킬 수 있는 개인 인프라에서 어떻게 사용되는지에 대해 썼습니다. 대규모 말뭉치에서 관련 문서를 가져오는 검색 시스템을 통합함으로써, RAG는 LLMs가 보다 관련성 높은 응답을 제공할 수 있게 합니다. 그러나 실제 RAG 아키텍처는 다중, 구별된 측면에서 정보가 필요한 쿼리에 어려움을 겪습니다.

## 다중 헤드 어텐션이란 무엇인가요?

<div class="content-ad"></div>

먼저, Multi-Head Attention이 무엇인지 설명해보겠습니다. Multi-Head Attention은 자연어 처리(NLP) 작업에서 널리 사용되는 Transformer 아키텍처의 중요 구성 요소입니다. 이 구조는 모델이 예측을 할 때 입력 문장의 다른 부분에 집중할 수 있게 해줍니다. 예를 들어, "고양이가 매트 위에 앉았다"는 문장에서, "누가 앉았나요?", "고양이는 어디에 있나요?", "누가 매트 위에 있나요?"와 같은 다양한 질문을 할 수 있습니다. Multi-Head Attention을 사용하면 모델은 이러한 질문들에 동시에 집중하여 누가, 어디에, 무엇을 하는지와 같은 다양한 측면을 포착할 수 있습니다.

# 어떤 도전이 있는가요?

기존의 자료 검색 증강 생성(Retrieval Augmented Generation, RAG) 시스템에서는 모델이 쿼리에 대답하기 위해 관련 문서를 검색합니다. 그러나 이러한 문서들의 임베딩(벡터 공간에서의 표현)이 매우 다르거나 멀리 떨어져 있는 경우 모델이 어려워합니다. 예를 들어, "북극곰의 식사"에 관한 첫 번째 문서와 "북극 지역의 기후 변화의 영향"에 관한 두 번째 문서가 있다고 가정해보세요. 그리고 사용자 쿼리가 "북극곰의 식사와 기후 변화가 그들의 서식지에 미치는 영향을 설명해주세요"인 경우입니다.

임베딩 공간에서 첫 번째와 두 번째 문서는 식사와 기후 변화에 중점을 둔 채로 멀리 떨어져 있을 수 있습니다. 기존 RAG 시스템은 이러한 문서들을 효과적으로 검색하고 통합하여 포괄적인 답변을 제공하는 데 어려움을 겪을 수 있습니다.

<div class="content-ad"></div>

# 어떤 길이 맞는가?

다양한 측면 검색 과제를 해결하기 위해, "Multi-Head RAG: Solving Multi-Aspect Problems with LLMs"¹이라는 새 논문이 Multi-Head Attention 메커니즘을 활용한 혁신적인 접근 방식을 제안했습니다. 텍스트 이해를 나타내는 단일 벡터 대신 각각 다른 측면의 텍스트를 포착하는 여러 벡터가 있는 여러 헤드가 제안되었습니다. 추가로, 논문은 초기 사용자 쿼리를 여러 벡터로 변환하여 다양한 데이터 측면을 탐색하고 다양한 정보를 개선하는 것을 제안했습니다.

다음은 MRAG가 작동하는 예시의 단계별 설명입니다:

1. 데이터 준비하기:

<div class="content-ad"></div>

- 데이터 준비 과정에서 MRAG는 멀티헤드 어텐션 레이어의 활성화를 사용하여 각 텍스트 청크에 대한 멀티-측면 임베딩을 구성합니다.
- 이러한 임베딩은 각 단일 측면의 임베딩이 원본 텍스트 청크를 가리키는 데이터 저장소에 저장됩니다.

```python
from transformers import DPRQuestionEncoder, DPRQuestionEncoderTokenizer
import torch

# 토크나이저와 모델 초기화
model_name: str = 'facebook/dpr-question_encoder-single-nq-base'
tokenizer = DPRQuestionEncoderTokenizer.from_pretrained(model_name)
model = DPRQuestionEncoder.from_pretrained(model_name)

# 예제 텍스트
text = "멀티헤드 어텐션 임베딩을 위한 샘플 텍스트입니다."

# 입력 텍스트 토큰화
inputs = tokenizer(text, return_tensors='pt')

# 모델에서 숨겨진 상태 가져오기
outputs = model(**inputs, output_hidden_states=True)

# 멀티헤드 어텐션 레이어에서 임베딩 추출
multi_head_embeddings = outputs.hidden_states[-1]
```

2. 질의 처리:

- 질의를 받으면, MRAG는 동일한 임베딩 모델을 사용하여 질의의 멀티-측면 임베딩을 생성합니다.

<div class="content-ad"></div>


# 예시 쿼리
쿼리 = "NLP에서 멀티 헤드 어텐션의 영향은 무엇인가요?"

# 쿼리 토크나이즈하고 인코딩하기
쿼리_인풋 = tokenizer(쿼리, return_tensors='pt')
쿼리_아웃풋 = model(**쿼리_인풋, output_hidden_states=True)

# 쿼리 임베딩 추출
쿼리_임베딩 = 쿼리_아웃풋.hidden_states[-1]


3. 검색:

- 검색 엔진은 특별한 멀티-측면 검색 전략을 사용하여 데이터 저장소에서 가장 가까운 멀티-측면 임베딩을 찾습니다.
- 검색된 문서는 그 후에 쿼리에 더 정확하고 관련성 높은 응답을 제공하기 위해 사용됩니다.

```python
from sklearn.metrics.pairwise import cosine_similarity

# 임베딩을 갖는 시뮬레이션 데이터 저장소
데이터_저장소_임베딩 = 멀티_헤드_임베딩[0].detach().numpy()

# 쿼리와 데이터 저장소 임베딩 간 코사인 유사도 계산
유사도_점수 = cosine_similarity(쿼리_임베딩[0].detach().numpy(), 데이터_저장소_임베딩)

# 가장 유사한 문서 검색
가장_유사한_인덱스 = 유사도_점수.argmax()
```

<div class="content-ad"></div>

여러 주의 흐름에서 가져온 포함으로 MRAG는 임베딩 공간에서 멀리 떨어져있는 문서의 임베딩을 사용하더라도 쿼리의 여러 측면을 효과적으로 검색할 수 있습니다.

## 인상적인 성능 및 실제 응용

논문은 표준 RAG 기준에 비해 20%의 관련성 향상을 보여주며, 이 방법의 상당한 잠재력을 강조합니다. MRAG의 향상된 성능은 연구, 고객 지원, 복잡한 문제 해결 시나리오와 같은 세밀한 이해와 정보 검색이 필요한 응용 프로그램에서 특히 가치 있습니다.

## 매끄러운 통합과 효율성

<div class="content-ad"></div>

MRAG의 주요 장점 중 하나는 기존 RAG 프레임워크와 데이터 저장소와의 원활한 통합입니다. 추가 저장 공간이나 여러 추론 단계가 필요하지 않으며 비용 효율적이고 에너지 효율적입니다. 이러한 통합의 용이성과 효율성은 높은 비용을 소요하지 않고 정보 검색 시스템을 개선하려는 조직에게 MRAG를 실용적인 선택으로 만듭니다.

MRAG 및 그 기본 메커니즘에 관한 더 자세한 이해를 위해 arXiv에서 이용 가능한 이 연구 논문을 참고할 수 있습니다.

## 결론

Multi-Head RAG는 자연 언어 처리 분야에서 특히 검색 증강 생성의 맥락에서 중요한 발전을 이룬 것으로 나타납니다. MRAG는 다양한 데이터 측면을 캡처하기 위해 다중 헤드 어텐션을 활용하여 복잡한 정보 검색 작업에 강력한 솔루션을 제공하며, LLM이 더 정확하고 관련성 높은 응답을 제공하도록 합니다. 이 혁신적인 접근 방식은 성능을 향상시킬 뿐만 아니라 기존 시스템과 심리스하게 통합되어 다양한 응용 프로그램에 매력적인 옵션으로 제공됩니다. MRAG의 실제 세계 영향을 보여주는 자세한 사례 연구 및 추가 업데이트를 기대해 주세요.

<div class="content-ad"></div>

# 참고 자료

[1] Besta, M., Kubicek, A., Niggli, R., Gerstenberger, R., Weitzendorf, L., Chi, M., Iff, P., Gajda, J., Nyczyk, P., Müller, J., Niewiadomski, H., Chrapek, M., Podstawksi, M., & Hoefler, T. (2024). Multi-Head RAG: Solving Multi-Aspect Problems with LLMs. arXiv:2406.05085.