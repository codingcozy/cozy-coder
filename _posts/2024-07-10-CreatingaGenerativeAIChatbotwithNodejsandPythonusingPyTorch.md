---
title: "Nodejs와 Python, PyTorch로 생성적인 AI 챗봇 만드는 방법"
description: ""
coverImage: "/issue-truck.github.io/assets/no-image.jpg"
date: 2024-07-10 01:04
ogImage: 
  url: /issue-truck.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Creating a Generative AI Chatbot with Node.js and Python using PyTorch"
link: "https://medium.com/@tc2018000487/creating-a-generative-ai-chatbot-with-node-js-and-python-using-pytorch-4b9f190199a6"
isUpdated: true
---





인공 지능(AI)은 모두가 챗봇을 개발하기 쉽게 만들어줬어요. 이를 통해 개발자들은 생성적 언어 모델을 사용하여 인간과 대화를 시뮬레이션할 수 있는 애플리케이션을 만들 수 있게 되었어요. 이 기사에서는 Node.js와 Python을 사용하여 생성적 챗봇을 만드는 방법을 살펴보고, PyTorch를 활용한 도구와 기술을 강조할 거예요. 이 챗봇에는 스페인어로 직업 테스트를 수행하는 기능도 포함돼 있을 거에요.

# 도구와 기술

Node.js: 브라우저 외부에서 JavaScript 코드를 실행할 수 있는 JavaScript 실행 환경이에요. 대규모 I/O 작업을 효율적으로 처리하고 확장할 수 있는 기능으로 인해 백엔드 애플리케이션(챗봇 포함)에 적합해요.

PyTorch: Facebook의 AI 연구실에서 개발한 오픈소스 머신 러닝 라이브러리로, 자연어 처리와 컴퓨터 비전과 같은 응용에 널리 사용돼요.

<div class="content-ad"></div>

GPT-3.5 Turbo: OpenAI에서 개발한 최첨단 생성 언어 모델로, 일관성 있고 문맥적으로 관련 있는 텍스트를 생성할 수 있습니다.

# 생성 대화형 챗봇 구현

Node.js와 Python을 사용한 PyTorch를 이용하여 생성 대화형 챗봇을 구현하려면 다음 단계를 따르세요:

## 환경 설정

<div class="content-ad"></div>

- Node.js 프로젝트를 설정하고 종속 항목을 설치하세요:

  shell
  mkdir chatbotpytorchtomas
  cd chatbotpytorchtomas
  npm init -y
  npm install express openai body-parser
  

- Python 환경을 설정하고 종속 항목을 설치하세요:

  shell
  pip install torch transformers openai flask
  

<div class="content-ad"></div>

## GPT-3.5 Turbo 모델 로딩

Node.js: index.js 파일 생성:

```js
const express = require('express');
const { Configuration, OpenAIApi } = require('openai');
const bodyParser = require('body-parser');
```

```js
const app = express();
const port = 3005;
const configuration = new Configuration({
    apiKey: 'YOUR_API_KEY',
});
const openai = new OpenAIApi(configuration);
app.use(bodyParser.urlencoded({ extended: true }));
app.use(express.static('templates'));
async function generateResponse(question) {
    const response = await openai.createChatCompletion({
        model: "gpt-3.5-turbo",
        messages: [{ role: "user", content: question }],
    });
    const answer = response.data.choices[0].message.content.trim();
    return answer;
}
async function vocationalTest(answers) {
    const evaluation = `Based on your answers: ${answers.join(", ")}.`;
    const suggestion = await generateResponse(evaluation + " What university majors in Peru would you recommend?");
    return suggestion;
}
app.get('/', (req, res) => {
    res.sendFile(__dirname + '/templates/index.html');
});
app.post('/chat', async (req, res) => {
    const user_input = req.body.user_input;
    let bot_response;
    if (user_input.toLowerCase().includes("vocational test")) {
        const questions = [
            "What subjects do you like the most in school?",
            "In what activities do you usually excel?",
            "What kind of job do you imagine doing in the future?"
        ];
        let answers = [];
        for (const question of questions) {
            answers.push(req.body[question]);
        }
        bot_response = await vocationalTest(answers);
    } else {
        bot_response = await generateResponse(user_input);
    }
    res.json({ response: bot_response });
});
app.listen(port, () => {
    console.log(`Chatbot listening on port ${port}`);
}
```

<div class="content-ad"></div>

파이썬: Create chatbotpytorch.py:

```js
import openai
from flask import Flask, request, jsonify, render_template
```

```js
openai.api_key = 'API_KEY'
def generate_response(question):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": question}],
    )
    answer = response.choices[0].message["content"].strip()
    return answer
def vocational_test(answers):
    evaluation = f"Based on your answers: {', '.join(answers)}."
    suggestion = generate_response(evaluation + " What university majors in Peru would you recommend?")
    return suggestion
app = Flask(__name__)
@app.route('/')
def home():
    return render_template('index.html')
@app.route('/chat', methods=['POST'])
def chat():
    user_input = request.form['user_input']
    if "vocational test" in user_input.lower():
        questions = [
            "What subjects do you like the most in school?",
            "In what activities do you usually excel?",
            "What kind of job do you imagine doing in the future?"
        ]
        answers = [request.form.get(q) for q in questions]
        bot_response = vocational_test(answers)
    else:
        bot_response = generate_response(user_input)
    return jsonify({'response': bot_response})
if __name__ == '__main__':
    app.run(debug=True, port=3005)
```

# 웹 페이지: index.html

<div class="content-ad"></div>

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generative AI Chatbot</title>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <style>
        body {
            background-color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .chat-container {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 600px;
        }
        .chat-box {
            height: 400px;
            overflow-y: scroll;
            margin-bottom: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #fff;
        }
        .chat-box p {
            margin: 0;
        }
        .chat-box .bot-message {
            background-color: #e1f5fe;
            border-radius: 10px;
            padding: 10px;
            margin-bottom: 10px;
        }
        .chat-box .user-message {
            background-color: #d1c4e9;
            border-radius: 10px;
            padding: 10px;
            margin-bottom: 10px;
            text-align: right;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <h1 class="text-center">Generative AI Chatbot</h1>
        <div id="chatbox" class="chat-box">
            <p class="bot-message"><strong>Bot:</strong> Hello! How can I assist you today?</p>
        </div>
        <div class="input-group">
            <textarea id="user_input" class="form-control" placeholder="Type your message here..."></textarea>
            <div class="input-group-append">
                <button class="btn btn-primary" onclick="sendMessage()">Send</button>
            </div>
        </div>
    </div>
```

```js
    <script>
        function sendMessage() {
            var user_input = $("#user_input").val();
            if(user_input.trim() === '') return;
            
            $("#chatbox").append("<p class='user-message'><strong>You:</strong> " + user_input + "</p>");
            $("#user_input").val('');
            
            $.post("/chat", { user_input: user_input }, function(data) {
                $("#chatbox").append("<p class='bot-message'><strong>Bot:</strong> " + data.response + "</p>");
                $("#chatbox").scrollTop($("#chatbox")[0].scrollHeight);
            });
        }
    </script>
</body>
</html>
```

# 장점과 응용

유연성과 확장성: Node.js를 사용하면 이벤트 기반 아키텍처의 효율성과 확장성을 활용하여 다중 동시 사용자와 대량 데이터를 처리하는 챗봇 애플리케이션을 확장할 수 있습니다.

<div class="content-ad"></div>

빠른 개발: PyTorch와 Node.js의 조합을 통해 개발자들은 기계학습에 대한 깊은 지식이 없어도 빠르게 생성형 챗봇을 구현할 수 있습니다.

잠재적인 응용 분야: 생성형 챗봇은 고객 서비스, 가상 비서, 대화형 튜토리얼, 게임 등 다양한 분야에서 활용되며, 개인화되고 문맥적으로 관련성 있는 응답을 제공함으로써 사용자 경험을 향상시킵니다.

# 결론

생성형 인공지능 챗봇을 만드는 것은 복잡해 보일 수 있지만, 올바른 도구와 언어를 사용하면 매우 쉽게 접근할 수 있는 프로젝트가 될 수 있습니다. 이 실용적인 예제들은 어떻게 Node.js와 Python을 PyTorch와 함께 사용하여 자연스럽고 일관된 대화를 이어나가며 직업 테스트를 위한 추가 기능을 제공하는 챗봇을 만드는지 보여줍니다. 생성형 인공지능 챗봇을 만드는 기본 원칙은 언어나 도구에 상관없이 보편적으로 적용됩니다.