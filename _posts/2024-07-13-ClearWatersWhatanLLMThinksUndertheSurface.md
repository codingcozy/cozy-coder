---
title: "LLM이 보는 깊은 물 속의 세계 명료한 분석"
description: ""
coverImage: "/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_0.png"
date: 2024-07-13 02:58
ogImage: 
  url: /assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_0.png
tag: Tech
originalTitle: "Clear Waters: What an LLM Thinks Under the Surface"
link: "https://medium.com/gitconnected/clear-waters-what-an-llm-thinks-under-the-surface-9b21206e31dc"
isUpdated: true
---





## |해석성|XAI|

![이미지](/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_0.png)

요즘 Anthropic은 대규모 언어 모델에서 해석성에 많은 관심을 기울이고 있습니다. 회사에 따르면, 신경망은 의미 있는 기능을 학습합니다. 문제는 시각화하는 것인데요.

트랜스포머 자체는 해석하기 어렵습니다. 다른 신경망보다 더 나은 것이 아니라, 트랜스포머는 블랙 박스 개념을 더 심화시킨 것입니다. 따라서 모델을 해석하려는 시도가 이루어졌는데요. 초기 접근법은 주로 어텐션 헤드를 시각화하려는 데 집중했습니다.

<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:1132/0*x80BOZKQLUpPw7J7.gif)

However, this approach oversimplifies things a bit. The different attention heads specialize in understanding complex relationships that may not be immediately apparent (deeper layers learn intricate relationships that are not easily discernible). Furthermore, there are now multiple attention heads per layer. Also, attention heads at a specific layer learn representations that rely on previous layers.

A Large Language Model (LLM) can grasp numerous concepts, and indeed, these concepts are somehow linked to an internal representation. The challenge lies in determining which concepts are tied to specific neurons. The issue is that they are not tied to single neurons but to a combination of neurons (similar to how an element in an image is not just one pixel but a combination of pixels).

For those familiar with embeddings, we create a vector representation of text, and there are dimensions associated with semantic concepts. These dimensions can also be visualized.


<div class="content-ad"></div>

![image](/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_1.png)

만약 엔쏘피크에 따라 임베딩이 참이라면 모델이 내부적으로 학습한 표현에도 그것이 참일 것입니다. "축"이 있어야 하며, 이러한 축은 의미 개념과 상관관계가 있습니다.

어느 정도 이 직관을 지원하는 근거가 있습니다. 이전 연구에서 특정 뉴런(아니면 특정 뉴런 패턴)이 특정 개념이 표현될 때 발화한다는 것을 보여주었습니다. 그러면 이러한 개념과 활성화 패턴을 매핑할 수 있습니다.

이는 연구 커뮤니티의 관심을 끌었으며, 가장 유망한 접근 중 하나는 희소 인코더를 사용하여 이 차원을 관리 가능한 개념으로 줄이는 것입니다.

<div class="content-ad"></div>

그래서 여러분, sparse autoencoders를 활용함으로써 이러한 개념을 매핑하고 마침내 시각화할 수 있습니다.

![Image](/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_2.png)

이러한 접근 방식은 매우 흥미로우나 일반적으로 장난감 모델이나 비교적 작은 트랜스포머와 함께 사용됩니다. 게다가 많은 사람들에게 이것은 가설보다는 확정된 사실보다는 더 가까운 것입니다.

실제로, 모든 것이 규모에 따라 변화한다고 말할 수 있습니다. 더 큰 모델이 더 작은 모델에는 없는 신흥적인 특성을 보여주는 것으로 알려져 있습니다. 따라서 이러한 접근 방식을 확장시켜야 했습니다. 이것은 쉽지 않은 작업이며, 연산 비용이 매우 비싸게 들 수 있습니다.

<div class="content-ad"></div>

이게 정말 맞는지 확신하려면:

- LLM의 입력에 개념이 나타나면 해당 특징이 활성화되어야 합니다.
- 특징을 활성화 또는 비활성화하면 출력에서 변화를 관찰해야 합니다.

"Golden Gate Claude"는 Golden Gate Bridge에 대한 언급이 있을 때 특정 양식의 뉴런이 활성화되는지 자세히 연구한 사실을 가리킵니다. 다시 말해, 유명한 샌프란시스코 다리에 언급이 있는 경우 해당 활성화가 있어야 합니다. 또한 Golden Gate Bridge일 필요는 없고, 다른 언어로 언급되더라도 이 패턴이 활성화되어야 합니다. 또한 이미지도 이 특징을 활성화시킵니다.

가장 흥미로운 점 중 하나는 저자들에게는 특징 간 거리를 계산할 수 있다는 것입니다. 따라서 다리 근처에는 샌프란시스코와 관련된 잠재 공간의 다른 특징이 있을 수 있습니다.

<div class="content-ad"></div>

모든 이것이 정확히 새로운 것은 아니라고 가정해 봅시다. 이미 트랜스포머의 특정 패턴과 관련된 요소들을 식별한 연구들이 있었습니다. 이것의 예시를 보여드리겠습니다:

![이미지](/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_3.png)

둘째로, CLIP는 멀티모달 임베딩을 학습할 수 있다는 것을 보여줬습니다. CLIP에서 이미지 속 요소("고양이")와 그에 대한 텍스트 설명("소파 위에 앉아 있는 고양이")는 잠재 공간에서 서로 가까이 위치하게 됩니다. 그래서 흥미로운 확인이지만 어느 정도 예상된 바였던 결과기도 합니다.

![이미지](/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_4.png)

<div class="content-ad"></div>

Anthropic가 LLM 크기 및 접근 방식의 세분화 면에서 확장되었다. 나는 모델이 다양한 언어간 차이보다 개념(다리)과 더 의미적으로 관련된 패턴을 학습한다는 것이 흥미롭다고 생각합니다. 게다가, 다소 의심스러운 점이지만, 멀티 모달 모델이 어떤 모드에서도 개념적 기능을 갖는다는 것은 흥미로운 점입니다.

그럼으로써 저자들에게 이러한 특징들은 특정 뉴런들에 의해 분리된다(개별 뉴런과 구분됩니다). 이는 저자들에게는 이점입니다만, 실제로는 그들이 더욱 선택적으로 뉴런을 연구할 수 있도록 해줄 것입니다. 또는 유해한 개념과 관련된 뉴런을 특정 지을 수 있게 하거나, 모델의 능력에 영향을 미치지 않고 그 뉴런들을 집중적으로 관찰할 수 있게 해줄 것입니다.

이는 사실상 이 기능들을 활용하여 행동 실험을 수행하기로 결정한 논문에서 가장 흥미로운 부분 중 하나입니다. 다시 말해, 이러한 특징들을 방해함으로써 오류를 유발하고, 모델의 목표, 선호도, 편향을 변경할 수 있습니다.

![Image](/assets/img/2024-07-13-ClearWatersWhatanLLMThinksUndertheSurface_5.png)

<div class="content-ad"></div>

For the authors, tinkering with these attributes involves not just altering the text input but also modifying the model's internal representations, which the model uses to determine its actions (essentially akin to zapping electrodes at the brain).

One fascinating aspect is the presence of a trait that can be activated by an excessively self-assured user, leading the model to generate responses that align with the user's beliefs or wishes rather than the truth (referred to as sycophancy). When activated, the model also endeavors to please the user, albeit in an inaccurate manner.

The methodology is undeniably captivating and holds significant engineering implications (such as scaling the autoencoder sparsity for a model of this magnitude). Nonetheless, this doesn't imply that we can fully comprehend the models. "the Golden Gate Bridge feature" is quite a captivating term. Particularly as this feature can also be activated in contexts unrelated to the bridge:

Furthermore, the authors identified a plethora of features, some of which may not be particularly practical. Having too many features can be nearly as useless as having none at all.

<div class="content-ad"></div>

이 기사는 접근 방식에 대해 매우 상세히 다루고 있지만 코드를 공개하지 않아서 조금 모순적입니다. "우리는 이 발견을 활용하여 모델을 더 안전하게 만들 수 있기를 희망합니다." 라고 말했지만, 실제로 이러한 발견들은 본질적으로 재현 가능하지 않습니다. 

어쨌든, 이 접근 방식은 흥미롭고 의심스러운 아이디어가 확인되었습니다. 더 나은 미래와 안전한 세상을 위해 LLM의 내부 표현을 이해하는 데 투자해야 합니다. LLM 내에서 개념을 매핑할 수 있는 능력은 우리가 조치를 취할 수 있게 해줍니다. 미래에는 특히 기계 학습데이터 삭제에 관련된 문제에서 유용할 수 있습니다.

## 여러분은 이에 대해 어떻게 생각하시나요? Transformer 해석 가능성을 위한 거대한 발전이라고 생각하시나요? 댓글에서 의견을 공유해주세요.

# 이 내용이 흥미롭게 느껴지셨다면:

<div class="content-ad"></div>

새로운 기사를 찾아보세요! 그리고 LinkedIn에서 저와 연결하거나 연락하실 수도 있습니다. 매주 업데이트되는 기계 학습 및 인공 지능 뉴스가 담긴 이 저장소를 확인해보세요. 협업 및 프로젝트에 대해 열려 있으며 LinkedIn을 통해 연락할 수 있습니다. 새로운 이야기를 게시할 때 알림을 받으려면 무료로 구독할 수도 있습니다.

여기 제 GitHub 저장소 링크가 있습니다. 거기서 기계 학습, 인공 지능 등과 관련된 코드 및 다양한 자료를 수집 중이에요.

혹시 최근 기사 중 하나에 관심이 있을 수도 있어요:

# 참고

<div class="content-ad"></div>

이 글을 쓰기 위해 주요 참고 자료로 삼은 목록을 준비했습니다. 여기서는 각 논문의 첫 번째 저자만을 인용했어요.

- Mikolov, 2013, Linguistic Regularities in Continuous Space Word Representations, [링크](링크)
- Rajamanoharan, 2024, Improving Dictionary Learning with Gated Sparse Autoencoders, [링크](링크)
- Marks, 2024, Sparse Feature Circuits: Discovering and Editing Interpretable Causal Graphs in Language Models, [링크](링크)
- OpenAI, 2024, Transformer Debugger, [링크](링크)
- Alammar, 2021, Ecco: An Open Source Library for the Explainability of Transformer Language Models, [링크](링크)
- Templeton, 2024, Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet, [링크](링크)