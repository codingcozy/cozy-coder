---
title: "코틀린 호출 참조 연산자 이해하기 사용법 및 예시"
description: ""
coverImage: "/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_0.png"
date: 2024-08-03 19:55
ogImage: 
  url: /assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_0.png
tag: Tech
originalTitle: "Understanding the  Operator in Kotlin The Callable Reference Operator"
link: "https://medium.com/@krishusharma292/understanding-the-operator-in-kotlin-the-callable-reference-operator-057ae42b4a6b"
---


안녕하세요 Kotlin 팬 여러분! 👋 오늘은 호출 가능한 참조 연산자로 알려진 ::이라는 흥미로운 연산자에 대해 소개합니다. 이 연산자는 함수, 속성 및 생성자를 코틀린에서 간결하고 가독성 있게 참조할 수 있게 해줍니다. 하지만 :: 연산자가 정확히 무엇일까요? 간단히 말하면, 이미 존재하는 코드 요소에 대한 참조를 만들어주어 고차 함수 및 다른 문맥에서 더 쉽게 작업할 수 있게 해줍니다. 호출 가능한 참조는 특히 고차 함수에서 유용하며 코드를 더 표현력 있고 유지보수 가능하게 만듭니다. 이 글에서는 코틀린에서 호출 가능한 참조의 다양한 유형과 사용법에 대해 살펴보며 이 강력한 기능에 대한 포괄적인 안내서를 제공할 것입니다. 🚀

# 📌함수 참조

:: 연산자의 주요 사용 사례 중 하나는 함수에 대한 참조를 만드는 것입니다. 이를 통해 함수를 매개변수로 전달하거나 다른 함수에서 반환하거나 변수에 저장할 수 있습니다. 이 기능은 특히 함수가 1급 시민으로 여겨지는 함수형 프로그래밍 패러다임에서 유용합니다.

# ✨예제

<div class="content-ad"></div>

다음 예시를 살펴보겠습니다. 여기서는 숫자가 짝수인지 확인하는 함수를 정의합니다:

![image](/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_0.png)

이 예시에서 ::isEven은 isEven 함수에 대한 참조를 만들어서 filter 함수로 전달합니다. 이를 통해 코드가 더 읽기 쉽고 간결해집니다.

# 💡실용적인 사용 사례

<div class="content-ad"></div>

함수 참조를 사용하면 고차 함수를 포함하는 코드를 간편하게 만들 수 있습니다. 예를 들어, 컬렉션과 작업할 때:

![image](/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_1.png)

여기서 String::length은 리스트의 각 요소에 적용되는 String 클래스의 length 속성에 대한 참조입니다.

# 🔖속성 참조

<div class="content-ad"></div>

`::` 연산자는 속성에 대한 참조를 만드는 데에도 사용될 수 있습니다. 이는 속성에 대한 참조가 예상되는 함수에 속성을 전달해야 할 때 유용합니다.

# ✨예시

<img src="/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_2.png" />

이 예시에서 Person::name은 Person 클래스의 name 속성에 대한 참조를 만듭니다. 이를 통해 Person 객체 목록에서 이름을 깔끔하고 읽기 쉽게 추출할 수 있습니다.

<div class="content-ad"></div>

# 💡 실용적인 사용 사례

속성 참조는 데이터 변환을 다룰 때 특히 유용할 수 있습니다:

![image](/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_3.png)

여기서 Person::age는 사람 객체를 나이에 따라 비교하고 정렬하는 데 사용됩니다.

<div class="content-ad"></div>

# 🏗️ 생성자 참조

호출 가능한 참조를 사용하여 생성자를 참조할 수도 있습니다. 이를 통해 생성자를 고차 함수에 인수로 전달하거나 함수 참조가 필요한 다른 상황에서 사용할 수 있습니다.

# ✨예시

<img src="/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_4.png" />

<div class="content-ad"></div>

# 💡 실제 활용 예시

생성자 참조는 팩토리 패턴이나 인스턴스를 동적으로 생성할 때 사용될 수 있습니다:

![Constructor References Example](/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_5.png)

이는 생성자 참조가 코드의 유연성을 향상시킬 수 있는 방법을 보여줍니다.

<div class="content-ad"></div>

# 🔄Bound and Unbound References

함수 참조는 바운드 또는 언바운드가 될 수 있습니다. 바운드 참조는 특정 인스턴스를 가리키는 반면, 언바운드 참조는 그렇지 않습니다. 이 차이는 참조가 다른 맥락에서 어떻게 사용될 수 있는지 이해하는 데 중요합니다.

# ✨예제

![예제 이미지](/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_6.png)

<div class="content-ad"></div>

이 경우 alice::name은 alice 인스턴스의 name 속성에 대한 바운드 참조이며, Person::age는 어떤 Person 인스턴스의 age 속성에 대한 언바운드 참조입니다.

# 💡실용적인 사용 사례

바운드 참조와 언바운드 참조는 이벤트 처리나 콜백 시나리오에서 특히 유용할 수 있습니다:

![UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_7](/assets/img/2024-08-03-UnderstandingtheOperatorinKotlinTheCallableReferenceOperator_7.png)

<div class="content-ad"></div>

이 방식은 코드베이스에서 유연성과 재사용성을 향상시킵니다.

# 📚 결론

Kotlin의 :: 연산자는 호출 가능한 참조를 생성하여 함수, 속성, 생성자의 사용을 간소화하는 강력한 기능입니다. 이 연산자는 특히 고차 함수를 다룰 때 코드 가독성과 유연성을 향상시킵니다. 호출 가능한 참조를 이해하고 활용함으로써, 개발자는 보다 간결하고 유지보수가 쉬운 Kotlin 코드를 작성할 수 있습니다. 컬렉션을 조작하거나 이벤트를 처리하거나 동적으로 객체를 생성하는 경우에도 호출 가능한 참조를 통해 Kotlin 개발 프로세스를 크게 간소화할 수 있습니다. 🚀

호출 가능한 참조를 자세히 살펴보면 Kotlin 코드에서 효율성과 표현력의 새로운 수준을 개척할 수 있습니다.