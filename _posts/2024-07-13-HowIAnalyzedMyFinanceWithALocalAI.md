---
title: "로컬 AI를 사용하여 내 재정을 분석한 방법"
description: ""
coverImage: "/assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_0.png"
date: 2024-07-13 03:16
ogImage: 
  url: /assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_0.png
tag: Tech
originalTitle: "How I Analyzed My Finance With A Local AI"
link: "https://medium.com/gitconnected/declutter-your-spending-with-local-ai-finance-insighter-049191711f9e"
isUpdated: true
---





봄이 들어와 집 청소 전통을 가져오면서, 나는 내 이스터 휴가를 활용하여 재정 상황을 잡고 비용을 줄이고 더 나은 재정 계획을 세우기로 결정했다.

ChatGPT에게 물어보거나 내 개인 정보를 OpenAI, Google 또는 기타 AI 솔루션 제공업체와 공유하는 생각이 마음에 들지 않았다. 절대 안 돼! 그래서 이스터 휴가 동안 "My Local AI Finance Insighter"라는 도구를 만들기로 결심했다.

![이미지](/assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_0.png)

이름 그대로, 이 도구는 완전히 로컬에서 작동하며 인터넷 연결 없이도 사용할 수 있으며 귀하의 금융 데이터를 안전하게 보관한다.

<div class="content-ad"></div>

내 지역 AI 금융 인사이터는 먼저 내 거래 데이터를 수집하여 수입 및 지출을 포함한 내 금융 상황을 분석하고, 그 후에는 내 라이프스타일과 금융 목표에 맞게 맞춤형 저축 팁을 제공하고 투자 계획을 제안합니다. 여기서 데모를 확인해보세요.

귀하의 노트북에서 이 도구를 실행하고 싶으신가요? 이 튜토리얼을 따라하시면 귀하의 고유한 로컬 AI 금융 인사이터를 구축하는 과정을 단계별로 안내해 드리겠습니다.

## 목차:

- 애플리케이션, 디자인, 및 개발 스택 개요
   - 애플리케이션 및 아키텍처 개요
   - Ollama 및 오픈 소스 LLMs
   - LangChain
   - Streamlit
- 사전 준비 및 데이터셋 준비
- 데이터 업로드 인터페이스 및 거래 데이터 처리 모듈 구축
- 개인 재무 대시보드 및 맞춤화된 보고서 생성
- 마무리 생각

<div class="content-ad"></div>

# 어플리케이션, 디자인, 및 개발 스택 개요

## 어플리케이션 및 아키텍처 개요

이 어플리케이션은 사용자 인터페이스로 streamlit을 사용하며, 핵심적으로 Langchain을 활용하여 지역 서버에 내장된 오픈소스 LLM 모델들과 상호 작용합니다. 

이 어플리케이션에서 우리는 Mistral과 LLAVA 같은 가장 진보된 오픈소스 모델을 활용하여 다중 모달리티 기능을 해제하고 있습니다.

<div class="content-ad"></div>

이 프로젝트에서는 LLM의 출력을 이용하여 "전문 재무 기획사의 의견"을 제시하기 위한 저축 및 투자 권고사항을 제공합니다. 

이 프로젝트에서 달성할 목표는 다음과 같습니다:

- 거래 데이터 처리: LLM을 사용하여 거래 데이터를 처리하고 분류합니다.
- 양적 분석 및 시각화: 거래 내역에서 총 수입, 지출 및 순저축률을 계산할 것입니다. 수입 및 지출 트렌드 분석, 지출 분석 등을 위해 수입 및 지출의 트렌드를 시각화하는 그래픽도 제공할 것입니다.
- 다중 모달리티로 데이터 해석: 그래프를 이해하고 트렌드를 발견하며 핵심 포인트를 강조하기 위해 다중 모달리티 기능 사용할 것입니다.
- 맞춤형 재무 계획: LLM을 활용하여 귀하의 독특한 라이프스타일과 재정 목표에 기반하여 맞춤형 저축 팁 및 투자 권고안을 생성할 것입니다.

그러나 기술 코딩에 들어가기 전에, 이 프로젝트에서 사용한 용어에 대한 소개가 있습니다. 이미 익숙하시다면 건너뛰셔도 됩니다.

<div class="content-ad"></div>

## 올라마

올라마는 나에게 있어서 오픈 소스 LLMs를 쉽게 시작하고 사용할 수 있는 최고이자 가장 쉬운 방법입니다. Llama 2, Mistral, Llava와 같은 가장 강력한 LLMs를 지원하며, 사용할 수 있는 모델 목록은 ollama.ai/library에서 확인할 수 있습니다. MacOS, Windows 및 Linux에서 사용할 수 있습니다.

## 랭체인

랭체인은 LLMs를 중심으로 구축된 오픈 소스 프레임워크입니다. 이는 챗봇, 요약 도구 등을 비롯한 GenAI 애플리케이션의 설계 및 개발을 크게 단순화합니다.

<div class="content-ad"></div>

The library's fundamental concept is to "chain" various components to streamline intricate AI tasks and enable the creation of more advanced scenarios with LLMs. It provides seamless integration with Ollama's open-source models.

## Streamlit

Streamlit is an open-source framework that empowers developers to swiftly create and distribute data applications. With just a few lines of Python code, it enables the development of web-based user interfaces.

Streamlit's user-friendly interface makes it an ideal choice for rapid prototyping and intricate data dashboard projects.

<div class="content-ad"></div>

# 사전 준비 및 데이터셋 준비

## Ollama 설치

먼저 Ollama 다운로드 페이지로 이동하여 사용 중인 운영 체제와 일치하는 버전을 선택하고 다운로드하여 설치하세요.

Ollama를 설치한 후에 명령 터미널을 열고 다음 명령어를 입력하세요. 이 명령어들은 모델을 다운로드하고 로컬 머신에서 실행합니다. 이 프로젝트에서는 Mistral과 LLAVA를 사용할 것입니다.

<div class="content-ad"></div>


ollama serve
ollama pull mistral
ollama pull llava
ollama run mistral
ollama run llava


## 데이터셋 준비하기

데모를 위해서, 제 개인 거래 데이터를 합성 데이터로 대체하겠습니다. 그래서 ChatGPT에게 1,000개의 거래를 생성하도록 요청했습니다. 아래는 여러분이 필요하다면 사용할 수 있는 테스트용 프롬프트입니다.


Generate a realistic dataset of 1,000 financial transactions for a young professional working in finance, residing in Europe, covering the period from January 2022 to December 2023. Ensure a balanced representation of income and expenses across various categories typical for this demographic. Include the following four columns:

Date: Transaction date (format: YYYY-MM-DD)
Name/Description: A unique and detailed description of each transaction (e.g., "Salary deposit," "Monthly rent payment," "Restaurant dinner with friends")
Expense/Income: Clearly indicate whether the transaction is an expense (e.g., "Expense") or income (e.g., "Income")
Amount(EUR): Transaction amount in Euros


<div class="content-ad"></div>

생성된 데이터셋은 네 개의 열로 구성되어야 합니다:

- 날짜: 거래 날짜
- 이름/설명: 거래에 대한 간단한 설명입니다. 이 필드는 거래의 주요 성격에 따라 LLM이 거래를 분류하는 데 사용됩니다.
- 지출/수입: 수입인지 지출인지 표시
- 금액(유로): 유로로 된 금액

![이미지](/assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_1.png)

# 데이터 업로드 인터페이스 및 거래 데이터 처리 모듈 구축하기

<div class="content-ad"></div>

## 의존성 설치

이제 Langchain과 Streamlit 관련 의존성을 설치해야 합니다.

```js
pip install langchain-community
pip install streamlit
```

## 데이터 업로드 모듈 설정

<div class="content-ad"></div>

새로운 파이썬 파일 "Upload.py"를 만들어서 다음 코드를 추가해 보겠습니다. 여기서는:

- 필요한 라이브러리를 가져옵니다.
- 거래 분류에 사용될 LLM 모델을 초기화합니다.
- 카테고리 정의: 다양한 종류의 수입 및 지출을 커버하는 카테고리 목록을 만듭니다. 이를 통해 LLM이 각 거래를 정확하게 분류하는 데 도움이 될 것입니다.

```python
import streamlit as st
import pandas as pd
from langchain_community.llms import Ollama

llm = Ollama(model="mistral")
categories = [
    "Salary/Wages", "Investment Income", "Freelance Income", "Business Revenue","Rental Income","Housing", "Utilities","Groceries","Transportation","Insurance","Healthcare","Entertainment","Personal Care","Education","Savings/Investments","Loans/Debt","Taxes","Childcare","Gifts/Donations","Dining Out","Travel","Shopping","Subscriptions","Pet Care", "Home Improvement","Clothing","Tech/Gadgets", "Fitness/Sports",
]
categories_string = ",".join(categories)
```

## 거래 분류 기능 구축하기

<div class="content-ad"></div>

- 거래 내역을 분류합니다: categorize_transactions 함수를 작성하세요. 이 함수는 거래 이름을 입력으로 받습니다. 우리는 LLM의 출력을 가이드하는 프롬프트 엔지니어링 기술을 사용할 것입니다. 거래 이름들을 프롬프트의 맥락 안에 포함시키고 LLM에게 미리 정의된 카테고리 목록에 따라 분류해 달라고 요청합니다. LLM의 출력을 받은 후에는 이 데이터를 구조화된 판다스 DataFrame으로 정리하여 변환할 것입니다.

```python
def categorize_transactions(transaction_names, llm):
    prompt = f"""다음 경비에 적절한 카테고리를 추가하세요. 기본 목적 또는 성격을 기준으로 가장 관련성 높은 항목을 선택해야 합니다. {categories_string} 중에서 하나의 카테고리만 선택하십시오.\n
        출력 형식은 항상 다음과 같아야 합니다: 거래명 - 카테고리. 예를 들어: Spotify #2 - 엔터테인먼트, Basic Fit Amsterdam Nld #3 - 피트니스/스포츠 \n 
        다음과 같이 분류할 거래내역들입니다: {transaction_names} \n"""

    print(prompt)
    filtered_response = []
    # LLM 출력이 일관적이지 않은 경우 다시 시도합니다
    while len(filtered_response) < 2:
        response = llm.invoke(prompt).split("\n")
        print(response)
        # "거래명: 카테고리" 쌍을 포함하지 않는 항목을 제거합니다
        filtered_response = [item for item in response if '-' in item]
    print(filtered_response)
    # DataFrame에 넣기
    categories_df = pd.DataFrame({"거래내역 vs 카테고리": filtered_response})

    size_dif = len(categories_df) - len(transaction_names.split(","))
    if size_dif >= 0:
        categories_df["거래내역"] = transaction_names.split(",") + [None] * size_dif
    else:
        categories_df["거래내역"] = transaction_names.split(",")[:len(categories_df)]
    categories_df["카테고리"] = categories_df["거래내역 vs 카테고리"].str.split("-", expand=True)[1]
    return categories_df
```

2. 데이터 처리 함수: process_data 함수를 만들어 업로드된 데이터 파일을 처리하고, categorize_transactions를 사용하여 거래를 분류한 후에 분류된 데이터를 전역 DataFrame에 다시 병합합니다. 이는 추가적인 데이터 분석에 사용될 것입니다.

```python
def hop(start, stop, step):
    for i in range(start, stop, step):
        yield i
    yield stop

def process_data(df: pd.DataFrame):
    unique_transactions = df["이름/설명"].unique()
    index_list = list(hop(0, len(unique_transactions), 30))

    # categories_df_all DataFrame 초기화
    categories_df_all = pd.DataFrame()

    # index_list를 반복
    for i in range(0, len(index_list) - 1):
        print(f"반복 중: {i}")
        transaction_names = unique_transactions[index_list[i] : index_list[i + 1]]
        transaction_names = ",".join(transaction_names)

        categories_df = categorize_transactions(transaction_names, llm)
        categories_df_all = pd.concat(
            [categories_df_all, categories_df], ignore_index=True
        )

    # 데이터 추가 정리:
    # NA 값 제거
    categories_df_all = categories_df_all.dropna()
    # 거래 열에서 숫자 제거 예: "1. "
    categories_df_all["거래내역"] = categories_df_all["거래내역"].str.replace(
        r"\d+\.\s?", "", regex=True
    ).str.strip()

    new_df = pd.merge(
        df,
        categories_df_all,
        left_on="이름/설명",
        right_on="거래내역",
        how="left",
    )
    new_df.to_csv(f"data/{uploaded_file.name}_categorized.csv", index=False)
    return new_df
```

<div class="content-ad"></div>

## 스트림릿 웹 앱 만들기

- 스트림릿 인터페이스 설정: 먼저 웹 앱의 제목을 설정하고 파일 업로더 위젯을 추가해보세요. 이를 통해 사용자들이 자신의 금융 데이터를 업로드할 수 있습니다.

```python
st.title("📝 여기에 금융 데이터를 업로드하세요")
uploaded_file = st.file_uploader("금융 데이터 업로드", type=("txt", "csv", "pdf"))
```

2. 업로드된 데이터 처리: 파일이 업로드되면 pandas DataFrame에 읽고 process_data 함수를 호출하여 거래를 분류합니다.

<div class="content-ad"></div>

```python
if uploaded_file:
    with st.spinner("데이터 처리 중..."):
        file_details = {"파일 이름": uploaded_file.name, "파일 유형": uploaded_file.type}
        df = pd.read_csv(uploaded_file)
        df = process_data(df)
        st.markdown("데이터 처리 완료 : OK")
```

3. 스트림릿 앱을 실행하세요. 다음과 같은 인터페이스가 나타날 것입니다.

![이미지](/assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_2.png)

# 개인 금융 대시보드 및 맞춤형 보고서 생성하기


<div class="content-ad"></div>

우리의 모든 거래가 LLM Mistral에 의해 깔끔하게 정리되었으므로, 이제 재무 분석을 처리할 준비가 되었습니다. 재무 분석은 네 단계로 구성되어 있습니다:  

- 양적 분석: 당신의 재무 건강 상태를 명확히 파악하기 위해 먼저 연간 소득 및 지출을 계산하고, 돈이 대부분 어디로 가고 있는지 등을 식별할 것입니다.  
- 시각적 표현: 거래 데이터를 차트로 나타내어 추세와 패턴을 발견할 것입니다.  
- LLM의 질적 분석: 우리가 수집한 주요 금융 지표 및 인사이트(그 멋진 그래프들 포함)를 Mistral에게 다시 공급할 것입니다. 신중하게 준비된 안내에 따라, 당신의 재정 상황에 대한 질적 분석을 요청할 것입니다.  
- 맞춤형 절약 팁 및 투자 추천 생성: 양적 및 질적 분석에서 얻은 통찰을 바탕으로 Mistral이 당신을 위한 맞춤형 조언을 생성할 것입니다.   

라이브러리 가져오기 및 재무 분석 함수 생성   

먼저, "Finance_Dashboard.py"라는 새로운 파이썬 파일을 만들고 필요한 파이썬 라이브러리를 가져와 Ollama 모델을 초기화해 보겠습니다.

<div class="content-ad"></div>

```python
import os
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
from langchain_community.llms import Ollama

llm_llava = Ollama(model="llava")
llm = Ollama(model="mistral")
```

Let's dive into the realm of financial analysis by creating a mystical function called financial_analysis. This enchanting function will unveil the secrets hidden within your financial data, revealing the dance of yearly and monthly income, expenses, savings rate, and the elusive top expense categories.

```python
def financial_analysis(data:pd.DataFrame):
    key_figures = {}
    # Unveil the yearly total income and expenses
    yearly_income = data.loc[data['Expense/Income'] == 'Income'].groupby('Year')['Amount(EUR)'].sum().mean()
    yearly_expenses = data.loc[data['Expense/Income'] == 'Expense'].groupby('Year')['Amount(EUR)'].sum().mean()

    # Decrypt the top expense categories
    top_expenses = data.loc[data['Expense/Income'] == 'Expense'].groupby('Category')['Amount(EUR)'].sum().sort_values(ascending=False)

    # Reveal the average monthly income and expenses
    monthly_income = data.loc[data['Expense/Income'] == 'Income'].groupby(data['Date'].dt.to_period('M'))['Amount(EUR)'].sum().mean()
    monthly_expenses = data.loc[data['Expense/Income'] == 'Expense'].groupby(data['Date'].dt.to_period('M'))['Amount(EUR)'].sum().mean()

    # Channel the mystical savings rate
    savings = yearly_income - yearly_expenses
    savings_rate = (savings / yearly_income) * 100 if yearly_income > 0 else 0

    key_figures['Average Annual Income'] = f"€{yearly_income:,.2f}"
    key_figures['Average Annual Expenses'] = f"€{yearly_expenses:,.2f}"
    key_figures['Annual Savings Rate'] = f" {savings_rate:.2f}%"
    key_figures['Top Expense Categories'] = {category: f"€{amount:,.2f}" for category, amount in top_expenses.head().items()}
    key_figures['Average Monthly Income'] = f"€{monthly_income:,.2f}"
    key_figures['Average Monthly Expenses'] = f"€{monthly_expenses:,.2f}"
    return key_figures
```

## Forging the Plotting Spells: Bonding with the Data Spirits

<div class="content-ad"></div>

여기서는 재정 데이터를 시각화하는 기능을 구현할 거에요. 이에는 시간에 따른 수입 대 지출, 월별 저축률, 수입원, 그리고 카테고리별 소비에 관한 네 가지 플롯이 포함돼요.

```javascript
def plot_income_vs_expense_over_time(df):
    # 시간에 따른 수입 대 지출
    st.markdown("1. 시간에 따른 수입 대 지출")
    income_expense_summary = (
        df.groupby(["YearMonth", "Expense/Income"])["Amount(EUR)"]
        .sum()
        .unstack()
        .fillna(0)
    )
    income_expense_summary.plot(kind="bar", figsize=(10, 8))
    plt.title("Income vs Expenses Over Time")
    plt.ylabel("금액 (유로)")
    plt.xlabel("월")
    plt.savefig("data/income_vs_expense_over_time.png", bbox_inches="tight")
    st.pyplot(plt)


def plot_saving_rate_trend(data: pd.DataFrame):
    st.markdown("2. 월간 저축률 추이")
    monthly_data = data.groupby(['YearMonth', 'Expense/Income'])['Amount(EUR)'].sum().unstack().fillna(0)
    monthly_data['Savings Rate'] = (monthly_data['수입'] - monthly_data['지출']) / monthly_data['수입'] * 100
    fig, ax = plt.subplots()
    monthly_data['Savings Rate'].plot(ax=ax)
    ax.set_xlabel('월')
    ax.set_ylabel('저축률 (%)')
    plt.savefig("data/saving_rate_over_time.png", bbox_inches="tight")
    st.pyplot(fig)

def plot_income_source_analysis(data: pd.DataFrame):
    st.markdown("3. 수입원 분석")

    income_sources = data[data['Expense/Income'] == '수입'].groupby('Category')['Amount(EUR)'].sum()
    income_sources.plot(kind="pie", figsize=(10, 8), autopct="%1.1f%%", startangle=140)
    plt.title("수입원 분석")
    plt.ylabel("")  # 원형 차트에서는 y-축 라벨이 필요 없으므로 숨깁니다
    plt.savefig("data/income_source_analysis.png", bbox_inches="tight")
    st.pyplot(plt)


def plot_category_wise_spending_analysis(data: pd.DataFrame):
    st.markdown("4. 카테고리별 소비 분석")
    expenses_by_category = data[data['Expense/Income'] == '지출'].groupby('Category')['Amount(EUR)'].sum()
    expenses_by_category.plot(kind="pie", figsize=(10, 8), autopct="%1.1f%%", startangle=140)
    plt.title("지출 분석")
    plt.ylabel("")  # 원형 차트에서는 y-축 라벨이 필요 없으므로 숨깁니다
    plt.savefig("data/expense_category_analysis.png", bbox_inches="tight")
    st.pyplot(plt)
```

재정 데이터 불러오기

CSV 파일에서 재정 데이터를 읽어와 처리하고 분석할 준비를 해볼게요.

<div class="content-ad"></div>

```python
total_df = pd.DataFrame()
for root, dirs, files in os.walk("data"):
    for file in files:
        if file.endswith(".csv"):
            df = pd.read_csv(os.path.join(root, file))
            total_df = pd.concat([total_df, df], ignore_index=True)

total_df["Date"] = pd.to_datetime(total_df["Date"])
total_df["YearMonth"] = total_df["Date"].dt.to_period("M")
total_df["Year"] = total_df["Date"].dt.year
```

스티름릿 대시보드를 설정하세요

스트림릿을 사용하여 대시보드에 제목을 만들고 분석 결과를 표시하고 플로팅 함수를 통합하세요.

```python
st.title("내 지역 AI 금융 인사이터")
st.markdown(
    "**금융 데이터를 분석하고, 개별적 요구에 맞는 통찰과 권장사항을 제공하는 맞춤형 및 안전한 방식**"
)

analysis_results = financial_analysis(total_df)
results_str = ""
# 사전(dictionary)를 반복
for key, value in analysis_results.items():
    if isinstance(value, dict):
        # 값이 또 다른 사전인 경우, 하위 키와 값을 가져오기 위해 추가 반복
        sub_results = ', '.join([f"{sub_key}: {sub_value}" for sub_key, sub_value in value.items()])
        results_str += f"{key}: {sub_results}\n"
    else:
        # 직접적인 키-값 쌍인 경우에는 간단히 연결
        results_str += f"{key}: {value}\n"

st.subheader("연간 수치")
col1, col2, col3 = st.columns(3)
col1.metric(label="평균 연간 수입", value=analysis_results['Average Annual Income'])
col2.metric(label="평균 연간 지출", value=analysis_results['Average Annual Expenses'])
col3.metric(label="저축율", value=analysis_results['Annual Savings Rate'])

# 평균 월간 수치 표시
st.subheader("평균 월간 수치")
col1, col2 = st.columns(2)
col1.metric(label="평균 월간 수입", value=analysis_results['Average Monthly Income'])
col2.metric(label="평균 월간 지출", value=analysis_results['Average Monthly Expenses'])

# 상위 비용 카테고리를 테이블로 표시
st.subheader("상위 비용 카테고리")
expenses_df = pd.DataFrame(list(analysis_results['Top Expense Categories'].items()), columns=['Category', 'Amount'])
st.table(expenses_df)

with st.container():
    col1, col2 = st.columns(2)
    with col1:
        plot_income_vs_expense_over_time(total_df)
    with col2:
        plot_saving_rate_trend(total_df)

with st.container():
    col3, col4 = st.columns(2)
    with col3:
        plot_income_source_analysis(total_df)
    with col4:
        plot_category_wise_spending_analysis(total_df)
``` 


<div class="content-ad"></div>

When you run Streamlit, you should see a dashboard like this:

![Dashboard](/assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_3.png)

## Creating Personalized Finance Analysis and Recommendations

Next, we'll provide Mistral with the quantitative and qualitative analysis we generated earlier so that it can develop personalized advice just for you!

<div class="content-ad"></div>

```python
with st.container():
    col3, col4 = st.columns(2)
    with col3:
        plot_income_source_analysis(total_df)
    with col4:
        plot_category_wise_spending_analysis(total_df)

with st.spinner("Generating reports ..."):
    total_response = ""
    for root, dirs, files in os.walk("data"):
        for file in files:
            if file.endswith(".png"):
                response = llm_llava.invoke(
                    f"금융 전문가로 활약하며 이미지를 분석하세요: {os.path.join(root, file)}. 이미지에서 추출한 통찰과 주요 수치를 제시해 주세요"
                )
                total_response += response
    total_response += f"\n다음은 사용자의 주요 금융 지표입니다: {results_str}"

    st.write("---------------")
    st.markdown("**금융 분석 및 예산 플래너**")

    summary = llm.invoke(
        f"당신은 도움이 되고 전문적인 금융 플래너입니다. 다음 분석을 바탕으로 사용자의 금융 상황에 대한 요약 및 저축 팁을 제공하세요. 사용자가 비용을 줄일 수 있는 카테고리를 강조하고, 소득과 목표에 기초하여 이상적인 저축율을 제안하세요. 이러한 제안을 사용자의 라이프스타일과 금융 목표에 맞게 맞추어 친근한 어조로 전달하세요."
    )
    st.write(summary)
    st.write("---------------")
    st.markdown("**투자 팁**")
    if "user_answers_str" in st.session_state:
        user_investment_answer = st.session_state.user_answers_str
    else:
        user_investment_answer = ""
    investment_tips = llm.invoke(
        f"당신은 도움이 되고 전문적인 금융 플래너입니다. 사용자의 위험 허용 수준과 투자 목표에 기초하여 적합한 투자 옵션을 개요로 제공하세요. 주식, 채권, 펀드, ETF 및 다른 투자 수단의 기본 사항을 논의하고, 사용자 프로필과 일치하는 투자차분의 중요성을 설명하세요. 투자 포트폴리오 다각화와 리스크 관리 역할에 대해 알려드리며, 사용자의 현재 금융 상황을 고려한 시작 단계를 제의하세요. 사용자의 투자 목표와 위험 허용 수준은 다음과 같습니다: {user_investment_answer}"
    )
    st.write(investment_tips)
```

보고서는 잘 구성되어 있고 일관성이 있어 보이지만, 제가 원하는 것보다 더 길어요. 더 간결한 결과물을 얻기 위해 프롬프트를 더 다듬을 수 있습니다.


<img src="/assets/img/2024-07-13-HowIAnalyzedMyFinanceWithALocalAI_4.png" />

# 마무리


<div class="content-ad"></div>

그래서, 우리가 함께 만든 Local AI-Powered Finance Insighter를 소개합니다. 이를 통해 우리의 재정 상태를 더 잘 이해할 수 있습니다!

Generative AI의 힘을 활용하여 맞춤형 조언을 제공하는 방식으로 이 프로젝트를 시작했습니다. 본 대시보드의 핵심 기능은 완전히 로컬에서 작동한다는 점입니다: 여러분의 재정 데이터는 안전하게 여러분의 노트북 안에 저장됩니다.

## 더 나아가기

처음에는 Thu Vu의 데이터 분석 관련 비디오에서 영감을 받아 시작했습니다. 그녀는 거래 데이터를 분류하기 위해 로컬 LLM을 사용하는 방법을 설명했습니다. 그러나 LLM은 창의적인 성격 때문에 대규모 데이터셋의 경우 일관된 분류에 어려움을 겪을 수 있다는 것을 발견했습니다! 좋은 결과를 얻기 위해 코드와 프롬프트를 조정해야 했습니다.

<div class="content-ad"></div>

이번에는 일반 인공지능과 LLMs를 사용하여 통찰력 있는 분석, 보고서 및 맞춤형 팁을 생성하는 것이 매우 효과적임이 입증되었습니다.

또 다른 흥미로운 기능으로는 사용자가 금융 관련 질문을 직접 할 수 있는 챗봇 인터페이스를 추가하는 것이 있을 수 있습니다.

이 프로젝트를 즐기셨기를 바랍니다! 거래를 일관되게 범주화하는 더 나은 방법을 발견하시면 댓글로 알려주세요!

## 떠나시기 전에! 🦸🏻‍♀️

<div class="content-ad"></div>

내 이야기가 마음에 들었다면, 아래 방법들로 저를 지지해주세요:

- Medium에서 클랩과 댓글, 하이라이트를 보내주세요. 여러분의 지원은 저에게 엄청난 의미를 갖습니다.👏
- Medium에서 저를 팔로우하고 최신 기사를 구독하세요. 🫶