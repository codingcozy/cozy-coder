---
title: "자율 주행에서 감지 엔지니어를 위한 플래닝 크래시 코스"
description: ""
coverImage: "/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_0.png"
date: 2024-06-30 23:46
ogImage: 
  url: /assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_0.png
tag: Tech
originalTitle: "A Crash Course of Planning for Perception Engineers in Autonomous Driving"
link: "https://medium.com/towards-data-science/a-crash-course-of-planning-for-perception-engineers-in-autonomous-driving-ede324d78717"
---


## 기획과 결정을 위한 기본

![이미지](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_0.png)

고전적인 모듈식 자율 주행 시스템은 일반적으로 인식, 예측, 기획 및 제어로 구성되어 있습니다. 2023년까지 대부분의 대량 생산 자율 주행 시스템에서 AI(인공지능) 또는 ML(머신러닝)은 주로 인식을 향상시켰으며, 그 영향은 하류 구성 요소에서 사라졌습니다. 기획 스택에서 AI의 낮은 통합과는 대조적으로, BEV(새눈뷰 인식 파이프라인과 같은 end-to-end 인식 시스템)과 같은 인식 시스템이 대량 생산 차량에 적용되었습니다.

![이미지](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_1.png)

<div class="content-ad"></div>

다중 이유가 있습니다. 사람이 만든 프레임워크에 기반을 둔 고전적인 스택은 기계 학습 기반 기능보다 설명 가능성이 높으며 현장 시험 문제를 빠르게 해결하기 위해 빠르게 반복할 수 있습니다(수 시간 내). 그러나 사용 가능한 인간 주행 데이터를 방치하는 것은 합리적이지 않습니다. 또한, 컴퓨팅 파워를 증가하는 것이 엔지니어링 팀을 확장하는 것보다 더 확장 가능합니다.

다행히도, 학계와 산업 모두에서 이 상황을 바꾸기 위한 강력한 추세가 있습니다. 먼저 하류 모듈은 점점 데이터 중심적이 되고, CVPR 2023의 최고 논문인 UniAD에서 제안된 것과 같은 다른 인터페이스를 통합할 수도 있습니다. 또한 점점 발전하는 생성적 인공지능의 물결에 이끌려, 단일 통합 비전-언어-행동(VLA) 모델은 복잡한 로봇 작업(RT-2에서 학계, TeslaBot 및 1X에서 산업) 및 자율 주행(GAIA-1, DriveVLM에서 학계, Wayve AI 운전자, Tesla FSD에서 산업)를 처리하는 데 큰 잠재력을 보여줍니다. 이는 인식 스택에서 계획 스택으로의 AI 및 데이터 중심 개발 도구들을 가져오게 됩니다.

본 블로그 포스트는 인지 엔지니어들이라는 인식 엔지니어들을 위한 급작스러운 강좌 형식으로, 계획 스택의 문제 설정, 기존 방법론 및 도전 과제를 소개하는 것을 목표로 합니다. 인지 엔지니어로서 저는 지난 몇 주 동안 고전적인 계획 스택을 체계적으로 학습할 시간이 생겼고, 배운 내용을 공유하고 싶습니다. 또한 AI 실무자의 관점에서 AI가 어떻게 도움을 줄 수 있는지에 대한 생각도 나누겠습니다.

본 게시물의 목표 독자는 특히 자율 주행 분야에서 활동하는 AI 실무자, 특히 인지 엔지니어를 대상으로 합니다.

<div class="content-ad"></div>

테이블 목차가 있는 긴 기사입니다 (11100 단어), 키워드로 빠르게 검색하고자 하는 분들께 도움이 될 것 같아요.

Table of Contents (ToC)

왜 계획을 배워야 하는가?
계획이란 무엇인가?
   문제 정의
   계획 용어집
   행동 계획
   Frenet vs 카테시안 시스템
고전적 도구 - 계획의 삼총사
   탐색
   샘플링
   최적화
산업 현장에서의 계획 실천
   경로 - 속력 분리된 계획
   공간-시간적 결합된 계획
의사 결정
   무엇과 왜?
   MDP와 POMDP
   가치 반복과 정책 반복
   알파고와 MCTS - 신경망과 트리의 만남
   자율주행에서의 MPDM (및 후속작)
산업 현장에서의 의사 결정
   트리
   트리 없이
자기 성찰
   왜 계획에서 신경망인가?
   e2e NN 플래너는 무엇인가?
   예측 없이도 할 수 있을까?
   트리 없이도 신경망만으로 할 수 있을까?
   의사 결정을 위해 LLM을 사용할 수 있을까?
진화 트렌드

<div class="content-ad"></div>

문제 해결적인 시각에서, 여러분의 고객들의 어려움을 더 잘 이해한다면, 인지 엔지니어로서 하향식 고객들에게 더 효과적으로 서비스를 제공할 수 있을 것입니다. 주요 관심을 인지 작업에 두지만, 새로운 도구를 도메인 지식과 결합하는 것이 문제를 해결하는 가장 효율적인 방법입니다. 특히 튼튼한 수학적 공식을 갖는 도메인 지식에 영감을 받은 학습 방법은 더 많은 데이터 효율성을 가질 것입니다. 계획이 규칙 기반에서 머신 러닝 기반 시스템으로 전환되면서 초기 프로토타입과 종단간 시스템의 제품이 도로에 등장하더라도, 계획과 머신 러닝의 기본을 깊이 이해할 수 있는 엔지니어가 필요합니다. 이러한 변화에도 불구하고, 고전적인 방법과 학습 방법이 상당 기간 동안 공존할 가능성이 높습니다. 아마 8:2에서 2:8로 이동하는 것일지도 모릅니다. 이 분야에서 일하는 엔지니어에게는 양쪽 세계를 이해하는 것이 거의 필수적입니다.

가치 중심의 개발 관점에서, 고전적인 방법의 한계를 이해하는 것이 중요합니다. 이 통찰력은 즉시 영향을 주고 현재 문제를 해결하는 시스템을 디자인하기 위해 새로운 머신 러닝 도구를 효과적으로 활용할 수 있게 해 줍니다.

또한, 계획은 자율 주행뿐만 아니라 모든 자율 에이전트에 중요한 부분입니다. 계획이 무엇인지, 어떻게 작동하는지를 이해한다면, 더 많은 머신 러닝 재능이 이 흥미로운 주제에 참여하여 자율 에이전트, 자동차든 다른 형태의 자동화든 발전에 기여할 수 있을 것입니다.

<div class="content-ad"></div>

# 계획이란 무엇인가요?

## 문제 정의

자율주행 차량의 "뇌"인 계획 시스템은 차량의 안전하고 효율적인 주행에 중요합니다. 플래너의 목표는 안전하고 편안하며 목표지점으로 효율적으로 진행하는 궤적을 생성하는 것입니다. 다시 말해, 안전, 편안함, 그리고 효율성이 계획의 세 가지 주요 목표입니다.

계획 시스템에 대한 입력은 정적 도로 구조물, 동적 도로 에이전트, 점유 네트워크에서 생성된 빈 공간, 그리고 교통 대기 조건을 포함한 모든 인지 출력물이 필요합니다. 계획 시스템은 부드러운 궤적을 위해 가속도와 거칠기를 모니터링하고 상호작용과 교통 예의를 고려하여 차량 안락성을 보장해야 합니다.

<div class="content-ad"></div>

계획 시스템은 자아 차량의 저수준 컨트롤러가 추적하기 위해 일련의 웨이포인트 형식의 궤적을 생성합니다. 구체적으로 이러한 웨이포인트는 미래의 자아 차량 위치를 일련의 고정된 시간 스탬프에서 나타냅니다. 예를 들어, 각 포인트는 0.4초 간격으로, 8초 계획 수평을 커버하며 총 20개의 웨이포인트가 생성될 수 있습니다.

고전적인 계획 스택은 전역 경로 계획, 지역 행동 계획 및 지역 궤적 계획으로 대략적으로 구성됩니다. 전역 경로 계획은 글로벌 지도상에서 시작점부터 도착점까지의 도로 수준 경로를 제공합니다. 지역 행동 계획은 다음 몇 초 동안의 의미 있는 운전 행동 유형(예: 따라가기, 기운주기, 차선 변경하기, 양보하기, 추월하기)을 결정합니다. 행동 계획 모듈에서 결정된 행동 유형을 기반으로, 지역 궤적 계획이 단기 궤적을 생성합니다. 전역 경로 계획은 보통 내비게이션이 설정된 후에 지도 서비스에 의해 제공되며, 본 글의 범위를 벗어납니다. 이제부터 행동 계획 및 궤적 계획에 초점을 맞출 것입니다.

행동 계획 및 궤적 생성은 명시적으로 함께 작동하거나 하나의 프로세스로 결합될 수 있습니다. 명시적 방법에서는, 행동 계획 및 궤적 생성이 계층적 프레임워크 내에서 다른 주파수에서 작동하는 별개의 프로세스로 작동합니다. 행동 계획은 1~5Hz, 궤적 계획은 10~20Hz에서 작동합니다. 대부분의 경우 높은 효율을 보이지만, 다양한 시나리오에 대응하려면 상당한 수정과 세부 조정이 필요할 수 있습니다. 더 발전된 계획 시스템은 두 가지를 하나의 최적화 문제로 결합합니다. 이 접근 방식은 효율성과 최적성을 모두 보장하며 어떠한 타협도 없이 가능성을 확보합니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_2.png)

<div class="content-ad"></div>

## 계획 용어집

이미지와 위 섹션에서 사용된 용어가 완전히 일치하지 않는 것을 눈치채셨을 수 있습니다. 모두가 사용하는 표준 용어가 없습니다. 학계와 산업에서 엔지니어들이 같은 개념을 가리키는 데 다른 이름을 사용하거나, 다른 개념을 가리키는 데 같은 이름을 사용하는 것은 흔치 않습니다. 이는 자율 주행 분야의 계획이 아직 미루거나 완전히 수렴하지 않았음을 나타냅니다.

여기서는 이 게시물에서 사용된 표기법을 나열하고 문헌에서 제시된 기타 개념들을 간단히 설명합니다.

- 계획(Planning): 제어와 병렬로 생성된 경로 중요점을 생성하는 최상위 개념. 계획과 제어는 함께 PnC(계획과 제어)로 참조됩니다.
- 제어(Control): 경로 중요점을 받아서 구동기가 실행할 고주파 스티어링, 스로틀 및 브레이크 명령을 생성하는 최상위 개념입니다. 제어는 다른 영역에 비해 비교적 잘 수립되었으며 일반적인 PnC 개념에도 불구하고 이 게시물의 범위를 벗어납니다.
- 예측(Prediction): 주행 차량 이외의 교통 에이전트의 미래 경로를 예측하는 최상위 개념. 예측은 기타 에이전트를 위한 가벼운 계획으로 간주될 수 있으며 동작 예측이라고도 합니다.
- 행동 계획(Behavior Planning): 고수준의 의미 있는 동작(예: 차선 변경, 추월)을 생성하는 모듈로 일반적으로 상위 수준의 궤적을 생성합니다. 상호 작용 맥락에서 특히 상호 작용에 관한 의사 결정 또는 작업 계획으로도 알려져 있습니다.
- 동작 계획(Motion Planning): 의미 있는 동작을 받아들여 제어가 실행할 계획 가시거리 동안 매끄러우며 실행 가능한 경로 중요점을 생성하는 모듈입니다. 궤적 계획이라고도 합니다.
- 궤적 계획(Trajectory Planning): 동작 계획과 같은 용어입니다.
- 의사 결정(Descision Making): 상호 작용에 초점을 맞춘 행동 계획입니다. 에고-에이전트 상호 작용이 없으면 단순히 행동 계획으로 참조됩니다. 전술적 의사 결정이라고도 합니다.
- 루트 계획(Route Planning): 도로 네트워크 상의 우선적인 경로를 찾아주는 모듈로 임무 계획이라고도 합니다.
- 모델 기반 접근(Model-Based Approach): 계획에서 고전적인 계획 스택에서 사용되는 수동으로 제작된 프레임워크를 가리킵니다. 학습 기반 방법과 대조되며 학습 기반 방법과 대조됩니다.
- 다중성(Multimodality): 계획의 컨텍스트에서 다양한 의도를 일반적으로 가리킵니다. 이는 인지에 다중 모달 입력이나 VLM이나 VLA와 같은 다중 모달 대형 언어 모델의 컨텍스트의 다중성과 대조됩니다.
- 참조선(Reference Line): 글로벌 경로 정보와 에고 차량의 현재 상태를 기반으로 한 지역(수백 미터) 및 굵은 경로입니다.
- 프레네트 좌표계(Frenet Coordinates): 참조선을 기반으로 한 좌표계입니다. 프레네트는 곡선 경로를 직선적인 터널 모델로 단순화합니다. 더 자세한 소개는 아래에서 확인하십시오.
- 궤적(Trajectory): (x, y, t)의 카테시안 좌표계 또는 프레네트 좌표계의 (s, l, t) 형식으로 3D 공간시간 곡선입니다. 궤적은 경로와 속도로 구성됩니다.
- 경로(Path): (x, y)의 카테시안 좌표계 또는 프레네트 좌표계의 (s, l) 형식으로 2D 공간 곡선입니다.
- 의미 있는 동작(Semantic Action): 명확한 인간 의도를 가진 동작 (예: 차량 추격, 가로막기, 옆 통행, 양보, 추월)의 고수준 추상화입니다. 의도, 정책, 조종 또는 기본 동작으로도 개별적으로 참조됩니다.
- 동작(Action): 고정된 의미가 없는 용어입니다. 제어(구동기가 실행할 고주파 스티어링, 스로틀 및 브레이크 명령)의 출력 또는 계획(경로 중요점)의 출력으로 참조될 수 있습니다. 의미 있는 동작은 동작 예측의 출력으로 참조됩니다.

<div class="content-ad"></div>

다양한 문헌에서는 각기 다른 표기법과 개념을 사용할 수 있습니다. 몇 가지 예시를 살펴보겠습니다:

- 의사결정 시스템: 때로는 계획과 제어를 포함하기도 합니다. (출처: A Survey of Motion Planning and Control Techniques for Self-driving Urban Vehicles 및 BEVGPT)
- 이동 계획: 때로는 최상위 계획 개념이며, 행동 계획과 궤적 계획을 포함하기도 합니다. (출처: Towards A General-Purpose Motion Planning for Autonomous Vehicles Using Fluid Dynamics)
- 계획: 때로는 행동 계획, 이동 계획, 루트 계획을 모두 포함하기도 합니다.

이러한 변형들은 용어 다양성과 이 분야의 진화하는 성격을 보여줍니다.

## 행동 계획

<div class="content-ad"></div>

기계 학습 엔지니어로, 행동 계획 모듈이 매우 수작업으로 만들어진 중간 모듈임을 알 수 있습니다. 정확한 형식과 내용에 대한 합의가 없습니다. 구체적으로, 행동 계획의 출력물은 참조 경로 또는 이고 동작에 대한 객체 라벨링일 수 있습니다 (예: 왼쪽 또는 오른쪽에서 지나가기, 통과 또는 양보). "의미 행동"이라는 용어에는 엄격한 정의나 고정된 방법이 없습니다.

행동 계획과 이동 계획의 분리는 자율 주행 차량의 극도로 높은 차원의 작업 공간을 효율적으로 해결하는 데 도움이 됩니다. 자율 주행 차량의 동작은 일반적으로 10 Hz 이상 (경로점에서의 시간 해상도)에서 사고되어야 하며, 이러한 대부분의 동작은 직진과 같이 비교적 간단합니다. 분리 후, 행동 계획 레이어는 상대적으로 굵은 해상도에서 미래 시나리오를 고려해야 하며, 이동 계획 레이어는 행동 계획에 의해 내려진 결정을 기반으로 지역 해결 공간에서 작동합니다. 행동 계획의 또 다른 이점은 비볼록 최적화를 볼록 최적화로 변환하는 것이며, 이에 대해 더 자세히 논의하겠습니다.

## Frenet vs Cartesian systems

프레네트 좌표계는 자신만의 소개 섹션을 가치 있게 여길 만큼 널리 사용되는 시스템입니다. 프레네트 프레임은 머리를 따라 경로에 대한 측면 및 종횡 운동을 독립적으로 관리함으로써 궤적 계획을 간단화합니다. sss 좌표는 종횡 편향 (도로를 따라 이동 거리)을 나타내며, lll (또는 ddd) 좌표는 경로에 대한 측면 위치 (참조 경로와의 상대적 위치)를 나타냅니다.

<div class="content-ad"></div>

Frenet은 카테시안 좌표계에서 곡선 경로를 직선 터널 모델로 간단화합니다. 이 변환은 곡선 도로의 비선형 경계 제약을 선형 제약으로 변환하여 후속 최적화 문제를 크게 간소화합니다. 또한 인간은 종방향 및 횡방향 움직임을 다르게 인식하며, Frenet 프레임은 이러한 움직임을 분리하고 더 유연하게 최적화할 수 있도록 합니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_3.png)

Frenet 좌표계는 낮은 곡률의 정형화된 도로 그래프가 필요합니다. 실제로는 고속도로나 도심 고속도로와 같이 곡률이 작은 구조화된 도로에 선호됩니다. 그러나 Frenet 좌표계의 문제는 기준선 곡률이 증가함에 따라 크게 증폭되므로, 가이드 라인이 있는 도시 교차로와 같이 높은 곡률을 갖는 구조화된 도로에는 신중히 사용해야 합니다.

항구, 광산 지역, 주차장 또는 가이드 라인이 없는 교차로와 같은 비정형 도로의 경우, 더 유연한 카테시안 좌표계를 권장합니다. 카테시안 시스템은 고곡률 및 구조화되지 않은 시나리오를 더 효과적으로 처리할 수 있기 때문에 이러한 환경에 더 적합합니다.

<div class="content-ad"></div>

# 고전적 도구 — 계획의 삼총사

자율 주행의 계획은 초기 고차원 상태(위치, 시간, 속도, 가속도 및 갑작스러운 변화 포함)부터 목표 부분 공간까지의 경로를 계산하고 모든 제약 조건을 충족하는 것을 포함합니다. 검색, 샘플링 및 최적화는 계획에 널리 사용되는 세 가지 도구입니다.

## 검색

고전적인 그래프 검색 방법은 계획에서 인기가 있으며 구조화된 도로 상의 경로/미션 계획이나 구조화되지 않은 환경에서 최적의 경로를 찾기 위해 직접 모션 계획에 사용됩니다(주차장이나 도시 교차로와 같은 맵이 없는 시나리오). Dijkstra 알고리즘에서 A*(에이 스타)로 그리고 하이브리드 A*로 발전 경로가 명확하게 있습니다.

<div class="content-ad"></div>

Dijkstra's algorithm is like a thorough explorer examining every possible route to find the shortest one. It's a diligent method that guarantees the best path but can be a bit slow in practice. The algorithm shown in the image below meanders around in search of the optimal path. Essentially, it's similar to a broad and far-reaching search, considering the costs involved in moving to each location. To make this process more efficient, factoring in the destination location can help narrow down the search area.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_4.png)

In contrast, the A* algorithm utilizes clever strategies to prioritize paths that seem to be heading closer to the end goal, which enhances its efficiency. A* cleverly combines the distance traveled so far (as in Dijkstra's) with an estimate of the remaining distance using heuristics. A* will guarantee the shortest path only if the heuristics are logical and reliable. If the heuristic is not well-suited, A* might perform worse than Dijkstra and may end up acting more like a simple best-first search.

For autonomous driving scenarios, the hybrid A* algorithm takes a step further by incorporating vehicle dynamics. A* alone may not adhere to the vehicle's movement restrictions and might not track accurately (for instance, maintaining the steering angle within a specific range like 40 degrees). While A* treats both the state and action spaces as a grid, the hybrid A* distinguishes between them, keeping the state in a grid-like structure but allowing smooth continuous actions based on the vehicle's dynamics.

<div class="content-ad"></div>

타로 전문가님 안녕하세요! 하이브리드 A*에서 제안된 핵심 혁신 중 하나인 분석 확장(목표로 향한 슛)이 있습니다. A*에 대한 자연스러운 개선 방법은 최근에 탐색한 노드를 목표 지점에 이어지는 충돌하지 않는 직선으로 연결하는 것입니다. 이를 가능하게 한다면, 우리는 해결책을 찾은 것입니다. 하이브리드 A*에서는 이 직선이 Dubins 및 Reeds-Shepp(RS) 커브로 대체되며, 이는 차량 키네마틱스를 준수합니다. 이 초기 중지 방법은 최적성과 실현 가능성 사이의 균형을 이루며, 더 나아가 실현 가능성에 더 초점을 맞춥니다.

하이브리드 A*는 주차 상황 및 지도 없는 도심 교차로에서 적극적으로 활용됩니다. 여기에 주차 상황에서 그 작동 방식을 시연한 매우 멋진 동영상이 있습니다.

![video](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_5.png)

## 샘플링

<div class="content-ad"></div>

다른 인기있는 계획 방법 중 하나는 샘플링입니다. 유명한 몬테카를로 방법은 무작위 샘플링 방법입니다. 본질적으로 샘플링은 많은 후보를 무작위로 선정하거나 사전에 따라 선택한 다음, 정의된 비용에 따라 최적의 후보를 선택하는 것을 포함합니다. 샘플링 기반 방법에서는 많은 옵션을 빠르게 평가하는 것이 중요합니다. 왜냐하면 이는 자율 주행 시스템의 실시간 성능에 직접적인 영향을 미치기 때문이죠.

샘플링은 이미 주어진 문제나 하위 문제에 대한 해석적 솔루션이 이미 알려져 있는 매개 변수화된 솔루션 공간에서 발생할 수 있습니다. 예를 들어, 일반적으로 시간에 따른 위치 p(t)의 세 번째 미분(삼차 도함수)인 제르크(가속도의 세 번째 미분)의 제곱의 시간 적분을 최소화하는 것을 원합니다. 

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_6.png)

수학적으로 증명될 수 있습니다. 이차 도학수의 제어 상태 사이의 최적의 연결을 제공하는 것은 신차(5차) 다항식이며, 추가된 비용 요소를 고려할 때에도 마찬가지입니다. 5차 다항식의 매개 변수 공간에서 샘플링함으로써 최소 비용을 갖는 것을 찾아 근사 솔루션을 얻을 수 있습니다. 비용은 속도, 가속도, 제르크 제한 및 충돌 확인과 같은 요소를 고려합니다. 이 방법은 본질적으로 샘플링을 통해 최적화 문제를 해결합니다.

<div class="content-ad"></div>

이미지 tag를 Markdown 형식으로 변경해주세요.

Sampling-based methods have inspired numerous ML papers, including CoverNet, Lift-Splat-Shoot, NMP, and MP3. These methods replace mathematically sound quintic polynomials with human driving behavior, utilizing a large database. The evaluation of trajectories can be easily parallelized, which further supports the use of sampling-based methods. This approach effectively leverages a vast amount of expert demonstrations to mimic human-like driving behavior, while avoiding random sampling of acceleration and steering profiles.

![Image 7](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_7.png)

Optimization

<div class="content-ad"></div>

최적화는 특정 제약 조건 하에서 목표 함수를 최대화하거나 최소화하여 문제에 대한 최상의 해법을 찾는 것을 의미합니다. 신경망 훈련에서는 경사 하강법과 역전파를 사용하여 네트워크의 가중치를 조정하는 유사한 원리가 적용됩니다. 그러나 신경망 이외의 최적화 작업에서는 일반적으로 모델이 덜 복잡하며 경사 하강법보다 효과적인 방법이 종종 사용됩니다. 예를 들어, 경사 하강법은 이차 프로그래밍에 적용될 수 있지만, 일반적으로 가장 효율적인 방법은 아닙니다.

자율 주행에서 최적화를 위한 계획 비용은 일반적으로 동적 객체(장애물 회피), 정적 도로 구조(차로 따라가기), 올바른 경로를 보장하기 위한 내비게이션 정보 및 자아 상태(부드러움 평가)를 고려합니다.

최적화는 볼록(convex) 및 비볼록(non-convex) 유형으로 분류할 수 있습니다. 핵심 차이점은 볼록 최적화 시나리오에서 전역 최적해가 하나만 존재하며 해당 해가 지역 최적해이기도 하다는 것입니다. 이 특징은 최적화 문제에 대한 초기 솔루션에 영향을 받지 않는다는 것을 의미합니다. 반면 비볼록 최적화에서는 초기 솔루션이 매우 중요하며 아래 차트에서 설명되듯이 초기 솔루션에 따라 결과가 크게 달라집니다.

![이미지](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_9.png)

<div class="content-ad"></div>

계획 수립은 많은 지역 최적점을 갖는 매우 비볼록 최적화를 포함하므로 초기 솔루션에 매우 의존합니다. 게다가 볼록 최적화는 일반적으로 더 빨리 실행되므로 자율 주행과 같은 실시간 애플리케이션에선 선호됩니다. 전형적인 방법론은 다른 방법들과 함께 볼록 해법 공간을 먼저 개요화하기 위해 볼록 최적화를 사용하는 것입니다. 이것이 행동 계획과 모션 계획을 분리하는 수학적 기초이며, 좋은 초기 솔루션을 찾는 것은 행동 계획의 역할입니다.

구체적인 예로 장애물 회피를 들어보겠습니다. 장애물의 위치는 최적화 문제의 하한 또는 상한 제약 조건 역할을 하면서 일반적으로 비볼록 문제를 소개합니다. 방향을 아는 경우 이를 살짝 밀어내는 것은 볼록 최적화 문제가 되며, 방향을 모르는 경우 미는 방향을 먼저 결정해야 하므로 모션 계획이 해결하는 볼록 문제로 전환됩니다. 이 방향 결정은 행동 계획에 속합니다.

![Image 1](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_10.png)

![Image 2](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_11.png)

<div class="content-ad"></div>

그런 결정을 어떻게 내릴까요? 비볼록 문제를 해결하기 위해 앞서 언급한 탐색 또는 샘플링 방법을 사용할 수 있습니다. 샘플링 기반 방법은 매개 변수 공간에 많은 옵션을 흩뿌려 비볼록 문제를 효과적으로 처리하며, 마치 탐색하는 것과 유사합니다.

어느 방향에서 밀어내야 하는지 결정함으로써 문제 공간이 볼록성을 보장하는 데 충분하다고 의문을 제기할 수도 있습니다. 이를 설명하기 위해 토폴로지에 대해 이야기할 필요가 있습니다. 경로 공간에서 유사한 실행 가능한 경로는 장애물 간섭 없이 서로 연속적으로 변환될 수 있습니다. 이러한 유사한 경로는 토폴로지의 형식적 언어로 "동인시류 클래스"로 그룹화되며, 이들과 동인시류 클래스에 속하는 단일 초기 솔루션에서 출발하는 것으로 모두 탐색할 수 있습니다. 이러한 경로들은 그림에서 빨간색 또는 녹색 그림자 영역으로 설명된 주행 복도를 형성합니다. 3D 시공간 경우에 대한 자세한 내용은 QCraft 기술 블로그를 참조해 주세요.

효율적으로 최적화 문제를 해결하는 핵심은 최적화 솔버의 능력에 있습니다. 일반적으로 솔버는 궤적을 계획하는 데 약 10밀리초가 소요됩니다. 이 효율성을 10배로 증가시킬 수 있다면, 이는 알고리즘 설계에 상당한 영향을 미칠 수 있습니다. 이 정확한 개선은 2022년 Tesla AI Day에서 강조되었습니다. 인지 시스템에서도 유사한 개선이 일어났는데, 가능한 컴퓨팅 파워가 10배로 증가함에 따라 2D 인식에서 Bird’s Eye View (BEV)로 전환되었습니다. 보다 효율적인 최적화 솔버를 만들기 위해서는 상당한 엔지니어링 자원이 필요합니다.

#계획의 산업 실천 방법

<div class="content-ad"></div>

다양한 계획 시스템의 중요한 차별 요인 중 하나는 시공간적으로 분리되어 있는지 여부입니다. 구체적으로 시공간적으로 분리된 방법은 먼저 공간 차원에서 경로를 생성한 다음 이 경로의 속도 프로필을 계획합니다. 이 접근 방식은 경로-속도 분리로도 알려져 있습니다.

경로-속도 분리는 가로방향-세로방향 (lat-long) 분리로 자주 언급되며, 여기서 가로방향 (lat) 계획은 경로 계획에 해당하고 세로방향 (long) 계획은 속도 계획에 해당합니다. 이 용어는 프레네트 좌표계에서 유래한 것으로 보입니다. 나중에 자세히 살펴보겠습니다.

분리된 해결책은 구현하기 쉽고 문제의 약 95%를 해결할 수 있습니다. 반면 결합된 해결책은 높은 이론적 성능 한계를 가지나 구현하기 어렵습니다. 더 많은 매개변수를 조정해야 하며 매개변수 조정에 보다 원칙적인 접근이 필요합니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_13.png)

## Path-speed decoupled planning

Baidu Apollo's EM planner is a great example of path-speed decoupled planning system.

The EM planner simplifies the computational complexity by breaking down a three-dimensional station-lateral-speed problem into two two-dimensional problems: station-lateral and station-speed. The core of Apollo’s EM planner consists of an iterative Expectation-Maximization (EM) step, which includes path optimization and speed optimization. Each step involves an E-step (projection and formulation in a 2D state space) and an M-step (optimization in the 2D state space). The E-step requires projecting the 3D problem into either a Frenet SL frame or an ST speed tracking frame.


<div class="content-ad"></div>

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_14.png)

In the realm of autonomous driving, the M-step (maximization step) plays a crucial role in path and speed optimization by tackling non-convex optimization challenges. When optimizing the path, decisions revolve around shifting an object to the left or right, while speed optimization requires choices like overtaking or yielding to dynamic objects on the path. The Apollo EM planner effectively handles these challenges through a two-step process: first employing Dynamic Programming (DP) and then transitioning to Quadratic Programming (QP).

Dynamic Programming employs a sampling or searching algorithm to create an initial solution, effectively transforming the non-convex space into a convex one, laying the groundwork for QP. Quadratic Programming then fine-tunes the rough DP results within the convex space. DP prioritizes feasibility, while QP focuses on refining the solution for optimal results within the constraints.

Using our defined terminology, Path DP corresponds to lateral BP, Path QP to lateral MP, Speed DP to longitudinal BP, and Speed QP to longitudinal MP. This process involves executing Basic Planning (BP) first, followed by Master Planning (MP) in both path and speed optimization steps.

<div class="content-ad"></div>

![Autonomous Driving Planning](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_15.png)

## 공간적 시간적 결합 계획

자율 주행에서 분리된 계획이 95%의 경우를 해결할 수 있지만, 남은 5%는 도전적인 동적 상호작용을 포함하여 분리된 해결책은 종종 최적의 궤적을 도출하지 못하는 복잡한 시나리오입니다. 이러한 복잡한 상황에서 지능을 증명하는 것은 중요하기 때문에 이는 그 분야에서 매우 핫한 주제입니다.

예를 들어, 좁은 공간 통과 시, 최적의 행동은 양보하기 위해 감속하거나 지나가기 위해 가속하는 것일 수 있습니다. 이러한 행동은 분리된 해결 공간 내에서 실현할 수 없으며, 공간과 속도를 동시에 고려하여 복잡한 동적 상호작용을 효과적으로 처리하는 것을 요구합니다. 공간적 시간적 결합 최적화는 더 통합된 접근 방식을 가능하게 하며, 경로와 속도를 동시에 고려하여 복잡한 동적 상호작용을 효과적으로 다룰 수 있습니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_16.png)

But, let's talk about the challenges in joint spatiotemporal planning. Firstly, solving the non-convex problem directly in a higher-dimensional state space is more complex and time-consuming compared to using a decoupled solution. Secondly, considering interactions in spatiotemporal joint planning adds another layer of complexity. We will delve deeper into this topic when we address decision-making.

Now, let's discuss two methods for solving these challenges: brute force search and constructing a spatiotemporal corridor for optimization.

Brute force search involves direct exploration in a 3D spatiotemporal space (2D in space and 1D in time) and can be conducted in either XYT (Cartesian) or SLT (Frenet) coordinates. Let's take SLT as an example. In SLT space, the layout resembles an elongated energy bar, stretched in the L dimension and flattened in the ST face. For brute force search, we can apply the hybrid A-star algorithm, where the cost consists of a combination of progress cost and cost to go. Throughout optimization, we must adhere to search constraints that prevent backward movements in both the s and t dimensions.

<div class="content-ad"></div>

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_17.png)

또 다른 방법은 환경 요소를 고려한 안전한 경로를 생성하는 공간시간 좌표계 골목을 구축하는 것입니다. SSC (Spatial Semantic Corridor)는 시맨틱 요소에 의해 주어진 요구 사항을 시맨틱 골목으로 해석하여 이에 맞는 안전한 경로를 생성합니다. 시맨틱 골목은 시공간 도메인에서 시맨틱 요소에 의해 제시된 동적 제약 조건이 있는 상호 연결된 일련의 충돌이 없는 큐브들로 구성됩니다. 각 큐브 내에서는 Quadratic Programming (QP)을 사용하여 해결할 수 있는 볼록 최적화 문제가 되며, 이에 의해 안전한 경로가 결정됩니다.

SSC는 여전히 BP (Behavior Planning) 모듈이 필요하여 초기 운전 경로를 제공합니다. 환경의 복잡한 시맨틱 요소는 참조 차선을 중심으로 시공간 도메인에 투영됩니다. EPSILON은 SSC가 동작 계획자와 협력하여 작동하는 운동 계획자로 구성된 시스템을 소개한 바 있습니다. 다음 섹션에서는 특히 상호 작용에 초점을 맞춘 동작 계획에 대해 논의할 것입니다. 이 문맥에서 동작 계획은 일반적으로 의사 결정으로 언급됩니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_18.png)

<div class="content-ad"></div>

# 의사 결정

## 무엇과 왜?

자율 주행에서의 의사 결정은 본질적으로 행동 계획이지만 다른 교통 요소와의 상호 작용에 초점을 맞춥니다. 가정은 다른 요소들이 대부분 합리적이며 우리의 행동에 예측 가능한 방식으로 반응할 것이라는 것입니다. 이를 "잡음 있는 합리성"으로 설명할 수 있습니다.

고급 계획 도구가 사용 가능한 경우 의사 결정의 필요성에 대해 의문을 제기할 수 있습니다. 그러나 불확실성과 상호 작용이라는 두 가지 주요 측면은 주로 동적 객체의 존재로 인해 확률적인 성격을 환경으로 도입합니다. 상호 작용은 자율 주행의 가장 어려운 부분으로, 일반 로보틱스와 구별짓습니다. 자율 주행 차량은 길을 따라 다니는 것 뿐만 아니라 다른 요소들의 행동을 예측하고 반응하기 위해서도 해야 하므로 안전하고 효율적인 의사 결정이 필수적입니다.

<div class="content-ad"></div>

결정적인(순수하게 기하학적인) 상호 작용이 없는 세계에서는 결정을 내리는 것이 불필요하며, 검색, 샘플링, 최적화를 통한 계획이 충분합니다. 3D XYT 공간에서의 무차별 검색은 일반적인 해결책으로 기능할 수 있습니다.

대부분의 고전적인 자율 주행 스택에서는 예측한 뒤 계획을 세우는 접근 방식을 채택하며, 자아 차량과 다른 차량 사이의 제로 오더 상호 작용을 가정합니다. 이 접근 방식은 예측 출력을 결정론적으로 취급하며, 자아 차량이 이에 맞게 반응해야 함을 요구합니다. 이는 지나치게 보수적인 행동을 야기하며, ‘얼어붙은 로봇’ 문제를 잘 보여줍니다. 이러한 경우에는 예측이 전체 시공간 공간을 채우므로 혼잡한 상황에서의 차로 변경과 같은 행동이 불가능해지는데, 이는 인간이 더 효과적으로 처리하는 것입니다.

확률적인 전략을 다루기 위해서는 마르코프 결정 과정(Markov Decision Processes, MDP) 또는 부분 관측 가능한 마르코프 결정 과정(Partially Observable Markov Decision Processes, POMDP) 프레임워크가 필수적입니다. 이러한 방식은 기하학에서 확률로의 초점을 옮겨 혼돈된 불확실성에 대응합니다. 교통 주체들이 합리적으로 행동하거나 적어도 노이지하게 합리적으로 행동한다고 가정하면, 결정을 통해 안전한 운전 복도를 창출할 수 있습니다.

계획의 세 가지 중요한 목표 중 하나인 안전, 편의, 효율 중에서, 결정을 통해 주로 효율성이 향상됩니다. 보수적인 행동은 안전과 편의를 극대화할 수 있지만, 다른 도로 주체들과의 효과적인 협상은 결정을 통해야만 최적의 효율성을 달성할 수 있습니다. 효과적인 결정은 또한 지능을 나타냅니다.

<div class="content-ad"></div>

## MDP과 POMDP

먼저 Markov Decision Processes (MDP) 및 Partially Observable Markov Decision Processes (POMDP)를 소개하고, 값 반복 및 정책 반복과 같은 체계적인 해결책을 살펴보겠습니다.

Markov Process (MP)는 정적인 확률이 아닌 동적인 랜덤 현상을 다루는 확률과정의 한 유형입니다. Markov Process에서 미래 상태는 현재 상태에만 의존하므로 예측에 충분합니다. 자율 주행의 경우, 관련 상태는 데이터의 마지막 1초만 포함할 수 있어 상태 공간을 확장하여 더 짧은 기록 창을 허용할 수 있습니다.

Markov Decision Process (MDP)는 Markov Process를 의사 결정을 포함시킴으로써 확장한 것입니다. MDP는 결과가 부분적으로 무작위이고 부분적으로 의사 결정자 또는 에이전트에 의해 제어되는 상황을 모델링합니다. MDP는 다섯 가지 요인으로 모델링될 수 있습니다:

<div class="content-ad"></div>

- 상태 (S): 환경의 상태.
- 행동 (A): 에이전트가 환경에 영향을 미치기 위해 취할 수 있는 행동.
- 보상 (R): 환경이 행동의 결과로 제공하는 보상.
- 전이 확률 (P): 에이전트의 행동에 따른 이전 상태에서 새로운 상태로 전이할 확률.
- 감마 (γ): 미래 보상에 대한 할인 요소.

이는 보상 학습 (RL)에서 주로 사용되는 일반적인 프레임워크이기도 한데, 본질적으로 MDP를 나타낸다. MDP 또는 RL의 목표는 장기적으로 누적된 보상을 최대화하는 것이다. 이를 위해 에이전트는 정책에 따라 환경으로부터 상태를 고려하여 좋은 결정을 내리도록 해야 한다.

정책 π는 각 상태 s ∈ S와 행동 a ∈ A(s)에 대해 각각의 행동 a를 취할 확률 π(a|s)로 매핑하는 것이다. MDP 또는 RL은 최적 정책을 도출하는 문제에 대해 연구한다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_19.png)

<div class="content-ad"></div>

파셜 옵저버블 마르코프 의사결정 프로세스(POMDP)는 상태가 직접 관찰되지 않고 옵저베이션을 통해 추론되는 복잡성을 추가합니다. POMDP에서 에이전트는 환경의 상태를 추정하기 위해 가능한 상태에 대한 확률 분포인 '신념'을 유지합니다. 자율 주행 시나리오는 환경의 부분적 가시성과 내재적 불확실성으로 인해 POMDP에 의해 더 잘 표현됩니다. 옵저베이션이 상태를 완벽하게 노출하는 특수한 경우인 MDP는 POMDP의 특별한 경우로 볼 수 있습니다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_20.png)

POMDP는 정보를 적극적으로 수집하여 필요한 데이터를 수집하는 행동을 이끌어내어 이러한 모델의 지능적인 행동을 나타낼 수 있습니다. 이 능력은 특히 교차로에서 대기하는 상황과 같은 시나리오에서 가치 있습니다. 다른 차량의 의도와 신호등의 상태에 대한 정보를 수집함으로써 안전하고 효율적인 결정을 내리는 데 중요하기 때문입니다.

<div class="content-ad"></div>

가치 반복과 정책 반복은 MDP 또는 POMDP 문제를 해결하는 체계적인 방법입니다. 이러한 방법은 복잡성 때문에 실제 응용에서는 흔히 사용되지는 않지만, 이를 이해하면 정확한 해결책과 어떻게 간소화될 수 있는지에 대한 통찰력을 제공합니다. 예를 들어, 알파고의 MCTS 사용이나 자율 주행에서의 MPDM과 같은 방법이 실제 적용됩니다.

MDP에서 최적 정책을 찾기 위해서는 상태에서 취하는 행동으로부터 발생하는 잠재적 또는 예상 보상을 평가해야 합니다. 이 예상 보상에는 즉각적인 보상 뿐만 아니라 미래 보상들까지 포함되는데, 이를 공식적으로는 반환 또는 누적 할인 보상이라고 합니다. (더 깊이 이해하기 위해서는 주제에 대한 권위적인 안내서로 여겨지는 "강화 학습: 입문"을 참고하세요.)

가치 함수(V)는 예상된 반환을 합하여 상태의 품질을 특성화합니다. 행동 가치 함수(Q)는 주어진 상태에 대한 행동의 품질을 평가합니다. 두 함수 모두 특정 정책에 따라 정의됩니다. 벨만 최적 방정식은 최적 정책이 최대 보상을 찾아서 최적이 되는 새로운 상태에서의 즉각적인 보상과 예상 미래 보상을 최대화하는 행동을 선택할 것이라고 말합니다. 간단히 말해, 벨만 최적 방정식은 즉각적인 보상과 행동의 미래 결과를 고려할 것을 권장합니다. 예를 들어, 직장을 바꿀 때 새로운 직책이 제공하는 미래 가치(S') 뿐만 아니라 즉각적인 임금 인상(R)도 고려해야 합니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_21.png)

<div class="content-ad"></div>

벨만 최적성 방정식으로부터 최적 정책을 추출하는 것은 최적 가치 함수가 있을 때 비교적 간단합니다. 하지만 이 최적 가치 함수를 어떻게 찾을까요? 이때 가치 반복이 구원을 줍니다.

![Image Description](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_22.png)

가치 반복은 각 상태의 가치를 안정화될 때까지 반복적으로 업데이트하여 최상의 정책을 찾습니다. 이 과정은 벨만 최적성 방정식을 업데이트 규칙으로 변환함으로써 유도됩니다. 궁극적으로, 최적 미래 상황을 활용하여 반복을 그 쪽 방향으로 유도하는 것입니다. 간단히 말해, "김이 석숩시다(fake it until you make it)"!

![Image Description](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_23.png)

<div class="content-ad"></div>

가치 반복은 유한 상태 공간에 대해 수렴이 보장됩니다. 상태에 할당된 초기 값에 관계없이 수렴합니다(자세한 증명은 RL의 성경을 참조하세요). 할인 요소(gamma)가 0으로 설정되면 즉각적 보상만 고려하므로 한 번의 반복 후에 수렴합니다. 보다 작은 gamma는 고려 범위가 짧아져 빠른 수렴을 이끌지만, 구체적인 문제 해결을 위한 최상의 옵션이 항상 되지는 않습니다. 할인 요소를 균형있게 설정하는 것이 엔지니어링 실무에서 중요합니다.

모든 상태를 0으로 초기화했을 때 이 작동하는 방식에 대한 의문을 제기할 수 있습니다. 벨만 방정식의 즉각적 보상은 추가 정보를 가져오고 초기 대칭을 깨는 데 중요합니다. 목표 상태로 직접 이끄는 상태를 생각해보세요. 이들의 가치는 상태 공간을 통해 바이러스처럼 전파됩니다. 간단히 말해, 작은 승리를 자주 이루는 것입니다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_24.png)

그러나 가치 반복은 비효율성도 갖습니다. 각 반복에서 모든 가능한 행동을 고려하여 최적 행동을 취해야 하므로 Dijkstra의 알고리즘과 유사합니다. 기본적인 접근 방식으로 실행 가능성을 보여줍니다만, 실제 응용 프로그램에는 typ이하지 않는 것이 일반적입니다.

<div class="content-ad"></div>

경찰 조치는 현재 정책에 따라 행동을 취하고 벨만 방정식(벨만 최적 방정식이 아님)에 따라 업데이트하여 정책 개선을 하는 것으로 향상됩니다. 정책 반복은 정책 평가를 정책 개선과 분리하여 훨씬 빠른 해결책을 만들어 냅니다. 각 단계는 목적을 극대화하는 행동을 찾기 위해 모든 가능한 행동을 탐색하는 대신 주어진 정책을 기반으로 취합니다. 정책 반복의 각 반복은 정책 평가 단계 때문에 계산적으로 더 많을 수 있지만, 전반적으로 빠른 수렴으로 이어집니다.

간단히 말하면 한 가지 행동의 결과를 완전히 평가할 수 있는 경우, 현재 정보를 활용하여 자신의 판단을 내리고 최선을 다하는 것이 좋습니다.

## 알파고와 MCTS - 신경망과 트리가 만날 때

<div class="content-ad"></div>

우리 모두는 2016년 알파고가 최고의 인간 플레이어를 이긴 놀라운 이야기를 들어왔습니다. 알파고는 바둑 게임을 MDP로 정의하고 몬테 카를로 트리 탐색(MCTS)으로 해결합니다. 하지만 왜 가치 반복이나 정책 반복을 사용하지 않을까요?

가치 반복과 정책 반복은 MDP 문제를 해결하는 체계적이고 반복적인 방법입니다. 그러나 향상된 정책 반복을 사용해도 여전히 모든 상태의 가치를 업데이트하기 위한 시간 소모적인 작업이 필요합니다. 표준 19x19 바둑판은 대략 2e170개의 가능한 상태가 있습니다. 이 방대한 상태 수는 전통적인 가치 반복이나 정책 반복 기술로 해결하기 어렵게 만듭니다.

알파고와 그 후속작품들은 가치 네트워크와 정책 네트워크에 기반을 둔 MCTS(Monte Carlo Tree Search) 알고리즘을 사용하여 움직임을 찾습니다. 이 네트워크는 인간과 컴퓨터 플레이에서 훈련을 받습니다. 먼저 일반적인 MCTS를 살펴보겠습니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_26.png)

<div class="content-ad"></div>

**MCTS (Monte Carlo Tree Search)**은 현재 상태로부터의 의사 결정을 중점적으로 하는 정책 평가 방법입니다. 한 번의 반복은 선택, 확장, 시뮬레이션(또는 평가), 백업으로 구성됩니다.

- **선택:** 알고리즘은 이전 시뮬레이션을 기반으로 가장 유망한 경로를 따라가며 미탐색 자식 노드인 리프 노드에 도달할 때까지 진행합니다.
- **확장:** 리프 노드에서 다음 가능한 이동을 나타내는 하나 이상의 자식 노드가 추가됩니다.
- **시뮬레이션(평가):** 알고리즘은 새로운 노드부터 랜덤 게임을 끝까지 진행하며 "롤아웃"이라고 알려진 것을 수행합니다. 이를 통해 무작위 이동을 시뮬레이션하여 종료 상태에 도달할 때까지의 잠재적 결과를 판단합니다.
- **백업:** 알고리즘은 게임 결과를 기반으로 선택한 경로 상의 노드 값들을 업데이트합니다. 승리라면 노드 값이 증가하고, 패배라면 값이 감소합니다. 이 과정은 롤아웃 결과를 트리 위로 다시 전파시켜 시뮬레이션 결과에 기반한 정책을 개선합니다.

일정한 반복 횟수 이후, MCTS는 시뮬레이션 중 루트에서 선택된 즉시 조치의 빈도를 제공합니다. 추론시에는 가장 많은 방문 횟수를 가진 조치가 선택됩니다. 이 글에서는 간편한 틱택토 게임을 통한 MTCS의 대화형 설명을 제공해드릴게요.

알파고의 MCTS는 두 개의 신경망에 의해 향상됩니다. **Value Network**는 주어진 상태(판의 구성)의 승률을 평가합니다. **Policy Network**는 모든 가능한 이동에 대한 조치 분포를 평가합니다. 이러한 신경망들은 MCTS의 효과적인 깊이와 너비를 줄임으로써 개선을 이룹니다. 정책 신경망은 조치 샘플링에 도움을 주어 유망한 이동에 집중시키며, 가치 신경망은 포지션을 더 정확히 평가하여 롤아웃의 필요성을 줄입니다. 이 조합은 알파고가 바둑의 방대한 상태 공간에서 효율적이고 효과적인 탐색을 수행할 수 있게 합니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_27.png)

In the expansion step, the policy network samples the most likely positions, effectively pruning the breadth of the search space. In the evaluation step, the value network provides an instinctive scoring of the position, while a faster, lightweight policy network performs rollouts until the game ends to collect rewards. MCTS then uses a weighted sum of the evaluations from both networks to make the final assessment.

What can we learn from AlphaGo for autonomous driving? AlphaGo demonstrates the importance of extracting an excellent policy using a robust world model (simulation). Similarly, autonomous driving requires a highly accurate simulation to effectively leverage algorithms similar to those used by AlphaGo. This approach underscores the value of combining strong policy networks with detailed, precise simulations to enhance decision-making and optimize performance in complex, dynamic environments.

# MPDM (and successors) in autonomous driving

<div class="content-ad"></div>

고의 게임에서는 모든 상태가 즉시 양쪽 플레이어에게 사용 가능하며, 관찰이 상태와 동일한 것으로 간주됩니다. 이는 게임이 MDP 프로세스에 의해 특징 지어질 수 있도록 합니다. 반면에 자율 주행은 관찰을 통해 상태를 추정할 수 있는 POMDP 프로세스입니다.

POMDP는 지각과 계획을 원칙적으로 연결하는 역할을 합니다. POMDP의 전형적인 해결책은 MDP와 유사하며, 제한된 탐색을 통해 이루어집니다. 그러나 주요 도전 과제는 차원의 저주(상태 공간의 증폭) 및 다른 에이전트와의 복잡한 상호작용에 있습니다. 실시간 진행을 가능하게 하기 위해 도메인 특정 가정이 보통 POMDP 문제를 단순화하는 데 사용됩니다.

MPDM(및 두 가지 후속 조치, 그리고 화이트 페이퍼)는 이 방향으로의 선구적인 연구 중 하나입니다. MPDM은 POMDP를 유한하고 이산적인 의미 수준 정책 집합의 닫힌 루프 전방 시뮬레이션으로 축소시킵니다. 모든 차량에 대해 가능한 모든 제어 입력을 평가하는 대신 의미 있는 정책의 관리 가능한 수에 집중함으로써 차원의 저주를 다루며, 자율 주행 시나리오에서 효과적인 실시간 의사 결정을 가능케 합니다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_28.png)

<div class="content-ad"></div>

MPDM(Multi-Policy Decision-Making)의 가정은 두 가지로 이루어져 있어요. 첫째, 인간 운전자의 많은 의사 결정은 이산적이고 고수준의 의미 있는 행동(예: 감속, 가속, 차선 변경, 정지)이 포함돼 있어요. 이러한 행동들은 이 문맥에서 정책이라고 불려지죠. 두 번째 암시적인 가정은 다른 주체들에 대한 것인데요: 다른 차량들은 합리적으로 안전한 결정을 내릴 것이라고 가정돼요. 한 차량의 정책이 결정되면, 그의 행동(궤적)도 결정돼요.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_29.png)

MPDM은 먼저 여러 옵션 중에서 자아차량을 위한 한 가지 정책을 선택하고, 주변 에이전트 각각에 대해 예측에 기반한 한 가지 정책을 선택해요. 그런 다음에는 앞으로의 시뮬레이션을 수행해요(MCTS의 빠른 롤아웃과 유사하죠). 평가 후 가장 좋은 상호 작용 시나리오는 용적 및 시공간 계획 세션에서 언급된 SCC(Spatiotemporal Semantic Corridor)와 같은 모션 플래닝에 전달돼요.

MPDM은 밀집된 교통 흐름 속으로 끼어드는 등 지능적이고 인간적인 행동을 가능하게 해줘요. 이러한 것은 상호 작용을 명시적으로 고려하지 않는 예측 후 계획 파이프라인에서는 불가능해요. MPDM의 예측 모듈은 앞으로의 시뮬레이션을 통해 행동 계획 모델과 긴밀하게 통합돼 있어요.

<div class="content-ad"></div>

MPDM은 결정 기간(10초) 동안 하나의 정책을 가정합니다. 기본적으로, MPDM은 하나의 깊이를 가진 MCTS 방식을 채택하며, 모든 가능한 에이전트 예측을 고려합니다. 이로써 EUDM, EPSILON, MARC 등의 후속 연구 작업을 영감을 주는 여지가 남습니다. 예를 들어, EUDM은 더 유연한 자아 정책을 고려하고, 8초 동안의 결정 기간에 대해 2초씩 시간 간격을 갖는 네 계층의 정책 트리를 할당합니다. 증가된 트리 깊이에 따른 추가 계산을 보상하기 위해, EUDM은 전담 분기에 의한 더 효율적인 넓이 가지치기를 수행하며, 중요한 상황과 주요 차량을 식별합니다. 이 방식은 더 균형 잡힌 정책 트리를 탐색합니다.

MPDM과 EUDM의 전방 시뮬레이션에는 매우 간단한 운전자 모델(Intelligent driver model 또는 IDM은 종방향 시뮬레이션에, Pure Pursuit 또는 PP는 측방향 시뮬레이션에 사용됩니다). MPDM은 고충실도 현실성이 클로즈드 루프 특성 자체보다는 정책 수준의 결정이 낮은 수준의 행동 실행 부정확성에 영향을 받지 않는 한 중요하지 않다고 지적합니다.

![An Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_30.png)

자율 주행의 맥락에서의 비상 대책 수립은 다양한 가능한 미래 시나리오를 고려하여 여러 잠재적인 궤적을 생성하는 것을 포함합니다. 경험 많은 운전자가 많은 미래 시나리오를 예측하고 항상 안전한 예비 계획을 세우는 것이 주요 동기부여 예시입니다. 이 예상 방식은, 다른 차량이 자신의 차로로 갑자기 차로 변경하는 상황에서도 더 부드러운 운전 경험으로 이어집니다.

<div class="content-ad"></div>

긴급 상황 대비 계획에서 중요한 측면 중 하나는 의사 결정 이분화 지점을 연기하는 것입니다. 이는 서로 다른 잠재적 궤적이 분기되는 지점을 지연시켜, 자아 차량이 정보를 수집하고 다양한 결과에 대응할 시간을 더 많이 확보하도록 하는 것을 의미합니다. 이를 통해 차량은 더 많은 정보를 바탕으로 한 결정을 내릴 수 있으며, 이는 숙련된 운전자와 유사한 부드럽고 자신감 있는 운전 행동을 낳을 수 있습니다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_31.png)

MPDM 및 이에 따르는 모든 연구들의 한 가지 가능한 단점은 고속도로와 유사한 구조화된 환경을 위해 설계된 간단한 정책에 의존한다는 점입니다. 이러한 의존은 전방 시뮬레이션의 복잡한 상호 작용을 처리하는 능력을 제한할 수 있습니다. 이를 해결하기 위해 MPDM의 예를 따라, POMDP를 더 효과적으로 만드는 핵심은 고수준 정책 트리의 성장을 통해 행동 및 상태 공간을 단순화하는 것입니다. 공간적 및 시간적 상대 위치 태그를 모든 상대 대상에 대해 열거하고 이후 지도된 가지를 실행함으로써, 더 유연한 정책 트리를 만들 수도 있습니다.

# 의사 결정 산업 실천법

<div class="content-ad"></div>

Decision-making이 현재 연구에서 핫한 주제입니다. 심지어 고전적인 최적화 방법조차 완전히 탐구되지 않은 상태입니다. 머신러닝 방법들은 특히 대형 언어 모델 (LLMs)의 등장과 Chain of Thought (CoT) 또는 Monte Carlo Tree Search (MCTS)와 같은 기법으로 더욱 빛을 발할 수 있습니다.

## Trees

Trees는 의사결정을 수행하는 체계적인 방법입니다. Tesla AI Day 2021 및 2022에서는 AlphaGo와 그 후속인 MuZero에 크게 영향받은 결정 능력을 선보였습니다. 이를 통해 매우 복잡한 상호작용을 다룰 수 있었습니다.

높은 수준에서 Tesla의 접근 방식은 행동 계획 (의사결정) 이후 모션 계획을 따릅니다. 먼저 볼록한 통로를 탐색하고 그 후 공간적-시간적 공동 계획을 사용하여 연속 최적화에 투입합니다. 이 방식은 좁은 통과와 같은 시나리오를 효과적으로 해결합니다. 해당 시나리오는 경로 속도 분리 계획의 전형적인 병목 현상입니다.

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 변경해주세요.

*Tesla*는 데이터 주도와 물리학 기반 체크를 결합한 하이브리드 시스템을 채용하고 있습니다. 명확한 목표로 시작하여, Tesla 시스템은 초기 경로를 생성하고 주요 시나리오를 평가합니다. 그런 다음, 교통 요소에 대해 주장하거나 양보하는 등의 시나리오 변형을 만들기 위해 나무 기반 시나리오 트리를 확장합니다. 이러한 정책 트리 상의 상호 작용 검색은 2021년과 2022년의 발표에서 소개되었습니다.

Tesla의 기계 학습 사용 중 하나의 하이라이트는 경로 최적화를 통한 트리 탐색 가속화입니다. 각 노드에 대해 Tesla는 물리학 기반 최적화와 신경 기반 플래너를 사용하여, 100 마이크로초 대비 10 밀리초의 시간 프레임을 달성합니다 — 이는 10배에서 100배의 개선을 가져옵니다. 신경망은 전문가 시범과 오프라인 최적화 프로그램으로 교육됩니다.

충돌 검사와 안락성 분석과 같은 고전적인 물리학 기반 검사를 신경망 평가자와 결합하여 진입 확률 및 인간과 유사한 정도를 예측하는 신경망 평가자를 만듭니다. 이 점수는 검색 공간을 가지치기하여 가장 유망한 결과에 계산을 집중함으로써 도와줍니다.

<div class="content-ad"></div>

많은 사람들이 기계 학습이 고수준 의사 결정에 적용되어야 한다고 주장하는 반면, 테슬라는 ML을 기본적으로 최적화를 가속화하고 결과적으로 트리 탐색을 진행하는 데에 활용합니다.

몬테 카를로 트리 탐색(Monte Carlo Tree Search, MCTS) 방법은 의사 결정에 궁극적인 도구로 보입니다. 흥미롭게도, 대규모 언어 모델(Large Language Models, LLMs)을 연구하는 사람들은 MCTS를 LLM에 통합하려고 하고, 자율 주행을 연구하는 사람들은 MCTS를 LLM으로 교체하려고 노력하고 있습니다.

약 2년 전까지 테슬라의 기술은 이러한 접근 방식을 따랐습니다. 그러나 2024년 3월 이후로, 테슬라의 완전 자율 주행(Full Self-Driving, FSD) 기술은 이전 방법과는 크게 다른, 보다 end-to-end 접근 방식으로 전환했습니다.

# 나무 없어요

<div class="content-ad"></div>

나무를 자라게 하는 암묵적 상호작용을 고려할 수 있습니다. 미리 예측하고 계획 수립 간에 일차 상호작용을 수행하기 위해 이삭 논리를 구현할 수 있습니다. 한 차수의 상호작용만으로도 이미 훌륭한 행동을 발생시킬 수 있음을 TuSimple의 시연에서 확인할 수 있습니다. MPDM은 본래 한 차수 상호작용이지만 더 원칙적이고 확장 가능한 방식으로 실행됩니다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_33.png)

TuSimple은 사고 대비 계획을 수행할 수 있는 능력을 시연했습니다. 이는 MARC에서 제안된 접근 방식과 유사하지만 (MARC는 사용자 정의 위험 선호도에 대응할 수도 있습니다). 

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_34.png)

<div class="content-ad"></div>

# 자기 성찰

고전 계획 시스템의 기본 구성 요소를 배운 후, 행동 계획, 동작 계획, 그리고 상호작용을 처리하는 원칙적인 방법에 대해 깊이 생각해 보았습니다. 시스템 내의 잠재적인 병목 현상과 기계 학습(ML) 및 신경망(NN)이 어떻게 도움이 될 수 있는지를 고찰 중에 있습니다. 여기에 제 생각 과정을 문서화하여 나중에 참고하고 유사한 궁금증을 가진 다른 사람에게도 도움이 되기를 바랍니다. 이 섹션의 정보에는 개인적인 편향과 추측이 포함될 수 있다는 점을 참고하세요.

# 계획에서 왜 신경망?

기존 모듈화된 파이프라인, e2e NN 계획자로서, 또는 e2e 자율 주행 시스템으로서 문제를 세 가지 다른 관점에서 살펴보겠습니다.

<div class="content-ad"></div>

그림판으로 다시 돌아가 볼까요? 자율 주행 시스템의 계획 시스템의 문제 정의를 검토해 봅시다. 목표는 차량 내의 실시간 엔지니어링 제약 조건을 준수하면서 매우 불확실하고 상호 작용적인 환경에서 안전, 편안함 및 효율성을 보장하는 궤적을 얻는 것입니다. 이러한 요소들은 아래 차트에 목표, 환경 및 제약 조건으로 요약되어 있습니다.

![이미지](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_35.png)

자율 주행에서의 불확실성은 지각 (관찰)과 미래의 장기 에이전트 행동을 예측하는 데 불확실성을 가리킬 수 있습니다. 계획 시스템은 또한 다른 에이전트의 미래 궤적 예측의 불확실성을 처리해야 합니다. 이전에 논의한 대로, 원칙에 따른 의사 결정 시스템은 이를 관리하는 효과적인 방법입니다.

게다가 일반적으로 간과되는 측면은 현재 시대의 시각 중심 및 HD지도 없이 운전하는 시대에 특히 불확실하고 불완전한 지각 결과를 견딜 수 있는 계획이어야 한다는 것입니다. SD 지도를 선행으로 가지고 있는 것은 이 불확실성을 완화해 주지만, 여전히 강하게 수작업된 계획 시스템에 상당한 어려움을 야기합니다. 이 지각 불확실성은 리다 및 HD지도의 적극적인 활용을 통해 레벨 4 (L4) 자율 주행 회사들에 의해 해결된 문제로 간주되었습니다. 그러나 이 두 지주 없이 대량 생산 자율 주행 솔루션으로 이동하는 산업은 이 문제가 재부각되고 있습니다. NN 계획자는 주로 불완전하고 불완전한 지각 결과를 처리할 수 있어 대량 생산을 위한 시각 중심 및 HD지도 없는 고급 운전자 지원 시스템 (ADAS)에 필수적인 것입니다.

<div class="content-ad"></div>

상호 작용은 몬테 카를로 트리 탐색(Monte Carlo Tree Search, MCTS)나 MPDM의 간소화된 버전과 같은 원칙적인 의사 결정 시스템으로 다뤄져야 합니다. 주요 도전 과제는 자율 주행의 도메인 지식을 통해 스마트한 제거를 통해 균형있게 정책 트리를 성장시켜 조합폭발의 문제(차원의 저주)를 처리하는 것입니다. MPDM 및 해당 변형은 학계와 산업(Tesla 등)에서 이러한 균형있게 트리를 성장시키는 방법의 좋은 예를 제공합니다.

또한 신경망(NNs)은 모션 계획 최적화를 가속화하여 실시간 성능을 향상시킬 수 있습니다. 이렇게 하면 CPU에서 GPU로 컴퓨팅 부하를 옮기며 몇 배나 속도를 향상할 수 있습니다. 최적화 속도의 10배 증가는 MCTS와 같은 고수준 알고리즘 설계에 근본적인 영향을 미칠 수 있습니다.

궤적은 인간다운 모습을 더해야 합니다. 인간다운 속성 및 운전 대처 예측 모델은 이용 가능한 방대한 인간 운전 데이터를 사용하여 훈련될 수 있습니다. 컴퓨팅 풀을 확대하는 것이 지속적으로 늘어나는 엔지니어링 인재 대규모 유지보다 확장 가능성이 더 높습니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_36.png)

<div class="content-ad"></div>

# 엔드 투 엔드(End-to-End) 신경망(Neural Network) 플래너는 어떤가요?

엔드 투 엔드(End-to-End) 신경망(Neural Network) 플래너는 여전히 모듈식 자율 주행(AD) 디자인을 구성하며, 구조화된 인식 결과(그리고 잠재적으로 잠복한 특징)를 입력으로 받습니다. 이 접근 방식은 예측, 판단 및 계획을 단일 네트워크로 결합합니다. DeepRoute(2022)와 Huawei(2024)와 같은 기업들이 이 방법을 활용한다고 주장합니다. 중요한 생 센서 입력(예: 내비게이션 및 자동차 정보)은 여기서 생략되었습니다.

![이미지](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_37.png)

이 엔드 투 엔드 플래너는 더 나아가 인식과 계획을 모두 결합한 엔드 투 엔드 자율 주행 시스템으로 발전시킬 수 있습니다. Wayve의 LINGO-2(2024)와 Tesla의 FSDv12(2024)가 이를 성취한다고 주장하는 것입니다.

<div class="content-ad"></div>

이 접근 방식의 이점은 두 가지로 나뉩니다. 첫째, 이는 인식 문제에 대처합니다. 주로 사용되는 인식 인터페이스로 명확하게 모델링하지 못하는 운전의 여러 측면이 있습니다. 예를 들어, 물 웅덩이를 피하기 위해 주행 시스템을 손수 조정하거나 웅덩이나 틈새에 대비해 속도를 줄이는 것은 매우 어렵습니다. 중간 인식 기능을 통과할 수 있지만, 이는 근본적인 문제를 해결하지 못할 수도 있습니다.

더불어 발생하는 행동은 모든 상황을 보다 체계적으로 해결하는 데 도움이 될 것입니다. 위의 예시와 같이 경계 케이스를 지혜롭게 처리하는 것은 대규모 모델의 발생적 행동에서 나올 수 있습니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_38.png)

내 추측으로, 최종 형태의 엔드 투 엔드 (e2e) 운전자는 대규모 비전 및 액션-네이티브 멀티모달 모델이라고 생각됩니다. 계산 제약이 없다면 몬테 카를로 트리 탐색 (MCTS)로 강화할 것입니다.

<div class="content-ad"></div>

자율 주행의 세계 모델은 2024년을 기준으로 주로 비전 및 행동 모드(또는 VA 모델)를 다루는 다중 모달 모델입니다. 언어는 훈련 가속화, 제어 가능성 추가, 설명 가능성 제공에 도움이 될 수 있지만 필수적이지는 않습니다. 세계 모델의 최종 형태는 VLA(비전-언어-행동) 모델이 될 것입니다.

세계 모델을 개발하는 데는 적어도 두 가지 접근 방식이 있습니다:

- 비디오 네이티브 모델: 미래 비디오 프레임을 예측하도록 모델을 훈련시키고, 관련 행동에 조건을 걸거나 출력하는 방식으로, GAIA-1과 같은 모델에서 시연된 것처럼.
- 다중 모달 어댑터: 사전 훈련된 대형 언어 모델(LLM)을 시작으로 하고, Lingo-2, RT2, ApolloFM과 같은 모델에서 볼 수 있듯이 다중 모달 어댑터를 추가합니다. 이러한 다모달 LLM은 비전이나 액션에 원시적이지 않지만 훈련 리소스를 훨씬 적게 필요로 합니다.

세계 모델은 액션 출력을 통해 정책을 직접 생성하여 차량을 운전할 수 있게 합니다. 또는 MCTS는 세계 모델에 쿼리를 하고 해당 정책 출력을 사용하여 탐색을 안내할 수 있습니다. 이 세계 모델-MCTS 접근 방식은 계산이 많이 필요하지만 명시적 추론 논리로 인해 모서리 사례를 처리하는 능력이 높을 수 있습니다.

<div class="content-ad"></div>

## 예측 없이도 가능할까요?

현재의 대부분의 모션 예측 모듈은 중심 차량 이외의 요소들의 미래 궤적을 하나 이상의 이산적 궤적으로 표현합니다. 이 예측-계획 인터페이스가 충분하거나 필요한지 여전히 의문입니다.

고전적인 모듈식 파이프라인에서는 여전히 예측이 필요합니다. 그러나 예측 후 계획 파이프라인은 자율 주행 시스템의 상한선을 근거하며 의사 결정 세션에서 논의된 바 있습니다. 더 중요한 질문은 이 예측 모듈을 전체 자율 주행 스택에 더 효과적으로 통합하는 방법입니다. 예측은 의사 결정을 지원해야 하며 MPDM 및 그 변형본과 같은 전체적인 의사 결정 프레임워크 내에서 쿼리 가능한 예측 모듈이 선호됩니다. 정책 트리 롤아웃을 통해 정확하게 통합된 경우와 같이 구체적인 궤적 예측에 심각한 문제가 없습니다.

예측의 다른 문제는 평균 이동 오차 (ADE) 및 최종 이동 오차 (FDE)와 같은 열린 루프 핵심 성과 지표(KPI)가 계획에 미치는 영향을 반영하지 못하는 비효과적인 측정 기준입니다. 대신 의도 수준에서의 재현율과 정밀도와 같은 지표를 고려해야 합니다.

<div class="content-ad"></div>

In an end-to-end system, having a separate prediction module may not always be required. However, incorporating implicit supervision, along with other domain knowledge from a traditional stack, can definitely enhance or improve the data efficiency of the learning system. It is also beneficial to evaluate the prediction behavior, whether it is explicit or implicit, to help debug such an end-to-end system.

## Can we rely solely on neural networks without using tree structures?

Let's start with the conclusions. Neural networks (nets) can achieve very high, even superhuman, performance for an assistant role. In the case of agents, having a tree structure can still be advantageous, although not always mandatory.

First and foremost, trees can complement neural networks. Trees can elevate the performance of a network, whether it is based on neural networks or not. In the case of AlphaGo, despite training a policy network through supervised and reinforcement learning, the overall performance was still outperformed by the Monte Carlo Tree Search (MCTS)-based AlphaGo, which integrates the policy network as one of its components.

<div class="content-ad"></div>

둘째로, 나무를 증류할 수 있는 네트워크가 있습니다. 알파고에서 MCTS는 트리에서 노드(상태 또는 보드 위치)를 평가하기 위해 가치 네트워크와 빠른 롤아웃 정책 네트워크로부터 보상을 사용했습니다. 알파고 논문에는 가치 함수만 사용할 수 있지만, 두 가지 결과를 결합하는 것이 최상의 결과를 얻는다고 언급되었습니다. 가치 네트워크는 정책 롤아웃으로부터 지식을 증류하며 상태-가치 쌍을 직접 배우는 역할을 합니다. 이는 인간이 느린 시스템 2의 논리적 사고를 빠른 직관적인 시스템 1의 반응으로 증류하는 방식과 유사합니다. 다니엘 칸만의 “사고, 빠르고 느린” 책에서는 체스 마스터가 연습을 통해 빠르게 패턴을 인식하고 신속한 결정을 내리는 능력을 키울 수 있다고 설명합니다. 반면 초보자는 유사한 결과를 얻으려면 상당한 노력이 필요합니다. 마찬가지로 알파고의 가치 네트워크는 주어진 보드 위치의 신속한 평가를 제공하기 위해 훈련되었습니다.

![Image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_39.png)

최근 논문들은 신경망을 사용한 이 빠른 시스템의 상한선을 탐구합니다. “탐색 없는 체스” 논문은 충분한 데이터(기존 알고리즘을 사용하여 트리 탐색을 통해 준비된)로 그랜드마스터 수준의 능력을 달성할 수 있다는 것을 보여줍니다. 데이터 및 모델 크기와 관련된 명확한 “스케일링 법칙”이 있으며, 데이터 및 모델 복잡성이 증가함에 따라 시스템의 능력도 증가한다는 것을 나타냅니다.

그래서 우리는 나무가 네트를 향상시키고, 네트가 나무를 증류한다는 강력한 조합을 가지고 있습니다. 이 긍정적 피드백 루프는 알파제로가 다중 게임에서 초인적인 성과에 이르기 위해 부스트하는 데 사용하는 핵심 요소입니다.

<div class="content-ad"></div>

대형 언어 모델(Large Language Models, LLM) 개발에도 동일한 원칙이 적용됩니다. 게임에서 우리는 승리 또는 패배와 같이 명확히 정의된 보상을 가지고 있기 때문에 특정 작업이나 상태의 가치를 결정하기 위해 전방 롤아웃을 사용할 수 있습니다. 그러나 LLM에서는 보상이 바둑 게임처럼 명확하게 정의되어 있지 않기 때문에 인간의 선호도를 통해 모델을 평가하기 위해 인간 피드백을 활용한 강화 학습(RLHF)을 의존합니다. 그러나 ChatGPT와 같이 이미 훈련된 모델을 사용할 경우 감독된 세밀 조정(Supervised Fine-tuning, SFT)을 사용하여 여전히 강력한 모델을 RLHF 없이도 축소할 수 있습니다.

원래 질문으로 돌아가서, 대량의 고품질 데이터를 사용하면 네트워크는 매우 뛰어난 성능을 낼 수 있습니다. 이는 오류 허용에 따라 비서로서 충분할 수 있지만, 자율 에이전트로서 충분하지 않을 수도 있습니다. 주행 보조 시스템(Advanced Driver Assistance Systems, ADAS)을 대상으로 하는 시스템의 경우, 모방 학습을 통한 네트워크가 적합할 수 있습니다.

나무는 명시적 추론 루프를 통해 네트워크의 성능을 크게 향상시킬 수 있으며, 완전 자율 에이전트에게 더 적합할 수 있습니다. 나무 또는 추론 루프의 범위는 공학 리소스에 대한 투자 수익에 따라 다릅니다. 예를 들어, 상호 작용의 수준이 한 단계만 높아져도 TuSimple AI Day에서 확인된 대로 상당한 이점을 제공할 수 있습니다.

# LLM을 사용하여 결정을 내릴 수 있을까요?

<div class="content-ad"></div>

가장 뜨거운 인공 지능 시스템 대표들의 요약을 보면, LLMs는 의사 결정을 수행하도록 설계되지 않았다는 것을 알 수 있습니다. 본질적으로 LLMs는 문서 작성을 위해 훈련되었으며, 심지어 SFT와 일치시킨 LLM 보조 시스템은 대화를 특별한 종류의 문서로 취급합니다(대화 기록 완성).

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_40.png)

최근 LLMs가 느린 시스템(System 2)이라는 주장에 완전히 동의하지는 않습니다. 하드웨어 제약으로 추론에서 불필요하게 느릴 수는 있지만, 그들의 원형인 LLMs는 대안 가능성 확인을 수행할 수 없기 때문에 빠른 시스템입니다. Chain of Thought (CoT)나 Tree of Thoughts (ToT)와 같은 프롬프팅 기술은 실제로 MCTS의 단순화된 형태로, LLMs가 더 느린 시스템처럼 기능하도록 만듭니다.

LLMs에 완전한 MCTS를 통합하려는 많은 연구가 진행 중입니다. 구체적으로, LLM-MCTS(NeurIPS 2023)는 LLM을 상식적인 "세계 모델"로 취급하고, LLM으로 인한 정책 행동을 탐색을 안내하는 휴리스틱으로 사용합니다. LLM-MCTS는 복잡하고 새로운 작업에 대해 MCTS만큼이나 LLM에 의한 정책보다 훨씬 우월한 성능을 보입니다. OpenAI의 높은 관심을 받고 있는 Q-star도 LLMs를 MCTS로 강화하는 동일한 접근 방식을 따르는 것으로 보입니다.

<div class="content-ad"></div>

# 진화의 흐름

자율주행에서의 계획 스택이 발전하는 과정을 간략하게 소개해드리겠습니다. 이는 상기된 솔루션이 위에 나열된 것들보다 반드시 더 발전된 것은 아니며, 그들의 등장은 정확한 연대순서를 따르지 않을 수 있기 때문에 초안입니다. 그럼에도 불구하고, 우리는 일반적인 추세를 관찰할 수 있습니다. 산업에서 소개된 대표적인 솔루션들은 다양한 보도 자료를 토대로 제 해석을 통해 나열하였으며, 오류의 가능성이 있음을 알려드립니다.

하나의 추세는 보다 종단간 디자인으로의 이동입니다. 여러 모듈이 하나로 통합된 디자인으로 스택이 경로-속도 분리 계획에서 공간-시간적 공동계획으로 진화되고, 예측 후 계획 시스템에서 공동 예측 및 계획 시스템으로 진화합니다. 다른 하나의 추세는 머신러닝 기반 컴포넌트의 증가로, 특히 마지막 세 단계에서 나타납니다. 이 두 가지 추세는 종단간 뉴럴네트워크(NN) 플래너(인식 없음)로 수렴하거나 심지어 종단간 NN 운전자(인식 포함)로 나아갑니다.

![image](/assets/img/2024-06-30-ACrashCourseofPlanningforPerceptionEngineersinAutonomousDriving_41.png)

<div class="content-ad"></div>

# 핵심 포인트

- ML은 도구로서의 역할: 머신 러닝은 독립적인 해결책이 아닌 도구로서의 역할을 합니다. 현재의 모듈식 설계에도 계획 수립에 도움을 줄 수 있습니다.
- 완전한 문제 정식화: 문제의 완전한 정식화로 시작한 후 성능과 자원의 균형을 맞추기 위해 합리적인 가정을 하세요. 이는 미래 지향적인 시스템 설계를 만들어주고 자원이 증가함에 따라 개선되도록 합니다. POMDP의 정식화에서 AlphaGo의 MCTS 및 MPDM과 같은 엔지니어링 솔루션으로의 전환을 상기해 보세요.
- 알고리즘 적응: 이론적으로 아름다운 알고리즘(Dijkstra 및 Value Iteration 등)들은 개념을 이해하는 데에 큰 도움이 되지만 실용적 엔지니어링에 적응이 필요합니다 (Value Iteration을 MCTS로, Dijkstra의 알고리즘을 Hybrid A-star로). 
- 결정론적 vs. 확률적: 계획은 결정론적(필연적으로 정적이지는 않음)인 상황을 해결하는 데 강점을 보입니다. 확률적 상황에서의 결정은 완전 자율주행으로 나아가는 데 가장 어려운 과제입니다.
- 비상 대비 계획: 여러 가지 미래를 공통 조치로 병합하는 데 도움이 됩니다. 항상 대비 계획으로 되짚을 수 있도록 적극적으로 대응하는 것이 유익합니다.
- End-to-end 모델: End-to-end 모델이 완전 자율주행을 해결할 수 있는지 여전히 불분명합니다. MCTS와 같은 고전적인 방법이 여전히 필요할 수 있습니다. 신경망은 보조 역할을 다룰 수 있으며, 트리는 에이전트를 관리할 수 있습니다.

# 감사의 글

- 본 블로그 글은 Dr. Wenchao Ding의 Shenlan Xueyuan (深蓝学院)에서의 계획 강좌에 큰 영감을 받았습니다.
- Naiyan Wang 및 Jingwei Zhao와의 적극적이고 영감을 주는 토론에 감사드립니다. 초기 초안에 중요한 피드백을 주신 论文推土机, Invictus, XXF, PYZ 및 JCL에게 감사드립니다. 학계의 동향에 관한 Insightful한 토론을 했던 버클리 대학의 Wei Zhan 교수에게도 감사드립니다.

<div class="content-ad"></div>

# 참고 자료

- 자율 주행의 최종 계획: 산업과 학계에서의 2022-2023, Arxiv 2024
- BEVGPT: 자율 주행 예측, 의사 결정 및 계획을 위한 대규모 사전 훈련된 모델, AAAI 2024
- 유체 역학을 활용한 자율 주행을 위한 일반적인 운동 계획에 대한 연구, Arxiv 2024
- Bilibili에서 영어 자막이 제공되는 중국어로 Tusimple AI day, 2023년 07월
- Zhihu에서 공개된 중국어에서의 Qcraft의 공간시간 연관 계획 기술 블로그, 2022년 08월
- Zhihu에 공개된 중국어에서 전체 자율 주행 스택에 대한 리뷰, 2018년 12월
- Zhihu에 공개된 중국어에서 Tesla AI Day Planning, 2022년 10월
- Tsinghua AIR이 작성한 중국어 ApolloFM 기술 블로그, 2024년
- Frenet Frame에서 동적 거리 상황을 위한 최적 궤적 생성, ICRA 2010
- 이미지 인코딩을 위한 Lift, Splat, Shoot: 임의의 카메라 장치에서 3D로 암시적으로 투영, ECCV 2020
- CoverNet: 궤적 세트를 사용한 다중 모달 행동 예측, CVPR 2020
- 2020년 CVPR에서 발표된 NMP: 최종 해석 가능한 신경 움직임 플래너
- 2021년 CVPR에서 발표된 MP3: 매핑, 인식, 예측 및 계획을 위한 통합 모델
- Baidu Apollo EM Motion Planner, 2018년, Baidu
- AlphaGo: 깊은 신경망과 트리 탐색을 통해 바둑을 마스터함, Nature 2016
- AlphaZero: 자가 대국을 통해 체스, 쇼기 및 바둑을 마스터하는 일반적인 강화 학습 알고리즘, Science 2017
- MuZero: 배워진 모델을 사용하여 Atari, 바둑, 체스 및 쇼기를 마스터하는 것, Nature 2020
- NeurIPS 2023 Oral 논문인 ToT: Tree of Thoughts: 대규모 언어 모델을 통한 의도적 문제 해결
- NeurIPS 2022에서 발표된 CoT: Chain-of-Thought 프롬프팅이 대규모 언어 모델에서 추론을 유도함
- NeurIPS 2023에서의 LLM-MCTS: 대규모 언어 모델을 통한 상식적 지식의 대규모 작업 계획
- ICRA 2015의 MPDM: 자율 주행을 위한 동적, 불확실한 환경에서의 다중 정책 의사 결정
- RSS 2015의 MPDM2: 자율 주행을 위한 Changepoint 기반 행동 예측을 통한 다중 정책 의사 결정
- RSS 2017의 MPDM3: Changepoint 기반 행동 예측을 통한 자율 주행을 위한 다중 정책 의사 결정: 이론 및 실험
- ICRA 2020의 EUDM: 안내 분기를 통한 자동주행을 위한 효율적인 불확실성 인식 의사 결정
- RAL 2023의 MARC: 자율 주행을 위한 다중 정책 및 위험 인지 대비 계획
- TRO 2021의 EPSILON: 고도 상호 작용 환경에서의 자동차를 위한 효율적인 계획 시스템