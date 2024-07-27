---
title: "파이썬으로 만드는 GenAI AI에 인격을 부여하고 대화하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer_0.png"
date: 2024-07-14 02:11
ogImage: 
  url: /assets/img/2024-07-14-GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer_0.png
tag: Tech
originalTitle: "GenAI With Python: Give Your AI a Personality and Speak with ”Her”"
link: "https://medium.com/towards-artificial-intelligence/genai-with-python-give-your-ai-a-personality-and-speak-with-her-5d3958d36a63"
---


![Link Name](https://miro.medium.com/v2/resize:fit:1400/1*BAkXlbE_qXSKeKpyyxiPTQ.gif)

안녕하세요! 이번 글에서는 GPU 없이 노트북에서 특정 인공지능의 성격을 만들고 음성 대화를 할 수 있는 방법에 대해 알려드릴게요.

## 소개

<div class="content-ad"></div>

**챗봇(Chatbot)에 관한 튜토리얼**

챗봇(Chatbot)은 텍스트나 텍스트 음성 변환을 통해 자동 채팅 대화를 수행하는 소프트웨어 응용 프로그램입니다. 인간 에이전트와 상호 작용을 모방합니다. 처음으로 사용된 것은 ELIZA(1966)였는데, 이는 패턴 매칭 및 대체 방법론을 사용하여 텍스트 대화를 시뮬레이션했습니다(들을 수도 말할 수도 없었어요).

챗봇이 대화 외의 작업을 수행할 수 있는 경우, "음성 어시스턴트(voice assistant)"라고 합니다. 자연어 처리(NLP)와 음성 인식을 활용하여 음성 명령에 응답합니다(i.e., Siri나 Alexa). 현재 시장에서 가장 발전된 것은 아마도 새로운 Apple의 AI인데, 이는 Siri를 화면 인식(화면에 있는 것들을 조작할 수 있습니다)과 대형 언어 모델(LLM)로 능력을 갖추게 합니다.

이 튜토리얼에서는 ChatGPT와 같이 AI를 생성하여 챗봇에 개성을 부여하고 음성 인식을 사용하여 채팅할 것입니다. 다른 유사한 경우에 쉽게 적용할 수 있는 유용한 Python 코드 일부를 제시하고(복사하여 붙여넣고 실행하기만 하면 됩니다), 주석이 달린 모든 코드 라인을 따라가면서 이 예제를 복제할 수 있도록 안내하겠습니다(전체 코드 링크는 아래에 있습니다).

## 언어 모델(Language Model)

<div class="content-ad"></div>

**최신 LLM 풍경:**

- 오픈AI의 ChatGPT, 가장 많이 사용되는 모델 (여기에서 시도해보세요)
- Anthropic의 Claude (여기에서 시도해보세요)
- 구글의 Gemini (여기에서 시도해보세요)
- Meta의 Llama (여기에서 시도해보세요)
- Microsoft의 Phi, 사용 가능한 가장 작은 모델로 GPU 없이 노트북에서 실행할 수 있습니다 (여기에서 시도해보세요)
- StabilityAI의 StableLM
- Cohere의 CommandR (여기에서 시도해보세요)
- Snowflake의 Arctic (여기에서 시도해보세요)
- Alibaba의 Qwen (여기에서 시도해보세요)
- 01AI의 Yi
- X의 Grok
- NVIDIA의 Megatron
- 아마존의 Olympus (아직 출시 예정)
- 애플의 MM1 (아직 출시 예정)

게다가, 다음 세대 인공지능 파동은 텍스트에서 비디오로의 변환일 것으로 보이며, 현재 최고의 모델은 오픈AI의 Sora와 Luma의 DreamMachine입니다.

![이미지 가리키는 링크](https://miro.medium.com/v2/resize:fit:1400/1*V27d3YBdK2TMEqQhq_SzNg.gif)

<div class="content-ad"></div>

오픈 소스 LLM을 로컬 머신에서 실행할 수 있는 대안 라이브러리가 여러 가지 있습니다. 그 중 주요한 것들은 Ollama, HuggingFace, DSPy, LangChain입니다. 저는 강력하면서 사용자 친화적인 Ollama를 사용하려고 합니다.

먼저, 해당 웹사이트에서 다운로드해야 합니다. 그런 다음 명령 프롬프트에서 다음 명령을 사용하여 노트북에서 LLM을 다운로드하고 실행할 수 있습니다:

![이미지 설명](/assets/img/2024-07-14-GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer_1.png)

지금은 Phi3를 사용하고 있습니다. 그 이유는 현재 가장 효율적인 모델이기 때문입니다. 또한 스마트하고 가벼워서 좋습니다.

<div class="content-ad"></div>

**이미지 파일을 Markdown 형식으로 변환해보세요:**
![image](https://miro.medium.com/v2/resize:fit:1400/1*nSN_VF1WVxDI-F26vOkCxg.gif)

/set을 입력하면 사용 가능한 모든 명령을 볼 수 있어요:

![image](/assets/img/2024-07-14-GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer_2.png)

우리는 /set 시스템 'prompt' 명령을 사용하여 베이스 모델을 사용자 정의하고 AI에게 일을 수행할 작업을 쓸 수 있는 개성을 부여할 수 있어요. 이를 Prompt Engineering이라고 하죠 (AI가 수행해야 할 작업을 쓰는 과제에 주어진 멋진 이름이에요):

<div class="content-ad"></div>

새로운 모델을 테스트해 봅시다:

![Image](https://miro.medium.com/v2/resize:fit:1400/1*FUI5JuDgtKCOW-m1MeyuLg.gif)

만족스러운 새로운 성격이라면, `/save '이름'` 명령어로 저장해 보세요:

<div class="content-ad"></div>


![GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer](/assets/img/2024-07-14-GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer_4.png)

안녕하세요! 채팅을 종료하려면 /bye를 실행하면 되고, 모델을 삭제하려면 ollama rm `name`을 사용하시면 됩니다.

이제 AI 모델을 사용할 수 있습니다. Python에서 ollama 라이브러리를 사용해보세요:

```python
!pip install ollama
#0.2.1
import ollama

res = ollama.generate(model="maya", prompt="who are you?")
print(res["response"])
```


<div class="content-ad"></div>

**이미지 태그를 Markdown 형식으로 변경해주세요:**


![이미지](/assets/img/2024-07-14-GenAIWithPythonGiveYourAIaPersonalityandSpeakwithHer_5.png)


또는 채팅 기능을 사용하여 AI 채팅처럼 스트리밍 효과를 낼 수 있어요:

```python
res = ollama.chat(model="maya", 
                  messages=[{"role":"system", "content":""},
                            {"role":"user", 
                             "content":"오늘 저녁에 뭐 먹을까요?"}],
                  stream=True)
for chunk in res:
    print(chunk["message"]["content"], end="")
```

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*mvOSie6J3umZRgaX5PefNg.gif)

<div class="content-ad"></div>

## 음성 인식

첫 음성 인식 시스템(1950)은 숫자를 알아들을 수 있었지만 단어는 인식하지 못했습니다. IBM Shoebox(1960)은 처음으로 몇 가지 영어 단어를 이해하고 응답할 수 있었습니다.

오늘날 가장 많이 사용되는 시스템은 Google의 API이며, 그들을 사용하는 쉬운 방법은 SpeechRecognition 라이브러리를 통해 가능합니다:

```python
!pip install SpeechRecognition
#3.10.4
import speech_recognition as sr

recognizer = sr.Recognizer()
with sr.Microphone() as mic:
    recognizer.adjust_for_ambient_noise(mic, duration=1)            
    print("listening...")
    audio = recognizer.listen(mic)

text = recognizer.recognize_google(audio)
print(text)
```

<div class="content-ad"></div>

![tarot-gif](https://miro.medium.com/v2/resize:fit:1400/1*ZaRptvj-diG1xRPgSEHNuQ.gif)

이제 음성인식 부분을 처리했으니, 이제 챗봇이 응답할 수 있도록 설정해야 합니다. 그러므로 텍스트를 음성으로 변환하는 작업이 필요합니다. 저는 Google Text-to-Speech(gTTS)를 사용하여 mp3 파일을 저장한 뒤 OS 라이브러리를 사용하여 쉽게 재생할 수 있도록 할 것입니다.

```python
!pip install gTTS
#2.5.1
from gtts import gTTS
import os

text = "안녕하세요!"

speaker = gTTS(text=text, lang="ko", slow=False)
speaker.save("res.mp3")
os.system("afplay res.mp3")  #mac->afplay | windows->start
os.remove("res.mp3")
```

## 챗봇

<div class="content-ad"></div>

마침내, 이제 모든 것을 하나의 스크립트에 통합할 수 있게 되었어요.

지금까지 본 모든 코드는 우리 AI에 대화 기능을 제공해 주었는데, 현재로서는 대화형 봇보다는 음성 비서에 가까워요. 사용자가 특정 키워드를 말하면 특정 답변을 하는 것과 시간을 알려 주는 등의 미리 지정된 명령도 추가할 수 있답니다.

```js
# 설정
## 음성 인식을 위한
import speech_recognition as sr #3.10.4

## 텍스트 음성으로 변환
from gtts import gTTS #2.5.1

## 언어 모델
import ollama #0.2.1
model = "maya"

## 데이터를 다루기 위해
import os
from datetime import datetime
import numpy as np


# AI 만들기
class ChatBot():
    def __init__(self, name):
        print("---", name, " 시작 중---")
        self.name = name.lower()

    def speech_to_text(self):
        recognizer = sr.Recognizer()
        with sr.Microphone() as mic:
            recognizer.adjust_for_ambient_noise(mic, duration=1)
            print("듣는 중...")
            audio = recognizer.listen(mic)
        try:
            self.text = recognizer.recognize_google(audio)
            print("나 --> ", self.text)
        except:
            print("나 -->  오류")

    @staticmethod
    def text_to_speech(text):
        print("AI --> ", text)
        speaker = gTTS(text=text, lang="en", slow=False)
        speaker.save("res.mp3")
        os.system("afplay res.mp3")  #맥->afplay | 윈도우->start
        os.remove("res.mp3")
    
    ## 미리 지정된 명령어
    def wake_up(self, text):
        lst = ["wake up "+self.name, self.name+"wake up ", "hey "+self.name]
        return True if any(i in ai.text.lower() for i in lst) else False

    def what(self, text):
        lst = ["what are you", "who are you"]
        return True if any(i in ai.text.lower() for i in lst) else False

    @staticmethod
    def action_time():
        return datetime.now().time().strftime('%H:%M')


# AI 실행하기
if __name__ == "__main__":
    
    ai = ChatBot(name="Maya")
    
    while True:
        ai.speech_to_text()

        ## 깨어나기
        if ai.wake_up(ai.text) is True:
            res = "안녕, 나는 AI Maya야, 무엇을 도와 드릴까요?"

        ## 나는 누구?
        elif ai.what(ai.text) is True:
            res = "나는 Mauro가 만든 AI야"

        ## 시간 알려주기
        elif "time" in ai.text:
            res = ai.action_time()
        
        ## 예의 바르게 대답하기
        elif any(i in ai.text for i in ["감사","고마워"]):
            res = np.random.choice(["천만에요!","언제든 물어보세요!","문제 없어!","멋져!","도와줄 준비가 되어 있어요!","조용히 하고 있을게요!"])
        
        ## 대화하기
        else:   
            res = ollama.generate(model=model, prompt=ai.text)["response"]
            res = res.split("\n")[0]

        ai.text_to_speech(res)
```

![동적이미지](https://miro.medium.com/v2/resize:fit:1400/1*EHuq5hogJotp6ZYChphNrg.gif)

<div class="content-ad"></div>

## 결론

이 기사는 Ollama와 Speech Recognition을 사용하여 직접 음성 비서를 구축하는 방법을 보여주는 튜토리얼이었습니다. 특히, 우리는 사용자 정의 LLM을 활용하여 대화 작업에 초점을 맞췄습니다. 

즐겁게 즐겼으면 좋겠어요! 궁금한 점이나 피드백, 흥미로운 프로젝트를 공유하고 싶다면 언제든지 연락해 주세요.