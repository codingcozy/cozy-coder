---
title: "BioASQ 팩토이드 질문을 SQuAD 형식으로 변환하는 사전 처리 방법"
description: ""
coverImage: "/assets/img/2024-07-07-TransformingBioASQFactoidQuestionsintoSQuADFormatAPreprocessingApproach_0.png"
date: 2024-07-07 03:32
ogImage:
  url: /assets/img/2024-07-07-TransformingBioASQFactoidQuestionsintoSQuADFormatAPreprocessingApproach_0.png
tag: Tech
originalTitle: "Transforming BioASQ Factoid Questions into SQuAD Format: A Preprocessing Approach"
link: "https://medium.com/@ahmedajminenehal/transforming-bioasq-factoid-questions-into-squad-format-a-preprocessing-approach-a43a884eeb98"
isUpdated: true
---

![TransformingBioASQFactoidQuestionsintoSQuADFormatAPreprocessingApproach](/assets/img/2024-07-07-TransformingBioASQFactoidQuestionsintoSQuADFormatAPreprocessingApproach_0.png)

BioASQ는 생물 의학 질문에 대한 기계 학습 모델을 교육하고평가하는 데 널리 사용되는 대규모 질문-응답 데이터 세트입니다. BioASQ를 인기있는 SQuAD(Stanford Question Answering Dataset) 형식으로 변환하는 것은 데이터 세트의 구조와 표현의 차이로 인해 도전적일 수 있습니다. 이 글은 BioASQ 사실 질문 데이터 세트를 SQuAD 형식으로 전처리하는데 사용되는 Python 스크립트를 제공합니다.

# 스크립트 개요

제공된 Python 스크립트는 JSON 파일에서 BioASQ 데이터를 읽고 PubMed ID(PMID)를 사용하여 PubMed에서 초록과 제목을 가져와 "context" 필드를 구성하고 데이터를 SQuAD 형식으로 구조화합니다.

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

## 코드의 자세한 분석

- Imports 및 Dependencies:

해당 스크립트는 필요한 라이브러리를 가져와 시작됩니다:

```js
import json
import requests
import xml.etree.ElementTree as ET
import time
import os
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

**json**은 JSON 파일을 읽고 쓰는 데 사용됩니다. **requests**는 PubMed API에 HTTP 요청을 보내는 데 사용됩니다. **xml.etree.ElementTree**는 XML 응답을 구문 분석하는 데 사용됩니다. **time**은 서버를 과부하시키지 않도록 API 요청 사이에 지연을 추가하는 데 사용됩니다. **os**는 파일 경로 조작에 사용됩니다.

**2. PubMed 요약문 및 제목 가져오기:**

함수 **fetch_pubmed_abstract_and_title(pmid)**은 PMID를 사용하여 주어진 PubMed 논문의 요약문과 제목을 가져오는 역할을 합니다. PubMed API를 위한 URL을 구성하고 요청을 보내며 XML 응답을 구문 분석하여 제목과 요약문을 추출합니다.

```python
def fetch_pubmed_abstract_and_title(pmid):
    url = f"https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=pubmed&id={pmid}&retmode=xml"
    try:
        response = requests.get(url)
        response.raise_for_status()  # 잘못된 상태 코드에 대해 예외를 던집니다.

        try:
            root = ET.fromstring(response.content)
            abstract_text = root.find(".//AbstractText")
            article_title = root.find(".//ArticleTitle")
            if abstract_text is not None and article_title is not None:
                context = f"{article_title.text}. {abstract_text.text}"
                return context
            else:
                print(f"PMID {pmid}에 대해 요약문 또는 제목을 찾을 수 없습니다.")
                return ""
        except ET.ParseError as e:
            print(f"PMID {pmid}에 대한 XML 구문 분석 오류: {str(e)}")
            print(f"응답 내용: {response.content[:500]}...")  # 처음 500자 출력
            return ""
    except requests.RequestException as e:
        print(f"PMID {pmid}에 대한 요청 오류: {str(e)}")
        return ""
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

- PubMed API URL을 제공된 PMID를 사용하여 생성합니다.
- PubMed API로 HTTP GET 요청을 보내고 오류를 확인합니다.
- 요청이 성공하면 XML 응답을 구문 분석하여 요약 및 제목을 추출합니다.
- 요약과 제목 모두를 찾으면 이를 연결하여 컨텍스트를 형성하고 반환합니다.
- 오류가 발생하면(요청 또는 구문 분석), 오류 메시지를 출력하고 빈 문자열을 반환합니다.

3. BioASQ를 SQuAD로 전처리하기:

preprocess_bioasq_to_squad(input_json) 함수는 입력 JSON을 읽고 각 질문을 처리하며 해당 요약을 가져와 데이터를 SQuAD 형식으로 변환합니다. 사실형 질문을 처리하고, PMCID를 추출하고, 데이터를 적절하게 구조화하는 논리가 포함되어 있습니다.

```python
def preprocess_bioasq_to_squad(input_json):
    # 입력 JSON 파싱
    data = json.loads(input_json)

    # SQuAD 형식 구조 초기화
    squad_format = {
        "version": "v1.0",
        "data": [{
            "title": "BioASQ Dataset",
            "paragraphs": []
        }]
    }

    # 데이터셋에 있는 각 질문 처리
    for question in data['questions']:
        # 사실형 질문만 처리
        if question['type'] != 'factoid':
            continue

        # 각 문서 URL 처리
        for doc_url in question['documents']:
            # URL에서 PMCID 추출
            pmcid = doc_url.split('/')[-1]

            # PubMed에서 요약과 제목 가져오기
            context = fetch_pubmed_abstract_and_title(pmcid)

            if not context:
                print(f"빈 요약 또는 제목으로 인해 PMCID {pmcid} 건너뜁니다")
                continue  # 요약 또는 제목이 없는 경우 건너뜁니다

            # 답변 처리
            exact_answers = question['exact_answer']
            if not isinstance(exact_answers, list):
                exact_answers = [exact_answers]

            # 중첩된 목록 평평화
            flat_answers = []
            for answer in exact_answers:
                if isinstance(answer, list):
                    flat_answers.extend(answer)
                else:
                    flat_answers.append(answer)

            # 각 답변에 대해 별도의 QA 쌍 생성
            for i, answer in enumerate(flat_answers):
                paragraph = {
                    "context": context,
                    "qas": [{
                        "id": f"{question['id']}_{pmcid}_{i}",
                        "question": question['body'],
                        "answers": [],
                        "is_impossible": True
                    }]
                }

                # 컨텍스트에서 답변의 시작 위치 찾기
                answer_start = context.lower().find(answer.lower())
                if answer_start != -1:
                    paragraph['qas'][0]['answers'].append({
                        "text": answer,
                        "answer_start": answer_start
                    })
                    paragraph['qas'][0]['is_impossible'] = False

                # 주 서식의 데이터셋에 단락 추가
                squad_format['data'][0]['paragraphs'].append(paragraph)

            # PubMed 서버에 과다한 요청을 피하기 위해 약간의 지연 추가
            time.sleep(0.5)

    return json.dumps(squad_format, indent=2)
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

- BioASQ JSON 데이터를 읽는 기능입니다.
- SQuAD 형식 데이터를 보유할 사전을 초기화합니다.
- 데이터 세트의 각 질문에 대해 해당 질문이 사실형인지 확인합니다.
- 질문과 관련된 각 문헌 URL에 대해 PMID를 추출하고 PubMed에서 요약과 제목을 가져옵니다.
- 정확한 답변을 처리하고 중첩된 답변 목록을 평평하게 합니다.
- 각 답변에 대해 QA 쌍을 만들고 컨텍스트에서 답변의 시작 위치를 찾아 단락 구조를 업데이트합니다.
- PubMed 서버를 압도하지 않도록 요청 간에 작은 지연을 추가합니다.
- 기능은 SQuAD 형식의 처리된 데이터를 JSON 문자열로 반환합니다.

4. 입력 받기 및 출력 쓰기:

스크립트는 BioASQ 입력 JSON 파일을 읽어 preprocess_bioasq_to_squad 함수를 사용하여 처리하고 변환된 데이터를 새 JSON 파일로 씁니다.

```python
# 파일에서 입력 JSON 읽기
input_file_path = '/content/test_process.json'
with open(input_file_path, 'r') as file:
    input_json = file.read()

# 데이터 전처리
squad_json = preprocess_bioasq_to_squad(input_json)

# 출력 파일명 결정
input_file_name = os.path.basename(input_file_path)
output_file_name = os.path.splitext(input_file_name)[0] + '_preprocessed.json'
output_file_path = os.path.join(os.path.dirname(input_file_path), output_file_name)

# 전처리된 데이터를 새 파일에 쓰기
with open(output_file_path, 'w') as file:
    file.write(squad_json)

print(f"전처리 완료. 출력은 '{output_file_path}'에 저장되었습니다.")
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

- 입력 JSON 파일에는 BioASQ 데이터가 포함되어 있습니다.
- 데이터를 SQuAD 형식으로 변환하기 위해 preprocess_bioasq_to_squad 함수를 호출합니다.
- 출력 파일 이름은 입력 파일 이름을 기반으로 결정됩니다.
- 변환된 데이터를 새 JSON 파일에 작성합니다.
- 출력 파일의 위치를 나타내는 메시지를 출력합니다.

## 사용자 정의

이 스크립트를 사용자 정의하려면 다음을 수행할 수 있습니다:

- fetch_pubmed_abstract_and_title 함수에서 URL 및 요청 매개변수를 수정하여 추가 데이터를 가져오거나 다른 API를 사용해야 하는 경우 수정합니다.
- preprocess_bioasq_to_squad 함수에서 질문과 답변이 처리되는 방식을 조정하여 다양한 유형의 질문이나 추가 답변 형식을 처리하도록 조정합니다.
- 파일 경로 및 입력/출력 처리를 환경이나 작업 흐름에 맞게 변경합니다.

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

## 결론

이 글은 BioASQ 데이터셋을 SQuAD 형식으로 변환하는 파이썬 스크립트에 대해 자세히 설명합니다.

## 자료:

Google Collab 링크: [여기를 클릭해주세요](https://colab.research.google.com/drive/1ehhfsvRkAS_SKZbPbKloV3ndEXWkmVI8?usp=sharing)
