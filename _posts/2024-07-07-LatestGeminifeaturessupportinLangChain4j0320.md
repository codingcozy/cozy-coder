---
title: "LangChain4j 0320에서 지원되는 최신 Gemini 기능 설명"
description: ""
coverImage: "/assets/img/2024-07-07-LatestGeminifeaturessupportinLangChain4j0320_0.png"
date: 2024-07-07 14:06
ogImage: 
  url: /assets/img/2024-07-07-LatestGeminifeaturessupportinLangChain4j0320_0.png
tag: Tech
originalTitle: "Latest Gemini features support in LangChain4j 0.32.0"
link: "https://medium.com/google-cloud/latest-gemini-features-support-in-langchain4j-0-32-0-732791e4c34c"
isUpdated: true
---





어제 LangChain4j 0.32.0이 출시되었습니다! 이번 업데이트에는 제가 제출한 새로운 젬니 특징들을 지원하는 Pull Request가 포함되어 있습니다:

- JSON 출력 모드: 젬니에게 JSON을 사용하여 응답하도록 강제합니다. 어떤 마크업도 없이,
- JSON 스키마: JSON 출력을 스키마를 준수하도록 제한하고 제어할 수 있습니다,
- Google 검색 웹 결과 및 Vertex AI 데이터 저장소 내의 개인 데이터와의 응답 연계,
- 새로운 빌더 메서드 덕분에 더 쉬운 디버깅,
- 함수 호출 모드(없음, 자동 또는 일부 함수),
- 유해한 프롬프트와 응답을 막는 안전 설정.

이 제품의 새로운 기능을 코드 예제를 통해 함께 살펴보겠습니다! 그리고 기사 맨 끝에 도착하면 2가지 추가 보너스 기능도 발견하실 수 있습니다. 함께 살펴보시죠!

<div class="content-ad"></div>

# JSON 출력 모드

LLM 기반 애플리케이션을 만드는 것은 텍스트 작업을 의미합니다. 왜냐하면 LLM은 텍스트를 반환하기 때문입니다. LLM 응답과 코드 간 통합을 용이하게 하기 위해 선택한 형식은 보통 JSON입니다. JSON은 사람이 읽기 쉽고 프로그래밍적으로 쉽게 구문 분석할 수 있기 때문입니다.

그러나 LLM은 다소 수다스러우며 깔끔한 원시 JSON 문서를 반환하는 대신에 약간의 추가 문구와 일부 마크다운 마크업을 넣어 응답합니다.

다행히 Gemini 1.5 (플래시 및 프로)에서는 응답 MIME 유형을 지정할 수 있습니다. 현재는 application/json만 지원되지만 향후 다른 형식이 추가될 수 있습니다.

<div class="content-ad"></div>

**요령을 기억하세요!** 제멘아이 모델을 인스턴스화 할 때, responseMimeType() 빌더 메소드를 사용해야 합니다:

```js
var model = VertexAiGeminiChatModel.builder()
    .project(PROJECT_ID)
    .location(LOCATION)
    .modelName("gemini-1.5-flash")
    .responseMimeType("application/json")
    .build();

String response = model.generate("Roll a dice");

System.out.println(response);
```

JSON만 남기고, 문장이나 마크다운은 제거해주세요:

```js
{"roll": 3}
```

<div class="content-ad"></div>

안녕하세요! 타로 분야 전문가로서 여러분을 환영합니다!

요청 내용에 JSON 응답을 원한다는 내용이 명시되어 있지 않았어도 JSON 문서의 키가 시간에 따라 변경될 수 있으므로, 여전히 더 명확한 가이드가 필요할 수 있습니다. 모델에게 명시적으로 JSON 반환을 요청하거나, 예상되는 JSON 출력의 예제를 제시하는 것이 좋습니다. 이는 일반적인 요청 방식입니다.

하지만 여기에 또 다른 추가 정보가 있습니다!

# JSON 스키마 출력

마크다운 형식으로 코드 블록을 사용하여 위 내용을 변경하시면 됩니다~ 만약 추가로 도움이 필요하시다면 언제든지 물어주세요!

<div class="content-ad"></div>

이 문서는 LLM 생태계에서 상당히 독특한데요, JSON 출력을 제약하는 JSON 스키마를 지정할 수 있는 유일한 모델이라고 생각합니다. 이 기능은 Gemini 1.5 Pro에서만 작동하며 Gemini 1.5 Flash에서는 작동하지 않습니다.

이전 주사위 굴리기 예시를 다시 살펴보고 출력 생성을 위한 JSON 스키마를 지정하는 방법을 업데이트해 봅시다.

```java
import static dev.langchain4j.model.vertexai.SchemaHelper.fromClass;
//...

record DiceRoll(int roll) {}

var model = VertexAiGeminiChatModel.builder()
    .project("genai-java-demos")
    .location("us-central1")
    .modelName("gemini-1.5-pro")
    .responseSchema(fromClass(DiceRoll.class))
    .build();

String response = model.generate("Roll a dice");

System.out.println(response);
```

생성된 JSON 문서에는 항상 roll 키가 포함될 것입니다.

<div class="content-ad"></div>

이 예시에서는 fromClass()라는 Convenience 메서드를 사용했습니다. 이 메서드는 Java type에 해당하는 JSON 스키마를 만드는 데 사용합니다 (여기서는 Java 레코드입니다).

하지만 fromJsonSchema()라는 다른 편리한 메서드를 사용할 수도 있습니다. 이 메서드를 사용하면 JSON 스키마 문자열을 전달할 수 있습니다:

```java
var model = VertexAiGeminiChatModel.builder()
    .project("genai-java-demos")
    .location("us-central1")
    .modelName("gemini-1.5-pro")
    .responseSchema(fromJsonSchema("""
        {
            "type": "object",
            "properties": {
                "roll": {
                    "type": "integer"
                }
            }
        }
        """))
    .build();
```

<div class="content-ad"></div>

이제 항상 일관된 JSON 출력을 얻을 수 있어요!

# Google Search 웹 결과 및 Vertex AI 데이터 저장소에 대한 응답 기반화

<div class="content-ad"></div>

대형 언어 모델은 환상적인 창의적인 기계들이지만, 그들의 높은 창의성보다는 데이터와 문서에 기반한 사실적인 답변을 선호합니다.

젬니(Gemini)는 답변을 근거로 하는 기능을 제공합니다:

- Google 검색 웹 결과에 대한,
- Vertex AI 검색 데이터 저장소에 대한.

# Google 검색을 사용하여 답변 근거 설정하기

<div class="content-ad"></div>

LLM의 교육은 특정 날짜, 즉 마감일에서 끝이 났습니다. 그래서 그 날짜 이후 일어난 소식을 알지 못합니다. 그러나 Google 검색을 이용하여 Gemini에게 더 최신 정보를 찾아오도록 요청할 수 있습니다.

예를 들어, 우리가 Gemini에게 현재 프랑스에서 진행 중인 선거에 대해 물어본다면, 다음과 같이 답변할 수 있습니다:

```python
현재 프랑스에서는 국가적 선거가 진행 중이지 않습니다.

프랑스에서 마지막으로 중요한 국가 선거는 **2022년 4월과 5월에 열린 대통령 선거**였으며, 그 때 에마뉘엘 마크롱이 두 번째 당선되었습니다.

그러나 프랑스의 각 지역에서 **지역 선거**가 규칙적으로 진행되고 있습니다.

프랑스 선거에 대한 최신 정보를 알고 싶다면, **프랑스 내무부**의 웹사이트나 **The Guardian, BBC, CNN 또는 Le Monde**와 같은 신뢰할 수 있는 뉴스 소스를 확인해보세요.
```

이제 useGoogleSearch(true) 메서드를 사용하여 Google 검색 웹 결과를 활성화해봅시다.

<div class="content-ad"></div>

```js
var model = VertexAiGeminiChatModel.builder()
    .project(PROJECT_ID)
    .location(LOCATION)
    .modelName("gemini-1.5-flash")
    .useGoogleSearch(true)
    .build();

String response = model.generate(
    "프랑스에서 현재 진행 중인 선거는 무엇인가요?");

System.out.println(response);
```

정답은 매우 다를 것이며 실제적이고 최신 정보일 것입니다:

```js
2024년 7월 4일에 프랑스는 의회 선거의 제1라운드를 개최했습니다. 제2라운드는 2024년 7월 7일에 진행될 예정입니다. 이 선거는 제2차 세계 대전 이후 프랑스에서 처음으로 극우 정부가 되는 결과를 초래할 수 있습니다. 국민연링, 대통령 에마뉘엘 마크롱의 중도주의 연합 및 뉴인기진 전선 연합이 이 선거에 참여하는 세 가지 주요 정치 집단입니다. 이 선거의 결과는 매우 불확실한데, 극우 국민연링이 의회 다수를 획득할 가능성이 있습니다. 국민연링이 다수를 획득하면 마크롱은 해당당 대표인 조르단 바르델라를 총리로 임명할 것으로 예상됩니다.
```

프랑스에서 현재 의회 선거가 진행 중입니다. 이 선거들은 딱 한 달 전에 결정되었기 때문에 모델의 지식 마감일 이후입니다.


<div class="content-ad"></div>

# Vertex AI Search을 활용한 Grounding

안녕하세요! 오늘은 우리 자신의 데이터를 기반으로 응답을 Grounding하는 방법에 대해 알아보려 합니다. 이는 우리가 필요로 하는 지식이 사실 개인 정보인 경우, 예를 들어 내부 문서나 고객 문서와 같은 경우에 특히 중요합니다.

제 동료 Mete가 개인 데이터를 사용해 Grounding 설정하는 방법을 설명한 훌륭한 기사를 썼어요. 아래에서는 구글 클라우드 스토리지 버킷을 백엔드로 사용하는 Vertex AI 검색 앱을 만들고, 그 안에 Cymbel Starlight 자동차 모델에 관한 가짜 문서인 차 설명서가 있는 상황을 상정하겠습니다! Mete의 기사에서 사용된 예시를 그대로 사용하겠습니다.

이번에는 search 위치를 명시적으로 지정하여 Vertex AI 검색 데이터 스토어를 가리키는 방식으로 작업할 거에요:

<div class="content-ad"></div>


이것은 실제로 존재하지 않는 허구의 자동차입니다만, 그 비밀 문서에 담겨 있었고 실제로 Gemini는 그 질문에 대답할 수 있게 되었습니다:

Cymbal Starlight 2024의 화물 수송 능력은 13.5 cubic feet 입니다.

더 흥미로운 점은 Gemini에 의해 반환된 응답이 사용자 쿼리에 답변하는 데 도움이 된 소스 문서에 대한 일부 맥락을 제공한다는 것입니다 (다음 섹션에서 요청 및 응답 로깅을 활성화하는 방법을 살펴볼 것입니다):


<div class="content-ad"></div>


그러나 솔직히 말해서, 숫자가 정확히 무슨 의미를 하는지 잘 모르겠어요. 이 메타데이터에는 클라우드 저장소에 업로드된 PDF가 LLM의 답변 형성에 사용된 것이며, 해당 문서에서 발견된 문장의 발췌가 포함되어 있습니다.

# 요청 및 응답 로깅

내부 작업을 더 잘 이해하기 위해 요청 및 응답 로깅을 활성화할 수 있습니다. 이렇게 하면 Gemini에 보내지는 내용과 Gemini의 답변을 정확하게 확인할 수 있습니다.


<div class="content-ad"></div>

로그를 활성화하려면 두 가지 방법을 사용할 수 있어요:

- logRequests(true)을 사용하면 Gemini에 보낸 요청을 로깅할 수 있고,
- logResponse(true)를 사용하면 Gemini로부터 받은 응답을 로깅할 수 있어요.

이를 실제로 동작하는 것을 살펴보죠:

```js
var model = VertexAiGeminiChatModel.builder()
    .project(PROJECT_ID)
    .location(LOCATION)
    .modelName("gemini-1.5-flash")
    .logRequests(true)
    .logResponses(true)
    .build();

String response = model.generate("Why is the sky blue?");

System.out.println(response);
```

<div class="content-ad"></div>

요청한 내용에 대해 좀 더 설명 드리겠습니다.

LangChain4j는 기본적으로 로깅을 위해 Slf4j를 사용합니다. 요청 및 응답 로그는 DEBUG 수준으로 기록됩니다. 따라서 로거와/또는 로거 파사드를 설정해야 합니다.

이 기사를 위한 테스트 프로젝트에서 Slf4j와 Simple 로거에 대한 Maven 종속성을 다음과 같이 구성했습니다:

```js
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.30</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>1.7.30</version>
</dependency>
```

어떤 사항에 대해 더 궁금한 것이 있으시다면 언제든지 물어보세요!

<div class="content-ad"></div>


안녕하세요! 오늘은 로깅 관련해서 이야기를 나눠볼게요.

로그 설정 파일인 src/main/resources/simplelogger.properties을 만들었어요. 여기에는 다음과 같은 설정이 포함되어 있어요:


org.slf4j.simpleLogger.defaultLogLevel=debug
org.slf4j.simpleLogger.log.io.grpc.netty.shaded=info


기본 로깅 레벨을 debug로 설정했어요. 그런데 Gemini Java SDK에서 내부적으로 사용하는 네트워킹 라이브러리인 Netty가 debug 레벨로 로그를 남기고 있었어요. 그래서 이 라이브러리에 대한 로깅은 info 이상으로만 설정하여 너무 많은 출력을 방지했어요.


<div class="content-ad"></div>

# 기능 호출 모드

지금까지 Gemini를 사용할 때 함수 호출에 대한 모델은 스스로 어떤 함수를 호출할지, 그리고 어떤 함수를 호출할지 결정했습니다.

하지만 Gemini는 함수나 도구 선택을 제어할 수 있는 기능을 소개합니다.

옵션은 3가지가 있습니다:

<div class="content-ad"></div>

- AUTO - 익숙하고 기본적인 모드로, 쌍둥이는 기능 호출이 필요한 지 스스로 결정하고 어떤 것을 선택해야 하는 지를 결정합니다.
- ANY - 사용 가능한 모든 기능 중 일부를 지정할 수 있지만, 모델이 그 중 하나를 선택하도록 강제합니다 (쌍둥이 1.5 Pro에서만 지원됨).
- NONE - 도구가 정의되고 사용 가능하더라도, 쌍둥이가 해당 도구 중 어느 것도 사용하지 못하게 합니다.

이 예제를 살펴보겠습니다:

```java
var model = VertexAiGeminiChatModel.builder()
    .project(PROJECT_ID)
    .location(LOCATION)
    .modelName("gemini-1.5-pro")
    .logRequests(true)
    .logResponses(true)
    .toolCallingMode(ToolCallingMode.ANY)
    .allowedFunctionNames(Arrays.asList("add"))
    .build();

ToolSpecification adder = ToolSpecification.builder()
    .description("두 숫자를 더합니다.")
    .name("add")
    .addParameter("a", JsonSchemaProperty.INTEGER)
    .addParameter("b", JsonSchemaProperty.INTEGER)
    .build();

UserMessage message = UserMessage.from("3 + 4는 얼마인가요?");
Response<AiMessage> answer = model.generate(asList(message), adder);

System.out.println(
    answer.content().toolExecutionRequests().getFirst());
```

우리는 ToolCallingMode.ANY 모드를 지정하고, 모델이 관련 요청에 답변하기 위해 선택해야 하는 함수들의 허용된 이름을 나열했습니다(allowedFunctionNames() 빌더 메서드를 사용하여).

<div class="content-ad"></div>

우리는 호출할 수 있는 도구를 설명합니다. 우리는 메시지를 만듭니다. 그리고 generate()를 호출할 때 호출하려는 함수에 해당하는 도구 사양을 전달합니다.

출력은 모델이 필수 도구 실행 요청으로 응답한 것을 보여줄 것입니다:

```js
ToolExecutionRequest { id = null, name = "add",
                       arguments = "{"a":3.0,"b":4.0}" }
```

이제 우리 차례로 주어진 인수로 add 함수를 호출합니다. 그런 다음 함수 실행 결과를 Gemini에 다시 전송합니다.

<div class="content-ad"></div>

# 안전 설정 지정

LLM(언어 연결 모델)을 사용하는 애플리케이션에서 사용자가 어떤 이상한 텍스트 입력을 할 수 있는 경우, 섭입할 수 있는 해로운 콘텐츠를 제한하고 싶을 것입니다. 이를 위해, 다양한 콘텐츠 범주에 대해 수용할 수 있는 임계값을 다르게 설정할 수 있는 안전 설정을 지정할 수 있습니다:

```java
import static dev.langchain4j.model.vertexai.HarmCategory.*;
import static dev.langchain4j.model.vertexai.SafetyThreshold.*;
//...
var model = VertexAiGeminiChatModel.builder()
    .project(PROJECT_ID)
    .location(LOCATION)
    .modelName("gemini-1.5-flash")
    .safetySettings(Map.of(
        HARM_CATEGORY_DANGEROUS_CONTENT, BLOCK_LOW_AND_ABOVE,
        HARM_CATEGORY_SEXUALLY_EXPLICIT, BLOCK_MEDIUM_AND_ABOVE,
        HARM_CATEGORY_HARASSMENT, BLOCK_ONLY_HIGH,
        HARM_CATEGORY_HATE_SPEECH, BLOCK_MEDIUM_AND_ABOVE
    ))
    .build();
```

앱을 더 안전하게 만들고, 악의적이거나 나쁜 사용자를 피하고 싶다면, 이 방법이 바로 좋은 방법입니다!

<div class="content-ad"></div>

# 보너스 포인트 #1: 람다 함수를 사용한 스트리밍 응답

저희는 젬니니 중심의 특징에 대한 리뷰를 마무리 지으며 프로젝트에 기여한 하나의 추가 기능을 소개하려 합니다. 스트리밍 모델을 사용할 때 스트리밍 콘텐츠 핸들러 대신 람다를 전달할 수 있는 기능입니다.

이것은 젬니니와 관련이 없는, 어떤 모델에서든 사용할 수 있는 기능입니다!

구체적으로 말하면, 모델을 스트리밍 모드로 사용하여 모델이 생성하는 대로 응답이 출력되도록 보고 싶다면, 일반적으로 다음과 같은 코드를 작성할 것입니다:

<div class="content-ad"></div>

익명 내부 클래스를 구현하는 StreamingResponseHandler 인터페이스를 활용하는 방식은 다소 장황합니다. 다행히도 저는 코드를 좀 더 간결하게 만들기 위해 몇 가지 정적 메서드를 제공하였습니다. 이를 import하여 사용하면 됩니다:

```java
import static dev.langchain4j.model.LambdaStreamingResponseHandler.onNext;
import static dev.langchain4j.model.LambdaStreamingResponseHandler.onNextAndError;
//...

// onNext
model.generate("Why is the sky blue?",
    onNext(System.out::println));

// onNextAndError
model.generate("Why is the sky blue?",
    onNextAndError(
        System.out::println,
        ex -> { throw new RuntimeException(ex); }
));
```

이제 LLM 출력을 쉽게 스트리밍할 수 있습니다!

<div class="content-ad"></div>

# 보너스 포인트 #2: Imagen v3을 이용해 멋진 이미지 생성하기

새 LangChain4j 릴리스에서 두 번째 보너스 포인트는 Vertex AI 이미지 모델이 이제 Imagen v3(Google DeepMind의 최신 고품질 이미지 생성 모델)를 지원한다는 것입니다. 

이제 이미지를 생성할 때 활용할 수 있는 몇 가지 새로운 매개변수가 있습니다. 다음 이미지 생성 코드를 살펴보겠습니다:

```js
var imagenModel = VertexAiImageModel.builder()
    .project(PROJECT)
    .location(LOCATION)
    .endpoint(ENDPOINT)
    .publisher("google")
    .modelName("imagen-3.0-generate-preview-0611")
    .aspectRatio(VertexAiImageModel.AspectRatio.LANDSCAPE)
    .mimeType(VertexAiImageModel.MimeType.JPEG)
    .compressionQuality(80)
    .watermark(true) // Imagen v3의 기본 설정값은 true입니다
    .withPersisting()
    .logRequests(true)
    .logResponses(true)
    .build();

String prompt = """
    서로 손을 흔드는 두 손, 한 쪽은 젊은 손이고 다른 한 쪽은 늙은 손으로, 
    세대 간의 진심어린 감사와 연결을 나타내는 두 손의 묵직한 브러시 스트로크가 담긴 유화 초상화
    """;

Response<Image> imageResponse = imagenModel.generate(prompt);
System.out.println(imageResponse.content().url());
```

<div class="content-ad"></div>

결과 이미지를 확인해 볼까요?

```js
![이미지 설명](/assets/img/2024-07-07-LatestGeminifeaturessupportinLangChain4j0320_1.png)
```

위 코드에서 새로운 빌더 메소드들을 눈여겨 보셨을 겁니다:

- aspectRatio() - 정사각형 뿐만 아니라 넓고 좁은 랜드스케이프와 포트레이트 모드가 제공됩니다.
- mimeType() - PNG 외에도 JPEG 이미지 생성을 요청할 수 있습니다.
- comressionQuality() - JPEG를 요청할 때 이미지 인코딩을 위한 압축 수준을 선택할 수 있습니다.
- watermark() - 생성된 모든 이미지에 SynthId 워터마크가 표시됩니다.
- logRequest() / logResponse() - 모델과 교환되는 내용을 확인할 수 있습니다.
- persistToCloudStorage() - 생성된 이미지를 클라우드 스토리지 버킷에 저장하도록 지정할 수 있습니다 (이 예제에서는 사용되지 않음).

<div class="content-ad"></div>

만약 이미지 v3에 액세스할 기회가 있다면, v2와 비교해서 정말 훌륭한 품질 향상을 느낄 수 있을 거에요!

# 결론

LangChain4j의 이번 릴리스에는 많은 새로운 쌍둥이 자리 관련 기능들이 포함되어 있어요! 이 글이 여러분께 그것들에 대해 알려주는 데 도움이 되었길 바라며, 여러분의 프로젝트에서 사용하고 싶게 만들기를 바랄게요.

만약 LangChain4j를 이용해 자바 개발자들을 위한 Gemini 셀프 페이스 코드랩인 'Gemini codelab'을 진행해보고 싶다면 잊지 말고 확인해보세요.

<div class="content-ad"></div>

2024년 7월 5일에 https://glaforge.dev에서 원문이 게시되었습니다.