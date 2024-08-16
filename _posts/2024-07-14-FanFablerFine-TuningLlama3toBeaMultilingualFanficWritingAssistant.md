---
title: "FanFabler Llama 3를 이용한 다국어 팬픽션 작성 어시스턴트 최적화 방법"
description: ""
coverImage: "/assets/img/2024-07-14-FanFablerFine-TuningLlama3toBeaMultilingualFanficWritingAssistant_0.png"
date: 2024-07-14 01:32
ogImage: 
  url: /assets/img/2024-07-14-FanFablerFine-TuningLlama3toBeaMultilingualFanficWritingAssistant_0.png
tag: Tech
originalTitle: "FanFabler: Fine-Tuning Llama 3 to Be a Multilingual Fanfic Writing Assistant"
link: "https://medium.com/towards-data-science/fanfabler-fine-tuning-llama-3-to-be-a-multilingual-fanfic-writing-assistant-dfc664ed4a72"
isUpdated: true
---





대규모 언어 모델(LLMs)의 등장으로 텍스트 기반 AI 시스템의 새 시대가 열렸습니다. 이러한 모델은 매우 우수하고 높은 능력을 갖추고 있지만, 그들의 훈련은 주로 영어에 집중되어 있습니다. 가장 큰 상용 LLMs는 "저자원" 언어를 사용하여 텍스트를 잘 생성하지만, 작은 오픈소스 모델들은 비유럽 언어에 대해 잘 처리하지 못합니다.

그러나 Meta는 새로운 Llama 3 모델을 더 다양한 언어로 훈련시켰습니다. 이는 그들이 출시할 때 공지된 글[1]에서 밝혔습니다.

5%는 별거 아닌 것처럼 들릴 수 있지만, Llama [2]의 이전 버전 및 Mistral [3]과 같은 다른 작은 LLMs보다 높습니다. 이를 증명하기 위해 나는 한국어로 동일한 프롬프트를 Llama 2 7B, Mistral 7B 및 Llama 3에 전송했습니다. 괄호 안의 번역은 편의를 위해 표시되었습니다.

여기 Llama 2 7B가 응답한 내용입니다.

<div class="content-ad"></div>

정말 죄송합니다. 이런 실수를 해서 죄송합니다. 한국어에 대해 더 공부하고 노력하겠습니다. 감사합니다.

<div class="content-ad"></div>

팬 패블러(FanFabler)를 소개합니다. 이는 Llama 3 모델의 사용자 정의 버전으로, 팬픽션 쿼리를 다국어로 다루기 위해 세밀하게 조정되었습니다. 팬 패블러는 40개 언어로 내용을 이해하고 생성하는 데 특히 훈련되어 있어 정확한 답변을 제공합니다. 이 모델을 통해 언어 이해력을 향상시키고 전 세계적으로 다양하며 풍부한 이야기를 만들 수 있는 능력을 향상시키고자 했습니다.

팬 패블러가 프롬프트에 대한 응답을 한국어로 자동으로 제공한 사실에 주목해보세요. 세밀한 조정을 통해 항상 질문하는 언어와 동일한 언어로 응답하도록 학습했습니다. 이 응답은 미스트랄(Mistral), Llama 2 및 원본 Llama 3의 답변보다 더 뛰어납니다.

<div class="content-ad"></div>

# 개요

이 글에서는 40개 언어로 팬 픽션을 작성할 수 있는 라마 3 모델을 세밀하게 조정하는 과정을 안내하겠습니다. 먼저 팬 픽션에 대한 간단한 개요부터 시작하여, 이 목적으로 라마 3 모델을 어떻게 맞춤 설정했는지의 기술적 세부사항에 대해 알아보겠습니다.

저는 다국어 팬 픽션 쿼리와 응답을 생성하여 포괄적인 데이터셋을 구축한 데이터 수집 과정을 다룰 것입니다. 그 다음, 데이터의 전처리 및 훈련 환경 설정과 같은 모델 세밀 조정의 기술적 단계를 설명하겠습니다.

또한 채팅 상호작용 중에 위키백과에서 관련 배경 정보를 자동으로 검색하여 시스템을 구성하는 방법과 같이, 모델이 다양한 언어로 정확하고 맥락이 풍부한 응답을 생성할 수 있는 능력을 향상시키는 방법에 대해서도 설명할 것입니다.

<div class="content-ad"></div>

마침내, 조정된 모델의 성능에 대한 통찰과 시스템 상호작용 결과를 보여드리겠습니다.

# 팬 픽션

팬 픽션은 공식 인가 없이 기존의 픽션 작품을 바탕으로 팬들이 이야기를 창작하는 아마추어 문학 장르입니다. 이러한 이야기들은 원작에서 캐릭터, 배경 등을 활용하여 짧은 단편에서부터 장편 소설에 이르기까지 다양할 수 있습니다. 팬 픽션은 책, 영화, 만화, 게임 등 다양한 주제와 매체에 걸쳐 다양하게 전개됩니다. 이는 팬 노동의 한 형태로 간주됩니다.

팬 픽션 작성은 20세기에 저작권 법이 허가된 사용과 무단 사용을 구별하도록 발전함에 따라 유명해졌습니다. 비공식적인 지위임에도 불구하고 팬 픽션의 수용은 매우 다양하며, 일부 원작 창작자는 환영하고 다른 사람들은 법적 수단을 통해 반대합니다. 이 용어의 사용은 저작권, 공정 이용, 창작 표현의 경계에 대한 지속적인 논의를 유발합니다.

<div class="content-ad"></div>

앨리스 팬픽션의 초기 사례로는 1917년 존 레이에 의해 쓰여진 "앨리스의 새로운 모험"이 있습니다. 이 작품은 1865년 루이스 캐럴의 "이상한 나라의 앨리스"를 바탕으로 하였죠. 새로운 이야기는 앨리스에 관한 책을 갈망하는 베티라는 소녀로 시작합니다. 꿈 속으로 빠져든 베티는 다락방에서 앨리스가 책으로 마더 구스 동화를 고양이 새끼들에게 읽는 모습을 발견하면서 새로운 모험을 시작하게 됩니다.

오늘날 Archive of Our Own (AO3)나 FanFiction.net과 같은 많은 팬픽션 웹사이트들은 작가들과 독자들이 다양한 팬 크리에이트 콘텐츠를 공유하고 발견할 수 있는 플랫폼을 제공합니다. 이들 사이트는 여러 판옴과 장르에 걸쳐 다채로운 이야기들로 가득한 도서관을 제공하며 팬들이 익숙한 캐릭터와 배경을 새롭고 창의적인 방식으로 체험할 수 있게 합니다.

주목할만한 팬픽션 시리즈인 "Master of the Universe"는 E. L. 제임스에 의해 창작되었으며 원래 트와일라잇 우주를 배경으로 하였습니다. 그 후에 큰 인기를 끌었던 "50가지 그림자" 시리즈로 진화하였죠. 이처럼 팬에 의해 창작된 작품이 대중적 성공을 이루는 것을 보여주는 팬픽션의 가능성을 잘 보여줍니다.

# 팬패블러

<div class="content-ad"></div>

아래는 팬 픽션을 다국어로 창작하는 데 도움을 주기 위해 Llama 3을 세밀하게 조정하는 데 사용한 과정과 구성요소를 보여주는 다이어그램입니다.

GPT-3.5 Turbo를 활용하여 사용자 지정 프롬프트를 기반으로 40개 언어로 팬 픽션 작성에 대한 모의 대화를 생성했습니다. 총 4,000개의 채팅 상호작용을 수집하고 결과를 데이터셋으로 저장했습니다. 그런 다음 이 데이터를 사용하여 Llama 3을 80억 개의 매개변수로 세밀하게 조정하여 FanaFabler 모델을 만들었습니다.

세밀하게 조정된 모델로 사용자는 어떤 언어로든 팬 픽션에 관한 도움을 요청할 수 있으며, 시스템은 유용한 답변을 생성하고 채팅 세션에 참여할 것입니다. 필요한 경우, 시스템은 창작 과정에 도움을 주기 위해 주요 소스에 대한 관련 정보를 위키피디아에서 확인할 것입니다.

# 언어 선택

<div class="content-ad"></div>

상기 블러브에서 언급했듯이 메타는 라마 3 모델을 주로 영어로 훈련시켰다고 합니다. 그러나 그들은 훈련 텍스트의 5% 이상이 “30개 이상의 언어를 다루었다”고 밝혔습니다. 그들은 게시물에서 사용한 언어를 구체적으로 명시하지는 않았습니다. 이 프로젝트에서 언어를 선택하기 위해 저는 위키피디아 언어 목록을 살펴보고 사용량을 기준으로 상위 40개 언어를 선택했어요.

차트를 통해 시각적으로 확인할 수 있는데, 왼쪽에서 영어의 680만 개 에서 독일어의 290만 개로 급격히 감소하며, 힌디어는 16만 1000개 글로우 지속됩니다. 중복성으로 인해 "간략한 영어" 위키피디아는 목록에서 제외하였고, 제가 40개 언어 세트를 완성하기 위해 기준 없이 8만 1000개의 기사만 있는 스와힐리 언어를 임의로 추가했습니다.

# 훈련 데이터 생성

멀티링귤 팬픽션 데이터세트를 만들기 위해 GPT-3.5 Turbo를 사용하여 텍스트를 생성했습니다. 이를 위해 챗봇과 사용자 질의 및 응답에 대한 모의 대화 상호작용을 생성했습니다. 다음은 사용한 단계입니다:

<div class="content-ad"></div>

오호! 당신이 수집한 데이터 양은 정말 놀라울 만큼 많아 보이네요! 40개 국가의 20가지 팬픽션 속성, 총 800개 조합이 있군요. 그리고 각 속성 당 5가지 채팅 상호작용까지! 대단하시네요.

파이썬 코드도 함께 활용하셨다니 멋진 접근이네요! 데이터 수집하는 방법들을 자세하게 설명해주셔서 감사합니다. 궁금하신 내용이 있거나 도움이 필요하시다면 또 언제든지 물어보세요!

<div class="content-ad"></div>

먼저 나의 훈련 데이터를 생성하는 첫 번째 단계는 팬픽션을 만들기 위한 기반으로 사용될 속성 목록을 확보하는 것이었습니다. 이것들은 책, 영화, TV 프로그램 등이 될 수 있습니다. 여기 코드를 사용하여 이 데이터를 생성했어요. 

```python
from openai import OpenAI
from google.colab import userdata
import json

api_key = userdata.get('OPEN_AI_KEY')
client = OpenAI(api_key=api_key)

def get_properties(language, script, wp_code):
  prompt = f"""Here is info on a language:
Language: {language}
Script: {script}
Wikipedia Code: {wp_code}

Create a JSON list called "properties" of 20 properties that are popular with 
{language} speakers who might want to write fan fiction.
The property type could could be books, tv shows, movies, video games, etc.
Show a mix of locally produced international properties for a total of 20.
List the common name for the property in the {language} language using the key 
"local_name", even for international properties.
Add the English name to the list as "english_name".
Add the property type with the value in lowercase English as "property_type".
Add a Boolean "is_local" value to indicate if the property is locally produced 
or not."""
  response = client.chat.completions.create(
    model="gpt-3.5-turbo-1106",
    messages=[
      {
        "role": "user",
        "content": prompt
      }
    ],
    temperature=1
  )
  return response.choices[0].message.content
```

이 함수는 언어, 스크립트 유형, 그리고 위키피디아 언어 코드를 입력으로 받습니다. 예를 들어, "스웨덴어," "latn," 그리고 "sv" 같은 값을 사용합니다. 그런 다음 GPT 3.5 Turbo에 JSON 레코드를 생성할 것을 요청하여, 팬픽션을 위한 20개 속성 목록을 만들어준답니다. 각 레코드에는 현지 이름, 영어 이름, 속성 유형, 그리고 해당 언어에서의 현지 여부가 포함됩니다. 여기 스웨덴어용 20개 결과 중 세 개를 확인해보세요.

```json
{
  "properties": [
    {
      "local_name": "Pippi Långstrump",
      "english_name": "Pippi Longstocking",
      "property_type": "books",
      "is_local": true
    },
    {
      "local_name": "Sagan om Ringen ",
      "english_name": "The Lord of the Rings",
      "property_type": "book",
      "is_local": false
    },
    
    . . .

    {
      "local_name": "Mamma Mia!",
      "english_name": "Mamma Mia! ",
      "property_type": "musical",
      "is_local": false
    }
  ]
}
```

<div class="content-ad"></div>

이 세 장의 샘플에는 “Pippi Longstocking”이라는 지역 속성과 “반지의 제왕” 및 “맘마미아!”와 같은 두 개의 전역 속성이 포함되어 있습니다. 아마도 후자는 스웨덴에서 시작되었거나 적어도 음악은 거기서 시작되었습니다.

LLM과 같은 GPT-3와 작업할 때는 결과를 JSON 형식으로 반환하도록 명시하는 것이 가장 좋습니다. 이렇게 하면 결과를 분석하기가 훨씬 더 쉬워집니다.

## 처음 묻는 질문들

800가지 언어/속성 쌍을 모두 생성한 후, 팬 픽션에 대해 작성할 수 있는 다섯 가지 사용자의 첫 질문을 생성하기 위해 다음 코드를 사용했습니다.

<div class="content-ad"></div>


def get_first_question(language, script, wp_code, local_name, property_type):
  prompt = f"Create a brief query to an AI writing assistant in the {language} language using the {script} script. The language code is {wp_code}. The query is about creating fan fiction for {local_name}, in the category of {property_type}. Keep it to one sentence. Use some variety."
  response = client.chat.completions.create(
    model="gpt-3.5-turbo-1106",
    messages=[
      {
        "role": "user",
        "content": prompt
      }
    ],
    temperature=1
  )
  return response.choices[0].message.content

This function takes in parameters such as language, script, wp_code, local_name, and property_type to construct a prompt for an AI assistant. It calls GPT-3.5 Turbo to get the response and returns the generated text.

## The First Answers

My next step involved using GPT-3.5 Turbo to answer five questions and provide an option to look up background information about the property in the user's native language if needed. Here's the code:


<div class="content-ad"></div>


def answer_first_question(first_question):
  system_prompt = """팬 픽션의 다국어 작성 보조 기업으로 활동 중인 당신입니다. 간단하고 일반적인 조언으로 시작해주세요. 속성 이름과 위키피디아 언어 코드를 세 개의 큰 부호로 감싼 후 입력해주세요:
>>>프로퍼티_이름
>>>위키_코드"""
  response = client.chat.completions.create(
    model="gpt-3.5-turbo-1106",
    messages=[
      {"role": "system", "content": system_prompt},
      {"role": "user", "content": first_question}
      ],
    temperature=1
  )
  return response.choices[0].message.content


이 함수는 초기 질문만을 입력으로 받습니다. 시스템이 언어를 자동으로 감지합니다. 제 프롬프트는 언어 모델이 질문에 대답하고 위키백과에서 일부 배경 정보를 가져오도록 유도합니다. 명령어 형식은 다음과 같습니다:
```js

## 배경 텍스트 검색

저의 코드에서 출력에서 "" 문자를 발견하면, 위키피디아에서 일부 배경 정보를 가져오려고 시도합니다. 이 방법을 사용하여 시스템은 언어 모델이 알지 못하는 속성에 대해 사용자를 더 잘 도울 수 있습니다.

<div class="content-ad"></div>

여기는 위키백과 검색 API를 사용하여 정보를 가져오는 코드입니다.

import requests
import time

def get_snippets(wp_code, subject, limit=5):
    search_url = f"https://{wp_code}.wikipedia.org/w/api.php"
    search_params = {
        "action": "opensearch",
        "search": subject,
        "limit": str(limit),
        "namespace": "0",
        "format": "json"
    }

    search_response = requests.get(search_url, params=search_params)
    search_data = search_response.json()

    titles = search_data[1]
    results = []
    for title in titles:
        extracts_params = {
            "action": "query",
            "format": "json",
            "prop": "extracts",
            "exsentences": 20,  # 500 단어에 대한 예상치
            "titles": title,
            "explaintext": True,
        }

        extracts_response = requests.get(search_url, params=extracts_params)
        extracts_data = extracts_response.json()

        pages = extracts_data['query']['pages']
        for page_id in pages:
            page = pages[page_id]
            snippet = page.get('extract', 'Not available.').replace('\n', ' ')
            results.append({"title": title, "snippet": snippet})

    return results

이 코드는 위키백과 API와 상호 작용하는 함수를 정의합니다. 이 함수는 주어진 언어와 주제에 관련된 위키백과 항목의 짧은 텍스트 설명(스니펫)을 검색합니다. 함수는 위키백과 언어 코드 (wp_code)와 검색 주제 (subject)를 사용합니다. 각각의 제목과 항목의 스니펫이 포함된 다섯 개의 결과 목록을 반환합니다.

여기는 wp_code가 "sv"로 설정되고 subject가 "Pippi Lånstrump"로 설정된 경우의 최상위 결과입니다.

<div class="content-ad"></div>

이해하신 대로, 마법의 시작이 들어있는 카드를 뽑으셨군요. 이 카드는 당신이 새로운 기회를 만나게 될 것을 암시합니다. 이전에 이루지 못했던 목표를 이루고 새로운 시작을 할 때가 올 것입니다. 이번에는 주변의 사람들과 협력하여 당신의 목표를 달성하는 데 도움을 받을 것입니다. 함께 하면 더 빠르고 멋진 결과를 얻을 수 있을 거에요. 기대해도 좋을 것 같네요! 🌟✨

<div class="content-ad"></div>

## 두 번째 질문과 답변

매 항목의 두 번째 질문과 답변을 생성하는 데 동일한 패턴을 따랐습니다. 각 항목에서 첫 번째 질문, 첫 번째 답변 및 배경 텍스트를 보내 두 번째 질문을 만들었습니다. 그럼 데이터세트가 완성되었어요.

## 다국어 팬픽 채팅 4K 데이터세트

전체 팬 픽션 데이터세트를 저장했어요. 훈깅페이스에 열차, 평가 및 테스트 분할을 모두 넣었어요. 항목을 자유롭게 살펴보세요.

<div class="content-ad"></div>

# 라마 3 모델

메타는 LLM에 대해 광범위한 연구를 진행해왔습니다. 원래의 LLaMa 모델은 2023년 2월에 소개되었습니다. 그들은 이어서 Llama 2를 2023년 7월에 발표했는데, 이 모델에서는 채팅 변형이 소개되었습니다. 2024년 4월에는 여러 개선을 거듭한 Llama 3을 출시했습니다. 메타는 최신 릴리스에서 모델의 크기를 확대했습니다. Llama 2는 7B, 13B 및 70B 매개변수를 가진 모델을 보유했고, Llama 3은 8B 및 80B 매개변수를 갖춘 모델을 보유하고 있습니다.

이들의 새로운 모델에 대해 이야기한 내용은 다음과 같습니다.

그들은 모델의 아키텍처를 개선했으며 어휘 크기를 32K에서 128K 토큰으로 늘렸으며 더 효율적인 추론을 위해 GQA를 채택했습니다. 또한 새로운 모델을 훨씬 큰 데이터셋에서 훈련시켰습니다.

<div class="content-ad"></div>

# 세부 조정

Llama 3 8B 모델은 즉시 사용할 때 잘 작동하지만, 세부 조정을 통해 특정 작업에 더 적합하게 만들 수 있습니다.

Llama 3를 세부 조정하는 여러 가지 방법이 있습니다. Meta는 torchtune과 Huggingface를 사용하는 방법을 여기에 설명하고 있습니다. 저는 이 프로젝트에 사용한 작업 가능한 Google Colab을 가진 unsloth라는 훌륭한 패키지를 발견했습니다. 주로 제가 사용한 것은 원래의 세부 조정 Colab의 기본 설정입니다. 변경한 것은 평가 데이터 세트 사용 및 몇 가지 하이퍼파라미터 조정이었는데, 이에 대해 아래에서 논의하겠습니다.

다음은 Llama 3 모델을 불러오는 코드입니다.

<div class="content-ad"></div>

```
저는 메모리 사용량을 낮추기 위해 Llama 3 8B의 4비트 버전을 사용했어요. 최대 시퀀스 길이를 1,024로 설정했기 때문에 메모리 사용을 제한할 수 있었어요.

그 다음에는 이 코드를 사용하여 데이터셋을 불러왔어요.

```python
from datasets import load_dataset

# 훈련 및 평가 데이터셋 불러오기
train_dataset = load_dataset("robgonsalves/Multilingual-FanFic-Chat-4K", split="train")
test_dataset = load_dataset("robgonsalves/Multilingual-FanFic-Chat-4K", split="test")

# 데이터 형식 맞추기
train_dataset = train_dataset.map(formatting_prompts_func, batched=True, remove_columns=train_dataset.column_names)
test_dataset = test_dataset.map(formatting_prompts_func, batched=True, remove_columns=test_dataset.column_names)

# 데이터셋 섞기
train_dataset = train_dataset.shuffle(seed=42)
test_dataset = test_dataset.shuffle(seed=42)
```


<div class="content-ad"></div>

훈련 및 평가 데이터 셋을 로드하고, 텍스트를 서식에 맞게 정리하고, 데이터를 섞었습니다. 이를 통해 학습 모델이 언어 및 속성 시퀀스에서 발생할 수 있는 의도하지 않은 패턴을 학습하는 것을 방지하기 위해 항목의 순서가 무작위로 바뀌었습니다.

시스템은 효율적인 훈련을 위해 Low-Rank Adaptation (LoRA)을 사용합니다 [7]. 모델을 미세 조정하는 동안 모든 매개변수를 업데이트하는 대신, 변경 사항은 별도의 작은 행렬에 누적되어 모델의 매개변수에 적용됩니다. 이는 속도와 효율성을 위해 훈련 가능한 매개변수의 수를 효과적으로 줄입니다.

다음은 LoRA로 훈련에 모델을 설정하는 코드입니다.

```js
model = FastLanguageModel.get_peft_model(
    model,
    r = 64,
    target_modules = ["q_proj", "k_proj", "v_proj", "o_proj",
                      "gate_proj", "up_proj", "down_proj"],
    lora_alpha = 64,
    lora_dropout = 0,
    bias = "none",
    use_gradient_checkpointing = True,
    random_state = 3407,
    use_rslora = False,
    loftq_config = None,
)
```

<div class="content-ad"></div>

실험을 통해 R 값을 16에서 64로 증가시켰으며, 이로 인해 학습 가능한 매개변수 집합의 크기가 증가했습니다. 또한 LoRA 알파 값을 16에서 64로 증가시켜 학습 가능한 집합에 상당한 영향을 미쳤습니다. 이러한 변경 사항은 완성된 모델과의 상호 작용 품질을 향상시켰습니다.

아래는 모델을 세부 조정하는 코드입니다. Huggingface의 Transformer Reinforcement Learning (TRL) 패키지의 일부인 지도 학습 세부 조정 트레이너(SFT Trainer)를 사용합니다.

```js
from trl import SFTTrainer 
from transformers import TrainingArguments

trainer = SFTTrainer(
    model=model,
    tokenizer=tokenizer,
    train_dataset=train_dataset,
    eval_dataset=test_dataset,
    dataset_text_field="text",
    max_seq_length=max_seq_length,
    dataset_num_proc=2,
    packing=False,
    args=TrainingArguments(
        per_device_train_batch_size=2,
        gradient_accumulation_steps=4,
        warmup_steps=5,
        num_train_epochs=1,
        learning_rate=2e-4,
        fp16=not torch.cuda.is_bf16_supported(),
        bf16=torch.cuda.is_bf16_supported(),
        logging_steps=1,
        optim="adamw_8bit",
        weight_decay=0.01,
        lr_scheduler_type="linear",
        seed=3407,
        output_dir="outputs",
        evaluation_strategy="steps",
        eval_steps=100
    ),
)
```

저는 unsloth Colab에서 대부분의 기본값을 사용했습니다. 변경한 내용은 테스트 데이터셋을 평가에 사용했다는 점뿐입니다. 모든 학습 매개변수에 대한 설명은 여기에서 확인할 수 있습니다.

<div class="content-ad"></div>

트레이닝은 Nvidia L2 GPU와 22GB RAM을 사용하여 2시간 20분 동안 진행되었습니다. 아래는 파인튜닝 도중 트레이닝 및 평가 손실에 대한 그래프입니다.

![Training and Evaluation Loss Graph](/assets/img/2024-07-14-FanFablerFine-TuningLlama3toBeaMultilingualFanficWritingAssistant_0.png)

트레이닝 및 평가 손실 그래프는 처음에 급격한 감소를 보이고 이후에는 점진적 감소로 지속적인 학습을 반영하고 있어 성공적인 파인튜닝 과정을 시사합니다. 트레이닝 및 평가 손실의 동시 감소는 오버피팅 없이 모델의 정확도가 향상되었음을 나타내며, 훈련에 사용된 보이지 않는 데이터에 대한 효과적인 범용화를 보여줍니다.

최종 단계는 다음 코드 라인을 사용하여 모델을 Huggingface에 저장하는 것이었습니다.

<div class="content-ad"></div>


model.push_to_hub_merged("robgonsalves/fan-fabler-lora", tokenizer, save_method="lora", token=token)


By setting the save method to "lora," only a small set of trainable parameters will be saved. You can find the complete Google Colab for training [here](link_to_google_colab).

## Utilizing the Model

Once I finished fine-tuning and saved the model to Huggingface, I reloaded it using the following code.


<div class="content-ad"></div>

이 코드를 실행하면 LoRA 가중치를 원래 Llama 3 모델에 로드하고 모델을 사용할 준비를 합니다. 마지막 줄은 추론을 위해 2배 속도를 허용합니다.

다음은 프롬프트를 설정하는 코드입니다.


system_prompt = """대화와 문맥을 기반으로 사용자의 쿼리에 응답합니다. 사용자의 쿼리와 동일한 언어로 응답하십시오.
위키백과에서 더 많은 문맥을 얻으려면 응답 끝에 다음 형식을 사용하여 Wiki 페이지와 언어 코드를 나타내십시오:
>>>search term
>>>language code"""

prompt = """<|begin_of_text|><|start_header_id|>system<|end_header_id|>
{}<|begin_of_text|><|start_header_id|>user<|end_header_id|>
{}<|begin_of_text|><|start_header_id|>assistant<|end_header_id>"""


<div class="content-ad"></div>

이 코드는 팬패블러에게 "Curb Your Enthusiasm"에 관한 팬픽션 아이디어 세 가지를 제안하는 내용을 묻는 것이에요. 라리가 시간 여행기를 발견하는 내용이네요.

```js
question = "Larry가 시간 여행기를 발견하면서 'Curb Your Enthusiasm'에 관한 팬픽션을 쓰기 위한 세 가지 아이디어를 나열해 보세요."

text = prompt.format(system_prompt, question)
inputs = tokenizer([text], return_tensors = "pt").to("cuda")
text_streamer = TextStreamer(tokenizer, skip_prompt=True)
resulting_tokens = model.generate(**inputs, streamer = text_streamer,
  max_new_tokens = 512, pad_token_id = tokenizer.eos_token_id,
  do_sample = True)
```

저는 실시간으로 모델에서 나오는 결과를 보여주는 텍스트 스트리머 기능을 사용했어요. 또한 do_sample을 설정하여 모델이 실행될 때마다 다른 답변을 생성하도록 했어요.

# 결과

<div class="content-ad"></div>

여기 팬페이블러와의 몇 가지 대화가 있어요.

## 열정을 억제하세요 (영어)

이건 흥미로운 시각이에요. 1920년대 금주 시대의 선택은 라리의 반사회적 감성과 역사적 배경 사이에 창의적이고 유머러스한 대조를 제공해요. 라리의 언행이 알 카폰과 같은 인물로 인해 말도 안 되는 상황을 만들 수 있는 동시에, 이러한 다이내믹은 독특한 코미디적 긴장감을 만들어낼 수 있어요. 하지만 유머와 역사적 맥락을 균형있게 유지하는 것은 어렵고 세심한 접근이 필요해요.

## 피피 롱스토킹 (스웨덴어)

<div class="content-ad"></div>

Här är en interaktion på svenska om fan fiction för Pippi Långstrump. (Here is an interaction in Swedish about fan fiction for Pippi Longstocking.)

Det är intressant att ha interaktioner mellan olika versioner av huvudkaraktären. En äldre, erfaren Pippi porträtteras som en pionjär med äventyrlig anda, medan en yngre version bär med sig nyfikenhet och stödjer sina vänner i deras äventyr. Samspel mellan dessa två Pippis, med deras olika personligheter och perspektiv, skulle kunna leda till en ny twist på karaktärsutveckling och dialog inom fan fiction-narrativet.

Du kan prova FanFabler med Google Colab här.

#Slutsats

<div class="content-ad"></div>

FanFabler은 Llama 3와 같은 대형 언어 모델의 발전을 기반으로한 다국어 팬픽션 작성을 향상시키는 데에 기여합니다. FanFabler은 걸러진 데이터셋으로 세밀한 조정 및 Low-Rank Adaptation (LoRA)과 같은 기술을 사용하여 40개 언어로 쓰기를 지원하도록 맞춤화합니다. 이 프로젝트는 신중한 데이터 수집과 고급 교육 방법의 중요성을 강조하며, GPT 3.5 Turbo 및 위키피디아를 활용하여 맥락 정보를 제공합니다. 결과물 공유는 창의적 AI에 대한 추가 실험을 영감으로 삼으며, 특화된 언어 모델의 잠재력을 보여줍니다.

# 소스 코드와 Colab

이 프로젝트의 코드는 GitHub에서 사용할 수 있습니다. 데이터셋 및 세밀하게 튜닝된 모델은 Huggingface에서 사용할 수 있습니다. 저는 이 모든 것을 CC BY-SA 라이선스에 따라 공개했습니다.

![FanFablerFine-TuningLlama3toBeaMultilingualFanficWritingAssistant](/assets/img/2024-07-14-FanFablerFine-TuningLlama3toBeaMultilingualFanficWritingAssistant_1.png)

<div class="content-ad"></div>

# 감사의 말

이 프로젝트에 도움을 주신 Jennifer Lim에게 감사드립니다.

# 참고 자료

[1] Meta, Meta Llama 3 소개: 오늘까지 가장 강력한 오픈 소스 LLM(2024)

<div class="content-ad"></div>

[2] H. Touvron et al., Llama 2: Open Foundation and Fine-Tuned Chat Models (2023)

[3] A. Q. Jiang, et. al, Mistral 7B (2023)

[4] B. Gribben, Fanfiction: A Legal Battle of Creativity (2016), Reporter Magazine

[5] D. M. Eberhard et al., Statistics (2024), Ethnologue: Languages of the World (27th ed.)

<div class="content-ad"></div>

**[6]** H. Touvron et al., LLaMA: Open and Efficient Foundation Language Models (2023)

**[7]** Edward Hu et al., LoRA: Low-Rank Adaptation of Large Language Models (2021)