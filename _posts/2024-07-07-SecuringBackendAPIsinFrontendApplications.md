---
title: "프론트엔드 애플리케이션에서 백엔드 API 보안을 강화하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-SecuringBackendAPIsinFrontendApplications_0.png"
date: 2024-07-07 13:37
ogImage: 
  url: /assets/img/2024-07-07-SecuringBackendAPIsinFrontendApplications_0.png
tag: Tech
originalTitle: "Securing Backend APIs in Frontend Applications"
link: "https://medium.com/@cvinothkumar/securing-backend-apis-in-frontend-applications-5194b6250cb6"
isUpdated: true
---





웹 개발의 세계는 프론트엔드 애플리케이션과 백엔드 API 간의 원활한 상호 작용에 기반을 두고 있습니다. 매력적인 사용자 인터페이스가 최전방을 장식하는 동안, 강력한 API는 뒷면에서 마법을 연출합니다. 그러나 위대한 힘에는 큰 책임이 따르며, 백엔드 API를 보호하는 것이 중요합니다.

이 글은 노출된 API를 피해를 입히는 취약성을 탐구하고, 프론트엔드 애플리케이션 내에서 이를 강화하기 위한 모범 사례를 제시합니다. 또한 코드 조각을 통해 구체적인 기술을 탐구하고, 프록시 서버를 활용하는 혜택을 알아볼 것입니다.

노출된 API의 내재적인 취약성

<div class="content-ad"></div>

프론트엔드 앱은 설계상 클라이언트 측에서 작동하여 누구나 웹 브라우저로 액세스할 수 있습니다. 이 투명성으로 동적 사용자 경험을 가능하게하지만, 백엔드로의 API 호출을 드러내기도 합니다. 악의적인 사용자가 잠재적으로:

- API 요청 및 응답을 가로챗다: 네트워크 트래픽을 도청함으로써 공격자들은 중요한 데이터를 도난하거나 응용 프로그램을 조작하기 위한 요청을 위조할 수 있습니다.
- API 엔드포인트를 역공학으로 찾아낸다: API 호출의 구조를 분석함으로써 공격자들은 문서화되지 않은 기능을 발견하거나 취약점을 이용할 수 있습니다.

백엔드 API를 안전하게 보호하는 최선의 방법

프론트엔드에서 API를 완전히 숨기는 것이 이상적으로 보일 수 있지만 항상 실행할 수 있는 것은 아닙니다. 프론트엔드 애플리케이션 내에서 방어층을 추가하는 방법과 일부 주요 실천 사례에 대한 코드 샘플은 다음과 같습니다:

<div class="content-ad"></div>

프록시 서버를 통한 보안 강화:

프론트엔드 응용 프로그램과 백엔드 API 서버 사이에 중개 역할을 하는 프록시 서버를 도입하는 것을 고려해보세요. 이를 통해 보안에 추가적인 층이 추가됩니다:

- API 호출 제한 외부로 분리: 프록시 서버는 원본 확인 및 속도 제한과 같은 작업을 처리하여 백엔드 서버에 부담을 줄일 수 있습니다.
- 백엔드 위치 숨김: 프록시를 통해 트래픽을 전송함으로써, 백엔드 인프라의 실제 위치를 숨길 수 있어 공격자가 직접 공격하기 어렵게 만들 수 있습니다.
- 중앙 집중식 보안 관리: 프록시 서버는 보안 정책을 시행하고 API 트래픽을 모니터링하는 중심 장소가 될 수 있습니다.

클라우드 기반 속도 제한:

<div class="content-ad"></div>

AWS, CloudFlare 및 Imperva와 같은 인프라 제공 업체들은 클라우드 기반 솔루션을 제공하여 비정상적으로 효과적일 수 있는 속도 제한 기능을 제공합니다. 이러한 서비스는 다음과 같은 기능을 제공합니다:

- 세부적인 제어: IP 주소, API 엔드포인트 또는 여러 요소를 기반으로 속도 제한을 정의할 수 있습니다.
- 확장성: 클라우드 기반 솔루션은 사용자 지정 속도 제한 메커니즘보다 훨씬 효율적으로 트래픽 증가를 처리할 수 있습니다.
- 실시간 분석: API 사용 패턴을 인식하고 잠재적 위협을 식별할 수 있는 통찰력을 제공합니다.

다음은 CloudFlare의 속도 제한 기능을 통합하는 개념적 예시입니다 (구체적인 구현 방법은 선택한 서비스에 따라 달라질 수 있습니다):

- CloudFlare 구성: CloudFlare 대시보드에서 IP 주소, API 엔드포인트 또는 시간대별로 한정된 규칙을 설정하여 속도 제한 규칙을 설정합니다.
- 프론트엔드 통합: 일반적으로 프론트엔드 측에서 코드 변경은 필요하지 않습니다. CloudFlare는 설정한 규칙을 기반으로 자동으로 속도 제한을 처리합니다.

<div class="content-ad"></div>

원산지별 API 호출 제한

인프라 및 백엔드 팀은 예상된 프론트엔드 도메인에서의 API 호출만을 제한하기 위해 함께 작업할 수 있습니다. 다음은 백엔드에서 Node.js를 사용한 예시입니다:

```js
const express = require('express');
const cors = require('cors');
const app = express();
// CORS 구성하여 'frontend.com' 원산지에서의 요청만 허용
const corsOptions = {
 origin: 'https://frontend.com'
}
app.use(cors(corsOptions));
// API 엔드포인트 (실제 API 로직으로 대체)
app.get('/api/data', (req, res) => {
 // … 요청 처리 및 데이터 전송
});
app.listen(3000, () => console.log('서버가 3000번 포트에서 수신 대기 중'));
```

이 코드 스니펫은 cors 미들웨어를 활용하여 'https://frontend.com'에서만 발신된 요청을 허용합니다. 실제 프론트엔드 도메인으로 교체하는 것을 잊지 마세요.

<div class="content-ad"></div>

시간 제한 요청과 암호화된 헤더

프론트엔드 개발팀은 현재 타임스탬프, 타임존 정보, 그리고 정적 키를 포함하는 암호화된 값을 생성할 수 있습니다. 이 값은 요청 헤더에 포함됩니다. 그런 다음 백엔드 팀은 동일한 정적 키를 사용하여 값을 복호화하고 요청이 특정 시간 범위 내에 있는지 확인할 수 있습니다 (예: 지난 5분 내에 있는지).

다음은 JavaScript를 사용한 간단한 예제입니다 (실제 사용 시에는 암호화에 안전한 라이브러리를 사용해야 합니다):

이것은 참고용 기본적인 예제입니다. 실제 응용 프로그램에서는 암호화와 복호화에 안전한 라이브러리를 사용해야 합니다.

<div class="content-ad"></div>

프론트엔드 (자바스크립트):

```js
function createEncryptedTimestampHeader(staticKey) {
 const timestamp = Date.now();
 const timezoneOffset = new Date().getTimezoneOffset();
 const data = JSON.stringify({ timestamp, timezoneOffset });
 const encryptedData = /* 여기에 안전한 암호화 라이브러리를 사용하세요 */;
 return { 'X-Time-Bound': encryptedData };
}
```

백엔드 (Node.js):

```js
function decryptTimestampHeader(encryptedData, staticKey) {
 const decryptedData = /* 여기에 안전한 복호화 라이브러리를 사용하세요 */;
 const { timestamp, timezoneOffset } = JSON.parse(decryptedData);
 const requestTime = timestamp + timezoneOffset * 60 * 1000;
 const currentTime = Date.now();
 return (currentTime - requestTime) <= (5 * 60 * 1000); // 5분 내에 있는지 확인
}
```

<div class="content-ad"></div>

기억해 주세요: 이것들은 몇 가지 예시일 뿐이며, 구체적인 구현은 요구사항, 선택한 기술 및 인프라에 따라 다를 수 있습니다.

결론

프론트엔드 응용 프로그램 내에서 백엔드 API를 안전하게 보호하기 위해서는 다층 접근 방식이 필요합니다. 위에 제시된 전략을 시행함으로써, 악의적 공격으로부터 응용 프로그램과 사용자를 안전하게 지킬 수 있는 견고한 방어 체계를 구축할 수 있습니다. 추가 보안 혜택을 위해 프록시 서버를 포함시키고, 확장 가능성과 효율적인 트래픽 관리를 위해 클라우드 기반의 속도 제한 솔루션을 활용하세요. 보안은 지속적인 과정임을 기억해 주세요. 지속적인 감시와 개선이 견고하고 신뢰할 수 있는 응용 프로그램을 유지하는 데 필수적입니다.