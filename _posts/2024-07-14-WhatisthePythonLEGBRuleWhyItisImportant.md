---
title: "Python LEGB 규칙이란 왜 중요한가"
description: ""
coverImage: "/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_0.png"
date: 2024-07-14 01:50
ogImage: 
  url: /assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_0.png
tag: Tech
originalTitle: "What is the Python LEGB Rule? Why It is Important?"
link: "https://medium.com/towards-data-science/what-is-the-python-legb-rule-why-it-is-important-1fdcfecfd62f"
---


다른 프로그래밍 언어들과 마찬가지로, 파이썬에도 변수와 함수가 정의된 다양한 스코프가 있습니다. 물론, 이러한 스코프 간의 동작을 결정하는 몇 가지 규칙과 계층 구조가 존재합니다. 예를 들어, 함수 내에서 정의된 변수를 외부에서 접근할 수 없습니다.

내 생각에 파이썬은 객체의 스코프를 명확하고 직관적으로 정의하고 있습니다. 이는 학습과 사용이 쉬운 특성에 기여합니다. 그러나 파이썬 개발자들이 초보자에서 전문가로 성장하고 싶다면 LEGB 규칙을 우회할 수 없습니다.

이 글에서는 파이썬에서 다른 네임스페이스의 논리와 동작을 설명하는 LEGB 규칙을 소개할 것입니다. 그리고 그에 따른 몇 가지 팁과 모범 사례를 제시할 것입니다. 따라서 이 글은 개념을 설명하는 것뿐만 아니라 규칙에서 유도된 설계 시 고려해야 할 점과 트릭을 제안합니다. 아직 LEGB 규칙에 대해 확신이 없다면 놓치지 마세요!

# 1. 파이썬의 LEGB 규칙이란?

<div class="content-ad"></div>

![What is the Python LEGB Rule and Why It is Important](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_0.png) 

Before diving into any demonstrations, let's first get a clear understanding of what L (Local), E (Enclosing), G (Global), and B (Built-in) mean in Python. These are the four scopes of objects in Python that have crucial roles during compiling and runtime. In this segment, I'll give a simple example for each term.

Feel free to skip this part if you're already familiar with them.

## 1.1 Local Scope

<div class="content-ad"></div>

파이썬 초심자이고 "로컬 스코프(local scope)" 개념에 대해 들어본 적이 없다면 걱정하지 마세요. 확실히 로컬 스코프에서 변수를 사용해 본 적이 있을 거에요. 그것이 바로 "로컬 변수(local variable)"입니다. 매우 전형적으로, 함수 내부의 변수는 로컬 변수입니다.

```python
def func():
    local_var = 10 # 이것은 로컬 변수입니다
    print(local_var)
```

위 코드에서 local_var은 로컬 변수입니다. 그 중요한 기준 중 하나는 함수 외부에서 접근할 수 없다는 것입니다.

![Click here to view the image](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_1.png)

<div class="content-ad"></div>

현재 지역 범위에 어떤 객체가 있는지 확인하는 꿀팁이 있어요. 하지만 이건 해당 범위 안에서만 확인할 수 있어요 (예: 함수 내부). 그것은 locals() 함수를 실행하는 것이죠.

```python
def func():
    local_var = 10
    print("Local Scope:", locals()) # Print all the objects in local scope

func()
```

![Image](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_2.png)

## 1.2 Enclosing Scope

<div class="content-ad"></div>

**내부/외부 함수와 파이썬 LEGB 규칙**

안녕하세요! 오늘은 파이썬 함수에서 중요한 개념 중 하나인 LEGB 규칙에 대해 이야기해 볼게요.

LEGB는 Local, Enclosing, Global, Built-in의 약자로, 이 순서대로 파이썬에서 변수를 찾는 규칙을 말해요. 다른 함수 내부에 있는 함수에서 외부 함수의 변수에 접근할 때 사용되는 **Enclosing scope(폐쇄 범위)** 가 정말 특별하답니다.

위의 코드 예시를 한 번 살펴볼까요? inner_func() 함수가 outer_func() 안에 정의되어 있어요. outer_func() 안에 outer_var이라는 변수가 정의되어 있죠. 물론 outer_func() 함수 바깥에서는 이 변수에 접근할 수 없어요. 하지만 inner_func() 함수 안에서는 여전히 outer_var 변수에 접근할 수 있답니다. 이러한 특징이 **Enclosing scope**의 매력이에요.

마치 마법처럼, nested 함수 안에서만 특별한 범위에 접근할 수 있다니, 파이썬 함수의 아름다운 세계를 더 깊게 탐험해 볼 수 있을 거에요! 이 규칙을 잘 활용하여 더욱 효율적이고 강력한 코드를 작성해 보세요. 🌟✨

Markdown 형식으로 이미지를 첨부했습니다. 이미지를 확인해 보세요!

![LEGB Rule](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_3.png)

<div class="content-ad"></div>

이러한 저수준 함수들은 보통 흔하지 않아요. 그래서 이 글의 후반부에서 이 주제에 집중할 섹션이 있을 거예요.

## 1.3 전역 범위

파이썬을 사용해본 적이 있다면 전역 범위에 대한 개념을 이미 알고 있을 거예요. 들여쓰기 없이 무언가를 정의하면, 이 모든 것은 전역 범위에 있답니다.

```python
global_var = 30

def func():
    print(global_var)
```

<div class="content-ad"></div>

**1.4 내장 범위**

내장 범위에는 Python의 기본 제공 객체가 포함되어 있습니다. 이러한 객체들은 정의할 필요가 없는데, 이미 Python에 내장되어 있기 때문입니다. 예를 들어, len() 함수를 사용하여 Python 어디서든 문자열의 길이를 얻을 수 있습니다.

```python
print(len('abc'))
```

<div class="content-ad"></div>

파이썬의 많은 내장 함수들은 사실 널리 알려지지 않고 사용되지 않는데, 그중 일부는 꽤 유용합니다. 관심이 있다면 아래의 기사를 확인해보세요.

## 2. 이름 해결 순서

![What is the Python LEGB Rule and Why It is Important](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_4.png)

LEGB 네임스페이스에 대해 이해한 후에 알아야 할 가장 중요한 규칙은 이름 해결 순서입니다. 즉, 파이썬이 변수 이름을 만나면 먼저 지역 범위를 찾은 다음, 감싸는 범위, 전역 범위, 마지막으로 내장 범위를 찾습니다.

<div class="content-ad"></div>

## LEGB 이름 해석 순서를 시연해보겠습니다.

아래 코드를 사용하여 이 LEGB 규칙을 보여줄 수 있습니다.

```js
a = "Global"

def outer():
    a = "Enclosing"
    
    def inner():
        a = "Local"
        print("a - inner:", a)
    
    inner()
    print("a - outer:", a)

outer()
print("a - global:", a)
```

![LEGB rule](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_5.png)

<div class="content-ad"></div>

inner() 함수 안에서 변수 a를 출력할 때, Python은 먼저 지역 스코프를 확인합니다. 그리고 지역 스코프에 a 변수가 있기 때문에, 다른 스코프에서의 a 변수가 있더라도 우리는 지역 버전을 얻게 됩니다.

이후 outer() 함수 안에서는 inner() 함수의 지역 스코프에 접근할 수 없습니다. 따라서 변수 a를 출력할 때 Python은 감싸고 있는 스코프에서 가져옵니다.

outer 함수를 호출한 후, 변수 a를 다시 출력하고 싶을 때, 전역 스코프에는 하나만 있습니다. 따라서 이번에는 전역 스코프에서 변수 a를 가져올 수 있습니다.

## 내장 스코프가 마지막 순서에 있는 것을 어떻게 증명할 수 있을까요?

<div class="content-ad"></div>

위의 예제에서는 내장 범위가 없었습니다. 이를 수행하기 위해 또 다른 코드 예제를 작성할 수 있습니다.

```python
def test_print():
    print = "내장 함수를 가림"
    try:
        print("이제 'print'가 함수가 아닌 문자열이기 때문에 오류가 발생합니다.")
    except TypeError as e:
        print = globals()['__builtins__'].print
        print("오류 발생:", e)

test_print()
```

이 test_print() 함수에서 print라는 변수를 정의하고 문자열을 할당했습니다. 그런 다음 print() 함수를 호출하면 이제 print가 문자열이므로 함수가 아니기 때문에 예외가 발생합니다.

<div class="content-ad"></div>

만약 예외 블록에서 print() 함수를 사용할 수 있는 이유가 궁금하시다구요? globals()[`__builtins__`] 리스트에서 내장 함수 print()을 명시적으로 가져와서 덮어쓴 적이 있기 때문이랍니다. 그래서 내장 print() 함수는 항상 존재하지만, 우리가 로컬 변수를 정의해서 그걸 먼저 찾고 사용하기 때문에 그렇지요.

# 3. 성능 및 메모리 사용에 미치는 영향

![What is the Python LEGB Rule? Why It is Important](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_7.png)

LEGB 규칙의 순서를 이해하면 성능 고려 사항을 이해하기 쉬워집니다. 즉, 가능한 경우 지역 변수 대신 전역 변수를 사용하는 것이 좋습니다. 이렇게 하면 프로그램을 빠르게 만드는 데 도움이 될 거예요.

<div class="content-ad"></div>

예를 들어, 아래 코드에서는 변수에 대해 일부 계산을 수행하는 함수가 정의되어 있습니다. 첫 번째 버전은 전역 변수를 사용하고, 두 번째 버전은 지역 변수를 사용합니다.

```js
# Version 1 - Use Global Variable
sum_global = 0

def sum_numbers_global():
    global sum_global
    for number in range(1000000):
        sum_global += number


# Version 1 - Use Local Variable
def sum_numbers_local():
    local_sum = 0
    for number in range(1000000):
        local_sum += number
    return local_sum
```

성능을 테스트해봅시다.

![Performance Test](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_8.png)

<div class="content-ad"></div>

지역 변수를 사용하면 성능이 약 30% 향상된다는 것을 보여줍니다. 그러나 위 예제에서는 함수 내에서 전역 변수에 명시적으로 액세스하기 위해 전역 키워드를 사용해야 합니다. 이는 보통의 사용 사례에서는 매우 흔하지는 않습니다. 이제 좀 더 일반적인 사례를 살펴보겠습니다.

리스트 내포를 들어 보거나 많이 사용한 적이 있을 것입니다. 그것을 사용하는 이유는 좀 더 "파이썬스러운" 것이기 때문일까요, 아니면 가독성이 더 좋기 때문일까요? 리스트 내포의 성능이 해당하는 for 루프보다 우수하다는 것을 알고 계십니까?

예를 들어, 우리는 몇 가지 계산된 결과를 목록에 추가하기 위해 매우 간단한 for 루프를 작성할 수 있습니다.

```python
factor = 2
results = []

for i in range(100):
    results.append(i * factor)
```

<div class="content-ad"></div>

단순한 for-loop은 쉽게 리스트 컴프리헨션으로 다시 작성할 수 있어요. 아래와 같이 수정해보세요.

```js
factor = 2
results = [i * factor for i in range(100)]
```

성능을 비교해보면, 매우 다를 수 있어요. 

![image](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_9.png)

<div class="content-ad"></div>

리스트 컴프리헨션의 성능이 더 좋은 이유 중 하나는 리스트 컴프리헨션이 변수 i를 로컬 변수로 컴파일하기 때문이라고 합니다. 한편 for 루프 내의 변수 i는 전역 변수로 작용한다는 차이가 있습니다.

자세한 설명이 궁금하다면 "For 루프 대 List 컴프리헨션"에 대한 이 기사를 확인해보세요.

# 4. 디자인 제약 사항과 모범 사례

![Image](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_10.png)

<div class="content-ad"></div>

이제, 이미 알고 있는 LEGB 규칙을 바탕으로 디자인 영향과 최적의 실천 방법을 살펴보겠습니다.

## 전역 네임스페이스 사용 최소화

물론 전역 네임스페이스를 사용하지 않을 수는 없습니다. 그러나 LEGB 규칙을 알면 전역 네임스페이스 사용을 최소화할 수 있습니다. 이를 통해 더 나은 성능과 스레드 안정성을 얻을 수 있습니다.

어플리케이션에서 아이템 목록을 필요로 한다고 가정해보겠습니다. 몇 가지 로직을 함수에 캡슐화해야 합니다. 간단하게 설명하면 목록에 아이템을 추가하는 함수와 모든 아이템을 지우는 함수가 필요합니다.

<div class="content-ad"></div>

```python
items = []

def add_item(item):
    items.append(item)

def clear_items():
    global items
    items.clear()

add_item("apple")
add_item("banana")
print(items)

clear_items()
print(items)
```

![Python LEGB Rule](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_11.png)

The code snippet above is error-free and will run smoothly. However, when incorporated into a larger program, there is an increased risk of introducing bugs and compromising the overall maintainability.

For instance, unintended modifications may occur if the items list is accessed elsewhere. Additionally, if these functions are utilized in multiple locations, concurrency issues may arise.

<div class="content-ad"></div>

그래서, 아래와 같이 클래스를 정의하는 것을 권장합니다. 그러면 모든 위험 요소가 완화됩니다.

```js
class ItemManager:
    def __init__(self):
        self.items = []

    def add_item(self, item):
        self.items.append(item)

    def clear_items(self):
        self.items = []

# Test
manager = ItemManager()
manager.add_item("사과")
manager.add_item("바나나")
print(manager.items)

manager.clear_items()
print(manager.items)
```

![이미지](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_12.png)

## 언제 주변 네임스페이스를 사용해야 할까요?

<div class="content-ad"></div>

LEGB 규칙 중에서, enclosing namespace은 가장 일반적이지 않을 수도 있습니다. 실제로, 이것은 전역 변수의 사용을 더욱 최소화하는 데 도움이 될 수 있는데요.

예를 들어, 함수가 호출된 횟수를 세는 "카운터"를 정의하고 싶다고 가정해보죠. 이 경우, 로컬 변수만으로는 이 요구사항을 해결할 수 없습니다. enclosing namespace에 대한 지식이 없다면, 다음과 같이 전역 변수를 사용해야 할 수 있습니다.


call_count = 0

def function_to_track():
    global call_count
    call_count += 1
    # 함수 로직


이 방법으로도 문제를 해결할 수 있습니다. 하지만, 앞서 언급한 것처럼, 다음과 같이 enclosing 변수를 사용하여 이 문제를 해결할 수도 있습니다.

<div class="content-ad"></div>

우리는 바깥쪽 함수 call_counter(func)를 정의했습니다. 이 함수는 카운트 변수에 1을 추가하는 내부 함수를 정의하고 전달된 함수를 반환합니다. 따라서 내부 함수는 전달된 함수에서 아무것도 변경하지 않고 카운트를 추가하는 역할을 합니다. 그런 다음, 상위 네임스페이스에 카운터를 정의하고 0으로 초기화합니다.

이 내포된 함수를 어떻게 사용할 수 있을까요? 파이썬에서는 쉽게 "장식"으로 사용할 수 있습니다. 즉, @call_counter를 사용하는 것입니다. function_to_track()은 func에 인자로 전달될 겁니다. 아래는 실행 후의 출력입니다.

!(/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_13.png)

<div class="content-ad"></div>

## 내장 객체를 가리지 마세요

마지막 팁은 간단합니다. **절대** 내장 함수를 가리지 마세요.

때로는 우리가 좋아하는 이름으로 무언가를 호출하고 싶을 때가 있습니다. 파이썬에는 예약된 용어가 많지 않지만, 즉 내장 함수나 다른 객체를 덮어쓰는 것이 허용된다는 것을 의미합니다. 그러나 혼란과 버그의 위험을 피하기 위해 **반드시** 그렇게 하지 않는 것을 권합니다.

![image](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_14.png)

<div class="content-ad"></div>

# Summary

![image](/assets/img/2024-07-14-WhatisthePythonLEGBRuleWhyItisImportant_15.png)

알겠습니다. 그렇기 때문에 이름 해결 순서인 LEGB 규칙을 이해하는 것이 왜 중요한지 이해하셨을 것입니다. 제가 소개한 개념들이 이해되고 제안들이 유용했으면 좋겠습니다.

이 규칙을 철저히 이해하는 것은 우리의 전체 성능을 향상시키고 일부 비효율적인 설계를 피하는 데 도움을 줄 뿐만 아니라, 보다 견고한 애플리케이션을 개발하기 위해 일부 함정을 피할 수 있습니다.