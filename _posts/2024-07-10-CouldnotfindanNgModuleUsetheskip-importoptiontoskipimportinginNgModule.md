---
title: "NgModule을 찾을 수 없어요 skip-import 옵션으로 NgModule에 포함하지 않는 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 01:01
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Could not find an NgModule. Use the skip-import option to skip importing in NgModule"
link: "https://medium.com/@fixitblog/solved-could-not-find-an-ngmodule-use-the-skip-import-option-to-skip-importing-in-ngmodule-bde455791dcd"
isUpdated: true
---





이 게시물 제목에 있는 오류를 해결하려고 할 때 CLI를 사용하여 새 컴포넌트를 생성하는 경우 ng g c my-component --project=my-project(컴포넌트가 생성되지 않음) 오류가 발생합니다.

동일한 오류 메시지를 포함한 다른 게시물을 보았는데, Nrwl/Nx가 관련되어 있다고 생각됩니다. 프로젝트는 Ng4로 시작하여 Ng6으로 업그레이드되었습니다.

환경:

- Angular CLI: 6.0.8
- Node: 8.9.4
- OS: win32 x64
- Angular: 6.0.6
- @angular-devkit/architect 0.6.8
- @angular-devkit/build-angular 0.6.8
- @angular-devkit/build-optimizer 0.6.8
- @angular-devkit/core 0.6.8
- @angular-devkit/schematics 0.6.8
- @angular/cdk 6.3.1
- @angular/cli 6.0.8
- @angular/material 6.3.1
- @ngtools/webpack 6.0.8
- @schematics/angular 0.6.1
- @schematics/update 0.6.8
- rxjs 6.2.1
- typescript 2.7.2
- webpack 4.8.3
- nrwl/nx 6.1.0

<div class="content-ad"></div>

업데이트 #1

저는 `--project` 플래그를 완전히 삭제해 보았지만 여전히 동일한 문제가 발생했습니다. 아직 무슨 뜻인지는 잘 모르겠네요.

# 해결책

마침내 `node_modules`를 완전히 삭제하고 yarn 캐시를 비우고 모든 패키지를 재설치하기 위해 yarn을 실행했습니다. 또한 모든 애플리케이션 및 라이브러리에 대해 경로가 아래와 같이 보이도록 angular.json 파일을 수정했습니다:

<div class="content-ad"></div>


"root": "libs/my-lib",
"sourceRoot": "libs/my-lib/src"


그 중 어떤 것이 문제를 해결했는지 정확히는 모르겠지만, 모든 것이 지금 괜찮아 보입니다. 적어도 당장의 문제는 해결된 것 같네요.

답변자: TimTheEnchanter

답변 확인자: Senaida (FixIt 자원봉사자)


<div class="content-ad"></div>

이 답변은 stackoverflow에서 수집되었으며, cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0 의 라이센스를 따릅니다.