---
title: "AI가 당신의 알고리즘을 대체하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-AIIsEatingYourAlgorithms_0.png"
date: 2024-07-12 23:59
ogImage:
  url: /assets/img/2024-07-12-AIIsEatingYourAlgorithms_0.png
tag: Tech
originalTitle: "AI Is Eating Your Algorithms"
link: "https://medium.com/towards-data-science/ai-is-eating-your-algorithms-2e79d2c3a421"
isUpdated: true
---

![2024-07-12-AIIsEatingYourAlgorithms_0.png](/assets/img/2024-07-12-AIIsEatingYourAlgorithms_0.png)

새로운 소프트웨어가 가능해진 것에 대한 큰 관심이 있습니다. 자연 언어 챗봇과 같은 응용 프로그램들이 복잡한 질문에 지식을 바탕으로 지능적으로 응답하여 흥미를 끕니다. 이러한 응용 프로그램들은 이전에는 불가능했던 것들입니다. 생성 ai는 매우 빠르게 전혀 새로운 응용 프로그램 집합을 가능케 하여 이를 정당하게 흥미롭게 만들었습니다.

흥미로운 것만큼 중요한 것은 이미 해결할 수 있었던 소프트웨어 문제를 전통적이고 특수하게 개발된 소프트웨어 대신 범용 LLMs로 대체하는 것입니다. 생성 ai는 매우 유연하며 적절한 공학적 장려를 통해 대규모 언어 모델(GPT-4o와 같은)을 사용하여 상당수의 전통적 소프트웨어 도전 과제를 해결할 수 있습니다. 이전에는 불가능했거나 매우 어려웠던 새로운 것을 구축하는 것은 흥미롭지만, 생성 ai의 영향이 많이 나타날 것은 이미 오늘 구축할 수 있는 것들을 다르게 구축하는 것입니다.

문제별 응용 프로그램 개발에서 문제에 중립적이고 일반 주의적인 생성 ai를 통합함으로써 주요 아키텍처 패턴, 조직적 기술 세트 요구 사항 및 개발 비용 계산식이 변경될 것입니다. 이것이 개발 조직이 반짝이는 새로운 생성 ai 응용 프로그램을 만들지 않더라도 생성 ai의 능력(그리고 한계!)을 이해해야 하는 이유입니다.

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

많은 사람들이 일반화된 솔루션으로 대체할 수 있는 전통 소프트웨어의 양을 과소평가합니다. 매일 나는 이렇게 생각하곤 합니다... "이건 정말 간단한 프롬프트일 수도 있겠네요".

아래에는 몇 가지 예시가 있습니다. 이들은 기술적으로 가장 탁월하거나 사회적으로 영향력이 큰 것은 아니지만, 어떤 종류의 생성적 인공지능으로 많은 소프트웨어가 대체될 수 있는지 보여줄 수 있을 것입니다.

## 연못 수위

저는 연못이 있어요. 연못의 수위는 계절과 날씨에 따라 크게 변해요. 수위를 숫자로 측정하고 오랜 기간 추적하고 싶은 생각이 들었어요.

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

지속적으로 원격으로 수위를 측정하려면 수위 측정값을 디지턈 레코딩으로 변환하여 사용자 정의 소프트웨어로 해석해야 하는 게이지가 필요합니다. 미국 지질 조사국이 강의 흐름을 측정하기 위해 사용하는 이 도구들은 설치가 필요하며 값이 싸지 않습니다. 이들은 물의 수위를 측정하기 위한 특정 문제에 대한 솔루션이며 제작하는 데 노력과 엔지니어링을 필요로 합니다.

나는 그러한 엔지니어링 스킬을 가지고 있지 않으며 비싼 디지털 게이지를 사는 데 돈을 쓰고 싶지 않습니다. 여기 다른 해결책이 있습니다. 이것은 생성적 인공지능으로 가능해졌죠:

- 웹 연결 카메라를 연못에 향하게 설정합니다 (이미 가지고 있습니다)
- 연못에 눈금이 있는 막대를 넣습니다
- 촬영한 이미지를 다중 모달 LLM(GPT-4o)에게 보내 "물이 얼마나 높은지" 물어본 후
- 답변을 분석하여 기록합니다.

사용자 정의 게이지는 필요 없습니다. 사용자 정의 코드도 적게 필요하며 단지 조각들을 함께 연결하는 몇 가닥의 파이프만 있으면 됩니다. 여기 연못의 이미지와 GPT-4o의 채팅 인터페이스를 통해 수위를 제시한 것입니다.

![Pond Image](image-url)

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

이것이 단지 채팅 인터페이스일 뿐이라는 것을 이미 알고 계시겠지만, 카메라에서 이미지를 가져와 GPT-4o URL로 전송하고 응답을 파싱하고 기록하기 위한 코드를 작성해야 합니다. 그러나 디지털 물 모니터링 게이지를 구축하는 것과 비교하면, 이것은 간단한 공학 문제입니다.

LLM 솔루션이 사용자 정의 수위 게이지만큼 효율적이고 신뢰할까요? 아마도 아닐 것입니다. 눈이 LLM을 혼란스럽게 만들 것으로 예상됩니다. 카메라에는 적외선이 있지만, 매우 낮은 조명 상황에서 문제가 발생합니다. 명시적인 지침에도 불구하고 GPT-4o는 항상 요청한 대로 응답하지 않아 파싱을 망칠 수 있고 재시도를 강제할 수 있습니다. 측정 간격은 OpenAI 구독으로 제한되며, OpenAI는 가끔 다운 타임을 경험하기도 합니다. 고려하지 않은 LLM 솔루션의 다른 단점도 있을 것입니다.

그러나 비용이 적게 들어 빠르게 구현될 수 있습니다. 그리고 모델의 지속적인 개선으로 이미지 평가가 로컬에서 무료로 수행될 수 있게 되어, GPT-4o에 원격 호출이 필요하지 않아질 것입니다.

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

물 레벨 측정은 정확히 섹시한 사용 사례는 아니지만, 이는 LLM이 이전에는 사용자 정의 솔루션이 필요했던 기능을 쉽게 복제할 수 있다는 것을 보여줍니다.

# 글로벌 일기 예보 설명

많은 날씨 사이트(웨더 언더그라운드, AccuWeather 등)는 국가 기상청이 운영하는 Global Forecasting System에서 생성된 원시 일기 예보 데이터의 인간이 이해할 수 있는 설명을 제공합니다. 그러나 위치와 시간의 수가 많기 때문에 이러한 장문의 예보는 알고리즘을 사용하여 생성해야 합니다.

예를 들어, 다음은 특정 시간과 장소(6월 20일 덴버 근처)의 예보입니다:

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

소나기와 천둥번개가 50% 확률로 발생할 수 있으며, 주로 자정 전까지 이어질 것으로 예상됩니다. 대체로 흐린 날씨로, 최저 기온은 약 60도입니다. 남풍이 15~20mph로 강하게 불다가 저녁에는 9~14mph로 감소할 것으로 예상되며 돌풍이 34mph까지 불 수도 있습니다.

7월 남부 텍사스 지역을 위해 제공된 정보:

허리케인 조건이 가능합니다. 저녁 7시 이전에 소나기가 있을 가능성이 높고 천둥번개도 있을 수 있습니다. 그 후 7시부터 10시 사이에는 소나기와 천둥번개가 있을 가능성이 높으며, 10시 이후에도 소나기가 있을 가능성이 있습니다. 대체로 흐린 날씨로, 최저 기온은 약 77도입니다. 강수 확률은 60%입니다.

이러한 생성된 정보 설명은 날씨 예보에 따라 매우 다양할 수 있습니다. 때로는 풍속, 기온, 강수량 등 다양한 측면을 강조하며, 문장의 순서와 형식이 크게 변경될 수 있습니다. 이러한 알고리즘을 오늘 제작하기 위해서는 복잡한 코드를 작성해야 할지, 아니면 기존 LLM의 능력을 활용하여 더 빠르게 구현할 수 있는지 고려해볼 필요가 있습니다.

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

To give you an idea, GPT-4o predicts the weather for Seattle, WA from a single image provided by GFS:

![Weather Prediction](/assets/img/2024-07-12-AIIsEatingYourAlgorithms_2.png)

Based on the weather map provided, here are the expected weather conditions for Seattle, WA:

- A low-pressure system (L) is located offshore to the west of Seattle.
- The green shading on the map indicates a significant amount of precipitation, suggesting rain.
- The central pressure of the low is around 997 mb, indicating a fairly strong system.
- The pressure gradient is tight, hinting at the possibility of windy conditions.
- With the proximity of the low-pressure system, Seattle can anticipate rain and potentially windy conditions on Friday, June 28, 2024.

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

요약하자면, 2024년 6월 28일 시애틀, 워싱턴은 서쪽에서 다가오는 저압 시스템 때문에 비 오고 바람 부는 날씨를 경험할 것으로 예측됩니다.

이 예보는 틀렸어요. 6월 28일은 아름다운 맑은 날씨였고, 예상된 비는 나준 뒤에야 왔습니다. 그러나 이 응답에서 보여주는 LLM이 가진 이해력을 고려해보세요: 그것은 시애틀이 지도상 어디에 있는지, 녹색이 어떤 의미를 가지는지, 등압선이 날씨에 어떤 영향을 미치는지, 날씨 시스템이 서쪽에서 동쪽으로 움직인다는 것을 알고 있었습니다. 명시적으로 말해주지 않아도 날짜를 읽을 수 있었죠. 모든 이점을 고려하더라도 LLM은 날씨 데이터 해석을 특히 훈련받지 않았음에도 가능했습니다.

만약 이미지에 사용된 기본 데이터에 접근할 수 있고 충분히 강력한 LLM을 보유한다면, 간단한 프롬프트 공학만으로 정확한 장기 예보 설명을 생성할 수 있습니다. 현재 사용 중인 사용자 정의 알고리즘을 복제하거나 능가할 것입니다. 어떠한 복잡한 데이터도 기술적으로 프롬프트를 통해 플랫폼용 LLM에 의해 실행될 것입니다. 특정한 용도에 맞춰 쓰여진 코드가 아닌 것입니다.

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

알 수 없는 종류의 체리 나무가 주기적으로 어떤 종류의 잎 질병에 시달리고 있습니다. 저는 새로운 과수원 주인이라서 나무에 무엇이 고통을 주는지나 그에 대해 어떻게 해야할지 전혀 모릅니다.

식물과 질병을 식별해주는 애플리케이션이 있습니다. 이러한 애플리케이션들은 주로 유료이며, 구축 비용이 발생하기 때문입니다.

예를 들어, PictureThis는 안드로이드 플레이 스토어에서 1억 명 이상의 사용자가 있는 가장 인기 있는 식물 식별 앱입니다. 첫 달에는 $2.99, 그 후 매년 $49.99의 요금이 부과됩니다.

![image](/assets/img/2024-07-12-AIIsEatingYourAlgorithms_3.png)

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

PictureThis는 이미 AI/ML 기반 응용 프로그램입니다. 그러나 GPT-4o와는 달리, 특정 유형의 질문에 대답하기 위해 특정 데이터 세트에서 훈련된 사용자 정의 솔루션입니다.

GPT-4o는 식물이나 식물 질병을 식별하도록 훈련되지 않았습니다. 확실히 모든 PictureThis에 투입된 노고와 엔지니어링을 복제하고 대체할 수는 없겠죠?

워밍업 테스트로서, 여기 GPT-4o 가 일하러 가는 길에 감탄한 나무를 식별한 결과입니다 (정확히 일치했습니다).

![Click to see the identified tree](/assets/img/2024-07-12-AIIsEatingYourAlgorithms_4.png)

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

당신의 질병에 걸린 체리 나무를 확인해 봅시다:

이것은 6월에 서부 워싱턴에서 촬영된 체리 나무 잎의 이미지입니다. 이 정보를 고려할 때, 나무에 어떤 것이 영향을 줄까요?

![체리 나무 잎 사진](/assets/img/2024-07-12-AIIsEatingYourAlgorithms_5.png)

사진 속 체리 나무 잎에는 체리 나무에 영향을 미치는 흔한 문제인 잎의 포기병이 나타났습니다. 구멍이 뚫린 작고 동글한 얼룩들이 나타나는데, 이는 곰팡이 Blumeriella jaapii에 의해 유발된 체리 잎 포기병의 전형적 증상입니다. 시간이 지나면 얼룩은 커지고, 영향을 받은 잎들은 노랗게 변하고 일찍 떨어질 수 있습니다.

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

이 병을 관리하고 예방하는 데 도움이 될 수 있는 몇 가지 단계가 있습니다:

- 위생: 낙엽과 감염된 식물 부스러기를 제거하고 처분하여 감염원을 줄입니다.
- 진균제 사용: 신장 기간 중에 적절한 진균제를 사용하며, 특히 봄철 신 잎이 돋을 때에 더욱 신경써야 합니다.
- 가지치기: 나무를 가지치기하여 캐노피를 열어 공기 순환이 개선되도록 하여 곰팡이 성장을 촉진하는 습도를 줄일 수 있습니다.
- 물 관리: 잎이 젖는 기간을 최소화하기 위해 상단 관개를 피하십시오. 곰팡이 감염에 중요한 시기입니다.

문제가 지속되는 경우, 현지 농업 연구소나 전문 수목원사와 상담하여 특정 지역에서 체리 잎 무늬병을 관리하는 데 더 맞춤형 조언을 얻을 수 있습니다.

해당 사용 사례에 적합하지 않은 소프트웨어로 정확하고 상세한 답변입니다.

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

Imagine being a start-up investor and someone comes to you seeking funding to develop an app similar to PictureThis utilizing customized AI/ML models for identifying plant diseases. Would you confidently invest a million dollars, or hesitate due to the ever-growing capabilities of advanced software like GPT-4o?

Personally, I would tread carefully in this scenario. It's becoming increasingly common for individuals who have invested in such specialized applications to see them easily and inexpensively reproduced by generative AI technology.

From monitoring water levels to providing detailed weather forecasts and diagnosing plant diseases, the examples are endless. How many potential use cases can eventually be simplified to just an LLM call and a few adjustments? And what about the capabilities of such tools in the near future?

For anyone in the software development field, it's essential to grasp the potential of generative AI and LLMs. In the coming years, many software applications will likely integrate these technologies in subtle yet significant ways. While the advancements in generative AI are indeed fascinating, it's crucial to consider the substantial impact it may have on existing software systems as well.

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

마법사의 덱 안에 마법처럼 변신하는 힘있는 비전이 담겨져 있네요. 모든 기업이 소프트웨어 기업이라는 생각이 아니라, 모든 소프트웨어 기업이 생산적인 AI 기업이 될 수 있다는 슬기로운 아이디어를 담고 있군요. 그 흥미진진한 전환, 마법처럼 매료적이네요!
