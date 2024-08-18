---
title: "파이프라인 디자인 패턴 소프트웨어 개발에서 활용하는 효율적인 방법"
description: ""
coverImage: "/assets/img/2024-08-19-PipelineDesignpattern_0.png"
date: 2024-08-19 02:38
ogImage: 
  url: /assets/img/2024-08-19-PipelineDesignpattern_0.png
tag: Tech
originalTitle: "Pipeline Design pattern"
link: "https://medium.com/@ankithahjpgowda/pipeline-design-pattern-2c16f05f5ad9"
isUpdated: false
---


<img src="/assets/img/2024-08-19-PipelineDesignpattern_0.png" />

파이프라인 디자인 패턴은 서로 다른 단계에서 순차적 데이터 처리를 허용하여 코드의 모듈성과 재사용성을 제공합니다.

우리가 호출해야 할 N개의 함수가 있고, 한 함수의 실행이 다른 함수의 출력에 의존하는 경우를 고려해봅시다. 이 구현을 간소화하기 위해, 각 함수를 일반화하여 입력과 출력이 있는 추상화로 전환하는 파이프라인 디자인 패턴을 사용할 수 있습니다.

장점:

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

- 분리: 각 단계가 느슨하게 연결되어 유지 보수가 쉽습니다.
- 재사용성: 한 단계는 다른 파이프라인의 일부가 될 수 있습니다.
- 확장성: 새로운 단계를 추가할 때 기존 단계에 중요한 변경이 필요하지 않습니다.

![](/assets/img/2024-08-19-PipelineDesignpattern_1.png)

이 문서에서는 특수 문자, 밑줄 및 숫자가 포함된 문자열이 입력으로 주어지는 매우 간단한 파이프라인을 구현해 보겠습니다. 우리는 밑줄, 특수 문자 및 숫자를 차례대로 필터링하는 3가지 메서드를 만들 것입니다. 마지막에는 문자만 있는 문자열을 얻게 됩니다. 이 시나리오에는 훨씬 간단한 접근 방법이 있지만, 이 예제를 통해 이해를 쉽게 하기 위해 유지합니다.

제네릭 입력과 출력을 사용하는 메서드를 가진 인터페이스를 만드세요:

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
package com.example.pipeline_design_pattern_overview.pipeline;

public interface BaseFunction<I,O> {

    public String getFunctionName(); // 해당 기능을 구현하는 함수의 이름을 가져옵니다.
    public O apply(I input); 
}
```

각 단계는 이 인터페이스를 구현하고 apply() 및 getFunctionName() 메서드를 구현합니다. 여기에는 이를 구현하는 3개의 클래스가 있습니다:

```java
public class RemoveSpecialChars implements BaseFunction<String, Object> {

    // apply()를 구현하여 String을 입력으로 받고 String을 출력합니다.
}

public class RemoveUnderScore implements BaseFunction<String, Object> {

    // apply()를 구현하여 String을 입력으로 받고 String을 출력합니다.
}

public class RemoveNumbers implements BaseFunction<String, Object> {

    // apply()를 구현하여 String을 입력으로 받고 char[]을 출력합니다.
}
```

BaseFunction 구현을 가져와 리스트에 저장하는 메서드를 포함하는 파이프라인 생성을위한 구성 요소를 생성하십시오.

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
package com.example.pipeline_design_pattern_overview.service;

import com.example.pipeline_design_pattern_overview.model.PipelineCreator;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class PipelineService {

    private final PipelineCreator pipelineCreator;

    @Autowired
    public PipelineService(PipelineCreator pipelineCreator) {
        this.pipelineCreator = pipelineCreator;
    }

    public Object executePipeline(String inputData) {
        return pipelineCreator.getHandlers().stream()
                .reduce(Function.identity(), BaseFunction::andThen)
                .apply(inputData);
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

```java
package com.example.pipeline_design_pattern_overview.service;

import com.example.pipeline_design_pattern_overview.model.PipelineCreator;
import com.example.pipeline_design_pattern_overview.pipeline.BaseFunction;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;

@Service
public class PipelineService {

    @Autowired
    PipelineCreator pipelineCreator;

    public char[] execute(String input) {
        List<BaseFunction<String, Object>> handlers = pipelineCreator.getHandlers();

        var interResult = (Object)input;
        for(int handler = 0; handler < handlers.size(); handler++) {
            interResult = handlers.get(handler).apply((String) interResult);
        }
        return (char[]) interResult;
    }

}
```

실행:

입력 값을 ab-(8)$9_UP+_F로 해보죠.

```java
Input is: ab-(8)$9_UP+_F
밑줄 제거 전: ab-(8)$9_UP+_F
밑줄 제거 후: ab-(8)$9UP+F
특수 문자 제거 전: ab-(8)$9UP+F
특수 문자 제거 후: ab89UPF
숫자 제거 전: ab89UPF
숫자 제거 후: abUPF
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

최종 결과는 → abUPF

여기에 사용한 예제는 여기에서 확인할 수 있어요.

읽어주셔서 감사합니다. 즐거운 탐험되세요!!!