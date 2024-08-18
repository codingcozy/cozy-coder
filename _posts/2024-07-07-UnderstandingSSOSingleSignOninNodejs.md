---
title: "Nodejs에서 SSO단일 로그인를 이해하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-UnderstandingSSOSingleSignOninNodejs_0.png"
date: 2024-07-07 23:04
ogImage:
  url: /assets/img/2024-07-07-UnderstandingSSOSingleSignOninNodejs_0.png
tag: Tech
originalTitle: "Understanding SSO (Single Sign On) in Node.js"
link: "https://medium.com/@patilchetan2110/understanding-sso-single-sign-on-in-node-js-7596ac73b9db"
isUpdated: true
---

현대 웹 애플리케이션에서 Single Sign-On (SSO)은 사용자 인증을 간편하게 하고 보안을 개선하는 중요한 구성 요소입니다. 이 블로그 포스트에서는 OAuth 2.0 및 OpenID Connect와 같은 인기있는 인증 프로토콜을 활용하여 Node.js 애플리케이션에 SSO를 구현하는 방법에 대해 살펴보겠습니다.

![image](/assets/img/2024-07-07-UnderstandingSSOSingleSignOninNodejs_0.png)

# Single Sign-On (SSO)이란 무엇인가요?

Single Sign-On을 사용하면 사용자는 한 번 인증하고 여러 애플리케이션이나 서비스에 다시 로그인할 필요 없이 여러 애플리케이션 또는 서비스에 액세스할 수 있습니다. 이는 사용자 경험을 향상시키는 것뿐만 아니라 조직에 대한 관리를 간소화하여 인증 및 권한 부여를 중앙 집중화함으로써 도와줍니다.

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

SSO가 어떻게 작동하는지 단계별로 이해해 봅시다:

- 사용자가 보호된 리소스에 액세스하고, 서비스 제공자(SP)에 의해 ID 공급자(IdP)로 리디렉션됩니다.
- 사용자는 자격 증명을 사용하여 IdP에서 인증합니다.
- IdP는 사용자의 신원을 확인하는 보안 토큰 또는 주장을 발급합니다.
- 사용자는 토큰을 가지고 SP로 돌아갑니다.
- SP는 토큰의 진실성과 무결성을 확인합니다.
- 유효하면, 리소스에 액세스가 허용됩니다.
- 선택 사항: 사용자를 위한 세션이 SP의 도메인 내에서 설정됩니다.
- 편리한 인증을 통해 사용자가 자격 증명을 다시 입력하지 않고도 여러 리소스에 액세스할 수 있습니다.
- 모든 서비스에서 동시 로그아웃할 수 있는 단일 로그아웃이 제공될 수 있습니다.
- SSO는 사용자 경험을 향상시키고, 마찰을 줄이며, 응용 프로그램 간 보안을 유지하는 데 도움이 됩니다.

![SSO 이미지](/assets/img/2024-07-07-UnderstandingSSOSingleSignOninNodejs_1.png)

# SSO 전략이란 무엇인가요?

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

싱글 사인온(SSO) 전략은 적절한 인증 프로토콜과 ID 제공자(IdP)의 선택, SSO 기능을 애플리케이션에 통합, 사용자 교육과 교육, 견고한 보안 조치의 실행, 지속적인 모니터링 및 유지 보수, 확장 가능성 계획, 사용자 경험 최적화, 그리고 규정 및 거버넌스 요구 사항 준수를 포함합니다. 요구 사항을 평가하고 적절한 프로토콜과 IDP를 선택하며, 완벽하게 통합하고, 사용자를 교육하며, 보안을 보장하고, 성능을 모니터링하며, 확장 가능성을 계획하고, 사용자 경험을 최적화하며, 규정을 준수한다면, 조직은 인증 프로세스를 강화하고, 보안을 강화하며, 응용 프로그램 생태계 전반에서 사용자 만족도를 향상시킬 수 있는 통합적인 SSO 전략을 수립할 수 있습니다.

# 왜 SSO가 중요한가

SSO를 구현하면 여러 가지 이점이 있습니다:

- 사용자 경험 향상: 사용자는 여러 세트의 자격 증명을 기억할 필요가 없어지므로 마찰이 줄어들고 사용성이 향상됩니다.
- 향상된 보안: 중앙 집중식 인증은 비밀번호 관련 보안 위반의 위험을 줄이고 사용자 액세스에 대한 적절한 제어를 가능하게 합니다.
- 효율적인 관리: 기관은 중앙에서 정책을 시행하고 관리 부담을 줄여 사용자 액세스를 효과적으로 관리할 수 있습니다.

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

# Node.js에서 SSO 구현하기

Node.js 애플리케이션에서 SSO 구현 세부사항에 대해 알아보겠습니다.

## 1. SSO 제공자 선택

Google, Facebook 또는 passport.js와 같은 라이브러리를 사용한 사용자 정의 솔루션을 포함한 OAuth 2.0 제공자와 같은 여러 SSO 제공자 중에서 선택할 수 있습니다. 이 예시에서는 Google을 SSO 제공자로 사용하겠습니다.

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

## 2. 인증 서버 설정

우선 Node.js와 Express를 사용하여 인증 서버를 설정해봅시다:

```js
const express = require("express");
const passport = require("passport");
const GoogleStrategy = require("passport-google-oauth20").Strategy;

const app = express();

passport.use(
  new GoogleStrategy(
    {
      clientID: "********your-client-id********",
      clientSecret: "********your-client-secret********",
      callbackURL: "/auth/google/callback",
    },
    (accessToken, refreshToken, profile, done) => {
      return done(null, profile);
    }
  )
);

app.get("/auth/google", passport.authenticate("google", { scope: ["profile", "email"] }));

app.get("/auth/google/callback", passport.authenticate("google", { failureRedirect: "/login" }), (req, res) => {
  res.redirect("/");
});

app.listen(3000, () => {
  console.log("서버가 3000포트에서 실행 중입니다");
});
```

이 코드에서:

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

- GoogleStrategy를 사용하여 Passport.js를 구성합니다.
- 인증을 시작하고 콜백을 처리하는 라우트를 정의합니다.

## 3. SSO 제공업체와 통합

다음으로, 우리는 Node.js 어플리케이션을 SSO 제공업체의 적절한 자격 증명으로 구성해야 합니다. Google의 경우, Google 개발자 콘솔에서 프로젝트를 생성하여 클라이언트 ID와 클라이언트 비밀을 얻을 수 있습니다.

## 4. 라우트 보호하기

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

인증이 필요한 경로를 보호하기 위해 사용자가 인증되었는지 확인하는 미들웨어를 만들 수 있습니다:

```js
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) {
    return next();
  }
  res.redirect("/login");
}

app.get("/profile", ensureAuthenticated, (req, res) => {
  res.render("profile", { user: req.user });
});
```

이 코드에서:

- ensureAuthenticated 미들웨어는 Passport.js의 isAuthenticated 메서드를 사용하여 사용자가 인증되었는지 확인합니다.
- 사용자가 인증되면 다음 미들웨어나 라우트 핸들러로 진행되고, 그렇지 않으면 로그인 페이지로 리다이렉트됩니다.

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

## 5. 사용자 세션 처리

사용자 세션을 관리하기 위해 express-session 미들웨어와 Passport.js를 사용할 수 있습니다:

```js
const session = require("express-session");

app.use(
  session({
    secret: "당신의-비밀-키",
    resave: false,
    saveUninitialized: false,
  })
);

app.use(passport.initialize());
app.use(passport.session());

passport.serializeUser((user, done) => {
  done(null, user);
});

passport.deserializeUser((obj, done) => {
  done(null, obj);
});
```

# SSO에서의 안전성 (SSO 보안)

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

싱글 사인온(SSO)은 올바르게 구현될 때 안전한 인증 방법이 될 수 있습니다. SSO의 안전성은 선택한 인증 프로토콜과 신원 제공자(IdP)의 신뢰성을 포함한 여러 요소에 달려 있습니다. OAuth 2.0 또는 OpenID Connect와 같은 인증 프로토콜은 보안 기능과 기존 시스템과의 호환성을 고려하여 신중하게 선택되어야 합니다. 또한 사용자를 인증하고 보안 토큰을 발급하는 IdP는 신뢰할 수 있고 암호화 및 다중 인증 요소(MFA)와 같은 강력한 보안 조치를 도입해야 합니다. 토큰의 안전한 전송, 저장 및 유효성 검사는 무단 액세스를 방지하기 위해 중요합니다. 적절한 세션 관리, 안전한 구현 관행 준수 및 보안 위협을 인식하고 피하는 데 사용자 교육이 SSO의 보안 수준에 도움이 됩니다.

그러나 SSO가 편리함과 효율성을 제공하면서 조직이 대응해야 할 잠재적인 보안 위험도 동반합니다. 세션 위조 및 피싱 공격과 같은 취약점은 SSO 시스템을 침해할 수 있습니다. 지속적인 모니터링, 감사 및 사용자 인식 훈련은 보안 사건을 신속히 감지하고 대응하는 데 필수적입니다. 적절한 보안 조치를 적용함으로써 조직은 SSO의 이점을 활용하면서 보안 위험을 효과적으로 완화할 수 있습니다. 사용자와 조직을 위한 안전한 SSO 환경을 보장하기 위해 시스템과 데이터의 기밀성, 무결성 및 가용성을 유지하는 것이 중요합니다.

# 결론

Node.js 애플리케이션에 싱글 사인온을 구현하면 보안을 강화하고 사용자 인증을 간소화하며 전체 사용자 경험을 향상시킬 수 있습니다. Passport.js와 같은 인기있는 인증 프로토콜과 라이브러리를 활용하여 개발자들은 애플리케이션에 SSO 기능을 원활하게 통합할 수 있습니다.

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

# 추가 자료

- Passport.js 문서
- Google OAuth 2.0 문서
