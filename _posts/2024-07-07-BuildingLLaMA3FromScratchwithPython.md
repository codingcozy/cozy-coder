---
title: "Python으로 LLaMA 3 처음부터 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_0.png"
date: 2024-07-07 22:56
ogImage: 
  url: /assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_0.png
tag: Tech
originalTitle: "Building LLaMA 3 From Scratch with Python"
link: "https://medium.com/gitconnected/building-llama-3-from-scratch-with-python-e0cf4dbbc306"
---


LLaMA 3은 Mistral 이후 가장 유망한 오픈 소스 모델 중 하나로 다양한 작업을 해결합니다. 이전에 LLM을 LLaMA 아키텍처를 사용하여 2.3백만 개가 넘는 매개변수로 처음부터 만드는 방법에 대한 블로그를 Medium에 작성했습니다. 이제 LLaMA-3이 출시되었으므로 조금 더 간단하게 재현하겠습니다.

이 블로그에서는 GPU를 사용하지는 않지만, 15GB 이상의 파일을 로드해야 하므로 적어도 17GB RAM이 필요합니다. 이 점이 문제라면 Kaggle을 해결책으로 사용할 수 있습니다. GPU가 필요하지 않기 때문에 Kaggle은 CPU 코어만 가속기로 사용하면서도 30GB RAM을 제공합니다.

이 블로그에서 코드를 복사하여 붙여 넣는 것을 피하기 위해 GitHub 저장소에 노트북 파일과 모든 코드 및 정보가 포함된 링크가 있습니다:

<div class="content-ad"></div>

여기 LLM(대규모 언어 모델)을 처음부터 만드는 방법을 안내하는 블로그 링크가 있어요:

# 목차

- 선행 요구 사항
- LLaMA 2와 LLaMA 3의 차이
- LLaMA 3의 트랜스포머 아키텍처 이해
∘ RMSNorm을 사용한 사전 정규화
∘ SwiGLU 활성화 기능
∘ Rotary Embeddings (RoPE)
∘ Byte Pair Encoding (BPE) 알고리즘
- 무대를 설정하기
- 파일 구조 이해하기
- 입력 데이터 토큰화
- 각 토큰에 대한 임베딩 생성
- RMSNorm을 사용한 정규화
- 어텐션 헤드 (쿼리, 키, 값)
- RoPE 구현
- Self Attention 구현
- Multi-Head Attention 구현
- SwiGLU 활성화 기능 구현
- 모든 것을 병합
- 출력 생성하기

# 선행 요구 사항

<div class="content-ad"></div>

좋은 소식은 객체 지향 프로그래밍(OOP) 코딩을 사용하지 않을 것이며, 순수한 파이썬 프로그래밍만 사용할 것이라는 것입니다. 그러나 신경망과 트랜스포머 구조에 대한 기본적인 이해가 필요합니다. 이 두 가지만 알고 있다면 블로그를 따라오는 데 문제가 없을 것입니다.

# LLaMA 2와 LLaMA 3의 차이

기술적인 세부 사항을 살펴보기 전에 우선 알아야 할 것은 LLaMA 3의 전체 아키텍처가 LLaMA 2와 동일하다는 것입니다. 따라서 아직 LLaMA 3의 기술적인 세부 정보를 확인하지 않았다면, 이 블로그를 따라가는 데 문제가 되지 않을 것입니다. LLaMA 2 아키텍처에 대한 이해가 없더라도 걱정하지 마세요. 우리는 그 기술적인 세부 정보에 대한 고수준 개요도 살펴볼 것입니다. 이 블로그는 여러분을 위해 설계되었습니다.

LLaMA 2와 LLaMA 3에 관한 중요한 몇 가지 포인트를 소개합니다. 이미 그들의 아키텍처에 익숙하다면:

<div class="content-ad"></div>

## LLaMA 3의 Transformer 아키텍처 이해하기

코딩에 들어가기 전에 LLaMA 3의 아키텍처를 이해하는 것이 중요합니다. 시각적 이해를 돕기 위해 일반 Transformer, LLaMA 2/3, 그리고 Mistral 간의 비교 다이어그램을 준비했습니다.

이제 LLaMA 3의 가장 중요한 구성 요소를 좀 더 자세히 살펴보겠습니다:

### 1. RMSNorm을 사용한 Pre-normalization:

<div class="content-ad"></div>

LLaMA 3 방식에서 LLaMA 2와 동일하게, 각 트랜스포머 서브 레이어의 입력을 정규화하는 기술인 RMSNorm을 사용합니다.

큰 시험을 준비 중이라고 상상해봅시다. 여러 장으로 이루어진 굉장히 큰 교과서가 있다고 상상해봅시다. 각 장은 다른 주제를 나타내지만, 일부 장은 다른 장보다 주제를 이해하는 데 있어서 더 중요합니다.

이제 전체 교과서에 뛰어들기 전에 각 장의 중요성을 평가하기로 결정합니다. 모든 장에 동일한 시간을 할애하고 싶지 않습니다. 대신 중요한 장에 더 많은 시간을 할애하고 싶습니다.

이것은 ChatGPT와 같은 대형 언어 모델(LLMs)에 대해 RMSNorm을 사용한 사전 정규화가 등장하는 곳입니다. 이는 각 장에 중요도를 부여하는 것과 같습니다. 주제에 필수적인 장은 높은 가중치를 받고, 중요하지 않은 장은 낮은 가중치를 받습니다.

<div class="content-ad"></div>

딥하게 공부에 들어가기 전에, 각 챕터의 중요도에 따라 공부 계획을 조절하는 거지. 더 중요한 챕터에는 더 많은 시간과 노력을 투자해서, 핵심 개념을 철저히 이해하는 걸 보장하는 거야.

![이미지](/assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_1.png)

비슷하게, RMSNorm을 사용한 사전 정규화는 LLM이 텍스트의 어떤 부분이 맥락과 의미를 이해하는 데 더 중요한지 우선순위를 정하는 거야. 중요한 요소에 높은 가중치를 할당하고 중요하지 않은 것은 낮은 가중치를 주기 때문에, 모델이 정확한 이해를 위해 가장 필요한 곳에 주의를 집중하도록 보장해. RMSNorm의 자세한 구현 내용은 여기서 확인할 수 있어.

## 2. SwiGLU 활성화 함수:

<div class="content-ad"></div>

LLaMA이 SwiGLU 활성화 함수를 소개합니다. 이는 PaLM에서 영감을 받았습니다.

상상해봅시다. 여러분은 복잡한 주제를 학생들에게 설명하려는 교사입니다. 중요한 요점을 적고 개념을 명확하게 전달하기 위해 다이어그램을 그릴 수 있는 큰 화이트보드가 있습니다. 그러나 때로는 여러분의 필기가 매우 깔끔하지 않을 수도 있고, 다이어그램이 완벽하게 그려지지 않을 수도 있습니다. 이는 학생들이 자료를 이해하는 데 어렵게 만들 수 있습니다.

이제 마법 펜이 있다고 상상해보세요. 이 펜은 각 핵심 요소의 중요성에 따라 글씨의 크기와 스타일을 자동으로 조정합니다. 극히 중요한 내용이면 펜은 그것을 더 크고 명확하게 쓰며 두드러지도록 만듭니다. 중요성이 덜한 경우, 펜은 그것을 작게 쓰지만 여전히 읽을 수 있게 작성합니다.

SwiGLU는 ChatGPT와 같은 대형 언어 모델(LLM)을 위한 그 마법 펜과 같은 것입니다. 텍스트를 생성하기 전에 SwiGLU는 각 단어나 구문의 문맥과의 관련성에 기초하여 중요도를 조정합니다. 마법 펜이 필기의 크기와 스타일을 조정하는 것처럼, SwiGLU는 각 단어나 구문의 강조를 조정합니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_2.png)

So, when the Language Model generates text, it can give more prominence to the important parts, making them more noticeable and ensuring they contribute more to the overall understanding of the text. This way, SwiGLU helps Language Models produce text that’s clearer and easier to understand, much like how the magic pen helps you create clearer explanations for your students on the whiteboard. Further details on SwiGLU can be found in the associated paper.

### 3. 로테이션 임베딩 (RoPE):

로테이션 임베딩 또는 RoPE는 LLaMA 3에서 사용되는 위치 임베딩의 한 유형입니다.

<div class="content-ad"></div>

수업실 안에 있다고 상상해보세요. 학생들에게 그룹 토론을 위한 좌석을 배치하고 싶습니다. 일반적으로 행과 열로 좌석을 배치하여 각 학생이 고정된 위치를 갖게 할 수 있습니다. 하지만 경우에 따라서는, 학생들이 더 자유롭게 움직이고 상호 작용할 수 있는 더 동적인 좌석 배치를 만들고 싶을 수도 있습니다.

ROPE은 각 학생이 상대적인 위치를 유지하면서 회전하고 위치를 변경할 수 있는 특별한 좌석 배치와 같습니다. 한 곳에 고정되어 있는 대신, 학생들은 이제 원형 운동을 통해 움직일 수 있어 더 유연한 상호 작용을 가능케 합니다.

이 시나리오에서 각 학생은 텍스트 시퀀스의 단어나 토큰을 나타내며, 그들의 위치는 시퀀스 내 위치와 대응됩니다. 학생들이 회전하고 위치를 변경할 수 있는 것처럼, ROPE은 텍스트 시퀀스 내 단어의 위치 임베딩을 상대적인 위치에 따라 동적으로 변환되도록 하는 것입니다.

그래서 텍스트를 처리할 때, ROPE는 위치 임베딩을 고정되고 정적으로 다루는 대신, 회전 요소를 도입하여 시퀀스 내 단어 사이의 동적 관계를 캡처하는 더 유연한 표현을 제공합니다. 이 유연성은 ChatGPT와 같은 모델이 자연스럽게 흘러가고 일관성을 유지하는 텍스트를 이해하고 생성하는 데 도움이 됩니다. 마치 동적인 좌석 배치가 수업실에서 더 활발한 토론을 유도하는 것처럼요. 수학적 세부사항에 관심이 있는 분들은 RoPE 논문을 참고할 수 있습니다.

<div class="content-ad"></div>

## 4. 바이트 페어 인코딩 (BPE) 알고리즘

LLaMA 3에서는 OpenAI에서 소개된 tiktoken 라이브러리의 바이트 페어 인코딩 (BPE)을 사용하며, LLaMA 2 토크나이저 BPE는 sentencepiece 라이브러리를 기반으로 합니다. 두 가지의 약간의 차이가 있지만, 먼저 BPE가 무엇인지 알아보겠습니다.

간단한 예시로 시작해봅시다. "ab", "bc", "bcd", 그리고 "cde" 단어들로 구성된 텍스트 코퍼스가 있다고 가정해봅시다. 우리는 텍스트 코퍼스의 모든 개별 문자로 용어집을 초기화하고, 따라서 초기 용어집은 '“a”, “b”, “c”, “d”, “e”' 입니다.

다음으로, 텍스트 코퍼스에서 각 문자의 빈도를 계산합니다. 이 예제에서 각 문자의 빈도는 '“a”: 1, “b”: 3, “c”: 3, “d”: 2, “e”: 1'입니다.

<div class="content-ad"></div>

이제 병합 과정을 시작합니다. 원하는 크기의 어휘 사전을 얻을 때까지 다음 단계를 반복합니다:

- 먼저 연속된 문자 쌍 중에서 가장 빈도가 높은 쌍을 찾습니다. 이 경우, 가장 빈도가 높은 쌍은 "bc"로 빈도가 2입니다. 이 쌍을 합쳐 새로운 하위어휘 단위 "bc"를 만듭니다. 합친 후에는 새로운 하위어휘 단위를 반영하기 위해 빈도를 업데이트합니다. 업데이트된 빈도는 "a: 1, b: 2, c: 2, d: 2, e: 1, bc: 2"입니다. 새로운 하위어휘 단위 "bc"를 어휘 사전에 추가하여 "a, b, c, d, e, bc"로 만듭니다.
- 이 과정을 반복합니다. 다음으로 가장 빈도가 높은 쌍은 "cd"입니다. "cd"를 병합하여 새로운 하위어휘 단위 "cd"를 형성하고 빈도를 업데이트합니다. 업데이트된 빈도는 "a: 1, b: 2, c: 1, d: 1, e: 1, bc: 2, cd: 2"입니다. "cd"를 어휘 사전에 추가하여 "a, b, c, d, e, bc, cd"로 만듭니다.
- 계속 진행하면 다음으로 가장 빈도가 높은 쌍은 "de"입니다. "de"를 병합하여 하위어휘 단위 "de"를 형성하고 빈도를 업데이트합니다. 업데이트된 빈도는 "a: 1, b: 2, c: 1, d: 1, e: 0, bc: 2, cd: 1, de: 1"입니다. "de"를 어휘 사전에 추가하여 "a, b, c, d, e, bc, cd, de"로 만듭니다.
- 다음으로 "ab"를 가장 빈도가 높은 쌍으로 찾습니다. "ab"를 병합하여 하위어휘 단위 "ab"를 형성하고 빈도를 업데이트합니다. 업데이트된 빈도는 "a: 0, b: 1, c: 1, d: 1, e: 0, bc: 2, cd: 1, de: 1, ab: 1"입니다. "ab"를 어휘 사전에 추가하여 "a, b, c, d, e, bc, cd, de, ab"로 만듭니다.
- 그 다음 빈도가 높은 쌍은 "bcd"입니다. "bcd"를 병합하여 하위어휘 단위 "bcd"를 형성하고 빈도를 업데이트합니다. 업데이트된 빈도는 "a: 0, b: 0, c: 0, d: 0, e: 0, bc: 1, cd: 0, de: 1, ab: 1, bcd: 1"입니다. "bcd"를 어휘 사전에 추가하여 "a, b, c, d, e, bc, cd, de, ab, bcd"로 만듭니다.
- 마지막으로 가장 빈도가 높은 쌍은 "cde"입니다. "cde"를 병합하여 하위어휘 단위 "cde"를 형성하고 빈도를 업데이트합니다. 업데이트된 빈도는 "a: 0, b: 0, c: 0, d: 0, e: 0, bc: 1, cd: 0, de: 0, ab: 1, bcd: 1, cde: 1"입니다. "cde"를 어휘 사전에 추가하여 "a, b, c, d, e, bc, cd, de, ab, bcd, cde"로 만듭니다.

이 기술은 LLM의 성능을 향상시키고 드물거나 어휘에 없는 단어들을 처리하는 데 도움이 됩니다. TikToken BPE와 sentencepiece BPE 사이의 큰 차이점은 TikToken BPE가 이미 알려진 단어가 전체 단어로 이미 알려져 있으면 단어를 더 작은 부분으로 분할하지 않는다는 것입니다. 예를 들어 "hugging"이 어휘에 있는 경우 [“hug”,”ging”]으로 분할하는 대신 하나의 토큰으로 유지됩니다.

# 준비 단계 설정

<div class="content-ad"></div>

작은 범위의 Python 라이브러리를 사용할 것이지만 "모듈을 찾을 수 없음" 오류를 피하기 위해 설치하는 것이 좋습니다.


pip install sentencepiece tiktoken torch blobfile matplotlib huggingface_hub


필요한 라이브러리를 설치한 후에는 몇 가지 파일을 다운로드해야 합니다. llama-3-8B의 아키텍처를 복제할 예정이므로 HuggingFace에 계정을 갖고 있어야 합니다. 또한 llama-3는 게이트된 모델이므로 해당 모델 콘텐츠에 액세스하려면 약관에 동의해야 합니다.

아래는 진행해야 할 단계입니다:

<div class="content-ad"></div>

- 이 링크를 통해 HuggingFace 계정을 만들어주세요.
- 이 링크를 통해 'llama-3-8B'의 약관에 동의해주세요.

이 두 단계를 완료하셨다면, 이제 파일을 다운로드해야 합니다. 두 가지 옵션이 있습니다:

(옵션 1: 수동) 이 링크를 통해 'llama-3-8B HF' 디렉토리로 이동하고 세 개의 파일을 각각 수동으로 다운로드하세요.

![BuildingLLaMA3FromScratchwithPython_3](/assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_3.png)

<div class="content-ad"></div>

(options 2: 코딩) 이전에 설치한 hugging_face 라이브러리를 사용하여 이 파일들을 모두 다운로드할 수 있습니다. 그러나 먼저 작업 노트북에서 HF Token을 사용하여 HuggingFace Hub에 로그인해야 합니다. 새 토큰을 만들거나 이 링크에서 액세스할 수 있습니다.

```python
# `huggingface_hub` 모듈에서 `notebook_login` 함수를 가져옵니다.
from huggingface_hub import notebook_login

# Hugging Face Hub에 로그인하려면 `notebook_login` 함수를 실행합니다.
notebook_login()
```

이 셀을 실행하면 토큰을 입력하라는 메시지가 나타납니다. 로그인 중에 오류가 발생하면 다시 시도해보세요. 그러나 반드시 'add token as git credential' 옵션을 선택 해제해야 합니다. 그 후에는 llama-3-8B 아키텍처의 핵심을 이루는 세 파일을 다운로드하기 위해 간단한 Python 코드를 실행하면 됩니다.

```python
# huggingface_hub 라이브러리에서 필요한 함수를 가져옵니다.
from huggingface_hub import hf_hub_download

# 저장소 정보를 정의합니다.
repo_id = "meta-llama/Meta-Llama-3-8B"
subfolder = "original"  # 저장소 내 하위 폴더를 지정합니다.

# 다운로드할 파일 이름 목록
filenames = ["params.json", "tokenizer.model", "consolidated.00.pth"]

# 다운로드한 파일을 저장할 디렉토리를 지정합니다.
save_directory = "llama-3-8B/"  # 원하는 경로로 대체하세요.

# 각 파일을 다운로드합니다.
for filename in filenames:
    hf_hub_download(
        repo_id=repo_id,       # 저장소 ID
        filename=filename,     # 다운로드할 파일 이름
        subfolder=subfolder,   # 저장소 내 하위 폴더
        local_dir=save_directory  # 다운로드한 파일을 저장할 디렉토리
    )
```

<div class="content-ad"></div>

모든 파일을 다운로드하면, 이 블로그 전체에서 사용할 라이브러리를 가져와야 합니다.


# Tokenization library
import tiktoken

# BPE loading function
from tiktoken.load import load_tiktoken_bpe

# PyTorch library
import torch

# JSON handling
import json


다음으로, 각 파일이 어떻게 사용될지 이해해야 합니다.

# 파일 구조 이해하기

<div class="content-ad"></div>

우리는 라마-3의 정확한 복제를 목표로 하고 있기 때문에, 입력 텍스트는 의미 있는 결과를 내야 합니다. 예를 들어, "태양의 색깔은?" 이라는 입력이 주어지면 결과는 "흰색"이어야 합니다. 이를 달성하기 위해서는 거대한 데이터셋에서 LLM을 훈련해야 하는데, 이는 높은 계산 능력이 필요하여 우리에게는 실현하기 어려운 과제입니다.

그러나 Meta는 llama-3 구조 파일 또는 좀 더 복잡한 용어로 말하자면, 사전 훈련된 가중치를 사용할 수 있도록 공개적으로 이를 공개했습니다. 우리는 방금 이 파일들을 다운로드 받았고, 훈련이나 거대한 데이터셋이 필요하지 않고도 그들의 아키텍처를 복제할 수 있게 되었습니다. 모든 것이 이미 준비되어 있고, 우리가 해야 할 일은 올바른 구성 요소를 올바른 위치에 사용하는 것뿐입니다.

각 파일과 그 중요성을 살펴보겠습니다:

tokenizer.model — 우리가 이전에 논의한 대로, LLaMA-3은 tiktoken에서 훈련된 Byte Pair Encoding (BPE) 토크나이저를 사용하고, 이는 15조 개의 토큰을 가진 데이터셋에서 학습되었으며, 이는 LLaMA-2에 사용된 데이터셋보다 7배 큽니다. 이 파일을 로드하여 내용을 살펴봅시다.

<div class="content-ad"></div>


# llama-3-8B 토크나이저를 불러오는 중입니다.
tokenizer_model = load_tiktoken_bpe("tokenizer.model")

# 토크나이저 모델의 길이를 얻습니다.
len(tokenizer_model)
# 출력: 128000

# `tokenizer_model` 객체의 타입을 확인합니다.
type(tokenizer_model)
# 출력: dictionary


토크나이저 모델의 길이 속성은 학습 데이터의 문자 수를 나타내며, 고유한 문자의 총 개수를 보여줍니다. tokenizer_model의 타입은 dictionary입니다.


# 토크나이저 모델의 첫 10개 아이템을 출력합니다.
dict(list(tokenizer_model.items())[5600:5610])

#### 출력 ####
{
 
  b'mitted': 5600,
  b" $('#": 5601,
  b' saw': 5602,
  b' approach': 5603,
  b'ICE': 5604,
  b' saying': 5605,
  b' anyone': 5606,
  b'meta': 5607,
  b'SD': 5608,
  b' song': 5609

}
#### 출력 ####


랜덤으로 10개의 아이템을 출력하면, 이전에 논의한 예시와 같이 BPE 알고리즘을 사용하여 형성된 문자열을 볼 수 있습니다. 키는 BPE 훈련에서 생성된 Byte 시퀀스를 나타내며, 값은 빈도수에 기반한 병합 순위를 나타냅니다.

<div class="content-ad"></div>

`consolidated.00.pth` 파일은 Llama-3-8B의 학습된 매개변수(가중치)를 포함하고 있어요. 이러한 매개변수에는 모델이 언어를 이해하고 처리하는 방법에 대한 정보가 포함되어 있어요. 예를 들어, 모델이 토큰을 어떻게 표현하며, 어텐션을 계산하고, 피드포워드 변환을 수행하며, 출력을 정규화하는지에 대한 정보가 있어요.

```python
# LLaMA-3-8B의 PyTorch 모델을 로드하는 방법
model = torch.load("consolidated.00.pth")

# 아키텍처의 처음 11개 레이어 출력
list(model.keys())[:11]

#### 출력 ####
[
 'tok_embeddings.weight',
 'layers.0.attention.wq.weight',
 'layers.0.attention.wk.weight',
 'layers.0.attention.wv.weight',
 'layers.0.attention.wo.weight',
 'layers.0.feed_forward.w1.weight',
 'layers.0.feed_forward.w3.weight',
 'layers.0.feed_forward.w2.weight',
 'layers.0.attention_norm.weight',
 'layers.0.ffn_norm.weight',
 'layers.1.attention.wq.weight',

]
#### 출력 ####
```

트랜스포머 아키텍처에 익숙하다면, 쿼리(query), 키(key) 행렬 등에 대해 알고 있을 거예요. 나중에 이러한 레이어/가중치를 사용하여 Llama-3의 아키텍처 내에서 이러한 행렬을 생성할 거예요.

`params.json` 파일은 다양한 매개변수 값을 포함하고 있어요.

<div class="content-ad"></div>


# 파라미터 JSON 파일 열기
with open("params.json", "r") as f:
    config = json.load(f)

# 내용 출력
print(config)

#### 출력 결과 ####
{
 'dim': 4096,
 'n_layers': 32,
 'n_heads': 32,
 'n_kv_heads': 8,
 'vocab_size': 128256,
 'multiple_of': 1024,
 'ffn_dim_multiplier': 1.3,
 'norm_eps': 1e-05,
 'rope_theta': 500000.0
}
#### 출력 결과 ####


이러한 값들은 우리가 나중에 사용할 수 있도록 르라마-3 아키텍처를 복제하는 데 도움이 될 것입니다. 헤드 수, 임베딩 벡터의 차원 등과 같은 세부 정보를 지정하는 것입니다.

나중에 사용할 수 있도록 이러한 값을 저장합시다.


# 차원
dim = config["dim"]

# 레이어
n_layers = config["n_layers"]

# 헤드
n_heads = config["n_heads"]

# KV_헤드
n_kv_heads = config["n_kv_heads"]

# 어휘 크기
vocab_size = config["vocab_size"]

# 배수
multiple_of = config["multiple_of"]

# 배율
ffn_dim_multiplier = config["ffn_dim_multiplier"]

# 엡실론
norm_eps = config["norm_eps"]

# RoPE
rope_theta = torch.tensor(config["rope_theta"])


<div class="content-ad"></div>

이제 토크나이저 모델, 가중치가 담긴 아키텍처 모델 및 설정 매개변수를 갖고 있으니, 우리만의 Llama-3를 처음부터 코딩해 보겠습니다.

# 입력 데이터의 토큰화

가장 먼저 수행해야 할 작업은 입력 텍스트를 토큰으로 변환하는 것입니다. 이를 위해 먼저 토큰화된 텍스트 내에서 구조적인 마커를 제공하기 위해 필요한 몇 가지 특별한 토큰을 생성해야 합니다. 특별한 조건이나 지시사항을 인식하고 처리할 수 있도록 하는 토크나이저에 필요한 특수 토큰입니다.

```js
special_tokens = [
    "<|begin_of_text|>",  # 텍스트 시퀀스의 시작을 표시합니다.
    "<|end_of_text|>",  # 텍스트 시퀀스의 끝을 표시합니다.
    "<|reserved_special_token_0|>",  # 향후 사용을 위해 예약되어 있습니다.
    "<|reserved_special_token_1|>",  # 향후 사용을 위해 예약되어 있습니다.
    "<|reserved_special_token_2|>",  # 향후 사용을 위해 예약되어 있습니다.
    "<|reserved_special_token_3|>",  # 향후 사용을 위해 예약되어 있습니다.
    "<|start_header_id|>",  # 헤더 ID의 시작을 나타냅니다.
    "<|end_header_id|>",  # 헤더 ID의 끝을 나타냅니다.
    "<|reserved_special_token_4|>",  # 향후 사용을 위해 예약되어 있습니다.
    "<|eot_id|>",  # 대화적 맥락에서 턴의 끝을 표시합니다.
] + [f"<|reserved_special_token_{i}|>" for i in range(5, 256 - 5)]  # 향후 사용을 위해 예약된 많은 수의 토큰 집합입니다.
```

<div class="content-ad"></div>

다음으로, 입력 텍스트의 다양한 부분 문자열 유형에 맞는 패턴을 지정하여 텍스트를 토큰으로 분할하는 규칙을 정의합니다. 아래는 그 방법입니다.

```js
# 패턴은 텍스트를 토큰으로 분할하는 데 사용됩니다.
tokenize_breaker = r"(?i:'s|'t|'re|'ve|'m|'ll|'d)|[^\r\n\p{L}\p{N}]?\p{L}+|\p{N}{1,3}| ?[^\s\p{L}\p{N}]+[\r\n]*|\s*[\r\n]+|\s+(?!\S)|\s+"
```

입력 텍스트에서 단어, 축약형, 숫자(최대 세 자리 숫자), 및 공백이 아닌 문자들의 시퀀스를 추출할 수 있습니다. 요구사항에 맞게 사용자 정의할 수 있습니다.

TikToken BPE를 사용하여 간단한 토크나이저 함수를 작성해야 합니다. 이 함수는 tokenizer_model, tokenize_breaker, special_tokens 세 가지 입력을 받습니다. 이 함수는 우리의 입력 텍스트를 인코딩/디코딩할 것입니다.

<div class="content-ad"></div>


# 설정된 매개변수로 토크나이저를 초기화합니다.
tokenizer = tiktoken.Encoding(

    # tokenizer.model 파일의 경로를 설정해주세요.
    name = "tokenizer.model",

    # 토큰화 패턴 문자열을 정의합니다.
    pat_str = tokenize_breaker,

    # LLaMA-3의 tokenizer_model에서 BPE 병합 가능 순위를 할당합니다.
    mergeable_ranks = tokenizer_model,

    # 특수 토큰에 인덱스를 할당합니다.
    special_tokens={token: len(tokenizer_model) + i for i, token in enumerate(special_tokens)},
)

# "hello world!"를 인코딩하고 토큰을 문자열로 디코딩합니다.
tokenizer.decode(tokenizer.encode("hello world!"))

#### 결과 ####
hello world!
#### 결과 ####


우리의 인코더 함수가 정확히 작동하는지 확인하기 위해 "Hello World"를 전달합니다. 먼저 텍스트를 숫자 값으로 변환하여 인코딩하고, 이후 다시 텍스트로 디코딩하여 "hello world!"로 결과를 얻습니다. 이것은 함수가 올바르게 작동함을 확인합니다. 이제 입력 텍스트를 토큰화해 봅시다.


# 입력 프롬프트
prompt = "the answer to the ultimate question of life, the universe, and everything is "

# 토크나이저를 사용하여 프롬프트를 인코딩하고 특수 토큰(128000)을 앞에 추가합니다.
tokens = [128000] + tokenizer.encode(prompt)

print(tokens)  # 인코딩된 토큰을 출력합니다.

# 토큰 목록을 PyTorch 텐서로 변환합니다.
tokens = torch.tensor(tokens)

# 각 토큰을 해당하는 문자열로 다시 디코딩합니다.
prompt_split_as_tokens = [tokenizer.decode([token.item()]) for token in tokens]

print(prompt_split_as_tokens)  # 디코딩된 토큰을 출력합니다.


#### 결과 ####
[128000, 1820, 4320, 311, ... ]
['<|begin_of_text|>', 'the', ' answer', ' to', ... ]
#### 결과 ####


입력 텍스트 "the answer to the ultimate question of life, the universe, and everything is "를 특수 토큰으로 시작하여 인코딩하였습니다.


<div class="content-ad"></div>

# 각 토큰에 대한 임베딩 생성

만약 우리의 입력 벡터의 길이를 확인한다면:

```js
# 입력 벡터의 차원을 확인합니다.
len(tokens)

#### 출력 ####
17
#### 출력 ####
```

```js
# llama-3 아키텍처에서 임베딩 벡터의 차원을 확인합니다.
print(dim)

#### 출력 ####
4096
#### 출력 ####
```

<div class="content-ad"></div>

우리의 입력 벡터는 현재 (17x1) 차원을 가지고 있습니다. 각 토큰화된 단어에 대한 임베딩으로 변환해야 합니다. 이는 (17x1) 토큰들이 각각 길이가 4096인 임베딩으로 변환될 것을 의미합니다.


# 어휘 크기와 임베딩 차원을 지정하여 임베딩 레이어 정의
embedding_layer = torch.nn.Embedding(vocab_size, dim)

# 사전 훈련된 토큰 임베딩을 임베딩 레이어에 복사
embedding_layer.weight.data.copy_(model["tok_embeddings.weight"])

# 주어진 토큰들에 대한 토큰 임베딩을 가져와서 torch.bfloat16 형식으로 변환
token_embeddings_unnormalized = embedding_layer(tokens).to(torch.bfloat16)

# 결과 토큰 임베딩의 형태 출력
token_embeddings_unnormalized.shape


#### 결과 ####
torch.Size([17, 4096])
#### 결과 ####


이러한 임베딩은 정규화되지 않았으며, 이를 정규화하지 않으면 심각한 영향을 미칠 수 있습니다. 다음 섹션에서는 입력 벡터에 정규화를 수행할 것입니다.

# RMSNorm을 사용한 정규화

<div class="content-ad"></div>

이전에 RMSNorm에 본 것과 동일한 공식을 사용하여 입력 벡터를 정규화할 것입니다.

![image](/assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_4.png)

```js
# RMSNorm 계산
def rms_norm(tensor, norm_weights):

    # 마지막 차원을 따라 텐서 값의 제곱의 평균 계산
    squared_mean = tensor.pow(2).mean(-1, keepdim=True)
    
    # 0으로 나누는 것을 피하기 위해 작은 값 추가
    normalized = torch.rsqrt(squared_mean + norm_eps)
    
    # 정규화된 텐서에 제공된 정규화 가중치를 곱함
    return (tensor * normalized) * norm_weights
```

우리는 미정규화된 임베딩을 정규화하기 위해 레이어_0에서 어텐션 가중치를 사용할 것입니다. 레이어_0을 사용하는 이유는 LLaMA-3 트랜스포머 아키텍처의 첫 번째 레이어를 만들고 있기 때문입니다.

<div class="content-ad"></div>


# RMS normalization 및 제공된 정규화 가중치 사용
token_embeddings = rms_norm(token_embeddings_unnormalized, 
                            model["layers.0.attention_norm.weight"])

# 결과 토큰 임베딩의 형태 출력
token_embeddings.shape


#### 결과 ####
torch.Size([17, 4096])
#### 결과 ####


이미 알고 계실 수도 있겠지만, 차원이 변하지 않는 이유는 벡터만을 정규화하기 때문입니다.

# 어텐션 헤드 (쿼리, 키, 값)

먼저, 모델로부터 쿼리, 키, 값 및 출력 벡터를 로드해 보겠습니다.


<div class="content-ad"></div>


이 무게를 측정한 결과 모델의 쿼리 무게 모양은 torch.Size([4096, 4096])입니다. 키 무게 모양은 torch.Size([1024, 4096])이고, 값 무게 모양은 torch.Size([1024, 4096])이며, 출력 무게 모양은 torch.Size([4096, 4096])입니다.

무게의 차원을 보면 우리가 다운로드한 모델의 무게는 각 헤드마다가 아니라 병렬 접근/훈련을 통해 여러 어텐션 헤드를 위한 것임을 나타냅니다. 그러나 이러한 행렬을 풀어서 단일 헤드에 대해서만 사용할 수 있습니다.

첫 번째 어텐션 레이어의 쿼리 무게를 검색한 뒤에 각 헤드당 차원을 계산합니다. 그런 다음 쿼리 무게를 각 헤드로 분리하기 위해 재구성해야 합니다.

여기서 32는 Llama-3의 어텐션 헤드 수를 나타내며, 128은 쿼리 벡터의 크기, 4096은 토큰 임베딩의 크기입니다.


<div class="content-ad"></div>

첫 번째 레이어의 첫 번째 헤드의 쿼리 가중치 행렬에 접근할 수 있어요:

```js
# 어텐션의 첫 번째 헤드의 첫 번째 레이어에서 쿼리 가중치 추출
q_layer0_head0 = q_layer0[0]

# 첫 번째 헤드의 추출된 쿼리 가중치 텐서의 모양 출력
q_layer0_head0.shape

#### 출력 ####
torch.Size([128, 4096])
#### 출력 ####
```

각 토큰의 쿼리 벡터를 찾으려면, 쿼리 가중치를 토큰 임베딩과 곱해야 해요.

```js
# 행렬 곱셈: 첫 번째 헤드의 쿼리 가중치와 토큰 임베딩의 전치행렬
q_per_token = torch.matmul(token_embeddings, q_layer0_head0.T)

# 결과 텐서 모양: 토큰 당 쿼리
q_per_token.shape

#### 출력 ####
torch.Size([17, 128])
#### 출력 ####
```

<div class="content-ad"></div>

쿼리 벡터들은 질문 내에서 자신의 위치를 본질적으로 모르기 때문에 RoPE를 사용하여 위치를 알립니다.

## RoPE 구현하기

쿼리 벡터들을 쌍으로 분할한 후 각 쌍에 대해 회전 각도 이동을 적용합니다.

```js
# 쿼리를 토큰 당 부동 소수점으로 변환하고 쌍으로 분할합니다
q_per_token_split_into_pairs = q_per_token.float().view(q_per_token.shape[0], -1, 2)

# 쌍으로 분할한 후 결과 텐서의 모양 출력
q_per_token_split_into_pairs.shape


#### 결과 ####
torch.Size([17, 64, 2])
#### 결과 ####
```

<div class="content-ad"></div>

[17x64x2] 사이즈의 벡터가 있습니다. 이 벡터는 128 길이의 쿼리를 각 프롬프트 토큰에 대해 64개의 쌍으로 분할한 것을 나타냅니다. 각 쌍은 해당 토큰의 위치에 따라 m*theta만큼 회전될 것입니다. 여기서 m은 회전시킬 쿼리를 나타내는 토큰의 위치입니다.

벡터를 회전시키기 위해 복소수의 점곱을 사용할 것입니다.

```js
# 0부터 1까지의 값을 64개로 분할하여 생성
zero_to_one_split_into_64_parts = torch.tensor(range(64)) / 64

# 결과 텐서 출력
zero_to_one_split_into_64_parts

#### 출력 ####
tensor([0.0000, 0.0156, 0.0312, 0.0469, 0.0625, 0.0781, 0.0938, 0.1094, 0.1250,
        0.1406, 0.1562, 0.1719, 0.1875, 0.2031, 0.2188, 0.2344, 0.2500, 0.2656,
        0.2812, 0.2969, 0.3125, 0.3281, 0.3438, 0.3594, 0.3750, 0.3906, 0.4062,
        0.4219, 0.4375, 0.4531, 0.4688, 0.4844, 0.5000, 0.5156, 0.5312, 0.5469,
        0.5625, 0.5781, 0.5938, 0.6094, 0.6250, 0.6406, 0.6562, 0.6719, 0.6875,
        0.7031, 0.7188, 0.7344, 0.7500, 0.7656, 0.7812, 0.7969, 0.8125, 0.8281,
        0.8438, 0.8594, 0.8750, 0.8906, 0.9062, 0.9219, 0.9375, 0.9531, 0.9688,
        0.9844])
#### 출력 ####
```

분할 단계 이후, 이를 계산하여 주파수를 구할 것입니다.

<div class="content-ad"></div>


# 파워 연산을 사용하여 주파수 계산하기
freqs = 1.0 / (rope_theta ** zero_to_one_split_into_64_parts)

# 계산된 주파수 출력
freqs

#### OUTPUT ####
tensor([1.0000e+00, 8.1462e-01, 6.6360e-01, 5.4058e-01, 4.4037e-01, 3.5873e-01,
        2.9223e-01, 2.3805e-01, 1.9392e-01, 1.5797e-01, 1.2869e-01, 1.0483e-01,
        8.5397e-02, 6.9566e-02, 5.6670e-02, 4.6164e-02, 3.7606e-02, 3.0635e-02,
        2.4955e-02, 2.0329e-02, 1.6560e-02, 1.3490e-02, 1.0990e-02, 8.9523e-03,
        7.2927e-03, 5.9407e-03, 4.8394e-03, 3.9423e-03, 3.2114e-03, 2.6161e-03,
        2.1311e-03, 1.7360e-03, 1.4142e-03, 1.1520e-03, 9.3847e-04, 7.6450e-04,
        6.2277e-04, 5.0732e-04, 4.1327e-04, 3.3666e-04, 2.7425e-04, 2.2341e-04,
        1.8199e-04, 1.4825e-04, 1.2077e-04, 9.8381e-05, 8.0143e-05, 6.5286e-05,
        5.3183e-05, 4.3324e-05, 3.5292e-05, 2.8750e-05, 2.3420e-05, 1.9078e-05,
        1.5542e-05, 1.2660e-05, 1.0313e-05, 8.4015e-06, 6.8440e-06, 5.5752e-06,
        4.5417e-06, 3.6997e-06, 3.0139e-06, 2.4551e-06])
#### OUTPUT ####


이제 각 토큰의 쿼리 요소에 복소수를 할당하고 위치에 따라 회전시키기 위해 닷 프로덕트를 사용합니다.


# 토큰 당 쿼리를 복소수로 변환
q_per_token_as_complex_numbers = torch.view_as_complex(q_per_token_split_into_pairs)

q_per_token_as_complex_numbers.shape
# 출력: torch.Size([17, 64])

# 각 토큰의 주파수 계산하기 - arange(17)과 freqs의 외적 사용
freqs_for_each_token = torch.outer(torch.arange(17), freqs)

# 극좌표를 사용하여 freqs_for_each_token에서 복소수 계산
freqs_cis = torch.polar(torch.ones_like(freqs_for_each_token), freqs_for_each_token)

# 주파수에 따라 복소수 회전
q_per_token_as_complex_numbers_rotated = q_per_token_as_complex_numbers * freqs_cis

q_per_token_as_complex_numbers_rotated.shape
# 출력: torch.Size([17, 64])


회전된 벡터를 얻은 후, 복소수를 다시 실수로 볼 때 원래 쿼리로 돌아갈 수 있습니다.

<div class="content-ad"></div>


# 회전된 복소수를 다시 실수로 변환
q_per_token_split_into_pairs_rotated = torch.view_as_real(q_per_token_as_complex_numbers_rotated)

# 결과 텐서의 모양 출력
q_per_token_split_into_pairs_rotated.shape

#### 출력 ####
torch.Size([17, 64, 2])
#### 출력 ####


이제 회전된 페어가 병합되어 새로운 쿼리 벡터(회전된 쿼리 벡터)를 만들었습니다. 이 벡터의 모양은 [17x128]이며, 여기서 17은 토큰 수이고 128은 쿼리 벡터의 차원입니다.


# 회전된 토큰 쿼리를 원래 모양에 맞게 다시 변형
q_per_token_rotated = q_per_token_split_into_pairs_rotated.view(q_per_token.shape)

# 결과 텐서의 모양 출력
q_per_token_rotated.shape

#### 출력 ####
torch.Size([17, 128])
#### 출력 ####


키에 대해서도 프로세스는 유사하지만, 키 벡터가 128차원임을 염두에 두세요. 쿼리와 비슷하게 키는 1/4분의 가중치 수를 가지며, 연산을 최소화하기 위해 한 번에 4개의 헤드에 공유됩니다. 또한 키 또한 위치 정보를 포함하여 회전됩니다.

<div class="content-ad"></div>


# 모델의 첫 번째 레이어에서 어텐션 메커니즘의 키에 대한 가중치 텐서를 추출합니다.
k_layer0 = model["layers.0.attention.wk.weight"]

# 어텐션의 첫 번째 레이어에 대한 키 가중치를 헤드별로 분리하려면 reshape합니다.
k_layer0 = k_layer0.view(n_kv_heads, k_layer0.shape[0] // n_kv_heads, dim)

# reshape된 키 가중치 텐서의 형태를 출력합니다.
k_layer0.shape  # 출력: torch.Size([8, 128, 4096])

# 어텐션의 첫 번째 레이어의 첫 번째 헤드에 대한 키 가중치를 추출합니다.
k_layer0_head0 = k_layer0[0]

# 첫 번째 헤드에 대한 추출된 키 가중치 텐서의 형태를 출력합니다.
k_layer0_head0.shape  # 출력: torch.Size([128, 4096])

# 토큰 임베딩과 키 가중치 간의 행렬 곱으로 토큰 당 키를 계산합니다.
k_per_token = torch.matmul(token_embeddings, k_layer0_head0.T)

# 각 토큰에 대한 키를 나타내는 결과 텐서의 형태를 출력합니다.
k_per_token.shape  # 출력: torch.Size([17, 128])

# 키를 쌍으로 분할하고 float로 변환합니다.
k_per_token_split_into_pairs = k_per_token.float().view(k_per_token.shape[0], -1, 2)

# 쌍으로 분할 후 텐서의 형태를 출력합니다.
k_per_token_split_into_pairs.shape  # 출력: torch.Size([17, 64, 2])

# 키를 복소수로 변환합니다.
k_per_token_as_complex_numbers = torch.view_as_complex(k_per_token_split_into_pairs)

# 키를 복소수로 나타내는 결과 텐서의 형태를 출력합니다.
k_per_token_as_complex_numbers.shape  # 출력: torch.Size([17, 64])

# 주파수에 따라 복소수로 된 키를 회전합니다.
k_per_token_split_into_pairs_rotated = torch.view_as_real(k_per_token_as_complex_numbers * freqs_cis)

# 회전된 복소수로 된 키의 형태를 출력합니다.
k_per_token_split_into_pairs_rotated.shape  # 출력: torch.Size([17, 64, 2])

# 회전된 키를 원래의 형태에 맞게 재구성합니다.
k_per_token_rotated = k_per_token_split_into_pairs_rotated.view(k_per_token.shape)

# 회전된 키의 형태를 출력합니다.
k_per_token_rotated.shape  # 출력: torch.Size([17, 128])


이제 모든 토큰에 대한 회전된 쿼리와 키를 얻었는데, 각각의 크기는 [17x128]입니다.

# Self Attention 구현

쿼리와 키 행렬을 곱하면 각 토큰을 다른 토큰에 매핑하는 점수가 생성됩니다. 이 점수는 각 토큰의 쿼리와 키 간의 관계를 나타냅니다.

<div class="content-ad"></div>


# 토큰별 쿼리-키 내적 계산
qk_per_token = torch.matmul(q_per_token_rotated, k_per_token_rotated.T) / (head_dim) ** 0.5

# 쿼리-키 내적 결과 텐서의 형태 출력
qk_per_token.shape


#### 결과 ####
torch.Size([17, 17])
#### 결과 ####


[17x17] 형태는 어텐션 점수를 나타냅니다(qk_per_token). 여기서 17은 프롬프트에 있는 토큰 수를 나타냅니다.

쿼리-키 점수를 마스킹해야 합니다. 훈련 중에는 미래 토큰 쿼리-키 점수를 마스킹해야 합니다. 이는 우리가 과거 토큰을 사용하여 토큰을 예측하는 것만을 학습하기 때문입니다. 따라서 추론 중에는 미래 토큰을 제로로 설정합니다.


# 음수 무한대 값으로 채워진 마스크 텐서 생성
mask = torch.full((len(tokens), len(tokens)), float("-inf"), device=tokens.device)

# 마스크 텐서의 상삼각부분을 음수 무한대로 설정
mask = torch.triu(mask, diagonal=1)

# 결과 마스크 텐서 출력
mask


#### 결과 ####
tensor([[0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., 0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., 0., 0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., 0., 0., 0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., 0., 0., 0., 0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., 0., 0., 0., 0., 0., -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf, -inf],
        [0., 0., 0., 0., 0., 0., 0., 0., -inf, -inf, -

<div class="content-ad"></div>

이제 토큰 벡터마다 쿼리-키에 마스크를 적용해야 합니다. 추가로 상위에 softmax를 적용하여 출력 점수를 확률로 변환하고 싶습니다. 이는 모델 어휘에서 가장 가능성 있는 토큰 또는 토큰 순서를 선택하는 데 도움이 되며, 모델의 예측이 언어 생성 및 분류와 같은 작업에 더 이해하기 쉽고 적합하게 만듭니다.

```python
# Add the mask to the query-key dot products per token
qk_per_token_after_masking = qk_per_token + mask

# Apply softmax along the second dimension after masking
qk_per_token_after_masking_after_softmax = torch.nn.functional.softmax(qk_per_token_after_masking, dim=1).to(torch.bfloat16)
```

키처럼, 셀프 어텐션 부분의 끝을 표시하는 값 행렬에 대해서도, 값 가중치는 모든 4개의 어텐션 헤드에 대해 공유됩니다. 결과적으로, 값 가중치 행렬의 형태는 [8x128x4096]입니다.

```python
# Retrieve the value weight for the first layer of attention
v_layer0 = model["layers.0.attention.wv.weight"]

# Reshape value weight for the first layer of attention to separate heads
v_layer0 = v_layer0.view(n_kv_heads, v_layer0.shape[0] // n_kv_heads, dim)

# Print the shape of the reshaped value weight tensor
v_layer0.shape

#### OUTPUT ####
torch.Size([8, 128, 4096])
#### OUTPUT ####
```

<div class="content-ad"></div>

다음은 첫 번째 레이어와 첫 번째 헤드에 대한 값 행렬을 얻는 방법과 비슷한 방식으로 쿼리 매트릭스와 키 매트릭스에 대한 값 행렬을 얻을 수 있습니다.

```js
# 주의의 첫 번째 레이어와 첫 번째 헤드에 대한 값 가중치 추출
v_layer0_head0 = v_layer0[0]

# 첫 번째 헤드의 추출된 값 가중치 텐서의 모양 출력
v_layer0_head0.shape


#### 출력 ####
torch.Size([128, 4096])
#### 출력 ####
```

값 가중치를 사용하여 각 토큰에 대한 주의 값을 계산하면 토큰당 값 행렬이 생성되며 이는 [17x128] 크기의 행렬을 만듭니다. 여기서 17은 프롬프트에 포함된 토큰 수를 나타내고, 128은 각 토큰에 대한 값을 나타내는 벡터의 차원을 의미합니다.

```js
# 각 토큰의 값 계산을 위한 행렬 곱셈
v_per_token = torch.matmul(token_embeddings, v_layer0_head0.T)

# 토큰당 값을 나타내는 결과 텐서의 모양 출력
v_per_token.shape


#### 출력 ####
torch.Size([17, 128])
#### 출력 ####
```

<div class="content-ad"></div>

우리는 다음과 같이 수행하여 결과 주목 행렬을 얻을 수 있습니다:

```js
# 행렬 곱셈으로 QKV 주목 계산
qkv_attention = torch.matmul(qk_per_token_after_masking_after_softmax, v_per_token)

# 결과 텐서의 모양 출력
qkv_attention.shape


#### 결과 ####
torch.Size([17, 128])
#### 결과 ####
```

이제 첫 번째 레이어와 첫 번째 헤드 또는 다른 말로 self attention에 대한 주목 값이 있습니다.

# Multi-Head Attention 구현하기

<div class="content-ad"></div>

위의 계산을 반복 수행하는 루프를 실행하면 첫 번째 레이어의 각 헤드에 대한 동일한 계산이 수행됩니다.

```js
# 각 헤드의 QKV 어텐션을 리스트에 저장합니다.
qkv_attention_store = []

# 각 헤드를 반복합니다.
for head in range(n_heads):
    # 현재 헤드용 쿼리, 키 및 값 가중치를 추출합니다.
    q_layer0_head = q_layer0[head]
    k_layer0_head = k_layer0[head//4]  # 키 가중치는 4개의 헤드 간에 공유됩니다.
    v_layer0_head = v_layer0[head//4]  # 값 가중치는 4개의 헤드 간에 공유됩니다.
    
    # 매트릭스 곱셈을 통해 각 토큰의 쿼리 계산
    q_per_token = torch.matmul(token_embeddings, q_layer0_head.T)
    
    # 매트릭스 곱셈을 통해 각 토큰의 키 계산
    k_per_token = torch.matmul(token_embeddings, k_layer0_head.T)
    
    # 매트릭스 곱셈을 통해 각 토큰의 값 계산
    v_per_token = torch.matmul(token_embeddings, v_layer0_head.T)
    
    # 쿼리를 쌍으로 분할하고 회전합니다.
    q_per_token_split_into_pairs = q_per_token.float().view(q_per_token.shape[0], -1, 2)
    q_per_token_as_complex_numbers = torch.view_as_complex(q_per_token_split_into_pairs)
    q_per_token_split_into_pairs_rotated = torch.view_as_real(q_per_token_as_complex_numbers * freqs_cis[:len(tokens)])
    q_per_token_rotated = q_per_token_split_into_pairs_rotated.view(q_per_token.shape)
    
    # 키를 쌍으로 분할하고 회전합니다.
    k_per_token_split_into_pairs = k_per_token.float().view(k_per_token.shape[0], -1, 2)
    k_per_token_as_complex_numbers = torch.view_as_complex(k_per_token_split_into_pairs)
    k_per_token_split_into_pairs_rotated = torch.view_as_real(k_per_token_as_complex_numbers * freqs_cis[:len(tokens)])
    k_per_token_rotated = k_per_token_split_into_pairs_rotated.view(k_per_token.shape)
    
    # 각 토큰의 쿼리-키 닷 프로덕트 계산
    qk_per_token = torch.matmul(q_per_token_rotated, k_per_token_rotated.T) / (128) ** 0.5
    
    # 음수 무한대 값으로 채워진 마스크 텐서 생성
    mask = torch.full((len(tokens), len(tokens)), float("-inf"), device=tokens.device)
    # 마스크 텐서의 상삼각 부분을 음수 무한으로 설정
    mask = torch.triu(mask, diagonal=1)
    # 쿼리-키 닷 프로덕트에 마스크 추가
    qk_per_token_after_masking = qk_per_token + mask
    
    # 마스킹 후 두 번째 차원을 따라 softmax 적용
    qk_per_token_after_masking_after_softmax = torch.nn.functional.softmax(qk_per_token_after_masking, dim=1).to(torch.bfloat16)
    
    # 매트릭스 곱셈을 통해 QKV 어텐션 계산
    qkv_attention = torch.matmul(qk_per_token_after_masking_after_softmax, v_per_token)
    
    # 현재 헤드의 QKV 어텐션을 저장
    qkv_attention_store.append(qkv_attention)

# 저장된 QKV 어텐션 개수 출력
len(qkv_attention_store)


#### 출력 ####
32
#### 출력 ####
```

이제 첫 번째 레이어의 32개 헤드에 대한 QKV 어텐션 행렬을 얻었으므로 모든 어텐션 점수를 하나의 큰 행렬 [17x4096]로 병합합니다.

```js
# 마지막 차원을 따라 모든 헤드의 QKV 어텐션을 이어붙입니다.
stacked_qkv_attention = torch.cat(qkv_attention_store, dim=-1)

# 결과 텐서의 모양 출력
stacked_qkv_attention.shape


#### 출력 ####
torch.Size([17, 4096])
#### 출력 ####
```

<div class="content-ad"></div>

레이어 0 주의의 마지막 단계 중 하나는 가중치 행렬과 쌓인 QKV 행렬을 곱하는 것입니다.

```js
# 출력 가중치로 행렬 곱셈하여 임베딩 델타 계산
embedding_delta = torch.matmul(stacked_qkv_attention, model["layers.0.attention.wo.weight"].T)

# 결과 텐서의 형태 출력
embedding_delta.shape


#### 출력 결과 ####
torch.Size([17, 4096])
#### 출력 결과 ####
```

이제 주의 후 임베딩 값의 변화가 있습니다. 이 값은 원래 토큰 임베딩에 추가되어야 합니다.

```js
# 임베딩 델타를 정규화되지 않은 토큰 임베딩에 추가하여 최종 임베딩 얻기
embedding_after_edit = token_embeddings_unnormalized + embedding_delta

# 결과 텐서의 형태 출력
embedding_after_edit.shape


#### 출력 결과 ####
torch.Size([17, 4096])
#### 출력 결과 ####
```

<div class="content-ad"></div>

편집된 임베딩의 변경 사항이 정규화되었고, 이후 피드포워드 신경망을 통해 실행됩니다.


# 제공된 가중치를 사용하여 제곱 평균 정규화를 수행한 편집된 임베딩
embedding_after_edit_normalized = rms_norm(embedding_after_edit, model["layers.0.ffn_norm.weight"])

# 정규화된 임베딩의 결과 형태 출력
embedding_after_edit_normalized.shape

#### 출력 결과 ####
torch.Size([17, 4096])
#### 출력 결과 ####


# SwiGLU 활성화 함수 구현

이전 섹션에서 SwiGLU 활성화 함수에 익숙해졌으므로, 여기서 이전에 학습한 방정식을 적용하겠습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-07-BuildingLLaMA3FromScratchwithPython_5.png)

```js
# 피드포워드 레이어의 가중치 확인
w1 = model["layers.0.feed_forward.w1.weight"]
w2 = model["layers.0.feed_forward.w2.weight"]
w3 = model["layers.0.feed_forward.w3.weight"]

# 피드포워드 레이어 연산 수행
output_after_feedforward = torch.matmul(torch.functional.F.silu(torch.matmul(embedding_after_edit_normalized, w1.T)) * torch.matmul(embedding_after_edit_normalized, w3.T), w2.T)

# 피드포워드 이후의 결과 텐서 모양 출력
output_after_feedforward.shape


#### 출력 결과 ####
torch.Size([17, 4096])
#### 출력 결과 ####
```

# 모든 것을 합치기

이제 모든 준비가 끝났으니, 코드를 통합하여 31개의 새로운 레이어를 생성해야 합니다.


<div class="content-ad"></div>


# 최종 임베딩 초기화 단계는 정규화되지 않은 토큰 임베딩으로 이루어져 있어요
final_embedding = token_embeddings_unnormalized

# 각 레이어를 순회하면서
for layer in range(n_layers):
    # 각 헤드의 QKV 어텐션을 저장할 리스트 초기화
    qkv_attention_store = []
    
    # root mean square 정규화를 사용하여 최종 임베딩을 정규화하고, 현재 레이어의 가중치를 활용해요
    layer_embedding_norm = rms_norm(final_embedding, model[f"layers.{layer}.attention_norm.weight"])
    
    # 현재 레이어의 어텐션 메커니즘에 필요한 쿼리, 키, 값 및 출력 가중치를 가져와요
    q_layer = model[f"layers.{layer}.attention.wq.weight"]
    q_layer = q_layer.view(n_heads, q_layer.shape[0] // n_heads, dim)
    k_layer = model[f"layers.{layer}.attention.wk.weight"]
    k_layer = k_layer.view(n_kv_heads, k_layer.shape[0] // n_kv_heads, dim)
    v_layer = model[f"layers.{layer}.attention.wv.weight"]
    v_layer = v_layer.view(n_kv_heads, v_layer.shape[0] // n_kv_heads, dim)
    w_layer = model[f"layers.{layer}.attention.wo.weight"]
    
    # 각 헤드를 순회하면서
    for head in range(n_heads):
        # 현재 헤드에 대한 쿼리, 키, 밸류 가중치를 추출해요
        q_layer_head = q_layer[head]
        k_layer_head = k_layer[head//4]  # 키 가중치는 4개의 헤드 간에 공유돼요
        v_layer_head = v_layer[head//4]  # 밸류 가중치는 4개의 헤드 간에 공유돼요
        
        # 행렬 곱셈을 통해 각 토큰에 대한 쿼리 계산
        q_per_token = torch.matmul(layer_embedding_norm, q_layer_head.T)
        
        # 행렬 곱셈을 통해 각 토큰에 대한 키 계산
        k_per_token = torch.matmul(layer_embedding_norm, k_layer_head.T)
        
        # 행렬 곱셈을 통해 각 토큰에 대한 밸류 계산
        v_per_token = torch.matmul(layer_embedding_norm, v_layer_head.T)
        
        ...

# 결과 생성

이제 우리는 다음 토큰을 맞추기 위한 모델의 최종 임베딩을 갖게 되었어요. 
그 형태는 일반 토큰 임베딩과 같아요, [17x4096], 17개의 토큰과 4096의 임베딩 차원을 갖고 있어요.

final_embedding을 제공된 가중치를 사용해 정규화해볼게요.
final_embedding = rms_norm(final_embedding, model["norm.weight"])

# 결과로 나온 정규화된 최종 임베딩의 형태 출력
print(final_embedding.shape)

#### 출력 ####
torch.Size([17, 4096])
#### 출력 ####



<div class="content-ad"></div>

이제 임베딩을 토큰 값으로 해석할 수 있어요.

```js
# 출력 가중치 텐서의 형태를 출력합니다
model["output.weight"].shape


#### 출력 ####
torch.Size([128256, 4096])
#### 출력 ####
```

다음 값을 예측하기 위해, 마지막 토큰의 임베딩을 사용해요.

```js
# 최종 임베딩과 출력 가중치 텐서의 전치행렬 간에 행렬 곱셈을 통해 로짓을 계산합니다
logits = torch.matmul(final_embedding[-1], model["output.weight"].T)

# 결과 로짓 텐서의 형태를 출력합니다
logits.shape


#### 출력 ####
torch.Size([128256])
#### 출력 ####
```

<div class="content-ad"></div>


# 마지막 차원에서 최댓값의 인덱스를 찾아 다음 토큰을 결정하세요
next_token = torch.argmax(logits, dim=-1)

# 다음 토큰의 인덱스 출력
next_token

#### 결과 ####
tensor(2983)
#### 결과 ####


토큰 ID에서 생성된 텍스트를 얻으려면


# 토크나이저를 사용하여 다음 토큰의 인덱스를 디코딩합니다
tokenizer.decode([next_token.item()])


#### 결과 ####
42
#### 결과 ####


그래서 우리의 입력은 "인생, 우주, 그리고 모든 것에 대한 궁극적인 질문의 답은 "이고, 그 결과는 "42"입니다. 이게 정답이죠.


<div class="content-ad"></div>

마법사 여러분! 이제 코드 라인을 본 영어 텍스트로 교체해보세요. 그 외 코드는 그대로 유지됩니다!

```js
# 입력 프롬프트
prompt = "당신의 입력"

# 17번째 숫자를 입력 문장의 토큰 총 수로 교체하세요
# 토큰 총 수는 len(tokens)을 사용해 확인할 수 있어요
freqs_for_each_token = torch.outer(torch.arange(17), freqs)
```

## 이 블로그를 통해 즐거운 시간을 보내셨고 새로운 것을 배우셨길 바래요! 🌟