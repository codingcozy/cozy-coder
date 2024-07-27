---
title: "게임 세계 마스터하기 강화 학습 심층 탐구"
description: ""
coverImage: "/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_0.png"
date: 2024-07-01 17:51
ogImage: 
  url: /assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_0.png
tag: Tech
originalTitle: "Mastering Game Worlds: A Deep Dive into Reinforcement Learning"
link: "https://medium.com/gitconnected/mastering-game-worlds-a-deep-dive-into-reinforcement-learning-5acccad89a18"
---


요즘 AI 기술은 바둑이나 체스와 같은 게임에서 놀라운 성과를 보여주며 세계 최고인 인간 선수들을 능가하는 모습을 보여주고 있습니다. 이런 업적들은 기술의 급속한 발전을 강조합니다.

이 분야에서의 중요한 발전 중 하나는 'ChatGPT'입니다. ChatGPT는 대인적 언어를 이해하고 활용하는 능력으로 유명한 큰 언어 모델입니다.

ChatGPT는 대체로 transformer와 감독 학습 기술에 기반을 두고 있지만, 인간 피드백으로 강화 학습을 통해 세밀하게 튜닝되었습니다. 이는 기계가 자신의 행동에서 배우고 경험에 기초해 개선하는 방법을 보여줍니다.

이 글은 두 부분으로 나뉘어질 예정입니다. 이 부분에서는 강화 학습의 기본 이론과 PyTorch에서 REINFORCE 방법을 사용하여 Flappy Bird 게임을 플레이하는 동안의 과정을 살펴보겠습니다.

<div class="content-ad"></div>

다음 부분에서는 어드밴티지 액터-크리틱(A2C)과 현재 최신 알고리즘인 PPO와 같은 더 복잡한 강화 학습 알고리즘을 더 알아볼 거에요! 😃

## 목차

- 강화 학습
- 환경
- 에이전트
- 보상
- 가치 함수
- Q 함수
- 정책 기반 방법
- REINFORCE
  - REINFORCE의 단계
  - 구현
  - BirdAgent 및 훈련
- 결론
- 참고문헌

# 강화 학습

<div class="content-ad"></div>

강화 학습은 특정 목표를 달성하거나 성능 메트릭을 최대화하기 위해 환경과 상호 작용을 통해 행동 전략을 학습하는 기계 학습 방법입니다. 이는 환경, 에이전트, 보상 세 가지 주요 요소로 구성되어 있습니다.

![이미지](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_0.png)

- 에이전트는 환경으로부터 상태 (s_t)를 수신합니다
- 해당 상태를 기반으로, 에이전트는 환경에 행동 (a_t)을 제공합니다
- 환경은 새로운 상태 (s_t+1)로 이동하고 일부 보상 (r_t+1)을 에이전트에 제공합니다

# 환경

<div class="content-ad"></div>

환경은 에이전트와 상호 작용하는 시스템입니다. 에이전트가 만날 수 있는 모든 가능한 상태, 해당 상태에서 취할 수 있는 행동 및 그러한 행동의 결과를 정의합니다.

환경은 에이전트의 행동에 대응하여 새로운 상태와 보상 정보를 제공함으로써 에이전트의 학습 과정을 이끌어내게 됩니다. 환경은 다음과 같은 정보를 정의합니다:

- 행동 공간: 에이전트가 취할 수 있는 가능한 행동 집합을 정의합니다.
- 관찰 공간: 에이전트가 관찰할 수 있는 모든 가능한 상태를 정의합니다. 이러한 상태는 에이전트가 다음 행동을 결정하기 위해 필요한 정보를 제공합니다. 상태 공간은 유한하거나 무한할 수 있으며 연속적이거나 이산적일 수 있습니다.
- 보상 함수: 보상 함수는 에이전트의 행동에 대한 즉각적인 평가(보상)를 제공하며, 에이전트가 그 전략을 조정하여 더 높은 총 보상을 얻는 방법을 학습하도록 이끕니다.
- 전이 역학: 전이 역학은 주어진 현재 상태 및 특정 행동 이후 환경이 새로운 상태로 이동할 확률을 설명합니다.

# 에이전트

<div class="content-ad"></div>

강화학습에서 에이전트는 환경을 관찰하고 결정을 내리며 행동을 실행할 수 있는 엔티티를 가리킵니다. 에이전트의 목표는 장기적으로 얻는 보상을 극대화하기 위해 주어진 환경 상태에서 최적의 행동을 선택하는 방법인 전략(정책)을 학습하는 것입니다.

## 보상

보상은 에이전트의 학습 프로세스를 안내하는 주요 신호로, 행동에 대한 피드백을 제공합니다. 이러한 보상은 긍정적이거나 부정적일 수 있으며, 에이전트가 목표를 달성하는 데 행동의 결과를 이해하는 데 도움을 줍니다.

에이전트의 주요 목표는 다양한 상태에서 최적의 행동을 선택하기 위한 전략(정책)을 학습하여 시간이 흐름에 따라 총 보상을 최대화하는 것입니다.

<div class="content-ad"></div>


![Image Description](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_1.png)

When we talk about Gt in reinforcement learning, we are referring to the total reward at timestep t, which is also known as the return. It is calculated by summing up the rewards from timestep t until the end of the episode (T). This total reward is adjusted with a discount rate, γ, which determines the significance of future rewards in relation to the reward at timestep t.

The discount rate γ ranges between 0 and 1. For instance, if we consider γ as 0.95, rewards r_t expected in the distant future are discounted by a factor of 0.95 raised to the power of (t-1).

This adjustment is made because the likelihood of achieving a state far into the future is lower, making the corresponding reward less impactful and, therefore, subject to a reduction for a practical assessment.


<div class="content-ad"></div>

![Image](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_2.png)

여기 보상을 계산하는 간단한 예제가 있어요. '뱀'과 '목표' 이미지는 각각 초기 상태와 종단 상태를 나타냅니다. 이제 목표에 도달하기 위한 두 가지 가능한 경로가 있습니다:

경로 1: 직진해서 위로 올라가서 왼쪽으로 목표로 이동

경로 2: 오른쪽으로 가서 위로 올라가서 목표로 이동

<div class="content-ad"></div>

할인율 γ = 1일 때,

- R(Path 1) = 2 + γ(-3) + γ²(0) + γ³(1) = 0
- R(Path 2) = 1 + γ(0) + γ²(3) + γ³(0) = 4

우리는 목표에 도달할 수 있는 서로 다른 경로가 있음을 볼 수 있지만, 각 경로가 매우 다른 보상 (0과 4)을 가져올 수 있다는 것을 알 수 있습니다. 따라서 총 보상을 극대화할 수 있는 최상의 경로를 찾는 것이 강화 학습(RL)의 목표입니다. 💪

# 가치 함수

<div class="content-ad"></div>

우리의 상태가 얼마나 잘 되어 있는지를 정의하려면 이를 결정하는 함수인 가치 함수가 필요합니다.

본질적으로 이 함수는 그 상태에 있을 때의 예상 수익을 추정합니다. 이 함수는 특정 정책(에이전트가 따르는 전략) 하에서 특정 상태에 있을 때 장기적 이점을 제공합니다.

![Image](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_3.png)

위에서 정의된 가치 함수는 재귀 형태로 재평가될 수 있으며, 이는 상태 값의 재귀적 계산을 가능하게 합니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_4.png)

In the value function, we utilize the expected value of the transition probability P(a ,s→s′) to accommodate real-world situations where the outcome (state s′) might differ when the same action (a) is taken in the same state (s).

The symbol π represents the agent's policy, with π(a | s) indicating the probability of the agent choosing action a in state s.

Now, let's engage in some practice~~ 🍎

<div class="content-ad"></div>

### 이미지
![Mastering Game Worlds: A Deep Dive into Reinforcement Learning](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_5.png)

이 상황에서는 네 가지 상태가 있습니다. s1은 시작 상태이고 s2, s3 및 s4는 종료 상태입니다. 두 가지 동작, 'left' 또는 'right'를 선택할 수 있으며 각각의 상태 간의 전이 확률을 가집니다.

## 다른 정책 하에서 상태 1의 가치:

<div class="content-ad"></div>

- V(π = 항상 왼쪽) = 0.5 * 2 + 0.5 * 4 = 3
- V(π = 항상 오른쪽) = 0.33 * 3 + 0.67 * 4 = 3.67
- V(π = 50% 왼쪽, 50% 오른쪽) = 0.5 * V(왼쪽) + 0.5 * V(오른쪽) = 3.335

이 예제는 전이 확률을 고려하여 (예: 행동 = 오른쪽: s3로 33%, s4로 67%) 동일한 행동이 매번 동일한 결과를 보장하지 않는 확률적 전이를 강조합니다.

더 복잡한 예제를 살펴보겠습니다~~

![MasteringGameWorldsADeepDiveintoReinforcementLearning_6](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_6.png)

<div class="content-ad"></div>

간단히 말씀드리면 이 예에서는 전이 확률을 무시하고 에이전트가 처음에 '위' 또는 '아래' 작업을 선택할 수 있으며, 이후 상태에서는 에이전트가 항상 오른쪽으로 이동한다.

(S1은 시작 상태이고, S6은 종단 상태이며, 종단 상태 값은 0)

π = 항상 위로, γ = 1인 경우

- V(s6) = 0
- V(s3) = V(s5) = 10 + γV(s6) = 10
- V(s2) = 2 + γV(s3) = 12
- V(s4) = 4 + γV(s5) = 14
- V(s1) = 1 + γV(s2) = 13

<div class="content-ad"></div>

For 이타 = (50% 상승, 50% 하락), 감마 = 1

- V(s6) = 0
- V(s3) = V(s5) = 10 + 감마V(s6) = 10
- V(s2) = 2 + 감마V(s3) = 12
- V(s4) = 4 + 감마V(s5) = 14
- V(s1) = (1 + 감마V(s2)) * 0.5 + (2 + 감마V(s4)) * 0.5 = 9.5

# Q 함수

실제로, 모든 상태의 가치가 알려진 경우, 에이전트가 각 타임스텝마다 가장 높은 가치의 상태로 이동하도록하여 누적 보상을 최대화할 수 있습니다.

<div class="content-ad"></div>

하지만, 상태값을 계산하기 위해서는 전이 확률에 대한 지식이 필요한데, 이는 대게 알 수 없습니다. 그러므로 상태-행동 함수(Q 함수)를 사용하는 것이 더 나은 선택이 됩니다.

Q 값은 경험을 통해 직접 학습할 수 있기 때문에 환경의 전이 확률을 알 필요가 없습니다.

![이미지](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_7.png)

Q 함수, 또는 행동-가치 함수는 상태 s에서 정책 π에 따라 행동 a를 취하는 가치를 정의합니다. Q 함수와 가치 함수의 차이는 Q 함수는 결정론적 행동의 가치를 특정하며, 이로써 정책 π(a∣s)에서 행동의 확률을 계산할 필요가 없어집니다.

<div class="content-ad"></div>


![Image 8](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_8.png)

The value function could also be seen as the sum of the Q function for all possible actions a.

![Image 9](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_9.png)

In a value-based approach like Q-learning, a policy that always picks the action with the highest Q value for each state s ensures automatic maximisation of the total reward.


<div class="content-ad"></div>

하지만 행동 공간이 매우 크거나 연속적인 경우, 모든 가능한 Q값을 계산하여 각 상태에 대한 최상의 행동을 식별하는 것은 현실적이지 않습니다. 😢

Policy-Based Methods

![Policy-Based Methods](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_10.png)

강화 학습의 정책 기반 방법은 가치 기반 방법(value-based methods)처럼 명시적으로 가치 함수를 계산하지 않고, 에이전트가 행동을 선택하는 정책을 직접 매개변수화하고 최적화합니다. 🌟

<div class="content-ad"></div>

**카드 읽기 전문가입니다!**

폴리시는 π(a∣s;θ)로 표시되며, 상태를 액션에 대한 확률 분포로 매핑하는 함수입니다. 여기서 θ는 폴리시의 매개변수를 나타내며 (예: 신경망의 가중치), 

# REINFORCE

REINFORCE는 폴리시 그레디언트 방법 중 하나로, 폴리시를 최적화하는 방법입니다. 이 방법은 기대되는 반환의 그레디언트를 폴리시 매개변수에 대해 추정한 다음 해당 매개변수를 그레디언트 하강을 사용하여 조정합니다.

![이미지](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_11.png)

<div class="content-ad"></div>

**리인포스 단계**

리인포스 방법에서는 고려된 행동의 품질을 나타내는 반환 (R(τ))에 비례하는 그래디언트 스케일이 사용됩니다. 한편, 그래디언트 자체는 해당 행동을 취할 확률의 로그로 표현됩니다.

이 방법은 높은 반환을 제공하는 행동의 확률을 높이고, 낮은 반환을 가져오는 행동의 확률을 줄이려고 합니다.

REINFORCE 방법에서는 실제 샘플을 사용하여 반환을 계산해야 합니다. 또한 Monte-Carlo 방법으로 얻은 전체 경로가 실행에 필요합니다.

<div class="content-ad"></div>

- 무작위 가중치로 네트워크 초기화하기
- N개의 전체 에피소드를 실행하여 궤적 (s, a, r, s') 수집하기
- 각 시간 단계마다 반환값 계산하기
- 손실 함수 L = -R(τ)logπ(a|s) 계산하기
- 모델 가중치 업데이트하기

## 구현

첫 번째 구현에서는 Gym 환경을 사용하여 FlappyBird 게임을 플레이합니다.

![Mastering Game Worlds](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_12.png)

<div class="content-ad"></div>

게임을 시작하기 전에 환경의 세부 사항을 이해하는 것이 매우 중요합니다. 구현하기 전에 매번 문서를 꼼꼼히 읽어주십시오.

![image](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_13.png)

관측값은 LIDAR 센서에 의해 수집된 180개의 데이터 포인트로 구성된 1차원 배열입니다. 에이전트는 아무 것도 하지 않는 것을 나타내는 0 또는 플래핑하는 것을 나타내는 1과 같은 두 가지 가능한 조치를 취할 수 있습니다.

![image](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_14.png)

<div class="content-ad"></div>

You're also welcome to consider the entire game screen as the observation for the model. Check [here](#) for more details.

## BirdAgent & Training

The BirdAgent plays a key role in decision-making based on the environment's state. It generates a probability distribution showing the chances of taking each available action.

Hence, the agent's number of actions, known as n_action, is equal to the environment's action space, set at 2.

<div class="content-ad"></div>

마침내, 훈련 루프가 여기 있어요~~ ✋

![Training Loop](https://miro.medium.com/v2/resize:fit:1400/1*_mmbMZnPoe96JeB90OrPsQ.gif)

# 결론

이 글에서는 보상 학습의 기본 원리인 가치 함수, Q 함수 등을 간단히 소개했습니다. 또한 정책 기울기 방법에서 REINFORCE 알고리즘을 소개하고 Flappy Bird 게임을 플레이하는 데 사용했습니다.

<div class="content-ad"></div>

이제 다음으로, A2C와 PPO라는 두 가지 정책 그래디언트 방법을 소개하고, Car Racing과 Snake 게임 두 가지에서 이를 구현해 보겠습니다.

마지막으로, 본 글이 즐거우셨기를 바랍니다. AI 관련 기사를 더 많이 작성할 예정이며, 그 기본 원리를 설명하고 구현하는 방법 등을 다룰 것입니다.

관심이 있으시다면, 제 팔로우 해주시면 감사하겠습니다. 👏 😁

제 Medium: [link](link)

<div class="content-ad"></div>

마이 리플 링크: [링크](link)

![이미지](/assets/img/2024-07-01-MasteringGameWorldsADeepDiveintoReinforcementLearning_15.png)

# 참고 자료

- Hugging Face: Deep RL Course
- OpenAI Spinning Up: Introduction to RL