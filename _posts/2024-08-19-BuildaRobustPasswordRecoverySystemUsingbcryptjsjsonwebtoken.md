---
title: "bcryptjs와 jsonwebtoken을 활용한 강력한 비밀번호 복구 시스템 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-BuildaRobustPasswordRecoverySystemUsingbcryptjsjsonwebtoken_0.png"
date: 2024-08-19 02:12
ogImage: 
  url: /assets/img/2024-08-19-BuildaRobustPasswordRecoverySystemUsingbcryptjsjsonwebtoken_0.png
tag: Tech
originalTitle: "Build a Robust Password Recovery System Using bcryptjs, jsonwebtoken"
link: "https://medium.com/@emmanueljirigho/build-a-robust-password-recovery-system-using-bcryptjs-jsonwebtoken-8a99be22b444"
isUpdated: true
updatedAt: 1724033115962
---


<img src="/assets/img/2024-08-19-BuildaRobustPasswordRecoverySystemUsingbcryptjsjsonwebtoken_0.png" />

시작하기 전에, express, mongoose 및 nodemailer가 설치되어 있고 설정되어 있다고 가정합니다. 이 프로젝트가 작동하려면 이미 mongoose를 사용하여 User 이메일, 이름 및 비밀번호 필드가 설정된 User 모델이 있어야 합니다. 이것은 총 초보자를 위한 프로젝트가 아니며 mongoose 및 express를 사용하고 express의 미들웨어에 대한 이해가 조금 있는 경우에 필요합니다. 즐거운 독서😊.

## 단계 1: 프로젝트 설정

이 단계에서는 bcryptjs를 사용하여 비밀번호를 해싱하기 위한 필수 패키지를 설치하고, 토큰을 생성하고 검증하기 위한 jsonwebtoken, 환경 변수를 안전하게 관리하기 위한 dotenv를 설치하여 프로젝트를 설정합니다. 이러한 도구들은 안전한 비밀번호 복구 시스템을 구축하는 데 도움이 됩니다.

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
npm i bcrypt jsonwebtoken dotenv
```

다음으로 백엔드 작업을 시작하기 전에 `.env` 파일을 만들어주셔야 해요.

```js
// .env 파일

EMAIL='youremail@gmail.com' // 여러분의 이메일 주소
PASSWORD='yourEmailPassword' // 여러분의 이메일 비밀번호
JWT_SECRET='any+secret' // 이건 추측하기 어려운 값을 사용해주세요
```

참고: 깃허브 저장소에 푸시하기 전에 해당 파일을 .gitignore에 추가해 이메일 유출을 방지하세요.

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

## 단계 2: `auth.js` 컨트롤러 설정

이번 단계에서는 `auth.js` 파일에 두 개의 컨트롤러를 생성할 것입니다: 하나는 비밀번호 복구 링크를 생성하고 보내는 기능(`recoverPassword`), 다른 하나는 비밀번호 재설정을 위한 기능(`changePassword`)입니다. 여기서는 사용자 ID를 인코딩한 JSON Web Token (JWT)을 생성합니다. 이 토큰은 사용자에게 보내는 비밀번호 복구 링크에 포함됩니다.

또한 nodemailer를 사용하여 복구 링크가 포함된 이메일을 보냅니다. 이 링크를 통해 사용자는 비밀번호를 재설정할 수 있는 페이지로 이동합니다.

```js
// auth.js

const jwt = require('jsonwebtoken');
const nodemailer = require('nodemailer');
const User = require('./models/User'); // User 모델이 임포트되어 있는지 확인해주세요
const bcrypt = require('bcryptjs');

// 비밀번호 복구 루트
const recoverPassword = async (req, res) => {
  const { email } = req.body;

  const user = await User.findOne({ email });
  if (user) {
    const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, {
      expiresIn: '1hr',
    });

    const link = `http://localhost:3000/auth?token=${token}`;

    const message = `Dear ${user.name},
비밀번호 재설정을 위해 다음 링크를 클릭해주세요: ${link}

보안 상의 이유로, 이 링크는 2분 후에 만료됩니다.
감사합니다.
`;

    let transporter = nodemailer.createTransport({
      service: 'Gmail',
      auth: {
        user: process.env.EMAIL,
        pass: process.env.PASSWORD,
      },
    });

    let mailOptions = {
      from: process.env.EMAIL,
      to: email,
      subject: '비밀번호 복구',
      text: message,
    };

    let info = await transporter.sendMail(mailOptions);
    res.status(200).json({ msg: `이메일 전송됨: ${info.response}` });
  } else {
    res.status(404).json({ msg: '사용자를 찾을 수 없습니다' });
  }
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

이 코드는 사용자로부터 이메일을 입력받아 데이터베이스에서 해당 이메일을 가진 사용자가 있는지 확인한 후, 없다면 오류를 발생시킵니다. 사용자 이메일이 확인된 후에는 사용자 이메일 주소와 ID로 jwt를 서명하여 링크를 생성합니다. 이 링크는 nodemailer 패키지를 사용하여 사용자 이메일로 전송됩니다. nodemailer는 이메일을 보내기 위한 이메일 주소가 필요하며, 여러분의 이메일을 사용하거나 이 멋진 프로젝트를 실행해 보기 위해 새로운 이메일을 만들 수 있습니다 😊.

## 단계 3: 미들웨어

이 미들웨어 함수인 authLink는 사용자가 해당 링크를 클릭할 때 복구 링크에서 토큰을 확인합니다. 토큰을 해독하여 사용자 ID를 검색하고 다음 단계에서 사용하기 위해 req 객체에 첨부합니다. next() 함수는 제어를 다음 미들웨어 또는 라우트 핸들러로 전달하여 비밀번호 재설정 요청을 처리할 것입니다.

```js
// authenticaLink.js
const jwt = require('jsonwebtoken');

const authLink = async (req, res, next) => {
  const {
    query: { token },
  } = req;
  try {
    const payload = jwt.verify(token, process.env.JWT_SECRET);
    req.user = { userId: payload.userId };
    next();
  } catch (error) {
    console.log(error)
  }
};

module.exports = authLink;
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

다음:

## 단계 4: 파싱된 사용자 처리

우리는 두 개의 비밀번호 필드가 모두 입력되었는지 및 일치하는지 확인하여 사용자가 올바른 새 비밀번호를 입력했는지 확인합니다. 새 비밀번호를 저장하기 전에 bcryptjs를 사용하여 해당 비밀번호를 해싱합니다. 이것은 데이터베이스에 평문 비밀번호를 저장하는 것이 중요한 취약점이 될 수 있기 때문에 중요한 보안층을 추가합니다.

이 튜토리얼은 오류 처리에 중점을 두지 않지만, 사용자 경험을 향상시키기 위해 사용자 정의 오류 메시지나 리디렉션을 추가할 수 있습니다.

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

```jsx
//auth.js

const changePassword = async (req, res) => {
  const {
    body: { password1, password2 },
    user: { userId },
  } = req;

  if (!password1 || !password2 || password1 !== password2) {
    throw new Error('Both passwords are required and must match');
  }

  const user = await User.findById(userId);
  if (!user) {
    throw new Error('User not found');
  }

  // Use Bcrypt to encrypt the new password
  const salt = await bcrypt.genSalt(10);
  const hashedPassword = await bcrypt.hash(password1, salt); // Create a new variable for the hashed password
  user.password = hashedPassword;

  await user.save();

  res.status(200).json({ user });
};

auth.js에 있는 모든 코드:

//auth.js

const jwt = require('jsonwebtoken');
const nodemailer = require('nodemailer');
const User = require('./models/User'); // 사용자 모델을 가져오는 것을 확인해주세요

// 비밀번호 찾기
const recoverPassword = async (req, res) => {
  const { email } = req.body;

  const user = await User.findOne({ email });
  if (user) {
    const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, {
      expiresIn: '1hr',
    });

    const link = `http://localhost:3000/auth?token=${token}`;

    const message = `Dear ${user.name},
비밀번호를 재설정하려면 다음 링크를 클릭하세요: ${link}

보안상의 이유로 이 링크는 2분 후에 만료됩니다.
감사합니다.
`;

    let transporter = nodemailer.createTransport({
      service: 'Gmail',
      auth: {
        user: process.env.EMAIL,
        pass: process.env.PASSWORD,
      },
    });

    let mailOptions = {
      from: process.env.EMAIL,
      to: email,
      subject: '비밀번호 변경',
      text: message,
    };

    let info = await transporter.sendMail(mailOptions);
    res.status(200).json({ msg: `이메일 전송: ${info.response}` });
  } else {
    res.status(404).json({ msg: '사용자를 찾을 수 없습니다' });
  }
});

// 비밀번호 변경
const changePassword = async (req, res) => {
  const {
    body: { password1, password2 },
    user: { userId },
  } = req;

  if (!password1 || !password2 || password1 !== password2) {
    throw new Error('비밀번호는 필수이며 일치해야 합니다');
  }

  const user = await User.findById(userId);
  if (!user) {
    throw new Error('사용자를 찾을 수 없습니다');
  }

  // Bcrypt를 사용하여 새 비밀번호를 암호화합니다
  const salt = await bcrypt.genSalt(10);
  const hashedPassword = await bcrypt.hash(password1, salt); // 암호화된 비밀번호를 위한 새 변수 생성
  user.password = hashedPassword;

  await user.save();

  res.status(200).json({ user });
};

module.exports = {
  recoverPassword,
  changePassword,
};

## 단계 5: auth.js 라우트 설정
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

여기서는 패스워드 복구 및 변경 프로세스를 처리할 경로를 정의하고 있습니다. /recover-password 경로는 복구 링크가 포함된 이메일을 트리거하며, /change-password는 새로운 암호를 처리하지만 authLink 미들웨어가 토큰을 확인한 후에만 처리합니다.

주의: 우리는 유효한 토큰을 가진 사용자만이 패스워드를 재설정할 수 있도록 /change-password 경로에 authLink 미들웨어를 적용하였습니다.

```js
const express = require('express');
const router = express.Router();

const {
  recoverPassword,
  changePassword,
} = require('../controllers/auth');
const authLink = require('../middleware/authenticateLink');

router.post('/recover-password', recoverPassword);
router.patch('/change-password', authLink, changePassword);

module.exports = router;
```

## 단계 6: 마지막으로, server.js를 설정합니다.

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

이 마지막 단계에서는 Express 서버를 설정하고 애플리케이션이 사용할 라우트를 정의합니다. authRouter를 가져와서 패스워드 복구 기능을 주요 애플리케이션에 연결합니다. dotenv를 사용하여 환경 변수를로드하여 이메일 자격 증명 및 JWT 비밀을 보안합니다.

```js
//server.js 파일
require('dotenv').config();
require('express-async-errors');

const express = require('express');
const app = express();

// 라우터
const authRouter = require('./routes/auth');

app.use(express.json());

// 라우트
app.use('/api/v1/auth', authRouter);

// Home route
app.get('/', (req, res) => {
  res.status(200).send('Jobs-API에 오신 것을 환영합니다');
});

const port = process.env.PORT || 3000;

const server = async () => {
    app.listen(port, () =>
      console.log(`서버가 포트 ${port}에서 청취 중입니다...`)
    );
};

server();
```

이 튜토리얼에서는 bcryptjs 및 jsonwebtoken을 사용하여 안전한 패스워드 복구 시스템을 성공적으로 구축했습니다. 프로젝트 설정, 복구 이메일 보내기 및 패스워드 재설정을위한 컨트롤러 생성, 및 프로세스를 보호하기위한 미들웨어 사용을 다루었습니다. 이 기능은 더 큰 애플리케이션에 통합할 수 있으며 사용자 정의 오류 처리 또는 추가 보안 조치를 추가하여 보강할 수 있습니다.