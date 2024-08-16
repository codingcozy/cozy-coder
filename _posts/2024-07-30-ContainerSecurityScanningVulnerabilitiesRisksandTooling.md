---
title: "컨테이너 보안 스캐닝 취약점, 위험 및 도구 모음"
description: ""
coverImage: "/assets/img/2024-07-30-ContainerSecurityScanningVulnerabilitiesRisksandTooling_0.png"
date: 2024-07-30 16:47
ogImage: 
  url: /assets/img/2024-07-30-ContainerSecurityScanningVulnerabilitiesRisksandTooling_0.png
tag: Tech
originalTitle: "Container Security Scanning Vulnerabilities, Risks and Tooling"
link: "https://medium.com/4th-coffee/container-security-scanning-vulnerabilities-risks-and-tooling-31b09f64e6f7"
---


<img src="/assets/img/2024-07-30-ContainerSecurityScanningVulnerabilitiesRisksandTooling_0.png" />

지난 10년 동안 마이크로서비스 아키텍처, 데브옵스 문화 및 Docker, Kubernetes와 같은 인기 있는 도구들의 등장으로 애플리케이션을 컨테이너로 배포하는 것이 점점 더 인기를 얻고 있습니다. 이에 따라 컨테이너 보안은 이전보다 더 중요해지고 있습니다.

이 글에서는 컨테이너 보안에 대해 깊이 살펴볼 것입니다. 어떤 일반적인 컨테이너 취약점이 있으며, 컨테이너 보안 스캔을 통해 위험을 어떻게 감소시킬 수 있는지, 그리고 어떤 인기 있는 컨테이너 보안 도구와 솔루션이 SDLC에 DevSecOps 방식으로 통합될 수 있는지 알아보겠습니다.

자 그럼 시작해보겠습니다.

<div class="content-ad"></div>

# 1 컨테이너 취약점과 컨테이너 보안 스캔 소개

## 1.1 컨테이너 취약점

컨테이너는 템플릿으로 작용하는 컨테이너 이미지를 사용하여 생성됩니다. 이미지에 취약점이 있는 경우, 컨테이너가 해당 취약점을 프로덕션 환경으로 도입할 수 있습니다.

컨테이너 이미지에는 소스 코드 뿐만 아니라 구성, 이진 파일 및 기타 종속성 등이 포함되어 있습니다. 이러한 모든 요소는 취약점을 도입할 수 있습니다. 그렇다면 어떤 컨테이너 취약점이 일반적일까요? 몇 가지를 살펴보겠습니다:

<div class="content-ad"></div>

- 설정 오류 및 하드 코딩된 비밀 정보.
- 애플리케이션 코드의 취약점.
- 이미지에 가져온 보안 라이브러리 또는 종속성으로 인한 취약점(예: 소프트웨어 공급망 공격으로 악성 코드가 도입될 수 있음).

## 1.2 컨테이너 보안 스캔

다행히 대부분의 컨테이너 취약점은 _컨테이너 보안 스캔_을 통해 상대적으로 쉽게 완화할 수 있습니다: 이미지 및 컨테이너를 보안 문제에 대해 분석하는 프로세스입니다. 여러분이 원하신다면, 지속적인 컨테이너 보안 스캔 또는 연속적인 컨테이너 보안은 DevSecOps의 중요한 부분입니다.

그렇다면 컨테이너 보안 스캔은 어떻게 작동할까요? 일반적으로 두 가지 다른 방법이 있습니다:

<div class="content-ad"></div>

- 파일, 패키지 및 의존성 분석하는 중입니다. 예를 들어, 보안 취약한 코딩 방식이나 잘못된 구성을 확인하거나, 하드코딩된 비밀 정보와 유사한 패턴을 찾거나, 응용프로그램의 의존성(타사 라이브러리 및 프레임워크 포함)을 알려진 취약점 데이터베이스와 비교하는 것까지 포함합니다.
- 컨테이너에서 활동을 분석합니다. 예를 들어, 예기치 않은 네트워크 트래픽이나 컨테이너에서 연결된 터미널을 가진 쉘이 생성된 경우 모두 의심스러운 활동으로, 보안 문제를 유발할 수 있습니다.

다음은 이러한 컨테이너 취약점을 더 자세히 살펴보겠습니다: 비밀 정보, 잘못된 구성, 코드 및 의존성에서 발생하는 취약점; 그리고 다양한 접근 방법과 도구가 이러한 위험을 완화하는 데 어떻게 도움이 될 수 있는지에 대해 알아보겠습니다.

# 2. 도커 이미지에서 하드코딩된 비밀 정보 및 잘못된 구성

잘못된 구성 및 하드코딩된 비밀 정보는 보안 수준을 낮추고, 쉽게 악용될 수 있어 새로운 취약점을 도입할 수 있습니다. 공격자는 중요한 시스템에 접근할 수 있고, 때로는 컨테이너 내에서 호스트 머신까지 이탈할 수도 있습니다.

<div class="content-ad"></div>

그래서 최상의 실천 방법으로 도커 이미지에서 미스컨피규레이션을 확인하고 하드코딩된 비밀 정보가 없는지 확인해야 합니다.

이제 몇 가지 인기 있는 도구들을 살펴보겠습니다. 이 도구들은 하드코딩된 비밀 정보와 미스컨피규레이션을 감지할 수 있습니다.

## 2.1 SecretScanner

이름에서 알 수 있듯이 SecretScanner(Deepfence에서 제공)는 비밀 정보를 스캔할 수 있습니다. 독립적으로 작동하는 오픈 소스 도구로, 컨테이너 및 호스트 파일 시스템을 검색하여 다양한 비밀 유형의 데이터베이스와 매칭합니다.

<div class="content-ad"></div>

SecretScanner를 사용하는 방법은 간단합니다. 도커 이미지를 가져오려면 docker pull quay.io/deepfenceio/deepfence_secret_scanner_ce:2.2.0을 실행하면 됩니다. 그리고 간단히 `docker run`을 사용하여 컨테이너 이미지를 스캔할 수 있습니다.

아래에서 실제 동작하는 과정을 확인해보세요.

## 2.2 ggshield

다른 옵션으로 ggshield가 있습니다(오픈소스 및 무료 옵션이 모두 제공됨). 이 도구는 350가지 이상의 하드코딩된 비밀 및 70가지 이상의 인프라스트럭처 코드 보안 구성 오류를 찾아 수정할 수 있습니다.

<div class="content-ad"></div>

패키지 관리자를 통해 간단히 설치할 수 있어요. 예를 들어 macOS의 경우 Homebrew를 사용하여 ggshield를 설치할 수 있어요: brew install gitguardian/tap/ggshield. 그러면 ggshield 비밀 스캔 도커 명령어를 사용하여 로컬 도커 이미지를 스캔할 수 있는 준비가 될 거예요. 이미지의 생성 프로세스(도커 파일 및 빌드 인수)와 이미지 레이어 파일 시스템에 포함된 비밀을 스캔할 수 있어요.

여기 미리보기가 있어요:

![image](/assets/img/2024-07-30-ContainerSecurityScanningVulnerabilitiesRisksandTooling_1.png)

ggshield는 비밀을 스캔하는 것 이상의 다양한 기능을 제공해요. 자세한 내용은 [공식 문서](https://example.com)를 참조해 주세요.

<div class="content-ad"></div>

## 2.3 trivy

또 다른 옵션으로 Aqua Security에서 제공하는 trivy가 있습니다. 이것은 비밀과 구성 오류를 모두 스캔할 수 있는 포괄적이고 다용도 보안 스캐너입니다. 사실, 이는 컨테이너 이미지, 파일 시스템, 원격 git 레포지토리 등을 스캔할 수 있는 포괄적인 도구입니다. (다음 섹션에서 조금 더 다룰 것이지만, 지금은 비밀과 구성 오류 감지 능력에 집중해 보겠습니다.)

이것은 패키지 관리자를 통해 설치할 수 있습니다(예: macOS의 경우 Homebrew를 사용하여 설치할 수도 있습니다: `brew install trivy`). Docker 이미지와 로컬 파일 시스템을 스캔하는 데 사용할 수 있습니다.

예를 들어, 이미지에서 비밀을 스캔하려면 `trivy image --scanners secret IMAGE_NAME` 명령을 실행하면 됩니다. 아래는 깔끔한 결과가 나오는 예시입니다:

<div class="content-ad"></div>

```js
$ trivy image — scanners secret ironcore864/go-hello-http:0.0.1
2024년 06월 23일 20:02:49+08:00 정보 시크릿 스캐닝이 활성화되었습니다
2024년 06월 23일 20:02:49+08:00 정보 스캐닝이 느릴 경우 ‘ — scanners vuln’을 시도하여 시크릿 스캐닝을 비활성화해보세요
2024년 06월 23일 20:02:49+08:00 정보 빠른 시크릿 탐지를 위한 권장 사항은 https://aquasecurity.github.io/trivy/v0.52/docs/scanner/secret/#recommendation 를 참고해주세요
```

Trivy는 인기있는 Infrastructure as Code 파일인 Docker, Kubernetes, Terraform, CloudFormation 등에서 설정 오류를 감지하기 위한 내장된 확인 항목을 제공합니다. 사용자 정의 확인 항목도 정의할 수 있습니다.

로컬에서 Dockerfile을 스캔하는 예시는 다음과 같습니다: 다음 Dockerfile을 사용하여 (Alpine을 베이스 이미지로 사용하여 간단한 Golang 앱을 빌드하는) Dockerfile을 스캔합니다:

```js
FROM alpine
WORKDIR $GOPATH/src/github.com/ironcore864/go-hello-http
COPY . .
RUN apk add git
RUN go get ./… && CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o hello
CMD [“./hello”]
EXPOSE 8080/tcp
```

<div class="content-ad"></div>

다음 명령을 실행하여 구성 오류를 확인하세요(4개를 발견했습니다):

```js
$ trivy fs --scanners misconfig .
2024-06-23T20:05:56+08:00 INFO 구성 오류 스캔이 활성화되었습니다.
2024-06-23T20:05:56+08:00 INFO 감지된 구성 파일 수=1
...
실패: 4 (UNKNOWN: 0, LOW: 1, MEDIUM: 1, HIGH: 2, CRITICAL: 0)

MEDIUM: 'alpine' 이미지의 'FROM' 문에 태그를 지정하세요
'FROM' 문을 사용할 때 이미지가 업데이트될 때 임의로 동작을 피하려면 특정 태그를 사용해야 합니다.
자세한 내용은 https://avd.aquasec.com/misconfig/ds001을 참조하세요

...

HIGH: Dockerfile에 최소 1개의 root가 아닌 사용자와 함께 'USER' 명령을 지정하세요
'root' 사용자로 컨테이너를 실행하는 것은 컨테이너 탈출 상황을 유발할 수 있습니다. 컨테이너를 'root' 사용자가 아닌 사용자로 실행하는 것이 가장 좋습니다. Dockerfile에 'USER' 문을 추가하여 수행할 수 있습니다.
자세한 내용은 https://avd.aquasec.com/misconfig/ds002을 참조하세요

...

HIGH: '--no-cache'가 누락됨: apk add git
'apk add'를 '--no-cache'와 함께 사용하여 패키지 캐시된 데이터를 정리하고 이미지 크기를 줄이세요.
자세한 내용은 https://avd.aquasec.com/misconfig/ds025를 참조하세요

...

LOW: Dockerfile에 HEALTHCHECK 지시어를 추가하세요
실행 중인 컨테이너의 상태를 확인하기 위해 Docker 컨테이너 이미지에 HEALTHCHECK 지시어를 추가해야 합니다.
자세한 내용은 https://avd.aquasec.com/misconfig/ds026을 참조하세요
```

# 3 CVE를 위한 Docker 이미지 스캔

이전 섹션에서는 하드 코딩된 비밀번호가 없고 구성이 완벽한지 확인했습니다. 이제 Docker 이미지에 있는 다른 파일들(운영 체제 패키지, 특정 언어 패키지, 의존성 등)로 넘어갑시다.

<div class="content-ad"></div>

상기 언급한 포괄적인 스캐너 `trivy`는 가장 인기 있는 오픈 소스 보안 스캐너 중 하나이며, 설치된 패키지 버전에 따라 알려진 취약점을 탐지하는 데 사용할 수도 있습니다. 이는 즉, 코드에 의해 호출되는 것인지 여부와 관계없이 이미지 내의 모든 것이 스캔된다는 것을 의미합니다.

예를 들어, 간단한 Python Flask 응용 프로그램을 작성하고 Docker 이미지를 빌드한 다음 `trivy`로 스캔해 보겠습니다.

매우 간단한 Flask "Hello, World!" 앱이 생성되었습니다:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```  

<div class="content-ad"></div>

다음으로 `requirements.txt`에 몇 가지 의존성을 추가합니다:

```js
requests==2.19.1
cryptography==3.3.2
flask
```

알겠지만, Flask 앱 코드에서 사용되지 않는 패키지를 테스트 용도로 추가했습니다.

이제 Dockerfile을 만들어봅시다:

<div class="content-ad"></div>

```js
FROM python:3.9-slim-buster
WORKDIR /app
COPY ./requirements.txt /app
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
ENV FLASK_APP=app.py
CMD [“flask”, “run”, “ —host”, “0.0.0.0”]
```

만약 원하신다면 위의 전체 코드를 얻으려면 저장소를 클론할 수 있습니다.

우리는 이를 빌드하고 태그합니다: `docker build . -t ironcore864/python-insecure-app:0.0.1`, 그런 다음 trivy로 검사합니다:

```bash
$ trivy image --scanners vuln ironcore864/python-insecure-app:0.0.1
2024-06-23T20:50:02+08:00 INFO 취약성 스캐닝이 활성화되었습니다
2024-06-23T20:50:03+08:00 INFO [python] METADATA 분류에서 획득한 라이센스는 추가 조건이 적용될 수 있습니다. 이름="pip" 버전="23.0.1"
2024-06-23T20:50:03+08:00 INFO [python] METADATA 분류에서 획득한 라이센스는 추가 조건이 적용될 수 있습니다. 이름="setuptools" 버전="58.1.0"
2024-06-23T20:50:03+08:00 INFO ...
```

<div class="content-ad"></div>

여러 취약점이 발견되었음을 알 수 있습니다. 일부는 OS 패키지에서 발생한 것이며, 일부는 우리가 추가한 의존성에서 발생한 것입니다. 우리의 코드에서는 직접적으로 해당 패키지를 호출하지는 않지만, 이미지 스캐닝은 OS 패키지 및 의존성을 스캔하여 이미지 내용을 파악하기 때문에 이러한 취약점을 모두 찾아냅니다.

trivy 외에도 언급할 가치가 있는 몇 가지 다른 도구가 있습니다:
- clair: 이미지 내용을 구문 분석하고 해당 내용에 영향을 미치는 취약점을 정적 분석을 통해 보고하는 오픈 소스 어플리케이션입니다.
- grype: 컨테이너 이미지 및 파일 시스템의 취약점 스캐너입니다. 동일 회사인 Anchore의 강력한 SBOM(소프트웨어 자재 목록) 도구인 syft와 함께 작동합니다. SBOM에 대한 자세한 내용은 SBOM 소개를 참고해주세요.

# 4 SCA: 소프트웨어 구성 분석

<div class="content-ad"></div>

다음으로 취약성 스캔에 대한 약간 다른 접근 방식인 소프트웨어 구성 분석 (SCA)에 대해 알아보겠습니다. 이는 구성 요소 및 제3자 라이브러리의 취약점을 자동으로 스캔하고 감지하는 것입니다.

Docker 이미지 스캔과 유사한 것으로 들릴 수도 있지만, 그러면 다음으로 snyk라는 SCA 도구 중 한 가지를 살펴보면서 SCA에 대한 실제적인 느낌을 익히는 것이 좋겠습니다. macOS 사용자는 다음 명령을 실행하여 snyk를 설치할 수 있습니다:

```js
brew tap snyk/tap
brew install snyk
```

이전 섹션에서와 같은 앱을 스캔해 보면:

<div class="content-ad"></div>

```js
git clone https://github.com/IronCore864/python-insecure-app.git
cd python-insecure-app
snyk test .
```

비슷한 결과를 얻어야 합니다:

```js
$ snyk test .

테스팅 중....

알려진 이슈로부터 15개의 종속성을 테스트했고, 4개의 이슈를 발견하였으며, 4개의 취약한 경로를 발견했습니다.


의존성을 업그레이드하여 수정해야 하는 이슈들:

  cryptography@42.0.7의 cryptography@42.0.8로 업그레이드하여 수정
  ✗ Uncontrolled Resource Consumption [낮은 심각도][https://security.snyk.io/vuln/SNYK-PYTHON-CRYPTOGRAPHY-6913422] cryptography@42.0.7에서 도입됨

  jinja2@3.1.3을 jinja2@3.1.4로 고정하여 수정
  ✗ Cross-site Scripting (XSS) [중간 심각도][https://security.snyk.io/vuln/SNYK-PYTHON-JINJA2-6809379] jinja2@3.1.3에서 도입됨

  urllib3@2.2.1을 urllib3@2.2.2로 고정하여 수정
  ✗ Improper Removal of Sensitive Information Before Storage or Transfer (new) [중간 심각도][https://security.snyk.io/vuln/SNYK-PYTHON-URLLIB3-7267250] urllib3@2.2.1에서 도입됨

  werkzeug@3.0.2을 werkzeug@3.0.3으로 고정하여 수정
  ✗ Remote Code Execution (RCE) [높은 심각도][https://security.snyk.io/vuln/SNYK-PYTHON-WERKZEUG-6808933] werkzeug@3.0.2에서 도입됨

...
```

보시다시피 감지된 이슈는 이전 섹션의 `trivy`로도 감지되었지만, `trivy`가 더 많은 이슈를 발견했습니다.

<div class="content-ad"></div>

이것은, 보통 Docker 이미지 스캔과 소프트웨어 구성 분석 사이에는 일부 중복이 있는 것으로 보입니다. 왜냐하면 둘 다 알려진 취약점 데이터베이스와 비교하여 작동하기 때문에 결과에는 몇 가지 중복된 문제가 있습니다.

하지만 Docker 이미지 스캔은 Docker 이미지 내부의 모든 것 (OS 패키지, 종속성 등)을 스캔하는 반면, SCA 도구는 패키지 관리자, 매니페스트 파일, 소스 코드 등을 검사하여 작동합니다. 이는 코드 및 종속성을 분석함으로써 (우리 경우에는 requirements.txt에 정의된) 이미지 내용이 아닌 코드 및 종속성과 관련된 문제를 찾는다는 것을 의미합니다.

결국, 종속성 (우리 경우에는 `requests` 및 `cryptography`)으로 인한 문제는 Docker 이미지 스캔 및 SCA에서 모두 포착되지만 차이점이 있습니다:

- Docker 이미지 스캔은 Docker 이미지를 스캔하지만 먼저 이미지를 빌드해야 합니다. 일반적으로 이는 테스트 환경에 배포하기 위해 CI 파이프라인에서 발생합니다. 반면 SCA는 코드를 스캔하며 이미지가 필요하지 않습니다. 이는 SCA가 SDLC의 초기 단계에서 발생할 수 있지만 Docker 이미지 스캔은 나중에 발생합니다. SCA는 이미지 빌드를 기다릴 필요가 없으며, 이는 좌측으로 이동하게 됨을 의미합니다. 이렇게 하면 SDLC 초반에 문제를 더 빨리 발견할 수 있습니다.
- SCA는 좀 더 이론적인 분석입니다: 당신의 코드에 어떤 일이 발생할 수 있는지에 대한 것이며 그 이상의 것이 아닙니다. 코드 뿐만 아니라 OS 패키지와 같은 코드 이외의 내용이 포함된 이미지도 분석하지 않습니다. 궁극적으로 이미지는 프로덕션 환경에 배포하는 것이지만 따라서 이미지도 스캔하는 것이 중요합니다.

<div class="content-ad"></div>

위의 Docker 이미지 스캔 및 SCA의 장단점을 결합해 보면, SDLC의 서로 다른 단계에서 서로 다른 가치를 제공한다는 것을 알 수 있습니다. 그래서 자동화된 DevSecOps 워크플로에 두 가지를 모두 통합할 수 있습니다.

snyk 이외에도 osv-scanner라는 또 다른 오픈 소스 프로젝트가 있는데, 이는 Go로 작성된 SCA 취약점 스캐너로, osv.dev에서 제공하는 데이터를 사용합니다. osv-scanner에 대한 자세한 정보(및 오픈 소스 보안 일반)는 이 블로그 "오픈 소스 소프트웨어 보안"을 읽어보세요.

# 5 컨테이너 런타임 보안

지금까지 코드/이미지 수준에서 일어나는 Docker 이미지 스캔 및 SCA를 다루었는데, 이를 통해 알려진 취약점을 발견하고 앱이 배포되기 전에 이를 수정할 수 있습니다. 다음으로 컨테이너 런타임 보안이라는 다른 유형의 컨테이너 보안 스캔을 살펴봅시다.

<div class="content-ad"></div>

컨테이너 런타임 보안은 이름에서 알 수 있듯이 런타임 수준에서 발생합니다. 런타임 단계 중에 컨테이너화된 애플리케이션을 보호하기 위해 적극적인 조치와 제어를 취하여 예기치 않은 동작, 구성 변경 및 공격을 거의 실시간으로 감지합니다.

요는 이론적으로 들릴 수 있으니, 하나의 구체적인 예시를 확인해보죠: Falco. Falco는 컨테이너 런타임 보안을 위해 설계된 오픈 소스 도구로, 비정상적인 동작 및 잠재적인 보안 위협을 실시간으로 감지하고 경고하는 기능을 제공합니다.

빠르게 Falco를 시도해보려면, K8s 클러스터를 준비하고 다음 명령을 사용하여 helm을 통해 설치하세요:

```js
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
kubectl create namespace falco
helm install falco -n falco --set driver.kind=ebpf --set tty=true falcosecurity/falco \
--set falcosidekick.enabled=true \
--set falcosidekick.config.slack.webhookurl=$(base64 --decode <<< "aHR0cHM6Ly9ob29rcy5zbGFjay5jb20vc2VydmljZXMvVDA0QUhTRktMTTgvQjA1SzA3NkgyNlMvV2ZHRGQ5MFFDcENwNnFzNmFKNkV0dEg4") \
--set falcosidekick.config.slack.minimumpriority=notice \
--set falcosidekick.config.customfields="user:changeme"
```

<div class="content-ad"></div>

더 자세한 설치 안내서는 공식 문서를 여기서 확인하세요.

Falco가 작동 중이면, 의심스러운 활동을 모의해봐서 이라는 "런타임 보안" 플랫폼이 잡아낼 수 있는지 확인해보겠습니다. 컨테이너를 시작하고 해당 컨테이너로 쉘을 열어보죠. 이 작업은 잠재적으로 위험할 수 있습니다. 왜냐하면 공격자가 컨테이너로 쉘 액세스를 획득하면 다양한 일을 할 수 있기 때문입니다.

다음과 같이 컨테이너를 시작해보세요: kubectl run alpine — image alpine — sh -c "sleep infinity", 그리고 실행 중인 컨테이너로 쉘을 열어 공격자 역할을 할 몇 가지 명령을 실행해보세요: kubectl exec -it alpine — sh -c "uptime" (좋습니다, 이것은 무해한 명령입니다, 하지만 의도를 파악하시겠지요).

만약 Falco의 출력을 확인하면, 다음과 유사한 출력을 얻어야 합니다:

<div class="content-ad"></div>

```js
22:09:57.827858816: 알림: 셸이 실행 중인 컨테이너에 연결하여 생성되었습니다.
…
cmdline=sh -c uptime pid=6461 terminal=34816
… (중략)
```

이는 Falco의 규칙 중 하나가 러닝 컨테이너로 대화식 셸을 실행하는 경우 경보를 발생시키기 때문입니다. 더 많은 표준 규칙에 대해서는 여기서 공식 문서를 참조하십시오. 또한 환경을 안전하게 보호하기 위해 직접 Falco 규칙을 작성하고 사용자 정의할 수도 있습니다.

# 6 요약

이 블로그에서는 컨테이너 취약점의 몇 가지 유형이 있는 것을 보았습니다: 하드코딩된 비밀, 잘못된 구성, 애플리케이션 코드에서 발생하는 취약성, OS 패키지 및 종속성에서 발생하는 취약성이 있습니다.


<div class="content-ad"></div>

다행히도, 대부분의 문제는 도구를 사용하여 상당히 쉽게 감지할 수 있습니다:

- 비밀을 스캔하는 ggshield, SecretScanner
- 잘못된 구성을 스캔하는 trivy
- 알려진 CVE를 스캔하기 위해 도커 이미지를 스캔하는 trivy, clair 및 grype
- snyk 및 osv-scanner와 같은 SCA 도구를 사용하여 소프트웨어 구성을 분석합니다

코드와 이미지를 스캔하는 것 외에도, Falco와 같은 컨테이너 런타임 보안 플랫폼을 사용하여 예기치 않은 동작 및 공격을 미리 정의된 및 사용자 정의 규칙으로 실시간으로 감지할 수 있습니다.

이 블로그의 끝에서 컨테이너 보안 스캔에는 한계가 있음을 강조할 가치가 있습니다. _알려지지 않은_ 취약점을 감지하는 데는 적합하지 않습니다. 일부 취약점이 아직 공개되지 않았거나 새로운 유형의 공격 동작이 발생했는데 런타임 보안 플랫폼의 규칙 목록에 포함되어 있지 않은 경우 이를 감지할 수 없습니다.

<div class="content-ad"></div>

그래서 컨테이너 보안 스캔은 좀 더 안전한 프로덕션 환경으로 나아가는 중요한 단계입니다. 다만, 이것이 완전한 보안 솔루션이 아님을 명심하는 것이 좋습니다.

추가 자료로 OWASP(Open Worldwide Application Security Project)의 Docker Security Cheat Sheet을 참고하시면 컨테이너 보안 체크리스트로 활용할 수 있습니다.

이 글이 유익했기를 바랍니다. 좋아요, 댓글, 구독해 주세요. 다음 글에서 만나요!