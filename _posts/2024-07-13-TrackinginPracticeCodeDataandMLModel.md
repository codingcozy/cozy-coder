---
title: "실전 활용 코드, 데이터 및 ML 모델 추적하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_0.png"
date: 2024-07-13 03:22
ogImage: 
  url: /assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_0.png
tag: Tech
originalTitle: "Tracking in Practice: Code, Data and ML Model"
link: "https://medium.com/towards-data-science/tracking-in-practice-code-data-and-ml-model-6787a881609c"
---


![Tracking](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_0.png)

Tracking! We’ve all done it before, whether you’re a researcher or an engineer; whether you’re involved in machine learning, data science, software development, or even a profiler (please don’t mind me, I’m into thriller books these days)! Well, what I want to say is that tracking is important and inevitable. In MLOps, we track all its components: code, data, and the machine learning model! 

In this article, we explain the importance of tracking through a practical example where we apply testing across the different steps of a machine learning workflow. The entire codebase for this article is accessible in the associated repository.

Table of contents:

1. Introduction
2. Project setting
3. Code tracking
4. Data tracking
5. ML Model tracking
6. Conclusion

<div class="content-ad"></div>

# 1. 소개

![Tracking](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_1.png)

트래킹은 생산성, 협업 및 시스템 유지 관리를 향상시키기 위해 여러 시스템 구성 요소의 변경 및 상태를 기록하고 추적하는 프로세스로 정의됩니다. MLOps에서 트래킹은 데이터, 머신 러닝 모델(ML 모델) 및 코드를 포함한 머신 러닝 워크플로우의 다양한 단계의 역사적 발전을 추적하는 데 있어서 중요한 원칙 중 하나입니다. 이전에 MLOps 원칙을 소개하는 튜토리얼에서, 모델이 배포된 후 모니터링과 관련해 트래킹을 연결시켰습니다. 사실, 이 두 가지는 관련되어 있지만 약간 다른 개념입니다. 모니터링은 배포 후 시스템 행위의 실시간 관찰 및 분석에 초점을 맞추는 반면, 트래킹은 전체 프로젝트 생애주기 동안 사용됩니다.

왜 트래킹을 해야 할까요? 코드, 데이터 및 모델을 추적함으로써 입력, 출력, 코드 실행, 워크플로우 및 모델을 추적함으로써 재현성을 향상시킵니다. 또한 모델/시스템 행위 및 성능의 이상 감지를 통해 테스트를 개선합니다. 게다가 머신 러닝 개발의 반복적 성격은 트래킹을 필요로 합니다.

<div class="content-ad"></div>

트래킹을 언제 수행해야 할까요? 이전에 언급했듯이, 트래킹은 전체 프로젝트 수명주기에 적용되며 코드, 데이터, 그리고 머신러닝을 동시에 포함합니다. 왜냐하면 이 요소들은 밀접하게 연결되어 있기 때문입니다: 데이터 처리와 ML 모델 개발은 코드를 사용하므로 데이터와 ML 모델을 추적하는 것은 코드를 추적하는 것을 필요로 하며, ML 모델의 성능은 데이터와 코드에 의존하기 때문에 ML 모델을 추적하는 것은 코드와 데이터를 추적하는 것을 필요로 합니다.

트래킹 사용 사례는 무엇일까요? 우리 수기 숫자 분류 프로젝트에서 특정 시나리오를 고려해 봅시다. 개발된 머신러닝 모델이 배포되었다고 상상해보세요. 이 모델은 공개 데이터셋을 사용하여 훈련되었고 개발 및 테스트 과정에서 특정 수준의 정확도를 달성했습니다. 그러나 프로덕션 환경으로 배포된 후, 모델의 성능이 시간이 지남에 따라 저하되었습니다. 모델의 성능 저하는 모델의 행동을 추적함으로써 초기에 감지되었습니다. 또한 MLOps 구성 요소(코드, 데이터 및 ML 모델)를 개별적으로 추적함으로써 이러한 문제의 원인을 발견할 수 있습니다:

- 코드를 추적하여 버그가 있는지 확인하고 이 버그와 관련된 커밋을 신속하게 식별할 수 있습니다. 결과적으로 배포를 일시적으로 되돌릴 수 있고 수정한 후 프로젝트에 다시 통합할 수 있습니다.
- 시간이 지남에 따라 들어오는 데이터의 분포와 특성의 변화를 추적하여 데이터 드리프트와 같은 데이터 관련 문제를 감지할 수 있습니다.
- 시스템 성능의 저하를 감지할 수 있는 능력에 추가로, ML 모델을 추적함으로써 중단 없이 롤백 및 업데이트도 가능합니다.

이 기사는 트래킹 개념에 관한 것이지만, 이것은 또한 제 MLOps 기사 시리즈 중 일부입니다. 또한, 이전 및 다음 튜토리얼을 따르면 워크플로부터 모델 배포 및 트래킹까지 완전한 MLOps 프로젝트를 만들 수 있습니다.

<div class="content-ad"></div>

만약 MLOps에 관심이 있다면, 나의 아티클을 확인해보세요:

- 튜토리얼 1: MLOps의 핵심 구성 요소 탐색하기
- 튜토리얼 2: MLOps 워크플로우에 대한 초보자 친화적 소개
- 튜토리얼 3: MLOps 원칙 소개
- 튜토리얼 4: MLOps를 염두에 두고 기계 학습 프로젝트 구조화하기
- 튜토리얼 5: 실무에서의 버전 관리: 데이터, ML 모델, 코드
- 튜토리얼 6: 실무에서의 테스트: 코드, 데이터, ML 모델
- 튜토리얼 7: 실무에서의 추적: 코드, 데이터, ML 모델

**2. 프로젝트 설정**

이 문서에서는 합성곱 신경망(CNN)을 사용한 손글씨 숫자 분류 프로젝트를 예시로 삼을 것입니다. 구체적으로, 0부터 9까지 범위의 손글씨 숫자가 들어있는 입력 이미지가 주어질 때, 모델은 해당 숫자를 식별하고 해당 레이블을 출력해야 합니다. AI 캔버스는 다음과 같이 구성되어 있습니다:

<div class="content-ad"></div>

![Tracking in Practice Code Data and ML Model](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_2.png)

이 프로젝트는 MLOps 시리즈 기사를 따라 만든 단계별 자습서용으로 제작되었습니다. 구조는 cookiecutter 프로젝트로 찾을 수 있는 MLOps 템플릿을 따릅니다. 또한, 이 프로젝트 구조에 대한 자세한 내용은 이전 글에서 확인할 수 있습니다. 물론, 코드 버전 관리에는 Git을 사용하고 데이터 버전 관리에는 DVC를 사용합니다. 이 프로젝트의 전체 코드베이스는 깃허브 저장소에서 확인할 수 있습니다.

이 글의 나머지 부분에서는 코드, 데이터 및 ML 모델 추적을 추가하여 이를 구현하는 방법을 예시로 설명해 나갈 것입니다.

# 3. 코드 추적

<div class="content-ad"></div>

코드 추적은 기계 학습 프로젝트를 유지하는 데 필수적입니다. 이는 코드 버전, 의존성 변경 및 모든 코드 관련 업데이트를 기록하는 것을 포함합니다. 효과적으로 우리의 코드를 추적하기 위해서는 다음과 같은 실천 방법을 따라야 합니다:

- Git과 같은 버전 제어 시스템을 사용하고 태그, 설명적인 커밋 메시지 및 기타 기능을 활용하여 히스토리를 표시하고 다른 커밋 간을 전환할 수 있습니다. 버전 제어에 대해 더 알고 싶다면, 제 블로그 기사인 "실무에서의 버전 제어: 데이터, ML 모델 및 코드"를 확인해보세요.
- 프로젝트 요구 사항에 적합한 Git 워크플로우를 채택하여 코드 변경과 기능 개발을 추적합니다. 이는 변경 사항이 병합되기 전에 변경 사항이 격리되어 추적이 쉬워지도록 보장합니다. Git 워크플로우에 대해 더 알고 싶다면, "효율적인 버전 제어를 위한 3가지 필수 워크플로우: Git 마스터하기" 또는 "기계 학습 프로젝트를 위한 Git 워크플로우: 제 프로젝트에서 사용하는 Git 워크플로우"를 확인해보세요.
- 파이썬과 같은 도구를 사용하여 의존성 및 버전 관리: 프로젝트를 공유하거나 배포하기 전에 requirements.txt 파일을 작성하고 의존성을 추적하기 위해 버전 제어 시스템에 추가합니다.
- MLOps 플랫폼과 저장소를 통합하여 머신 러닝 수명주기 전반을 조율하여 추적을 용이하게 합니다.
- 배포 후에 다른 실천 방법이 수행되며 CD/CI와 같은 내용은 이후 튜토리얼에서 자세히 다룰 것입니다.

이제 코드 추적에서 주로 사용되는 Git 명령어를 자세히 살펴보겠습니다:

- 저장소의 상태를 확인하기 위해 다음 명령어를 사용합니다: `git status`. 이 명령어는 현재 브랜치의 상태를 보여줍니다. 원격 브랜치와 최신 상태인지, 파일의 변경 사항을 나열하고 추적되지 않는 파일을 볼 수 있습니다.

<div class="content-ad"></div>

```js
이 사진 카드는 할 일이 많아 보이네요! 분명히 작업 중이시군요.

- 브랜치 목록을 보려면 다음 명령 중 하나를 사용합니다:

$ git branch # 모든 로컬 브랜치를 나열하려면
  feature/data-test
* feature/grad_cam
  feature/integration_test
  feature/model-test
  feature/preprocessing_test
  master

$ git branch -r # 모든 리모트 브랜치를 나열하려면
  origin/HEAD -> origin/master
  origin/feature/data
  origin/feature/data-dvc
  origin/feature/data-test
  origin/feature/grad_cam
  origin/feature/integration_test
  # ...

$ git branch -a # 모든 브랜치(리모트와 로컬 브랜치)를 나열하려면
  feature/data-test
* feature/grad_cam
  feature/integration_test
  feature/model-test
  feature/preprocessing_test
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/feature/data
  remotes/origin/feature/data-dvc
  remotes/origin/feature/data-test
  # ...

$ git branch -vv # 자세한 정보가 포함된 모든 브랜치를 나열하려면
  feature/data-test  a976d83 [origin/feature/data-test] test : add features domain validation.
* feature/grad_cam   f959be7 [origin/feature/grad_cam] Merge branch 'feature/integration_test'
  # ...

- 커밋 이력을 보려면
``` 

<div class="content-ad"></div>


$ git log 명령어를 사용하면 상세한 커밋 이력을 확인할 수 있어요.
# ...

# 또는 이렇게 사용할 수도 있어요:
$ git log --pretty=format:"%h %s" 명령어를 사용하면 커밋 ID와 커밋 메시지만 확인할 수 있어요.
f959be7 Merge branch 'feature/integration_test'
eca40ba fix: predict using the latest run.
aa53e29 feat: system integration testing.


- 제가 주로 편리하고 가독성 있게 사용하는 다른 명령어들이 있어요:


$ git diff 명령어를 사용하면 작업 디렉토리와 스테이징 영역 사이의 변경 사항을 볼 수 있어요.

$ git diff --staged 명령어를 사용하면 스테이징 영역과 마지막 커밋 사이의 변경 사항을 볼 수 있어요.

$ git reset <file> 명령어를 사용하면 변경 사항을 스테이징 취소할 수 있어요.

$ git checkout -- <file_name> 명령어를 사용하면 로컬 변경 사항을 삭제할 수 있어요.

$ git revert <commit_hash> 명령어를 사용하면 커밋을 되돌릴 수 있어요.

$ git reset --soft <commit_hash> 명령어를 사용하면 HEAD를 특정 커밋으로 이동하지만 변경 사항을 스테이징 영역에 유지할 수 있어요.


# 4. 데이터 추적


<div class="content-ad"></div>

데이터 추적은 머신러닝 프로젝트를 유지하기 위한 또 다른 필수적인 실천 방법입니다. 데이터 추적에는 데이터 버전, 메타데이터, 적용된 처리 및 시간이 지남에 따른 데이터 품질을 기록하는 것이 포함됩니다. 우리의 데이터를 효과적으로 추적하기 위해서는 다음의 일련의 조치를 따라야 합니다:

- 변경 사항을 추적하고 재현할 수 있도록 하는 데이터 버전 관리.
- 데이터 원본을 추적하고 데이터 처리 및 머신러닝 파이프라인을 통해 데이터가 어떻게 변형되는지 추적하는 데이터 선조.
- 데이터 원본 및 전처리 단계, 적용된 변형 등을 기록하는 메타데이터 로깅.

이전 자습서에서는 데이터 추적을 위해 DVC를 사용했습니다. 이제 데이터 추적에 일반적으로 사용되는 명령어 중 일부를 자세히 살펴보겠습니다:

- 데이터 파일의 상태를 표시하고 최신 상태인지 동기화가 필요한지 확인하려면 다음 명령어를 사용하세요:

<div class="content-ad"></div>


$ dvc status


- To check the correct version of data files based on the current Git commit, use:


$ dvc checkout


- When the data is stored in remote storage, use:

<div class="content-ad"></div>


$dvc pull # 최신 데이터 파일 버전을 로컬 작업 공간으로 가져오려면

$dvc push # 로컬 작업 공간에서 최신 데이터 파일 버전을 원격 저장소로 업로드하려면

$dvc fetch # 원격 저장소에서 데이터 파일을 가져와서 작업 공간에 체크 아웃하지 않고 가져오려면


# 5. ML 모델 추적

만약 중요도에 따라 목록을 정렬해야 한다면, ML 모델 추적을 가장 중요한 것으로 놓겠어요. 모델의 성능을 추적함으로써 시스템의 오작동을 조기에 감지하고 의사 결정을 내리고 상황을 신속하게 수정할 수 있게 도와줍니다.

ML 모델 추적에는 모델의 이름, 아키텍처, 매개변수, 가중치, 실험 추적이 포함됩니다. 또한 모델 훈련에 사용된 코드와 데이터 버전도 추적합니다. 음, 추적해야 할 것이 많죠, 동의해요! MLOps의 세계에 발을 딛기 전에, 저는 항상 실험을 효과적으로 그리고 효율적으로 저장하는 데 어려움을 겪었어요. 그 때, 설정을 저장하기 위해 기본 파일 기반 저장 방식 (pickle 및 csv 파일)과 같은 기본 전통적인 방법을 사용했습니다. 후자는 수동 관리가 필요하고 재현성과 협업 가능성을 제한하기 때문에 확장성이 부족합니다. 다행히도 이 어려움은 저를 더 나은 접근 방법을 연구하도록 이끌었고 MLOps와 관련된 새로운 기술과 도구를 배우게 되었습니다. 오늘날 여러 플랫폼과 도구가 존재하여 MLOps의 다른 단계에 대한 서로 다른 요구 사항을 충족하지만 이것은 이 글의 주제가 아닙니다.


<div class="content-ad"></div>

이 기사에서는 이전 튜토리얼(실전에서의 버전 관리: 데이터, ML 모델, 코드)에서 이미 소개한 MLFlow를 사용하여 ML 모델의 버전 관리를 적용해 보겠습니다.

우선 로컬 MLflow 추적 서버를 시작합니다:

```js
mlflow 서버 --host 127.0.0.1 --port 8080
```

그리고 MLflow 실험을 설정하여 훈련 실행을 조직화하고 관리합니다:

<div class="content-ad"></div>


# 로깅을 위한 추적 서버 URI를 설정합니다.
mlflow.set_tracking_uri(tracking_uri)
# MLflow 실험을 생성합니다.
mlflow.set_experiment(experiment_name)


- MLflow는 실험과 메타데이터를 추적하기 위해 강력한 기능인 mlflow.autolog()를 제공합니다. 이 기능은 메트릭 및 매개변수를 자동으로 로그에 기록합니다. mlflow.autolog()를 학습 코드 이전에 호출해야 합니다:


with mlflow.start_run():
    mlflow.autolog()
    
    # 학습:
    model.compile(loss=loss, optimizer='adam', metrics=[metric])
    history = model.fit(x_train, y_train, epochs=epochs, batch_size=batch_size, verbose=1,
                        validation_data=(x_val, y_val))


MLflow auto-log는 배치 크기, 에포크 수, 옵티마이저 이름을 포함한 약 29개의 매개변수를 저장하며, 손실 값을 포함한 학습 메트릭을 기록합니다. MLflow의 또 다른 강점은 로그를 볼 수 있는 그래픽 인터페이스를 제공하고 심지어 그래프를 표시할 수 있다는 것입니다:


<div class="content-ad"></div>

![2024-07-13-TrackinginPracticeCodeDataandMLModel_3.png](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_3.png)

![2024-07-13-TrackinginPracticeCodeDataandMLModel_4.png](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_4.png)

- 종종, 우리는 다른 메트릭과 매개변수를 기록해야 할 때도 있습니다. 이를 위해 mlflow.log_metrics()와 mlfow.log_params()를 사용하여 달성할 수 있습니다. 여기서는 손실 함수 이름, 정밀도 및 f1을 로깅하는 데 사용했습니다:

```js
with mlflow.start_run():
    mlflow.autolog()
    
    # Train:
    model.compile(loss=loss, optimizer='adam', metrics=[metric])
    history = model.fit(x_train, y_train, epochs=epochs, batch_size=batch_size, verbose=1,
                        validation_data=(x_val, y_val))

    # 평가
    # ...

    # 다른 매개변수 로깅:
    mlflow.log_params({
        'loss': loss,
        'metric': metric,
    })

    # 다른 메트릭 로깅
    mlflow.log_metrics({
        'acc': acc,
        'precision': precision,
        'recall': recall,
        'f1': f1,
        'training_loss': history.history['loss'][-1],
        'training_acc': history.history['accuracy'][-1],
        'val_loss': history.history['val_loss'][-1],
        'val_acc': history.history['val_accuracy'][-1],
        'test_loss': test_loss,
        'test_metric': test_metric
    })
```

<div class="content-ad"></div>

보시다시피, 추가 메트릭과 매개변수가 올바르게 로깅되어 아름답게 표시될 수 있습니다. 또한, 다른 실행을 비교하고 최상의 모델을 선택할 수 있습니다:

![이미지](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_5.png)

뿐만 아니라, MLflow는 자동으로 기타 메타데이터를 저장합니다: git 커밋, 사용자, 훈련 파일 소스, 모델 요약 및 요구사항 파일 등:

![이미지](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_6.png)

<div class="content-ad"></div>

- **'Register Model'** 버튼을 클릭하여 MLFlow UI를 통해 모델을 쉽고 빠르게 등록 및 버전 관리할 수도 있습니다. 모델은 이제 배포를 위해 준비된 상태입니다.

![TrackinginPracticeCodeDataandMLModel_7](/assets/img/2024-07-13-TrackinginPracticeCodeDataandMLModel_7.png)

MLflow와 같은 도구를 사용하는 중요성에 대해 다시 언급하지 않고는 이 섹션을 마무리할 수 없겠어요. 이러한 도구들은 ML 라이프사이클의 복잡성을 관리하는 데 필수적입니다. 개발, 실험 및 배포의 구조와 효율성을 향상시킵니다. MLflow를 사용하면 ML 모델 추적이 보다 효과적이고 유익해집니다. 게다가 MLflow UI는 ML 모델을 관리, 추적 및 비교하는 상호 작용 및 시각적인 방법을 제공합니다.

# 6. 결론

<div class="content-ad"></div>

이 기사의 마지막 부분이에요! 이 기사에서는 가장 중요한 MLOps 원칙 중 하나인 추적에 대해 소개했어요. 추적은 기계 학습 워크플로의 품질, 신뢰성 및 재현성을 보장해줘요. 또한 우리가 향후 기사에서 자세히 다룰 모델을 선택하는 데도 중요한 역할을 하고 있어요.

제 기사를 통해 독자들에게 명확하고 잘 구성되며 쉽게 따라갈 수 있는 자습서를 제공하고 다뤄야 할 주제들에 대한 튼튼한 소개를 제공하여 코딩 및 추론 능력을 향상시키는 것이 목표예요. 저는 끊임없는 자기 발전을 향한 여정을 꾸준히 이어가고 있어요. 이러한 기사를 통해 제가 찾아낸 것들을 여러분과 공유하고 있어요. 필요할 때 자주 제 기사를 소중한 자료로 참고하곤 해요.

이 기사를 읽어주셔서 감사해요. 제 GitHub 프로필에서 제가 제공하는 다양한 자습서 예제를 모두 찾아볼 수 있어요. 제 자습서를 좋아해주신다면, 제 페이지를 팔로우하고 메일링 리스트에 가입해주시면 고맙겠어요. 그렇게 하면 새로운 기사에 대한 통지를 받게 될 거에요. 질문이나 제안이 있으면 언제든 댓글을 남겨주세요.

# 이미지 크레딧

<div class="content-ad"></div>

이 글에 포함된 모든 이미지와 도표는 제가 찍은 것입니다. 출처가 언급되지 않은 항목은 저자가 찍었습니다.