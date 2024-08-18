---
title: "영화 대사 분석  장르를 예측하는 통사적 및 의미적 특징은 무엇인가"
description: ""
coverImage: "/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_0.png"
date: 2024-07-13 03:38
ogImage:
  url: /assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_0.png
tag: Tech
originalTitle: "Evaluating Cinematic Dialogue — Which Syntactic and Semantic Features Are Predictive of Genre?"
link: "https://medium.com/towards-data-science/evaluating-cinematic-dialogue-which-syntactic-and-semantic-features-are-predictive-of-genre-2c69a71af6e2"
isUpdated: true
---

## 자연어 처리

![이미지](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_0.png)

스릴러 영화의 단편화된 대화에서부터 액션 영화의 욕설이 가득한 대화까지, 영화의 장르를 알기만 하면 대사의 시맨틱 및 구문적 특성을 통해 감추어볼 수 있을까요? 그렇다면, 어떤 특성들이 그것을 알려줄까요?

우리는 대본 내 섬세한 대화 패턴들 - 어휘, 구조, 그리고 리듬 - 이 장르를 예측하는 강력한 요소가 될 수 있는지 여부에 대해 조사할 것입니다. 여기서 주목해야 할 점은, 구문적 및 의미론적 대본 특성을 예측적 요소로 활용하고, 숙고된 특성 공학의 중요성을 강조하는 것입니다.

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

많은 데이터 과학 과정에서 주요 부족한 점 중 하나는 도메인 전문 지식과 피처 생성, 엔지니어링, 그리고 선택에 대한 강조가 부족하다는 것입니다. 많은 과정들이 이미 정리된 데이터셋을 제공하기도 하는데, 때로는 이미 정제된 데이터셋들이 제공되기도 합니다. 게다가 직장에서 결과물을 빨리 내야 하는 바쁜 상황 속에서 예측적인 피처를 가설 설정하고 검증하는 과정은 종종 후미지고, 도메인 특정 탐구와 이해에 대한 여유 공간이 줄어듭니다.

저의 “다중 작업 및 앙상블 학습을 활용한 알츠하이머 인지 기능 예측”에 대한 경험에서 알 수 있듯이, 정보에 기반한 피처 엔지니어링이 어떻게 긍정적인 영향을 미칠 수 있는지 목격할 수 있었습니다. 알츠하이머의 알려진 예측 변수에 대한 연구를 통해 초기 작업과 데이터를 의심하고, 결과적으로 모델링 중 핵심 피처들을 포함하게 되었습니다.

![배경 이미지](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_1.png)

본 글에서는 영화 대화를 검토하여 연구 및 피처 추출 접근 방식을 설명하겠습니다. 텍스트, 의미론적, 문법적 요소를 식별 및 분석하고, 그들이 어떻게 관련되어 있는지 조사하며, 영화 장르를 정확하게 예측하는 능력을 평가할 것입니다.

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

# 초기 질문들

저는 모든 프로젝트를 시작할 때 문헌 조사를 진행하는 것을 좋아해요. 관련 개념과 질문들을 메모하여 리뷰를 안내하는 것으로 시작합니다. 이 초기 단계는 매우 중요하며, 가지고 있는 시간에 따라 일부러 모델링 문제와 직접적으로 관련된 연구를 피하도록 합니다. 목표는 보다 넓은 맥락을 이해하고 우선 보충 정보를 찾는 것입니다. 이 전략은 주제에 대한 편겨되지 않은 이해를 육성하여 문제에 대한 접근 방식이 타인이 이미 탐구한 솔루션과 방법론에 조기에 좁히지 않도록 보장합니다.

## 제 메모해 둔 몇 가지 질문들:

- 대화와 감정 사이에는 어떤 관계가 있을까요?
- 실생활에서의 대화와 대본에서의 대화는 어떻게 다를까요?
- 영화 대사를 통해 장르에 대해 어떤 이해를 얻을 수 있을까요?

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

## 내가 발견한 것

자연스러운 대화와 우리 감정 사이의 상호작용을 탐구하는 문학적 연구들이 있습니다. 대본작가들은 텍스트와 문법적 관계에서 감정이나 분위기를 잡아냅니다. 이러한 특징들은 장르에 따라 다르기 때문에 다양한 장르가 다른 감정과 연관이 있습니다.

# 이 문법상과 텍스트적인 특징들은 무엇인가요?

아래 나열된 네 가지 특징을 추출하고 평가할 것입니다. 각 섹션에서 나는 그 이유에 대해 설명하겠습니다:

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

- 길이 특성
- 문장 유형
- 품사 및 저주
- 감정 분석

## 데이터

여기서 사용된 데이터셋은 Kaggle의 코넬 영화 대화 말뭉치 (MIT 라이센스)이며, 원래는 ConvoKit 툴킷(Chang et al., 2020)에서 얻었습니다. 이 데이터는 약 220,000개의 대화 교환에서 파생된 617개 다른 영화에서의 30만 개 이상의 구어 문장으로 구성되어 있습니다.

## 데이터 불러오기

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

먼저 'movie_lines.txt' 파일을 사용하여 데이터를 로드하는 방법을 알아보겠습니다.

```python
# 'movie_lines.txt' 파일이 있는 디렉토리 경로를 정의합니다
corpus_directory = 'cornell movie-dialogs corpus'

# 전체 파일 경로를 구성합니다
file_path = os.path.join(corpus_directory, 'movie_lines.txt')

# 'mac_roman' 인코딩을 사용하여 파일을 읽기 모드로 엽니다
with open(file_path, 'r', encoding='mac_roman') as file:

    # 파일의 내용을 읽고 각 줄로 나눕니다
    lines = file.read().splitlines()
```

```python
lines[:2]
['L1045 +++$+++ u0 +++$+++ m0 +++$+++ BIANCA +++$+++ They do not!',
 'L1044 +++$+++ u2 +++$+++ m0 +++$+++ CAMERON +++$+++ They do to!']
```

열은 +++$+++로 구분되어 있으므로 이를 구분자로 사용하여 각 줄을 분할하고 열을 추출한 다음 데이터를 데이터 프레임에 읽어올 것입니다.

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

```python
# 'lines'를 ' +++$+++ '를 구분자로 사용하여 각 줄을 분할합니다
preprocessed_list = list(
    map(lambda x: (str(x).split(' +++$+++ ')), lines)
)

# DataFrame을 위한 열 이름을 정의합니다
column_names = ['line', 'speaker_id', 'movie_id', 'name', 'text']

# 'preprocessed_list'를 사용하여 DataFrame을 생성합니다
df = pd.DataFrame(preprocessed_list, columns=column_names)

# DataFrame의 처음 2행을 표시합니다
df.head(2)
```

![Image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_2.png)

## 텍스트 전처리

저는 spaCy를 사용했습니다. 이는 파이썬과 사이썬(Cython)으로 작성된 오픈 소스 자연어 처리 라이브러리입니다. 이를 사용하여 압축형을 정리하고, 구두점을 제거하며, 단어를 원형으로 바꾸는 작업이 포함되었습니다.

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

# 모든 축약형을 긴 형태로 변환합니다.

df['text'] = df.text.map(clean_contractions)

# 데이터에서 모든 구두점과 구두점 오류를 제거합니다.

df['text_no_punct'] = df.text.map(remove_punctuation)

# 2글자 미만의 단어와 불용어를 제거하고, 표제어 추출을 하며 소문자로 변환합니다.

df['clean_text'] = df.text_no_punct.map(
람다 x: 전처리(' '.join(x))
)

![Image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_3.png)

# 1. 길이 특성

긴장감 넘치는 영화에서 대화는 종종 간결하며, 구문과 감정 사이의 연결을 보여줍니다. 캐릭터가 공포에 떨 때, 그들의 말은 간결하고, 긴장감은 종종 긴 발언(예: 횡설수설)으로 이어지며, 이는 코미디에서 더 자주 볼 수 있는 특징입니다. 따라서 우리는 말뭉치의 각 줄의 길이 특성을 조사할 것입니다.

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

이 섹션에서는 다음을 살펴볼 것입니다:

- 한 줄의 평균 단어 수
- 코퍼스 전체의 평균 문장 수
- 한 줄당 평균 단어 수 분포
- 코퍼스 전체에서 한 줄당 평균 문장 수 분포

```js
# 각 줄의 단어 수 계산
df['num_words'] = df['text_no_punct'].map(len)

# 각 줄의 문장 수 추출
df['num_sentences'] = df['text'].map(
    lambda x: len(nltk.sent_tokenize(x))
)

# 비어 있거나 비텍스트 콘텐츠가 들어있는 항목 제거
df = df[df['num_words'] != 0]
```

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

이미지 태그를 Markdown 형식으로 변경해주세요.

## Statistics Per Movie: Boxplot and Statistics DataFrame

[Boxplot Image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_6.png)

[Statistics DataFrame Image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_7.png)

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

위의 상자그림과 통계 데이터 프레임에서 볼 수 있는 바와 같이:

- 단어 길이는 0부터 30까지의 범위를 가지며, 중앙값은 7입니다. 중간 50%의 데이터를 보존하는 사분위범위는 문장의 길이가 4에서 14글자인 가운데 50%의 범위를 나타냅니다.
- 문장들은 일반적으로 1 또는 2 문장으로 이루어져 있으며, 때때로 최대 3문장까지 늘어납니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_8.png)

대부분의 대본 라인은 1문장을 초과하지 않습니다. 이로써 각 대본 라인이 짧으며 그에 맞게 구성되어야 함을 알 수 있습니다.

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

## 영화 당 분포

위에서 언급한 메트릭은 영화 대본 데이터에서 '한 줄' 당 계산되었습니다. 다음 섹션에서는 각 영화별 대사 평균 길이를 살펴볼 것이며, 이를 통해 영화 수준에서의 단어 길이 변화를 조사할 수 있습니다.

![이미지](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_9.png)

## 대화 밀도 변화

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

**내레이티브 리듬**

대화에 대한 라인별 분석에서 39% 이상의 대본 라인이 한 문장 이상을 포함하고 있음을 보여줍니다. 그러나 영화 평균치의 표준 편차가 좁은(문장당 0.29) 것은 서로 다른 영화 사이에서 서사 리듬의 일관성을 시사하며, 대화 전달 속도를 일정하게 유지하려는 것을 보여줍니다.

![그림](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_10.png)

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

## 대본 일관성

개별 대사 길이의 중앙값(7 단어)과 영화 전반의 평균(11.36 단어) 간의 대조는 대본 작가들이 종종 대화의 짧은 줄을 긴 독백이나 교환과 함께 섞어 사용하는 것을 시사할 수 있습니다. 이 기술은 캐릭터 간 동적 상호작용을 만들거나 청중을 끌어들이고 각 영화가 독특한 리듬과 스타일을 갖도록 고의적으로 선택될 수 있습니다.

## 오른쪽으로 평균을 끌어올리는 이상치 시각화

히스토그램은 오른쪽으로 치우친 분포를 보여줍니다. 영화들이 일반적으로 7~13 단어를 평균으로 하는 성향을 가지고 있다는 것을 시사합니다. 이 치우침은 비정상적으로 긴 대사를 가진 소수의 영화들로 이어지며 전체 평균에 큰 영향을 미치고 있습니다.

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

---

![Image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_11.png)

![Image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_12.png)

After removing the outliers, the bimodal distribution for words per line becomes more apparent, suggesting that there are two common line lengths in scripts. This observation is intriguing as it may signify different styles or genres within the corpus. The distribution of sentences per line seems to be roughly normal, with a slight right skew, indicating a consistent sentence structure across screenplays.

## 2. Types of Sentences

---

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

## 느낌표

감정이 과도한 상태를 나타내는 다양한 방법이 있습니다. 강조를 위해 느낌표(!)를 사용하거나, 대문자로 강조할 수도 있습니다. 우리는 이 두 가지를 확인하여 총체적인 감정과의 상관 관계를 살펴볼 것입니다.

## 하이픈

대사 끝에 놓인 하이픈(-)은 캐릭터의 말을 가로막거나 캐릭터의 생각에서 갑작스러운 일시정지를 나타낼 수 있습니다 (예: 캐릭터가 깨달음을 얻는 경우). 또한 단편적인 말을 전달할 수도 있습니다.

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

## 질문

스크립트에서의 질문의 존재와 다른 특징 간의 관계에 대해 사전 지식이나 직감이 전혀 없었습니다. 그러나 질문의 비율은 쉽게 측정할 수 있고, 어떠한 패턴이 감지될 수 있는지 탐구하는 것이 흥미로울 수 있습니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_13.png)

아래에서 우리는 질문이 포함된 대사의 비율이 31.4%인 것을 볼 수 있습니다. 이는 영화에서 상호작용적인 대화를 선호하는 강한 성향이 있음을 시사합니다. 이 비율은 느낌표가 포함된 대사의 비율인 8.9%보다 상당히 높으며, 이는 강렬한 감정 표현이 존재하지만, 의문문 교환보다는 덜 자주 발생한다는 것을 시사할 수 있습니다.

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

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_14.png)

올 캡스 워드의 카운트에 대한 상자그림 결과, 대문자 사용이 일반적이지 않음을 보여줍니다. 대본 작가들은 텍스트 포맷보다는 더 섬세한 강조 전달 방법을 선호할 수 있음을 시사합니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_15.png)

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_16.png)

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

질문은 더 흔하지만, 영화에서의 사용 범위는 장르나 감독 스타일에 따라 매우 다양합니다. 예를 들어, 스릴러는 긴장감을 유지하기 위해 대화에 더 많은 질문을 넣을 수 있고, 코미디는 펀치라인을 강조하기 위해 감탄사를 사용할 수 있습니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_17.png)

하이픈으로 끝나는 대사의 히스토그램은 하이픈으로 끝나는 대사가 영화 대본에서 비교적 드물다는 것을 나타내는 낮은 비율로 향하는 융통성을 보여줍니다. 이는 대화의 흐름을 유지하거나 그 외의 효과를 과용하지 않고 사용하기 위해, 대화를 중단시키거나 행동으로 이어지는 문장(이러한 경우 일반적으로 하이픈으로 나타남)을 삼소품으로 사용하는 것으로 추측될 수 있습니다.

# 3. 품사 및 저주

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

단어의 문법 기능을 이해하는 데 일부사 방언은 많은 명사로 잠식된 경우가 많아, 이와 같은 일반적인 태그를 추적하는 것이 잠재적으로 공개적으로 될 수 있습니다. "시나리오 평가에 따른 영화 분석"에서 스티븐 팔로우와 조시 콕크로프트는 "욕은 모든 대본에 공평하게 분포되어있지 않습니다. 코미디 영화가 행동과 공포 대본을 약간 능가해 가장 많이 욕인데, 욕수가 가장 적은 장르는 가족, 애니메이션 및 믿음 기반 대본입니다" 라고 말합니다.

## 품사 태깅

먼저 텍스트로 가장 빈도가 높은 태그를 살펴보면, 텍스트를 펼쳐 샘플을 가져와 POS 태깅을 위해 SpaCy를 사용할 것입니다.

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

안녕하세요! 영화 대사를 평가하는 것에 관한 내용입니다. 전체적으로 보면, 명사가 월등히 가장 많은 품사이며, 형용사와 동사는 비슷한 수를 유지하고 있습니다. 부사는 저희 영화에서 가장 드물게 사용되는 품사입니다. 요약하면, 명사가 가장 많이 사용되고, 형용사와 동사는 유사한 빈도로 나타납니다. 부사는 가장 드물게 사용됩니다. 실제 품사 및 그에 대한 설명은 아래와 같습니다.

- NN: 명사, 단수 또는 질량
- JJ: 형용사
- VB: 동사, 기본 형태
- VBP: 동사, 3인칭 단수 현재형이 아님
- RB: 부사

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

이 그래프에서 네 개의 히스토그램을 모두 표시한 이유는 각 부분에 대한 사용량의 명확한 차이를 부각시켜주기 때문입니다. 명사가 언어적 풍경을 지배하며 대화의 40%에서 60%를 차지하고 있으며, 반면 부사는 0%에서 10%까지 범위에 걸쳐 있습니다. 이 우세함은 영화 서술의 구체적이고 명확한 성격을 강조하며, 종종 특정 명사를 통해 대화를 고정시키고 장면을 설정합니다. 반면, 부사는 드물게 나타나고, 이는 영화 대화가 묘사적이거나 자격 부여 구절보다 간단하고 간결한 언어를 선호할 수 있음을 시사합니다.

## 욕설

저희는 'badwords.txt'를 사용하여 저속한 단어를 감지할 것입니다.

![2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_20.png](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_20.png)

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

대부분의 영화 대사는 저속한 언어를 포함하지 않지만, 특정 대본에서는 상당량의 저속한 언어가 존재합니다. 이 중 일부 대본에서는 비율이 0.37에 이르기도 합니다. 이는 장르, 설정 또는 캐릭터 발전 선택을 반영할 수 있으며, 저속한 언어가 현실성과 강도를 더하거나 캐릭터의 성격을 분명히하기 위해 사용될 수 있습니다.

**4. 감정**

우리는 두 가지 감정 분석 모델을 활용할 것입니다: NLTK Vader는 빠르지만 기본 규칙 기반 접근 방식을 사용하며, Flair는 더 정확하지만 연산이 많이 필요합니다.

NLTK Vader는 개별 단어를 기반으로 감정 점수를 할당하며, 강한 부정적인 단어가 있더라도 중립적인 단어로 인해 편향될 수 있어 정확도가 떨어질 수 있습니다. 또한 비꼼이나 맥락적 뉘앙스를 식별하는 데 어려움을 겪을 수 있습니다.

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

## 긍정적, 부정적, 중립적 단어 빈도 시각화

플레어(Flair)는 문맥을 파악할 수 있는 임베딩 기반 모델입니다. 벡터 표현이 유사한 단어는 일반적으로 동일한 문맥에서 사용됩니다. 이 접근 방식을 사용하는 당신의 단점은, 무분별한 규칙 기반 접근 방식에 비해 상당히 느리다는 점입니다. NLTK 모델은 실행하는 데 약 4분가 걸렸지만 이 모델은 약 3시간이 소요되었습니다.

![이미지](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_21.png)

![이미지](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_22.png)

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

# 변수 간 관계

## 상관 관계

우리의 분석에서는 모든 값 사이의 단조 관계를 식별하기 위해 스피어먼 상관 계수를 사용할 것입니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_23.png)

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

## 중요한 상관관계만 표시

아래에는 Spearman 상관관계의 p값이 0.05보다 작은 경우만 중요한 상관관계만 나타냅니다.

![이미지](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_24.png)

평균 단어 수와 평균 문장 수 또는 대문자 단어의 평균 수와 같은 중요한 상관관계를 찾을 것으로 예상되어 있습니다. 또한 다음과 같은 상관관계를 예상했습니다:

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

- 감정 특성은 욕설 비율과 관련이 있습니다.
- 서로 다른 품사 태그 비율 사이의 상관 관계.

흥미로운 몇 가지 관찰 사항이 있었습니다:

- 느낌표 사용과 욕설 사이에 상관 관계가 없습니다.
- 질문 사용과 욕설 사이에는 유의미하나 약한 상관 관계가 있습니다.
- Flair 감정과 질문 사용 사이에는 약한 부정적 상관 관계가 있습니다.

평균 단어 수와 고유 명사(prop_noun) 사용 사이에는 상당한 양의 상관 관계가 있을 수도 있으며, 이는 더 복잡한 대화가 보다 구체적인 엔티티나 이름에 관한 더 많은 언급을 포함할 수 있다는 것을 나타낼 수 있습니다. 이는 과학 소설이나 판타지와 같은 복잡한 세계관을 가진 일부 장르의 특성일 수 있습니다.

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

## 변수에 대한 욕설

이전에 언급한대로, 질문과 욕설 사이에는 상관 관계가 있음에 놀라웠는데 느낌표와 욕설 사이에는 관련성이 없다는 것을 발견했습니다. 따라서, 그 관계를 확인할 수 있는 기울기 그래프를 작성하기로 결정했습니다.

![2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_25](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_25.png)

## 변수에 대한 욕설 요약

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

흥미로운 점은 놀랄 만한 일이지만, 느낌표의 비율과 욕설의 비율 간에 유의미한 상관 관계가 없다는 것이 놀라운 사실입니다. 그럼에도 불구하고, 욕설 비율에서 가장 큰 증가가 나타나는 것은 느낌표가 붙은 대화에서 아닌 대화로 넘어가는 경우입니다.

## 최종 데이터

![Final Data](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_26.png)

# 모델링

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

이전의 내용을 빠르게 살펴보고 모델링 과정과 성능에 대한 간략한 개요를 제공할 예정이에요. 하지만 여기서 진행한 모델링 작업에 대해 더 자세히 알고 싶다면 부담 갖지 마시고 말씀해 주세요. 그러면 제가 두 번째 부분을 준비해 드릴게요 :)

우리는 드라마 장르를 예측하는 분류기를 만들고 있어요.

### LazyPredict

모델링 단계를 가속화하기 위해, 우리는 LazyPredict를 활용했어요. LazyPredict는 AutoML Python 패키지로, 일반적인 머신러닝 알고리즘을 데이터셋에 적용하고, 해당 작업 기반의 일반적인 메트릭을 제공하는 도구에요.

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

## 초모수 튜닝: 베이지안 최적화

이후 첫 4개 모델에 대해 초모수 튜닝을 수행했습니다:

- ExtraTreesClassifier
- AdaBoostClassifier
- Perceptron
- XGBClassifier

고전적으로, 초모수 스윕은 그리드 탐색(무작위)을 통해 실행됩니다. 여기서 초모수의 모든 가능한 조합이 최적화를 위해 경험적으로 평가됩니다. 새로운 초모수마다 시도 횟수가 기하급수적으로 증가하기 때문에 이것은 보통 비현실적입니다. 다른 방법으로는 무작위 탐색이 있습니다. 이 방법은 초모수를 무작위로 결합하여 그리드 탐색보다 더 효율적으로 로컬 최적값에 도달합니다. 모든 조합이 소진되지 않은 경우, 무작위 탐색은 그리드 탐색보다 효과적입니다.

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

베이지안 최적화 대신, 저는 베이지안 최적화 방법을 활용할 것입니다. 이 방법은 가우시안 프로세스를 구축하여 블랙박스 함수와 탐색 공간을 모델링합니다. 이 방법의 가장 큰 장점은 단순히 다양한 하이퍼파라미터를 시도하는 대신 로컬 솔루션으로 수렴함으로써 (다른 머신러닝 모델과 마찬가지로) 접근하는 것입니다.

## Extra Trees Classifier 수동 하이퍼파라미터 튜닝

```js
학습 F1 점수: 0.985
학습 정확도 점수: 0.984

테스트 F1 점수: 0.696
테스트 정확도 점수: 0.675
```

F1 점수는 정밀도와 재현율의 조화 평균으로, 모델 성능의 주요 지표로 작용합니다. 정밀도는 영화를 ‘드라마’ 장르로 올바르게 식별하는 모델의 신뢰성을 반영하며, 재현율은 모델이 모든 관련 드라마 영화 인스턴스를 포착하는 능력을 측정합니다.

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

해당 요인을 고려할 때, 저분산 열을 걸러내는 완전히 발전된 파이프라인의 부재, 잠재적 다중 공선성 대응, 보다 체계적인 특성 엔지니어링 과정을 고려한다면, 해당 모델은 합리적인 효과를 나타냈습니다. 다음 섹션에서는 모델에 가장 중요한 특성을 강조하겠습니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_27.png)

# 향후 작업

본 문서는 주로 시나리오 내에서 특성 생성과 데이터 분석에 초점을 맞췄습니다. 그러나 모델링에 더 많은 시간을 할애하려면, 특성 엔지니어링에 집중하고, 다중 공선성의 영향을 확인하며, 모델 선택에 더 많은 시간을 투자할 것입니다.

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

더 자세히 말하자면, 저는 다음과 같은 방식을 사용할 겁니다:

- 문서 특성 중 가장 중요한 단어들을 사용하여 tfidf 벡터화기법을 이용해 첫 번째 모델을 적합시키고, 여기 제시된 특성을 활용하여 두 번째 모델을 적합시킬 겁니다. 그 후, 두 모델을 결합하여 두 모델을 통합하는 것이 성능 향상에 도움이 될지 확인할 겁니다.
- TFIDF 및 Latent Dirichlet allocation을 사용하여 주제 모델링을 수행할 겁니다. 특히 다중 레이블 분류기에 대해서 주제를 추출하는 것이 성능을 향상시킬 수 있습니다.
- 영화 등급, 영화 년도, 캐릭터 당 대화 평균량과 같은 장르에 대한 예측력이 있는 다른 특성들을 추가할 겁니다.
- 시간 경과에 따른 평균 감정을 분석하고, 이를 "플롯 아크" 범주로 분류할 겁니다. Fellow에 따르면, 6가지 일반적인 이야기 플롯 아크가 있습니다: Riches to Rags (계속된 감정적 하락), Rags to Riches (계속된 감정적 상승), Oedipus (하락-상승-하락), Cinderella (상승-하락-상승), Man in a Hole (하락-상승) 그리고 Icarus (상승-하락).
- 영화의 평균 감정을 가져다 쓰고, 영화를 플롯 아크 중 하나로 분류하는 함수를 작성해 보고, 그것 역시 장르를 예측할 수 있는지 확인할 수 있을 겁니다.

![image](/assets/img/2024-07-13-EvaluatingCinematicDialogueWhichSyntacticandSemanticFeaturesArePredictiveofGenre_28.png)

# 결론적인 의견

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

이 분석을 즐겁게 보셨길 바라며, 이 기사가 분야의 독특한 특성에 맞게 분석을 맞춤화하는 잠재력을 보여준 것을 바랍니다. 저는 영화 대화에 초점을 맞추었지만, 도메인 중심의 데이터 분석과 모델링 원칙은 보편적입니다. 여러분께서 선택한 분야를 연구하고, 호기심을 갖는 것을 장려하며, 다음 모델링 작업 중에 특성 엔지니어링에 창의적으로 접근해 보시기를 권유합니다. 또한, 여러분의 독특한 도메인 중심 분석에 대한 경험도 듣고 싶습니다. 언제든지 댓글로 남기시거나 christabellepabalan@gmail.com으로 이메일 주시기 바랍니다. 감사합니다!

## 참고 자료

- Chang, J. P., Chiam, C., Fu, L., Wang, A., Zhang, J., & Danescu-Niculescu-Mizil, C. (2020). ConvoKit: 대화 분석을 위한 툴킷. SIGDIAL 학회 발표문.
- Cornell Movie-Dialogs Corpus. https://www.kaggle.com/rajathmc/cornell-moviedialog-corpus/kernels에서 검색 (원천: ConvoKit).
- Follows, S. (2019). 커버리지에 따른 시나리오 평가. https://stephenfollows.com/wp-content/uploads/2019/01/JudgingScreenplaysByTheirCoverage_StephenFollows_c.pdf에서 확인.
- Minitab. (2023). Pearson 및 Spearman 상관 관계 방법 비교. https://support.minitab.com/en-us/minitab/21/help-and-how-to/statistics/basic-statistics/supporting-topics/correlation-and-covariance/a-comparison-of-the-pearson-and-spearman-correlation-methods/:에서 확인하세요.
