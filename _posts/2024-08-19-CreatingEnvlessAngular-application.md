---
title: "환경 설정이 필요없는 Angular 애플리케이션 만드는 방법"
description: ""
coverImage: "/assets/img/2024-08-19-CreatingEnvlessAngular-application_0.png"
date: 2024-08-19 02:13
ogImage: 
  url: /assets/img/2024-08-19-CreatingEnvlessAngular-application_0.png
tag: Tech
originalTitle: "Creating Envless Angular-application"
link: "https://medium.com/itnext/creating-envless-angular-application-0ce3c2ecaddd"
isUpdated: true
updatedAt: 1724033212744
---


<img src="/assets/img/2024-08-19-CreatingEnvlessAngular-application_0.png" />

# 소개

안녕하세요! Angular에 대해서 알고 계신다면, 다양한 환경에 대한 애플리케이션을 빌드하기 위한 도구가 있음을 아실 것입니다.

해당 환경에 따라 빌드를 위해 환경.`env`.ts 파일을 만들고 사용함으로써 이를 실현할 수 있습니다. 이를 통해 다음과 같은 설정 간에 전환할 수 있습니다:

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

- 개발 (environment.ts)
- 테스트 (environment.test.ts)
- 프로덕션 (environment.prod.ts)

## environment.ts 파일의 주요 역할:

- API 설정. 각 파일은 환경에 따라 API 서버의 다른 URL을 포함할 수 있습니다.
- 최적화. 프로덕션 파일은 디버그 기능을 비활성화하고 최적화를 활성화하여 성능을 개선합니다.
- 환경 변수. API 키 및 기능을 활성화 또는 비활성화하는 플래그와 같은 환경 변수를 쉽게 관리할 수 있습니다.

그리고 모든 것이 잘 돼 보입니다 — 각 환경에 대한 각각의 파일

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

# 문제

환경의 수가 늘어남에 따라 다음 작업을 수행해야 합니다:

- 각각의 환경을 위한 `env.ts` 파일을 생성합니다.
- 별도의 빌드 구성을 생성하고 `fileReplacements`를 지정합니다.
- 이 빌드 구성을 `serve-build`에 추가합니다.
- `package.json`에 "my-app.build.`env`": "ng build — configuration `env`” 명령을 추가합니다.

![이미지](https://miro.medium.com/v2/resize:fit:960/1*ML2Aj-vgqcO-vx2Mxjwo0g.gif)

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

프리프로덕션 환경의 e2e 테스트나 기능 브랜치 전용 빌드에 대해서 이야기하고 있지 않아요.

그래서 무엇을 위해? — CI에서 템플릿 명령어를 사용하기 위해서

```js
- run: npm run my-app:build:${ SOME_VAR.ENV }
```

이러한 애플리케이션의 개념적인 파이프라인은 아래와 같이 묘사될 수 있습니다.

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

그리고 당신은 말할 수 있어요 — “이것은 기본 확장과 환경에 독립적인 CI입니다. TEST1을 사용하려면 ENV=TEST1을 전달하면 돼요”.

아니, 젊은 건축가, 이건 틀렸어요. 왜냐면? — 당신의 애플리케이션이 사용되는 모든 환경을 알고 있고 모든 구성을 추적하는 것이 문제예요:

- 구성에 새 매개변수를 추가하고 싶나요? — 각 파일을 업데이트해야 하고, 각 환경에 필요한 값이 무엇인지 알아야 해요.
- 특정 환경에 있는 애플리케이션을 위한 매개변수를 변경하려고 하나요? — 레포지토리로 가서 파일을 업데이트하고 실행해야 해요.

이와 같은 많은 예가 있고 매번 확장성과 지원 문제가 발생해요.

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

## 환경.ts 파일 사용의 단점

- 모든 환경.`env`.ts 파일의 지원이 필요합니다.
- angular.json에서 각 환경에 대한 대체 빌드 생성
- 몇 가지 매개변수만을 사용하여 중복 도커 이미지를 생성하여 효율적이지 못한 레지스트리 공간 활용
- 각 ENV에 대해 전체 파이프라인을 실행해야 하는 필요성
- 사용자에게 제공되지 않을 조립체에서 테스트를 수행합니다.
- 개별 환경에 대한 매개변수를 변경할 유연성 부족
- HeadLess 솔루션으로 응용 프로그램을 공유할 수 없음

# 할 일 목록

주요 달성할 사항:

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

- 어플리케이션과 도커 이미지를 한 번만 빌드하도록 확인해주세요.
- 우리 어플리케이션은 다양한 환경에 맞게 구성할 수 있어야 하며, 필요한 변수들의 인터페이스만 정의하면 됩니다.
- 개발 공간을 분리하고 어플리케이션 빌드 작업을 수행하기 위해, 소스 파일 environment.ts 및 environment.prod.ts를 유지해야 합니다.

이 결과로 다음과 같은 방식으로 어플리케이션을 사용하고 배포해야 합니다.

배포 일환시:

- Envless 빌드 파이프라인은 한 번만 실행되며, 어플리케이션의 프로덕션 빌드를 수행하고, 해당 빌드를 레지스트리에 도커 이미지로 저장합니다.
- `Env` 배포 파이프라인은 이미 알고 있는 환경(ENV)에 따라 어플리케이션을 배포해야 하는 릴리즈 파이프라인입니다.

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

애플리케이션 구성은 릴리스 파트에 가까워질 때까지 되감아지고 있어요

# 해결책 1 — 서버에서 구성 가져오기

이 해결책을 구현하려면 필요한 구성을 모두 얻을 수 있는 API 엔드포인트 서버가 필요합니다

## 작업 구조

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

## 구현

- 어플리케이션 내에서 구성 설정을 선언하고 구성 설정의 프로비저닝을 위한 토큰을 만들어 봅시다. 또한, 초기 상태에 대한 기본값도 생성할 것입니다.

```js
export interface IAppConfig {
 apiHost: string,
 imageHost: string,
 titleApp: string
}

export const APP_CONFIG_DEFAULT: IAppConfig = {
 apiHost: 'https://my-backend.com',
 imageHost: 'https://image-service.com',
 titleApp: 'Production Angular EnvLess App'
}

export const APP_CONFIG_TOKEN: InjectionToken<IAppConfig> = new InjectionToken<IAppConfig>(
   'APP_CONFIG_TOKEN'
)
```

- 구성을 요청하고 제공할 함수를 만들어 보세요. 서버 오류가 발생할 경우, 유효한 작동을 위한 어플리케이션의 기본 설정을 제공합니다.

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
function loadConfig(): Promise<IAppConfig> {
   // 또는 상대 경로 /app/config
   return fetch('http://localhost:3000/app/config').then(
       (res) => res.json(),
       () => APP_CONFIG_DEFAULT
   )
}

export async function bootstrapApplicationWithConfig(
   rootComponent: Type<unknown>,
   appConfig?: ApplicationConfig
): Promise<ApplicationRef> {

   return bootstrapApplication(rootComponent, {
       providers: [
           ...appConfig?.providers || [],
           {
               provide: APP_CONFIG_TOKEN,
               useValue: await loadConfig()
           },
       ]
   })
}
```

- 기존의 native bootstrapApplication 함수를 새로운 bootstrapApplicationWithConfig 함수로 교체합니다.

```js
// bootstrapApplication(AppComponent, appConfig)
bootstrapApplicationWithConfig(AppComponent, appConfig)
   .catch((err) =>
 console.error(err)
);
```

- 애플리케이션 구성 서버는 express의 간단한 구성일 것입니다.  

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
app.get('/app/config', (req, res) => {
 res.json({
   apiHost: 'https://my-backend.<ENV>.com',
   imageHost: 'https://image-service.<ENV>.com',
   titleApp: '<Env> - Angular EnvLess App'
 });
});
```

## 런타임 검증

애플리케이션은 런타임에 구성되어 있으므로 필요한 URL을 알고 있다면 로컬 환경에서 작동 여부를 확인하는 것은 문제가 되지 않습니다.

애플리케이션 시작 시점에 요청에 대한 구성을 가져와 매끄럽게 수행할 수 있습니다. 이 구성을 다른 API URL을 사용하는 다른 애플리케이션 서비스에 사용할 수 있습니다.


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

서버로부터 응답 오류가 발생하는 경우 기본값이 사용되며 응용 프로그램은 계속 작동합니다.

## 장단점

이 접근 방식을 사용하는 또 다른 장점은 다음과 같습니다:

- 응용 프로그램은 런타임에 구성되며 재배포가 필요하지 않습니다.
- 구성은 애플리케이션이 시작되기 전에로드되므로 APP_INITIALIZER 토큰을 통해 병렬 요청을 사용할 수 있으며 구성이 수신되는 순서에 대해 걱정할 필요가 없습니다.
- e2e-테스트에서 요청을 가로채고 구성 파일을 테스트에 제공하는 것이 쉽습니다.

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

하지만 단점도 있습니다:

- 구성을 얻으려면 URL을 알아야 하거나 프론트엔드 및 백엔드 애플리케이션에 동일한 호스트가 있어야 합니다.
- 서버에서의 응답이 길 수 있어 사용자의 기대에 영향을 줄 수 있습니다.
- 요청 오류가 발생할 경우 FALLBACK 값을 가지고 있어야 합니다.
- 각 ENV마다 구성을 위한 전용 데이터베이스가 필요합니다.
- 관리자가 전용 액세스 권한을 가지고 구성을 읽고 수정할 수 있는 API가 필요합니다.
- 런타임 구성으로 인해 오류 발생 확률이 높아지므로 구성은 프론트엔드 애플리케이션에서 유효성을 검사해야 합니다.
- 구성 지원은 백엔드 개발자와 데브옵스에서 제공됩니다.

기본 빌드 파이프라인을 건들지 않으면서 구성을 얻는 체계를 유지하되, 서버에 의존하지 않고 단점을 고려할 수 있는 방법이 있을까요? — 네, 도커 이미지를 구성할 수 있습니다

# 해결책 2 — 도커 이미지 설정

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

이 솔루션의 핵심은 간단합니다. 원격 서버에 설정을 가져오는 대신 Frontend 애플리케이션이 위치한 파일 디렉토리로 요청을 보낼 것입니다.

구성 파일은 소스 도커 이미지 검색 단계에서 생성되며 기본 구성(config.json)을 대체합니다. 프런트엔드 애플리케이션이 요청을 실행하는 기본 디렉토리는 자산이나 공용(public)입니다.

구성 파일은 배포 파이프라인을 시작할 때 지정된 ENV 변수를 기반으로 생성됩니다. ENV 변수가 발견되지 않으면 기본 값이 사용됩니다.

## 작업 방법

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

해결책은 우아하지만, 프론트엔드 뿐만 아니라 데브옵스와 CI 스크립트에도 숙련된 기술이 필요할 것입니다.

"config.json" 파일을 "업데이트"하기 위해 작업 계획을 구현해야 합니다.

## 배포 파이프라인

![Creating Envless Angular Application](/assets/img/2024-08-19-CreatingEnvlessAngular-application_1.png)

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

## 구현

서버에서 config.json을 가져오는 대신에, config의 key들이 업데이트될 때 ENV 변수명과 일치하도록 해야 합니다.

- 로컬 assets/config.json

```js
{
 "APP_ENV_API_HOST": "https://local.my-backend.com",
 "APP_ENV_API_IMAGE_HOST": "https://local.image-service.com",
 "APP_ENV_TITLE_APP": "Local - Angular EnvLess App"
}
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

- 업데이트된 URL로 구성 요청 기능 구성

```js
function loadConfig(): Promise<IAppConfig> {
   // 상대 주소
   // 공용 또는 자산 경로
   return fetch('/config.json').then(
       (res) => res.json(),
   )
}
```

- 새 config.json을 생성하고 docker 이미지를 업데이트하는 예제 스크립트

```js
#!/bin/bash

set -x
set -e

# CONFIG ENVIRONMENT VARIABLES
APP_ENV_API_HOST="https://<ENV>.my-backend.com"
APP_ENV_API_IMAGE_HOST="https://<ENV>.image-service.com"

# SETTINGS
PORT=4110
NGINX_PORT=80
CONTAINER_NAME="angular-envless-container"
IMAGE="angular-envless"
NEW_IMAGE="patched-angular-envless"
CONFIG_NAME="config.json"
APP_PATH="/usr/share/nginx/html"

#Step 1
temp_container_run(){
  docker run -it -d -p $PORT:$NGINX_PORT --name $CONTAINER_NAME $IMAGE
}

#Step 2
temp_container_get_config(){
  docker cp $CONTAINER_NAME:$APP_PATH/$CONFIG_NAME ./$CONFIG_NAME
}

#Step 3
create_config_json(){
  temp_container_get_config

  if [[ ! -f "./$CONFIG_NAME" ]]; then
    echo "지정된 디렉토리에 구성 파일을 찾을 수 없습니다."
    temp_container_stop
    temp_container_rm
    return 1
  fi

  # JSON에서 키와 값을 추출
  KEY_VALUES=$(awk -F '[:,]' '/:/{gsub(/"| /,""); print $1 "=\"" $2 "\""}' "./$CONFIG_NAME")
  
  # 새로운 JSON 객체 생성
  PROD_CONFIG="{"
  
  for PAIR in $KEY_VALUES; do
    KEY=$(echo $PAIR | cut -d '=' -f 1)
    DEFAULT_VALUE=$(echo $PAIR | cut -d '=' -f 2 | sed 's/,$//') 
  
    VALUE=${!KEY}
  
    if [[ -z "$VALUE" ]]; then
      VALUE=$DEFAULT_VALUE
    else
      VALUE="\"$VALUE\""
    fi
  
    PROD_CONFIG+="\"$KEY\":$VALUE,"
  done
  
  PROD_CONFIG=${PROD_CONFIG%,}
  PROD_CONFIG+="}"
  
  echo "$PROD_CONFIG" > "./$CONFIG_NAME"
  
  echo "구성이 성공적으로 업데이트되었으며 ./$CONFIG_NAME에 저장되었습니다."
}

#Step 4
temp_container_upsert_config(){
  docker cp ./$CONFIG_NAME $CONTAINER_NAME:$APP_PATH/$CONFIG_NAME
}

#Step 5
temp_container_commit(){
  docker commit --pause $CONTAINER_NAME $NEW_IMAGE
}

#Step 6.1
temp_container_stop(){
  docker stop $CONTAINER_NAME
}

#Step 6.2
temp_container_rm(){
  docker rm $CONTAINER_NAME
}

main(){
  temp_container_run
  create_config_json
  temp_container_upsert_config
  temp_container_commit
  temp_container_stop
  temp_container_rm
}

main
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

## 런타임 검증

이 스크립트를 실행한 후, 모든 작업을 마치셨습니다. 도커 이미지를 컨테이너에서 실행하고 환경 변수가 구성에 적용되었는지 확인하면 됩니다.

APP_ENV_TITLE_APP 변수가 도커 이미지를 구성할 때 전달되지 않았다면 코드가 정상적으로 작동합니다.

## 장단점

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

이 방식 덕분에 서버가 프런트엔드 애플리케이션 구성에 미치는 영향이 줄었습니다. 서버를 통해 구성하는 장점 외에도 다음과 같은 이점이 있습니다:

- 구성이 즉시 사용 가능하며 지연이 최소화됩니다.
- 요청 오류 발생 시 FALLBACK 값이 필요하지 않습니다.
- 구성에 대한 API나 외부 관리가 필요하지 않습니다.
- 신뢰성이 높습니다. 실행 중에 어셈블리가 손상될 수 없습니다.
- 각 환경별 별도 데이터베이스를 생성할 필요가 없습니다.

하지만 다른 측면에서 조심해야 할 점도 있습니다:

- 응용 프로그램 배포 인프라의 복잡성 증가
- 별도의 파이프라인 배포가 필요합니다.
- DevOps 엔지니어의 지원 및 확인이 필요합니다.
- 배포 시작 시 애플리케이션을 구성하기 위해 올바른 ENV 변수를 알아야 하며 미리 정의된 목록을 설정해야 합니다.

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

# 마무리로

제가 제시한 솔루션들은 프론트엔드 애플리케이션을 유연하게 관리하고 독립적으로 관리해야 할 필요가 있는 경우에만 적합합니다. 현재 애플리케이션이 유연한 구성을 요구하지 않거나 DevOps 엔지니어가 도커 레지스트리 메모리를 효과적으로 관리하거나 헤드리스 애플리케이션을 생성할 계획이 없는 경우에는 이전과 같이 environment.ts 파일을 사용할 수 있습니다.

개발 초기에 애플리케이션의 가능한 구현 방법과 사용 사례를 알 수 없는 경우, 이 접근 방식은 미래에 많은 시간을 절약하고 빌드 관리에 대한 완전한 제어력을 제공할 수 있습니다.