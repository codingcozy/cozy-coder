---
title: "Deep AutoViML과 C을 사용하여 다중 클래스 이미지 분류기 구축하는 방법"
description: ""
coverImage: "/assets/img/2024-07-12-BuildingaMulti-ClassImageClassifierusingDeepAutoViMLandC_0.png"
date: 2024-07-12 23:49
ogImage: 
  url: /assets/img/2024-07-12-BuildingaMulti-ClassImageClassifierusingDeepAutoViMLandC_0.png
tag: Tech
originalTitle: "Building a Multi-Class Image Classifier using Deep AutoViML and C#"
link: "https://medium.com/@codezone/building-a-multi-class-image-classifier-using-deep-autoviml-and-c-62882d5df057"
---


기계 학습의 세계에서 효과적인 이미지 분류기를 구축하는 것은 흥미롭고 동시에 도전적일 수 있습니다. Deep AutoViML (Automated Variational Model Learning)은 딥 러닝 모델 구축 및 튜닝 프로세스를 간단화하는 데 도움이 되는 도구입니다. 본 문서에서는 Deep AutoViML을 활용하여 C#에서 다중 클래스 이미지 분류기를 만드는 단계를 안내해 드리겠습니다.

![image](/assets/img/2024-07-12-BuildingaMulti-ClassImageClassifierusingDeepAutoViMLandC_0.png)

# Deep AutoViML이란 무엇인가요?

Deep AutoViML은 모델 선택, 하이퍼파라미터 튜닝 및 데이터 전처리와 같은 모델 훈련 프로세스의 다양한 측면을 자동화하는 것을 목표로 합니다. 이를 통해 사용자는 최소한의 코드로 강력한 모델을 만들 수 있어 초보자 및 경험 많은 개발자 모두에게 접근하기 쉽습니다.

<div class="content-ad"></div>

# 환경 설정 시작하기

시작하기 전에 개발 환경을 설정해야 합니다. 다음 전제 조건을 충족하는지 확인해주세요:

- Python(Deep AutoViML은 Python 라이브러리이기 때문에)
- .NET Core SDK
- NumSharp 또는 Python과 상호 작용하는 다른 .NET 라이브러리(e.g., Python.NET)

# 데이터 준비하기

<div class="content-ad"></div>

먼저 이미지 데이터를 준비해야 합니다. 데이터셋이 올바르게 레이블이 지정되고 구성되어 있는지 확인하세요. 간단하게, 데이터셋이 다음과 같이 구조화되어 있다고 가정해봅시다:

```js
/data
    /train
        /class1
        /class2
        /class3
    /validation
        /class1
        /class2
        /class3
```

# Python을 이용한 모델 훈련

Deep AutoViML은 Python 라이브러리이기 때문에 모델 훈련 코드는 Python으로 작성해야 합니다. 아래는 예시입니다:

<div class="content-ad"></div>


```python
from deep_autoviml import deep_autoviml as deep

# Load data
train_path = 'data/train'
valid_path = 'data/validation'
train_data, valid_data = deep.load_data(train_path, valid_path)

# Build and train model
model, history = deep.fit(train_data, valid_data, modeltype='Image', keras_options={'class_mode':'categorical'})

# Save the model
model.save('model.h5')
```

## Integrating with C#

After training the model in Python, you can use it in your C# application. Below is an example of how to load and use the trained model in a .NET Core application.

To integrate with Python in a C# application, you can install Python.NET:


<div class="content-ad"></div>

```js
dotnet add package Python.Runtime
```

C# 코드를로드하고 모델을 사용하는 방법:

```js
using System;
using Python.Runtime;

class Program
{
    static void Main()
    {
        // 파이썬 엔진 초기화
        PythonEngine.Initialize();

        // Python 스크립트 및 모델 로드
        using (Py.GIL())
        {
            dynamic keras = Py.Import("keras.models");
            dynamic np = Py.Import("numpy");

            var model = keras.load_model("model.h5");

            // 이미지를로드하고 전처리하는 함수가 있다고 가정
            var image = LoadAndPreprocessImage("path_to_image.jpg");

            // 이미지의 클래스 예측
            var result = model.predict(np.array(image));
            Console.WriteLine($"예측된 클래스: {result}");
        }

        // 파이썬 엔진 종료
        PythonEngine.Shutdown();
    }

    static float[,] LoadAndPreprocessImage(string imagePath)
    {
        // 이미지로드 및 전처리를 여기에서 구현
        // 전처리 된 이미지를 2D float 배열로 반환
    }
}
```

# 결론

<div class="content-ad"></div>

딥 AutoViML과 Python.NET을 활용하면 C# 애플리케이션에 강력한 다중 클래스 이미지 분류기를 구축 및 통합할 수 있습니다. 이 방법은 딥 AutoViML을 사용한 모델 구축의 편리함과 C#을 사용한 애플리케이션 개발의 유연성을 결합하여 이미지 분류 작업에 강력한 솔루션을 제공합니다.