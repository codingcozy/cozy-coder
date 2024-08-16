---
title: "웹 애플리케이션 방화벽WAF을 사용해 네트워크 서비스를 보호하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-UsingWebApplicationFirewallWAFtoProtectOurNetworkServices_0.png"
date: 2024-07-09 23:00
ogImage: 
  url: /assets/img/2024-07-09-UsingWebApplicationFirewallWAFtoProtectOurNetworkServices_0.png
tag: Tech
originalTitle: "Using Web Application Firewall(WAF) to Protect Our Network Services"
link: "https://medium.com/codex/using-web-application-firewall-waf-to-protect-our-network-services-91368086e626"
---


<img src="/assets/img/2024-07-09-UsingWebApplicationFirewallWAFtoProtectOurNetworkServices_0.png" />

이전 글에서 SSL/TLS 프로토콜을 사용하여 전체 통신 프로세스를 암호화하여 악의적인 도청 및 변경을 방지하여 데이터 보안을 보호하는 HTTPS에 대해 논의했습니다.

그러나 HTTPS는 통신 링크의 보안만을 보장하여 제3자가 전송된 내용을 알 수 없게하는 것으로, 통신 링크의 끝점 인 클라이언트와 서버에서의 보호를 제공하지 않습니다.

HTTP는 공개 프로토콜이며 웹 서비스는 공개 인터넷에서 운영되므로 해커의 타깃이 될 수밖에 없습니다.

<div class="content-ad"></div>

또한, 해커들은 우리가 상상하는 것 이상의 기술을 보유하고 있습니다. 데이터 전송 중에 데이터를 조작할 수는 없지만 시스템에 접근하기 위해 합법적인 사용자로 "가장할" 수 있고 기회를 기다려 손해를 입힐 수 있습니다.

# 웹 서비스가 직면한 위협

해커들은 웹 서비스를 어떻게 공격할까요? 일반적인 공격 방법을 알아봅시다.

첫 번째로 "DDoS" 공격 (분산 서비스 거부 공격)이라고도 불리는 "플러드 공격"이 있습니다.

<div class="content-ad"></div>

해커들은 다수의 "좀비" 컴퓨터를 제어하여 대상 서버로 유효하지 않은 많은 요청을 발사합니다. 서버는 일반 사용자와 해커를 구별할 수 없기 때문에 "모든 요청을 수락"해야만 하며, 이로 인해 일반 사용자를 위한 자원을 소모하게 됩니다. 공격의 강도가 높으면 웹 사이트의 서비스 능력을 압도하여 대역폭, CPU 및 메모리를 고갈시켜 웹 사이트가 정상적인 서비스를 제공할 수 없게 만들 수 있습니다.

DDoS 공격은 비교적 "둔감"하며 효과적이지만 HTTP 프로토콜의 복잡성을 포함하지 않아 "기술적 콘텐츠"가 비교적 낮습니다. 그러나 아래에 설명된 방법은 다릅니다.

웹 서비스는 종종 비즈니스 응용프로그램에 대한 HTTP 메시지에서 다양한 정보를 추출하지만 때로는 엄격한 검사가 부족합니다. HTTP 프로토콜은 의미론적으로 느슨하고 유연하기 때문에 URI의 쿼리 문자열, 헤더 필드 및 본문 데이터를 임의로 설정하여 보안 위험을 초래하고 해커가 "코드 삽입" 가능성을 제공할 수 있습니다.

해커들은 HTTP 요청 메시지를 작성하여 서버로 보낼 수 있습니다. 서비스 애플리케이션이 충분히 준비되어 있지 않으면 해커의 코드를 실행하도록 "속일" 수 있습니다.

<div class="content-ad"></div>

"SQL injection( SQL 인젝션)"은 아마도 가장 유명한 "코드 인젝션" 공격 중 하나일 겁니다. 이 공격은 서버 문자열 연결의 취약점을 이용하여 SQL 문을 형성하고 데이터베이스에서 민감한 정보에 접근하는 비정상적인 SQL 쿼리를 만듭니다.

또 다른 방법으로 "HTTP 헤더 인젝션( HTTP 헤더 인젝션)"이 있습니다. 이 공격은 "Host", "User-Agent", "X-Forwarded-For"와 같은 필드에 악성 데이터나 코드를 삽입하는 원리를 따릅니다. 서버측 응용 프로그램이 이러한 필드를 부적절하게 구문 분석하면 악성 코드가 실행될 수 있습니다.

또한 "쿠키"를 악용하는 방법도 있습니다. 이를 "크로스사이트 스크립팅(XSS) 공격"이라고합니다. 이 유형의 공격은 "JS 코드 인젝션( JS 코드 인젝션)"을 포함하며 JavaScript 스크립트를 사용하여 보호되지 않은 쿠키를 얻는 데 사용됩니다.

# 웹 응용 프로그램 방화벽

<div class="content-ad"></div>

너무 많은 해킹 방법에 어떻게 대처해야 할까요?

여기서 "웹 애플리케이션 방화벽" (WAF)이 필요합니다.

전통적인 "방화벽"에 대해 더 익숙하실 수도 있습니다. 전통적인 방화벽은 외부 네트워크와 내부 네트워크를 분리하는 제 3 또는 제 4 계층에서 작동합니다. 미리 설정된 규칙을 사용하여 특정 IP 주소와 포트 번호만 통과시키고, 기준을 충족하지 않는 데이터 흐름을 거부합니다. 본질적으로 그들은 네트워크 데이터 필터링 장치로 작동합니다.

WAF도 "방화벽"이지만, 제 7 계층에서 작동합니다. IP 주소와 포트 번호 뿐만 아니라 전체 HTTP 메시지를 확인할 수 있습니다. 이를 통해 더 심층적이고 상세한 메시지 내용 검사를 수행하며, 더 복잡한 조건과 규칙을 사용하여 데이터를 필터링할 수 있습니다.

<div class="content-ad"></div>

간단히 말해, WAF는 "HTTP 침입 탐지 및 방지 시스템"입니다.

![WAF image](/assets/img/2024-07-09-UsingWebApplicationFirewallWAFtoProtectOurNetworkServices_1.png)

WAF가 할 수 있는 일은 무엇인가요?

일반적으로 WAF로 간주되기 위해 제품은 다음과 같은 기능을 가지고 있어야 합니다:

<div class="content-ad"></div>

- IP 블랙리스트와 화이트리스트: 블랙리스트에 있는 주소로부터의 액세스를 거절하거나 화이트리스트에 있는 사용자만 액세스를 허용합니다.
- URI 블랙리스트와 화이트리스트: IP 블랙리스트와 화이트리스트와 유사하게, 특정 URI에 대한 액세스를 허용하거나 거부합니다.
- DDoS 공격 방어: 특정 IP 주소에 대해 연결 및 속도를 제한합니다.
- 요청 메시지 필터링: "코드 주입" 공격에 대한 방어 조치를 취합니다.
- 응답 메시지 필터링: 민감한 정보 유출을 방지합니다.
- 감사 로그: 감지된 침입 작업을 모두 기록합니다.

WAF는 복잡해 보일 수 있지만, 원리를 이해하면 그리 어렵지 않아요.

프로그램을 작성할 때 매개변수를 확인하는 것과 비슷합니다. WAF는 HTTP 요청 및 응답 메시지를 받아와 문자열 처리 함수를 사용하여 키워드나 민감한 단어를 확인하거나 패턴 일치를 위해 정규식을 사용합니다. 규칙이 일치하면 403/404 상태 코드를 반환하는 등 해당 조치를 실행합니다.

Apache, Nginx 또는 OpenResty에 익숙하다면, 구성 파일을 수정하고 일부 JS 또는 Lua 코드를 작성하여 기본적인 WAF 기능을 구현할 수 있어요.

<div class="content-ad"></div>

예를 들어, Nginx에서 IP 주소 블랙리스트를 구현하려면 "map" 지시문을 사용하여 변수 $remote_addr에서 IP 주소를 가져와 블랙리스트에 있는 경우 값 1로 매핑한 다음 "if" 지시문을 사용하여 확인할 수 있습니다:

```js
map $remote_addr $blocked {
    default       0;
    "1.2.3.4"     1;
    "5.6.7.8"     1;
}
 
 
if ($blocked) {
    return 403 "you are blocked.";  
}
```

Nginx 구성 파일은 정적으로로드되어 변경 사항을 적용하려면 Nginx를 다시 시작해야 하기 때문에 불편할 수 있습니다. OpenResty로 전환하면 훨씬 쉬워집니다. 액세스 단계에서 확인을 수행하고 IP 주소 목록은 Redis 또는 MySQL과 같은 외부 데이터베이스에 코소켓을 사용하여 동적 업데이트할 수 있습니다:

```js
local ip_addr = ngx.var.remote_addr
 
local rds = redis:new()
if rds:get(ip_addr) == 1 then 
    ngx.exit(403) 
end
```

<div class="content-ad"></div>

위의 예시를 보고 나면, 당신도 자신만의 WAF(웹 애플리케이션 방화벽)을 개발하고 싶은 욕구를 느낄 수도 있을 것입니다. 그러나, 네트워크 보안 분야에서 "버킷 효과" (또는 "가장 짧은 널판이론"으로 알려짐)에 대해 상기시켜 드려야 합니다. 웹사이트 전체의 보안은 당신이 강화한 가장 강력한 부분이 아니라, 심지어 알지 못할 수도 있는 가장 약한 링크에 의해 결정됩니다. 해커들은 종종 "가장 쉬운 길"을 택합니다. 한 번에 웹사이트의 단일한 취약점을 찾으면 그 부분을 통해 들어가 다른 모든 보안 조치들을 효과 없게 만들 수 있습니다.

그러므로 WAF를 사용할 때에는 "바퀴를 재발명하는" 대신, 실제 시나리오에서 시험된 기존의 완성도 있는 WAF 제품을 사용하는 것이 가장 좋습니다.

# 포괄적인 WAF 솔루션

여기에서, WAF 분야에서 최고 수준의 제품 중 하나인 ModSecurity를 소개해 드리겠습니다. 이 제품은 종종 WAF 산업의 사실상의 표준으로 인식되며, 실제 시나리오에서 테스트된 제품입니다.

<div class="content-ad"></div>

ModSecurity은 장기간의 역사로 Nginx보다 여러 해 앞서가는 생산용 WAF 툴킷인 오픈소스입니다. 프라이빗 프로젝트로 시작되어 나중에 상업 회사 Breach Security에 인수되었고, 현재는 TrustWave의 SpiderLabs 팀에 의해 유지보수되고 있습니다.

초기에는 ModSecurity가 아파치 모듈이었기 때문에 아파치에서만 실행할 수 있었습니다. 그러나 우수한 품질과 인기로 인해 나중에 2.x 버전에서 Nginx와 IIS를 지원하기 시작했습니다. 그러나 기반이 되는 아키텍처 차이로 인해 이 지원은 안정성이 떨어졌습니다.

최근 몇 년간 SpiderLabs 팀은 아파치 아키텍처에 대한 종속성을 제거하는 새로운 3.0 버전을 개발했습니다. 대신 새로운 "커넥터"를 사용하여 아파치나 Nginx와 원활하게 통합되어 버전 2.x에 비해 향상된 안정성, 속도 및 낮은 오겹도 양성율을 제공합니다.

ModSecurity에는 두 가지 주요 구성 요소가 있습니다. 첫 번째는 "rules engine"으로 특정 구문을 사용하는 사용자 정의 "SecRule" 언어를 구현합니다. 처음에는 정규 표현식을 기반으로 했지만, 스크립팅 기능을 제공하는 Lua로 확장되어 보다 유연한 구성을 제공합니다.

<div class="content-ad"></div>

ModSecurity의 규칙 엔진은 C++11로 구현되어 있으며 GitHub에서 소스 코드로 다운로드할 수 있어 Nginx에 통합할 수 있습니다. 그러나 컴파일하는 데 시간이 걸릴 수 있으므로 권장하는 방법은 동적 모듈로 컴파일하여 구성 파일에서 "load_module" 지시문을 사용하여로드하는 것입니다:

```js
load_module modules/ngx_http_modsecurity_module.so;
```

엔진만으로는 충분하지 않습니다. 엔진을 작동시키기 위해서는 견고한 방어 규칙이 필요합니다. 따라서 ModSecurity의 두 번째 중요한 구성 요소는 "규칙 집합"입니다.

ModSecurity 소스 코드에는 "modsecurity.conf-recommended"이라는 기본 규칙 구성 파일이 포함되어 있습니다. 이 파일을 사용하기 전에 확장자를 ".conf"로 변경해야 합니다.

<div class="content-ad"></div>

규칙 집합이 설정되면 Nginx 설정 파일에 로드하여 규칙 엔진을 활성화할 수 있습니다:

```js
modsecurity on;
modsecurity_rules_file /path/to/modsecurity.conf;
```

ModSecurity의 전체 보호 기능을 활성화하려면 "modsecurity.conf" 파일에서 "SecRuleEngine" 지시문을 "On"으로 조정해야 합니다. 이렇게 하면 침입 방지 기능이 활성화됩니다:

```js
#SecRuleEngine DetectionOnly
SecRuleEngine On
```

<div class="content-ad"></div>

보다 포괄적인 웹사이트 보호를 위해 ModSecurity가 제공하는 규칙 세트는 "OWASP ModSecurity Core Rule Set" (CRS)로 알려져 있으며 "Open Web Application Security Project ModSecurity Core Rule Set"이라는 공식 명칭이 있습니다. 이 최신 규칙 세트는 종종 "Core Rule Set" 또는 "CRS"로 줄여서 표기됩니다. 

![이미지](/assets/img/2024-07-09-UsingWebApplicationFirewallWAFtoProtectOurNetworkServices_2.png)

이 CRS는 완전히 오픈 소스이며 GitHub에서 무료로 다운로드할 수 있습니다.

```js
git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
```

<div class="content-ad"></div>

그 중 하나는 "crs-setup.conf.example" 파일인데, 이는 CRS의 기본 구성을 담고 있습니다. "modsecurity.conf" 파일에 "Include" 지시문을 사용하여 이를 포함시킬 수 있고, 그 후에 "rules" 섹션에서 다양한 규칙을 추가할 수 있습니다.

```js
Include /path/to/crs-setup.conf
Include /path/to/rules/*.conf
```

관심이 있다면 이 구성 파일들을 확인해보세요. 이들은 "SecRule"을 사용하여 다양한 규칙을 정의하며, 일반적으로 "SecRule 변수 연산자 작업" 형식으로 작성됩니다. 하지만 ModSecurity의 구문은 다소 복잡하며 완전히 이해하는 데 시간이 걸리므로, 여기서는 자세한 설명을 하지 않겠습니다.

게다가, ModSecurity에는 사후 오프라인 분석을 위해 의심스러운 데이터를 기록하는 강력한 감사 로그 기능이 있습니다. 그러나, 공격이 많은 프로덕션 환경에서는 이러한 로그가 신속하게 디스크 공간을 차지하고 디스크 쓰기 작업으로 Nginx 성능에 영향을 미칠 수 있습니다. 따라서 일반적으로 이를 비활성화하는 것이 권장됩니다.

<div class="content-ad"></div>

```js
SecAuditEngine off  #RelevantOnly
SecAuditLog /var/log/modsec_audit.log
```

# 결론

오늘은 다양한 해커 공격으로부터 웹 서비스를 보호하는 "웹 애플리케이션 방화벽" (WAF)에 대해 배웠습니다. 이러한 공격은 DDoS 및 코드 인젝션과 같이 공개 인터넷에서 서비스를 운영할 때 자주 발생합니다.

WAF에 대한 중요한 포인트는 다음과 같습니다:

<div class="content-ad"></div>

- 공개 인터넷에서 실행되는 웹 서비스는 DDoS 공격 및 코드 인젝션과 같은 공격에 취약하여 정상 서비스 운영을 방해할 수 있습니다. 따라서 보호 조치가 필수적입니다.
- WAF는 Layer 7에서 작동하는 "HTTP 침입 탐지 및 방지 시스템"으로, 웹 서비스에 포괄적인 보호를 제공합니다.
- ModSecurity는 오픈 소스의 프로덕션 등급 WAF 제품입니다. 핵심 구성 요소는 "규칙 엔진"과 "규칙 세트"로, 백신 엔진 및 바이러스 시그니처 데이터베이스와 유사합니다.
- WAF는 주로 패턴 매칭과 데이터 필터링을 통해 작동하며 CPU 자원을 소비하고 계산 오버헤드를 추가할 수 있어 서비스 능력을 감소시킬 수 있습니다. 따라서 WAF를 배포하려면 보안과 성능 사이의 균형을 찾아야 합니다.

![이미지](/assets/img/2024-07-09-웹애플리케이션방화벽WAF를통한네트워크서비스보호_3.png)