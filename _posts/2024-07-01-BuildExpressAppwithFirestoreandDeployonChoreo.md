---
title: "Express 앱을 Firestore와 연동하여 Choreo에 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_0.png"
date: 2024-07-01 20:42
ogImage: 
  url: /assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_0.png
tag: Tech
originalTitle: "Build Express App with Firestore and Deploy on Choreo"
link: "https://medium.com/@ishanhansakasilva/build-express-app-with-firebase-and-deploy-on-choreo-07152f2423ba"
---


## 데이터베이스 통합 및 Choreo 플랫폼에 배포하는 단계별 가이드

![이미지](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_0.png)

이 블로그 포스트에서는 Express.js를 사용하여 서버를 생성하고 프로젝트 세부 정보를 저장할 간단한 데이터베이스와 통합하는 과정을 안내합니다. 그 후에는 Choreo 플랫폼에 배포할 것입니다. 데이터베이스로는 Cloud Firestore를 사용할 것입니다.

![이미지](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_1.png)

<div class="content-ad"></div>

## 기술 개요

Express.js는 간결하고 유연한 Node.js 프레임워크로, 강력한 기능과 미들웨어 지원을 통해 서버 측 애플리케이션의 작성을 간소화합니다. Firebase의 NoSQL 문서 데이터베이스 인 Cloud Firestore와 결합되어 있습니다. Choreo는 API 관리, 이벤트 스트리밍 및 마이크로서비스 통합을 제공하여 통합을 간편화합니다. WSO2가 개발한 이 직관적인 플랫폼은 스리랑카의 주요 소프트웨어 회사로, 애플리케이션의 배포 및 관리를 단순화하여 프로젝트를 구축하고 확장하기가 이전보다 쉬워집니다.

## 프로젝트 설정하기

코드에 들어가기 전에 다음 항목이 설치되어 있는지 확인하세요:

<div class="content-ad"></div>

- 기기에 Node.js와 npm을 설치해주세요
- Firebase 계정 (Firebase에서 가입하세요)
- Choreo 계정 (Choreo에서 가입하세요)
- Github 계정

이 튜토리얼에서는 다음 주요 단계를 다룰 예정입니다:

- Express.js 설정
- Firebase Admin SDK 초기화
- Firebase GET 및 POST 작업을 위한 라우트 추가
- Choreo에 배포하기

# 단계 01: Express.js 설정

<div class="content-ad"></div>

- 프로젝트 초기화하기

우선, 프로젝트를 위한 새 디렉토리를 만들어주세요. 터미널이나 명령 프롬프트를 열고 프로젝트를 생성하고 싶은 디렉토리로 이동해주세요. 그런 다음, 다음 명령어를 실행해주세요:

```js
npm init -y
```

- Express 설치하기

<div class="content-ad"></div>

Express를 설치하려면 npm을 사용하면 됩니다. 이 명령은 Express를 프로젝트에 설치합니다.

```js
npm install express
```

- Express 앱 만들기

Express 애플리케이션을 위한 새 파일을 생성하세요. 예를 들어, 프로젝트의 루트 디렉토리에 app.js 또는 index.js와 같은 파일을 만들어주세요. Express를 요구하고 앱의 인스턴스를 생성하세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_2.png" />

# 단계 02: Firebase Admin SDK 초기화

- Firebase 프로젝트 설정:

Firebase 콘솔로 이동하여 새 프로젝트를 만들고 웹 앱을 추가하세요. 추가적인 정보 및 자세한 설정 지침은 Firebase 웹 앱을 위한 Firebase 설정에 대한 공식 문서인 Firebase 웹 설정 가이드를 참조해주세요.

<div class="content-ad"></div>

- 서비스 계정 키 생성하기:

파이어베이스 콘솔의 프로젝트 설정으로 이동한 후, "서비스 계정"으로 이동하여 "새 비공개 키 생성"을 클릭합니다. 이 동작은 애플리케이션을 Firebase 서비스와 안전하게 인증하는 데 필수적인 JSON 키 파일을 생성합니다. 이 파일을 다운로드하여 프로젝트 디렉토리에 안전하게 저장하세요.

- 익스프레스 앱에서 Firebase Admin SDK 설정하기:

먼저, npm을 사용하여 Firebase Admin SDK npm 패키지를 설치합니다. 이 명령어는 Firebase Admin SDK 패키지를 설치하고, 서버 측에서 Firebase 서비스에 액세스할 수 있게 합니다.

<div class="content-ad"></div>

```js
npm install --save firebase-admin  
```

그런 다음 JSON 파일에서 서비스 계정 자격 증명을로드하고 Firebase Admin SDK를 초기화합니다.

<img src="/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_3.png" />

초기화되면 Firebase Admin SDK 메서드를 사용하여 Firestore와 상호 작용할 수 있습니다. Firebase에서 클라우드 firestore 데이터베이스를 만들고 project_id에서 databaseURL을 추가하려면 이것을 사용할 수 있어요.

<div class="content-ad"></div>

# 단계 03: Firebase GET 및 POST 작업을 위한 라우트 추가

이 단계에서는 Express 애플리케이션에 Firestore와 GET 및 POST 작업을 수행하기 위한 라우트를 추가할 것입니다.

## GET 엔드포인트 ("/projects")

<img src="/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_4.png" />

<div class="content-ad"></div>

## POST Endpoint (“/projects”)

포스트 메소드를 설정하기 전에, Express 애플리케이션이 JSON 요청 바디를 효과적으로 처리할 수 있도록 하는 것이 중요합니다. 이를 위해, JSON 및 URL 인코딩된 요청 바디를 파싱하기 위한 미들웨어인 body-parser를 사용할 것입니다. 먼저, npm을 사용하여 body-parser를 설치해야 합니다:

```js
npm install --save body-parser
```

그런 다음, 이러한 기능은 프로젝트의 index.js 파일 내에서 구현할 수 있습니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_5.png)

이제 Firestore에 프로젝트를 추가하기 위한 POST 메소드 설정에 대해 알아봅시다.

![Image](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_6.png)

Postman 또는 Insomnia와 같은 도구를 사용하여 서버에 HTTP 요청을 보내고 기능을 확인할 수 있습니다. 이 예제에서는 Insomnia를 통해 서버 엔드포인트와 상호 작용하기 위해 HTTP 요청을 보냅니다.

<div class="content-ad"></div>

아래에 클라우드 Firestore 데이터베이스가 표시됩니다.

Firebase GET 및 POST 작업에 대한 루트를 추가하는 세 번째 단계를 완료하였으므로, 이제 Choreo에 배포하는 최종 단계로 진행하겠습니다.

<div class="content-ad"></div>

# 단계 04: Choreo에서 배포하기

GitHub 계정을 설정하고 서비스 구현이 포함된 저장소를 만들었다면, Choreo를 사용하여 애플리케이션을 배포하는 방법을 따르세요.

이 프로세스는 Choreo 플랫폼으로 서비스를 배포하는 것을 간소화할 것입니다.

1. 프로젝트 생성

<div class="content-ad"></div>


![Image 1](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_9.png)

2. Create a service component

On the project home page, click Service under create a component.

![Image 2](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_10.png)


<div class="content-ad"></div>

· GitHub 탭으로 이동해서 레포지토리 URL을 추가해주세요.

Choreo가 귀하의 GitHub 계정에 연결할 수 있도록 하려면 'GitHub와 인증'을 클릭하세요. Choreo에 GitHub 레포지토리를 연결하지 않았다면, 사전 준비 단계에서 생성한 GitHub 자격 증명을 입력하고 Choreo GitHub 앱을 설치할 레포지토리를 선택해주세요.

![이미지](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_11.png)

· NodeJS 빌드팩을 선택해주세요.

<div class="content-ad"></div>

· NodeJS 프로젝트 디렉토리 설정

배포 과정의 일환으로 endpoints.yaml 파일을 생성해야 합니다. 이 파일은 Express 앱이 연결해야 하는 외부 서비스의 세부 정보(데이터베이스 또는 API 등)를 지정하는 데 사용됩니다. 더 알아보기.

![file](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_12.png)

파일 디렉토리 = `.choreo/endpoints.yaml`

<div class="content-ad"></div>

프로젝트에 openapi.yaml 파일을 만들어주세요.

openapi.yaml 파일을 생성하는 것은 표준화된 API 문서, 자동화된 테스트, 유효성 검사, 클라이언트 라이브러리 생성을 위해 중요합니다. 이는 일관된 API 설계를 보장하며 다양한 도구와 통합되어 배포 자동화 및 API 관리의 간소화로 이어지며, 사용성을 향상시킵니다.

openapi.yaml 파일에서 일관성을 유지하기 위해 Swagger Editor와 같은 온라인 도구를 사용하세요. Swagger Editor는 실시간 피드백을 제공하고 오류를 강조해줍니다. Swagger Editor에서 엔드포인트를 실행할 때 "Failed to fetch" 오류가 발생할 수 있는데, 이는 CORS 문제 때문일 수 있습니다. 프로젝트에 cors 패키지를 설치하고 Express.js 앱을 업데이트하여 cors 미들웨어를 사용하면 엔드포인트의 원활한 실행을 보장할 수 있습니다.

![이미지](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_13.png)

<div class="content-ad"></div>

작업이 완료되면 변경 내용을 저장소로 푸시하고 다음 단계로 진행할 수 있습니다.

- 생성을 클릭하세요.

NodeJS 빌드팩을 사용하여 서비스 구성 요소를 성공적으로 생성했습니다. 이제 서비스를 빌드하고 배포해 봅시다.

![이미지](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_14.png)

<div class="content-ad"></div>

- 빌드 및 배포

## 빌드

서비스를 빌드하려면 다음 단계를 따르세요:

1. 프로젝트 홈페이지에서 Component Listing 아래에 나열된 Book List 구성 요소를 클릭하십시오. 이렇게 하면 구성 요소 개요 페이지로 이동됩니다.

<div class="content-ad"></div>

2. 왼쪽 네비게이션에서 'Build'를 클릭해주세요.

3. 'Build Latest'를 클릭하세요.

빌드 프로세스가 완료되면 빌드 상태가 'Success'로 변경됩니다.

<img src="/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_15.png" />

<div class="content-ad"></div>

## 배포

이제 서비스를 배포할 준비가 되었습니다. 다음 단계를 따르세요:

1. 왼쪽 네비게이션 메뉴에서 배포를 클릭하세요.

2. 설정 카드에서 구성 및 배포를 클릭하세요.

<div class="content-ad"></div>

3. 환경 구성 창에서 '다음'을 클릭해주세요.

4. 파일 마운트 창에서 '다음'을 클릭해주세요.

5. 엔드포인트 세부 정보를 확인하고 '배포'를 클릭해주세요.

![image](/assets/img/2024-07-01-BuildExpressAppwithFirestoreandDeployonChoreo_16.png)

<div class="content-ad"></div>

# 요약

이 블로그를 통해 프로젝트를 관리하기 위해 클라우드 Firestore와 통합된 Express.js 서버를 설정하는 방법을 안내했습니다. REST API 엔드포인트를 생성하고 어플리케이션을 Choreo에 배포하여 확장성과 효율적인 클라우드 기반 작업을 가능케 했습니다. 이러한 기술은 클라우드 데이터베이스 솔루션을 효과적으로 활용하여 강력한 서버 측 응용 프로그램을 구축할 수 있는 능력을 갖추게 해 줍니다.

# 참고 자료

- wso2.com/choreo/docs
- deploy-your-first-service
- configure-endpoints
- firebase.google.com
- firestore
- cors.html
- OpenAPI Specification
- Swagger editor
- projectManaging_Choreo