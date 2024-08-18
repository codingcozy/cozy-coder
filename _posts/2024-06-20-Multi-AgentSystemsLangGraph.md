---
title: "다중 에이전트 시스템    LangGraph"
description: ""
coverImage: "/assets/img/2024-06-20-Multi-AgentSystemsLangGraph_0.png"
date: 2024-06-20 18:42
ogImage:
  url: /assets/img/2024-06-20-Multi-AgentSystemsLangGraph_0.png
tag: Tech
originalTitle: "Multi-Agent Systems   LangGraph"
link: "https://medium.com/@minekayaa/multi-agent-systems-langgraph-63c1abb3e242"
isUpdated: true
---

응, 맞아, Smiths가 여기 있어! 내 이전 게시물이 꽤 오래되었네. 처음 보는 사람이라면, 안녕. 이 게시물에서는 LangGraph에 대해 이야기하고, LangSmith에 대해서도 조금 언급하고 싶어. 최근에 우리는 에이전트 감독자를 구현하기 시작했어; 이것은 다중 에이전트 시스템을 구현하는 방법이야.

그러나 너무 기술적으로 들어가기 전에, 잠시만 기다려 줄 수 있을까? 이것은 매트릭스의 Smith 에이전트야. 매트릭스에서 '질서'를 유지하기 위해 만들어진 코드 일부였다는 걸 기억해봐, 사람들을 시뮬레이션에 유지하는 시스템 내에서. (시스템이 붕괴해도, 네오에게 한 큰 박수). Smiths는 계층 구조였지; 다른 Smiths에게 일을 시키는 'the Smith'가 있었어. (그는 나중에 시뮬레이션 내에서 자신의 존재를 복제할 수 있었지. 사실, 많은 요청이 있는 시스템을 다루는 매우 멋진 방법이라고 할 수 있어, 아마 LangChain의 다음 움직임이 될지도 ;))

![Smith Image](/assets/img/2024-06-20-Multi-AgentSystemsLangGraph_0.png)

내 농담 세션 이후, LangGraph는 정말 멋진 라이브러리야; LangChain이 감당할 수 없는 경우에 매우 유용해. 복잡한 문제, 사용 사례 또는 흐름을 나누는 해결책을 제공해줘. 이전에 언급한 대로, 다중 에이전트 시스템에 대해 이야기할 테니, 주로 감독자 구현에 대해 이야기할 거야, 왜냐하면 이러한 에이전트를 결합하는 다양한 방법이 있거든. 그리고 우리가 챗봇을 구현하려고 노력하고 있기 때문에, 고객 지원 봇 튜토리얼에서 많은 도움을 받았어. 채팅에서 무엇을 할 수 있고, 에이전트를 제어하여 올바른 일을 수행하는 방법에 대해 매우 명확한 아이디어를 제공해줘.

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

# LangGraph 소개

LangGraph는 LangChain 위에 구축되어 있으며 LangChain 생태계와 완전히 호환됩니다. 그것은 기본적으로 그래프 기반 상태 머신을 사용하여 복잡하고 확장 가능한 AI 에이전트를 구축하는 Python 라이브러리입니다. LangChain을 시도해 본 적이 있다면, 에이전트를 프로덕션 환경에서 실행하려고 할 때 그 부족함을 느낄 것입니다. 프로덕션에서는 종종 더 많은 제어가 필요합니다. 특정 도구를 항상 호출하도록 강제하고 싶을 수 있습니다. 도구를 호출하는 방법을 더 많이 제어하고 싶을 수 있습니다. 상태에 따라 에이전트에 대한 서로 다른 프롬프트를 사용하고 싶을 수 있습니다.

그렇다면, 이 “상태 머신”이란 무엇일까요? 이를 통해 인간 상호작용을 순환하며 LLM(Lang Language Model)을 통해 작업을 수행할 수 있는 권한을 얻게 됩니다. 이는 어떤 에이전트가 실행되었는지, 어떤 도구를 사용했는지, 그리고 원한다면 메모리까지 추적합니다. 현재 메모리에 대해 자세히 알아볼 필요는 없지만, LangChain에서 본 것과는 조금 다릅니다. Checkpointer는 상태를 영속화하여 에이전트에게 "메모리"를 제공합니다.

## StateGraph

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

StateGraph은 그래프를 나타내는 클래스입니다. 상태 정의를 전달하여이 클래스를 초기화합니다. 그래프 내의 노드가 상태를 업데이트하고, 이는 키-값 저장소 형식의 작업을 반환합니다.

```js
from langgraph.graph import StateGraph
from typing import TypedDict, List, Annotated
import Operator
from langchain_core.messages import BaseMessage

class State(TypedDict):
    input: str
    messages: Annotated[Sequence[BaseMessage], operator.add]

graph = StateGraph(State)
```

## 노드

StateGraph를 만든 후 graph.add_node(name, value) 구문을 사용하여 노드를 추가합니다. value 매개변수는 호출 될 함수 또는 LCEL 실행 가능이어야 합니다. (즉 실행 가능한 도구 또는 LLM)

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

```js
graph.add_node("model", model);
graph.add_node("tools", tool_executor);
```

그래프를 순환할 것이기 때문에 프로세스 중 어딘가에서 종료하는 것이 중요합니다. 그래프의 끝을 나타내는 END 노드를 사용하세요.

```js
from langgraph.graph import END

graph.add_node("end", END)
```

## 엣지

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

노드를 추가한 후에 그래프를 만들기 위해 엣지를 추가할 수 있습니다. 현재는 세 가지 유형의 엣지가 있습니다:

1 - 시작 엣지: 이 엣지는 그래프의 시작점을 특정 노드에 연결하는 엣지입니다. 아래 코드는 우리의 그래프가 'model' 노드에서 시작한다는 것을 의미합니다.

```js
graph.set_entry_point("model");
```

2 - 일반 엣지: 이러한 엣지는 한 노드가 항상 다른 노드 뒤에 호출되도록합니다. 아래 코드는 'tools' 노드를 호출할 때 'model' 노드가 항상 그 뒤에 호출된다는 것을 의미합니다.

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

```js
graph.add_edge("tools", "model");
```

3 - 조건부 엣지: 이는 LLM이 첫 번째로 이동할 노드를 결정하는 데 사용하는 엣지입니다. 엄격히 어디로 이동할지를 지정하지 않습니다. LLM은 상태와 사용자 입력을 확인하여 목적지를 결정합니다.

조건부 엣지에는 세 가지 매개변수가 있습니다. 첫 번째 매개변수는 다음에 할 일을 결정할 노드입니다. 두 번째 매개변수는 다음으로 호출할 노드를 결정하는 함수입니다. 세 번째 매개변수는 함수(2)가 반환할 수 있는 가능한 값이어야 합니다. 그리고 값은 이동할 노드의 이름이어야 합니다.

```js
graph.add_conditional_edge("model", should_continue, {
  end: END,
  continue: "tools",
});
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

## 컴파일

우리가 그래프를 정의한 후, 그것을 실행 가능한 형태로 컴파일할 수 있습니다. 이 실행 가능한 형태는 LangChain 러너블과 똑같은 메소드를 가지고 있습니다 (.invoke, .stream, .astream_log 등).

```js
app = graph.compile();
```

# Multi-Agent Systems

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

단일 에이전트는 순차적으로 실행해야 할 너무 많은 도구가 있을 때 실패할 수 있습니다. 그래서 다중 에이전트 시스템에서는 문제를 분할하여 각 단계를 다른 에이전트로 정복하고 적절한 전문가에게 업무를 라우팅합니다.

## 에이전트 감독

에이전트 그룹을 만들 것입니다. 각 에이전트는 작업을 완료하는 데 필요한 특정 도구를 갖게 됩니다. 에이전트 감독은 작업을 위임하는 데 도움을 줄 것입니다.

![에이전트 감독](/assets/img/2024-06-20-Multi-AgentSystemsLangGraph_1.png)

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

이 예제에서는 2명의 에이전트와 1명의 감독관이 있습니다. 첫 번째 에이전트는 무작위 숫자를 생성하고, 다른 에이전트는 해당 무작위 숫자에 대한 다이어그램을 그립니다. 기대한 대로, 감독관이 작업을 위임하며, 무작위 숫자 생성 에이전트가 작업을 마치면 다른 에이전트에게 바퀴를 넘깁니다.

우리는 기초를 정의하며 시작합니다.

```js
from langchain_openai import ChatOpenAI
from typing import Annotated, List, Tuple, Union
from langchain.tools import BaseTool, StructuredTool, Tool
from langchain_experimental.tools import PythonREPLTool
from langchain_core.tools import tool
import random


#Model
llm = ChatOpenAI(model="gpt-3.5-turbo")

#Tools

#다이어그램 그리기용
python_repl_tool = PythonREPLTool()

#무작위 숫자 생성용
@tool("random_number", return_direct=False)
def random_number(input:str) -> str:
    """0-100 사이의 무작위 숫자를 반환합니다. 'random'이라는 단어를 입력하세요."""
    return random.randint(0, 100)

tools = [random_number,python_repl_tool]
```

도우미 함수로 계속하세요.

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

```js
from langchain.agents import AgentExecutor, create_openai_tools_agent
from langchain_core.messages import BaseMessage, HumanMessage
from langchain_openai import ChatOpenAI

# 주어진 도구와 프롬프트로 AgentExecutor를 반환하는 함수
def create_agent(llm: ChatOpenAI, tools: list, system_prompt: str):
    prompt = ChatPromptTemplate.from_messages(
        [
            (
                "system",
                system_prompt,
            ),
            MessagesPlaceholder(variable_name="messages"),
            MessagesPlaceholder(variable_name="agent_scratchpad"),
        ]
    )
    agent = create_openai_tools_agent(llm, tools, prompt)
    executor = AgentExecutor(agent=agent, tools=tools)
    return executor

# 에이전트 노드, 그래프에서 에이전트를 호출하는 데 사용할 함수
def agent_node(state, agent, name):
    result = agent.invoke(state)
    return {"messages": [HumanMessage(content=result["output"], name=name)]}
```

이제 그래프를 만들어 이 2개의 에이전트를 노드로 추가하겠습니다 :

```js
import operator
from typing import Annotated, Any, Dict, List, Optional, Sequence, TypedDict
import functools
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langgraph.graph import StateGraph, END

# 난수 생성기를 노드로 설정
random_agent = create_agent(llm, [random_number], "You get random numbers")
random_node = functools.partial(agent_node, agent=random_agent, name="Random_Number_Generator")

# 코더를 노드로 설정
code_agent = create_agent(llm, [python_repl_tool], "You generate charts using matplotlib.")
code_node = functools.partial(agent_node, agent=code_agent, name="Coder")
```

이제 슈퍼바이저를 생성해 봅시다!

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

```js
from langchain.output_parsers.openai_functions import JsonOutputFunctionsParser
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder

members = ["Random_Number_Generator", "Coder"]
system_prompt = (
    "안녕하세요! 대화를 관리하는 감독관으로 지정되었습니다. {members}와(과) 같은 작업자들 간의 대화 관리를 맡았습니다. 아래 사용자 요청이 제시된 경우, 다음에 행동할 작업자를 응답하세요. 각 작업자는 작업을 수행하고 결과 및 상태에 회신합니다. 작업이 완료되면 FINISH로 회신해주세요."
)
# 다음 작업자 노드를 선택하거나 처리를 종료하기 위해 함수 호출을 사용합니다.
options = ["FINISH"] + members
# openai 함수 호출
function_def = {
    "name": "route",
    "description": "다음 역할을 선택합니다.",
    "parameters": {
        "title": "routeSchema",
        "type": "object",
        "properties": {
            "next": {
                "title": "Next",
                "anyOf": [
                    {"enum": options},
                ],
            }
        },
        "required": ["next"],
    },
}
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", system_prompt),
        MessagesPlaceholder(variable_name="messages"),
        (
            "system",
            "위 대화를 참고하여, 다음에 누가 행동해야 할까요? 아니면 FINISH로 종료해야 할까요? 다음 중 하나를 선택해주세요: {options}",
        ),
    ]
).partial(options=str(options), members=", ".join(members))


# 라우팅 함수 및 시스템 프롬프트와 결합된 LLM으로 체인 생성
supervisor_chain = (
    prompt
    | llm.bind_functions(functions=[function_def], function_call="route")
    | JsonOutputFunctionsParser()
)
```

그래프를 생성해봅시다! (코멘트를 참고해주세요)

먼저 상태를 정의하고 에이전트 노드와 감독관 노드를 추가합니다.

```js
# 메시지를 보유하고 어디로 이동할지를 나타내는 AgentState 정의
class AgentState(TypedDict):
    messages: Annotated[Sequence[BaseMessage], operator.add]
    # 'next' 필드는 다음으로 라우팅할 위치를 나타냅니다
    next: str

# 상태 그래프(StateGraph) 정의
workflow = StateGraph(AgentState)

# 에이전트를 노드로 추가, 감독관 체인을 노드로 추가
workflow.add_node("Random_Number_Generator", random_node)
workflow.add_node("Coder", code_node)
workflow.add_node("Supervisor", supervisor_chain)

# 에이전트가 작업을 완료하면 다음은 항상 감독관이어야 합니다
workflow.add_edge("Random_Number_Generator", "supervisor")
workflow.add_edge("Coder", "supervisor")

# 감독관이 그래프 상태의 "next" 필드를 결정하여 노드 또는 종료로 라우팅합니다. (위의 END 특수 노드를 기억하세요)
workflow.add_conditional_edges(
    "supervisor",
    lambda x: x["next"],
    {
        "Random_Number_Generator": "Random_Number_Generator",
        "Coder": "Coder",
        "FINISH": END
    })

# 시작점은 항상 감독관이어야 합니다
workflow.set_entry_point("supervisor")

graph = workflow.compile()
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

한 번 해 보세요, 그래프를 스트리밍하거나 직접 실행할 수 있습니다.

```js
for s in graph.stream(
    {
        "messages": [
            HumanMessage(content="10개의 무작위 숫자를 가져와 히스토그램을 생성합니다.")
        ]
    }, config={"recursion_limit": 20}
):
    if "__end__" not in s:
        print(s)
        print("----")
```

<img src="/assets/img/2024-06-20-Multi-AgentSystemsLangGraph_2.png" />

출력에서 보듯이, 우리는 선언된 Supervisor로 시작합니다. Supervisor는 우리를 Random_Number_Generator로 경로 설정합니다. Random_Number_Generator가 작업을 완료한 후에는 엣지를 추가했기 때문에 Supervisor로 돌아갑니다. 그런 다음 Supervisor는 Coder로 경로를 설정하며 Coder가 완료되어 Supervisor로 돌아옵니다. 작업이 완료되면 Supervisor가 처리를 완료합니다.

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

🥳

# LangSmith

LangSmith은 LLM 애플리케이션 개발, 모니터링 및 테스트를 위한 플랫폼입니다. 저는 주로 모니터링에 사용하고 있어서 해당 측면만 언급할 예정이에요. LangSmith 추적을 활성화하면 LLM을 디버깅할 수 있어요.

문서를 보려면 여기를 클릭하세요.

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

위에서 실행한 예제는 아래와 같이 보입니다:

![image1](/assets/img/2024-06-20-Multi-AgentSystemsLangGraph_3.png)

더 자세한 정보를 원하신다면:

![image2](/assets/img/2024-06-20-Multi-AgentSystemsLangGraph_4.png)

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

많은 도구를 가진 에이전트를 사용할 때 매우 유용합니다. 우리 경우에는 하나의 도구만 가지고 있었지만, 문제가 더 복잡해지고 내부에서 무슨 일이 일어나고 있는지 이해하고 싶을 때, LangSmith가 당신이 그 과정을 따라가는 데 정말 도움이 될 것입니다. 디버그 콘솔로도 할 수 있지만, 왜 이 도구가 있는데 불편하게 해야 하나요?

환경 변수를 추가하여 추적 기능을 활성화할 수 있습니다:

```js
os.environ["LANGCHAIN_TRACING_V2"] = "true";
```

추가 링크:

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

- [https://blog.langchain.dev/langgraph/](https://blog.langchain.dev/langgraph/)
- [https://langchain-ai.github.io/langgraph/](https://langchain-ai.github.io/langgraph/)

즐겁게 즐겼다면 좋겠어요! 다음에 또 만나요 :)
