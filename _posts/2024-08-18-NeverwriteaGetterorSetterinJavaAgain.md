---
title: "자바에서 Getter 또는 Setter를 다시 작성하지 마세요"
description: ""
coverImage: "/assets/img/2024-08-18-NeverwriteaGetterorSetterinJavaAgain_0.png"
date: 2024-08-18 11:24
ogImage: 
  url: /assets/img/2024-08-18-NeverwriteaGetterorSetterinJavaAgain_0.png
tag: Tech
originalTitle: "Never write a Getter or Setter in Java Again"
link: "https://medium.com/@abhisheksinghjava/never-write-a-getter-or-setter-in-java-again-a40ce6c3a05f"
isUpdated: false
---


<img src="/assets/img/2024-08-18-NeverwriteaGetterorSetterinJavaAgain_0.png" />

왜 Lombok을 사용해야 하나요?

Lombok은 현재 Java 세계에서 매우 인기 있는 매우 유명한 라이브러리입니다. Lombok의 주요 사용 목적은 Java 코드에서 보일러플레이트 코드를 줄이는 것입니다.

여러 애너테이션을 제공하여 애너테이션에 따라 메서드를 자동으로 생성할 수 있습니다.

<div class="content-ad"></div>

## 1. 가독성 향상:

보일러플레이트 코드의 양을 줄이면, Lombok을 통해 코드가 더 읽기 쉽고 정돈되게 될 수 있습니다.

개발자들은 코드의 중요한 부분에 집중할 수 있어서 이해하고 유지관리하기 쉬워집니다.

## 2. 보일러플레이트 코드 감소:

<div class="content-ad"></div>

롬복 라이브러리는 게터 및 세터 메서드, 생성자, equals(), hashCode(), 그리고 toString() 메서드와 같은 반복적이고 장황한 코드를 제거하는 데 도움이 됩니다.

이러한 보일러플레이트 코드의 감소는 코드를 더 깔끔하고 간결하게 만듭니다.

## 3. 일관된 코드 스타일:

롬복은 일관된 코드 패턴을 제공하여 다른 개발자들이 코드를 더 잘 이해할 수 있도록 돕고, 다양한 보일러플레이트 코드를 제거함으로써 코딩 오류를 줄입니다.

<div class="content-ad"></div>

## 4. 빌더 패턴을 간소화합니다:

롬복은 자바에서 매우 유용한 'Builder' 어노테이션을 제공합니다. 이를 사용하면 더 간결하고 가독성이 좋은 구문으로 객체를 인스턴스화할 수 있습니다.

'Builder' 어노테이션은 많은 선택적 매개변수가 있는 클래스가 있을 때 매우 유용합니다.

## 5. IDE와의 호환성:

<div class="content-ad"></div>

**롬복**은 IntelliJ IDEA, Eclipse, Visual Studio Code 및 기타 대부분의 인기있는 IDE와 통합될 수 있습니다.

많은 IDE가 롬복을 지원하여 생성된 메소드에 대한 코드 완성 및 탐색과 같은 기능을 제공합니다.

## 6. 개발 속도 향상:

롬복은 작성하고 유지해야 하는 다양한 보일러플레이트 코드를 제거하는 데 도움을 줍니다. 따라서 여기서 개발자는 주로 비즈니스 로직 작성에 주력할 수 있습니다.

<div class="content-ad"></div>

개발자들은 반복적인 작업에 시간을 보내는 대신 클래스의 필수 로직을 작성하는 데 집중할 수 있습니다.

## 7. 오픈 소스 커뮤니티 지원:

롬복은 활발한 오픈 소스 커뮤니티를 보유하고 있으며 Java 코딩에서 널리 사용됩니다.
이는 개발자가 라이브러리의 기능을 활용하고 커뮤니티 기반의 개선 사항과 업데이트를 얻을 수 있다는 것을 의미합니다.

Java에서 롬복 주석의 몇 가지 실용적인 예제는 아래와 같습니다.

<div class="content-ad"></div>

## @Getter 및 @Setter 어노테이션-

가장 많이 사용되는 롬복 어노테이션입니다. 이 어노테이션은 클래스 필드에 대한 getter 및 setter 메서드를 생성합니다.

```js
import lombok.Getter;
import lombok.Setter;
@Getter
@Setter
public class Student {
    private String firstName;
    private String middleName;
    private String lastName;
}
```

## @ToString 어노테이션-

<div class="content-ad"></div>

이 주석은 클래스 내용을 출력하는 toString() 메서드를 생성합니다.

```java
import lombok.ToString;
@ToString
public class Student {
    private String firstName;
    private String middleName;
    private String lastName;
}
```

## @Data Annotation-

이 주석은 Getter, Setter, ToString, EqualsAndHashCode 및 RequiredArgumentConstructor 주석을 결합합니다.

<div class="content-ad"></div>

```js
import lombok.Data;

@Data
public class Student {
    private String firstName;
    private String middleName;
    private String lastName;
}
```

## @NonNull Annotation-

This annotation marks the field as non-null and also generates a null check in the constructor.

```js
import lombok.NonNull;

public class Student {
    @NonNull
    private String firstName;
    private String middleName;
    private String lastName;
}
```

<div class="content-ad"></div>

## @Builder Annotation-

이 어노테이션은 클래스의 인스턴스를 생성하기 위한 빌더 패턴을 만듭니다.

```js
import lombok.Builder;

@Builder
public class Student {
    private String firstName;
    private String middleName;
    private String lastName;
}
```

## @EqualsAndHashCode Annotation-

<div class="content-ad"></div>

이 주석은 클래스 필드에 대한 equals() 및 hashCode() 메서드를 생성합니다.

```js
import lombok.EqualsAndHashCode;

@EqualsAndHashCode
public class Student {
    private String firstName;
    private String middleName;
    private String lastName;
}
```

## @NoArgsConstructor 및 @AllArgsConstructor 주석-

이 주석은 각각 매개변수가 없는 생성자와 모든 매개변수를 갖는 생성자를 생성합니다.

<div class="content-ad"></div>

```java
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;

@NoArgsConstructor
@AllArgsConstructor
public class Student {
    private String firstName;
    private String middleName;
    private String lastName;
}
```

럼복을 사용하는 것에 찬성이세요? 아니면 반대이세요? 의견을 남겨주세요.
