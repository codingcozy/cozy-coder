---
title: "LLM에서 환각 감지하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_0.png"
date: 2024-07-13 03:41
ogImage: 
  url: /assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_0.png
tag: Tech
originalTitle: "How to Detect Hallucinations in LLMs"
link: "https://medium.com/towards-data-science/real-time-llm-hallucination-detection-9a68bb292698"
---


아니요, 이블린 하트웰(Evelyn Hartwell)은 여러 개의 가짜 신분을 사용하는 사기꾼이 아니며 여러 직업으로 속이고 삼중 삶을 살고 있는 것도 아닙니다. 사실, 그녀는 아예 존재하지 않지만, 대신 모델인데, 알지 못한다는 대신 사실을 지어내기 시작합니다. 우리는 LLM 환각과 달래야 합니다.

길고 상세한 결과물은 실제와 같아 보일 수 있습니다. 하지만 그것이 소설일지라도 믿게 만들 수는 있을까요? 다행히도 올바른 보호장치를 갖춘 경우, 챗봇이 거짓말을 하는 경향이 적어질 수도 있습니다.

![이미지1](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_1.png)

<div class="content-ad"></div>

위의 결과물들에 대해, 저는 온도를 0.7로 높게 설정했어요. 이렇게 하면 LLM이 각 세대마다 동일한 텍스트를 가지지 않게끔 문장 구조를 변경할 수 있어요. 출력물 간의 차이는 의미상의 것으로만 있을 거예요. 사실적인 것이 아닌 거죠.

이 간단한 생각은 새로운 샘플 기반의 환현 탐지 매커니즘을 소개할 수 있게 했어요. LLM이 동일한 프롬프트에 대한 출력물이 서로 모순된다면, 그것들은 환현일 가능성이 높아요. 서로를 함의한다면, 정보가 사실적일 확률이 높습니다.

이러한 종류의 평가를 위해서, 우리는 LLM의 텍스트 출력만 필요로 해요. 이를 블랙박스 평가라고 해요. 또한 외부 지식이 필요하지 않기 때문에 제로-리소스라고 불려요. 

# 문장 임베딩 코사인 거리

<div class="content-ad"></div>

아주 기본적인 유사성 측정 방법으로 시작해봅시다. 내장된 문장들 간의 대응하는 쌍의 코사인 유사도를 계산할 것입니다. 벡터의 방향에만 집중해야 하기 때문에 정규화합니다. 아래 함수는 원래 생성된 output 문장과 sampled_passages에 있는 3개의 샘플 출력 목록을 입력으로 받습니다. 모든 완성은 기사 첫머리에 있는 이미지에 있습니다.

임베딩 생성에는 all-MiniLM-L6-v2 경량 모델을 사용하고 있습니다. 문장을 임베딩하면 벡터 표현으로 변환됩니다.

```js
output = "Evelyn Hartwell is a Canadian dancer, actor, and choreographer." 
output_embeddings = model.encode(output)

array([ 6.09108340e-03, -8.73148292e-02, -5.30637987e-02, -4.41815751e-03,
 1.45469820e-02, 4.20340300e-02, 1.99541822e-02, -7.29453489e-02,
…
 -4.08893749e-02, -5.41420840e-02, 2.05906332e-02, 9.94611382e-02,
 -2.24501686e-03, 2.29083393e-02, 7.80007839e-02, -9.53456461e-02],
 dtype=float32)
```

LLM의 각 출력에 대한 임베딩을 생성한 후, sentence_transformers의 pairwise_cos_sim 함수를 사용하여 쌍별 코사인 유사성을 계산합니다. 원래 응답을 각각의 샘플 응답과 비교하고 평균을 낼 것입니다.

<div class="content-ad"></div>

```js
from sentence_transformers.util import pairwise_cos_sim
from sentence_transformers import SentenceTransformer

def get_cos_sim(output,sampled_passages):
    model = SentenceTransformer('all-MiniLM-L6-v2')
    sentence_embeddings = model.encode(output).reshape(1, -1)
    sample1_embeddings = model.encode(sampled_passages[0]).reshape(1, -1)
    sample2_embeddings = model.encode(sampled_passages[1]).reshape(1, -1)
    sample3_embeddings = model.encode(sampled_passages[2]).reshape(1, -1)
    cos_sim_with_sample1 = pairwise_cos_sim(
    sentence_embeddings, sample1_embeddings
    )
    cos_sim_with_sample2  = pairwise_cos_sim(
    sentence_embeddings, sample2_embeddings
    )
    cos_sim_with_sample3  = pairwise_cos_sim(
    sentence_embeddings, sample3_embeddings
    )
    cos_sim_mean = (cos_sim_with_sample1 + cos_sim_with_sample2 + cos_sim_with_sample3) / 3
    cos_sim_mean = cos_sim_mean.item()
    return round(cos_sim_mean,2)
```

이 함수가 어떻게 작동하는지에 대한 직관을 제공합니다. 매우 간단한 2D 직교 공간의 벡터 쌍을 사용합니다. A와 B는 원래 벡터이고, Â와 B̂는 정규화된 벡터입니다.

![image](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_2.png)

위 이미지에서 볼 수 있듯이, 벡터 사이의 각도는 약 30⁰로, 서로 가깝습니다. 코사인 값은 약 0.87입니다. 코사인 값이 1에 가까울수록 벡터들이 서로 가까이에 있습니다.

<div class="content-ad"></div>


cos_sim_score = get_cos_sim(output, [sample1,sample2,sample3])


우리의 임베드된 출력에 대한 cos_sim_score는 평균 값 0.52입니다.

이 숫자를 해석하는 방법을 이해하기 위해 이미 존재하는 사람에 대한 정보를 요청할 때 유효한 출력의 코사인 유사도 점수와 비교해 봅시다.

![HowtoDetectHallucinationsinLLMs_3](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_3.png)


<div class="content-ad"></div>

The pairwise cosine similarity score, in this case, is 0.93. Looks promising, especially as it’s a very fast method of assessing the similarity between outputs.

![](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_4.png)

# SelfCheckGPT- BERTScore

The BERTScore builds on the pairwise cosine similarity idea we implemented previously.

<div class="content-ad"></div>

![2024-07-13-HowtoDetectHallucinationsinLLMs_5.png](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_5.png)

안녕하세요! 로베르타 토크나이저는 문맥 임베딩을 계산하기 위한 기본 토크나이저입니다. 문맥 임베딩은 주변 문맥을 고려하기 때문에 정적 임베딩과 다릅니다. 예를 들어, "bat"이라는 단어는 주변 문맥이 '날아다니는 포유동물'을 의미하는지 '야구 배트'를 가리키는지에 따라 다른 토큰 값에 대응합니다.

아래는 `get_bertscore` 함수의 일부 코드입니다. 이 함수는 `output`과 `sampled_passages`를 입력으로 받아 BERT 점수를 계산합니다. 이때, 먼저 `output`을 개별 문장으로 분할하여 사용합니다.

자세한 내용은 블로그에서 자세히 확인해보세요! 🌟

<div class="content-ad"></div>

```js
['Evelyn Hartwell is an American author, speaker, and life coach.',
 'She is best known for her book, The Miracle of You: How to Live an Extraordinary Life, which was published in 2007.',
 'She is a motivational speaker and has been featured on TV, radio, and in many magazines.',
 'She has authored several books, including How to Make an Impact and The Power of Choice.']
```

This step is crucial for the selfcheck_bertscore.predict function to calculate the BERTScore for each sentence matched to the original response from the samples. Initially, it generates an array with the number of rows equal to the number of sentences in the original output and the number of columns equal to the number of samples.


[[0., 0., 0.],
[0., 0., 0.],
[0., 0., 0.],
[0., 0., 0.]]


The model employed for calculating the BERTScore between candidate and reference sentences is RoBERTa large with 17 layers. Our original output consists of 4 sentences, labeled as r1, r2, r3, and r4. The first sample contains two sentences: c1 and c2. We calculate the F1 BERTScore individually for each sentence from the original output corresponding to each sentence from the first sample. Subsequently, we perform base rescaling with respect to the baseline tensor b = tensor([0.8315, 0.8315, 0.8312]). The baseline b was derived using 1 million randomly paired sentences from the Common Crawl monolingual datasets. BERTScore was computed for each pair and then averaged, representing a lower limit as random pairings have minimal semantic overlap. [1]

<div class="content-ad"></div>

이미지를 Markdown 형식으로 바꿔주시겠어요?

BERTScore는 각 문장의 유사도를 계산하여 원본 응답과 각 그려진 샘플의 가장 유사한 문장의 점수를 유지합니다. 이 논리는 동일한 프롬프트에서 생성된 여러 샘플에 동일한 정보가 나타나면 해당 정보가 사실일 확률이 높다는 것입니다. 한 문장이 한 샘플에만 나타나고 동일한 프롬프트에서 다른 샘플에는 나타나지 않는 경우, 해당 문장은 고안된 것일 가능성이 높습니다.

첫 번째 샘플의 배열에서 최대 유사도를 추가해봅시다:

```js
bertscore_array
array([[0.43343216, 0. , 0. ],
[0.12838356, 0. , 0. ],
[0.2571277 , 0. , 0. ],
[0.21805632, 0. , 0. ]])
```

<div class="content-ad"></div>

이제 다음 두 샘플에 대해 동일한 과정을 반복합니다:

```js
array([[0.43343216, 0.34562832, 0.65371764],
       [0.12838356, 0.28202596, 0.2576825 ],
       [0.2571277 , 0.48610589, 0.2253703 ],
       [0.21805632, 0.34698656, 0.28309497]])
```

그런 다음 각 행의 평균을 계산하여 원래 응답의 각 문장과 각 후속 샘플 간의 유사도 점수를 얻습니다.

```js
array([0.47759271, 0.22269734, 0.32286796, 0.28271262])
```

<div class="content-ad"></div>

각 문장의 환각 점수는 위의 각 값을 1에서 뺀 값으로 얻습니다.

![HowtoDetectHallucinationsinLLMs_7](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_7.png)

결과를 니콜라스 케이지의 정답과 비교해보세요.

![HowtoDetectHallucinationsinLLMs_8](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_8.png)

<div class="content-ad"></div>

합리적인 발상인 것 같아요; 유효한 결과물에 대한 환각 점수는 낮고, 꾸며낸 결과물에 대한 점수는 높습니다. 하지만 BERTScore를 계산하는 과정은 매우 시간이 많이 소요되어 실시간 환각 탐지에는 부적절할 수 있어요.

![How to Detect Hallucinations in Large Language Models](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_9.png)

# SelfCheckGPT-NLI

자연어 추론(NLI)은 가설이 주어진 전제로부터 논리적으로 유도될 수 있는지 또는 상반되는지를 결정하는 작업을 포함합니다. 이 관계는 융통성(entailment), 모순(contradiction), 또는 중립(neutral)으로 분류됩니다. SelfCheck-NLI에서 우리는 MNLI 데이터셋에서 세분화된 DeBERTa-v3-large 모델을 활용하여 NLI를 수행해요.

<div class="content-ad"></div>

아래는 선행-가설 쌍과 레이블의 몇 가지 예시입니다.


![Hallucinations Pair](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_11.png)


```python
def get_self_check_nli(output, sampled_passages):
    # spacy 문장 토크나이제이션
    sentences = [sent.text.strip() for sent in nlp(output).sents] 
    selfcheck_nli = SelfCheckNLI(device=mps_device) # GPU를 사용할 수 있는 경우 장치를 'cuda'로 설정
    sent_scores_nli = selfcheck_nli.predict(
        sentences = sentences, # 문장 목록
        sampled_passages = sampled_passages, # 샘플링된 단락 목록
    )
    df = pd.DataFrame({
    '문장 번호': range(1, len(sent_scores_nli) + 1),
    '모순 확률': sent_scores_nli
    })
    return df
```

<div class="content-ad"></div>

selfcheck_nli.predict 함수에서는 원본 응답의 각 문장이 세 샘플 중 하나와 짝지어집니다.

```python
logits = model(**inputs).logits  # 중립은 이미 제거되었습니다
probs = torch.softmax(logits, dim=-1)
prob_ = probs[0][1].item()  # 확률(반대)
```

![image](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_12.png)

이제 우리는 네 문장 각각에 대해 이 프로세스를 반복합니다.

<div class="content-ad"></div>

이 모델은 모순의 매우 높은 확률을 출력하는 것을 볼 수 있습니다. 이제 사실적인 결과와 비교해 봅시다.

이 모델은 훌륭한 일을 하고 있습니다! 안타깝게도 NLI 체크에는 다소 시간이 걸립니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_15.png" />

# 셀프체크 GPT-Prompt

새로운 방법들이 시작되었어요. 최근 LLMs를 활용해 생성된 텍스트를 평가하는 방법들이 나타나고 있어요. 점수를 계산하는 수식을 사용하는 대신, 생성된 출력물을 gpt-3.5-turbo에 전송할 거예요. 모델은 원본 출력물이 다른 세 샘플과 얼마나 일관성 있는지를 결정할 거에요. [3]

```js
def llm_evaluate(sentences,sampled_passages):
    prompt = f"""당신에게 텍스트 단락이 제공될 거에요. 주어진 문맥과 그 텍스트의 일관성을 평가하는 것이 당신의 과제예요. 당신의 답변은 반드시 0.0부터 1.0까지의 숫자여야 해요. 소수점 둘째 자리에서 반올림하여 표현되며, 0.0은 일치성이 없음을, 1.0은 완벽한 일치성과 유사성을 나타내요. \n\n \
                텍스트 단락: {sentences}. \n\n \
                문맥: {sampled_passages[0]} \n\n \
                {sampled_passages[1]} \n\n \
                {sampled_passages[2]}."""

    completion = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": ""},
        {"role": "user", "content": prompt}
    ]
    )

    return completion.choices[0].message.content
```

<div class="content-ad"></div>

이블린 하트웰에 대한 자기 유사성 점수는 0입니다. 반면, 니콜라스 케이지에 관련된 결과물의 점수는 0.95입니다. 이 점수를 얻는 데 필요한 시간도 꽤 짧습니다.

![이미지](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_16.png)

이것은 우리 사례에 가장 적합한 해결책으로 보입니다. 우리는 소스 논문의 비교 분석에서도 이를 기대하고 있었습니다. SelfCheckGPTPrompt는 모든 다른 방법들을 크게 앞서나가며, NLI가 두 번째로 성능이 우수한 방법입니다.

![이미지](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_17.png)

<div class="content-ad"></div>

평가 데이터셋은 WikiBio 데이터셋과 GPT-3를 사용하여 합성 위키피디아 기사를 생성함으로써 만들어졌어. 이때, 이해하기 어려운 개념을 피하기 위해 가장 긴 기사의 상위 20%에서 무작위로 238개 기사 주제가 샘플링되었어. 각 개념에 대해 GPT-3에게 위키피디아 스타일의 첫 번째 단락을 생성하도록 했지.

![image](/assets/img/2024-07-13-HowtoDetectHallucinationsinLLMs_18.png)

이후, 생성된 단락은 문장 수준에서 사실성을 수동으로 주석을 달았어. 각 문장은 사전에 정의된 가이드라인을 기반으로 큰 오차가 있는, 작은 오차가 있는, 또는 정확한 중 하나로 레이블이 붙여졌어. 총 1908개의 문장이 주석이 달렸는데, 약 40%는 주요 오차, 33%는 작은 오차, 27%는 정확했어.

주석 작업자 간의 일치도를 확인하기 위해 201개 문장에는 이중 주석이 있었어. 주석 작업자가 동의했다면 해당 레이블을 사용하고, 그렇지 않으면 최악의 경우 레이블을 선택했어. Cohen의 카파로 측정한 주석 작업자 간의 일치도는 정확함, 작은 오차, 주요 오차 사이를 선택할 때 0.595이고, 작은/주요 오차를 하나의 레이블로 합칠 때 0.748이었어.

<div class="content-ad"></div>

평가 메트릭인 AUC-PR은 Precision-Recall 곡선 아래 영역을 가리키며, 이는 분류 모델을 평가하는 데 사용하는 메트릭입니다.

# 실시간 환각 탐지

마지막으로, 실시간 환각 탐지를 위한 Streamlit 앱을 만들어 봅시다. 이전에 언급했듯이, 최적의 메트릭은 LLM 자기 유사성 점수입니다. 0.5의 임계값을 사용하여 생성된 출력을 표시할지 또는 고지서를 표시할지 결정하겠습니다.

```python
import streamlit as st
import utils
import pandas as pd

# Streamlit 앱 레이아웃
st.title('환각 방지 챗봇')

# 텍스트 입력
user_input = st.text_input("텍스트를 입력하세요:")

if user_input:

    prompt = user_input

    output, sampled_passages = utils.get_output_and_samples(prompt)

    # LLM 점수
    self_similarity_score = utils.llm_evaluate(output, sampled_passages)

    # 출력 표시
    st.write("**LLM 출력:**")
    if float(self_similarity_score) > 0.5:
        st.write(output)
    else:
        st.write("죄송하지만, 귀하의 질문에 정확히 대답할 수 있는 특정 정보가 없습니다.")
```

<div class="content-ad"></div>

이제 최종 결과를 시각화할 수 있습니다.

![Image](https://miro.medium.com/v2/resize:fit:1400/1*YG5aC7EKqvkQx31_9wi2KQ.gif)

# 결론

결과는 매우 유망합니다! 챗봇에서의 환각 감지는 오랫동안 논의되어 온 품질 문제입니다.

<div class="content-ad"></div>

이 글에서 소개된 기법들이 흥미로운 이유는 다른 LLM의 출력물을 평가하기 위해 LLM을 사용하는 혁신적인 방법론입니다. 특히 동일한 프롬프트에 대해 여러 응답을 생성하고 그들의 일관성을 비교하는 과정이 이루어집니다.

더 많은 연구가 필요하지만, 인간의 평가나 수작업 규칙에 의존하는 대신, 모델 자체가 일관성의 부족을 잡아내는 것이 좋은 방향으로 보입니다.

. . .

이 글을 즐겨보셨다면, Text Generation에 가입해보세요 - 저희 뉴스레터는 매주 두 편의 게시물을 통해 창조적 AI 및 대형 언어 모델에 대한 최신 통찰을 제공합니다.

<div class="content-ad"></div>

GitHub에서 이 프로젝트의 전체 코드를 찾을 수 있어요.

또한 LinkedIn에서도 저를 만날 수 있어요.

. . .

# 참고문헌:

<div class="content-ad"></div>

- BERTSCORE: BERT를 활용한 텍스트 생성 평가
- SELFCHECKGPT: 생성형 대형언어모델의 Zero-Resource 블랙박스 환각 탐지
- [Qualtiy and Safety of Large Language Model Applications](https://learn.deeplearning.ai/quality-safety-llm-applications)
- 추론을 통한 문장 이해를 위한 광범위한 도전 코퍼스
- [다운로드 링크1](https://drive.google.com/file/d/13LUBPUm4y1nlKigZxXHn7Cl2lw5KuGbc/view)
- [다운로드 링크2](https://drive.google.com/file/d/1EzQ3MdmrF0gM-83UV2OQ6_QR1RuvhJ9h/view)