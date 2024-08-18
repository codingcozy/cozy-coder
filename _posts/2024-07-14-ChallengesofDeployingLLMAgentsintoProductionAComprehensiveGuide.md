---
title: "LLM 에이전트를 프로덕션에 배포하는 도전 과제 종합 가이드"
description: ""
coverImage: "/assets/img/2024-07-14-ChallengesofDeployingLLMAgentsintoProductionAComprehensiveGuide_0.png"
date: 2024-07-14 01:40
ogImage:
  url: /assets/img/2024-07-14-ChallengesofDeployingLLMAgentsintoProductionAComprehensiveGuide_0.png
tag: Tech
originalTitle: "Challenges of Deploying LLM Agents into Production: A Comprehensive Guide"
link: "https://medium.com/gitconnected/challenges-of-deploying-llm-agents-into-production-a-comprehensive-guide-f750c1a2d810"
isUpdated: true
---

대규모 언어 모델(LLM) 에이전트를 운용 환경으로 배치하는 것은 복잡하고 다면적입니다. 이 글에서는 제작 단계에서 자주 마주치는 여덟 가지 문제를 공유하려 합니다. 이 프로세스에 대한 프레임워크에 대한 많은 질문을 받습니다. 기본적으로 프레임워크 중립적이 되려고 노력하지만, 어떤 문제들은 특정 프레임워크에 더 관련이 있습니다.

여행처럼 이것도 많은 장애물과 예상치 못한 도전이 동반됩니다. 이 문제점을 다루는 주요 전략과 함께 적용 가능한 코드 예제가 제시됩니다.

![image](/assets/img/2024-07-14-ChallengesofDeployingLLMAgentsintoProductionAComprehensiveGuide_0.png)

# 1. 신뢰성: 신뢰의 기초

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

상상해보세요. 지도가 매번 살펴볼 때마다 바뀌는 여행을 떠나는 것을요. 이것이 신뢰할 수 없는 LLM 에이전트가 기업들에게 느껴지는 느낌입니다. 이러한 에이전트의 신뢰성을 보장하는 것은 매우 중요합니다. 왜냐하면 기업은 일관된, 뛰어난 성능을 기대하기 때문이죠. 많은 기업이 "Five Nines" (99.999%)의 업타임을 목표로 하지만, LLM 에이전트는 일관성에 어려움을 겪어 자주 인간의 감독이 필요한 결과물을 만들어 내곤 해요.

대부분의 기업은 높은 신뢰성을 추구하며 종종 99%의 업타임에 만족하지만, LLM 에이전트는 일반적으로 약 60-70%의 신뢰성만을 달성합니다. 이러한 불규칙성은 지속적인 인간 감독이 필요하기 때문에 자동화의 취지에 어긋나죠. 더 높은 신뢰성 달성을 위해서는 탄탄한 테스트와 검증 프레임워크가 필요합니다.

## 도전 과제 이해하기

성능의 변동성: LLM들은 예측할 수 없고, 때때로 출력물의 확률적 성격 때문에 일관성이 없는 결과물을 생성할 때가 많습니다. 이는 높은 정확도와 일관성이 필요한 작업에 문제가 될 수 있습니다. 예를 들어, 한 순간에는 고객 지원 문의를 올바르게 처리할 수 있지만, 그 다음 순간에는 관련 없는 응답을 제공할 수도 있습니다.

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

컨텍스트 관리: 장기간 상호작용이나 여러 세션 동안의 컨텍스트 유지는 어려운 일입니다. LLM은 대화 흐름을 잃어버려 관련 없거나 잘못된 응답을 할 수 있습니다. 이 문제는 고객 서비스 응용 프로그램에서 특히 중요하며, 에이전트는 이전 상호작용을 기억해야 할 수 있습니다.

도구 통합: 많은 LLM 에이전트는 데이터베이스 쿼리나 웹 검색과 같은 특정 작업을 수행하기 위해 외부 도구에 의존합니다. 효과적인 통합 및 이러한 도구 사이의 원활한 전환은 신뢰할 수 있는 성능에 중요합니다. 에이전트는 CRM 시스템에서 데이터를 가져와 처리하고 컨텍스트를 놓치지 않고 쿼리에 응답해야 할 수 있습니다. 이와 관련된 내용은 SuperAnnotate의 이 기사에서 강조되었습니다.

## 신뢰성 향상 전략

견고한 테스트: 실제 시나리오를 모방하는 포괄적인 테스트 체제를 구현합니다. 이를 통해 배포 전에 잠재적인 문제를 식별하고 해결할 수 있습니다. 회귀 테스트와 같은 기술은 새로운 업데이트가 새로운 오류를 도입하지 않도록 보장할 수 있습니다.

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

해당 게시물에서 설명한 대로 (Analytics Vidhya), 최종 응답 생성에 사용된 검색된 컨텍스트를 기반으로 "허상 확인자"를 만들 수 있습니다. 이것은 '예' 또는 '아니요'로 답변을 제공합니다.

```js
### 허상 평가자

# 데이터 모델
class GradeHallucinations(BaseModel):
    """생성된 답변에 존재하는 허상을 위한 이진 점수."""

    binary_score: str = Field(
        description="답변이 사실에 근거한 것인지 판단, '예' 또는 '아니요'"
    )


# 전문
전문 = """LLM 생성이 검색된 사실 집합에 기초하여 지지되는지를 평가하는 등급자입니다. \n
'예' 또는 '아니요'로 이진 점수를 부여하세요. '예'는 답변이 사실에 기초하거나 지지된다는 것을 의미합니다."""

# LLM 함수 호출
llm = ChatCohere(model="command-r", temperature=0)
structured_llm_grader = llm.with_structured_output(
    GradeHallucinations, preamble=전문
)

# 프롬프트
hallucination_prompt = ChatPromptTemplate.from_messages(
    [
        # ("system", system),
        ("human", "사실 집합: \n\n {documents} \n\n LLM 생성: {generation}"),
    ]
)

hallucination_grader = hallucination_prompt | structured_llm_grader
hallucination_grader.invoke({"documents": docs, "generation": generation})
```

우리는 또한 LLM 모델을 통해 질문과 관련이 있는 생성된 답변을 확인하는 답변 평가자를 통해 점검할 수 있습니다.

```js
# 데이터 모델
class GradeAnswer(BaseModel):
    """질문에 대한 답변이 적절한지를 평가하는 이진 점수."""

    binary_score: str = Field(
        description="답변이 질문에 적합한지 판단, '예' 또는 '아니요'"
    )


# 전문
전문 = """답변이 질문을 다루거나 해결하는지를 평가하는 등급자입니다. \n
'예' 또는 '아니요'로 이진 점수를 부여하세요. '예'는 답변이 질문을 해결한다는 것을 의미합니다."""

# LLM 함수 호출
llm = ChatCohere(model="command-r", temperature=0)
structured_llm_grader = llm.with_structured_output(GradeAnswer, preamble=전문)

# 프롬프트
answer_prompt = ChatPromptTemplate.from_messages(
    [
        ("human", "사용자 질문: \n\n {question} \n\n LLM 생성: {generation}"),
    ]
)

answer_grader = answer_prompt | structured_llm_grader
answer_grader.invoke({"question": question, "generation": generation})
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

향상된 피드백 메커니즘: 에이전트의 성능을 계속 모니터링하고 실시간 데이터를 기반으로 조정하는 피드백 루프를 통합하세요. ReAct 및 Reflexion과 같은 방법은 LLMs가 생각, 행동 및 관찰 주기를 반복함으로써 개선할 수 있게 도와줍니다. ReAct 및 Reflexion에 대해 더 많은 정보를 얻으려면 에이전트 계획에 대한 이전 기사를 참조하십시오.

성능 모니터링 도구: 에이전트의 성능을 모니터링하고 개선이 필요한 영역에 대한 통찰을 제공하는 도구를 활용하세요. 실시간 대시보드 및 경보 시스템을 활용하여 발생하는 신뢰성 문제를 신속하게 대응할 수 있습니다. 예를 들어, 데이터를 효율적으로 관리하기 위해 벡터 저장소를 활용하면 관련 정보를 빠르고 정확하게 검색하여 에이전트가 다양한 시나리오에서 신뢰성을 유지할 수 있도록 도와줍니다.

## 2. 과도한 루프 처리: 순환을 깨는 법

이 도전에서는 미로에 갇혔다는 듯이 계속 반복되는 루프에 갇힌 것처럼 느낄 수 있습니다. 에이전트도 유사한 루프에 갇히면서 만족스러운 결과를 반복적으로 달성하지 못할 수 있습니다. 최대 재시도 횟수를 설정하거나 타임아웃 메커니즘을 사용하는 등의 메커니즘을 구현함으로써 이 끝없는 루프를 방지하고 리소스를 보전할 수 있습니다.

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

에이전트들은 종종 실패한 도구나 LLM(Large Language Model)이 작업을 반복하도록 결정하여 루프에 갇히는 경우가 있습니다. 이는 특히 비싼 모델의 경우 비효율적이고 비용이 소모될 수 있습니다. 이를 완화하는 전략으로는 재시도 횟수를 제한하고 타임아웃 메커니즘을 구현하는 것이 있습니다.

과도한 루프 처리 방법:

재시도 제한과 타임아웃: 에이전트가 루프에 갇히는 것을 방지하는 주요 방법 중 하나는 최대 재시도 횟수를 설정하고 타임아웃 메커니즘을 도입하는 것입니다. 이렇게 하면 에이전트가 일정 횟수의 시도 내에 만족할만한 결과를 달성하지 못하면 시도를 중단하고 계산 리소스를 보존합니다.

작업 실행 관리: CrewAI가 제공하는 고급 작업 실행 관리 전략과 같은 전략을 활용합니다. max_iter(최대 반복 횟수) 및 max_execution_time(최대 실행 시간)과 같은 속성을 구성하여 에이전트가 작업을 중지하기 전에 작업에 얼마나 오랫동안 작업할지 제한할 수 있습니다.

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

고급 모니터링 및 디버깅: 에이전트옵스와 같은 도구는 포괄적인 모니터링 및 디버깅 기능을 제공합니다. 에이전트옵스는 세션 재생을 추적하고 비용 및 토큰 사용량을 모니터링하며, 무한 반복 루프로 이어지는 재귀적 사고 패턴을 식별할 수 있습니다. 이를 통해 개발자들은 이러한 루프의 루트 원인을 더 효과적으로 파악하고 해결할 수 있습니다.

코드 예시: 최대 재시도 한도를 설정한 다시 시도 메커니즘 구현

파이썬을 사용하여 최대 재시도 한도를 설정하고 타임아웃을 활용하여 무분별한 루프를 처리하는 다시 시도 메커니즘을 구현한 예시입니다:

```python
import time

def call_agent_with_retry(agent, input_data, max_retries=5, timeout=2):
    attempts = 0
    while attempts < max_retries:
        try:
            result = agent(input_data)
            return result
        except Exception as e:
            attempts += 1
            time.sleep(timeout)
            if attempts >= max_retries:
                raise e

# 사용 예시
result = call_agent_with_retry(my_agent_function, "input_data")
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

크루 AI에서는 특정 속성을 설정하여 에이전트들이 루프를 효과적으로 처리할 수 있습니다:

```python
from crewai import Agent
agent = Agent(
 role='Data Analyst',
 goal='Extract actionable insights',
 max_iter=10, # 무한 루프 방지를 위해 최대 10번의 반복으로 제한
 max_execution_time=60, # 실행 시간을 60초로 제한
 verbose=True
)
```

이러한 전략을 적용하고 AgentOps와 CrewAI의 구성 옵션과 같은 고급 도구를 활용하면 에이전트가 과도한 루프에 갇히는 가능성을 크게 줄일 수 있어서 LLM 배포에서 효율성과 신뢰성을 향상시킬 수 있습니다.

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

# 3. 도구 맞춤 설정: 완벽한 나침반 만들기

일반적인 도구는 당신의 응용 프로그램의 특정 요구를 충족시키지 못할 수 있습니다. 모험가가 특정 지형에 맞게 장비를 맞춤 설정하듯이, 이러한 도구를 맞춤 설정함으로써 성능을 크게 향상시킬 수 있습니다. 특정 작업에 맞게 설계된 데이터 스크래핑 도구를 개발하거나 특정 작업에 맞게 설계된 정확한 API를 통합함으로써 에이전트의 효율성을 향상시킬 수 있습니다.

LangChain과 같은 프레임워크의 도구들은 종종 너무 일반적입니다. 특정 입력 및 출력을 처리할 수 있는 도구를 맞춤 설정하는 것이 중요합니다. 예를 들어, 사용자 정의 도구는 LLMs를 위한 데이터 추출, 조작 및 준비를 관리할 수 있습니다.

## 코드 예제:

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

뷰티풀수프를 사용하여 사용자 정의 웹 스크레이핑 도구를 만들어보세요:

```python
import requests
from bs4 import BeautifulSoup

def custom_scraper(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')

    # 사용자 정의 스크레이핑 로직
    data = {
        "title": soup.find('title').text,
        "links": [a['href'] for a in soup.find_all('a', href=True)]
    }
    return data

# 사용 예시
scraped_data = custom_scraper('https://example.com')
```

## 4. 자가검사 메커니즘: 항해사의 눈

자가검사 메커니즘을 구현하면 생성된 출력물이 정확하고 유용하다는 것을 보장할 수 있습니다. 특히 특정 기준을 충족해야 하는 코드를 생성하거나 데이터를 처리하는 애플리케이션에 대해 중요합니다. 항해사가 매 단계를 두 번 확인하고 정상적으로 진로를 유지하도록 하는 것과 같습니다.

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

에이전트들은 결과물을 검증해야 합니다. 코드 생성의 경우, 이는 단위 테스트를 실행하는 것을 포함할 수 있습니다. 다른 결과물의 경우, 데이터의 유효성이나 정확성을 확인하는 것일 수도 있습니다.

## 코드 예시:

생성된 코드에 대한 자가검사 메커니즘을 'exec'를 사용하여 구현한 예시:

```js
def self check code(code):
    try:
        exec(code, {})
        return True
    except Exception as e:
        return False

# 사용 예시
generated code = """
def example function():
    return "안녕, 세상!"
"""

if self check code(generated code):
    print("코드가 유효합니다")
else:
    print("코드가 유효하지 않습니다")
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

# 5. 해석 가능성: 안내서

해석 가능성은 사용자 신뢰에 중요합니다. 사용자는 왜 에이전트가 특정 결정을 내렸거나 특정 결과물을 생성했는지 이해해야 합니다. 에이전트의 행동에 대한 데이터와 추론에 대한 명확한 설명이나 인용을 제공함으로써 시스템에 대한 신뢰를 증가시킬 수 있습니다.

해석 가능성은 에이전트의 의사 결정 과정을 기록하는 것을 포함합니다. 이는 사용자가 왜 특정 결정이 내려졌는지 이해하도록 도와 시스템에 대한 신뢰를 높일 수 있습니다.

코드 예시:

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

안녕하세요 🌟! 오늘은 LLM의 결정에 대한 설명을 로깅하는 과정을 알아보겠습니다.

```python
import logging

logging.basicConfig(level=logging.INFO)

def explainable_agent(input_data):
    explanation = f"Received input: {input_data}"
    logging.info(explanation)

    # 출력 생성
    output = "생성된 출력"

    explanation += f" | 생성된 출력: {output}"
    logging.info(explanation)

    return output, explanation

# 사용 예시
output, explanation = explainable_agent("샘플 입력")
print("Output:", output)
print("Explanation:", explanation)
```

# 6. 보안 및 규정 준수: 수호자의 방패

LLM을 배포하면 독특한 보안 도전 과제가 발생합니다. 견고한 액세스 제어, 정기적인 보안 감사 및 지속적인 모니터링은 민감한 데이터를 보호하고 규정을 준수하는 데 필수적입니다. 🛡️✨

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

# 7. 대기 시간 및 성능 최적화: 신속한 경로

대기 시간은 실시간 애플리케이션에서 특히 중요한 장애물이 될 수 있습니다. 데이터, 텐서, 파이프라인, 하이브리드 병렬성과 같은 기법을 사용하여 성능을 최적화할 수 있습니다. 또한 양자화와 같은 모델 압축 기술을 활용하면 메모리 요구 사항을 줄이고 효율성을 높일 수 있습니다.

해결책: 의미론적 캐싱

의미론적 캐싱은 비슷한 쿼리에 대한 응답을 저장하고 재사용함으로써 대기 시간을 크게 줄이고 성능을 향상시킬 수 있습니다. 이 접근 방식은 중복 API 호출의 수를 줄이고 계산 오버헤드를 감소시킬 수 있습니다.

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

코드 예시:

GPTCache와 시멘틱 캐싱을 사용하는 방법:

```python
from gptcache import Cache, Config
from gptcache.embedding import Onnx
from gptcache.manager import manager_factory
from gptcache.similarity_evaluation import OnnxModelEvaluation
from gptcache.processor_pre import last_content

# 캐시 초기화
openai_cache = Cache()
encoder = Onnx()
sqlite_faiss_data_manager = manager_factory(
    "sqlite, faiss",
    data_dir="openai_cache",
    scalar_params={"sql_url": "sqlite:///./openai_cache.db", "table_name": "openai_chat"},
    vector_params={"dimension": encoder_dimension, "index_file_path": "./openai_chat_faiss.index"},
)
onnx_evaluation = OnnxModelEvaluation()
cache_config = Config(similarity_threshold=0.75)

openai_cache.init(
    pre_func=last_content,
    embedding=encoder,
    data_manager=sqlite_faiss_data_manager,
    evaluation=onnx_evaluation,
    config=cache_config
)

# 캐시된 응답 사용하는 함수
def get_response_with_cache(question):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": question}],
        cache_obj=openai_cache,
    )
    return response["choices"][0]["message"]["content"]

# 사용 예시
question = "What is GitHub?"
response = get_response_with_cache(question)
print(response)
```

# 8. 비용 관리: 보물 지킴이

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

**솔루션: 비용 절감을 위한 시맨틱 캐싱**

LLM(Large Language Models)를 배포하는 것은 자원 집약적이며 비용이 많이 듭니다. 하드웨어의 사용을 최적화하고 클라우드 컴퓨팅 자원을 현명하게 활용하며 모델 요청의 효율적인 스케줄링과 배칭을 통해 비용을 관리할 수 있습니다.

시맨틱 캐싱은 API 호출 수를 최소화하여 계산 및 대역폭 비용을 절약하여 비용을 줄일 수 있습니다.

더 많은 정보를 원하시면 [이 기사](링크)를 참조해주세요.

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

# 결론

LLM 에이전트를 실제 운영 환경에 배포하는 것은 신뢰성을 다루고, 과도한 루프를 처리하며, 도구를 사용자화하고, 강력한 모니터링 및 피드백 메커니즘을 구현하는 다면에 걸친 도전 과제입니다. 이러한 일반적인 문제를 이해하고 효과적인 전략을 적용함으로써, LLM 배포의 성능과 신뢰성을 향상시킬 수 있습니다. 실제 운영 환경에서 기대되는 높은 기준을 충족시켜 줄 것입니다. AI 및 CrewAI, AgentOps와 같은 도구들이 지속적으로 발전함에 따라, 이러한 도전에 대한 관리가 점점 더 실현 가능해 지며, 더 효율적이고 신뢰할 수 있는 AI 솔루션을 위한 길을 열어 줍니다.
