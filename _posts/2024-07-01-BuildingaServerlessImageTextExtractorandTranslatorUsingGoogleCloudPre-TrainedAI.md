---
title: "Google Cloud 사전 학습 AI를 사용한 서버리스 이미지 텍스트 추출기 및 번역기 만들기"
description: ""
coverImage: "/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_0.png"
date: 2024-07-01 00:31
ogImage:
  url: /assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_0.png
tag: Tech
originalTitle: "Building a Serverless Image Text Extractor and Translator Using Google Cloud Pre-Trained AI"
link: "https://medium.com/google-cloud/building-a-serverless-image-text-extractor-and-translator-using-google-cloud-pre-trained-ai-adfdccdb18d9"
isUpdated: true
---

안녕하세요!

저희 미디엄 블로그를 주시는 분들은 제가 주로 건축, 전략, 그리고 구글 클라우드에 관련된 글을 쓴다는 걸 아실 거예요. 가끔은 파이썬에 관한 글도 올립니다.

하지만 제가 구글 클라우드에서 실용적인 애플리케이션을 직접 구축하는 건 정말 드물죠. 그래서 이번에는 구글 클라우드에서의 완전한 개발 경험에 관한 블로그를 써보기로 했습니다.

다음에 뵙겠습니다!

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

이 블로그에서는 사용자가 업로드한 이미지에서 텍스트를 추출하고 필요한 경우 번역하는 서버리스 AI 애플리케이션을 구축하는 방법에 대해 이야기하겠습니다. 다음을 활용할 예정입니다:

- Python Flask 애플리케이션 형태로 UI를 호스팅하는 Cloud Run.
- 사용자가 이미지를 업로드할 때 백엔드 로직을 처리하는 Cloud Functions.
- Google의 미리 구축된 이미지 및 번역 인공지능 머신러닝 API.
- 로컬 개발을 위해 Visual Studio Code를 사용하며, 로컬 Cloud Functions 개발에는 functions-framework, 로컬 Cloud Run 개발에는 Cloud Code를 활용합니다.

## AI 및 AI 제품 빠른 개요

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

## 인공 지능

안녕하세요! 이번에 함께 이야기할 주제는 인공 지능(AI)에 대한 것이에요. 인공 지능은 기계 자동화를 활용하여 보통 인간의 지능이 필요한 작업을 수행하는 넓은 개념을 가리킵니다. 예를 들어 음성 인식, 시각 지각, 언어 번역, 의사 결정 등이 있어요.

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_2.png)

## 기계 학습

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

특정 AI 하위분야는 기계에게 데이터 내의 패턴을 인식하고 명시적인 코딩 솔루션 없이 예측하고 문제를 해결할 수 있도록 가르치는 것과 관련이 있습니다.

## 생성 AI (생 AI)

생 AI는 훈련된 데이터와 비슷하지만 똑같지는 않은 새로운 데이터를 생성할 수 있는 AI 하위 클래스입니다. 생성 AI는 대형 언어 모델 (LLM), 생성 이미지 모델 및 다중 모달 모델과 같은 기초 모델에 의존합니다. 다중 모달 모델은 여러 종류의 입력 데이터 (예: 텍스트, 이미지 및 비디오)를 처리하고 여러 종류의 콘텐츠를 생성할 수 있는 모델 유형입니다.

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_3.png)

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

내 해결책은 Gen AI를 사용하지 않을 거예요. 그래도 Gen AI가 너무 흔하게 사용되고 있어서 이 글에 언급했어요. 내 솔루션에서 사용하고 있는 예측 모델과 어떻게 다른지 이해하실 수 있도록 말이에요.

## 구글의 사전 훈련된 머신 러닝 API

이들은 특정 작업을 수행하기 위해 구글에 의해 미리 구축되고 훈련된 머신 러닝 모델입니다. 이들은 예측 모델로 분류되며 생성적 모델과는 다릅니다. 예시로는:

- Google Cloud Vision API — 분류, 얼굴 인식, 텍스트 감지와 같은 작업용.
- Google Cloud Natural Language API — 텍스트 뒤에 담긴 의미를 이해하는데 사용돼. 이는 텍스트의 중요 요소를 식별하는 것부터 감정 분석까지 포함돼.
- Google Cloud Translation API — 한 언어에서 다른 언어로 번역하는데 사용돼.
- Google Cloud Video — 비디오 분석 및 주석용.

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

# 어플리케이션에 대한 동기

요즘 몇 달 동안 우크라이나어를 배우고 있어요. 너무나 아름다운 언어죠. 우크라이나어 레슨 팟캐스트인 안나 오호이코가 만들어 놓은 팟캐스트를 듣기 시작했어요. 그곳에서 우크라이나어를 배우는 활발하고 번창하는 커뮤니티인 '우크라이나어 학습자들'을 발견했어요. 가끔 커뮤니티 회원들이 우크라이나어로 된 미담을 올리곤 해요. 때로는 그 미담을 이해하기도 했고, 자주는 몰랐어요.

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_4.png)

그래서 생각했어요... "내가 원하는 어플리케이션을 만들고 싶네요. 이미지나 미담을 업로드하면 그 안의 텍스트를 추출해서 제 모국어로 번역해 주는 기능이 있는 '어플'일까요."

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

물론, 이 작업을 수행하는 다른 방법들이 있을 수 있어요. 하지만 저는 새로운 서버리스 응용 프로그램을 Google Cloud에서 처음부터 만드는 데 좋은 사용 사례라고 생각했어요.

# 응용 프로그램 구조

Google Cloud 서버리스 서비스에서 호스팅되는 꽤 간단한 응용 프로그램 구조에요:

![Building a Serverless Image Text Extractor and Translator Using Google Cloud Pre-Trained AI](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_5.png)

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

**웹앱 UI - 클라우드 런**

여기서는 Google Cloud Run에서 컨테이너로 Flask 웹 애플리케이션을 실행합니다. 이 웹 애플리케이션은 여러 가지를 수행합니다:

- 사용자 입력(예: 언어 및 이미지 업로드)를 캡처하는 데 사용되는 폼이 포함된 프론트엔드 페이지를 렌더링합니다.
- 사용자의 요청을 처리하여 이미지를 캡처하고 클라우드 함수 백엔드로 전송합니다.

클라우드 런을 선택한 이유는 다음과 같습니다:

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

- 컨테이너화된 응용 프로그램(예: 우리의 Python Flask 웹 응용 프로그램)을 호스팅하고 실행하는 서버리스 방식을 제공합니다.
- 이 서비스는 이와 같은 간단한 상태를 유지하지 않는 웹 응용 프로그램을 호스팅하는 데 적합합니다.
- 수요가 없을 때 자동으로 스케일 업하고 0개의 인스턴스로 다운스케일링됩니다.

## 백엔드 - 클라우드 함수

클라우드 함수는 사용자로부터 이미지를 받아(웹 응용 프로그램 UI를 통해) 해당 이미지에서 텍스트를 추출하고 번역하기 위해 해당하는 Google Cloud API를 호출합니다.

왜 클라우드 함수를 선택했는지 확인하세요:

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

- 이는 사건에 따른 단기 처리를 수행하기에 적합하며, 사용자가 업로드한 이미지에 대한 추출 및 번역 작업을 실행하는 데 이상적입니다.
- 수요가 없을 때 자동으로 축소되며, 0으로 축소됩니다.
- 이미지 처리를 사용자 프론트엔드에서 분리할 수 있습니다. 따라서 다른 프론트엔드를 사용하고 싶다면, 코드를 변경하지 않고 쉽게 변경할 수 있습니다.

## 텍스트 추출과 번역

나는 Google의 사전 제작된 Vision API 및 Translation API를 사용하고 있습니다. 그러나 Gemini Pro Vision과 같은 생성 AI 모델을 사용해 보는 것은 어떨까요?

- Vision 및 Translation API는 수행하려는 작업에 특별히 설계되어 있습니다.
- 반면에, Gemini Pro Gen AI multimodal foundation 모델은 자연 언어 프롬프트에 대한 응답으로 동일한 결과를 얻을 수 있습니다. 하지만, 여기서는 자연어 상호작용이 필요하지 않습니다. 왜냐하면 이미지 업로드에 대한 API의 반응을 정확히 알고 있기 때문입니다.
- Gemino Pro Vision은 멀티모달 기반 모델로 더 많은 다재다능성을 가지고 있지만, 이 기능은 더 높은 가격표와 함께 제공됩니다. Vision API는 매월 1000회의 무료 실행을 제공하며, Translate API는 매월 처음 50만 자의 문자에 대한 무료 번역을 제공합니다.

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

# 나의 개발 환경

## WSL

나는 윈도우를 사용하고 있고, Windows Subsystem for Linux(WSL)을 사용하고 있어. WSL은 윈도우 10 이상에 포함된, 윈도우 안에서 직접 리눅스 환경을 실행할 수 있게 해주는 환경이야. 나는 우분투를 사용하고 있어.

WSL 안에서 작업하는 장점은 필요한 스크립트를 bash로 작성할 수 있다는 거야. 이 말은 나의 코드가 더 이식성이 높다는 걸 의미해. 예를 들어, 나의 환경 안에서 Google Cloud Shell 안에서 작업하는 것과 동일한 스크립트를 실행할 수 있다는 말이야.

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

## 비주얼 스튜디오 코드

비주얼 스튜디오 코드는 저의 코드 편집기입니다. 무료이며 오픈 소스입니다. Windows, Linux 및 Mac에서 실행되며 Git 통합, Google Cloud Code와 같은 많은 유용한 플러그인을 갖추고 있습니다. Google Cloud 서비스를 이용한 로컬 개발을 용이하게 하는 AI 지원 플러그인인 Gemini Code Assist를 포함한 세트가 있습니다.

# 개발 프로젝트 구조

Git 저장소를 확인하려면 여기에서 찾을 수 있습니다.

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

전체 구조는 이렇게 생겼어요:

```js
└── image-text-translator
    ├── docs/                   - 레포지토리 문서
    |
    ├── infra-tf/               - 인프라 설치용 Terraform
    |
    ├── scripts/                - 환경 설정 및 보조 스크립트
    |   └── setup.sh            - 설정 보조 스크립트
    |
    ├── app/                    - 애플리케이션
    │   ├── ui_cr/                - 브라우저 UI (Cloud Run)
    │   │   ├── static/             - 프론트엔드 정적 콘텐츠
    |   |   ├── templates/          - 프론트엔드용 HTML 템플릿
    |   |   ├── app.py              - Flask 애플리케이션
    |   |   ├── requirements.txt    - UI Python 요구 사항
    |   |   ├── Dockerfile             - Flask 컨테이너 빌드용 Dockerfile
    |   |   └── .dockerignore          - Dockerfile에서 무시할 파일
    |   |
    │   └── backend_gcf/          - 백엔드 (Cloud Function)
    │       ├── main.py             - 백엔드 CF 애플리케이션
    │       └── requirements.txt    - 백엔드 CF Python 요구 사항
    |
    ├── testing/
    │   └── images/
    |
    ├── requirements.txt          - 프로젝트 로컬 개발용 Python 요구 사항
    └── README.md
```

# Google 프로젝트 설정 및 권한 부여하기

## Google Cloud 프로젝트 만들기

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

구글 클라우드 프로젝트를 귀하의 애플리케이션을 위해 생성해 주세요. 제 프로젝트는 이렇게 생겼어요:

![Project Screenshot](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_6.png)

충분한 권한을 가진 계정으로 다음 단계를 수행해 주세요. 프로젝트 관리자 또는 조직 관리자 권한이 필요합니다.

## API 활성화하기

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

이 구성을 최종적으로는 테라폼으로 조정할 것입니다. 하지만 처음에는 다음과 같은 API를 활성화해야 합니다:

```js
# Google Cloud에 인증
gcloud auth list

# 올바른 프로젝트가 선택되었는지 확인
export PROJECT_ID=<프로젝트 ID를 입력하세요>
gcloud config set project $PROJECT_ID

# Cloud Build API 활성화
gcloud services enable cloudbuild.googleapis.com

# Cloud Storage API 활성화
gcloud services enable storage-api.googleapis.com

# Artifact Registry API 활성화
gcloud services enable artifactregistry.googleapis.com

# Eventarc API 활성화
gcloud services enable eventarc.googleapis.com

# Cloud Run Admin API 활성화
gcloud services enable run.googleapis.com

# Cloud Logging API 활성화
gcloud services enable logging.googleapis.com

# Cloud Pub/Sub API 활성화
gcloud services enable pubsub.googleapis.com

# Cloud Functions API 활성화
gcloud services enable cloudfunctions.googleapis.com

# Cloud Translation API 활성화
gcloud services enable translate.googleapis.com

# Cloud Vision API 활성화
gcloud services enable vision.googleapis.com

# Service Account Credentials API 활성화
gcloud services enable iamcredentials.googleapis.com
```

## 서비스 계정과 역할

서비스 계정은 응용 프로그램의 인증 및 권한 부여를 관리하는 표준 접근 방식입니다. 우리의 Cloud Run 애플리케이션은 Cloud Function에 인증해야 하며, Cloud Function은 Cloud Vision 및 Translation API에 인증해야 합니다.

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

서비스 계정을 생성해 봅시다.

```js
# 이 작업을 수행하기 전에 PROJECT_ID 변수가 설정되어 있는지 확인하세요!
export SVC_ACCOUNT=image-text-translator-sa
export SVC_ACCOUNT_EMAIL=$SVC_ACCOUNT@$PROJECT_ID.iam.gserviceaccount.com

# 클라우드 상에서 실행되는 제품용 코드에 자격 증명을 제공하는 권장 방법은 사용자 관리형 서비스 계정을 부착하는 것입니다.
gcloud iam service-accounts create $SVC_ACCOUNT
```

이제 서비스 계정에 여러 역할(role)을 부여할 것입니다.

```js
# 서비스 계정에 역할(role) 부여
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:$SVC_ACCOUNT_EMAIL" \
  --role=roles/run.admin

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:$SVC_ACCOUNT_EMAIL" \
  --role=roles/run.invoker

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:$SVC_ACCOUNT_EMAIL" \
  --role=roles/cloudfunctions.admin

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:$SVC_ACCOUNT_EMAIL" \
  --role=roles/cloudfunctions.invoker

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:$SVC_ACCOUNT_EMAIL" \
  --role="roles/cloudtranslate.user"

# 서비스 계정을 다른 리소스에 부착할 주체에 필요한 역할(role)을 부여합니다.
gcloud iam service-accounts add-iam-policy-binding $SVC_ACCOUNT_EMAIL \
  --member="group:gcp-devops@my-org.com" \
  --role=roles/iam.serviceAccountUser

# 서비스 계정 잠조권한 부여
gcloud iam service-accounts add-iam-policy-binding $SVC_ACCOUNT_EMAIL \
  --member="group:gcp-devops@my-org.com" \
  --role=roles/iam.serviceAccountTokenCreator

# 클라우드 함수 및 클라우드 실행에 배포할 수 있는 권한이 있는지 확인하세요.
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="group:gcp-devops@my-org.com" \
  --role roles/run.admin
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

# 로컬 개발 환경 설정

(개발을 계획하는 모든 기기에서 이러한 단계를 따라야 합니다.)

터미널을 열어주세요. (저는 Windows 터미널에서 Ubuntu 셸을 열고 있어요.) 이전에 하지 않았다면 Google gcloud CLI 및 지원 도구를 로컬 환경에 설치해야 합니다:

```bash
# 로컬 리눅스 환경에 Google Cloud CLI 설치.
# 자세한 내용은 https://cloud.google.com/sdk/docs/install 를 참조해주세요.

# Gcloud CLI에서 Python 및 pip 설정
# https://cloud.google.com/python/docs/setup 를 참조해주세요.

# 로컬 개발을 위한 Google Cloud CLI 추가 패키지 설치
sudo apt install google-cloud-cli-gke-gcloud-auth-plugin kubectl google-cloud-cli-skaffold google-cloud-cli-minikube
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

여기부터는 개발자 계정으로 인증을 받아오기로 했어요. 조직 관리자 계정이 아니라는 거죠. 왜냐면 최소 권한 원칙을 따르고 싶어서요.

```js
# Google Cloud에 인증하기
gcloud auth login
```

인증할 때, 나오는 링크 중 첫 번째 링크를 클릭하세요. 그러면 비밀번호를 입력하라는 안내가 나올 거에요.

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_7.png)

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

다음으로 애플리케이션 프로젝트 폴더를 설정하고 몇 가지 종속 항목을 설치할 것입니다. 처음부터 애플리케이션을 만들고 있다면 다음 단계를 따르세요:

```shell
# 내 프로젝트를 보관하는 위치입니다
cd ~/localdev/gcp/image-text-translator

# Python 가상 환경을 만듭니다. 예를 들어...
python3 -m venv .venv
# 이제 활성화합니다

# 필요한 Python 패키지를 추가합니다
python3 -m pip install Flask
python3 -m pip install pillow # 이미지 처리용
python3 -m pip install functions-framework
python3 -m pip install google-cloud-storage google-cloud-translate google-cloud-vision google-auth

# 그리고 requirements.txt 파일을 만듭니다
python3 -m pip freeze > requirements.txt
```

또는 제 깃허브 레포를 복제하려는 경우:

```shell
git clone https://github.com/derailed-dash/image-text-translator.git

cd image-text-translator

# Python 가상 환경을 만듭니다. 예를 들어...
python3 -m venv .venv
# 그리고 이제 활성화합니다. 예를 들어,
source .venv/bin/activate

# 이제 Python 종속 항목을 설치합니다
python3 -m pip install -r requirements.txt
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

## Git 설치

만약 다른 사람의 저장소를 클론하지 않고 처음부터 모든 것을 만들고 있다면, 지금 Git과 GitHub 환경을 설정해야 합니다. 먼저 .gitignore 파일을 만드는 것을 잊지 마세요. 어떻게 보여야 하는지 감을 잡기 위해 제 저장소를 확인해보세요. 그런 다음 다음 단계를 따르세요:

```bash
# Cloud Shell에서 git을 설정합니다. 이전에 한 번도 설정하지 않았다면
git config --global user.email "bob@wherever.com"
git config --global user.name "Bob"
git config --global core.autocrlf input # WSL을 사용하는 경우 매우 중요합니다!

# 로컬 git 저장소를 생성합니다.
# 진행하기 전에 .gitignore 파일을 생성했는지 확인하세요
# .terraform 디렉토리 및 로컬 state, plans 등을 무시하도록 설정했나요?
git init
git add .
git commit -m "Initial commit"

# GitHub 명령줄 도구를 인증합시다.
# 이미 Cloud Shell에 설치되어 있습니다.
gh auth login

# 이제 'gh' 명령줄 도구를 사용하여 GitHub에 원격 저장소를 생성합시다.
# 원하는 경우 비공개로 설정할 수 있습니다.
gh repo create image-text-translator --public --source=.
git push -u origin master
```

## VS Code 실행하기

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

우리 프로젝트 폴더에서 VS Code를 열어봅시다:

```js
# /path/to/your/image-text-translator에서 실행
code .
```

VS Code는 필요한 WSL 플러그인을 자동으로 설정해줍니다.

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_8.png)

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

## 애플리케이션 기본 자격 증명(ADC) 설정

ADC는 인증 라이브러리가 현재 환경을 기반으로 자격 증명을 자동으로 찾을 수 있게 하는 전략입니다. 이는 Cloud SDK를 사용한 로컬 환경에서만이 아니라 Google Cloud의 대상 환경에서도 ADC를 활용할 수 있어 유용합니다.

ADC는 서비스 계정 자격 증명을 사용하도록 구성할 수 있습니다. 이를 구성하는 방법에는 두 가지가 있습니다:

- 우리는 자신의 사용자 ID를 사용하여 서비스 계정을 흉내 낼 수 있습니다.
- 우리는 서비스 계정을 위해 개인 키를 만들고 ADC를 이 키의 위치로 지정할 수 있습니다.

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

먼저 이렇게 시도해 보았어요:

```bash
gcloud auth application-default login --impersonate-service-account $SVC_ACCOUNT_EMAIL
```

그러나 Cloud Run으로부터 Cloud Function 호출을 인증하는 과정에서 작동하는지 확인하기가 조금 어려웠어요. 대신에 ADC(인증 키)를 가리킬 수 있는 서비스 계정 키를 생성했어요:

```bash
gcloud auth application-default login

# 이미 설정되어 있지 않은 경우...
export SVC_ACCOUNT=image-text-translator-sa
export SVC_ACCOUNT_EMAIL=$SVC_ACCOUNT@$PROJECT_ID.iam.gserviceaccount.com

# 로컬 개발을 위해 서비스 계정 키 만들기
gcloud iam service-accounts keys create ~/.config/gcloud/$SVC_ACCOUNT.json \
  --iam-account=$SVC_ACCOUNT_EMAIL

# 클라이언트 라이브러리에서 자동으로 감지하는 ADC 환경 변수 설정
export GOOGLE_APPLICATION_CREDENTIALS=~/.config/gcloud/$SVC_ACCOUNT.json
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

이미 어플리케이션용 환경 변수를 설정해야 해요. 매 세션마다 GOOGLE_APPLICATION_CREDENTIALS 환경 변수를 설정해줘야 합니다. 이제 그에 대해 이야기해볼게요.

# 매 세션을 위한 설정

모든 새로운 터미널 세션마다 아래 명령어를 실행해야 해요. (또는 다음 명령어를 실행하여 `project_dir`/scripts/setup.sh의 명령어를 실행할 수도 있어요.)

```js
export PROJECT_ID=$(gcloud config list --format='value(core.project)')
export REGION=europe-west4
export SVC_ACCOUNT=image-text-translator-sa
export SVC_ACCOUNT_EMAIL=$SVC_ACCOUNT@$PROJECT_ID.iam.gserviceaccount.com
export GOOGLE_APPLICATION_CREDENTIALS=~/.config/gcloud/$SVC_ACCOUNT.json

# Functions
export FUNCTIONS_PORT=8081
export BACKEND_GCF=https://$REGION-$PROJECT_ID.cloudfunctions.net/extract-and-translate

# Flask
export FLASK_SECRET_KEY=some-secret-1234
export FLASK_RUN_PORT=8080

echo "환경 변수가 구성되었습니다:"
echo PROJECT_ID="$PROJECT_ID"
echo REGION="$REGION"
echo SVC_ACCOUNT_EMAIL="$SVC_ACCOUNT_EMAIL"
echo BACKEND_GCF="$BACKEND_GCF"
echo FUNCTIONS_PORT="$FUNCTIONS_PORT"
echo FLASK_RUN_PORT="$FLASK_RUN_PORT"
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

# The Cloud Function Backend

## Local Development of the Function

Hey there! Instead of copying all the code here, you can find it on GitHub. Let me highlight some important details for you.

In the backend-gcf folder, I create a `requirements.txt`. This file lists all the Python packages that need to be installed. When you deploy the function, Cloud Functions will automatically take care of installing these packages for you.

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

그럼 main.py 파일을 만들었어요. extract_and_translate() 함수의 일부분을 보여드릴게요.

```python
@functions_framework.http
def extract_and_translate(request):
    """이미지에서 텍스트를 추출하고 번역합니다.
    이미지는 요청으로 POST할 수도 있고 GCS 객체 참조일 수도 있어요.

    만약 POST된 이미지라면, enctype는 multipart/form-data여야 하며 파일은 'uploaded'로 이름이 지정돼야 해요.
    GCS 객체 참조를 전달하는 경우, content-type은 'application/json'이어야 하고,
    두 가지 속성을 가져야 해요:
    - bucket: 파일이 저장된 GCS 버킷의 이름
    - filename: 읽을 파일의 이름
    """

    # 요청 메서드가 POST인지 확인해요
    if request.method == 'POST':
        # 요청에서 업로드된 파일을 가져와요
        uploaded = request.files.get('uploaded')  # 입력 파일 이름을 'uploaded'로 가정
        to_lang = request.form.get('to_lang', "en")
        print(f"{uploaded=}, {to_lang=}")
        if not uploaded:
            return flask.jsonify({"error": "No file uploaded."}), 400

        if uploaded: # 업로드된 파일 처리
            file_contents = uploaded.read()  # 파일 내용 읽기
            image = vision.Image(content=file_contents)
        else:
            return flask.jsonify({"error": "Uploaded 파일을 읽을 수 없습니다."}), 400
```

설명이 꽤 자명해요.

- 함수가 POST를 받았는지 확인해요. (이후에 클라우드런 애플리케이션을 만들어서 요청을 POST할 거예요.)
- 그렇다면 요청에서 'uploaded'라는 객체를 찾아요. (클라우드런 애플리케이션이 이것을 요청에 첨부할 거예요.)
- 이 객체가 첨부되어 있다면, 이진으로 읽은 후 vision.Image 객체를 생성해요.
- 그 다음 detect_text() 함수를 호출해요. 이미지를 전달하는데, 이 함수는 Vision API를 사용하여 이미지에 텍스트가 있는지 확인할 거예요.

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

만약 그렇다면, Python 사전을 반환할 것입니다. 그것은 그 텍스트를 translate_text() 함수에 전달하고, 선택한 언어로 번역합니다.

여기 detect_text() 함수가 있어요:

```python
def detect_text(image: vision.Image) -> dict | None:
    """이미지에서 텍스트 추출하기"""
    text_detection_response = vision_client.text_detection(image=image)
    annotations = text_detection_response.text_annotations

    if annotations:
        text = annotations[0].description
    else:
        text = ""
    print(f"이미지에서 추출된 텍스트:\n{text}")

    # 언어 식별자를 ISO 639-1 형식으로 반환합니다. 예: en.
    # https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes 확인해보세요
    detect_language_response = translate_client.detect_language(text)
    src_lang = detect_language_response["language"]
    print(f"검출된 언어: {src_lang}.")

    message = {
        "text": text,
        "src_lang": src_lang,
    }

    return message
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

이 코드는 이미지에서 텍스트를 감지하는 것뿐만 아니라 Google Language API를 사용하여 텍스트의 언어를 결정합니다.

다음은 translate_text() 함수입니다:

```js
def translate_text(message: dict, to_lang: str) -> dict:
    """
    메시지의 텍스트를 지정된 소스 언어에서 요청된 대상 언어로 번역한 다음,
    결과를 저장할 다른 서비스에 요청을 보내는 함수입니다.
    """

    text = message["text"]
    src_lang = message["src_lang"]

    translated = {
        "text": text,
        "src_lang": src_lang,
        "to_lang": to_lang,
    }

    if src_lang != to_lang and src_lang != "und":
        print(f"{to_lang}로 텍스트를 번역 중.")
        translated_text = translate_client.translate(
                text, target_language=to_lang, source_language=src_lang)

        translated = {
            "text": unescape(translated_text["translatedText"]),
            "src_lang": src_lang,
            "to_lang": to_lang,
        }
    else:
        print("번역이 필요하지 않습니다.")

    return translated
```

소스 언어와 대상 언어가 다르고 소스 언어가 undefined가 아닌지 확인합니다. 이 체크를 통과하면 Google Translate API를 사용하여 텍스트를 번역합니다.

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

## 로컬 테스트

먼저, 함수를 로컬에서 실행해 봅시다. backend-gcf 폴더에서 다음 명령을 실행해 주세요:

```js
# 함수 실행
functions-framework --target extract_and_translate \
  --debug --port $FUNCTIONS_PORT
```

아래와 같이 보일 것입니다:

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

**이미지 파일을 가지고 테스트를 진행할 거에요:**

![이미지 파일](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_10.png)

두 번째 터미널에서 curl을 사용해 함수에 POST해보세요:

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

```bash
# 먼저 이 터미널에서 인증하고 환경 변수를 설정해주세요.
source ./scripts/setup.sh

# 이제 호출하세요.
curl -X POST localhost:$FUNCTIONS_PORT \
   -H "Content-Type: multipart/form-data" \
   -F "uploaded=@./testing/images/ua_meme.jpg" \
   -F "to_lang=en"
```

그리고 성공했어요!!

![BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_11.png)

## 클라우드 함수를 배포하세요 (Google Cloud로)

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

이제 로컬에서 테스트를 완료했으니 Google Cloud에 배포할 차례입니다. 다시 한 번 강조하지만, backend-gcf 폴더에서 실행해야 합니다.

```bash
# backend-gcf 폴더에서 실행
gcloud functions deploy extract-and-translate \
  --gen2 --max-instances 1 \
  --region $REGION \
  --runtime=python312 --source=. \
  --trigger-http --entry-point=extract_and_translate \
  --no-allow-unauthenticated

# 해당 함수를 서비스 계정에서 호출할 수 있도록 허용
gcloud functions add-invoker-policy-binding extract-and-translate \
  --region=$REGION \
  --member="serviceAccount:$SVC_ACCOUNT_EMAIL"
```

배포 결과:

![Deployment Result](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_12.png)

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

여기 한 가지 멋진 것이 있어요... VS Code의 Cloud Code 확장 프로그램을 통해 이제 Google Cloud에서 배포된 클라우드 함수를 볼 수 있어요!

![image](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_13.png)

## 클라우드 함수 테스트

우리는 약간 다른 curl 명령어만 필요해요:

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

```bash
curl -X POST https://$REGION-$PROJECT_ID.cloudfunctions.net/extract-and-translate \
    -H "Authorization: Bearer $(gcloud auth print-identity-token)" \
    -H "Content-Type: multipart/form-data" \
    -F "uploaded=@./testing/images/ua_meme.jpg" \
    -F "to_lang=en"
```

![Image 14](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_14.png)

Let’s test English-to-English with this meme:

![Image 15](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_15.png)

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

아래는 마크다운 형식으로 수정된 이미지 태그입니다.

![2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_16.png](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_16.png)

번역은 다음과 같습니다:

이제 우크라이나어로 번역해 볼게요:

![2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_17.png](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_17.png)

와우! 작동되네요! 이제 프랑스어로 번역을 해보겠습니다:

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

![Link](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_18.png)

와쥐! 모든 테스트가 작동 중인 것 같습니다.

참고: 인증 및 권한이 부여된 자격 증명 없이 함수를 사용하려고 하면 다음과 같은 오류가 발생할 수 있습니다:

```js
<html><head>
<meta http-equiv="content-type" content="text/html;charset=utf-8">
<title>403 Forbidden</title>
</head>
<body text=#000000 bgcolor=#ffffff>
<h1>Error: Forbidden</h1>
<h2>Your client does not have permission to get URL <code>/extract-and-translate</code> from this server.</h2>
<h2></h2>
</body></html>
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

## 새 버전 배포하기

우리가 코드를 업데이트하고 다시 배포하려면, 동일한 배포 명령어를 다시 실행하면 됩니다.

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_19.png)

## 함수 삭제하기

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

만약 클라우드 함수를 삭제하고 싶다면, 다음 명령어를 실행해주세요:

```js
gcloud functions delete extract-and-translate --region=$REGION
```

# Flask UI

우리는 Cloud Run을 사용하여 간단한 Flask Python 웹 애플리케이션을 만들 것입니다. 이 애플리케이션은 폼 페이지를 렌더링하고, 폼을 처리할 것입니다. 그런 다음, 업로드된 이미지를 포함한 폼 응답을 검색한 후, 클라우드 함수를 호출할 것입니다.

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

## 플라스크 웹 응용 프로그램 만들기

이 기사는 플라스크 튜토리얼을 목적으로하지 않았습니다. 그래서 나의 권장 사항은 GitHub 레포지토리의 ui_cr 폴더에있는 코드를 확인해보는 것입니다.

하지만 여기 몇 가지 중요한 포인트가 있습니다:

- 이 응용 프로그램에서 필요한 Python 패키지를 정의하기 위해 requirements.txt를 만들었습니다.
- 플라스크 응용 프로그램을 도커 컨테이너로 패키징하는 책임을 지는 Dockerfile을 만들었습니다. (나중에 Cloud Run에 배포하기 위해 필요합니다.) 이렇게하려면 ui_cr 폴더의 내용을 복사하고, requirements.txt에서 정의된 Python 종속성을 설치하고, 그런 다음 응용 프로그램의 진입점을 정의해야합니다. 즉, python app.py를 실행하는 것입니다.

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

우리의 ui_cr 폴더는 다음과 같은 구조를 가지고 있어요:

```js
├── ui_cr/                - 브라우저 UI (Cloud Run)
    ├── static/             - 프론트엔드를 위한 정적 콘텐츠
    ├── templates/          - 프론트엔드를 위한 HTML 템플릿
    ├── app.py              - Flask 애플리케이션
    ├── requirements.txt    - UI Python 요구 사항
    ├── Dockerfile          - Flask 컨테이너를 빌드할 Dockerfile
    └── .dockerignore       - Dockerfile에서 무시할 파일들
```

## 애플리케이션 코드

저는 코드를 자세히 살펴볼 예정이 아니에요. 하지만 몇 가지 흥미로운 부분과 주의해야 할 점을 강조할게요.

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

app.py를 살펴보겠습니다. 먼저 Flask 애플리케이션을 인스턴스화하는 함수입니다.

```js
def create_app():
    """ 앱 만들고 설정 """
    flask_app = Flask(__name__, instance_relative_config=True)
    flask_app.config.from_mapping(
        SECRET_KEY='dev', # FLASK_SECRET_KEY 환경 변수로 재정의
    )

    # FLASK_로 시작하는 환경 변수들 로드
    # 예: FLASK_SECRET_KEY, FLASK_PORT
    flask_app.config.from_prefixed_env()
    client = translate.Client()
    flask_app.languages = {lang['language']: lang['name'] for lang in client.get_languages()}
    flask_app.backend_func = os.environ.get('BACKEND_GCF', 'undefined')
    return flask_app

app = create_app()
```

- Flask 앱은 세션을 관리하기 위해 비밀 키가 필요합니다. FLASK_SECRET_KEY 환경 변수를 통해 애플리케이션에 키를 전달할 수 있습니다.
- Google 번역 API를 사용하여 번역할 수 있는 사용 가능한 언어 목록을 가져옵니다. 이것을 제 양식의 드롭다운 선택 상자에 채우는 데 사용할 것입니다.
- 대상 함수의 URL을 전달해야 합니다. 다시 한 번 이 작업을 위해 환경 변수를 사용할 것입니다: BACKEND_GCF.

## Flask 홈페이지 요청 처리하기

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

여기는 /.에 대한 요청을 처리하는 곳이에요. 사용자가 페이지를 처음 방문할 때는 GET 요청이 전송되지만, 이미지를 번역하려고 폼을 제출하면 요청은 POST로 수신될 거에요. 그래서 두 가지 모두 처리해야 해요.

```js
@app.route('/', methods=['GET', 'POST'])
def entry():
    """ 이미지 업로드 양식을 렌더링합니다 """
    message = "이미지를 업로드하세요!"
    to_lang = os.environ.get('TO_LANG', 'en')
    encoded_img = ""
    translation = ""

    if request.method == 'POST': # 폼이 게시됨
        app.logger.debug("POST 받음")
        file = request.files.get('file')
        to_lang = request.form.get('to_lang')

        if file is None:
            flash('파일이 없습니다.')
        elif file.filename == '':
            flash('선택된 파일이 없습니다.')
        elif not allowed_file(file.filename):
            filename = secure_filename(file.filename)
            flash(f'{secure_filename(filename)}은(는) 지원되지 않는 이미지 형식입니다. '
                  f'지원되는 형식은: {ALLOWED_EXTENSIONS}')
        else:
            filename = secure_filename(file.filename)
            app.logger.debug("%s 받음", filename)
            app.logger.debug("%s로 번역 중", to_lang)

            # 이미지를 저장할 필요는 없어요. 바이너리로 인코딩만 하려고 해요.
            try:
                img = Image.open(file.stream)
                with BytesIO() as buf:
                    if img_format := img.format: # 예: JPEG, GIF, PNG
                        img.save(buf, img_format.lower())
                        content_type = f"image/{img_format.lower()}"
                        image_bytes = buf.getvalue()
                        encoded_img = base64.b64encode(image_bytes).decode()
                    else:
                        flash('이미지 형식을 결정할 수 없습니다.')
            except UnidentifiedImageError:
                # 폼을 다시 제출하면 발생합니다
                flash('이미지를 처리할 수 없습니다.')

            if encoded_img:
                message = f"<{secure_filename(filename)}> 처리완료. 새 이미지를 업로드하세요."
                func_response = make_authorized_post_request(endpoint=app.backend_func,
                                        image_data=image_bytes, to_lang=to_lang,
                                        filename=filename, content_type=content_type)
                app.logger.debug("함수 응답 코드: %s", func_response.status_code)
                app.logger.debug("함수 응답 텍스트: %s", func_response.text)
                translation = func_response.text

    return render_template('index.html',
                           languages=app.languages,
                           message=message,
                           to_lang=to_lang,
                           img_data=encoded_img,
                           translation=translation), 200
```

입력을 유효성 검사하고, 이미지가 업로드되었고 유효한 이미지인 경우에만 클라우드 함수로 요청을 보내요. 이 요청은 업로드된 이미지의 원시 바이트를 전달해야 해요.

또한 사용자에게 업로드된 이미지를 반환된 페이지에 표시하고 싶어요. 백엔드에 디스크에 이미지를 저장하지 않으려고 해서 이 방법을 채택했어요:

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

- 업로드된 이미지를 메모리에 있는 BytesIO 개체로 변환합니다.
- 메모리에 있는 객체를 JPEG로 변환합니다.
- 이미지의 원시 바이트 데이터를 버퍼에서 검색합니다. (이것은 make_authorized_post_request() 함수로 전송할 원시 데이터입니다.)
- JPEG 바이너리 이미지 데이터를 Base64 인코딩을 사용하여 인코딩합니다. 이는 안전하게 브라우저로 다시 보낼 수 있는 문자열 표현입니다.

## 우리 기능에 인증된 호출 만들기

Cloud Run Flask 애플리케이션에서 클라우드 함수를 호출할 때, 요청 헤더에 서비스 계정 액세스 토큰을 포함해야 합니다. Google 클라이언트 라이브러리는 ADC에서 자격 증명을 자동으로 검색할 것입니다.

```js
def make_authorized_post_request(endpoint:str,
                                 image_data, to_lang:str,
                                 filename:str, content_type:str):
    """
    지정된 HTTP 엔드포인트로 POST 요청을 생성하여 지정된 audience 값 사용하는 google-auth 클라이언트 라이브러리에서 얻은 ID 토큰으로 인증합니다.
    이미지 데이터가 이미지의 bytes 표현인 것을 예상합니다.
    """
    if endpoint == "undefined":
        raise ValueError("Function 엔드포인트를 검색할 수 없습니다.")

    # Cloud Functions는 함수의 URL을 `audience` 값으로 사용합니다.
    # Cloud Functions의 경우, `endpoint` 및 `audience`는 동일해야 합니다.
    # ADC는 유효한 서비스 계정 자격 증명을 필요로 합니다.
    audience = endpoint
    auth_req = GoogleAuthRequest()

    # 서비스 ID에 대한 OAuth 2.0 액세스 토큰 요청
    # 인스턴스 메타데이터 서버나 로컬 ADC와 함께 사용합니다. 예:
    # export GOOGLE_APPLICATION_CREDENTIALS=/path/to/svc_account.json
    id_token = google.oauth2.id_token.fetch_id_token(auth_req, audience)

    headers = {
        "Authorization": f"Bearer {id_token}",
        # "Content-Type": "multipart/form-data" # requests 라이브러리가 내용 유형을 자동으로 결정하도록 합니다.
    }

    files = {
        "uploaded": (filename, image_data, content_type),
        "to_lang": (None, to_lang)
    }

    # 클라우드 함수에 HTTP POST 요청을 보냅니다.
    response = requests.post(endpoint, headers=headers, files=files, timeout=10)

    return response
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

## 사용자 입력 받아오기

플라스크(Flask)는 Jinja2 템플릿을 사용하여 콘텐츠를 브라우저로 렌더링합니다. 이 템플릿은 내장 코드를 포함하는 HTML 파일입니다. 앱 파일(app.py)에서 render_template()를 호출할 때, 템플릿에서 참조되는 여러 변수를 전달합니다.

## 실행 및 디버깅

로컬에서 플라스크(Flask) 앱을 실행하는 몇 가지 방법이 있습니다:

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

### VS Code 대화형 디버거를 사용하고 싶다면, 다음과 같이 보이는 실행 구성을 생성하는 것을 추천합니다:

```json
{
  "configurations": [
    {
      "name": "Python Debugger: Flask",
      "type": "debugpy",
      "request": "launch",
      "module": "flask",
      "cwd": "${workspaceFolder}/app/ui_cr",
      "env": {
        "FLASK_APP": "app.py",
        "FLASK_DEBUG": "1",
        "FLASK_RUN_PORT": "8080"
      },
      "args": ["run", "--debug", "--no-debugger", "--no-reload"],
      "jinja": true,
      "autoStartBrowser": false
    }
    // 다른 설정들
  ]
}
```

## 애플리케이션 테스트하기

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

알겠어요, 어플리케이션을 실행할 준비가 되었어요!

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_20.png)

브라우저에서는 이렇게 보일 거에요:

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_21.png)

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

저희의 우크라이나 미믹을 번역하는 데 사용해 봐요:

![Building a Serverless ImageText Extractor and Translator Using Google Cloud Pre-Trained AI](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_22.png)

만세!

# Google Cloud Run에 배포하기

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

이제 우리는 Flask 응용 프로그램을 Cloud Run에 배포할 준비가 되었습니다. Cloud Run은 서버 레스 컨테이너 런타임인만큼 배포하기 전에 응용 프로그램을 컨테이너 이미지로 패키징해야 합니다.

다음 단계를 실행해 보겠습니다:

- Flask 앱 컨테이너 이미지를 저장할 Google Artifact Registry (GAR) 리포지토리를 만듭니다.
- 소스에서 컨테이너 이미지를 빌드하고 GAR에 저장하기 위해 Cloud Build를 사용합니다.
- GAR에서 응용 프로그램을 Cloud Run으로 배포합니다.

```js
gcloud artifacts repositories create image-text-translator-artifacts \
  --repository-format=docker \
  --location=$REGION \
  --project=$PROJECT_ID
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

아래 링크에서 클라우드 콘솔에 레포지토리가 생성되었는지 확인할 수 있어.

[이 링크](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_24.png)

이제 도커 이미지를 빌드해주세요.

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

```bash
export IMAGE_NAME=$REGION-docker.pkg.dev/$PROJECT_ID/image-text-translator-artifacts/image-text-translator-ui

# Google Cloud CLI를 사용하여 Docker를 구성하여 Artifact Registry에 인증 요청합니다.
gcloud auth configure-docker $REGION-docker.pkg.dev

# 이미지 빌드 및 Artifact Registry에 푸시합니다
# ui_cr 폴더에서 실행합니다.
gcloud builds submit --tag $IMAGE_NAME:v0.1 .
```

몇 분 정도 걸릴 수 있습니다. 이제 컨테이너 이미지가 레포지토리에 푸시되었습니다:

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_25.png)

마지막으로, 이미지를 사용하여 Cloud Run에 배포할 수 있습니다.

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
# Flask 애플리케이션을 위한 랜덤 시크릿 키 생성
export RANDOM_SECRET_KEY=$(openssl rand -base64 32)

gcloud run deploy image-text-translator-ui \
--image=$IMAGE_NAME:v0.1 \
--region=$REGION \
--platform=managed \
--allow-unauthenticated \
--max-instances=1 \
--service-account=$SVC_ACCOUNT \
--set-env-vars BACKEND_GCF=$BACKEND_GCF,FLASK_SECRET_KEY=$RANDOM_SECRET_KEY
```

다음은 출력 결과입니다:

![이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_26.png)

Google Cloud 콘솔에서 서비스가 성공적으로 배포되었는지 확인할 수 있습니다.

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

## 리디플로잉

새로운 버전의 애플리케이션을 배포하려면 다음과 같이 할 수 있어요:

```js
# IMAGE_NAME이 제대로 설정되었는지 확인하세요
export IMAGE_NAME=$REGION-docker.pkg.dev/$PROJECT_ID/image-text-translator-artifacts/image-text-translator-ui
# 새 버전 번호 설정하기
export VERSION=v0.2

# 컨테이너 이미지를 다시 빌드하고 GAR에 푸시하기
gcloud builds submit --tag $IMAGE_NAME:$VERSION .

# Flask 애플리케이션을 위한 랜덤 시크릿 키 생성
export RANDOM_SECRET_KEY=$(openssl rand -base64 32)

# 리디플로잉
gcloud run deploy image-text-translator-ui \
  --image=$IMAGE_NAME:$VERSION \
  --region=$REGION \
  --platform=managed \
  --allow-unauthenticated \
  --max-instances=1 \
  --service-account=$SVC_ACCOUNT \
  --set-env-vars BACKEND_GCF=$BACKEND_GCF,FLASK_SECRET_KEY=$RANDOM_SECRET_KEY
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

## 필요에 따라 사용자 정의 DNS 매핑 설정하기

https://image-text-translator-ui-adisqviovq-ez.a.run.app/와 같은 URL은 기억하기 어렵습니다! 사용자 정의 도메인으로 매핑하고 싶을 것입니다. 자세한 안내는 여기에서 찾아볼 수 있어요.

예를 들어, image-text-translator.mydomain.com과 같은 서브도메인을 만들었고 이 주소를 애플리케이션에 사용하고 싶다고 가정해 봅시다.

```js
# Google에 도메인 소유권 확인하기
gcloud domains verify mydomain.com
# 확인하기
gcloud domains list-user-verified

# 도메인에 매핑하기
gcloud beta run domain-mappings create \
  --region $REGION \
  --service image-text-translator-ui \
  --domain image-text-translator.mydomain.com
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

이제 이 도메인 매핑에 대한 DNS 레코드를 확보해야 합니다:

```js
# DNS 레코드를 가져옵니다. `resourceRecords` 아래의 모든 것을 원합니다.
gcloud beta run domain-mappings describe \
  --region $REGION \
  --domain image-text-translator.mydomain.com
```

resourceRecords 아래에 나타나는 모든 DNS 레코드를 가져와서 DNS 등록기에서 이 DNS 레코드들을 만들어야 합니다. 저한테는 추가해야 할 CNAME 레코드가 한 개만 있었습니다:

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

이미지 태그를 Markdown 형식으로 바꿔주세요.

[이미지](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_29.png)

DNS 레코드가 전파되고 Google이 관리 SSL 인증서를 프로비저닝하는 데 꽤 많은 시간이 걸릴 수 있습니다. 저는 거의 2시간이 걸렸어요.

기다리는 동안 다음 도구들로 진행 상황을 확인할 수 있어요:

- [Google Toolbox](https://toolbox.googleapps.com/apps/dig/)
- [SSL Labs](https://www.ssllabs.com/ssltest/)

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

하지만 마침내... 모든 것이 내 도메인에서 작동 중입니다!

![Building a Serverless Image Text Extractor and Translator Using Google Cloud Pre-Trained AI](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_30.png)

# 가격 및 비용 관리

작성 시점에서...

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

## Google Cloud Functions

비용은 기능 호출, 사용된 컴퓨팅 및 네트워크 외부 전송의 총합입니다.

- 매달 첫 번째 백만 컴퓨팅 초는 무료이며, 첫 번째 200만 기능 호출도 무료입니다.
- 처음 400,000 GB-초 및 처음 200,000 GHz-초의 컴퓨팅 시간은 무료입니다.
- 또한, Cloud Functions은 사용되지 않을 때 0으로 축소되므로, 때로는 불규칙하거나 저사용량 워크로드에 매우 비용 효율적일 수 있습니다.

## Google Cloud Run

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

비용은 사용된 CPU와 메모리의 조합으로 결정됩니다.

- 매월 첫 240,000 vCPU-초는 무료입니다. 이후에는 vCPU-초당 $0.00001800으로 계산됩니다.
- 매월 첫 450,000 GiB-초는 무료입니다. 이후에는 GiB-초당 $0.00000200으로 메모리 비용이 부과됩니다.
- Cloud Run은 사용되지 않을 때 스케일을 0으로 조정하므로, 간헐적 또는 저부하 워크로드에 매우 비용 효율적일 수 있습니다.

## **Cloud Vision API**

이미지 당 매월 처음 1000개의 텍스트 감지는 무료입니다. 그 후에는 매 1000개 이미지 당 $1.50로 청구됩니다.

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

## 구름 번역 API

매달 처음 50만 글자는 감지 및 번역 호출 모두 무료입니다. 그 이후에는 100만 글자당 20달러가 청구됩니다.

## 일부 비용 통제 전략들

이 비용 통제 전략들을 시행하는 것을 고려해보시기 바랍니다:

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

- 지불 계정에 예산 경보를 설정하세요. 예산 임계값이 초과되면 이메일 알림을 받게 됩니다 (예: 50%, 75%, 90%, 100%). 참고로, 이는 지출을 제한하는 것이 아니라, 임계값이 충족될 때 알림을 보내는 것입니다.
- 오토스케일링을 제한하세요. Cloud Run과 Cloud Functions은 모두 서버리스 오토스케일링 서비스이지만, 작은 애플리케이션에 대규모 수요가 예상되지는 않습니다. 따라서, Cloud Function과 Cloud Run 서비스의 max-instances를 각각 1로 설정했습니다. 이는 각 서비스가 한 번에 하나의 동시 인스턴스 이상을 배포하지 않음을 의미합니다.
- 더 정교한 전략: 예산을 초과하면 Pub/Sub에 알림을 보내세요. Pub/Sub 이벤트를 사용하여 Cloud Function을 트리거하고, 해당 Cloud Function에서 프로젝트를 지불 계정에서 분리하여 프로젝트의 모든 리소스를 비활성화합니다.

## 성능 고려 사항

0으로 스케일링하는 서버리스 서비스의 단점 중 하나는, 수요가 적고 불규칙한 경우 일반적으로 서비스의 실행 인스턴스가 없을 것이라는 점입니다. 수요가 적고 불규칙한 경우에는 Cloud Functions에는 나름대로 빠르게 시작하며 가벼운 특징이 있기 때문에 별 문제가 되지 않습니다. 그러나 Cloud Run Flask 애플리케이션은 약간 더 크기 때문에 차가운 시작하는 데 몇 초가 걸릴 수 있습니다. 그래서 사용자가 페이지를 방문할 때 애플리케이션이 느리게 느껴질 것입니다.

Cloud Run에서 이 문제를 다루는 몇 가지 전략이 있습니다:

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

- 최소 인스턴스 수를 1로 설정할 수 있습니다. 이렇게 하면 클라우드 런 서비스가 절대 0으로 축소되지 않습니다. 항상 요청을 처리할 준비가 된 인스턴스가 있을 것입니다. 그러나 이는 이 인스턴스를 항상 사용 중이라는 것을 의미합니다. 이것은 종종 좋은 전략이 됩니다. 그러나 노디, 저 사용량 애플리케이션에 대해 이렇게 하고 싶지는 않습니다.
- 클라우드 런 시작 CPU 부스트를 구성할 수 있습니다. 여기서 Google Cloud는 클라우드 런 컨테이너에 더 많은 CPU를 동적으로 할당하여 시작할 때 우리의 시작 시간을 현격히 개선할 수 있습니다. 또한 이 추가 CPU는 드물게 발생하는 콜드 스타트에만 할당되기 때문에 항상 인스턴스를 유지하는 것보다 비용 효율적일 가능성이 높습니다.

```bash
gcloud beta run services update image-text-translator-ui \
  --region=$REGION --cpu-boost
```

또한 Flask 애플리케이션이 디버깅이 가능한 상태로 배포되지 않았는지 확인하세요. 이는 확실히 시작 시간에 영향을 줄 것입니다!

# 일부 FAQs, 일반적인 관찰 및 팁

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

## 창의적인 AI!

물론, Gen AI에 대해 다시 한 번 언급해야겠죠!! 실제 솔루션에서 Gen AI를 사용한 적은 없지만, 솔루션을 개발하면서 조언과 답변을 얻는 데 상당히 활용했습니다. 특히, Gemini Code Assist(이것은 VS Code의 클라우드 코드 플러그인의 일부로 제공됩니다)와 ChatGPT 4와 대화를 자주 나눴습니다. 이 도구들을 문제 해결에 활용한 것으로 추정되는데, 전체 시간과 노력의 40%를 아마 절감했을 겁니다.

## 왜 Cloud Run과 Cloud Functions을 동시에 사용했을까요?

내 Flask 애플리케이션에서 추출 및 번역 API 호출을 바로 포함시키지 않고 Cloud Functions를 사용했다면 어땠을까요? 그러면 Cloud Functions가 전혀 필요하지 않았을 텐데요.

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

Two 가지 이유가 있었습니다:

- UI 로직과 추출 및 번역 로직을 분리하고 싶었어요. 이렇게 하면 나중에 UI를 바꾸거나 (예를 들어 모바일 앱을 추가하는 경우) 변경하지 않고도 클라우드 함수를 계속 사용할 수 있게 되었거든.
- 두 서버리스 구성 요소를 통합해야 하는 애플리케이션을 개발하고 싶었어요. 이렇게 하면 한 구성 요소에서 다른 구성 요소를 호출하는 방법을 보여줄 뿐만 아니라 서비스 계정 사용과 같은 필수 요소도 보여줄 수 있게 되었어요.

# 끝맺음

모든 과정이 끝났어요! 우리가 이룬 것들을 다시 한번 정리해볼까요?

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

- 우리는 애플리케이션을 호스팅하기 위해 Google Cloud 프로젝트를 만들었습니다.
- 로컬 개발 프로젝트와 환경을 설정했습니다.
- 서비스 계정을 정의하고 역할을 할당했습니다.
- 어플리케이션 기본 자격 증명을 구성하여 코드가 배포된 환경에서 자격 증명을 찾을 수 있도록 했습니다.
- 이미지 데이터를 받아와 텍스트를 추출하고 지정된 언어로 번역하는 Google의 AI API를 호출하여 Python으로 구현된 Google Cloud Function을 구축했습니다.
- Functions Framework을 사용하여 로컬에서 클라우드 함수를 테스트했습니다.
- 클라우드 함수를 GCP에 배포하고 테스트했습니다. 사용자 인증 및 권한 부여된 클라이언트만 호출할 수 있습니다.
- Flask를 사용하여 Python 웹 사용자 인터페이스 애플리케이션을 구축했습니다.
- HTML Jinja 템플릿, CSS 스타일시트 및 Javascript를 사용하여 프론트 엔드를 구축했습니다.
- Flask 웹 앱이 함수 호출에 인증된 요청을 할 수 있도록 구성했습니다.
- 로컬에서 Flask 애플리케이션을 테스트한 후 Google Cloud Build를 사용하여 애플리케이션을 컨테이너 이미지로 패키징했습니다.
- 이미지를 Google Artifact Registry에 푸시했습니다.
- Artifact Registry에서 Cloud Run에 배포했습니다.
- Cloud Run 서비스를 자체 도메인 이름을 사용하여 노출했습니다.
- 비용 관리의 최상의 관행 및 제어 사항을 검토했습니다.
- Cloud Run의 cold start 속도를 높이기 위해 CPU 부스트를 구현했습니다.

![이미지](https://miro.medium.com/v2/resize:fit:440/0*u6XSWun2IeK0o48E.gif)

이거 정말 재미있었습니다! 여러분도 즐거웠으면 좋겠네요!

# 다음은?

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

I'll be back with the second part soon! In Part 2, we'll delve into:

- Implementing Google Cloud infrastructure with Terraform.
- Establishing a CI/CD pipeline for automated deployment.
- Enhancing the application further.

### Before You Go

- If you found this helpful, please share it with those who might benefit. It supports me and could be valuable for them too!
- Show some love with claps! You can clap more than once, you know?
- Drop a comment 💬 if you have any thoughts to share.
- Stay updated by following and subscribing to my content. Visit my Profile Page and click on these icons:

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

![Tarot Image](/assets/img/2024-07-01-BuildingaServerlessImageTextExtractorandTranslatorUsingGoogleCloudPre-TrainedAI_31.png)

# 유용한 링크

## 애플리케이션

- [https://image-text-translator.just2good.co.uk/](https://image-text-translator.just2good.co.uk/)

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

## 어플리케이션의 소스 코드

- GitHub의 소스 코드 저장소

## 개발 환경 설정

- Gcloud CLI 설정
- Gcloud CLI용 Python 설정
- VS Code용 Cloud Code
- VS Code: 샘플 Python Flask 튜토리얼

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

## 클라우드 함수

- Python을 사용하여 Cloud Functions 설정 및 호출하기 - 로컬 개발 포함
- 로컬 개발을 위한 Python용 Functions Framework
- Cloud Functions - 이미지 어노테이션
- Cloud Functions - HTTP 트리거
- CF로 OCR 및 번역

## 클라우드 런

- VS Code의 샘플 애플리케이션에서 클라우드 런 서비스 생성
- Cloud Code for VS Code에서 로컬에서 클라우드 런 서비스 개발
- VS Code에서 클라우드 런 디버깅
- 클라우드 런 - Hello World
- 클라우드 런 - 이미지 처리
- 사용자 정의 도메인을 클라우드 런 서비스에 매핑
- 클라우드 런 가격
- 클라우드 런 시작 CPU 부스트

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

## AI/ML APIs

- Vision API — 이미지 내 텍스트 감지
- Google Cloud Vision API 요금
- Translation API — 텍스트 번역
- Google Cloud Translation API 요금

## 인증

- 애플리케이션 기본 자격 증명 작동 방식
- 애플리케이션 기본 자격 증명 설정
- Python 서비스 간 인증
- 함수 액세스 관리
- 클라우드 함수: 실행을 위한 인증
- 클라우드 런: 서비스 ID
- 클라우드 런: 서비스 간 인증 — 서비스 계정 만들기; Google이 서명한 ID 토큰 가져오기 및 해당 토큰을 헤더에 추가하기.

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

## DNS와 SSL 프로비저닝

- [Google Apps Toolbox](https://toolbox.googleapps.com/apps/dig/)
- [SSL Labs](https://www.ssllabs.com/ssltest/)
