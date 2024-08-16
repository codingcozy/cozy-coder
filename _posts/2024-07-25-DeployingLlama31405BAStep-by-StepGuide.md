---
title: "Llama 31 405B 배포하기 단계별 가이드"
description: ""
coverImage: "/assets/img/2024-07-25-DeployingLlama31405BAStep-by-StepGuide_0.png"
date: 2024-07-25 11:55
ogImage: 
  url: /assets/img/2024-07-25-DeployingLlama31405BAStep-by-StepGuide_0.png
tag: Tech
originalTitle: "Deploying Llama 31 405B A Step-by-Step Guide"
link: "https://medium.com/@isaiah_bjork/deploying-llama-3-1-405b-a-step-by-step-guide-9b1b852f3dc9"
---


<img src="/assets/img/2024-07-25-DeployingLlama31405BAStep-by-StepGuide_0.png" />

안녕하세요! 오늘은 라마의 최신 모델인 라마 3.1 405 억 개 매개변수 모델을 배포하는 방법에 대해 알아볼 거에요. 이 강력한 모델은 VRAM을 상당히 많이 필요로 하는데, 특히 4비트 양자화된 버전의 경우 231 기가바이트가 필요합니다. 그러나 최적화를 통해 8x4090 GPU를 사용하여 192 기가바이트에서 실행할 수 있습니다. 그리고 가장 좋은 부분은 모든 것을 설정하는 데 세 개의 터미널 명령만 실행하면 된다는 것이에요.

https://isaiahbjork.gumroad.com/l/llama3-1-405B

## 단계 1: GPU 클러스터 설정하기

<div class="content-ad"></div>

우선, vast.ai 또는 클러스터 설정을 지원하는 다른 GPU 공급업체에 계정이 필요합니다. 이 가이드에서는 8x4090 GPU를 사용할 것입니다. 주요 단계는 다음과 같습니다:

인스턴스 생성하기:

- vast.ai에 로그인하고 8x4090 GPU로 구성된 인스턴스를 생성합니다.
- 325 기가바이트의 디스크 공간을 할당합니다. 제공되는 CUDA 템플릿을 사용하세요.
- 모델이 231 기가바이트이므로 빠른 다운로드 프로세스를 보장하기 위해 초당 메가바이트 속도가 높은 공급업체를 찾으세요.

비용 확인하기:

<div class="content-ad"></div>

- 시간당 비용을 확인하고 예산 범위 내에 있는지 확인하세요.
- 인터넷 가격을 확인하여 예상치 못한 요금을 피하세요.

인스턴스 렌트하기:

- 인스턴스를 구성한 후에 "렌트"를 클릭하세요.
- 안전한 접근을 위해 인스턴스에 SSH 키를 추가하세요.

## 단계 2: 클러스터로 SSH 접속하기

<div class="content-ad"></div>

이제 인스턴스가 준비되었으니, 다음 단계로 SSH 접속을 해보겠습니다:

초기 SSH 액세스:

- 터미널을 열고 제공된 명령어를 사용하여 클러스터로 SSH 연결을 합니다.
- "known hosts" 목록에 SSH 키를 추가하라는 메시지가 표시되면 "yes"라고 입력하십시오.

tmux 종료 및 재시작:

<div class="content-ad"></div>

- tmux kill-server 명령을 실행하여 이전 세션에 영향을 받지 않도록 합니다.
- 클러스터로 다시 SSH 연결합니다.

여러 개의 터미널 열기:

- 두 번째 터미널을 열고 프로세스를 효율적으로 관리하기 위해 클러스터로 다시 SSH 연결합니다.

## 단계 3: Llama 3.1 설치 및 실행

<div class="content-ad"></div>

클러스터 설정이 완료되었으니, Llama 3.1을 설치하고 실행해 봅시다:

Ollama 설치:

- 첫 번째 터미널에서 제공된 스크립트를 실행하여 Llama를 설치합니다.

Ollama Serve 실행:

<div class="content-ad"></div>

- 설치가 완료되면 첫 번째 터미널에서 llama serve 명령을 실행하세요.

설치 모니터링:

- 두 번째 터미널로 전환하여 동일한 명령을 실행하여 프로세스를 모니터링하세요.
- 설치 진행 상황과 예상 소요 시간을 확인할 수 있습니다.

## 테스트 및 탈옥

<div class="content-ad"></div>

Llama 3.1 버전이 작동 중이면 테스트를 시작할 수 있어요:

출력 모니터링:

- 출력물을 관찰하여 정상적으로 작동하는지 확인하세요. 하드웨어에 따라 속도가 다를 수 있음을 참고해 주세요.

해킹 방지 메시지:

<div class="content-ad"></div>

- "jailbreak" prompt를 실행해보고 작동하는지 확인해 보세요. 결과는 다를 수 있으며, 효과적인 jailbreak를 찾기 위해 실험할 수 있습니다.

여기에 있습니다! 이렇게 하면 vast.ai나 다른 호환되는 제공 업체를 사용하여 GPU 클러스터에 Llama 3.1을 배포할 수 있습니다. 문제가 발생하거나 제안 사항이 있으면 아래에 댓글을 남겨주세요.