---
title: "2024년 가장 저렴하고 강력한 LLM Platypus"
description: ""
coverImage: "/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_0.png"
date: 2024-07-14 02:04
ogImage:
  url: /assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_0.png
tag: Tech
originalTitle: "Platypus: Quick, Cheap, and Powerful LLM"
link: "https://medium.com/gitconnected/platypus-quick-cheap-and-powerful-llm-404b86af8755"
isUpdated: true
---

## |인공지능|LLMS|NLP|

![PlatypusQuickCheapandPowerfulLLM](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_0.png)

오픈 LLM 리더 보드의 상위 위치에 도달한 세밀하게 조정된 모델은 어떻게 이루어졌을까요?

# 모델 비용을 줄이는 방법은 무엇일까요?

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

이미지 링크는 아래와 같이 Markdown 형식으로 변환해 주세요.

![Platypus Quick Cheap and Powerful](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_1.png)

요즘들어 모델 파라미터 수는 엄청난 숫자로 폭발적으로 증가했습니다(PaLM에서 540 B). 이렇게 많은 파라미터가 필요한지 의문이 제기되고 있습니다.

OpenAI에 따르면, 모델 크기가 커질수록 성능이 향상되며, 새로운 특성이 나타날 수 있다고 합니다(특정 규모에서만 확인할 수 있는 특성들이죠).

이러한 관점은 실제로 더 많은 데이터가 필요하며, 모델을 최적으로 학습시키기 위해 필요한 토큰의 수에 제약을 받는다는 것을 고려해야 한다는 반론이 있습니다. 또한, 이러한 새로운 특성들이 실제로 존재하지 않을 수도 있다는 의견도 있습니다.

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

두 번째로, 이러한 독점 모델들은 과학 커뮤니티에서 자유롭게 분석되거나 사용될 수 없습니다. 그 결과, 커뮤니티는 BLOOM을 시작으로 META의 LLaMA로 나아가며 오픈 소스 모델을 사용하게 되었습니다. LLaMA는 데이터에 대한 관심을 높이는 것이 작은 모델들이 대형 모델들과 경쟁할 수 있게 만드는 것을 보여주기도 했습니다.

하지만, 작은 모델들은 대형 모델들만큼 일반화할 수 없습니다. 이로 인해 이러한 모델들의 비용을 줄이기 위한 기술들을 찾는 노력이 계속되었습니다. 가르침 증류(선생 모델이 학생 모델을 가르치는 방법)과 같은 지식 증류를 통해 이 비용을 줄이는 방식을 찾았습니다. 나중에는 대형 교육 데이터셋에서 시작하여 더 작지만 동시에 효과적인 데이터셋으로 증류하는 방법을 모색하기도 했습니다.

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

다른 아이디어로는 전문가들의 혼합이 비용을 줄이는 데 도움이 될 수 있습니다. 여기서 네트워크의 일부는 입력에 따라 활성화됩니다. 예를 들어, 스위치 트랜스포머에서는 각 예제마다 (그리고 서로 다른 토큰에 대해) 다른 매개변수 집합이 선택됩니다.

![Image](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_4.png)

반면에, 최근 몇 달 동안 대형 언어 모델 (LLMs)을 세밀하게 조정하는 기술이 개발되었습니다. LoRA 이전과 Quantized-LoRA 이후에도 많은 효율적인 훈련이 가능해졌으며 특정 작업이나 도메인에 특화된 모델이 등장했습니다 (코딩이나 생명과학 영역에 전용 모델).

# 플라티퍼스: 신속하고 저렴하며 강력합니다

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

![Image](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_5.png)

Recently, Platypus shared some exciting updates in an article. They introduced open-platypus, a meticulously curated dataset with separate training and test sets to ensure no contamination. The article also delves into the impact of redundancy, provides detailed method descriptions, code snippets, and additional valuable resources. Fascinating developments in the world of Platypus!

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

## Open-platypus, a human dataset

안녕하세요! 오늘은 "Open-platypus, a human dataset"에 관해 이야기하려고 해요.

저희 저자들은 기본 모델로 LLaMa-2의 세밀 조정을 결정했어요. 사실, 저자들은 세 가지 아이디어에서 영감을 받았지요: 모델은 사전 학습에서 대부분의 지식을 학습하며 정렬은 모델이 이 지식을 활용할 수 있도록 합니다. 기준 모델은 포화 상태에 이르지 않았으며 따라서 여전히 훈련될 수 있습니다. 데이터 품질은 성능 모델에 있어 중요합니다.

그래서 저자들은 데이터셋의 품질을 최대화하고 동시에 계산 효율성을 높이기 위해 데이터셋의 크기를 최소화하려고 합니다. 따라서 저자들은 공개 데이터셋을 선정하고 품질이 우수한 예제를 필터링하는데 중점을 두었어요 (특히 STEM에 초점을 맞추었습니다).

저자들은 총 11개의 데이터셋을 선정하고 주로 인간들이 생성한 질문을 선택했어요 (LLM이 생성한 질문의 약 10%만을 선택했습니다). LLM이 생성한 텍스트를 사용하는 것은 훈련에 있어서 최적이 아니기 때문이라고 해요.

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

**이미지**: [PlatypusQuickCheapandPowerfulLLM_6.png](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_6.png)

다양한 소스에서 질문을 가져왔기 때문에 저자들은 동일하거나 너무 유사한 질문을 확인하고 제거했습니다. 이는 모델이 답변을 기억하는 것을 방지하기 위한 것이었습니다:

- 중복 질문을 제거했습니다.
- 질문을 임베딩하기 위해 SentenceTransformers를 사용하고 유사한 질문을 제거했습니다 (코사인 유사도 80%)

## 테스트 세트를 오염시키지 마세요

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

\*\*저자들은 벤치마크 데이터셋의 질문 중 어떤 것도 훈련 세트에 유출되지 않도록 주의를 기울였습니다 (가장 흔한 오류 중 하나).

그러나 이 작업은 쉽지 않습니다. 왜냐하면 질문들이 유사할 수 있고, 질문을 요구하는 여러 가지 방법이 있기 때문입니다. 따라서 저자들은 비슷한 쿼리를 모두 제거했습니다. 실제로 그들의 분석 후, 잠재적인 유출로 간주된 질문을 세 가지 그룹으로 묶었다고 합니다:

- 복제. 복제된 쿼리 중 많은 것들이 정확한 복사본이거나 문장의 재배치 또는 일부 단어의 추가가 있는 것입니다.
- 회색 영역. 정확히 중복되지 않고 일반 지식 범위 내에 속하는 질문들입니다. 이러한 질문들은 전문가들에 의해 평가되어야 합니다. 왜냐하면 이러한 질문들은 동의어를 포함하거나 매우 유사한 지침을 가지고 있거나 재구성되어 있기 때문입니다.
- 유사하지만 다름. 이러한 질문들은 높은 코사인 유사도를 가지고 있지만 서로 다른 답변을 가지고 있습니다. 이것은 질문의 구조가 변경되었기 때문입니다.\*\*

![Image](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_7.png)

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

## 세부 조정

작가들은 QLoRA가 늦게 나와 LoRA를 사용했습니다. 그러나 미래에는 QLoRA를 사용할 계획입니다. 또한, State-of-the-art Parameter-Efficient Fine-Tuning (PEFT) 라이브러리를 사용하여 훈련 효율을 더욱 향상시켰습니다. 어쨌든, 그들은 작은 13B 모델을 단일 1 A100 80GB를 사용하여 5시간 동안 미세 조정할 수 있었다고 주장합니다. 또한, 파라미터 선택에 특별히 신경쓴 것으로 밝혀졌습니다.

![이미지](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_8.png)

또 다른 흥미로운 방법은 어댑터를 학습하고 나면 다른 모델들과 통합된다는 점입니다.

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

Here is the code for each case has been published and is available [here](https://yourwebsite.com).

You can also find it in IPython Notebook and detailed online documentation.

### Results: How did it work?

![Platypus Quick Cheap and Powerful](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_9.png)

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

작성자들은 HuggingFace 리더보드를 활용하여 모델 결과를 비교하기로 결정했습니다. 작성자들은 8월에 모델이 리더보드에서 1위를 달성했다고 언급하고 있습니다:
![이미지](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_10.png)

작성자들은 접근 방식이 기본 모델(LLaMA2)의 성능을 제고한다는 것을 강조하며, 특히 작은 모델의 경우 병합이 흥미로운 결과를 낳는다고 언급합니다 (작성자들에 따르면, 병합은 모델이 인식하지 못했던 정보에 접근하게 함). 따라서 병합은 모델 성능을 높이는 더 낮은 비용의 전략으로 고려될 수 있습니다. 물론, 이 기술에도 한계가 있습니다: 도메인에 따라 결과가 달라지며, 대수학에서는 오히려 영향이 적을 수 있습니다. 따라서 모델과 응용 프로그램 도메인에 대해 신중한 선택을 통해 병합이 이루어져야 합니다.

작성자들은 또한 모델이 오픈 소스 모델 중에서 최초로 나타났다고 언급합니다.

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

![Image](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_11.png)

실제로 오리너구리는 HuggingFace 리더보드에서 최근에 추월당했습니다. 그 성능은 상당히 놀랍다는 것이 밝혀졌습니다.

![Image](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_12.png)

# 한계

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

![image](/assets/img/2024-07-14-PlatypusQuickCheapandPowerfulLLM_13.png)

당연히, 이 모델에는 제약사항이 있습니다. 이 중 일부는 모델이 기본 모델인 LLaMA2에서 세밀하게 조정되었기 때문입니다. 사실, 연속 학습과 호기심을 가질 수 있고, 편향과 유해한 콘텐츠를 생성할 수 있습니다.

LLaMA2는 주로 영어 텍스트로 훈련된 모델이기 때문에 다른 언어에 대한 능력은 상대적으로 낮습니다. 나중 연구에서 LLM은 악의적인 목적(비정보의 전파 또는 민감한 주제의 조사)으로 사용될 수 있다는 것을 보여줍니다. 이것은 Platypus에도 적용됩니다.

Platypus가 STEM 분야로 훈련되었음에도 기본 전문 분야 이외의 주제를 다룰 때 어려움을 겪을 수 있습니다.

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

마침내, 작가들은 오염을 피하기 위해 주의깊게 한 것 같지만, 여전히 걸러내지 않은 질문들이 남을 수 있습니다.

# 결론

모델을 훈련하는 데는 비용이 많이 들지만, 리더보드에서 작은 모델이 일부 작업에서 성공적일 수 있다는 것을 알 수 있습니다. LoRA 및 기타 기술의 사용으로 LLM에 대한 접근이 더 민주화되었습니다. 이 연구는 전문가 영역이 실현 가능한 접근법이며, 어댑터를 통합하는 방법 및 품질 데이터 세트를 확보하는 방법을 보다 명확히 보여줍니다.

미래에는 각 업무 영역에 대한 어댑터가 각각 있어서 기본 모델과 함께 사용될 것이며, 작업에 따라 어댑터가 변경될 수도 있을 것입니다.

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

## 남성들은 무엇을 생각하시나요? 댓글로 알려주세요

# 만약 여러분이 이 글을 흥미롭게 여기셨다면:

제 다른 글을 찾아보세요, 또한 게시글을 게시할 때 알림을 받기 위해 구독할 수도 있고, LinkedIn에서 저와 연결하거나 연락할 수도 있습니다.

제 GitHub 저장소 링크는 여기 있습니다. 그 곳에서 기계 학습, 인공 지능 및 기타 다양한 자원과 관련된 코드를 수집할 계획입니다.

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

또는 제 최근 게시물 중 하나가 궁금할 수도 있습니다:
