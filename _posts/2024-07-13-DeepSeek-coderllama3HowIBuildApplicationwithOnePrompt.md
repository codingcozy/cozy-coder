---
title: "DeepSeek-coder와 llama 3 하나의 프롬프트로 애플리케이션을 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_0.png"
date: 2024-07-13 03:14
ogImage: 
  url: /assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_0.png
tag: Tech
originalTitle: "DeepSeek-coder + llama 3 How I Build Application with One Prompt"
link: "https://medium.com/gitconnected/deepseek-coder-llama-3-how-i-build-application-with-one-prompt-05a77b02cace"
isUpdated: true
---





![image](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_0.png)

마에스트로가 비디오 게임을 만들 수 있는지 궁금했어요. 그래서 게임을 만들기 위해 부탁했는데, 하나의 규칙이 있어요: 한 줄의 코드도 쓰지 않겠어요. 마에스트로가 모든 코드를 작성할 거예요. 저는 그저 HTML 파일을 실행시킬 거에요. 이 일은 매우 잘 될 수도 있고 놀랄만한 결과를 가져올 수도 있어요.

스네이크 게임에 들어가는 모든 구성 요소를 설명하는 프롬프트를 작성했어요. 빠르게 실행되면 스네이크 게임을 만들기 위한 코드가 생성되어요. 코드 작성이 완료되면 해당 코드가 포함된 HTML, CSS, JavaScript 파일이 타임스탬프와 함께 이름이 지정된 폴더에 생성돼요. 작업 기록은 Markdown 문서 파일로 저장돼요.

![image](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_1.png)

<div class="content-ad"></div>

그러면 제가 생성한 결과물을 실행했습니다. 확장 프로그램을 클릭해서 라이브 서버를 실행하고, 생성된 index.html 파일을 마우스 오른쪽 버튼으로 클릭하여 '라이브 서버로 열기'를 선택했어요. 결과를 확인해 봅시다!

하지만 작동하지 않아요. 어떤 문제인지 정확히 모르지만, 모든 UI 요소가 멋있어 보이지만 작동하지 않아요. DeepSeek 코더가 에이전트 작업에 최적화되지 않았을 수도 있어요. 클로드와 달리 그런 작업에 능숙한 Maestro가 더 나을 수도 있어요. 혹은 마에스트로가 제대로 실행하지 않았을 수도 있죠.

![이미지](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_2.png)

문제를 해결하기 전에, Maestro라는 자동화 도구를 소개할게요. Maestro는 간단한 텍스트 입력으로 다양한 애플리케이션을 만들어내는 자동화 도구에요. Maestro는 Claude 3.5 Sonnet, GPT-4o, Gemini 1.5 Pro와 같은 AI 모델을 사용하는 혁신적인 프레임워크에요. 사용자가 요청한 목표를 하위 작업으로 분해하고 각 작업을 실행하여 생산된 결과를 합성하는 과정을 자동화해요.

<div class="content-ad"></div>

이 비디오에서는 Maestro에 대해 다루겠습니다. 주요 기능과 작동 방법을 설명하겠습니다. Maestro를 설치하는 방법을 보여드리고 마지막으로 Snake Game 문제를 해결하고 Memo 앱을 만들고 랜딩 페이지를 생성하는 방법에 대해 알려드리겠습니다.

# 시작하기 전에! 🦸🏻‍♀️

이 주제를 좋아하시고 제게 도움을 주고 싶다면:

- 제 글을 50번 클랩해주세요; 정말로 도움이 됩니다. 👏
- Medium에서 저를 팔로우하고 최신 글을 구독해주세요 🫶
- YouTube 채널에서도 저를 팔로우해주세요
- Discord에서 자세한 정보 확인하기

<div class="content-ad"></div>

# Maestro란 무엇인가요?

Maestro는 사용자의 목표를 관리 가능한 하위 작업으로 나누고 각 하위 작업에 대한 실행 결과를 정제하여 통합하여 일관된 최종 출력물을 생성하는 Python 기반 프레임워크입니다. Maestro는 Orchestrator 모델, Sub-agent 모델 및 Refiner 모델을 사용하여 Anthropic, OpenAI, Google AI, Ollama와 같은 다양한 API를 통해 지정된 셈 계획 작업, 하위 작업 및 정제 작업을 실행합니다.

## Maestro의 주요 기능은 다음과 같습니다:

a. 목표 분해: Orchestrator 모델을 사용하여 사용자의 요청된 목표를 상세 작업으로 분해합니다.

<div class="content-ad"></div>

**b. Task Execution**: Perform each subtask using the Sub-agent model and generate the output for each task.

**c. Results Refinement**: The Refiner model combines the results of subtasks to create the final output and verifies if the output is acceptable.

**d. Detailed Log**: Save the exchange log that documents the entire process as a Markdown file.

**e. Code Generation**: Generate the necessary files and folders for your code project with timestamps.

<div class="content-ad"></div>

# 마에스트로 구현 방법?

## 1- 올라마 설치하기

올라마가 설치되어 있지 않다면, Ollama.ai로 이동해서 다운로드 버튼을 클릭하고, 사용 중인 운영 체제를 선택하여 설치하세요.

![이미지](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_3.png)

<div class="content-ad"></div>

1- 설치 및 배포

이 비디오에서는 DeepSeek-Coder-V2를 사용합니다. Ollama 라이브러리로 이동하여 DeepSeek Coder V2를 클릭하고 터미널에 복사하여 붙여 넣은 후 엔터를 누릅니다.

![이미지](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_4.png)

```js
ollama run deepseek-coder-v2
```

<div class="content-ad"></div>

오라마가 현재 DeepSeek Coder를 다운로드할 것입니다. 인터넷 속도에 따라 몇 분이 걸릴 수 있어요. 설치가 완료되면 다음 단계로 넘어갑니다.

3- 깃허브 저장소

깃허브 저장소를 클론해주세요. 아래 명령어를 사용하여 저장소를 클론하세요.


git clone https://github.com/Doriandarko/maestro.git


<div class="content-ad"></div>

Visual Studio를 열어 가상 환경을 생성하고 활성화하세요.

```shell
source env/bin/activate
```

환경이 활성화되면 아래 명령어로 종속 항목을 설치하세요.

```shell
pip install litellm rich tavily-python
```

<div class="content-ad"></div>

설치하고 완료한 후에는 마에스트로-어니 API 파일로 이동하여 각 단계에 대해 모델을 정의합니다. 로컬 모델을 사용하고 있기 때문에 저렴하고 효과적이며, API 비용을 지불하지 않고도 모든 작업을 로컬에서 수행할 수 있습니다.

나는 모든 단계에 DeepSeek-Code V2 모델을 사용할 것입니다. 해당 모델을 서브 에이전트 및 리파이너에 복사하여 붙여넣은 다음 파일을 저장하세요.

![이미지](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_5.png)

```js
# 각 단계에 사용할 모델 정의
ORCHESTRATOR_MODEL = "ollama/deepseek-coder-v2"
SUB_AGENT_MODEL = "ollama/deepseek-coder-v2"
REFINER_MODEL = "ollama/deepseek-coder-v2"
```

<div class="content-ad"></div>

한 번 완료되면 마에스트로 폴더로 이동해서 파일을 실행하세요.

```python
cd maestro
python maestro-anyapi.py
```

# 노트 앱 만들기:

이제 노트 앱을 만들어봅시다. 다음과 같이 입력해보세요:

<div class="content-ad"></div>

친애하는 성역들, 

참좋아요! 당신의 채팅 봇을 통해 파일 텍스트를 추가할 지 물어보고 싶진 않아. 그럼, Tavily API를 사용할지 물어보죠. 우린 그것도 원하지 않아. 할 일들을 생성하려면 엔터 키를 눌러 코드 생성을 기다려봐. 

Markdown 형식으로 변경된 이미지입니다.

![2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_6](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_6.png)

먼저 다양한 하위 작업을 만들고 그런 다음 이러한 작업을 하위 에이전트에게 할당합니다. 그들은 설정한 모델에 작업을 전달하며 앱에 대한 폴더를 몇 분 동안 생성하는 데 필요할 것입니다.

<div class="content-ad"></div>

**Markdown format:**

[2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_7.png](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_7.png)

[2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_8.png](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_8.png)

When we navigate to the Maestro directory, we'll find a newly generated folder containing HTML, CSS, and JavaScript files. It's great to have everything neatly organized in one place for easier access.

Let's give it a test run and see how amazing it is! The functionality is superb, and it works seamlessly. This platform is a gem - fully functional, visually appealing, and top-notch in performance.

<div class="content-ad"></div>

마에스트로에게 코드 생성을 요청하여 랜딩 페이지를 만들어 봅시다.

```js
Html, ccs 및 Js를 사용하여 완전한 웹사이트 생성기로 즉시 완벽한 랜딩 페이지를 만들어보세요.
```

<div class="content-ad"></div>

자, 이걸 해볼까요? 코드 생성에 몇 분이 걸릴 것입니다.

![Image](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_10.png)

조금 시간이 걸립니다. 홈페이지의 텍스트는 명확하고 간결하며, 제품의 주요 기능을 강조하는 데 점을 찍어 보였습니다. 하지만, 웹사이트에 이미지가 표시되지 않습니다.

웹사이트는 수용할만한 모습이지만, 개선이 필요합니다. 이것은 AI SaaS를 만들기 위한 출발점이 될 수 있습니다. 프롬프트로 돌아가보면, 이것은 단순한 샘플일 뿐입니다. 원하는 결과를 얻기 위해 프롬프트를 개선할 수 있습니다.

<div class="content-ad"></div>

**Snake 문제 해결하기:**

우리는 Snake 문제를 해결하는 방법을 새로운 모델인 Llama 3로 추가함으로써 해결했습니다. 이 모델은 Meta가 만든 GPT-3 모델로, 다양한 질의에 자연어로 응답할 수 있는 챗봇을 만드는 데 사용할 수 있습니다.

Llama 3을 설치해야 합니다. Ollama 라이브러리로 이동하여 Llama 3을 검색하고, 선택한 버전을 클릭하세요. 이 비디오에서는 최신 버전을 사용하고 있습니다. 모델을 복사하여 터미널에 붙여넣고, 설치가 완료될 때까지 몇 분 기다려주세요.

![image](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_11.png)

<div class="content-ad"></div>

Maestro_any_API 파일로 가서 Orchestrator 모델을 Llama 3로 변경하세요. DeepSeek Code보다 자연어를 더 잘 이해하는 Llama 3로 변경한 이유입니다. 모델을 결합하면 더 나은 결과를 얻을 수 있어요.

![image](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_12.png)

터미널로 돌아가서 프롬프트를 입력해보세요.

```js
create snake game with start button using html, css, and js
```

<div class="content-ad"></div>

엔터 키를 눌러 몇 분 기다려 주세요. 라마 3는 큰 모델이라서 결과를 출력하는 데 시간이 걸려요.

이제 코드를 가지고 있어요. Snake 파일로 가서 HTML을 클릭하고 라이브 서버로 열어보세요. 게임이 멋있어 보이고 간단한 UI 요소를 갖고 있네요. 게임이 작동하는지 확인해 보겠습니다. 놀랍죠? 정상적으로 작동하고 모든 것이 멋져 보이네요.

![2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_13](/assets/img/2024-07-13-DeepSeek-coderllama3HowIBuildApplicationwithOnePrompt_13.png)

## 추천:  

<div class="content-ad"></div>

마에스트로와 함께 사용하기 좋은 세 가지 모델을 강력히 추천해 드려요: Gemma2, DeepSeek-Coder 및 Llama 3이에요. 이들이 제공하는 결과물은 상당히 인상적이며 멋진 인터페이스를 생성해냅니다.

마지막으로 :

마에스트로는 AI 기술을 통해 프로그래밍 장벽을 낮추는 혁신적인 프레임워크에요. 누구나 손쉽게 다양한 애플리케이션을 만들 수 있게 도와주죠. 복잡한 코딩 없이도 여러 AI 모델을 조합하여 최적의 결과를 달성할 수 있게 되어 원하는 애플리케이션을 만들어보세요. 다양한 아이디어를 실현하기 위해 마에스트로를 활용해보세요.

마에스트로에 대한 몇 가지 리뷰를 소개해 드리겠어요:

<div class="content-ad"></div>

- 오직 대상 기능을 설명하는 텍스트를 입력하는 것만으로도 누구나 쉽게 프로그램을 만들 수 있어요.
- 한 번 프로그램을 만들고 나면 수정할 수 있는 능력이 없어요.
- 프로그램의 완성도는 AI 모델의 성능에 의해 결정돼요.

마에스트로의 간단하고 다재다능한 기능을 활용하여 더 창의적이고 혁신적인 결과물을 만들어보세요. 다음에 더 유용한 정보와 함께 다시 찾아올게요. 

📚제 다른 글도 언제든지 확인해보세요: