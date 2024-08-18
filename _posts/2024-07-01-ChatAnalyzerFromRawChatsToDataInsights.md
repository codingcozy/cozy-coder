---
title: "채팅 분석기  원시 채팅 데이터를 인사이트로 변환하는 방법"
description: ""
coverImage: "/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_0.png"
date: 2024-07-01 00:38
ogImage:
  url: /assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_0.png
tag: Tech
originalTitle: "Chat Analyzer — From Raw Chats To Data Insights"
link: "https://medium.com/towards-artificial-intelligence/chat-analyzer-from-raw-chats-to-data-insights-d6dcbb2db1fa"
isUpdated: true
---

## 데이터 앱 개발 리뷰

![image](https://miro.medium.com/v2/resize:fit:924/1*tDyyvh3fnFdZ9Zo070ZIFA.gif)

## 목차

- 이 글을 쓴 이유
- 어떻게 작동하는가?
- 데이터 정규화
- A 섹션: 기본 통계
- B 섹션: 사용자 수준 분석
  - 가장 빈도 높은 이모티콘 (최다 빈도 N)
  - 가장 관련된 이모티콘 (클래스 기반 TF-IDF)
- C 섹션: 텍스트 분석
  - 채팅 탐색기
  - 대화 요약 도구
- D 섹션: 지리적 정보
- 결론

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

# Why Did I Write This Article?

안녕하세요! 여러분은 채팅 데이터가 어떤 비밀을 품고 있는지 궁금해 해 보셨나요? (예: Whatsapp / Telegram/ 등등...) 전 궁금했고, 온라인 옵션들이 기본적이고 제한적이라는 걸 알았습니다. 그래서 당연히 제가 직접 채팅 분석 앱을 만들기로 결심했죠.

Flask & Dash와의 긴 싸움 끝에, 더 간단한 프레임워크를 찾기로 결심하고 Streamlit.io를 우연히 발견했습니다 — 공개적이고 무료인 데이터 앱을 공유할 수 있는 프레임워크입니다. Streamlit의 미묘한 점을 익히는 데 시간이 좀 걸렸지만, 알아낸 후에는 개발 과정이 순조롭고 쉬워졌어요!

본문의 목적은 다른 사람들에게 Streamlit 데이터 앱 개발을 소개하는 것입니다. 전반적으로 앱 섹션에 대해 알려주며, 앱의 흐름, NLP 방법론 및 시각화 기법 등 다양한 측면을 논의하기 위해 선택된 위젯에 초점을 맞출 거예요.

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

Before we dive into the details, let me share a couple of notes with you:

- The Chat Analyzer is a non-profit app designed for entertainment purposes only. The code is open-source and can be found [here](link).
- Rest assured, your data is safe with us! Once you upload it to the app, we do not store or share it elsewhere. Your data is deleted once your session ends.
- In this article, I will showcase the capabilities of the Chat Analyzer using a public WhatsApp export from the Indian Developers’ group in 2020, generously shared on GitHub by Tushar Nankani. You can check it out [here](link).
- Please note that this article will only touch upon a fraction of what the Chat Analyzer can do. If you want to explore all its features, give it a try [here](link).

# How does it work?

Anyone can easily export their chat data (check out the instructions for WhatsApp & Telegram). The Chat Analyzer is a powerful tool that takes these exports, standardizes them into a unified format, and creates insightful widgets based on the processed data.

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

# 데이터 정규화

![Chat Analyzer](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_0.png)

각 채팅 플랫폼은 데이터 형식과 스키마가 다릅니다. 예를 들어, WhatsApp은 txt를 지원하고, Telegram은 html/json을 지원합니다. 정규화 단계에서는 데이터 내보내기를 통해 받은 데이터를 normalization 함수로 전달하여 pandas 데이터 프레임을 출력합니다. WhatsApp 파일의 정규화 로직은 다음과 같습니다:

```python
from chatminer.chatparsers import WhatsAppParser
import streamlit as st
import tempfile
import os

def normalize_whatsapp_data(chat_export):

  # 데이터를 로컬로 쓰기
  tempdir = tempfile.mkdtemp(prefix='chat_exports')
  with open(os.path.join(tempdir, 'chat_export.txt'), mode='wb') as f:
      f.write(chat_export.read())

  # whatsapp txt 파일 파싱
  parser = WhatsAppParser(os.path.join(tempdir, 'chat_export.txt'))
  parser.parse_file()

  # 메타데이터 추가
  df = parser.parsed_messages.get_df(as_pandas=True).rename(columns={'author':'username'})
  df['date'] = df['timestamp'].dt.date
  df['hour'] = df['timestamp'].dt.floor('h').dt.strftime("%H:%M")
  df['week'] = df['timestamp'].dt.to_period('W').dt.start_time
  df['month'] = df['timestamp'].to_numpy().astype('datetime64[M]')
  df['day_name'] = df['timestamp'].dt.day_name()
  df['is_media'] = df['message'].str.contains('<Media omitted>')
  df['text_length'] = df['message'].str.split().map(len)

# WhatsApp 내보내기 파일 업로드
uploaded_file = st.file_uploader("파일을 업로드하세요")
if uploaded_file:
  df = normalize_whatsapp_data(uploaded_file)
  st.session_state['data'] = df
  df
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

![image](https://miro.medium.com/v2/resize:fit:1400/1*ypVKOYKO8RYUifmv3cVtYg.gif)

지금 st.session_state에 채팅 데이터가 pandas 데이터 프레임으로 저장되어 있으므로, 손쉽게 액세스하여 대화를 확인하고 대화를 대화로 나눌 수 있습니다. 앱의 각 섹션은 채팅 그룹의 다양한 측면을 탐색할 수 있는 다른 시각을 제공하며, 사용자로 하여금 데이터의 다양한 측면을 발견할 수 있게 합니다.

st.sidebar를 사용하여 모든 섹션에서 데이터에 기간 간격 또는 선택한 사용자와 같은 다양한 필터를 적용할 수 있습니다.

# Section A: 기본 통계

기본 통계 섹션은 전체적인 활발한 날짜/사용자, 시간별 활동, 사용자 활동 비율 등과 같은 고수준 통찰력을 밝히기 위해 설계되었습니다. 이 섹션의 위젯 중 하나는 "시간에 따른 활동" 그래프입니다. 이 그래프는 # 메시지/사용자의 추세선을 보여줍니다.

Streamlit은 Plotly 시각화를 지원하며, 우리는 여기서 그것을 사용할 것입니다. 다음 스크립트는 다양한 단위 (메시지/사용자) 및 다양한 시간 단위 (월별/주별/일별)를 지원하면서 추세선 그래프를 생성합니다.

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
import streamlit as st
import plotly.express as px

def generate_activity_overtime(df, unit, granularity):
    unit_dict = {'Messages': 'count', 'Users': 'nunique'}
    agg_df = df.groupby(granularity, as_index=False).agg({'username': unit_dict[unit]}).reset_index() \
        .rename(columns={'username': f'# {unit}', 'index': granularity.capitalize()})
    fig = px.line(agg_df, x=granularity.capitalize(), y=f'# {unit}')
    fig['data'][0]['line']['color'] = "#24d366"
    fig.update_layout(paper_bgcolor="rgba(18,32,43)", plot_bgcolor="rgba(18,32,43)", hovermode="x",
                      title_text='Overall Chat Activity Over Time')
    return fig

df = st.session_state['data']
unit = st.selectbox("Messages / Users", ("Messages", "Users"))
sub0, sub1, sub2 = st.columns(3)
sub0.plotly_chart(generate_activity_overtime(df, unit, 'month'))
sub1.plotly_chart(generate_activity_overtime(df, unit, 'week'))
sub2.plotly_chart(generate_activity_overtime(df, unit, 'date"))
```

![Chat Analyzer](https://miro.medium.com/v2/resize:fit:1400/1*E_vl6xEYXTnDYMUSqvuelg.gif)

# Section B: 사용자 수준 분석

![Chat Analyzer Image](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_1.png)

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

사용자 레벨 섹션은 그룹 참여자들에 관한 이야기를 전할 수 있게 해줍니다. 전체 메시지 중 일부 기본 채팅 사용자 수치와 N 단어 같은 숫자를 보여줍니다만, "최고 이모지" 같은 좀 더 복잡한 아이디어도 포함하고 있습니다.

이제 최고 이모지 기능을 열어 봅시다 — 이모지들은 특히 채팅 그룹에서 매우 중요한 부분이 되었습니다. 사람들은 이모지를 각자의 방식으로 사용하곤 하는데, 이를 분석해 볼 수 있습니다.
첫 번째 단계에서는 데이터를 전처리하여 텍스트를 이모지만으로 필터링하고 사용자 레벨로 집계할 것입니다.

```js
import streamlit as st
import emoji

df = st.session_state['data']
df['emojis_list'] = df['message'].map(emoji.distinct_emoji_list)
emoji_df = df[df['emojis_list'].apply(len) > 0].groupby('username', as_index=False).agg({'emojis_list': 'sum'})
emoji_df.head()
```

![ChatAnalyzerFromRawChatsToDataInsights_2](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_2.png)

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

다음으로는 각 채팅 참가자를 위한 최고 이모티콘을 선택할 방법론을 고안해야 합니다. 저는 두 가지를 생각해 냈습니다 — 가장 빈도 높은 이모티콘과 가장 연관된 이모티콘.

**가장 빈도 높은 이모티콘 (상위 Freq N)**

단순한 방식: 채팅 사용자 수준에서 이모티콘의 등장 횟수를 세어 가장 많이 나온 것을 선택하는 것입니다. 이 방법은 쉽게 적용할 수 있지만 한 가지 큰 단점이 있습니다 — 모든 참가자가 대부분의 이모티콘을 많이 사용하는 경우, 그 결과가 모든 참가자에게 동일한 최고 이모티콘을 도출할 수 있습니다.

**가장 연관된 이모티콘 (클래스 기반 TF-IDF)**

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

이 접근 방식은 다소 복잡합니다. 일반적으로 여러 클래스(우리 경우 사용자들)에 적용되는 TF-IDF 공식을 사용하며, 이는 사용자들의 이모티콘 발생 빈도를 모든 다른 사용자의 이모티콘 발생 빈도에 대비하여 고려합니다. c-TF-IDF에 대해 더 알고 싶다면 여기를 확인해보세요.

아래 코드는 두 가지 접근 방식을 이모지 데이터프레임(emoji_df)에 적용하고 사용자 수준의 데이터프레임을 생성합니다. 방법론 간의 차이를 쉽게 확인할 수 있습니다.

```python
import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer
from bertopic.vectorizers._ctfidf import ClassTfidfTransformer

# 이모지 데이터프레임(emoji_df)를 BOW(bag of words)로 변환
def dummy(doc):
    return doc
vectorizer = CountVectorizer(tokenizer=dummy, preprocessor=dummy)
X = vectorizer.fit_transform(emoji_df['emojis_list'])
emojies = vectorizer.get_feature_names_out()

# 가장 빈도가 높은 이모지
bow_df = pd.DataFrame(X.toarray(), columns=emojies, index=emoji_df['username'])
top_freq_emoji = pd.DataFrame(bow_df.idxmax(axis=1)).reset_index() \
    .rename(columns={0: 'Most Frequent Emoji'})

# c-TF-IDF(가장 연관된 이모지)
ctfidf = ClassTfidfTransformer(reduce_frequent_words=True).fit_transform(X).toarray()
words_per_class_min = {user_name: [emojies[index] for index in ctfidf[label].argsort()[-1:]]
                        for label, user_name in zip(emoji_df.username.index, emoji_df.username)}
top_ctfidf_emoji = pd.DataFrame(words_per_class_min).T.reset_index() \
    .rename(columns={0: 'Most Associated Emoji', 'index': 'username'})
top_ctfidf_emoji.merge(top_freq_emoji, on='username')
```

![ChatAnalyzerFromRawChatsToDataInsights_3](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_3.png)

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

# 섹션 C: 텍스트 분석

![Chat Analyzer](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_4.png)

텍스트 분석 섹션은 채팅에서 가장 중요한 부분인 텍스트 메시지를 다룹니다. 현재 2개의 위젯이 있는데, 채팅 탐색기와 대화 요약기입니다.

## 채팅 탐색기

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

![tarot](https://miro.medium.com/v2/resize:fit:1400/1*2vXesyHuYhOQt0z20DQwrw.gif)

위젯은 채팅의 텍스트 필터링을 사용하여 트렌드와 패턴을 찾는 탐색 도구입니다. 이 아이디어는 기본 통계 그래프를 보여준다는 점에서 출발했지만, 이번에는 텍스트 필터링 기능이 추가되었습니다. 채팅과 그래프를 나란히 보여주기 위해 st.columns를 사용하여 왼쪽에 필터링된 테이블을 할당하고, 오른쪽에는 추세선 그래프를 표시했습니다.

분석한 채팅 그룹의 토론은 주로 개발 문제를 중심으로 하지만, 역사적으로 중요한 공개 이벤트가 반영될 가능성이 높습니다. 예를 들어, COVID-19과 관련된 용어로 구성된 다음 쿼리를 보십시오. 채팅에 이를 적용하면 COVID-19 발생과 그에 대한 메시지와의 상관 관계를 확인할 수 있습니다.
쿼리: covid|corona|virus|quarantine|lockdown

```js
import streamlit as st

df = st.session_state['data']
text_query = st.text_input('텍스트로 검색')
filtered_df = df[df['message'].str.lower().str.contains(text_query.lower())]
col0, col1 = st.columns((5, 5))
with col0:
    st.dataframe(filtered_df, hide_index=True)
with col1:
    st.plotly_chart(generate_activity_overtime(filtered_df, "메시지", "월"))
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

![Conversations Summarizer](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_5.png)

## 대화 요약기

지금까지 우리는 대화 텍스트를 전통적 방법으로 다루어 왔는데, 이제는 무거운 무기를 꺼내 볼 때입니다! 딥 러닝이 어떻게 대화 요약이라는 짧은 단락으로 대화를 요약하는 데 도움이 될 수 있는지 살펴봅시다.

대화를 대화로 나누기
대화를 분할하는 일반적인 방법론을 찾는 것은 채팅 그룹 간에 채팅 행동이 변하기 때문에 어려운 과제입니다. 여기서는 메시지의 시간 차이를 분할 매개변수로 사용하는 소박한 방법론을 제안했습니다. 이제 인접 메시지 간 시간 차이에 대한 간단한 분석을 해보겠습니다.

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
import streamlit as st
import seaborn as sns
import matplotlib.pyplot as plt

# 데이터프레임에 time_diff_minutes 추가
df = st.session_state['data'].sort_values('timestamp')
df = df.join(df[['timestamp']].shift(-1), lsuffix='', rsuffix='_prev')
df['time_diff_minutes'] = ((df['timestamp_prev'] - df['timestamp']).dt.seconds / 60)
# 시간 차이 Cumulative Distribution Function 그리기
sns.ecdfplot(df['time_diff_minutes'])
offset_factor = 0.95
plt.title('메시지 시간 차이'), plt.axhline(0.90, color='r'), plt.ylabel('% of Messages');
plt.text(22*offset_factor, 0.9*offset_factor, f'22 분', ha='center', va='center', color='black')
```

![Chat Analyzer](/assets/img/2024-07-01-ChatAnalyzerFromRawChatsToDataInsights_6.png)

메시지의 시간 차이 Cumulative Distribution Function을 살펴보면 대부분의 메시지가 몇 분 차이를 보이는 것으로 나타납니다. 90번째 백분위는 22분이며, 이는 합리적인 채팅 대화 분할 임계값이라고 생각됩니다. 다음 스크립트는 이 임계값을 채팅에 적용하고 결과를 시각화하여 보여줍니다.

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
from matplotlib.patches import Rectangle

def viz_conversations(df, start_period, end_period):
  sample_df = df.query(f"timestamp >= '{start_period}' and timestamp < '{end_period}'")
  plt.figure(figsize=(20, 2))
  ax = plt.gca()
  ax.plot(sample_df['timestamp'], np.ones(len(sample_df)), '.b', alpha=0.4, markersize=10)
  for conv_id, conv_df in sample_df.groupby('conversation_id'):
    if len(conv_df) > 3:  # 적어도 3개의 메시지
      start = mdates.date2num(conv_df['timestamp'].min())
      end = mdates.date2num(conv_df['timestamp'].max())
      ax.add_patch(Rectangle([start, 0.75], end - start, 0.8, color='b', alpha=0.2))
      ax.text(start + (end - start) / 2, 1.4, f'id: {conv_id}', ha='center', va='center', color='black')
    else:
      ax.plot(conv_df['timestamp'], np.ones(len(conv_df)), '.r', markersize=10)
      ax.text(conv_df['timestamp'].min(), 1.4, f'잡음', ha='center', va='center', color='black')
  ax.xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d %H:%M'))
  plt.xticks(rotation=20), plt.title('채팅 대화 시간대'), plt.xlabel('시간'), plt.yticks([]), plt.ylim([0, 2]);

threshold = 0.9
df['conversation_id'] = (df['time_diff_minutes'] >= df['time_diff_minutes'].quantile(threshold)).astype(int).cumsum()
df.loc[(df['time_diff_minutes'] >= df['time_diff_minutes'].quantile(threshold)), 'conversation_id'] -= 1
start_period = '2020-01-28'
end_period = '2020-01-30'
viz_conversations(df, start_period, end_period)
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

요약된 채팅 대화
이제 대화의 텍스트를 대화 ID로 분리했으니, 요약으로 넘어가 보겠습니다. 자연어 처리(NLP)의 텍스트 요약은 주어진 텍스트를 자동으로 요약하여 중요한 정보를 추출하거나 생성하는 것을 목표로 합니다. 특정 유형의 텍스트(채팅)를 다루고 있기 때문에, 텍스트의 성격에 맞는 모델을 신중히 선택하는 것이 좋습니다.

Hugging Face의 이상적인 텍스트 요약 모델을 찾는 여정은 philschmid/bart-large-cnn-samsum을 선택하게 되었어요. 이 모델은 BART 기반 모델(facebook/bart-large-cnn)로, SAMSum 데이터 세트를 사용해 미세 조정되었고, 대화를 요약한 약 16,000개의 메신저 스타일 대화를 포함하고 있습니다. 선택된 대화를 가져와서 요약 모델에 입력한 후 결과를 제시해주는 위젯을 만들어봅시다.
모델 실행은 컴퓨팅 자원을 많이 사용하므로,st.cache_data 데코레이터를 사용하여 입력과 출력을 pickle 형식으로 저장하고, 모델의 중복 실행을 방지할 거에요.

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

# 구역 D: 지리정보

이번에 다룰 마지막 섹션은 지리정보입니다. 이 섹션은 대화 텍스트에서 모든 위치를 추출하고 몇 가지 메타데이터와 함께 멋진 지도를 제시합니다.
위치는 주로 Google 지도 링크를 공유할 때 보내집니다. 텍스트를 올바르게 필터링하면 대화 위치가 위도 및 경도로 출력됩니다. 다행히 Streamlit은 강력한 지도 시각화 기능을 제공하는 Folium을 지원합니다.

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
import streamlit as st
from streamlit_folium import st_folium
import folium

# 'data'라는 세션 상태에서 데이터를 불러와요
df = st.session_state['data']
LOCATIONS_PATTERN = r"(https:\/\/maps\.google\.com\/\?q=-?\d+\.\d+,-?\d+\.\d+)"
# 위치 정보가 있는 데이터를 필터링해요
locations_df = df[(df['message'].str.contains(r'^(?=.*maps.google.com)(?=.*q=)'))]
if not locations_df.empty:
    # 위치 정보에서 위도와 경도 추출해요
    locations_df['lat'], locations_df['lon'] = zip(*locations_df['message'].str.extract(LOCATIONS_PATTERN)[0] \
                                                   .apply(lambda x: x.split('=')[1].split(',')))
# 지도 생성하고 마커 추가해요
m = folium.Map(location=locations_df[['lat', 'lon']].mean().values.tolist(),
               zoom_start=15, tiles='cartodbpositron')
for i in locations_df.to_dict('records'):
    folium.Marker(location=[i['lat'], i['lon'],
                  tooltip=i['username'] + '<br>' + i['date']).add_to(m)
# 제목 표시하고 지도 출력해요
st.title('Chat Group Locations')
st_folium(m)
```

![Chat Analyzer](https://miro.medium.com/v2/resize:fit:1400/1*ufu9c_UQzT9_t0vy_5QlOg.gif)

# 결론

채팅 분석기는 누구든지 쉽게 채팅 그룹 데이터를 분석할 수 있도록 만들어졌어요. 이 글에서는 도구의 기능과 몇 가지 코드 예제를 소개했어요. Streamlit을 활용하여 채팅 분석기를 빠르고 간편하게 개발할 수 있었고, 상당한 가치가 있어요. 하지만 이 도구에는 몇 가지 제한사항이 있어요.

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

**통합 및 커버리지** — 현재 챗 분석기를 사용하려면 채팅을 적극적으로 내보내어 로컬로 다운로드한 다음 도구에 업로드해야 합니다. 이 점이 불편하며 고민하는 사용자들을 막을 수 있습니다. 또한, 현재 이 도구는 WhatsApp과 Telegram을 지원하고 있지만 많은 다른 채팅 플랫폼이 있습니다.

**누락된 기능** — 이 도구는 섹션과 위젯으로 풍부하지만, NLP 방법론 측면에서 아직 완전한 잠재력에 도달하지 못했습니다. 더 구체적으로 말하면, LLM(Large Language Models)은 도구에 올바르게 구현된다면 많은 가치를 만들어낼 수 있습니다. 예를 들어, 채팅 전체를 벡터화하고 사용자가 자연어로 질문할 수 있는 LLM 기반 위젯을 상상해 보세요.

**Streamlit의 한계:**

- **확장성** — 채팅 분석기는 Streamlit의 커뮤니티 클라우드 플랫폼에 배포되었습니다. 이 플랫폼은 무료이며 Github와 쉽게 연결할 수 있지만 자원이 제한되어 있어 고용량 사용에 준비가 되어 있지 않습니다.
- **프론트엔드 유연성** — Streamlit을 사용하면 프론트엔드 개발이 용이해집니다. 반면에, Streamlit 사용자들은 패키지가 제공하는 바에 얽매여 있습니다.

이 글을 즐겁게 읽으셨기를 바랍니다!
의견을 자유롭게 남겨 주시거나 질문을 해 주세요.
제작자 — 알론 코헨
