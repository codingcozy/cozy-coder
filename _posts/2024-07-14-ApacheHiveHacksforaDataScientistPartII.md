---
title: "데이터 과학자를 위한 Apache Hive 핵심 팁 파트 II"
description: ""
coverImage: "/code-tower.github.io/assets/no-image.jpg"
date: 2024-07-14 01:46
ogImage: 
  url: /code-tower.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Apache Hive Hacks for a Data Scientist: Part II"
link: "https://medium.com/towards-artificial-intelligence/apache-hive-hacks-for-a-data-scientist-part-ii-84ba1f19a19b"
isUpdated: true
---





아파치 하이브는 SQL 명령어를 통해 대용량 데이터를 처리하는 SQL 엔진입니다. 하이브와 왜 데이터 과학자/머신러닝 엔지니어를 위한 것인지에 대해 더 알고 싶다면, 이전 게시물을 확인해주세요. 오늘은 머신러닝 파이프라인에서 대용량 데이터를 사용할 때 매우 유용한 몇 가지 SQL 해킹에 대해 깊게 살펴보려고 합니다.

큰 데이터 엔지니어로서 대용량 데이터에서 머신러닝 세계로 전향한 저는 하이브를 머신러닝 파이프라인에 삽입하는 다양한 사례를 발견했습니다. 아래는 몇 가지 그런 예시입니다:

- 데이터 분석: 이진 또는 범주형 특성이 있는 경우가 있고 이러한 특성의 분포를 대규모로 파악해야 하는 경우를 가정해봅시다. 대규모 데이터라 함은 매월 1억 건의 데이터가 1년 동안 축적된 경우를 의미합니다. 이 데이터를 Python에 로드하려면 500GB의 메모리를 갖춘 강력한 컴퓨터가 필요하며 일부 데이터만 로드할 수 있을 것입니다. 샘플링은 정확한 필요 숫자를 알려주지 않습니다. 하이브는 이러한 통계를 수집하는 쉬운 방법입니다.
- 피처 엔지니어링: 스파크가 대규모로 피처를 생성하는 엔진으로 가장 인기 있는 선택지이지만, 스칼라/파이썬에서 코드를 컴파일하는 것보다 하이브를 사용하는 것이 더 간편한 경우도 있습니다. 예를 들어, 특정 특성의 월별 집계 또는 연간 롤링 집계를 생성하려는 경우, 이를 스파크를 사용하여 정규화된 또는 평균 특성을 생성하는 데 추가로 활용할 수 있습니다.
- 모델링: 하이브가 모델링 파이프라인에서 빛을 발하는 또 다른 주요 영역은 Python 프레임워크를 사용하여 만들기 어려운 거대한 데이터셋을 위한 모델링 샘플을 만드는 것입니다.

지금까지 머신러닝 파이프라인에서 하이브의 적용 가능성에 대해 많이 이야기했습니다. 이어서 하이브 해킹에 대해 더 알아보겠습니다. 이전 블로그 게시물에서 초기 하이브 해킹을 확인하려면 이 블로그를 참조해주세요.

<div class="content-ad"></div>

오늘은 주로 하이브 필터 조건에 초점을 맞출 것입니다. 필터 기준을 사용하면 테이블의 열에 다양한 조건을 적용하여 원하는 레코드/행을 검색할 수 있습니다. 이 블로그에서는 시연용으로 이전 `블로그`에서 논의한 것과 동일한 테이블을 사용할 것입니다. 소비자 활동에 대한 매일 데이터가 매일 디렉토리에 기록됩니다. 두 개의 테이블이 있습니다:

- 'user_details' 테이블은 ID, 이름 및 연령과 같은 사용자 세부 정보가 포함된 작은 테이블입니다.
- 'hourly activity' 테이블에는 하루 종일 모든 사용자의 활동 항목이 있습니다.

## 1. 필터 조건: records in operator

당신의 데이터에 사용자 범주가 다음과 같은 값을 가지고 있는 경우: 활동 중, 비활성, 휴면, 그리고 활동 중인 사용자만 포함하려는 경우, 하이브 쿼리에서 쉽게 다음과 같이 할 수 있습니다:

<div class="content-ad"></div>


사용자 세부 정보 중에서 사용자 이름, 사용자 나이, 매시간 활동 수, 활동 시간을 가져오는 쿼리입니다. 사용자 세부 정보 테이블과 시간별 활동 테이블을 조인하여 사용자 ID가 일치하는 경우 출력합니다. 사용자 카테고리가 '활성' 또는 '휴면'인 경우에 해당합니다.

## 2. 필터 조건: 다른 테이블에서 조건 없음으로 필터링

모델을 학습하여 어떤 사용자가 비활성 상태인지 예측하는 샘플을 생성한다고 가정해봅시다. 데이터 양이 많으면 Hive를 통해 이러한 필터링을 쉽게 수행할 수 있습니다. 긍정 클래스(비활성 사용자) 및 부정 클래스(비활성이 아닌 사용자)를 위한 레코드를 필터링해야 할 수도 있습니다. 비활성 사용자에 대한 예제를 가져오기 위해 사용자 카테고리인 '활성' 및 '휴면'이 아닌 경우를 가져오기 위해 'not in' 절을 추가하여 간단히 수행할 수 있습니다. 또한 실제로 비활성이며 클라이언트/외부 엔티티가 공유한 다른 테이블에서 오는 고객을 더 세부적으로 필터링/타겟팅할 수 있습니다:

```sql
select user_details.user_name, 
user_details.user_age, 
hourly_act.num_of_games, 
hourly_act.active_min 
from user_details join hourly_act 
on user_details.userid = hourly_act.user_id
where
user_details.category not in ('active', 'dormant')
and 
user_details.user_name is in (select user_name from external_user_details);
```

<div class="content-ad"></div>

상기 테이블 external_user_details는 외부 클라이언트에서 제공되거나 알려진 일련의 사용자로, 당신의 모델의 레이블입니다. 기계 학습에서 레이블은 모델에서 예측하려는 행동을 나타내는 Ground Truth 값입니다. 이 경우 우리의 레이블은 확실히 비활성인 것으로 알려진 일련의 사용자이며, 이를 모델에 제대로 예측하도록 학습시키기 위해 사용됩니다.

부정 클래스 (활성 사용자)에 대해:

- 활성 사용자의 모든 예제를 얻기 위해 첫 번째 'not in'을 'is in'으로 변경할 수 있습니다.
- 그리고 비활성으로 알려진 사용자를 제외하기 위해 두 번째 'is in'을 'not in'으로 변경할 수 있습니다.

## 3. Filter 조건: 열을 고정하여 변환하기

<div class="content-ad"></div>

이제 모델에 사용자를 찾는 기능을 추가해야하는데, 해당 기능은 게임 분야에서 가장 활발한 인구 집단인 Gen Z를 찾고 싶습니다. 문제는 세대에 대한 데이터가 문자열 열 'generation'에 있으며 'Gen Z'가 값 5로 코드화되어 있다는 것입니다. Hive에는 이에 대한 솔루션이 있습니다. Hive에서 테이블에서 열을 가져와 원하는 데이터 유형으로 변환할 수 있습니다. 이를 Hive에서 어떻게 수행할 수 있는지 알아봅시다:

```js
select user_details.user_name, 
user_details.user_age, 
hourly_act.num_of_games, 
hourly_act.active_min 
from user_details join hourly_act 
on user_details.userid = hourly_act.user_id
where
cast(user_details.generation as int) == 5;
```

여기서는 테이블의 'generation' 열을 가져와 cast()를 사용하여 정수로 변환한 후, generation = 5인 사용자만 포함되도록 조건을 추가했습니다. Gen Z를 뜻합니다.

## 4. 필터 조건: 열이 널이 아닌지 확인하기

<div class="content-ad"></div>

데이터 누락은 기계 학습 데이터 세트에서 매우 흔한 현상입니다. 하이브에는 누락된 데이터가 포함된 열을 식별하는 방법이 있습니다. 누락된 데이터를 찾으면 해당 누락/널 값에 대해 조치를 취할 수 있습니다. 예를 들어 위의 사용 사례에서 'Gen Z'에 대해서만 레코드를 필터링하려면 해당 열이 있는 경우에만 필터링할 수 있도록 위의 쿼리를 수정하여 다음과 같은 비널 체크 조건을 추가할 수 있습니다:

```sql
select user_details.user_name, 
user_details.user_age, 
hourly_act.num_of_games, 
hourly_act.active_min 
from user_details join hourly_act 
on user_details.userid = hourly_act.user_id
where
isnotnull(user_details.generation)
and
cast(user_details.generation as int) == 5;
```

## 5. 필터 조건: 고유 레코드 가져오기

기계 학습 문제에서 많은 경우 우리는 솔루션을 모델링할 다양한 데이터 범주가 필요합니다. 예를 들어 'Gen Z' 세대 및 소득이 100K인 사용자를 대상으로 모델을 만들고 싶다면, 데이터 세트를 생성할 때 두 범주를 여러 번 선택할 수 있습니다. 이때 하이브의 distinct 지원이 빛을 발합니다. 이 시나리오에 대한 데이터를 가져오기 위한 하이브 쿼리를 살펴보겠습니다:

<div class="content-ad"></div>


Let's simplify this complex query step by step:

- The first inner select block is filtering users from the 'Gen Z' generation.
- The second inner select block is filtering users with an income ≥ 100,000.
- The union combines the results from the above queries.
- Some user records may overlap in both queries.
- The outer select query with DISTINCT removes duplicates and gives unique user records meeting both criteria of 'Gen Z' and income over 100K.

This wraps up our discussion on Hive hacks for machine learning enthusiasts. I hope this blog sheds light on how impactful Hive can be for data scientists and ML engineers. From null filtering to converting data types and selecting specific data categories for modeling, Hive offers a wide range of functionalities. Leveraging its big data support, we can handle data volumes from gigabytes to petabytes for various modeling tasks.


<div class="content-ad"></div>

이 블로그를 좋아하시면 더 많은 데이터 과학과 빅데이터 관련 흥미로운 주제에 대해 더 알고 싶다면 medium.com에 가입해보세요. 그렇게 하면 medium.com에서 더 많은 스토리에 완전히 접근할 수 있습니다.