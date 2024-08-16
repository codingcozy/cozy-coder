---
title: "S3 파일 업로드에 반응하는 AWS Lambda 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-07-AwsLambdathatReactstoS3FileUpload_0.png"
date: 2024-07-07 23:13
ogImage: 
  url: /assets/img/2024-07-07-AwsLambdathatReactstoS3FileUpload_0.png
tag: Tech
originalTitle: "Aws Lambda that Reacts to S3 File Upload"
link: "https://medium.com/gitconnected/aws-lambda-that-reacts-to-s3-file-upload-4d16b08fede1"
isUpdated: true
---





![Architecture](/assets/img/2024-07-07-AwsLambdathatReactstoS3FileUpload_0.png)

오늘은 파일을 AWS S3에 업로드하고, 파일이 성공적으로 업로드되면 람다가 트리거되는 아키텍처를 디자인하는 방법을 살펴보겠습니다.

해당 람다는 파일을 다운로드하고 그 위에 몇 가지 작업을 수행할 것입니다. 일부 가능한 옵션은 다음과 같습니다.

- 전체 크기 이미지의 썸네일 버전 생성
- 엑셀 파일에서 데이터 읽기

<div class="content-ad"></div>

Ana씨 여러분

코드의 최종 버전은 다음 위치에서 확인할 수 있습니다.

# 프로젝트 초기화

이 프로젝트는 aws sam을 사용하여 진행합니다. 이 프로젝트에는 TypeScript 설정을 지원하는 보일러플레이트를 사용할 것입니다. 아래 저장소를 복제하여 시작할 수 있습니다.