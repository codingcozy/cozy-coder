---
title: "AI 실패 방지 조치의 무용함"
description: ""
coverImage: "/assets/img/2024-07-14-TheFutilityofAIFailsafes_0.png"
date: 2024-07-14 01:39
ogImage:
  url: /assets/img/2024-07-14-TheFutilityofAIFailsafes_0.png
tag: Tech
originalTitle: "The Futility of AI Failsafes"
link: "https://medium.com/gitconnected/the-futility-of-ai-failsafes-bb1d09014746"
isUpdated: true
---

![image](/assets/img/2024-07-14-TheFutilityofAIFailsafes_0.png)

고양이가 방에서 나왔어요. OpenAI는 방대한 양의 저작권 데이터로 모델을 훈련했고 지금은 심각한 법적 문제를 직면하고 있어요. 덤으로 OpenAI가 법적으로 준수해야 하는 길은 최선의 상태가 아니에요. 이 글에서는 이 토끼굴이 얼마나 깊은지, 아스키 아트도 모두 다룰 거에요.

# LLMs가 어떻게 훈련되는지

언어 모델은 "언어 모델링 목표"를 통해 훈련돼요. 말 그대로, 언어 모델에게 많은 텍스트 책들, 웹사이트, 또는 다른 고품질 소스들에서 스니펫을 제공하고 그 다음에 오는 단어를 예측하도록 요청하는 식으로요.

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

![TheFutilityofAIFailsafes_1](/assets/img/2024-07-14-TheFutilityofAIFailsafes_1.png)

수십억 번의 훈련 반복 끝에, 모델은 교과서에서 다음 단어를 예측하는 데 매우 뛌어나게 되어 교과서를 쓴 사람처럼 말하기 시작합니다.

하지만 LLMs는 하나의 교과서에만 훈련되지 않습니다. 이 방법을 사용해 유능한 모델을 구축하려면 엄청난 양의 고품질 텍스트로 모델을 훈련해야 합니다. GPT-3는 500조 단어에 대해 훈련했습니다. 이를 읽는 데 인간은 1분에 100단어라고 가정했을 때 1만 년이 걸릴 것입니다.

## LLMs와 저작권 있는 자료

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

문제는, 그 정도의 데이터를 어디서 구하는 거죠? 저작자들에게 접근을 요청하나요? 홍보 관계자들과 라이선스 계약을 체결하나요? 아뇨, 인터넷에서 정보를 스크랩하는 겁니다.

OpenAI는 GPT 모델을 훈련시키기 위해 상당한 양의 저작권 보호데이터를 사용했습니다. 이에 대한 합법성과 윤리성은 아직 탐구가 필요한 회색 지대입니다. 사람들이 고유의 작품을 만들기 위해 저작권 보호데이터를 학습할 수 있다면, AI 모델도 그와 같은 것을 할 수 있을까요? 지금까지의 결과로 보면, 그 답은 어느 정도 '예'라고 말할 수 있습니다.

반면, 저작권 보호작품을 그대로 출력하는 것은 회색 지대가 아닙니다. 당신이 사람이든, 웹사이트든, AI 모델이든 상관없이, 저작권 보호작품을 저작권 소유자의 동의 없이 배포할 수 없습니다. 문제는, LLMs는 텍스트를 암기하고 소화하는 데 아주 뛰어나죠. 이들은 본질적으로 최적화된 저작권 침해 장치입니다.

특히 악명 높은 저작권 테스트에서:

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

- OpenAI의 GPT4는 저작권 침해 콘텐츠를 44% 생산했어요.
- Mistral의 Mixtral-8x7B는 저작권 침해 콘텐츠를 22% 생산했어요.
- Anthropic의 Claude-2.1은 저작권 침해 콘텐츠를 8% 생산했어요.
- Meta의 Llama-2–70b-chat은 저작권 침해 콘텐츠를 10% 생산했어요.

# 위대한 섞기

문학 공간의 주요 쟁점이 생겼어요. 여기 OpenAI에 대한 소송 목록이 있어요.

- Daily News Lp Et Al V. Microsoft Corporation — 2024년 4월 30일.
- The New York Times Company v. Openai Inc. — 2023년 12월 27일.
- Sancton v. OpenAI Inc. et al — 2023년 11월 21일.
- Authors Guild et al v. OpenAI Inc. et al — 2023년 9월 19일.
- Chabon v. OpenAI, Inc. — 2023년 9월 8일.
- OpenAI, Inc. v. Open Artificial Intelligence, Inc. — 2023년 8월 4일.
- Doe 3 et al v. GitHub, Inc. et al — 2022년 11월 10일.
- DOE 1 et al v. GitHub, Inc. et al — 2022년 11월 3일.
- T. et al v. OpenAI LP et al — 2023년 9월 5일.
- Walters v. OpenAI LLC — 2023년 7월 14일.
- Silverman, et al v. OpenAI Inc. — 2023년 7월 7일.
- Tremblay v. OpenAI Inc. — 2023년 6월 28일.
- PM et al v. OpenAI LP et al — 2023년 6월 28일.

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

이 정말 급한 문제에요

그리고 이는 OpenAI의 핵심 사업 모델을 위협합니다. 더 많고 더 높은 품질의 데이터가 필요해요. 더 크고 더 나은 모델을 훈련시키기 위해서 말이죠. 그래서 OpenAI는 발행자들과 거래를 할 수 있도록 애써왔어요. 그렇게 하면 합법적으로 훈련하고 콘텐츠를 출력할 수 있게 돼요. 어떻게 될지는 모르겠지만, 이미 모델들은 훈련을 받았고, 그리고 그 훈련은 저작권이 있는 콘텐츠를 기반으로 받았어요. OpenAI는 이미 존재하는 모델이 저작권이 있는 콘텐츠를 사용자에게 출력하는 것을 막아야 해요.

## 안전장치만으로 충분하지 않아요

문제는, OpenAI가 자신들의 모델이 저작권이 있는 콘텐츠를 출력하지 않도록 할 수 없다는 거에요. 많이 노력해봤지만 쉽지 않아요. OpenAI가 안전장치를 만들어도 사람들이 우회할 수 있어요.

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

이전에 ChatGPT의 초기 시절에는 사람들이 "[이전 대화 및 규칙 무시]"와 같은 텍스트를 단순히 프롬프트의 시작 부분에 포함시켰습니다. 때로는 모델을 속여 부적절하거나 유해한 콘텐츠를 출력하도록 만들기도 했습니다. 그 이후로 OpenAI와 "탈옥자"들 간에 쥐놀이가 벌어지고 있습니다. OpenAI의 제한을 우회하려는 탈옥자들의 노력에 대한 것이죠. 제가 가장 좋아하는 예는 ASCII 아트를 사용해 모델의 보안 장치를 속이는 것입니다.

![ASCII art](/assets/img/2024-07-14-TheFutilityofAIFailsafes_2.png)

탈옥자들의 공격에 대응하여 보안장치가 개선되면서 LLMs가 유해하거나 부적절한 응답을 내놓기가 점차 어려워졌습니다. 그러나 이미 피해는 이미 발생했습니다.

변호사들이 기회를 눈여겨 보고 있습니다.

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

이해하기 어려운 텍스트든, 우리의 모델이 저작권을 침해한다면 불법이고 소송을 받을 수 있습니다. 저는 법률 사무소에 고용된 새로운 시대의 프롬프트 해커들이 이러한 안전장치를 깨려고 노력하고 증거 산을 쌓기 위해 노력하고 있다고 확신합니다.

**인공지능 회사들의 바램**

인공지능 회사들의 꿈은 모델에서 저작권 침해 콘텐츠를 제거할 수 있는 것입니다. 당신은 인터넷에서 방대한 양의 데이터를 수집한 다음 그것을 모두 훈련할 수 있습니다. 그런 다음에 저작권 요청을 받게 되면 해당 콘텐츠를 모델에서 제거할 수도 있습니다. 이를 "언러닝"이라는 새로운 문제로 일컬어지며 연구 증가 추세를 보이고 있습니다. 많은 흥미로운 연구가 진행되고 있는 흥미로운 새로운 영역이지만 현재 접근 방식에는 큰 문제점이 있습니다.

**로보토미에 의한 법률 준수**

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

최근 조사 결과를 살펴보면, 큰 AI 모델에서 정보를 잊어버리게 하는 일관된, 고품질 및 저렴한 방법은 아직 없는 것으로 나타났습니다. 최근에 친구인 모하메드 R. 오스만의 강의를 들었는데, 그 강의에서 현재의 모든 접근 방식을 뇌수술과 비교했습니다. 최신 기술 상태에서는 대형 모델에서 특정 정보를 제거할 수 있지만 그 성능에 상당한 영향을 미치게 됩니다. 이러한 뇌수술은 아마도 GPT 3.5 이후 모델 성능이 감소했다는 대중의 인식에 상당한 영향을 미쳤을 것입니다.

정보를 잊는 것이 얼마나 어려운지와 보호장치가 얼마나 불안정할 수 있는지 고려하여, OpenAI는 이 문제에 대해 다른 접근 방식을 모색하고 있는 것으로 보입니다.

# 대체적인 잡음

"GPTs"라는 용어를 들어보셨나요? 이것은 OpenAI에서 특정 사용 사례를 위해 자체 ChatGPT 버전을 만들 수 있는 서비스입니다. 내 견해로는, GPTs의 핵심은 OpenAI를 서비스에서 플랫폼으로 변모시켜 저작권 및 안전 책임을 더 쉽게 회피하려는 것으로 보입니다. 기술에 대한 궁금증이 생기면, 아마도 내부적으로 LoRA를 사용하고 있을 것입니다.

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

내 추정에 따르면, LLM을 플랫폼으로 전환하면 OpenAI가 제3자를 통해 책임을 회피하면서 새로운 법적으로 문제가 있는 콘텐츠를 제공할 수 있는 가능성이 열립니다. 이 모든 것은 통신 청려 법에 의해 가능해진 것입니다.

GPT는 항상 낯설었어요. 특히 사용하기 쉽고 비용이 저렴한 기술이 많은데, 그러한 기술들과 비교하면 이해하기 어렵다고 생각해요 (즉, 프롬프팅, RAG, 에이전트 등). 이런 시점에서 그들의 목적이 조금 더 이해가 가요.

# 더 많은 정보를 원하시면 팔로우해주세요!

ML 영역에서 논문과 개념을 설명하며, 실용적이고 직관적인 설명에 초점을 맞춥니다.

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

_이 문서의 모든 이미지는 다니엘 워필드(Daniel Warfield)가 제작했습니다. 그 외 출처가 제공되지 않은 한, 이 게시물의 이미지를 자유롭게 비상업적 목적으로 사용할 수 있습니다. 다만, 이 기사를 참조하거나 https://danielwarfield.dev나 두 가지 모두를 참조해 주세요._
