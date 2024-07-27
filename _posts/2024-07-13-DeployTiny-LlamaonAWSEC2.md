---
title: "AWS EC2에 Tiny-Llama 배포하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_0.png"
date: 2024-07-13 22:38
ogImage: 
  url: /assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_0.png
tag: Tech
originalTitle: "Deploy Tiny-Llama on AWS EC2"
link: "https://medium.com/towards-data-science/deploy-tiny-llama-on-aws-ec2-f3ff312c896d"
---


![Tiny Llama](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_0.png)

## 소개

나는 언제나 세상에서 가장 좋은 프로젝트라도 사람들이 사용할 수 없다면 그리 큰 가치가 없다고 생각해 왔습니다. 그래서 기계 학습 모델을 배포하는 법을 배우는 것이 매우 중요하다고 생각합니다. 이 글에서는 작은 대형 언어 모델 Tiny-Llama를 AWS 인스턴스인 EC2에 배포하는 데 초점을 맞추었습니다.

이 프로젝트에 사용한 도구 목록:

<div class="content-ad"></div>

- 딥노트(Deepnote): 협업 데이터 과학 프로젝트에 좋고 프로토타이핑에 좋은 클라우드 기반 노트북 서비스입니다.
- FastAPI: Python으로 API를 구축하는 웹 프레임워크입니다.
- AWS EC2: 클라우드에서 상당한 컴퓨팅 용량을 제공하는 웹 서비스입니다.
- Nginx: HTTP 및 리버스 프록시 서버로, FastAPI 서버를 AWS와 연결하는 데 사용됩니다.
- GitHub: 소프트웨어 프로젝트를 호스팅하는 서비스입니다.
- HuggingFace: 무제한 모델, 데이터셋 및 애플리케이션을 호스팅하고 협업하는 플랫폼입니다.

## Tiny Llama에 대해

Tiny Llama-1.1B는 3조 토큰에 대한 1.1B Llama 사전훈련을 목표로 하는 프로젝트입니다. Llama2와 동일한 아키텍처를 사용합니다.

현재의 대형 언어 모델은 인상적인 능력을 가지고 있지만 하드웨어 측면에서 매우 비용이 듭니다. 많은 분야에서 우리는 하드웨어가 제한되어 있습니다. 스마트폰이나 위성을 생각해 보세요. 따라서 에지 장치에 배포될 수 있도록 작은 모델을 만드는 연구가 많이 진행되고 있습니다.

<div class="content-ad"></div>

여기 인기를 얻고 있는 "small" 모델 목록이 있어요:

- Mobile VLM (Multimodal)
- Phi-2
- Obsidian (Multimodal)

저는 AWS에서 GPU를 사용하지 않고는 유료로 사용하려면 더 긴 시간이 걸릴 것이기 때문에 큰 모델보다는 Tiny-Llama를 사용할 거에요. CPU에서 답변을 얻는 데 시간이 오래 걸리기 때문이에요.

## FastAPI 서비스 개발하기

<div class="content-ad"></div>

프로젝트를 배포하기 전에 물론 만들어야 합니다. 만약 원하신다면, 내 GitHub 저장소를 직접 사용하여 이 글의 첫 부분을 건너뛸 수 있습니다.

그래서 제가 먼저 하는 일은 GitHub에 가서 tiny-llm-ec2라는 새 저장소를 만드는 것입니다 (이미지의 이름에 오타가 있어 죄송합니다).

![GitHub repository screenshot](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_1.png)

이제 HTTP 연결 URL을 복사하고, 저장소를 복제하여 좋아하는 IDE(VScode ❤️)로 엽니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_2.png" />

자, 이제 requirements.txt 파일을 만들어 봅시다. 아래의 패키지들을 추가해 주세요.

```js
fastapi==0.108.0
uvicorn==0.25.0
transformers==4.36.2
einops==0.7.0
accelerate==0.25.0
pydantic==2.5.3
pydantic_core==2.14.6
```

터미널에서 이 명령어를 실행하여 requirements 파일로부터 모든 패키지를 동시에 설치해 보세요.

<div class="content-ad"></div>

```js
pip install -r requirements.txt
```

그래요. 이제 코딩을 시작할 수 있어요! 👨‍💻

저는 tiny-llama-1B 모델을 인스턴스화하고 싶어요. 해당 모델과 모델 카드는 HuggingFace(HF)에서 쉽게 찾을 수 있어요. 

모델 카드는 모델이 어떻게 작동하는지 이해하고 사용하는 방법을 잘 파악할 수 있게 도와줘요. 그래서 HuggingFace에서 좋은 설명이 함께 제공되지 않는 프로젝트들을 항상 의심해요. 🌟


<div class="content-ad"></div>

이제 HF에서 모델 카드에 나와 있는 대로 영감을 받아 입력 쿼리를 바탕으로 답변을 생성하는 함수를 만들었다. 이 작업은 model.py라는 새 파일에서 수행했다.

모델을 사용하는 첫 번째 시간은 Hugging Face로부터 다운로드해야 하기 때문에 시간이 오래 걸린다.

GPU 액세스를 제공하지 않는 AWS의 무료 인스턴스를 사용할 것이기 때문에 이 모델을 CPU에서 실행한다고 기억하라.

```python
# Install transformers from source - only needed for versions <= v4.34
# pip install git+https://github.com/huggingface/transformers.git
# pip install accelerate

import torch
from transformers import pipeline


def model_query(query: str):
    pipe = pipeline(
        "text-generation",
        model="TinyLlama/TinyLlama-1.1B-Chat-v1.0",
        torch_dtype=torch.bfloat16,
        device_map="cpu",
    )

    # We use the tokenizer's chat template to format each message - see https://huggingface.co/docs/transformers/main/en/chat_templating
    messages = [
        {
            "role": "system",
            "content": "You are a friendly chatbot who always funny and interesting. You only reply with the actual answer without repeating my question.",
        },
        {"role": "user", "content": f"{query}"},
    ]
    prompt = pipe.tokenizer.apply_chat_template(
        messages, tokenize=False, add_generation_prompt=True
    )
    outputs = pipe(
        prompt,
        max_new_tokens=256,
        do_sample=True,
        temperature=0.7,
        top_k=50,
        top_p=0.95,
    )


    output = outputs[0]["generated_text"]
    return output


if __name__ == "__main__":
    print(model_query("tell me a joke about politicians"))
```

<div class="content-ad"></div>

이제 완벽합니다. 당신의 기능을 갖추었어요. 단지 API를 통해 이 기능을 사용할 수 있는 웹 서비스를 만들면 누구나 모델에 연결하여 사용할 수 있어요.

이를 위해 Flask, Django와 같은 여러 프레임워크가 있지만, 여기서는 FastAPI를 사용할 거에요.

main.py 파일을 만들어 API의 엔드포인트를 설정해뒀어요.
```python
from fastapi import FastAPI
from model import model_query
from pydantic import BaseModel

class Query(BaseModel):
    prompt: str

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "안녕!"}

@app.post("/query")
async def root(query: Query):
    res = model_query(query=query.prompt)
    return {"message": f"{res}"}
```

<div class="content-ad"></div>

이 경우에서 가장 중요한 API는 "/query" 경로 아래에 있는 것입니다. 이 API는 위에서 정의된 Query 유형의 객체를 입력으로 받습니다. 기본적으로 프롬프트가 정의된 JSON 객체이며, 모델 메시지가 포함된 JSON을 반환합니다.

FastAPI 서비스를 시작하려면 단순히 다음 터미널 명령을 사용하면 됩니다.

```bash
uvicorn main:app --reload
```

로컬호스트에 방금 생성한 서비스에 액세스할 수 있는 링크가 나타납니다. 이 링크를 클릭해 보세요!

<div class="content-ad"></div>


![Screenshot 1](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_3.png)

If you click on that link, you should see a screen with the messaging set on the main API at route “/”.

![Screenshot 2](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_4.png)

A very useful feature of FastAPI is that typing a “/docs” next to the URL will show us all the APIs developed and allow us to use them without using external services such as Postman.


<div class="content-ad"></div>

**![DeployTiny-LlamaonAWSEC2_5.png](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_5.png)**

그럼 인터페이스에서 쿼리 API를 사용해보세요. 그냥 클릭하면 되요. 그러면 사용하는 방법은 꽤 직관적입니다. 이전에 정의된 쿼리 객체의 JSON 필드를 채우면 되요.

이제 모델에게 농담해달라고 요청해볼게요!

(모델을 다운로드하는 데 시간이 오래 걸릴 거라고 기억해주세요)

<div class="content-ad"></div>

여기 모델의 응답이 있어요.

![모델 응답](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_7.png)

모델 응답이 입력 쿼리가 반복되는 형태로 깔끔하지 않다는 걸 알 수 있어요. 원한다면 결과를 더 처리하고 구문 분석하여 개선할 수 있지만, 이 기사의 목적은 아니라서 일단 너무 많은 시간을 허비하기는 원치 않아요.

<div class="content-ad"></div>

프로젝트가 준비되었어요. 이제 모든 것을 GitHub에 저장해야 해요. 코드를 푸시할 때 다음 명령어를 사용해요.

```js
git add . 
git commit -m "feat: api endpoints for model query"
git push origin main
```

이제 GitHub 저장소가 제 경우와 같이 최신 상태인지 확인해 보세요.

![GitHub Repo](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_8.png)

<div class="content-ad"></div>

## AWS EC2 Instance에 배포하기

모델을 배포하는 다양한 옵션을 알고 있는 것은 매우 중요합니다. 구글, 마이크로소프트, AWS를 포함하여 다양한 제공업체가 있습니다. 이 중에서도 각각 다른 비용과 요구 사항에 따라 다른 솔루션을 제공합니다.

나는 특별한 이유가 없어서 AWS를 기반으로 이 글을 쓰기로 선택했습니다. 비즈니스 세계에서 매우 흔하고 필요하기 때문입니다. 그러나 개인이나 친구와 함께 하는 사이드 프로젝트에는 여전히 비용이 많이 드는 것 같습니다.

EC2는 특정 리소스(예: RAM, CPU, GPU 등)를 사용하여 가상 머신을 빌릴 수 있는 서비스입니다. 몇 번의 클릭으로 더 많은 리소스를 요청하여 쉽게 확장할 수 있습니다. 인스턴스를 생성하면 로컬에서 작업하는 것처럼 정상적으로 작업할 수 있는 터미널이 제공되므로 특별한 능력이 필요하지 않습니다.

<div class="content-ad"></div>

자 그럼 출발해 봅시다!

물론 AWS(Amazon Web Services)에 무료로 계정을 생성하고 기본 콘솔에 로그인해야 합니다. 다음과 같은 화면이 보일 것입니다.

이제 'EC2'라는 단어를 클릭해주세요. 보이지 않는 경우에는 검색 바로 찾을 수 있습니다.

![이미지](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_9.png)

<div class="content-ad"></div>

아래 이미지를 통해 프로젝트를 위한 새로운 인스턴스를 생성하는 방법을 확인할 수 있어요. 그런 다음 "인스턴스 시작" 버튼을 클릭해주세요.

![인스턴스 생성](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_10.png)

이제 인스턴스를 위해 몇 가지 간단한 설정을 해야 해요. 이름을 정의하는 것부터 시작해봐요. "fastapi_server"라고 지정하겠습니다. 그리고 우분투(OS 이미지)를 설정해주세요.

![간단한 설정](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_11.png)

<div class="content-ad"></div>

안녕하세요! 지금 사용 중인 인스턴스 유형은 "t3_micro"이며 무료 티어를 제공합니다. 그래서 우리는 앱을 무료로 테스트할 수 있어요. 사용 가능한 한도를 초과하면 사용량에 따라 요금을 지불해야 합니다. 모델을 지원하지 않는 경우에는 약간 더 강력한 인스턴스를 쉽게 사용할 수 있습니다.

또한, ssh 연결을 위해 키페어를 만들어야 합니다. "새 키페어 생성"을 클릭해봅시다.

![이미지](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_12.png)

이 화면이 표시될 겁니다. 여기서 키페어 이름을 지정해야 합니다. 저는 "fastapi_ec2_pair"라는 이름을 사용할 거에요. (이 이름으로 이미 키페어를 만든 적이 있어서 빨간색이 표시되었습니다). 나머지 설정은 기본값으로 남기고 키파법를 생성해요. 브라우저가 파일을 다운로드하는 것을 확인하실 거에요. 이 파일은 나중에 작업 디렉토리에서 사용해야 할 파일입니다.

<div class="content-ad"></div>

이제 각 옵션 SSH, HTTP, HTTPS에서 트래픽을 활성화해야 해요. 마지막으로 인스턴스를 시작할게요. 

[이미지](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_14.png)

우리의 인스턴스들을 시각화해 볼까요? "모든 인스턴스 보기"를 클릭해 주세요.

<div class="content-ad"></div>

이렇게 제 새로운 인스턴스가 보입니다! 설정을 시각화하려면 클릭해 보세요.

![Instance Configuration](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_16.png)

이 인스턴스의 IP 주소는 "Public_IPv4_address" 텍스트 아래에 표시됩니다. 저의 경우에는 13.48.46.248입니다.

<div class="content-ad"></div>


![image1](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_17.png)

Let's explore what happens when we click on the generated IP. An error appears because our instance is empty and there is no project inside.

![image2](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_18.png)

On the previous screen where we were introduced to our AWS instance, there was a tab in the upper right corner labeled "connect." Clicking on it provides instructions on how to connect to the created instance via SSH connection.


<div class="content-ad"></div>

우리가 해야 할 다음 단계는 터미널을 조금 사용해야 합니다. 기본 터미널 명령어에 익숙하지 않은 경우 이 안내서를 참고해 주세요.

AWS에 SSH를 통해 연결하려는 노트북의 폴더를 만들어 주세요. 이전에 생성한 키를 해당 폴더에 붙여넣으십시오.

![image](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_20.png)

<div class="content-ad"></div>

이제 터미널을 열어 이 폴더 내에서 위치를 찾아보세요.

우리는 chmod 명령어를 사용하여 키가 있는 파일의 권한을 변경할 거에요.

```js
chmod 400 fastapi_ec2_key.pem
```

AWS 연결 창에서 찾은 연결 문자열을 복사하세요 (이미지 두 장 위에 있는 부분).

<div class="content-ad"></div>

이 경우에는 이 문자열입니다: (연결을 확인하느냐고 물으면 yes를 입력하세요)

```js
ssh -i "fastapi_ec2_key.pem" ubuntu@ec2-16-171-176-76.eu-north-1.compute.amazonaws.com
```

이제 AWS 인스턴스에 로그인하고, 기본적으로 비어 있는 리눅스 머신에 들어왔습니다.

![2024-07-13-DeployTiny-LlamaonAWSEC2_21](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_21.png)

<div class="content-ad"></div>

Let's update all repositories on your machine. 


sudo apt-get update


Next, we need to install `pip` and `nginx`. We use `pip` to install Python libraries for our project, and `nginx` is a service that helps connect our FastAPI service to the AWS instance's IP address. This way, when visitors access the IP address, they will be directed to FastAPI.


sudo apt install -y python3-pip nginx


<div class="content-ad"></div>

설치가 성공적으로 완료되었는지 확인해 주세요.

이제 nginx를 구성하기 위한 설정 파일을 생성해야 합니다. Vim을 사용하여 새 파일을 만들어 보겠습니다.

```bash
sudo vim /etc/nginx/sites-enabled/fastapi_nginx
```

Vim을 사용해보지 않았다면 조금 어려울 수 있습니다. "i"를 눌러 쓰기 모드로 전환하고 원하는 내용을 입력해 보세요.

<div class="content-ad"></div>

To save and exit, press "esc," then type ":" followed by "wq" and press enter.

I have left a repository link for you to learn how to use VIM.

In this document, we have added the settings below:

```javascript
server {
 listen 80;
 server_name 16.171.176.76;
  location / {
  proxy_pass http://127.0.0.1:8000;
 }
}
```

<div class="content-ad"></div>

여기 제 터미널 스크린샷이에요.

![2024-07-13-DeployTiny-LlamaonAWSEC2_22.png](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_22.png)

새로운 설정을 불러오기 위해 nginx 서비스를 다시 시작해야 해요.

```bash
sudo service nginx restart
```

<div class="content-ad"></div>

거의 끝나갑니다!
이 순간에는 깃허브 프로젝트를 복제할 수 있어요.
복제하는 방법은:


git clone http_link_of_yout_github_project


복제한 프로젝트로 이동하세요.


cd tiny-llm-ec2/


<div class="content-ad"></div>

우리가 요구 사항에 포함시킨 라이브러리들을 설치해 주세요.

```js
pip install -r requirements.txt
```

## 설치 중 오류 발생 시?

우리 인스턴스에는 자원이 제한적이어서 필요한 라이브러리가 너무 크기 때문에 오류가 발생할 수 있습니다. 이 경우에는 pip를 사용하여 한 번에 하나의 라이브러리만 설치하는 것을 제안합니다.

<div class="content-ad"></div>

만약 CPU에 PyTorch만 필요하다면 torch 전체를 설치하는 대신 CPU 버전만 설치할 수 있어요.

이렇게 하면 모든 것을 설치할 수 있을 거에요.


pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu



pip install uvicorn


<div class="content-ad"></div>


pip install fastapi



pip install transformers



pip install accelerate


Now let's launch the web service and the created API using uvicorn.


<div class="content-ad"></div>

When you run the command `python3 -m uvicorn main:app` in your terminal, if you access the IP address in your browser, you might encounter a black screen. This occurs because FastAPI does not support HTTPS by default, allowing only HTTP.

![Click here for the image!](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_23.png)

To fix this, simply change the URL to start with `http://ip`.

<div class="content-ad"></div>

이제 FastAPI의 초기 메시지를 볼 수 있을 겁니다. URL http://ip/docs를 입력하면 API를 사용할 수 있어요.

![image](/assets/img/2024-07-13-DeployTiny-LlamaonAWSEC2_24.png)

쿼리 API를 사용하면 터미널에서 모델이 다운로드되는 것을 볼 수 있어요. 이 시점에서 EC2 인스턴스가 너무 작아서 전체 모델을 메모리에 저장하지 못하는 문제가 발생할 수 있어요.

## 메모리 오류가 나타날까요?

<div class="content-ad"></div>

우리가 할 수 있는 건 인스턴스를 변경하고 더 큰 것을 사용하는 것이에요. 인스턴스를 중지한 후 액션을 클릭하고 인스턴스 설정 - `인스턴스 유형 변경을 선택해주세요.

더 많은 메모리를 제공하는 t3.medium 인스턴스를 선택하고 저장해주세요. 그런 다음 인스턴스를 다시 활성화해주세요.

하지만 이제 IP 주소가 변경되었으니 새 IP로 nginx 설정을 다시 수정해야 해요.

```bash
sudo vim /etc/nginx/sites-enabled/fastapi_nginx
```

<div class="content-ad"></div>

이미지 태그를 Markdown 형식으로 변경해주세요.

```js
sudo service nginx restart
```

## 시간 초과 오류?

또 다른 오류가 발생할 수 있습니다. 서버가 CPU에서 LLM을 사용하기 때문에 실행하는 데 시간이 오래 걸리면 기본값으로 timeout 오류가 발생할 수 있습니다.
최대 timeout을 변경하여 3분으로 설정하도록 nginx 설정을 수정해봅시다.

<div class="content-ad"></div>

한 번 더 설정을 변경해 보겠습니다:

```bash
sudo vim /etc/nginx/sites-enabled/fastapi_nginx
```

이전에 작성한 설정을 다음과 같이 변경해 봅시다. 여기서는 응답을 더 오래 기다리도록 설정했어요. (IP는 꼭 본인의 IP를 사용해야 합니다.)

```js
server {
    listen 80;
    server_name 16.171.176.76;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 180s;  # 3분(180초)으로 설정
    }
}
```

<div class="content-ad"></div>

우리가 uvicorn을 실행하면 모든 것이 완벽하게 작동해야 합니다!

```bash
python3 -m uvicorn main:app
```

잠시 기다리시면 API를 사용하고 모델의 응답을 받을 수 있을 거에요!

<div class="content-ad"></div>

**이제 성공했어요!**

모델을 배포하는 것은 쉬운 작업이 아니지만 사용할 수 있는 무언가를 만드는 데 필수적입니다. 만약 이 프로젝트를 실제로 사용하지 않는다면, 마지막에 EC2 인스턴스를 종료하여 원치 않는 요금을 지불하지 않도록 주의하세요.

**지금까지 훌륭한 일을 해냈습니다!** 🌟

<div class="content-ad"></div>

# 최종 생각

여러 해 동안 기계 학습의 유일한 흥미로운 부분은 모델의 생성과 훈련뿐인 줄 알았습니다. 그러나 실제 응용 프로그램에서는 무슨 소용이 있겠습니까? 고객 또는 대중이 당신이 구축한 것을 사용할 수 없다면.

모델을 배포하는 방법을 이해하는 것은 키 파이프라인의 중요한 부분이며, 절대 쉽지 않은 특정 기술을 필요로 합니다. 이 기사가 AWS에서 인스턴스를 설정하여 기계 학습 모델을 배포하는 방법을 이해하는 데 도움이 되기를 바랍니다.

이 기사가 마음에 든다면 저를 Medium에서 팔로우해주세요! 😁

<div class="content-ad"></div>

🌟 Linkedin ️| 🐦 Twitter | 💻 Website