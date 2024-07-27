---
title: "이미지에서 텍스트 요약까지 Tesseract와 BART로 텍스트 추출 및 요약하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-FromImagetoSummaryUsingTesseractandBARTforTextExtractionandSummary_0.png"
date: 2024-07-07 22:46
ogImage: 
  url: /assets/img/2024-07-07-FromImagetoSummaryUsingTesseractandBARTforTextExtractionandSummary_0.png
tag: Tech
originalTitle: "From Image to Summary: Using Tesseract and BART for Text Extraction and Summary"
link: "https://medium.com/@bambadij/from-image-to-summary-using-tesseract-and-bart-for-text-extraction-and-summary-b74e6eb77f0b"
---


이미지 태그를 Markdown 형식으로 변경해주세요.

---

> ![image](/assets/img/2024-07-07-FromImagetoSummaryUsingTesseractandBARTforTextExtractionandSummary_0.png)

데이터 중심적인 세상에서 정보를 빠르고 효율적으로 추출하고 요약하는 능력은 중요합니다. 이 글은 이미지에서 텍스트 추출을 위한 Tesseract와 텍스트 합성 및 요약을 위한 Facebook의 BART(Bidirectional and Auto-Regressive Transformers) 기술의 활용을 탐구합니다. 이러한 기술들을 Gradio 인터페이스에 통합하여 텍스트 추출과 요약 과정을 간단하고 자동화하는 방법을 보여드릴 것입니다.

라이브러리 설치

프로젝트 설정을 위해 다음 단계를 따라주세요.

<div class="content-ad"></div>

- 1 가상 환경 생성하기

가상 환경을 만드는 것은 의존성을 관리하고 깨끗한 작업 공간을 유지하는 데 도움이 됩니다.


python -m venv env
source env/bin/activate  # 윈도우의 경우, `env\Scripts\activate`를 사용하세요


- 2 requirements.txt 파일 생성하기

<div class="content-ad"></div>

## Requirements.txt


```js
tesseract==4.41.2
transformers==4.9.2
gradio==2.4.7
torch==2.3.1
numpy==1.23.5
pillow==10.3.0
pytesseract==0.3.10
protobuf
```

## Installation

To install the necessary libraries, run the following command:

```bash
pip install -r requirements.txt
```

<div class="content-ad"></div>

이제 파이썬 애플리케이션 파일 app.py를 설정해봅시다. 필요한 import와 함수들을 추가해주세요.

```python
import pytesseract
from transformers import pipeline, BartForConditionalGeneration, BartTokenizer
import gradio as gr
from PIL import Image, UnidentifiedImageError
import re
import logging
```

모델을 초기화합니다:

<div class="content-ad"></div>

BART를 초기화했어요. hugging에서 파이프라인을 사용했어요.

```js
summarize =pipeline('summarization',model="facebook/bart-large-cnn")
```

이미지에서 텍스트를 추출하는 함수를 만들어보세요.

<div class="content-ad"></div>

```python
def image_extract(image):
    try:
        if image is None:
            return "이미지를 찾을 수 없습니다."
        raw_text = pytesseract.image_to_string(image)
        preprocessing = re.sub(r'\s+',' ', raw_text).strip()
        text_summary = summarize(preprocessing, do_sample=False, min_length=50, max_length=512)
        summary_text_from_image = text_summary[0]['summary_text']

        return summary_text_from_image
    except UnidentifiedImageError:
        return "이미지를 불러오는 중 오류가 발생했습니다."
    except Exception as e:
        return str(e)
```

**Gradio 인터페이스**

- Gradio 인터페이스는 이미지 입력과 파이프라인 또는 직접적인 모델/토크나이저 상호작용을 선택하는 체크박스를 제공합니다.
- 선택한 방법에 따라 추출된 텍스트와 요약된 버전을 표시합니다.

Gradio 인터페이스를 생성하세요.


<div class="content-ad"></div>


안녕하세요! 이 아직 새로운 프로젝트입니다. 더 궁금한 점이 있으시면 언제든지 물어주세요! 😊

**용어 해석위주로 한국어로 변경된 문장:**

인터페이스 = gr.Interface(
fn=image_extract,
inputs=gr.Image(label="텍스트 추출을 위한 파일 업로드", type='pil'),
outputs=gr.Textbox(label="요약"),
title="이미지에서 요약으로: Bart et Tesseract 사용"
)

if __name__=="__main__":
interface.lunch()


어플리케이션 실행:

```python
python.app
```

웹 브라우저 열고 127.0.0.1:7860을 입력하세요. 🌟


<div class="content-ad"></div>

![image](/assets/img/2024-07-07-FromImagetoSummaryUsingTesseractandBARTforTextExtractionandSummary_1.png)

결론

이 프로젝트는 OCR 기술과 고급 NLP 모델을 결합하여 대량의 텍스트 데이터를 효율적으로 처리할 수 있는 잠재력을 보여줍니다. Gradio와 같은 접근 가능한 도구를 제공함으로써, 본 어플리케이션은 이러한 기술이 다양한 분야에서 데이터 기반 워크플로를 간소화하고 강화하는 데 어떻게 활용될 수 있는지 보여줍니다.

나의 GitHub 링크: