---
title: "화학 개체 인식 자동화 ChemNER 모델을 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_0.png"
date: 2024-07-13 03:49
ogImage: 
  url: /assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_0.png
tag: Tech
originalTitle: "Automating Chemical Entity Recognition: Creating Your ChemNER Model"
link: "https://medium.com/towards-data-science/text-mining-for-chemists-a-diy-guide-to-chemical-compound-labeling-ea3145e24dc4"
---


<img src="/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_0.png" />

화학에 대한 흥미가 항상 강했어요. 그 흥미는 제 학업과 직업적인 여정을 형성하는 데 중요한 역할을 해 왔어요. 화학 배경을 가진 데이터 전문가로서, 제 과학적 그리고 연구 기술을 창의성, 호기심, 인내, 예리한 관찰, 그리고 데이터 프로젝트에 대한 분석 등과 결합하여 적용하는 여러 방법을 찾았어요. 이 기사에서는 제가 개발한 간단한 Named Entity Recognition (NER) 모델인 ChemNER의 개발 과정을 안내할 거에요. 이 모델은 텍스트 내의 화학 화합물을 식별하고 알칸, 알켄, 알킬인, 알코올, 알데하이드, 케톤, 또는 카르복실산과 같은 범주로 분류할 수 있어요.

## 간단 요약: 

ChemNER 모델로 놀고 싶거나 제가 만든 Streamlit 앱을 사용하길 원한다면 아래 링크를 통해 액세스할 수 있어요:

<div class="content-ad"></div>

**안녕하세요!**

이번에 공유드릴 내용은 매우 흥미로운 것이에요! 

오늘 함께 살펴볼 내용은 NER(Named Entity Recognition) 접근 방식에 대한 것이에요. 일반적으로 이 NER 접근 방식은 다음 3가지 범주 중 하나로 분류될 수 있답니다:

<div class="content-ad"></div>

- 렉시콘 기반: 클래스 및 용어 사전 정의
- 규칙 기반: 각 클래스에 해당하는 용어에 대한 규칙 정의
- 기계 학습 (ML) 기반: 모델이 교육 말뭉치에서 명명 규칙을 학습하게 함

이러한 접근 방식 각각에는 그들만의 장단점이 있습니다. 항상 복잡하고 정교한 모델이 항상 최상의 접근 방식은 아닙니다.

이 경우, 렉시콘 기반 접근 방식은 범위에서 제한적일 수 있습니다. 왜냐하면 우리가 분류하고 싶어하는 각 화합물 클래스에 대해 해당 범주에 속하는 모든 화합물을 수동으로 정의해야 합니다. 다시 말해, 이 방법이 모든 것을 아우르게 만들려면 각 화학 화합물 클래스에 대해 수동으로 모든 화합물을 입력해야 합니다.

ML 접근 방식이 가장 강력할 수 있지만, 데이터 집합에 주석을 다는 것은 상당히 근사일 수 있습니다. (스포일러: 나는 모델을 교육할 것이지만 교육 목적으로 전체 과정을 보여주고 싶어합니다.) 대신, 미리 정의된 명명 규칙부터 시작해보는 것은 어떨까요?

<div class="content-ad"></div>

화학 명명법은 확립되고 명확히 정의된 규칙 세트를 갖고 있습니다. 이를 통해 우리는 분자 내에 어떤 기능성 기준이 있는지 쉽게 판별할 수 있습니다. 이러한 규칙들은 국제 순수 및 응용 화학 연합(IUPAC)에 의해 확립되었으며, IUPAC 블루 북, 다양한 웹사이트 또는 유기 화학 교과서에서 쉽게 찾을 수 있습니다. 예를 들어, 탄화수소는 탄소 및 수소 원자로만 구성된 화합물입니다. 알켄, 알케인 및 알카인이라는 세 가지 주요 탄화수소 클래스가 있으며, 이들은 각각 단일, 이중 또는 삼중 결합을 화학 구조의 일부로 갖고 있는지에 따라 쉽게 식별할 수 있습니다. 아래에는 세 가지 화학 화합물 예시(에탄, 에틸렌, 에틸인)를 보여드리겠습니다.

![Chemical Compounds](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_1.png)

여기서 중요한 것은 이름 끝맺음(즉, 사본)입니다. 이것이 화학 화합물을 차별화할 수 있도록 해줍니다. 예를 들어, 알켄은 -ane로 끝나는 접미사로 식별되며, 알케인은 -ene, 알카인은 -yne로 끝납니다. 알콜, 케톤, 알데하이드, 카복실산 등과 같은 모든 화합물 클래스는 이와 같은 유일한 명명 체계를 갖고 있으며, 이것이 이 프로젝트의 기초가 될 것입니다.

# 규칙 수립

<div class="content-ad"></div>

이제 배경을 이해했으니, rule-based 접근 방식이 어떻게 Spacy를 사용하여 Python에서 구현될 수 있는지 보여드릴게요. 우선 가장 간단한 수소화탄소만 다루도록 시작할 거에요. 다른 클래스들은 나중에 추가할 거에요. 먼저, Spacy로 빈 영어 모델을 로드하고 'Entity Ruler' component를 파이프라인에 추가할 거에요:

```python
# Load a blank English model
nlp = spacy.blank("en")

# Create the EntityRuler
ruler = nlp.add_pipe("entity_ruler")
```

다음으로, 각 클래스를 정의하는 규칙/패턴을 설정하고 rules component에 추가할 거에요:

```python
# Define patterns
patterns = [
    {"label": "ALKANE", "pattern": [{"TEXT": {"REGEX": ".*ane$"}}]},
    {"label": "ALKENE", "pattern": [{"TEXT": {"REGEX": ".*ene$"}}]},
    {"label": "ALKYNE", "pattern": [{"TEXT": {"REGEX": ".*yne$"}}]}
]

ruler.add_patterns(patterns)
```

<div class="content-ad"></div>

여기서 마치겠습니다! 이제 우리 모델에 공급할 텍스트를 만들어 보겠습니다!

```js
text = "에테인, 프로필렌, 부티인은 모두 탄화수소의 예시입니다."

doc = nlp(text)

# 엔티티 추출
for ent in doc.ents:
    print (ent.text, ent.start_char, ent.end_char, ent.label_)
```

이의 출력은 다음과 같습니다:

```js
에테인 0 6 ALKANE
프로필렌 9 16 ALKENE
부티인 22 28 ALKYNE
```

<div class="content-ad"></div>

이미 잘 하고 계시네요! 그러나 이 초기 접근 방식에는 아마도 깨달았을 두 가지 즉각적인 한계가 있습니다.

- 현재의 정규 표현식을 사용하면 복합 명사의 복수형이 감지되지 않을 것입니다.
- 접미사만을 기반으로 분류하는 것은 잘못된 레이블이 많이 붙을 것입니다.

화학 화합물은 전형적으로 가산성 명사로 간주됩니다(공기나 음악과 같은 단어를 생각해보세요). 그러나 복수형 버전이 사용될 수있는 경우도 있습니다. 예를 들어, 에탄 분자의 모음을 다루고 있다면, 누군가 그것을 에테인 집단이라고 언급할 수 있습니다. 따라서 첫 번째 점은 우리의 정규 표현식을 아래 형식으로 수정하여 쉽게 해결할 수 있습니다:

```js
# 패턴 정의
패턴 = [
    {"label": "알케인", "pattern": [{"TEXT": {"REGEX": ".*anes?$"}]},
    {"label": "알케인", "pattern": [{"TEXT": {"REGEX": ".*enes?$"}]},
    {"label": "알케인", "pattern": [{"TEXT": {"REGEX": ".*ynes?$"}]},
]
```

<div class="content-ad"></div>

이제 개체 지배자에게는 단수 및 복수 인스턴스가 모두 인식됩니다. 그러나 두 번째 문제는 여전히 존재합니다. 예를 들어, "신비한", "인간애로운", "관료", "찰나의", "길"과 같은 단어들이 텍스트에 포함되어 있다면 이들은 모두 부정확하게 "알케인"으로 라벨링될 것입니다.

이 접근 방식을 강화하기 위해 구현할 수 있는 다른 규칙들이 있기는 하지만, 그에는 상당한 추가 작업이 필요할 것입니다. 이 한계를 해결하기 위해 고려 중인 세 가지 접근 방식은 다음과 같습니다:

- 이 응용 프로그램에 대한 ML 기반 개체명 인지(Named Entity Recognition, NER) 모델을 학습시키기 위한 말뭉치(corpus) 구축
- 모델 출력에서 발생하는 라벨링 오류를 교정하는 데 도움이 되는 Named Entity Linking (NEL) 사용
- SciBERT 또는 PubMedBERT와 같은 트랜스포머(transformer) 모델을 사용자 지정 데이터셋에 대해 세밀하게 조정(fine-tune)하기

본 문서에서는 앞선 두 가지 접근 방식에 대해서만 다룰 것입니다. 그러나 관심이 있다면, 향후 글에서 세밀하게 조정 과정이 어떻게 달성될 수 있는지 보여드리겠습니다.

<div class="content-ad"></div>

# 데이터셋 생성하기

코퍼스를 만드는 다양한 방법이 있습니다. 챗GPT를 활용하여 특정 클래스와 관련된 화합물을 포함한 문장 세트를 생성하면 빠르고 쉽게 코퍼스를 만들 수 있습니다. 이 방법을 사용하면 데이터셋을 직접 만들고 맞춤화할 수 있어 후속 주석 작업이 훨씬 쉬워집니다. 나의 요청은 간단합니다:

```js
고유한 알켄을 다루는 문장 50개로 이루어진 세트를 만들어주세요
```

그리고 다른 관심 있는 클래스(알켄, 알키닌, 알콜, 키톤, 알데하이드, 카복실산)에 대해 같은 프롬프트를 반복했습니다. 7개 클래스를 가지고 있으므로 전체 350개의 문장으로 코퍼스를 완성했습니다. 이 이상의 대규모 코퍼스가 이상적이지만, 이것은 개념 증명을 목적으로 하고 있기 때문에 충분한 시작점이라고 생각합니다. 또한 필요에 따라 성능을 향상시키기 위해 필요한 만큼 데이터를 추가하는 것이 항상 더 쉽습니다. 문장들을 chem_text.txt 파일에 저장했습니다.

<div class="content-ad"></div>

마지막 단계로, 문서의 각 문장을 분할하기 위해 문장 토크나이저를 사용하겠습니다.

```js
doc = nlp(chem_text)

corpus = []

for sent in doc.sents:
    corpus.append(sent.text.strip())
```

이제 이 코퍼스가 준비되었으니 라벨링을 시작해야 합니다. 이를 수행하는 몇 가지 방법이 있습니다. 예를 들어, Prodigy와 같은 주석 도구를 사용할 수 있습니다(정말 놀라운 도구이니, NLP를 할 때 사용해보세요) 또는 이전에 사용한 규칙 기반 접근법을 사용하여 초기 주석 작업을 돕는 것도 가능합니다. 일단 저는 대규모 데이터셋을 주석 작업하지 않을 예정이기 때문에 모델 접근 방식을 사용하겠습니다.

<div class="content-ad"></div>


DATA = []

# 한번 더 말뭉치에 대해 반복합니다.
for sentence in corpus:
    doc = nlp(sentence)
    
    # Entity는 목록의 인덱스 1에 있는 딕셔너리 형식이어야 하므로 빈 목록이어야 합니다.
    entities = []
    
    # Entity 추출
    for ent in doc.ents:

        # 올바른 형식으로 Entity에 추가합니다.
        entities.append([ent.start_char, ent.end_char, ent.label_])
        
    DATA.append([sentence, {"entities": entities}])


내가 관심 있는 모든 클래스를 포함하려면 아래의 규칙을 업데이트해야 합니다:


# 패턴 정의
patterns = [
    {"label": "ALKANE", "pattern": [{"TEXT": {"REGEX": ".*anes?$"}}]},
    {"label": "ALKENE", "pattern": [{"TEXT": {"REGEX": ".*enes?$"}}]},
    {"label": "ALKYNE", "pattern": [{"TEXT": {"REGEX": ".*ynes?$"}}]},
    {"label": "ALCOHOL", "pattern": [{"TEXT": {"REGEX": ".*ols?$"}}]},
    {"label": "ALDEHYDE", "pattern": [{"TEXT": {"REGEX": ".*(al|als|aldehyde|aldehydes)$"}}]},
    {"label": "KETONE", "pattern": [{"TEXT": {"REGEX": ".*ones?$"}}]},
    {"label": "C_ACID", "pattern": [{"TEXT": {"REGEX": r"\b\w+ic\b"}}, {"TEXT": {"IN": ["acid", "acids"]}}]}
]


규칙 기반 접근 방식을 실행한 결과, 아래와 같이 데이터셋을 빠르게 주석 처리할 수 있습니다.


<div class="content-ad"></div>

이미지 링크에 Markdown 형식을 사용해 주세요.

우리는 코퍼스를 훈련 및 테스트 세트로 분할할 준비가 거의 끝났어요. 하지만, 앞으로 나아가기 전에 우리의 주석 품질을 확인해야 합니다. 데이터셋을 조사한 결과, 앞서 언급한 잘못된 레이블링 문제를 발견했어요. "essential", "crystals", "potential", "materials"와 같은 단어들이 데이터셋에서 발견되었고 알데하이드로 레이블링 되었습니다. 이는 규칙 기반 접근 방식의 한계를 강조하는 것입니다. 저는 아래 메서드를 사용하여 이러한 레이블을 수동으로 제거하고 코퍼스의 주석을 다시 처리했어요:

```js
# 무시할 단어 목록
ignore_set = {"essential", "crystals", "potential","materials","bioorthogonal","terminal","chemicals",
              "spiral","natural","positional","structural","special","yne","chemical","positional",
              "terminal","hormone","functional","animal","agricultural","typical","floral","pharmaceuticals",
              "medical","central","recreational"}  # 무시 목록을 세트로 변환

DATA = []

# 코퍼스를 반복
for sentence in corpus:
    doc = nlp(sentence)
    
    entities = []

    # 엔티티 추출
    for ent in doc.ents:
        # 엔티티가 무시 목록에 없는지 확인
        if ent.text.lower() not in ignore_set:
            # 올바른 형식의 entities에 추가
            entities.append([ent.start_char, ent.end_char, ent.label_])

    DATA.append([sentence, {"entities": entities}])
```

이제 훈련 및 테스트 세트를 만들 준비가 되었어요. 이 작업은 scikit-learn의 train_test_split 함수를 사용하여 쉽게 할 수 있어요. 저는 표준 80:20 훈련:테스트 분할을 사용했습니다.

<div class="content-ad"></div>


# 데이터 분할하기
train_data, valid_data = train_test_split(DATA, test_size=0.2, random_state=42)


## 모델 훈련하기

훈련 데이터가 준비되었으니 이제 모델을 훈련할 차례입니다. 모델을 훈련하기 위해 기본적인 Spacy NER 훈련 매개변수를 사용했습니다. Adam 옵티마이저와 0.001의 학습률을 사용했습니다. 이 훈련은 Google Colab의 CPU에서 1시간 이상 소요되었는데 GPU를 사용하면 크게 단축될 수 있습니다. 훈련 결과는 아래와 같습니다:

![Training Results](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_4.png)


<div class="content-ad"></div>

위의 그래프들은 이 모델의 F1 점수, 정밀도, 재현율 및 전체 점수가 꾸준히 향상되고 있다는 것을 보여줍니다. 이는 좋은 신호입니다. NER 손실은 NER 구성 요소 전체의 손실에 대응되는데, 이는 최소값을 향해 갈 듯 합니다. 모델의 최종 성능 점수는 0.97로, 약간으로 유망해 보입니다.

그러나 Tok2Vec 손실은 에폭 300 근처에서 뚜렷하게 증가한 것으로 나타났습니다. 이는 학습률이 너무 높거나, 사라지는/폭발하는 그래디언트로 인한 숫자 불안정성, 과적합 문제 등이 원인일 수 있습니다. Tok2Vec 손실은 토큰을 벡터로 변환하는 모델의 토큰-벡터 부분의 효과를 나타냅니다. 우리가 선택한다면 처리할 수 있는 다양한 방법들이 있지만, 지금은 계속 진행하겠습니다.

## 모델 테스트

간단한 테스트를 해보겠습니다. 몇 가지 문장을 입력하여 얼마나 잘 분류하는지 확인해보겠습니다. 아래 결과를 확인할 수 있습니다:

<div class="content-ad"></div>


![Extracted entities](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_5.png)

Great job! It looks like all the relevant entities were successfully extracted and correctly labeled! This is one of the fascinating aspects of the machine learning approach. Instead of manually defining rules, the algorithm learns them during training. While this is impressive, let's now subject the model to a more rigorous test.

## Querying Wikipedia (stress testing)

To further challenge my model, I've decided to conduct a stress test by providing it with an entire Wikipedia article. I plan to create a quick routine to achieve this using the wikipedia-api package in Python:


<div class="content-ad"></div>


위의 코드를 Markdown 형식으로 변경하겠습니다.

```python
import wikipediaapi

# 사용자 에이전트 정의
user_agent = "MyApp/1.0 (your@email)"

# 위키백과 API 및 spaCy 초기화
wiki_wiki = wikipediaapi.Wikipedia(user_agent,'en')

# 위키백과 문서 가져오는 함수
def get_wikipedia_article(page_title):
    page = wiki_wiki.page(page_title)
    return page.text if page.exists() else None

# 텍스트에서 개체명 인식 수행하는 함수
def perform_ner(text):
    doc = nlp(text)
    return [(ent.text, ent.label_) for ent in doc.ents]
```

이제 위키피디아에서 '벤젠(Benzene)'에 대한 글을 찾아보겠습니다:

```python
# 위키피디아에서 글 검색
article_title = "Benzene"  # 원하는 글 제목으로 변경해주세요
article_content = get_wikipedia_article(article_title)
```

이 과정의 결과는 다음과 같습니다:


<div class="content-ad"></div>

오예! 쿼리가 작동되는 것을 확인했으니 이제 ChemNER 모델을 실행해 봅시다. Benzene 기사에서 ChemNER 모델은 총 444개의 엔티티를 추출했어요. 이 엔티티 추출은 1초도 걸리지 않았어요. 결과를 데이터프레임에 넣고 아래의 카운트 플롯에서 라벨 수를 시각화했답니다:

![ChemNER_Count_Plot](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_7.png)

그 기사에서 가장 일반적인 클래스는 alkene이었어요. 이는 Benzene이 해당하는 화합물 클래스라는 점을 감안하면 말이 되는 선택이에요. 조금 놀라운 점은, 이 특정 기사에는 각 클래스에 속하는 엔티티가 있었다는 것이었어요.

<div class="content-ad"></div>

이거 짱이에요! 하지만 추출된 엔티티 데이터프레임의 처음 몇 행을 빠르게 살펴보니 모델에 문제가 있는 것 같아요. '화학'과 '육각형'이라는 단어가 알데하이드로 레이블링되었고 '하나'라는 단어가 케톤으로 레이블링되었어요. 이것들은 분명히 화합물이 아니며 해당 분류로 지정되어서는 안 됩니다. 제가 각 엔티티를 수동으로 올바르게 식별했고 추출 정확도가 70.3%임을 확인했어요. 모델이 학습한 규칙에 따라 '올바르게' 레이블링된 모든 추출된 엔티티든 있었지만, 모델이 아직 단어의 문맥을 제대로 학습하지 못했어요.

![image](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_8.png)

괜찮은 건 발견한 부분은 올바르게 레이블링된 엔티티가 모두 화학 화합물이라는 거에요. 다시 말해, 엔티티가 화학 화합물인지 판단할 수 있는 방법이 있다면 이 애플리케이션의 레이블링 성능을 크게 향상시킬 수 있을 거에요.

이 시점에서 우리가 취할 수 있는 몇 가지 방법이 있어요. 한 가지 방법은 말뭉치로 돌아가 모델이 학습할 예시 데이터를 더 생성하는 거에요. 다른 한 가지 방법은 명명된 엔티티 연결(NEL)을 사용하여 레이블을 보정하는 거예요. 좀 덜 시간이 소요되는 후자의 옵션을 선택할게요.

<div class="content-ad"></div>

# PubChem을 활용한 NEL

ChemNER 모델은 화학 클래스에 따라 엔터티를 라벨링하는 데 매우 탁월한 성과를 내고 있습니다. 이 모델을 더 잘 지원하기 위해 PubChem에 있는 API를 활용하여 화합물에 대한 쿼리를 수행할 것입니다. 여기서의 아이디어는 화합물에 대한 쿼리는 정보를 반환하고, 화합물이 아닌 것에 대한 쿼리는 빈 쿼리를 반환할 것이라는 것입니다. 이 쿼리 결과를 사용하여 애플리케이션의 라벨링 성능을 개선할 수 있습니다.

예시로 Benzene에 대한 쿼리를 시작해 보겠습니다. 아래 코드를 사용하여 PubChem API에 쿼리할 수 있습니다.

```python
def get_compound_info(compound_name):
    base_url = "https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name"
    response = requests.get(f"{base_url}/{compound_name}/JSON")
    if response.status_code == 200:
        return response.json()
    else:
        return None
compound_name = "benzene"
compound_info = get_compound_info(compound_name)
```

<div class="content-ad"></div>

이 쿼리의 결과가 아래에 표시됩니다.

![Automating Chemical Entity Recognition](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_9.png)

이 쿼리를 통해 벤젠에 대한 많은 정보를 얻었어요. 나중에 사용할 수 있겠죠. 하지만 지금은 쿼리가 무언가 반환했다는 사실만 중요해요. 반면, ‘사람’이나 ‘기린’과 같이 화합물이 아닌 것을 조회할 때는 쿼리 결과가 ‘없음’이에요.

![Automating Chemical Entity Recognition](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_10.png)

<div class="content-ad"></div>

저는 이것을 제가 신청서를 돕는 데에 활용할 수 있을 것 같아요. 질의는 상당히 빠르지만, 프로세스를 좀 더 가속화하기 위해 중복 엔티티를 제거하여 오직 고유 용어만을 쿼리하도록 할 거예요. 게다가, PubChem API는 우리가 개별 화합물을 쿼리하는 것으로 가정하는 것 같아요. 따라서 cinammaldehydes와 같은 단어는 빈 쿼리를 반환할 거예요. 이건 복수 용어에서 끝에 붙은 's'를 제거함으로써 쉽게 해결할 수 있어요. 제 데이터프레임에 'Chemical Compound'라는 새 열을 만들기 위해 다음 코드를 사용했어요. 이 열을 통해 각 엔티티가 쿼리 결과에 따라 화합물인지 여부를 분류할 수 있을 거예요.

![image](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_11.png)

그거 정말 잘 작동했어요! 하지만 이 과정을 진행하면서 하나 더 알게 된 게 있어요. 바로 클래스 레이블 자체가 쿼리 결과가 null인 것 같아요. 다시 말해, 만약 PubChem에 alkane, alkene, alkyne 등을 쿼리하면 이것들은 구체적인 화합물이 아니라 화합물의 클래스임에도 빈 쿼리를 받게 되요. 어떻게 진행할지에 대한 약간의 섬세함이 있어요. 저는 이러한 화합물 클래스가 화학 엔티티로 인식되기를 원했기 때문에 클래스 레이블이 구체적인 화합물 없이도 문장에서 독립적으로 사용될 수 있기 때문이에요. 이 문제를 해결하기 위해, 단순히 Entity 열의 항목이 클래스 레이블의 단수 또는 복수 변형인지를 확인하고, 이러한 레이블과 일치하는 경우 'Chemical Compound' 열의 값을 1로 설정하고, 그렇지 않은 경우에는 0으로 설정하는 간단한 루틴을 추가했어요.

아래는 삽입한 코드 예요.

```js
# 구체적 화합물 유형 목록
chemical_compounds = ['alkane', 'alkene', 'alkyne', 'ketone', 'aldehyde', 'alcohol', 'carboxylic acid']

# 'Chemical Compound' 열을 업데이트하는 함수
def update_chemical_compound(row):
    entity = row['Entity'].lower()
    if any(compound in entity for compound in chemical_compounds + [c + 's' for c in chemical_compounds]):
        return 1
    return row['Correct']

# 각 행에 함수 적용
df_unique['Chemical Compound'] = df_unique.apply(update_chemical_compound, axis=1)
```

<div class="content-ad"></div>

멋지네요! 이제 444개의 결과가 담긴 원본 데이터프레임에 이 결과를 합칠 수 있어요.

```python
df_merged = pd.merge(df_ents2, df_unique[['Entity', 'Chemical Compound']], on='Entity', how='left')
```

![Automating Chemical Entity Recognition](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_12.png)

다음으로, 화합물에 해당하지 않는 행은 제거할 거에요.

<div class="content-ad"></div>


# 'Chemical Compound' 값이 0인 행을 제거합니다.
df_filtered = df_merged[df_merged['Chemical Compound'] != 0]

![Markdown](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_13.png)

이제 성능을 확인해봅시다!

![Markdown](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_14.png)


<div class="content-ad"></div>

정말 멋져요! 모든 추출된 엔티티들이 이제 올바르게 레이블이 붙었어요. NER 모델과 PubChem을 통한 NEL을 결합하여 텍스트에서 엔티티를 추출할 뿐만 아니라 결과를 중의성 해소하고 그것을 사용하여 레이블링 정확도를 크게 향상시킬 수 있게 되었어요.

# HuggingFace에 모델 배포하기

좋은 소식이에요. 이번 보너스로, 제가 보여드린 이러한 루틴들을 모두 결합하여 모델을 HuggingFace에 배포해보기로 했어요. 그래서 이를 streamlit 애플리케이션에서 보여줄 수 있게 됐어요. 이제 HuggingFace 모델을 https://huggingface.co/victormurcia/en_chemner 에서 찾을 수 있어요. 아래는 추론 API 결과인데 꽤 좋아 보이네요:

![Inference API Results](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_15.png)

<div class="content-ad"></div>

이용하거나 제안사항이 있으면 언제든지 알려주세요! 미래에 모델을 확장할 계획이 있으며 탐색하고 싶은 다른 기능들이 있습니다.

# Streamlit 앱을 통해 모든 것을 연결하기

모델을 배포했으니 이제 Streamlit 앱에서 이를 사용할 수 있습니다. 이 앱을 통해 사용자는 위키피디아 문서에 링크하거나 가공되어야 하는 원시 텍스트를 입력할 수 있습니다. 이후 ChemNER 모델에 의해 처리될 것입니다. 이 루틴의 출력물로는 추출된 및 레이블이 지정된 엔티티로 이루어진 다운로드 가능한 데이터프레임, 제공된 텍스트에서 각 레이블의 수를 보여주는 카운트 플롯, 그리고 텍스트의 전체적으로 주석이 달린 버전이 제공됩니다. Streamlit 앱은 여기에서 확인하실 수 있습니다: [chemner-5i7mrvyelw79tzasxwy96x.streamlit.app](https://chemner-5i7mrvyelw79tzasxwy96x.streamlit.app/)

<div class="content-ad"></div>

아래처럼 벤젠에 대한 위키피디아 문서를 검색해보겠습니다. 결과는 다음과 같이 각 클래스가 고유한 색으로 표시된 주석이 붙은 버전의 문서입니다.

![벤젠](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_17.png)

또한 엔티티와 그에 해당하는 레이블이 포함된 데이터프레임 및 카운트 플롯을 다운로드할 수 있는 .csv 파일이 포함된 결과도 제공됩니다.

![데이터프레임](/assets/img/2024-07-13-AutomatingChemicalEntityRecognitionCreatingYourChemNERModel_18.png)

<div class="content-ad"></div>

# 결론

이 글이 유익하고 여러분이 자신만의 NLP 애플리케이션을 개발하는 데 도움이 되었으면 좋겠습니다. 나는 모델과 응용 프로그램에 대해 좀 더 작업할 계획이며, 더 탐구하고 싶은 멋진 부분이 있다고 생각합니다. 예를 들어, 테스트를 한 결과, 여전히 모델이 추출한 특정 엔티티들 중에서 PubChem 방법이 유기 화합물이 아닌 화합물로 분류한 것을 발견했습니다. 예를 들어, 'pm'이라는 단어가 엔티티로 추출되고 알데히드로 레이블링되었습니다. PubChem 검색 결과 'pm'(또는 Pm이 더 적합하게)은 Promethium 원소의 화학 기호입니다. 모델은 완벽하지 않지만, 복잡한 언어 모델을 필요로하지 않고도 꽤 강력한 도구를 얻을 수 있다는 점을 보여줄 수 있기를 바랍니다.

언제나 읽어주셔서 감사합니다!