---
title: "Kubernetes 컨테이너와 사라진 SIGTERM 신호들"
description: ""
coverImage: "/assets/img/2024-07-02-KubernetescontainersandthelostSIGTERMsignals_0.png"
date: 2024-07-02 22:35
ogImage:
  url: /assets/img/2024-07-02-KubernetescontainersandthelostSIGTERMsignals_0.png
tag: Tech
originalTitle: "Kubernetes: containers, and the “lost” SIGTERM signals"
link: "https://medium.com/itnext/kubernetes-containers-and-the-lost-sigterm-signals-40007f35759a"
---

<table> 태그를 Markdown 형식으로 변경해 주세요.

<div class="content-ad"></div>

그래서, 이게 어떻게 생겼는지 보여 드릴게요.

우리는 Kubernetes Pod이 있어요:

```js
$ kk get pod
NAME                          READY   STATUS    RESTARTS   AGE
fastapi-app-89d8c77bc-8qwl7   1/1     Running   0          38m
```

로그를 읽어보세요:

<div class="content-ad"></div>

```js
$ ktail fastapi-app-59554cddc5-lgj42
==> Attached to container [fastapi-app-59554cddc5-lgj42:fastapi-app]
```

이렇게 해제하세요:

```js
$ kk delete pod -l app=fastapi-app
pod "fastapi-app-6cb6b46c4b-pffs2" deleted
```

하지만 그의 로그를 확인해 보니, 아무 것도 없군요!

<div class="content-ad"></div>

```js
...
fastapi-app-6cb6b46c4b-9wqpf:fastapi-app [2024-06-22 11:13:27 +0000] [9] [INFO] 애플리케이션 시작이 완료되었습니다.
==> 컨테이너가 종료되었음 [fastapi-app-6cb6b46c4b-pffs2:fastapi-app]
==> 새 컨테이너 시작 [fastapi-app-6cb6b46c4b-9qtvb:fastapi-app]
==> 새 컨테이너 시작 [fastapi-app-6cb6b46c4b-9qtvb:fastapi-app]
fastapi-app-6cb6b46c4b-9qtvb:fastapi-app [2024-06-22 11:14:15 +0000] [8] [INFO] gunicorn 22.0.0 시작 중
...
fastapi-app-6cb6b46c4b-9qtvb:fastapi-app [2024-06-22 11:14:16 +0000] [9] [INFO] 애플리케이션 시작이 완료되었습니다.
```

여기서:

- 서비스가 시작되었음 - “애플리케이션 시작이 완료되었습니다.”
- 파드가 종료됨 - “컨테이너가 종료되었음”
- 새로운 파드가 시작됨 - “새 컨테이너” 및 “gunicorn 시작 중”

일반적인 경우에는 이와 같이 보여져야 합니다:

<div class="content-ad"></div>

```js
...
fastapi-app-59554cddc5-v7xq9:fastapi-app [2024-06-22 11:09:54 +0000] [8] [INFO] 어플리케이션 종료를 기다리는 중입니다.
fastapi-app-59554cddc5-v7xq9:fastapi-app [2024-06-22 11:09:54 +0000] [8] [INFO] 어플리케이션 종료가 완료되었습니다.
fastapi-app-59554cddc5-v7xq9:fastapi-app [2024-06-22 11:09:54 +0000] [8] [INFO] 서버 프로세스 완료 [8]
fastapi-app-59554cddc5-v7xq9:fastapi-app [2024-06-22 11:09:54 +0000] [1] [ERROR] Worker (pid:8)가 SIGTERM을 받았습니다!
fastapi-app-59554cddc5-v7xq9:fastapi-app [2024-06-22 11:09:54 +0000] [1] [INFO] 종료 중: 마스터
==> 컨테이너 왼쪽(종료됨) [fastapi-app-59554cddc5-v7xq9:fastapi-app]
```

그렇다, 여기서 Gunicorn이 SIGTERM을 수신하고 올바르게 작업을 완료합니다.

뭐지?

이것에 대해 좀 더 살펴봅시다.

<div class="content-ad"></div>

# Kubernetes 및 Pod 종료 과정

Pod를 중지하는 과정은 어떻게 이루어질까요?

이에 대해 더 자세히 적은 내용은 쿠버네티스: NGINX/PHP-FPM 우아한 종료 - 502 오류 제거에서 설명했으니, 여기에 매우 간단한 요약이 있습니다:

- 우리는 kubectl delete pod을 실행합니다.
- 해당 WorkerNode의 kubelet은 API 서버로부터 Pod를 중지하라는 명령을 받습니다.
- kubelet은 Pod의 컨테이너에서 PID 1의 프로세스에 SIGTERM 신호를 보냅니다. 즉, 컨테이너가 생성될 때 시작된 첫 번째 프로세스입니다.
- 종료 기간이 지나도 컨테이너가 중지되지 않으면 SIGKILL이 전송됩니다.

<div class="content-ad"></div>

이렇게 하면, 우리의 Gunicorn 프로세스는 SIGTERM을 받아서 로그에 쓰고 워커를 중지하기 시작해야 합니다.

그러나 아무것도 받지 못하고 그냥 종료됩니다.

왜 그럴까요?

# 컨테이너에서 PID 1과 SIGTERM

<div class="content-ad"></div>

이 Pod의 컨테이너 프로세스에 어떤 것들이 있는지 확인해봅시다:

```js
root@fastapi-app-6cb6b46c4b-9qtvb:/app# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2576   948 ?        Ss   11:14   0:00 /bin/sh -c gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
root           8  0.0  1.3  31360 27192 ?        S    11:14   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
root           9  0.2  2.4 287668 49208 ?        Sl   11:14   0:04 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
```

여기에서 PID 1은 gunicorn을 -c를 통해 실행하는 /bin/sh 프로세스이군요.

이제 Pod에서 strace를 실행하여 수신하는 시그널을 확인해보겠습니다.

<div class="content-ad"></div>

```js
root@fastapi-app-6cb6b46c4b-9pd7r:/app# strace -p 1
strace: Process 1 attached
wait4(-1,
```

kubect delete pod 명령어를 실행하지만 명령어를 실행하는 데 걸리는 시간을 측정하려면 아래와 같이 실행하세요:

```js
$ time kk delete pod fastapi-app-6cb6b46c4b-9pd7r
pod "fastapi-app-6cb6b46c4b-9pd7r" deleted

real    0m32.222s
```

32초가 걸렸어요...

<div class="content-ad"></div>

strace에는 무엇이 있나요?

```js
root@fastapi-app-6cb6b46c4b-9pd7r:/app# strace -p 1
strace: Process 1 attached
wait4(-1, 0x7ffe7a390a3c, 0, NULL)      = ? ERESTARTSYS (To be restarted if SA_RESTART is set)
--- SIGTERM {si_signo=SIGTERM, si_code=SI_USER, si_pid=0, si_uid=0} ---
wait4(-1,  <unfinished ...>)            = ?
command terminated with exit code 137
```

그래서 여기서 무슨 일이 있었나요?

- kubelet은 PID 1인 프로세스에 SIGTERM 신호를 보냈습니다. - SIGTERM 'si_signo=SIGTERM' - PID 1은 이 신호를 자식 프로세스에 전달하여 그들을 중지시키고, 그 후에 자신을 종료해야 합니다.
- 하지만 프로세스가 중지되지 않았습니다 — 그래서 kubelet은 프로세스가 올바르게 작업을 완료하기 위해 기본 30초를 기다렸습니다 - Pod 상태 확인
- 그런 다음 kubelet은 컨테이너를 종료하고, 프로세스는 "exit code 137로 종료되었습니다"

<div class="content-ad"></div>

일반적으로 137 종료 코드는 OutOfMemory Killer와 관련이 있습니다. 프로세스가 SIGKILL로 종료되었는데도 OOMKill이 발생하지 않은 경우 Pod 내 프로세스가 제때 종료되지 않아 SIGKILL이 전송되었을 가능성이 있습니다.

그렇다면 우리의 SIGTERM은 어디로 갔을까요?

컨테이너에서 직접 시그널을 실행해 봅시다. 먼저 SIGTERM인 kill -s 15을 실행한 후, SIGKILL인 kill -s 9을 실행해 봅시다.

<div class="content-ad"></div>

뭐요? 어떻게요? 왜요?

왜 SIGTERM이 무시되죠? 게다가 무시되면 안 될 SIGKILL은 더 그렇죠. "무시되지 않는 시그널"이어야 하는데요 - man 시그널을 참조하세요:

# Linux kill() 및 PID 1

Linux의 PID 1은 특별한 프로세스입니다. 시스템에 의해 처음 실행되는 첫 프로세스이며 "우연한 종료"로부터 보호되어야 하기 때문입니다.

<div class="content-ad"></div>

만일 우리가 man kill을 살펴보면, 명시적으로 언급되어 있고, 프로세스에 대한 시그널 핸들러에 대해서도 이야기합니다:

우리의 프로세스가 어떤 핸들러를 가지고 있는지 확인할 수 있습니다. 즉, 어떤 시그널을 프로세스가 받아서 처리할 수 있는지는 /proc/1/status 파일에서 확인할 수 있습니다:

```js
root@fastapi-app-6cb6b46c4b-r9fnq:/app# cat /proc/1/status | grep SigCgt
SigCgt: 0000000000010002
```

SigCgt 시그널은 프로세스가 자체적으로 받을 수 있고 처리할 수 있는 시그널을 나타냅니다. 나머지는 무시되거나 SIG_DFL 핸들러로 처리될 것이며, PID 1에 대한 SIG_DFL 핸들러는 해당 시그널을 무시하며, 해당 프로세스에 별도의 핸들러가 없습니다.

<div class="content-ad"></div>

ChatGPT에게 이 신호들이 정확히 무엇인지 물어 봅시다:

![이미지](/assets/img/2024-07-02-KubernetescontainersandthelostSIGTERMsignals_1.png)

(관심이 있다면 직접 번역해 보실 수 있습니다. 예를 들어 프로세스가 수신 대기 중인 신호를 확인하는 방법이나 신호의 비트마스크를 읽는 방법 등을 참고하세요)

그런데 이렇게 됩니다:

<div class="content-ad"></div>

- PID 1으로 동작 중인 /bin/sh 프로세스
- PID 1은 특별한 프로세스입니다
- PID 1을 확인하면 유일한 SIGHUP 및 SIGCHLD 시그널을 "인식"한다는 것을 알 수 있습니다
- 그리고 SIGTERM 및 SIGKILL은 무시됩니다

그런데 컨테이너는 어떻게 멈추는 걸까요 ?

# 도커 중지 및 리눅스 시그널

Docker에서 컨테이너를 중지하는 과정 (또는 Containerd)은 사실 Kubernetes에서 중지하는 것과 다르지 않습니다. 실제로 kubelet은 컨테이너 런타임에 명령을 전달하기 때문입니다. AWS Kubernetes에서는 이제 containerd를 사용하고 있습니다.

<div class="content-ad"></div>

간편함을 위해 도커를 사용하여 로컬에서 처리하겠습니다.

그러니까 쿠버네티스에서 테스트한 도커 이미지와 동일한 이미지를 사용해 컨테이너를 시작합니다:

```js
$ docker run --name test-app 492***148.dkr.ecr.us-east-1.amazonaws.com/fastapi-app-test:entry-2
[2024-06-22 14:15:03 +0000] [7] [INFO] Starting gunicorn 22.0.0
[2024-06-22 14:15:03 +0000] [7] [INFO] Listening at: http://0.0.0.0:80 (7)
[2024-06-22 14:15:03 +0000] [7] [INFO] Using worker: uvicorn.workers.UvicornWorker
[2024-06-22 14:15:03 +0000] [8] [INFO] Booting worker with pid: 8
[2024-06-22 14:15:03 +0000] [8] [INFO] Started server process [8]
[2024-06-22 14:15:03 +0000] [8] [INFO] Waiting for application startup.
[2024-06-22 14:15:03 +0000] [8] [INFO] Application startup complete.
```

PID 1에 SIGKILL을 보내서 중지시켜보세요 - 아무 변화가 없어요, 시그널을 무시합니다:

<div class="content-ad"></div>

```js
$ docker exec -ti test-app sh -c "kill -9 1"
$ docker ps
CONTAINER ID   IMAGE                                                                   COMMAND                  CREATED              STATUS              PORTS     NAMES
99bae6d55be2   492***148.dkr.ecr.us-east-1.amazonaws.com/fastapi-app-test:entry-2   "/bin/sh -c 'gunicor…"   약 1분 전          Up 약 1분 전                 test-app
```

도커 stop 명령어로 중지를 시도하고 시간을 확인하세요:

```js
$ time docker stop test-app
test-app

실제    0m10.234s
```

그리고 컨테이너의 상태:

<div class="content-ad"></div>

```js
$ 도커 ps -a
CONTAINER ID   IMAGE                                                                        COMMAND                  CREATED              STATUS                        PORTS                                                                                                                                  NAMES
cab29916f6ba   492***148.dkr.ecr.us-east-1.amazonaws.com/fastapi-app-test:entry-2        "/bin/sh -c 'gunicor…"   About a minute ago   Exited (137) 52 seconds ago
```

같은 137 코드이므로 컨테이너는 SIGKILL로 중지되었고, 중지하는 데 걸린 시간은 10초입니다.

그러나 PID 1에 시그널을 무시하는 경우 어떻게 될까요?

도커 케이블 문서에는 못 찾았지만, 두 가지 방법으로 컨테이너 프로세스를 종료할 수 있습니다:

<div class="content-ad"></div>

컨테이너 내에서 모든 자식 프로세스를 종료하면 부모(PID 1)도 종료됩니다.
호스트에서 프로세스 그룹을 해당 SID(Session ID)로 종료할 수 있습니다. 이로 인해 PID 1이 신호를 무시하지만 모든 자식 프로세스가 종료되어 부모도 종료됩니다.

컨테이너 내의 프로세스를 다시 살펴봅시다:

```js
root@cddcaa561e1d:/app# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2576  1408 ?        Ss   15:58   0:00 /bin/sh -c gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
root           7  0.1  0.0  31356 26388 ?        S    15:58   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
root           8  0.5  0.1  59628 47452 ?        S    15:58   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app

root@cddcaa561e1d:/app# pstree -a
sh -c gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
  └─gunicorn /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
      └─gunicorn /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
```

PID 1은 종료되지 않지만 PID 7을 종료할 수 있습니다!

<div class="content-ad"></div>

부모가 자식 프로세스를 일으킨 후에는 PID 8을 죽이겠죠. PID 1은 더 이상 자식이 없다는 것을 알게 되면 스스로 종료되어 컨테이너가 멈출 것입니다:

```js
root@cddcaa561e1d:/app# kill 7
```

컨테이너 로그:

```js
...
[2024-06-22 16:02:54 +0000] [7] [INFO] Handling signal: term
[2024-06-22 16:02:54 +0000] [8] [INFO] Shutting down
[2024-06-22 16:02:54 +0000] [8] [INFO] Error while closing socket [Errno 9] Bad file descriptor
[2024-06-22 16:02:54 +0000] [8] [INFO] Waiting for application shutdown.
[2024-06-22 16:02:54 +0000] [8] [INFO] Application shutdown complete.
[2024-06-22 16:02:54 +0000] [8] [INFO] Finished server process [8]
[2024-06-22 16:02:54 +0000] [7] [ERROR] Worker (pid:8) was sent SIGTERM!
[2024-06-22 16:02:54 +0000] [7] [INFO] Shutting down: Master
```

<div class="content-ad"></div>

그러나 팟/컨테이너는 종료 코드 137로 종료되기 때문에 SIGKILL로 종료되었습니다. 이는 Docker나 다른 컨테이너 실행 환경이 PID 1 프로세스를 SIGKILL로 중지할 수 없을 때 컨테이너 내 모든 프로세스에 SIGKILL을 보내기 때문입니다.

다음과 같이 진행됩니다:

- 먼저, PID 1로 SIGTERM이 전송됩니다.
- 10초 후, PID 1로 SIGKILL이 전송됩니다.
- 이것이 도움되지 않으면, 컨테이너 내 모든 프로세스에 SIGKILL이 전송됩니다.

예를 들어, 이 작업은 세션 ID(SID)를 kill 명령에 전달하여 수행할 수 있습니다.

<div class="content-ad"></div>

컨테이너의 주요 프로세스를 찾으세요:

```js
$ docker inspect --format '{ .State.Pid }' test-app
629353
```

ps j -A를 실행하세요:

```js
$ ps j -A
   PPID     PID    PGID     SID TTY        TPGID STAT   UID   TIME COMMAND
 ...
 629333  629353  629353  629353 ?             -1 Ss       0   0:00 /bin/sh -c gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
 629353  629374  629353  629353 ?             -1 S        0   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
 629374  629375  629353  629353 ?             -1 S        0   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
```

<div class="content-ad"></div>

우리는 SID 번호 629353을 확인했어요.

전체 그룹을 종료시켜요:

```js
$ sudo kill -9 -- -629353
```

좋아요.

<div class="content-ad"></div>

이 모든 것은 매우 좋고 매우 흥미로운 내용이에요.

하지만 이런 지팡이 없이도 할 수 있는 방법이 있을까요?

# 컨테이너에서 프로세스 시작하는 "올바른 방법"

마침내, 우리의 Dockerfile을 살펴보겠습니다:

<div class="content-ad"></div>

```js
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
ENTRYPOINT gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
```

도커 — 셸 및 실행 형식 문서를 참조해주세요:

그래서 우리의 경우에는 셸 형식을 사용했습니다 — 그 결과로 /bin/sh가 PID 1로 사용되며, 이것은 -c를 통해 Gunicorn을 호출합니다.

만약 실행 형식으로 다시 작성해본다면:

<div class="content-ad"></div>

```js
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
ENTRYPOINT ["gunicorn", "-w", "1", "-k", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:80", "app:app"]
```

위 이미지로부터 컨테이너를 실행하면 Gunicorn 프로세스만 실행됩니다:

```bash
root@e6087d52350d:/app# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.6  0.0  31852 27104 ?        Ss   16:13   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
root           7  2.4  0.1  59636 47556 ?        S    16:13   0:00 /usr/local/bin/python /usr/local/bin/gunicorn -w 1 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:80 app:app
```

이미 SIGTERM 시그널을 처리할 수 있습니다:

<div class="content-ad"></div>

```js
root@e6087d52350d:/app# cat /proc/1/status | grep SigCgt
SigCgt: 0000000008314a07
```

<img src="/assets/img/2024-07-02-KubernetescontainersandthelostSIGTERMsignals_2.png" />

이제 PID 1에 SIGTERM을 보내면 컨테이너가 정상적으로 작업을 마치게 됩니다:

```js
root@e6087d52350d:/app# kill 1
```

<div class="content-ad"></div>

그리고 로그:

```js
...
[2024-06-22 16:17:20 +0000] [1] [INFO] 신호 처리 중: term
[2024-06-22 16:17:20 +0000] [7] [INFO] 종료 중
[2024-06-22 16:17:20 +0000] [7] [INFO] 소켓 닫힐 때 에러 발생 [Errno 9] 잘못된 파일 기술자
[2024-06-22 16:17:20 +0000] [7] [INFO] 응용 프로그램 종료 대기 중.
[2024-06-22 16:17:20 +0000] [7] [INFO] 응용 프로그램 종료 완료.
[2024-06-22 16:17:20 +0000] [7] [INFO] 서버 프로세스 [7] 완료.
[2024-06-22 16:17:20 +0000] [1] [ERROR] 워커 (pid:7)에 SIGTERM 전송되었습니다!
[2024-06-22 16:17:21 +0000] [1] [INFO] 종료 중: 마스터
```

그리고 이제 쿠버네티스 포드는 일반적으로 정지됩니다 — 그리고 빠르게, 응용 프로그램 종료를 기다리지 않기 때문에.

# 유용한 링크

<div class="content-ad"></div>

- 가끔 PID 1 프로세스를 컨테이너에서 종료시킬 수 없는 이유 — 리눅스 커널에서 PID 1에 대한 SIGKILL이 어떻게 처리되는지에 대한 훌륭한 포스트
- 커널 내에서 시그널이 작동하는 방식 — 그리고 커널과 시그널에 대한 좀 더 많은 정보
- Docker 컨테이너 내에 초기화 프로세스 (PID 1)를 필요로 하는 이유
- 프로세스 및 해당 모든 자식 프로세스 종료에 대해

RTFM: Linux, DevOps 및 시스템 관리에서 최초로 발행된 기사입니다.
