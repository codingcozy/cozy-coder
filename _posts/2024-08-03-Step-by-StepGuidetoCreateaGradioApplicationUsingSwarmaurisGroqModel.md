---
title: "Swarmauri의 GroqModel을 사용하여 Gradio 애플리케이션 만드는 단계별 가이드"
description: ""
coverImage: "/assets/img/2024-08-03-Step-by-StepGuidetoCreateaGradioApplicationUsingSwarmaurisGroqModel_0.png"
date: 2024-08-03 20:36
ogImage: 
  url: /assets/img/2024-08-03-Step-by-StepGuidetoCreateaGradioApplicationUsingSwarmaurisGroqModel_0.png
tag: Tech
originalTitle: "Step-by-Step Guide to Create a Gradio Application Using Swarmauris GroqModel"
link: "https://medium.com/@swarmauri/step-by-step-guide-to-create-a-gradio-application-using-swarmauris-groqmodel-b1817c26512f"
---


![이미지](/assets/img/2024-08-03-Step-by-StepGuidetoCreateaGradioApplicationUsingSwarmaurisGroqModel_0.png)

그라디오는 머신 러닝 모델을 위한 웹 인터페이스를 빠르게 만들 수 있는 훌륭한 도구입니다. 이 안내서에서는 Swarmauri의 GroqModel을 사용하는 간단한 Gradio 애플리케이션을 만드는 방법을 배우게 될 것입니다.

# 단계 1: 환경 설정

시작하기 전에 필요한 라이브러리와 패키지를 설치했는지 확인하세요. Swarmauri의 swarmauri-sdkpython 패키지가 필요합니다. 주의: 현재 swarmauri-sdk는 베타 버전입니다.

<div class="content-ad"></div>

이 종속성을 설치하려면 다음 명령을 실행하세요:

```js
pip install swarmauri[full]==0.4.0
```

# 단계 2: 필요한 라이브러리 가져오기

새 Python 파일(예: app.py)을 만들고 필요한 라이브러리를 가져오세요.

<div class="content-ad"></div>

```js
import os
import gradio as gr
from swarmauri.standard.llms.concrete.GroqModel import GroqModel
from swarmauri.standard.agents.concrete.SimpleConversationAgent import SimpleConversationAgent
from swarmauri.standard.conversations.concrete.Conversation import Conversation
```

# Step 3: GroqModel 초기화하기

API 키를 사용하여 GroqModel을 설정하세요. API 키는 Groq API 서비스에서 받을 수 있습니다. API 키를 환경 변수를 사용하여 안전하게 저장하고 불러오는 것을 기억하세요.

```js
# 환경 변수에서 API 키를 가져오거나 직접 정의하세요 (배포용으로 권장되지 않습니다)
API_KEY = os.getenv('GROQ_API_KEY')

# GroqModel 초기화하기
llm = GroqModel(api_key=API_KEY)

# GroqModel을 가지고 SimpleConversationAgent를 생성하세요
conversation = Conversation()
agent = SimpleConversationAgent(llm=llm, conversation=conversation)
```

<div class="content-ad"></div>

# 단계 4: Gradio 인터페이스 정의하기

Gradio 인터페이스를 설정하고 사용자 입력을 처리하고 응답을 반환하는 함수를 정의하세요.

```js
# Gradio 인터페이스를 실행할 함수 정의
def converse(input_text, history):
    result = agent.exec(input_text)
    return result

# Gradio 인터페이스 구성 요소 생성
demo = gr.ChatInterface(
    fn=converse,
    examples=["Hello!"],
    title="무엇이든 물어보세요!",
    multimodal=False,
)
```

# 단계 5: 애플리케이션 실행하기

<div class="content-ad"></div>

그라디오 인터페이스를 실행하세요.

```js
if __name__ == "__main__":
    interface.launch(share=True)
```

# app.py 전체 코드

다음은 app.py 전체 코드입니다:

<div class="content-ad"></div>

```js
import os
import gradio as gr
from swarmauri.standard.llms.concrete.GroqModel import GroqModel
from swarmauri.standard.agents.concrete.SimpleConversationAgent import SimpleConversationAgent
from swarmauri.standard.conversations.concrete.Conversation import Conversation

# 환경 변수에서 API 키 가져오기 또는 직접 정의하기 (프로덕션 환경에서 권장되지 않음)
API_KEY = os.getenv('GROQ_API_KEY')

# GroqModel 초기화
llm = GroqModel(api_key=API_KEY)

# GroqModel과 함께 SimpleConversationAgent 생성
agent = SimpleConversationAgent(llm=llm, conversation=Conversation())

# gradio 인터페이스에 실행할 함수 정의
def converse(input_text, history):
    result = agent.exec(input_text)
    return result

demo = gr.ChatInterface(
    fn=converse,
    examples=["Hello!"],
    title="Ask me anything!",
    multimodal=False,
)

if __name__ == "__main__":
    demo.launch()
```

# 애플리케이션 실행하기

애플리케이션을 실행하려면 터미널을 열어서 app.py가 있는 디렉토리로 이동한 후 다음을 실행하십시오:

```js
python app.py
```

<div class="content-ad"></div>

터미널에 URL(예: http://127.0.0.1:7860/)이 표시됩니다. 이 URL을 웹 브라우저에서 열어서 Swarmauri 에이전트와 상호 작용해보세요.

# 축하합니다!

이제 GroqModel을 사용하는 Swarmauri의 SimpleConversationAgent와 상호 작용하는 간단한 Gradio 애플리케이션을 만드셨습니다. 이 안내서는 기본적인 기반 역할을 하며, 더 복잡한 대화 흐름 처리나 추가 서비스 통합과 같은 추가 기능을 포함하여이 애플리케이션을 확장할 수 있습니다. 즐거운 코딩 되세요!

# 함께하실래요?

<div class="content-ad"></div>

우리는 Swarmauri를 개발자와 데이터 과학자들을 위한 강력한 도구 모음으로 지속적으로 발전시키고 있습니다. 여러분의 기여, 피드백 및 참여가 이 프로젝트를 번창하게 만듭니다.

- GitHub 저장소
- 커뮤니티 Discord

Swarmauri 커뮤니티의 일원이 되어 주셔서 감사합니다. 함께 텍스트 처리와 기계 학습을 지금까지보다 더 접근 가능하고 강력하게 만들어 봅시다!

즐겁게 코딩하세요! 🚀