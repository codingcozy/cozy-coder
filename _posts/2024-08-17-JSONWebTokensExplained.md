---
title: "JSON Web Tokens JWT 정리"
description: ""
coverImage: "/assets/img/2024-08-17-JSONWebTokensExplained_0.png"
date: 2024-08-17 00:56
ogImage:
  url: /assets/img/2024-08-17-JSONWebTokensExplained_0.png
tag: Tech
originalTitle: "JSON Web Tokens Explained"
link: "https://medium.com/javascript-in-plain-english/json-web-tokens-explained-f32913d586d2"
isUpdated: true
updatedAt: 1723863739118
---

![JWT](/assets/img/2024-08-17-JSONWebTokensExplained_0.png)

JSON Web Token (JWT)은 안전하게 JSON 객체 형식으로 정보를 주고받기 위한 간결하고 자기 포함 방식을 정의하는 개방 표준(RFC 7519)입니다. 이 정보는 디지털로 서명되어 신뢰할 수 있습니다. JWT는 HMAC 알고리즘을 사용하여 비밀 (secret)을 이용하거나 RSA 또는 ECDSA를 사용하는 공개/비공개 키 쌍으로 서명할 수 있습니다.

간단히 말해, JSON Web Token (JWT)은 변경되지 않았음을 보장하기 위해 서명된 특별한 비밀 메시지인 것 같습니다.

# 왜 JWT를 사용해야 하나요?

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

JWT는 서버가 매번 사용자를 추적할 필요 없이 로그인할 수 있게 해줍니다. 전통적인 세션 기반 인증은 서버가 세션 정보를 저장하는 데 의존합니다. 사용자가 로그인하면 서버가 세션을 생성하고 메모리나 데이터베이스에 저장합니다. 그런 다음 서버는 클라이언트에 세션 ID를 제공하고 클라이언트가 후속 요청에 포함시킵니다. 서버는 세션 ID를 사용하여 세션 정보를 검색합니다.

반면에 JWT 기반 인증은 상태를 유지하지 않습니다. 서버는 사용자 정보를 포함한 JWT를 생성하고 클라이언트에 전송합니다. 클라이언트는 후속 요청에 JWT를 포함시킵니다. 서버는 세션 정보를 저장할 필요 없이 토큰을 확인할 수 있어, 확장 가능하며 분산 시스템에 적합합니다. JWT를 사용하면 한 번의 클릭으로 여러 웹사이트나 앱에 로그인할 수 있는 토큰을 사용할 수 있어서 Single Sign-On (SSO)과 같은 기능에 매우 유용합니다. 어디서나 작동하는 일반적인 패스가 있는 것과 같습니다!

JWT는 다음에 널리 사용됩니다:

- 인증: 사용자가 로그인한 후, 각 후속 요청에는 JWT가 포함되어 해당 토큰으로 허용된 경로, 서비스 및 리소스에 액세스할 수 있습니다.
- 정보 교환: JWT는 당사자 간에 안전하게 정보를 전송하는 데 사용될 수 있습니다. JWT는 서명될 수 있기 때문에 발신자가 자신들이 가장한다는 것을 확신할 수 있습니다.

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

# JWT 구조

JWT는 세 부분으로 구성된 문자열이며 각 부분은 점(.)으로 구분되어 있으며 base64로 직렬화됩니다 —

직렬화된 형태의 JWT는 일반적으로 다음과 같이 보입니다 — [헤더].[페이로드].[서명]

## 1. 헤더 —

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

해당 헤더는 주로 JWT에 적용된 암호화 작업을 설명하는 데 사용됩니다. 예를 들어, 해당 JWT에 적용된 서명/해독 기술 등이 포함됩니다. 또한 전송하는 정보의 미디어/콘텐츠 유형에 대한 데이터도 포함할 수 있습니다. 이 정보는 JSON 객체로 제공되며 해당 JSON 객체는 BASE64URL로 인코딩됩니다. 일반적으로 두 부분으로 구성됩니다:

- 토큰의 유형('typ'): 일반적으로 JWT.
- 서명 알고리즘('alg'): HMAC SHA256 또는 RSA와 같이 토큰에 서명하는 데 사용되는 알고리즘

예를 들어, 아래 헤더는 HS256 알고리즘을 사용하여 토큰 서명을 확인해야 함을 나타냅니다.

```json
{
  "typ": "JWT",
  "alg": "HS256"
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

## 2. 페이로드 —

페이로드는 JWT의 일부로 모든 사용자 데이터가 추가되는 부분입니다. 이 데이터는 JWT의 ‘클레임’으로도 불립니다. 클레임은 엔티티(일반적으로 사용자)에 대한 진술과 추가 데이터입니다. 이 정보는 누구에게나 읽을 수 있으므로 여기에 기밀 정보를 넣지 않는 것이 좋습니다. 이 정보는 JSON 객체로 나타나고, 이 JSON 객체는 BASE64URL로 인코딩됩니다. 우리는 페이로드 내부에 원하는 만큼의 클레임을 넣을 수 있지만, 헤더와 달리 페이로드에는 필수 클레임이 없습니다. 일반적으로 세 가지 유형의 클레임이 있습니다: 등록된 클레임, 공개 클레임, 그리고 개인 클레임 —

- 등록된 클레임: 권장되지만 필수는 아닌 미리 정의된 클레임(예: 발급자를 나타내는 iss, 만료 시간을 나타내는 exp 등).
- 공개 클레임: JWT를 사용하는 사람들이 자유롭게 정의할 수 있는 클레임입니다. 그러나 충돌을 피하기 위해 이들은 IANA JSON Web Token Registry에 정의하거나 충돌 방지 네임스페이스를 포함한 URI로 정의해야 합니다.
- 개인 클레임: 사용자 간에 정보를 공유하기 위해 생성된 사용자 정의 클레임입니다.

페이로드를 포함한 JWT는 다음과 같이 나타납니다:

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

```json
{
  "userId": "a12b89-c67d",
  "iss": "https://provider.domain.com/",
  "sub": "auth/some-hash-here",
  "exp": 153452683,
  "name": "Ishaan Gupta"
}
```

- 등록된 클레임:

  - iss (발급자): 토큰의 발급자를 식별합니다.
  - sub (주제): 토큰의 주제(사용자)를 식별합니다.
  - exp (만료 시간): 토큰의 만료 시간을 지정합니다.

2. 사용자 정의 클레임:

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

- userId: 시스템 내에서 사용자를 식별하는 고유 식별자입니다.
- name: 사용자의 이름입니다.

## 3. 서명 —

이는 JWT의 세 번째 부분으로 토큰의 진위성을 확인하는 데 사용됩니다. 서명 부분을 생성하려면 인코딩된 헤더, 인코딩된 페이로드, 비밀 키, 헤더에 지정된 알고리즘을 가져와야 합니다. 이 서명은 그 후에 .(점)을 사용하여 헤더와 페이로드에 추가됩니다. 이로써 실제 토큰 헤더.페이로드.서명을 생성합니다.

```js
해시알고리즘(base64UrlEncode(헤더) + “.” + base64UrlEncode(페이로드), 비밀키)
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

서명은 JWT의 보낸 사람이 자신을 신원 확인하고 메시지가 중간에 수정되지 않았음을 보장하는 데 사용됩니다.

따라서 이러한 모든 구성 요소가 JWT를 구성합니다. 이제 실제 토큰이 어떻게 생겼는지 살펴보겠습니다:

```js
JWT 예시:

헤더:
{
  "alg" : "HS256",
  "typ" : "JWT"
}

페이로드:
{
  "id": 1234567890,
  "name": "Ishaan Gupta"
}

비밀: Ishaan-Medium
```

```js
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTIzNDU2Nzg5MCwibmFtZSI6IklzaGFhbiBHdXB0YSJ9.XP--SEqvUXDpqRgwDMQaeitc9_r54lFccDna52Zd-TY
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

위에 제공된 예제용 JWT를 생성했습니다. 웹사이트 링크로 이동하여 직접 만들어 보세요—

아래 Node.js 코드를 사용하여 토큰을 생성해 보세요. 나중을 위해 이 gist를 템플릿 리포로 시작하려면 아래 코드를 사용하세요.

## JWT 작동 방식

아래는 JWT가 어떻게 확인되는지에 대한 기본 flowchart입니다. 각 단계는 자명한 것 같죠?

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

<img src="/assets/img/2024-08-17-JSONWebTokensExplained_1.png" />

이제 토큰을 생성하고 유효성을 검사하는 방법, 그리고 클라이언트와 서버 간 안전한 통신을 위해 JWT를 사용하는 방법에 대해 배워봅시다. 하지만 먼저, JWT의 인증 플로우를 이해해야 합니다.

## JWT 인증 플로우

일반적으로 JWT 인증 플로우는 다음 단계를 포함합니다:

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

- 사용자 로그인: 사용자가 자격 증명(예: 사용자 이름 및 암호)을 인증 서버에 제공합니다.
- 서버가 자격 증명 확인: 서버는 사용자의 자격 증명을 데이터베이스와 비교합니다.
- 서버가 JWT 생성: 인증에 성공하면, 서버는 사용자 정보(클레임)가 포함된 JWT를 생성하고 클라이언트에 다시 전송합니다.
- 클라이언트가 JWT 저장: 클라이언트는 JWT를 저장합니다(일반적으로 로컬 저장소나 쿠키에 저장)하고 이를 서버로의 후속 요청의 인증 헤더에 포함시킵니다.
- 서버가 JWT 유효성 검사: 각 요청마다, 서버는 JWT의 유효성을 검증하여 인증되었으며 만료되지 않았음을 확인합니다.
- 서버가 요청 처리: JWT가 유효하면, 서버는 요청을 처리하고 응답을 클라이언트에 반환합니다.

![JSON Web Tokens Explained](/assets/img/2024-08-17-JSONWebTokensExplained_2.png)

이제 코딩할 시간입니다. 코드 편집기를 열고 함께 코딩해 봅시다. 저는 Node.js와 Express를 사용한 위의 인증 흐름에 대한 매우 기본적인 예제를 안내해 드리겠습니다.

저희 프로젝트 구조는 이렇게 보일 것입니다 —

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
jwt-example/
├── config/
│   └── db.js
├── controllers/
│   └── authController.js
├── middlewares/
│   └── authMiddleware.js
├── models/
│   └── User.js
├── routes/
│   └── authRoutes.js
├── utils/
│   └── generateTokens.js
├── .env
├── .gitignore
├── index.js
```

## 단계 1: 프로젝트 설정하기

먼저, 기본 Node.js 프로젝트를 설정하세요. 아래 코드를 참조하세요:

```js
mkdir jwt-example
cd jwt-example
npm init -y
npm install express jsonwebtoken bcryptjs body-parser mongoose dotenv
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

## 단계 2: 구성 및 환경 변수

환경 변수를 안전하게 저장할 루트 디렉토리에 .env 파일을 만드세요:

```js
PORT=3000
MONGO_URI=mongodb://localhost:27017/jwt-example
JWT_SECRET=your-256-bit-secret
JWT_EXPIRATION=1h
REFRESH_TOKEN_SECRET=another-256-bit-secret
REFRESH_TOKEN_EXPIRATION=7d
```

## 단계 3: 데이터베이스 구성

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

MongoDB 연결을 구성하기 위해 config/db.js 파일을 만들어주세요. Mongoose를 사용하여 연결 설정을 합니다:

```js
const mongoose = require("mongoose");
const dotenv = require("dotenv");

dotenv.config();

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    console.log("MongoDB connected...");
  } catch (error) {
    console.error(error.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

## 단계 4: 사용자 모델

사용자 스키마를 정의하기 위해 models/User.js 파일을 만들어주세요.

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
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const UserSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true,
  },
  password: {
    type: String,
    required: true,
  },
});

UserSchema.pre("save", async function (next) {
  if (!this.isModified("password")) {
    return next();
  }
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
  next();
});

UserSchema.methods.matchPassword = async function (password) {
  return await bcrypt.compare(password, this.password);
};

module.exports = mongoose.model("User", UserSchema);
```

## 단계 5: 토큰 생성 유틸리티

토큰 생성을 처리하기 위해 utils/generateTokens.js를 만듭니다:

```javascript
const jwt = require("jsonwebtoken");
const dotenv = require("dotenv");

dotenv.config();

const generateAccessToken = (user) => {
  return jwt.sign({ id: user.id }, process.env.JWT_SECRET, { expiresIn: process.env.JWT_EXPIRATION });
};

const generateRefreshToken = (user) => {
  return jwt.sign({ id: user.id }, process.env.REFRESH_TOKEN_SECRET, {
    expiresIn: process.env.REFRESH_TOKEN_EXPIRATION,
  });
};

module.exports = { generateAccessToken, generateRefreshToken };
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

## 단계 6: 인증 컨트롤러

인증 로직을 처리하기 위한 controllers/authController.js를 생성하세요:

```js
const jwt = require("jsonwebtoken");
const User = require("../models/User");
const dotenv = require("dotenv");
const { generateAccessToken, generateRefreshToken } = require("../utils/generateTokens");

dotenv.config();

exports.register = async (req, res) => {
  const { username, password } = req.body;
  try {
    let user = await User.findOne({ username });
    if (user) {
      return res.status(400).json({ message: "이미 존재하는 사용자입니다" });
    }

    user = new User({ username, password });
    await user.save();

    const accessToken = generateAccessToken(user);
    const refreshToken = generateRefreshToken(user);

    res.cookie("refreshToken", refreshToken, { httpOnly: true, secure: true });

    res.status(201).json({ accessToken });
  } catch (error) {
    console.error(error.message);
    res.status(500).send("서버 오류");
  }
};

exports.login = async (req, res) => {
  const { username, password } = req.body;
  try {
    const user = await User.findOne({ username });
    if (!user) {
      return res.status(400).json({ message: "유효하지 않은 자격 증명입니다" });
    }

    const isMatch = await user.matchPassword(password);
    if (!isMatch) {
      return res.status(400).json({ message: "유효하지 않은 자격 증명입니다" });
    }

    const accessToken = generateAccessToken(user);
    const refreshToken = generateRefreshToken(user);

    res.cookie("refreshToken", refreshToken, { httpOnly: true, secure: true });

    res.json({ accessToken });
  } catch (error) {
    console.error(error.message);
    res.status(500).send("서버 오류");
  }
};

exports.refreshToken = (req, res) => {
  const refreshToken = req.cookies.refreshToken;
  if (!refreshToken) {
    return res.status(401).json({ message: "토큰이 제공되지 않았습니다" });
  }

  jwt.verify(refreshToken, process.env.REFRESH_TOKEN_SECRET, (err, decoded) => {
    if (err) {
      return res.status(401).json({ message: "유효하지 않은 토큰입니다" });
    }

    const accessToken = generateAccessToken({ id: decoded.id });
    res.json({ accessToken });
  });
};
```

## 단계 7: 인증 미들웨어

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

중간웨어를 보호하기 위해 middlewares/authMiddleware.js를 생성하세요:

```js
const jwt = require("jsonwebtoken");
const dotenv = require("dotenv");

dotenv.config();

exports.verifyToken = (req, res, next) => {
  const token = req.headers["authorization"];
  if (!token) {
    return res.status(403).json({ message: "No token provided" });
  }

  jwt.verify(token.split(" ")[1], process.env.JWT_SECRET, (err, decoded) => {
    if (err) {
      return res.status(500).json({ message: "Failed to authenticate token" });
    }

    req.user = decoded.id;
    next();
  });
};
```

## 단계 8: 인증 라우트

인증 라우트를 정의하는 routes/authRoutes.js를 생성하세요:

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
const express = require("express");
const { register, login, refreshToken } = require("../controllers/authController");
const { verifyToken } = require("../middlewares/authMiddleware");
const router = express.Router();

router.post("/register", register);
router.post("/login", login);
router.post("/token", refreshToken);
router.get("/protected", verifyToken, (req, res) => {
  res.json({ message: "보호된 경로에 오신 것을 환영합니다!", user: req.user });
});

module.exports = router;
```

## 단계 9: 서버 파일 만들기

서버를 설정하려면 index.js 파일을 만드세요:

```javascript
const express = require("express");
const bodyParser = require("body-parser");
const connectDB = require("./config/db");
const authRoutes = require("./routes/authRoutes");
const cookieParser = require("cookie-parser");
const dotenv = require("dotenv");

dotenv.config();
connectDB();

const app = express();
const port = process.env.PORT || 3000;

app.use(cookieParser());
app.use(bodyParser.json());
app.use("/api/auth", authRoutes);

app.listen(port, () => {
  console.log(`포트 ${port}에서 서버가 실행 중입니다.`);
});
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

제가 말한 대로 테스트 부분은 여러분께 맡기도록 할게요. Postman이나 cURL을 사용해서 하시면 됩니다. cURL을 사용하려면 아래에 명령어를 작성해 놨어요. Postman을 사용하려면 직접 해보신 후 댓글로 문제가 있는 부분 알려주세요.

# JWT 사용 사례

JSON Web Tokens (JWT)는 안전한 정보 교환과 인증이 필요한 다양한 시나리오에서 사용할 수 있습니다:

## 사용 사례 1: 인증과 권한 부여 —

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

위 구현에서 설명이 명확히 되어 있기 때문에 이를 설명할 필요가 없습니다.

## 사용 사례 2: 정보 교환

JWT는 시스템의 서로 다른 부분 또는 다른 시스템 간에 안전하게 정보를 전송하는 데 사용할 수 있습니다. JWT는 서명되어 있기 때문에 수신자는 정보의 진위성과 무결성을 확인할 수 있습니다.

![이미지](/assets/img/2024-08-17-JSONWebTokensExplained_3.png)

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

## 사용 사례 3: 단일 로그인 (SSO)

JWT는 단일 로그인 (SSO) 시스템에서 널리 사용됩니다. SSO를 사용하면 한 번 로그인하면 다시 로그인할 필요 없이 여러 응용 프로그램 또는 서비스에 액세스 할 수 있습니다.

![SSO](/assets/img/2024-08-17-JSONWebTokensExplained_4.png)

위와 같은 경우에 대한 기본 샘플 코드도 제공했습니다.

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
// SSO 제공자는 JWT를 발급합니다
app.post('/sso-login', (req, res) => {
  const { username, password } = req.body;

  const user = users.find(u => u.username === username);
  if (!user) {
    return res.status(401).json({ message: '잘못된 자격 증명입니다' });
  }

  const isPasswordValid = await bcrypt.compare(password, user.password);
  if (!isPasswordValid) {
    return res.status(401).json({ message: '잘못된 자격 증명입니다' });
  }

  const token = jwt.sign({ username }, secretKey, { expiresIn: '1h' });
  res.json({ token });
});

// 애플리케이션은 JWT를 검증합니다
app.get('/app1', verifyToken, (req, res) => {
  res.json({ message: '어플리케이션 1에 오신 것을 환영합니다!', user: req.user });
});
```

## 사용 사례 4: 마이크로서비스 아키텍처

마이크로서비스 아키텍처에서 JWT를 사용하여 각 마이크로서비스 간에 정보를 안전하게 전달할 수 있습니다. 각 마이크로서비스는 JWT를 검증하여 요청이 신뢰할 수 있는 출처에서 오는지 확인합니다.

![이미지](/assets/img/2024-08-17-JSONWebTokensExplained_5.png)

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

## 사용 사례 5: API 보안

JWT는 인증된 사용자만 특정 엔드포인트에 액세스할 수 있도록 하는 방식으로 API를 보호하는 데 사용될 수 있습니다. 각 API 요청에는 JWT가 포함되며, 서버는 액세스 권한을 부여하기 전에 토큰을 확인합니다.

# JWT의 장단점

## 장점 —

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

## 1.) 상태 없는 인증

JWT는 상태 없는 인증을 가능하게 합니다. 이는 서버가 세션 정보를 저장할 필요가 없음을 의미하며, 이는 서버의 메모리 사용량을 줄이고 아키텍처를 간소화합니다.

예시: 전통적인 세션 기반 인증에서는 서버가 각 사용자의 세션 데이터를 저장합니다. 그러나 JWT를 사용하면, 서버는 토큰을 확인하기만 하면 되기 때문에 세션 저장소가 필요하지 않습니다.

## 2.) 확장성

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

JWTs는 stateless이므로 확장 가능한 애플리케이션에 이상적입니다. 여러 서버가 세션 정보를 공유할 필요 없이 요청을 처리할 수 있습니다.

예: 부하 분산 환경에서 클라이언트가 요청 헤더에 유효한 JWT를 포함하면 어떤 서버든 요청을 처리할 수 있습니다.

## 3.) 간결하고 휴대 가능

JWT는 간결하고 URL, 헤더 또는 쿠키를 통해 쉽게 전송될 수 있습니다. 작은 크기로 대역폭 사용을 줄이고 모바일 애플리케이션에 적합합니다.

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

## 4.) 안전

JWT는 비밀 (HMAC) 또는 공개/개인 키 쌍 (RSA 또는 ECDSA)을 사용하여 디지털로 서명되어 클라이언트나 공격자에 의해 수정되는 것을 방지합니다.

## 단점 —

## 1. ) 토큰 크기

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

JWT가 많은 클레임을 포함하거나 RSA 또는 ECDSA로 서명된 경우 토큰의 크기가 커질 수 있습니다. 이는 대부분 대역폭이 제한된 모바일 애플리케이션에서 성능에 영향을 줄 수 있습니다.

예시: 다중 클레임과 RSA 서명이 있는 JWT는 간단한 HMAC으로 서명된 토큰보다 상당히 크게 될 수 있습니다. 한 번 확인해보세요!

## 2.) 내장 폐기 기능이 없음

JWT에는 폐기 기능이 내장되어 있지 않습니다. 발급된 후에는 JWT가 만료될 때까지 유효하며, 별도의 폐기 전략이 구현되지 않는 한 계속 유효합니다.

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

예시: 사용자의 JWT가 유출된 경우, 토큰을 무효화하는 방법은 토큰 블랙리스트를 구현하거나 리프레시 토큰을 사용하여 단기간의 토큰을 발급하는 방법뿐입니다.

## 3.) 단기 수명

폐기 매커니즘 부재를 완화하기 위해 JWT는 주로 짧은 수명을 갖습니다. 그러나 이는 사용자에게 빈번한 재인증이나 토큰 갱신을 필요로 하며, 이는 사용자에게 불편을 주는 요소가 될 수 있습니다.

예시: 만료 시간이 짧은 토큰은 자주 갱신해야 하며, 이는 클라이언트 및 서버 코드에 복잡성과 추가 서버 호출을 추가하게 됩니다.

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

## 4.) 보안 위험

올바르게 구현되지 않으면 JWT(Javascript Web Tokens)는 보안 위험을 초래할 수 있습니다. 이러한 위험에는 다음과 같은 것들이 있습니다:

- 토큰 노출: JWT가 안전하지 않은 곳(e.g., 로컬 스토리지)에 저장되어 있으면 악성 스크립트에 의해 액세스 될 수 있습니다. (XSS 공격을 통해 액세스 가능합니다. HttpOnly 쿠키에 저장하는 것이 좋습니다.)
- 알고리즘 혼란: 약한 또는 안전하지 않은 알고리즘을 사용하여 JWT에 서명하는 경우 보안이 위험에 노출될 수 있습니다.
- 재생 공격: JWT는 유효 기간 내 여러 번 재사용될 수 있으며 추가적인 조치를 취하지 않는 한 위험이 있습니다.
- 하나의 비밀 키에 의존: JWT의 생성은 하나의 비밀 키에 의존합니다. 해당 키가 노출되면 공격자가 자신의 JWT를 조작할 수 있으며 API 레이어에서 수락할 것입니다. 이는 비밀 키가 노출되면 공격자가 모든 사용자의 ID를 가장할 수 있다는 것을 의미합니다. 이 위험을 감소시키기 위해 때때로 비밀 키를 변경할 수 있습니다.

# JWT를 사용하는 시기

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

- API에 액세스: 상태를 유지하지 않고 안전한 API 인증을 위해 사용합니다.
- 성공적인 검증 후 세션 시작: 도메인 간 및 SSO와 같은 유연한 인증을 위해 사용합니다.
- 다른 메커니즘과 함께 사용: 다른 인증 표준과 함께 보안 및 기능을 강화하기 위해 사용합니다.

## JWT를 사용하지 말아야 할 때:

- 쿠키로 사용하지 마세요: 보안 위험과 저장 제한 때문에 JWT를 쿠키에 직접 저장하는 것을 피하세요.
- 사용자 세션 관리에 사용해서는 안 됩니다: 브라우저에서 사용자 세션을 관리하기에는 JWT가 적합하지 않습니다.
- 일회용 시나리오에 사용하지 마세요: JWT는 재사용될 수 있으므로 일회용 시나리오에는 적합하지 않습니다.
- 권한 또는 애플리케이션 관련 데이터를 JWT에 넣지 마세요: JWT에 너무 많은 데이터를 저장하면 토큰 크기가 커지고 보안 위험이 증가할 수 있습니다.

# 마지막으로

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

이 기사에서는 기초부터 더 많은 사용 예제에 이르기까지 거의 모든 것을 JWT에 대해 다뤘어요. 초심자가 이해해야 할 모든 것을 설명하려고 노력했고, 자신있게 말할 수 있어요. 이제 JWT를 사용하여 애플리케이션을 만들 준비가 되었습니다!

이해하셨기를 바라며 최대한 많이 활용해보세요. 이 기사를 즐겁게 읽으셨고 비슷한 주제나 다른 기술에 대해 더 알아보고 싶다면 박수를 치고 댓글을 남겨주세요. 이 기사를 친구들과 JWT 및 인증에 대해 배우고 싶어하는 사람들과 공유해주세요!

인증 관련 내 기사도 확인해보세요 -

![이미지](https://miro.medium.com/v2/resize:fit:1000/0*6YeWzl-N9Vnrcqv4.gif)

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

# 쉬운 영어로 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 반드시 글쓴이를 Clap하고 팔로우해 주세요 👏
- 팔로우: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문: CoFeed | Differ
- PlainEnglish.io 에서 더 많은 컨텐츠 확인하기
