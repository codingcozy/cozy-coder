---
title: "NodeJS와 ExpressJS에서 JOI 유효성 검사 사용하기"
description: ""
coverImage: "/assets/img/2024-08-19-JOIValidationinNodeJSExpressJS_0.png"
date: 2024-08-19 02:33
ogImage: 
  url: /assets/img/2024-08-19-JOIValidationinNodeJSExpressJS_0.png
tag: Tech
originalTitle: "JOI Validation in NodeJS  ExpressJS"
link: "https://medium.com/@ahmadmukhtar2001/joi-validation-in-node-js-express-js-0b1955b5ea4b"
isUpdated: false
---


## CRUD 애플리케이션을 위한 요청 유효성 검증 기술 간소화

![이미지](/assets/img/2024-08-19-JOIValidationinNodeJSExpressJS_0.png)

# JOI란 무엇인가요?

JOI는 데이터 유효성 검사를 간편하게 만드는 JavaScript 라이브러리입니다. 양식, API 요청 또는 다른 데이터 소스로부터의 사용자 입력일 경우 JOI를 사용하면 검증 규칙을 쉽게 정의하고 적용할 수 있습니다. 들어오는 데이터가 깨끗하고 안전한지 보장하여 JOI는 잘못된 또는 악의적인 입력에 의해 발생하는 예기치 않은 문제로부터 응용 프로그램을 보호합니다.

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

# JOI의 일반적인 사용 방법

가정해 보겠습니다. 사용자가 응용 프로그램에 로그인하기 위해 다음 자격 증명을 입력했다고 가정해 봅시다:

```js
{ 
  email: "myemail00@xyz.com",
  password: "This_Is_My_Password11"
}
```

이제 응용 프로그램을 통해 사용자가 입력한 이메일과 비밀번호를 유효성 검사해야 하는데, 다음은 여러분의 요구 사항입니다:

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

- 이메일:
— ‘String’ 으로 지정되어야 함
— 최소 5자 이상 최대 255자
— 비어 있을 수 없음, 필수 필드임
— 유효한 이메일 형식이어야 함 (즉, ‘@’, ‘.com’ 등이 포함되어 있어야 함)
- 비밀번호
— ‘String’ 으로 지정되어야 함
— 최소 5자 이상 최대 255자
— 비어 있을 수 없음, 필수 필드임

## 수동 확인

일반적인 시나리오에서 라이브러리를 사용하지 않고, 각 개별 엔티티에 대해 이러한 조건이 충족되는지 수동으로 확인하는 함수를 만들어야 합니다. 이 함수는 다음과 같이 보일 수 있습니다:

```js
function validateUser(user) {
  const errors = {};

  // 이메일이 존재하고 문자열이며 5자에서 255자 사이의 길이를 가지고 있는지, 유효한 이메일 형식인지 확인
  if (!user.email || typeof user.email !== 'string') {
    errors.email = '이메일은 필수이며 문자열이어야 합니다.';
  } else if (user.email.length < 5 || user.email.length > 255) {
    errors.email = '이메일은 5자에서 255자 사이여야 합니다.';
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(user.email)) {
    errors.email = '유효한 이메일 주소여야 합니다.';
  }

  // 비밀번호가 존재하고 문자열이며 5자에서 255자 사이의 길이를 가지고 있는지 확인
  if (!user.password || typeof user.password !== 'string') {
    errors.password = '비밀번호는 필수이며 문자열이어야 합니다.';
  } else if (user.password.length < 5 || user.password.length > 255) {
    errors.password = '비밀번호는 5자에서 255자 사이여야 합니다.';
  }
  
  // 오류를 발견하지 못했을 경우 null 반환
  return Object.keys(errors).length === 0 ? null : errors;
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

이 방법은 긴, 번잡하고 확장 가능하지 않습니다. 더 쉽고 훨씬 더 확장 가능한 방법은 JOI 검증 라이브러리를 사용하는 것입니다.

## JOI를 활용한 확인(아주 쉬워요)

JOI를 사용하려면 먼저 JOI 패키지를 설치한 다음 JOI 라이브러리를 가져와야 합니다. 설치를 위해 터미널에서 다음 명령을 실행하세요:

- npm install joi

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

지금 다음 명령어를 사용하여 JS 파일에서 JOI 모듈을 가져오세요:

```js
const Joi = require('joi');
```

이제 위에 작성된 함수를 간단히 다음 JOI 함수로 대체할 수 있습니다:

```js
function validateUser(user) {
  const schema = {
    email: Joi.string().min(5).max(255).required().email(),
    password: Joi.string().min(5).max(255).required()
  };

  return Joi.validate(user, schema);
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

단순화된 것이 분명히 보입니다. 이 예시에서는 매우 적은 JOI 체크만 사용되었지만 min(x), max(y), required(), int(), string(), alphanum() 등 많은 다른 체크가 있습니다.

이런 명령어들은 문서 사이트에서 확인할 수 있어요: [https://joi.dev/api/?v=17.13.3](https://joi.dev/api/?v=17.13.3)

## Joi.validate( obj , schema ) function

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

위의 리턴문에서 검사되는 것은 입력된 객체를 정의된 스키마와 비교하며, 불리언 값이 아닌 두 가지 주요 속성이 있는 객체를 반환합니다:

- `error`:
유효성 검사에 실패하면 이 속성에는 유효성 검사 실패를 설명하는 오류 객체가 포함됩니다. 유효성 검사에 성공하면 이 속성은 `null`입니다.
- `value`:
유효성 검사에 성공하면 이 속성에는 정리되고 유효성이 검사된 데이터가 포함됩니다. 유효성 검사에 실패하면 이 속성은 `undefined`입니다.

따라서 반환된 객체의 `error` 속성을 검사하여 유효성 검사가 성공했는지 실패했는지 확인할 수 있습니다. `error`가 `null`이면 유효성 검사가 성공했습니다. `error`가 `null`이 아닌 경우에는 유효성 검사가 실패했으며, `error.details`를 사용하여 오류 세부 정보에 액세스할 수 있습니다. 

# CRUD 애플리케이션에서 무엇을 유효성 검사해야 하는가?

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

CRUD 애플리케이션에서 각 HTTP 요청 유형은 고유한 목적을 제공하며 특정한 특성을 가지고 있습니다. 따라서 JOI를 사용한 요청 유효성 검사는 해당 내용에 맞게 설정되어야 합니다. 다음 표에서는 각 요청 유형에서 어떤 부분을 검증해야 하는지 안내합니다:

![JOIValidationinNodeJSExpressJS_1](/assets/img/2024-08-19-JOIValidationinNodeJSExpressJS_1.png)

## GET(Read) 요청

- Headers: ✅
엔드포인트를 안전하게 유지하고 무단 액세스를 제한하기 위해 GET 요청의 헤더를 확인하여 요청이 승인되었고 합법적인 소스에서 온 것임을 보장합니다.
- Body: ❌
보통 GET 요청에는 요청 본문이 포함되지 않습니다. 데이터를 보내는 대신 데이터를 검색하는 데 사용되기 때문에 이 경우 일반적으로 본문을 검증할 필요가 없습니다.

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

다음은 JOI를 사용하여 GET 요청을 유효성 검사하는 예제입니다:

```js
app.get('/get-user/:id', (req, res) => {
  
  // 요청 헤더 유효성 검사
  const { error: headerError } = headerSchema.validate(req.headers);
  if (headerError) {
    return res.status(400).json({ error: headerError.details[0].message });
  }

  // URL 매개변수 유효성 검사 (요청에 'id'가 포함되어 있는 경우)
  const paramSchema = Joi.string().guid();
  const { error: paramError } = paramSchema.validate(req.params.id);
  if (paramError) {
    return res.status(400).json({ error: paramError.details[0].message });
  }

  // 요청 처리
  // ...
});
```

## POST (생성) 요청

- 헤더: ✅
GET 요청과 유사하게, POST 요청에서는 권한 및 보안을 위해 헤더를 유효성 검사할 수 있습니다.
- 본문: ✅
POST 요청에서는 일반적으로 새로운 리소스를 생성하는 데 사용할 데이터가 요청 본문에 포함됩니다. 요청 본문을 유효성 검사하면 새 리소스를 생성하기 전 제공된 데이터가 예상 형식과 제한에 부합하는지 확인할 수 있습니다.

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

다음은 POST 예시입니다:

```js
app.post('/create-user', (req, res) => {

  // 요청 헤더 유효성 검사
  const { error: headerError } = headerSchema.validate(req.headers);
  if (headerError) {
    return res.status(400).json({ error: headerError.details[0].message });
  }


  const userSchema = Joi.object({
    username: Joi.string().alphanum().min(3).max(30).required(),
    email: Joi.string().email().required(),
    age: Joi.number().integer().min(18).max(99)
  });


  // 요청 본문 유효성 검사
  const { error } = userSchema.validate(req.body);
  if (error) {
    return res.status(400).json({ error: error.details[0].message });
  }


  // 요청 처리 및 새로운 사용자 생성
  // ...
});
```

## PUT (수정) 요청

- 헤더: ✅
GET 및 POST 요청과 마찬가지로, PUT 요청에서는 헤더를 유효성 검사하여 권한과 보안을 확인할 수 있습니다.
- 본문: ✅
PUT 요청에서는 일반적으로 요청 본문에 기존 리소스의 업데이트된 데이터가 포함됩니다. 헤더와 본문 모두 유효성을 검사하면 요청이 인가되었으며 업데이트된 데이터가 필요한 형식과 제약 조건을 준수하는지 확인할 수 있습니다.

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

다음은 PUT에 대한 예시입니다:

```js
app.put('/update-user/:id', (req, res) => {

  // 요청 헤더 유효성 검사
  const { error: headerError } = headerSchema.validate(req.headers);
  if (headerError) {
    return res.status(400).json({ error: headerError.details[0].message });
  }


  // URL 매개변수 유효성 검사
  const { error: paramError } = Joi.string().guid().validate(req.params.id);
  if (paramError) {
    return res.status(400).json({ error: paramError.details[0].message });
  }


  // 사용자 업데이트를 위한 요청 본문 유효성 검사
  const updateUserSchema = Joi.object({
    username: Joi.string().alphanum().min(3).max(30),
    email: Joi.string().email(),
    age: Joi.number().integer().min(18).max(99)
  });


  const { error: bodyError } = updateUserSchema.validate(req.body);
  if (bodyError) {
    return res.status(400).json({ error: bodyError.details[0].message });
  }


  // 요청 처리 및 사용자 업데이트
  // ...
});
```

## DELETE 요청

- 헤더: ✅
기타 요청 유형과 마찬가지로, DELETE 요청에서도 헤더를 유효성 검사할 수 있어서 권한이 있는 요청만 처리되도록 할 수 있습니다.
- 본문: ❌
일반적으로 DELETE 요청은 URL에 의해 식별된 리소스를 삭제하는 것이 목적이기 때문에 요청 본문이 필요하지 않습니다. 따라서 DELETE 요청에서 본문을 유효성 검사할 필요가 거의 없습니다.

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

다음은 DELETE 요청에 대한 예시입니다:

```js
app.delete('/delete-user/:id', (req, res) => {
  // 요청 헤더 유효성 검사
  const { error: headerError } = headerSchema.validate(req.headers);
  if (headerError) {
    return res.status(400).json({ error: headerError.details[0].message });
  }


  // URL 매개변수 유효성 검사
  const { error: paramError } = Joi.string().guid().validate(req.params.id);
  if (paramError) {
    return res.status(400).json({ error: paramError.details[0].message });
  }


  // 요청 처리 및 사용자 삭제
  // ...
});
```

## 결론

이 방법의 목표는 CRUD 작업에서 데이터 무결성, 보안 및 일관성을 보호하는 것입니다. 요청 헤더를 유효성 검사함으로써, 인가된 요청만 의도한 작업을 수행할 수 있도록 보장합니다. 반면에 요청 본문을 검증하면, 데이터가 처리되거나 데이터베이스 작업에 의해 추가로 처리되기 전에 데이터가 예상대로 충족되는지 확인함으로써 오류를 방지하고 애플리케이션을 취약점으로부터 보호하는 역할을 합니다.

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

JOI는 직관적인 인터페이스를 제공하여 전체 유효성 검사 프로세스를 간단하게 만들어줍니다. 애플리케이션이 발전함에 따라 유효성 규칙을 조정하고 업데이트하기가 쉬워집니다. 앱이 성장하고 필요 사항이 변화할 때, JOI를 사용하여 검사를 손쉽게 정제하여 모든 것을 일관되고 안전하게 유지할 수 있습니다.