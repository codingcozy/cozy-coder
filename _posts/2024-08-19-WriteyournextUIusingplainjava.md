---
title: "자바로 다음 UI 만들기 초보자를 위한 단계별 안내"
description: ""
coverImage: "/assets/img/2024-08-19-WriteyournextUIusingplainjava_0.png"
date: 2024-08-19 02:37
ogImage: 
  url: /assets/img/2024-08-19-WriteyournextUIusingplainjava_0.png
tag: Tech
originalTitle: "Write your next UI using plain java"
link: "https://medium.com/@miguelperezcolom/write-your-next-ui-using-plain-java-3c1fd981ed29"
isUpdated: false
---


저는 개발자로써 항상 최소한의 노력과 최소한의 코드 라인을 사용하여 애플리케이션을 개발하고 싶어했습니다. 알다시피, 실패하지 않는 코드는 쓰지 않은 코드이며, 적을수록 좋아요...

이 글에서는 순수 자바를 사용하여 사용자 인터페이스(UI)를 쉽게 구축하는 방법을 소개하고 싶습니다.

# Mateu에 대해

Mateu는 순수 자바를 사용하여 웹 UI를 생성하기 위한 오픈 소스 라이브러리입니다. Mateu를 통해 핵심 로직에 집중할 수 있도록 최소한의 코드만 작성하는 것이 원칙이며, 이에는 많은 장점이 있습니다(예: 비즈니스 로직에 집중할 수 있는 기회가 생깁니다).

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

또한 Mateu는 완전히 마이크로 프론트엔드 및 마이크로 서비스 친화적이므로 분산 UI를 구축하고 UI 코드를 비즈니스 로직이 있는 곳인 동일한 위치 (마이크로 서비스)에 작성할 수 있습니다. 이는 프론트엔드를 지원하기 위해 비용이 많이 드는 API를 구축하지 않아도 되며 각 팀이 자체 e2e 플로우를 관리할 수 있게 해줍니다.

또한, Mateu UI는 상태를 유지하지 않으므로 서버 측 세션에 데이터를 저장할 필요가 없습니다. 이는 트래픽이 여러 리플리카로 균형을 이루는 마이크로 서비스 환경에 특히 적합합니다.

또한, Mateu로 구축된 UI 컴포넌트를 어떤 웹 사이트에도 쉽게 포함시킬 수 있습니다. 워드프레스, 정적 html 사이트 또는 React, Vue 또는 Angular로 만든 Jamstack 애플리케이션인지 여부에 상관없이 가능합니다.

# 프로젝트 만들기

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

오늘 기준으로 Mateu는 스프링 부트 Webflux만 지원합니다. 스프링 부트 이니셜 라이저를 사용하거나 IntelliJ Idea 내에서 프로젝트를 쉽게 만들 수 있습니다.

프로젝트를 만든 후에는 Mateu의 의존성을 추가해야 합니다. Mateu는 백엔드와 프론트엔드를 모두 포함하는 매우 유용한 아티팩트를 제공합니다.

```js
<dependency>
    <groupId>io.mateu</groupId>
    <artifactId>embedded-front</artifactId>
    <version>3.0-alpha.26</version>
</dependency>
```

Mateu의 의존성을 추가한 후에는 @MateuUI 주석을 사용하여 간단한 자바 클래스를 만들면 UI를 만들 수 있습니다:

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

```java
package com.example.demo;

import io.mateu.core.domain.uidefinition.shared.annotations.MateuUI;

@MateuUI("")
public class HelloWorld {

}
```

Spring Boot 앱을 시작하면 http://localhost:8080으로 이동하여 다음 결과를 얻을 수 있습니다:

![UI](/assets/img/2024-08-19-WriteyournextUIusingplainjava_0.png)

쉽죠?

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

# 폼 만들기

Mateu와 함께 양식을 만들기 쉽습니다. Java 클래스를 생성하고 필드를 추가하기만 하면 됩니다. 해당 필드에 getter를 추가하면 읽기 전용 필드가 되며 setter를 추가하면 편집 가능한 필드가 됩니다. @Action, @MainAction 또는 @Button으로 주석이 달린 메서드를 위한 버튼이 생성됩니다.

예를 들어, 이메일을 보내는 양식을 다음과 같이 만들 수 있습니다:

```java
package com.example.demo;

import io.mateu.core.domain.uidefinition.shared.annotations.MainAction;
import io.mateu.core.domain.uidefinition.shared.annotations.MateuUI;
import io.mateu.core.domain.uidefinition.shared.annotations.TextArea;
import lombok.Getter;
import lombok.Setter;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Service;

@MateuUI("/form")             // 이 URL을 설정합니다
@Service@Scope("prototype")   // 모든 사용자가 동일한 양식을 공유하고 싶지 않다면 프로토타입 범위를 사용하세요
@Getter@Setter                // 양식에서 필드를 보이게 하려면 필요합니다
public class SimpleForm {

    String email;

    String subject;

    @TextArea
    String body;

    @MainAction
    public void send() {
        // 이메일 보내기
    }
    
}
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

이제 Spring Boot 애플리케이션을 시작하고 http://localhost:8080/form으로 이동하면 다음과 같은 것을 볼 수 있습니다:

![이미지](/assets/img/2024-08-19-WriteyournextUIusingplainjava_1.png)

값을 입력하고 Send를 클릭할 수 있습니다. 그렇게 하면 서버 측에 SimpleForm 객체가 생성되고 브라우저에서 입력한 값으로 채워집니다. 그런 다음 send() 메서드가 호출됩니다.

메서드의 반환 유형에 따라 다음 단계가 결정됩니다. void 메서드는 동일한 뷰에 머무르게 하고 자바 객체를 반환하면 그 객체를 사용하여 브라우저에 표시될 다음 뷰가 만들어집니다. Mateu에서 지원하는 다양한 UX 패턴이 있습니다. 이를 살펴보려면 https://demo.mateu.io/를 방문해 보세요.

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

# CRUD(CREATE, READ, UPDATE, DELETE) 생성

양식 외에도, 내 앱에서 사용하는 다른 큰 구성요소는 CRUD(CREATE, READ, UPDATE, DELETE)입니다. Mateu와 함께 CRUD를 생성하는 것은 Crud 인터페이스를 구현하는 자바 클래스를 만드는 것만큼 간단합니다. 다음과 같은 코드 예시입니다:

```js
package com.example.demo;

import io.mateu.core.domain.uidefinition.core.interfaces.Crud;
import io.mateu.core.domain.uidefinition.shared.annotations.MateuUI;
import io.mateu.core.domain.uidefinition.core.interfaces.SortCriteria;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

import java.util.List;

@MateuUI("/crud")
@Component@Scope("prototype")
public class SimpleCrud implements Crud<SearchForm, Row> {

    @Autowired
    SimpleCrudService service;

    @Override
    public Flux<Row> fetchRows(SearchForm filters, List<SortCriteria> sortOrders,
                               int offset, int limit) throws Throwable {
        return service.fetchRows(filters, sortOrders, offset, limit);
    }

    @Override
    public Mono<Long> fetchCount(SearchForm filters) throws Throwable {
        return service.fetchCount(filters);
    }

}
```

SimpleCrudService 클래스는 JPA 리포지토리에 액세스하거나 REST HTTP 요청을 보낼 수 있습니다. 여러분의 선택에 달려있습니다.

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

두 가지 추가 클래스가 관련되어 있음을 알 수 있습니다: SearchForm과 Row입니다. SearchForm은 검색 양식을 만들 때 사용되며, 다음과 같은 형태를 가질 수 있습니다:

```js
package com.example.demo;

import lombok.Data;

@Data
public class SearchForm {

    String text;

}
```

한편, Row는 (당연하게도) 결과 열에 사용됩니다:

```js
package com.example.demo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data@AllArgsConstructor@NoArgsConstructor
public class Row {

    String id;

    String name;

    int age;

}
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

이 세 가지 클래스를 사용하면 다음과 같은 결과가 나타납니다:

![UI 예시](/assets/img/2024-08-19-WriteyournextUIusingplainjava_2.png)

기존 레코드를 편집하거나 레코드를 삭제하거나 새로 생성하려면 CRUD에서 일부 메소드를 재정의하면 됩니다:

```java
@Override
public void delete(List<Row> selection) throws Throwable {
  // 선택한 행 삭제
}

@Override
public NewRecordForm getNewRecordForm() throws Throwable {
  // 새 레코드를 생성하는 양식의 인스턴스 반환
}

@Override
public DetailForm getDetail(Row row) throws Throwable {
  // 새 레코드를 생성하는 양식의 인스턴스 반환
}
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

위의 방법을 재정의함으로써 Mateu는 사용자가 레코드를 삭제, 추가, 편집할 수 있도록 가능하게 됩니다. 이제 버튼들이 CRUD에서 볼 수 있게 됩니다.

전체 예시는 https://github.com/miguelperezcolom/mateu/blob/master/demo/src/main/java/com/example/demo/domain/programmingLanguages/ProgrammingLanguages.java에서 확인할 수 있습니다.

# 메뉴로 모두 통합하기

지금까지 서로 다른 경로에 폼과 CRUD를 만들었으며, 이제 이들을 모두 통합하고 사용자가 열고자 하는 뷰를 선택할 수 있는 메뉴를 추가하고 싶습니다. 이를 위해 우리는 UI 클래스에 몇 가지 필드를 추가하고 @MenuOption으로 주석을 달면 됩니다:

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

```java
package com.example.demo;

import io.mateu.core.domain.uidefinition.core.interfaces.HasAppTitle;
import io.mateu.core.domain.uidefinition.shared.annotations.MateuUI;
import io.mateu.core.domain.uidefinition.shared.annotations.MenuOption;

@MateuUI("")
public class HelloWorld implements HasAppTitle {

    @MenuOption
    SimpleForm sendEmail;

    @MenuOption
    SimpleCrud crud;

    @Override
    public String getAppTitle() { // we want to set the title of our app
        return "My app";
    }
}
```

이렇게 하면 상단에 툴바가 있는 뷰가 생성되고 2개의 메뉴 옵션이 표시됩니다:

![UI 예시](/assets/img/2024-08-19-WriteyournextUIusingplainjava_3.png)

실제로 여러 수준의 메뉴를 만들 수 있습니다. 여기처럼 여러 메뉴 옵션이나 다른 하위 메뉴가 있는 클래스를 만들면 됩니다:


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
package com.example.mateuarticle1.ui;

import com.example.mateuarticle1.ui.crud.SimpleCrud;
import io.mateu.core.domain.uidefinition.shared.annotations.MenuOption;

public class SimpleMenu {

    @MenuOption
    SimpleForm sendEmail;

    @MenuOption
    SimpleCrud crud;

}
```

그리고 여기처럼 @Submenu로 주석이 달린 필드로 UI 클래스에 추가하세요:

```js
@MateuUI("")
public class HelloWorld {

    ...

    @Submenu
    SimpleMenu submenu;

    ...
}
```

이제 툴바에 서브메뉴가 나타납니다.


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

<img src="/assets/img/2024-08-19-WriteyournextUIusingplainjava_4.png" />

# 결론

우리는 Mateu로 할 수 있는 일의 일부만 살짝 소개했지만, 오직 순수 자바만을 사용하여 전체 UI를 구축할 수 있다는 것을 알 수 있었습니다. 전체 데모는 https://demo.mateu.io/에서 확인할 수 있으며 사용자 매뉴얼은 https://github.com/miguelperezcolom/mateu/wiki에서 확인할 수 있습니다.

이 프로젝트를 좋아하신다면 https://github.com/miguelperezcolom/mateu를 방문하고 별을 눌러 평가해주세요 :)

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

![이미지](/assets/img/2024-08-19-WriteyournextUIusingplainjava_5.png)

이 글의 코드는 https://github.com/miguelperezcolom/mateu-article-1 에서 확인할 수 있습니다.