---
title: "AWS Lambda, SQS, Nodejs, MongoDB로 자동화 워크플로우 만들기"
description: ""
coverImage: "/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_0.png"
date: 2024-08-04 18:30
ogImage: 
  url: /assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_0.png
tag: Tech
originalTitle: "How to Build Scalable Automated Workflows like HubSpot using AWS Lambda, SQS, Nodejs, and MongoDB"
link: "https://medium.com/@bhaskar.csawant417/how-to-build-scalable-automated-workflows-like-hubspot-using-aws-lambda-sqs-node-js-and-mongodb-1ac28dd4b26e"
isUpdated: true
---





HubSpot의 강력한 시스템과 유사한 자동화된 워크플로우를 만드는 데에는 고급 클라우드 기술을 활용해야 합니다. 이 안내서는 AWS Lambda, Amazon SQS, Node.js, 그리고 MongoDB를 사용하여 이러한 워크플로우를 개발하는 과정을 안내해 드릴 것입니다.

## 자동화된 워크플로우 이해

자동화된 워크플로우는 특정 이벤트에 의해 트리거된 작업들의 연속입니다. 이들은 반복적인 작업을 간소화하고, 알림을 보내고, 기록을 업데이트하며 등등을 수동 개입 없이 처리합니다. AWS Lambda, SQS, Node.js, 그리고 MongoDB를 통합함으로써 견고한 워크플로우를 구축할 수 있습니다. 이 튜토리얼은 이러한 워크플로우를 만드는 단계별 접근 방식을 제공합니다.

## 배울 내용

<div class="content-ad"></div>

- 자동화된 Workflow 아키텍처: 자동화된 Workflows의 내부 작동 방식에 대한 통찰력을 얻어보세요.
- API Server 이해: 코드 예제와 설명을 통해 Workflow가 어떻게 동작하는지 배웁니다.
- AWS CLI 구성: AWS CLI를 설치하고 구성하여 명령줄에서 AWS 리소스를 관리하세요.
- Serverless Framework를 사용한 배포: Serverless Framework를 사용하여 AWS Lambda 함수 및 다른 리소스를 생성하고 배포하세요.
- AWS SQS와 API 통합: API 서버에서 신뢰할 수 있는 Workflows를 위해 AWS SQS로 메시지를 보내세요.
- AWS EC2에 API 서버 배포: AWS EC2에 Node.js API 서버를 배포하여 고가용성과 성능을 갖추세요.
- 확장성: Workflows 및 API 서버의 확장법을 이해하여 트래픽과 수요가 증가할 때 대비하세요.

# 필수 사항

- AWS 계정.
- Node.js 및 MongoDB의 기본 지식.
- MongoDB Atlas 계정 또는 로컬 MongoDB 설정에 대한 액세스.

# 자동화된 Workflow 아키텍처.

<div class="content-ad"></div>


<img src="/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_0.png" />

이제 사용자가 양식을 제출하면 메시지를 보내는 워크플로우 예시로 설명해보겠습니다. 다음은 단계별 프로세스입니다:

- 트리거 이벤트: 양식 제출이 발생하면 트리거 역할을 합니다.
- SQS로 메시지 전송: 양식 제출 기능에 코드 조각을 추가하여 새 양식 제출마다 TriggerQueue라는 SQS 대기열에 메시지를 푸시합니다.
- 람다 함수 호출: TriggerQueue는 메시지 수신시 람다 함수를 트리거합니다.
- DB에서 워크플로 검사: 람다 함수는 SQS 메시지에서 제공된 구성과 일치하는 데이터베이스에 있는 워크플로가 있는지 확인합니다.
- 워크플로 액션 가져오기: 일치하는 워크플로가 발견되면 제공된 데이터로 수행할 작업 세트를 가져옵니다.
- 두 번째 SQS 대기열로 작업 전달: 람다 함수는 작업을 다른 SQS 대기열인 ActionsQueue에 트리거 데이터와 함께 전송합니다.
- 두 번째 람다 함수 호출: ActionsQueue는 메시지 수신시 두 번째 람다 함수를 트리거합니다.
- 작업 수행: 두 번째 람다 함수는 작업 데이터를 가져와 해당 API를 호출하여 사용자에게 메시지를 보내는 등의 작업을 수행합니다.

이 설정을 통해 워크플로우가 신뢰할 수 있고 다양한 작업을 효율적으로 처리할 수 있음을 보장합니다.


<div class="content-ad"></div>

<img src="/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_1.png" />

# API 서버 이해하기.

프로젝트의 소스 코드는 여기에서 찾을 수 있어요.

이제 Model 및 Controller에 대해 자세히 살펴보고 API 서버가 어떻게 작동하는지 이해해 보겠습니다.

<div class="content-ad"></div>

## 워크플로우 모델

워크플로우 모델에서는 하나의 트리거와 하나 이상의 액션을 가지고 있습니다. 아래는 워크플로우와 트리거의 스키마 정의입니다.

```js
// models/workflow.ts

const triggerSchema = new Schema<Trigger>({
  type: { type: String, required: true },
  app: { type: String, required: true },
  metaData: { type: String, required: false },
  data: { type: Schema.Types.Mixed, required: false } // 동적 데이터에 대한 혼합 형식
});

const workflowSchema = new Schema<Workflow>({
  workflowName: { type: String, required: true },
  trigger: { type: triggerSchema, required: true },
  actions: { type: [triggerSchema], required: true },
  created_at: { type: Date, default: Date.now },
  updated_at: { type: Date, default: Date.now },
});

// 모델 생성 및 내보내기
const WorkflowModel = mongoose.model<Workflow>('Workflow', workflowSchema);

export default WorkflowModel;
```

이 모델에서:

<div class="content-ad"></div>

- Trigger: type, app, optional metaData, 및 data 속성을 포함합니다.
- Workflow: workflowName, trigger, actions, created_at, 및 updated_at과 같은 속성이 포함된 Mongoose Document 인터페이스를 확장합니다.

예를 들어, trigger가 폼 제출인 경우, app은 `forms`이고 type은 `new_submission`이 됩니다. metaData는 아래와 같이 특정 워크플로를 식별하는 데 도움이 됩니다:

```js
{
  "app": "forms",
  "type": "new_submission",
  "metadata": "{_id:someUniqueId}"
}
```

## 폼 제출 생성하기

<div class="content-ad"></div>

다음은 양식 제출을 처리하고 워크플로를 트리거하는 방법입니다:

```js
export const createFormSubmission = async (req: Request<{}, {}, FormSubmissionBody>, res: Response): Promise<void> => {
  try {
    const { formId, customerName, customerEmail, customerPhone } = req.body;
    const newForm = new FormSubmission({ formId, customerName, customerEmail, customerPhone });
    await newForm.save();

    // 워크플로로 보낼 데이터 준비
    let workflowData = {
      app: 'forms',
      type: 'new_submission',
      metaData: JSON.stringify({ _id: formId }),
      data: newForm,
    }
    
    let params: params = {
      MessageBody: JSON.stringify(workflowData),
      QueueUrl: process.env.AWS_SQS_URL as string,
    };

    // 메시지 전송
    sqs.sendMessage(params, (err, data) => {
      if (err) {
        console.log('에러 발생', err);
      } else {
        console.log('성공', data.MessageId);
      }
    });
    res.status(201).json(newForm);
  } catch (error) {
    res.status(500).json({ error: '내부 서버 오류' });
  }
};
```

이 컨트롤러에서는 다음과 같은 작업이 수행됩니다:

- 새 양식 제출이 생성되고 데이터베이스에 저장됩니다.
- 양식 제출 세부 정보를 포함한 워크플로 데이터가 준비됩니다.
- 워크플로 데이터가 AWS SQS 대기열로 전송됩니다.
- 트리거 구성과 일치하는 워크플로(예: 앱: 'forms', 유형: 'new_submission', metaData: ' _id: someUniqueId')가 식별되며 해당 작업은 실행을 위해 다른 대기열로 전송됩니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_2.png)

# AWS CLI 구성하기

Serverless Framework을 사용하려면 먼저 AWS CLI를 구성해야 합니다. IAM 사용자를 생성하고 AWS CLI를 구성하는 단계를 따라하세요.

## IAM 사용자 생성


<div class="content-ad"></div>

- AWS 콘솔에 로그인하세요: AWS 관리 콘솔에 로그인합니다.
- IAM 찾기: 검색창에 "IAM"을 입력하고 결과에서 IAM 서비스를 선택합니다.
- 사용자로 이동: IAM 대시보드에서 "사용자" 옵션을 클릭합니다.
- 새 사용자 만들기:

   - "사용자 추가"를 클릭합니다.
   - 사용자 이름을 입력합니다.
   - "다음"을 클릭합니다.

5. 권한 설정:

   - "정책 직접 연결"을 선택합니다.
   - "권한 정책" 섹션에서 "관리자 액세스"를 선택합니다.
   - "다음"을 클릭한 후 "사용자 만들기"를 클릭합니다.

<div class="content-ad"></div>

6. 액세스 키 생성 방법:

- 새로 생성한 사용자를 클릭합니다.
- "보안 자격 증명" 탭으로 이동합니다.
- "액세스 키 생성"을 클릭합니다.
- 사용 사례로 "Command Line Interface (CLI)"를 선택합니다.
- 확인란을 체크합니다.
- "다음"을 클릭합니다.

7. 설명 추가 및 키 생성:

- 원하는 경우 설명을 추가합니다.
- "액세스 키 생성"을 클릭합니다.

<div class="content-ad"></div>

8. 액세스 키 저장:

- 액세스 키 ID 및 시크릿 액세스 키를 복사합니다.
- 시크릿 액세스 키는 다시 표시되지 않으므로 안전하게 보관하세요.

## AWS CLI 구성

- AWS CLI 설치: 아직 설치하지 않았다면 AWS CLI를 다운로드하고 설치하거나 brew install awscli를 실행합니다.
- 터미널/명령 프롬프트 열기: 터미널(Linux/Mac) 또는 명령 프롬프트(Windows)를 엽니다.
- AWS 구성 명령 실행:

<div class="content-ad"></div>

```js
aws configure
```

4. 자격 증명 입력: 자격 증명을 입력하라는 프롬프트가 나타납니다.

- AWS 액세스 키 ID: 액세스 키 ID를 입력하세요.
- AWS 비밀 액세스 키: 비밀 액세스 키를 입력하세요.
- 기본 지역 이름: 선호하는 AWS 지역을 입력하세요 (예: us-east-1).
- 기본 출력 형식: 선호하는 출력 형식을 입력하세요 (예: json).

5. 구성 확인:  

<div class="content-ad"></div>

다음 명령을 실행하여 구성을 확인하세요:

```js
aws sts get-caller-identity
```

이 명령을 실행하면 IAM 사용자에 대한 세부 정보가 반환되며, AWS CLI가 올바르게 구성되었음을 확인할 수 있어요.

<div class="content-ad"></div>

# Serverless Framework으로 배포하기.

프로젝트의 소스 코드는 여기에서 찾을 수 있어요.

## Serverless Framework란?

Serverless Framework는 서버리스 애플리케이션을 구축하고 배포하는 프로세스를 간단하게 만들어주는 오픈 소스 도구입니다. 이 도구는 클라우드 인프라의 복잡성을 추상화하여 개발자가 애플리케이션 코드 작성에 집중할 수 있도록 도와줍니다. AWS, Azure, Google Cloud와 같은 다양한 클라우드 제공 업체를 지원하며, Serverless Framework는 함수를 배포하고 리소스를 관리하며 애플리케이션을 신속하게 확장하는 일관된 경험을 제공합니다.

<div class="content-ad"></div>

이 가이드에서는 람다 함수를 개발하고 Serverless Framework를 사용하여 리소스를 배포하는 방법을 알아볼 것입니다. 람다에서 데이터베이스를 사용하는 방법, MongoDB 사용, 및 한 람다 큐 워커에서 다른 큐로 메시지를 보내는 등의 주제를 다룰 것입니다.

- Serverless Framework 설치: 먼저 머신 전반에 Serverless Framework를 전역으로 설치하세요.

```js
npm install -g serverless
```

2. Serverless 서비스 생성: 프로젝트 디렉토리로 이동하여 새로운 Serverless 서비스를 생성하세요.

<div class="content-ad"></div>

- 터미널에 서버리스 명령을 입력하고 AWS / Node.js / Simple Function 옵션을 선택하세요.
- 이제 프로젝트 이름을 지정하고 새 앱 만들기 옵션을 선택하여 앱 이름을 지정하면 서버리스 서비스가 생성됩니다.

3. 워커/핸들러 생성

- TriggerQueue 핸들러: 이 핸들러는 트리거 큐에서 메시지를 가져와 메시지에 제공된 트리거 구성과 동일한 워크플로를 찾아 해당하는 작업 집합을 작업 큐에 트리거 데이터와 함께 푸시합니다.

```js
'use strict';

const { MongoClient } = require('mongodb');
const AWS = require('aws-sdk');
const sqs = new AWS.SQS();

const MONGODB_URI = process.env.MONGODB_URI;
const MONGODB_DB = process.env.MONGODB_DB;
const OUTPUT_QUEUE_URL = process.env.OUTPUT_QUEUE_URL;

let cachedDb = null;

async function connectToDatabase(uri) {
  if (cachedDb) {
    return cachedDb;
  }

  const client = await MongoClient.connect(uri, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  });

  const db = client.db(MONGODB_DB);
  console.log('Connected to MongoDB');

  cachedDb = db;
  return db;
}

const buildQuery = (type, app, metaData) => {
  const query = { 'trigger.type': type };
  if (app) {
    query['trigger.app'] = app;
  }
  if (metaData) {
    query['trigger.metaData'] = metaData;
  }
  return query;
};

module.exports.triggerHandler = async (event) => {
  // 환경 변수 확인
  if (!MONGODB_URI || !MONGODB_DB || !OUTPUT_QUEUE_URL) {
    console.error('환경 변수가 누락되었습니다');
    return {
      statusCode: 500,
      body: JSON.stringify({
        message: '내부 서버 오류',
      }),
    };
  }

  let db;
  try {
    db = await connectToDatabase(MONGODB_URI);
  } catch (error) {
    console.error('MongoDB에 연결하는 중 오류 발생:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({
        message: 'MongoDB에 연결하는 중 오류 발생',
      }),
    };
  }

  for (const record of event.Records) {
    const { body } = record;
    console.log(`메시지 수신: ${body}`);

    let parsedBody;
    try {
      parsedBody = JSON.parse(body);
    } catch (error) {
      console.error('메시지 본문 구문 분석 오류:', error);
      continue; // 다음 메시지로 건너뛰기
    }

    const { type, app, metaData, data } = parsedBody;

    let result;
    try {
      const collection = db.collection('workflows');
      const query = buildQuery(type, app, metaData);
      result = await collection.find(query).toArray();
    } catch (error) {
      console.error('MongoDB 쿼리 중 오류 발생:', error);
      continue; // 다음 메시지로 건너뛰기
    }

    for (const workflow of result) {
      const MessageBody = {
        actions: workflow.actions,
        data: data,
      };
      console.log('전송할 메시지:', MessageBody);

      const params = {
        QueueUrl: OUTPUT_QUEUE_URL,
        MessageBody: JSON.stringify(MessageBody),
      };

      try {
        await sqs.sendMessage(params).promise();
        console.log('출력 큐로 메시지 전송 완료');
      } catch (error) {
        console.error('출력 큐로 메시지 보내는 중 오류 발생:', error);
      }
    }
  }

  return {
    statusCode: 200,
    body: JSON.stringify({
      message: '메시지 처리 및 성공적으로 전송됨',
    }),
  };
};
```

<div class="content-ad"></div>

- ActionsQueue Handler: 이 핸들러는 작업 대기열에서 메시지를 가져와 해당 API를 호출하여 워크플로 작업을 수행합니다.

```js
'use strict';
const axios = require('axios');

const API_SERVER_URL = process.env.API_SERVER_URL;

const handleMessage = async (type, data) => {
  try {
    switch (type) {
      case 'send_message':
        const messageBody = {
          from: 'workflow',
          message: JSON.stringify(data),
        };
        await axios.post(`${API_SERVER_URL}/api/v1/send_message`, messageBody);
        console.log('메시지 전송 성공');
        break;
      default:
        console.log('알 수 없는 유형:', type);
    }
  } catch (error) {
    console.error('메시지 처리 중 오류:', error);
  }
};

const performAction = async (action, data) => {
  const { type, app } = action;
  switch (app) {
    case 'messaging':
      await handleMessage(type, data);
      break;
    default:
      console.log('알 수 없는 앱:', app);
  }
};

module.exports.actionsHandler = async (event) => {
  for (const record of event.Records) {
    const { body } = record;

    let parsedBody;
    try {
      parsedBody = JSON.parse(body);
    } catch (error) {
      console.error('메시지 본문 파싱 오류:', error);
      continue; // 다음 레코드로 이동
    }

    const { actions, data } = parsedBody;
    for (const action of actions) {
      console.log('작업 수행 중:', action, data);
      try {
        await performAction(action, data);
        console.log('작업 성공적으로 수행');
      } catch (error) {
        console.error('작업 수행 중 오류:', error);
      }
    }
  }

  return {
    statusCode: 200,
    body: JSON.stringify({
      message: '메시지 처리 및 전송 성공',
    }),
  };
};
```

4. Serverless 서비스 배포: serverless.yml 구성 파일은 Serverless Framework를 사용하여 AWS에 서버리스 애플리케이션을 배포하는 설정을 개요로 설명합니다.

간단한 용어로 설명하자면:

<div class="content-ad"></div>

- 기본 정보:

- 조직: bhaskarsawant
- 애플리케이션: trigger
- 서비스: trigger

2. 플러그인:

- serverless-offline: 개발 및 테스트를 위해 서비스를 로컬에서 실행할 수 있도록 합니다.

<div class="content-ad"></div>

3. 공급자 구성:

- 공급자: AWS
- 런타임: Node.js 버전 20.x
- 지역: ap-south-1 (AWS 지역)

4. IAM 역할 권한

- 권한: 해당 서비스는 다음을 수행할 수 있습니다:
- SQS에서 메시지 수신
- SQS에서 메시지 삭제
- SQS 큐 속성 가져오기
- SQS로 메시지 전송

<div class="content-ad"></div>

5. 자원:

- TriggerQueue ARN (Amazon 리소스 이름)
- ActionsQueue ARN

6. 환경 변수:

- OUTPUT_QUEUE_URL: ActionsQueue를 참조하는 URL
- MONGODB_URI: 환경 변수로부터 가져온 MongoDB 연결 URL
- MONGODB_DB: 환경 변수로부터 가져온 MongoDB 데이터베이스 이름
- API_SERVER_URL: 환경 변수로부터 가져온 API 서버 URL

<div class="content-ad"></div>

7. 기능:

- sqsHandler:
   - Handler: handler.triggerHandler
   - 트리거: 1개의 배치 크기를 가진 SQS 큐 TriggerQueue
- actionsHandler:
   - Handler: actionsHandler.actionsHandler
   - 트리거: 1개의 배치 크기를 가진 SQS 큐 ActionsQueue

8. 리소스

- TriggerQueue:
   - 유형: AWS SQS 큐
   - 이름: trigger_queue
- ActionsQueue:
   - 유형: AWS SQS 큐
   - 이름: actions_queue

<div class="content-ad"></div>

```js
org: bhaskarsawant
app: trigger
service: trigger

plugins:
  - serverless-offline

provider:
  name: aws
  runtime: nodejs20.x
  region: ap-south-1
  iamRoleStatements:
    - Effect: "Allow"
      Action:
        - "sqs:ReceiveMessage"
        - "sqs:DeleteMessage"
        - "sqs:GetQueueAttributes"
        - "sqs:SendMessage"
      Resource:
        - !GetAtt TriggerQueue.Arn
        - !GetAtt ActionsQueue.Arn
  environment:
    OUTPUT_QUEUE_URL:
      Ref: ActionsQueue
    MONGODB_URI: ${env:DB_URL}
    MONGODB_DB: ${env:DB_NAME}
    API_SERVER_URL: ${env:API_URL}

functions:
  sqsHandler:
    handler: handler.triggerHandler
    events:
      - sqs:
          arn:
            Fn::GetAtt:
              - TriggerQueue
              - Arn
          batchSize: 1
  actionsHandler:
    handler: actionsHandler.actionsHandler
    events:
      - sqs:
          arn:
            Fn::GetAtt:
              - ActionsQueue
              - Arn
          batchSize: 1

resources:
  Resources:
    TriggerQueue:
      Type: "AWS::SQS::Queue"
      Properties:
        QueueName: "trigger_queue"
    ActionsQueue:
      Type: "AWS::SQS::Queue"
      Properties:
        QueueName: "actions_queue"
```

이 구성에는 두 개의 AWS 람다 함수가 설정되어 있으며, 이는 두 개의 SQS 큐(trigger_queue와 actions_queue)로부터 메시지를 트리거합니다. 또한 필요한 권한 및 환경 변수를 정의합니다.

이제 AWS에서 서비스를 배포하려면 터미널에 다음 명령어를 입력하세요.

```js
serverless deploy --region ap-south-1
```

<div class="content-ad"></div>


![How to Build Scalable Automated Workflows like HubSpot using AWS Lambda SQS Node.js and MongoDB](/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_4.png)

# AWS SQS와 통합하기

프로젝트의 소스 코드는 여기서 찾을 수 있습니다.

이 섹션에서는 API를 AWS SQS와 통합하는 방법에 대해 알아보겠습니다. AWS 구성을 위한 헬퍼 파일을 생성하여 AWS SQS에 연결하는 설정을 처리할 것입니다.


<div class="content-ad"></div>

## AWS SQS란 무엇인가요?

AWS Simple Queue Service(SQS)는 완전히 관리되는 메시지 대기열 서비스로, 마이크로서비스, 분산 시스템 및 서버리스 애플리케이션의 분리 및 확장을 가능하게 합니다. 소프트웨어 구성 요소간에 메시지를 보내고 저장하며 수신할 수 있도록 지원합니다.

## AWS SQS 통합 방법

## AWS Helper 파일 만들기

<div class="content-ad"></div>

AWS 구성을 위한 새 파일을 만들어야 합니다. 이 도우미 파일에는 AWS SQS의 구성이 포함될 것입니다.

- 파일 생성: 프로젝트 디렉토리에서 awsHelper.ts라는 새 파일을 만듭니다.
- AWS SQS 구성 추가: 다음 코드를 awsHelper.ts에 추가하여 AWS SQS를 구성합니다.

```js
// AWS SDK 가져오기
import AWS from 'aws-sdk';

// AWS SQS 구성
export const sqs = new AWS.SQS({
  accessKeyId: process.env.AWS_ACCESS_KEY_ID,
  secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  region: process.env.AWS_REGION,
  apiVersion: process.env.AWS_API_VERSION,
});
```

이러한 자격 증명은 AWS CLI 구성 섹션에서 찾을 수 있습니다.

<div class="content-ad"></div>

## 핸들러에서 AWS Helper 사용하기

AWS SQS와 상호 작용해야하는 핸들러에서는 AWS 헬퍼 파일에서 sqs 인스턴스를 가져와 사용하세요.

```js
// formController.ts

import { sqs } from './awsHelper';

let params = {
  MessageBody: JSON.stringify(workflowData),
  QueueUrl: process.env.AWS_SQS_URL as string,
};

// 메시지 보내기
sqs.sendMessage(params, (err, data) => {
  if (err) {
    console.log('Error', err);
  } else {
    console.log('Success', data.MessageId);
  }
});
```

서버리스 프레임워크를 사용하여 배포하고 AWS 리소스를 생성한 후,

<div class="content-ad"></div>

AWS 관리 콘솔에 로그인해주세요.

2. SQS로 이동: 검색 창에서 "SQS"를 검색하고 SQS 서비스를 선택합니다.

3. TriggerQueue 찾기:

- 큐 목록에서 TriggerQueue를 찾아주세요.
- TriggerQueue를 클릭하여 세부 정보를 확인합니다.
- TriggerQueue의 URL을 찾아 복사해주세요.

<div class="content-ad"></div>

4. 환경 설정 업데이트:

  - 방금 복사한 TriggerQueue의 URL을 AWS_SQS_URL로 환경 설정을 업데이트하세요.
  - 이 URL은 애플리케이션에서 큐로 메시지를 보내는 데 사용됩니다.

5. 통합 테스트:

  - Postman을 사용하여 API 엔드포인트를 호출하여 큐로 메시지를 보내는 것을 시도하여 테스트하세요.

<div class="content-ad"></div>

![이미지](/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_5.png)

# AWS EC2에 API 서버 배포하기

프로젝트의 소스 코드는 여기에서 찾을 수 있어요.

API 서버와 람다 함수를 철저히 테스트한 후, 다음 단계는 AWS EC2 인스턴스에 API 서버를 배포하는 것입니다. 원활한 배포 프로세스를 위해 다음 단계를 따라 주세요.

<div class="content-ad"></div>

- EC2 인스턴스 시작하기

- AWS 관리 콘솔에 로그인합니다.
- EC2 대시보드로 이동하여 "인스턴스 시작"을 클릭합니다.
- 인스턴스에 이름을 지정하고 운영 체제(Ubuntu)를 선택합니다.
- t2.micro(무료 티어) 또는 t2.small과 같은 인스턴스 유형을 선택합니다.
- 인스턴스에 액세스하는 데 사용할 새 키 페어를 생성합니다.
- 네트워크 설정에서 HTTPS 및 HTTP에서 트래픽을 허용하도록 선택합니다.
- 인스턴스를 검토하고 시작합니다.

2. 포트 8080 노출시키기

- EC2 대시보드에서 인스턴스를 선택합니다.
- 보안 그룹을 클릭하고 인바운드 규칙을 편집하여 포트 8080에서 트래픽을 허용하는 규칙을 추가합니다.

<div class="content-ad"></div>

![이미지1](/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_6.png)

![이미지2](/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_7.png)

3. EC2 인스턴스에 연결하기

- 터미널을 열고 SSH 키에 올바른 권한을 설정하세요.

<div class="content-ad"></div>

```js
chmod 700 Your_SSH_Key.pem
```

- EC2 인스턴스 URL을 사용하여 머신에 SSH로 접속하세요:

```js
ssh -i Your_SSH_Key.pem ubuntu@your-ec2-instance-url
```

4. 프로젝트 설정하기


<div class="content-ad"></div>

- Git 저장소를 복제해 주세요:

```js
git clone https://github.com/bhaskarcsawant/hubspot_workflows
```

- Node.js 설치해 주세요:

```js
// nvm 설치
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash

// nvm 활성화
. ~/.nvm/nvm.sh

// node.js 설치
nvm install 20
```

<div class="content-ad"></div>

- 프로젝트 종속성 설치:

```js
cd your-project

npm install
```

- 환경 변수 추가:

```js
// .env 파일 생성
vim .env

// 다음 변수들을 .env 파일에 추가하고 저장한 후 나가기
MONGODB_URI=your_mongodb_uri
AWS_SQS_URL=your_trigger_sqs_queue_url
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
AWS_REGION=your_aws_region
AWS_API_VERSION=latest
```

<div class="content-ad"></div>

- 서버를 시작하세요:

```js
npm start
```

- 브라우저에서 서버를 방문해보세요:

```js
http://your_ec2_domain:8080 

// 예: http://ec2-54-81-243-55.compute-1.amazonaws.com:8080/
```

<div class="content-ad"></div>

- 서버를 성공적으로 설정한 후 람다 함수의 환경 변수에 API URL을 업데이트하고 다음 명령을 사용하여 업데이트된 서버리스 서비스를 배포하세요.

```js
serverless deploy --region ap-south-1
```

5. 워크플로우 테스트

- Postman을 사용하여 양식을 제출하세요.
- 제출한 양식이 메시지 MongoDB 컬렉션 내에 생성되는지 확인하세요.

<div class="content-ad"></div>

이를 실제로 확인해 봅시다. 😄

이 아키텍처는 가상의 워크플로 시스템을 구축할 수 있게 해줍니다. 이 아키텍처를 사용하면 다양한 유형의 트리거와 작업을 생성할 수 있어요. 이를 통해 좋은 프론트엔드를 사용하여 사용자가 코딩 없이 자동화된 워크플로를 설정할 수 있죠.

![이미지](/assets/img/2024-08-04-HowtoBuildScalableAutomatedWorkflowslikeHubSpotusingAWSLambdaSQSNodejsandMongoDB_8.png)

# 확장성.

<div class="content-ad"></div>

주어진 아키텍처가 확장 가능하다면, 몇 가지 개선 사항과 추가 사항을 통해 효율성과 성능을 향상시킬 수 있습니다. 이러한 개선 사항을 탐색하고 효과적으로 구현하는 방법에 대해 알아봅시다.

## 연결 풀링

- 문제: triggerHandler 람다 함수에서 각 호출마다 새 데이터베이스 연결이 생성되어 데이터베이스를 규모 확장할 때 압도할 수 있습니다.
- 해결책:
  - EC2 인스턴스를 대기열 워커로 사용: 람다 함수 대신 EC2 인스턴스를 대기열 워커로 설정할 수 있습니다. 이를 통해 여러 요청 사이에서 동일한 데이터베이스 연결을 사용하고 수평으로 확장할 수 있습니다.
  - BullMQ 사용: 또 다른 접근 방법은 BullMQ와 같은 서비스를 사용하는 것입니다. BullMQ는 대기열 워커를 제공하고 대기열 메시지를 비동기적으로 처리합니다.

## 오토스케일링 그룹

<div class="content-ad"></div>

- 문제: 부하가 증가함에 따라 수동으로 스케일링하는 것은 비효율적일 수 있고 다운타임을 초래할 수 있습니다.
- 해결책: 수요에 따라 EC2 인스턴스 수를 자동으로 조정하는 오토스케일링 그룹을 구현하세요. 이를 통해 응용 프로그램이 변화하는 부하를 효율적으로 처리할 수 있습니다.

## 확장성을 강화하기 위한 추가 사항

- 로드 밸런서: Elastic Load Balancer(ELB)를 사용하여 들어오는 트래픽을 여러 EC2 인스턴스로 분산시킵니다. 이를 통해 고가용성과 내결함성을 보장할 수 있습니다.
- 캐싱: Redis 또는 Memcached와 같은 캐싱 메커니즘을 구현하여 자주 액세스하는 데이터를 저장하세요. 이렇게 하면 데이터베이스 부하가 감소하고 응답 시간이 향상됩니다.
- 데이터베이스 스케일링: Amazon RDS를 사용하여 읽기 복제본을 배포하여 읽기 부하를 여러 인스턴스로 분산시킵니다.
- 모니터링 및 로깅: AWS CloudWatch 또는 NewRelic을 사용하여 견고한 모니터링 및 로깅을 구현하세요. 이를 통해 응용 프로그램의 성능을 추적하고 문제를 신속하게 식별하고 해결할 수 있습니다.

추가로 확장성을 향상시키는 다른 방법을 찾거나 구현 중 문제가 발생하면 언제든지 의견을 남기거나 저에게 연락해 주세요. 여러분의 피드백은 모두에게 도움이 되며 매우 감사히 받겠습니다. 저는 항상 제안에 열려 있고 도와드릴 준비가 되어 있습니다! 추가로 코드베이스에 개선 사항을 기여해 주세요. 여러분의 기여는 가치 있고 환영받습니다! 😊

<div class="content-ad"></div>

# 더 많은 통찰력을 위해 팔로우하세요

만일 이 안내서가 도움이 되었고 웹 개발, 소프트웨어 엔지니어링 및 다른 흥미로운 기술 주제에 대해 더 배우고 싶다면, 제게 여기 Medium에서 팔로우하세요. 제가 꾸준히 자습서, 통찰력 및 팁을 공유하며 여러분이 더 나은 개발자가 되도록 돕겠습니다. 함께 연결하여 계속 학습하는 이 여정에서 함께 자라요!

# 저와 연락하기

- Twitter: https://twitter.com/Bhaskar_Sawant_
- LinkedIn: https://www.linkedin.com/in/bhaskar-sawant/
- GitHub: https://github.com/bhaskarcsawant

<div class="content-ad"></div>

더 많은 기사를 기대해주세요. 궁금한 점이 있거나 추후 주제에 대한 제안이 있다면 언제든지 연락해 주세요.

읽어 주셔서 감사합니다! 😊

코딩 즐기세요! 🚀