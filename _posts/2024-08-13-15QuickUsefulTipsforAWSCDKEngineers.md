---
title: "AWS CDK 엔지니어를 위한 15가지 빠르고 유용한 팁"
description: ""
coverImage: "/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_0.png"
date: 2024-08-13 11:05
ogImage: 
  url: /assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_0.png
tag: Tech
originalTitle: "15 Quick Useful Tips for AWS CDK Engineers"
link: "https://medium.com/@leejamesgilmore/15-quick-useful-tips-for-aws-cdk-engineers-a7675e1557aa"
---


![이미지](/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_0.png)

## 서문

다음 시나리오 및 코드 조각들을 다루겠습니다:

- 구성을 위한 Convict 사용!
- 배포 단계를 사용하여 속성 변경!
- S3 버킷 자동으로 객체 삭제!
- s3Deploy 패키지를 사용하여 로컬 자산 배포!
- 타입드 스택 구성!
- DynamoDB 테이블 자동으로 채우기!
- 문서 자동 생성을 위한 TypeDoc!
- 변경 사항을 빠르게 배포하기 위한 HotSwap!
- Lambda Powertools 및 Middy!
- 스택 의존성 전달!
- 각 테스트 유형별 Jest 구성!
- Imports를 위한 경로 별칭!
- 클라이언트를 BFF에 가깝게 유지!
- CDK Nag 및 사용자 정의 측면!

<div class="content-ad"></div>

# 소개 👋🏽

이 짧은 기사에서는 AWS CDK 사용자를 위한 코드 조각과 함께 유용한 15가지 팁을 다룰 것입니다. 바로 시작해 봅시다!

# 팁과 요령 🎊

## ✔️ Config를 위한 Convict!

<div class="content-ad"></div>

빠른 시작으로 'convict'라는 멋진 작고 효율적인 패키지를 사용하여 AWS CDK 앱에서 시작해봅시다. 코드 베이스 전반에 process.env.SOMETHING이 흩어져 있는 것보다 더 나쁜 것은 없습니다. 이는 타입이 지정되지 않았고 철자를 틀리게 쓸 수 있다는 문제가 있으며 합리적인 기본값도 없고 앱 구성을 위한 중앙 위치가 없기 때문입니다. 이 모든 문제를 손쉽게 해결할 수 있습니다! 새로운 'convict backed' 구성 파일이 환경 변수를 감싸는 방법을 살펴봅시다:

```js
const convict = require('convict');

export const config = convict({
  stage: {
    doc: 'The stage being deployed',
    format: String,
    default: '',
    env: 'STAGE',
  },
  region: {
    doc: 'The region being deployed to',
    format: String,
    default: 'eu-west-1',
    env: 'REGION',
  },
  tableName: {
    doc: 'The table name',
    format: String,
    default: '',
    env: 'TABLE_NAME',
  }
}).validate({ allowed: 'strict' });
```

그런 다음 다음 구문을 사용하여 코드 베이스에서 필요한 위치에 구성에 액세스할 수 있습니다:

```js
import config from '../config';
...
const tableName = config.get('tableName');
```

<div class="content-ad"></div>

더 많은 정보를 원하시면 다음 링크를 확인해보세요: [https://www.npmjs.com/package/convict](https://www.npmjs.com/package/convict)

## ✔️ 배포 단계를 활용하여 속성을 변경하세요!

우리 애플리케이션을 구축하고 배포할 때 환경 단계에 따라 항상 다른 요구 사항이 있습니다. 몇 가지 예시는 다음과 같습니다:
  
- 일시적 환경을 사용할 때는 각 환경마다 하나씩 생성하는 대신 단일 Amazon OpenSearch 클러스터를 가리키고 싶을 것입니다. 그러나 스테이징 및 프로덕션 환경에서는 각각의 특정 클러스터를 생성하고 싶을 것입니다.
- 일시적인 환경에서는 S3 버킷에서 모든 객체를 지우고 삭제하기 전에 지우고 싶을 수 있지만, 프로덕션 환경에서는 이렇게 하고 싶지 않을 것입니다!

<div class="content-ad"></div>

이것은 응용 프로그램을 구축할 때 만날 수 있는 수백만 가지 중에서 두 가지의 기본 예시에 불과해요! 이런 종류이 아닌 마법 문자열을 사용하는 대신 환경을 입력함으로써 접근할 수 있어요:

```js
export const enum Region {
  dublin = 'eu-west-1',
  london = 'eu-west-2',
  frankfurt = 'eu-central-1',
}

export const enum Stage {
  featureDev = 'featureDev',
  staging = 'staging',
  prod = 'prod',
  develop = 'develop',
}
```

그런 다음 아래와 같이 CDK 로직의 일부로 이러한 값들을 사용할 수 있어요:

```js
this.table = new Table(this, 'Table', {
  removalPolicy:
    props.stageName === Stage.prod
      ? RemovalPolicy.RETAIN
      : RemovalPolicy.DESTROY,
  partitionKey: {
    name: 'id',
    type: dynamodb.AttributeType.STRING,
  },
});
```

<div class="content-ad"></div>

위에서 확인할 수 있듯이, 우리는 만약 스테이지가 프로덕션인 경우 DynamoDB 테이블의 보존 정책을 '보존'으로 설정하고, 그 외의 환경 스테이지에 대해서는 '삭제'로 설정할 것입니다.

## ✔️ S3 버킷 자동 객체 삭제!

가끔씩 AWS CDK 앱에서 일시적인 배포가 파이프라인에서 제거되지 못하는 S3 버킷이 있는 경우가 있습니다. 이때 S3 버킷의 'autoDeleteObjects' 속성을 사용하여 쉽게 해결할 수 있습니다:

```js
import { RemovalPolicy } from 'aws-cdk-lib';

new s3.Bucket(scope, 'Bucket', {
  blockPublicAccess: s3.BlockPublicAccess.BLOCK_ALL,
  encryption: s3.BucketEncryption.S3_MANAGED,
  enforceSSL: true,
  versioned: true,
  removalPolicy: RemovalPolicy.RETAIN,
  autoDeleteObjects: true,
});
```

<div class="content-ad"></div>

프로덕션 환경에서 스택을 실수로 제거하고 파일을 잃어버리는 일이 없도록 이 속성을 파괴되지 않게하는지 확인하세요! 이렇게 하면 될 것 같아요:

```js
...
autoDeleteObjects = props.stage === Stage.prod ? false : true,
...
```

## ✔️ s3Deploy 패키지를 사용하여 로컬 에셋을 배포하세요!

CDK 애플리케이션을 구축하는 경우 로컬 파일을 S3 버킷으로 올리는 것이 이상적인 경우가 많을 것입니다. 예시로는:

<div class="content-ad"></div>

- 웹 앱으로 제공하기 위해 미리 빌드된 정적 클라이언트 앱을 버킷에 업로드하고 싶습니다.
- 환경 설정 파일을 업로드하고자 할 수 있습니다.
- Amazon Bedrock Knowledge Base를 위한 파일을 업로드하고자 할 수도 있습니다.

이러한 다양한 시나리오를 모두 s3Deploy CDK 패키지를 사용하여 쉽게 달성할 수 있습니다. 사용하기 위해, 단순히 다음과 같이 이 construct를 스택에 임포트하면 됩니다:

```js
import * as s3deploy from 'aws-cdk-lib/aws-s3-deployment';
```

<div class="content-ad"></div>

```js
new s3deploy.BucketDeployment(this, 'ClientBucketDeployment', {
    sources: [s3deploy.Source.asset(path.join(__dirname, '../../client/'))],
    destinationBucket: this.bucket,
});
```

위 간단한 코드 스니펫에서 우리는 로컬에서 빌드된 클라이언트 앱(React, Vue 또는 기타)을 가져와 CDK 배포 시 S3 버킷에 넣는 작업을 하고 있어요. 간단하죠!

## ✔️ 타입화된 스택 구성!

애플리케이션을 개발하고 확장해 나갈 때, 컨스트럭트와 애플리케이션 코드로 전달해야 할 많은 정적 구성 값이 있음을 곧 알게 되실 거예요. 그러나 이러한 구성 값들은 일반적으로 환경에 따라 다르다는 것을 알게 될 겁니다.

<div class="content-ad"></div>

우리는 모든 환경(일시적인 환경 포함)에 대한 타이핑된 구성 값을 하나의 파일에 쉽게 만들 수 있습니다. 그런 다음 스택으로 전달될 때 스테이지별 구성을 참조할 수 있습니다:

```js
import * as dotenv from 'dotenv';
import { Region, Stage } from '../../types';
import { EnvironmentConfig } from '../environment-config';

dotenv.config();

export const environments: Record<Stage, EnvironmentConfig> = {
  // 개발자가 작업 중인 PR(예: pr-124)에 대한 빠른 브랜치를 빠르게 생성할 수 있습니다.
  [Stage.develop]: {
    env: {
      account:
        process.env.ACCOUNT || (process.env.CDK_DEFAULT_ACCOUNT as string),
      region: process.env.REGION || (process.env.CDK_DEFAULT_REGION as string),
    },
    stateful: {
      bucketName:
        `your-service-${process.env.PR_NUMBER}-bucket`.toLowerCase(),
    },
    stateless: {
      lambdaMemorySize: parseInt(process.env.LAMBDA_MEM_SIZE || '128'),
    },
    stageName: process.env.PR_NUMBER || Stage.develop,
  },
  [Stage.staging]: {
    env: {
      account: '123456789123',
      region: Region.dublin,
    },
    stateful: {
      bucketName: 'your-service-staging-bucket',
    },
    stateless: {
      lambdaMemorySize: 1024,
    },
    stageName: Stage.staging,
  },
  [Stage.prod]: {
    env: {
      account: '123456789123',
      region: Region.dublin,
    },
    stateful: {
      bucketName: 'your-service-prod-bucket',
    },
    stateless: {
      lambdaMemorySize: 1024,
    },
    stageName: Stage.prod,
  },
};
```

이제 우리는 우리가 생성하는 스택에 올바른 타입의 구성을 가져올 수 있습니다. 이를 위해 스택을 만들 수 있으며, 예를 들어 아래와 같이 특정 환경을 위한 스택을 생성할 수 있습니다:

```js
import { environments } from '../environments';

new StateStack(this, `Develop-${environments.develop.stageName}`, {
  ...environments.develop,
});

const featureDevStack = new StatelessStack(this, 'FeatureDev', {
  ...environments.featureDev,
});

const stagingStack = new StatelessStack(this, 'Staging', {
  ...environments.staging,
});

const prodStack = new StatelessStack(this, 'Prod', {
  ...environments.prod,
});
```

<div class="content-ad"></div>

우리는 스택 프롭 타입으로 사용할 타이핑된 구성 인터페이스를 내보내고 공유할 수도 있어요. 그렇게 하면 항상 동기화되며 어떤 프롭이 스택에 전달되는 지 정확히 알 수 있어요!

```js
...
 constructor(scope: Construct, id: string, props: EnvironmentConfig) {
    super(scope, id, props);
...
```

## ✔️ DynamoDB 테이블 자동으로 채우기!

일시적인 환경에서 작업할 때는 다양한 시나리오에 대해 이미 시드된 데이터가 있으면 항상 좋아요. 예를 들어, 기능 브랜치 환경에서는 이미 다른 항목, 제품 및 상태가 있는 주문이 배포될 수 있어요. 그렇게 하려면 스택이 배포될 때 DynamoDB 테이블에 데이터를 채워넣어야 합니다.

<div class="content-ad"></div>

DynamoDB 테이블 구성 요소의 importSource 속성을 사용하여 작동합니다. 이 속성은 DynamoDB JSON 레코드를 포함하는 S3 버킷을 가리킵니다. 이후 DynamoDB가 이 버킷에서 테이블로 자동으로 가져옵니다:

```js
{"Item":{"PK":{"S":"COMPANY#1"},"SK":{"S":"EMPLOYEE#1#PAYSLIP#1"},"Date":{"S":"2024-05-01"},"Amount":{"N":"1000"},"Type":{"S":"Payslip"}}
{"Item":{"PK":{"S":"COMPANY#1"},"SK":{"S":"EMPLOYEE#1#PAYSLIP#2"},"Date":{"S":"2024-06-01"},"Amount":{"N":"1100"},"Type":{"S":"Payslip"}}
{"Item":{"PK":{"S":"COMPANY#2"},"SK":{"S":"EMPLOYEE#2#PAYSLIP#1"},"Date":{"S":"2024-05-01"},"Amount":{"N":"1200"},"Type":{"S":"Payslip"}}
```

하지만 이 스택을 배포할 때 DynamoDB 레코드의 JSON 파일을 S3 버킷으로 어떻게 가져와야 할까요...아까 우리가 다룬 내용입니다! 자세한 예시는 다음 기사를 참고하세요:

## ✔️ TypeDoc를 사용하여 문서 자동 생성하기!

<div class="content-ad"></div>

AWS CDK 애플리케이션을 구축하는 과정에서 개발자 문서를 자동으로 생성해야 할 필요가 거의 틀림없이 있습니다. 이 문서는 동적이고 pre-commit 후에 다시 생성되어야 합니다.


![Quick Useful Tips for AWS CDK Engineers](/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_1.png)


위의 요구 사항을 충족시킬 수 있는 TypeDoc 패키지를 사용할 수 있습니다. 이를 설정하기 위해 간단히 패키지를 설치합니다:

```js
npm install --save-dev typedoc
```

<div class="content-ad"></div>

우리는 그런 다음 typedoc.json 구성 파일을 만듭니다:

```js
{
  "entryPoints": [
    "./stateless/src/use-cases/create-customer-account/create-customer-account.ts",
    "./stateless/src/use-cases/retrieve-customer-account/retrieve-customer-account.ts",
    "./stateless/src/use-cases/upgrade-customer-account/upgrade-customer-account.ts",
    "./stateless/src/use-cases/create-customer-playlist/create-customer-playlist.ts"
  ],
  "out": "../docs/documentation",
  "theme": "default",
  "name": "Your Service",
  "includeVersion": true,
  "lightHighlightTheme": "light-plus",
  "hideGenerator": true,
  "exclude": ["**/*+(index|.test|.spec|.e2e).ts"],
  "readme": "../docs/DOCS.md",
  "disableSources": true,
  "excludePrivate": true,
  "excludeProtected": true
}
```

그리고 우리의 package.json에 NPM 스크립트를 추가해요, 아래와 비슷한 형태입니다:

```js
"docs": "npx typedoc",
```

<div class="content-ad"></div>

이제 엔지니어가 변경사항을 만들고 커밋할 때마다 Husky와 precommit 훅을 사용하여 문서를 자동으로 생성할 수 있습니다! 자세한 내용은 다음 기사를 참고해주세요:

👇 더 알고 싶다면 — 향후 블로그 포스트와 Serverless 뉴스를 위해 LinkedIn에서 저와 연락해주세요 https://www.linkedin.com/in/lee-james-gilmore/

![이미지](/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_2.png)

## ✔️ 빠르게 변경사항을 배포하기 위한 HotSwap!

<div class="content-ad"></div>

개발 중에는 일시적인 환경에서 람다 함수 또는 StepFunction에 빠르게 변경을 가하고 싶을 때가 있습니다. CloudFormation이 변경 사항을 배포하는 데 걸리는 시간을 기다리기 싫어요 (분이 아닌 초 단위로!)

이러한 상황에 직면했을 때 AWS CDK CLI의 'hotswap' 기능을 사용할 수 있습니다:

```js
cdk deploy --hotswap
```

이 명령을 통해 변경 사항이 적용된 리소스를 즉시 업데이트할 수 있습니다. 예를 들어, AWS CDK 앱의 람다 함수의 코드만 변경했지만 CDK 코드 자체에는 다른 변경 사항이 없는 경우 CloudFormation을 건너뛰고 영향을 받는 리소스를 직접 업데이트하려고 시도합니다. 함께 중첩 스택에있는 리소스에 대한 변경 사항도 포함됩니다.

<div class="content-ad"></div>

도구가 변경을 지원하지 않는 것을 감지하면 ‘핫스왑’을 지원하지 않고 전체 CloudFormation 배포를 수행합니다.

핫스왑할 수 있는 리소스에 대한 자세한 정보를 보려면 다음을 참조하세요:

## ✔️ 람다 파워툴 및 미디!

핵심 팁 중 하나는 Middy와 람다 파워툴 패키지를 조합하여 사용하는 것입니다. 이 패키지는 람다 함수 내에서 로깅, 추적 및 메트릭을 처리해줍니다. handler의 예시는 아래와 같습니다:

<div class="content-ad"></div>

```js
import { MetricUnit, Metrics } from '@aws-lambda-powertools/metrics';
import { errorHandler } from '@shared';
import { APIGatewayProxyEvent, APIGatewayProxyResult } from 'aws-lambda';
import { Logger } from '@aws-lambda-powertools/logger';
import { injectLambdaContext } from '@aws-lambda-powertools/logger/middleware';
import { logMetrics } from '@aws-lambda-powertools/metrics/middleware';
import { Tracer } from '@aws-lambda-powertools/tracer';
import { captureLambdaHandler } from '@aws-lambda-powertools/tracer/middleware';
import { ValidationError } from '@errors/validation-error';
import middy from '@middy/core';

const tracer = new Tracer();
const metrics = new Metrics();
const logger = new Logger();

export const getOrder = async ({
  pathParameters,
}: APIGatewayProxyEvent): Promise<APIGatewayProxyResult> => {
  try {
    if (!pathParameters?.id) {
      throw new ValidationError('id is required');
    }

    const { id } = pathParameters;

    // business logic here

    metrics.addMetric('SuccessfulGetOrder', MetricUnit.Count, 1);

    return {
      statusCode: 200,
      body: JSON.stringify({}), // the response
      headers: {}
    };
  } catch (error) {
    let errorMessage = 'Unknown error';
    if (error instanceof Error) errorMessage = error.message;
    logger.error(errorMessage);

    metrics.addMetric('GetOrderError', MetricUnit.Count, 1);

    return errorHandler(error);
  }
};

export const handler = middy(getOrder)
  .use(injectLambdaContext(logger))
  .use(captureLambdaHandler(tracer))
  .use(logMetrics(metrics));
```

위에서 보듯이, 풍부한 기능을 갖춘 로거, 자동 추적, 및 클라우드워치 메트릭 추가 기능을 얻을 수 있습니다. 클라우드워치 경보 또는 보고용으로 사용할 수 있습니다. 이것과 함께 Middy를 미들웨어로 사용하여 이러한 복잡성을 추상화합니다.

Lambda 함수와 Lambda Powertools를 함께 사용하는 자세한 내용은 다음 기사를 참조하세요:

## ✔️ 스택 의존성 전달하기!

<div class="content-ad"></div>

AWS CDK을 시작할 때, 많은 엔지니어들이 우리 주요 앱에서 생성자를 통해 값을 전달함으로써 서로 다른 스택 간에 값을 전달할 수 있다는 것을 깨닫지 못하는 경우가 많습니다. 아래의 간단한 예제를 살펴보겠습니다:

```js
#!/usr/bin/env node

import 'source-map-support/register';

import * as cdk from 'aws-cdk-lib';

import { ApproachOneStatefulStack } from '../stateful/stateful';
import { ApproachOneStatelessStack } from '../stateless/stateless';

const app = new cdk.App();

const statefulStack = new ApproachOneStatefulStack(
  app,
  'ApproachOneStatefulStack',
  {}
);

// stateless 스택으로 table 참조를 전달합니다.
new ApproachOneStatelessStack(app, 'ApproachOneStatelessStack', {
  table: statefulStack.table, // <-- 여기에 참조가 있습니다
});
```

우리는 DynamoDB 테이블의 값을 stateful 스택에서 stateless 스택으로 전달했다는 것을 알 수 있습니다. 이는 stateless 스택에서 테이블을 다음과 같이 참조할 수 있음을 의미합니다:

```js
import * as apigw from 'aws-cdk-lib/aws-apigateway';
import * as cdk from 'aws-cdk-lib';
import * as dynamodb from 'aws-cdk-lib/aws-dynamodb';
import * as lambda from 'aws-cdk-lib/aws-lambda';
import * as nodeLambda from 'aws-cdk-lib/aws-lambda-nodejs';
import * as path from 'path';

import { Construct } from 'constructs';

export interface ApproachOneStatelessStackProps extends cdk.StackProps {
  table: dynamodb.Table;
}

export class ApproachOneStatelessStack extends cdk.Stack {
  private table: dynamodb.Table;

  constructor(
    scope: Construct,
    id: string,
    props: ApproachOneStatelessStackProps
  ) {
    super(scope, id, props);

    const { table } = props;

    this.table = table;

    // 새로운 고객을 생성하는 람다 함수를 생성합니다
    const createCustomerLambda: nodeLambda.NodejsFunction =
      new nodeLambda.NodejsFunction(this, 'CreateCustomerLambda', {
        functionName: 'create-customer-lambda',
        runtime: lambda.Runtime.NODEJS_20_X,
        entry: path.join(
          __dirname,
          'src/adapters/primary/create-customer/create-customer.adapter.ts'
        ),
        memorySize: 1024,
        handler: 'handler',
        tracing: lambda.Tracing.ACTIVE,
        bundling: {
          minify: true,
        },
        environment: {
          TABLE_NAME: this.table.tableName, // 이는 테이블 참조를 사용합니다
        },
      });

    this.table.grantWriteData(createCustomerLambda);
  }
}
```

<div class="content-ad"></div>

더 자세한 정보를 원하시면 다음 기사를 확인해주세요:

## ✔️ 테스트 유형별 Jest 설정!

AWS CDK 애플리케이션을 테스트할 때, CDK와 함께 기본 설치된 Jest를 사용하여 다양한 테스트 전략을 적용할 수 있습니다:

- 단위 테스트 (비즈니스 로직 코드 및 CDK 인프라).
- e2e 테스트 (끝에서 끝).
- 통합 테스트 (인프라 일부).
- 모듈 통합 테스트 (예를 들어 DynamoDB 모듈을 통합 테스트).

<div class="content-ad"></div>

개인적으로 저 자신이 모듈식으로 구성을 설정하는 가장 효과적인 방법은 각 테스트 유형별로 다른 Jest 구성을 만들고 테스트를 이에 맞게 명명하는 것입니다. e2e 테스트용 Jest 구성의 예시는 아래와 같습니다:

```js
module.exports = {
  testEnvironment: 'node',
  roots: ['<rootDir>'],
  testMatch: ['**/*.e2e.ts'],
  transform: {
    '^.+\\.tsx?$': 'ts-jest',
  },
  setupFiles: ['dotenv/config'],
};
```

그런 다음 다음과 같이 보이는 npm 스크립트를 만들면 됩니다:

```js
...
"test:e2e": "jest --config ./jest.config.e2e.js --detectOpenHandles --runInBand",
"test:e2e:watch": "jest --config ./jest.config.e2e.js --watch --detectOpenHandles --runInBand"
...
```

<div class="content-ad"></div>

이제 e2e 테스트를 만들 때는 루트에 tests/e2e 폴더를 만들고 다음과 같이 이름을 지정할 것입니다:

![이미지](/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_3.png)

통합 테스트와 마찬가지로 integration 테스트를 만들 수 있습니다. 예를 들어 create-order-api-to-dynamodb.integration.ts와 같은 이름을 사용할 수 있습니다. 또한 단위 테스트는 *.test.ts 접미사를 사용하여 customer-use-case.test.ts와 같이 이름을 지을 수 있습니다.

더 자세한 내용은 아래의 자세한 기사를 참조해주세요:

<div class="content-ad"></div>

## ✔️ Imports를 위한 경로 별칭!

앱이 커질수록 폴더 구조와 하위 폴더 및 파일 수도 증가합니다. 이는 다음과 같이 매우 복잡하고 지저분한 import 경로로 이어질 수 있습니다:

```js
import { something } from '../../../../../src/use-cases/something/something';
```

이 문제를 해결하기 위해 우리는 barrel 파일과 TypeScript 경로 별칭을 사용하여 다음과 같이 우리의 Imports를 보기 좋게 만들 수 있습니다:

<div class="content-ad"></div>

```js
import { something } from '@use-cases/something';
```

우리는 단순히 tsconfig.json 파일을 업데이트하여 baseURL과 재작성하려는 경로를 포함시킴으로써 이 작업을 수행할 수 있습니다:

```js
{
  "compilerOptions": {
    ...
    "baseUrl": ".",
    "paths": {
      "@adapters/*": ["./stateless/src/adapters/*"],
      "@config/*": ["./stateless/src/config/*"],
      "@domain/*": ["./stateless/src/domain/*"],
      "@entity/*": ["./stateless/src/entity/*"],
      "@repositories/*": ["./stateless/src/repositories/*"],
      "@errors/*": ["./stateless/src/errors/*"],
      "@schemas/*": ["./stateless/src/schemas/*"],
      "@shared/*": ["./stateless/src/shared/*"],
      "@models/*": ["stateless/src/models/*"],
      "@dto/*": ["stateless/src/dto/*"],
      "@use-cases/*": ["./stateless/src/use-cases/*"],
      "@packages/*": ["./packages/*"],
      "@events/*": ["stateless/src/events/*"]
    }
  },
  ...
}
```

위에서 볼 수 있듯이 use-cases 경로가 이제 ./stateless/src/use-cases/* 아래로 매핑되어 있어서 우리가 좋은 임포트를 수행할 수 있게 됩니다.

<div class="content-ad"></div>

알아두어야 할 한 가지는 Jest 구성에도 비슷한 변경을 해야만 테스트가 계속 작동한다는 점입니다. 아래 내용을 추가하여 변경사항을 적용하세요:

```js
...
moduleNameMapper: {
    '^@adapters/(.*)': '<rootDir>/stateless/src/adapters/$1',
    '^@config/(.*)': '<rootDir>/stateless/src/config/$1',
    '^@domain/(.*)': '<rootDir>/stateless/src/domain/$1',
    '^@entity/(.*)': '<rootDir>/stateless/src/entity/$1',
    '^@schemas/(.*)': '<rootDir>/stateless/src/schemas/$1',
    '^@shared/(.*)': '<rootDir>/stateless/src/shared/$1',
    '^@errors/(.*)': '<rootDir>/stateless/src/errors/$1',
    '^@repositories/(.*)': '<rootDir>/stateless/src/repositories/$1',
    '^@events/(.*)': '<rootDir>/stateless/src/events/$1',
    '^@models/(.*)': '<rootDir>/stateless/src/models/$1',
    '^@dto/(.*)': '<rootDir>/stateless/src/dto/$1',
    '^@use-cases/(.*)': '<rootDir>/stateless/src/use-cases/$1',
    '^@packages/(.*)': '<rootDir>/packages/$1',
  },
...
```

이제 코드가 더 깔끔해지고 복잡한 import 경로를 고민할 필요가 없어졌습니다.

## ✔️ 클라이언트를 BFF와 가까이 유지하세요!

<div class="content-ad"></div>

자주 발견되는 실수 중 하나는 엔지니어링 팀이 클라이언트 애플리케이션을 지원하는 BFF(Backend for Frontend)와 별도의 코드 저장소에 두는 것입니다. 그럴 경우 타입, API 계약 등이 동기화되지 않을 수 있습니다. 두 파이프라인이 모두 정상이더라도요!

그래서 제안드리는 것은 두 요소를 동일한 저장소에 두고 AWS CDK 앱을 통해 함께 배포하는 것입니다! 필요한 것은 세 개의 스택이 있을 뿐입니다:

- Stateful: 데이터베이스와 같은 상태 유지형 리소스를 위한 스택입니다.
- Stateless: API 게이트웨이와 람다 함수와 같은 상태 무상태 리소스를 위한 스택입니다.
- Client: 클라이언트 스택은 S3 버킷, CloudFront 배포 및 클라이언트 앱을 호스팅하기 위한 모든 구성을 다룹니다.

CDK 구조인 s3Deploy를 사용하여 이전에 보았던 방식을 활용하여 클라이언트 앱을 동일한 저장소에서 S3 버킷으로 자동으로 배포할 수 있습니다!

<div class="content-ad"></div>

```js
...
new s3deploy.BucketDeployment(this, 'ClientBucketDeployment', {
  sources: [
    s3deploy.Source.asset(path.join(__dirname, '../../web/build')),
    s3deploy.Source.jsonData('config.json', {
      stage,
      domainName,
      subDomain,
      api,
    }), // runtime config for client
  ],
  destinationBucket: this.bucket,
  distribution: cloudFrontDistribution,
  distributionPaths: ['/*'],
});
...
```

지금은 클라이언트와 BFF가 하나의 단위로 배포되므로 결코 동기화되지 않습니다. 이는 파이프라인 테스트에서의 신뢰를 크게 높입니다.

## ✔️ CDK Nag 및 사용자 정의 측면!

마지막으로 위의 모든 작업을 수행할 수 있지만 여전히 우리가 최선의 규칙과 표준을 따르고 있는지 어떻게 알 수 있을까요?

<div class="content-ad"></div>

한 가지 방법은 AWS 솔루션, HIPAA 보안, NIST 800-53 rev 4, NIST 800-53 rev 5 및 PCI DSS 3.2.1과 관련된 주어진 규칙 팩을 준수하는 우리의 구성 요소를 검증하는 CDK Nag를 AWS CDK 응용 프로그램에 추가하는 것입니다.

또한 AWS CDK의 Aspects라는 기능을 활용하여 우리만의 규정 세트, 검사 또는 문제 수정을 만들 수 있습니다. 다음 기사에서 다양한 예제를 살펴볼 수 있습니다:

더 자세한 기사 및 예제를 보려면 Serverless Advocate Patterns & Solutions 레지스트리를 자유롭게 참조해 주세요:

![image](/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_4.png)

<div class="content-ad"></div>

# 마무리 인사 👋🏽

이 짧은 글을 즐겁게 읽어 주셨기를 바랍니다. 만약 즐겁게 읽으셨다면 공유해주시고 피드백도 부탁드립니다!

비슷한 콘텐츠를 원하신다면 제 유튜브 채널을 구독해주세요!

![이미지](/assets/img/2024-08-13-15QuickUsefulTipsforAWSCDKEngineers_5.png)

<div class="content-ad"></div>

다음 링크 중 하나로 저와 연결하고 싶습니다:

- [LinkedIn](https://www.linkedin.com/in/lee-james-gilmore/)
- [Twitter](https://twitter.com/LeeJamesGilmore)

만약 이 글을 즐겼다면, 제 프로필 Lee James Gilmore를 팔로우하여 더 많은 글/시리즈를 읽어보세요. 그리고 연결하고 인사도 잊지마세요! 👋

또한, 이 글 하단에 있는 '박수' 기능을 사용하여 즐겼다면 박수를 칠 수도 있습니다! (한 번 이상 박수를 칠 수 있어요!)

<div class="content-ad"></div>

# 나에 대해

안녕하세요, 저는 영국을 기반으로 활동 중인 AWS 커뮤니티 빌더이자 블로거이며 AWS 인증 클라우드 아키텍트이자 주요 클라우드 아키텍트이자 클라우드 실무 리드인 이입니다. 지난 10년간 AWS에서 주로 풀스택 JavaScript를 사용해 왔습니다.

저는 서버리스를 옹호하는 사람으로, AWS, 혁신, 소프트웨어 아키텍처, 기술에 대한 애정이 있습니다.

*** 제공된 정보는 제 개인적인 견해이며 해당 정보의 사용에 대한 책임을 지지 않습니다. ***

<div class="content-ad"></div>

다음 내용도 확인해 보세요: