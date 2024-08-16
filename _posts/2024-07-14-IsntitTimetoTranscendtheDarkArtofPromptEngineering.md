---
title: "프롬프트 엔지니어링의 암흑 예술을 넘어설 때가 되지 않았나요"
description: ""
coverImage: "/assets/img/2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_0.png"
date: 2024-07-14 01:58
ogImage: 
  url: /assets/img/2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_0.png
tag: Tech
originalTitle: "Isn’t it Time to Transcend the Dark Art of Prompt Engineering?"
link: "https://medium.com/ai-mind-labs/isnt-it-time-to-transcend-the-dark-art-of-prompt-engineering-5e50ee216853"
isUpdated: true
---





![Image](/assets/img/2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_0.png)

# 프롬프트 엔지니어링: 어둠의 예술?

대규모 언어 모델(LLM) 시대가 도래하면서 몇 가지 새로운 현상이 등장했는데, 그 중에서도 프롬프트 엔지니어링은 가장 주목할 만합니다. 프롬프트 엔지니어들은 원하는 결과물을 생성하도록 언어 모델을 유도하기 위한 텍스트 조합을 시도합니다.

저의 겸손한 의견으로는 프롬프트 엔지니어링의 표준적인 실천은 공학보다는 연금술에 더 가깝습니다. 하드 코딩된 긴 텍스트 스크립트는 컴퓨터 프로그래밍보다 주술에 더 가깝습니다.

<div class="content-ad"></div>

---
마크다운 형식으로 이미지를 변경해주세요:
[2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_1.png](/assets/img/2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_1.png)

코딩의 최상의 실천 방법은 가능한 한 하드코딩을 피하는 것을 가르칩니다. 그러나 이렇게 복잡한 텍스트 프롬프트는 하드코딩을 다시 도입하고, 가능한 광범위하게 규모가 확대됩니다.

이제 대안을 탐색할 적절한 시기입니다.이 기사에서는 프롬프트 엔지니어링에 대해 더 견고한 접근 방식인 DSPy를 살펴보겠습니다. 그러나 먼저, 하드코딩된 프롬프트 엔지니어링이 왜 최적이 아닌지 이해해 봅시다.

* 이 기사에는 노트북이 있습니다. 끝 부분에 링크했습니다.
---

<div class="content-ad"></div>

## 하드코딩 된 프롬프트: 취약하고 유지 관리가 어려움

하드코딩된 프롬프트는 프롬프트 엔지니어링을 하는 데 그다지 최적화되지 않은 방법입니다. 가장 명백한 이유는 융통성의 부족입니다. 하드코딩된 프롬프트는 데이터나 심지어 LLM의 선택에 적응하지 않습니다. 그것들은 가독성 측면에서 다루기 힘들기 때문에 장기적으로 코드베이스 유지보수에 문제가 발생합니다. 특히 대규모 언어 모델이 함께 작동하는 시스템이 있을 때는 이 문제가 심각해집니다.

그러므로 하드코딩된 프롬프트는 데이터 드리프트, 모델 업데이트에 취약하게 만들며 기술적 부채를 악화시킬 수 있습니다.

# 보다 견고하고 체계적인 프롬프트 엔지니어링을 향하여 — DSPy

<div class="content-ad"></div>

DSPy 프레임워크를 이해하려면 그 주요 구성 요소인 모듈, 서명 및 텔레프럼터를 이해해야 합니다.

- 모듈: DSPy는 LLM(Language Learning Models)에 지시하기 위한 구성 가능하고 선언적인 모듈을 제공합니다. 이 모듈들은 프로그램이라고 불리는 임의로 복잡한 파이프라인을 만드는 데 사용될 수 있습니다.
- 서명: 각 모듈의 입력 및 출력 동작을 정의합니다. 이 서명은 언어 모델에 의해 해석되므로 정확하고 간결해야 합니다.
- 텔레프럼터: DSPy는 프로그램을 컴파일하는 데 사용되는 텔레프럼터를 제공합니다. 여기서 컴파일이란 주어진 LLM 및 훈련 데이터 집합에 대해 프로그램을 정의된 성능 지표에 맞게 최적화하는 것을 의미합니다.

참고 — gif는 단일 모듈로 구성된 프로그램을 보여줍니다. gif는 약 15초입니다.

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*Migw6jk0uGjSvYJFpAGg2w.gif)

<div class="content-ad"></div>

마치 마법사처럼, DSPy는 최적의 프롬프트를 만들기 위한 모델 및 데이터에 중립적인 프레임워크를 제공합니다. 여기서 '프로그램'이라는 용어를 사용한 것은 곧 당신이 프롬프트를 직접 쓰는 것이 아니라 프롬프트를 생성하기 위해 프로그램을 작성하게 될 것이기 때문입니다.

참고 - 만약 DSPy에 대해 깊이 파고들고 싶다면, "Compiling Declarative Language Calls Into Self-Improving Pipelines" 논문을 읽어보세요.

# 프롬프트를 쓰는 것이 아니라 프로그래밍을 생각해보세요!

이 프레임워크의 힘을 입증하기 위해, 저는 수학 문제 해결을 위해 간단한 것부터 (조금 더) 복잡한 것까지 다양한 예제 프로그램 네 개를 만들어 보았습니다.

<div class="content-ad"></div>

GSM8K를 사용할 예정입니다. 이는 LLM의 수리 추론 능력을 성능 기준으로 자주 사용되는 수학 문제 데이터셋입니다.

코드에 대한 설명에 들어가기 전에, 프로그램이 어떻게 구성되는지 이해하는 데 도움이 되는 몇 가지 개념을 알아두세요.

- 프로그램은 작은 모듈들이 연결된 클래스로 구성됩니다. 이러한 모듈들은 클래스의 init 메서드에서 선언합니다.
- forward 메서드를 사용하면 이전에 선언한 모듈을 사용하여 임의로 복잡한 프로그램을 구성할 수 있습니다.
- DSPy에서 여러 "사용 가능한" 모듈들을 제공하며, LLM에 대한 바닐라 호출, Chain-of-Thought, ReAct 등을 수행할 수 있습니다. 또한 사용자 정의 모듈을 생성할 수도 있습니다.

# 수학 문제 해결을 위한 프로그램 예시

<div class="content-ad"></div>

DSPy 프로그램들은 모델에 구애받지 않기 때문에 여러 LLMs 간에 프로그램이 어떻게 작동하는지 비교할 수 있어요. 제가 선택한 모델은 다음과 같아요:

- 16k 컨텍스트를 가진 GPT-3.5 Turbo (오픈 AI).
- Mistral-7B-Instruct-v0.1-GPTQ: 퀀터이즈된 70억 개 파라미터 버전인 Mistral instruct 모델입니다.

사용한 텔레프롬프터는 DSPy에서 기본 제공되는 Few-shot Bootstrap Teleprompter를 사용했어요. 이 텔레프롬프터는 "Teacher" 모델을 활용하여, 학습 데이터에서 "부트스트랩" 프롬프트를 생성하고 우리가 정의한 성능 평가 지표에 대해 반복적으로 성능을 측정하여 프로그램을 컴파일합니다. 결과적으로, 우리는 한 모델의 능력을 활용하여 다른 모델을 안내하는 방식으로 작동하고 있어요.

# 생성된 프롬프트에 대한 중요한 참고 사항

<div class="content-ad"></div>

만약 이미 알지 못하는 정보이기 때문에 프롬프트가 프로그램에서 생성된 것이라는 점을 명확하게 해주고 싶다면, 그렇게 되는 이유는 컴파일된 프로그램이 트레이닝 데이터셋에 대해 텔레프롬프터를 실행하여 생성된 프롬프트인 경우이다. 이 경우, 트레이닝 데이터셋은 GSM8K를 위한 다섯 가지 예시 수학 문제이다. 컴파일되지 않은 프롬프트는 트레이닝 데이터 없이 생성된다.

생성된 프롬프트를 따르는 데 도움을 주기 위해 중요한 것은 그들이 다음 질문에 답하려고 노력하고 있다는 것을 알아두는 것입니다:

# 1. Baseline Program

기준 프로그램은 질문을 가져와 답변으로 응답하는 단일 모듈 프로그램입니다.

<div class="content-ad"></div>

DSPy에서 제공하는 predict 모듈을 사용합니다. 이 모듈은 LLM에 일반 호출을 실행합니다. 서명은 여기에서 약식으로 정의됩니다. ‘질문 -` 답변’으로, predict 모듈에게 질문 입력을 예상하고 출력으로 답변을 생성하도록 알립니다.

생성된 프롬프트: GPT-3.5 Turbo 16K

- 컴파일되지 않은 베이스라인 프로그램 프롬프트.
- 컴파일된 베이스라인 프로그램 프롬프트.

생성된 프롬프트: Mistral-7B-Instruct-v0.1-GPTQ

<div class="content-ad"></div>

**- 미컴파일된 베이스라인 프로그램 프롬프트.
- 컴파일된 베이스라인 프로그램 프롬프트.**

## 2. 연상의 연쇄 프로그램

이것은 '연상의 연쇄' 모듈을 사용하는 또 다른 단일 모듈 프로그램입니다. 서명은 이전에 정의된 베이스라인 프로그램과 마찬가지로 기본 질문 입력과 답변 출력을 사용합니다. 주요 차이점은 연상의 연쇄 모듈이 답변을 반환하기 전에 추가적인 "추론" 단계를 추가한다는 것입니다.

생성된 프롬프트: GPT-3.5 Turbo 16K

<div class="content-ad"></div>

- 컴파일하지 않은 'Chain of Thought' 프로그램 프롬프트.
- 컴파일된 'Chain of Thought' 프로그램 프롬프트.

생성된 프롬프트: Mistral-7B-Instruct-v0.1-GPTQ:

- 컴파일하지 않은 'Chain of Thought' 프로그램 프롬프트.
- 컴파일된 'Chain of Thought' 프로그램 프롬프트.

# 3. Multi Comparison Program

<div class="content-ad"></div>

이 프로그램은 모듈을 여러 개 호출하여 예측 모듈을 통해 임의의 요청을 발생시켜 질문에 대한 여러 후보 답변을 생성합니다. 이러한 후보 답변을 다른 모듈에 공급하여 최적의 답변을 결정할 수 있습니다.

우리는 압축된 표기법으로 시그니처를 정의하는 대신 약간 더 복잡한 입출력 동작을 통합할 수 있는 클래스로서 정의합니다.

생성된 프롬프트: GPT-3.5 Turbo 16K:

- 컴파일되지 않은 다중 비교 프로그램 프롬프트
- 컴파일된 다중 비교 프로그램 프롬프트

<div class="content-ad"></div>

**10. The Tarot Guide**

안녕하세요! 오늘은 타로 전문가와 함께해요.

**4. 아날로그 추론 프로그램**

이 프로그램은 "대형 언어 모델을 아날로그 추론자로 사용"이라는 논문에서 영감을 받은 멀티 모듈 프로그램입니다. 이 프로그램은 Analogy, ExampleQuestionAnswer, 및 ContextQuestionAnswer 클래스에 의해 정의된 사용자 지정 서명을 가진 Predict 및 Chain of Thought 모듈을 사용합니다.

<div class="content-ad"></div>

프로그램은 먼저 유사한 예시 질문(또는 유추)을 임의로 생성한 후, 사고 연쇄 프롬프팅을 사용하여 유추 질문에 답변을 생성합니다. 마지막으로, 사고 연쇄 프롬프팅을 사용하여 예시 질문과 답변의 문맥을 기반으로 원래 질문에 대한 답변이 생성됩니다.

생성된 프롬프트: GPT-3.5 Turbo 16K

- 미컴파일된 유추 프로그램 프롬프트.
- 컴파일된 유추 프로그램 프롬프트.

생성된 프롬프트: Mistral-7B-Instruct-v0.1-GPTQ

<div class="content-ad"></div>

**- 컴파일되지 않은 유추 프로그램 프롬프트.
- 컴파일된 유추 프로그램 프롬프트.**

## 프로그램 평가

프로그램들은 GSM8K 데이터셋의 일부를 사용하여 20개의 무작위 테스트 예제로 평가됩니다. 성능은 테스트 예제에서 정답의 백분율로 채점됩니다.

테스트 데이터셋이 매우 작기 때문에 결과는 결론을 내기에는 아직 미흡할 수 있습니다.

<div class="content-ad"></div>

## GPT-3.5 Turbo 16K

지난 GPT-3.5 Turbo 모델은 여러 모듈 프로그램 (유추 및 다중 구성)을 컴파일한 후 컴파일하지 않은 프로그램보다 개선효과를 나타냅니다. 그럼에도, 모든 프로그램은 기준과 비교했을 때 모델의 성능을 향상시킵니다.

![image](/assets/img/2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_2.png)

## Mistral-7B-Instruct-v0.1-GPTQ

<div class="content-ad"></div>

Mistral 모델은 컴파일 후 어떤 향상도 나타내지 않습니다. 마지막 섹션에 링크된 프롬프트를 면밀히 조사하면, 모델이 부트스트랩된 예제가 프롬프트에 포함된 경우 혼란스러워하는 것으로 보입니다.

컴파일되지 않은 Chain of Thought 프로그램은 베이스 모델 대비 일부 개선을 보여줍니다. 아마도 더 간단한 프로그램이 더 작은 모델과 더 잘 작동한다는 것을 나타낼 수 있습니다.

![img](/assets/img/2024-07-14-IsntitTimetoTranscendtheDarkArtofPromptEngineering_3.png)

# 결론

<div class="content-ad"></div>

DSPy는 프롬프트 엔지니어링에서의 패러다임 변화입니다. 긴 텍스트 프롬프트를 손으로 입력하는 대신, 프로그램을 구축하여 자동으로 생성하고 있습니다. 또한, 문구를 다듬는 대신, 정의된 성능 지표에 대해 프로그램을 최적화하고 있습니다.

DSPy는 모델과 데이터에 중립적인 측면이 있어, 하드코딩된 프롬프트의 부서진 측면을 어느 정도 해소하는 방식으로 나아가고 있습니다. 이는 저자들이 이 프레임워크를 "자가 발전형" 파이프라인 구축 방법으로 손꼽을 정도로까지 이르기도 합니다. 프로그램을 구축할 때 저에게는 하늘이 한계가 없다는 느낌을 주었습니다.

DSPy를 사용한 프롬프트 엔지니어링은 컴퓨터 프로그래밍 쪽으로 나아가고 연금술적인 측면에서 멀어지는 올바른 방향으로 나아가는 것 같습니다. 이는 프로그램을 평가할 수 있는 능력을 제공함으로써 프로세스에 엄격성을 도입합니다.

프레임워크에 대해 두 가지 문제점만 있습니다. 첫 번째는 접근 가능한 문서가 부족하다는 것입니다. 그러나, 저자들은 노트북, 강의, 그리고 물론 소스 코드를 제공했습니다. 충분한 인내심과 연습을 통해 그 문제를 극복하는 것은 그리 어렵지 않습니다. 두 번째 문제는 모니터링에 관한 것으로, 컴파일 과정 주변에 어떤 더 나은 분석이 있으면 좋겠다는 점입니다.

<div class="content-ad"></div>

If you want to delve deeper into the subject, I highly recommend checking out the collaborative notebook I've put together. I provide a walkthrough of the notebook in my YouTube video.

Keep in mind that the results when running the notebook could differ slightly from what's mentioned in this article, as the testing is conducted on a very limited set of questions.

If you're eager to enhance your artificial intelligence skills, consider joining the waiting list for my course, where I'll assist you in creating advanced applications powered by large language models.

For those looking to bring AI transformation to their business, schedule a discovery call today.

<div class="content-ad"></div>

인공 지능, 데이터 과학 및 대규모 언어 모델에 대한 추가 통찰을 얻고 싶다면 YouTube 채널을 구독해보세요. 🌟✨