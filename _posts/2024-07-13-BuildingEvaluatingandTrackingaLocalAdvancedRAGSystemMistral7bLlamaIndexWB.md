---
title: "로컬 고급 RAG 시스템 구축, 평가, 추적 방법  Mistral 7b  LlamaIndex  W,B 사용"
description: ""
coverImage: "/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_0.png"
date: 2024-07-13 03:34
ogImage:
  url: /assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_0.png
tag: Tech
originalTitle: "Building, Evaluating and Tracking a Local Advanced RAG System | Mistral 7b + LlamaIndex + W,B"
link: "https://medium.com/towards-data-science/building-evaluating-and-tracking-a-local-advanced-rag-system-mistral-7b-llamaindex-w-b-5c9c69059f92"
isUpdated: true
---

![Image](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_0.png)

Retrieval Augmented Generation (RAG)은 대규모 언어 모델과 지식에 대한 선택적 액세스를 결합한 강력한 NLP 기술입니다. 우리는 문서에서 주요한 문맥 조각들을 제공함으로써 LLM 환각을 줄이는 RAG를 통해 가능합니다. 이 글의 아이디어는 로컬로 실행되는 LLM을 사용하여 자신만의 RAG 시스템을 구축하는 방법을 보여주고, 어떤 기술을 사용하여 이를 개선할 수 있는지, 마지막으로 실험을 추적하고 W&B에서 결과를 비교하는 방법에 대해 설명합니다.

# 소개

우리는 다음의 주요 측면을 다룰 것입니다:

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

- Mistral-7b와 LlamaIndex를 활용해 로컬 RAG 시스템의 기본 구축 중입니다.
- 충실성과 관련성 측면에서 성능을 평가하고 있습니다.
- Weights & Biases (W&B)를 활용해 실험을 끝까지 추적 중입니다.
- 계층 노드와 재랭킹과 같은 고급 RAG 기술을 구현하고 있습니다.

GitHub에서 자세한 주석과 코드를 포함한 완벽한 노트북을 확인할 수 있습니다.

# 🏠 로컬 RAG 시스템 구축 중

![이미지](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_1.png)

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

먼저 LlamaIndex 라이브러리를 설치해주세요. 저희는 환경 설정하고 실험에 사용할 문서를 불러오는 작업부터 시작할 것입니다. LlamaIndex는 유연한 데이터 통합을 가능케 하는 다양한 사용자 정의 데이터 로더를 지원합니다.

```js
# LlamaIndex에서 PDFReader를 불러오기
from llama_index import VectorStoreIndex, download_loader

# 커스텀 로더 초기화
PDFReader = download_loader("PDFReader")
loader = PDFReader()

# PDF 파일 읽기
documents = loader.load_data(file=Path("./Mixtral.pdf"))
```

이제 LLM 설정을 할 차례입니다. 저는 M1 맥북을 사용 중이라 llama.cpp를 활용하는 것이 매우 유용합니다. 이는 Metal과 Cuda 모두와 자연스럽게 작동하며 메모리 제한이 있는 환경에서 LLM을 실행할 수 있게 합니다. 설치하기 위해서는 공식 저장소를 참고하거나 다음 명령을 실행해보세요:

```js
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
make
```

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

여기서는 Mistral-7b 모델을 사용합니다. 이 실험을 더 빠르고 모든 사람들에게 이용 가능하게 하기 위해 이 모델의 양자화된 버전을 사용할 것입니다. 품질 손실이 극히 낮은 2비트 양자화까지 지원하는 QuIP 양자화된 모델을 확인하는 것을 적극 권장합니다.

from llama_index import (
SimpleDirectoryReader,
VectorStoreIndex,
ServiceContext,
)
from llama_index.llms import LlamaCPP
from llama_index.llms.llama_utils import (
messages_to_prompt,
completion_to_prompt,
)

llm = LlamaCPP( # 자동으로 다운로드할 GGML 모델의 URL을 전달할 수 있습니다.
model_url="https://huggingface.co/ikawrakow/various-2bit-sota-gguf/blob/main/mistral-7b-2.20bpw.gguf", # 선택적으로, model_url 대신 미리 다운로드한 모델의 경로를 설정할 수 있습니다.
model_path=None,
temperature=0.1,
max_new_tokens=512, # llama2의 콘텍스트 창은 4096 토큰이지만, 약간의 여유 공간을 남기기 위해 낮게 설정하였습니다.
context_window=3900,
generate_kwargs={}, # 최소 1로 설정하여 GPU를 사용합니다.
model_kwargs={"n_gpu_layers": 1}, # 입력을 Llama2 형식으로 변환합니다.
messages_to_prompt=messages_to_prompt,
completion_to_prompt=completion_to_prompt,
verbose=False,
)

다음으로, 문서를 벡터화하고 벡터 DB를 생성하며, 검색 시스템을 설정해보겠습니다.

## 단계 #1 | ServiceContext

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

ServiceContext는 LlamaIndex에서 모델과 콜백의 라이프사이클을 관리합니다. 필수 설정으로 로컬 모델을 사용하여 설정합니다. LlamaIndex는 사용 가능한 GPU를 자동으로 감지할 것입니다. M-프로세서를 사용하는 맥북이라면 걱정하지 마세요.

```python
# Setting up the ServiceContext with the language model and embedding model
embed_model = "local:BAAI/bge-small-en-v1.5"
service_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    callback_manager=callback_manager
)
```

## Step #2 | VectorStore

VectorStore는 LLAMAIndex에서 문서 벡터의 쪼개기, 임베딩, 저장을 담당합니다. 여기서 VectorStore를 생성하고 설정합니다.

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

# 문서 처리를 위한 VectorStoreIndex 생성

index = VectorStoreIndex.from_documents(documents, service_context=service_context)

# 검색을 위한 쿼리 엔진으로 인덱스 변환

query_engine = index.as_query_engine()

## 테스트해 봅시다

def query_and_display(question):
response = query_engine.query(question)
display_response(response)

query_and_display("누가 믹스트럴 논문을 썼나요?")
query_and_display("Sparse MoE가 무엇인가요?")
query_and_display("Sparse MoE에 몇 명의 전문가가 사용되나요?")

최종 응답: 믹스트럴 논문은 알버트 Q. 장, 알렉산드르 사블라울 등 연구자 팀이 썼습니다.

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

최종 응답: Sparse Mixture of Experts (MoE)은 입력 시퀀스의 각 토큰이 여러 서브 네트워크 또는 전문가들에 의해 처리되는 신경망 아키텍처 유형을 가리킵니다.

최종 응답: Sparse MoE에서 사용되는 전문가의 특정 수는 제공되지 않습니다. 🚩 🚩 🚩 (실제로는, 8이 정답입니다)

# 🔎 평가

![이미지](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_2.png)

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

알겠어요. 좋아요. 그럼, 얼마나 잘 작동하고 있나요? 이제 우리는 RAG 설정의 성능을 평가할 단계로 이동하게 됩니다. 여기서는 메트릭스를 사용하여 RAG 시스템의 성능을 평가하고 또 다른 LLM을 사용할 것입니다 🙂

우리의 RAG 시스템을 평가하기 위해 주목해야 할 핵심 메트릭스는 믿음성과 관련성입니다. 믿음성은 생성된 답변이 검색된 문맥에 얼마나 근접하게 따라가는지를 측정하는데, 이는 답변이 사실에 기반하고 가공되지 않았음을 보장합니다. 한편, 관련성은 사용자의 질의에 실제로 대답하는지를 평가하며, 문맥의 말 그대로가 아니더라도 효과적으로 대응하는지 확인합니다.

## 단계 #1 | 질문 생성

평가하기 위해 질문이 필요합니다. 솔직히 말하자면, 우리는 직접 이를 작성하는 것이 귀찮아요. 그러니 이미 구비된 Llamaindex + GPT-3.5 Api 내의 QuestionsGenerator를 사용하여 질문을 생성하도록 하겠습니다. 또는 로컬 LLM 모델을 사용할 수도 있습니다. llm 객체를 간단히 바꿔주기만 하면 됩니다.

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

# 문서 설정 및 평가용 질문 생성하기

임의의 문서 = 문서의 복제(deep copy)

# 문서를 섞고 5개의 임의의 문서 선택하기. 평가를 빠르게 진행하기 위함

random.shuffle(임의의 문서)
임의의 문서 = 임의의 문서[:5]

# GPT를 locall llm으로 대체할 수 있음

llm_eval = OpenAI(온도=0, 모델="gpt-3.5-turbo")

# GPT API 사용으로 인해 새로운 서비스 컨텍스트 생성. 기존 컨텍스트를 사용할 수도 있음

서비스*컨텍스트 = 기본값에서 가져온 서비스*컨텍스트(
llm=llm_eval,
embed_model=embed_model,
)

# 평가를 위해 문서에서 질문 생성

데이터*생성자 = 문서에서*데이터셋*생성기(
임의의 문서, 서비스*컨텍스트=서비스*컨텍스트, 개별*청크당*질문*수=2
)

# Async 코드를 실행하기 위해 nest_asyncio 적용

nest*asyncio.apply()
평가*질문 = 데이터\_생성자.generate_questions_from_nodes()

그리고 마침!

평가\_질문[:3]

['Agieval 벤치마크의 목적은 무엇입니까?',
'전문가 선택 라우팅을 사용한 Mixture-of-experts 접근법이 신경 정보 처리 시스템 분야에 어떻게 기여하나요?',
'라우팅 분석에서 선택된 전문가의 분포는 다른 도메인 간에 어떻게 변하는가?']

## 단계 #2 | 메트릭스 가져오기

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

BatchEvalRunner을 사용하여 FaithfulnessEvaluator와 RelevancyEvaluator로 평가를 수행합니다. BatchEvalRunner은 여러 쿼리를 동시에 처리할 수 있도록 설계되어 있습니다. 대규모 질문 세트에 대해 RAG 시스템을 평가할 때 특히 유용한 대량 처리 기능입니다.

```python
# BatchEvalRunner를 사용하여 평가를 실행합니다.
from llama_index.evaluation import (
    BatchEvalRunner,
    FaithfulnessEvaluator,
    RelevancyEvaluator,
)

# eval_llm을 사용하여 evaluator를 생성합니다.
faithfulness_evaluator = FaithfulnessEvaluator(service_context=service_context_eval)
relevancy_evaluator = RelevancyEvaluator(service_context=service_context_eval)

# 빠른 처리를 위한 Batch runner
runner = BatchEvalRunner(
    {"faithfulness": faithfulness_evaluator, "relevancy": relevancy_evaluator},
    workers=8,
)

# 결과 가져오기
eval_results = await runner.aevaluate_queries(
    index.as_query_engine(), queries=eval_questions
)
```

이제 관련성과 충실도에 대한 결과를 확인하세요.

```python
# 결과로부터 데이터프레임 만들기
faithfulness_df = pd.DataFrame.from_records([eval_result.dict() for eval_result in eval_results["faithfulness"]])
relevancy_df = pd.DataFrame.from_records([eval_result.dict() for eval_result in eval_results["relevancy"]])

# 점수 필요합니다!
faithfulness_df["score"].mean(), relevancy_df["score"].mean()

>> (0.9166666666666666, 0.9166666666666666)
```

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

# 📋 실험 추적 중 | W&B

![Experiment Image](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_3.png)

이상적으로는 결과를 노트북 출력뿐만 아니라 저장하길 원합니다. 여기서 Weights&Biases가 우리를 도와줍니다. 더 나은 추적을 위해, 우리는 평가 점수와 같은 핵심 결과를 w&b에 로그합니다. 또한 생성된 평가 질문과 다른 파일을 아티팩트로 영구적으로 유지할 수 있습니다.

```python
!pip install wandb
```

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

평가 결과를 추적하고 적절히 저장해봐요.

```js
import wandb

# 현재 세션을 위한 실행 생성
wandb.init(project=PROJECT, name="baseline-evaluation")

# 결과를 담은 테이블 생성
faithfulness_table = wandb.Table(dataframe=faithfulness_df)
relevancy_table = wandb.Table(dataframe=relevancy_df)

# 기록하기
wandb.log({"faithfulness": faithfulness_table, "relevancy": relevancy_table})
```

이제 대시보드에서 멋지게 확인할 수 있어요:

![Dashboard Image](https://yourwebsite.com/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_4.png)

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

귀하가 소중한 지표를 추적하는 데 유용한 것은 좋은 점수의 평균과 같은 지표를 추적하는 것입니다. 또한 대시보드에 사용자 정의 차트를 추가할 수도 있습니다. 이 예시에서는 이전에 생성한 평가 질문을 저장하는 방법을 소개합니다.

```js
# wandb log scalar 평균 충실성 및 관련성 점수
wandb.log({"faithfulness_mean": faithfulness_df["score"].mean()})
wandb.log({"relevancy_mean": relevancy_df["score"].mean()})
```

![Image](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_5.png)

대용량 파일(체크포인트, 텍스트 또는 IndexStore(벡터화된 데이터베이스))을 저장하는 데도 유용합니다.

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

# W&B을 사용하여 질문을 CSV 파일에 유지하고, 나중에 로드할 수 있게 합니다.

# Artifact 객체 생성

artifact = wandb.Artifact(name="eval-questions", type="text")

# 질문 목록을 artifact에 파일로 추가합니다.

with artifact.new_file("questions.txt", mode="w") as f:
f.write("\n".join(eval_questions))

# Artifact를 W&B에 기록합니다.

wandb.log_artifact(artifact)

![Artifact Image1](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_6.png)

# W&B 초기화

wandb_args = {"project": WANDB_PROJECT, "name": "baseline-evaluation"}
wandb_callback = WandbCallbackHandler(run_args=wandb_args)
callback_manager = CallbackManager([wandb_callback])

service_context = ServiceContext.from_defaults(
...
callback_manager=callback_manager # 여기에 W&B 핸들러가 있습니다.
)

![Artifact Image2](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_7.png)

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

# 🏎️ 고급 RAG

![이미지](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_8.png)

이제 RAG 품질을 업그레이드 해봅시다. 기본 RAG는 이미 질문에 응할 수 있지만 그다지 잘하지는 않습니다. 계층적 노드 구문 분석 및 재순위 설정을 통해 더 나은 컨텍스트 통합과 검색 우선 순위 부여가 가능한 고급 설정을 탐색해봅시다.

![이미지](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_9.png)

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

우리는 서로 다른 계층 구조에서 문서 청크 트리를 구축하기 위한 계층 노드 파서를 만듭니다. 이는 VectorStore에 색인화됩니다. 초기 검색 후, 교차 인코더를 사용하여 결과를 다시 순위 지정합니다. 이렇게 함으로써 쿼리와 컨텍스트 간의 세밀한 관계를 포착하는 데 도움이 됩니다.

```python
from llama_index.node_parser import HierarchicalNodeParser

# 계층 노드 파서를 생성합니다. 주의: 청크 크기를 지정해야 합니다.
node_parser = HierarchicalNodeParser.from_defaults(
    chunk_sizes=[2048, 512, 128]
)

# 문서에서 노드를 가져옵니다.
nodes = node_parser.get_nodes_from_documents(documents)
```

노드가 어떻게 보이는지 궁금하신가요?

```python
from llama_index.node_parser import get_leaf_nodes

leaf_nodes = get_leaf_nodes(nodes)
print(leaf_nodes[0].text)

>>> 전문가들의 혼합물
알버트 Q. 장, 알렉산드르 사블라이롤, ...
...
우리는 Sparse Mixture of Experts (SMoE) 언어 모델인 Mixtral 8x7B를 소개합니다. Mixtral은 Mistral 7B와 동일한 아키텍처를 가지며 각 레이어가 구성되는 것이 다른 차이가 있습니다.
```

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

이제 이 계층 구조에서 색인을 만들어봅시다.

```js
# context 검색 명세 생성
auto_merging_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model="local:BAAI/bge-small-en-v1.5",
    node_parser=node_parser,  # hierarchical 노드 파서가 여기에 있음을 명심하세요
)

# 노드, 그래프 및 기타를 위한 유틸리티 컨테이너인 StorageContext
storage_context = StorageContext.from_defaults()
storage_context.docstore.add_documents(nodes)

# 구성에서 인덱스 생성
automerging_index = VectorStoreIndex(
    leaf_nodes, storage_context=storage_context, service_context=auto_merging_context
)
```

이제 이러한 노드들을 재순위 지정해봅시다. 초기 검색 시스템은 종종 깊은 의미적 이해보다는 속도를 우선시합니다. 교차 인코더를 기반으로 하는 SentenceTransformerRerank는 쿼리와 텍스트 사이의 세부 의미적 관계를 잘 파악하는 데 좋습니다.

```js
from llama_index.indices.postprocessor import SentenceTransformerRerank
from llama_index.retrievers import AutoMergingRetriever
from llama_index.query_engine import RetrieverQueryEngine

# 인덱스에서 retriever 가져오기
automerging_retriever = automerging_index.as_retriever(
    similarity_top_k=12
)

# AutoMergingRetriever 생성
# hierarchical 노드 파서가 있는 Index에서 retriever를 전달하는 것에 주목하세요
retriever = AutoMergingRetriever(
    automerging_retriever,
    automerging_index.storage_context,
    verbose=True,
)

# re-ranker 생성, 나중에 병합된 청크에 필요할 것입니다
rerank = SentenceTransformerRerank(top_n=4, model="BAAI/bge-reranker-base")

# 쿼리 엔진 래퍼 생성. 후처리기를 넣기위해 래퍼가 필요합니다.
auto_merging_engine = RetrieverQueryEngine.from_args(
    automerging_retriever,
    node_postprocessors=[rerank],
    verbose=True,
    service_context=auto_merging_context
)
```

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

## 완료했어요, 테스트해보죠

```js
response = auto_merging_engine.query("Sparse MoE에서 전문가는 몇 명 사용되나요?");
display_response(response);
```

최종 응답: Mixtral 8x7B에서 각 레이어는 8개의 피드포워드 블록 또는 전문가로 구성됩니다. 따라서 전문가는 총 8명입니다. ✅

우리는 이 시스템을 기준선과 동일한 방식으로 평가할 수 있어요. 기준선보다 개선된 점을 확인할 수 있어요. W&B에 결과를 기록한 뒤, 각 실행 사이의 비교 차트를 관찰할 수 있어요.

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

![BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_10.png](/assets/img/2024-07-13-BuildingEvaluatingandTrackingaLocalAdvancedRAGSystemMistral7bLlamaIndexWB_10.png)

모든 코드와 자세한 내용은 GitHub에서 확인할 수 있어요. 이 글이 여러분에게 도움이 되었기를 바라며 🤗
