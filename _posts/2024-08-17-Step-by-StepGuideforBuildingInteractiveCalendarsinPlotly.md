---
title: "Plotly로 인터랙티브 캘린더 만드는 방법 단계별 가이드"
description: ""
coverImage: "/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_0.png"
date: 2024-08-17 00:16
ogImage: 
  url: /assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_0.png
tag: Tech
originalTitle: "Step-by-Step Guide for Building Interactive Calendars in Plotly"
link: "https://medium.com/towards-data-science/step-by-step-guide-for-building-interactive-calendars-in-plotly-277053f6ee7c"
isUpdated: false
---


![Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_0.png)

Plotly는 시각화를 만들기 위한 가장 포괄적인 Python 라이브러리 중 하나입니다. 이 라이브러리는 대부분의 데이터 분석에 유용한 많은 미리 정의된 시각화를 제공합니다. 이러한 시각화는 높은 사용자 정의 가능성을 제공하여 인사이트를 시각화할 때 더 창의적일 수 있으며 새로운 시각화를 구현할 수도 있습니다.

이 기사에서는 Plotly를 사용하여 2024년 바르셀로나의 모든 공휴일을 시각화할 수 있는 캘린더를 만드는 방법을 단계별로 설명하겠습니다. 이 캘린더는 상호작용이 가능하여 도시의 다양한 공휴일에 대한 정보를 호버오버를 통해 제공합니다. 이 기사에서 보여준대로, Plotly는 캘린더를 만들기 위한 내장된 시각화 기능을 갖고 있지는 않지만 히트맵을 기반으로하고 상상력을 조금 가미함으로써 이러한 유형의 시각화를 쉽게 구현할 수 있습니다.

Plotly의 캘린더는 미리 정의된 도구로 생성된 것보다 더 많은 사용자 정의화가 가능합니다. 또한 가장 큰 장점 중 하나는 주피터 노트북과 통합이 가능하다는 점입니다. 예를 들어 이러한 노트북 내에서 시간적 분석을 수행할 수 있습니다. 또한 호버 오버를 우리의 분석 요구에 맞게 사용자 정의할 수 있습니다. 미리 정의된 도구와 비교했을 때 또 다른 큰 장점은 Plotly가 완전히 무료이며 구독이 필요하지 않다는 것입니다.

<div class="content-ad"></div>

달력을 만드는 방법을 알고 싶나요? 시작해 봅시다!

# ICS 파일을 데이터프레임으로 변환하기

첫 번째 단계는 해당 도시의 모든 공휴일이 포함된 데이터 프레임을 얻는 것입니다. 달력 관련 정보는 일반적으로 ICS( iCalendar) 형식의 파일에 저장되어 있습니다. 이 유형의 파일은 달력 및 이벤트 정보를 교환하는 데 가장 일반적이며 .ics 확장자를 가지고 있습니다.

이 문서에서 사용된 파일은 바르셀로나 시의 공개 데이터 페이지에서 얻었으며 다음 링크에서 다운로드할 수 있습니다:

<div class="content-ad"></div>

파일을 다운로드한 후, 문서 편집기로 열어서 파일의 구조를 확인할 수 있습니다. 속성인 BEGIN:VEVENT와 END:VEVENT는 파일에서 추출하려는 이벤트를 구분합니다. 각 이벤트에는 시작 날짜(DTSTART), 종료 날짜(DTEND), 요약(SUMMARY), 설명(DESCRIPTION)에 대한 정보가 포함되어 있습니다. 이 정보만 있으면 우리 캘린더를 만드는 데 충분합니다.

![Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_1.png)

이 파일에서 모든 정보를 추출하는 데 정규 표현식이 필요하지 않습니다. Python에는 ics라는 라이브러리가 있어 이러한 유형의 파일에서 쉽게 정보를 추출할 수 있습니다.

ICS 파일의 내용을 추출하여 데이터 프레임에 저장하기 위해 convert_ics_to_dataframe 함수를 정의했습니다.

<div class="content-ad"></div>

해당 기능은 파일의 내용을 읽고 ics 라이브러리에서 Calendar 객체를 생성합니다. 캘린더의 이벤트는 events 속성을 통해 액세스할 수 있습니다. 그런 다음 각 이벤트에 대해 시작 날짜 (event.begin), 종료 날짜 (event.end), 요약 (event.name) 및 설명 (event.description)을 추출하여 데이터 프레임에 저장합니다.

![이미지](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_2.png)

데이터프레임을 살펴보면 이벤트의 시작 날짜를 나타내는 열과 종료 날짜를 나타내는 열이 포함되어 있습니다. 목표는 세 가지 열만 포함하는 데이터프레임을 만드는 것입니다: (1) 날짜, (2) 요약 및 (3) 설명. 따라서 이벤트가 여러 날에 걸쳐 있는 경우 각 날이 데이터세트의 별도 행으로 표시됩니다. 이를 달성하기 위해 아래에 설명된 expand_date_range 함수를 사용합니다. 함수를 적용하기 전에 'Start Date'와 'End Date' 열은 datetime 유형으로 변환되었습니다.

![이미지](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_3.png)

<div class="content-ad"></div>

# 2024 캘린더를 위한 기본 DataFrame 생성

이전 DataFrame은 바르셀로나 시의 모든 공휴일 정보를 포함하고 있습니다. 하지만 이 모든 정보를 Plotly로 시각화하려면, 2024 캘린더의 모든 관련 데이터를 가진 DataFrame을 생성해야 합니다. 다음 함수를 사용하여 입력 매개변수로 지정된 연도(이 경우 2024년)에 대한 정보가 있는 DataFrame을 생성할 수 있습니다. 데이터 세트에는 2024년의 각 날짜에 대한 정보가 포함되어 있으며, 요일, ISO 주 번호 및 월 이름을 포함합니다.

![2024 Calendar](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_4.png)

이 DataFrame은 나중에 Plotly에서 캘린더를 만드는 데 사용될 것입니다. Date Type 열은 날짜 유형을 나타냅니다: 평일인 경우 (1), 주말인 경우 (2). 그리고 나중에 공휴일의 존재를 나타내기 위해 다른 유형의 날짜 (3)가 추가될 것입니다.

<div class="content-ad"></div>

# 2024 기본 DataFrame에 휴일 정보 통합하기

다음 단계는 2024 달력을 기본 DataFrame에 모든 휴일 정보를 통합하는 것입니다. 아래와 같이, 휴일은 Date Type이 3인 것으로 표시됩니다. Summary 열에는 휴일에 관한 정보가 포함되어 있으며, 나머지 날짜에 대해서는 평일인지 주말인지 여부가 나타납니다.

![이미지](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_5.png)

결과 DataFrame은 달력을 시각화하는 데 사용할 것입니다.

<div class="content-ad"></div>

# 2024 바르셀로나 휴일 달력 시각화

드디어, 바르셀로나 시내의 2024 달력 및 휴일을 시각화하는 히트맵을 생성하는 함수들이 정의됩니다. 먼저, 특정 월에 대한 히트맵을 생성하는 create_heatmap_for_month 함수를 만들고, 그 다음에는 이 함수를 사용하여 create_all_month_heatmaps 함수에서 전체 연간 각 월에 대한 히트맵을 생성할 것입니다.

해당 함수는 우리가 HTML 파일로 저장하거나 웹페이지에 삽입하거나 심지어 Medium 기사에 포함시킬 수 있는 figure를 반환합니다.

이전에 정의된 DataFrame을 사용하여 create_all_month_heatmaps 함수를 통해 달력을 생성합니다.

<div class="content-ad"></div>

거기, 이제 우리는 인터랙티브 달력을 가지고 있어요. 평일, 주말, 그리고 공휴일이 각기 다른 색상으로 표시되어 있습니다.

![인터랙티브 달력](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_6.png)

다음 이미지에서 보듯이, 만들어진 달력은 상호작용적이며, 마우스를 해당 날짜 위에 올리면 공휴일의 이름을 확인할 수 있습니다.

![인터랙티브 달력 확인](/assets/img/2024-08-17-Step-by-StepGuideforBuildingInteractiveCalendarsinPlotly_7.png)

<div class="content-ad"></div>

이제 창의적으로 시도해보세요 - 다양한 레이아웃, 예를 들어 한 행에 3개월씩 배치하거나 요일을 표시할 주석 추가와 같은 다양한 색상 팔레트를 사용해 보세요!

그다음, 시각화를 디자인하는 데 참고할 수 있는 아이디어를 얻을 수 있는 몇 가지 웹사이트를 소개하겠습니다.

# 캘린더 디자인에 창의적인 접근 방법

시각화를 만들 때 가장 중요한 측면 중 하나는 올바른 디자인입니다. 타이포그래피, 제목이나 부제목과 같은 시각화에 표시될 구성 요소, 선택한 색상과 같은 요소들이 결정적인 역할을 합니다. 종종 우리는 시각화를 어떻게 디자인할지 모르며 영감의 원천이 필요합니다.

<div class="content-ad"></div>

드리블은 시각화나 대시보드를 만들 때 영감을 얻을 수 있는 내가 가장 좋아하는 웹사이트 중 하나야. 모바일 기기와 웹사이트에 대한 시각화를 위한 수천 개의 디자인 예시가 이 사이트에 있다구.

색상 선택을 위해 내가 항상 참고하는 웹사이트는 Adobe Color야. 이 사이트를 통해 기분이나 키워드와 관련된 색상 팔레트를 검색할 수 있어.

나는 캘린더의 가능한 디자인과 색상 팔레트에 대한 영감을 위해 이 두 웹사이트를 참고했어. 아래에 보여지는 것처럼, 시각화에 미니멀한 터치를 주고 싶을 때나 빈티지한 느낌을 원할 때 등 다양한 스타일을 평가하고 실험했어. Plotly 시각화는 매우 사용자 정의 가능해서 디자인과 창의성에 실험할 수 있게 해줘.

<div class="content-ad"></div>

Plotly에는 달력을 만들기 위한 미리 정의된 시각화가 없지만, 이것은 우리가 사용자 정의 디자인의 동적 달력을 만들기 위한 해결책을 사용할 수 없다는 것을 의미하지 않습니다. 히트맵 시각화를 사용하면 Plotly에서 달력 및 다양한 시각화 형식을 만들 수 있습니다. 우리는 그들을 사용하는 데 창의적일 필요가 있습니다. 이 기사에서는 발레아리드 시티의 휴일을 시각화하기 위해 달력을 작성하는 방법을 단계별로 설명했습니다. 또한, Plotly가 제공하는 모든 사용자 정의 옵션을 사용함으로써 달력에 부여하고자 하는 스타일에 따라 다양한 디자인을 탐색했습니다.

읽어 주셔서 감사합니다.
아만다 이글레시아스