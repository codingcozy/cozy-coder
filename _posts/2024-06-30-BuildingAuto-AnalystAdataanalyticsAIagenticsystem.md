---
title: "최신 데이터 분석 AI 시스템 Auto-Analyst 구축 방법"
description: ""
coverImage: "/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_0.png"
date: 2024-06-30 23:59
ogImage: 
  url: /assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_0.png
tag: Tech
originalTitle: "Building “Auto-Analyst” — A data analytics AI agentic system"
link: "https://medium.com/firebird-technologies/building-auto-analyst-a-data-analytics-ai-agentic-system-3ac2573dcaf0"
---


## AI ‘Auto-Analyst’를 만드는 기술 가이드

![BuildingAuto-AnalystAdataanalyticsAIagenticsystem](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_0.png)

나는 데이터 과학자/분석가로서의 업무 부담을 줄이기 위해 AI 기반 에이전트를 개발해왔습니다. 대중문화에서는 종종 AI가 인간의 일자리를 대체하는 것을 보여주지만, 현실에서는 대부분의 AI 에이전트가 인간의 대체품이 아닙니다. 대신 우리가 더 효율적으로 일할 수 있도록 도와줍니다. 이번 에이전트는 정확히 그 목적으로 디자인되었습니다. 이전에는 자연어 입력만을 사용하여 시각화를 더 빨리 만들 수 있도록 도와주는 데이터 시각화 에이전트를 디자인한 적이 있습니다.

## 디자인

<div class="content-ad"></div>

![Flow Diagram](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_1.png)

This flow diagram demonstrates a system that begins with a user-defined goal. The planner agent then assigns tasks to a group of worker agents, each responsible for generating code to solve a specific part of the problem. Eventually, all the pieces of code produced by individual worker agents are collected and combined by a code combiner agent, creating a unified script that achieves the overall objective.

Please keep in mind that the planner agent may assign tasks to only some of the worker agents, not necessarily to all of them. Additionally, each agent will have its own unique set of inputs, even though they are not depicted in the diagram.

## Components of the system

<div class="content-ad"></div>

이 블로그 게시물에서는 각각의 구성 요소에 대한 코드 블록을 제공하면서 에이전트를 직접 구축하는 단계별 안내를 제공할 것입니다. 다음 섹션에서는 이러한 부분들이 완벽하게 통합되는 방법을 시연할 것입니다.

## Planner Agent

플래너 에이전트는 사용자가 정의한 목표, 사용 가능한 데이터셋 및 에이전트 설명을 세 가지 입력으로 받습니다. 이는 다음 형식으로 계획을 출력합니다:

Agent1-` Agent2-` Agent3....

<div class="content-ad"></div>


# 다른 orchestration 라이브러리를 사용할 수 있지만 DSPy를 사용하여 빠르고 간편하게 응용프로그램을 구축하고 평가하는 데 좋았습니다.

import dspy

# 이 객체는 dspy.Signature 클래스를 상속받습니다.
# """ 안의 텍스트는 프롬프트입니다.
class analytical_planner(dspy.Signature):
    """ 데이터 분석 플래너 에이전트입니다. 세 가지 입력에 액세스할 수 있습니다.
    1. 데이터셋
    2. 데이터 에이전트 설명
    3. 사용자 정의 목표
    이 세 가지 입력을 사용하여 데이터 및 사용 가능한 에이전트로부터 사용자 정의 목표를 달성하기 위한 포괄적인 계획을 개발합니다.
    사용자 정의 목표가 실현 불가능하다고 생각되면 사용자에게 목표를 다시 정의하거나 설명을 추가할 것을 요청할 수 있습니다.

    다음 형식으로 결과를 제공하십시오:
    plan: Agent1->Agent2->Agent3
    plan_desc = 이유로 인해 Agent 1을 사용한 후, Agent 2를 사용하고 마지막으로 Agent 3을 사용합니다.

    쿼리의 응답으로 모든 에이전트를 사용할 필요는 없습니다.
    """
    
    # 입력 필드와 그 설명
    dataset = dspy.InputField(desc= "시스템에 로드된 사용 가능한 데이터셋, 이 df_name,columns 사용하여 df를 df_name의 사본으로 설정합니다.")
    Agent_desc = dspy.InputField(desc="시스템에 있는 에이전트들")
    goal = dspy.InputField(desc="사용자가 정의한 목표")
    
    # 출력 필드와 해당 설명
    plan = dspy.OutputField(desc="사용자가 정의한 목표를 달성하는 계획")
    plan_desc = dspy.OutputField(desc="선택된 계획 뒤에 있는 이유")


![Building Auto Analyst Agent AI System](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_2.png)

## 분석 에이전트

대부분의 분석 에이전트들은 프롬프트에 약간의 차이가 있는 일반적인 구조를 공유합니다. 사용자가 정의한 목표와 데이터세트 인덱스를 받습니다. 분석 코드와 설명의 두 출력을 생성하는데, 이는 디버깅에 유용하거나 에이전트를 재지정하는 데 도움이 될 수 있습니다.


<div class="content-ad"></div>

```yaml
# 중계 요원은 중간 계층에 있는 에이전트로 정의됩니다
# 그들은 특정 데이터 분석 작업을 위한 코드를 생성합니다
class 데이터전처리에이전트(dspy.Signature):
    """ 데이터 전처리 에이전트로, 사용자가 정의한 목표와 사용 가능한 데이터 집합을 가져와서
    탐색적 데이터 분석 파이프라인을 작성합니다. 이를 수행하려면 필요한 파이썬 코드를 출력합니다.
    numpy와 pandas만 사용하여 전처리 및 입문 분석을 수행할 것입니다.

    """
    dataset = dspy.InputField(desc="시스템에 로드된 사용 가능한 데이터 세트, 이 df_name, columns을 사용하여 df를 df_name의 복사본으로 설정")
    goal = dspy.InputField(desc="사용자가 정의한 목표 ")
    commentary = dspy.OutputField(desc="수행 중인 분석에 대한 주석")
    code = dspy.OutputField(desc="데이터 전처리 및 입문 분석을 수행하는 코드")

class 통계분석에이전트(dspy.Signature):
    """ 통계 분석 에이전트로,
    데이터 집합과 사용자가 정의한 목표를 가져와 해당 목표를 달성하기 위해
    적절한 통계 분석을 수행하는 파이썬 코드를 출력합니다.
    Python statsmodel 라이브러리를 사용해야 합니다 """
    dataset = dspy.InputField(desc="시스템에 로드된 사용 가능한 데이터 세트, 이 df_name, columns을 사용하여 df를 df_name의 복사본으로 설정")
    goal = dspy.InputField(desc="분석을 수행할 사용자가 정의한 목표")
    commentary = dspy.OutputField(desc="수행 중인 분석에 대한 주석")
    code = dspy.OutputField(desc="statsmodel을 사용하여 통계 분석을 수행하는 코드")

class sk_learn에이전트(dspy.Signature):
    """머신 러닝 에이전트로,  
    데이터 집합과 사용자가 정의한 목표를 가져와 해당 목표를 달성하기 위해 
    적절한 기계 학습 분석을 수행하는 파이썬 코드를 출력합니다.
    scikit-learn 라이브러리를 사용해야 합니다."""
    dataset = dspy.InputField(desc="시스템에 로드된 사용 가능한 데이터 세트, 이 df_name, columns를 사용하여 df를 df_name의 복사본으로 설정")
    goal = dspy.InputField(desc="사용자가 정의한 목표 ")
    commentary = dspy.OutputField(desc="수행 중인 분석에 대한 주석")
    code = dspy.OutputField(desc="탐색적 데이터 분석을 수행하는 코드")

## 데이터 시각화 에이전트를 작업하여 이미 DSPy를 사용하여 최적화했습니다.
## 유일한 큰 차이점은 요 에이전트가 스타일링 지수의 추가 입력을 받는다는 것입니다
```

![이미지](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_3.png)

## 코드 결합 에이전트

이 에이전트의 목적은 모든 에이전트의 출력물을 하나의 일관된 스크립트로 정리하는 것입니다. 긴 문자열의 코드 목록을 입력으로 받아 코드를 출력합니다.

<div class="content-ad"></div>

```python
class code_combiner_agent(dspy.Signature):
    """안녕하세요! 여러분은 코드 합산 에이전트입니다. 많은 에이전트들로부터 받은 Python 코드 출력을 하나로 합치는 작업을 맡고 있으며, 코드에서 발생한 오류도 수정해 드립니다.
    에이전트 코드 목록 = dspy.InputField(desc="각 에이전트가 제공한 코드 목록")
    정제된_완전한_코드 = dspy.OutputField(desc="정제된 완전한 코드 베이스")
```

## 선택적인 에이전트/인덱스

에이전트가 더 원할하게 작동하고 오류를 잡기 위해 다음과 같은 추가적인 에이전트나 인덱스도 만들었습니다.

```python
# 데이터 시각화 에이전트 게시물에서 사용한 동일한 시그니처
class Data_Viz(dspy.Signature):
    """
    데이터 시각화를 생성하는 AI 에이전트입니다. Plotly를 사용하여 데이터 시각화를 생성하는 것이 목표입니다.
    사용 가능한 도구들을 활용해야 합니다
    {dataframe_index}
    {styling_index}
    
    사용자가 원하는 데이터와 차트에 대한 정보를 포함하는 사용자 정의 목표를 사용해야 합니다.
    데이터 프레임 내의 관련 열이 없는 경우 관련 정보가 없다고 명시해야 합니다.
    """
    goal = dspy.InputField(desc="사용자가 정의한 데이터와 차트를 나타내는 정보를 포함하는 목표")
    dataframe_context = dspy.InputField(desc="데이터 프레임 내 데이터에 관한 정보를 제공합니다. 열 이름과 데이터프레임 이름만 사용하면 됩니다.")
    styling_context = dspy.InputField(desc="Plotly 그림을 어떻게 스타일링할지에 대한 지시를 제공합니다.")
    code = dspy.OutputField(desc="사용자의 쿼리 및 데이터프레임 인덱스 및 스타일링 콘텍스트에 따라 필요한 시각화를 시각화하는 Plotly 코드")
    
# 사용자가 정의한 목표가 잘 작동하는지 확인하는 선택적인 에이전트
class goal_refiner_agent(dspy.Signature):
    """AI 데이터 분석가 플래너 에이전트에게 제공된 사용자 정의 목표를 받아서, 시스템에 로드된 데이터셋과 에이전트 설명을 활용하여 목표를 보다 상세하게 만들어 줍니다."""
    dataset = dspy.InputField(desc="시스템에 로드된 사용 가능한 데이터셋, 데이터프레임 이름과 열로 df를 설정하세요.")
    Agent_desc = dspy.InputField(desc="시스템에 있는 사용 가능한 에이전트들")
    goal = dspy.InputField(desc="사용자가 정의한 목표")
    refined_goal = dspy.OutputField(desc="플래너 에이전트가 더 나은 계획을 세울 수 있도록 도와주는 세분화된 목표")
```

<div class="content-ad"></div>

이곳에서 데이터 세트 전체에 대한 정보를 제공하는 대신, 데이터 사용 가능한 정보를 입력하는 리트리버를 구축했습니다.

```js
# 저는 더 편리했던 LLama-Index 기반 리트리버를 선택했습니다.
# 기본적으로 데이터를 여러 가지 방식으로 입력할 수 있습니다.
# 열 이름에 대한 설명, 데이터프레임 참조를 제공하고
# 데이터 수집 목적 등을 설명할 수 있습니다.
dataframe_index = VectorStoreIndex.from_documents(docs)

# 또한 데이터 시각화 에이전트를 위한 스타일링 인덱스를 정의했습니다.
# 다양한 시각화를 어떻게 스타일링할지에 대한 자연어 지침을 포함하고 있습니다.
style_index = VectorStoreIndex.from_documents(styling_instructions)
```

# 모든 것을 하나의 시스템으로 통합

DSPy에서 복잡한 LLM 애플리케이션을 컴파일하려면 두 가지 필수 메소드 __init__ 및 forward를 정의해야합니다.

<div class="content-ad"></div>

_init_ 메서드는 모듈을 초기화하여 사용될 모든 변수를 정의합니다. 그러나 핵심 기능이 구현되는 곳은 forward 메서드입니다. 이 메서드는 한 구성 요소의 출력이 다른 구성 요소와 상호작용하는 방식을 개요로 제공하여 응용 프로그램의 논리를 효과적으로 구동합니다.

```python
# 이 모듈은 시작 시 하나의 입력만 사용합니다
class auto_analyst(dspy.Module):
    def __init__(self, agents):
        # 사용 가능한 에이전트, 그들의 입력 및 설명을 정의합니다
        self.agents = {}
        self.agent_inputs = {}
        self.agent_desc = []
        i = 0
        for a in agents:
            name = a.__pydantic_core_schema__['schema']['model_name']
            # CoT 프롬프팅 사용 - 경험상 더 나은 응답을 생성하는 데 도움이 됨
            self.agents[name] = dspy.ChainOfThought(a)
            agent_inputs[name] = {x.strip() for x in str(agents[i].__pydantic_core_schema__['cls']).split('->')[0].split('(')[1].split(',')}
            self.agent_desc.append(str(a.__pydantic_core_schema__['cls']))
            i += 1
        # planner, refine_goal 및 code combiner 에이전트를 따로 정의
        # 코드 및 분석을 생성하지 않고 계획 수립, 목표 개선, 코드 결합 지원
        self.planner = dspy.ChainOfThought(analytical_planner)
        self.refine_goal = dspy.ChainOfThought(goal_refiner_agent)
        self.code_combiner_agent = dspy.ChainOfThought(code_combiner_agent)
        # 이 두 리트리버는 llama-index 리트리버를 사용하여 정의되며
        # 에이전트를 원하는 대로 사용자 정의할 수 있습니다
        self.dataset = dataframe_index.as_retriever(k=1)
        self.styling_index = style_index.as_retriever(similarity_top_k=1)

    def forward(self, query):
        # 에이전트 입력 인수를 빠르게 전달하기 위해 사용되는 dict_
        dict_ = {}
        # 쿼리에 관련된 컨텍스트 검색
        dict_['dataset'] = self.dataset.retrieve(query)[0].text
        dict_['styling_index'] = self.styling_index.retrieve(query)[0].text
        dict_['goal'] = query
        dict_['Agent_desc'] = str(self.agent_desc)
        # 모든 에이전트 출력을 저장하는 output_dictionary
        output_dict = {}
        # 계획 수립
        plan = self.planner(goal=dict_['goal'], dataset=dict_['dataset'], Agent_desc=dict_['Agent_desc'])
        output_dict['analytical_planner'] = plan
        plan_list = []
        code_list = []
        # 계획이 예상대로 작동하면 에이전트가 ->로 분리됩니다
        if plan.plan.split('->'):
            plan_list = plan.plan.split('->')
        # 목표가 모호한 경우, refined goal 에이전트에 전송
        else:
            refined_goal = self.refine_goal(dataset=data, goal=goal, Agent_desc=self.agent_desc)
            forward(query=refined_goal)
        # 목표 및 다른 입력을 계획의 모든 해당 에이전트에 전달
        for p in plan_list:
            inputs = {x: dict_[x] for x in agent_inputs[p.strip()]}
            output_dict[p.strip()] = self.agents[p.strip()](**inputs)
            # 생성된 모든 코드를 하나의 스크립트로 결합하기 위한 코드 목록
            code_list.append(output_dict[p.strip()].code)
        # 마지막 출력 저장
        output_dict['code_combiner_agent'] = self.code_combiner_agent(agent_code_list=str(code_list))
        return output_dict

# 사용 가능한 모든 에이전트 서명을 목록으로 저장할 수 있습니다
agents = [preprocessing_agent, statistical_analytics_agent, sk_learn_agent, data_viz_agent]

# 에이전트 시스템을 정의합니다
auto_analyst_system = auto_analyst(agents)

# 시스템에 시카고 범죄 데이터를 프리로드합니다
goal = "시카고의 범죄 원인은 무엇인가요?"

# 이 쿼리에 대한 분석을 수행하도록 에이전트 시스템에 요청합니다
output = auto_analyst_system(query=goal)
```

이제 쿼리 결과를 단계별로 살펴보겠습니다.

이 쿼리 = '시카고의 범죄 원인은 무엇인가요?'

<div class="content-ad"></div>

The images show the progress of your project. The first one showcases the preprocessing agent, taking the initial steps to bring your plan to life. And in the next image, the statistical analysis agent takes over, moving your project forward with data-driven insights. Keep up the good work! 🌟


![BuildingAuto_AnalystAdataanalyticsAIagenticsystem_4.png](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_4.png)

Executing the plan, first preprocessing agent

![BuildingAuto_AnalystAdataanalyticsAIagenticsystem_5.png](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_5.png)

Next statistical analysis agent


<div class="content-ad"></div>

![BuildingAuto-AnalystAdataanalyticsAIagenticsystem_6.png](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_6.png)

Next is the Plotly data visualization agent

![BuildingAuto-AnalystAdataanalyticsAIagenticsystem_7.png](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_7.png)

And finally, the code combiner agent, to bring it all together

<div class="content-ad"></div>

마법사카드를 이야기하는 건가요? 라스트번 에이전트 코드를 실행한 결과물을 보여드릴게요.

![이미지 8](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_8.png)

마지막 에이전트 코드 실행 후에 나온 결과물이에요.

![이미지 9](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_9.png)

![이미지 10](/assets/img/2024-06-30-BuildingAuto-AnalystAdataanalyticsAIagenticsystem_10.png)

<div class="content-ad"></div>

# 한계사항

많은 에이전트들처럼, 의도한 대로 작동할 때 우수한 성과를 보입니다. 이 프로젝트의 첫 번째 이터레이션에 불과하며 시간이 흐름에 따라 개선할 계획입니다. 업데이트를 받기 위해 제게와 FireBird Technologies를 팔로우해 주세요. 현재의 한계사항은 다음과 같습니다:

- 환각: 때로 에이전트는 환각으로 실행할 수 없는 코드를 생성합니다.
- 신뢰성/일관성 부족: 에이전트의 출력물은 일관성이 없으며, 동일한 쿼리의 다른 변형은 매우 다른 코드로 나타납니다.
- 혼합된 출력물: 많은 에이전트들이 문제의 서로 다른 측면을 독점적으로 처리하지 않습니다. 예를 들어, 데이터 전처리 에이전트는 자체 시각화를 생성하며, 데이터 시각화 에이전트도 자체 시각화를 생성합니다.

# 다음 단계

<div class="content-ad"></div>

이 프로젝트는 계속 진행 중이에요. 다음으로 이 프로젝트를 개선하기 위해 예상되는 단계들을 살펴볼게요:

- 시그니처/프롬프트 최적화: DSPy는 LLM 애플리케이션을 평가하기 위해 설계되었어요. 이것은 단순히 구현일 뿐이에요. 다음에는 최상의 접두사, 시그니처, 그리고 프롬프트를 찾는 것이 중요할 거예요.
- 가드레일 추가: 에이전트가 생성한 코드를 자동 수정하는 것은 많은 다른 에이전시 시스템에서 사용되는 해결책이에요. 또한 프롬프트 삽입 공격을 제약하는 것도 로드맵에 있어요.
- 메모리/상호작용 추가: 이 에이전트는 한 번에 모든 것을 수행해요. 또한 개별 구성요소 간에 서로의 출력을 확인하는 상호작용이 없어요.
- UI 구축: 지금은 추가 테스트를 위해 에이전트 백엔드만 구축했어요. 사용자 의견을 수용하고 더 많은 피드백을 받기 위해 UI를 구축할 거예요.

읽어 주셔서 감사해요!

제글과 FireBird Technologies를 팔로우해 주세요.