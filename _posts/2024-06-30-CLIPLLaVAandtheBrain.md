---
title: "CLIP, LLaVA, 그리고 뇌 2024 최신 AI 모델 비교 분석"
description: ""
coverImage: "/assets/img/2024-06-30-CLIPLLaVAandtheBrain_0.png"
date: 2024-06-30 23:53
ogImage: 
  url: /assets/img/2024-06-30-CLIPLLaVAandtheBrain_0.png
tag: Tech
originalTitle: "CLIP, LLaVA, and the Brain"
link: "https://medium.com/towards-data-science/clip-llava-and-the-brain-2073dfb33d7e"
---


## DEEP LEARNING AND THE BRAIN

![Brain Network](/assets/img/2024-06-30-CLIPLLaVAandtheBrain_0.png)

Hey there! Ever wondered how cutting-edge multimodal transformer networks like CLIP (Radford et al. 2021) and LLaVA (Liu et al. 2023) stack up against the complexity of the human brain? Do you see any parallels between the attention mechanisms in these networks and our brains? In this piece, I delve into these transformer models and explore the intriguing overlaps and distinctions they share with our own mammalian brain.

One fascinating aspect that caught my attention is how vision transformers, CLIP, and LLaVA seem to mimic a form of processing akin to the pre-attentive visual processing observed in the brain. This initial processing occurs during the forward visual responses to a stimulus even before any recursive actions take place. While a significant amount of tasks can be handled via this forward process, research indicates that the brain encounters challenges with pre-attentive processing in the following areas:

<div class="content-ad"></div>

- 유사한 유형의 물체의 신분이나 특성을 뚜렷하게 구별하려고 할 때 특히 물체들이 서로 가깝거나 혼잡하거나 물체들이 자연이 아니거나 인공적인 경우 (VanRullen 2007).
- 더 복잡한 작업들은 세는 작업이거나 미로나 곡선 추적 작업과 같은 것이다.
- 물체들을 인식하는 것이 더 어렵기 때문에 물체들의 경계를 인식하기 어려운 경우와 같은 것이다.

피드포워드(Feed-forward) 처리와는 대조적으로, 뇌에서 두드러지는 점 중 하나는 영역들 간 상호작용의 풍부함인데, 다음 섹션에서 더 자세히 논의하겠습니다.

# 두방향 활동(Bidirectional Activity) 뇌에서

대부분의 현재 심층학습(Deep learning) 아키텍처에서 활동은 한 방향으로 전파됩니다. 예를 들어, 이미지가 네트워크에 입력으로 제공되고 그런 다음 계층별로 전파되어 분류가 출력으로 나올 때까지입니다.

<div class="content-ad"></div>

![image](/assets/img/2024-06-30-CLIPLLaVAandtheBrain_1.png)

안녕하세요! 

이 문구는 선형 전달 모델보다 더 흥미로운 두뇌의 역할을 설명하는 것입니다. 시각 체계에서 자극은 처음에는 하위 수준에서 상위 수준 시각 영역으로 선형 전달 방식으로 전파되고, 그 다음 상위 수준 영역이 하위 수준 영역에 영향을 미친다고 그림 1에 나와 있어요.

이러한 피드백 중 일부는 의식적인 탑-다운 주의인데요, 이는 우리가 관심 대상이 되는 객체와 특징에 더 많은 자원을 할당하고 복잡하거나 모호한 자극을 명확히 하는 데 도움을 줍니다. 다른 일부 피드백은 자동적이어서 상위 수준 영역이 하위 수준 영역에 선형 전달만으로는 알 수 없는 정보를 주입합니다.

의식적인 탑-다운 주의는 시각 자극의 의식을 지원한다고 생각됩니다. 테두리와 가장자리를 부호화하는 하위 수준 영역에 의식적으로 액세스할 수 없다면, 테두리를 공간적으로 정확하게 인식하기 어렵습니다. 곡선을 정신적으로 추적하거나 미로를 풀듯이 한 작업들이 불가능해질 거예요.

제 마음을 놓아 주셔서 감사합니다! 🌟

<div class="content-ad"></div>

자동 무의식적 피드백의 한 예는 시각 영역 V2의 방향 선택적 뉴런 약 절반에서 관찰되는 경계 소유권 코딩입니다(Zhou et al. 2000, Williford and von der Heydt 2013). 이러한 뉴런들은 약 40ms 안에 지역 정보를 부호화하며, 이 초기 응답 이후 약 10ms만에 전역 맥락을 통합하여 베둘러를 해결합니다. 이 과정은 배경을 가리는 물체들이 어떤 경계를 만들고 있는지에 대한 정보를 유지합니다.

다른 예로 Poort et al. (2012)가 제시한 무의식적 피드백이 있습니다. Figure 2와 같은 이미지를 사용하여 연구를 진행했습니다. 맥a크 V1 초기 시각 피질에서 뉴런들은 주로 자신의 수용 영역 내에서 지역적 특징(예: 녹색 사각형)만 초기에 부호화합니다. 그러나 약 75ms 이후, 더 높은 수준의 영역에서 피드백을 받게 되며, 그 질감이 이 그림과 같은 도형에 속할 때에 반응이 더 높아집니다. 이는 원숭이가 도형에서 주의를 돌리더라도 발생하지만, 원숭이가 도형에 주의를 집중할 때 뉴런들이 일반적으로 더 강하게 반응합니다.

두 방향으로 상호작용을 볼 수 있는 한 가지 방법은 각 뉴런이 항상 사용 가능한 예측 신호를 탐욕스럽게 활용한다는 것입니다. 특히 시각적 경계가 중요한 1차 대조 가장자리와 일치하지 않을 때 더 높은 수준의 영역이 예측적일 수 있다는 것을 생각해 볼 수 있습니다.

<div class="content-ad"></div>

# Transformers

최근 transformer(바스와니 및 다른 연구진, 2017년)가 등장하면서 주목을 받고, 단어를 하나씩 생성할 수 있는 능력 때문에, transformer가 순환적(recurrent)이라고 생각할 수도 있습니다. 그러나 transformer의 각 단계 사이에는 어떤 내부 상태가 유지되지 않고, 이전 출력값만이 다음 입력값으로 제공됩니다. 따라서 재귀성은 제한되어 있고 뇌에서 널리 사용되는 양방향성은 갖추고 있지 않습니다. Transformer에는 여러 개의 헤드를 가진 어텐션(attention)이 있어, 고정된 개수(본 논문에서는 8개)의 것들에 동시에 주의를 기울일 수 있는 기능을 가지고 있습니다. 따라서 이미지 transformer는 몇 가지 수정을 거친 사전주의식 피드포워드(feedforward) 처리와 유사하다고 볼 수 있습니다.

# CLIP

![CLIP](/assets/img/2024-06-30-CLIPLLaVAandtheBrain_3.png)

<div class="content-ad"></div>

Radford과 OpenAI 팀은 2021년 논문 "Learning Transferable Visual Models from Natural Language Supervision"에서 CLIP을 소개했습니다. CLIP의 아이디어는 간단하며 Figure 3에서 보여집니다. 이는 인터넷에서 이미지와 캡션 쌍을 가져와 이미지를 이미지 인코더에, 텍스트를 텍스트 인코더에 주입합니다. 그런 다음 이미지와 텍스트의 인코딩을 동일한 쌍에 사용할 때 서로 가까이 모으는 손실을 사용하며, 그렇지 않으면 인코딩의 거리를 증가시킵니다. 여기에 CLIP이 제공하는 것이 있습니다: 텍스트와 이미지 간 유사성을 비교할 수 있는 능력입니다. 이것은 Figure 4에서 나타나는 것처럼 zero-shot 분류에 사용될 수 있습니다. CLIP은 이미지로부터 텍스트 설명을 생성하지는 않습니다.

이미지 인코더와 텍스트 인코더는 독립적이므로 작업 중심 조절이 이미지 인코딩에 영향을 미치지 못합니다. 이는 이미지 인코더가 작업에 잠재적으로 관련될 수 있는 모든 것을 인코딩해야 한다는 것을 의미합니다. 일반적으로 입력 이미지의 해상도는 작아서 계산 및 메모리 요구 사항이 폭발하는 것을 방지하는 데 도움이 됩니다.

`![CLIPLLaVAandtheBrain](/assets/img/2024-06-30-CLIPLLaVAandtheBrain_4.png)`

# LLaVA

<div class="content-ad"></div>

![LLaVA](/assets/img/2024-06-30-CLIPLLaVAandtheBrain_5.png)

안녕하세요! LLaVA (큰 언어 및 비전 보조 프로그램) (Liu et al. 2023)은 CLIP를 확장하고 개선하여 이미지에 대한 설명 및 질문에 대답할 수 있는 능력을 추가한 대규모 언어 및 비전 아키텍처입니다. 이 유형의 아키텍처는 뇌과학 및 심리학에서 사용되는 작업을 시도할 수 있어 저에게 흥미롭습니다.

LLaVA는 CLIP에서 이미지 인코딩을 위해 훈련된 ViT-L/14 비전 트랜스포머 모델을 사용합니다. 첫 논문에서는 인코딩을 토큰(token)으로 변환하기 위해 단일 선형 투영 매트릭스 W를 사용합니다. 이미지 Hᵥ 및 텍스트 지침 Hq에서 계산된 토큰이 입력으로 제공됩니다. 그런 다음 LLaVA는 언어 응답 Xₐ를 한 번에 한 토큰씩 생성하여 지금까지의 응답을 다음 반복의 입력으로 추가할 수 있습니다.

LLaVA 훈련 방법에 대해 자세히 설명은 생략하겠지만, Figure 5의 캡션(Xc)을 확장하여 이미지에 대한 지침(Hq) 및 응답(Xₐ 훈련에 사용)을 형성하고 바운딩 박스 정보를 사용하는 방법이 흥미롭다는 것에 주목할 만합니다.

<div class="content-ad"></div>

지난 2024년에 발표된 LLaVA 1.5 버전에서는 여러 가지 개선 사항이 있습니다:

- 선형 투영 행렬 W가 다층 퍼셉트론으로 대체되었습니다.
- 이미지 해상도가 향상되었는데, 336x336 픽셀 크기의 이미지를 사용하고 이미지 인코더를 사용하여 이미지를 그리드로 분할하고 각각을 개별적으로 인코딩했습니다.

뇌의 작업 주도형 주의는 객체, 위치 또는 관심 기능에 리소스를 동적으로 할당할 수 있어 정보 처리를 가능하게 합니다. 이를 통해 혼잡이나 다른 객체에 압도되는 정보를 처리할 수 있습니다. LLaVA에서 이미지 인코더는 텍스트 명령과 독립적이므로 유용한 정보가 이미지 토큰(Hᵥ)에 저장되도록 보장해야 합니다.

# 결론

<div class="content-ad"></div>

LLaVA와 CLIP는 양방향 및 내부 상태에서의 재발 및 재발을 제한하여 그들의 처리를 방해합니다. 이는 이미지 처리에 특히 사실입니다. 이미지 처리는 텍스트 지시사항과 독립적으로 수행되기 때문입니다. 대부분의 합성곱 신경망도 이러한 제한을 공유합니다. 이것이 내 추측으로 이어집니다:

이것은 비판이 아니라 정보를 제공할 수 있는 통찰력일 뿐입니다. 피드포워드 처리는 많은 일을 할 수 있으며 빠릅니다. 그러나 사용해야 할 리소스가 얼마나 동적인지에 따라 정보가 너무 많아질 때 혼잡한 장면에서 정보 병목 현상을 야기할 수 있으며, 순방향 처리가 충분한 정보를 인코딩하지 못할 수 있습니다. 복잡한 작업을 수행할 수 있는 임의의 크기 증폭 없이 인코딩의 크기가 폭발하는 것입니다. 순방향 방식으로 작동하는 모델을 만드는 것은 재발 및 양방향 처리를 추가하는 어려움 때문에 중요한 발걸음입니다.

일부 네트워크는 선행적인 피드포워드 네트워크에 제한되지 않지만 현재 대부분의 아키텍처는 트랜스포머의 뒤를 뒤쫓고 있습니다. 이는 LSTM(Long-Short Term Memory) 모델 및 더 최근에는 여러 이점이 있는 Mamba 아키텍처를 포함합니다 (Gu and Dao 2024). 연장된 LSTM(Beck et al. 2024, Alkin et al. 2024)이 최근에 제안되어 트랜스포머와 LSTM 간의 균형을 맞추는 데 도움이 되었습니다. 확산 모델은 반복 사용 사이에 상태로 이미지를 사용하는 유형의 제한된 종류의 재발을 가지고 있습니다.

<div class="content-ad"></div>

B. Alkin, M. Beck, K. Pöppel, S. Hochreiter, and J. Brandstetter가 함께 쓴 "Vision-LSTM: xLSTM as Generic Vision Backbone" (2024) 논문 번호는 http://arxiv.org/abs/2406.04303 입니다.

M. Beck, K. Pöppel, M. Spanring, A. Auer, O. Prudnikova, M. Kopp, G. Klambauer, J. Brandstetter, 그리고 S. Hochreiter.가 함께 쓴 "xLSTM: Extended Long Short-Term Memory" (2024) 논문 번호는 http://arxiv.org/abs/2405.04517입니다.

A. Gu 와 T. Dao가 쓴 "Mamba: Linear-Time Sequence Modeling with Selective State Spaces" (2024) 논문 번호는 http://arxiv.org/abs/2312.00752 입니다.

H. Liu, C. Li, Y. Li, 그리고 Y. J. Lee의 "Improved Baselines with Visual Instruction Tuning" (2024)은 IEEE/CVF CVPR 학회에서 발표되었습니다.

<div class="content-ad"></div>

H. Liu, C. Li, Q. Wu, and Y. J. Lee, Visual Instruction Tuning (2023), [DOI: 10.48550/arXiv.2304.08485]

J. Poort, F. Raudies, A. Wannig, V. A. F. Lamme, H. Neumann, and P. R. Roelfsema. The Role of Attention in Figure-Ground Segregation in Areas V1 and V4 of the Visual Cortex (2012) Neuron

A. Radford, J. W. Kim, C. Hallacy, A. Ramesh, G. Goh, S. Agarwal, G. Sastry, A. Askell, P. Mishkin, and J. Clark. Learning Transferable Visual Models from Natural Language Supervision (2021) ICML

R. VanRullen, The Power of the Feed-Forward Sweep (2007) Advances in Cognitive Psychology

<div class="content-ad"></div>

A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. N. Gomez, Ł. Kaiser, and I. Polosukhin의 'Attention Is All You Need' (2017)은 NeurIPs에서 발표되었습니다.

J. R. Williford와 R. von der Heydt의 'Border-Ownership Coding' (2013)은 Scholarpedia에서 찾을 수 있습니다.

H. Zhou, H. S. Friedman, and R. von der Heydt의 "Coding of Border Ownership in Monkey Visual Cortex" (2000)은 The Journal of Neuroscience에서 발표되었습니다.

최초 게시된 내용은 2024년 6월 19일 neural.vision에서 확인할 수 있습니다.