---
title: "AWS CodeArtifact로 Android Java 또는 Kotlin 라이브러리 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_0.png"
date: 2024-07-06 11:11
ogImage: 
  url: /assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_0.png
tag: Tech
originalTitle: "Publishing Android, Java or Kotlin Libraries with AWS CodeArtifact"
link: "https://medium.com/proandroiddev/publishing-android-java-or-kotlin-libraries-with-aws-codeartifact-complete-guide-2fc4aca7fef9"
---


카드 아티팩트를 사용하여 안드로이드, 자바 또는 코틀린 라이브러리를 안전하게 배포하고 사용하고 싶다면, AWS CodeArtifact가 강력한 솔루션을 제공합니다. CodeArtifact는 아마존 웹 서비스(AWS)가 제공하는 완전히 관리되는 아티팩트 저장소 서비스로, 소프트웨어 패키지를 저장, 정리 및 공유할 수 있는 중앙 위치를 제공합니다.

# 저장 및 요청

<div class="content-ad"></div>

**CodeArtifact 요금**

코드아티팩트(CodeArtifact)에 아티팩트를 저장하는 데 드는 비용은 아티팩트 저장 용량(GB) 및 한 달 동안의 저장 기간에 따라 결정됩니다. 코드아티팩트로 수행된 요청에 대한 요금은 모든 요청 유형(npm 레지스트리, Maven Central, PyPI, NuGet.org 등)에 일관되게 부과되며 요청 수를 기준으로 요금이 부과됩니다. 요금은 요청 양에 따라 결정됩니다. 요금은 10,000개의 요청당 0.05달러로 책정됩니다.

AWS 무료 사용 티어의 일환으로 매월 처음 2GB의 저장 공간 및 최초 100,000개의 CodeArtifact 사용 요청은 무료로 제공됩니다.

# 설정 전

Android, Java 또는 Kotlin 라이브러리를 게시하려면 적절한 권한이 있는 CodeArtifact 리포지토리가 AWS 콘솔에 생성되어 있어야 합니다.

<div class="content-ad"></div>

코드 아티팩트를 설정하려면 다음 단계가 필요합니다.

## 단계:

- AWS 콘솔에서 아티팩트 저장소 설정
- AWS CLI 설치 및 구성
- Android 코드 통합하여 라이브러리를 가져오고 푸시하기

## 토큰 생성:

<div class="content-ad"></div>

- **코드아티팩트 토큰 생성 및 갱신하기**
- **GetAuthorizationToken API 사용 중 발생하는 일반 문제점과 해결책**

## 단계 1: AWS에서 Artifact Repository 설정하기

시작하기 전에 AWS 계정이 있는지 확인하세요. 계정이 없다면 [AWS 공식 페이지](https://aws.amazon.com/)에서 쉽게 생성할 수 있습니다.

AWS 콘솔에 로그인한 후 IAM 대시보드로 이동하세요. IAM은 "보안, 식별 및 규정 준수" 섹션 아래에서 찾을 수 있습니다. IAM 대시보드에서 IAM에 대한 개요를 볼 수 있습니다. 여기에서 사용자, 그룹, 역할 및 정책을 관리할 수 있습니다. IAM 대시보드에서 왼쪽 탐색 창에서 "사용자"를 클릭하세요. "사용자 추가" 버튼을 클릭하세요.

<div class="content-ad"></div>

/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_1.png

<p>기존 정책 또는 사용자 정의 정책을 첨부하여 권한을 설정하세요.</p>

<p>사용자를 생성한 후 사용자 섹션으로 이동하여 새로 생성된 사용자를 클릭하세요. 우측에 있는 "액세스 키 생성" 링크를 클릭하여 사용자의 액세스 키 ID 및 비밀 액세스 키를 생성하고 명령 줄 인터페이스 (CLI) 용도를 선택하세요. 이 자격 증명은 프로그래밍 방식으로 접근하는 데 필요하므로 다운로드하거나 복사해 두세요.</p>

/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_2.png

<div class="content-ad"></div>

**이미지 추가:** 
![2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_3.png](/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_3.png)

**필수 권한으로 저장소 및 도메인 생성하기**

AWS 관리 콘솔에서 상단의 검색 바를 사용하거나 "Developer & Tools"로 이동하여 "CodeArtifact"를 선택합니다.

CodeArtifact 대시보드에서 "Create repository" 버튼을 클릭하세요.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_4.png)

필수 정보를 입력해주세요:

- 저장소 이름: 저장소에 대해 고유한 이름을 입력하세요.
- 도메인: 기존 도메인을 선택하거나 새로 생성하세요.
- 도메인 소유자: "AWS" 또는 "External" 중 하나를 선택하세요.

CodeArtifact 도메인을 통해 많은 저장소를 보다 쉽게 관리할 수 있습니다. 이는 여러 AWS 계정에 속한 다양한 저장소에 걸쳐 권한을 설정하는 데 도움이 됩니다. 도메인 내에서는 자산이 한 번만 저장되며 여러 저장소에서 접근 가능한 경우에도 해당됩니다.

<div class="content-ad"></div>

**권한 및 정책**

CodeArtifact에서 CodeArtifact 및 sts:GetServiceBearerToken에 대한 리스트, 생성, 삭제, 업데이트 권한을 부여하기 위해서는 도메인 및 리포지토리 권한을 모두 부여해야 합니다.

**도메인 정책 적용 또는 편집**

정책을 편집하려면 CodeArtifact 대시보드로 이동하여 권한을 부여하려는 도메인을 선택해야 합니다. 도메인이 없는 경우 새 도메인을 만들고 정책을 적용하세요. 도메인 세부 정보 페이지에는 "도메인 정책 편집" 버튼이 있습니다.

<div class="content-ad"></div>

웹 페이지를 수정할 수 있는 페이지가 표시되는데, 이것은 AWS Identity and Access Management (IAM) 정책을 JSON 형식으로 작성한 것이며, 주로 AWS CodeArtifact와 관련된 권한을 관리하기 위한 것입니다.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ContributorPolicy",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::444455556666:root"
            },
            "Action": [
                "codeartifact:CreateRepository",
                "codeartifact:DeleteDomain",
                "codeartifact:DeleteDomainPermissionsPolicy",
                "codeartifact:DescribeDomain",
                "codeartifact:GetAuthorizationToken",
                "codeartifact:GetDomainPermissionsPolicy",
                "codeartifact:ListRepositoriesInDomain",
                "codeartifact:PutDomainPermissionsPolicy",
                "sts:GetServiceBearerToken"
            ],
            "Resource": "arn:aws:codeartifact:eu-east-1:12345678910:domain/repository_name"
        }
    ]
}
```

## 저장소 정책 적용 또는 편집

저장소 정책을 편집하거나 적용하려면 CodeArtifact 대시보드로 이동하여 권한을 부여하려는 저장소를 선택하십시오. 저장소 세부 정보 페이지에서 "도메인 정책 편집" 버튼이 있습니다.

<div class="content-ad"></div>

## 업스트림 저장소 선택

CodeArtifact 저장소를 생성할 때 업스트림 저장소를 구성할 수 있는 옵션이 있습니다. 업스트림 저장소는 CodeArtifact 저장소가 프록시하고 아티팩트를 가져올 수 있는 외부 저장소입니다. 외부 소스에서 종속성을 캐시하고 관리하여 더 빠르고 신뢰할 수 있는 빌드를 보장하려는 경우 사용됩니다.

![이미지](/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_5.png)

<div class="content-ad"></div>

만약 안드로이드 리포지토리를 만드려면 maven-central-store, google-android-store, 그리고 gradle-plugins-store가 흔히 필요합니다.

![이미지](/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_6.png)

AWS CLI로 도메인과 리포지토리를 만들 수도 있습니다:

```bash
# 도메인 생성
aws codeartifact create-domain --도메인 도메인이름

# 리포지토리 생성
aws codeartifact create-repository 
  --도메인 my-domain 
  --저장소 REPO_NAME 
  --설명 "내 CodeArtifact 리포지토리"

# 상류 저장소 설정
aws codeartifact update-repository 
  --도메인 my-domain 
  --저장소 REPO_NAME 
  --upstreams repositoryName=UPSTREAM_REPO_NAME,domainName=UPSTREAM_DOMAIN_NAME,displayName=UPSTREAM_REPO_DISPLAY_NAME,repositoryType=Maven
```

<div class="content-ad"></div>

- UPSTREAM_REPO_NAME: 상류 저장소의 이름입니다.
- UPSTREAM_DOMAIN_NAME: 상류 저장소의 도메인 이름입니다.
- UPSTREAM_REPO_DISPLAY_NAME: 상류 저장소의 표시 이름입니다.
- repositoryType: 저장소 유형을 지정합니다 (예: NPM, Maven, PyPI 등).

AWS 저장소가 생성된 후 Android, Java 또는 Kotlin 프로젝트를 설정하려면 AWS 콘솔로 이동하여 CodeArtifact로 이동한 다음 Repositories에서 “View Connection Instructions” 버튼을 클릭하십시오. 이렇게 하면 패키지 관리자 클라이언트를 선택할 수 있는 대화 상자가 열립니다. 패키지 관리자로 Gradle을 선택하십시오.

![이미지](/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_7.png)

![이미지](/assets/img/2024-07-06-PublishingAndroidJavaorKotlinLibrarieswithAWSCodeArtifact_8.png)

<div class="content-ad"></div>

네, 이 문장은 아주 흥미로운 것 같네요! AWS CLI를 설치하고 설정하는 방법을 설명하고 계시는군요. Windows 운영체제를 사용하신다면 아래와 같은 단계들을 따르시면 됩니다. 

1. 먼저 AWS CLI(Command Line Interface)를 다운로드하세요.
2. 운영체제에 맞는 버전을 설치해 주세요. Windows, Linux, macOS 중 어느 것을 사용하시는지에 따라 다를 수 있어요.

계속해서 다음 단계도 설명해드릴게요. 함께 진행해봐요!

<div class="content-ad"></div>

AWS CLI MSI Installer를 다운로드하여 Windows에서 설치하세요. 설치 후에 명령 프롬프트 또는 PowerShell을 열어 `aws --version` 명령을 실행하여 AWS CLI가 올바르게 설치되었는지 확인할 수 있어요.

## Linux:

```js
sudo apt update                        # Debian/Ubuntu용
sudo apt install python3 python3-pip   # Debian/Ubuntu용
sudo pip3 install awscli               # pip3를 사용하여 awscli 설치
aws --version                          # 설치 및 버전 확인
```

<div class="content-ad"></div>

## macOS:

가장 빠른 방법은 MacOS PKG 설치 프로그램을 다운로드하거나 Homebrew를 사용하는 것입니다.

```js
brew install awscli        # Homebrew를 사용하여 awscli 설치
aws --version              # 설치 및 버전 확인
```

## AWS CLI 구성

<div class="content-ad"></div>

AWS Command Line Interface (CLI) 설정은 AWS 자격 증명, 지역 및 기타 옵션을 설정하는 것을 포함합니다.

`aws configure` 명령을 실행하여 AWS 액세스 키 ID, 비밀 액세스 키, 지역 및 출력을 입력하거나 필요한 정보를 직접 설정할 수 있습니다.

```shell
aws configure set aws_access_key_id <ACCESS_KEY_ID>
aws configure set aws_secret_access_key <SECRET_ACCESS_KEY>
aws configure set region <REGION>  // eu-west-1, eu-north-1 등
aws configure set output <OUTPUT>  // text, json 또는 table
```

## AWS Configure 목록

<div class="content-ad"></div>

당신의 자격 증명이 정상적으로 설정되었는지 확인하기 위해 aws configure list 명령어를 사용하세요.

```js
aws configure list
```

```js
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************ABCD      shared-credentials-file    
secret_key     ****************WXYZ      shared-credentials-file    
    region                us-east-1      config-file    ~/.aws/config
```

## Profiles

<div class="content-ad"></div>

AWS configure를 사용할 때 "default"라는 기본 프로필이 생성됩니다.

여러 프로필을 가질 수도 있습니다. 다중 프로필을 사용하면 프로젝트 또는 역할을 구분할 수 있어서 각각 다른 권한을 부여할 수 있습니다. 예를 들어, 개인 및 업무 프로젝트용으로 별도 프로필을 만들고 특정 작업에 대한 맞춤 권한을 부여할 수 있습니다. 프로필 전환을 통해 다양한 작업에 대한 자격 증명 및 권한을 쉽게 관리할 수 있어 조직화와 보안을 강화할 수 있습니다. "work" 프로필을 생성하는 예시가 아래에 제공되어 있습니다.

```js
aws configure --profile work set aws_access_key_id <ACCESS_KEY_ID>
aws configure --profile work set aws_secret_access_key <SECRET_ACCESS_KEY>
aws configure --profile work set region <REGION>
aws configure --profile work set output <OUTPUT>
```

AWS 프로필을 확인하거나 편집하는 방법은:

<div class="content-ad"></div>

## macOS & Linux

```js
nano ~/.aws/config
```

Alternatively, you can use Vim, TextEdit, or the Cat command.

## Windows

<div class="content-ad"></div>

**Step 3: Android Code Integration to Pull & Push**

안녕하세요! 오늘은 타로 전문가가 나와 함께하는 타로 예측 시간입니다. 이번에는 한국어로 친근하게 알려드리겠습니다.

타로 카드를 통해 당신이 저만치 나아갈 수 있는 길을 찾아보겠습니다. 마음을 여는 것이 중요한데, 함께 여는 것부터 시작해볼까요? 타로 카드가 당신에게 이야기 하길 기대해봅니다.

지금부터 Step 3을 하기 위한 안드로이드 코드 통합 방법에 대해 알려드리겠습니다. 시작해볼까요?

그러면 마치면서 타로로 확실한 답을 찾아보시길 바라며 모두 좋은 하루 보내세요! 🌟

<div class="content-ad"></div>

라이브러리를 가져오거나 내보내려면 라이브러리 build.gradle 파일을 수정하여 저장소를 불러올 때 사용할 퍼블리싱 URL, 사용자명 및 암호를 포함해야 합니다. URL은 `Domain`, `Domain-owner`, `Region` 및 `Repository-name`으로 구성됩니다.

```js
https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/
```

## Groovy:

```js
// Android 라이브러리용 플러그인: "com.android.library"
// Java/Kotlin 라이브러리용 플러그인: "java-library"

plugins {
    id 'com.android.library'
    id 'maven-publish'
}

publishing {
  publications {
      mavenJava(MavenPublication) {
          groupId = '<groupId>'
          artifactId = '<artifactId>'
          version = '<version>'
      }
  }
  repositories {
      maven {
          url 'https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/'
          credentials {
              username "사용자명"
              password System.env.CODEARTIFACT_AUTH_TOKEN
          }
      }
  }
}

// 선택 사항: Java 버전 설정
configure(JavaPluginExtension) {
    sourceCompatibility = JavaVersion.VERSION_1_8
    targetCompatibility = JavaVersion.VERSION_1_8
}
```

<div class="content-ad"></div>

## 의존성 섹션

이 설정이 추가되면, 앱의 build.gradle에 있는 프로젝트 의존성 섹션을 업데이트해야 합니다. 이를 통해 `groupId`, `artifactId`, `version`을 사용하여 라이브러리를 가져올 수 있습니다.

```js
dependencies {
    implementation '<groupId>:<artifactId>:<version>'
}
```

## CodeArtifact에 안드로이드 라이브러리 발행하기

<div class="content-ad"></div>

AWS CLI를 설치하고 구성해야만 게시 기능을 사용할 수 있어요.

## 게시하기

안드로이드 프로젝트를 빌드하고 게시하려면 다음 명령어를 실행하세요:

```bash
./gradlew :<your_library>:assembleRelease
```

<div class="content-ad"></div>

코드아티팩트로 아티팩트를 게시하려면 다음 명령어를 사용하세요:

```js
./gradlew publish
```

프로젝트의 build.gradle 파일에서 메이븐-퍼블리시 플러그인을 구성했는지 확인해 주세요.

## 코드아티팩트에 업로드된 파일 확인

<div class="content-ad"></div>

CodeArtifact 콘솔로 이동하여 레포지토리를 확인해 보세요. 파일이 성공적으로 발행되었는지 확인할 수 있습니다.

## .aar 파일로 라이브러리 발행하기

.aar 파일로 라이브러리를 발행하려면 Gradle 설정의 publications에 사용자 정의 라이브러리 경로를 추가하고 의존성 구현 섹션에 명시적으로 aar을 포함시키세요.

## Groovy:

<div class="content-ad"></div>


publications {
      mavenJava(MavenPublication) {
          groupId = '<groupId>'
          artifactId = '<artifactId>'
          version = '<version>'
          from components.java // Use if creating a Java or Kotlin Library
          artifact(file('custom-library.aar')) // .aar file
      }
}


## Dependencies

If dependencies with .aar files are not fetched by default, append the string to your dependency import to force import.


dependencies {
    implementation '<groupId>:<artifactId>:<version>@aar'
}


<div class="content-ad"></div>

그게 다에요, 발표를 완료했네요!

# 플러그인 끌어오기

CodeArtifact 저장소를 생성할 때, 위류 저장소를 구성할 수 있는 옵션이 있습니다. 위류 저장소는 CodeArtifact 저장소가 프록시할 수 있는 외부 저장소로, 여기서 종속성을 끌어올 수 있습니다. 외부 소스로부터 종속성을 캐시하고 관리하려는 경우 유용하며, 빌드를 더 빠르고 신뢰할 수 있게합니다. 사용자 정의 라이브러리에 외부 종속성이 있을 수 있으므로 이러한 경우에 유용합니다.

플러그인을 가져오려면 pluginManagement 블록을 추가하십시오. 이는 settings.gradle에서 다른 문이 나타나기 전에 나타나야합니다.

<div class="content-ad"></div>

## 플러그인 관리 블록:

```js
pluginManagement {
    repositories {
        maven {
            name 'my_repo'
            url 'https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/'
            credentials {
                username 'aws'
                password System.env.CODEARTIFACT_AUTH_TOKEN
            }
        }
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}
```

만약 Gradle 설정이 dependencyResolutionManagement 블록을 필요로 한다면, 그곳에도 추가해주어야 합니다.

```js
dependencyResolutionManagement {
    repositories {
        maven {
            url 'https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/'
            credentials {
                username 'aws'
                password System.env.CODEARTIFACT_AUTH_TOKEN
            }
        }
        google()
        mavenCentral()
    }
}
```

<div class="content-ad"></div>

# 단계 4: CodeArtifact 토큰 생성 및 갱신

토큰을 생성하려면 CodeArtifact 권한 토큰 명령을 복사하여 토큰을 가져와 터미널에서 실행하세요. 내보내기 키워드를 사용하여 토큰을 환경 변수에 저장해야 합니다.

## macOS 또는 Linux:

```bash
export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token 
  --domain your_domain 
  --domain-owner 111122223333 
  --region us-east-1 
  --query authorizationToken 
  --output text
  --profile default
```

<div class="content-ad"></div>

## 윈도우 (기본 명령 셸 사용):

```js
for /f %i in ('aws codeartifact get-authorization-token --domain your_domain --domain-owner 111122223333 --query authorizationToken --output text --profile default') do set CODEARTIFACT_AUTH_TOKEN=%i
```

## 윈도우 PowerShell:

```js
$env:CODEARTIFACT_AUTH_TOKEN = aws codeartifact get-authorization-token --domain your_domain --domain-owner 111122223333 --query authorizationToken --output text --profile default
```

<div class="content-ad"></div>

# 생성된 토큰 저장하기

생성된 토큰은 프로젝트에서 사용할 수 있도록 저장할 수 있습니다. 저장하는 방법은 2가지가 있습니다. 토큰을 gradle.properties 파일에 저장하거나 macOS 및 Linux에서 echo 명령을 사용하여 텍스트 파일에 저장할 수 있습니다.

## gradle.properties에 저장하기:

이 방법은 gradle.properties 파일의 내용을 덮어쓸 권한이 있는 경우 사용할 수 있습니다.

<div class="content-ad"></div>

마법사들아, 여러분 안녕하세요! 

이미지 태그를 Markdown 형식으로 변경해보면:


```js
echo "codeartifactToken=$CODEARTIFACT_AUTH_TOKEN" > ~/.gradle/gradle.properties
```

build.gradle에서 저장된 토큰을 가져오는 방법은 다음과 같습니다:

## Groovy:

```js
publishing {
  repositories {
      maven {
          url 'https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/'
          credentials {
              username "USERNAME"
              password project.properties['codeartifactToken']
          }
      }
  }
}
```



어떤가요? 코드를 마크다운 형식으로 작성하니 보기가 편하지 않나요? 부드럽고 친근한 분위기에서 코드를 공유하는 것은 상대방이 이해하기 쉽게 도와줍니다. 더 궁금한 점이 있으면 언제든지 물어봐주세요! 🌟

<div class="content-ad"></div>

# CodeArtifact 토큰 새로 고침
CodeArtifact의 인증 토큰은 기본 유효 기간이 12시간으로 설정되어 있습니다. 그러나 이러한 토큰은 15분부터 12시간까지의 범위 내에서 사용자 설정이 가능합니다. 지정된 수명이 경과하면 새로운 토큰을 발급받아야 합니다.

따라서 원활한 프로세스를 위해 빌드 전에 토큰을 생성하고, 또는 일정 시간마다 토큰을 생성하고 저장할 수 있는 새로 고침 메커니즘이 필요합니다.

<div class="content-ad"></div>

## 옵션 1: 각 실행마다 토큰 새로고침

이 옵션을 선택하면 실행할 때마다 새로운 인증 토큰을 생성하므로 토큰이 즉시 생성되고 어떤 파일 시스템에도 저장되지 않습니다.

## Groovy:

```js
publishing {
  // 토큰을 생성하고 즉시 사용
  def codeartifactToken = "aws codeartifact get-authorization-token --domain my_domain --domain-owner 111122223333 --query authorizationToken --output text --profile profile-name".execute().text
  repositories {
      maven {
          url 'https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/'
          credentials {
              username "USERNAME"
              password codeartifactToken
          }
      }
  }
}
```

<div class="content-ad"></div>

## 코틀린:

```kotlin
import java.io.BufferedReader
import java.io.InputStreamReader

configure<PublishingExtension> {
    // 토큰을 생성하고 사용
    val processBuilder = ProcessBuilder(
        "aws",
        "codeartifact",
        "get-authorization-token",
        "--domain", "your_domain",
        "--domain-owner", "111122223333",
        "--region", "us-east-1",
        "--query", "authorizationToken",
        "--output", "text",
        "--profile", "profile-name"
    )
    processBuilder.redirectErrorStream(true)
    val process = processBuilder.start()
    val codeArtifactToken = when (process.waitFor()) {
        0 -> {
            val reader = BufferedReader(InputStreamReader(process.inputStream))
            val output = reader.readText().trim()
            reader.close()
            output
        }
        else -> "noCodeArtifactToken"
    }
    repositories {
        maven {
            url = "https://<DOMAIN>-<DOMAIN-OWNER>.d.codeartifact.<REGION>.amazonaws.com/maven/<REPOSITORY-NAME>/"
            credentials {
                username = "USERNAME"
                password = codeArtifactToken
            }
        }
    }
}
```

## 옵션 2: 캐시 만료 시간 및 토큰 갱신

토큰이 갱신되어야하는지를 확인할 수 있습니다. 초기 요청때문인지 또는 만료되었기 때문인지. 필요한 경우 갱신 토큰 함수가 새 토큰을 생성합니다.

<div class="content-ad"></div>

카드 오픈스레이어스를 통해 무료로 제공되는 기능을 사용하면 태그를 Markdown 형식으로 바꿀 수 있어요.

Kotlin:

```js
import java.io.BufferedReader
import java.io.InputStreamReader

buildscript {
  ext {
     cachedToken = null
     tokenExpiryTime = 0L
  }
}

configure<PublishingExtension> {
  publications {
    // configure publish extensions 
  }
  repositories {
    maven {
       // configure repositories
    }
  }
}

// Declare nullable variables for the cached token and its expiry time
var cachedToken: String? = null
var tokenExpiryTime: Long = 0

// Function to fetch token if needed
fun refreshToken(): String? {
    val currentTime = System.currentTimeMillis()

    return if (cachedToken == null || currentTime >= tokenExpiryTime) {
        // If token is null or expired, generate a new one
        cachedToken = generateToken()
        // Set the expiry time for the token (12 hours from current time)
        tokenExpiryTime = currentTime + (12 * 60 * 60 * 1000)
        cachedToken // Return the fetched token
    } else {
        // If token is still valid, return the existing token
        cachedToken
    }
}

fun generateToken(): String {
  // generate token code
}
```

## Option 3: macOS 또는 Linux에 crontab 설정하여 토큰을 자동 갱신하세요

cron 작업을 설정하여 generateToken.sh 파일을 만들고 일정 간격으로 실행하여 토큰을 주기적으로 갱신하세요.

<div class="content-ad"></div>

매일 오전 9시에 generate token 스크립트를 실행하는 방법은 다음과 같습니다.


0 9 * * * /path/to/generateToken.sh


`* * * * *`는 매분, 매시간, 매일, 매월, 매주의 모든 날을 의미합니다.


*  *  * * * 
│  │  │ │ └───── 요일 (0 - 7, 0과 7은 일요일을 나타냄)
│  │  │ └─────── 월 (1 - 12)
│  │  └───────── 일 (1 - 31)
│  └──────────── 시간 (0 - 23)
└─────────────── 분 (0 - 59)


GetAuthorizationToken API를 사용하는 데 자주 발생하는 문제 및 해결책을 소개해드리겠습니다. 이를 통해 더 원활한 작업을 진행할 수 있습니다.

<div class="content-ad"></div>

AWS CLI와 인증 자격 증명을 설정하는 동안 발생할 수 있는 몇 가지 오류를 문서화했습니다.
### — 발행 명령어에서 401 오류가 발생하는 경우
```js
* 문제점:
작업 ':Your_library:publishMavenJavaPublicationToMavenRepository'을(를) 실행하지 못했습니다.
> 'maven' 저장소에게 'mavenJava' 발행물을 게시하지 못했습니다.
   > '<전체 저장소 URL>'을(를) PUT할 수 없음.
   > 서버에서 상태 코드 401을(를) 수신했습니다: 인증되지 않음
```
해결 방법: URL, 사용자 이름 및 토큰과 같은 모든 자격 증명이 정확한지 확인하십시오. 토큰이 만료된 경우 토큰을 재생성하십시오.

<div class="content-ad"></div>

## — 루트 사용자에 대한 권한 오류

만약 이 오류 메시지를 받는다면, 이는 GetAuthorizationToken API가 루트 사용자에게 호출되고 있음을 나타냅니다. IAM 사용자 자격 증명을 사용하도록 AWS 자격 증명을 구성해야 합니다. 루트 사용자는 GetServiceBearerToken을 호출할 수 없습니다.

```js
GetAuthorizationToken 작업을 호출할 때 오류가 발생했습니다: 
User: arn:aws:iam::12345678910:root은(는) 
리소스인 arn:aws:iam::12345678910:root에서 
sts:GetServiceBearerToken을 실행할 권한이 없습니다.
```

수정: AWS 자격 증명을 IAM 사용자 자격 증명을 사용하도록 구성하세요.

<div class="content-ad"></div>

## — IAM 사용자에 대한 권한 오류

이 오류 메시지를 받으면, IAM 사용자의 사용자 이름이 지정된 리소스 (arn:aws:codeartifact:eu-west-1:12345678910:domain/repo)에서 codeartifact:GetAuthorizationToken 작업을 수행할 필요한 권한을 갖고 있지 않음을 나타냅니다.

```js
GetAuthorizationToken 작업을 호출할 때 AccessDeniedException 오류가 발생했습니다: 
사용자 arn:aws:iam::12345678910:user/username은 아이덴티티 기반 정책에서 명시적으로 거부하여 
arn:aws:codeartifact:eu-west-1:12345678910:domain/repo 리소스에서 
codeartifact:GetAuthorizationToken 작업을 수행할 권한이 없습니다
```

해결 방법: IAM 도메인 정책을 업데이트하여 지정된 리소스에 codeartifact:GetAuthorizationToken 작업을 포함시킵니다.

<div class="content-ad"></div>

## — 만료된 토큰 예외 오류

AWS CLI에서 "ExpiredTokenException" 오류 메시지를 받게 되면, 요청의 인증에 사용된 보안 토큰이 만료되었음을 나타냅니다. AWS 보안 토큰은 일반적으로 보안을 위해 제한된 수명을 가지고 있으며, 만료되면 더 이상 요청을 인증하는 데 사용할 수 없습니다.

```js
GetAuthorizationToken 작업을 호출할 때 오류가 발생했습니다: 요청에 포함된 보안 토큰이 만료되었습니다. (ExpiredTokenException)
```

<div class="content-ad"></div>

**고치세요:** 새 보안 토큰을 획득하고 인증에 사용해야 합니다. AWS CLI 구성을 새 자격 증명으로 업데이트하세요. 일반적으로 ~/.aws/config 또는 ~/.aws/credentials에 있는 AWS CLI 구성 파일을 직접 편집하거나 aws configure 명령을 사용해서 이를 수행할 수 있습니다.

## 출처

- Gradle과 함께 CodeArtifact 사용하기
- AWS CodeArtifact 인증 및 토큰
- CodeArtifact 저장소 정책

## 팔로우하세요

<div class="content-ad"></div>

- X/Twitter: @tosinmath