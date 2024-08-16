---
title: "Medprompt 이해와 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingandImplementingMedprompt_0.png"
date: 2024-07-07 13:24
ogImage: 
  url: /assets/img/2024-07-07-UnderstandingandImplementingMedprompt_0.png
tag: Tech
originalTitle: "Understanding and Implementing Medprompt"
link: "https://medium.com/towards-data-science/understanding-and-implementing-medprompt-77bbd2777c91"
isUpdated: true
---





## 프롬프트 프레임워크 뒷면에 숨겨진 세부사항 파헤치기

![image](/assets/img/2024-07-07-UnderstandingandImplementingMedprompt_0.png)

첫 번째 블로그 글에서는 프롬프팅과 그 중요성에 대해 다뤘습니다. 프롬프팅은 대형 언어 모델 (LLM)의 맥락에서 높은 품질의 결과를 얻는 데 중요한 역할을 합니다. 이는 모델의 응답을 지도하고 해당 과제와 관련이 있는지 확인함으로써 보장됩니다. 이러한 기초 위에 구축되며, LLM을 사용하여 사용 사례를 해결하려고 할 때 종종 두 가지 중요한 질문이 제기됩니다. 프롬프팅만으로 얼마나 성능을 끌어올릴 수 있으며, 대신 모델을 세밀하게 조정하는 것이 더 효과적일 수도 있는 시점은 언제인가요?

프롬프팅을 활용하는 디자인 결정을 내릴 때 여러 가지 고려 사항이 들어갑니다. 적은 양의 입력을 사용한 프롬프팅 및 Chain-of-Thought (CoT) [2] 프롬프팅과 같은 기술은 대부분의 작업에 대해 LLM의 성능을 향상시키는 데 도움이 될 수 있습니다. 검색 증가 생성 (RAG) 파이프라인은 세부 조정 없이 새로운 도메인에 적응하고 생성된 결과물을 구립하는 데 있어 더 많은 통제력을 제공하면서 환각을 줄일 수 있어 LLM의 성능을 더욱 향상시킬 수 있습니다. 전반적으로, 우리는 세부 조정에 명시적으로 의존하지 않고도 LLM의 성능을 개선하기 위한 다양한 도구들이 있습니다.

<div class="content-ad"></div>

파인튜닝은 레이블 데이터 요구사항과 LLM의 학습 및 배포에 따른 비용과 같은 고유한 도전과 복잡성을 동반합니다. 특정 상황에서 LLM의 환각을 증가시킬 수도 있습니다. 모든 이를 종합해보면, 파인튜닝에 의존하기 전에 프롬프트를 통해 우리의 작업을 위해 LLM 성능을 최적화하려는 가치가 크다는 것을 알 수 있습니다.

그럼 어떻게 이를 수행할까요? 이 글에서는 Microsoft이 소개한 고급 프롬프팅 전략 중 하나인 Medprompt를 탐색해봅니다. Medprompt는 퓨샷 프롬프팅, CoT 프롬프팅 및 RAG에서 원리를 결합하여 GPT-4의 성능을 향상시키는 데 사용되며, 도메인 특정 파인튜닝 없이도 의료 분야에서 우수한 성능을 발휘합니다.

## 목차:

- Medprompt 설명
- Medprompt의 구성 요소
- Medprompt 구현
- 성능 평가
- 결론
- 참고문헌

<div class="content-ad"></div>

# 메드프롬프트 설명

의료 분야를 중심으로 LLM(대형 언어 모델)은 다양한 분야에서 놀라운 능력을 보여주고 있습니다. 지난해 Google은 MedPaLM 및 MedPaLM-2를 소개했는데, 이들 LLM은 의료 다지선다형 질문 응답(MCQA) 데이터셋에서 우수한 성과를 보이며, 열린형 의료 질문 응답에서는 임상가를 경쟁적으로 제쳐놓기도 합니다. 이러한 모델들은 업무 지식 세밀 조정과 임상가가 작성한 Chain-of-Thought 템플릿을 사용하여 의료 분야에 특화되었으며, 그 성능을 크게 향상시켰습니다.

마이크로소프트의 “일반적인 기반 모델이 전문 목적 튜닝을 능가할 수 있을까? 의학 사례 연구”[1] 논문은 이러한 의문을 제기하고 있습니다.

본 연구의 일환으로, 이 논문은 모델의 성능을 향상시키는 혁신적인 프롬프팅 전략인 메드프롬프트를 소개합니다. 이 방법은 MedPaLM-2와 같은 전문 모델을 능가하는 데에 성공했을 뿐 아니라 모델 성능을 향상시킵니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-07-UnderstandingandImplementingMedprompt_1.png)

안녕하세요! 오늘은 Medprompt의 구성 요소에 대해 알아보렵니다.

# Medprompt의 구성 요소

Medprompt는 few-shot prompting, CoT prompting 및 RAG의 원칙을 결합한 것입니다. 구체적으로 이 파이프라인에는 3가지 구성 요소가 있습니다.

<div class="content-ad"></div>

## 다이내믹 퓨-샷 선정

퓨-샷 프롬프팅은 LLM을 위한 문맥으로 예시 입력-출력 쌍을 활용하는 것을 의미합니다. 이러한 퓨-샷 샘플이 정적인 경우, 새로운 입력에 대한 가장 관련성이 높은 예시일 수 없는 단점이 있습니다. Medprompt의 첫 번째 구성 요소인 다이내믹 퓨-샷 선정은 이를 극복하기 위해 각 새로운 작업 입력에 기반하여 퓨-샷 예시를 선택합니다. 이 방법은 훈련 세트에서 K-Nearest Neighbors (K-NN) 알고리즘을 훈련시킨 다음, 임베딩 공간에서 코사인 유사도를 기반으로 테스트 입력에 가장 유사한 훈련 세트 예시를 검색합니다. 이 전략은 기존의 훈련 데이터 세트를 효율적으로 활용하여 LLM을 위해 관련된 퓨-샷 예시를 검색합니다.

## 셀프-생성된 CoT

논문 [1]에서 언급된 바와 같이, CoT는 기존에 MedPaLM-2에서 의학 전문가들이 작성한 상세한 추론 단계를 포함하는 수동으로 제작된 퓨-샷 예본에 의존했습니다. Medprompt은 두 번째 모듈로 셀프-생성된 CoT를 소개합니다. 여기서 LLM을 사용하여 추론 과정의 상세한 단계별 설명을 생성하며 최종 답변 선택으로 이어집니다. 각 훈련 데이터 포인트에 대해 CoT 추론 단계를 자동으로 생성함으로써 수동으로 제작된 예시가 우회됩니다. GPT-4에 의해 생성된 답변이 실제 답변과 교차 확인되어 올바른 예측만 추론 단계와 함께 유지되고 잘못된 응답은 필터링됩니다.

<div class="content-ad"></div>

## 선택 섞기 앙상블

선택 섞기 앙상블 기술은 Medprompt에서 소개된 세 번째 기술입니다. 이 기술은 모델의 의사결정 과정에 영향을 미칠 수 있는 내재적 편향을 해소하기 위해 고안되었습니다. 특히 다중 선택 상황에서의 위치 편향을 해결하기 위해 설계되었습니다. 응답 선택의 순서가 섞이고, 이 과정이 k 번 반복되어 동일한 질문의 k 가지 변형이 만들어집니다. 추론 중에 각 변형이 사용되어 응답을 생성하고, 모든 변형에 대해 과반수 투표가 이루어져 최종 예측 옵션을 선택합니다.

## 이러한 구성 요소들은 전처리 및 추론 단계에서 어떻게 사용되나요?

이제 Medprompt의 전처리 및 추론 단계를 살펴보겠습니다.

<div class="content-ad"></div>

## 전처리 단계

전처리 파이프라인에서는 훈련 데이터셋에서 각 질문을 가져와 상세한 지침을 포함하여 응답과 관련 추론 단계의 생성을 안내합니다. LLM에 응답과 추론 단계를 생성하도록 프롬프트합니다. 생성된 응답을 얻은 후, 해당 질문에 대한 예측 응답을 실제 정답과 비교하여 정확성을 확인합니다.

예측이 잘못된 경우, 해당 인스턴스를 관련 질문의 데이터베이스에서 제외합니다. 예측이 올바른 경우, 질문을 텍스트 임베딩 모델을 사용하여 임베딩하고, 그리고 질문, 질문 임베딩, 응답, 그리고 추론 체인 (CoT)을 버퍼에 저장합니다. 모든 질문을 처리한 후, 임베딩을 사용하여 훈련용 KNN 모델을 훈련합니다. 이 훈련된 KNN 모델은 RAG 파이프라인에서 우리의 리트리버로 작용하여 임베딩 공간 내의 코사인 유사성을 기반으로 상위 k개의 유사 데이터 포인트를 효율적으로 질의하고 검색할 수 있게 합니다.

추론 파이프라인

<div class="content-ad"></div>

추론 단계에서는 테스트 세트의 각 질문을 먼저 텍스트 임베딩 모델을 사용하여 임베딩합니다. 그런 다음 KNN 모델을 활용하여 가장 유사한 상위 k개의 질문을 식별합니다. 검색된 각 데이터 포인트에 대해 자체 생성된 사상 체인 (CoT) 추론과 예측된 답변에 액세스할 수 있습니다. 이러한 요소들인 질문, CoT 추론 및 답변을 최종 프롬프트를 위해 몇 가지 지표로 포맷합니다.

이제 우리는 선택 셔플 앙상블링을 수행하여 각 테스트 질문의 답변 선택 순서를 섞어 동일한 질문의 여러 변형을 만듭니다. 그런 다음 LLM에게 이러한 변형과 해당하는 몇 가지 지표를 함께 제공하여 각 변형에 대한 추론 단계와 답변을 생성하게 합니다. 마지막으로 모든 변형에서의 예측에 대한 과반 투표를 수행하고 최종 예측을 선택합니다.

# Medprompt 구현

Medprompt를 실행하고 평가하기 위해 MedQA [6] 데이터 세트를 사용합니다. 먼저 jsonl 파일을 구문 분석하는 도우미 함수를 정의합니다.

<div class="content-ad"></div>


def write_jsonl_file(file_path, dict_list):
    """
    Write a list of dictionaries to a JSON Lines file.

    Args:
    - file_path (str): The path to the file where the data will be written.
    - dict_list (list): A list of dictionaries to write to the file.
    """
    with open(file_path, 'w') as file:
        for dictionary in dict_list:
            json_line = json.dumps(dictionary)
            file.write(json_line + '\n')

def read_jsonl_file(file_path):
    """
    Parses a JSONL (JSON Lines) file and returns a list of dictionaries.

    Args:
        file_path (str): The path to the JSONL file to be read.

    Returns:
        list of dict: A list where each element is a dictionary representing
            a JSON object from the file.
    """
    jsonl_lines = []
    with open(file_path, 'r', encoding="utf-8") as file:
        for line in file:
            json_object = json.loads(line)
            jsonl_lines.append(json_object)
            
    return jsonl_lines


## 자체 생성된 CoT 구현

우리의 구현에서는 MedQA의 훈련 세트를 활용합니다. Zero-shot CoT 프롬프트를 구현하고 모든 훈련 질문을 처리합니다. 우리는 구현에서 GPT-4o를 사용합니다. 각 질문에 대해 CoT와 해당 답변을 생성합니다. Medprompt 논문에서 제공된 템플릿을 기반으로하는 프롬프트를 정의합니다. 


system_prompt = """당신은 전문적인 의료 전문가입니다. 여러 선택지가 있는 의료 질문이 제공됩니다.
질문을 신중히 고민하고 마지막 답변을 선택하기 전에 추론 과정을 단계별로 설명하는 것이 목표입니다.
다음과 같은 형식으로 각 질문과 답변을 제공합니다:

Input:
## 질문: {question}
{answer_choices}

Output:
## 답변
(모델 생성된 추론 과정 설명)
따라서, 답은 [최종 모델 답변 (예: A, B, C, D)]"""


<div class="content-ad"></div>


```python
def build_few_shot_prompt(system_prompt, question, examples, include_cot=True):
    """
    제로샷 프롬프트를 작성합니다.

    Args:
        system_prompt (str): LLM을 위한 작업 지침
        content (dict): 쿼리를 생성하기 위해 필요한 형식으로 포맷된 콘텐츠

    Returns:
        list of dict: 작업을 정의하는 시스템 메시지와 입력 질문이 포함된 메시지의 목록
    """
    messages = [{"role": "시스템", "content": system_prompt}]
    
    for elem in examples:
        messages.append({"role": "사용자", "content": create_query(elem)})
        if include_cot:
            messages.append({"role": "어시스턴트", "content": format_answer(elem["cot"], elem["answer_idx"])})        
        else:           
            answer_string = f"""## Answer\n따라서, 답은 {elem["answer_idx"]}입니다"""
            messages.append({"role": "어시스턴트", "content": answer_string})
            
    messages.append({"role": "사용자", "content": create_query(question)})
    return messages

def get_response(messages, model_name, temperature = 0.0, max_tokens = 10):
    """
    채팅-완성 API를 통해 모델의 응답/답변을 얻습니다.

    Args:
        messages (list of dict): API에 제공된 작성된 메시지
        model_name (str): API를 통해 액세스할 모델의 이름
        temperature (float): 출력의 무작위성을 제어하는 0과 1 사이의 값.
        온도 값이 0이면 모델이 가장 가능성 높은 토큰을 선택하도록 만들어 출력을 결정론적으로 만듭니다.
        max_tokens (int): 모델이 생성해야 하는 최대 토큰 수

    Returns:
        str: 모델로부터의 응답 메시지 내용
    """
    response = client.chat.completions.create(
        model=model_name,
        messages=messages,
        temperature=temperature,
        max_tokens=max_tokens
    )
    return response.choices[0].message.content
```

LLM 응답에서 추론과 최종 답변 옵션을 파싱하기 위한 도우미 함수를 정의합니다.

```python
def matches_ans_option(s):
    """
    이 문자열이 '따라서, 답은 [A-Z]'의 구체적인 패턴으로 시작하는지 확인합니다.
    
    Args:
    s (str): 확인할 문자열

    Returns:
    bool: 문자열이 패턴과 일치하면 True, 아니면 False
    """
    return bool(re.match(r'^따라서, 답은 [A-Z]', s))

def extract_ans_option(s):
    """
    문자열 시작부분에서 답변 옵션(단일 대문자)을 추출합니다.
    
    Args:
    s (str): 답변 패턴을 포함하는 문자열

    Returns:
    str or None: 패턴이 발견되면 캡처 된 답변 옵션, 그렇지 않으면 None
    """
    match = re.search(r'^따라서, 답은 ([A-Z])', s)
    if match:
        return match.group(1)  # 캡처 된 알파벳 반환
    return None

def matches_answer_start(s):
    """
    이 문자열이 마크다운 헤더 '## Answer'로 시작하는지 확인합니다.
    
    Args:
    s (str): 확인할 문자열

    Returns:
    bool: 문자열이 '## Answer'로 시작하면 True, 아니면 False
    """
    return s.startswith("## Answer")

def validate_response(s):
    """
    응답이 '## Answer'로 시작하고 답변 패턴으로 끝나는 다중 줄 문자열 응답을 유효성 검사합니다.
    
    Args:
    s (str): 유효성을 검사 할 다중 줄 문자열 응답

    Returns:
    bool: 응답이 유효하면 True, 그렇지 않으면 False
    """
    file_content = s.split("\n")
    
    return matches_ans_option(file_content[-1]) and matches_answer_start(s)

def parse_answer(response):
    """
    '## Answer'로 시작하는 응답을 구문 분석하여 CoT 추론과 답안 선택을 추출합니다.
    
    Args:
    response (str): 답변 및 추론이 포함 된 다중 줄 문자열 응답

    Returns:
    tuple: 추출 된 CoT 추론 및 답안 선택을 포함하는 튜플
    """
    split_response = response.split("\n")
    assert split_response[0] == "## Answer"
    cot_reasoning = "\n".join(split_response[1:-1]).strip()
    ans_choice = extract_ans_option(split_response[-1])
    return cot_reasoning, ans_choice
```

이제 MedQA의 교육 자료 질문을 처리합니다. 모든 질문에 대한 CoT 응답 및 답변을 가져 와서 폴더에 저장합니다.


<div class="content-ad"></div>

```js
train_data = read_jsonl_file("data/phrases_no_exclude_train.jsonl")

cot_responses = []
# os.mkdir("cot_responses")
existing_files = os.listdir("cot_responses/")

for idx, item in enumerate(tqdm(train_data)):
    if str(idx) + ".txt" in existing_files:
        continue
    
    prompt = build_zero_shot_prompt(system_prompt, item)
    try:
        response = get_response(prompt, model_name="gpt-4o", max_tokens=500)
        cot_responses.append(response)
        with open(os.path.join("cot_responses", str(idx) + ".txt"), "w", encoding="utf-8") as f:
            f.write(response)           
    except Exception as e :
        print(str(e))
        cot_responses.append("")
```

이제 생성된 모든 답변에서 유효하며 시스템 프롬프트에서 정의된 예측 형식을 준수하는지 확인하기 위해 반복합니다. 필요한 형식에 맞지 않는 응답은 버립니다. 그 후 각 질문에 대한 예측된 답변을 참값과 비교하여 일치하는 경우에만 질문을 보존합니다.

```js
questions_dict = []
ctr = 0
for idx, question in enumerate(tqdm(train_data)):
    file =  open(os.path.join("cot_responses/", str(idx) + ".txt"), encoding="utf-8").read()
    if not validate_response(file):
        continue
    
    cot, pred_ans = parse_answer(file)
    
    dict_elem = {}
    dict_elem["idx"] = idx
    dict_elem["question"] = question["question"]
    dict_elem["answer"] = question["answer"]
    dict_elem["options"] = question["options"]
    dict_elem["cot"] = cot
    dict_elem["pred_ans"] = pred_ans
    questions_dict.append(dict_elem)        

filtered_questions_dict = []
for item in tqdm(questions_dict):
    pred_ans = item["options"][item["pred_ans"]]
    if pred_ans == item["answer"]:
        filtered_questions_dict.append(item)
```

## KNN 모델 구현하기


<div class="content-ad"></div>

학습 세트를 처리하고 모든 질문에 대한 CoT 응답을 얻었습니다. 이제 OpenAI의 text-embedding-ada-002를 사용하여 모든 질문을 임베딩합니다.

```python
def get_embedding(text, model="text-embedding-ada-002"):
    return client.embeddings.create(input=[text], model=model).data[0].embedding

for item in tqdm(filtered_questions_dict):
    item["embedding"] = get_embedding(item["question"])
    inv_options_map = {v:k for k,v in item["options"].items()}
    item["answer_idx"] = inv_options_map[item["answer"]]
```

이제 이러한 질문 임베딩을 사용하여 KNN 모델을 훈련시킵니다. 이는 추론 시 리트리버 역할을 하며, 테스트 세트의 질문과 가장 유사한 학습 세트에서 유사한 데이터포인트를 검색하는 데 도움이 됩니다.

```python
import numpy as np
from sklearn.neighbors import NearestNeighbors

embeddings = np.array([d["embedding"] for d in filtered_questions_dict])
indices = list(range(len(filtered_questions_dict))

knn = NearestNeighbors(n_neighbors=5, algorithm='auto', metric='cosine').fit(embeddings)
```

<div class="content-ad"></div>

## Dynamic Few-Shot 및 Choice Shuffling Ensemble Logic 구현

이제 추론을 수행할 수 있습니다. 우리는 MedQA 테스트 세트에서 500개의 질문을 하위 샘플링하여 평가에 활용합니다. 각 질문마다 KNN 모듈을 사용해 학습 세트에서 가장 유사한 5개의 질문을 검색하며, 이에 해당하는 CoT 추론 단계와 예측된 답변을 함께 가져옵니다. 이러한 예시들을 활용하여 few-shot 프롬프트를 작성합니다.

각 질문마다 옵션의 순서를 5번 섞어 다양한 변형을 생성합니다. 그리고 생성된 few-shot 프롬프트를 사용하여 각각의 옵션을 섞은 변형에 대한 예측된 답변을 가져옵니다.

```python
def shuffle_option_labels(answer_options):
    """
    질문의 옵션들을 섞습니다.
    
    Parameters:
    answer_options (dict): 옵션을 포함한 사전 형태의 데이터.

    Returns:
    dict: 옵션을 섞은 새로운 사전.
    """
    options = list(answer_options.values())
    random.shuffle(options)
    labels = [chr(i) for i in range(ord('A'), ord('A') + len(options))]
    shuffled_options_dict = {label: option for label, option in zip(labels, options)}
    
    return shuffled_options_dict
```

<div class="content-ad"></div>

```python
test_samples = read_jsonl_file("final_processed_test_set_responses_medprompt.jsonl")

for question in tqdm(test_samples, colour ="green"):
    question_variants = []
    prompt_variants = []
    cot_responses = []
    question_embedding = get_embedding(question["question"])
    distances, top_k_indices = knn.kneighbors([question_embedding], n_neighbors=5)
    top_k_dicts = [filtered_questions_dict[i] for i in top_k_indices[0]]
    question["outputs"] = []
    
    for idx in range(5):
        question_copy = question.copy()
        shuffled_options = shuffle_option_labels(question["options"])
        inv_map = {v:k for k,v in shuffled_options.items()}
        
        question_copy["options"] = shuffled_options
        question_copy["answer_idx"] = inv_map[question_copy["answer"]]
        question_variants.append(question_copy)
        prompt = build_few_shot_prompt(system_prompt,  question_copy, top_k_dicts)
        prompt_variants.append(prompt)
    
    for prompt in tqdm(prompt_variants):
        response = get_response(prompt, model_name="gpt-4o", max_tokens=500)
        cot_responses.append(response)
    
    for question_sample, answer in zip(question_variants, cot_responses):
        if validate_response(answer):
            cot, pred_ans = parse_answer(answer)
            
        else:
            cot = ""
            pred_ans = ""
                
        question["outputs"].append({"question": question_sample["question"], "options": question_sample["options"], "cot": cot, "pred_ans": question_sample["options"].get(pred_ans, "")})
```

We are now examining the outcomes of Medprompt on the test dataset. For each question, we have created five predictions using ensemble logic. We determine the final prediction for each question based on the most frequent prediction. During evaluation, we encountered two exceptional scenarios:

- Two different answer options have been predicted twice each without a clear winner.
- An error occurred in generating the response, resulting in the absence of a predicted answer option.

In both cases, the question is considered incorrectly answered by the LLM.


<div class="content-ad"></div>

```python
def find_mode_string_list(string_list):
    """
    가장 빈번하게 발생하는 문자열을 찾습니다.

    매개변수:
    string_list (str 목록): 문자열 목록입니다.
    반환값:
    str 목록 또는 None: 입력 목록에서 가장 빈번한 문자열을 포함하는 목록입니다.
                         입력 목록이 비어있는 경우 None을 반환합니다.
    """    
    if not string_list:
        return None  

    string_counts = Counter(string_list)
    max_freq = max(string_counts.values())
    mode_strings = [string for string, count in string_counts.items() if count == max_freq]
    return mode_strings

ctr = 0 
for item in test_samples:
    pred_ans = [x["pred_ans"] for x in item["outputs"]]
    freq_ans = find_mode_string_list(pred_ans)
    
    if len(freq_ans) > 1:
        final_prediction = ""
    else:
        final_prediction = freq_ans[0]
        
    if final_prediction == item["answer"]:
        ctr +=1

print(ctr / len(test_samples))
```

## 성능 평가

저희는 Medprompt과 GPT-4o의 정확성을 MedQA 테스트 하위 집합에서 평가합니다. 추가로, 제로샷 프로팅, 랜덤 퓨-샷 프로팅, 그리고 랜덤 퓨-샷 및 CoT 프로팅의 성능을 벤치마킹합니다.

![UnderstandingandImplementingMedprompt_2](/assets/img/2024-07-07-UnderstandingandImplementingMedprompt_2.png)


<div class="content-ad"></div>

우리는 Medprompt와 Random Few-Shot CoT prompting이 Zero 및 Few-Shot prompting baselines보다 우월하다는 것을 알 수 있습니다. 그러나 놀랍게도, Random Few-Shot CoT가 우리의 Medprompt 성능을 능가한다는 점을 발견했습니다. 이는 몇 가지 이유 때문일 수 있습니다:

- 원본 Medprompt 논문에서는 GPT-4의 성능을 기준으로 삼았습니다. 우리는 GPT-4o가 다양한 텍스트 벤치마크에서 GPT-4T와 GPT-4보다 뚜렷하게 우월함을 발견했습니다(https://openai.com/index/hello-gpt-4o/), 이는 Medprompt가 GPT-4o와 같은 강력한 모델에는 더 적은 영향을 미칠 수 있다는 것을 시사할 수 있습니다.
- 우리는 MedQA에서 추출한 500개의 질문을 평가로 제한했습니다. Medprompt 논문에서는 다른 의료 MCQA 데이터셋 및 MedQA의 전체 버전을 평가합니다. 데이터셋의 전체 버전에서 GPT-4o를 평가하면 전반적인 성능에 대한 더 나은 그림을 제공할 수 있을 것입니다.

# 결론

Medprompt는 도메인 특화 일반 LLM을 세밀한 튜닝 없이 만들기 위한 복잡한 프롬프트 파이프라인을 위한 흥미로운 프레임워크입니다. 또한 각 상황에 맞게 프롬프팅과 세밀한 튜닝 사이의 선택 고려 사항을 강조합니다. LLM 성능을 향상시키기 위해 프롬프트가 얼마나 발전시킬 수 있는지 탐구하는 것은 중요합니다. 왜냐하면 이는 세밀한 튜닝에 대한 비용 효율적인 대안을 제공하기 때문입니다.

<div class="content-ad"></div>

# 레퍼런스:

[1] Nori, H., Lee, Y. T., Zhang, S., Carignan, D., Edgar, R., Fusi, N., … & Horvitz, E. (2023). Can generalist foundation models outcompete special-purpose tuning? case study in medicine. arXiv preprint arXiv:2311.16452. [링크](https://arxiv.org/abs/2311.16452)

[2] Wei, J., Wang, X., Schuurmans, D., Bosma, M., Xia, F., Chi, E., … & Zhou, D. (2022). Chain-of-thought prompting elicits reasoning in large language models. Advances in Neural Information Processing Systems, 35, 24824–24837. [링크](https://openreview.net/pdf?id=_VjQlMeSB_J)

[3] Gekhman, Z., Yona, G., Aharoni, R., Eyal, M., Feder, A., Reichart, R., & Herzig, J. (2024). Does Fine-Tuning LLMs on New Knowledge Encourage Hallucinations?. arXiv preprint arXiv:2405.05904. [링크](https://arxiv.org/abs/2405.05904)

<div class="content-ad"></div>

[4] 싱할, K., 아지지, S., 투, T., 마다비, S. S., 웨이, J., 정, H. W., … & 나타라잔, V. (2023). 대형 언어 모델은 임상 지식을 인코딩합니다. 네이처, 620(7972), 172–180. ([링크](https://www.nature.com/articles/s41586-023-06291-2))

[5] 싱할, K., 투, T., 곳트바이스, J., 세이어스, R., 울지인, E., 후, L., … & 나타라잔, V. (2023). 대형 언어 모델을 활용한 전문가 수준의 의료 질문 응답으로 나아가기. arXiv 사전 인쇄 arXiv:2305.09617. ([링크](https://arxiv.org/abs/2305.09617))

[6] 진, D., 팬, E., 우파톨, N., 웽, W. H., 팡, H., & 졸로비츠, P. (2021). 이 환자가 어떤 질병을 가지고 있습니까? 의료 시험에서의 대규모 오픈 도메인 질문 응답 데이터셋. 응용 과학, 11(14), 6421. ([링크](https://arxiv.org/abs/2009.13081)) (원본 데이터셋은 MIT 라이선스 하에 공개됨)