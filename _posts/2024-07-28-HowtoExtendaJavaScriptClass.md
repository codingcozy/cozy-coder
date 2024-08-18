---
title: "자바스크립트 클래스 extend 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-28-HowtoExtendaJavaScriptClass_0.png"
date: 2024-07-28 14:06
ogImage:
  url: /assets/img/2024-07-28-HowtoExtendaJavaScriptClass_0.png
tag: Tech
originalTitle: "How to Extend a JavaScript Class"
link: "https://medium.com/@hohanga/how-to-extend-a-javascript-class-dc4aa14434c0"
isUpdated: true
---

가끔은 JavaScript 클래스를 확장하고 싶을 때가 있습니다.

이 기사에서는 JavaScript 클래스를 확장하는 방법을 살펴보겠습니다.

## JavaScript 클래스 확장하기

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

JavaScript 클래스를 확장하려면 extends 키워드와 클래스 구문을 사용할 수 있습니다.

예를 들어, 다음과 같이 작성할 수 있습니다:

```js
class Person {
  sayHello() {
    console.log("hello");
  }
  walk() {
    console.log("I am walking!");
  }
}
class Student extends Person {
  sayGoodBye() {
    console.log("goodbye");
  }
  sayHello() {
    console.log("Hi, I am a student");
  }
}
const student1 = new Student();
student1.sayHello();
student1.walk();
student1.sayGoodBye();
console.log(student1 instanceof Person);
console.log(student1 instanceof Student);
```

Person 클래스는 Student 클래스의 기본 클래스 역할을 합니다.

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

그럼 sayHello 및 walk 메서드가 있습니다.

그리고 Person 클래스의 내용을 상속하는 Student 클래스가 있습니다.

Student 클래스는 sayGoodBye 및 sayHello와 같은 고유한 메서드도 있습니다.

확장 키워드는 Student가 기본 클래스로 Person을 가지고 있음을 나타내므로 그것도...
