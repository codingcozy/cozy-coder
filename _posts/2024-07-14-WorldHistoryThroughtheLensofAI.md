---
title: "AI로 바라본 세계사"
description: ""
coverImage: "/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_0.png"
date: 2024-07-14 02:14
ogImage:
  url: /assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_0.png
tag: Tech
originalTitle: "World History Through the Lens of AI"
link: "https://medium.com/towards-data-science/world-history-through-the-lens-of-ai-340df6241fbf"
isUpdated: true
---

![World History Through the Lens of AI_0](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_0.png)

안녕하세요! 이 기사는 제 블로그에서 원래 발행되었습니다.

인공지능의 발전, 특히 대형 언어 모델은 역사 연구와 교육에 흥미로운 가능성을 열어줍니다. 그러나 이러한 모델이 과거를 해석하고 상기하는 방식을 면밀히 살펴보는 것이 중요합니다. 그들이 역사에 대한 이해에서 고유한 편견을 반영하는지 확인해야 합니다.

저는 역사의 주관성을 잘 알고 있어요 (학부 시절에 역사를 전공했거든요!). 우리가 기억하는 사건과 과거에 대해 형성하는 이야기는 역사학자와 우리가 속한 사회에 크게 영향을 받죠. 예를 들어, 제 고등학교 세계사 강좌는 유럽 역사에 75% 이상을 할애해 전 세계 사건에 대한 제 이해를 흐트러뜨렸습니다.

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

이 기사에서는 AI의 시각을 통해 인류 역사가 기억되고 해석되는 방법을 탐구합니다. 여러 대형 언어 모델에 의한 주요 역사적 사건의 해석을 살펴보면서 다음을 파악합니다:

- 이러한 모델들은 사건에 대해 서양 또는 미국적 편향을 보이는가?
- 프롬프트로 사용된 언어에 따라 모델들의 역사적 해석이 다르게 나타나는가? 예를 들어, 한국어나 프랑스어 프롬프트가 한국 또는 프랑스 사건에 더 강조됩니까?

이러한 질문을 고려하여 함께 살펴보겠습니다!

# 예시: 1910

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

예를 들어, 저는 세 가지 다른 대형 언어 모델 (LLM)에게 1910년의 주요 역사적 사건이 무엇이었는지 물어보았습니다. (각 LLM에 대한 자세한 내용은 다음 섹션에서 확인할 수 있습니다.)

![image](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_1.png)

제가 제기한 질문은 객관적인 답이 없는 적재가있게 제작되었습니다. 1910년의 의의는 문화적 시각에 따라 크게 다릅니다. 한국 역사에서는 이 해가 일본의 점령의 시작으로, 그 나라의 궤적에 큰 영향을 미쳤다는 절박한 지점이었습니다. 그러나 일본의 한반도 합병은 어느 답변에도 나타나지 않았습니다. 제가 궁금해진 것은 같은 모델들이 언어가 다른 맥락에서 - 예를 들어, 한국어로 묻힌 경우 - 이 질문을 어떻게 해석할지입니다.

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

![Image](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_2.png)

한국어로 지시받은 것 중 하나는 Claude가 언급하는 것처럼 일본의 한국 병합입니다. 그러나 GPT-4가 중요한 이벤트로 지적한 것 중 미국 중심적인 것(소년 스카우트와 만-엘킨스 법)이 두 가지인 것을 발견했고, 한국의 병합을 언급하지 않았다는 점은 흥미로웠습니다. 게다가 Falcon은 한국어로 지시를 받았을 때도 영어로 응답했습니다.

# 실험

실험 설정은 다음과 같습니다:

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

- 3개의 모델: OpenAI의 GPT-4, Anthropic의 Claude, 그리고 TII의 Falcon-40B-Instruct
- 6개의 언어: 영어, 프랑스어, 스페인어, 한국어, 일본어, 중국어
- 3개의 연도 (610년, 1848년, 1910년)
- 실행 당 5개의 역사적 사건
- 10회 실행
- = 총 2700개의 사건

## 언어와 프롬프트

선택한 언어들은 대부분 임의로 선택되었고, 제가 가장 익숙한 언어(영어, 한국어)와 가까운 친구들이 구사하고 제게 번역할 수 있는 언어들(중국어, 일본어, 프랑스어, 스페인어)를 중심으로 선택했습니다. 번역은 글의 마지막에 제공됩니다. 그들에게 영어를 번역해달라고 요청했습니다:

"''해의 상위 다섯 가지 역사적 사건, 중요도 순으로 나열해주세요. 간단하게 요약하고 사건의 이름만 제공해주세요."

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

## 모델들

- OpenAI의 GPT-4는 ChatGPT의 최신 세대로, 매달 활성 사용자가 1억 명 이상인 인기 있는 AI 챗봇 중 하나입니다.
- Anthropic의 Claude는 헤로인리스하고 도움이 되는 ChatGPT 경쟁 모델로, 헌법 AI라는 방법을 사용하여 훈련되었습니다.
- Technical Innovation Institute의 Falcon-40B-Instruct는 HuggingFace의 Open LLM Leaderboard에 따르면 최고의 오픈 소스 언어 모델입니다.

## 사건 일반화

각 실행마다 모델이 동일한 이벤트를 생성하더라도, 동일한 이벤트를 설명하는 방식에 다양성이 많이 있었습니다.

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

예를 들어, 다음은 모두 동일한 사건을 가리킵니다:

- “일본의 한국 흡수”
- “일본의 한국 흡수”
- “일본이 한국을 흡수”
- “일본-한국 흡수 조약”

나는 동일한 어휘를 사용하여 단일 사건(일본의 한국 흡수)을 참조할 수 있는 방법이 필요했습니다. 또한, 동일한 사건이 여섯 가지 다른 언어로 설명될 수 있었습니다!

나는 normalization(정규화) 과정을 통해 일본의 한국 흡수 사건을 참조하기 위해 수동 규칙, Google 번역 및 GPT-4의 조합을 사용했습니다. 처음에는 다른 LLM의 사건을 정규화하는 것(예: GPT-4를 통해 Claude의 사건을 정규화; Claude가 Falcon의 사건을 정규화하는 것 등)을 통해 편향을 줄일 수 있도록 하기를 희망했습니다. 하지만, Claude와 Falcon은 정규화 지침을 따르는 데 그리 능숙하지 않았고, GPT-4가 이 작업에 가장 적합한 모델로 나타났습니다.

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

내가 GPT-4의 다른 세션을 사용하여 역사적 사건을 생성하고 정규화하는 것에 따른 편견을 인식한다. 그러나 콘텍스트에 중복이 없었기 때문에 이벤트를 정규화하기 위해 GPT-4의 다른 세션을 사용했다. 미래에는 보다 객관적인 방법을 사용하여 정규화할 수 있다.

# 결과

전체적으로, 각 모델이 역사를 이해하는 방식에 놀라웠다.

- GPT-4는 어떤 언어로 프롬프트되었는지에 관계없이 동일한 이벤트를 생성하는 경향이 있었다.
- Anthropic은 프롬프트된 언어와 관련된 역사적 이벤트를 생성하는 경향이 있었다.
- Falcon은 (유감스럽게도) 가짜 이벤트를 만들 가능성이 높았다.
- 세 모델 모두 서양 또는 미국의 이벤트에 대한 편견을 보여주었지만, 예상과는 다른 방식이었다. 영어 이외의 언어로 프롬프트된 경우, 모델은 미국 또는 영국의 역사적 사건을 생성하는 경향이 있었다 (영어로 프롬프트된 경우에는 해당 이벤트를 생성하지 않는 모델의 경우도 포함). 세 모델 모두 이 같은 현상이 나타났다.

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

# 1. 각 모델의 언어별 비교 (1910)

각 모델 x 언어 조합은 "상위 5개의 역사적 사건"을 10번 생성했습니다 (= 총 50개의 사건). 적어도 하나의 언어가 5번 이상 생성한 사건들의 하위 집합을 취했습니다. 이것은 모델이 때때로 한 번만 예측한 이벤트가 있었기 때문입니다. 값이 10인 셀들은 모델이 해당 이벤트를 프롬프트할 때마다 매번 예측했음을 의미합니다.

이 섹션에서는 3개 모델마다 예측한 상위 이벤트를 언어별로 나눠서 보여드립니다. 610년과 1848년에 대한 유사한 차트는 GitHub 페이지에서 찾을 수 있습니다. 거기에는 모든 코드와 분석을 공유했습니다.

GPT-4 (OpenAI)

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

- 멕시코 혁명: 모든 언어에서 멕시코 혁명은 항상 중요한 세계 사건으로 여겨졌어요. 한국어나 일본어 같은 언어에서도 예상치 못한 곳에서 중요한 사건으로 언급되었죠.
- 일본의 한국 국유화: 스페인어나 프랑스어로 물었을 때는 언급되지 않았어요. 하지만 일본어로 물었을 때, 한국을 값을 언급한 횟수가 한국어로 물었을 때보다 더 많았어요 (9번 대 6번). 이것은 흥미롭고 이상하게 느껴졌죠.
- 미국 보이 스카우트 창립: 일본어로 물었을 때, GPT-4는 이 사건을 거의 두 배나 더 언급할 확률이 있었어요 (7번 대 4번). 이것은 1910년대 미국 정보의 일부가 일본어의 이해에 인코딩된 것 같아요.
- 글래셔 국립 공원 설립: 심지어 영어로 물었을 때는 언급되지 않았지만, 스페인어와 프랑스어로 물었을 때, GPT-4는 이 사건을 예측했어요.

![이미지](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_3.png)

클로드 (Anthropic)

전체적으로: GPT-4와 달리, 모든 언어에서 "중요한 역사적 사건"으로 평가된 단일 사건은 없었어요.

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

- 멕시코 혁명: 프랑스어, 스페인어, 그리고 (이상하게도) 한국어로 묻히면 자주 생성되지만, GPT-4에서처럼 영어에서는 중요성이 떨어짐
- 일본의 한국 병합: 해당 사건은 주로 한국어와 일본어에서 더 중요하며, 해당 사건에 관여된 두 나라에게 중요
- 에드워드 7세의 사망: 영어와 프랑스어에게 더 중요하며, 다른 언어들에게는 그렇지 않음. 에드워드 7세는 영국 왕이었으며, 프랑스와 좋은 관계를 가졌다고 전해짐
- 남극 탐험: 이 사건은 사실 영국 남극 탐험으로, 처음으로 영국인이 남극에 도착한 일이다. 그러나 이 사건은 무슨 이유에서인지, 클로드가 중국어나 일본어로만 질문 받았을 때에만 생성됨 (영어로는 아닐 때에)

![그림](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_4.png)

팔콘 40B 지시 (오픈 소스; TII)

전반적으로 팔콘은 다른 두 모델보다 일정성과 정확성 면에서는 떨어지는 편이었습니다. 차트에 표시된 이벤트 수가 적은 이유는 팔콘이 5번 이상 예측한 다른 이벤트가 없었기 때문입니다! 즉, 팔콘이 예측하는 데 일관성이 조금 떨어졌다는 것을 의미합니다.

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

- 타이타닉 침몰: 이 일은 실제로 1912년에 발생했어요
- 제1차 세계 대전 발발: 이 사건은 실제로 1914년에 일어났죠
- Falcon은 예언에서 역사적으로 부정확합니다. 하지만 적어도 그 실제 시대는 맞아떨어졌네요?

![이미지](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_5.png)

# 2. 각 언어별 모델 상관 관계 비교 (1910)

이제, 하나의 모델의 전반적인 예측이 다른 모델들과 얼마나 유사한지 비교해보겠습니다. 두 개의 예측 분포가 얼마나 유사한지 결정하기 위해 수학적인 방법(코사인 유사도)을 사용했어요. 값이 1에 가까울수록 예측이 동일함을 나타내고, 0에 가까울수록 두 개의 예측 세트가 아무 관련이 없다는 것을 나타내요.

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

다시 한 번, 1910년에 대한 예시를 보여드리겠습니다. 다른 연도에 대한 정보는 GitHub 페이지에서 확인하실 수 있습니다.

대부분의 언어에서 GPT-4와 Claude는 더 높은 상관 관계 값을 보였습니다. 즉, 모든 언어에 걸쳐 두 모델이 유사한 이벤트를 높은 확률로 예측했다는 것을 의미합니다.

한편 Falcon은 상대적으로 상관 관계가 낮았는데, 이는 그것의 역사에 대한 이해가 GPT-4와 Claude의 것과는 다르다는 것을 의미합니다.

![Image](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_6.png)

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

# 3. 각 연도별 모델 비교

그 다음으로, 각 연도에 대해 다른 언어 모델을 비교해봤어. 모든 언어로 예측된 모든 이벤트를 결합하고, 언어에 상관없이 모델이 예측한 전반적인 이벤트를 고려했어. 적어도 하나의 모델이 10번 이상 생성한 이벤트들의 하위 집합을 가져왔어.

위 섹션에서 발견된 추세와 유사하게, GPT-4와 Claude는 각 연도에 대해 비슷한 주요 역사적 이벤트들을 예측하는 경향이 있었어 — 610년의 무함마드의 첫 계시와 비잔틴 제국 황제 헤라클리우스의 승천; 1848년의 유럽 혁명; 그리고 1910년의 멕시코 혁명.

다른 모델들에 비해 특정 이벤트를 지나치게 예측하는 경우도 있었어. 예를 들어, 1848년에 대해 GPT-4은 "공산당 선언서 발표"라는 이벤트를 42번 예측했는데, Claude는 15번에 불과했어. 1910년에는 Claude가 "에드워드 7세 사망"을 26번 예측한 반면, GPT-4는 1번에 불과했어.

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

오염타오와 함께하면서 역사적 사건에 대한 가장 적은 이해를 가지고 있었어요. 세 해 동안 중요한 사건을 놓쳤죠. 610년에는 헤라클리우스 황제의 승첨을 예측하지 못했어요. 1910년에는 한국의 일본 병합, 남아프리카 동맹 형성, 그리고 포르투갈 혁명과 같은 세계적 사건을 예측하지 못했는데, 그 대신 1911년에 벌어진 Triangle Shirtwaist Factory Fire와 같은 미국 중심의 사건을 예측했어요. 재미있게도, 오염타오는 다른 두 모델과 비슷하게 1848년의 대부분의 사건을 예측했는데요 – 아마도 1848년의 사건들이 더 서방 중심이었기 때문일 수도 있습니다(예: 유럽 혁명들)?

더 오래된 시기의 사건들(예: 610년)은 역사가 조금 더 모호해져요. 당나라는 618년에 세워졌고, 대운새 황제 아래의 대운수의 건설은 실제로 더 오랜 시간 동안(604년부터 609년까지) 완료되었어요.

610

![image](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_7.png)

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

1848

![World History Through the Lens of AI - Card 8](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_8.png)

1910

![World History Through the Lens of AI - Card 9](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_9.png)

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

# 토론

그래서 이 모든 것이 왜 중요한 걸까요?

교육 기업들이 대형 언어 모델 (LLMs)을 제품에 점점 더 통합함에 따라 -- Duolingo는 언어 학습을 위해 GPT-4를 활용하고, 칸 아카데미는 AI 교육 보조 '칸미고'를 도입하며, 하버드 대학은 컴퓨터 과학 교육에 AI를 통합하기 위한 계획을 세우고 있다 -- 이러한 모델의 내재적인 편향을 이해하는 것이 중요해졌습니다. 학생이 역사 학습을 위해 LLM을 사용한다면, 그들이 무의식적으로 어떤 편향을 흡수할 수 있을까요?

본문에서 보여진 바와 같이, GPT-4와 같은 일부 인기 있는 언어 모델은 프롬프트 언어에 관계없이 일관되게 “중요한 사건”을 예측합니다. 다른 모델인 Claude는 더 언어 특정적인 예측을 보여주었습니다. 폐쇄형 모델은 일반적으로 선도적인 오픈 소스 대안보다 일관성과 정확도가 더 높았습니다. 이 기사에서 시험한 모든 모델들을 통틀어, 서구 또는 미국의 사건을 (신비한 사건조차도) 다른 세계적 사건을 희생시키며 예측하는 경향이 있었습니다.

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

미래 작업에는 다음이 포함될 수 있습니다:

- 더 많은 언어와 연도를 포함한 분석 확대
- 모델 결과의 역사적 정확성에 대한 심층 분석
- 최상위 역사적 사건의 순위에 대한 심층 분석
- 이벤트 정규화에 대한 더 객관적인 방법 개발

본 기사의 목표는 LLMs를 비하하거나 교육 환경에서 제거해야 한다는 것이 아니라, 그들의 편향성을 인식하고 완화하는 비판적이고 신중한 접근을 촉구하는 것입니다. 책임있게 사용될 때 LLMs는 교과계에서 학생들과 교사들 양쪽에게 소중한 자료가 될 수 있습니다. 그러나 서양 중심주의와 같은 편향성을 이해하고 그들의 사용을 그에 맞게 조절해야 합니다.

역사 교수나 교과서를 LLM으로 대체하는 것은 왜곡된 편향된 역사 해석을 낳을 우려가 있습니다. 궁극적으로, 우리는 이 도구들을 신중하게 활용해야 하며, 그들이 지닌 고유한 편향성을 인식하여 세계를 이해하는 데 도움이 되는 도구로 활용해야 합니다.

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

이 글을 읽어 주셔서 감사합니다!

이 기사는 원래 제 블로그에 게시되었습니다. 새로운 글을 찾으려면 팔로우해주세요!

## 실수 모음

저는 몇 가지 다른 오픈 소스 모델을 시도해 봤어요. 아래는 발견한 이상한 결과물들의 몇 가지 실수 모음(모두 한국어)입니다!

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

## Falcon 40B Instruct

![Image 1](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_10.png)

![Image 2](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_11.png)

## Pythia 12B

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

모델이 캥거루, 항공 우편, 그리고 'торговать'의 변형들로 이루어진 반복 루프에 갇혔다고 보입니다.

![이미지](/assets/img/2024-07-14-WorldHistoryThroughtheLensofAI_12.png)

# 번역

```js
영어: "{}년에 일어난 중대한 역사적인 사건 다섯가지는 무엇인가? 간단히 그 사건의 명칭들을 열거하시오.";
한국어: "{}년에 일어난 중대한 역사적인 사건 다섯가지는 무엇인가? 간단히 그 사건의 명칭들을 열거하시오.";
중국어: "{} 年的前五个历史事件。 简短一点，只给出事件的名称。";
프랑스어: "{}년에 일어난 중대한 역사적인 사건 다섯가지는 무엇인가? 간단히 그 사건의 명칭들을 열거하시오.";
일본어: "{}年にあったトップの出来事を五つイベント名のみで簡潔に記載してください。最も重要な出来事から。";
스페인어: "{}년에 일어난 중대한 역사적인 사건 다섯가지는 무엇인가? 간단히 그 사건의 명칭들을 열거하시오.";
```
