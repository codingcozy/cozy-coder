---
title: "Mistral-7B 미세 조정 문학과 AI의 여정 "
description: ""
coverImage: "/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_0.png"
date: 2024-07-09 23:37
ogImage:
  url: /assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_0.png
tag: Tech
originalTitle: "Fine-Tuning Mistral-7B: A Journey Through Literature and AI"
link: "https://medium.com/data-bistrot/fine-tuning-mistral-7b-a-journey-through-literature-and-ai-3322e68b65c3"
isUpdated: true
---

## 문장 데이터셋에서 Mistral 7B를 섬세하게 튜닝하기

![Fine-Tuning Mistral 7B](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_0.png)

안녕하세요! 이번 글에서는 Mistral-7B-v0.1 대형 언어 모델을 섬세하게 튜닝하는 혁신적인 여정을 안내합니다. 이 모델은 70억 개의 파라미터를 자랑하는 생성 텍스트 기술의 최전선에 위치하고 있습니다.

이 노력은 12,000개가 넘는 명언을 포함한 데이터셋을 바탕으로 이루어졌습니다. 이 데이터는 다양한 주제와 스타일에 걸쳐 현명함의 보물창고로, Mistral-7B에게 언어적, 주제적으로 풍부한 사상과 문학의 종합적인 시각을 제공합니다. 함께 즐겁게 여정을 떠나봐요! 🌟

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

저희의 여정은 Mistral-7B를 fine-tune 하는 과정에서 필수적인 단계들을 다룰 것입니다. 데이터 전처리부터 교육 파이프라인 설정, 마지막으로는 실제 교육 과정까지 진행할 예정입니다. 우리는 Mistral-7B의 능력을 높여 새로운 명언을 생성할 수 있도록 하고자 합니다. 이 명언들은 이전의 문학적 선배들의 지혜, 유머, 그리고 깊이를 반영할 것입니다.

## Mistral-7B 아키텍처

Fine-tuning의 기술적 측면에 들어가기 전에, Mistral-7B LLM이 현대 자연어 처리(NLP) 및 생성적 AI의 새로운 지평을 열어 놓은 이유에 대해 살펴보겠습니다.

“Mistral 7B”는 70억 개의 파라미터로 이루어진 언어 모델로, 우수한 성능과 효율성을 위해 설계되었습니다. 이 모델은 모든 평가된 벤치마크에서 최고 수준의 13억 모델 (Llama 2)과 수학 및 코드 생성 분야에서 최고 수준의 34억 모델 (Llama 1)보다 뛰어난 성과를 보여줍니다. 주요 혁신 요소로는 빠른 추론을 위한 그룹화된 쿼리 어텐션 (GQA) 및 임의 길이의 순서를 효과적으로 관리하기 위한 슬라이딩 윈도우 어텐션 (SWA) 등이 있습니다. 이를 통해 추론 비용을 줄일 수 있습니다.

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

이 유즈 케이스에서 사용할 모델은 지시 사항을 따르고 Llama 2를 능가하는 성능을 발휘할 만큼 미세 조정되었습니다. 이 모델은 Apache 2.0 라이선스 하에 공개되었습니다.

## 아키텍처 세부정보

다음은 Mistral 7B에 대한 과학 논문에서 찾을 수 있는 아키텍처 세부정보에 대한 자세한 요약입니다:

- Mistral 7B는 시퀀스 데이터 처리에서 효과적인 것으로 널리 알려진 transformer 아키텍처에 기반을 두고 있습니다.
- Mistral 7B는 32개의 레이어, 32 attention 헤드, 8 키-밸류 헤드를 가지고 있습니다. 숨겨진 크기는 4096이고, 중간 크기는 14336입니다. 모델의 최대 위치 임베딩은 32768이며 어휘 크기는 32000입니다. 이 모델은 silu 활성화 함수와 bfloat16 데이터 유형을 사용합니다. 슬라이딩 윈도우는 4096이고, 로프 세타는 10000입니다.

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

이러한 가치들은 신비로운 것이 아닌, 모두 논문에서 설명되어 있습니다.

## 슬라이딩 윈도우 어텐션 (SWA):

SWA는 트랜스포머 모델에 전통적으로 연결된 계산 복잡성을 관리하기 위해 소개되었습니다, 특히 긴 시퀀스에 대해서. 각 토큰이 이전 레이어에서 일정 수의 토큰에 주의를 기울일 수 있도록 허용함으로써, SWA는 어텐션 메커니즘의 제곱 복잡성을 더 관리하기 쉬운 형태로 줄입니다. 이 방법론은 전형적인 트랜스포머 모델이 처리하는 것보다 훨씬 더 긴 시퀀스를 처리하는 데 도움이 됩니다.

![이미지](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_1.png)

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

### **타로 전문가의 시선**

이 놀라운 접근 방식은 상당한 속도 향상과 계산 비용 절감을 가능하게 합니다.

**주목해야 할주제에 대한 추가 정보**

## 롤링 버퍼 캐시

메모리 사용량을 더욱 최적화하기 위해, Mistral 7B는 롤링 버퍼 캐시 전략을 채택합니다. 이 접근 방식은 창 크기와 동일한 고정 크기의 캐시를 사용하는 것을 포함합니다. 이 방법을 통해 시퀀스 길이가 증가하더라도 캐시 크기가 일정하게 유지됩니다. 이 방법은 모델 품질을 희생하지 않고 캐시 메모리 사용량을 8배로 줄이는 효과를 가져옵니다.

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

## 사전 채우기 및 청킹:

이 모델은 시퀀스 생성을 위해 사전 채우기 및 청킹 기술을 활용합니다. 여기서 캐시는 이미 알려진 시퀀스 부분(예: 프롬프트)으로 사전 채워져 계산을 줄입니다. 프롬프트가 크면 이를 더 작은 청크로 나누어 캐시가 청크 단위로 채워지게 됩니다. 이 과정은 효율적인 메모리 사용을 허용하고 긴 시퀀스 처리에 따른 계산 오버헤드를 줄입니다.

![이미지](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_3.png)

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

## 그룹화된 쿼리 어텐션 (GQA):

그룹화된 쿼리 어텐션 (GQA)은 대량의 데이터 시퀀스를 처리하는 데 전이하고 효과적인 트랜스포머의 성능을 향상시키기 위해 설계된 고급 어텐션 메커니즘입니다. 원래의 트랜스포머와 같은 모델에서의 전통적인 어텐션 메커니즘은 시퀀스 길이에 대해 제곱 복잡성을 가지기 때문에 장기 시퀀스에 대한 확장성 문제를 직면합니다. 이는 시퀀스 길이가 증가함에 따라 어테션 점수를 계산하는 데 필요한 계산 자원(메모리 및 처리 능력 모두)이 기하급수적으로 증가하기 때문에 장문 문서나 고해상도 이미지를 다루기가 어려워진다는 것을 의미합니다.

![이미지](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_4.png)

GQA는 쿼리를 그룹화하여 키와 값과 함께 어텐션 점수를 계산하기 전에 이 문제에 대처합니다. 이 접근 방식은 어텐션 메커니즘의 계산 복잡성을 시퀀스 길이에 대해 선형으로 줄여 제곱에서 선형으로 만들어줍니다. 특히 장기 시퀀스에 대해 훨씬 효율적으로 만들어주는데, 이는 효율적이고 효율적입니다.

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

These architectural advancements have positioned Mistral 7B as a highly efficient and effective model, capable of handling long sequences with reduced computational costs and improved inference speeds.

The Mistral 7B model has been further enhanced to better follow instructions, resulting in the creation of a variant called "Mistral 7B — Instruct." This enhancement process aimed to increase the model's generalization capabilities, especially in scenarios where adherence to specific instructions is crucial.

**Mistral 7B — Instruct Fine-tuning:**

![Image](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_5.png)

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

- **Mistral 7B에 대한 정밀 조정 과정** — 'Instruct'은 Hugging Face 레포지토리에서 공개된 지시 데이터셋을 활용했습니다. 이 방법은 Mistral 7B의 적응성을 강조하며, 전문 데이터나 복잡한 학습 기술 없이도 특화된 데이터셋에서의 정밀 조정을 통해 성능을 크게 향상시킬 수 있다는 가능성을 보여줍니다.
- **결과적으로 나타난 Mistral 7B — Instruct 모델**은 지시 따르기 능력을 평가하는 MT-Bench에서 유사한 크기의 다른 모델들보다 뛰어난 성능을 보였습니다. 또한 더 큰 모델들과도 유사한 수준의 성과를 보여줌으로써, 지시 중심의 정밀 조정의 효과를 명확히 보여주었습니다.

## **Mistral 7B와 Mistral 7B — Instruct 간의 차이:**

- **기본 모델 대 정밀 조정 모델:** Mistral 7B와 Mistral 7B — Instruct의 주된 차이점은 후자가 지시 데이터셋으로 정밀 조정되었다는 점입니다. Mistral 7B는 다양한 벤치마크에서 강력한 기본 성능을 보여주는 일반 목적의 언어 모델이지만, Mistral 7B — Instruct은 주어진 지시에 대한 정확한 준수를 요구하는 작업에 더 적합하도록 최적화되어 지시를 더 효과적으로 이해하고 실행할 수 있습니다.
- **성능 향상:** Mistral 7B — Instruct는 지시가 생성 과정을 이끌어야 하는 상황에서 기본 모델(그리고 유사한 다른 모델)보다 우수한 성능을 보입니다. 특히 MT-Bench에서 Mistral 7B — Instruct는 지시를 이해하고 대응하는 능력에서 상당한 향상을 나타냅니다.
- **일반화 능력:** 정밀 조정 과정은 Mistral 7B의 지시로부터 일반화하는 능력을 향상시켜 Mistral 7B — Instruct가 지시와 그 문맥을 섬세하게 이해하는 작업에 보다 능숙해지도록 돕습니다. 이는 챗봇과 같은 응용 프로그램에서 복잡하고 다양한 사용자 지시를 따르는 능력이 필수적인 상황에 매우 가치 있는 정밀 조정된 모델을 만들어냅니다.

**결론적으로, 정밀 조정 과정은 Mistral 7B의 지침 따르기 능력을 정제하는데 그치지 않고, 실세계 응용 프로그램에서 세밀한 이해와 실행이 필요한 경우에 그 유용성을 크게 높여줍니다. 이는 Mistral 7B와 같은 기본 모델의 능력을 향상시키는 데 정밀 조정의 힘을 보여줍니다.**

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

## 참고 및 추가 독해:

# 인용문에 LLM 미세 조정의 중요성

![이미지](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_6.png)

인용문은 인간 경험의 본질을 나타내며 지혜, 유머 및 통찰력을 간결하고 기억하기 쉬운 방식으로 담고 있습니다. 스크랩된 인용문 데이터셋을 이용해 Mistral-7B를 미세 조정함으로써, 나의 목표는 이 모델에게 흥미로운 문장을 생성할 뿐만 아니라 독자들에게 영감을 줄 뿐만 아니라 동기부여를 하며 생각할 요소를 제공할 수 있는 능력을 부여하는 것입니다.

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

이 놀라운 여정에 함께해요. 우리는 어제의 통찰력과 오늘의 기술적 발전을 결합할 거예요. 나의 목표는 과거의 유산과 AI의 미래 잠재력을 혼합한 독특한 모델을 구축하는 예를 제시하는 거에요. 이 글을 통해, 문학의 심오함과 AI의 혁신적 재능이 결합된, 창의력과 지적 성장을 약속하는 통합을 목격하게 할 거에요.

**My Fine-tuning of Mistral 7B**- Quotes Dataset에 대한 가르침

파인튜닝 과정을 탐험해봐요. 이 코드는 구글 코랩에서 무료 GPU를 이용하면 실행할 수 있어요.

라이브러리를 설치하고 import하세요.

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
!pip install transformers accelerate scipy ipywidgets bitsandbytes peft datasets trl -qU
!pip install torch>=1.10
!pip install huggingface-hub -qU
```

```python
# 필요한 라이브러리 불러오기
from datetime import datetime
import pandas as pd
import json
import os
import re
import time
import requests
from urllib.request import urlopen
from urllib.error import HTTPError
import bs4
import pandas as pd
from math import ceil
from google.colab import userdata
from datasets import load_dataset
```

```python
# 최대 열 너비를 100으로 변경
pd.set_option('display.max_colwidth', 1000)
```

## 데이터셋 불러오기

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

이 코드 스니펫은 CSV 파일을 pandas DataFrame으로 읽어들이고, 불필요한 열을 제거하고 결측치가 있는 행을 처리한 후에 DataFrame의 인덱스를 재설정합니다.

```python
file_name = '/content/best_quotes_en.csv'
df = pd.read_csv(file_name)
df.drop(columns=["is_english"], inplace=True)
df.dropna(inplace=True)
df.reset_index(inplace=True, drop=True)
df.head(5)
```

여기에서 인용구 파일의 버전을 다운로드할 수 있습니다:

[Kaggle Quote Files](https://www.kaggle.com/datasets/gianpieroandrenacci/best-quotes-dataset)

## 훈련 및 검증 데이터 분리

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

이 코드 스니펫은 DataFrame을 훈련 및 검증 세트로 분할하기 위해 지정된 훈련 세트 비율로 변수를 설정합니다.

```python
# 데이터 분할
train_perc = 0.8
len_df = len(df)
train_len = ceil(len_df * train_perc)
val_len = len_df - train_len

print(f"전체 샘플 수: {len_df}")
print(f"훈련 세트 크기: {train_len}")
print(f"검증 세트 크기: {val_len}")
```

제 경우:

```python
전체 샘플 수: 12344
훈련 세트 크기: 9876
검증 세트 크기: 2468
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

## 데이터프레임을 Hugging Face 데이터셋으로 변환하기

```python
from datasets import Dataset

# 데이터셋에서 DataFrame의 인덱스를 제외하고 가져옴
dataset = Dataset.from_pandas(df, preserve_index=False)

# 데이터셋의 기본 정보 및 열 유형 표시
print("데이터셋 구조 및 열 유형:")
print(dataset)

# 데이터셋의 처음 몇 개 항목 미리보기
print("\n처음 몇 개 레코드 미리보기:")
print(dataset[:2])
```

```python
# 데이터셋을 학습 및 검증 세트로 분할
dataset_train = dataset.select(range(train_len))  # 첫 번째 부분을 학습 데이터로 선택
dataset_val = dataset.select(indices=range(train_len, len(dataset)))  # 나머지 부분을 검증 데이터로 선택

# 학습 및 검증 데이터셋에 대한 상세 정보 출력
print("학습 데이터셋 정보:")
print(dataset_train)
print("\n검증 데이터셋 정보:")
print(dataset_val)
```

아래 코드 조각은 다음 단계를 수행합니다:

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

- 데이터셋 클래스 불러오기: 먼저, 데이터셋 클래스는 Hugging Face의 데이터셋 라이브러리에 포함된 datasets 모듈에서 불러옵니다.
- 데이터셋으로 변환: 판다스 DataFrame(df)를 사용하여 Dataset.from_pandas(df, preserve_index=False) 메서드를 이용해 Hugging Face Dataset 객체로 변환합니다. preserve_index=False 인자는 DataFrame의 인덱스가 결과 데이터셋의 별도 열로 포함되지 않도록 합니다. 이는 데이터셋을 깔끔하고 데이터 내용에만 집중할 수 있도록 돕습니다.
- 데이터셋 구조 확인: 변환된 데이터셋의 구조와 포함된 데이터 유형을 이해하기 위해 print(dataset)을 통해 스키마를 출력합니다. 이는 데이터셋의 열과 해당 데이터 유형에 대한 통찰을 제공합니다.
- 데이터셋 내용 미리보기: 마지막으로 데이터가 어떻게 보이는지 구체적으로 파악하기 위해 print(dataset[:2])을 사용해 데이터셋의 처음 두 항목을 미리 볼 수 있습니다. 이는 데이터 변환을 확인하고 데이터셋의 초기 항목을 이해하는 데 도움이 됩니다.

이 과정은 주로 NLP 작업을 위해 Hugging Face 생태계와 함께 작업할 때 데이터를 모델링하기 위해 중요합니다.

# 함수: create_prompt

```js
def create_prompt(row, output_format='string', bos_token="<s>", eos_token="</s>"):
    """
    해당 입력 데이터를 사용하여 주제 카테고리에 관한 인용구를 생성하도록 서식이 지정된 프롬프트를 생성합니다. 문자열 또는 사전 출력 형식을 선택하는 옵션 포함.

    매개변수:
        row (dict): 프롬프트에 포함되어야 하는 'quote' 및 'tags'를 포함하는 딕셔너리.
        output_format (str, optional): 출력 형식; 'string' 또는 'dict'. 'string'으로 기본 설정됨.
        bos_token (str, optional): 시작 시퀀스 토큰. "<s>"로 기본 설정됨.
        eos_token (str, optional): 종료 시퀀스 토큰. "</s>"로 기본 설정됨.

    반환값:
        str 또는 dict: 지정된 형식으로 생성된 프롬프트.
    """
    system_message = "[INST]제공된 입력을 사용하여 주제 카테고리에 대한 인용구를 생성하세요[/INST]"
    response = row.get("quote", "")
    input_tags = row.get("tags", "")

    full_prompt = f"{bos_token}{system_message} ### Input: {input_tags} ### Response:{response}{eos_token}"

    if output_format == 'dict':
        return {"prompt": full_prompt}
    return full_prompt
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

**창조\_알림** 기능은 사용자의 입력 데이터의 내용과 맥락을 반영하여 언어 모델 생성 작업을 위한 프롬프트를 동적으로 구성합니다. 다양한 NLP 및 생성적 AI 응용 프로그램에 적합하도록 출력 형식을 유연하게 지원하며 사용자 정의 가능한 시퀀스 토큰을 통합합니다.

## 과정:

- 시스템 메시지: 미리 정의된 안내 메시지가 포함되어 있어, 입력에 기반한 언어 모델 생성을 안내합니다.
- 프롬프트 구성: 시퀀스 시작 토큰, 시스템 메시지, 입력 태그, 인용구(응답) 및 시퀀스 종료 토큰을 하나로 결합하여 형식에 맞는 프롬프트로 구성합니다.
- 출력: output_format 매개변수에 따라 함수는 프롬프트를 일반 문자열로 반환하거나 딕셔너리 내에서 반환합니다.

## 사용 시나리오:

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

딥러닝 워크플로에서 이 함수는 언어 모델에 맥락적 프롬프트를 생성해야 하는 경우에 특히 유용합니다. 이는 창의적 글쓰기, 요약, 또는 특정 주제나 테마에 기반한 콘텐츠 생성과 관련된 작업에 활용됩니다.

토크나이저는 모델의 요구에 따라 필요한 특별 토큰을 자동으로 추가합니다. 이러한 특별 토큰은 종종 시퀀스의 시작과 끝을 나타내는 토큰을 포함하는데, 이는 모델이 입력 텍스트의 시작과 종료를 이해하는 데 중요합니다.

BERT와 같은 모델의 경우, 특별 토큰인 [CLS] (시퀀스 시작을 위한) 및 [SEP] (분리 또는 시퀀스 끝을 위한)과 같은 토큰들이 토크나이저에 의해 자동으로 삽입됩니다. 비슷하게, and 토큰을 시퀀스의 시작 및 끝 표시자로 사용하여 훈련된 모델의 경우, add_special_tokens=True가 지정될 때 토크나이저가 이들을 처리합니다. 이는 프롬프트를 수동으로 and 태그로 감싸지 않아도 되는 것을 의미하는데, 토크나이저가 대신 처리해줄 거에요.

```javascript
# 호출 예제
create_prompt(dataset_train[0], output_format='string')

# 훈련 데이터셋 - 출력 형식 'dict'로 적용하기 위해 create_prompt를 적용하는 람다 함수 사용
instruct_tune_dataset_train = dataset_train.map(lambda row: create_prompt(row, output_format='dict'))
# 테스트 데이터셋 - 출력 형식 'dict'로 적용하기 위해 create_prompt를 적용하는 람다 함수 사용
instruct_tune_dataset_val = dataset_val.map(lambda row: create_prompt(row, output_format='dict'))
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

# 기본 모델 불러오기

이 코드 스니펫은 Hugging Face Transformers 라이브러리에서 언어 모델을 위한 모델과 토크나이저를 설정하고 양자화 설정을 구성하며 모델 실행을 위한 적절한 장치 및 데이터 유형을 결정합니다.

```python
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig
import torch

# 모델에 액세스하기 위해 사용자 데이터에서 Hugging Face API 토큰 가져오기
access_token = userdata.get('HF_TOKEN')

# CUDA 지원 GPU가 있는지 확인하고 사용 가능한 경우 GPU를 사용하고 그렇지 않으면 CPU를 사용합니다.
device_type = "cuda:0" if torch.cuda.is_available() else "cpu"

# 장치에 따라 텐서 데이터 유형 설정: GPU의 경우 float16을 사용 (더 빠르고 메모리 사용량이 효율적), 그렇지 않으면 float32를 사용합니다.
torch_dtype = torch.float16 if torch.cuda.is_available() else torch.float32
```

- Imports: 모델 및 토크나이저 설정에 필요한 라이브러리를 불러옵니다.
- 액세스 토큰: 모델 접근을 위해 Hugging Face 토큰을 가져옵니다.
- 장치 유형: GPU 또는 CPU를 사용할지 결정합니다.
- 텐서 데이터 유형: 장치에 따라 텐서에 적절한 데이터 유형을 설정합니다.

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

## BitsAndBytes를 활용한 양자화된 모델 설정 및 로드

이 섹션에서는 양자화를 통해 향상된 효율성으로 대형 언어 모델을 설정하고 로드하는 과정을 소개합니다. BitsAndBytes 라이브러리를 사용하여 최적의 성능을 얻는 방법을 보여줍니다.

# BitsAndBytes 구성을 특정 양자화 및 연산 설정과 함께 정의합니다

nf4_config = BitsAndBytesConfig(
load_in_4bit=True, # 모델을 4비트 정밀도로 로드하도록 설정
bnb_4bit_quant_type="nf4", # "nf4" 양자화 유형 사용
bnb_4bit_use_double_quant=True, # 향상된 정밀도를 위해 이중 양자화 활성화
bnb_4bit_compute_dtype= torch_dtype # 효율성을 위한 연산 데이터 유형 설정
)

# Mistral-7B-Instruct-v0.1 모델 ID 지정

model_id = "mistralai/Mistral-7B-Instruct-v0.1"

# 모델을 정의된 구성과 장치 할당 및 캐싱을 위한 추가 설정과 함께 로드합니다

model = AutoModelForCausalLM.from_pretrained(
model_id,
device_map=device_type, # 사용 가능한 장치에 모델을 자동으로 할당
quantization_config=nf4_config, # 정의된 BitsAndBytes 구성을 적용
use_cache=False, # 동적 연산을 위해 캐싱 비활성화
token=access_token, # 허깅 페이스 액세스 토큰 안전하게 전달
)

## BitsAndBytes로 구성하기:

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

- 4비트 정밀도로 양자화: load_in_4bit=True로 설정하여 모델이 4비트 정밀도로 로드되도록 구성되어, 메모리 요구 사항이 크게 줄어듭니다.
- 양자화 유형 (nf4): bnb_4bit_quant_type을 "nf4"로 설정하여 사용할 양자화 방법을 지정하며, 이 경우 신경망 계산에 특화된 방법입니다.
- 이중 양자화: bnb_4bit_use_double_quant=True를 통해 양자화된 모델의 높은 정밀도를 가능하게 합니다.
- 데이터 유형 계산: bnb_4bit_compute_dtype은 torch_dtype 변수를 기반으로 설정되어 사용자 정의 계산 효율성을 제공합니다.

## 모델 로딩:

- 모델 식별자: 변수 model_id는 Mistral-7B-Instruct-v0.1 모델의 식별자를 보유하며, 이를 검색할 수 있게 합니다.
- 장치 할당: device_map=device_type로 모델은 사용 가능한 컴퓨팅 리소스에 자동으로 분산되어 성능을 향상시킵니다.
- 양자화 구성: 이전에 정의된 nf4_config이 모델에 적용되어 양자화 설정이 활성화됩니다.
- 캐시 관리: 동적 계산과 메모리 효율성을 우선시하기 위해 캐싱이 비활성화됩니다 (use_cache=False).
- 인증: 액세스 토큰 (token=access_token)을 사용하여 모델에 대한 액세스가 안전하게 유지되어 자원에 안전한 액세스를 보장합니다.

이 접근 방식은 리소스가 제한되어 있는 환경이거나 매우 큰 모델을 처리할 때 모델 로딩과 작동을 최적화하기 위한 고급 구성 옵션을 어떻게 활용하는지를 보여줍니다.

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

## Mistral-7B-v0.1 토크나이저 설정하기

토크나이저 설정 과정은 Mistral-7B-v0.1 모델과 관련된 토크나이저를 로딩하고 텍스트 데이터 처리를 위해 최적으로 구성하는 것을 포함합니다. 이 설정은 시퀀스 길이의 균일성을 보장하고 더 긴 입력을 올바르게 처리하는 것을 보증합니다.

```python
from transformers import AutoTokenizer

# 시작 및 끝 토큰 정의
bos_token = "<s>"
eos_token = "</s>"

# 토큰화를 위한 최대 시퀀스 길이 정의
max_length = 512

# Mistral-7B-v0.1 모델용 토크나이저 및 특정 설정으로 로딩
tokenizer = AutoTokenizer.from_pretrained(
    "mistralai/Mistral-7B-v0.1",  # 모델 식별자
    model_max_length=max_length,  # 최대 모델 입력 길이 설정
    padding_side="right",  # 패딩이 오른쪽에 적용되도록 설정
    token=access_token,  # 안전한 토큰으로 인증
    truncation=True,  # 최대 길이로 부족한 부분은 자르기
    padding="max_length",  # 균일한 길이를 위해 최대 길이로 패딩
)

# 토크나이저 설정 수정
# 이미 정의되지 않은 경우 특수 토큰 확인 및 설정
if tokenizer.eos_token is None:
    tokenizer.eos_token = eos_token  # 끝 토큰 설정
    print("EOS 토큰이 설정되었습니다.")

if tokenizer.bos_token is None:
    tokenizer.bos_token = bos_token  # 시작 토큰 설정
    print("BOS 토큰이 설정되었습니다.")

# 시작 토큰을 패딩 토큰으로 사용
tokenizer.pad_token = tokenizer.bos_token

print(tokenizer)
```

이러한 단계는 Mistral-7B-v0.1 모델로 텍스트 데이터를 처리하기 위한 준비작업으로, 모든 입력이 효율적이고 정확한 자연어 이해 작업을 위해 모델 요구 사항에 부합함을 보장합니다.

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

출력:

```js
LlamaTokenizerFast(
  (name_or_path = "mistralai/Mistral-7B-v0.1"),
  (vocab_size = 32000),
  (model_max_length = 512),
  (is_fast = True),
  (padding_side = "right"),
  (truncation_side = "right"),
  (special_tokens = { bos_token: "<s>", eos_token: "</s>", unk_token: "<unk>", pad_token: "<s>" }),
  (clean_up_tokenization_spaces = False)
),
  (added_tokens_decoder = {
    0: AddedToken(
      "<unk>",
      (rstrip = False),
      (lstrip = False),
      (single_word = False),
      (normalized = False),
      (special = True)
    ),
    1: AddedToken(
      "<s>",
      (rstrip = False),
      (lstrip = False),
      (single_word = False),
      (normalized = False),
      (special = True)
    ),
    2: AddedToken(
      "</s>",
      (rstrip = False),
      (lstrip = False),
      (single_word = False),
      (normalized = False),
      (special = True)
    ),
  });
```

## 주요 설정 단계:

- 최대 시퀀스 길이: 모든 입력 시퀀스의 길이를 표준화하기 위해 제한이 설정됩니다 (max_length = 512). 이 한도는 토큰화 및 모델 추론 중에 메모리 사용량과 계산 부하를 관리하는 데 도움이 됩니다.
- 토크나이저 로딩: transformers 라이브러리의 AutoTokenizer 클래스를 사용하여 특정 모델 식별자("mistralai/Mistral-7B-v0.1")로 토크나이저를 로드합니다. 이 단계를 통해 토크나이저가 모델의 예상 입력 형식과 완벽하게 일치하도록 보장합니다.

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

## 패딩과 자르기:

- 오른쪽 패딩: padding_side="right"로 설정하여 max_length보다 짧은 모든 시퀀스를 오른쪽에 패딩 토큰으로 확장함으로써 일관된 시퀀스 길이를 보장합니다.
- 자르기: truncation=True 옵션을 사용하면 max_length보다 긴 시퀀스를 자동으로 줄여, 입력 오버플로 오류를 방지합니다.
- 최대 길이에 맞는 패딩: padding="max_length" 설정은 모든 시퀀스에 패딩을 적용하여 정의된 max_length에 도달하도록 하여 균일한 입력 크기를 보장합니다.
- 안전한 인증: 토크나이저는 안전한 토큰(access_token=token)을 사용하여 인증되며, 허깅페이스 플랫폼에서 비공개 모델이나 프리미엄 기능에 액세스하는 데 필수적입니다.

다음 코드 조각은 훈련 및 검증 데이터 세트에서 토큰화된 입력 시퀀스의 길이 분포를 플롯하는 함수를 정의하고 사용합니다.

```python
import matplotlib.pyplot as plt

def plot_data_lengths(tokenized_train_dataset, tokenized_val_dataset):
    # 훈련 데이터 세트에서 각 데이터 샘플의 input_ids 길이 계산
    lengths = [len(x['input_ids']) for x in tokenized_train_dataset]
    # 검증 데이터 세트에서 각 데이터 샘플의 input_ids 길이 추가
    lengths += [len(x['input_ids']) for x in tokenized_val_dataset]

    # 총 샘플 수 출력
    print(len(lengths))

    # 히스토그램 그리기
    plt.figure(figsize=(10, 6))  # 지정된 크기로 새로운 그림 생성
    plt.hist(lengths, bins=20, alpha=0.7, color='blue')  # 20개의 bin을 사용하여 히스토그램 그리기
    plt.xlabel('input_ids의 길이')  # x축 레이블 설정
    plt.ylabel('빈도수')  # y축 레이블 설정
    plt.title('input_ids 길이 분포')  # 플롯 제목 설정
    plt.show()  # 플롯 표시

# 토큰화된 훈련 및 검증 데이터 세트를 이용하여 함수 호출
plot_data_lengths(tokenized_train_dataset, tokenized_val_dataset)
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

이 기능은 토큰화된 입력 시퀀스의 길이 분포를 시각화하는 데 도움이 되며 데이터셋의 특성을 이해하고 적절한 모델 입력 처리를 보장하는 데 유용합니다.

![Image](/assets/img/2024-07-09-Fine-TuningMistral-7BAJourneyThroughLiteratureandAI_7.png)

# 함수: generate_and_tokenize_prompt

generate_and_tokenize_prompt 함수는 입력 데이터에서 프롬프트를 생성하고 이러한 프롬프트를 토큰화하여 모델에 대한 데이터 진입을 간소화합니다. 이 프로세스는 훈련 또는 추론을 위해 토큰화된 입력이 필요한 모델에 중요합니다.

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
def generate_and_tokenize_prompt(row):
    """
    데이터 행에서 프롬프트를 생성하고 토큰화합니다.

    매개변수:
        row (dict): 프롬프트를 생성하는 데 필요한 데이터가 포함된 사전입니다.

    반환값:
        토큰화된 프롬프트입니다.
    """
    prompt = create_prompt(row)  # output_format='string'을 지정할 필요가 없습니다. 기본값이기 때문입니다.
    return tokenizer(prompt)

tokenized_train_dataset = dataset_train.map(generate_and_tokenize_prompt)
tokenized_val_dataset = dataset_val.map(generate_and_tokenize_prompt)
```

## 프로세스 개요:

- 프롬프트 생성: 데이터 집합의 각 항목에 대해(딕셔너리 형식의 행으로 표시) create_prompt 함수를 사용하여 프롬프트가 생성됩니다. 이 함수는 행 내에 있는 데이터를 활용하여 텍스트 프롬프트를 구성합니다. 기본 출력 형식은 토큰화 요구 사항에 맞는 문자열입니다.
- 프롬프트 토큰화: 생성된 문자열 프롬프트는 다음에 토크나이저(tokenizer(prompt))에 전달되어 텍스트를 기계 학습 모델이 이해할 수 있는 형식으로 변환됩니다. 일반적으로 토크나이저의 어휘 집합에서 토큰을 나타내는 정수 시퀀스로 표시됩니다.

## .map 메서드를 사용한 일괄 토큰화:

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

- Tokenized Training Dataset: `dataset_train.map(generate_and_tokenize_prompt)` applies the `generate_and_tokenize_prompt` function to each entry in the `dataset_train`. This results in a new dataset where each original row is replaced with its tokenized prompt version.
- Tokenized Validation Dataset: Similarly, `dataset_val.map(generate_and_tokenize_prompt)` transforms the validation dataset, ensuring that both training and validation datasets are in the correct format for model consumption.

## Prompt generation and tokenization in LLM NLP Workflows

By automating prompt generation and tokenization, this method significantly streamlines the preprocessing pipeline for NLP LLM tasks, enabling more efficient model training and evaluation.

This approach is particularly valuable in NLP workflows for several reasons:

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

- 일관성: 데이터 세트 항목 간에 균일한 처리 및 토큰화를 보장합니다.
- 효율성: 일괄 처리를 위해 .map 메서드를 활용하여 대규모 데이터 세트의 준비를 가속화합니다.
- 호환성: 모델과 바로 호환되는 형식으로 데이터를 준비하여 더 부드러운 훈련 및 평가 프로세스를 용이하게 합니다.

generate_response 함수는 Transformers 라이브러리에서 사전 훈련된 모델 및 토크나이저를 활용하여 주어진 입력 프롬프트에 기반한 텍스트 응답을 생성하는 데 사용됩니다. 이 함수는 입력 준비, 모델 추론 및 출력 처리 과정을 캡슐화하여, 챗봇, 자동화된 글 쓰기 도우미 또는 프롬프트에서 인간과 유사한 텍스트를 생성해야 하는 모든 시나리오와 같은 응용 프로그램에 유용한 도구입니다.

```python
def generate_response(prompt, model, tokenizer):
    """
    지정된 모델과 토크나이저를 사용하여 주어진 프롬프트에 대한 응답을 생성합니다.

    Parameters:
        prompt (str): 응답을 생성할 입력 텍스트 프롬프트입니다.
        model (transformers.PreTrainedModel): 응답 생성에 사용되는 사전 훈련된 모델입니다.
        tokenizer (transformers.PreTrainedTokenizer): 프롬프트 전처리 및 모델 출력 디코딩을 위한 토크나이저입니다.

    Returns:
        str: 생성된 응답 텍스트입니다.
    """
    # 프롬프트를 모델 입력 형식으로 인코딩
    encoded_input = tokenizer(prompt, return_tensors="pt", add_special_tokens=True)
    # 모델 입력을 GPU로 이동
    model_inputs = encoded_input.to('cuda')

    # 모델을 사용하여 응답 생성
    generated_ids = model.generate(
        **model_inputs,
        max_new_tokens=1000,  # 생성되는 새 토큰의 최대 수 제한
        do_sample=True,       # 다양한 응답 생성을 위한 샘플링 활성화
        pad_token_id=tokenizer.eos_token_id  # 생성 중 패딩에 EOS 토큰 사용
    )

    # 생성된 응답 토큰을 텍스트로 디코딩
    decoded_output = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)

    # 생성된 응답만 반환하도록 출력에서 원래 프롬프트 제거
    response = decoded_output[0].replace(prompt, "")
    return response
```

여기에는 작동 워크플로우에 대한 간략한 설명이 있습니다:

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

- 입력 프롬프트 토큰화: 이 함수는 제공된 토크나이저를 사용하여 입력 프롬프트를 토큰화하는 것으로 시작합니다. 이 단계에서는 텍스트 문자열을 모델이 이해할 수 있는 형식(토큰 ID의 텐서)으로 변환하며, 시퀀스의 시작 및 끝을 나타내는 특수 토큰이 추가됩니다.
- GPU 할당: 토큰화된 입력은 가속 컴퓨팅을 활용하기 위해 GPU로 이동됩니다. 특히 대규모 언어 모델을 다룰 때는 성능을 위해 이 단계가 중요하며, 추론에 소요되는 시간을 크게 줄여줍니다.
- 응답 생성: 입력이 준비되면, 함수는 모델의 generate 메서드를 호출하여 응답을 생성합니다. 이 메서드는 생성 프로세스를 제어하기 위해 여러 매개변수를 사용합니다:
  - max_new_tokens는 모델이 생성할 수있는 새 토큰의 수를 제한하여 지나치게 긴 출력을 방지합니다.
  - do_sample은 다음 토큰의 확률적 샘플링을 활성화하여 더 다양하고 자연스러운 응답을 가능하게 합니다.
  - pad_token_id는 패딩에 사용되는 토큰을 지정하여 모델이 입력 길이의 변화를 적절히 처리하도록 합니다.
- 디코딩 및 정리: 생성 후, 출력 토큰 ID를 다시 텍스트로 디코딩합니다. 해당 함수는 이 텍스트에서 원래 프롬프트를 제거하여 반환된 응답에는 새롭게 생성된 내용만 포함되도록 합니다.

결과적으로 모델이 입력 프롬프트에 대한 응답으로 생성한 독립된 텍스트 조각이 나옵니다. 이는 모델이 주어진 텍스트를 이해하고 일관되고 맥락적으로 적절하게 계속할 수 있는 능력을 보여줍니다.

토크나이저(prompt, return_tensors="pt", add_special_tokens=True)

이 코드 줄은 텍스트 프롬프트를 트랜스포머 모델에 입력할 수 있는 형식으로 변환하기 위해 토크나이저를 사용합니다.

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

이것은 모델이 응답하거나 분석하도록 원하는 텍스트 입력이에요.

`return_tensors="pt"`: 출력 형식이 PyTorch 텐서 형식이어야 한다는 것을 지정해요. "pt"는 PyTorch를 의미해요. 만약 TensorFlow를 사용 중이라면 대신 "tf"를 사용할 수 있어요.

`add_special_tokens=True`: 토크나이저에게 문장의 시작과 끝을 이해하기 위해 필요한 특별 토큰을 자동으로 삽입하도록 지시해요. 예를 들어, 많은 모델에서 [CLS] 토큰이 문장의 시작 부분에 사용됩니다.

비조정 모델로 응답 예시입니다.

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

# 예제 프롬프트

프롬프트 = "[INST]주어진 입력을 사용하여 주제 범주에 대한 새로운 원래 인용구를 작성하십시오[/INST] ### 입력:야망 ### 응답:"

# 응답 생성

응답\_생성(프롬프트, 모델, 토크나이저)

출력: "야망은 우리를 인생에서 위대한 성취로 이끄는 궁극적인 연료입니다."

# Mistral-7B 모델 학습

## 학습 가능한 매개변수

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

print_trainable_parameters 함수는 주어진 신경망 모델의 매개변수에 대한 통계를 계산하고 출력하는 데 사용됩니다. 특히, 이 함수는 이러한 매개변수 중 몇 개가 학습 가능한지에 초점을 맞춥니다.

# 추론을 위해 다시 활성화!

model.config.use_cache = False

# 학습 가능한 매개변수 출력

def print*trainable_parameters(model):
"""
모델 내의 학습 가능한 매개변수 수를 출력합니다.
"""
trainable_params = 0
all_param = 0
for *, param in model.named_parameters():
all_param += param.numel()
if param.requires_grad:
trainable_params += param.numel()
print(
f"trainable params: {trainable_params} || all params: {all_param} || trainable%: {100 \* trainable_params / all_param}"
)

이것이 작동하는 방법에 대한 상세한 설명입니다:

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

\*\*개요: 딥 러닝 모델에서, 매개변수(주로 가중치)는 훈련 데이터로부터 학습되는 모델의 구성 요소입니다. 매개변수는 훈련 가능한지 여부에 따라 구분됩니다. 훈련 가능한 매개변수는 역전파를 통해 훈련 중에 업데이트되며, 훈련 불가능한 매개변수는 정적이며 이 과정 동안 업데이트되지 않습니다.

함수 과정:

- 이 함수는 모델 내 훈련 가능한 총 매개변수 수와 총 매개변수 수를 추적하기 위해 두 개의 카운터인 trainable_params 및 all_param을 초기화합니다.
- 이 함수는 model.named_parameters()를 호출하여 모델의 모든 매개변수를 반복하며 가져옵니다. 이 방법은 각 매개변수 이름과 매개변수 자체가 포함된 튜플의 반복자를 반환합니다.
- 각 매개변수에 대해 함수는 param.numel()를 사용하여 해당 텐서의 요소 총 수를 가져오며, 이는 그 텐서의 매개변수 수에 해당합니다. 이 수를 all_param에 추가하여 모델 내 모든 매개변수의 러닝 총 수를 유지합니다.
- param.requires_grad를 조사하여 매개변수가 훈련 가능한지 확인합니다. requires_grad가 True이면 해당 매개변수가 훈련 가능하며, 해당 요소 수가 trainable_params에 추가됩니다.
- 모든 매개변수를 반복한 후, 함수는 trainable_params를 all_param으로 나누고 100을 곱하여 훈련 가능한 매개변수의 백분율을 계산합니다.
- 마지막으로 함수는 훈련 가능한 총 매개변수 수, 총 매개변수 수 및 훈련 가능한 매개변수의 백분율을 출력합니다.

이 함수는 모델의 복잡성과 용량, 그리고 훈련 중에 발생하는 학습 범위를 이해하는 데 유용합니다. 훈련 가능한 매개변수와 훈련 불가능한 매개변수를 구분함으로써, 모델의 다른 부분이 학습 과정에 어떻게 기여하는지에 대한 통찰을 얻을 수도 있습니다.\*\*

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

## 파라미터 효율적 파인 튜닝(PEFT)

파라미터 효율적 파인 튜닝(PEFT)은 사전 훈련된 모델을 모두 파인 튜닝하지 않고도 하위 응용 프로그램에 효율적으로 적응할 수 있게 합니다. PEFT는 널리 사용되는 대형 언어 모델의 Low-Rank Adaptation(LoRA)을 지원합니다.

```python
from peft import AutoPeftModelForCausalLM, LoraConfig, get_peft_model, prepare_model_for_kbit_training

peft_config = LoraConfig(
    r=8,
    lora_alpha=16,
    target_modules=[
        "q_proj",
        "k_proj",
        "v_proj",
        "o_proj",
        "gate_proj",
        "up_proj",
        "down_proj",
        "lm_head",
    ],
    bias="none",
    lora_dropout=0.05,  # Conventional
    task_type="CAUSAL_LM"
)
```

제공된 코드 스니펫은 peft 라이브러리를 사용하여 파라미터 효율적 파인 튜닝(PeFT)을 위한 모델 구성과 관련이 있습니다. 구체적으로, 대규모 사전 훈련 모델을 최소한의 학습 가능한 파라미터 증가로 조정하는 기술인 LoRA(저랭크 적응) 설정에 초점을 맞추고 있습니다. 여기서 주요 구성 요소와 그 의미를 살펴보겠습니다:

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

- 수입 문: 이 코드는 PeFT 라이브러리에서 AutoPefModelForCausalLM, LoraConfig, get_peft_model 및 prepare_model_for_kbit_training을 가져옵니다. 이러한 유틸리티 및 클래스는 LoRA와 같은 PeFT 기술을 사용하는 데 도움이 되도록 설게되었습니다.
- LoRA 구성 (LoraConfig): LoRA의 구성은 LoraConfig를 통해 정의되며 적응이 어떻게 적용될지를 지정합니다. LoraConfig 내의 매개변수는 특정한 역할을 가지고 있습니다:
  - r: 이 매개변수는 적응 행렬의 순위를 지정합니다. 순위가 낮을수록 매개변수가 적고 적응용 용량이 줄어들지만, 더 매개변수 효율적입니다.
  - lora_alpha: 이는 적응의 규모를 결정합니다. 더 높은 lora_alpha 값은 적응된 모델의 출력에 LoRA 매개변수의 영향을 증가시킵니다.
  - target_modules: LoRA 적응이 적용되어야 하는 트랜스포머 모델 내의 모듈 이름 목록입니다. 주요 대상은 어텐션 메커니즘에 관여하는 프로젝션 레이어 (q_proj, k_proj, v_proj, o_proj) 및 언어 모델의 lm_head와 같은 모델 아키텍처에 특화된 레이어입니다.
  - bias: 적응 과정 중에 편향을 어떻게 다룰지 지정합니다. "none"으로 설정하면 편향이 적응되지 않음을 나타냅니다.
  - lora_dropout: LoRA 매개변수에 적용된 드롭아웃 비율입니다. 드롭아웃은 과적합을 방지하기 위해 훈련 중에 매개변수의 일부를 무작위로 제로로 설정하는 정칙적인 기술입니다.
  - task_type: 모델이 적응될 작업 유형을 정의하며, 이 경우 "CAUSAL_LM"(인과 언어 모델링)입니다. 이 설정은 작업의 특성에 맞게 적응 과정을 맞추는 데 도움이 됩니다.

LoraConfig를 사용한 설정은 사전 훈련된 모델을 구체적 작업에 대해 미세 조정하기 위한 부분으로, 모델이 작업 관련 데이터와 최소한의 매개변수를 수정하도록 하는 것이 목표입니다. 이 접근법은 매우 큰 모델을 다룰 때 특히 유용할 수 있으며, 전통적인 미세 조정이 계산적으로 비용이 많이 드는 경우나 작업별 데이터 양이 제한적인 경우에 유용합니다.

```js
model.gradient_checkpointing_enable();
model = prepare_model_for_kbit_training(model);
model = get_peft_model(model, peft_config);
print_trainable_parameters(model);
```

1. model.gradient_checkpointing_enable():

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

모델에 그래디언트 체크포인팅을 활성화하는 방법이에요. 그래디언트 체크포인팅은 훈련 중 메모리 사용량을 줄이는 기술이거든. 연산 시간과의 교환이라고 해도 돼. 모든 중간 활성화를 메모리에 저장하지 않고, 역전파를 위해 일부만 저장하고 필요할 때 나머지를 다시 계산해. 훈련 중 메모리가 부족한 대규모 모델이나 매우 깊은 네트워크를 훈련하는 데 특히 유용해.

2. `model = prepare_model_for_kbit_training(model)`:

모델을 'k-bit' 훈련을 위해 준비하는 거야. 이 과정은 모델을 수정하여 표준 32비트 부동 소수점 숫자보다 더 적은 비트를 사용하는 연산을 지원하도록 하는 걸 포함해. 이를 통해 메모리 사용량을 줄이고 계산 속도를 높일 수 있어. 하지만 이 준비의 구체적인 성격은 구현에 따라 다를 수 있고 양자화 기술이나 더 낮은 정밀도 산술을 지원하기 위해 모델 계층을 조정하는 것이 포함될 수도 있어.

3. `model = get_peft_model(model, peft_config)`:

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

이 함수는 위에서 본 것처럼 주어진 모델을 Parameter-Efficient Fine-Tuning (PeFT) 구성에 따라 적응시킵니다.

4. print_trainable_parameters(model):

해당 함수를 호출하여 모델 내 전체 파라미터 수, 훈련 가능한 파라미터 수, 그리고 훈련 가능한 파라미터의 백분율을 출력합니다. 위에서 본 것과 같습니다.

출력:

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

**trainable params:** 21260288 || **all params:** 3773331456 || **trainable%:** 0.5634354746703705

# Model Parameter Summary

- **Trainable Parameters:** 21,260,288
- These are the parameters that will be updated during the training process.

- **Total Parameters:** 3,773,331,456
- This includes both trainable and non-trainable parameters in the model.

- **Trainable Percentage:** 0.56%
- This indicates that only 0.56% of the total parameters in the model are trainable.

Reference:

Let’s continue; we are almost there to conclude our long explanation.

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

다음 코드는 대규모 모델의 학습 또는 추론을 최적화하기 위해 여러 GPU를 활용하여 계산 부하를 분산하는 데 유용합니다.

```javascript
if torch.cuda.device_count() > 1: // GPU가 1개 이상인 경우
    model.is_parallelizable = true;
    model.model_parallel = true;
```

- 다중 GPU 확인: 코드는 사용 가능한 GPU가 1개 이상인지 확인합니다.
- 병렬 처리 활성화: 여러 GPU가 사용 가능한 경우, 모델을 병렬 처리할 수 있도록 설정합니다.
- 병렬 처리 활성화: 그런 다음 모델 병렬 처리를 활성화하여 모델 동작에 여러 GPU를 활용합니다.

이 시점에서, 때때로 colab이 다음과 같은 오류로 작동하지 않을 수 있습니다: NotImplementedError: UTF-8 로케일이 필요합니다. Colab이 이러한 종류의 오류를 표시할 때 다음 코드를 사용했습니다.

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

만약 실험을 추적하고 싶다면 Weights and Biases를 사용해보세요. Weights and Biases를 활용하면 모델을 학습하고 세부 조정하며, 실험부터 제품 생산까지 모델을 관리하며, LLM 애플리케이션을 추적하고 평가할 수 있습니다.

이 설정은 실험을 추적하고 시각화하는 데 꼭 필요하며, 재현성을 확보하고 다른 사람들과 협업할 수 있도록 도와줍니다.

!pip install -q wandb -U

import wandb, os
wandb.login()

wandb_project = "quotes-finetune"
if len(wandb_project) > 0:
os.environ["WANDB_PROJECT"] = wandb_project

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

- Weights & Biases 설치: 이 코드는 실험 추적을 위한 wandb 라이브러리를 설치합니다.
- 인증하기: Weights & Biases 계정에 로그인합니다. wandb.login(): Weights & Biases에 대한 로그인 프로세스를 시작하여 사용자가 인증하고 Weights & Biases 계정에 연결할 수 있게 합니다.
- 프로젝트 이름 설정: Weights & Biases에서 실험을 정리하기 위해 프로젝트 이름을 설정합니다.

```js
run_name = "quotes-finetune";
```

```js
from huggingface_hub import notebook_login

notebook_login()
```

노트북 로그인: 이 코드는 노트북 환경에서 사용자를 직접 인증하기 위해 huggingface_hub의 notebook_login을 먼저 사용합니다.

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

# Mistral 7-B 테스트 파인튜닝을 실행하세요

전체 훈련 세션을 시작하기 전에, 몇 가지 훈련 단계로 초기 테스트를 진행하는 것이 좋습니다. 이 초기 테스트를 통해 파인튜닝 프로세스가 올바르게 설정되어 있는지 확인하고 잠재적인 문제점을 미리 식별하여 시간과 자원을 절약할 수 있습니다.

## 테스트 실행의 장점

- 설정 확인: 훈련 환경, 데이터 로딩, 그리고 모델 구성이 예상대로 작동하는지 확인합니다.
- 오류 초기 감지: 긴 훈련 세션에 앞서 훈련 스크립트에서 발생하는 오류나 잘못된 설정을 식별합니다.
- 성능 모니터링: 작은 규모로 모델 성능과 자원 사용량을 모니터링하며, 매개변수와 설정을 적절히 조정하는 데 도움이 됩니다.

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

짧은 테스트를 통해 몇 가지 학습 단계만 실행하여 설정을 세밀하게 조정하고 원활하고 효율적인 전체 훈련 과정을 보장할 수 있습니다.

```python
from transformers import TrainingArguments
# Change bf16 to fp16 for non Ampere GPUs
args = TrainingArguments(
  output_dir = "./mistral_instruct_generation_V2",
  #num_train_epochs=1,
  max_steps = 4, # comment out this line if you want to train in epochs
  per_device_train_batch_size = 4,
  gradient_accumulation_steps=4,
  gradient_checkpointing=True,
  warmup_steps = 1,
  #evaluation_strategy="epoch",
  evaluation_strategy="steps",
  eval_steps=1, # comment out this line if you want to evaluate at the end of each epoch
  save_steps=1,
  learning_rate=2e-4,
  fp16=True,
  lr_scheduler_type='constant',
  logging_steps=50,
  logging_dir="./logs",
  save_strategy="steps",   # Save the model checkpoint every logging step
  do_eval=True,    # Perform evaluation at the end of training
  neftune_noise_alpha=5,
  push_to_hub=False,
  report_to="wandb", # Comment this out if you don't want to use weights & baises
  run_name=f"{run_name}-{datetime.now().strftime('%Y-%m-%d-%H-%M')}"          # Name of the W&B run (optional)
  # optim="paged_adamw"
)

# model.config.use_cache = False  # silence the warnings. Please re-enable for inference!
```

## 하이퍼파라미터 설명

이 코드 스니펫은 허깅 페이스 트랜스포머 라이브러리를 사용하여 모델 파인튜닝을 위한 훈련 인자를 구성하는 것입니다. TrainingArguments 클래스는 훈련 과정에 대한 여러 설정과 하이퍼파라미터를 지정하는 데 사용됩니다. 위에서 제공된 주요 인자들에 대한 설명은 다음과 같습니다:

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

이 설정은 특정 작업의 요구 사항에 맞게 조정된 상세한 훈련 규제를 설정합니다. 학습률, 배치 크기 및 평가 및 모델 체크포인트 저장을 위한 전략과 같은 하이퍼파라미터를 포함합니다. 또한 성능 및 리소스 활용을 최적화하기 위해 그래디언트 누적 및 혼합 정밀도 훈련과 같은 실천 방법을 통합합니다.

NEFTune은 채팅 모델의 성능을 향상시키는 기술로, "NEFTune: Noisy Embeddings Improve Instruction Finetuning" 논문에서 소개되었습니다. 이는 훈련 중에 임베딩 벡터에 노이즈를 추가하는 것으로 구성됩니다. 논문의 초록에 따르면:

Alpaca를 사용한 LLaMA-2-7B의 표준 미세 조정은 AlpacaEval에서 29.79%를 달성하며, 노이지 임베딩을 사용하면 64.69%로 상승합니다. NEFTune은 최신 지시어 데이터셋에서 강력한 베이스라인을 능가합니다. Evol-Instruct로 훈련된 모델은 10% 향상되었으며, ShareGPT는 8% 향상되었으며, OpenPlatypus는 8% 향상되었습니다. 심지어 NEFTune으로 추가 훈련을 한 LLaMA-2-Chat과 같은 RLHF로 계속 세분화된 강력한 모델도 이득을 볼 수 있습니다.

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

**진짜 훈련 시나리오**

파라미터가 "최적화되었는가"는 특정 작업, 미세 조정 중인 모델, 데이터셋의 크기와 성격, 그리고 사용 가능한 계산 자원에 상당히 의존합니다. 각 매개변수가 훈련에 어떻게 영향을 미칠 수 있는지 간략히 설명하고 최적화를 위한 고려 사항을 살펴보겠습니다:

- [huggingface.co(https://huggingface.co/docs/trl/sft_trainer)
- [GitHub(neelsjain/NEFTune)](https://github.com/neelsjain/NEFTune)

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

- **max_steps = 250**: 훈련 단계의 총 횟수를 결정합니다. 이는 작업의 복잡성과 데이터셋 크기에 따라 최적인지 여부가 달라집니다. 많은 작업에는 수렴을 달성하기 위해 더 많은 단계가 필요할 수 있습니다.
- **per_device_train_batch_size = 4**: 장치 당 배치 크기입니다. GPU의 사용 가능한 메모리에 대한 균형을 맞춰야 합니다. 더 큰 배치 크기는 빠른 수렴을 이끌지만 더 많은 메모리가 필요합니다.
- **gradient_accumulation_steps=4**: 이는 추가 메모리 비용 없이 그라디언트를 여러 단계에 걸쳐 누적하여 모델 가중치를 업데이트하는 효과적인 큰 배치 크기를 가능하게 합니다. 최적의 값은 다시 데이터셋 및 모델 크기에 따라 다릅니다.
- **gradient_checkpointing=True**: 메모리 사용량을 줄이는 데 도움을 주며, 더 큰 모델이나 긴 시퀀스를 훈련할 수 있도록 계산과 메모리를 교환합니다.
- **warmup_steps = 5**: 학습률을 0부터 초기 학습률까지 증가시키기 위한 단계 수입니다. 최적의 수는 달라질 수 있으며, 초기 학습 불안정성을 방지하기 위해 더 큰 수가 종종 사용됩니다.
- **evaluation_strategy="steps" and eval_steps=50**: 모델을 검증 세트에서 평가할 빈도를 결정합니다. 최적의 평가 빈도는 훈련 기간 및 모델이 얼마나 빨리 향상될 것으로 예상되는지에 따라 다릅니다.
- **save_steps=50**: 모델 체크포인트를 저장하는 빈도입니다. 일반적으로 훈련 중에 진행이 손실되지 않도록 자주 저장하는 것이 유용할 수 있지만, 많은 파일을 생성할 수도 있습니다.
- **learning_rate=2e-4**: 훈련에 사용되는 학습률입니다. 최적의 학습률을 찾는 것은 중요하며 보통 실험이나 학습률 파인더와 같은 기법이 필요할 수 있습니다.
- **fp16=True**: 호환되는 GPU에서 훈련을 크게 가속화하고 메모리 사용량을 줄일 수 있는 혼합 정밀도 훈련을 활성화합니다.
- **lr_scheduler_type=`constant`**: 일정한 학습률 사용은 간단한 방법입니다. 그러나 작업에 따라 다른 전략이 더 나은 결과를 이끌 수 있습니다.
- **do_eval=True**: 평가 수행 여부입니다. 지속적인 평가는 진행 상황을 모니터링하는 데 도움이 되지만 훈련 속도를 늦출 수도 있습니다.
- **push_to_hub=True**: Hugging Face의 모델 허브로 자동으로 푸시합니다. 모델을 공유하는 데 편리하지만 모든 시나리오에서 원치 않을 수도 있습니다.
- **report_to="wandb"**: Weights & Biases 통합은 실험을 추적하는 데 훌륭합니다. 하지만 최적의 사용을 위해 일관된 실험 추적 전략을 설정하고 따르는 것이 중요합니다.

이러한 매개변수의 최적화에는 포기에 대한 이해가 필요하며 하이퍼파라미터 튜닝 기법이나 실험이 포함될 수 있습니다. 손실 이외의 정확도나 F1 점수와 같은 메트릭을 모니터링해서 모델이 실제로 중요한 작업에 대해 향상되고 있는지 확인하는 것이 중요합니다.

이 설정은 언어 모델을 세밀하게 조정하는 현대적인 방법을 포괄하며, 효율적인 매개변수와 고급 데이터 전처리 기술을 활용하여 모델을 특정 작업이나 데이터셋에 보다 효과적으로 맞출 수 있습니다.

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
from trl import SFTTrainer

max_seq_length = 512

trainer = SFTTrainer(
  model=model,
  peft_config=peft_config,
  max_seq_length=max_seq_length,
  # tokenizer=tokenizer, if data are not tokenized
  packing=True,
  formatting_func=create_prompt, # this will apply the create_prompt mapping to all training and test datasets
  args=args,
  train_dataset=tokenized_train_dataset,
  eval_dataset=tokenized_val_dataset,
  # [SFTTrainer Documentation](https://huggingface.co/docs/trl/sft_trainer)
)
```

```python
trainer.train()
```

In this code snippet, we set up an instance of the SFTTrainer class from the trl library to fine-tune a model using Supervised Fine-Tuning (SFT). Let's delve into the key components and their functions:

- Importing SFTTrainer: The SFTTrainer class is imported from the trl module. It's tailored for refining language models and offers a more controlled and potentially more efficient approach compared to traditional methods.
- max_seq_length: This variable defines the maximum sequence length for the model inputs. Maintaining a fixed sequence length is essential for batching and maintaining consistent tensor shapes. Here, it's set to 512, a common choice for NLP tasks balancing context consideration and computational efficiency.
- Instantiation of SFTTrainer:
  - model: Specifies the pre-trained model to fine-tune.
  - peft_config: Provides the configuration for Parameter-Efficient Fine-Tuning (PeFT), which may include techniques like LoRA or adapters to alter fewer parameters during fine-tuning.
  - max_seq_length: Determines the maximum sequence length passed to the model, ensuring proper padding or truncation of inputs.
  - packing: Indicates whether sequence packing will be utilized. It can enhance training efficiency by accommodating variable-length sequences in a batch, albeit requiring more intricate data preprocessing.
  - formatting_func: This function, create_prompt in this case, is applied to each example in the training and evaluation datasets. It structures raw data into a format the model can process, potentially involving tokenization, although the datasets mentioned are pre-tokenized.
  - args: Holds the TrainingArguments object configuring the training process, encompassing optimization parameters, save intervals, evaluation strategies, and more.
  - train_dataset and eval_dataset: Represent the training and evaluation datasets, anticipated to be pre-tokenized and potentially pre-processed by the formatting_func.

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

# Hugging Face Hub로 모델 푸시하기

Hugging Face Hub로 모델을 훈련 중이나 훈련 후에 푸시하면, 모델을 다른 프로젝트 간이나 커뮤니티와 공유하고 재사용하는 것이 매우 용이해집니다. 제공된 코드 스니펫에는 Hugging Face Hub로 모델을 푸시하는 단계가 명시적으로 포함되어 있지 않지만, Hugging Face Transformers와 Hugging Face Hub 유틸리티를 조합하여 이를 달성할 수 있습니다. 아래는 모델을 푸시하고 나중에 재사용하는 방법입니다:

- 자동 푸시: Hugging Face Transformers 라이브러리의 TrainingArguments를 사용하는 경우, push_to_hub 매개변수를 True로 설정하고 hub_model_id 및 hub_token과 같은 추가 세부 정보를 제공하여 자동 푸시를 활성화할 수 있습니다. SFTTrainer는 이 기능을 직접적으로 보여주지는 않지만, 이는 Transformers의 Trainer 클래스에서 흔히 볼 수 있는 기능입니다.
- 수동 푸시: 훈련 후에, huggingface_hub 라이브러리를 사용하여 모델을 수동으로 Hub에 푸시할 수 있습니다. Hugging Face Hub에 로그인되어 있는지 확인한 후 (huggingface-cli login을 사용할 수 있습니다), Repository 클래스를 사용하여 저장소를 생성하고 모델을 푸시하세요:

```python
from huggingface_hub import Repository, HfFolder

# 로그인했는지 확인하고 'your_model_name'를 모델 이름으로 대체하세요
model_dir = "./your_model_dir"  # 모델 파일이 저장된 디렉토리
repo_name = "your_model_name"
username = "your_huggingface_hub_username"  # 자신의 Hugging Face Hub 사용자명으로 대체

repo = Repository(local_dir=model_dir,
                  repo_id=f"{username}/{repo_name}",
                  clone_from=f"{username}/{repo_name}",
                  use_auth_token=True)
repo.push_to_hub()
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

# 인퍼 및 퀄리티 테스트를 위해 튜닝된 미스트랄-7B 모델을 인용구에 적용해보세요

```python
# 응답 생성하는 함수
def generate_response(prompt, model):
    encoded_input = tokenizer(prompt, return_tensors="pt", add_special_tokens=True)
    model_inputs = encoded_input.to('cuda')

    generated_ids = model.generate(**model_inputs, max_new_tokens=5000, do_sample=True, pad_token_id=tokenizer.eos_token_id)

    decoded_output = tokenizer.batch_decode(generated_ids)

    return decoded_output[0].replace(prompt, "")
```

```python
# 추론을 위해 다시 활성화!
model.config.use_cache = True
```

```python
# 사전 훈련 레이어를 피피엣 모델과 병합
from peft import PeftConfig, PeftModel

config = PeftConfig.from_pretrained("Gianpiero/afo-mistral-model-200")
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.1")
ft_model = PeftModel.from_pretrained(model, "Gianpiero/afo-mistral-model-200")
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

1. "꿈은 영혼의 물감이며, 그것으로 우리 마음의 깊은 욕망을 만들어냅니다."
2. "내일의 실현에 대한 유일한 제한은 오늘의 의심일 것입니다."
3. "당신의 꿈을 따르세요, 첫걸음을 내지 않는다면 아무도 당신을 위해 내지 않을 것입니다."
4. "꿈을 꿔보세요, 그리고 크게 꾸세요. 꿀수 있는 것은 꿈뿐입니다."
5. "자신을 믿는다면 당신의 꿈을 실현할 수 있습니다."
6. "우리가 꿈꾸는 길은 크게 꾸는 용기에서 나옵니다."
7. "우리의 꿈은 우리가 성장하고 성취하는 데 필요한 씨앗입니다."
8. "꿈은 우리 마음의 속삭임이며, 그것들은 위대함을 이루도록 우리를 영감을 줄 수 있습니다."
9. "꿈은 아프지 않아요; 그저 이를 실현할 때 아픕니다."
10. "자신의 꿈을 포기하지 마세요, 인생은 당신이 믿는다면 잡을 수 있는 꿈 같습니다."
11. "꿈은 우리를 운명으로 이끄는 날갯짓입니다."
12. "꿈꾸는 것은 존재하는 방법이며, 그것은 우리 자신을 가장 순수하고 진정한 형태로 표현하는 방법입니다."
13. "꿈은 연료입니다. 그리고 우리 자신을 믿는다면 불길로 변할 수 있습니다."
14. "꿈이라면 불가능한 것은 없습니다. 그러니 한계 없이 꿈꾸세요."
15. "꿈은 우리 마음과 머릿속 깊은 곳에서 기원하며, 우리가 갖는 유일한 한계입니다."
16. "꿈은 세상에서 가장 강력한 힘이며, 우리가 스스로 설정한 유일한 한계입니다."
17. "꿈을 쫓는 것에는 늦음이란 없습니다, 당신의 것으로 만들고, 망설이지 않고 그것을 향해 나아가세요."
18. "성공의 기반이 되는 것은 꿈이며, 그것이 우리가 더 큰 것을 위해 노력하도록 이념을 주기 때문입니다."
19. "꿈꾸면 이룰 수 있으며, 크게 꾼다면 어떤 것이든 이룰 수 있습니다."
20. "꿈을 꾸고, 믿고, 이루는 것은 모든 인간의 기본적인 권리입니다."
21. "꿈은 우리의 열정을 불러일으키는 불꽃이며, 우리의 목표와 희망으로 이끕니다."
22. "우리는 작은 걸음을 내지 않아서가 아니라, 거대한 한 걸음을 내어 꿈을 이룹니다."
23. "우리의 꿈을 이루는 유일한 방법은 우리 자신을 믿고, 절대 포기하지 않는 것입니다."
24. "꿈은 가능성과 기회의 문을 열어주는 열쇠입니다."
25. "우리는 우리 자신의 운명을 창조하는 자입니다. 그리고 그 모든 시작은 꿈, 믿음, 행동하는 용기에서 시작됩니다."
