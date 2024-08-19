---
title: "자바 Spring Boot에서 로그문 사용 방법"
description: ""
coverImage: "/assets/img/2024-08-19-MasteringLogStatementsinJavaSpringBootBestPracticesExamplesandLogLevelConfiguration_0.png"
date: 2024-08-19 02:35
ogImage: 
  url: /assets/img/2024-08-19-MasteringLogStatementsinJavaSpringBootBestPracticesExamplesandLogLevelConfiguration_0.png
tag: Tech
originalTitle: "Mastering Log Statements in Java Spring Boot Best Practices, Examples, and Log Level Configuration"
link: "https://medium.com/@tecnicorabi/mastering-log-statements-in-java-spring-boot-best-practices-examples-and-log-level-configuration-8618c2a61b60"
isUpdated: true
updatedAt: 1724034688149
---


![2024-08-19-MasteringLogStatementsinJavaSpringBootBestPracticesExamplesandLogLevelConfiguration_0.png]

안녕하세요, 개발자 여러분!

자바 Spring Boot에서 로깅은 애플리케이션의 흐름을 추적하고 문제를 진단하며 상태를 이해하는 데 중요합니다. 새로 오신 분이든 경험이 많은 분이든 효과적인 로깅을 습득하면 디버깅 및 모니터링 역량이 향상됩니다. 이 간단한 가이드에서는 다음 사항을 다룰 것입니다:

- Best Practices: 중요한 로그 문장 작성 방법을 배워보세요.
- Examples: 명확하고 실용적인 예제를 통해 어떻게 하는지 확인해 보세요.
- Do’s and Don’ts: 무엇을 로그로 남길지, 건너뛸지 알아봅니다.
- Log Levels: 각 로그 레벨의 차이와 어떤 경우에 사용해야 하는지 이해합니다.
- Configuration: 다양한 로그 레벨을 활성화하거나 비활성화하는 방법을 알아봅니다.

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

자, 함께 들어가서 로깅을 활용해보는 건 어때요?

자바 스프링 부트에서 로깅이 중요한 이유

로깅은 소프트웨어 개발 세계에서 생명선과 같아요. 다음과 같은 이점을 제공해줍니다:

- 실행 흐름 추적: 코드가 실행되는 경로를 이해합니다.
- 문제 진단: 잘못된 부분을 식별합니다.
- 애플리케이션 상태 모니터링: 성능과 잠재적 병목 현상을 주시합니다.
- 사용자 활동 감사: 보안 및 규정 준수를 위해 중요한 사용자 활동을 추적합니다.

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

1. Java Spring Boot에서 로그인하는 위치

전략적 배치: 로그 문을 배치하는 위치를 알아내는 것은 유용한 로그를 만드는 데 중요합니다.

- 애플리케이션 시작 및 종료: 애플리케이션이 시작되고 종료될 때 로그를 남깁니다.

```js
logger.info("애플리케이션이 성공적으로 시작되었습니다.");
logger.info("애플리케이션이 종료 중입니다.");
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

- 오류 처리: 항상 예외와 오류를 기록하여 실패 지점을 정확히 파악하세요.

```javascript
try {
    // 일부 코드
} catch (Exception e) {
    logger.error("오류가 발생했습니다: ", e);
}
```

- 중요한 비즈니스 로직: 비즈니스 로직에서 중요한 이벤트를 기록하여 애플리케이션의 동작을 추적하세요.

```javascript
logger.info("사용자 {}가 로그인했습니다.", userId);
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

- 외부 시스템 상호작용: 투명성을 위해 외부 시스템과의 상호작용을 로그로 기록합니다 (예: API, 데이터베이스).

```js
logger.debug("외부 API에 요청 보내는 중: {}", requestPayload);
logger.debug("외부 API로부터 응답 수신: {}", responsePayload);
```

2. 자바 스프링 부트에서 로깅해야 하는 내용 및 로깅해서는 안 되는 내용

로그하는 항목:

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

- 에러 및 예외: 항상 스택 트레이스와 함께 이러한 것들을 로그에 남기세요.

```js
logger.error("요청 처리 중 예외가 발생했습니다: ", exception);
```

- 중요한 상태 변화: 애플리케이션에서 중요한 상태 변경 사항을 캡처하세요.

```js
logger.info("주문 상태가 {}에서 {}(으)로 변경되었습니다", oldStatus, newStatus);
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

- 성능 지표: 성능이 중요한 섹션에 걸린 시간을 추적합니다.

```js
long startTime = System.currentTimeMillis();
// 코드 실행
long endTime = System.currentTimeMillis();
logger.info("methodX 메서드의 실행 시간: {} ms", (endTime - startTime));
```

- 사용자 작업: 감사 목적으로 중요한 사용자 작업을 로깅합니다.

```js
logger.info("사용자 {}가 {} 작업을 수행했습니다", userId, action);
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

로그하지 말아야 할 내용:

- 민감한 정보: 패스워드, 신용카드 번호 또는 개인 정보와 같은 민감한 데이터는 로깅하지 말아야 합니다.

```js
// 좋지 않은 예시
logger.info("사용자 비밀번호: {}", password);
```

- 과도한 세부 정보: 로그를 메우는 과도한 세부 정보를 로깅하지 마세요. 필요한 경우가 아니라면 모든 변수와 모든 메소드 호출을 로깅하지 마세요.

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

3. 자바 스프링 부트에서 로그 레벨 이해하기

로그 레벨은 메시지의 심각성 또는 중요성을 정의합니다. 가장 심각한 것부터 가장 중요하지 않은 것까지 로그 레벨은 다음과 같습니다:

- ERROR: 즉각적인 주의가 필요한 심각한 문제를 나타냅니다. 어플리케이션이 계속 실행되는 것을 방해할 수 있는 중요한 문제를 기록합니다.

```js
logger.error("트랜잭션 처리 실패 {}", transactionId, exception);
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

- WARN: 잠재적으로 해로운 상황을 나타냅니다. 심각하지는 않지만 살펴봐야 할 문제에 대한 경고를 기록합니다.

```js
logger.warn("Memory usage is high: {}%", memoryUsage);
```

- INFO: 대략적인 수준에서 응용 프로그램의 진행 상황을 강조하는 정보 메시지를 제공합니다.

```js
logger.info("Service started on port {}", port);
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

- DEBUG: 애플리케이션을 디버깅하는 데 가장 유용한 세밀한 정보 이벤트를 제공합니다.

```js
logger.debug("userId에 대한 사용자 세부 정보 가져 오기: {}", userId);
```

- TRACE: DEBUG보다 더 세밀한 매우 상세한 로깅 정보를 제공합니다.

```js
logger.trace("파라미터로 methodX 시작: {}", params);
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

4. 스프링 부트에서 로그 레벨 구성하기

애플리케이션.properties 또는 애플리케이션.yml 파일을 사용하여 스프링 부트에서 로그 레벨을 구성할 수 있습니다. 다음은 그 방법입니다:

application.properties를 사용하는 방법:

```js
# 루트 로깅 레벨 설정
logging.level.root=INFO

# 특정 패키지에 대한 로깅 레벨 설정
logging.level.com.example=DEBUG
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

application.yml 파일을 사용하면:

```yaml
logging:
  level:
    root: INFO
    com.example: DEBUG
```

# 특정 로그 레벨 활성화 및 비활성화

특정 유형의 로그를 켜거나 제외하려면 구성 파일에서 로깅 수준을 조정할 수 있습니다. 몇 가지 예시는 다음과 같습니다:

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

DEBUG 로깅을 특정 패키지에 활성화하세요:

```js
logging.level.com.example=DEBUG
```

이 설정은 com.example 패키지의 DEBUG, INFO, WARN 및 ERROR 로그가 출력될 것을 의미합니다.

TRACE 로그 제외:

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
logging.level.com.example=INFO
```

이 설정은 TRACE 로그가 출력되지 않지만 DEBUG, INFO, WARN 및 ERROR 로그는 출력됩니다.

# 예시 시나리오

- DEBUG 로깅 활성화:

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
logging.level.com.example=DEBUG
```

어떤 내용이 출력될까요?

- com.example 패키지에 대한 DEBUG, INFO, WARN 및 ERROR 메시지가 출력됩니다.

2. TRACE 로깅 활성화:

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
logging.level.com.example=TRACE
```

무엇이 출력됩니까?

- com.example 패키지에 대한 TRACE, DEBUG, INFO, WARN 및 ERROR 메시지가 출력됩니다.

3. 루트 로깅 레벨을 WARN으로 설정하기:

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
logging.level.root=WARN
```

어떻게 출력되나요?

- 특정 패키지에 대해 재정의되지 않는 한 전체 어플리케이션에서는 WARN 및 ERROR 메시지만 출력됩니다.

4. 서로 다른 패키지에 대한 서로 다른 로깅 수준:

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

```yaml
logging:
  level:
    root: WARN
    com.example.service: DEBUG
    com.example.controller: INFO
```

무슨 메시지가 출력되나요?

- 전체 애플리케이션에 대한 WARN 및 ERROR 메시지.
- com.example.service에 대한 DEBUG, INFO, WARN 및 ERROR 메시지.
- com.example.controller에 대한 INFO, WARN 및 ERROR 메시지.

# 스프링 부트에서 로그 레벨을 동적으로 변경하기

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

스프링 부트 액추에이터는 애플리케이션을 다시 시작하지 않고 로그 레벨을 동적으로 변경할 수 있는 방법을 제공합니다. 다음은 그 방법입니다:

- 액추에이터 종속성 추가:

```js
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

2. 로거 엔드포인트 활성화:


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

```javascript
management.endpoints.web.exposure.include=loggers
```

3. HTTP POST 요청을 통해 로그 레벨 변경:

```javascript
curl -X POST "http://localhost:8080/actuator/loggers/com.example" -H "Content-Type: application/json" -d '{"configuredLevel":"DEBUG"}'
```

- 이를 통해 com.example의 로깅 레벨이 DEBUG로 설정됩니다.


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

# 자바 Spring Boot에서 로깅하는 뛰어난 방법

- 일관된 형식 사용: 로그 메시지가 쉽게 구문 분석되도록 일관된 형식을 따르도록 합니다.

```js
logger.info("주문 ID: {}, 상태: {}", orderId, status);
```

- 구조화된 로깅 사용: 가능한 경우 구조화된 로깅을 사용하여 쉬운 구문 분석 및 검색을 가능하게 합니다.

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
{
  "timestamp": "2024-08-06T12:34:56.789Z",
  "level": "INFO",
  "message": "주문이 성공적으로 처리되었습니다",
  "orderId": 12345,
  "status": "완료"
}
```

- 반복문 안에서 로그를 남기지 말기: 반복문 내에서 로그를 남기면 성능에 상당한 영향을 줄 수 있습니다.

```js
// 대신 반복문 내에서 로그를 남기지 말고 한 번에 집계하고 루프 외부에서 로그를 남깁니다.
```

- 적절한 수준에서 로깅: 로깅할 메시지에 적절한 로그 수준을 사용하여 혼란을 피하고 로그를 유지 관리하세요.

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
logger.debug("methodX의 디버깅 정보: {}", details);
logger.warn("methodY에서 잠재적인 문제 발견");
```

- 로그 정기적으로 확인하기: 주기적으로 로그 문장들을 확인하여 여전히 관련성이 있고 유용한지 확인해보세요.

# 스프링 부트에서 로깅 예시

다음은 스프링 부트 애플리케이션에서 로깅을 설정하고 사용하는 실제 예시입니다.

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

- 로깅 의존성 추가: 스프링 부트는 기본적으로 Logback을 사용하므로 추가 의존성이 필요하지 않습니다.
- application.properties에서 로깅 레벨 구성:

```js
logging.level.root=INFO
logging.level.com.example.myapp=DEBUG
```

3. 코드에 로그 문 작성:

```js
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {
    private static final Logger logger = LoggerFactory.getLogger(MyController.class);

    @GetMapping("/hello")
    public String hello() {
        logger.info("Hello endpoint was called");
        try {
            // 비즈니스 로직
            logger.debug("비즈니스 로직이 성공적으로 실행되었습니다");
            return "Hello, World!";
        } catch (Exception e) {
            logger.error("안녕 엔드포인트 처리 중 오류가 발생했습니다", e);
            throw e;
        }
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

그런데 제 최신 블로그 "자바 개발자가 하는 5가지 실수 (및 그것을 피하는 방법)"도 확인해보세요.

결론

효과적인 로깅은 모든 개발자에게 중요한 기술입니다. 이러한 모범 사례를 따르고 다양한 로그 수준의 사용 방법을 이해함으로써 로그가 유용하고 정보를 제공하며 과도하게 상세하지 않도록 할 수 있습니다. 전략적으로 로그를 남기고 정기적으로 로그를 검토하여 유용성을 유지하십시오.