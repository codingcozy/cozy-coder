---
title: "Gemma 7B LLM과 Upstash 벡터 데이터베이스를 활용한 RAG 애플리케이션 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_0.png"
date: 2024-07-12 23:21
ogImage:
  url: /assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_0.png
tag: Tech
originalTitle: "Building RAG Application using Gemma 7B LLM , Upstash Vector Database"
link: "https://medium.com/towards-artificial-intelligence/building-rag-application-using-gemma-7b-llm-upstash-vector-database-ff50b715d906"
isUpdated: true
---

Retrieval-Augmented Generation (RAG)은 외부 지식 출처에서 큰 언어 모델(Large Language Models, LLMs)에게 추가 정보를 제공하는 개념입니다. 이를 통해 그들은 더 정확하고 맥락적인 답변을 생성하면서 환각을 줄일 수 있습니다. 이 글에서는 구글 젬마 7B와 Upstash 서버리스 벡터 데이터베이스를 활용해 완전한 RAG 애플리케이션을 만드는 단계별 안내를 제공할 것입니다.

![Building RAG Application using Gemma 7B LLM Upstash Vector Database](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_0.png)

## 목차:

- 시작하기 및 작업 환경 설정
- Cosmopedia 데이터셋 다운로드 및 분할
- 문장 트랜스포머 모델로 임베딩 생성
- Upstash 벡터 데이터베이스에 임베딩 저장
- Gemma 7B LLM 소개 및 활용
- RAG 애플리케이션 질의하기

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

## 여기서 Upstash Vector Database를 무료로 사용해보실 수 있어요:

# 1. 시작하기 & 작업 환경 설정

RAG 애플리케이션을 구축하는 첫 번째 단계는 작업 환경을 준비하는 것입니다. 애플리케이션을 구축할 때 사용할 패키지를 다운로드하는 것부터 시작해보겠어요:

```bash
%pip install -q -U langchain torch transformers sentence-transformers datasets tiktoken upstash_vector
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

다음으로 사용할 패키지와 라이브러리를 가져오겠습니다:

```python
import torch
from upstash_vector import Vector
from datasets import load_dataset
from tqdm import tqdm, trange
from langchain_community.document_loaders.csv_loader import CSVLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import HuggingFaceEmbeddings
from transformers import AutoTokenizer, AutoModelForCausalLM
from transformers import AutoTokenizer, pipeline
from langchain import HuggingFacePipeline
from langchain.chains import RetrievalQA
```

검색 지식을 제공하는 기능인 검색 증강 생성(Retrieval-Augmented Generation, RAG)은 LLMs에 외부 지식 출처로부터 추가 정보를 제공하는 개념입니다. 이를 통해 더 정확하고 맥락적인 답변을 생성할 수 있으며 환각을 줄일 수 있습니다. RAG에는 두 가지 주요 구성 요소가 있습니다:

- 검색: 정보 검색(IR) 구성 요소는 방대한 텍스트 데이터(일반적으로 문서 또는 단락)를 탐색하여 특정 사용자 쿼리에 대한 가장 관련 있는 정보를 식별합니다. 이 과정은 시스템이 문서 뿐만 아니라 쿼리 의도와 일치하는 특정 텍스트 단락도 검색하는 밀집 통로 검색과 같은 기술을 포함합니다.
- 생성: 검색된 단락은 강력한 언어 처리 엔진 역할을 하는 LLM에 입력됩니다. LLM은 이러한 단락을 분석하고 언어적 이해를 활용하여 사용자 쿼리에 종합적으로 대답하는 응답을 생성합니다.

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

사용자가 LLM에게 질문을 하면 LLM에 직접 물어볼 대신 이 질의에 대한 임베딩을 생성한 후, 유지되는 지식 라이브러리에서 관련 데이터를 검색한 다음 그 문맥을 사용하여 답변을 반환합니다.

우리는 벡터 임베딩(숫자로 표현된 데이터)을 사용하여 요청된 문서를 검색합니다. 벡터 데이터베이스에서 필요한 정보를 찾은 후, 그 결과가 사용자에게 반환됩니다.

이를 통해 환상의 가능성을 크게 줄이고 비용이 많이 드는 모델 재학습 없이 모델을 최신 상태로 업데이트합니다. 다음은 이 과정을 보여주는 매우 간단한 다이어그램입니다.

[다이어그램](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_1.png)

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

# 2. Cosmopedia 데이터셋 다운로드 및 분할하기

![image](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_2.png)

저희는 가장 최신 기술인 RAG (Retrieval-Augmented Generation) 애플리케이션을 만들기 위해 Hugging Face에서 호스팅하는 Cosmopedia 데이터셋을 전략적으로 선택했습니다.

이 신중하게 편집된 데이터셋에는 교과서, 블로그 게시물, 서술, 소셜 미디어 글 및 WikiHow 기사 등 다양한 합성 텍스트 소스가 포함되어 있습니다. Mixtral-8x7B-Instruct-v0.1 모델에 의해 생성된 Cosmopedia는 감동적인 컬렉션이며, 3천만 개 이상의 개별 파일과 놀라운 250억 개의 토큰을 자랑합니다.

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

그 규모와 포괄성 자체가 현재 사용 가능한 가장 큰 오픈 합성 데이터셋으로 이를 보면 자연어 처리 및 생성에서의 혁신적 노력에 활용할 수 있는 풍부한 자료가 제공됩니다.

Cosmopedia 데이터셋에는 8개 하위 데이터셋이 포함되어 있습니다:

- auto_math_text: 195만 행
- khanacademy: 2.41만 행
- Openstax: 12.6만 행
- Stanford: 102만 행
- Stories: 40만 행
- web_samples_v1: 1240만 행
- web_samples_v2: 1030만 행
- wikiHow: 17.9만 행

우리는 ‘Stanford’ 하위 데이터셋과 계속 작업할 것입니다. 데이터셋을 로드하기 위해 datasets 라이브러리를 활용할 것입니다.

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

다음으로 pandas dataframe으로 변환하고 CSV 파일로 저장할 거에요.

```python
data = data.to_pandas()
data.to_csv("stanford_dataset.csv")
data.head()
```

![Image](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_3.png)

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

오늘 우리는 시스템에 데이터셋을 저장한 후 LangChain CSVLoader 메소드를 사용하여 이 데이터셋을 로드할 것입니다.

```js
loader = CSVLoader((file_path = "./stanford_dataset.csv"));
data = loader.load();
```

이제 데이터가 로드되었으니, 데이터 안의 문서들을 모델의 컨텍스트 윈도우에 맞게 작은 조각들로 나눌 필요가 있습니다.

긴 텍스트를 다루어야 할 때는 이를 조각내는 것이 필요합니다. 이런 작업은 간단히 들리지만 그 안에는 잠재적인 복잡성이 많이 있습니다. 의미론적으로 관련된 텍스트 조각들을 함께 유지하도록 하세요.

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

LangChain에는 문서를 쉽게 분할, 병합, 필터링 및 기타 조작할 수 있는 다양한 문서 변환기가 내장되어 있습니다. 우리는 RecursiveCharacterTextSplitter를 사용할 것인데, 이는 재귀적으로 다양한 문자로 분할을 시도하여 작동하는 문자를 찾아냅니다. 청크 크기 = 1000으로 설정하고 청크 중복 = 150으로 설정할 것입니다.

```js
text_splitter = RecursiveCharacterTextSplitter((chunk_size = 1000), (chunk_overlap = 150));
docs = text_splitter.split_documents(data);
```

# 3. 문장 Transformer 모델을 사용하여 임베딩 생성

![이미지](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_4.png)

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

다음 단계는 불러온 텍스트에 대한 임베딩을 생성하는 것입니다. 임베딩을 생성하는 것은 RAG 애플리케이션을 구축하는 데 필수적인 단계로, 텍스트에 대한 의미와 맥락을 인코딩하여 의미와 문맥을 이해하는 데 중요합니다. 이는 질문과 문서 임베딩을 비교하여 검색 정확도를 향상시키는 검색 및 생성 프로세스에 도움이 됩니다.

우리는 SentenceTransformers의 all-MiniLM-L6-v2 임베딩 모델을 사용하여 임베딩을 생성할 것입니다. 먼저, 아래 코드를 사용하여 이를 초기화할 것입니다:

```js
modelPath = "sentence-transformers/all-MiniLM-l6-v2";
model_kwargs = { device: "cpu" };
encode_kwargs = { normalize_embeddings: False };
embeddings = HuggingFaceEmbeddings(
  (model_name = modelPath),
  (model_kwargs = model_kwargs),
  (encode_kwargs = encode_kwargs)
);
```

이를 초기화한 후에 우리는 이를 문서에 적용하여 임베딩을 생성할 수 있습니다:

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

# 텍스트 조각마다 임베딩 생성하기

chunk_embeddings = []
for doc in docs: # 각 조각에 대한 임베딩 생성
chunk_embedding = embeddings.encode(doc)
chunk_embeddings.append(chunk_embedding)

임베딩을 생성한 후에는 벡터 데이터베이스에 저장해야 합니다. 벡터 데이터베이스는 임베딩과 같은 고차원 데이터에 대해 최적화된 인덱싱 및 클러스터링 기술을 지원하며, 이는 검색 효율성과 정확도를 더욱 향상시킬 수 있습니다. 우리는 Uptstash 벡터 데이터베이스를 사용하여 임베딩을 저장할 것입니다.

## 4. Upstash 벡터 데이터베이스에 임베딩 저장하기

![이미지](https://example.com/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_5.png)

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

우리는 이전에 생성된 벡터 임베딩을 저장하기 위해 Upstash Vector 데이터베이스를 사용할 것입니다. Upstash Vector는 벡터 임베딩과 함께 작업하기 위해 설계된 서버리스 벡터 데이터베이스입니다.

서버리스 모델로 작동하기 때문에 호스팅 및 관리 복잡성을 추상화하여 추가적인 복잡성없이 LLM 애플리케이션을 구축하는 데 집중할 수 있습니다. API 호출에 기반한 요금이 부과됩니다.

384차원과 Dot_Product 거리로 무료 벡터 데이터베이스를 Upstash Console에서 만들 것입니다. 차원 크기는 임베딩 모델의 차원과 일치해야하기 때문에 중요합니다.

![Image](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_6.png)

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

벡터 인덱스를 생성한 후 벡터 REST URL 및 벡터 REST 토큰에 액세스할 수 있게 됩니다. 코드에서 이들을 정의하여 생성된 벡터 데이터베이스에 액세스할 수 있게 됩니다.

```js
UPSTASH_VECTOR_REST_URL = "<YOUR_UPSTASH_VECTOR_REST_URL>";
UPSTASH_VECTOR_REST_TOKEN = "<YOUR_UPSTASH_VECTOR_REST_TOKEN>";
```

생성된 임베딩을 벡터 데이터베이스에 삽입하기 위해 그들을 벡터 객체로 변환해야 합니다. 아래의 코드를 사용하여 이 작업을 수행할 것입니다:

```js
from tqdm import tqdm, trange
from upstash_vector import Vector

vectors = []

# 10개 단위로 배치로 벡터 생성합니다
batch_count = 10

for i in trange(0, len(chunks), batch_count):
    batch = chunks[i:i+batch_count]

    embeddings = chunk_embedding[batch]

    for i, chunk in enumerate(batch):
        vec = Vector(id=f"chunk-{i}", vector=embeddings[i], metadata={
            "text": chunk
        })

        vectors.append(vec)
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

벡터를 생성한 후에는 한꺼번에 모든 벡터를 색인에 올릴 수 있습니다. Upstash는 무료 인덱스 당 1000개의 벡터를 지원합니다.

```python
from upstash_vector import Index

index = Index(
    url=UPSTASH_VECTOR_REST_URL,
    token=UPSTASH_VECTOR_REST_TOKEN
)

# 인덱스를 리셋하려면 먼저 주석 처리된 코드를 활성화하세요
# index.reset()

index.upsert(vectors)
```

# 5. 젬마 7B LLM 소개 및 사용하기

![BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_7.png](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_7.png)

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

Gemma는 Gemini를 기반으로 한 Google의 새로운 LLM 모델 패밀리로 4가지 모델이 있습니다. Gemma에는 2가지 크기가 있으며, 각각 베이스(Pretrained)와 인스트럭션 튜닝 버전이 있습니다.

모든 변형 모델은 양자화 없이도 다양한 종류의 소비자 하드웨어에서 실행할 수 있으며, 8K 토큰의 컨텍스트 길이를 가지고 있습니다:

- gemma-7b: 7B 베이스 모델
- gemma-7b-it: 베이스 7B 모델의 인스트럭션 튜닝 버전
- gemma-2b: 2B 베이스 모델
- gemma-2b-it: 베이스 2B 모델의 인스트럭션 튜닝 버전

Gemma 모델을 사용하려면 Hugging Face에서 약관에 동의해야 합니다. 그 후에는 Hugging Face 토큰을 로그인할 때 전달해야 합니다.

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

다음으로, 저희는 토크나이저와 모델을 초기화할 거에요

```js
model = AutoModelForCausalLM.from_pretrained("google/gemma-7b");
tokenizer = AutoTokenizer.from_pretrained("google/gemma-7b", (padding = True), (truncation = True), (max_length = 512));
```

텍스트 생성 파이프라인을 만들어봐요.

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

pipe = pipeline(
"text-generation",
model=model,
tokenizer=tokenizer,
return_tensors='pt',
max_length=512,
max_new_tokens=512,
model_kwargs={"torch_dtype": torch.bfloat16},
device="cuda"
)

Tarot readers, get ready to unlock the mystic powers of the LLM with this simple initialization! 🌟

llm = HuggingFacePipeline(
pipeline=pipe,
model_kwargs={"temperature": 0.7, "max_length": 512},
)

Let's delve into the realm of unknown possibilities by harnessing the vector store and the LLM for captivating question-answering revelations! 🔮✨

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

# 6. RAG 어플리케이션에서 답변을 조회하는 방법

![image](/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_8.png)

마지막 단계는 벡터 저장소와 LLM을 사용하여 답변을 생성하는 것입니다. 먼저, 입력 쿼리 또는 질문에 대한 임베딩을 생성하고 벡터 저장소에서 컨텍스트를 검색하여 이를 LLM에 전달하여 답변을 생성합니다:

```js
def ask_question(question):

    # 질문에 대한 임베딩 가져오기
    question_embedding = embeddings.encode(doc)

    # 유사한 벡터 검색
    res = index.query(vector=question_embedding, top_k=5, include_metadata=True)

    # 컨텍스트에서 결과 수집
    context = "\n".join([r.metadata['text'] for r in res])

    prompt = f"Question:{question}\n\nContext: {context}"

    # LLM을 사용하여 답변 생성
    answer = llm(prompt)

    # 생성된 답변 반환
    return answer[0]['generated_text']
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

The RAG pipeline is now ready, and we can pass an input query to observe the output and its performance.

```js
ask_question("Write an educational story for young children.");
```

## If you enjoyed the article and would like to support me, please take the following actions:

- 👏 Give the story a round of applause (50 claps) to help it get featured.
- Subscribe to the To Data & Beyond Newsletter.
- Follow me on Medium.
- 📰 View more content on my Medium profile.
- 🔔 Follow Me: LinkedIn | Youtube | GitHub | Twitter

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

## 내 뉴스레터 '데이터 & 비욘드'를 구독하시면 제 기사에 대한 전체적이고 일찍 접근할 수 있습니다:

## 데이터 과학과 인공지능 분야에서 경력을 시작하려는가 막연한가요? 데이터 과학 멘토링 세션과 장기 경력 멘토링을 제공하고 있습니다:

- 멘토링 세션: [링크](https://lnkd.in/dXeg3KPW)
- 장기 멘토링: [링크](https://lnkd.in/dtdUYBrM)

![이미지](https://yourwebsite.com/assets/img/2024-07-12-BuildingRAGApplicationusingGemma7BLLMUpstashVectorDatabase_9.png)
