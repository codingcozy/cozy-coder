---
title: "requirementstxt가 이제 무쓸모 최신 개발 트렌드 및 대안 소개"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-17 00:13
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "requirementstxt Is Obsolete"
link: "https://medium.com/towards-data-science/ditch-requirements-use-poetry-00a936fe9b6d"
isUpdated: false
---


파이썬의 표준 라이브러리는 사용자에게 다양한 기능을 제공하는 내장 모듈 모음으로, 일상적인 프로그래밍 작업을 수행하는 방식을 표준화하는 것을 목표로 합니다. 몇 가지 예시로는 I/O 작업, 텍스트 처리, 파일 압축, 수학 연산 등이 있습니다. 이 방대한 라이브러리를 통해 개발자들은 추가 패키지를 설치할 필요 없이 다양한 작업을 수행할 수 있습니다.

그러나 표준 라이브러리의 다재다능함에도 불구하고, 현대 파이썬 응용 프로그램은 종종 상자에서 제공되는 내용을 벗어나 더 많은 고급 기능이 필요합니다. 이 때 다양한 팀, 커뮤니티 또는 개인들이 관리하는 방대한 오픈소스 프로젝트들이 중요한 역할을 합니다.

이러한 패키지들은 파이썬 패키지 인덱스(PyPI)에서 사용할 수 있으며, 해당 의존성들은 어떤 개발자라도 쉽게 설치할 수 있습니다. Django나 Flask와 같은 웹 개발 프레임워크부터 Pandas나 Scikit-learn과 같은 데이터 과학 라이브러리까지, 이러한 패키지들은 파이썬 프로그래밍에 필수적인 요소가 되었습니다.

<div class="content-ad"></div>

이러한 종속성을 효과적으로 관리하는 것은 안정적인 릴리스를 유지하고 효율적인 개발 환경을 만드는 데 매우 중요합니다.

# (지원 중단된) requirements.txt 파일

전통적으로, 파이썬 개발자들은 프로젝트 종속성을 나열하는 requirements.txt 파일에 의존해왔습니다. 이 파일은 기본적으로 파이썬 패키지 관리자인 pip를 통해 설치할 수 있는 여러 종속성으로 구성된 파일입니다.

```js
pandas
pytest>=8.0
scikit-learn==2.1.0
django<4
numpy
```

<div class="content-ad"></div>

요구 사항 파일의 각 줄은 설치해야 하는 종속성을 나타냅니다. 또한 선택적으로 버전 제약 조건을 지정할 수 있습니다(PEP-440은 유효한 버전 사양 목록을 제공합니다). 이 접근 방식은 작동하지만 프로젝트가 복잡해지면 한계가 있습니다.

## requirements.txt 파일의 문제점은 무엇인가요?

주요 문제 중 하나는 종속성 자체가 다른 종속성을 가지고 있다는 점에서 발생합니다. 이는 단일 패키지를 설치하면 추가적인 패키지가 연쇄적으로 설치될 수 있다는 것을 의미합니다. 예를 들어, 패키지 A가 패키지 B와 C에 의존하는 경우를 생각해 봅시다. 패키지 A를 설치하면 B와 C도 함께 설치됩니다.

문제는 패키지 A를 제거할 때 발생합니다. 패키지 A가 제거되더라도 패키지 B와 C가 환경에 남아 있을 수 있습니다. 이는 requirements.txt가 부모 패키지가 제거된 후에 더 이상 필요하지 않은 패키지를 추적하지 않기 때문입니다. 결과적으로 사용되지 않는 오래된 종속성인 낡은 종속성이 발생할 수 있으며, 여전히 공간을 차지하거나 보안 취약점이나 충돌을 일으킬 가능성이 있습니다.

<div class="content-ad"></div>

예를 들어, 프로젝트를 위해 초기에 패키지 A를 설치했다고 상상해봅시다. 패키지 A는 패키지 B와 C를 함께 가져왔습니다. 이후에 패키지 A를 더 이상 필요하지 않다고 판단하여 제거하기로 결정했다고 가정해봅시다. 그러나 패키지 B와 C는 여전히 남아 있을 수 있습니다. 따라서 이러한 남은 패키지가 여전히 사용 중인지 아니면 폐기된 것인지 판단하기 어려워집니다.

또 다른 중요한 requirements.txt의 단점은 개발에 필요한 종속성과 프로덕션에 필요한 종속성을 구별할 수 없다는 것입니다. 예를 들어, 테스트를 위한 패키지나 문서 빌드를 위한 패키지가 있을 수 있는데, 이러한 패키지들은 애플리케이션이 프로덕션 환경에서 실행되는 데 반드시 필요하지는 않습니다.

그러나 requirements.txt는 모든 종속성을 함께 나열하기 때문에 개발 전용 패키지와 애플리케이션 자체에 필수적인 패키지를 구분하기 어려워집니다. 이러한 구분 부족은 필요 없는 패키지로 인해 배포 크기와 복잡성이 증가하는 부풀어진 프로덕션 환경으로 이어질 수 있습니다.

이를 해결하기 위해 많은 프로젝트들은 프로덕션을 위한 requirements.txt 파일과 개발을 위한 별도의 requirements.txt 파일을 유지하게 됩니다. 프로덕션 파일은 일반적으로 애플리케이션이 실행되는데 필요한 패키지만 포함하며, 개발 파일에는 테스트, 디버깅 또는 문서 작성과 같은 작업에 필요한 추가적인 패키지가 포함됩니다.

<div class="content-ad"></div>

그러나, 이 방법은 고유한 문제를 도입합니다. 여러 개의 requirements.txt 파일을 관리하면 중복과 일관성 문제가 발생할 수 있습니다. 의존성 버전이 올바르게 유지되는지, 두 파일이 동기화되어 있는지 확인하는 것은 실수를 유발할 수 있고 지루할 수도 있습니다. 게다가, 프로덕션 및 개발 모두에서 필요한 종속성을 처리할 자동 메커니즘이 없어 혼란을 야기할 수 있습니다.

프로젝트가 더 복잡해지면 이러한 문제들이 개발 효율성과 프로젝트 안정성에 상당한 영향을 미칠 수 있습니다. 이것이 고급 도구인 Poetry가 더 강력한 해결책을 제공하는 곳입니다.

# Poetry 소개

Poetry는 Python 프로젝트의 의존성 관리 및 패키징을 처리하는 도구로, requirements.txt의 많은 한계를 해결합니다.

<div class="content-ad"></div>

시, 시는 종속성을 컴파일하고 관리하며 가상 환경을 만들고 동기화하며 PyPI를 통해 패키지를 게시하는 데 사용할 수 있습니다.

## pyproject.toml 파일

이른 시절에는 distutils가 Python 소프트웨어를 빌드하고 배포하는 주요 도구였습니다. 이는 프로젝트 메타데이터 및 설치 요구 사항을 정의하기 위해 setup.py 파일을 사용하여 프로젝트를 패키징하는 기본 프레임워크를 제공했습니다. distutils는 Python 표준 라이브러리의 일부이므로 외부 도구 없이 개발자가 패키지를 빌드하고 배포할 수 있게 했습니다.

생태계가 발전함에 따라 setuptools는 distutils의 기능을 개선한 것으로 나타났으며 더 많은 기능과 유연성을 추가했습니다. setuptools는 의존성을 자동으로 설치하는 기능과 같은 기능을 소개함으로써 distutils를 발전시켰습니다. 그러나 distutils와 setuptools는 여전히 프로젝트의 종속성 목록을 나열하는 별도의 requirements.txt 파일에 의존하고 있었습니다.

<div class="content-ad"></div>

프로젝트 메타데이터와 의존성을 단편화된 관리하는 것이 주요 문제 중 하나였습니다. 설정 및 의존성 목록에 setup.py 및 requirements.txt를 사용하는 것은 종종 일관성이 없고 복잡성이 추가되는 결과를 낳았습니다.

더욱이, 빌드 의존성을 지정하기 위한 setup_requires 인자는 특정한 빌드 의존성을 지정하면, 이는 자신의 의존성을 결정하기 위해 setup.py를 실행해야 하는 "계란이 선표라" 문제를 일으켰으며, 이는 빌드 프로세스를 복잡하게 만들고 필요한 도구의 설치를 지연시키는 결과를 초래했습니다. 이러한 도전에 의해 의존성 관리와 프로젝트 패키징이 번거로워지게 되었습니다.

이에 PEP-518이 개발되었고, Python 프로젝트에서 메타데이터 및 빌드 의존성을 관리하기 위한 새로운 방법이 제안되었습니다. PEP-518은 프로젝트 구성 및 메타데이터를 단일하고 선언적인 형식으로 중앙 집중화하는 pyproject.toml 파일의 개념을 소개했습니다.

이 명세는 프로젝트가 더 간결하고 일관된 방식으로 빌드 시스템 요구사항과 의존성을 정의할 수 있도록 합니다. 이러한 요소들을 하나의 파일로 통합함으로써, PEP-518은 프로젝트 관리를 간소화하고 호환성을 향상시키며, Python 프로젝트의 패키징 및 빌드를 더 현대적이고 효율적인 방식으로 제공할 수 있도록 목표로 했습니다.

<div class="content-ad"></div>

프로젝트의 루트 디렉터리에 위치한 pyproject.toml 파일은 TOML (Tom’s Obvious, Minimal Language) 구문을 사용하여 프로젝트 이름, 버전, 설명, 작성자 및 라이선스와 같은 필수 메타데이터를 통합합니다.

이 파일의 주목할 만한 기능 중 하나는 프로젝트 의존성을 정의할 수 있는 능력입니다. 이를 통해 개발자는 필요한 패키지 및 버전을 지정하여 일관성과 재현성을 유지할 수 있습니다.

또한, pyproject.toml은 extras 개념을 지원하여 선택적 의존성을 정의할 수 있습니다. 이는 pytest와 같은 테스트 도구에 필요한 것과 같은 선택적 의존성을 포함합니다. 표준 메타데이터와 의존성을 넘어서, 이 파일은 써드파티 도구를 위한 사용자 정의 필드 정의도 허용하여 linters 및 formatters와 같은 추가 기능을 지원하는데 확장성을 제공합니다.

다음 섹션에서는 pyproject.toml 파일의 구조를 탐색하고 Poetry와 함께 사용하여 의존성을 더 효율적으로 관리하는 방법을 배워보겠습니다.

<div class="content-ad"></div>

## Poetry를 사용하여 Python 종속성 관리하기

예제 pyproject.toml 파일은 Poetry가 requirements.txt 파일의 한계를 효과적으로 해결하는 방법을 보여줍니다. 이제 우리는 [tool.poetry.dependencies] 및 [tool.poetry.group.dev.dependencies]이라는 종속성의 여러 그룹을 가지고 있어, 필요한 환경에서만 특정 패키지가 설치되도록 분리됩니다.


[tool.poetry]
name = "my_project"
version = "0.1.0"
description = "내 프로젝트 설명"
package-mode = true
authors = [
    "나의 팀 <team@my_company.com>",
]

[tool.poetry.dependencies]
python = "~3.10"
fastapi = "^0.111.1"
pydantic = "^2.8.2"
Jinja2 = "^3.1.2"
uvicorn = {extras = ["standard"], version = "^0.30.3"}

[tool.poetry.group.dev.dependencies]
pytest = "^8.2.2"
black = "^24.3.0"
pre-commit = "^3.8.0"
mypy = "^1.11"
flake8 = "^7.1.0"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"

[tool.flake8]
max-line-length = 99
extend-ignore = "E203"


또한, pyproject.toml은 버전 및 패키징의 일부로 사용되는 메타데이터로 구성됩니다. 이 정보는 팀이 내부 또는 공개 레지스트리에 프로젝트를 배포해야 할 때 매우 중요하며, 다른 사용자들도 설치하고 사용할 수 있도록 해줍니다.

<div class="content-ad"></div>

외부 도구인 flake8와 같은 도구가 사용할 수 있는 추가 메타데이터를 포함할 수 있습니다. 위의 예시에서 flake8는 [tool.flake8] 섹션에 정의된 메타데이터를 사용하여 관련 체크를 실행할 것입니다.

## poetry.lock 파일

Poetry는 pyproject.toml 파일에서 제공된 정보를 읽고, 이를 기반으로 poetry.lock 파일을 생성합니다. 이 lock 파일은 프로젝트 종속성을 관리하는 데 중요한 역할을 합니다.

Poetry install 또는 poetry update를 실행하면 Poetry는 정확한 버전을 포함한 모든 종속성(하위 종속성 포함)의 버전을 기록하기 위해 이 lock 파일을 생성합니다. 이 파일은 프로젝트를 특정 버전으로 잠그는데 도움을 주며, 서로 다른 환경 간의 일관성을 유지하고 "내 컴퓨터에서는 작동한다" 문제를 방지하는 데 도움이 됩니다.

<div class="content-ad"></div>

poetry.lock 파일은 종속성 관리와 관련된 여러 문제를 해결합니다. 먼저, 모든 개발자와 배포 환경이 동일한 종속성 버전을 사용하도록 보장하여 재현성을 보장합니다. 이는 서로 다른 버전이 서로 다른 시간에 설치됨으로 인해 발생할 수 있는 불일치를 제거합니다.

둘째, 종속성의 버전을 잠그는 방식으로 일어날 수 있는 충돌과 호환성 문제를 피하고, 프로젝트가 안정적이고 신뢰할 수 있도록 보장합니다.

# 제가 가장 좋아하는 Poetry의 기능

Poetry는 종속성 관리와 패키지 릴리스를 원활하게 할 수 있는 다양한 기능을 제공합니다.

<div class="content-ad"></div>

개발자로서, 친구 같은 톤으로 번역해 드리겠습니다.

개인적으로 저는 제 모든 프로젝트에서 의존성을 관리하는 데 사용합니다. 예를 들어, 내가 poetry를 사용하여 프로젝트 의존성 및 PyPI에 대한 릴리스를 관리하는 하나의 오픈 소스 패키지인 dbt-airflow를 참고할 수 있습니다. 이렇게 함으로써 패키지가 사용자에게 공개적으로 제공됩니다.

또한 dbt-airflow 패키지 뒤에 숨은 이야기에 대해 더 많이 알고 싶다면, 이 프로젝트의 동기와 구현 세부 정보를 자세히 설명한 최신 게시물 중 하나를 꼭 읽어보세요.

아래에서, 나는 개발 속도를 높이고 가상 환경이 항상 최신 상태임을 보장하기 위해 광범위하게 사용하는 poetry의 가장 매력적인 기능 중 일부를 공유하겠습니다.

## 여러 그룹의 의존성 보유하기

<div class="content-ad"></div>

위에서 강조한 것처럼, 눈에 띄는 기능 중 하나는 서로 다른 종속성 그룹을 유지할 수 있는 능력입니다. requirements.txt 파일과 대조적으로, 이제 테스트용 문맥에서만 사용되는 패키지들을 제품용 코드와 분리하여 유지할 수 있습니다.

특정 그룹에서 종속성을 설치하거나 다른 그룹의 종속성을 제외할지를 선택할 수 있습니다.

예를 들어, 아래 명령어는 테스트 그룹과 문서 그룹에 나열된 종속성을 제외할 것입니다.

```js
poetry install --without test,docs
```

<div class="content-ad"></div>

다음 명령어는 dev 그룹에 나열된 종속성만 설치합니다:

```js
poetry install --only dev
```

## 내부 또는 비공개 소스에서 Python 패키지 설치

개발 팀은 종종 내부 패키지 저장소 또는 아티팩트 레지스트리를 사용하여 PyPI와 같은 공개 패키지 인덱스에서 사용할 수 없는 프로프라이어터리하거나 내부 라이브러리를 호스팅합니다.

<div class="content-ad"></div>

시인은 내부 원천을 사용하는 과정을 단순화하여 pyproject.toml 파일에서 직접 구성할 수 있도록 합니다. 이는 [[tool.poetry.source]] 섹션을 통해 수행되며, 여기에서 내부 저장소의 URL과 우선 순위를 지정할 수 있습니다.

예를 들어, Google Cloud의 Artifact Registry에서 호스트된 개인 저장소에 연결하려면 다음과 같이 구성할 수 있습니다:

```js
[[tool.poetry.source]]
name = "my-aritfact-registry-repo"
url = "https://europe-west2-python.pkg.dev/my-gcpproject/my-af-repo/simple"
priority = "explicit"
```

## 특정 종속성의 세부 정보 개요

<div class="content-ad"></div>

특히 가치 있는 기능 중 하나는 시에로 포함된 시 명령어입니다. 특정 종속성을 더 깊게 파헤쳐야 할 때 — 예를 들어 해당 패키지가 도입하는 다른 패키지를 파악하거나 해당 패키지를 필요로 하는 종속성을 식별해야 할 때 — 이 명령어는 필요한 모든 통찰력을 제공해줍니다.

예를 들어, `$ poetry show 'package-name'`을 실행하면 해당 패키지에 대한 자세한 정보와 종속성, 그리고 해당 패키지를 필요로 하는 다른 패키지 등이 표시됩니다. 이는 프로젝트에서 종속성 트리를 추적하고 이해하는 간단한 방법입니다.

```js
$ poetry show pendulum

name        : pendulum
version     : 1.4.2
description : Python datetimes made easy

dependencies
 - python-dateutil >=2.6.1
 - tzlocal >=1.4
 - pytzdata >=2017.2.2

required by
 - calendar >=1.4.0
```

## 종속성 동기화

<div class="content-ad"></div>

이전에 언급한 대로 requirements.txt 파일과 pip을 사용하는 것으로 발생하는 한 가지 문제는 가상 환경 내에 남아 있는 오래된 종속성일 수 있다는 것입니다.

Poetry는 poetry.lock 파일에 지정된 잠긴 종속성만 환경에 존재하도록 보장하기 위해 종속성 동기화를 지원합니다. 다른 오래된 종속성은 제거됩니다.

이를 달성하려면 설치 명령을 실행할 때 --sync 플래그를 사용하면 됩니다.

```js
poetry install --sync
```

<div class="content-ad"></div>

## 파이썬 프로젝트를 시작하는 방법

포에트리는 새롭게 생성된 파이썬 프로젝트의 보일러플레이트 구조를 만드는 데 사용할 수 있어요.

```js
$ poetry new my-package
```

위의 명령어는 대부분의 프로젝트에 적합한 디렉토리 구조를 생성하며, pyproject.toml 파일과 패키지 소스 코드 및 테스트 스위트를 위한 해당 하위 디렉토리들이 포함돼요.

<div class="content-ad"></div>


my-package
├── pyproject.toml
├── README.md
├── my_package
│   └── __init__.py
└── tests
    └── __init__.py


전반적으로 Poetry는 기능이 다양한 매우 풍부한 CLI 도구를 제공합니다. 명령어 전체 목록은 공식 문서를 참조하시면 됩니다.

# 마지막으로

Python 프로젝트가 더 복잡해지면 효율적이고 유지보수가 쉬운 도구의 필요성도 증가합니다. Poetry는 현대적이고 효율적이며 간소화된 의존성 관리 및 프로젝트 패키징 방법을 제공하여 게임 체인저로 등장했습니다.


<div class="content-ad"></div>

Poetry는 여러 종속성 그룹을 관리하고 내부 패키지 저장소와 통합하며 종속성에 대한 상세한 정보를 제공하고 환경을 동기화하는 기능을 통해 requirements.txt와 setuptools와 같은 전통적인 구조에서 직면한 많은 제한 사항을 해결합니다.