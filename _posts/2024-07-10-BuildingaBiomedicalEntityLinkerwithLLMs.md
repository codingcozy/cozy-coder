---
title: "LLM을 사용하여 생물의학 엔티티 링크 시스템 구축하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_0.png"
date: 2024-07-10 00:27
ogImage: 
  url: /assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_0.png
tag: Tech
originalTitle: "Building a Biomedical Entity Linker with LLMs"
link: "https://medium.com/towards-data-science/building-a-biomedical-entity-linker-with-llms-d385cb85c15a"
---


## 생명과학 개체 링킹에 LLM을 효과적으로 적용하는 방법은 무엇인가요?

![Building a Biomedical Entity Linker with LLMs](/assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_0.png)

생명과학 텍스트는 연구 논문, 임상 시험 보고서 및 환자 기록과 같은 문서를 포괄하는 용어로, 다양한 생물학, 의학, 및 과학적 개념에 대한 풍부한 정보 저장소로 기능합니다. 생명 과학 분야의 연구 논문은 약물 발견, 약물 부작용 및 새로운 질병 치료에 대한 혁신적인 발전을 제공합니다. 임상 시험 보고서는 새로운 약물이나 치료의 안전성, 효과성 및 부작용에 대한 심층적인 세부 정보를 제공합니다. 한편, 환자 기록에는 의사와 의료 전문가가 기록한 포괄적인 의료력, 진단, 치료 계획 및 결과가 포함되어 있습니다.

이러한 텍스트를 채굴하면 실무자들이 가치 있는 통찰력을 얻을 수 있으며, 이는 다양한 하류 작업에 도움이 될 수 있습니다. 예를 들어, 텍스트를 채굴하여 약물의 부작용을 식별하거나 자동 의료 코딩 알고리즘을 구축하거나 방대한 연구 말뭉치에서 정보를 추출하기 위해 정보 검색 또는 질문-응답 시스템을 구현할 수 있습니다. 그러나 생명 과학 문서 처리에 영향을 미치는 한 가지 문제는 텍스트의 종종 구조화되지 않은 성격입니다. 예를 들어, 연구자들은 동일한 개념을 지칭하기 위해 서로 다른 용어를 사용할 수 있습니다. 한 연구자가 “심근 경색”이라고 부르는 것은 다른 연구자가 “심근경험”이라고 부를 수 있습니다. 마찬가지로, 약 관련 문서에서 기술 용어와 일반적인 이름이 서로 교차적으로 사용될 수 있습니다. 예를 들어, “아세트아미노펜”은 한 약물의 기술적인 이름이며, “파라세타몰”은 더 일반적인 대응입니다. 약어의 보편화도 복잡성의 추가 요소로 작용합니다. 예를 들어, “질산화아질소”는 다른 맥락에서 “NO”로 언급될 수 있습니다. 동일한 개념을 가리키는 이러한 다양한 용어에도 불구하고 이러한 차이로 인해 비전문가나 텍스트 처리 알고리즘은 해당 개념이 동일한지 여부를 결정하기 어려워집니다. 따라서 이러한 상황에서 Entity Linking이 매우 중요해집니다.

<div class="content-ad"></div>

# 목차:

- Entity Linking이란 무엇인가?
- LLM이 여기에 어떻게 관련되어 있는가?
- 실험 설정
- 데이터셋 처리
- LLM을 활용한 제로샷 Entity Linking
- Entity Linking을 위한 LLM과 검색 증진 생성
- LLM 및 외부 KB Linker를 사용한 제로샷 Entity 추출
- LLM 및 외부 KB Linker를 사용한 정교한 Entity 추출
- Scispacy의 벤치마킹
- 주요 요점
- 한계
- 참고 문헌

# Entity Linking이란?

텍스트가 구조화되지 않은 상태에서 의료 개념을 정확하게 식별하고 표준화하는 것이 중요해집니다. 이를 달성하기 위해 통합 의료 언어 체계(UMLS) [1], 의학 용어 체계(SNOMED-CT) [2], 의학 주제 헤딩(MeSH) [3]와 같은 의료 용어 체계가 중요한 역할을 합니다. 이러한 시스템들은 의료 개념들의 포괄적이고 표준화된 집합을 제공하며, 각각은 알파벳과 숫자로 구성된 코드로 고유하게 식별됩니다.

<div class="content-ad"></div>

Entity linking은 텍스트 내에서 entity를 인식하고 추출하여 큰 용어집핍에서 표준화된 개념으로 매핑하는 것을 의미합니다. 이 문맥에서 지식 베이스(KB)는 의학 용어, 질병 및 약물과 관련된 표준화된 정보 및 개념을 포함하는 상세한 데이터베이스를 가리킵니다. 일반적으로 KB는 전문가가 선별하고 설계하며, 다양한 용어의 변형이나 다른 개념과의 관련 정보를 포함하는 상세한 정보가 포함되어 있습니다.

![이미지](/assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_1.png)

Entity recognition은 작업 문맥에서 중요한 단어나 구를 추출하는 것을 의미합니다. 이 문맥에서는 주로 약물, 질병 등과 관련된 생체의학 용어 추출을 의미합니다. 일반적으로 entity recognition에는 조회 기반 방법이나 기계 학습/딥 러닝 기반 시스템이 사용됩니다. entity를 KB에 링크하는 것은 일반적으로 KB를 인덱싱하는 검색 시스템을 통해 이루어집니다. 이 시스템은 이전 단계에서 추출된 각 entity를 가져와 KB에서 해당하는 식별자를 검색합니다. 여기서 retriever도 핵심 요소로, sparse한(예: BM-25), dense한(임베딩 기반) 또는 LLM(대형 언어 모델)과 같이 KB를 파라미터로 인코딩한 생성 시스템인 경우도 있습니다.

# 여기서 LLM이 어떻게 관련되는지요?

<div class="content-ad"></div>

For a while, I've been diving into the intriguing realm of integrating LLMs into biomedical and clinical text-processing pipelines. Since Entity Linking plays a vital role in these pipelines, I embarked on a journey to explore the most effective ways LLMs can be harnessed for this purpose. Here's what I delved into:

- **Zero-Shot Entity Linking with an LLM**: The power of an LLM to directly pinpoint entities and concept IDs from input biomedical texts without any fine-tuning.
- **LLM with Retrieval Augmented Generation (RAG)**: Embedding the LLM within a RAG setup by infusing information about relevant concept IDs in the prompt for entity linking.
- **Zero-Shot Entity Extraction with LLM with an External KB Linker**: Leveraging the LLM for zero-shot entity extraction from biomedical texts, with assistance from an external linker/retriever to map the entities to concept IDs.
- **Fine-tuned Entity Extraction with an External KB Linker**: Tweaking the LLM initially for the entity extraction task, employing it as an entity extractor with support from an external linker/retriever to map the entities to concept IDs.
- **Comparison with an existing pipeline**: How do these approaches stack up against Scispacy, a widely used library for biomedical text processing?

# Experimental Setup

To delve into these experiments, we opt for the Mistral-7B Instruct [9] as our LLM. For aligning the medical jargon to entities, we rely on the MeSH terminology. Quoting the National Library of Medicine website:

<div class="content-ad"></div>

BioCreative-V-CDR-Corpus [4,5,6,7,8]을 사용하여 평가합니다. 이 데이터셋에는 질병 및 화학물질 엔티티에 대한 주석과 해당 MeSH ID가 포함되어 있습니다. 평가 목적으로 테스트 세트에서 무작위로 100개의 데이터 포인트를 샘플링합니다. Scispacy [10,11]에서 제공한 MeSH KB의 버전을 사용했는데, 이 KB에는 MeSH 식별자에 대한 정보 (정의 및 해당 ID에 대응하는 엔티티와 같은)가 포함되어 있습니다.

성능 평가를 위해 두 가지 메트릭을 계산합니다. 첫 번째 메트릭은 엔티티 추출 성능에 관련된 것입니다. 원본 데이터셋에는 텍스트의 모든 엔티티 언급이 포함되어 있으며, 부분 문자 수준에서 주석이 달려 있습니다. 엄격한 평가 방법은 알고리즘이 모든 엔티티의 모든 발생을 출력했는지 확인하는 것입니다. 그러나 더 쉬운 평가를 위해 이 프로세스를 간소화했습니다; 우리는 주어진 명단의 엔티티를 소문자로 변환하고 중복을 제거했습니다. 그런 다음 각 인스턴스에 대해 정밀도, 재현율 및 F1 점수를 계산하고 각 메트릭에 대한 매크로 평균을 계산했습니다.

실제 엔티티 집합인 ground_truth와 각 입력 텍스트에 대한 모델이 예측한 엔티티 집합인 pred가 있다고 가정해보겠습니다. TP(True Positives)는 pred와 ground_truth 사이의 공통 요소를 식별하여 결정할 수 있습니다. 이 두 집합의 교집합을 계산함으로써 이를 수행합니다.

각 입력에 대해 다음을 계산할 수 있습니다:

<div class="content-ad"></div>

정밀도 = TP 수 / pred 수,

재현율 = TP 수 / ground_truth 수이며,

f1 = 2 * 정밀도 * 재현율 / (정밀도 + 재현율)

그리고 마지막으로 각 메트릭에 대한 매크로 평균을 계산하여 모든 값을 합한 다음 테스트 세트의 데이터 포인트 수로 나눕니다.

<div class="content-ad"></div>

오늘도 엔티티 링킹 성능을 평가해 보겠습니다. 각 입력 데이터 포인트마다 (엔티티, mesh_id) 쌍으로 이루어진 튜플 집합이 있습니다. 메트릭은 이전과 같이 계산됩니다.

# 데이터셋 처리

그럼, 데이터셋을 처리하는 데 도움이 되는 몇 가지 헬퍼 함수부터 정의해 보겠습니다.

```javascript
def parse_dataset(file_path):
    """
    BioCreative 데이터셋 파싱.

    Args:
    - file_path (str): 문서가 포함된 파일 경로.

    Returns:
    - dict의 리스트: 각 요소가 문서를 나타내는 사전인 리스트.
    """
    문서 = []
    현재_문서 = None

    with open(file_path, 'r', encoding='utf-8') as file:
        for line in file:
            line = line.strip()
            if not line:
                continue
            if "|t|" in line:
                if current_doc:
                    documents.append(current_doc)
                id_, title = line.split("|t|", 1)
                current_doc = {'id': id_, 'title': title, 'abstract': '', 'annotations': []}
            elif "|a|" in line:
                _, abstract = line.split("|a|", 1)
                current_doc['abstract'] = abstract
            else:
                parts = line.split("\t")
                if parts[1] == "CID":
                    continue
                annotation = {
                    'text': parts[3],
                    'type': parts[4],
                    'identifier': parts[5]
                }
                current_doc['annotations'].append(annotation)

        if current_doc:
            documents.append(current_doc)

    return documents

def deduplicate_annotations(documents):
    """
    주석 일관성을 보장하기 위해 문서 필터링.

    Args:
    - documents (dict의 리스트): 확인할 문서 목록.
    """
    for doc in documents:
        doc["annotations"] = remove_duplicates(doc["annotations"])
        
def remove_duplicates(dict_list):
    """
    딕셔너리 목록에서 중복된 것을 제거.

    Args:
    - dict_list (dict의 리스트): 중복을 제거할 딕셔너리 목록.

    Returns:
    - dict의 리스트: 중복을 제거한 후의 딕셔너리 목록.
    """
    고유_딕트 = []  
    보임 = set()

    for d in dict_list:
        dict_tuple = tuple(sorted(d.items()))
        if dict_tuple not in seen:
            seen.add(dict_tuple)
            unique_dicts.append(d)

    return unique_dicts
```

<div class="content-ad"></div>

원본 데이터셋에서 제공된 텍스트 파일로부터 데이터셋을 먼저 파싱합니다. 원본 데이터셋에는 제목, 초록, 모든 엔티티의 엔티티 유형(질병 또는 화학물질), 텍스트 내에서 정확한 위치를 나타내는 부분 문자열 인덱스, 그리고 MeSH ID가 주석 처리되어 있습니다. 데이터셋을 처리하는 과정에서 몇 가지 단순화를 진행합니다. 부분 문자열 인덱스와 엔티티 유형은 무시합니다. 또한, 동일한 엔티티 이름과 MeSH ID를 공유하는 주석을 중복 제거합니다. 이 단계에서는 대소문자를 구분하여 중복을 제거하는데, 즉, 동일한 엔티티가 문서 전체에서 소문자와 대문자로 나타나더라도 우리는 현재 처리 중인 내용에 두 인스턴스를 모두 유지합니다.

## LLM을 활용한 제로샷 개체 링킹

먼저, LLM이 사전 훈련을 통해 MeSH 용어에 대한 이해를 이미 보유하고 있고 제로샷 개체 링커로 작동할 수 있는지를 결정하고자 합니다. 여기서 제로샷이란 LLM이 바이오의학 텍스트에서 엔티티를 직접 MeSH ID에 링크하는 능력을 외부 KB 링커에 의존하지 않고 내재된 지식을 기반으로 하는 것을 의미합니다. 이 가설은 완전히 비현실적이지 않습니다. 온라인에서 MeSH에 대한 정보가 제공되므로, 근거로 삼은 훈련 단계에서 LLM이 MeSH 관련 정보를 만났을 가능성이 있습니다. 그러나 모델이 이러한 정보로 훈련되었다 하더라도, 생명 과학 용어의 복잡성과 정확한 엔티티 링킹에 필요한 정밀성 때문에 단독으로 제로샷 엔티티 링킹을 효과적으로 수행할 수 있을지는 의문입니다.

이를 평가하기 위해 입력 텍스트를 LLM에 제공하고 엔티티 및 해당 MeSH ID를 예측하도록 직접 프롬프트합니다. 추가적으로, 훈련 데이터셋에서 세 개의 데이터 포인트를 샘플링하여 퓨샷 프롬프트를 생성합니다. 여기서 "제로샷"과 "퓨샷"의 사용법을 명확히 해야 합니다. "제로샷"은 LLM 전체가 이 작업에 대한 특정 훈련 없이 엔티티 링킹을 수행하는 것을 의미하며, "퓨샷"은 이 문맥에서 채택된 프롬프트 전략을 나타냅니다.

<div class="content-ad"></div>

![BuildingaBiomedicalEntityLinkerwithLLMs_2](/assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_2.png)

To calculate our metrics, we have defined functions that evaluate the performance:

```python
def calculate_entity_metrics(gt, pred):
    """
    Calculate precision, recall, and F1-score for entity recognition.
    
    Args:
    - gt (list of dict): A list of dictionaries representing the ground truth entities. 
                         Each dictionary should have a key "text" with the entity text.
    - pred (list of dict): A list of dictionaries representing the predicted entities.
                           Similar to `gt`, each dictionary should have a key "text".

    Returns:
    tuple: A tuple containing precision, recall, and F1-score (in that order).
    """
    # Implementation of the function goes here

def calculate_mesh_metrics(gt, pred):
    """
    Calculate precision, recall, and F1-score for matching MeSH (Medical Subject Headings) codes.

    Args:
    - gt (list of dict): Ground truth data
    - pred (list of dict): Predicted data

    Returns:
    tuple: A tuple containing precision, recall, and F1-score (in that order).
    """
    # Implementation of the function goes here
```

Let’s now run the model and get our predictions:

<div class="content-ad"></div>

```python
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2",  torch_dtype=torch.bfloat16).cuda()
tokenizer = AutoTokenizer.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2")
model.eval()

mistral_few_shot_answers = []
for item in tqdm(test_set_subsample):
    few_shot_prompt_messages = build_few_shot_prompt(SYSTEM_PROMPT, item, few_shot_example)
    input_ids = tokenizer.apply_chat_template(few_shot_prompt_messages, tokenize=True, return_tensors = "pt").cuda()
    outputs = model.generate(input_ids = input_ids, max_new_tokens=200, do_sample=False)   
    # [여기](https://github.com/huggingface/transformers/issues/17117#issuecomment-1124497554)에서 얻은 방법을 통해 토큰화 후 결과를 디코딩합니다.
    gen_text = tokenizer.batch_decode(outputs.detach().cpu().numpy()[:, input_ids.shape[1]:], skip_special_tokens=True)[0]
    mistral_few_shot_answers.append(parse_answer(gen_text.strip()))
```

엔티티 추출 수준에서 LLM은 명시적으로 이 작업을 위해 미세 조정되지 않았음에도 꽤 잘 작동합니다. 그러나 Zero-shot 링커로서의 성능은 전체적으로 1% 미만으로 매우 나쁩니다. 이 결과는 직관적이며, 왜냐하면 MeSH 레이블의 출력 공간이 방대하며 엔티티를 특정 MeSH ID로 정확하게 매핑하는 것은 어려운 작업이기 때문입니다.

# 엔티티 링킹을 위한 검색 보강 생성을 한 LLM

검색 보강 생성(RAG) [12]은 LLM을 외부 KB와 쿼링 함수가 장착된 검색기/링커와 결합하는 프레임워크를 가리킵니다. 각 쿼리에 대해 시스템은 먼저 쿼링 함수를 사용하여 KB에서 쿼리와 관련된 지식을 검색합니다. 그런 다음 검색된 지식과 쿼리를 결합하여 이 결합된 프롬프트를 LLM에 제공하여 작업을 수행합니다. 이 접근 방식은 LLM이 모든 필요한 지식이나 정보를 효과적으로 대답하는 데 갖추고 있지 않을 수 있기 때문에 외부 지식원을 쿼리하여 모델에 지식을 주입합니다.


<div class="content-ad"></div>

RAG 프레임워크를 활용하는 것에는 몇 가지 장점이 있어요:

- 새로운 도메인이나 작업에 기존의 LLM을 활용할 수 있어요. 도메인 특정 섬세한 조정 없이 관련 정보를 질의하고 모델에 제공할 수 있기 때문이죠.
- LLM은 때로 쿼리에 대답할 때 잘못된 답변을 제공하기도 해요(환각). RAG를 LLM과 함께 사용하면 이러한 환각을 크게 줄일 수 있어요. 왜냐하면 LLM이 제공하는 답변은 주어진 지식으로 더욱 사실에 기반하기 때문이거든.

MeSH 용어에 대한 구체적인 지식이 부족한 LLM을 고려하여 RAG 설정이 성능을 향상시킬 수 있는지 조사해봤어요. 이 접근 방식에서 각 입력 단락마다 BM-25 검색기를 사용하여 KB를 쿼리합니다. 각 MeSH ID에 대해 ID의 일반 설명과 연결된 개체 이름에 액세스할 수 있어요. 검색 후, 이 정보를 모델에 엔티티 링킹을 위해 프롬프트를 통해 주입해요.

모델에 제공되는 검색된 ID 수가 엔티티 링킹 프로세스에 미치는 영향을 조사하기 위해 이러한 설정을 실행하고 상위 10, 30 및 50 문서를 모델에 제공하여 엔티티 추출 및 MeSH 개념 식별의 성능을 측정했어요.

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 변경해 주세요.

먼저 저희 BM-25 검색기를 정의해 봅시다:

```js
from rank_bm25 import BM25Okapi
from typing import List, Tuple, Dict
from nltk.tokenize import word_tokenize
from tqdm import tqdm

class BM25Retriever:
    """
    BM25 알고리즘을 사용하여 문서를 검색하는 클래스입니다.
    
    Attributes:
        index (List[int, str]): 문서 ID를 키로, 문서 텍스트를 값으로 가지는 딕셔너리입니다
        tokenized_docs (List[List[str]]): `processed_index`의 문서들을 토큰화한 버전입니다.
        bm25 (BM25Okapi): rank_bm25 패키지의 BM25Okapi 모델의 인스턴스입니다.
    """
    
    def __init__(self, docs_with_ids: Dict[int, str]):
        """
        문서 딕셔너리로 BM25Retriever를 초기화합니다.
        
        Args:
            docs_with_ids (Dict[int, str]): 문서 ID를 키로, 문서 텍스트를 값으로 가지는 딕셔너리입니다.
        """
        self.index = docs_with_ids
        self.tokenized_docs = self._tokenize_docs([x[1] for x in self.index])
        self.bm25 = BM25Okapi(self.tokenized_docs)
            
    def _tokenize_docs(self, docs: List[str]) -> List[List[str]]:
        """
        NLTK의 word_tokenize를 사용하여 문서를 토큰화합니다.
        
        Args:
            docs (List[str]): 토큰화할 문서 목록입니다.
        
        Returns:
            List[List[str]]: 토큰화된 문서 목록입니다.
        """
        return [word_tokenize(doc.lower()) for doc in docs]
    
    def query(self, query: str, top_n: int = 10) -> List[Tuple[int, float]]:
        """
        BM25 모델을 쿼리하고 상위 N개 문서와 점수를 검색합니다.
        
        Args:
            query (str): 쿼리 문자열입니다.
            top_n (int): 상위 문서 개수입니다.
        
        Returns:
            List[Tuple[int, float]]: 문서 ID와 해당 BM25 점수가 포함된 튜플 목록입니다.
        """
        tokenized_query = word_tokenize(query.lower())
        scores = self.bm25.get_scores(tokenized_query)
        doc_scores_with_ids = [(doc_id, scores[i]) for i, (doc_id, _) in enumerate(self.index)]
        top_doc_ids_and_scores = sorted(doc_scores_with_ids, key=lambda x: x[1], reverse=True)[:top_n]
        return [x[0] for x in top_doc_ids_and_scores]
```

이제 KB 파일을 처리하고 그를 색인화하는 BM-25 검색기 인스턴스를 만들어 봅시다. KB를 색인하는 동안, 각 ID를 그 설명, 별칭 및 공식 이름을 연결하여 색인화합니다.

<div class="content-ad"></div>


def process_index(index):
"""
초기 문서 인덱스를 처리하여 별칭, 정규 이름 및 정의를 하나의 텍스트 인덱스로 결합하는 함수입니다.

매개변수:
- index (Dict): MeSH 지식 베이스
반환값:
    List[List[int, str]]: 문서 ID를 키로 사용하고 결합된 텍스트 인덱스를 값으로 사용하는 딕셔너리
"""
processed_index = []
for key, value in tqdm(index.items()):
    assert(type(value["aliases"]) != list)
    aliases_text = " ".join(value["aliases"].split(","))
    text_index = (aliases_text + " " +  value.get("canonical_name", "")).strip()
    if "definition" in value:
        text_index += " " + value["definition"]
    processed_index.append([value["concept_id"], text_index])
return processed_index

mesh_data = read_jsonl_file("mesh_2020.jsonl")
process_mesh_kb(mesh_data)
mesh_data_kb = {x["concept_id"]:x for x in mesh_data}
mesh_data_dict = process_index({x["concept_id"]:x for x in mesh_data})
retriever = BM25Retriever(mesh_data_dict)



mistral_rag_answers = {10:[], 30:[], 50:[]}

for k in [10,30,50]:
    for item in tqdm(test_set_subsample):
        relevant_mesh_ids = retriever.query(item["title"] + " " + item["abstract"], top_n = k)
        relevant_contexts = [mesh_data_kb[x] for x in relevant_mesh_ids]
        rag_prompt = build_rag_prompt(SYSTEM_RAG_PROMPT, item, relevant_contexts)
        input_ids = tokenizer.apply_chat_template(rag_prompt, tokenize=True, return_tensors = "pt").cuda()
        outputs = model.generate(input_ids = input_ids, max_new_tokens=200, do_sample=False)
        gen_text = tokenizer.batch_decode(outputs.detach().cpu().numpy()[:, input_ids.shape[1]:], skip_special_tokens=True)[0]
        mistral_rag_answers[k].append(parse_answer(gen_text.strip()))



entity_scores_at_k = {}
mesh_scores_at_k = {}

for key, value in mistral_rag_answers.items():
    entity_scores = [calculate_entity_metrics(gt["annotations"],pred) for gt, pred in zip(test_set_subsample, value)]
    macro_precision_entity = sum([x[0] for x in entity_scores]) / len(entity_scores)
    macro_recall_entity = sum([x[1] for x in entity_scores]) / len(entity_scores)
    macro_f1_entity = sum([x[2] for x in entity_scores]) / len(entity_scores)
    entity_scores_at_k[key] = {"macro-precision": macro_precision_entity, "macro-recall": macro_recall_entity, "macro-f1": macro_f1_entity}

    mesh_scores = [calculate_mesh_metrics(gt["annotations"],pred) for gt, pred in zip(test_set_subsample, value)]
    macro_precision_mesh = sum([x[0] for x in mesh_scores]) / len(mesh_scores)
    macro_recall_mesh = sum([x[1] for x in mesh_scores]) / len(mesh_scores)
    macro_f1_mesh = sum([x[2] for x in mesh_scores]) / len(mesh_scores)
    mesh_scores_at_k[key] = {"macro-precision": macro_precision_mesh, "macro-recall": macro_recall_mesh, "macro-f1": macro_f1_mesh}

일반적으로 RAG 설정은 원래의 제로샷 설정과 비교하여 전반적인 MeSH 식별 과정을 개선합니다. 하지만 모델에 제공되는 문서 수의 영향은 무엇일까요? 모델에 제공된 검색된 ID 수에 따라 점수를 그래프로 표시합니다.


<div class="content-ad"></div>

플롯을 조사하는 동안 흥미로운 추이를 관찰했습니다. 엔티티 추출을 위해 검색된 문서 수가 증가함에 따라, 마크로 정밀도가 급격히 증가하여 약 50%보다 약간 높은 점수에 도달했습니다. 이는 모델의 제로샷 엔티티 추출 성능보다 거의 10% 더 높습니다. 그러나 마크로 리콜에 미치는 영향은 과제에 따라 다릅니다. 엔티티 추출의 경우에는 변하지 않지만, 엔티티 링킹에서는 개선됩니다. 전반적으로, 모델에 제공된 문서 수를 늘림으로써 MeSH 식별 설정에서 모든 메트릭을 크게 개선하지만, 엔티티 추출 설정에서는 혼합된 이익을 얻습니다.

이 실험에서 고려해야 할 중요한 제한 사항은 상류 검색기의 성능입니다. 검색된 문서가 관련이 없는 경우, 실제 응답이 모델에 제공된 지식에 없으므로 LLM의 성능에 영향을 줄 수 있습니다.

![이미지](/assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_4.png)

이를 조사하기 위해 우리는 검색된 MeSH ID 중 실제 MeSH ID의 평균 %를 각 입력 텍스트 당 계산했습니다. 우리의 연구 결과는 BM-25 검색기가 각 입력 데이터 포인트당 평균적으로 관련 MeSH ID의 약 12.6%에서 17.7% 정도만 검색하는 것으로 나타났습니다. 따라서 검색기의 선택과 검색 방법은 RAG 설정에 대한 중요한 성능 병목 현상이며, 더 나은 성능을 위해 최적화될 수 있습니다.

<div class="content-ad"></div>

# 제로 샷 Entity 추출과 LLM 및 외부 KB 링커

지금까지 LLM이 제로 샷 엔티티 링커로 어떻게 수행되는지 그리고 RAG가 얼마나 성능을 향상시킬 수 있는지 살펴보았습니다. RAG는 제로 샷 설정과 비교하여 성능을 향상시킴으로써 성능을 개선합니다. 그러나 이 방법에는 한계가 있습니다.

RAG 설정에서 LLM을 사용할 때, 우리는 지금까지 지식 구성 요소 (KB + retriever)를 모델의 상류에 유지해 왔습니다. RAG 설정에서 지식을 검색하는 것은 거칠기 때문에 전체 생체 의학 텍스트를 사용하여 검색기에 쿼리를 보내는 것으로 다음했습니다. 이로 인해 검색된 결과의 다양성이 일정 수준 보장됩니다. 왜냐하면 검색된 결과는 텍스트의 다른 엔티티에 해당할 확률이 높기 때문입니다. 그러나 결과가 정확하지는 않을 수 있습니다. 처음에는 문제처럼 보이지 않을 수 있지만, RAG 설정에서 모델에 더 많은 관련 결과를 제공하여 어느 정도 완화할 수 있습니다. 그러나 이에는 두 가지 단점이 있습니다:

- LLM은 일반적으로 텍스트 처리를 위한 컨텍스트 길이 상한선이 있습니다. LLM의 컨텍스트 길이는 LLM이 새로운 텍스트를 생성하기 전에 고려할 수 있는 토큰의 최대 수(프롬프트의 토큰 수)를 대략적으로 나타냅니다. 이것은 LLM에 제공할 수 있는 지식의 양을 제한할 수 있습니다.
- 우리가 긴 컨텍스트 길이를 처리할 수 있는 LLM을 가정해 봅시다. 이제 모델에 더 많은 컨텍스트를 검색하여 추가할 수 있습니다. 좋아요! 그러나 더 긴 컨텍스트 길이가 LLM의 RAG 능력을 향상시킨다고는 한정되지 않습니다[13]. 더 많은 결과를 검색하여 LLM에 더 많은 관련 지식을 전달하더라도, 이것이 LLM이 정확하게 올바른 답을 추출할 것을 보장하지는 않습니다.

<div class="content-ad"></div>

이는 처음에 설명한 엔티티 링킹의 전통적인 파이프라인으로 돌아가게 됩니다. 이 설정에서 지식 구성 요소는 모델보다 하류에 유지되며, 엔티티 추출 후 외부 검색기에 해당 MeSH ID를 얻기 위해 엔티티가 제공됩니다. 좋은 엔티티 추출기가 있다면 보다 정확한 MeSH ID를 검색할 수 있습니다.

이전에 완전한 제로샷 설정에서 관찰한 바와 같이, LLM은 MeSH ID를 예측하는 데에는 능숙하지 못했지만, 엔티티 추출 성능은 상당히 양호했습니다. 이제 Mistral 모델을 사용하여 엔티티를 추출하고 이를 외부 검색기에 제공하여 MeSH ID를 가져오게 됩니다.

![image](https://example.com/assets/img/2024-07-10-BuildingaBiomedicalEntityLinkerwithLLMs_5.png)

여기서 검색을 위해 KB 링커로 BM-25 검색기를 다시 사용합니다. 그러나 이곳에서 변경한 작은 점은 ID를 정규 이름과 별칭을 연결하여 색인화하는 것입니다. 이전 제로샷 설정에서 추출한 엔티티들을 이 실험에 재사용합니다. 이제 이 설정이 얼마나 잘 수행되는지 평가해 봅시다.

<div class="content-ad"></div>


엔티티 메쉬 데이터 딕셔너리 = [[x["concept_id"], " ".join(x["aliases"].split(",")) + " " + x["canonical_name"]] for x in mesh_data]
엔티티 리트리버 = BM25Retriever(엔티티 메쉬 데이터 딕셔너리)

파싱된 퓨 샷 엔티티 = [[y["text"] for y in x] for x in 미스트랄_퓨_샷_답변]
검색된_답변 = []

for 아이템 in tqdm(파싱된 퓨 샷 엔티티):
    답변_요소 = []
    for 엔티티 in 아이템:
        검색된_메쉬_ID = 엔티티_리트리버.query(엔티티, top_n=1)
        답변_요소.append({"text": 엔티티, "identifier": 검색된_메쉬_ID[0]})
    검색된_답변.append(답변_요소)

메쉬_점수 = [calculate_mesh_metrics(gt["annotations"], pred) for gt, pred in zip(test_set_subsample, 검색된_답변)]
매크로_정밀도_메쉬 = sum([x[0] for x in 메쉬_점수]) / len(metric_scores)
매크로_재현율_메쉬 = sum([x[1] for x in 메쉬_점수]) / len(metric_scores)
매크로_F1_메쉬 = sum([x[2] for x in 메쉬_점수])


이 설정에서의 성능은 모든 메트릭에서 RAG 설정보다 크게 향상됩니다. 최적의 RAG 설정(문서 50개에서 검색)과 비교했을 때, 매크로-정밀도에서 12% 이상, 매크로-재현율에서 20% 이상, 매크로-F1 점수에서 16% 이상의 향상을 달성했습니다. 다시 강조하면, 이는 엔티티 추출의 전통적인 파이프라인과 더 유사합니다. 여기에는 엔티티 추출 및 링킹이 별도 구성요소로 있는 형태입니다.

# LLM과 외부 KB 링커를 사용한 엔티티 추출의 세밀한 조정

지금까지 우리는 LLM을 더 큰 파이프라인 내에서 엔티티 추출기로 사용함으로써 최상의 성능을 얻었습니다. 그러나 엔티티 추출을 제로샷 방식으로 수행했습니다. LLM을 엔티티 추출을 위해 특별히 세밀하게 조정하여 추가적인 성능 향상을 달성할 수 있을까요?


<div class="content-ad"></div>

미세 조정을 위해 BioCreative V 데이터셋의 학습 세트를 활용합니다. 해당 데이터셋은 500개의 데이터포인트로 구성되어 있습니다. 미세 조정에는 LLM을 Q-Lora [14]를 사용하여 4비트로 양자화하고 고정한 후 Low-Rank Adapter를 미세 조정하는 과정이 포함됩니다. 이 방법은 일반적으로 매개변수 및 메모리를 효율적으로 사용할 수 있습니다. Adapter는 원본 LLM에 비해 가중치의 일부만 포함하므로, 전체 LLM을 미세 조정하는 것보다 훨씬 적은 가중치를 미세 조정하게 됩니다. 또한 이를 통해 모델을 단일 GPU에서 미세 조정할 수 있습니다.

이제 미세 조정 구성요소를 구현해 봅시다. 이 부분에서는 Niels Rogge의 Mistral 모델을 Q-Lora로 미세 조정하는 노트북을 참고하여 수정했습니다. 수정 사항은 주로 데이터셋을 올바르게 준비하고 처리하는 것 주변에 있습니다.

```python
from datasets import load_dataset
import json
from tqdm import tqdm
from itertools import chain
from datasets import DatasetDict
from transformers import AutoTokenizer, BitsAndBytesConfig
import torch
from trl import SFTTrainer
from peft import LoraConfig
from transformers import TrainingArguments
from helpers import *


def read_jsonl_file(file_path):
    """
    JSONL (JSON Lines) 파일을 구문 분석하고 사전 목록을 반환합니다.

    Args:
        file_path (str): 읽을 JSONL 파일의 경로입니다.

    Returns:
        dict의 목록: 각 요소가 파일에서 가져온 JSON 개체를 나타내는 사전인 목록입니다.
    """
    jsonl_lines = []
    with open(file_path, 'r', encoding="utf-8") as file:
        for line in file:
            json_object = json.loads(line)
            jsonl_lines.append(json_object)
            
    return jsonl_lines

def convert_to_template(data):
    messages = []
    messages.append({"role": "user", "content": data["question"]})
    messages.append({"role": "assistant", "content": data["answer"]})
    
    return tokenizer.apply_chat_template(messages, tokenize = False)

mesh_dataset = parse_dataset("CDR_TrainingSet.PubTator.txt")
```

이제 토크나이저를 로드하고 적절한 매개변수를 설정합니다.

<div class="content-ad"></div>


model_id = "mistralai/Mistral-7B-Instruct-v0.2"

tokenizer = AutoTokenizer.from_pretrained(model_id)

# set pad_token_id equal to the eos_token_id if not set
tokenizer.pad_token_id = tokenizer.eos_token_id
tokenizer.padding_side = "right"

# Set reasonable default for models without max length
if tokenizer.model_max_length > 100_000:
  tokenizer.model_max_length = 512


이제 데이터셋을 적절하게 준비하고 형식을 맞추겠습니다. 모델에 맞는 프롬프트를 정의하고 예상된 채팅 템플릿으로 데이터셋을 형식화합니다.


prepared_dataset = []
system_prompt = "질문에 사실적이고 명확하게 답해주세요."
entity_prompt = "이 생물 의학 텍스트에 포함된 화학 및 질병 관련 엔티티는 무엇인가요?"
prepared_dataset = []

def prepare_instructions(elem):
    entities = []
    for x in elem["annotations"]:
        if x["text"] not in entities:
            entities.append(x["text"])
            
    return {"question": system_prompt + "\n" + entity_prompt + "\n" + elem["title"] + " " + elem["abstract"], "answer": "The entities are:" + ",".join(entities)}

questions = [prepare_instructions(x) for x in tqdm(mesh_dataset)]
chat_format_questions = [{"text": convert_to_template(x)} for x in tqdm(questions)]

df = pd.DataFrame(chat_format_questions)
train_dataset = Dataset.from_pandas(df)


이제 모델을 세밀 조정하기 위한 적절한 구성을 정의하겠습니다. LLM의 양자화를 위한 구성을 정의합니다:

<div class="content-ad"></div>


quantization_config = BitsAndBytesConfig(
            load_in_4bit=True,
            bnb_4bit_quant_type="nf4",
            bnb_4bit_compute_dtype=torch.bfloat16,
)

device_map = {"": torch.cuda.current_device()} if torch.cuda.is_available() else None

model_kwargs = dict(
    torch_dtype=torch.bfloat16,
    use_cache=False, # set to False as we're going to use gradient checkpointing
    device_map=device_map,
    quantization_config=quantization_config,
)


이제 모델 파인튜닝을 준비했어요:


output_dir = 'entity_finetune'

# based on config
training_args = TrainingArguments(
    bf16=True, # specify bf16=True instead when training on GPUs that support bf16
    do_eval=False,
    # evaluation_strategy="no",
    gradient_accumulation_steps=1,
    gradient_checkpointing=True,
    gradient_checkpointing_kwargs={"use_reentrant": False},
    learning_rate=1.0e-04,
    log_level="info",
    logging_steps=5,
    logging_strategy="steps",
    lr_scheduler_type="cosine",
    max_steps=-1,
    num_train_epochs=5,
    output_dir=output_dir,
    overwrite_output_dir=True,
    per_device_eval_batch_size=1, 
    per_device_train_batch_size=8,
    save_strategy="no",
    save_total_limit=None,
    seed=42,
)

# based on config
peft_config = LoraConfig(
        r=16,
        lora_alpha=16,
        lora_dropout=0.1,
        bias="none",
        task_type="CAUSAL_LM",
        target_modules=["q_proj", "k_proj", "v_proj", "o_proj"],
)

trainer = SFTTrainer(
        model=model_id,
        model_init_kwargs=model_kwargs,
        args=training_args,
        train_dataset=train_dataset,
        eval_dataset=train_dataset,
        dataset_text_field="text",
        tokenizer=tokenizer,
        packing = True,
        peft_config=peft_config,
        max_seq_length=tokenizer.model_max_length,
    )

train_result = trainer.train()
trainer.save_model(output_dir)


이제 파인튜닝 프로세스를 완료했으니, 모델을 활용하여 추론하고 성능 메트릭을 얻어봅시다:

<div class="content-ad"></div>


def parse_entities_from_trained_model(content):
    """
    훈련된 모델의 출력에서 엔티티 목록을 추출합니다.

    매개변수:
    - content (str): 훈련된 모델의 원시 문자열 출력.

    반환값:
    - list of str: 모델 출력에서 추출된 엔티티 목록.
    """
    return content.split("The entities are:")[-1].split(",")

mistral_few_shot_answers = []
for item in tqdm(test_set_subsample):
    few_shot_prompt_messages = build_entity_prompt(item)
    # input_ids = tokenizer.apply_chat_template(few_shot_prompt_messages, tokenize=True, return_tensors = "pt").cuda()
    prompt = tokenizer.apply_chat_template(few_shot_prompt_messages, tokenize=False)
    tensors = tokenizer(prompt, return_tensors="pt")
    input_ids = tensors.input_ids.cuda()
    attention_mask = tensors.attention_mask.cuda()
    outputs = model.generate(input_ids=input_ids, attention_mask=attention_mask, max_new_tokens=200, do_sample=False)
    # https://github.com/huggingface/transformers/issues/17117#issuecomment-1124497554
    gen_text = tokenizer.batch_decode(outputs.detach().cpu().numpy()[:, input_ids.shape[1]:], skip_special_tokens=True)[0]
    mistral_few_shot_answers.append(parse_entities_from_trained_model(gen_text.strip()))



parsed_entities_few_shot = [[y["text"] for y in x] for x in mistral_few_shot_answers]
retrieved_answers = []

for item in tqdm(parsed_entities_few_shot):
    answer_element = []
    for entity in item:
        retrieved_mesh_ids = entity_ranker.query(entity, top_n=1)
        answer_element.append({"identifier": retrieved_mesh_ids[0], "text": entity})
    retrieved_answers.append(answer_element)

entity_scores = [calculate_entity_metrics(gt["annotations"], pred) for gt, pred in zip(test_set_subsample, retrieved_answers)]
macro_precision_entity = sum([x[0] for x in entity_scores]) / len(entity_scores)
macro_recall_entity = sum([x[1] for x in entity_scores]) / len(entity_scores)
macro_f1_entity = sum([x[2] for x in entity_scores]) / len(entity_scores)

mesh_scores = [calculate_mesh_metrics(gt["annotations"], pred) for gt, pred in zip(test_set_subsample, retrieved_answers)]
macro_precision_mesh = sum([x[0] for x in mesh_scores]) / len(mesh_scores)
macro_recall_mesh = sum([x[1] for x in mesh_scores]) / len(mesh_scores)
macro_f1_mesh = sum([x[2] for x in mesh_scores]) / len(mesh_scores)


전 설정은 이전 설정과 완전히 동일합니다. 계속해서 LLM을 엔티티 추출기로 사용하고, 각 엔티티를 MeSH ID에 링크하는 외부 리트리버를 사용합니다. 모델 세부 조정은 엔티티 추출과 링킹 모두에서 중요한 개선을 이끌어냅니다.

영 제로샷 엔티티 추출에 비해 세부 조정은 모든 메트릭을 최대 또는 20% 이상 개선합니다. 비슷하게, 이전 설정과 비교하여 엔티티 링킹도 모든 메트릭에서 약 12-14% 정도 개선됩니다. 작업별 모델이 영 제로샷 설정보다 훨씬 우수한 성능을 발휘할 것으로 예상되는 결과이지만, 이러한 개선 사항을 구체적으로 측정할 수 있다는 점은 좋은 소식입니다!


<div class="content-ad"></div>

# Scispacy 성능 평가

기존 엔터티 링킹을 수행할 수 있는 도구와 이 구현을 비교하면 어떻게 되는 것일까요? Scispacy는 생명의학 및 임상 텍스트 처리를 위한 흔한 작업 도구이며, 엔터티 추출 및 엔터티 링킹 기능을 제공합니다. 특히 Scispacy는 엔터티를 MeSH KB에 링크하는 기능도 제공합니다. 이는 원래 LLM 실험에 사용한 KB 파일입니다. 우리는 Scispacy를 사용하여 시험 세트에서의 성능을 평가하고 LLM 실험과 비교할 것입니다.

우리는 "en_ner_bc5cdr_md" 모델을 Scispacy의 엔터티 추출 모듈로 사용합니다. 이 모델은 BioCreative V 데이터 세트에 특별히 훈련되었습니다. 성능을 평가해 봅시다:

```python
from scispacy.linking import EntityLinker
import spacy, scispacy
import pandas as pd
from helpers import *
from tqdm import tqdm

# MeSH 링커를 설치하는 코드는 https://github.com/allenai/scispacy/issues/355에서 참조했습니다
config = {
    "resolve_abbreviations": True,  
    "linker_name": "mesh", 
    "max_entities_per_mention":1
}

nlp = spacy.load("en_ner_bc5cdr_md")
nlp.add_pipe("scispacy_linker", config=config) 

linker = nlp.get_pipe("scispacy_linker")

def extract_mesh_ids(text):
    mesh_entity_pairs = []
    doc = nlp(text)
    for e in doc.ents:
        if e._.kb_ents:
            cui = e._.kb_ents[0][0]
            mesh_entity_pairs.append({"text": e.text, "identifier": cui})
        else:
            mesh_entity_pairs.append({"text": e.text, "identifier": "None"})
    
    return mesh_entity_pairs
```

<div class="content-ad"></div>

```python
all_mesh_ids = []
for item in tqdm(test_set_subsample):
    text = item["title"] + " " + item["abstract"]
    mesh_ids = extract_mesh_ids(text)
    all_mesh_ids.append(mesh_ids)

entity_scores = [calculate_entity_metrics(gt["annotations"],pred) for gt, pred in zip(test_set_subsample, all_mesh_ids)]
macro_precision_entity = sum([x[0] for x in entity_scores]) / len(entity_scores)
macro_recall_entity = sum([x[1] for x in entity_scores]) / len(entity_scores)
macro_f1_entity = sum([x[2] for x in entity_scores]) / len(entity_scores)

mesh_scores = [calculate_mesh_metrics(gt["annotations"],pred) for gt, pred in zip(test_set_subsample, all_mesh_ids)]
macro_precision_mesh = sum([x[0] for x in mesh_scores]) / len(entity_scores)
macro_recall_mesh = sum([x[1] for x in mesh_scores]) / len(entity_scores)
macro_f1_mesh = sum([x[2] for x in mesh_scores]) / len(entity_scores)
```

친애하는 팔레터, 보시다시피,사이패시는 실제로 튜닝된 LLM보다 모든 측정 항목에서 체어 보나이 10% 정도 뛰어난 성과를 보이고, 엔티티 링킹에서는 14-20%까지도 성과가 더 우수합니다! 생명 과학 엔티티 추출 및 링킹 작업에서 사이패시는 여전히 견고한 도구로 남아 있습니다.

# 결론

우리의 실험을 마친 결과, 이들로부터 얻을 수 잇는 구체적인 교훈은 무엇일까요?


<div class="content-ad"></div>

- 제로샷 엔티티 추출의 장점: Mistral-Instruct는 생명 과학 텍스트용으로 꽤 괜찮은 제로샷 엔티티 추출기입니다. 제로샷 MeSH 엔티티 링킹을 수행하기에는 파라미터식 지식이 부족하지만, 우리는 실험에서 외부 KB 리트리버와 함께 이를 엔티티 추출기로 활용하여 훨씬 더 나은 성능을 얻었습니다.
- RAG의 제로샷 예측에 대한 개선: RAG 설정에서의 LLM은 엔티티 링킹을 위한 순수한 제로샷 접근 방식보다 개선된 성능을 보여줍니다. 그러나 RAG 설정 내의 리트리버 구성 요소는 병목 현상이 될 수 있는데, 우리의 경우 BM-25 리트리버는 데이터 당 관련 ID의 약 12–17%만 검색해냅니다. 이는 더 효과적인 검색 방법이 필요하다는 것을 시사합니다.
- 파이프라인 추출이 최상의 성능 제공: LLM의 엔티티 추출기로서의 능력을 고려할 때, 가장 좋은 성능은 외부 리트리버를 포함하는 더 큰 파이프라인 내에서 이 능력을 활용하여 엔티티를 MeSH 지식 베이스(KB)에 링크하는 것으로 달성됩니다. 이는 엔티티 추출과 KB 링킹이 별도의 모듈로 유지되는 전통적인 설정과 동일합니다.
- 파인튜닝의 혜택: QLora를 사용하여 LLM을 엔티티 추출 작업에 파인튜닝하면 외부 리트리버와 함께 사용될 때 엔티티 추출 및 엔티티 링킹에서 상당한 성능 향상을 가져옵니다.
- Scispacy가 최고의 성능을 발휘합니다: 생명 과학 텍스트 처리에 있어 Scispacy는 우리의 실험에서 모든 LLM 기반 방법보다 뛰어난 엔티티 링킹 작업 성능을 보여줍니다. Scispacy는 강력한 도구로 남아 있으며, 빠른 추론을 위해 좋은 GPU가 필요한 LLM과 비교해 실행에 적은 컴퓨팅 성능을 요구합니다. 이에 비해 Scispacy는 좋은 CPU만 필요합니다.
- 최적화 기회: 엔티티 링킹을 위한 LLM 기반 파이프라인의 현재 구현은 매우 단순하여 개선할 여지가 많습니다. 최적화에서 혜택을 받을 수 있는 몇 가지 영역은 검색 선택과 검색 논리 그 자체입니다. 더 많은 데이터로 LLM을 파인튜닝하는 것도 엔티티 추출 성능을 더욱 향상시킬 수 있습니다.

## 한계점

지금까지 우리의 실험에는 몇 가지 한계가 있습니다.

- 엔티티당 여러 MeSH ID: 우리 데이터셋에서 각 문서의 몇 가지 엔티티는 여러 MeSH ID에 링크될 수 있습니다. 테스트 세트의 총 100 개 문서 중 968 개 엔티티 중 15 건 (1.54%)에서 이런 경우가 발생합니다. Scispacy 평가 및 엔티티 추출 후 외부 KB 링커(BM-25 리트리버)를 사용한 모든 LLM 실험에서는 엔티티당 하나의 MeSH 개념에만 링크합니다. Scispacy는 하나 이상의 MeSH ID를 엔티티당 링킹할 수 있는 기능을 제공하지만, 공정한 비교를 위해 해당 기능을 사용하지 않기로 선택합니다. 더 한 개념에 연결할 수 있는 기능을 확장하는 것도 흥미로운 추가 사항일 것입니다.
- 지식 베이스에 없는 MeSH ID: 시험 데이터셋에는 KB에 포함되지 않은 엔티티를 위한 MeSH ID가 몇 개 있습니다. 구체적으로 64 개 엔티티 (총 6.6%의 경우)가 KB에 없는 MeSH ID를 가지고 있습니다. 이 한계는 리트리버 측면에 있으며, KB를 업데이트함으로써 해결할 수 있습니다.
- MeSH ID가없는 엔티티: 마찬가지로 다른 968 개 중 16 개의 엔티티 (1.65%)는 MeSH ID에 매핑될 수 없습니다. 엔티티 추출 후 외부 KB 링커를 사용하는 모든 LLM 실험에서 현재 엔티티에 MeSH ID가 없는지 여부를 파악하는 기능이 없습니다.

<div class="content-ad"></div>

### 참고 자료

- Bodenreider O. (2004). The Unified Medical Language System (UMLS): integrating biomedical terminology. *Nucleic acids research*, 32(Database issue), D267–D270. [doi.org/10.1093/nar/gkh061](https://doi.org/10.1093/nar/gkh061)

- [SNOMED CT](https://www.nlm.nih.gov/healthit/snomedct/index.html)

- [MeSH (Medical Subject Headings)](https://www.nlm.nih.gov/mesh/meshhome.html)

<div class="content-ad"></div>

[4] Wei CH, Peng Y, Leaman R, Davis AP, Mattingly CJ, Li J, Wiegers TC, Lu Z. BioCreative V 화학 질병 관계(CDR) 과제 개요, 다섯 번째 BioCreative Challenge Evaluation Workshop 논문집, 2015년, p154–166

[5] Li J, Sun Y, Johnson RJ, Sciaky D, Wei CH, Leaman R, Davis AP, Mattingly CJ, Wiegers TC, Lu Z. 생물의학 문헌에서 화학 물질, 질병 및 상호 작용 어노테이션, 다섯 번째 BioCreative Challenge Evaluation Workshop 논문집, 2015년, p173–182

[6] Leaman R, Dogan RI, Lu Z. DNorm: 질병명 정규화를 위한 이항순위 학습, Bioinformatics 29(22):2909–17, 2013

[7] Leaman R, Wei CH, Lu Z. tmChem: 화학 명칭 개체 인식 및 정규화를 위한 고성능 접근 방식. J Cheminform, 7:S3, 2015

<div class="content-ad"></div>

[8] Li, J., Sun, Y., Johnson, R. J., Sciaky, D., Wei, C. H., Leaman, R., Davis, A. P., Mattingly, C. J., Wiegers, T. C., & Lu, Z. (2016). BioCreative V CDR task corpus: a resource for chemical disease relation extraction. Database: the journal of biological databases and curation, 2016, baw068. [Read more](https://doi.org/10.1093/database/baw068)

[9] Jiang, A. Q., Sablayrolles, A., Mensch, A., Bamford, C., Chaplot, D. S., Casas, D. D. L., … & Sayed, W. E. (2023). Mistral 7B. [Read more](https://arxiv.org/abs/2310.06825)

[10] Neumann, M., King, D., Beltagy, I., & Ammar, W. (2019, August). ScispaCy: Fast and Robust Models for Biomedical Natural Language Processing. In Proceedings of the 18th BioNLP Workshop and Shared Task, pp. 319–327.

[11] [Download dataset](https://ai2-s2-scispacy.s3-us-west-2.amazonaws.com/data/kbs/2020-10-09/mesh_2020.jsonl)

<div class="content-ad"></div>

**[12]** Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., … & Kiela, D. (2020). 지식 집약형 NLP 작업을 위한 검색 보강 생성. 신경 정보 처리 시스템의 진보, 33, 9459–9474.

**[13]** Liu, N. F., Lin, K., Hewitt, J., Paranjape, A., Bevilacqua, M., Petroni, F., & Liang, P. (2024). 중간에 길 잃다: 언어 모델이 긴 맥락을 활용하는 방법. 계산언어협회 트랜잭션, 12.

**[14]** Dettmers, T., Pagnoni, A., Holtzman, A., & Zettlemoyer, L. (2024). Qlora: 양자화된 LLMS의 효율적인 파인튜닝. 신경 정보 처리 시스템의 진보, 36.

**[15]** [Download link](https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.4.0/en_ner_bc5cdr_md-0.4.0.tar.gz)