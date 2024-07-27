---
title: "고급 RAG 06 쿼리 재작성 방법 탐구"
description: ""
coverImage: "/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_0.png"
date: 2024-07-13 23:10
ogImage: 
  url: /assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_0.png
tag: Tech
originalTitle: "Advanced RAG 06: Exploring Query Rewriting"
link: "https://medium.com/@florian_algo/advanced-rag-06-exploring-query-rewriting-23997297f2d1"
---


In Retrieval Augmented Generation (RAG), we often face challenges with the original queries provided by users. The queries may contain inaccurate wording or lack important semantic information. For example, a query like "The NBA champion of 2020 is the Los Angeles Lakers! Tell me what is langchain framework?" could lead to incorrect or unanswerable responses when directly searched using LLM.

Ensuring that the semantic understanding of user queries aligns with the content of the documents is crucial. This is where query rewriting technology plays a crucial role in addressing such issues within RAG, as shown in Figure 1:

![Figure 1: Query Rewriting in RAG](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_0.png)

Query rewriting is a pre-retrieval method when viewed from a positional perspective. The diagram gives a rough idea of where query rewriting fits into the RAG process. In the upcoming section, we will explore how certain algorithms can enhance this process further.

<div class="content-ad"></div>

쿼리 재작성은 쿼리와 문서의 의미를 조율하는 중요 기술입니다. 

예를 들어:

- Hypothetical Document Embeddings (HyDE)는 가상 문서를 통해 쿼리와 문서의 의미 공간을 조율합니다.
- Rewrite-Retrieve-Read는 전통적인 검색 및 읽기 순서와 다른 프레임워크를 제안하여 쿼리 재작성에 초점을 맞춥니다.
- Step-Back Prompting은 LLM이 고수준 개념에 기반한 추상적 추론과 검색을 수행할 수 있도록 합니다.
- Query2Doc은 LLM에서 몇 가지 프롬프트를 사용하여 의사 문서를 생성합니다. 그런 다음 원래 쿼리와 병합하여 새로운 쿼리를 구성합니다.
- ITER-RETGEN은 이전 생성물의 결과를 이전 쿼리와 결합하는 방법을 제안합니다. 이후 관련 문서를 검색하고 새로운 결과를 생성합니다. 이 프로세스는 최종 결과를 달성할 때까지 반복됩니다.

이러한 방법들의 세부 내용에 대해 자세히 살펴보도록 하죠.

# Hypothetical Document Embeddings (HyDE)

<div class="content-ad"></div>

“Relevance Label이 없는 정확한 제로 샷 밀도 검색”라는 논문은 Hypothetical Document Embeddings (HyDE)에 기반한 방법을 제안합니다. 주요 과정은 아래 그림 2에 나와 있습니다.

![Figure 2](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_1.png)

이 프로세스는 주로 네 단계로 나뉘어집니다:

1. LLM을 사용하여 쿼리를 기반으로 k개의 가상 문서를 생성합니다. 이 생성된 문서들은 사실적일 필요가 없고 오류를 포함할 수 있지만, 관련 문서와 닮아야 합니다. 이 단계의 목적은 LLM을 통해 사용자의 쿼리를 해석하는 것입니다.

<div class="content-ad"></div>

2. 생성된 가상 문서를 인코더에 공급하여 밀집 벡터 f(dk)로 매핑합니다. 인코더는 가상 문서 내의 소음을 걸러내는 필터 기능을 한다고 믿습니다. 여기서 dk는 k번째 생성된 문서를 나타내며, f는 인코더 작업을 나타냅니다.

3. 다음 k 벡터의 평균을 주어진 공식을 사용하여 계산하십시오,

![image](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_2.png)

또한 원래 쿼리 q를 가능한 가설로 고려할 수 있습니다:

<div class="content-ad"></div>

4. 문서 라이브러리에서 답변을 검색하기 위해 벡터 v를 사용합니다. 단계 3에서 설정했던대로, 이 벡터는 사용자 쿼리와 원하는 답변 패턴의 정보를 모두 담고 있어서 검색률을 향상시킬 수 있습니다.

제가 이해한 HyDE의 개념은 아래 그림 3에 설명되어 있습니다. HyDE의 목표는 가상 문서를 생성하여 최종 쿼리 벡터 v가 벡터 공간에서 실제 문서와 가능한 한 일치하도록 하는 것입니다.

![그림](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_4.png)

<div class="content-ad"></div>

HyDE는 LlamaIndex와 Langchain 둘 다에 구현되어 있어요. 아래 설명은 LlamaIndex를 예시로 들어 설명하겠습니다.

이 파일을 **당신의_디렉토리_경로**에 넣어주세요. 테스트 코드는 다음과 같습니다(LlamaIndex 버전은 0.10.12를 설치했어요):

```js
import os
os.environ["OPENAI_API_KEY"] = "당신의_OPENAI_API_KEY"

from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
from llama_index.core.indices.query.query_transform import HyDEQueryTransform
from llama_index.core.query_engine import TransformQueryEngine

# 문서를 불러오고, VectorStoreIndex를 구축하세요
dir_path = "당신의_디렉토리_경로"
documents = SimpleDirectoryReader(dir_path).load_data()
index = VectorStoreIndex.from_documents(documents)

query_str = "what did paul graham do after going to RISD"

# 변형 없는 쿼리: 동일한 쿼리 문자열을 사용하여 임베딩 조회와 요약에 사용됩니다.
query_engine = index.as_query_engine()
response = query_engine.query(query_str)

print('-' * 100)
print("기본 쿼리:")
print(response)

# HyDE 변형을 적용한 쿼리
hyde = HyDEQueryTransform(include_original=True)
hyde_query_engine = TransformQueryEngine(query_engine, hyde)
response = hyde_query_engine.query(query_str)

print('-' * 100)
print("HyDEQueryTransform 적용 후:")
print(response)
```

먼저, LlamaIndex의 기본 HyDE 프롬프트를 확인해보세요.

<div class="content-ad"></div>

```js
############################################
# 하이드
##############################################

HYDE_TMPL = (
    "질문에 대한 답변을 작성하세요.\n"
    "가능한 한 많은 주요 세부 정보를 포함하려고 노력하세요.\n"
    "\n"
    "\n"
    "{context_str}\n"
    "\n"
    "\n"
    'Passage:"""\n'
)

DEFAULT_HYDE_PROMPT = PromptTemplate(HYDE_TMPL, prompt_type=PromptType.SUMMARY)
```

하이드 쿼리 변환 클래스인 HyDEQueryTransform의 코드는 다음과 같습니다.

def _run 함수의 목적은 가상 문서를 생성하는 것이며, 가상 문서의 내용을 모니터링하기 위해 def _run 함수에 세 가지 디버깅 문이 추가되었습니다.:

```js
class HyDEQueryTransform(BaseQueryTransform):
    """가상 문서 임베딩 (HyDE) 쿼리 변환.

    주어진 쿼리에 대한 가상 답변을 생성하기 위해 LLM을 사용하고,
    결과 문서를 임베딩 문자열로 사용합니다.

    `[레벨런스 레이블 없이 정확한 제로샷 밀집 검색]` 
    (https://arxiv.org/abs/2212.10496)`에서 설명되어 있습니다.
    """

    def __init__(
        self,
        llm: Optional[LLMPredictorType] = None,
        hyde_prompt: Optional[BasePromptTemplate] = None,
        include_original: bool = True,
    ) -> None:
        """HyDEQueryTransform을 초기화합니다.

        Args:
            llm_predictor (Optional[LLM]): 가상 문서 생성을 위한
                LLM
            hyde_prompt (Optional[BasePromptTemplate]): HyDE를 위한 사용자 정의 프롬프트
            include_original (bool): 원본 쿼리 문자열을 
                임베딩 문자열 중 하나로 포함할지 여부
        """
        super().__init__()

        self._llm = llm or Settings.llm
        self._hyde_prompt = hyde_prompt or DEFAULT_HYDE_PROMPT
        self._include_original = include_original

    def _get_prompts(self) -> PromptDictType:
        """프롬프트를 가져옵니다."""
        return {"hyde_prompt": self._hyde_prompt}

    def _update_prompts(self, prompts: PromptDictType) -> None:
        """프롬프트를 업데이트합니다."""
        if "hyde_prompt" in prompts:
            self._hyde_prompt = prompts["hyde_prompt"]

    def _run(self, query_bundle: QueryBundle, metadata: Dict) -> QueryBundle:
        """쿼리 변환 실행."""
        # TODO: 여러 개의 가상 문서 생성 지원
        query_str = query_bundle.query_str
        hypothetical_doc = self._llm.predict(self._hyde_prompt, context_str=query_str)
        embedding_strs = [hypothetical_doc]
        if self._include_original:
            embedding_strs.extend(query_bundle.embedding_strs)

        # 다음 세 줄은 추가된 디버깅 문이 포함되어 있습니다.
        print('-' * 100)
        print("가상 문서:")
        print(embedding_strs)

        return QueryBundle(
            query_str=query_str,
            custom_embedding_strs=embedding_strs,
        )
```

<div class="content-ad"></div>

테스트 코드는 다음과 같이 작동합니다:

```python
(llamaindex_010) Florian:~ Florian$ python /Users/Florian/Documents/test_hyde.py 
----------------------------------------------------------------------------------------------------
Base query:
Paul Graham은 RISD를 다닌 후 뉴욕에서 옛 생활을 재개했습니다. 그는 부유해지고 택시를 손쉽게 호출하며 매력적인 레스토랑에서 식사하는 등 새로운 기회를 가지게 되었습니다. 또한 그는 새로운 종류의 정물화 기술에 실험을 시작했습니다.
----------------------------------------------------------------------------------------------------
가상 문서:
["RISD를 다닌 후, Paul Graham은 온라인 스토어 빌더인 Viaweb의 공동 창업자가 되었으며, 해당 기업은 나중에 야후에 4900만 달러에 인수되었습니다. Viaweb의 성공 이후 Graham은 기술 산업에서 영향력 있는 인물이 되었고, 2005년에 스타트업 가속기인 Y Combinator를 공동 창업했습니다. Y Combinator는 이후 드롭박스, 에어비앤비, 레딧 등의 기업을 시작하는 데 도움을 주며 세계에서 가장 명문이자 성공적인 스타트업 가속기 중 하나가 되었습니다. Graham은 또한 기술, 스타트업 및 기업가精의 주제로 다양한 글을 썼으며, 그의 에세이는 기술 커뮤니티에서 널리 읽히고 존경받고 있습니다. 총론적으로 말하면, RISD 이후 Paul Graham의 경력은 혁신, 성공 및 스타트업 생태계에 미치는 중요한 영향으로 표시되어 있습니다.", 'RISD 다닌 후, 폴 그레이엄이 무엇을 했나요']
----------------------------------------------------------------------------------------------------
HyDEQueryTransform 후:
RISD를 다닌 후, Paul Graham은 뉴욕에서 옛 생활을 재개했지만 이제는 부자였습니다. 그는 이제는 택시를 쉽게 호출하고 매력적인 레스토랑에서 식사하는 등 새로운 기회들을 가지며 옛 습관들을 이어나갔습니다. 또한 그는 새로운 기술을 시도하면서 그림에 더 집중하기 시작했습니다. 게다가 아파트를 사기 위해 앞길을 모색하며 웹 앱 제작을 위한 웹 앱 아이디어를 고민하기 시작했고, 이로 인해 새 회사 Aspra를 시작하게 되었습니다.
```

embedding_strs는 두 요소가 포함된 리스트입니다. 첫 번째 요소는 생성된 가상 문서이고, 두 번째는 원본 쿼리입니다. 이들은 벡터 계산을 용이하게 하기 위해 리스트로 결합됩니다.

이 예시에서 HyDE는 RISD 이후 폴 그레이엄이 무엇을 했는지를 정확하게 상상하여 (가상 문서 참조) 출력 품질을 크게 향상시킵니다. 이는 임베딩 품질 및 최종 출력을 개선합니다.

<div class="content-ad"></div>

자연스럽게, HyDE에는 몇 가지 실패 케이스도 있습니다. 관심 있는 독자들은 이 웹페이지를 방문하여 직접 테스트해볼 수 있습니다.

HyDE는 감독되지 않으며, HyDE에서는 모델을 훈련시키지 않습니다: 생성 모델과 대조 인코더는 그대로 유지됩니다.

요약하자면, HyDE는 쿼리 재작성을 위한 새로운 방법을 소개하지만 몇 가지 제한사항이 있습니다. HyDE는 쿼리 임베딩 유사성에 의존하지 않고 대신 하나의 문서가 다른 문서와 얼마나 유사한지에 중점을 둡니다. 그러나 언어 모델이 주제에 정통하지 않은 경우, 최적의 결과를 항상 얻을 수 없을 수 있으며, 오류가 증가할 수도 있습니다.

# 재작성-검색-읽기

<div class="content-ad"></div>

이 아이디어는 "Query Rewriting for Retrieval-Augmented Large Language Models"라는 논문에서 나온 것입니다. 이 논문은 실제 상황에서 원래 쿼리가 LLM에 의해 항상 검색에 최적이 되지 않을 수 있다고 믿습니다.

따라서 이 논문은 먼저 LLM을 사용하여 쿼리를 다시 쓰는 것을 제안합니다. 그런 다음 검색 및 답변 생성이 이어져야 합니다. 원래 쿼리에서 콘텐츠를 직접 검색하고 답변을 생성하는 것이 아니라 Figure 4 (b)에 나와 있는 것처럼요.

![이미지](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_5.png)

쿼리 재작성이 어떻게 컨텍스트 검색 및 예측 성능에 영향을 미치는지 설명하기 위해 예를 들어보겠습니다. "The NBA champion of 2020 is the Los Angeles Lakers! Tell me what is langchain framework?"라는 쿼리는 재작성을 통해 정확하게 처리됩니다.

<div class="content-ad"></div>

Langchain을 사용하고 있습니다. 설치에 필요한 주요 라이브러리는 아래와 같습니다:

```js
pip install langchain
pip install openai
pip install langchainhub
pip install duckduckgo-search
pip install langchain_openai
```

환경 설정 및 라이브러리 불러오기:

```js
import os
os.environ["OPENAI_API_KEY"] = "YOUR_OPEN_AI_KEY"

from langchain_community.utilities import DuckDuckGoSearchAPIWrapper
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_openai import ChatOpenAI
```

<div class="content-ad"></div>

```python
def june_print(msg, res):
    print('-' * 100)
    print(msg)
    print(res)


base_template = """아래 맥락만 고려하여 사용자의 질문에 답해주세요:

<context>
{context}
</context>

질문: {question}
"""

base_prompt = ChatPromptTemplate.from_template(base_template)

model = ChatOpenAI(temperature=0)

search = DuckDuckGoSearchAPIWrapper()


def retriever(query):
    return search.run(query)

chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | base_prompt
    | model
    | StrOutputParser()
)

query = "NBA 2020 우승자는 로스앤젤레스 레이커스입니다! 랭체인 프레임워크는 무엇인지 알려주세요."

june_print(
    '쿼리 결과:', 
    chain.invoke(query)
)

june_print(
    '검색된 맥락 결과:', 
    retriever(query)
)
```

<div class="content-ad"></div>

결과에서 보듯이 "rngformat"에 대한 매우 적은 정보가 있는 것으로 나타났습니다.

이제 검색 쿼리를 재작성하기 시작하세요.


제공할 더 나은 검색 쿼리를 제공하여 주어진 질문에 대한 답변을 얻을 수 있도록 하십시오. 질문: {x} 답변:**


이 결과는 다음과 같습니다: 

<div class="content-ad"></div>

### 리라이트, 검색, 읽기 체인 설명

랭체인 프레임워크는 무엇이며 어떻게 작동하는지에 대해요. 리라이트_리트리브_리드_체인을 구성하고 재작성 된 쿼리를 활용하세요.


리라이트_리트리브_리드_체인 = (
    {
        "콘텍스트": {"x": RunnablePassthrough()} | 리라이터 | 리트리버,
        "질문": RunnablePassthrough(),
    }
    | base_prompt
    | model
    | StrOutputParser()
)

june_print(
    '리라이트_리트리브_리드_체인의 결과:', 
    리라이트_리트리브_리드_체인.invoke(query)
)


연산 결과는 다음과 같습니다:

<div class="content-ad"></div>


----------------------------------------------------------------------------------------------------
The result of the rewrite_retrieve_read_chain:
LangChain is a Python framework designed to help build AI applications powered by language models, especially large language models (LLMs). It provides a generic interface to different foundation models, a framework for managing prompts, and a central interface to long-term memory, external data, other LLMs, and more. It simplifies the process of interacting with LLMs and can be used to build a wide range of applications, including chatbots that interact with users naturally.


Through the rewriting process, we have successfully obtained the accurate response.

## STEP-BACK PROMPTING

STEP-BACK PROMPTING is a simple technique that allows LLMs to abstract, distilling high-level concepts and basic principles from instances with specific details. The concept involves defining "step-back problems" as more abstract issues derived from the original problem.


<div class="content-ad"></div>

예를 들어, 쿼리에 많은 세부 정보가 포함되어 있으면 LLM이 작업을 해결할 수 있는 관련 사실을 검색하기 어려울 수 있습니다. 도표 5의 첫 번째 예시에 나와 있는 것처럼, 물리 문제인 "이상 기체의 압력 P가 온도를 2배로 증가시키고 체적을 8배로 증가시킬 때 어떻게 변화하는가?" 라는 문제에 대해 고려할 때, LLM은 문제를 직접 추론할 때 이상 기체 법칙의 원칙에서 벗어날 수 있습니다.

마찬가지로, "에스텔라 레오폴드가 1954년 8월부터 1954년 11월까지 어떤 학교에 다녔습니까?"라는 질문은 특정 시간 범위 제약으로 인해 직접적으로 대답하기 어려운 문제입니다.

![이미지](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_6.png)

이러한 경우에는 보다 폭넓은 질문을 제시하면 모델이 구체적인 쿼리에 효과적으로 답변하는 데 도움이 될 수 있습니다. 특정 시간에 에스텔라 레오폴드가 다녔던 학교를 직접 묻는 대신, "에스텔라 레오폴드의 교육 배경"에 대해 묻는 것이 좋습니다.

<div class="content-ad"></div>

이 보다 포괄적인 주제는 원래의 질문을 포함하며, "에스텔라 레오폴드가 특정 시기에 다닌 학교는 무엇인가"를 추론할 수 있는 모든 필요한 정보를 제공할 수 있습니다. 이러한 포괄적인 질문들이 원래의 구체적인 질문보다 답하기 쉬운 경우가 많다는 점을 주목하는 것이 중요합니다.

이러한 추상화에서 유래된 추론은 "사고 연쇄"로서 그림 5 (좌측)에 표시된 중간 단계에서 오류를 방지하는 데 도움이 됩니다.

요약하면, 뒤로 밀어내는 질문은 두 가지 기본 단계로 이루어져 있습니다:

- 추상화: 우선, 우리는 LLM에게 질문을 하도록 하여 고수준 개념이나 원칙에 대한 포괄적인 질문을 하도록 유도하여 직접적으로 쿼리에 대답하는 대신에 관련된 사실을 검색합니다.
- 추론: LLM은 이러한 고수준 개념이나 원칙에 대한 사실을 바탕으로 원래 질문에 대한 답을 추론할 수 있습니다. 이것을 추상적 추론이라고 합니다.

<div class="content-ad"></div>

확인할 수 있는 것처럼, 스텝백 프롬프팅이 컨텍스트 검색 및 예측 성능에 어떤 영향을 미치는지 설명하려면 Langchain을 사용하여 구현된 데모 코드가 있습니다.

환경 설정 및 라이브러리 가져오기:

```js
import os
os.environ["OPENAI_API_KEY"] = "당신의 오픈AI 키"

from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate, FewShotChatMessagePromptTemplate
from langchain_core.runnables import RunnableLambda
from langchain_openai import ChatOpenAI
from langchain_community.utilities import DuckDuckGoSearchAPIWrapper
```

체인을 구축하고 원래 쿼리를 실행하세요:

<div class="content-ad"></div>

```python
데이터를 찾았습니다. ChatGPT는 트럼프가 대통령이었을 때 이미 존재했습니다. ChatGPT는 OpenAI에서 개발한 인공지능 언어 모델로, 11월 말에 대중에 공개되었습니다. 이 모델은 에세이, 이야기, 그리고 노래 가사를 생성할 수 있습니다. 이는 방금 전에 바이든 대통령에 관한 시를 쓰는 데 사용될 수도 있지만 구체적인 전 대통령 트럼프를 다루는 가상 시나리오 등 다른 여러 상황에 사용될 수도 있습니다.
```

<div class="content-ad"></div>

step_back_question_chain 및 step_back_chain을 올바른 결과를 얻기 위해 구성하기 시작하세요.

```js
# Few Shot Examples
examples = [
    {
        "input": "The Police의 구성원들이 합법적으로 체포를 할 수 있나요?",
        "output": "The Police의 구성원들이 무엇을 할 수 있는가요?",
    },
    {
        "input": "Jan Sindel은 어느 나라에서 태어났나요?",
        "output": "Jan Sindel의 개인적 역사가 무엇인가요?",
    },
]
# 이제 이를 예제 메시지로 변환합니다.
example_prompt = ChatPromptTemplate.from_messages(
    [
        ("human", "{input}"),
        ("ai", "{output}"),
    ]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
    example_prompt=example_prompt,
    examples=examples,
)

step_back_prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            """세계 지식 전문가입니다. 질문을 더 일반적인 형태로 바꾸는 작업을 해야 합니다. 다음은 몇 가지 예시입니다:""",
        ),
        # Few shot examples
        few_shot_prompt,
        # New question
        ("user", "{question}"),
    ]
)
step_back_question_chain = step_back_prompt | ChatOpenAI(temperature=0) | StrOutputParser()
june_print('단계 전 질문:', step_back_question_chain.invoke({"question": question}))
june_print('단계 전 질문의 검색된 맥락:', retriever(step_back_question_chain.invoke({"question": question})) )

response_prompt_template = """세계 지식 전문가입니다. 질문을 드리겠습니다. 답변은 포괄적이어야 하며 다음과 관련이 있는 경우에는 상반되지 않아야 합니다. 관련이 없는 경우는 무시하세요.

{normal_context}
{step_back_context}

원래 질문: {question}
답변:"""
response_prompt = ChatPromptTemplate.from_template(response_prompt_template)

step_back_chain = (
    {
        # 보통 질문을 사용하여 맥락 검색
        "normal_context": RunnableLambda(lambda x: x["question"]) | retriever,
        # 단계 전 질문을 사용하여 맥락 검색
        "step_back_context": step_back_question_chain | retriever,
        # 질문 전달
        "question": lambda x: x["question"],
    }
    | response_prompt
    | ChatOpenAI(temperature=0)
    | StrOutputParser()
)

june_print('단계 전 체인의 결과:', step_back_chain.invoke({"question": question}) )
```

결과는 다음과 같습니다:

```js
----------------------------------------------------------------------------------------------------
단계 전 질문:
ChatGPT는 언제 사용 가능해졌습니까?
----------------------------------------------------------------------------------------------------
단계 전 질문 검색된 맥락:
OpenAI는 ChatGPT의 초기 데모를 2022년 11월 30일에 공개했으며, 이 챗봇은 빠르게 소셜 미디어에서 확산되어 사용자들이 그것이 무엇을 할 수 있는지 예시를 공유했습니다. 이야기와 샘플은 ... 2023년 3월 14일 - Anthrop... API에서 거의 일년 동안 기본 모델이 사용 가능했습니다. 다른 의미에서 우리는 사람들이 그것을 사용하여 하려는 바에 더 맞게 만들었습니다. 유료 ChatGPT Plus 구독 서비스가 있습니다. (이미지 크레딧: OpenAI) ChatGPT는 OpenAI가 말하는 GPT-3.5 시리즈의 언어 모델을 기반으로 하며, 이 모델은 2022년 초에 훈련을 마쳤다고 합니다.
----------------------------------------------------------------------------------------------------
단계 전 체인 결과:
아니요, ChatGPT는 트럼프가 대통령이었을 때 존재하지 않았습니다. ChatGPT는 트럼프 대통령의 임기가 끝난 후인 11월 말에 대중에게 공개되었습니다. 제공된 맥락에 나오는 ChatGPT 관련 항목은 모두 트럼프 대통령의 임기 이후로 날짜가 지정되어 있습니다. 예를 들어, 2022년 11월 30일에 초기 데모가 공개되고 ChatGPT Plus 구독 서비스가 론칭된 것과 같은 내용입니다. 따라서 ChatGPT는 트럼프 대통령의 임기 중에는 존재하지 않았다고 말할 수 있습니다.
```

<div class="content-ad"></div>

우리는 원본 질문을 더 추상적인 문제로 "후퇴"시키고, 추상화된 질문과 원본 질문을 모두 검색에 활용함으로써, LLM은 올바른 추론 경로를 따라 해결책으로 나아가는 능력을 향상시킨다.

에드스거 W. 디익스트라가 말한 대로, "추상화의 목적은 모호할 것이 아니라, 절대적으로 정확할 수 있는 새로운 의미 수준을 만들어내는 것"입니다.

# Query2doc

대규모 언어 모델을 활용한 쿼리 확장인 Query2doc은 query2doc을 소개합니다. 이는 LLM의 몇 가지 프롬프트를 활용해 가상 문서를 생성하고, 이를 원본 쿼리와 결합하여 새로운 쿼리를 생성합니다. Figure 6에서 보여지듯이:

<div class="content-ad"></div>

![Image](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_7.png)

In Dense Retrieval, the new query, denoted as q+, is a simple concatenation of the original query (q) and pseudo-documents (d’), separated by [SEP]: q+ = concat(q, [SEP], d’).

Query2doc believes that HyDE implicitly assumes that the groundtruth document and pseudo-documents express the same semantics in different words, which may not hold for some queries.

Another distinction between Query2doc and HyDE is that Query2doc trains a supervised dense retriever, as outlined in the paper.

<div class="content-ad"></div>

Currently, Langchain이나 LlamaIndex에서 query2doc의 복제본을 찾을 수 없었습니다.

# ITER-RETGEN

ITER-RETGEN 접근법은 생성된 콘텐츠를 검색 안내에 활용합니다. 이 방법은 "검색 강화 생성(retrieval-enhanced generation)"과 "생성 강화 검색(generation-enhanced retrieval)"을 반복적으로 수행하여 Retrieve-Read-Retrieve-Read 흐름 내에서 작동합니다.

![이미지](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_8.png)

<div class="content-ad"></div>

Figure 7에서 보듯이, 주어진 질문 q와 검색 말뭉치 D = 'd'에 대해, 여기서 d는 하나의 단락을 나타냅니다. ITER-RETGEN은 계속해서 검색 생성을 T번 반복합니다.

각 반복 t에서, 우리는 먼저 이전 반복에서의 생성 yt-1을 사용하여, 이를 q와 결합하고 상위 k개의 단락을 검색합니다. 그 다음, 우리는 LLM M에게 출력 yt를 생성하도록 유도하며, 이는 검색된 단락을 (Dyt-1||q로 표현)과 q를 프롬프트에 통합합니다. 따라서, 각 반복은 다음과 같이 설계될 수 있습니다:

![이미지](/assets/img/2024-07-13-AdvancedRAG06ExploringQueryRewriting_9.png)

마지막 출력 yt는 최종 응답으로 생성될 것입니다.

<div class="content-ad"></div>

Query2doc과 유사하게, Langchain이나 LlamaIndex에서는 아직 복제본을 찾지 못했습니다.

# 결론

이 글은 코드 예시를 포함한 다양한 쿼리 재작성 기술을 소개했습니다.

실제로 이러한 쿼리 재작성 방법은 모두 시도해볼 수 있으며, 어떤 방법이나 방법 조합을 사용할지는 특정 효과에 따라 다릅니다.

<div class="content-ad"></div>

그렇지만, LLM을 활용하면 물론 재작성 방법에 따라 성능 트레이드 오프가 발생하는데, 실제 사용 시 이를 고려해야 합니다.

게다가 쿼리 라우팅, 쿼리를 여러 하위 질문으로 분해하는 등 몇 가지 메소드들은 쿼리 재작성에 속하지는 않지만 사전 검색 방법으로, 이러한 방법들은 향후 소개될 가능성이 있습니다.

RAG에 관심이 있다면 저의 다른 기사들을 확인해보세요.

그리고 최신 AI 관련 콘텐츠는 제 뉴스레터에서 찾아볼 수 있습니다.

<div class="content-ad"></div>

마지막으로, 만일 이 기사에 부정확한 점이나 누락된 내용이 있거나 궁금한 점이 있다면, 댓글 섹션에 남겨주시면 감사하겠습니다.

## Medium의 부스트 / AI 예측 / 무료 GPT 대안 / 비디오2월드