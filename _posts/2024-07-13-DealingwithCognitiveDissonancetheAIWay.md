---
title: "AI를 이용한 인지부조화 해결 방법"
description: ""
coverImage: "/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_0.png"
date: 2024-07-13 03:07
ogImage:
  url: /assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_0.png
tag: Tech
originalTitle: "Dealing with Cognitive Dissonance, the AI Way"
link: "https://medium.com/towards-data-science/dealing-with-cognitive-dissonance-the-ai-way-1f182a248d6d"
isUpdated: true
---

![Image](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_0.png)

Cross posted on Art Fish Intelligence.

Hey there! Today, let's dive into the intriguing topic of how language models tackle conflicting instructions in a prompt.

Cognitive dissonance is a fascinating concept in psychology, referring to the mental discomfort that arises when a person holds two contradictory beliefs. Imagine this: you're at the grocery store, faced with a checkout lane sign saying "10 items or fewer," but everyone in line has 10 items or more. What should you do?

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

AI 문맥에서 대형 언어 모델(LLM)이 인지적 불일치를 다루는 방법에 대해 알고 싶었어요. 예를 들어, 영어에서 한국어로 번역하도록 LLM에 지시했지만 영어에서 프랑스어로 번역하는 예를 제공하는 경우 등.

이 글에서는 LLM에 상충되는 정보를 제공하여, 어떤 상충 정보에 LLM이 더 많이 부합하는지 확인하는 실험을 진행했어요.

# 시스템 메시지, 프롬프트 지침, 그리고 퓨샷(몇 번 학습으로) 예제

사용자로서, LLM에게 할 일을 알려주는 방법은 세 가지 중 하나를 선택할 수 있어요:

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

이미지 문제를 다음과 같이 해결하세요.

![이미지 제목](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_1.png)

시스템 메시지는 모든 것 중에서도 가장 신비로운 것 같아요. Microsoft에 따르면 "시스템 메시지는 대화의 시작 부분에서 지침을 전달하거나 모델에 맥락을 제공하는 데 사용됩니다."

내가 알기로는 시스템 메시지가 프롬프트에 얼마나 영향을 미치는지 명확하지 않아요 (시스템 메시지를 프롬프트에 바로 넣는 것과 비교해서). 적어도 내가 보기엔 이에 대한 깊은 연구가 없었어요.

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

프롬프트 지시사항은 주로 모델에게 무엇을 해야 하는지 알려주기 위해 사용됩니다. "영어를 프랑스어로 번역하세요" 또는 "내 수업 내용 중 모든 맞춤법 오류를 수정하세요" 또는 "다음 문제를 해결하기 위한 코드를 작성해주세요"와 같은 지시사항이 포함됩니다.

퓨-샷 예시는 유사한 입력에 대한 정확한 답변을 모델에게 보여주는 선택적인 시범입니다.

이러한 정의를 기반으로 알고 싶은 것은 다음과 같습니다:

- 퓨-샷 예시는 실제로 얼마나 중요한가요? 프롬프트에서 모순된 지시사항을 제시하면 LLMs는 예시를 따르기보다는 지시사항을 더 따르는 경향이 있나요?
- 시스템 메시지는 실제로 얼마나 중요한가요? 시스템 메시지에서 한 지시사항을 주고 프롬프트에서 다른 지시사항을 주면, LLMs는 어떤 지시사항을 더 따를 가능성이 더 높을까요?

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

이러한 질문들을 검증해보기 위해 상반된 지시사항과 몇 가지 예시를 포함한 작은 데이터셋을 구성했습니다. 이 데이터셋은 간단한 작업들을 포함하고 있습니다. 이 기사의 나머지 부분에서 여러 언어로 영어 번역의 단일 예시를 소개할 것입니다.

다음 실험은 OpenAI의 GPT-4o와 Anthropic의 최신 Claude-3.5 모델을 사용해서 진행되었습니다.

# 실험 1: 모순된 적은 예시를 가진 프롬프트 지시사항

![이미지](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_2.png)

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

The behavior of LLM can be quite unpredictable when faced with conflicting instructions between the prompt and the few shot examples. Studies have found that these models do not consistently lean towards following either the instructions or the examples in case of a contradiction.

GPT-4o tends to prioritize the few shot examples over the prompt instructions, often disregarding the latter or encountering errors when faced with conflicting guidance. On the other hand, Claude-3.5 has a more random approach, sometimes following the prompt instructions and other times emulating the few shot examples.

![Experiment 2: System message with contradictory few shot examples](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_3.png)

# 실험 2: 모순된 few shot 예시와 함께한 시스템 메시지

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

**이 실험은 이전 것과 매우 유사했습니다. 다만, "영어에서 독일어로 번역하세요"와 같은 지시사항이 프롬프트에서 시스템 메시지로 이동된 차이가 있었습니다.**

**대부분의 작업에서 GPT-4o는 시스템 메시지에 있는 지시사항을 더 잘 따르는 경향이었습니다. 이는 이전 실험에서와 대조적입니다. 이전 실험에서는 동일한 지시사항이 일반 프롬프트에 나타나면, 모델이 퓨 샷 예제들을 더 자주 따를 때였습니다.**

**반면, Claude-3.5는 이전 실험과 정확히 같은 방식으로 행동했습니다 (시스템 메시지나 퓨 샷 예제를 따르는 것이 거의 무작위였습니다).**

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

이게 무슨 뜻일까요? 한 가지 해석은 시스템 메시지에 있는 지시사항이 일반 프롬프트의 지시사항보다 GPT-4o에게 더 중요하다는 것입니다 (적어도 이 예시에 한해서). 클로드의 경우, 시스템 메시지가 덜 중요하게 여겨지며, 그 메시지를 동일하게 프롬프트에 입력하는 것과 비슷한 역할을 합니다.

# 실험 3: 모순된 프롬프트 지시와 시스템 메시지

![Experiment 3](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_6.png)

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

이 실험에서는 몇 가지 샷 실험을 제거했습니다. 시스템 메시지와 프롬프트에 대한 지시 사항이 서로 모순되고 있습니다. 이 설정에서 두 모델 모두가 시스템 메시지 보다는 프롬프트에서 지시 사항을 압도적으로 따르는 경향이 있었습니다.

시스템 메시지와 프롬프트에서 모순된 지시 사항이 주어진 상황에서 두 모델 모두 시스템 메시지의 지시 사항을 무시하는 경향을 보였습니다.

![Experiment 4: System message, prompt, and few shot examples all contradict each other](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_7.png)

# 실험 4: 시스템 메시지, 프롬프트 및 몇 가지 샷 예제가 서로 모순

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

![Image](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_8.png)

안녕하세요! 오늘은 전혀 상반된 방법으로 모델을 혼란스럽게 만들어 봅시다. 이 실험적인 설정에서는 시스템 메시지 지침, 프롬프트 지시 사항 및 몇 가지 예시가 모두 서로 모순됩니다.

상상할 수 있겠지만, 모델의 행동은 일관성이 없습니다.

이 모순 속에서 놀라운 점은 GPT-4o가 시스템 메시지를 따르는 경향이 더 있었고, 클로드-3.5는 프롬프트 지침을 따르는 경향이 더 있었다는 점이었습니다.

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

![Dealing with Cognitive Dissonance the AI Way](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_9.png)

# 토론과 결론

이 글에서는 시스템 메시지, 프롬프트 및 몇 가지 샷 예시에 모순된 지시를 제공하는 실험을 진행했습니다.

이 실험에서는 일부 상반된 결과가 나왔는데, 경우에 따라 모델이 시스템 프롬프트에 제공된 지시를 더 따르는 경향이 있었지만, 약간의 변화가 있는 실험에서는 이러한 행동이 변할 수 있었습니다. 시스템 메시지가 GPT-4o의 응답에 더 큰 영향을 미치는 반면, Claude-3.5의 응답에는 미미한 영향을 미쳤습니다.

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

모델의 결정에는 몇 개의 샘플 예시가 중요한 역할을 했다 (언제나 그렇지는 않았지만). 몇 개의 샘플 예시를 통해 모델이 "즉시 학습"하는 경향을 보여주는 것은 이러한 데모의 강도를 보여준다. 이러한 모습은 최근 Anthropics의 "다양한 샷 탈옥" 방법을 상기시킨다. 이 방법은 유해한 행동의 충분한 예시를 제공하면 모델이 그런 방식으로 반응하도록 이끌 수 있음을 보여준다. 이때 모델은 원래 그러한 응답을 생성하지 않도록 교육되었음에도 불구하고 그렇다.

![Image](/assets/img/2024-07-13-DealingwithCognitiveDissonancetheAIWay_10.png)

이 논문에서 탐구한 실험들은 손으로 선별한 몇 가지 작은 샘플을 바탕으로 진행되었다. 언어 모델이 프롬프트에서 제시된 다양한 형태의 모순을 다루는 방식에 대해 아직 탐구할 것이 많이 남아 있다.

본 문서에 사용된 예시의 변형과 다른 언어 모델을 사용하면, 매우 다른 결과가 나올 것으로 예상된다. 또한 다음 버전의 이러한 모델(예: 다음 GPT 및 Claude 모델)은 이 논문에서 발견된 정확한 패턴을 엄수하지 않을 가능성이 크다.

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

본 글에서는 언어 모델이 모순되는 지시에 직면했을 때 일관성이 없다는 점을 강조하고자 했습니다. 본문의 요점은 특정 예시나 작업에 대한 모델이 어떤 지시와 일치하는지에 대한 정확한 지침보다는 이러한 일치성이 실제로 존재하지 않는다는 점입니다.

또한 언어 모델이 어떤것이 이상적인 결과인지에 대한 몇 가지 질문을 던집니다. 언어 모델은 시스템 메시지에 기술된 내용을 가장 우선적으로 따르도록 훈련되어야 하는가? 유연성을 가장 중요시하고 가장 최근의 지시를 따르는 것이나, "배워가는 과정"을 중시하고 "올바른 답변"의 조밀한 예시에 부합하는 것이어야 하는가?

이는 이러한 구축된 테스트 예시 외의 상황에서 중요한 문제입니다. 예를 들어, 시스템 메시지가 모델에게 도움이 되도록 지시하고, 몇 가지 조밀한 예시가 모델에게 해로운 방법을 가르치는 경우. 또는, 새로운 지시사항을 반영하지 못하고 낡은 몇 가지 조밀한 예시가 포함된 프롬프트.

이러한 질문들에 대한 언어 모델 행동에 관해 알지 못하는 부분이 많지만, 이를 탐구하고 더 배우는 것이 중요합니다.
