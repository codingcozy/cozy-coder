---
title: "수작업으로 배우는 Multi-Headed Self Attention 이해하기"
description: ""
coverImage: "/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_0.png"
date: 2024-07-13 02:51
ogImage: 
  url: /assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_0.png
tag: Tech
originalTitle: "Multi-Headed Self Attention — By Hand"
link: "https://medium.com/towards-data-science/multi-headed-self-attention-by-hand-d2ce1ae031db"
isUpdated: true
---






![Multi-Headed Self Attention](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_0.png)

Multi-Headed Attention is probably one of the most crucial architectural paradigms in machine learning. This summary delves into all the crucial mathematical operations involved in multi-headed self-attention, enabling you to grasp its inner workings at a fundamental level. If you want to explore the intuition behind this topic further, be sure to check out the IAEE article.

## Step 1: Defining the Input

Multi-headed self-attention (MHSA) is utilized in various contexts, each potentially formatting the input differently. In a natural language processing scenario, one would likely employ word-to-vector embedding along with positional encoding to derive a vector representing each word. In general, irrespective of the data type, multi-headed self-attention requires a sequence of vectors, with each vector representing something.


<div class="content-ad"></div>

### Step 2: 학습 가능한 매개변수 정의

다머리 self attention은 기본적으로 세 개의 가중치 행렬을 학습합니다. 이들은 나중에 메커니즘에서 사용되는 "쿼리", "키", 및 "값"을 구성하는 데 사용됩니다. 이 예제에서 사용할 몇 가지 가중치 행렬을 정의할 수 있습니다. 이는 모델의 학습 가능한 매개변수를 나타내는 것으로, 처음에는 임의로 정의되고, 그 후에 훈련 과정에서 업데이트됩니다.

![이미지](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_2.png)

<div class="content-ad"></div>

# 스텝 3: 쿼리, 키, 밸류 정의하기

이제 모델을 위한 가중치 행렬이 준비되었으니, 이를 입력값과 곱하여 쿼리, 키, 밸류를 생성할 수 있습니다. 행렬 곱셈에서 각 행의 값은 첫 번째 행렬에서 해당되는 열의 값과 곱해집니다. 이 곱해진 값들은 하나의 출력을 표현하기 위해 합산됩니다.

![이미지](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_3.png)

키와 밸류 행렬을 구성하는 데에도 동일한 연산이 사용되며, 그렇기에 쿼리, 키, 밸류를 구성하였습니다.

<div class="content-ad"></div>


# Step 4: Dividing into Heads

이 예제에서는 두 개의 self attention head를 사용할 것입니다. 즉, 입력의 두 하위 표현으로 self attention을 수행할 것입니다. 이를 위해 query, key 및 value를 두 부분으로 나누어 설정할 것입니다.

![Step 5](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_5.png)

<div class="content-ad"></div>

# 단계 5: Z 행렬 계산하기

주의 1을 가진 쿼리, 키 및 값은 첫 번째 주의 헤드로 전달될 것이고, 라벨 2와 함께 있는 쿼리, 키 및 값은 두 번째 주의 헤드로 전달될 것입니다. 기본적으로, 이는 다양한 방식으로 동시에 동일한 입력에 대해 이유를 생각할 수 있도록 하는 다중 헤드 셀프 어텐션을 가능케 합니다.

**Z 행렬 계산하기**

주의 매트릭스를 구성하기 위해, 먼저 쿼리와 키를 곱해서 일반적으로 "Z" 행렬이라고 하는 것을 만들 것입니다. 이를 주의 헤드 1에만 적용할 것이지만, 이 모든 계산이 주의 헤드 2에서도 진행되고 있음을 명심하세요.

<div class="content-ad"></div>

수학 계산 방식 때문에 "Z" 매트릭스의 값이 쿼리 및 키의 크기가 커질수록 증가하는 경향이 있습니다. 이는 "Z" 매트릭스의 값들을 시퀀스 길이의 제곱근으로 나눔으로서 상쇄됩니다.

![image](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_7.png)

## 단계 6: 마스킹

마스킹이 어떻게 나타나는지는 어텐션 메커니즘의 적용에 따라 다릅니다. 텍스트 시퀀스에서 다음 단어를 예측하는 언어 모델의 경우, 우리는 주어진 마스크를 적용하여 어텐션 메커니즘이 이전 단어들과 상관관계를 맺게 하고 향후 단어들과는 그렇지 않게 합니다. 이는 중요한데, 언어 모델이 시퀀스에서 다음 단어를 예측하려고 할 때, 훈련 중일 때 그 단어를 보면 안 되기 때문입니다. 모델이 마스크와 함께 훈련되었으므로 추론도 마스크와 함께 진행됩니다. 이는 키 값을 -∞로 대체함으로써 이루어집니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_8.png)

# Step 7: Attention Matrix Calculation

Calculating the “Z” matrix and applying a mask of -∞ were essential steps in creating the attention matrix. To obtain this, we employ softmax on each row of the masked “Z” matrix. The formula for softmax calculation is represented below:

![image](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_9.png)


<div class="content-ad"></div>

각 행의 값이 해당 값으로 제곱한 e를 ​​해당 행의 모든 값에 제곱한 e의 합으로 나눈 값과 같습니다. 첫 번째 행을 소프트맥스할 수 있습니다:

![image](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_10.png)

이것은 다소자명한데요, -∞이 아닌 하나의 값만 있습니다. 두 번째 행을 소프트맥스 해 봅시다.

![image](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_11.png)

<div class="content-ad"></div>

그리고 각 행의 소프트맥스를 계산하여 주의 행렬을 계산합니다.

![이미지](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_12.png)

# 단계 8: 주의 헤드의 출력 계산

이제 주의 행렬을 계산했으니, 값 행렬과 곱하여 주의 헤드의 출력을 구성할 수 있습니다.

<div class="content-ad"></div>

![multi-headed self-attention output](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_13.png)

# 진행단계 9: 결과 연결하기

각 어텐션 헤드는 고유한 출력을 생성하며, 이를 연결하여 다중 헤드 셀프 어텐션의 최종 출력을 생성합니다.

![다중 헤드 셀프 어텐션의 최종 출력](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_14.png)

<div class="content-ad"></div>

# 결론

그게 다야. 이 글에서는 YOLO의 출력을 계산하는 주요 단계들을 다루었어:

- 입력 정의하기
- 메커니즘의 학습 가능한 매개변수 정의하기
- Query, Key, Value 계산하기
- 여러 개의 헤드로 분기하기
- Z 매트릭스 계산하기
- 마스킹
- 어텐션 매트릭스 계산하기
- 어텐션 헤드의 출력 계산하기
- 출력을 연결하기

이론을 이해하고 싶다면 IAEE 글을 확인해보세요:

<div class="content-ad"></div>

# Join IAEE

이 곳에서 IAEE는 다음을 찾을 수 있어요:

- 방금 읽은 글처럼 긴 형식의 콘텐츠
- 데이터 과학자, 엔지니어링 디렉터, 기업가로서의 제 경험을 기반으로 한 사상적인 글
- AI 학습에 초점을 맞춘 디스코드 커뮤니티
- 저에 의한 매주 강의

![Multi-HeadedSelfAttentionByHand](/assets/img/2024-07-13-Multi-HeadedSelfAttentionByHand_15.png)