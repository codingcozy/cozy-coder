---
title: "날짜 및 시간 데이터를 다루는 3가지 강력한 SQL 쿼리 사용법"
description: ""
coverImage: "/assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_0.png"
date: 2024-07-14 01:42
ogImage: 
  url: /assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_0.png
tag: Tech
originalTitle: "3 Powerful SQL Queries To Work With Date-Time Data"
link: "https://medium.com/towards-data-science/3-powerful-sql-queries-to-work-with-date-time-data-41681fea7c89"
isUpdated: true
---





## 데이터 과학

![SQL Queries](/assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_0.png)

날짜 및 시간 값과 작업하기 – 데이터 분석의 중요한 부분 중 하나입니다.

데이터에서 날짜 및 시간 값을 보게 되면, 정보가 가득 담긴 보물로 보세요. 적어도 제가는 그렇게 생각해요.

<div class="content-ad"></div>

보물 가운데 유용한 부분을 얻는 것은 쉽지 않죠. 이 글의 영감이 여기서 나왔어요.

네 맞아요! SQL 쿼리 3개를 제공해 드릴 텐데요, 이는 데이터의 날짜 및 시간 값에서 최대한 활용하는 데 도움이 될 거예요. 이를 통해 데이터로부터 가치 있는 통찰을 얻고 효과적인 의사결정을 지원할 수 있을 거예요.

파이썬을 통해 데이터 처리 및 분석을 진행하신다면, 아래의 2편 글에서 유용한 팁을 설명해드렸어요. 한 번 확인해보세요 —

- 파이썬에서 날짜 - 시간 데이터 다루기를 위한 3가지 강력한 요령
- 날짜시간 데이터 다루기 위한 3가지 유용한 판다스 팁

<div class="content-ad"></div>

To get the most out of your date-time data in SQL, we'll share some insights on how you can maximize your results. Here's a sneak peek at what you'll discover:

- Using the BETWEEN keyword in SQL
- Extracting the year and month from dates in SQL
- Aggregating data by month or year in SQL
- Formatting date values in SQL

Excited to dive in? Let's begin our journey into the world of SQL date-time analysis! 🌟

<div class="content-ad"></div>

한 가지 기본적인 것부터 시작해봅시다. 날짜-시간 값으로 작업할 때 알아야 할 키워드인 BETWEEN에 대해 알아보겠습니다.

# SQL에서의 BETWEEN 키워드

<div class="content-ad"></div>

BETWEEN 키워드는 WHERE 절에서 자주 사용되어 특정 범위 내에서 결과를 필터링하는 데 도움이 됩니다.

날짜 및 시간 데이터를 처리할 때 이 키워드를 사용하여 두 날짜 사이의 레코드를 가져올 수 있습니다. 이제 보여드리겠습니다.

예를 들어, 2023년 8월 1일부터 8월 31일까지의 모든 레코드를 가져오고 싶다면, 아래와 같이 BETWEEN 키워드 뒤에 이러한 날짜를 제공하면 됩니다.

```sql
SELECT *
FROM alldata.salesdata
WHERE 1=1
AND order_date BETWEEN '2021-08-01' AND '2021-08-31';
```

<div class="content-ad"></div>

![Markdown format](/assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_2.png)

Oh, interesting! When you use the BETWEEN keywords in SQL, the dates you specify are inclusive, meaning the query will include all results falling on or between those dates.

If you prefer not to use BETWEEN, you can achieve the same results with the query below:

```js
SELECT *
FROM alldata.salesdata
WHERE 1=1
AND order_date >= '2021-08-01' 
AND order_date <= '2021-08-31';
```

<div class="content-ad"></div>

하지만 두 번째 쿼리를 추천하지 않습니다. WHERE 절에서 언급된 조건을 모든 레코드에서 확인하고 평가하기 때문입니다.

BETWEEN 키워드를 사용하는 것은 날짜 값을 기준으로 레코드를 필터링하는 더 빠르고 더 깔끔하며 직관적인 방법입니다.

이제 날짜-시간 값과 함께 작업할 때 알아야 할 첫 번째 SQL 쿼리를 살펴보겠습니다.

내가 언급했듯이, 데이터의 날짜-시간 값은 보물로 여겨지며 유용한 통찰력을 얻기 위해서는 더 심층적으로 파고들고 돌을 깨야 합니다.

<div class="content-ad"></div>

# SQL에서 날짜로부터 연도 및 월 추출하기

날짜-시간 값의 일부를 추출하는 것, 예를 들어 월, 연도 또는 분기를 추출하는 것은 데이터를 더 심층적으로 파헤치는 데 도움이 될 수 있습니다. 이는 자세한 트렌드 분석을 지원하고 더 구체적인 보고서를 제공해줍니다. 이러한 보고서는 더 나은 의사결정을 지원합니다.

SQL 방언에 따라 날짜-시간 값에서 일, 월, 연도를 추출하는 데 작은 차이점이 있을 수 있습니다. MySQL을 사용할 때는 EXTRACT 키워드를 사용하여 이러한 값을 추출할 수 있습니다.

이제 그 방법을 보여드릴게요.

<div class="content-ad"></div>

년, 월, 일, 분기와 같은 일부를 선택해서 어떤 정보를 원하는지 알려주세요. 그리고 날짜 및 시간 값을 포함하는 열의 이름도 알려주셔야 도와드릴 수 있어요.

주문일자(order_date)에서 년, 월, 분기 정보를 추출하는 예시를 확인해보세요.

```sql
SELECT EXTRACT(YEAR FROM order_date) AS year
    , EXTRACT(MONTH FROM order_date) AS month
    , EXTRACT(QUARTER FROM order_date) AS quarter
    , order_date
FROM alldata.salesdata;
```

![Link to the Image](/assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_3.png)

<div class="content-ad"></div>

간단하죠?

MySQL WorkBench 문서에 따르면, 날짜 및 시간 값에서 다음 정보를 추출할 수 있습니다.

```sql
SELECT EXTRACT(YEAR FROM order_date) AS year
    , EXTRACT(MONTH FROM order_date) AS month
    , EXTRACT(QUARTER FROM order_date) AS quarter
    , EXTRACT(WEEK FROM order_date) AS week_of_year
    , EXTRACT(DAY FROM order_date) AS day_of_month
    , EXTRACT(HOUR FROM order_date) AS hour_of_day
    , EXTRACT(MINUTE FROM order_date) AS minute_of_hour
    , EXTRACT(SECOND FROM order_date) AS second_of_minute
    , order_date
FROM alldata.salesdata;
```

![작업 날짜 데이터를 처리하기 위한 3가지 강력한 SQL 쿼리](/assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_4.png)

<div class="content-ad"></div>

만약 SQL 서버를 사용 중이라면 DATEPART 함수를 사용할 수 있습니다. AWS Redshift를 사용 중이라면 EXTRACT 함수를 계속 사용할 수 있습니다.

하지만 이러한 값만 얻는 것으로는 데이터셋에서 최대의 통찰력을 얻을 수 없습니다. 데이터 집계를 수행하여 데이터셋의 전체 잠재력을 탐색해야 하며, 그것이 바로 다음 SQL 쿼리가 나타나는 곳입니다.

# SQL에서 월 또는 연별 데이터 집계

날짜 부분별로 데이터 집계를 수행하면 데이터에 숨겨진 패턴과 트렌드를 식별할 수 있습니다.

<div class="content-ad"></div>

특정 기간에 대한 트렌드를 식별하거나 월별, 연도별로 데이터를 분석하면 데이터셋을 더 잘 이해하고 비즈니스 인사이트를 얻을 수 있어요.

데이터셋의 다른 수치 필드를 각 월, 분기 또는 연도별로 집계할 수 있어요.

이러한 데이터 집계는 데이터의 이상 현상을 감지하는 데 도움이 되며, 데이터 품질 점검을 설계할 때 사용할 수 있어요. 다양한 데이터 품질 점검의 자세한 내용은 아래 기사에서 확인할 수 있어요 —

이러한 유형의 데이터 집계에 대한 전형적인 사용 사례에는 전자 상거래 회사의 매출, 재고 및 고객 가입/로그인 분석이 있습니다.

<div class="content-ad"></div>

한 예를 들어보겠습니다.

다음 쿼리를 시도해 보세요. 이 쿼리는 주문 수를 집계하며 해당 연도의 각 월마다 평균 배송 비용을 제공합니다.

```js
SELECT EXTRACT(MONTH FROM order_date) AS month
    , EXTRACT(YEAR FROM order_date) AS year
    , COUNT(orderid) AS number_of_orders
    , AVG(shipping_cost) AS avg_shipping_cost
FROM alldata.salesdata
GROUP BY EXTRACT(MONTH FROM order_date)
       , EXTRACT(YEAR FROM order_date);
```

<div class="content-ad"></div>

이는 월별 및 연도별 집계 된 데이터 통찰력을 제공합니다. 예를 들어 여기서 다음과 같이 말할 수 있습니다 — 평균 배송 비용은 올해 내내 거의 동일했습니다.

그러나 2021년 전체 주문 수와 평균 배송 비용을 얻고 싶다면 어떻게 할까요?

수동 계산을 하시겠어요? 아니면 다른 SQL 쿼리를 작성하실 건가요?

여기 마법 키워드를 알려드릴게요 — ROLLUP.

<div class="content-ad"></div>

아래 쿼리를 살펴보세요. 어느 순간에도 귀하의 질문을 해결해 주며 추가적인 노력도 필요하지 않습니다.

```js
SELECT EXTRACT(MONTH FROM order_date) AS month
    , EXTRACT(YEAR FROM order_date) AS year
    , COUNT(orderid) AS number_of_orders
    , AVG(shipping_cost) AS avg_shipping_cost
FROM alldata.salesdata
GROUP BY EXTRACT(MONTH FROM order_date)
       , EXTRACT(YEAR FROM order_date) WITH ROLLUP;
```

![Check this SQL query](assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_6.png)

상기 결과에서 마지막 행에서 월과 연도 열이 모두 NULL로 표시된 것은 모든 연도의 모든 월에 대한 결과를 보여준다는 것을 의미합니다. 즉, 2021년의 총 주문 수와 평균 배송 비용을 제공합니다.

<div class="content-ad"></div>

SQL 쿼리의 GROUP BY 절 끝에 키워드 WITH ROLLUP을 추가하면 됩니다.

이 기능이 어떻게 작동하는지 궁금하신가요??

자세한 내용은 아래 이전 글을 읽어보세요. 거기서 이 기능에 대해 자세히 설명했어요.

읽어보시고 유용한 정보를 얻으셨으면 좋겠네요. 🌟

<div class="content-ad"></div>

데이트의 다양한 부분을 추출하는 것만큼 중요한 데이터 조작 방법이 또 있습니다. 때로는 데이터셋에 있는 날짜-시간 값이 최종 분석이나 보고서에서 원하는 형식과 다른 경우가 있습니다.

그래서 이 마지막 부분에서는 어떻게 날짜-시간 값을 원하는 형식으로 변경할 수 있는지 살펴보겠습니다.

# SQL에서 날짜 값 포맷팅하기

<div class="content-ad"></div>

데이터셋의 날짜 및 시간 값이 사용하기 어려운 형식에 있다면, 새로운 보다 가독성 좋은 형식으로 변환해야 합니다.

또한 종종 서로 다른 데이터셋 간 일관성을 유지하거나 현지의 날짜 및 시간 값 형식 요구 사항과 일치시키기 위해 특정 형식의 날짜 및 시간 값이 필요할 수 있습니다.

게다가 SQL 쿼리 결과를 CSV 파일로 내보내려 할 때는 날짜 값이 올바르게 해석되도록 특정 형식이어야 할 수도 있습니다.

하지만, 날짜 값의 형식을 변경하는 것은 일반적이거나 내장 함수를 사용하여 수행하는 방법을 모른다면 꽤 시간이 많이 소요될 수 있습니다.

<div class="content-ad"></div>

다시 한번, SQL의 서로 다른 방언은 다양한 함수를 사용합니다. 이번 파트의 끝에 그것들을 나열하겠습니다.

만약 MySQL Workbench를 사용 중이라면 아래와 같이 DATE_FORMAT 함수를 사용해야 합니다.

```js
SELECT order_date
    , DATE_FORMAT(order_date, '%e-%M-%Y') as date_format_dmy
    , DATE_FORMAT(order_date, '%D %M, %Y') as date_format_dthmy
    , DATE_FORMAT(order_date, '%M-%d-%y') as date_format_dmy
FROM alldata.salesdata
WHERE 1=1
AND sales_manager = 'Pablo'
AND product_category = 'Healthcare'
AND order_date BETWEEN '2021-08-01' AND '2021-08-15';
```

![SQL Date Time](/assets/img/2024-07-14-3PowerfulSQLQueriesToWorkWithDate-TimeData_7.png)

<div class="content-ad"></div>

당신이 해야 할 일은 날짜 및 시간 값을 포함하는 열 이름을 제공하고 출력에서 보고 싶은 날짜 형식을 지정하는 것뿐입니다.

이제 여러분이 궁금해하실 수도 있는데, 이러한 값인 %m, %Y 또는 %d가 무슨 의미인지 알아볼 차례입니다. 여기 이러한 값들과 그 의미에 대한 전체 목록이 있습니다.

- %Y — 4자리 연도
- %y — 2자리 연도
- %M — 전체 월 이름
- %m — 월 숫자
- %d — 두 자리 형식의 일 수
- %D — 'th'가 뒤따르는 일 수
- %H — 시간
- %i — 분
- %s — 초

SQL Server를 사용하고 있다면 FORMAT 함수를 사용할 수 있으며, AWS Redshift를 사용하고 있다면 TO_DATE 또는 TO_CHAR 함수를 사용할 수 있습니다.

<div class="content-ad"></div>

That’s all for this article!

If you enjoyed this content, make sure to explore my other articles as well. They will definitely enhance your SQL knowledge.

I trust you found this article insightful, practical, and easy to follow. 

Extracting information such as month or year from your data can provide you with detailed insights and allow you to uncover hidden trends and patterns. These insights will ultimately support you in making well-informed decisions.

<div class="content-ad"></div>

해당 내용 중 유용한 부분에 강조를 표시하고 생각을 댓글로 남겨보세요.

전문가 팁과 트릭을 더 보려면 팔로우해 주시고, 이 글을 꼭 여러분의 네트워크와 공유해 주세요.

읽어 주셔서 감사합니다.