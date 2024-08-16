---
title: "2024년에 선택해야 할 오픈소스 LLM은"
description: ""
coverImage: "/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_0.png"
date: 2024-07-10 00:32
ogImage: 
  url: /assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_0.png
tag: Tech
originalTitle: "Which Open-Source LLM Should You Choose in 2024?"
link: "https://medium.com/towards-artificial-intelligence/which-open-source-llm-should-you-choose-in-2024-c3901ce02271"
isUpdated: true
---





<img src="/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_0.png" />

안녕하세요! 2017년 논문 "Attention Is All You Need"에서 Transformer 아키텍처가 개발되었을 때부터 자연어 처리(NLP)는 엄청난 성장을 이루었습니다. 그리고 2022년 11월에 ChatGPT가 출시되면서 대형 언어 모델(Large Language Models, LLMs)이 모두의 관심을 사로잡았죠.

당신의 사용 사례에 LLM을 사용하고 있지만 모든 프롬프트에 비용을 지불하고 싶지 않다면 이 기사가 도움이 될 것입니다. 이 기사는 2024년의 LLM 현황을 이해하는 데 도움이 되며, 여러분의 사용 사례에 적합한 오픈 소스 모델을 선택하는 데 도움이 될 것입니다.

# Transformer 모델

<div class="content-ad"></div>

본래 Transformer 아키텍처는 두 부분으로 나뉩니다: 왼쪽에는 인코더가 있고 오른쪽에는 디코더가 연결되어 있습니다.

인코더는 입력 단어를 깊은 벡터 표현으로 변환하는 역할을 합니다. 디코더의 역할은 새로운 단어를 생성하는 것입니다.

![이미지](/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_1.png)

먼저 입력 문장을 토큰화해야 합니다; 즉, 단어(문자열)를 토큰(숫자)에 매핑해야 합니다. 예를 들어, 단어 "the"를 토큰 342로 매핑할 수 있습니다.

<div class="content-ad"></div>

토큰들은 고차원 임베딩 벡터로 변환됩니다. 비슷한 단어 임베딩은 이 고차원 벡터 공간에서 서로 가깝게 위치합니다. 따라서, 우리의 토큰 번호 342는 512차원 벡터로 인코딩됩니다.

문장에 포함된 단어 순서를 보존하기 위해 임베딩 벡터에 위치 인코딩이 추가됩니다. 이것은 셀프 어텐션 메커니즘에 중요합니다. 어텐션 레이어는 문장 내의 단어 간의 관계를 캡처합니다. 예를 들어, 문장에서의 동사는 주어에 속합니다.

마지막으로, 디코더는 우리 사전의 각 토큰에 대한 출력 확률 값을 생성합니다. 따라서, 디코더의 각 반복에서 우리는 가장 가능성 있는 다음 단어를 선택할 수 있습니다.

# LLM의 진화: 현재 기술 수준

<div class="content-ad"></div>

Yang 및 그 동료들은 최신 LLM의 진화를 나무로 기록했으며, 해당 모델들을 아키텍처 선택과 오픈 소스 여부에 따라 분류했습니다. 최신 정보는 https://github.com/JingfengYang/LLMsPracticalGuide에서 확인할 수 있습니다.

2021년 이후, 새로운 LLM 중 대부분은 디코더 전용 LLM입니다. Microsoft는 많은 인코더-디코더 모델을 공개했지만, 대부분의 다른 기업은 디코더 전용 모델에 초점을 맞추고 있습니다.

디코더 전용 LLM(예: GPT-4)는 자기 회귀 언어 모델이라고도 불립니다. 입력 단어에 대해 다음 단어를 예측하여 사전 훈련되었습니다. 디코더 전용 LLM은 텍스트 생성에 가장 적합합니다.

![이미지](/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_2.png)

<div class="content-ad"></div>

인코더-디코더 모델은 일련의 입력을 일련의 출력으로 변환하는 모델로도 불립니다. 사전 훈련 중에는 주어진 텍스트의 일부 단어가 가려지고, 모델은 그 가려진 단어를 예측해야 합니다. 이론적으로, 인코더-디코더 언어 모델은 번역, 텍스트 요약 및 생성적 질문 응답과 같은 작업에 가장 적합합니다.

![image](/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_3.png)

# 최근 오픈 소스 모델

아래 표에는 최근 오픈 소스 트랜스포머 모델에 대한 요약과 추가 정보가 포함되어 있습니다. 이러한 모든 언어 모델은 다운로드하여 로컬에서 사용할 수 있습니다. 대부분의 모델은 허깅페이스의 Transformers API에서 제공됩니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_4.png)

에를 들어, 질문에 대답하는 챗봇을 원한다면, 이미 지침에 맞게 튜닝된 모델을 선택하고 모델 크기가 하드웨어에 맞는지 확인해야 합니다.

또한 과제별 모델도 있습니다. 예를 들어, 과학 지식으로 훈련된 "Galactica"나 Python 코드에 특화된 "Code Llama Python" 등이 있습니다.

# 모델 크기

<div class="content-ad"></div>

파라미터 수와 양자화는 LLM 사용의 주요 제약사항으로 모델의 크기를 결정합니다.

![Image](/assets/img/2024-07-10-WhichOpen-SourceLLMShouldYouChoosein2024_5.png)

LLM을 사용하려면 모델을 메모리에 맞춰야 합니다. 32비트 부동 소수점(FP32)을 사용하면 1개의 파라미터당 4바이트의 RAM이 필요합니다.

16비트 양자화(BFLOAT16 또는 FP16)를 사용하면 1개의 파라미터당 2바이트로 줄일 수 있습니다.

<div class="content-ad"></div>

8비트 정수(INT8)를 사용할 경우, 매개변수 당 1바이트의 RAM이 필요합니다.

따라서 LLM의 10억(10억)개의 매개변수를 메모리에 저장하려면 약 4GB의 메모리가 32비트 완전 정밀도에 필요하며, 16비트 반정밀도에는 2GB, 8비트 정밀도에는 1GB가 필요합니다.

예를 들어, 내 GeForce 2060 그래픽 카드에는 6GB의 메모리가 있어 32비트로 약 15억 개의 매개변수, 16비트로 약 30억 개의 매개변수 또는 8비트로 약 60억 개의 매개변수를 저장할 수 있습니다.

그러나 CUDA 커널을 로드하는 것만으로도 1~2GB의 메모리를 사용할 수 있습니다. 따라서 실제로는 매개변수로 GPU 메모리 전체를 채울 수 없을 수도 있습니다.

<div class="content-ad"></div>

LLM을 훈련시키려면 GPU RAM이 더 많이 필요합니다. 옵티마이저 상태, 그래디언트 및 포워드 활성화는 매개변수 당 추가 메모리가 필요하기 때문이죠.

LLM을 선택할 때는 GPU의 메모리 용량을 확인하고, 그에 맞는 모델을 선택하세요. 대략 1B 매개변수 = 2GB (16비트) 또는 1GB (8비트)를 사용하는 것이 좋습니다.

## 결론

만약 상업용 모델 비용을 지불하고 싶지 않지만 자신의 용도에 LLM을 사용하고 싶다면, 오픈 소스 LLM을 활용해보세요.

<div class="content-ad"></div>

LLMs(Large Language Models)은 아직 상대적으로 새로운 기술이며 지속적으로 개선되고 있으므로 최근에 출시된 모델을 사용하는 것이 좋을 것 같아요.

가능하다면, 특정 하향 작업에 맞게 세밀하게 조정된 모델을 사용하는 것이 좋아요. 예를 들어, 질문-답변 프롬프트에 대한 지시 모델을 사용하면 좋아요. 해당 모델이 없는 경우에는 기초 모델을 직접 세밀하게 조정해야 할 수도 있어요.

마지막으로, 모델 파라미터의 수를 살펴서 이 모델을 로드할 수 있는지 하드웨어의 RAM 양과 비교해보아야 해요.

# 참고문헌

<div class="content-ad"></div>

**References:**

- A. Vaswani et al., "Attention Is All You Need" (2017), [arXiv:1706.03762](https://arxiv.org/abs/1706.03762)
  
- J. Yang et al., "Harnessing the Power of LLMs in Practice: A Survey on ChatGPT and Beyond" (2023), [arXiv:2304.13712](https://arxiv.org/abs/2304.13712)
  
- Hugging Face Documentation, "Anatomy of Model’s Operations" (2024)