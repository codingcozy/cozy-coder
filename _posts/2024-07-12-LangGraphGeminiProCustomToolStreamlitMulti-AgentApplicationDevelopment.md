---
title: "LangGraph  Gemini Pro  Custom Tool  Streamlit  다중 에이전트 애플리케이션 개발 방법 설명"
description: ""
coverImage: "/assets/img/2024-07-12-LangGraphGeminiProCustomToolStreamlitMulti-AgentApplicationDevelopment_0.png"
date: 2024-07-12 23:52
ogImage:
  url: /assets/img/2024-07-12-LangGraphGeminiProCustomToolStreamlitMulti-AgentApplicationDevelopment_0.png
tag: Tech
originalTitle: "LangGraph + Gemini Pro + Custom Tool + Streamlit = Multi-Agent Application Development"
link: "https://medium.com/gitconnected/langgraph-gemini-pro-custom-tool-streamlit-multi-agent-application-development-79c1473086b8"
isUpdated: true
---

![Chatbot Creation](/assets/img/2024-07-12-LangGraphGeminiProCustomToolStreamlitMulti-AgentApplicationDevelopment_0.png)

안녕하세요! 이번 포스트에서는 LangGraph나 Gemini Pro와 같은 모델로 사용자의 지원 요청에 응답할 수 있는 챗봇을 만드는 방법에 대해 알아볼 거예요.

LangGraph는 LLM을 이용해 상태가 있는 "다중 액터 응용프로그램"을 구축하기 위한 라이브러리에요. LangChain 표현 언어는 여러 체인(또는 액터)이 여러 단계에 걸쳐 순환하도록 확장될 수 있어요.
"LangChain"의 가치 중 하나는 사용자 정의 체인을 쉽게 만들 수 있다는 점이에요.

우리는 이를 위한 함수로 LangChain 표현 언어를 제공했지만, 순환을 도입하는 것은 쉽지 않았어요. LangGraph는 순환을 LLM 응용프로그램에 손쉽게 도입할 수 있도록 도와줍니다.

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

LangChain Expression 언어는 순환이나 루프를 설명하기에 적합하지 않지만, LangGraph를 사용하면 요소에 필요한 순환을 설명하고 소개할 수 있습니다.

# 시작하기 전에! 🦸🏻‍♀️

만약 이 주제를 좋아하시고 저를 지원하고 싶으시다면:

- 제 글을 50번 클랩해주세요; 그렇게 하면 저에게 큰 도움이 됩니다. 👏
- 저를 Medium에서 팔로우하고 최신 글을 무료로 받아보세요. 🫶
- 제 YouTube 채널을 팔로우해주세요.
- 저의 Discord에서 더 많은 정보를 얻을 수 있습니다.

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

우리의 애플리케이션을 시작하기 전에, 코드가 작동할 수 있는 이상적인 환경을 만들 것입니다. 이를 위해 필요한 Python 라이브러리를 설치해야 합니다. 먼저, 모델을 지원하는 라이브러리를 설치하는 것으로 시작하겠습니다. 아래 라이브러리들을 pip를 이용해 설치할 겁니다.

pip install streamlit
pip install langchainhub
pip install langgraph
pip install langchain_google_genai
pip install -U langchain-openai langchain

설치가 완료되면 Langchain, Langchain Google, Langchain Community, os, typing, Langchain Core, operator, Langchain Prebuilt, LangGraph, 그리고 Streamlit을 import할 것입니다.

from langchain import hub
from langchain.agents import Tool, create_react_agent
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_community.utilities import GoogleSerperAPIWrapper
import os
from typing import TypedDict, Annotated, Union
from langchain_core.agents import AgentAction, AgentFinish
from langchain_core.messages import BaseMessage
import operator
from typing import TypedDict, Annotated
from langchain_core.agents import AgentFinish
from langgraph.prebuilt.tool_executor import ToolExecutor
from langgraph.prebuilt import ToolInvocation
from langgraph.graph import END, StateGraph
from langchain_core.agents import AgentActionMessageLog
import streamlit as st

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

```python
st.set_page_config(page_title="LangChain Agent", layout="wide")
```

Let's begin by creating a function called main, which will be the central focus of our Streamlit web application. Set the title for the webpage and then create a text area where users can input text. To enhance the user experience, include a button on the page. When the user clicks this button, the code inside the corresponding if block will be executed.

```python
def main():
    # Streamlit UI elements
    st.title("LangGraph Agent + Gemini Pro + Custom Tool + Streamlit")

    # Input from user
    input_text = st.text_area("Enter your text:")

    if st.button("Run Agent"):
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

오랫동안 타로 카드에 대한 전문 지식을 쌓아오시면서 온라인에서 다른 나라의 토론에 참여하시는 것이 어때요? 시간을 찾아 카드의 의미와 해석에 대해 나누어 보세요.

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

다음으로 함수들을 만들어보겠습니다:

먼저: 우리는 알파벳 순으로 단어를 정렬하는 Sort 함수를 정의합니다.

둘째: 대소문자를 변환하는 case toggle 함수를 정의합니다.

```js
def toggle_case(word):
    toggled_word = ""
    for char in word:
        if char.islower():
            toggled_word += char.upper()
        elif char.isupper():
            toggled_word += char.lower()
        else:
            toggled_word += char
    return toggled_word

def sort_string(string):
    return ''.join(sorted(string))
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

지금까지 우리는 모든 사용자 정의 도구들을 정의했고 이제 그것들을 에이전트에 추가할 시간입니다.

우리의 도구 배열 안에는 도구 목록이 정의되어 있습니다. 검색, 정렬 및 글자 변환 도구는 매개변수가 하나뿐이므로 직접 정의할 수 있습니다. 도구의 이름, 함수 및 설명 속성을 사용했습니다. 여기에 도구의 이름, 에이전트가 사용하길 원할 때 호출될 함수, 그리고 그에 대한 설명이 정의되어 있습니다. 에이전트는 이 도구를 언제 사용해야 할지 판단하기 위해 이 설명을 읽습니다.

```js
tools = [
      Tool(
          name = "Search",
          func=search.run,
          description="useful for when you need to answer questions about current events",
      ),
      Tool(
          name = "Toogle_Case",
          func = lambda word: toggle_case(word),
          description = "use when you want covert the letter to uppercase or lowercase",
      ),
      Tool(
          name = "Sort String",
          func = lambda string: sort_string(string),
          description = "use when you want sort a string alphabetically",
      ),

        ]
```

- ChatGoogleGenerativeAI는 Gemini LLM을 사용하기 위해 사용되는 클래스입니다. 그런 다음 ChatGoogleGenerativeAI 클래스에 작업할 Gemini 모델을 전달하여 LLM 클래스를 생성합니다. 그 다음에는 ReAct 접근 방식을 사용하여 도구와 상호 작용할 수 있는 AI 시스템인 에이전트를 만듭니다.

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

먼저, 에이전트 내에서 상태를 유지하기 위한 클래스를 정의합니다. 에이전트 처리 중의 내부 상태는 이 클래스의 인스턴스에 유지됩니다. 나중에 LangGraph 내에서 이 클래스가 사용될 것입니다.

상태로 유지되는 정보는 다음과 같습니다.

- input: 사용자로부터의 입력 콘텐츠
- chat_history: 에이전트 실행 전의 대화 기록
- intermediate_steps: 에이전트 처리 중 중간 실행 세부사항 및 결과
- agent_outcome: 에이전트 응답 결과, AgentAction 또는 AgentFinish의 인스턴스가 저장됩니다. 응답 결과가 AgentFinish인 경우, 프로세스를 종료해야 하며, 그렇지 않은 경우에는 도구를 실행해야 합니다.

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

에이전트 상태 클래스를 정의합니다.

```js
class AgentState(TypedDict):
    input: str
    chat_history: list[BaseMessage]
    agent_outcome: Union[AgentAction, AgentFinish, None]
    return_direct: bool
    intermediate_steps: Annotated[list[tuple[AgentAction, str]], operator.add]
```

에이전트에 의해 실행될 프로세스를 정의합니다. 이러한 함수들은 그래프의 노드에 해당합니다.

여기서 정의된 노드에 대한 함수들은 다음과 같습니다.

- run_agent: 단계 4에서 정의된 에이전트 체인을 실행하는 프로세스

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

```python
 tool_executor = ToolExecutor(tools)

def run_agent(state):
    """
    # 중간 단계를 더 잘 관리하고 싶다면
    inputs = state.copy()
    if len(inputs['intermediate_steps']) > 5:
        inputs['intermediate_steps'] = inputs['intermediate_steps'][-5:]
    """
    agent_outcome = agent_runnable.invoke(state)
    return {"agent_outcome": agent_outcome}
```

저희는 execute_tools라는 함수를 정의했는데, 이 함수는 state라는 매개변수를 받습니다. 도구 이름과 매개변수를 가져오면, if 문은 tool_name이 "Search", "Sort" 또는 "Toggle_Case"인지 확인합니다. 만약 arguments에 return_direct가 있는 경우, 해당 항목을 삭제합니다.

그런 다음, 지정된 입력 값으로 도구를 호출하고 return 문을 준비합니다.

```python
def execute_tools(state):
    messages = [state['agent_outcome']]
    last_message = messages[-1]
    ######### 사용자와 상호 작용 ###########
    # 사용자 입력 y/n
    # 가장 최근의 agent_outcome 가져오기 - 이는 위의 `agent`에서 추가된 키입니다
    # state_action = state['agent_outcome']
    # human_key = input(f"[y/n] continue with: {state_action}?")
    # if human_key == "n":
    #     raise ValueError

    tool_name = last_message.tool
    arguments = last_message
    if tool_name == "Search" or tool_name == "Sort" or tool_name == "Toggle_Case":

        if "return_direct" in arguments:
            del arguments["return_direct"]
    action = ToolInvocation(
        tool=tool_name,
        tool_input=last_message.tool_input,
    )
    response = tool_executor.invoke(action)
    return {"intermediate_steps": [(state['agent_outcome'], response)]}
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

안녕하세요! 이제 전에 받은 응답 결과를 확인하여 에이전트 처리를 계속 할지 종료할지 결정하는 함수에 대해 이야기해볼게요.

```python
def should_continue(state):
    messages = [state['agent_outcome']]
    last_message = messages[-1]
    if "Action" not in last_message.log:
        return "end"
    else:
        arguments = state["return_direct"]
        if arguments is True:
            return "final"
        else:
            return "continue"
```

일반적으로는 이 정도면 충분하지만, 이번에 사용하고 있는 모델이 도구를 잘 활용하지 않는 것 같아요. 그래서 먼저 도구를 사용하도록 보장하려면 추가적인 노드 함수를 정의해야 해요.

```python
def first_agent(inputs):
    action = AgentActionMessageLog(
        tool="Search",
        tool_input=inputs["input"],
        log="",
        message_log=[]
    )
    return {"agent_outcome": action}
```

이렇게 하면 모델이 도구를 더 잘 활용할 수 있게 될 거예요. 혹시 궁금한 점이 있거나 더 도와드릴 게 있으면 언제든지 물어보세요! 🌟

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

에이전트 처리의 그래프를 정의했습니다. 그래프는 대략적으로 노드와 엣지로 구성됩니다. 위키백과의 그래프 이론을 살펴보면 더 잘 이해할 수 있습니다.

```js
 workflow = StateGraph(AgentState)

        workflow.add_node("agent", run_agent)
        workflow.add_node("action", execute_tools)
        workflow.add_node("final", execute_tools)
        # 특정 도구를 항상 먼저 호출하고 싶다면 주석을 해제하세요
        # workflow.add_node("first_agent", first_agent)
work

        workflow.set_entry_point("agent")
        # 특정 도구를 항상 먼저 호출하고 싶다면 주석을 해제하세요
        # workflow.set_entry_point("first_agent")

        workflow.add_conditional_edges(

            "agent",
            should_continue,

            {
                "continue": "action",
                "final": "final",
                "end": END
            }
        )


        workflow.add_edge('action', 'agent')
        workflow.add_edge('final', END)
        # 특정 도구를 항상 먼저 호출하고 싶다면 주석을 해제하세요
        # workflow.add_edge('first_agent', 'action')
        app = workflow.compile()
```

에이전트가 실행할 준비가 되었습니다. 지금까지 정의된 에이전트를 위해 그래프를 실행해보세요. 컴파일 후 LangChain Runnable 객체가 되므로 LangChain 표현 언어나 LCEL 체인과 동일하게 사용할 수 있습니다.

그럼 실행해 봅시다.

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

```python
inputs = {"input": input_text, "chat_history": [], "return_direct": False}
results = []
for s in app.stream(inputs):
    result = list(s.values())[0]
    results.append(result)
    st.write(result)  # Display each step's output
if __name__ == "__main__":
    main()
```

에이전트 처리 결과(그래프의 각 노드)는 순서대로 출력됩니다. 마지막 agent_outcome 줄의 내용이 얻은 최종 결과가 될 것입니다. 특히 실행 로그가 필요없으면, 호출하여 마지막 결과만 얻을 수 있습니다. 다른 쿼리를 실행하고 최종 결과만 인쇄해 봅시다.

```python
result = app.invoke({"input": input_text, "chat_history": [], "return_direct": False})
print(result["agent_outcome"].return_values["output"])
```

어쩌면 코드 결과를 확인하고 싶다면 제 비디오를 확인해보세요.

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

# 결론 :

LangGraph를 사용한 에이전트 구현이었습니다. 이 실행 예에서는 프로세스가 순환되지 않기 때문에 LangGraph를 사용하는 것에는 그다지 의미가 없습니다. 그럼에도 불구하고, LLM 응답 → 도구 실행 → 결과를 사용한 LLM 응답 → 다른 도구를 실행하는 에이전트를 구축할 때 매우 강력한 프레임워크가 될 것이라고 생각합니다.

📚 다른 기사들도 확인해보세요:
