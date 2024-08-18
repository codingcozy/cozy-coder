---
title: "로컬 환경에서 Kubernetes로 NET과 Angular 배포하기"
description: ""
coverImage: "/assets/img/2024-08-19-LocallyKubernetesDeploymentNETAngular_0.png"
date: 2024-08-19 02:25
ogImage: 
  url: /assets/img/2024-08-19-LocallyKubernetesDeploymentNETAngular_0.png
tag: Tech
originalTitle: "Locally Kubernetes Deployment NET  Angular"
link: "https://medium.com/@uahmad565565/local-kubernetes-deployment-net-angular-b8ef35e06d8f"
isUpdated: false
---


사전 준비 사항: Docker CLI 및 Kubernetes가 설치되어 있어야 해요.

로컬 Kubernetes에 배포해 봅시다. 먼저, 기본 Angular 및 .NET Web API 예제의 저장소를 복제하세요. 마스터 브랜치에 있는지 확인해주세요.


git clone https://github.com/uahmad565/Kubernetes-Angular-.NET-API.git


단계 1. .NET 8 API 배포

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Visual Studio에서 BasicEcommerceWebApi.csproj 파일을 열어주세요. 그런 다음 Docker 지원을 추가하세요.

![이미지](/assets/img/2024-08-19-LocallyKubernetesDeploymentNETAngular_0.png)

컨테이너 스캐폴딩 옵션:
대상 운영 체제: Linux
컨테이너 빌드 유형: Dockerfile

cmd에서 로컬로 Docker 이미지를 빌드하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

아래는 Docker Account 웹 사이트에서 얻은 "usman565" 사용자 이름으로 바꾸세요 (Google Cloud Kubernetes에 배포하기 위한 선택적 단계).

```js
docker build -t  usman565/ecommerceapi:v1 .
```

전제 조건: docker-cli가 설치되어 있으며 docker login을 사용하여 로그인되어 있어야 합니다.

```js
docker push
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapi
  labels:
    app: product-app
spec:
  replicas: 2
  selector:
    matchLabels:
      service: webapi
  template:
    metadata:
      labels:
        app: product-app
        service: webapi
    spec:
      containers:
        - name: webapicontainer
          image: usman565/ecommerceapi:v1 #이미지 변경
          # imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
              protocol: TCP
          env:
            - name: ASPNETCORE_URLS
              value: http://+:80 #컨테이너가 80번 포트에서 수신하도록 함
---
apiVersion: v1
kind: Service
metadata:
  name: webapiservice
  labels:
    app: product-app
    service: webapi
spec:
  ports:
    - port: 8080
      targetPort : 80 #2. 파드 컨테이너와 통신
      protocol: TCP
  selector:
    service: webapi #1. 중요한 부분
```

사용된 개념: backend-manifest.yml 파일은 포트를 지정하는 Pods + Containers의 간단한 Deployment인 Deployment입니다. Service의 역할은 무엇인가요? Service는 Deployment와 통신하는 데 사용됩니다.

- Angular Deployment에는 NodePort 타입의 서비스 "webappservice"가 있으며 이 서비스는 "webapp" Deployment Containers로 향하는 포트를 지정합니다. 이 서비스는 브라우저와 같은 외부 클라이언트에게 접근 가능합니다.
- .NET Deployment에는 ClusterIP 타입의 서비스 "webapiservice"가 있으며 이 서비스는 "webapi" Deployment Containers로 향하는 포트를 지정합니다. 이 서비스는 외부 IP가 없기 때문에 접근할 수 없지만 내부에서는 접근할 수 있습니다.

참고: NodePort 서비스는 프로덕션 환경에서 권장되지 않습니다. LoadBalancer 타입의 서비스는 여러 노드 간의 로드 밸런싱을 고려하므로 로컬 Kubernetes에서 사용하지 않습니다. 로드 밸런서는 클라우드별로 구현되는 것이 일반적입니다. 프로덕션 환경에서는 NodePort 대신 LoadBalancer 유형의 서비스를 사용합니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이미지를 backend-manifest.yml에서 다음과 같이 교체하세요.

```js
kubectl apply -f backend-manifest.yml
```

Angular 단계

"ng serve"가 작동하는 Angular 앱 루트에 "Dockerfile"을 추가하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


### STAGE 1: Build ###
FROM node:18.13-alpine AS build
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build

### STAGE 2: Run ###
FROM nginx:1.17.1-alpine
COPY /nginx-custom.conf /etc/nginx/conf.d/default.conf
COPY --from=build /usr/src/app/dist/projectwith-ng-module/browser /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]
EXPOSE 80


옵션으로 .dockerignore을 추가할 수 있습니다.


**/node_modules/
**/.git
**/README.md
**/LICENSE
**/.vscode
**/npm-debug.log
**/coverage
**/.env
**/.editorconfig
**/.aws
**/dist


nginx-custom.conf


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
server {
    listen 80;
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri /index.html;
    }

    location /api/ {
        # The following statement will proxy traffic to the upstream named Backend
        proxy_pass http://webapiservice:8080;
    }
    
}
```

Reverse Proxy 역할이 매우 중요합니다. 브라우저가 요청을 nginx 서버로 전송하고 HTML, CSS 및 Javascript 파일을 제공받습니다. .NET에 대한 "/api/get/prodocuts"와 같은 요청을 보낼 때, /api가 일치하면 웹 API 서비스로 요청을 보냅니다. 이는 외부(브라우저 외부)에서 접근할 수 없습니다.

Angular 앱의 루트에 다음 deployment.yml 파일을 배치하세요.

```js
docker build -t usman565/ecommercefrontend:v1 . //usman565을 사용자 이름으로 바꿔주세요
docker push
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
  labels:
    app: product-app
spec:
  replicas: 2
  selector:
    matchLabels:
      service: webapp
  template:
    metadata:
      labels:
        app: product-app
        service: webapp
    spec:
      containers:
        - name: webappcontainer
          image: ecommercefrontend:v1
          ports:
            - containerPort: 80
              protocol: TCP
          env:
            - name: API_LINK
              value: http://webapiservice
---
apiVersion: v1
kind: Service
metadata:
  name: webappservice
  labels:
    app: product-app
    service: webapp
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 80
      protocol: TCP
  selector:
    service: webapp
```

다음 명령어를 사용하여 배포하실 수 있어요:

```bash
kubectl apply -f deployment.yml
```

웹앱 서비스에 할당된 포트를 확인하려면 다음 명령어를 사용해보세요:

```bash
kubectl get services
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


![이미지](/assets/img/2024-08-19-LocallyKubernetesDeploymentNETAngular_1.png)

웹 API 서비스에 할당된 로컬호스트:포트로 브라우저 URL에 접속해보세요. 관련 질문이 있으면 언제든지 남겨주세요!

다음 구글 클라우드 쿠버네티스 엔진 GKE에 Kubernetes 배포 방법을 따르세요 - Uahmad - Medium

도움이 되는 기사들:


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

프론트엔드와 백엔드를 서비스를 이용하여 연결해보세요 | 쿠버네티스

쿠버네티스를 활용한 완전한 .NET Core API 및 앵귤러 애플리케이션 빌드하기 | Jaydeep Patil 저 | Plain English에서 JavaScript