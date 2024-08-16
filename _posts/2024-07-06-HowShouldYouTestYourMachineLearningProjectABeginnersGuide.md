---
title: "머신 러닝 프로젝트 테스트 방법 초보자를 위한 가이드"
description: ""
coverImage: "/assets/img/2024-07-06-HowShouldYouTestYourMachineLearningProjectABeginnersGuide_0.png"
date: 2024-07-06 02:59
ogImage: 
  url: /assets/img/2024-07-06-HowShouldYouTestYourMachineLearningProjectABeginnersGuide_0.png
tag: Tech
originalTitle: "How Should You Test Your Machine Learning Project? A Beginner’s Guide"
link: "https://medium.com/towards-data-science/how-should-you-test-your-machine-learning-project-a-beginners-guide-2e22da5a9bfc"
isUpdated: true
---





## Pytest 및 Pytest-cov와 같은 표준 라이브러리를 사용하여 머신 러닝 프로젝트를 테스트하는 친절한 소개

![image](/assets/img/2024-07-06-HowShouldYouTestYourMachineLearningProjectABeginnersGuide_0.png)

# 소개

소프트웨어 개발에서 테스트는 중요한 구성 요소입니다. 그러나 제 경험상 머신 러닝 프로젝트에서는 이를 널리 소홀히 합니다. 많은 사람들이 코드를 테스트해야 한다는 것을 알지만, 그 방법을 잘 알고 실제로 하는 사람은 많지 않습니다.

<div class="content-ad"></div>

**이 가이드는 머신 러닝 파이프라인의 다양한 부분을 테스트하는 기본 사항을 소개합니다. 우리는 IMDb 데이터셋에서 텍스트 분류를 위해 BERT를 세밀하게 조정하고 pytest와 pytest-cov와 같은 산업 표준 라이브러리를 사용하여 테스트하는 방법에 초점을 맞출 것입니다.**

**이 Github 저장소의 코드를 따라가는 것을 강력히 권장합니다:**
 
# 프로젝트 개요

**프로젝트의 간략한 개요입니다.**

<div class="content-ad"></div>


Tarot experts often say that splitting your codebase into different sections can help organize and manage your project effectively:

- **src**: This directory houses essential files for tasks like loading datasets, training models, and evaluating their performance.
- **tests**: Here, you'll find various Python scripts dedicated to testing different functionalities of your codebase. A common convention is to name a test script "test_filename.py" where "filename.py" is the script being tested.

For instance, if you wish to test the `evaluation.py` file, you would create a corresponding `test_evaluation.py` file within the tests directory.


<div class="content-ad"></div>

**참고:** 테스트 폴더에 conftest.py 파일이 있음을 알 수 있습니다. 이 파일은 정확히 말하자면 테스트 함수가 아니지만, 주로 우리가 나중에 약간 설명할 픽스처와 같은 테스트에 대한 몇 가지 설정 정보를 포함하고 있습니다.

# 시작하는 방법

이 글을 읽는 것만으로도 좋지만, 리포지토리를 클론하고 코드를 다뤄보는 것을 강력히 권장합니다. 활동적으로 배우는 것이 항상 더 나은 방법이기 때문이죠. 그를 위해 깃허브 리포지토리를 클론하고 환경을 생성하고 모델을 얻어야 합니다.


# 깃허브 리포지토리 클론
git clone https://github.com/FrancoisPorcher/awesome-ai-tutorials/tree/main

# 해당 폴더로 이동
cd MLOps/how_to_test/

# 환경 생성
conda env create -f environment.yml
conda activate how_to_test


<div class="content-ad"></div>

평가를 실행하는 데 모델이 필요합니다. 제 결과를 재현하려면 메인 파일을 실행할 수 있습니다. 훈련은 CUDA, MPS 또는 CPU에 따라 2분에서 20분이 소요될 수 있습니다.

```python
python src/main.py
```

만약 BERT를 세밀하게 조정하고 싶지 않다면(하지만 강력히 권장하는 바이니, 직접 BERT를 세밀하게 조정하는 것이 좋습니다), BERT의 기본 버전을 가져와서 다음 명령을 사용하여 2개 클래스를 얻기 위해 선형 레이어를 추가할 수 있습니다:

```python
from transformers import BertForSequenceClassification

model = BertForSequenceClassification.from_pretrained(
            "bert-base-uncased", num_labels=2
        )
```

<div class="content-ad"></div>

이제 모든 준비가 되었어요!

테스트를 작성해 봅시다:

하지만 먼저, Pytest에 대해 간단히 소개하겠습니다.

# Pytest란 무엇이며 어떻게 사용하는 건가요?

<div class="content-ad"></div>

pytest은 산업에서 표준이자 성숙한 테스트 프레임워크로, 테스트 작성을 쉽게 만드는 장점이 있어요.

pytest의 멋진 점 중 하나는 다양한 레벨에서 테스트할 수 있다는 것이죠: 단일 함수, 스크립트 또는 전체 프로젝트를 테스트할 수 있어요. 이 3가지 옵션을 어떻게 사용하는지 알아봐요.

## 테스트는 어떻게 생겼나요?

테스트는 다른 함수의 동작을 테스트하는 함수입니다. 관례상 함수 called foo를 테스트하려면 test_foo와 같이 이름 지어주는 거죠.

<div class="content-ad"></div>

우리는 그 후에 테스트를 몇 가지 정의하여 우리가 테스트하는 기능이 우리가 원하는 대로 작동하는지 확인합니다.

## 예를 들어 아이디어를 명확히 해 보겠습니다:

data_loader.py 스크립트에서 clean_text라는 매우 표준 함수를 사용하고 있습니다. 이 함수는 대문자와 공백을 제거하는 함수로 다음과 같이 정의됩니다:

```python
def clean_text(text: str) -> str:
    """
    Clean the input text by converting it to lowercase and stripping whitespace.

    Args:
        text (str): The text to clean.

    Returns:
        str: The cleaned text.
    """
    return text.lower().strip()
```

<div class="content-ad"></div>

해당 기능이 잘 작동하는지 확인하려면 test_data_loader.py 파일에 test_clean_text 함수를 작성할 수 있습니다.

```python
from src.data_loader import clean_text

def test_clean_text():
    # 대문자 테스트
    assert clean_text("HeLlo, WoRlD!") == "hello, world!" 
    # 공백 제거 테스트
    assert clean_text("  Spaces  ") == "spaces"
    # 빈 문자열 테스트
    assert clean_text("") == ""
```

여기서는 assert 함수를 사용했습니다. 단언문이 참이면 아무 일도 일어나지 않고, 거짓인 경우 AssertionError가 발생합니다.

이제 테스트를 호출해 보겠습니다. 터미널에서 다음 명령을 실행해 주세요.

<div class="content-ad"></div>

흐음, 멋지네요! 당신이 하고 있는 일이 정말 잘 되고 있어요! pytest를 사용해서 테스트를 실행하고 있는군요. test_data_loader.py 스크립트가 tests 폴더 안에 있는 것을 확인했습니다. 그리고 test_clean_text 테스트만 실행하려는 거군요.

테스트가 통과되면, 이 그림을 보실 수 있을 거에요: 

![How to test your machine learning project](/assets/img/2024-07-06-HowShouldYouTestYourMachineLearningProjectABeginnersGuide_1.png)

<div class="content-ad"></div>

테스트가 통과되지 않을 때 어떻게 되는지 궁금할 수 있죠?

이번 예시를 위해 test_clean_text 함수를 다음과 같이 수정했다고 상상해 봅시다:

```js
def clean_text(text: str) -> str:
    # return text.lower().strip()
    return text.lower()
```

이제 함수가 더 이상 공백을 제거하지 않으므로 테스트를 통과하지 못할 것입니다. 테스트를 다시 실행하면 이런 결과가 나올 거예요: 

<div class="content-ad"></div>

이번에는 테스트가 실패한 이유를 알게 되었네요. 멋지네요!

### 단일 기능을 테스트하려면 왜 하려고 할까요?

테스트는 많은 시간이 소요될 수 있습니다. 이 프로젝트처럼 작은 경우에는 IMDb 데이터셋 전체를 평가하는 데 이미 몇 분이 걸릴 수 있습니다. 때로는 전체 코드베이스를 매번 다시 테스트하지 않고도 특정 동작을 테스트하고 싶을 때가 있습니다.

<div class="content-ad"></div>

이제 다음 단계로 이동해 보겠습니다: 스크립트 테스트하기.

## 전체 스크립트를 어떻게 테스트할까요?

이제 data_loader.py 스크립트를 복잡하게 만들고 tokenize_text 함수를 추가할 차례입니다. tokenize_text 함수는 입력으로 문자열이나 문자열 목록을 받아들이고 입력의 토큰화된 버전을 출력합니다.

```python
import torch
from transformers import BertTokenizer

def clean_text(text: str) -> str:
    """
    Clean the input text by converting it to lowercase and stripping whitespace.

    Args:
        text (str): The text to clean.

    Returns:
        str: The cleaned text.
    """
    return text.lower().strip()

def tokenize_text(
    text: str, tokenizer: BertTokenizer, max_length: int
) -> Dict[str, torch.Tensor]:
    """
    Tokenize a single text using the BERT tokenizer.

    Args:
        text (str): The text to tokenize.
        tokenizer (BertTokenizer): The tokenizer to use.
        max_length (int): The maximum length of the tokenized sequence.

    Returns:
        Dict[str, torch.Tensor]: A dictionary containing the tokenized data.
    """
    return tokenizer(
        text,
        padding="max_length",
        truncation=True,
        max_length=max_length,
        return_tensors="pt",
    )
```

<div class="content-ad"></div>

이 함수가 무엇을 하는지 좀 더 이해할 수 있도록 예제를 통해 알아보겠습니다:

```python
from transformers import BertTokenizer
tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")
txt = ["Hello, @! World! qwefqwef"]
tokenize_text(txt, tokenizer=tokenizer, max_length=16)
```

위 코드를 실행하면 다음과 같은 결과가 출력됩니다:

```python
{
  'input_ids': tensor([[ 101, 7592, 1010, 1030,  999, 2088,  999, 1053, 8545, 2546, 4160, 8545,2546,  102,    0,    0]]), 
  'token_type_ids': tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]), 
  'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0]])
}
```

<div class="content-ad"></div>

-max_length: 시퀀스가 가질 수 있는 최대 길이를 말합니다. 이 경우에는 16을 선택했지만, 시퀀스의 길이가 14이므로 마지막 2개 토큰은 채워진 것을 볼 수 있습니다.
- input_ids: 각 토큰은 해당하는 ID로 변환됩니다. 이는 어휘 집합에 속하는 단어들을 나타냅니다. 참고: 토큰 101은 CLS 토큰이고, 토큰 102는 SEP 토큰입니다. 이 두 토큰은 문장의 시작과 끝을 나타냅니다. 자세한 내용은 "Attention is all you need" 논문을 읽어보세요.
- token_type_ids: 이것은 그렇게 중요하지 않습니다. 입력으로 2개의 시퀀스를 주면 두 번째 문장에 대한 1개의 값이 있을 것입니다.
- attention_mask: 이것은 모델에게 자기 어텐션 메커니즘에서 어떤 토큰에 주의를 기울여야 하는지 알려줍니다. 문장이 채워졌기 때문에 어텐션 메커니즘은 마지막 2개의 토큰에 신경 쓸 필요가 없으므로 여기에는 0이 있습니다.

이제 우리의 test_tokenize_text 함수를 작성해봅시다. 이 함수는 tokenize_text 함수가 올바르게 작동하는지 확인할 것입니다:

```python
def test_tokenize_text():
    """
    Test the tokenize_text function to ensure it correctly tokenizes text using BERT tokenizer.
    """
    tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")

    # 예제 입력 텍스트
    txt = ["Hello, @! World!", 
           "Spaces    "]

    # 텍스트 토큰화
    max_length = 128
    res = tokenize_text(text=txt, tokenizer=tokenizer, max_length=max_length)
    
    # 출력이 사전인지, 그리고 키가 올바른지 확인하는 테스트
    assert all(key in res for key in ["input_ids", "token_type_ids", "attention_mask"]), "출력 사전에 누락된 키가 있습니다."
    
    # 출력 텐서의 차원을 확인하는 테스트
    assert res["input_ids"].shape[0] == len(txt), "잘못된 input_ids 개수입니다."
    assert res['input_ids'].shape[1] == max_length, "잘못된 토큰 개수입니다."
    
    # 모든 연관된 텐서가 파이토치 텐서인지 확인하는 테스트
    assert all(isinstance(res[key], torch.Tensor) for key in res), "모든 값이 파이토치 텐서가 아닙니다."
```

<div class="content-ad"></div>

- test_tokenize_text
- test_clean_text

터미널에서 다음 명령어를 사용하여 전체 테스트를 실행할 수 있어요.

```js
pytest tests/test_data_loader.py
```

그리고 아래 결과가 나와야 해요:

<div class="content-ad"></div>

앗! 축하해요! 이제 전체 스크립트를 테스트하는 방법을 알게 되었어요. 이제 마지막 단계인 전체 코드베이스 테스트로 넘어가볼까요?

## 전체 코드베이스를 어떻게 테스트할까요?

이전과 같은 논리를 따라서, 각 스크립트에 대한 다른 테스트를 작성할 수 있으며, 비슷한 구조를 가져야 합니다:

<div class="content-ad"></div>


### 파일 구조
- tests/
  - conftest.py
  - test_data_loader.py
  - test_evaluation.py
  - test_main.py
  - test_trainer.py
  - test_utils.py

모든 이 테스트 함수들에서 특정 변수들이 상수임을 주목해보세요. 예를 들어, 우리가 사용하는 토크나이저는 모든 스크립트에서 동일합니다. Pytest에는 Fixture를 사용하여 이를 처리하는 멋진 방법이 있습니다.

Fixture는 테스트를 실행하기 전에 일부 컨텍스트나 상태를 설정하고 실행 후에 정리하는 방법입니다. 이들은 테스트 의존성을 관리하고 재사용 가능한 코드를 테스트에 주입하는 메커니즘을 제공합니다.

Fixture는 @pytest.fixture 데코레이터를 사용하여 정의됩니다.


<div class="content-ad"></div>

Tokenizer는 사용할 수 있는 fixture의 좋은 예입니다. 이를 위해 tests 폴더에 있는 conftest.py 파일에 추가해 봅시다:

```python
import pytest
from transformers import BertTokenizer

@pytest.fixture()
def bert_tokenizer():
    """BERT 토크나이저를 초기화하는 fixture입니다."""
    return BertTokenizer.from_pretrained("bert-base-uncased")
```

이제 test_data_loader.py 파일에서 test_tokenize_text의 인수로 bert_tokenizer fixture를 호출할 수 있습니다.

```python
def test_tokenize_text(bert_tokenizer):
    """
    BERT 토크나이저를 사용하여 텍스트를 올바르게 토큰화하는지 확인하는 tokenize_text 함수를 테스트합니다.
    """
    tokenizer = bert_tokenizer

    # 예시 입력 텍스트
    txt = ["Hello, @! World!", 
           "Spaces    "]

    # 텍스트를 토큰화합니다.
    max_length = 128
    res = tokenize_text(text=txt, tokenizer=tokenizer, max_length=max_length)
    
    # 출력이 사전인지 확인하고 키가 올바른지 테스트합니다.
    assert all(key in res for key in ["input_ids", "token_type_ids", "attention_mask"]), "출력 사전에 키가 누락되었습니다."
    
    # 출력 텐서의 차원을 확인합니다.
    assert res["input_ids"].shape[0] == len(txt), "잘못된 input_ids 개수입니다."
    assert res['input_ids'].shape[1] == max_length, "잘못된 토큰 개수입니다."
    
    # 모든 연관된 텐서가 파이토치 텐서인지 확인합니다.
    assert all(isinstance(res[key], torch.Tensor) for key in res), "모든 값이 PyTorch 텐서가 아닙니다."
```

<div class="content-ad"></div>

Fixtures are a highly potent and flexible tool in the world of testing. If you're eager to delve deeper into their intricacies, the official documentation is your ultimate guide. At this point, you're well-equipped with the knowledge necessary to handle most of your machine learning testing needs.

To execute the entire codebase, run the command below in your terminal using Markdown format:


pytest tests


Upon doing so, you should receive the following message:

<div class="content-ad"></div>

/assets/img/2024-07-06-HowShouldYouTestYourMachineLearningProjectABeginnersGuide_4.png

축하해요!

# Pytest-cov를 사용하여 테스트 커버리지를 측정하는 방법은?

이전 섹션에서 코드를 테스트하는 방법을 배웠습니다. 대형 프로젝트에서는 테스트 커버리지를 측정하는 것이 중요합니다. 다시 말해, 코드 중에 얼마나 많은 부분이 테스트되었는지 파악할 필요가 있습니다.

<div class="content-ad"></div>

**pytest-cov**은 pytest를 위한 플러그인으로, 테스트 커버리지 보고서를 생성합니다.

하지만, 커버리지 백분율에 속지 마세요. 100% 커버리지가 되었다고 해서 코드가 버그가 없다는 것은 아닙니다. 이는 단지 코드의 어느 부분이 더 많은 테스트가 필요한지를 식별할 수 있는 도구일 뿐입니다.

터미널에서 아래 명령을 실행하여 커버리지 보고서를 생성할 수 있습니다:

```js
pytest --cov=src --cov-report=html tests/
```

<div class="content-ad"></div>

어제 보셨던 내용에 대해 이렇게 말해 볼게요:

- 진술: 코드에서 실행 가능한 총 문장 수입니다. 조건문, 반복문 및 함수 호출을 포함한 실행 가능한 모든 코드 라인을 세어줍니다.
- 누락: 테스트 실행 중 실행되지 않은 문장의 수를 나타냅니다. 즉, 어떠한 테스트에도 포함되지 않은 코드 라인입니다.
- 커버리지: 테스트 중 실행된 전체 문장의 백분율을 나타냅니다. 실행된 문장 수를 전체 문장 수로 나누어 계산됩니다.
- 제외: 커버리지 측정에서 명시적으로 제외된 코드 라인을 가리킵니다. 디버깅 문에서처럼 테스트 커버리지에 필요하지 않은 코드를 무시하는 데 유용합니다.

<div class="content-ad"></div>

우리는 main.py 파일의 커버리지가 0%인 것을 볼 수 있습니다. 이게 정상입니다. 우리는 test_main.py 파일을 작성하지 않았거든요.

또한, 우리는 평가 코드의 19%만 테스트되었다는 것을 볼 수 있습니다. 이것은 우리가 먼저 어디에 집중해야 할지에 대한 아이디어를 제공해줍니다.

축하해요, 당신이 성공했어요!

읽어주셔서 감사합니다! 떠나시기 전에:

<div class="content-ad"></div>

더 멋진 안내서를 찾고 싶다면, Github에서 제가 편집한 AI 튜토리얼 모음을 확인해보세요.

내 글을 이메일로 받고 싶다면 여기에서 구독하세요.

Medium의 프리미엄 기사에 접근하고 싶다면 매월 $5의 멤버십만 있으면 됩니다. 제 링크로 가입하면 추가 비용 없이 회비의 일부를 지원해줄 수 있어요.

# 참고문헌

<div class="content-ad"></div>

* https://docs.pytest.org/en/8.2.x/
* https://pypi.org/project/pytest-cov/