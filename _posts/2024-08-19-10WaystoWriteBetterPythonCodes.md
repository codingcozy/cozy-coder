---
title: "파이썬 코드를 더 멋지게 작성하는 10가지 방법"
description: ""
coverImage: "/assets/img/2024-08-19-10WaystoWriteBetterPythonCodes_0.png"
date: 2024-08-19 02:03
ogImage: 
  url: /assets/img/2024-08-19-10WaystoWriteBetterPythonCodes_0.png
tag: Tech
originalTitle: "10 Ways to Write Better Python Codes"
link: "https://medium.com/gitconnected/10-ways-to-write-better-python-codes-55fc862ab0ef"
isUpdated: true
updatedAt: 1724033054864
---


## 꼭 알아야 할 일상 팁과 노하우!

![이미지](/assets/img/2024-08-19-10WaystoWriteBetterPythonCodes_0.png)

2024년의 절반정도가 지나갔습니다. 기술 산업은 Generative AI와 Large Language Models의 등장으로 더 빠르게 움직이고 있습니다.

데이터 애호가이자 파이썬 개발자로서, 오늘날 경쟁이 치열한 시장에서 필요한 멋진 팁과 노하우를 알면서 수많은 기술 분야의 사람들이 해고되는 상황에서 계속 중요하게 대처할 수 있습니다.

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

여기서 주목해야 할 첫 번째 사항은,

## 왜 Python을 사용해야 할까요?

Python은 매우 강력한 일반 목적 프로그래밍 언어로, 데이터 과학 관련 작업에 매우 적합합니다. SciKit-Learn, TensorFlow, PyTorch와 같은 놀라운 프레임워크 및 다양한 라이브러리를 활용하여 쉽게 이해할 수 있는 문법으로 만들어진 Python은 다양한 응용 프로그램을 만들 수 있는 유연성을 제공합니다.

따라서 Python을 습득하면 자동화 작업이나 오픈 소스 LLMs와 상호 작용하는 챗봇을 작성하는 등 다양한 응용 프로그램을 만들 수 있습니다. 특히 초보자라면 다른 프로그래밍 언어와 비교하여 Python을 배우는 것이 상당히 직관적이고 빠릅니다.

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

그렇다면 코딩으로 들어가보도록 하죠!

## 어떻게 더 나은 Python 코드를 작성할 수 있을까요?

코딩 방법을 개선하고 발전시킬 수 있는 10 가지 팁에 대해 이야기해 보겠습니다.

### 1. 메모리 저장을 위해 "Generator" 사용하기

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

일반적으로 `large_file`을 메모리에 저장할 수 없는 대용량 파일을 분석해야 할 때가 있습니다. 이러한 문제를 효율적으로 해결하기 위해 `process_large_file` 제너레이터 함수를 생성하여 대용량 파일을 한 줄씩 읽는 방법을 사용할 수 있습니다.

```python
def process_large_file(file_path):
    """
    대용량 파일을 한 줄씩 읽는 제너레이터 함수
    """
    with open(file_path, 'r') as file:
        for line in file:
            # 각 줄을 처리
            yield line

# 로그 파일 경로 설정
log_file_path = 'path/to/your/large_file.txt'

# 대용량 파일을 처리
large_file = process_large_file(file_path)

for entry in large_file:
    # 각 로그 항목에 대해 작업 수행
    print(entry)
```

위의 코드에서는 `large_file.txt` 파일에 작성된 출력을 표시합니다.

# 2. "setdefault()"를 사용한 사전(dictionary)에서의 사용법

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

재고 시스템을 관리하고 다양한 제품의 재고 수준을 추적하고자 한다고 가정해봅시다. 시스템에 새 제품이 추가될 때, 해당 제품에 기본 재고 수준이 설정되지 않은 경우를 대비하여 기본 재고 수준을 설정하고 싶습니다.

이러한 프로세스를 간소화하기 위해 setdefault() 함수를 사용할 수 있습니다. 이 함수는 딕셔너리에 특정 키가 이미 존재하지 않으면 지정된 기본값을 가지는 키를 삽입합니다.

```js
# 초기 재고
inventory: dict[str, int] = {"jeans": 500, "top": 600}
              
# 존재하지 않는 제품에 기본 재고 수준 추가
products_to_add: list[str] = ["skirt", "shirt", "tee"]

for product in products_to_add:
    inventory.setdefault(product, 500)

# 최종 업데이트된 재고 출력
print("최종 업데이트된 재고:", inventory)

"""
# 출력:
최종 업데이트된 재고: {'jeans': 500, 'top': 600, 'skirt': 500, 'shirt': 500, 'tee': 500}
"""
```

이 방법을 사용하면 명시적 검사와 할당이 필요하지 않아 코드를 더 간결하고 가독성 있게 만들 수 있습니다.

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

# 3. "if-elif 지옥" 피하기 위해 딕셔너리 사용하기

만약 사용자 입력에 따라 여러 함수를 호출하고 싶다고 가정해보세요.

이 문제를 해결하는 가장 일반적인 방법은 if-elif 문을 사용하는 것인데, 이 방법은 많은 함수를 처리할 때 매우 길고 복잡해질 수 있습니다.

대안적인 접근 방법은 실행할 함수와 대조할 키가 포함된 딕셔너리를 생성하는 것입니다.

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
# 기능 1 
def first():
  print("첫 번째 함수 호출 중...")

# 기능 2
def second():
  print("두 번째 함수 호출 중...")

# 기능 3
def third():
  print("세 번째 함수 호출 중...")

# 기본 함수
def default():
  print("기본 함수 호출 중...")

# 사용자 입력
options: int = int(input("옵션을 입력하세요: "))

# 옵션을 키로, 함수를 값으로 하는 딕셔너리 생성
funcs_dict: dict[int, str] = {1: first, 2: second, 3: third}

# 입력한 옵션이 딕셔너리의 키에 있는지 확인하고, 없으면 기본 함수 실행
final_result = funcs_dict.get(options, default)
final_result()
```

프로그램을 실행하면 화면에 다음과 같은 결과를 볼 수 있습니다.

```python
"""
# 출력:
# 옵션이 0일 때
옵션을 입력하세요: 0
기본 함수 호출 중...

# 옵션이 1일 때
옵션을 입력하세요: 1
첫 번째 함수 호출 중...

# 그리고 계속...
"""
```  

참고: 사용자가 무엇이든 입력하면 `default()` 함수가 실행됩니다.

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

# 4. Collections 모듈에서 "Counter" 사용하기

큰 텍스트 데이터를 다룰 때, 텍스트 분석의 가장 일반적인 작업은 문제 상황에 따라 특정 문서나 전체 말뭉치에서 각 단어의 빈도를 확인해야 하는데요.

Counter는 이터러블에서 요소를 세는 간단하고 효율적인 방법을 제공하여 사용자 정의 계수 논리를 작성하는 복잡성을 추상화합니다.

이를 구현해봅시다,

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
from collections import Counter
import re

# 텍스트 파일 읽기
with open("sample_text.txt", "r") as file:
  text = file.read()

# 텍스트 정리 및 토큰화
cleaned_text: str = re.sub(r'[^\w\s]', '', text.lower().split())

# Counter()를 사용하여 단어 개수 세기
word_counts: Counter = Counter(cleaned_text)

# 두 번째로 많이 나오는 단어 출력
most_common = word_counts.most_common(2)  # 출력할 공통 단어의 개수를 인수로 전달 (숫자는 1-n으로 시작)
print("두 번째로 많이 나오는 단어: ", most_common[0])  # 2개 중에서 0번째 인덱스 요소 출력

"""
# 출력:
두 번째로 많이 나오는 단어: ('data', 82)
"""
```

참고: 또한, Counter 객체를 다른 자료 구조인 딕셔너리로 쉽게 변환하거나 elements(), most_common() 등 유용한 메서드를 사용하여 작업할 수 있습니다.

# 5. 코드 최적화를 위해 "메모이제이션" 사용하기

메모이제이션은 동적 프로그래밍에서 사용되는 기술로, 동일한 입력이 다시 발생할 때 비싼 함수 호출을 재사용하여 재귀 알고리즘의 시간 복잡도를 개선하는 기법입니다.


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

이것의 대표적인 예는 피보나치 수열로 알려진 토끼 문제입니다.

```python
import time

def memo_fibonacci(num: int, dictionary: dict[int, int]):
    if num in dictionary:
        return dictionary[num]
    else:
        dictionary[num] = memo_fibonacci(num-1, dictionary) + memo_fibonacci(num-2, dictionary)
        return dictionary[num]

# Dictionary를 이용해 Cache 활용하기
dictionary: dict[int, int] = {0: 1, 1: 1}

# 소요된 시간
start_time: float = time.time()
# 함수 호출
result: int = memo_fibonacci(48, dictionary)
end_time: float = time.time()
# 경과 시간 계산
elapsed_time: float = end_time - start_time

print(f"결과: {result}") # 결과: 7778742049
print(f"경과 시간: {elapsed_time:.6f} 초") # 경과 시간: 0.000133 초
```

참고: 이것은 시간 복잡도를 크게 줄입니다. 하지만 결과를 저장하는 캐시를 유지해야 하기 때문에 공간-시간의 교환을 고려해야 합니다.

# 6. 중복을 피하기 위해 "데코레이터" 사용하기

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

파이썬 프로젝트를 개발 중이시군요! 함수가 실행되는 데 얼마나 시간이 걸리는지 측정하고 싶을 때 사용할 수 있는 방법에 대해 설명드릴게요. 위에서 언급한 것처럼 해당 함수에 대해 시간 기능을 사용할 수 있지만, 수십 개 또는 백 개의 함수가 있는 경우는 어떨까요?

‘start-time’과 ‘end-time’을 쓰는 데 영원이 걸릴 텐데, 대신에 우리가 직접 `elapsed_time` 함수를 만들어 사용할 수 있어요. 우리가 시간을 측정하려는 함수 위에 `@elapsed_time`을 추가하기만 하면 됩니다.

파이썬은 @ 기호를 보고 그 아래의 함수가 `elapsed_time` 함수에 전달되어야 함을 이해하고, 해당 함수가 `elapsed_time`에서 실행되어 원하는 만큼의 함수를 시간 측정할 수 있도록 코드를 감싼 후 실행합니다.

```python
import time

def elapsed_time(func):
    def wrapper():
        start_time: float = time.time()
        
        func()
        
        end_time: float = time.time() - start_time
        print(f"{func.__name__}() took {end_time:.6f} seconds")
        
    return wrapper

@elapsed_time
def some_code():
    # 실행 중인 코드를 시뮬레이션합니다.
    time.sleep(0.00002)

# 함수 호출
some_code() # some_code() took 0.000009 seconds
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

일지, 시간 측정, 접근 제어 강화 등에 널리 사용됩니다.

## 7. `dataclass`를 사용하여 깔끔한 데이터 구조 만들기

데이터 값을 보유하기 위해 설계된 일반 클래스에서 __init__ 메서드를 반복적으로 작성하는 것은 잠재적 오류 증가로 인해 정말 지루합니다. 

![이미지](/assets/img/2024-08-19-10WaystoWriteBetterPythonCodes_1.png)

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

그러나 Python 3.7에서 소개된 dataclasses 모듈은 프로그램의 다른 부분 사이에 전달될 데이터를 저장하는 더 효율적인 방법입니다.

```python
from dataclasses import dataclass

@dataclass
class Employee:
    id_: int
    name: str
    salary: float

e1 = Employee(id_=1, name='Tusk', salary=69999.99)
print(e1) # Employee(id_=1, name='Tusk', salary=69999.99)
```

여기서 출력 결과는 마치 __repr__을 사용하여 구현된 표준 Python 클래스와 동등합니다.

참고: Employee 클래스의 표현 방식을 사용자 정의할 수도 있습니다:

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
from dataclasses import dataclass

@dataclass
class Employee:
    id_: int
    name: str
    salary: float

    def __repr__(self):
       return f"{self.name} earns ${self.salary}."

e1 = Employee(id_=1, name='Tusk', salary=69999.99)
print(e1) # Tusk earns $69999.99.
```

만약 보일러플레이트 코드를 줄이고 코드를 더 읽기 쉽고 유지보수성 높게 만들고 싶다면 dataclass를 사용해보세요.

# 8. 깔끔한 입력 처리를 위한 "match" 사용하기

Python 3.10부터 새롭게 도입된 구조적 패턴 매칭은 match 패턴과 관련된 case 문을 사용하여 입력을 처리합니다.

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

"Point"라는 클래스가 있는데, 이는 2D 좌표계에서 한 점을 나타냅니다. 이제 사용자가 입력한 경우를 처리하기 위해 2D 평면 상의 점을 찾는 함수 "where_is"를 만들겠습니다.

`match` 문은 식을 가져와서 그 값과 연속적인 패턴 case 블록을 비교합니다.

```js
from dataclasses import dataclass

# dataclass를 사용하여 클래스 정의
@dataclass
class Point:
    x: int
    y: int

# 서로 다른 경우를 처리하기 위한 match 문
def where_is(point):
    match point:
        case Point(x=0, y=0):
            return ("Origin")
        case Point(x=0, y=y):
            return (f"Y={y}")
        case Point(x=x, y=0):
            return (f"X={x}")
        case Point(x, y):
            return ("다른 곳")
        # 사용자가 입력하는 모든 것을 잡기 위함
        case _:
            return ("점이 아닙니다")

# 예시
print(where_is(Point(0, 0)))   # 결과: Origin
print(where_is(Point(0, 10)))  # 결과: Y=10
print(where_is(Point(10, 0)))  # 결과: X=10
print(where_is(Point(10, 10)))  # 결과: 다른 곳
print(where_is("점이 아닙니다"))  # 결과: 점이 아닙니다
```

match-case 문을 사용하면 모든 가능한 경우를 처리할 수 있어서 완전한 패턴 매칭을 보장할 수 있습니다.

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

# 9(A). “and” 대신 “all” 연산자 사용하기

유저 프로필 시스템을 구축 중이라고 상상해봅시다. 폼에 입력된 모든 필수 필드를 사용자가 입력했는지를 확인하고 싶을 때가 있을 겁니다 (뭐 굳이 폼에 *필수 입력란 표시를 안 한 이유는 모르겠지만, 여기선 그냥 집중하도록 합시다👇).

어쨌든 제공된 iterable의 모든 요소가 True인 경우에만 True를 반환하는 all 함수를 사용해서 and 조건 대신 사용할 수 있어요.

```js
# 사용자 등록 폼에서 사용자 입력
form_data: dict[str, str] = {"name" : "Nikita", 
             "email": "analyticalnikita@gmail.com",
             "phone": "123478911"}

# 필수 필드 목록
required_fields: list[str] = ["name", "email", "phone"]

# all 연산자 사용
if all(field in form_data for field in required_fields):
    print("모든 필수 필드가 작성되었습니다.")
else:
    print("일부 필수 필드가 누락되었거나 비어 있습니다.")

"""
# 출력:
모든 필수 필드가 작성되었습니다.
"""
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

# 9(B). "or" 대신 "any" 연산자 사용하기

any 함수는 반복 가능한 객체에 제공된 요소 중 하나라도 True인 경우 True를 반환합니다.

예를 들어, 특정 기준에 따라 특정 사용자에게 권한을 제한해야 하는 경우, or 조건 대신 any를 사용할 수 있습니다.

```js
# 사용자에게 할당된 권한 목록
user_permission: list[str] = ["read", "execute"]

# 사용자가 필요한 권한 중 하나 이상을 가지고 있는지 확인
required_permissions: list[str] = ["write", "read", "admin"]
    
# "all" 연산자 사용
if any(permission in user_permission for permission in required_permissions):
    print(f"필요한 권한을 가지고 있어서 접속이 허용됩니다.")
else:
    print("당신은 표준 사용자입니다. 접속이 허용되지 않습니다.")

"""
# 결과:
필요한 권한을 가지고 있어서 접속이 허용됩니다.
"""
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

이것들은 어떻게 any와 all을 사용하여 여러 개의 or 또는 and 문을 필요로하는 조건들을 단순화할 수 있는지 보여주는 몇 가지 예시들입니다.

마지막으로, 모든 프로그래머의 영원한 즐거움(만약 당신의 것이 아니라면, 그럼 제안을 함께 배워보세요😉).

# 10. 더 짧은 구문을 위한 Comprehensions 사용

Comprehension은 파이썬이 제공하는 강력한 도구 세트로, 모든 iterable 데이터 유형에 대해 제공됩니다. 이는 상황에 따라 여러 줄의 루프를 사용하는 것을 피할 수 있는 간결한 방법을 제공합니다.

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

한 가지씩 탐색해 봅시다:

## 10(A). 리스트 컴프리헨션

여기서, 중첩된 if 문을 사용하여 리스트 컴프리헨션의 힘을 보여드리겠습니다.

```js
# 리스트 컴프리헨션을 사용한 중첩 if
fruits: list[str] = ["apple", "orange", "avacado", "kiwi", "banana"]
basket: list[str] = ["apple", "avacado", "apricot", "kiwi"]

[i for i in fruits if i in basket if i.startswith("a")] # ['apple', 'avacado'] 
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

비슷하게, 중첩된 for 루프를 적용하여 이해하기 어려운 코드가 될 수 있습니다.

## 10(B). 튜플 컴프리헨션

파이썬에는 튜플 컴프리헨션이 없습니다. 대신, 제너레이터 표현식을 사용하여 튜플을 만들 수 있습니다.

```js
# 제너레이터 표현식을 튜플로 변환
tuple(i**2 for i in range(10))

# (0, 1, 4, 9, 16, 25, 36, 49, 64, 81)
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

노트: 우리가 알다시피 위에서 생성기 표현식은 리스트 내포보다 더 메모리를 효율적으로 사용합니다.

## 10(C). 사전 내포식

만약 당신이 `apple_names`라는 리스트가 있다고 가정하고, 이 리스트에 있는 각 요소의 길이를 포함하는 새 리스트를 출력하고 싶다면,

여기서도 리스트 내포를 사용할 수 있습니다. 그러나 사실 이 표기법을 사용하여 사전을 만들 수도 있는지 알고 계시나요? 네, 그것이바로, 사전 내포입니다.

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

```js
# 사과 이름 목록 만들기
apple_names: list[str] = ["apple", "pineapple", "green apple"]

# 사과 이름을 키로하고 그길이를 값으로하는 사전 만들기
print({apple_name: len(apple_name) for apple_name in apple_names})

# {"apple": 5, "pineapple": 9, "green apple": 11} 
```

대신에 루프나 사전 생성자를 사용하는 것보다 더 읽기 쉽습니다.

## 10(D). 집합 구조체 컴프리헨션

특정 조건에 따라 필터링을 사용하여 컴프리헨션을 만들 수 있습니다.

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


# 조건으로 집합 생성
print({i**2 for i in range(1, 11) if i > 5})

# {64, 36, 100, 49, 81}


참고: 컴프리헨션은 표현력이 풍부하지만, 모든 경우에 적합한 것은 아닙니다. 특히 너무 복잡한 논리를 포함하는 경우에는 그렇습니다.

# 결론

기억하세요, 코드를 컴퓨터가 아닌 함께 일하는 팀을 위해 작성합니다. 그러므로 다른 사람이 여러분의 프로덕션 수준의 코드를 이해할 수 있도록 더 나은 코드를 작성하는 것이 정말 중요합니다.


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

이러한 팁을 채택함으로써 효율적인 코드를 작성할 뿐만 아니라 생산성도 향상시킬 수 있어요.

다음에 또 방문하기 전에... 궁금한 점/제안 사항/생각이 있으면 아래에 남겨주세요. 🖋️

그리고, 이 내용을 즐겁게 보셨다면 박수 50번 👏 하고, 앞으로의 업데이트를 위해 팔로우하시는 거 잊지 마세요.

이만큼이에요. 곧 다시 이야기할게요! 🙋🏻‍♀️

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

테이블 태그를 마크다운 형식으로 변경해보세요.