---
title: "Nodejs를 사용한 과제 관리 마이크로서비스 개발 방법"
description: ""
coverImage: "/assets/img/2024-07-07-DevelopingaTaskManagementMicroservicewithNodejs_0.png"
date: 2024-07-07 23:13
ogImage: 
  url: /assets/img/2024-07-07-DevelopingaTaskManagementMicroservicewithNodejs_0.png
tag: Tech
originalTitle: "Developing a Task Management Microservice with Node.js"
link: "https://medium.com/@sameemabbas/developing-a-task-management-microservice-with-node-js-f51bfdc24626"
isUpdated: true
---





![이미지](/assets/img/2024-07-07-DevelopingaTaskManagementMicroservicewithNodejs_0.png)

이 문서에서는 Node.js를 사용하여 Task Management 웹 서비스를 예로 들어 마이크로서비스를 구축하는 방법을 보여드리겠습니다. 이 마이크로서비스는 작업을 만들고, 검색하고, 업데이트할 수 있는 간단하면서도 강력한 API를 제공할 것입니다. 우리는 인기 있는 Node.js 스택 구성 요소를 사용하여 마이크로서비스를 확장 가능하고 견고하며 신뢰성 있게 그리고 성능을 높이는 데 중점을 둘 것입니다.

# API 개요

우리의 Task Management API는 다음 작업을 지원할 것입니다:

<div class="content-ad"></div>

- 작업 생성: 이름과 설명을 가진 작업을 생성하고 상태를 'new'로 설정할 수 있습니다.
- 작업 가져오기: 작업을 식별자로 가져옵니다.
- 작업 업데이트: 작업의 상태, 이름 및 설명을 업데이트합니다.

## 어플리케이션 요구 사항

- 작업 생성: 새로운 작업은 상태 'new'로 생성되어야 합니다.
- 상태 전환:
  - 'new' → 'active'
  - 'new' → 'canceled'
  - 'active' → 'completed'
  - 'active' → 'canceled'
- 경합 조건 회피: 경합 조건 없이 일관된 업데이트를 보장합니다.

## 기능적 요구 사항

<div class="content-ad"></div>

- 확장성: 증가하는 요청을 효율적으로 처리합니다.
- 탄력성: 트래픽 증가에도 원활하게 대응합니다.
- 성능: 사용자 경험을 향상시키기 위한 신속한 응답 제공.
- 복원력: 장애에 터래반하고 우아하게 복구할 수 있습니다.
- 모니터링 및 관찰성: 상태, 로그 및 메트릭을 추적합니다.
- 테스트 용이성: 서비스가 쉽게 테스트될 수 있도록 합니다.
- 상태 무관성: 클라이언트 컨텍스트를 서비스가 아닌 데이터베이스에 저장합니다.
- 배포 용이성: 쉽게 배포하고 업데이트할 수 있습니다.

# 기술 스택

## Node.js

Node.js는 속도, 큰 커뮤니티 및 프론트엔드 및 백엔드 개발에 적합한 점으로 선택되었습니다.

<div class="content-ad"></div>

## 데이터베이스

MongoDB: 스키마가 없는 저장공간을 제공하는 문서 중심 NoSQL 데이터베이스로, 다음과 같은 특징을 가지고 있습니다:

- 스키마의 부재: 컬렉션은 서로 다른 스키마를 가진 문서를 담을 수 있습니다.
- 확장성: 수평 확장을 위해 설계되었습니다.
- 성능: 읽기 중심의 작업을 위해 최적화되었습니다.

## 웹 프레임워크

<div class="content-ad"></div>

친구 같이 해석해드릴게요:

## 유효성 검사

Joi: 강력한 스키마 설명 및 데이터 유효성 검사 라이브러리입니다. Express-mongo-sanitize: MongoDB 연산자 삽입을 방지하기 위한 미들웨어입니다.

## 설정

<div class="content-ad"></div>

Dotenv: .env 파일에서 환경 변수를 로드하는 모듈이에요.

## 정적 분석

ESLint: ECMAScript/JavaScript 코드에서 발견된 패턴을 식별하고 보고하는 도구예요. 코드 품질에 중점을 둔다구요.

## 테스트

<div class="content-ad"></div>

Jest: 코드의 정확성을 단위 및 통합 테스트를 통해 보증하는 JavaScript 테스트 프레임워크입니다.

## 로깅

Winston: 다양한 전송을 지원하는 유연한 로깅 라이브러리입니다.

## 메트릭스

<div class="content-ad"></div>

프로메테우스 미들웨어: 표준 웹 애플리케이션 메트릭 수집용.

## 모니터링 스택

- 프로메테우스: 모니터링 및 경보 툴킷
- 프롬테일: 로그 수집기 및 발송기
- 로키: 로그 집계 시스템
- 그라파나: 시각화 및 분석 플랫폼

## 로컬 인프라estructure

<div class="content-ad"></div>

Docker: 프로덕션과 비슷한 로컬 환경을 만들기 위해 사용합니다. Docker Compose: 멀티 컨테이너 Docker 애플리케이션을 정의하고 관리하기 위해 사용합니다.

# 지속적 통합

GitHub Actions: CI/CD를 위해 사용되며, 새로운 커밋이 빌드를 망치지 않도록합니다.

# 애플리케이션 개발

<div class="content-ad"></div>

위의 스택을 사용하여 MongoDB 백엔드를 가진 Node.js 애플리케이션을 생성할 예정입니다.

## 프로젝트 구조

다음은 디렉토리 구조의 예시입니다:

```js
.
├── src
│   ├── config
│   │   ├── config.js
│   │   ├── logger.js
│   ├── controllers
│   │   └── task.js
│   ├── middlewares
│   │   ├── validate.js
│   │   └── error.js
│   ├── models
│   │   └── task.js
│   ├── routes
│   │   ├── v1
│   │   │   └── task.js
│   ├── services
│   │   └── task.js
│   ├── utils
│   │   └── pick.js
│   ├── validations
│   │   └── task.js
│   ├── app.js
│   ├── index.js
├── tests
│   ├── integration
│   │   └── task.test.js
│   ├── unit
│   │   └── task.test.js
├── .env
├── .eslintrc.json
├── docker-compose.yml
├── Dockerfile
├── jest.config.js
└── package.json
```

<div class="content-ad"></div>

# 샘플 코드 스니펫

## 작업 모델

Mongoose를 사용하여 작업 스키마를 정의합니다:

```js
const mongoose = require('mongoose');
const { Schema } = mongoose;

const TaskSchema = new Schema(
  {
    name: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: false,
    },
    status: {
      type: String,
      enum: ['new', 'active', 'completed', 'cancelled'],
      default: 'new',
    },
    createdAt: {
      type: Date,
      default: Date.now,
    },
    updatedAt: Date,
  },
  { optimisticConcurrency: true }
);

module.exports = mongoose.model('task', TaskSchema);
```

<div class="content-ad"></div>

## 작업 컨트롤러

작업 업데이트를 처리하는 컨트롤러를 구현하세요:

```js
const updateTaskById = catchAsync(async (req, res) => {
  const result = await taskService.updateTaskById(req.params.id, req.body);
  if (result.error) {
    switch (result.code) {
      case taskService.errorCodes.AT_LEAST_ONE_UPDATE_REQUIRED_CODE:
        res.status(400).json({ success: false, message: '최소한 하나의 업데이트가 필요합니다' });
        return;
      case taskService.errorCodes.INVALID_STATUS_CODE:
        res.status(400).json({ success: false, message: '유효하지 않은 상태' });
        return;
      case taskService.errorCodes.INVALID_STATUS_TRANSITION_CODE:
        res.status(404).json({ success: false, message: '작업을 찾을 수 없습니다' });
        return;
      case taskService.errorCodes.TASK_NOT_FOUND_CODE:
        res.status(400).json({ success: false, message: result.error });
        return;
      case taskService.errorCodes.CONCURRENCY_ERROR_CODE:
        res.status(500).json({ success: false, message: '동시성 오류' });
        return;
      default:
        res.status(500).json({ success: false, message: '내부 서버 오류' });
        return;
    }
  }

  res.status(200).json({
    success: true,
    task: toDto(result),
  });
});
```

## Task 서비스

<div class="content-ad"></div>

비즈니스 규칙 및 데이터 지속성을 처리하는 서비스 로직을 구현하였습니다:

```js
async function updateTaskById(id, { name, description, status }) {
  if (!name && !description && !status) {
    return { error: '적어도 하나의 업데이트가 필요합니다', code: AT_LEAST_ONE_UPDATE_REQUIRED_CODE };
  }

  if (status && !(status in availableUpdates)) {
    return { error: '유효하지 않은 상태입니다', code: INVALID_STATUS_CODE };
  }
  
  for (let retry = 0; retry < 3; retry += 1) {
    const task = await Task.findById(id);
    if (!task) {
      return { error: '작업을 찾을 수 없습니다', code: TASK_NOT_FOUND_CODE };
    }
    if (status) {
      const allowedStatuses = availableUpdates[task.status];
      if (!allowedStatuses.includes(status)) {
        return {
          error: `'${task.status}'에서 '${status}'(으)로 업데이트할 수 없습니다`,
          code: INVALID_STATUS_TRANSITION_CODE,
        };
      }
    }
    task.status = status ?? task.status;
    task.name = name ?? task.name;
    task.description = description ?? task.description;
    task.updatedAt = Date.now();
    try {
      await task.save();
    } catch (error) {
      if (error.name === 'VersionError') {
        continue;
      }
    }
    return task;
  }
  return { error: '동시성 오류', code: CONCURRENCY_ERROR_CODE };
}
```

## 라우트

라우트를 등록하고 유효성 검사 미들웨어를 적용하세요:

<div class="content-ad"></div>


## 테이블 태그를 전환하세요.

```js
const { Router } = require('express');
const taskController = require('../../../controllers/task');
const taskValidation = require('../../../validation/task');
const validate = require('../../../middlewares/validate');

const router = Router();
router.get('/:id', validate(taskValidation.getTaskById), taskController.getTaskById);
router.put('/', validate(taskValidation.createTask), taskController.createTask);
router.post('/:id', validate(taskValidation.updateTaskById), taskController.updateTaskById);

module.exports = router;
```

## 테스트

API 기능을 확인하기 위해 통합 테스트를 구현하세요.

```js
describe('Task API', () => {
  setupServer();

  it('할 일 생성 및 업데이트해야 함', async () => {
    let response = await fetch('/v1/tasks', {
      method: 'put',
      body: JSON.stringify({
        name: 'Test Task',
        description: 'Task description',
      }),
      headers: { 'Content-Type': 'application/json' },
    });
    expect(response.status).toEqual(201);
    const result = await response.json();
    expect(result.success).toBe(true);
    const taskId = result.task.id;

    response = await fetch(`/v1/tasks/${taskId}`, {
      method: 'post',
      body: JSON.stringify({ status: 'active' }),
      headers: { 'Content-Type': 'application/json' },
    });
    expect(response.status).toEqual(200);
    const updateResult = await response.json();
    expect(updateResult.success).toBe(true);
    expect(updateResult.task.status).toBe('active');
  });
});
```

<div class="content-ad"></div>

# 배포

도커와 도커 컴포즈를 사용하여 마이크로서비스를 배포하세요. 아래는 도커 컴포즈 예시입니다:

```js
version: '3.8'
services:
  mongo:
    image: mongo
    container_name: mongodb
    ports:
      - '27017:27017'
    volumes:
      - mongo-data:/data/db

  api:
    build: .
    container_name: task_api
    ports:
      - '3000:3000'
    depends_on:
      - mongo
    environment:
      - MONGO_URL=mongodb://mongo:27017/taskdb
    volumes:
      - .:/usr/src/app

volumes:
  mongo-data:
```

# 결론

<div class="content-ad"></div>

이 가이드를 따라하면 업무를 관리하는 강력하고 확장 가능하며 신뢰할 수 있는 마이크로서비스를 만들 수 있습니다. 이 아키텍처는 마이크로서비스가 문제 없이 동시 업데이트를 처리할 수 있고, 원활한 배포를 지원하며 쉬운 확장성과 모니터링을 제공합니다.