---
title: "생성형 AI 애플리케이션을 프로덕션 환경에 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-ProductionizingGenerativeAIApplications_0.png"
date: 2024-07-13 03:30
ogImage: 
  url: /assets/img/2024-07-13-ProductionizingGenerativeAIApplications_0.png
tag: Tech
originalTitle: "Productionizing Generative AI Applications"
link: "https://medium.com/towards-artificial-intelligence/productionizing-generative-ai-applications-3fe310d95e9b"
isUpdated: true
---





지난 일년 동안 고객을 대상으로 한 GenAI 애플리케이션을 구축하고 확장해 왔어. 이 블로그 글에서는 Large Language Model (LLM) GenAI 애플리케이션의 속도, 안전성, 그리고 신뢰성을 향상시킬 수 있는 다섯 가지 실용적인 팁과 코드 예제를 모았어.

## 1. 비동기 API로 LLM 앱 속도 70% 향상시키기

이 간단한 트릭은 LLM 앱의 속도를 70% 향상시킬 수 있어! OpenAI API를 사용하는 LLM 앱은 IO 바운드로, 상당한 지연 문제를 야기해. API에 여러 동시 요청을 보내는 것으로 앱의 성능을 크게 향상시킬 수 있어. OpenAI 파이썬 라이브러리는 AsyncOpenAI를 통해 고 동시성을 네이티브로 지원해.

<div class="content-ad"></div>

Async 애플리케이션은 이전 사용자 입력에 대한 응답을 기다리지 않고 새로운 사용자 입력을 병렬로 처리할 수 있습니다. 이러한 작업 중첩은 휴식 시간을 크게 줄여 전체 성능을 최적화합니다. 이는 동기 앱과 대조적으로, 각 작업이 이전 작업이 완료된 후에만 시작할 수 있는 동기 앱과 대조됩니다. 아래 그림에서 IO 바운드 애플리케이션에 대한 Async의 이점을 직관적으로 이해할 수 있습니다.

![ProductionizingGenerativeAIApplications_1](/assets/img/2024-07-13-ProductionizingGenerativeAIApplications_1.png)

아래 실행한 간단한 테스트에서 AsyncOpenAI를 사용하면 처리량이 놀랍게도 70% 증가했습니다! 이 속도 향상은 특히 포함 생성 및 CI/CD 테스트와 같은 일괄 처리 워크플로에 중요합니다.

LLM 앱이 대규모 동시 요청 집합으로 확장하는 문제가 발생하면 Async를 사용하는 것이 좋습니다. 그러나 실제 응용 프로그램에서 Async를 사용하면 분당 토큰 및 분당 요청 속도 제한에 도달할 수 있습니다. 따라서 이러한 문제를 처리하기 위해 견고하고 종합적인 전략이 필요합니다. 다행히 OpenAI는 공식적인 Python 스크립트를 게시하여 요청률 제한 내에서 API 요청을 효과적으로 병렬 처리할 수 있습니다:

<div class="content-ad"></div>

🔗 OpenAI의 공식 Python 스크립트를 사용한 API 요청 병렬화: [OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/3f8d3f34054526173c0c9cd110d21d90fe993c3f/examples/api_request_parallel_processor.py)

```python
import asyncio
import os

import openai
from tqdm import tqdm

with open("questions.txt", "r") as file:
    questions = file.readlines()
questions = [question.strip() for question in questions]

# 일반 (동기) OpenAI
client = openai.OpenAI(api_key=os.environ["OPENAI_API_KEY"])

def sync_main(question: str) -> None:
    response = client.chat.completions.create(
        model="gpt-3.5-turbo-1106", messages=[{"role": "user", "content": question}]
    )
    return response.choices[0].message.content

def sync_run_loop(questions):
    return {idx: sync_main(question) for idx, question in enumerate(tqdm(questions))}

sync_answers = sync_run_loop(questions[:10])  # 8.417s

# 비동기 OpenAI
client = openai.AsyncOpenAI(api_key=os.environ["OPENAI_API_KEY"])

async def async_main(question: str) -> None:
    response = await client.chat.completions.create(
        model="gpt-3.5-turbo-1106", messages=[{"role": "user", "content": question}]
    )
    return response.choices[0].message.content

async def async_run_loop(questions):
    tasks = [async_main(question) for question in tqdm(questions)]
    answers = await asyncio.gather(*tasks)
    return {idx: answer for idx, answer in enumerate(answers)}

async_answers = asyncio.run(async_run_loop(questions[:10]))  # 2.459s. 70% faster!!!
```

# 2. MLFlow를 사용한 LLM 응답 평가:

LLM 애플리케이션은 확률적 성격 때문에 테스트하기가 까다롭습니다. 그러나 생성된 답변의 신뢰성을 보장하기 위해 고객을 대상으로 한 애플리케이션에 대한 테스트는 중요합니다. 그렇다면 LLM 앱을 효과적으로 어떻게 테스트할까요? MLFlow의 LLM Evaluate가 등장하면서, MLOps가 LLMOps로 나아가는 발판을 마련했습니다.

<div class="content-ad"></div>

MLFlow Evaluate의 주요 기능:

- 사용 편의성: 이제 배포 후 LLM 봇이 직면할 수 있는 실제 질문이 담긴 팬더 데이터프레임에서 실행할 수 있습니다.
- 레이블 없는 표준 메트릭: 독성 및 가독성과 같은 메트릭을 사용하여 레이블이 지정되지 않은 데이터로 봇의 성능을 측정할 수 있습니다! 그리고 RAG 시스템의 충실성 및 관련성 같은 메트릭을 통해 독립 LLM이 당신의 LLM 성능을 평가하는 혁신적인 방법도 포함되어 있습니다! 자세한 프롬프트에 대한 링크도 참고하세요.
- 사용자 정의 메트릭: 비즈니스 요구 사항에 맞게 사용자 정의 메트릭을 정의하여 평가 과정을 맞춤화할 수 있습니다.
- MLFlow UI 내 편리한 액세스!

LLM 평가를 위한 내 권장사항:

- OpenAI GPT4를 LLM 심사위원으로 활용하세요.
- 테스트 데이터세트 생성: GPT-4에 봇을 설명하고 내외 영역의 포괄적인 질문 세트를 생성해 달라고 요청해보세요!
- 정적 데이터세트 평가: 여러 메트릭을 실행하는 경우, 응답을 한 번 생성하고 팬더 데이터프레임에 저장한 후 정적 데이터 평가 기능을 사용하여 모든 메트릭을 계산할 수 있습니다. 이렇게 하면 API 비용을 줄일 수 있습니다—자세한 내용 확인해보세요.
- 환각 양 측정: 저는 신뢰성 메트릭을 좋아합니다. 왜냐하면 RAG에서 검색기가 제공한 문맥과 사실적으로 일치하는 생성된 출력의 얼마나 많은 부분인지를 나타내 줍니다!
- CI/CD: 이러한 테스트를 CI/CD 파이프라인에서 자동으로 실행하여 매 코드와 프롬프트 변경이 무엇인가를 망가뜨리지 않도록 항상 확인하세요.
- 가드레일: 몇 가지 빠른 테스트는 답변을 제공하기 전 사용자에게 대답하려는 모든 API 호출에 실행할 수 있습니다. 더 느린 테스트는 CI/CD에 남겨두세요.

<div class="content-ad"></div>

🔗 MLFlow LLM Evaluate: [링크](https://mlflow.org/docs/latest/llms/llm-evaluate/index.html)

```python
#! pip install openai 'mlflow[genai]' evaluate 'transformers[torch]' textstat
import mlflow
from mlflow.metrics import genai
import pandas as pd
import os

os.environ["OPENAI_API_KEY"] = "..."

# 질문, 답변 및 소스 문서가 포함된 데이터프레임
eval_df = ...

# | 질문               | 소스 문서                            | 생성된 답변      |
# |:-------------------|:------------------------------------|:----------------|
# | 'MLflow란 무엇인가?' | ['MLFlow는 ...', '...', '...']     | '...'           |
# | 'Spark란 무엇인가?'  | ['Spark는 ...', '...', '...']      | '...'           |

# MLFlow Evaluate
with mlflow.start_run() as run:
    results = mlflow.evaluate(
        data=eval_df,
        model="gpt-4",
        model_type="question-answering",
        evaluators="default",
        predictions="generated_answers",
        extra_metrics=[
            genai.faithfulness(),
            genai.relevance()
        ],
        evaluator_config={
            "col_mapping": {
                "inputs": "questions",
                "context": "source_documents",
            }
        },
    )

results.tables["eval_results_table"].head()
```

![](2024-07-13-ProductionizingGenerativeAIApplications_2.png)

![](2024-07-13-ProductionizingGenerativeAIApplications_3.png)

<div class="content-ad"></div>

# 3. 강사와 함께 하는 출력 구문 분석

LLM의 확률적인 성격은 마이크로서비스의 API 계약 강제 적용에 중대한 도전을 제기한다. LLM의 출력이 API 계약과 일치하도록 보장하는 두 가지 방법이 있습니다:

- 정확한 프롬프트 작성: 기대되는 응답 형식으로 정교하게 디자인된 프롬프트를 작성하고, LLM이 당신의 바램을 존중할 것을 바라십시오 🙏. 온도를 0으로 설정하고 LLM에게 자신을 수정하도록 요청하는 재시도 로직을 구현하여 신뢰성을 향상시킬 수 있습니다. 그러나 이 접근 방식도 보장할 수는 없습니다.
- OpenAI API 기능 활용: 함수 호출 및 JSON 모드 기능은 파싱 가능한 JSON을 더 신뢰할 수 있게 제공합니다. 그러나 응답 형식과 재시도 로직의 신중한 설계가 여전히 필요하며 환각된 필드를 피하기 위해 노력해야 합니다!

오늘은 세 번째로, 아마도 더 견고한 옵션이 제안됩니다! LLM 응용 프로그램에서 Pydantic과 Instructor의 힘을 활용하여 4단계 안에 API 계약을 강제하는 방법을 소개합니다:

<div class="content-ad"></div>

- 설치: pip install instructor을 사용해주세요.
- OpenAI 클라이언트에 Instructor 패치: 이로써 OpenAI 클라이언트에 새로운 response_model argument가 도입됩니다.
- LLM 응답을 위한 Pydantic 모델: 필드와 유효성 함수를 가질 수 있습니다!
- 실행: patched OpenAI 클라이언트를 일반적으로 사용할 수 있습니다. Pydantic 모델을 response_model argument에 전달하면 됩니다. LLM이 처음에 실패할 경우, max_retries=2로 설정하여 출력을 수정하도록 요청할 수 있습니다!

Instructor는 깔끔하고 가독성이 좋으며 최소한의 추상화를 갖춘 코드로 Pydantic를 통합하여 OpenAI의 기능 호출과 JSON 모드 능력을 향상시킵니다. 이 방식은 프롬프트 엔지니어링을 간소화하고 기본 재시도 로직을 통합하여 여러 파서의 귀찮음 없이 응답이 원하는 스키마에 맞게 되도록 보장합니다!

🔗Instructor [3천🌟]: [여기](https://github.com/jxnl/instructor)

```python
#! pip install instructor
import os

import instructor
import openai
from pydantic import BaseModel, Field, field_validator

# OpenAI 클라이언트를 instructor 라이브러리를 사용하도록 패치합니다
openai_client = instructor.patch(openai.OpenAI(api_key=os.getenv("OPENAI_API_KEY")))

system_message = (
    "당신은 영화 출연 목록 봇입니다. 제공된 배우의 출연 작품 목록을 생성하세요."
)


# 사용자 정의 응답 스키마 정의
class ActorFilmography(BaseModel):
    # 원하는 필드를 출력에 추가합니다
    name: str = Field(description="배우의 이름")
    film_names: list[str] = Field(description="출연한 영화 목록")

    # 사용자 정의 유효성 함수를 추가합니다
    @field_validator("name")
    def name_upper_case(cls, name):
        if not name.isupper():
            raise ValueError("이름은 대문자여야 합니다")
        return name


# 일반적으로 OpenAI API에 요청을 보냅니다
response = openai_client.chat.completions.create(
    model="gpt-3.5-turbo-1106",
    response_model=ActorFilmography, # 사용자 정의 응답 스키마
    max_retries=2, # 응답 스키마 유효성 검사 실패 시 재시도
    messages=[
        {"role": "system", "content": system_message},
        {"role": "user", "content": "배우 이름: Brad Pitt"},
    ],
)

print("응답:")
print(response.model_dump())
```

<div class="content-ad"></div>

![Presidio](/assets/img/2024-07-13-ProductionizingGenerativeAIApplications_4.png)

# 4. 개인 식별 정보 (PII) 삭제하기: Presidio 사용법

대규모 언어 모델 (LLM)과 같은 AI 엔티티와는 다르게 인간이랑은 말하지 않는 것을 왜 PII(개인 식별 정보)를 공유해야 할까요? 사용자 앞에서 사용되는 GenAI 봇에서는 사용자들이 무심코 민감한 PII를 입력할 수 있어 중요한 위험을 야기할 수 있습니다. 사용자 입력의 잠재적 PII를 관리하기 위한 세 가지 전략이 있습니다:

- 오픈 소스 배포: 인프라 내에서 오픈 소스 LLM을 활용하여 사용자 입력이 제 3자 API를 회피하고 네트워크 내에서 안전하게 보호되도록 합니다.
- 클라우드 관리 LLM 서비스: Microsoft Azure OpenAI, Amazon Web Services (AWS) Bedrock 또는 Google Cloud VertexAI Studio를 가상 사설 클라우드 내에서 활용합니다. 클라우드 서비스 제공업체 사용 시 데이터 보호 법률을 준수하기 위해 언제나 법률 팀과 상의하십시오.
- 입력 살식화: 사용자의 입력을 사전 처리하여 LLM API로 전송하기 전에 PII를 감지하고 삭제합니다. Microsoft의 Presidio 라이브러리는 전형적인 PII 범주를 효과적으로 처리하며 GenAI 백엔드에 독립적인 마이크로서비스로 통합할 수 있어 좋은 선택입니다.

<div class="content-ad"></div>

첫 번째 두 가지 옵션은 기업에 이상적이지만, 가능한 경우 개인 식별 정보를 수정하는 것은 항상 신중한 사이버 보안 조치입니다. 유명한 속담이 있죠. 안전이 최우선이죠!

🔗Microsoft Presidio [2.7K🌟]: [링크](https://github.com/microsoft/presidio/)

🔗Langchain Presidio 통합: [링크](https://python.langchain.com/docs/guides/privacy/presidio_data_anonymization/)

```python
#! pip install presidio-analyzer presidio-anonymizer

from presidio_analyzer import AnalyzerEngine
from presidio_anonymizer import AnonymizerEngine
from presidio_anonymizer.entities import OperatorConfig

# 엔진 초기화
analyzer = AnalyzerEngine()
anonymizer = AnonymizerEngine()

original_text = """My name is Marie Stephen Leo, 
call me at (123) 456-7890 or email me at some_fake.address23@gmail.com"""

# PII 데이터를 식별하기 위해 텍스트 분석
analyzer_results = analyzer.analyze(
    text=original_text,
    entities=["PERSON", "PHONE_NUMBER", "EMAIL_ADDRESS", "LOCATION"],
    language="en",
)

# 식별된 PII 데이터 삭제
pii_sanitized_text = anonymizer.anonymize(
    text=original_text,
    analyzer_results=analyzer_results,
    operators={
        "PERSON": OperatorConfig("replace", {"new_value": "[REDACTED NAME]"}),
        "PHONE_NUMBER": OperatorConfig("replace", {"new_value": "[REDACTED PHONE NUMBER]"}),
        "EMAIL_ADDRESS": OperatorConfig("replace", {"new_value": "[REDACTED EMAIL]"}),
        "LOCATION": OperatorConfig("replace", {"new_value": "[REDACTED PLACE]"}),
    },
)

print(pii_sanitized_text.text)
# My name is [REDACTED NAME], call me at [REDACTED PHONE NUMBER] or email me at [REDACTED EMAIL]
# 평균 실행 시간: 12.2 ms ± 1.06 ms (평균 ± 표준 편차, 7개 실행 중 100번 반복)
```

<div class="content-ad"></div>

# 5. Prompt Injection Attacks 방어하기

Prompt injection은 악의적인 사용자들이 응답을 조작하여 기대와 다른 행동을 유발하는 고객을 대면하는 GenAI 봇에 상당한 도전을 제공합니다. 여기에는 채팅봇이 신뢰성 있고 안전한 상태를 유지하기 위해 이 위험을 완화하는 몇 가지 전략이 나열되어 있습니다.

- Prompt 공학: 봇의 목적을 "당신은 만화 책 고문이며 만화 책과 관련된 질문에만 답해야 합니다"와 같이 가능한 한 명확하게 명시하세요. 또한 "사용자의 질문: `'질문'`"과 같이 질문을 다른 지시사항과 문맥 예제에서 구분하기 위한 테두리 문자열을 추가해야 합니다. 이러한 조치로 60-70%의 prompt injections을 해결할 수 있을 것입니다.

- 최신 모델 버전: 이전 버전보다 시스템 메시지를 따르는 데 더 능숙한 gpt-3.5-turbo의 -1106 버전을 사용하세요.

- 전통적인 텍스트 EDA: 잠재적인 사용자 질문을 분석하여 캐릭터를 벗어난 쿼리를 감지하고 관리하는 휴리스틱 필터링 규칙을 생성하세요. 일부 아이디어: 연속적인 사용자 질문이 높은 의미론적 유사성이 있는가? 해당 질문이 통계적 이상치인가요?

- RAG (Retrieval Augmented Generation): RAG는 질문과 가장 유사한 문서를 기반으로 답변을 생성합니다. 악의적인 질문들은 아마 데이터베이스에 관련 문서가 없을 것입니다.

- 쿼리 변환: 사용자의 쿼리를 다시 작성함으로써 악의적인 prompt를 LLM에 도달하기 전에 제거할 수 있는 추가적인 보호층을 만들 수 있습니다.

- Prompt Injection 분류 모델: Deepset의 오픈 소스 데이터셋 및 Hugging Face의 사전 훈련된 모델과 같은 리소스를 사용하면 악의적인 prompt를 식별하는 데 뛰어난 도구를 얻을 수 있습니다.

- 질문 없는 가드레일: 사용자의 질문을 완전히 제거하고 생성된 답변이 1번에서 정의한 범위와 일치하는지만 확인하여 남아 있는 위험을 제거할 수 있습니다.

실제로, 옵션 1-4는 모든 앱에게 간단하고 당연한 선택입니다. 한편, 옵션 5-7은 별도의 복잡성과 잠재적인 지연을 야기할 수 있습니다. 따라서 앱에 추가하기 전에 이러한 이점을 신중하게 고려해야 합니다.

<div class="content-ad"></div>

마침내, 모든 대처 방법이 기술적일 필요는 없습니다. 제품 관점에서 보면, 봇 사용을 위해 로그인을 필요로하고 악의적인 행동을 하는 사용자를 금지함으로써 삶을 더 쉽게 만들 수 있습니다. 또한 법률 팀이 적절한 이용 약관을 작성하도록 할 필요가 있습니다.

# 결론

![Conclusion](/assets/img/2024-07-13-ProductionizingGenerativeAIApplications_6.png)

<div class="content-ad"></div>

마지막으로, 다음 GenAI 애플리케이션을 구축하는 데 도움이 되는 다섯 가지 실용적인 팁을 적용하는 것이 중요합니다. LLM을 사용하여 빠른 처리를 위해 비동기 API 호출을 하고, MLFlow를 사용하여 LLM 응답을 평가하며, Instructor를 사용하여 출력을 구문 분석함으로써 LLM 애플리케이션의 성능, 신뢰성 및 API 계약 준수를 크게 향상시킬 수 있습니다. 더 나아가, Presidio를 활용하여 PII 데이터를 필터링하고, Prompt Injection 보호를 위한 여러 전략을 구현함으로써 고객 애플리케이션에 안전한 앱을 만들 수 있습니다.