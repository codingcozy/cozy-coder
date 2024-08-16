---
title: "데이터 엔지니어링에서 가장 흔히 쓰이는 리눅스 명령어 10가지"
description: ""
coverImage: "/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_0.png"
date: 2024-07-12 22:55
ogImage: 
  url: /assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_0.png
tag: Tech
originalTitle: "Most Common Linux Commands in Data Engineering"
link: "https://medium.com/gitconnected/most-common-linux-commands-in-data-engineering-7cea3e60ec4f"
---


데이터 엔지니어링 과정을 시작하기 전에 이 내용을 꼭 알아야 해요

![링크](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_0.png)

2019년에 데이터 엔지니어가 되어야 한다는 결심을 했을 때, 몇 가지 리눅스 명령어에 익숙해져야 하는 중요성에 대해 깨달았어요.

그리고 그 결정은 옳았어요! 😁

<div class="content-ad"></div>

데이터 톡 클럽에서 데이터 엔지니어링 Zoomcamp을 수강했을 때 몇 가지 기본 명령이 머릿 속에 남아 있어야 하고 다른 고급 명령을 찾기 위해 구글링하는 방법을 알아야 한다는 것을 알게 되었어요.

리눅스 명령어를 아시나요?

리눅스를 주 운영 체제로 사용하는 컴퓨터가 필요하지 않아요.

리눅스 명령어를 사용하는 방법은:

<div class="content-ad"></div>

- Windows Subsystem Linux (WSL);
- Git Bash Terminal;
- Remote Github Codespaces.

이 글에서는 항상 알아야 할 기본적인 Linux 명령어를 배울 수 있습니다.

# Linux 명령어의 중요성

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_1.png)

<div class="content-ad"></div>

리눅스 명령어는 당신과 같은 데이터 엔지니어에게 중요한 도구입니다.

데이터 엔지니어링의 다양한 측면에서 왜 핵심적인지 살펴봅시다:

데이터 파이프라인 및 자동화:

- 데이터 파이프라인 구축 및 자동화 스크립팅과 같은 작업에 리눅스를 사용할 것입니다. (예: GitLab CI/CD)
- 리눅스 명령어를 사용하여 파일 조작, 디렉토리 탐색 및 프로세스 관리와 같은 작업을 수행할 수 있습니다.

<div class="content-ad"></div>

리소스 모니터링 및 최적화:

리눅스의 가벼운 특성과 효율적인 리소스 관리는 데이터 엔지니어링 작업에 이상적인 선택지가 됩니다.

터미널 명령어를 사용하여:

- 시스템 리소스( CPU, 메모리, 디스크 사용량)를 모니터링합니다.
- 프로세스를 최적화합니다(예: 프로세스 우선 순위 조정).
- 성능 문제를 해결합니다(리소스 병목 현상 식별).

<div class="content-ad"></div>

도커 및 컨테이너화:

도커는 데이터 엔지니어에게 혁신적인 변화를 가져다줍니다. 개발자들은 컨테이너를 사용하여 손쉽게 응용 프로그램이나 모델을 배포할 수 있게 해줍니다.

- 리눅스는 도커의 기본 플랫폼입니다.
- 당신과 다른 데이터 엔지니어들은 리눅스 명령어를 이해하여 도커 컨테이너를 만들고 관리하고 문제 해결하는 데 필요합니다.

깊은 디버깅 및 문제 해결:

<div class="content-ad"></div>

시스템이 문제가 발생할 때 (자주 그렇습니다), Linux 명령어를 활용하여 심층 디버깅을 수행할 수 있어요.

- 로그 파일을 검사하고 시스템 동작을 분석하며, 데이터 파이프라인, 데이터베이스 또는 서비스와 관련된 문제를 진단할 수 있어요.
- Linux 명령어에 익숙해지면 문제 해결을 효율적으로 수행할 수 있고, 데이터 신뢰성을 보장할 수 있어요.

# 가장 흔한 Linux 명령어

이 섹션에서는 '암속'으로 알고 있어야 하는 가장 기본적인 Linux 명령어를 배울 수 있습니다.

<div class="content-ad"></div>

GitHub Codespaces를 사용하는 프린트 스크린도 몇 개 보게 될 거예요.

## ls (List)

ls 명령어는 디렉토리의 내용을 나열합니다.

기본적으로 현재 디렉토리 내의 파일과 하위 디렉토리를 표시합니다.

<div class="content-ad"></div>

여기 유용한 변형 몇 가지를 볼 수 있어요:

- ls: 현재 디렉토리의 파일과 디렉토리 목록을 표시합니다.
- ls -l: 긴 형식으로 목록을 표시합니다 (권한, 소유자, 크기 등 포함).
- ls -a: 숨겨진 파일을 표시합니다 (점으로 시작하는 파일).

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_2.png)

## pwd (현재 작업 디렉토리 출력)

<div class="content-ad"></div>

pwd 명령은 현재 작업 디렉토리(즉, 파일 시스템 내 현재 위치)를 표시합니다.

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_3.png)

## cd (디렉토리 변경)

cd 명령을 사용하여 디렉토리 간에 전환할 수 있습니다. 다음은 몇 가지 예시입니다:

<div class="content-ad"></div>

- cd ..: 상위 폴더로 이동합니다 (부모 디렉토리로).
- cd /경로/대상/디렉토리: 절대 경로로 이동합니다.
- cd : 루트로 이동합니다

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_4.png)

## echo

echo 명령어는 텍스트를 터미널에 출력합니다.

<div class="content-ad"></div>

일반적으로 메시지를 표시하거나 간단한 스크립트를 만드는 데 사용됩니다.

- echo "Hello, World!": 지정된 텍스트를 출력합니다.
- echo "새 파일에 대한 정보" ` file.txt: echo의 정보로 새 파일 생성

![이미지1](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_5.png)

![이미지2](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_6.png)

<div class="content-ad"></div>

## mkdir (디렉토리 만들기)

mkdir을 사용하여 지정된 위치에 디렉토리(폴더)를 생성하세요:

- mkdir my_directory: "my_directory"라는 새 디렉토리를 만듭니다.

![링크 없음](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_7.png)

<div class="content-ad"></div>

## cp (복사)

cp 명령어는 파일이나 디렉토리를 한 위치에서 다른 위치로 복사합니다. 예시:

- cp file.txt /목적지/경로: 파일 복사
- cp -r dir1 dir2: 디렉토리를 재귀적으로 복사합니다.

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_8.png)

<div class="content-ad"></div>

## mv (이동 또는 이름 바꾸기)

mv 명령어는 파일이나 디렉토리를 한 위치에서 다른 위치로 옮기는 데 사용됩니다. 또한 파일의 이름을 바꾸는 데도 사용할 수 있습니다:

- mv old_name.txt new_name.txt: 파일 이름 바꾸기.
- mv file.txt /new/location: 파일 이동.

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_9.png)

<div class="content-ad"></div>

## rm (삭제)

rm 명령어는 파일이나 디렉토리를 삭제합니다. 파일을 영구적으로 삭제하기 때문에 주의해야 합니다. 예시:

- rm file.txt: 파일을 삭제합니다.
- rm -r my_directory: 디렉토리를 삭제합니다 (재귀적으로).

![이미지](/assets/img/2024-07-12-MostCommonLinuxCommandsinDataEngineering_10.png)

<div class="content-ad"></div>

# 최종 코멘트

리눅스 명령어는 데이터 엔지니어가 반드시 알아야 할 중요한 노하우 중 하나입니다.

리눅스 명령어를 이용하면 디버깅이나 Docker 이미지 생성과 같은 고급 작업을 수행할 수 있습니다.

GitHub Codespaces와 함께 작업하는 방법을 알고 싶다면 아래 링크를 참고해 주세요.

<div class="content-ad"></div>

이 글이 마음에 드셨나요? 더 많은 기사를 보려면 저를 팔로우해주세요.