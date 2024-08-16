---
title: "DSPy 입문 프롬프트 작별, 프로그래밍 환영"
description: ""
coverImage: "/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_0.png"
date: 2024-07-13 02:47
ogImage: 
  url: /assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_0.png
tag: Tech
originalTitle: "Intro to DSPy: Goodbye Prompting, Hello Programming!"
link: "https://medium.com/towards-data-science/intro-to-dspy-goodbye-prompting-hello-programming-4ca1c6ce3eb9"
isUpdated: true
---





![IntrotoDSPyGoodbyePromptingHelloProgramming_0.png](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_0.png)

요즘에는 대형 언어 모델 (LLMs)을 사용하여 애플리케이션을 개발하는 것이 복잡하고 부서지기 쉬울 수 있습니다. 전형적인 파이프라인은 종종 프롬프트를 사용하여 구현되며, LLMs는 프롬프트 방식에 민감하기 때문에 시행착오를 통해 수동으로 만들어집니다. 따라서 LLM이나 데이터와 같은 파이프라인의 요소를 변경할 때는 프롬프트 (또는 세부 조정 단계)에 적응하지 않는 한 성능이 저하될 가능성이 높습니다.

DSPy [1]는 프롬프팅보다 프로그래밍을 우선시하여 언어 모델 (LM) 기반 애플리케이션에서 부서짐 문제를 해결하려는 프레임워크입니다. 요소를 변경할 때 모든 파이프라인을 다시 컴파일하여 작업을 최적화할 수 있게 해줍니다. 이를 통해 수동으로 프롬프트 엔지니어링을 반복하는 대신 특정 작업에 최적화하기 좋습니다.

프레임워크에 대한 논문 [1]은 이미 2023년 10월에 발표되었지만, 저는 요즘에야 그것을 알게 되었습니다. Connor Shorten의 "DSPy 설명"이라는 비디오를 한 편 시청한 후에도 DSPy에 대한 개발자 커뮤니티가 왜 그렇게 흥분하고 있는지 이해할 수 있었습니다!

<div class="content-ad"></div>

## DSPy Framework 개요

이 기사에서는 다음 주제를 다루며 DSPy 프레임워크에 대한 간단한 소개를 제공합니다:

- DSPy가 무엇인가 (DSPy vs. LangChain vs. LlamaIndex 및 DSPy vs. PyTorch에 대한 토론 포함)
- DSPy 프로그래밍 모델: 시그니처, 모듈 및 텔레프롬퍼
- DSPy 컴파일러
- DSPy 예제: Naive RAG 파이프라인

**DSPy가 무엇인가**

DSPy (“Declarative Self-improving Language Programs (in Python)”, 발음은 “디-에스-파이”)은 Stanford NLP 연구원들이 개발한 “foundation models로 프로그래밍”을 강조하는 프레임워크입니다. 이는 프롬프트 조작에서 LM 기반 파이프라인 구축으로 프로그래밍을 이동시킴으로써 LM 기반 응용 프로그램 구축 시의 부서짐 문제를 해결하려고 합니다.

<div class="content-ad"></div>

DSPy 또한 LM 기반 응용 프로그램을보다 체계적으로 구축하기 위한 접근 방식을 제공함으로써 프로그램의 정보 흐름과 각 단계의 매개 변수 (프롬프트 및 LM 가중치)을 분리합니다. DSPy는 프로그램을 가져와서 귀하의 특정 작업에 대해 LM을 프롬프트하거나 세밀 조정하는 방법을 자동으로 최적화합니다.

이를 위해, DSPy는 다음과 같은 개념 세트를 소개합니다:

- 손으로 쓴 프롬프트 및 세밀 조정은 서명으로 대체되고 추상화됩니다.
- Chain of Thought 또는 ReAct와 같은 프롬프팅 기술은 모듈로 추상화되고 대체됩니다.
- 수동 프롬프트 엔지니어링은 옵티마이저 (텔레프롬프터)와 DSPy 컴파일러로 자동화됩니다.

DSPy를 사용하여 LM 기반 응용 프로그램을 구축하는 작업 흐름은 DSPy Intro Notebook에서 논의된 것처럼 아래에 표시됩니다. 이것은 신경망을 훈련하는 작업 흐름을 생각나게 할 것입니다:

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_1.png)

- 데이터셋 수집: 프로그램의 입력과 출력의 몇 가지 예제를 수집하여 파이프라인을 최적화하는 데 사용합니다 (예: 질문과 답변 쌍).
- DSPy 프로그램 작성: 프로그램의 로직을 서명과 모듈을 사용하여 정의하고 구성 요소 간의 정보 흐름을 결정하여 작업을 해결합니다.
- 유효성 검사 로직 정의: 유효성 측정 항목 및 옵티마이저 (텔레프롬프터)를 사용하여 프로그램을 최적화하기 위한 로직을 정의합니다.
- DSPy 프로그램 컴파일: DSPy 컴파일러는 훈련 데이터, 프로그램, 옵티마이저 및 유효성 검사 항목을 고려하여 프로그램을 최적화합니다 (예: 프롬프트 또는 파인튜닝).
- 반복: 데이터, 프로그램 또는 검증을 개선하여 파이프라인의 성능에 만족할 때까지 프로세스를 반복합니다.

다음은 DSPy와 관련된 모든 중요한 링크의 간단한 목록입니다:

- DSPy 논문: DSPy: 선언적 언어 모델 호출을 자가 개선 파이프라인으로 컴파일하기 [1]
- DSPy GitHub: [https://github.com/stanfordnlp/dspy](https://github.com/stanfordnlp/dspy)
- Omar Khattab의 업데이트를 따를 수 있도록 DSPy를 확인하세요.

<div class="content-ad"></div>

## DSPy는 LangChain이나 LlamaIndex와 어떻게 다를까요?

LangChain, LlamaIndex, 그리고 DSPy는 모두 개발자들이 LM을 기반으로 한 애플리케이션을 쉽게 구축할 수 있게 도와주는 프레임워크입니다. LangChain과 LlamaIndex를 사용한 전형적인 파이프라인은 종종 프롬프트 템플릿을 이용하여 구현되는데, 이는 전체 파이프라인이 구성 요소의 변경에 민감하게 만듭니다. 반면, DSPy는 LM을 기반으로 한 파이프라인 구축을 프롬프트 조작에서 멀리하고 프로그래밍에 가깝게 만듭니다.

DSPy의 새롭게 추가된 컴파일러는 LM 기반 애플리케이션에서 부분을 변경할 때 추가적인 프롬프트 엔지니어링이나 세밀한 조정 작업을 필요로 하지 않습니다. 대신, 개발자들은 단순히 프로그램을 다시 컴파일하여 새로 추가된 변경 사항에 최적화된 파이프라인을 얻을 수 있습니다. 따라서 DSPy는 LangChain이나 LlamaIndex보다 적은 노력으로 파이프라인의 성능을 얻는 데 도움을 줍니다.

LangChain과 LlamaIndex가 이미 개발자 커뮤니티에서 널리 채택되어 있지만, DSPy는 새로운 대안으로 동일한 커뮤니티에서 상당한 관심을 불러일으키고 있습니다.

<div class="content-ad"></div>

## DSPy와 PyTorch의 관계

만약 데이터 과학 분야에 경험이 있다면, DSPy를 사용하기 시작할 때 PyTorch와 문법적 유사성을 빨리 알아차릴 것입니다. DSPy 논문의 저자들은 명시적으로 PyTorch를 영감의 근원으로 언급하고 있습니다.

PyTorch와 같이 일반 목적의 레이어가 모든 모델 구조에서 조합될 수 있는 것처럼, DSPy에서도 일반 목적의 모듈이 모든 LM 기반 응용 프로그램에서 조합될 수 있습니다. 또한, DSPy 모듈의 매개변수가 자동으로 최적화되는 DSPy 프로그램을 컴파일하는 것은 PyTorch에서 뉴럴 네트워크를 학습시키는 것과 유사합니다. 즉, 모델 가중치를 옵티마이저를 사용하여 학습시키는 것처럼 DSPy 모듈의 매개변수가 최적화됩니다.

다음 표는 PyTorch와 DSPy 사이의 유사성을 요약한 것입니다:

<div class="content-ad"></div>

![Image](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_2.png)

# DSPy 프로그래밍 모델

이 섹션에서는 DSPy 프로그래밍 모델에서 소개된 다음 세 가지 핵심 개념에 대해 설명합니다:

- 서명: 프롬프팅 및 세밀 조정 추상화
- 모듈: 프롬프팅 기술 추상화
- 텔레프롬프터: 임의의 파이프라인에 대한 자동 프롬프팅

<div class="content-ad"></div>

## 서명: 유도 및 미세 조정의 추상화

DSPy 프로그램에서 LM을 호출할 때마다 자연어 서명이 있어야 합니다. 이 서명은 기존의 손으로 쓴 프롬프트를 대체합니다. 서명은 변환을 어떻게 유도할지가 아닌 변환이 무엇을 하는지를 명시하는 간결한 함수입니다. 예를 들어 "질문과 맥락을 소비하고 답변을 반환한다"와 같습니다.

![image](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_3.png)

서명은 그 최소 형태에서 입력 및 출력 필드들의 튜플입니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_4.png)

Below are some examples of shorthand syntax signatures.

```js
"질문 -> 대답"

"긴 문서 -> 요약"

"문맥, 질문 -> 대답"
```

In most situations, these shorthand syntax signatures are satisfactory. However, for cases requiring more precision, you can define signatures using the following notation. In this scenario, a signature comprises three elements:

<div class="content-ad"></div>

LM이 해결해야 할 하위 작업에 대해 간단히 설명해 드릴게요. 입력 필드 및 출력 필드에 대한 설명도 참고해 주세요.

아래는 서명 컨텍스트, 질문 및 답변에 대한 완전한 표기법이 있어요:


class GenerateAnswer(dspy.Signature):
    """짧은 사실 기반 답변으로 질문에 답변합니다."""
    context = dspy.InputField(desc="관련 사실을 포함할 수 있음")
    question = dspy.InputField()
    answer = dspy.OutputField(desc="보통 1개에서 5개 단어 사이의 답변")


수기로 작성된 프롬프트와는 달리, 서명은 각 서명에 대한 예제를 부트스트래핑하여 자체 향상 및 파이프라인 적응형 프롬프트 또는 파인튜닝으로 컴파일될 수 있어요.

<div class="content-ad"></div>

## 모듈: 프롬프트 기법 추상화

어떤 프롬프트 기법들을 이미 알고 계실 것입니다. "당신의 일은 ..." 또는 "당신은 ..."과 같은 문장을 프롬프트의 시작에 추가하는 것, 연쇄적 사고 ("단계별로 생각해 봅시다") 또는 "아무 것도 꾸며 내지 마십시오" 라고 말하는 것, 또는 프롬프트의 끝에 "제공된 맥락만 사용하십시오"와 같은 문장을 추가하는 것 등이 있습니다.

DSPy의 모듈들은 이러한 프롬프트 기법을 추상화하고 매개변수화하여 사용합니다. 이는 프롬프트, 세밀 조정, 증강 및 추론 기술을 적용하여 DSPy 서명을 작업에 조정하는 데 사용됩니다.

아래에서 시그니처가 ChainOfThought 모듈에 전달되고 입력 필드 context와 question의 값이 포함된 후 호출되는 방법을 볼 수 있습니다.

<div class="content-ad"></div>


# 옵션 1: ChainOfThought 모듈에 최소 서명 전달
generate_answer = dspy.ChainOfThought("context, question -> answer")

# 옵션 2: 또는 전체 표기 서명을 ChainOfThought 모듈에 전달
generate_answer = dspy.ChainOfThought(GenerateAnswer)

# 특정 입력에서 모듈 호출하기.
pred = generate_answer(context = "Which meant learning Lisp, since in those days Lisp was regarded as the language of AI.",
                       question = "What programming language did the author learn in college?")


아래에서 ChainOfThought 모듈이 초기에 구현한 "context, question -> answer" 서명을 확인할 수 있습니다. 직접 시도해 보고 싶다면 lm.inspect_history(n=1)을 사용하여 마지막 프롬프트를 출력할 수 있습니다.

![Image](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_5.png)

작성 시점 기준으로 DSPy에는 다음 여섯 개의 모듈이 구현되어 있습니다:


<div class="content-ad"></div>

- dspy.Predict: 입력 및 출력 필드를 처리하고 지시 사항을 생성하며 지정된 서명을 위한 템플릿을 작성합니다.
- dspy.ChainOfThought: Predict 모듈을 상속하고 "Chain of Thought" 처리 기능을 추가합니다.
- dspy.ChainOfThoughtWithHint: Predict 모듈을 상속하고 추론에 힌트를 제공하는 옵션으로 ChainOfThought 모듈을 향상시킵니다.
- dspy.MultiChainComparison: Predict 모듈을 상속하고 여러 체인 비교 기능을 추가합니다.
- dspy.Retrieve: 검색 모듈에서 단락을 검색합니다.
- dspy.ReAct: 사고, 행동 및 관찰의 교차 단계를 구성하는 데 사용됩니다.

이러한 모듈을 함께 연결하여 dspy.Module에서 상속된 클래스에서 두 가지 메서드를 사용할 수 있습니다. 이미 PyTorch와 구문적으로 유사성을 느낄 수 있을 것입니다:

- __init__(): 사용된 서브모듈을 선언합니다.
- forward(): 정의된 서브모듈 간의 제어 흐름을 설명합니다.

```python
class RAG(dspy.Module):
    def __init__(self, num_passages=3):
        super().__init__()

        self.retrieve = dspy.Retrieve(k=num_passages)
        self.generate_answer = dspy.ChainOfThought(GenerateAnswer)
    
    def forward(self, question):
        context = self.retrieve(question).passages
        prediction = self.generate_answer(context=context, question=question)
        return dspy.Prediction(context=context, answer=prediction.answer)
```

<div class="content-ad"></div>

The code snippet above visualizes the information flow within the modules of the RAG() class. Here's what it does in a nutshell:

![RAG Class Information Flow](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_6.png)

## Teleprompters: Enhancing DSPy Program Efficiency

Teleprompters in DSPy serve as efficiency boosters. By analyzing metrics and collaborating with the DSPy compiler, they are designed to generate and choose optimal prompts for the various modules in a DSPy program.

<div class="content-ad"></div>

```python
from dspy.teleprompt import BootstrapFewShot

# 간단한 텔레프롬프터 예제
teleprompter = BootstrapFewShot(metric=dspy.evaluate.answer_exact_match)
```

작성 시점에 DSPy는 다음 다섯 가지 텔레프롬프터를 구현합니다:

- dspy.LabeledFewShot: 예측기에 의해 사용될 k개의 샘플을 정의합니다.
- dspy.BootstrapFewShot: 부트스트랩
- dspy.BootstrapFewShotWithRandomSearch: BootstrapFewShot 텔레프롬프터를 상속하며 무작위 탐색 과정에 대한 추가 속성을 도입합니다.
- dspy.BootstrapFinetune: t는 세부 설정을 위한 BootstrapFewShot 인스턴스로 텔레프롬프터를 정의합니다.
- dspy.Ensemble: 여러 프로그램의 앙상블 버전을 생성하여 여러 프로그램의 다양한 출력을 단일 출력으로 줄입니다.

또한 SignatureOptimizer와 BayesianSignatureOptimizer가 있습니다. 이들은 제로/퓨-샷 상태에서 모듈 내 서명의 출력 접두사와 설명을 개선합니다.

<div class="content-ad"></div>

다양한 텔레프롬프터가 비용 대비 품질 등을 최적화하는 방식에서 서로 다른 트레이드오프를 제공합니다.

## DSPy 컴파일러

DSPy 컴파일러는 프로그램을 내부적으로 추적한 다음 최적화기(텔레프롬프터)를 사용하여 작업에 대한 특정 지표(예: 품질 또는 비용 개선)를 극대화합니다. 최적화는 사용 중인 LM 유형에 따라 달라집니다:

- LLM의 경우: 고품질 퓨-샷 프롬프트 생성
- 더 작은 LM의 경우: 자동 파인튠 훈련

<div class="content-ad"></div>

DSPy 컴파일러는 내부적으로 모듈을 고급 구성으로 매핑하여 프롬프팅, 세밀 조정, 추론 및 증강을 조화롭게 합니다. 이 과정에서는 프로그램의 다양한 버전을 시뮬레이션하고 각 모듈의 예시 트레이스를 부트스트랩하여 파이프라인을 작업에 최적화하기 위해 자체 개선합니다. 이 프로세스는 신경망의 훈련 과정과 유사합니다.

예를 들어, 이전에 생성된 ChainOfThought 모듈과 같은 초기 프롬프트는 작업을 이해하는 데 좋은 시작점일 수 있지만 아마 최적의 프롬프트는 아닐 것입니다. 다음 이미지에서 볼 수 있는 것처럼, DSPy 컴파일러는 초기 프롬프트를 최적화하여 수동 프롬프트 튜닝이 필요 없도록 합니다.

![image](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_7.png)

다음과 같은 입력을 컴파일러가 헤아립니다. 코드와 이미지로 보여집니다:

<div class="content-ad"></div>

프로그램, 텔레프롬프터(포함하여 정의된 검증 메트릭), 그리고 몇 가지 학습 샘플을 준비하였습니다.

```python
from dspy.teleprompt import BootstrapFewShot

# 질문과 답변 쌍을 포함한 소규모 훈련 세트
trainset = [dspy.Example(question="대학 전에 저자가 주로 다룬 두 가지 주제는 무엇인가요?", 
                         answer="글쓰기와 프로그래밍").with_inputs('question'),
            dspy.Example(question="대학 전에 저자가 어떤 종류의 글쓰기를 했나요?", 
                         answer="단편 소설").with_inputs('question'),
            ...
            ]

# 텔레프롬프터는 누락된 라벨(추론 체인 및 검색 컨텍스트)을 부트스트랩합니다
teleprompter = BootstrapFewShot(metric=dspy.evaluate.answer_exact_match)
compiled_rag = teleprompter.compile(RAG(), trainset=trainset)
```

![DSPy Example: Naive RAG Pipeline](/assets/img/2024-07-13-IntrotoDSPyGoodbyePromptingHelloProgramming_8.png)  

# DSPy 예제: 단순 RAG 파이프라인

<div class="content-ad"></div>

이제 DSPy의 모든 기본 개념에 익숙해졌으니 첫 번째 DSPy 파이프라인을 만들어봅시다.

현재 생성적 AI 공간에서 RAG(Retrieval-augmented generation)이 화제입니다. 제 작업을 따라오셨다면 LangChain(튜토리얼 참조)과 LlamaIndex(튜토리얼 참조)을 사용하여 기본과 고급 RAG 파이프라인을 만들었음을 보셨을 것입니다. 따라서 DSPy를 빠르고 간단한 RAG 파이프라인으로 시작하는 것이 합리적입니다.  

Jupyter 노트북 형식의 end-to-end 파이프라인을 찾으려면 DSPy GitHub 저장소의 Intro Notebook이나 Connor Shorten의 Getting Started with RAG in DSPy 노트북을 확인하시는 것을 추천합니다.

## Prerequisites: Installing DSPy

<div class="content-ad"></div>

dspy-ai Python 패키지를 설치하려면 간단히 다음 명령어를 사용할 수 있어요.

```bash
pip install dspy-ai
```

## 단계 1: 설정

먼저, LM 및 검색 모델(RM)을 설정해야 해요.

<div class="content-ad"></div>

- 좌측 메시지(LM): 오픈AI의 gpt-3.5-turbo를 사용할 것입니다. 이를 위해 오픈AI API 키가 필요합니다. API 키를 얻으려면 오픈AI 계정이 있어야 하며 API 키 섹션에서 "새 비밀 키 생성"을 클릭하세요.
- 우측 메시지(RM): 추가 데이터로 채워넣을 오픈 소스 벡터 데이터베이스 Weaviate를 사용할 것입니다.

LlamaIndex GitHub 리포지토리(MIT 라이선스)에서 예제 데이터를 외부 데이터베이스에 넣는 작업으로 시작해보겠습니다. 이 부분은 여러분의 데이터로 교체할 수 있습니다.

```js
mkdir -p 'data'
!wget 'https://raw.githubusercontent.com/run-llama/llama_index/main/docs/examples/data/paul_graham/paul_graham_essay.txt' -O 'data/paul_graham_essay.txt'
```

다음으로, 문서를 단일 문장으로 분할하고 데이터베이스에 삽입할 것입니다. 이 간단한 작업에 맞춰 기사에 포함된 Weaviate를 사용할 것이며 무료로 API 키에 가입하지 않고 사용하실 수 있습니다. Weaviate를 사용할 때 데이터를 "content"라는 속성으로 삽입하는 것이 중요하다는 점을 유의해주세요.

<div class="content-ad"></div>


```js
import weaviate
from weaviate.embedded import EmbeddedOptions
import re

# Weaviate 클라이언트를 임베디드 모드로 연결합니다.
client = weaviate.Client(embedded_options=EmbeddedOptions(),
                             additional_headers={
                                "X-OpenAI-Api-Key": "sk-<OPENAI-API-KEY>",
                             }
                         )

# Weaviate 스키마 생성
# DSPy는 컬렉션이 'content'라는 텍스트 키를 가정합니다.
schema = {
   "classes": [
       {
           "class": "MyExampleIndex",
           "vectorizer": "text2vec-openai",
            "moduleConfig": {"text2vec-openai": {},
           "properties": [{"name": "content", "dataType": ["text"]}]
       }      
   ]
}
    
client.schema.create(schema)

# 문서를 단일 문장으로 분할합니다.
chunks = []
with open("./data/paul_graham_essay.txt", 'r', encoding='utf-8') as file:
    text = file.read()
    sentences = re.split(r'(?<!\w\.\w.)(?<![A-Z][a-z]\.)(?<=\.|\?)\s', text)
    sentences = [sentence.strip() for sentence in sentences if sentence.strip()]
    chunks.extend(sentences)

# 일괄처리로 벡터 데이터베이스를 채웁니다.
client.batch.configure(batch_size=100)  # 일괄처리 구성

with client.batch as batch:  # 일괄 프로세스 초기화
    for i, d in enumerate(chunks):  # 데이터 일괄 가져오기
        properties = {
            "content": d,
        }
        batch.add_data_object(
            data_object=properties,
            class_name="MyExampleIndex"
        )
```

이제 글로벌 설정에서 LM 및 RM을 구성할 수 있습니다.

```js
import dspy
import openai
from dspy.retrieve.weaviate_rm import WeaviateRM

# OpenAI API 키 설정
openai.api_key = "sk-<OPENAI-API-KEY>"

# LLM 구성
lm = dspy.OpenAI(model="gpt-3.5-turbo")

# 리트리버 구성
rm = WeaviateRM("MyExampleIndex", 
                weaviate_client = client)

# DSPy를 다음 언어 모델 및 검색 모델을 기본 설정으로 사용하도록 구성합니다.
dspy.settings.configure(lm = lm, 
                        rm = rm)
```

## 단계 2: 데이터 수집


<div class="content-ad"></div>

이제, 몇 가지 학습 예제를 수집할 차례입니다 (이 경우, 수동으로 주석 달린 예제들입니다). 신경망을 학습하는 것과는 다르게, 여러분은 몇 가지 예제만 필요합니다.

```js
# 질문과 답변 쌍을 포함하는 작은 학습 세트
trainset = [dspy.Example(question="대학에 진학하기 전에 저자가 주로 일했던 두 가지는 무엇인가요?", 
                         answer="글쓰기와 프로그래밍").with_inputs('question'),
            dspy.Example(question="대학에 진학하기 전에 저자가 어떤 글쓰기를 했나요?", 
                         answer="단편 소설").with_inputs('question'),
            dspy.Example(question="저자가 처음으로 공부한 컴퓨터 언어는 무엇이었나요?", 
                         answer="포트란").with_inputs('question'),
            dspy.Example(question="저자의 아버지가 어떤 컴퓨터를 샀나요?", 
                         answer="TRS-80").with_inputs('question'),
            dspy.Example(question="저자의 대학 계획은 무엇이었나요?", 
                         answer="철학 공부").with_inputs('question'),]
```

## 단계 3: DSPy 프로그램 작성

이제, 여러분은 첫 번째 DSPy 프로그램을 작성할 준비가 되었습니다. RAG 시스템이 될 것입니다. 먼저, GenerateAnswer라는 Signatures에 표시된 것과 같은 서명 문맥, 질문-응답을 정의해야 합니다.

<div class="content-ad"></div>

```python
class GenerateAnswer(dspy.Signature):
    """질문에 간단한 사실 기반 답변을 제공합니다."""

    context = dspy.InputField(desc="관련된 사실을 포함할 수 있음")
    question = dspy.InputField()
    answer = dspy.OutputField(desc="보통 1부터 5단어 사이")
```

위의 서명을 정의한 후에는 dspy.Module을 상속받은 사용자 정의 RAG 클래스를 작성해야 합니다. __init__(): 메서드에서 관련 모듈을 선언하고 forward() 메서드에서 모듈 간의 정보 흐름을 설명합니다.

```python
class RAG(dspy.Module):
    def __init__(self, num_passages=3):
        super().__init__()

        self.retrieve = dspy.Retrieve(k=num_passages)
        self.generate_answer = dspy.ChainOfThought(GenerateAnswer)
    
    def forward(self, question):
        context = self.retrieve(question).passages
        prediction = self.generate_answer(context=context, question=question)
        return dspy.Prediction(context=context, answer=prediction.answer)
```

## 스텝 4: DSPy 프로그램 컴파일하기


<div class="content-ad"></div>

드디어, 텔레프롬프터를 정의하고 DSPy 프로그램을 컴파일할 수 있게 됐어요. ChainOfThought 모듈에서 사용할 프롬프트를 업데이트할 거에요. 예를 들어 간단한 BootstrapFewShot 텔레프롬프터를 사용할 거예요.

```python
from dspy.teleprompt import BootstrapFewShot

# 우리의 RAG 프로그램을 컴파일할 기본 텔레프롬프터 설정
teleprompter = BootstrapFewShot(metric=dspy.evaluate.answer_exact_match)

# 컴파일!
compiled_rag = teleprompter.compile(RAG(), trainset=trainset)
```

이제 아래와 같이 RAG 파이프라인을 호출할 수 있어요:

```python
pred = compiled_rag(question="대학에서 저자가 배운 프로그래밍 언어는 무엇인가요?")
```

<div class="content-ad"></div>

여기서 당신은 결과를 평가하고, 파이프라인의 성능에 만족할 때까지 과정을 거듭할 수 있습니다. 평가에 대한 자세한 지침은 DSPy GitHub 저장소의 Intro Notebook이나 Connor Shorten의 DSPy Notebook에서 "Getting Started with RAG"을 확인하는 것을 추천드립니다.

## 요약

이 글은 현재 Generative AI 커뮤니티에서 관심을 받고 있는 DSPy 프레임워크[1]를 간단히 소개했습니다. DSPy 프레임워크는 LM 기반 응용 프로그램을 수동 프롬프트 엔지니어링에서 프로그래밍으로 이동시키는 일련의 개념을 소개합니다.

DSPy에서는 전통적인 프롬프트 엔지니어링 개념이 다음과 같이 대체됩니다:

<div class="content-ad"></div>

- 시그니처는 손으로 쓴 프롬프트를 대체합니다.
- 모듈은 구체적인 프롬프트 엔지니어링 기법을 대체합니다.
- 텔레프롬프터와 DSPy 컴파일러가 프롬프트 엔지니어링의 수동 반복을 대체합니다.

DSPy 개념을 소개한 후, 이 기사는 OpenAI 언어 모델을 사용한 나이브 RAG 파이프라인 예제와 검색 모델로 Weaviate 벡터 데이터베이스를 소개합니다.

# 이야기가 마음에 드셨나요?

구독하고 무료로 새 이야기를 발행할 때 알림을 받으세요.

<div class="content-ad"></div>

LinkedIn, Twitter, 그리고 Kaggle에서 저를 찾아주세요! 

# 면책 성명

본 글 작성 시점에서 제는 Weaviate의 개발자 대변인입니다.

# 참고문헌

<div class="content-ad"></div>

## 문헌

[1] Khattab, O., Singhvi, A., Maheshwari, P., Zhang, Z., Santhanam, K., Vardhamanan, S., ... & Potts, C. (2023). Dspy: Compiling declarative language model calls into self-improving pipelines. arXiv preprint arXiv:2310.03714.

## 추가 학습 자료

- DSPy에서 제공하는 Intro Notebook
- Connor Shorten의 비디오 시리즈: DSPy 설명!
- Connor Shorten의 "DSPy에서 RAG 시작하기!"와 관련된 Jupyter Notebook

<div class="content-ad"></div>

## 이미지

그 외 명시되지 않은 한, 모든 이미지는 저자가 만들었습니다.