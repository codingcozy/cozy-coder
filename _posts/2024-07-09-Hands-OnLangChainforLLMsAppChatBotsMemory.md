---
title: "LangChain과 LLMs를 활용한 실전 ChatBots 메모리 구축 방법"
description: ""
coverImage: "/assets/img/2024-07-09-Hands-OnLangChainforLLMsAppChatBotsMemory_0.png"
date: 2024-07-09 23:54
ogImage: 
  url: /assets/img/2024-07-09-Hands-OnLangChainforLLMsAppChatBotsMemory_0.png
tag: Tech
originalTitle: "Hands-On LangChain for LLMs App: ChatBots Memory"
link: "https://medium.com/towards-artificial-intelligence/hands-on-langchain-for-llms-app-chatbots-memory-9394030e5a9e"
---


언어 모델과 대화하는 경우, 기존의 메모리가 없다는 점은 자연스럽고 연결된 대화를 만드는 데 중요한 장애물이 될 수 있습니다. 사용자는 연속성과 문맥 유지를 기대하며, 이는 기존의 모델에서 부족한 부분입니다. 이 제한은 계속되는 대화가 사용자 참여와 만족도에 중요한 요소인 응용 프로그램에서 특히 두드러집니다.

LangChain은 이 도전 과제를 해결하기 위한 견고한 솔루션을 제공합니다. 여기서 "메모리"란 언어 모델이 이전 대화의 일부를 기억하고 해당 정보를 사용하여 이후 상호 작용을 안내할 수 있는 능력을 가리킵니다. 메모리를 모델 구조에 통합함으로써, LangChain은 Chatbot 및 유사한 응용 프로그램이 인간과 유사한 대화를 모방하는 대화 흐름을 유지할 수 있도록 합니다.

LangChain의 메모리 기능은 단순히 과거 상호 작용을 상기시키는 것을 넘어섭니다. 이는 관련 정보를 저장, 조직화, 검색하기 위한 정교한 메커니즘을 포함하며, 이를 통해 Chatbot이 대화 문맥을 기반으로 적절하게 응답할 수 있도록 합니다. 이는 사용자 경험을 향상시킬 뿐만 아니라 Chatbot이 시간이 지남에 따라 더 정확하고 관련성 있는 응답을 제공할 수 있도록 합니다.

![이미지](/assets/img/2024-07-09-Hands-OnLangChainforLLMsAppChatBotsMemory_0.png)

<div class="content-ad"></div>

## 목차:

- 작업 환경 설정 및 시작하기
- 대화 버퍼 메모리
- 대화 버퍼 창 메모리
- 대화 토큰 버퍼 메모리
- 대화 요약 메모리

# 1. 작업 환경 설정

시작하려면 OS를 가져오고 OpenAI를 가져와야 합니다. OpenAI 비밀 키를 로드해야 합니다. 이를 로컬에서 실행하고 아직 OpenAI가 설치되지 않았다면 OpenAI를 설치하기 위해 pip를 실행해야 할 수 있습니다.

<div class="content-ad"></div>


이 후에 gpt-3.5-turbo로 사용할 LLM 모델을 정의할 것입니다.

LLM 모델 = "gpt-3.5-turbo"

# 2. 대화 버퍼 메모리

<div class="content-ad"></div>

마음을 다잡고 기억력을 향상시키는 예제부터 시작해봅시다. LangChain을 사용하여 채팅이나 챗봇 대화를 관리해보는 것으로요. 저는 오픈AI의 LLM을 채팅 인터페이스로 설정하고, 온도를 0으로 설정할 거에요. 우리는 ConversationBufferMemory를 메모리로 사용하고 대화 체인을 구축할 거에요.

```python
from langchain.chat_models import ChatOpenAI
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

llm = ChatOpenAI(temperature=0.0, model=llm_model)
memory = ConversationBufferMemory()
conversation = ConversationChain(
    llm=llm, 
    memory=memory,
    verbose=True
)   
```

이제 `conversation.predict()`를 사용하여 대화를 시작해볼게요. 주어진 입력은 "안녕, 나는 유세프야" 가 될 거에요. 그리고 응답을 확인해보아요.

```python
conversation.predict(input="Hi, my name is Youssef")
```

<div class="content-ad"></div>

그러니까, "1 더하기 1은 뭐야?" 라고 물어보세요. 1 더하기 1은 2지요. 그리고 그 다음에, "내 이름이 뭐야?" 하고 물어보세요.


`conversation.predict(input="What is 1+1?")`


그럼 다시 "내 이름이 뭐야?" 물어보세요. 그러면 "당신의 이름은 Youssef입니다, 앞서 말씀하신대로" 라는 대답을 확인하실 수 있어요.


`conversation.predict(input="What is my name?")`


<div class="content-ad"></div>

verbose 변수를 true로 변경하면 LangChain이 무엇을 하는지 확인할 수 있어요. 대화.predict을 다시 실행하면 다음 내용이 반환돼요:

LangChain이 낙심 있는 친근한 대화를 나누기 위해 생성한 프롬프트에요. 대화의 두 번째, 세 번째 부분에 이를 실행하면 프롬프트가 아래와 같이 유지돼요.

저는 메모리 변수를 사용해 대화 내용을 저장했어요. 그래서 만약 memory.buffer를 출력하면 지금까지의 대화가 저장돼 있어요.

```js
print(memory.buffer)
```

<div class="content-ad"></div>

메모리 변수를 로드하려면 `memory.load_memory_variables('')`를 사용할 수 있어요. 여기서 중괄호는 빈 사전을 나타내는 거에요. 좀 더 고급 기능들이 있는데, 이 글에서는 다루지 않았지만 LangChain 문서에서 찾을 수 있어요. 그래서 여기에 빈 중괄호가 있다는 걱정은 하지 마세요. 지금까지의 대화 기억을 LangChain이 기억한 대로 보여드린 거예요. 대화를 저장하는 방법은 ConversationBufferMemory를 사용해서 이렇게 할 수 있어요. 메모리를 명시적으로 추가하고 싶다면 이렇게 하면 돼요.


memory.load_memory_variables({})


LangChain이 대화를 저장하는 방법은 ConversationBufferMemory를 사용하는 거예요. 몇 가지 입력과 출력을 지정하려면, 새로운 항목을 메모리에 추가하는 방법은 이렇게 되요.


memory = ConversationBufferMemory()
memory.save_context({"input": "안녕"}, 
                    {"output": "어떻게 지내?"})


<div class="content-ad"></div>

Memory.saveContext가 말했어요. 안녕, 어떤가요? 이거 그렇게 흥미로운 대화는 아니지만, 짧은 예제를 주고 싶었어요. 그래서, 이게 현재의 메모리 상태에요. 한 번 더, 메모리 변수를 보여줄게요. 이제, 만약 메모리에 추가적인 데이터를 추가하고 싶다면, 계속해서 추가적인 컨텍스트를 저장할 수 있어요. 대화는 계속되어요. 그렇게 많이는 없어요, 그냥 놀고 있어요, 멋져요.

```js
print(memory.buffer)
```

대화를 위해 큰 언어 모델을 사용할 때, 그 큰 언어 모델 자체는 상태를 가지고 있지 않아요. 언어 모델 자체는 지금까지 진행된 대화를 기억하지 않아요. 그래서 API 엔드포인트 호출마다 독립적으로 이뤄져요. 챗봇은 보통 이미 진행된 전체 대화를 LLM에게 컨텍스트로 제공해주는 빠른 코드 때문에 메모리를 가지게 됐어요. 그래서 메모리는 지금까지의 용어나 발언을 명백히 저장할 수 있어요. 안녕, 내 이름은 Youssef야. 안녕, 당신을 만나서 반가워요 등.

이 메모리 저장소는 LLM에 대한 입력이나 추가적인 컨텍스트로 사용되어요. 이렇게 함으로써, 그것은 지금까지 말했던 것을 알고 다음 대화 턴을 갖는 것처럼 출력을 생성할 수 있어요.

<div class="content-ad"></div>

대화가 길어질수록 필요한 메모리 양이 많아지고, LLM에게 전송해야 할 토큰의 양도 많아지면서 처리해야 하는 토큰의 수에 기반한 비용도 더 비싸질 수 있어요.

그래서 LangChain은 대화를 저장하고 축적하기 위한 여러 유형의 편리한 메모리를 제공해요. 지금까지 우리는 ConversationBufferMemory를 살펴봤어요. 이번에는 다른 유형의 메모리를 살펴볼게요.

# 3. Conversation Buffer Window Memory

오직 메모리 창을 유지하는 대화 버퍼 창 메모리를 가져오는 것으로 시작해봐요. k가 1인 대화 버퍼 창 메모리로 설정하면, 변수 k=1은 제가 대화 교환이 하나만 기억하려고 한다는 걸 지정해요. 즉, 제가 하나, 챗봇이 하나의 발언만을 기억하고 싶다는 거죠.

<div class="content-ad"></div>

```python
from langchain.memory import ConversationBufferWindowMemory
메모리 = ConversationBufferWindowMemory(k=1)
메모리.load_memory_variables({})
```

만약 "안녕, 뭐해?, 별거 아닌데, 그냥 노는 중" 이런 대화를 저장하고 싶다면, `memory.load_memory_variables('')`를 사용하면 아래와 같이 가장 최근의 발언만 기억하게 됩니다.

```python
메모리.save_context({"input": "안녕"},
                   {"output": "뭐해?"})
메모리.save_context({"input": "별거 아닌데, 그냥 노는 중"},
                   {"output": "굿"})
메모리.load_memory_variables({})
```

k가 1이었기 때문에 최근 대화만 반환되었습니다. 이 기능은 몇 가지 최근 대화를 추적할 수 있도록 해주기 때문에 좋은 기능입니다. 실제로 이것을 k가 1인 상태로 사용하지는 않을 것입니다. 보통 k를 더 큰 숫자로 설정해서 사용할 것입니다.

<div class="content-ad"></div>

위의 대화를 다시 실행한다면 “안녕, 제 이름은 Youssef입니다”라고 말할 것입니다. 그런 다음 “1 더하기 1은 무엇인가요?”라고 물을 것입니다. 마지막으로 “제 이름은 무엇인가요?”라고 물을 것입니다. k를 1로 설정했기 때문에, 1 더하기 1은 무엇인가요?의 정보만 기억합니다. 1 더하기 1 답은 2이며, 이 초기 교환이 잊혔으며, 이제 지금은 그 정보에 접근할 수 없다고 말합니다.

```js
conversation.predict(input="안녕, 제 이름은 Youssef입니다")
```

```js
conversation.predict(input="1+1은 무엇인가요?")
```

```js
conversation.predict(input="제 이름은 무엇인가요?")
```

<div class="content-ad"></div>

# 4. 대화 토큰 버퍼 메모리

대화 토큰 버퍼 메모리를 사용하면 메모리가 저장되는 토큰의 수를 제한할 수 있습니다. 많은 LLM 가격은 토큰에 기반을 두기 때문에, 이는 LLM 호출 비용과 직접적으로 연결됩니다. 최대 토큰 제한을 50으로 설정해봅시다. 다음과 같은 대화를 입력해보겠습니다. "AI가 뭐야? 놀라운거지. 역전파는 뭐야? 아름다워. 챗봇은 뭐야? 매력적이지."

```python
from langchain.memory import ConversationSummaryBufferMemory
from langchain.memory import ConversationTokenBufferMemory
from langchain.llms import OpenAI

llm = ChatOpenAI(temperature=0.0, model=llm_model)

memory = ConversationTokenBufferMemory(llm=llm, max_token_limit=50)
memory.save_context({"input": "AI가 뭐야?!"}, 
                    {"output": "놀라운거지!"})
memory.save_context({"input": "역전파는 뭐야?"}, 
                    {"output": "아름다워!"})
memory.save_context({"input": "챗봇은 뭐야?"}, 
                    {"output": "매력적이지!"})

memory.load_memory_variables({})
```

토큰 제한을 높게 설정하고 실행하면 대화의 거의 전부를 저장합니다. 토큰 제한을 100으로 늘리면 이제 대화 전체를 저장합니다.

<div class="content-ad"></div>


메모리 = ConversationTokenBufferMemory(llm=llm, max_token_limit=50)
memory.save_context({"input": "AI는 무엇인가?!"}, {"output": "멋진!"})
memory.save_context({"input": "역전파는 무엇인가?"}, {"output": "아름다운!"})
memory.save_context({"input": "챗봇은 무엇인가?"}, {"output": "매력적인!"})
memory.load_memory_variables({})


만약 20으로 줄인다면, 최신 교환에 해당하는 토큰 수를 유지하기 위해, 한도를 초과하지 않도록 이전 대화의 초기 부분을 잘라냅니다.


메모리 = ConversationTokenBufferMemory(llm=llm, max_token_limit=20)
memory.save_context({"input": "AI는 무엇인가?!"}, {"output": "멋진!"})
memory.save_context({"input": "역전파는 무엇인가?"}, {"output": "아름다운!"})
memory.save_context({"input": "챗봇은 무엇인가?"}, {"output": "매력적인!"})
memory.load_memory_variables({})


# 5. 대화 요약 메모리


<div class="content-ad"></div>

마침내, 우리가 탐구할 마지막 유형의 기억이 있습니다. 바로 대화 요약 버퍼 메모리입니다. 이 아이디어는 최근 발언에 기반한 고정된 토큰 수로 메모리를 제한하는 대신, 대화 요약을 작성하고 그것을 메모리로 사용하는 것입니다.

여기 제품 팀과 미팅 일정을 잡는 긴 문자열을 만들 예제가 있습니다:

```js
# 긴 문자열 만들기
schedule = "제품 팀과 아침 8시에 미팅이 있습니다. \
파워포인트 발표 자료를 준비해야 합니다. \
오전 9시부터 12시까지 LangChain 프로젝트에 시간을 할애하고 싶습니다. \
Langchain이 매우 강력한 도구이기 때문에 빠르게 진행될 것입니다. \
정오에 이탈리아 레스토랑에서 고객과 식사를 합니다. \
고객은 1시간 이상 거리를 운전하여 AI의 최신 동향을 이해하고자 당신을 만나러 옵니다. \
최신 LLM 데모를 보여주기 위해 노트북을 반드시 챙기세요."
```

이 경우에는 400개의 최대 토큰 제한을 사용하는 대화 요약 버퍼 메모리를 사용할 것입니다. 이는 상당히 높은 토큰 제한이며, "안녕하세요, 어떤 소식이야, 그저 노는 중이야, 멋지다, 오늘 일정은 어떤가, 그리고 답변은 위의 긴 일정"이라는 대화 용어를 몇 개 삽입할 것입니다.

<div class="content-ad"></div>

그 기억소재는 이제 상당히 많은 텍스트를 포함하고 있어요. 이 기억소재 변수들을 살펴볼까요? 이 모든 텍스트가 들어있는 이유는 400 토큰이 이 텍스트를 저장하는 데 충분했기 때문이에요.

```js
memory = ConversationSummaryBufferMemory(llm=llm, max_token_limit=100)
memory.save_context({"input": "안녕"}, {"output": "어떻게 지내?"})
memory.save_context({"input": "별일 없어, 그냥 노는 중"},
                    {"output": "멋져"})
memory.save_context({"input": "오늘 일정은 뭐야?"}, 
                    {"output": f"{schedule}"})

memory.load_memory_variables({})
```

만약 최대 토큰 제한을 100으로 줄인다면, 대화 요약 버퍼 메모리는 아래와 같이 대화 내용을 요약하는데 LLM(언어 모델)을 사용할 거에요. OpenAI 엔드포인트를 사용해서 대화 내용을 요약해요.

```js
memory = ConversationSummaryBufferMemory(llm=llm, max_token_limit=400)
memory.save_context({"input": "안녕"}, {"output": "어떻게 지내?"})
memory.save_context({"input": "별일 없어, 그냥 노는 중"},
                    {"output": "멋져"})
memory.save_context({"input": "오늘 일정은 뭐야?"}, 
                    {"output": f"{schedule}"})
```

<div class="content-ad"></div>

만약 우리가 대화를 나누어 이 LLM을 사용한다면, 이전과 같이 대화 체인을 생성해볼까요? 제가 "어떤 데모를 보여줄까 좋을까"라는 입력을 받아들였고, verbose를 true로 설정했습니다. LLM은 현재 대화가 이 주제에 대해 논의되었으므로 이와 같은 요약을 제시하고 있습니다.

```js
conversation = ConversationChain(
    llm=llm, 
    memory=memory,
    verbose=True
)
conversation.predict(input="어떤 데모를 보여줄까 좋을까?")
```

대화 요약 버퍼 메모리를 통해 명시적으로 메시지를 지정된 토큰 한도까지 저장하는 것을 목표로 하고 있습니다. 이 경우, 명시적 저장을 100토큰까지 제한하고 있습니다. 이 한도를 초과하는 내용은 CLM을 사용해 요약되었습니다.

LangChain에는 추가적인 메모리 타입도 포함되어 있으며, 가장 주목할 만한 것 중 하나는 벡터 데이터 메모리입니다. 단어 임베딩과 텍스트 임베딩에 익숙한 경우, 벡터 데이터베이스 내에 이러한 임베딩을 저장하는 것이 특히 유용합니다. 이러한 유형의 벡터 데이터베이스 메모리를 활용하면 LangChain이 가장 관련성 높은 텍스트 블록을 추출하여 기억 능력을 크게 향상시킬 수 있습니다.

<div class="content-ad"></div>

LangChain을 사용하여 응용 프로그램을 개발할 때, 여러 가지 메모리 유형을 유연하게 활용할 수 있습니다. 이에는 이 기사에서 보여준 대화 메모리를 활용하는 것 뿐만 아니라 특정 개인을 기억하기 위한 엔티티 메모리도 포함됩니다.

이 접근 방식을 통해 대화의 요약된 버전과 관련된 주요 개인에 대한 명시적 세부 정보를 보관할 수 있습니다. 게다가, 개발자들은 종종 대화의 전체 내용을 키-값 스토어나 SQL 데이터베이스와 같은 전통적인 데이터베이스에 저장하기도 합니다.

이를 통해 감사 목적이나 시스템 기능 강화를 위해 대화로 다시 참조할 수 있게 됩니다. 따라서, 이러한 메모리 유형들은 응용 프로그램을 구축하는 강력한 프레임워크를 제공합니다.

## 이 기사가 마음에 들었다면, 제게 서포트를 원하신다면 반드시:


<div class="content-ad"></div>

- 👏 스토리에 박수를 치면(50번) 이 기사가 주목받게 도와주세요
- To Data & Beyond 뉴스레터 구독하기
- 제 Medium 팔로우하기
- 📰 제 Medium 프로필에서 더 많은 콘텐츠 보기
- 🔔 나를 팔로우하기: LinkedIn | Youtube | GitHub | Twitter

## To Data & Beyond 뉴스레터 구독하고 제 글을 완전하게 이해하고 빠르게 엑세스하세요:

## 데이터 과학과 AI 채용에 관심이 있으신가요? 어떻게 할 지 모르시나요? 데이터 과학 멘토링 세션과 장기적 경력 멘토링 제공합니다:

- 멘토링 세션: [링크](https://lnkd.in/dXeg3KPW)
- 장기적 멘토링: [링크](https://lnkd.in/dtdUYBrM)

<div class="content-ad"></div>

![Hands-OnLangChainforLLMsAppChatBotsMemory](/assets/img/2024-07-09-Hands-OnLangChainforLLMsAppChatBotsMemory_1.png)