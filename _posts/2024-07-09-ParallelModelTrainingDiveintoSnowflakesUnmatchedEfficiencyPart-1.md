---
title: "병렬 모델 학습 Snowflake의 비할 데 없는 효율성 탐구 1부"
description: ""
coverImage: "/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_0.png"
date: 2024-07-09 11:26
ogImage: 
  url: /assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_0.png
tag: Tech
originalTitle: "Parallel Model Training: Dive into Snowflake’s Unmatched Efficiency (Part -1)❄️"
link: "https://medium.com/@maseedilyas9848/parallel-model-training-dive-into-snowflakes-unmatched-efficiency-%EF%B8%8F-b4aeb3469f56"
---


![image](/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_0.png)

헤이, 데이터 팸! 컴퓨터가 멋진 머신 러닝 모델들을 훈련하는 데 영원히 걸리는 느낌이 들었던 적이 있나요? 우리 모두가 그런 적이 있습니다. 그런데 만약 여러 모델을 동시에 훈련시킬 수 있다면, 마치 초능력을 가진 훈련 머신을 가지고 있는 것과 같은 느낌이 들지 않을까요?

머신 러닝 모델을 훈련하는 것은 흥미로울 수 있지만, 훈련이 완료되기를 기다리는 시간은 그렇게 재미있지 않을 수도 있어요. 기어이 움직이는 프로그레스 바를 바라보고 있어야 한다는 것에 굴욕받았나요? Snowflake가 여러분의 구원자가 될지도 모릅니다! 본 블로그 글은 Snowflake에서 병렬 모델 훈련에 대해 소개합니다. 동시에 여러 모델을 훈련시키고, 훈련 시간을 단축시키고 작업 흐름을 가속화할 수 있는 상상해보세요. 정말 멋진 일이 아닌가요?

Snowflake의 병렬 처리가 이런 효율성을 발휘하는 방법을 살펴보고, 혜택을 소개하며 여러분이 머신 러닝 프로젝트를 더욱 빠르게 완성할 수 있도록 도와드릴 거에요. 그러니 훈련 시간의 우울증을 떨쳐버리고 Snowflake를 통해 병렬 처리의 힘을 펼치기 준비하세요!

<div class="content-ad"></div>

먼저 해결책에 접근하기 전에 지금 다루고 있는 사용 사례에 대해 이야기해 보겠습니다. 이 특정 블로그에서는 특히 여러 매장 간 판매를 예측하는 소매 업계의 매출 예측 사용 사례를 살펴볼 것입니다.

모든 매장을 위해 하나의 큰 모델을 학습시키는 것은 효율적으로 보일 수 있지만 종종 목표를 이루지 못할 수 있습니다. 각 매장에는 고유한 고객층, 제품 믹스 및 지역적 요인이 있습니다. 일률적인 접근은 부정확한 예측을 초래할 수 있어 재고, 인력 및 마케팅을 최적화하는 데 어렵게 만들 수 있습니다.

이 블로그에서는 Snowflake 상에서 병렬 모델 학습에 대해 자세히 살펴보겠습니다. Snowflake의 병렬 처리 능력을 활용하여 각 매장에 대해 별도로 맞춤형 모델을 동시에 학습한다고 상상해 보세요! 이 접근 방식이 어떻게 학습 속도를 극적으로 향상시키고 모델 정확도에 영향을 미치지 않으면서 각 매장의 제품 수준에서 매출 트렌드를 예측할 수 있는지 살펴볼 것입니다.

이제 Snowflake에서 병렬 처리가 어떻게 작동하는지 알아보겠습니다!

<div class="content-ad"></div>

눈꽃 가상 데이터 창고는 병렬로 워크로드를 실행할 수 있는 매우 강력한 컴퓨팅 엔진입니다. 일반적으로 눈꽃 데이터 창고는 노드 클러스터로 구성됩니다. 예를 들어 —

- XS(엑스트라 스몰) 사이즈 눈꽃 창고는 클러스터 당 1개의 노드를 가지고 있습니다.
- Small 사이즈 눈꽃 창고는 클러스터 당 2개의 노드를 가지고 있습니다.
- Medium 사이즈 눈꽃 창고는 클러스터 당 4개의 노드를 가지고 있습니다. 등등.

일반적으로 눈꽃에서 머신러닝 모델을 훈련시키기 위해, 우리는 파이썬 기반의 저장 프로시저를 사용할 수 있습니다. 그리고 저장 프로시저는 눈꽃에서 단일 노드 작업이기 때문에 모든 가능한 코어를 활용할 수 없습니다. 이 문제를 해결하기 위해 우리는 UDTF(즉, 사용자 정의 테이블 함수)를 활용할 수 있습니다. 이것이 작동하는 방식입니다: UDTF는 지능적인 데이터 프로세서 역할을 합니다. 데이터를 변환하는 것뿐만 아니라 데이터셋 내 각 상점에 대해 고유한 결과를 생성할 수 있습니다. 이를 통해 각 상점에 맞는 개별 훈련 데이터셋을 생성하여 그 위치의 특정 판매 동향과 패턴을 포착하는 모델을 학습할 수 있습니다.

<div class="content-ad"></div>

진정한 마법은 데이터 파티셔닝으로 펼쳐집니다. UDTF(사용자 정의 테이블 함수)는 선택한 열, 예를 들어 STORE_ID를 기반으로 데이터를 똑뗉하게 분할할 수 있습니다. 이로써 특정 상점에 관련된 정보가 담긴 별도의 데이터 파티션들이 생성됩니다.

이제 Snowflake의 병렬 처리가 중요한 역할을 합니다. 2-노드 웨어하우스에서 한 노드가 Store X를 위한 모델을 학습하고 있는 동안, 다른 노드는 동시에 Store Y를 위한 모델을 학습하고 있습니다. 마치 두 개의 미니 학습 공장이 병렬로 작동하는 것과 같아요. 전체 프로세스를 크게 가속화할 수 있습니다.

이 접근 방식은 모든 상점을 위한 포괄적인 단일 모델을 학습하는 대기 시간을 제거합니다. 대신 병렬로 여러 사용자 정의 모델을 동시에 훈련하여 Snowflake 웨어하우스의 효율성을 극대화할 수 있습니다.

UDTF를 구축하는 데는 데이터 파티셔닝과 모델 학습을 효과적으로 처리할 기술적 전문 지식이 필요할 수 있습니다. 2-노드 웨어하우스를 사용하면 2개의 모델을 동시에 학습할 수 있습니다. 상점 수가 많다면 웨어하우스를 확장하여 병렬 처리 능력을 더욱 가속화할 수 있습니다.

<div class="content-ad"></div>

![Screenshot](/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_1.png)

위 다이어그램에서 보듯이, 데이터 파티션에서 작동하는 사용자 정의 테이블 함수(UDTF)를 생성해야 합니다. 그런 다음 이 UDTF를 파티션 절이 있는 SELECT 쿼리에서 호출하여 이러한 파티션에 따라 데이터를 처리할 수 있도록 합니다. 각 파티션은 단일 노드에서 머신러닝 모델을 훈련하는 데 사용됩니다. 사용 가능한 노드 수에 따라 X개의 모델이 병렬로 훈련되며(X는 데이터웨어하우스의 노드 수입니다).

# UDTF 생성:-

UDTF는 테이블 형태의 결과를 반환하는 사용자 정의 함수(UDF)입니다. 다양한 유형의 UDTF에 대한 자세한 정보는 내 이전 블로그인 Snowpark 마스터리를 참조하세요.

<div class="content-ad"></div>

Python UDTF를 만들려면 Snowflake가 UDTF를 호출할 때 호출할 메서드가 구현된 클래스를 작성해야 합니다. 샘플 UDTF는 아래와 같이 보일 것입니다.

```js
create or replace function stock_sale_sum(symbol varchar, quantity number, price number(10,2))
returns table (symbol varchar, total number(10,2))
language python
runtime_version=3.8
handler='StockSaleSum'
as $$
class StockSaleSum:
    def __init__(self):
        self._cost_total = 0
        self._symbol = ""

    def process(self, symbol, quantity, price):
        self._symbol = symbol
        cost = quantity * price
        self._cost_total += cost
        yield (symbol, cost)

    def end_partition(self):
        yield (self._symbol, self._cost_total)
$$;
```

위 UDTF에는 StockSaleSum이라는 클래스와 생성자 뒤에 process와 end_partition이라는 두 개의 메서드가 있습니다.

- process 메서드는 각 입력 행을 처리하고 튜플로 된 탭릿값을 반환합니다. 위 예제에서는 각 입력 행에 대해 비용을 계산하고 symbol과 cost 두 열을 튜플로 반환합니다. process 메서드는 UDTF가 수신하는 각 입력 행에 대해 호출됩니다. yield 키워드를 사용하여 발생자 객체를 반환합니다.
- End partition 메서드는 입력 파티션 처리를 완료하고 튜플로 된 탭릿값을 반환합니다. 위 예제에서는 입력 데이터의 각 symbol에 대해 파티션당 총 비용을 계산합니다. UDTF의 end partition 메서드는 선택 사항이며 입력 데이터를 파티션으로 나눌 필요가 있는 경우에만 필요합니다.

<div class="content-ad"></div>

프리파플이 여러분을 안내해요! 아래는 파티션별로 실행되는 코드가 포함된 경우 우리 핸들러에 대한 호출 순서를 설명하고 있어요.

- Snowflake는 파티션 처리가 시작될 때, 첫 번째 행이 처리되기 전에 핸들러 클래스의 __init__ 메서드를 사용하여 클래스의 인스턴스를 생성해요.
- 여기서 우리는 파티션 범위의 상태를 설정할 수 있어요. 예를 들어, 우리는 파티션의 행에서 계산된 값을 보관하는 인스턴스 변수를 초기화할 수 있어요.
- 각 파티션의 각 행에 대해, Snowflake는 process 메서드를 호출해요.
- 메서드가 실행될 때마다, 상태 값을 변경할 수 있어요. 예를 들어, process 메서드를 통해 인스턴스 변수의 값을 업데이트할 수 있어요.
- 코드가 파티션에서 마지막 행을 처리한 후, Snowflake가 end_partition 메서드를 호출해요.

이제 여러 모델을 병렬로 훈련시키기 위한 UDTF를 만들어보죠.

먼저, 다루고 있는 데이터에는 아래 열이 있어요:

<div class="content-ad"></div>

이미지 링크를 Markdown 형식으로 변경해보세요.


![이미지](/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_2.png)


데이터 세트에는 약 10 개의 서로 다른 상점 ID가 있으며 각 상점마다 하나의 머신러닝 모델을 병렬로 훈련하고 싶습니다.

우선 아래 코드를 사용하여 클래스와 생성자 메서드를 만들어 봅시다.

```js
class ParallelTraining:
    def __init__(self):
        self.product_id=[]
        self.store_id=[]
        self.order_quarter=[]
        self.order_month=[]
        self.order_year=[]
        self.total_sales=[]
```

<div class="content-ad"></div>

이 상단 구조체 메서드는 모든 변수를 리스트로 초기화하는 데 사용됩니다. 이러한 리스트는 각 파티션에 대한 모든 입력 데이터를 저장하는 데 사용됩니다.

아래와 같이 process 메서드를 정의합시다.

```js
def process(self, product_id, store_id, order_quarter, order_month, order_year, total_sales):
        self.product_id.append(product_id)
        self.store_id.append(store_id)
        self.order_quarter.append(order_quarter)
        self.order_month.append(order_month)
        self.order_year.append(order_year)
        self.total_sales.append(total_sales)
```

process 메서드는 각 입력 행을 처리하고 구조체 메서드에서 생성한 리스트에 데이터를 추가합니다.

<div class="content-ad"></div>

Finally, let's define our `end_partition` method:

```python
def end_partition(self):
    df = pd.DataFrame(zip(self.product_id, self.store_id, self.order_quarter, self.order_month, self.order_year, self.total_sales), columns=['product_id', 'store_id', 'order_quarter', 'order_month', 'order_year', 'total_sales'])
    
    # Splitting the dataset
    X_train, X_test, y_train, y_test = train_test_split(df.drop(["total_sales"], axis=1), df["total_sales"], test_size=0.2)
    
    # Training the model
    model = LinearRegression()
    model.fit(X_train, y_train)
    
    # Making predictions
    y_pred = model.predict(X_test)
    
    random_string = str(uuid.uuid4())
    
    pred_df = pd.DataFrame({"predicted_orders": y_pred})
    for idx, row in pred_df.iterrows():
        yield (row["predicted_orders"], random_string)
```

The `end_partition` method is called after processing the last row in the partition. It creates a Pandas dataframe using the lists defined in the constructor. Then, it splits the data into training and testing sets, trains a simple linear regression model, makes predictions, and returns the predicted orders with a random string. The random string is used to verify if the User-Defined Table Function (UDTF) is training a model per partition. If it does, the random string value will be the same for all rows within a partition and different for each partition.

Let's bring all the pieces together to complete the process.

<div class="content-ad"></div>

위의 코드는 파티션 당 하나의 모델을 학습하고 학습된 모델을 사용하여 예측을하는 최종 UDTF 코드입니다. 이 모든 과정은 병렬로 이루어집니다.

UDTF를 실행하려면 다음과 같이 파티션 단위로 선택 쿼리가 필요합니다

```js
-- 파티션 절을 포함한 UDTF를 호출하는 쿼리
-- combined_sales는 제 입력 테이블
-- parallel_training은 제 UDTF
-- 우리는 각 상점당 모델 하나를 학습하기 위해 store_id로 파티션을 나눕니다

select store_id, predicted_orders, model_name
from combined_sales,
table(parallel_training(product_id,store_id,order_quarter, order_month, order_year, total_orders)
 over (partition by store_id));
```

<div class="content-ad"></div>

위 쿼리를 실행하면 다음과 같은 결과가 나타납니다.

![Image 1](/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_3.png)

![Image 2](/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_4.png)

각 파티션마다 사용한 model_name (끝 부분 파티션 방식으로 사용한 랜덤 문자열)이 다르게 보입니다. 그리고 모든 파티션 내의 모든 행에서 model_name이 동일하다는 것을 확인할 수 있습니다. 이는 UDTF가 각 파티션을 별도로 처리하고 병렬로 실행했다는 것을 의미합니다. 또한 10개의 모델을 병렬로 훈련하고 예측을 수행하는 데 7.3초가 소요되었습니다. 반면에, ML 모델을 순차적으로 훈련시켜야 할 경우 (하나씩), 저장 프로시저를 사용한다면 모델을 훈련하는 데 몇 분이 소요될 것입니다.

<div class="content-ad"></div>

이렇게하면 눈펌프의 강력한 UDTF를 활용하여 병렬로 여러 모델을 훈련시킬 수 있어요. 그런데 잠깐, 잔디가 항상 푸릇푸릇한 것만은 아니에요. 이 방법에는 다음과 같은 제한 사항도 있어요

- UDTF는 Snowflake 세션 개체를 사용하여 훈련된 모델을 피클 파일로 저장할 수 없기 때문에 훈련된 모델을 저장할 수 없어요. 모델 레지스트리나 Snowflake 스테이지로도 저장할 수 없어요.

즉, 우리는 매번 훈련과 추론을 해야 해요. 이를 어떻게 극복할 수 있는지에 대해서는 걱정하지 마세요.

![이미지](/assets/img/2024-07-09-ParallelModelTrainingDiveintoSnowflakesUnmatchedEfficiencyPart-1_5.png)

<div class="content-ad"></div>

다음 블로그 포스트에서는 이 제약을 극복할 다른 방법을 살펴보겠습니다! 그러니 기대해 주세요!

결론적으로, Snowflake UDTFs와 병렬 처리는 소매업체 체인 내 각 상점에 대한 맞춤형 판매 예측 모델을 학습하는 강력한 솔루션을 제공했습니다. 이 접근 방식은 상당한 효율성 향상을 불러오며 초개인화된 판매 예측을 가능하게 합니다. Snowflake 웹사이트에서 UDTF 개발 및 병렬 처리에 대한 권장 사항을 더 알아보기 위해 리소스를 탐색해 보세요. Snowflake의 능력을 활용하여 머신러닝 파이프라인을 혁신해 보세요!

언제나 만약 이 탐구가 유익하다고 느꼈다면 박수를 보내주는 것을 망설이지 마세요. 그리고 더 매혹적인 블로그를 위해 저를 팔로우해주세요. LinkedIn에서도 저를 팔로우하여 계속 소통할 수 있도록 해주세요.