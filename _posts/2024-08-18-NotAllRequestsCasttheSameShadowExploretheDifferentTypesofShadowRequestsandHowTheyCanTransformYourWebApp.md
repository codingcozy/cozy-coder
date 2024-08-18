---
title: "각기 다른 그림자 요청의 변화 웹 앱을 변화시킬 수 있는 다양한 종류의 그림자 요청 탐구"
description: ""
coverImage: "/assets/img/2024-08-18-NotAllRequestsCasttheSameShadowExploretheDifferentTypesofShadowRequestsandHowTheyCanTransformYourWebApp_0.png"
date: 2024-08-18 11:14
ogImage: 
  url: /assets/img/2024-08-18-NotAllRequestsCasttheSameShadowExploretheDifferentTypesofShadowRequestsandHowTheyCanTransformYourWebApp_0.png
tag: Tech
originalTitle: "Not All Requests Cast the Same Shadow Explore the Different Types of Shadow Requests and How They Can Transform Your Web App"
link: "https://medium.com/@guhaprasaanth/not-all-requests-cast-the-same-shadow-explore-the-different-types-of-shadow-and-how-they-can-02d798cf51d6"
isUpdated: true
updatedAt: 1723951866961
---

그림자 요청 유형의 세세한 부분을 파헤쳐 보고, 웹 응용 프로그램의 성능과 사용자 경험을 혁신할 수 있는 가능성을 발휘해보세요.

![Shadow Requests](/assets/img/2024-08-18-NotAllRequestsCasttheSameShadowExploretheDifferentTypesofShadowRequestsandHowTheyCanTransformYourWebApp_0.png)

그림자 요청은 현대 소프트웨어 개발과 인프라 관리에서 강력한 도구이지만, 일반해 답은 아닙니다. 다양한 그림자 요청은 특정 목적에 맞게 설계되어 있으며 다양한 맥락에서 사용됩니다. 이 섹션에서는 이러한 유형과 이유를 살펴보고, 구현을 설명하기 위해 코드 샘플을 제공할 것입니다.

# 1. 동기식 그림자 요청

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

목적:
동기식 그림자 요청은 실시간으로 라이브 시스템과 그림자 시스템의 응답을 비교하는 것이 중요할 때 사용됩니다. 이러한 요청은 일반적으로 라이브 요청과 동시에 보내며 결과는 동시에 처리됩니다.

사용처:

- 실시간 모니터링: 그림자 시스템의 성능에 대한 즉각적인 피드백이 필요한 환경에서 사용됩니다.
- 고가용성 시스템: 고가용성과 신뢰성이 요구되는 시스템에서는 동기식 그림자 요청이 그림자 시스템의 문제를 즉시 감지하고 해결하는 데 도움이 됩니다.
- A/B 테스팅: A/B 테스팅 시나리오에서 두 서비스 버전 간의 실시간 비교가 필요할 때 유용합니다.

코드 샘플:

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

# Node.js 버전:

```js
const express = require("express");
const axios = require("axios");
const app = express();
const port = 3000;

app.use(express.json());
async function sendShadowRequest(data) {
  try {
    const response = await axios.post("https://shadow-system.example.com/api", data);
    console.log("Shadow Response:", response.data);
    return response.data; // 그림자 응답 반환
  } catch (error) {
    console.error("그림자 요청 전송 중 오류 발생:", error);
    throw error; // 오류 전파
  }
}

app.post("/api/process", async (req, res) => {
  const liveRequestData = req.body;
  try {
    // 그림자 요청 동기적으로 전송
    const shadowResponse = await sendShadowRequest(liveRequestData);
    // 라이브 시스템에 라이브 요청 전송
    const liveResponse = await axios.post("https://live-system.example.com/api", liveRequestData);
    // 라이브 시스템의 응답과 그림자 응답을 사용자에게 반환
    res.json({
      liveResponse: liveResponse.data,
      shadowResponse: shadowResponse,
    });
  } catch (error) {
    res.status(500).json({ error: "요청 처리 중 오류 발생" });
  }
});

app.listen(port, () => {
  console.log(`서버가 포트 ${port}에서 실행 중입니다.`);
});
```

## 설명 (Node.js):

- 동기적 실행:

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

- 이 Node.js 구현에서 그림자 및 실시간 요청은 동기적으로 처리됩니다. 서버는 그림자 요청이 완료될 때까지 기다렸다가 실시간 요청을 보냅니다.
- sendShadowRequest 함수는 await 되어 서버가 그림자 요청이 완전히 처리될 때까지 차단되는 것을 의미합니다.

2. 오류 전파:

- 그림자 요청이 실패하면 오류가 전파되어 전체 프로세스가 실패할 수 있으며 사용자에게 오류 응답이 반환될 수 있습니다.
- 이 방법을 통해 사용자에게 응답하기 전에 그림자 및 실시간 시스템이 올바르게 작동하는지 확인됩니다.

3. 두 응답 반환:

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

- 라이브 및 그림자 시스템에서의 응답이 사용자에게 반환되어, 두 시스템을 직접 비교하거나 확인할 수 있습니다.

## 사용 사례:

- 이 패턴은 라이브 및 그림자 시스템의 무결성이 중요한 시나리오에서 유용하며, 계속 진행하기 전에 두 시스템이 올바르게 작동하는지 확인해야 할 때 유용합니다. 그러나 요청의 차단적인 성질로 인해 응답 시간이 늘어날 수 있습니다.

## Python 버전:

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

```python
import requests
from flask import Flask, request, jsonify

app = Flask(__name__)

def send_shadow_request(data):
    try:
        response = requests.post("https://shadow-system.example.com/api", json=data)
        response_data = response.json()
        print("Shadow Response:", response_data)
        return response_data  # 그림자 응답을 반환합니다.
    except Exception as e:
        print("그림자 요청 보내기 오류:", str(e))
        raise e  # 오류를 전파합니다.

@app.route('/api/process', methods=['POST'])
def process_request():
    live_request_data = request.get_json()
    try:
        # 그림자 요청을 동기적으로 보냅니다.
        shadow_response = send_shadow_request(live_request_data)
        # 라이브 시스템으로 라이브 요청을 보냅니다.
        live_response = requests.post("https://live-system.example.com/api", json=live_request_data)
        live_response_data = live_response.json()
        # 라이브 시스템의 응답과 그림자 응답을 사용자에게 반환합니다.
        return jsonify({
            'liveResponse': live_response_data,
            'shadowResponse': shadow_response
        })
    except Exception as e:
        return jsonify({'error': '요청 처리 중 오류 발생'}), 500

if __name__ == "__main__":
    app.run(port=3000)
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

2. 오류 처리:

- 만일 셰도우 요청이 실패하면 예외가 발생하여 전체 프로세스가 중지될 수 있습니다. 이는 서버가 셰도우 및 라이브 요청이 성공한 경우에만 진행되도록 보장합니다.

3. 두 응답 반환:

- 셰도우 및 라이브 시스템에서의 응답이 모두 클라이언트에게 반환되며, 이는 셰도우 시스템의 정확성을 검증하는 데 유용할 수 있습니다.

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

# 사용 사례:

- 이 접근 방식은 그림자 시스템의 정확성이 중요하고 라이브 시스템과 함께 확인되어야 할 때 유용합니다. 그러나 요청의 동기식 특성은 서버가 두 시스템 모두 응답할 때까지 기다리므로 응답 시간이 느려질 수 있습니다.

# 차이점:

Node.js:

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

- 비동기 작업을 동기적으로 처리하기 위해 async/await을 사용하여 서버가 그림자 및 라이브 요청이 완료될 때까지 기다리도록 보장합니다.
- 그림자 요청이 완료될 때까지 기다려야 하기 때문에 응답 시간이 증가합니다.

Python:

- 그림자 요청을 직접 실행하고 완료될 때까지 기다린 후 라이브 요청을 진행합니다.
- Node.js와 유사하게, 이 접근 방식은 두 시스템이 모두 유효성을 검사하지만 응답 시간이 더 소요된다는 비용이 발생합니다.

양쪽 경우 모두, 그림자 시스템의 응답을 라이브 시스템의 응답과 함께 유효성을 검사할 때, 전체 응답 시간을 지연시키더라도 중요한 경우 동기적인 그림자 요청이 사용됩니다.

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

# 2. 비동기 그림자 요청

목적:
비동기 그림자 요청은 그림자 시스템의 결과를 비교하는 데 시간적 제약이 없을 때 사용됩니다. 그림자 요청은 라이브 요청과 별도로 보내어 결과를 백그라운드에서 처리합니다.

사용처:

- 백그라운드 테스트: 결과를 즉시 필요로 하지 않고 백그라운드에서 테스트가 진행될 수 있는 환경에서 이상적입니다.
- 배포 후 유효성 검사: 라이브 시스템의 성능에 영향을 주지 않고 배포 후 변경 사항을 유효성을 확인하는 데 사용됩니다.
- 장기간 실행 프로세스: 그림자 시스템이 요청을 처리하는 데 오랜 시간이 걸리고 즉시 비교가 필요하지 않은 시스템에서 유용합니다.

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

# 노드.js 버전:

```js
const express = require("express");
const axios = require("axios");
const port = 3000;

const app = express();
app.use(express.json());

function sendShadowRequest(data) {
  axios
    .post("https://shadow-system.example.com/api", data)
    .then((response) => {
      console.log("Shadow Response:", response.data);
    })
    .catch((error) => {
      console.error("Error sending shadow request:", error);
    });
}

app.post("/api/process", async (req, res) => {
  const liveRequestData = req.body;
  // 그림자 요청을 비동기적으로 보냄 (논블로킹)
  sendShadowRequest(liveRequestData);
  try {
    // 라이브 시스템에 라이브 요청을 보내기
    const liveResponse = await axios.post("https://live-system.example.com/api", liveRequestData);
    // 사용자에게는 라이브 시스템의 응답만 반환
    res.json(liveResponse.data);
  } catch (error) {
    res.status(500).json({ error: "라이브 요청 처리 중 오류 발생" });
  }
});

app.listen(port, () => {
  console.log(`서버가 포트 ${port}에서 실행 중입니다.`);
});
```

## 설명 (Node.js):

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

- 비동기 실행:

- Node.js 구현에서는 axios.post를 사용하여 그림자 요청을 비동기적으로 전송합니다. 이는 서버가 그림자 요청이 완료될 때까지 기다리지 않음을 의미합니다.
- sendShadowRequest 함수는 결과를 기다리지 않고 호출되므로 서버는 즉시 라이브 요청을 처리할 수 있습니다.

2. 논블로킹:

- 이 접근 방식의 논블로킹 특성은 그림자 요청이 라이브 요청의 응답 시간에 영향을 미치지 않도록 보장합니다. 그림자 요청은 백그라운드에서 처리됩니다.

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

3. 응답 처리:

- 사용자에게 반환되는 것은 실제 시스템의 응답뿐이며, 그림자 시스템의 응답은 모니터링 목적으로 기록됩니다.

# 사용 사례:

- 이 접근법은 그림자 시스템을 모니터링하거나 유효성 검사해야 하지만 실제 시스템의 성능이나 시간에 영향을 미치지 않도록 하는 경우에 이상적입니다. 사용자 경험에 영향을 주지 않으면서 새 시스템을 점진적으로 테스트하고 싶은 환경에서 일반적으로 사용됩니다.

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

# Python Version:

```js
import requests
import threading
from flask import Flask, request, jsonify

app = Flask(__name__)
def send_shadow_request(data):
    try:
        response = requests.post("https://shadow-system.example.com/api", json=data)
        print("Shadow Response:", response.json())
    except Exception as e:
        print("Error sending shadow request:", str(e))

@app.route('/api/process', methods=['POST'])
def process_request():
    live_request_data = request.get_json()
    # Sending the shadow request asynchronously using a separate thread
    threading.Thread(target=send_shadow_request, args=(live_request_data,)).start()
    try:
        # Sending the live request to the live system
        live_response = requests.post("https://live-system.example.com/api", json=live_request_data)
        live_response_data = live_response.json()
        # Return only the live system's response to the user
        return jsonify(live_response_data)
    except Exception as e:
        return jsonify({'error': 'Error processing live request'}), 500

if __name__ == "__main__":
    app.run(port=3000)
```

## 설명 (Python):

- 비동기 실행:

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

- 이 Python 버전에서는 그림자 요청을 별도의 스레드 (threading.Thread)를 사용하여 비동기적으로 전송합니다. 이를 통해 그림자 요청이 완료될 때까지 기다리지 않고도 주 프로세스가 라이브 요처리를 계속할 수 있습니다.

2. 비차단:

- Node.js 버전과 유사하게, 비동기적 접근 방식을 통해 라이브 요청에 대한 응답 시간이 그림자 요청에 영향을 받지 않습니다. 그림자 요청은 백그라운드에서 독립적으로 실행됩니다.

3. 응답 처리:

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

- 라이브 시스템의 응답은 사용자에게 반환되고, 그림자 시스템의 응답은 내부 사용이나 모니터링을 위해 로그에 남깁니다.

## 사용 사례:

- 이 패턴은 라이브 사용자 경험에 영향을 주지 않고 그림자 시스템을 유효성 검사하거나 테스트할 때 유용합니다. 그림자 시스템이 점진적으로 도입되거나 성능이 중요한 시나리오에서 일반적으로 사용됩니다.

## 차이점:

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

- Node.js는 JavaScript의 비차단 I/O 모델을 활용하기 위해 비동기 프라미스와 axios를 사용하여 그림자 요청을 보냅니다.
- Python은 비동기 동작을 달성하기 위해 쓰레딩을 사용하여 그림자 요청을 별도의 스레드에서 실행하여 주 프로세스를 차단하지 않습니다.

## 3. 선택적 그림자 요청

목적:
선택적 그림자 요청은 특정 유형의 요청에 대해서만 또는 특정 조건 하에서만 전송됩니다. 이 유형의 그림자 요청은 특정 시나리오에 집중하거나 그림자 시스템에 부하를 줄이기 위해 사용됩니다.

사용하는 곳:

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

- 대상 테스트: 특정 기능이나 사용자 세그먼트와 관련된 요청만 테스트해야 할 때 사용됩니다.
- 비용 관리: 그림자 요청의 수를 줄이고, 고영향 영역에만 집중함으로써 비용을 관리하는 데 도움이 됩니다.
- 성능 튜닝: 특정 유형의 요청을 그림자 시스템에서 최적화하거나 튜닝해야 하는 시나리오에서 유용합니다.

코드 샘플:

# NodeJS 버전:

```js
// NodeJS 예제
const express = require("express");
const axios = require("axios");
const port = 3000;

const app = express();
app.use(express.json());

async function sendShadowRequest(data) {
  try {
    const response = await axios.post("https://shadow-system.example.com/api", data);
    console.log("그림자 응답:", response.data);
  } catch (error) {
    console.error("그림자 요청 보내기 오류:", error);
  }
}

app.post("/api/process", async (req, res) => {
  const liveRequestData = req.body;
  // 선택적 그림자 요청 조건 예시
  if (liveRequestData.type === "high_priority") {
    sendShadowRequest(liveRequestData); // 대기할 필요 없이 백그라운드로 실행
  }
  // 라이브 시스템으로 라이브 요청 보내기
  try {
    const liveResponse = await axios.post("https://live-system.example.com/api", liveRequestData);
    res.json(liveResponse.data);
  } catch (error) {
    res.status(500).json({ error: "라이브 요청 처리 오류" });
  }
});

app.listen(port, () => {
  console.log(`서버가 포트 ${port}에서 실행 중입니다.`);
});
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

# 설명:

- Express 애플리케이션: 이 Node.js 코드는 Express.js를 사용하여 단순한 웹 서버를 설정하고 /api/process 엔드포인트에서 수신된 POST 요청을 수신합니다.
- 선택적인 그림자 요청:

  - 요청이 수신되면, 서버는 요청 타입이 "high_priority" 인지 확인합니다.
  - 조건이 충족되면 axios 라이브러리를 사용하여 그림자 시스템으로 그림자 요청을 보냅니다. 이는 응답을 기다리지 않고 실행되며, 메인 프로세스가 계속 실행되는 동안 배경에서 실행됩니다.

3. 요청 처리:

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

- 라이브 시스템으로 라이브 요청이 axios.post를 사용하여 전송되고, 라이브 시스템으로부터의 응답은 사용자에게 리턴됩니다.
- 그 반면에 섀도우 요청은 백그라운드에서 처리됩니다. 그 응답은 로깅되지만, 이는 사용자에게 전달되는 라이브 시스템의 응답을 방해하지 않습니다.

# Python 버전:

```js
import requests
from flask import Flask, request, jsonify
import threading
app = Flask(__name__)

def send_shadow_request(data):
    try:
        response = requests.post("https://shadow-system.example.com/api", json=data)
        print("Shadow Response:", response.json())
    except Exception as e:
        print("Error sending shadow request:", str(e))

@app.route('/api/process', methods=['POST'])
def process_request():
    live_request_data = request.get_json()
    # 선택적 섀도우 요청을 위한 예제 조건
    if live_request_data.get("type") == "high_priority":
        threading.Thread(target=send_shadow_request, args=(live_request_data,)).start()
    try:
        # 라이브 시스템으로 라이브 요청을 전송
        live_response = requests.post("https://live-system.example.com/api", json=live_request_data)
        live_response_data = live_response.json()
        # 사용자에게는 라이브 시스템의 응답만 전달
        return jsonify(live_response_data)
    except Exception as e:
        return jsonify({'error': '라이브 요청 처리 중 오류 발생'}), 500

if __name__ == "__main__":
    app.run(port=3000)
```

## 설명 (Python):

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

- 선택적 실행:

- 이 Python 구현에서는 수신된 요청의 내용에 따라 그림자 요청을 조건부로 트리거합니다. 예를 들어, 요청에 "type" 필드가 "high_priority"의 값을 포함하는 경우 그림자 요청이 전송됩니다.

2. 비동기 처리:

- Python의 threading 모듈을 사용하여 그림자 요청을 비동기적으로 처리합니다. 이는 메인 프로세스가 그림자 요청의 완료를 기다리지 않고 실시간 요청을 처리하도록 하는 것을 의미합니다. 이는 Node.js 버전에서 사용되는 논블로킹 접근과 유사합니다.

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

# 사용 사례:

- 이 접근 방식은 특정 기준에 따라 분석이나 테스트를 위해 보조 시스템으로 요청을 선택적으로 전송해야 할 때 특히 유용합니다. 그림자 요청을 비동기적으로 처리함으로써 본 시스템의 성능에 영향을 미치지 않고 유지할 수 있습니다.

# 차이점:

Node.js:

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

- 라이브 요청 데이터 유형이 `high_priority` 인지 확인하기 위해 조건부 확인을 사용합니다(if (liveRequestData.type === `high_priority`)).
- 그림자 요청은 비동기적으로 실행되어 라이브 시스템의 응답이 지연되지 않도록 합니다.

Python:

- 비슷한 조건부 확인을 사용합니다(if live_request_data.get("type") == "high_priority") 그림자 요청을 보낼지 결정합니다.
- 그림자 요청은 별도의 스레드에서 처리되어 본 프로세스가 그림자 요청이 완료될 때까지 기다리지 않고 라이브 요청을 계속 처리할 수 있습니다.

# 중요 포인트:

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

- 비동기 처리: 쉐도우 요청을 기다리지 않음으로서 서버는 라이브 요청의 응답 시간에 영향을주지 않습니다. 쉐도우 요청은 비동기적으로 처리됩니다.
- 선택적 쉐도잉: 요청을 쉐도우할지 여부는 유형 필드를 기반으로하기 때문에 선택적인 프로세스입니다. 이는 쉐도우 시스템에 불필요한 부하를 줄이고 중요 요청에만 집중하는 데 도움이 됩니다.

이 패턴은 새로운 시스템이나 기능을 테스트하고 실제 환경에 영향을 미치지 않도록할 때 특히 유용합니다. 이를 통해 특정하고 중요한 요청만이 쉐도우 시스템으로 복제되도록 보장합니다.

양쪽 경우에도 특정 조건에 따라 일부 요청을 복제하고 쉐도우 시스템으로 보내도록 구현된 선택적 쉐도우 요청 패턴이 있습니다. 이 접근 방식은 주 시스템의 성능이 저하되지 않으면서 테스트 또는 모니터링 목적에 효과적입니다.

# 4. 병렬 쉐도우 요청

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

목적:
병렬 그림자 요청은 그림자 요청을 여러 그림자 시스템으로 동시에 보내는 것을 의미합니다. 이는 서로 다른 시스템 또는 동일 시스템의 다른 버전 간의 성능 또는 결과를 비교하기 위해 수행됩니다.

사용하는 장소:

- 다중 버전 테스트: 서비스나 시스템의 다른 버전들의 성능을 비교하는 데 유용합니다.
- 인프라 벤치마킹: 서로 다른 인프라 구성이나 설정을 벤치마킹하여 가장 성능이 우수한 옵션을 결정하는 데 도움이 됩니다.
- 중복성 테스팅: 여러 시스템이 동일한 부하를 처리하고 일관된 결과를 제공할 수 있는지 확인합니다.

코드 샘플:

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

# Node.js 버전:

```js
const express = require("express");
const axios = require("axios");
const port = 3000;

const app = express();
app.use(express.json());

async function sendShadowRequest(data) {
  try {
    const response = await axios.post("https://shadow-system.example.com/api", data);
    console.log("Shadow Response:", response.data);
    return response.data;
  } catch (error) {
    console.error("Error sending shadow request:", error);
    return null;
  }
}

async function sendLiveRequest(data) {
  try {
    const response = await axios.post("https://live-system.example.com/api", data);
    return response.data;
  } catch (error) {
    throw error;
  }
}

app.post("/api/process", async (req, res) => {
  const requestData = req.body;
  try {
    // 라이브 및 섀도우 요청을 동시에 실행합니다.
    const [liveResponse, shadowResponse] = await Promise.all([
      sendLiveRequest(requestData),
      sendShadowRequest(requestData),
    ]);
    // 사용자에게 라이브 시스템 응답을 반환합니다.
    res.json({
      liveResponse,
      shadowResponse, // 디버깅/모니터링을 위해 선택적으로 섀도우 응답을 포함할 수 있습니다.
    });
  } catch (error) {
    res.status(500).json({ error: "요청 처리 중 오류 발생" });
  }
});

app.listen(port, () => {
  console.log(`서버가 포트 ${port}에서 실행 중입니다.`);
});
```

## 설명 (Node.js):

- 병렬 실행:

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

- Node.js를 구현한 이 코드에서는 Promise.all을 사용하여 실시간 요청과 그림자 요청이 병렬로 실행됩니다. 이를 통해 서버는 두 요청을 동시에 시작하여 전반적인 응답 시간을 줄일 수 있습니다.

2. 사용자에게 비차단:

- 두 요청이 병렬로 실행되지만, 주요 초점은 실시간 요청에 있습니다. 서버는 두 요청이 모두 완료될 때까지 기다리지만 사용자에게 보내는 응답은 귀하의 요구에 따라 그림자 시스템의 응답을 포함하거나 제외할 수 있습니다.

# 활용 사례:

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

- 이 패턴은 라이브 시스템과 그림자 시스템의 결과를 실시간으로 비교하고 싶을 때 특히 유용합니다. 사용자에게 동일한 성능 수준을 유지하면서 새로운 시스템을 테스트하거나 보조 시스템의 일관성을 확인해야 하는 시나리오에 이상적입니다.

# Python 버전:

```python
import requests
from flask import Flask, request, jsonify
import concurrent.futures

app = Flask(__name__)

def send_shadow_request(data):
    try:
        response = requests.post("https://shadow-system.example.com/api", json=data)
        return response.json()
    except Exception as e:
        print("그림자 요청 보내는 중 오류 발생:", str(e))
        return None

def send_live_request(data):
    try:
        response = requests.post("https://live-system.example.com/api", json=data)
        return response.json()
    except Exception as e:
        raise e

@app.route('/api/process', methods=['POST'])
def process_request():
    request_data = request.get_json()
    with concurrent.futures.ThreadPoolExecutor() as executor:
        # 라이브 및 그림자 요청을 병렬로 실행합니다
        future_live = executor.submit(send_live_request, request_data)
        future_shadow = executor.submit(send_shadow_request, request_data)
        try:
            live_response = future_live.result()
            shadow_response = future_shadow.result()
            # 사용자에게 라이브 시스템의 응답을 반환합니다
            return jsonify({
                'liveResponse': live_response,
                'shadowResponse': shadow_response  # 디버깅/모니터링을 위해 선택적으로 포함할 수 있습니다
            })
        except Exception as e:
            return jsonify({'error': '요청 처리 중 오류 발생'}), 500

if __name__ == "__main__":
    app.run(port=3000)
```

## 설명 (Python):

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

- 병렬 실행:

- 이 Python 구현에서는 concurrent.futures.ThreadPoolExecutor를 사용하여 라이브 및 셰도우 요청을 병렬로 실행합니다. 이를 통해 두 요청이 동시에 처리되어 두 작업을 완료하는 데 걸리는 시간을 최소화할 수 있습니다.

2. 스레드 관리:

- 스레드 풀을 사용하면 Python 애플리케이션이 여러 스레드를 효율적으로 관리하고, 두 요청이 주 프로세스를 차단하지 않고 처리될 수 있도록 합니다. future.result()의 사용을 통해 주 스레드가 두 요청의 완료를 기다릴 수 있습니다.

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

# 사용 사례:

- 이 방법은 새로운 시스템을 검증하거나 두 시스템 간의 일관성을 보장할 때 두 가지 시스템을 병렬로 실행하는 데 이상적입니다. 주 사용자를 대상으로 하는 주요 프로세스를 지연시키지 않으면서 보조 시스템에서 데이터를 수집해야 하는 성능에 민감한 응용 프로그램에 적합합니다.

# 차이점:

Node.js:

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

- Promise.all을 사용하여 라이브 및 쉐도우 요청을 병렬로 실행합니다.
- 이 접근 방식은 JavaScript의 비동기성을 활용하여 병렬 작업을 효율적으로 관리하며, 두 요청을 동시에 처리하면서 전체 응답 시간을 최소화합니다.

Python:

- concurrent.futures.ThreadPoolExecutor를 사용하여 병렬 실행을 관리합니다.
- 쓰레드 풀을 사용하면 파이썬이 여러 스레드를 효과적으로 처리할 수 있어, 라이브 및 쉐도우 요청의 병렬 처리를 가능하게 합니다. 이는 Node.js 버전과 유사하지만, 파이썬의 쓰레딩 능력을 활용합니다.

두 구현 모두, 병렬 쉐도우 요청 패턴을 사용하여 라이브 및 쉐도우 요청을 동시에 실행하여, 사용자 경험에 영향을 미치지 않고 두 시스템이 모두 테스트되거나 유효화되도록 합니다. 이는 성능이 중요하지만 쉐도우 시스템을 테스트하거나 모니터링해야 하는 시나리오에서 특히 유용합니다.

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

# 5. 시간적 그림자 요청

목적:
시간적 그림자 요청은 특정 간격이나 특정 시간대에 그림자 요청을 보내는 것을 의미합니다. 이는 다양한 조건 하에서 시스템이 어떻게 작동하는지 테스트하는 데 유용합니다. 예를 들어, 다양한 트래픽 패턴이나 피크 시간대에 대한 성능을 확인하는 데 활용됩니다.

사용하는 곳:

- 최대 부하 테스트는 시스템이 고부하 조건을 처리할 수 있는지 확인하기 위해 피크 시간대의 트래픽을 시뮬레이션합니다.
- 시간 기반 기능 테스트: 이 방법은 하루 중 일정 시간에만 활성화되는 기능이나 설정을 테스트하는 데 유용합니다.

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

코드 샘플:

# Node.js 버전:

```js
const express = require("express");
const axios = require("axios");
const port = 3000;

const app = express();
app.use(express.json());
async function sendShadowRequestWithDelay(data, delay) {
  try {
    await new Promise((resolve) => setTimeout(resolve, delay));
    const response = await axios.post("https://shadow-system.example.com/api", data);
    console.log("Shadow Response:", response.data);
  } catch (error) {
    console.error("Error sending shadow request:", error);
  }
}

async function sendLiveRequest(data) {
  try {
    const response = await axios.post("https://live-system.example.com/api", data);
    return response.data;
  } catch (error) {
    throw error;
  }
}

app.post("/api/process", async (req, res) => {
  const requestData = req.body;
  // Sending the shadow request with a temporal delay
  const shadowDelay = 5000; // 5 seconds delay
  sendShadowRequestWithDelay(requestData, shadowDelay);
  try {
    // Sending the live request to the live system
    const liveResponse = await sendLiveRequest(requestData);
    // Return the live system's response to the user
    res.json({
      liveResponse,
      message: "Shadow request will be processed after a delay",
    });
  } catch (error) {
    res.status(500).json({ error: "Error processing live request" });
  }
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

## 설명 (Node.js):

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

- 임시 지연:

- Node.js 구현에서는 암시 요청이 의도적으로 Promise로 래핑된 setTimeout을 사용하여 지연됩니다. 지연 시간은 밀리초로 지정됩니다(5초 지연의 경우 5000). 이는 암시 요청이 지정된 지연 시간 후에만 전송된다는 것을 의미합니다.

2. 실시간 요청에 대한 차단 없음:

- 실시간 요청은 즉시 처리되고 응답되며, 암시 요청 완료를 기다리지 않습니다. 이는 사용자 경험을 지연시키지 않도록 하는 것을 보장합니다.

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

# 사용 사례:

- 임시 그림자 요청은 라이브 시스템이 우선권을 가지도록하고, 그림자 시스템이 주 프로세스 이후에만 트리거되도록하는 시나리오에서 유용합니다. 이는 특히 로드 테스트 또는 그림자 시스템이 라이브 시스템의 성능에 영향을 미치지 않아야 하는 경우에 유용할 수 있습니다.

# Python 버전:

```python
import time
import requests
from flask import Flask, request, jsonify
import threading

app = Flask(__name__)

def send_shadow_request_with_delay(data, delay):
    try:
        time.sleep(delay)
        response = requests.post("https://shadow-system.example.com/api", json=data)
        print("그림자 응답:", response.json())
    except Exception as e:
        print("그림자 요청 보내기 오류:", str(e))

def send_live_request(data):
    try:
        response = requests.post("https://live-system.example.com/api", json=data)
        return response.json()
    except Exception as e:
        raise e

@app.route('/api/process', methods=['POST'])
def process_request():
    request_data = request.get_json()
    # 임시 지연 시간으로 그림자 요청 전송
    shadow_delay = 5  # 5초 지연
    threading.Thread(target=send_shadow_request_with_delay, args=(request_data, shadow_delay)).start()
    try:
        # 라이브 시스템에 라이브 요청 보내기
        live_response = send_live_request(request_data)
        # 사용자에게 라이브 시스템의 응답 반환
        return jsonify({
            'liveResponse': live_response,
            'message': '지연 후 그림자 요청이 처리될 예정입니다'
        })
    except Exception as e:
        return jsonify({'error': '라이브 요청 처리 중 오류가 발생했습니다'}), 500

if __name__ == "__main__":
    app.run(port=3000)
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

## 설명 (Python):

- 시간 지연:

- 이 Python 구현에서 그림자 요청은 time.sleep을 사용하여 지연됩니다. 이 함수는 지정된 시간(5초) 동안 실행을 일시 중지합니다. 이후 그림자 요청이 전송됩니다.

2. 비동기 실행:

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

- 그림자 요청은 threading.Thread를 사용하여 별도의 스레드에서 실행됩니다. 이를 통해 그림자 요청이 완료될 때까지 기다리지 않고 메인 프로세스가 실시간 요청을 처리할 수 있습니다.

# 사용 사례:

- Python에서 시간적 그림자 요청은 더 늦은 시간에 그림자 요청을 연기하고 실시간 시스템이 우선 처리되도록 할 때 유용합니다. 이는 특히 테스트 목적이나 그림자 시스템이 실시간 운영과 독립적으로 작동해야 하는 경우에 특히 유용할 수 있습니다.

# 차이점:

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

Node.js:

- Promise를 사용하여 setTimeout을 감싸서 그림자 요청 실행 전에 지연을 도입합니다.
- 그림자 요청은 비동기적으로 실행되며 라이브 요청이 즉시 처리되도록 하며, 그림자 시스템이 라이브 시스템의 성능에 영향을 미치지 않도록 합니다.

Python:

- time.sleep을 사용하여 지연을 도입하며, threading.Thread를 사용하여 별도의 스레드에서 그림자 요청을 실행합니다.
- Node.js 버전과 유사하게, 라이브 요청은 즉시 처리되고, 그림자 요청은 지연되어 백그라운드에서 처리됩니다.

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

양 구현 모두, 임시 그림자 요청은 라이브 시스템의 성능을 우선시하면서 그림자 시스템이 테스트되거나 모니터링될 수 있도록 설계되었습니다. 그림자 요청의 지연 실행은 주 사용자를 대상으로 한 과정에 간섭하지 않도록 보장하며, 이 패턴은 시스템 부하와 성능을 신중하게 관리해야 하는 시나리오에 유용합니다.

# 6. 조건부 그림자 요청

목적:
조건부 그림자 요청은 시스템 오류 발생 시 또는 라이브 요청 데이터 내에서 특정 기준을 충족할 때와 같이 특정 조건하에만 트리거됩니다. 이 방식은 주로 오류 모니터링 및 대상 테스트에 유용합니다.

사용 예시:

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

- 오류 처리: 라이브 시스템에서 오류가 발생할 때 그림자 요청을 트리거하여 사용자 경험에 영향을 미치지 않고 문제를 진단하고 수정하는 데 도움이 됩니다.
- 대상 기능 테스트: 새로운 기능 또는 설정을 특정 조건 하에만 테스트하는 데 유용합니다.

코드 샘플:

# Node.js 버전: 조건부 그림자 요청

```js
const express = require("express");
const axios = require("axios");
const port = 3000;

const app = express();
app.use(express.json());

async function sendShadowRequest(data) {
  try {
    const response = await axios.post("https://shadow-system.example.com/api", data);
    console.log("그림자 응답:", response.data);
  } catch (error) {
    console.error("그림자 요청 전송 중 오류 발생:", error);
  }
}

async function sendLiveRequest(data) {
  try {
    const response = await axios.post("https://live-system.example.com/api", data);
    return response.data;
  } catch (error) {
    throw error;
  }
}

app.post("/api/process", async (req, res) => {
  const requestData = req.body;
  try {
    // 그림자 요청을 보내기 전 조건 확인
    if (requestData.priority === "high" || requestData.source === "mobile") {
      sendShadowRequest(requestData); // 논블로킹 호출
    }
    // 라이브 시스템으로 라이브 요청 보내기
    const liveResponse = await sendLiveRequest(requestData);
    // 사용자에게는 라이브 시스템의 응답만 반환
    res.json(liveResponse);
  } catch (error) {
    res.status(500).json({ error: "라이브 요청 처리 중 오류 발생" });
  }
});

app.listen(port, () => {
  console.log(`서버가 포트 ${port}에서 실행 중입니다.`);
});
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

## 설명 (Node.js):

- 조건부 실행:

- 이 Node.js 구현에서는 수신된 요청 데이터의 특정 기준에 따라 그림자 요청이 조건부로 실행됩니다. 예를 들어, 우선 순위 필드가 "high"로 설정되어 있거나 소스 필드가 요청이 "mobile" 기기에서 온 것을 나타내는 경우에, 그림자 요청이 전송됩니다.

2. 논블로킹:

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

- 그림자 요청은 논 블로킹 방식으로 전송되므로 서버는 완료를 기다리지 않고 라이브 요청 처리를 계속합니다. 이는 그림자 요청이 라이브 시스템의 성능에 영향을 미치지 않도록 합니다.

# 사용 사례:

- 조건부 그림자 요청은 특정 요청만 그림자화해야 하는 시나리오에서 유용합니다. 요청의 우선 순위, 요청의 원본 또는 응용 프로그램과 관련된 다른 기준에 따라 기반을 둘 수 있습니다. 이는 모든 요청을 그림자화하는 오버헤드 없이 특정 그림자화를 가능하게 합니다.

# Python Version: 조건부 그림자 요청

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

```python
import requests
from flask import Flask, request, jsonify
import threading

app = Flask(__name__)

def send_shadow_request(data):
    try:
        response = requests.post("https://shadow-system.example.com/api", json=data)
        print("Shadow Response:", response.json())
    except Exception as e:
        print("Error sending shadow request:", str(e))

def send_live_request(data):
    try:
        response = requests.post("https://live-system.example.com/api", json=data)
        return response.json()
    except Exception as e:
        raise e

@app.route('/api/process', methods=['POST'])
def process_request():
    request_data = request.get_json()
    # Check the condition before sending the shadow request
    if request_data.get("priority") == "high" or request_data.get("source") == "mobile":
        threading.Thread(target=send_shadow_request, args=(request_data,)).start()
    try:
        # Sending the live request to the live system
        live_response = send_live_request(request_data)
        # Return only the live system's response to the user
        return jsonify(live_response)
    except Exception as e:
        return jsonify({'error': 'Error processing live request'}), 500

if __name__ == "__main__":
    app.run(port=3000)
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

2. 비동기 실행:

- Python의 threading 모듈을 사용하여 그림자 요청을 비동기적으로 실행하면 주 프로세스가 지연 없이 실시간 요청을 처리할 수 있습니다. 이를 통해 사용자는 그림자 요청이 백그라운드에서 처리될 때에도 빠르게 실시간 시스템으로부터 응답을 받을 수 있습니다.

# 사용 사례:

- Python에서 조건부 그림자 요청은 특정 기준에 따라 그림자만 특정 요청에 대해 그림자화해야 할 때 특히 유용합니다. 예를 들어, 요청의 중요성이나 원천에 따라서입니다. 이 접근 방식은 그림자 시스템의 부하를 최소화하고 관련 요청만을 실시간 시스템과 병렬로 처리함으로써 필요한 요청만 처리되도록 합니다.

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

# 차이점:

Node.js:

- 요청 처리 파이프라인 내에서 조건부 로직을 직접 구현합니다. 지정된 조건 (우선순위 또는 소스)에 따라 그림자 요청이 전송됩니다.
- 라이브 요청 처리가 지연되지 않도록 그림자 요청이 비동기적으로 전송됩니다.

Python:

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

- 비슷한 조건부 논리 접근 방식을 사용하지만, threading 모듈을 사용하여 별도의 스레드에서 쉐도우 요청을 실행합니다.
- 라이브 요청은 즉시 처리되며, 조건이 충족되면 쉐도우 요청이 병렬로 실행됩니다.

두 구현 모두 조건부 쉐도우 요청은 사전 정의된 기준에 따라 쉐도우 시스템으로 요청을 선택적으로 보내는 방법을 제공합니다. 이 접근 방식을 통해 불필요한 요청으로 쉐도우 시스템을 과도하게 충격을 주는 것을 방지하고, 라이브 시스템의 최적 성능을 유지하는 데 도움이 됩니다.

# 7. 단계별 쉐도우 요청

목적:
단계별 쉐도우 요청은 쉐도우 시스템으로 보내는 트래픽을 점진적으로 증가시키는 것을 포함하며, 먼저 소량의 요청부터 시작하여 시간이 지남에 따라 증가시켜 나갑니다. 이 기술은 쉐도우 시스템이 증가하는 부하를 처리할 수 있고 성능을 점진적으로 확인할 수 있도록 하는 데 사용됩니다.

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

사용 예시:

- 점진적인 배포: 새로운 서비스 또는 기능을 점진적으로 배포하여 전체 배포 이전에 팀이 성능을 모니터링하고 조정할 수 있는 이상적인 방법입니다.
- 부하 테스트: 새 인프라 또는 구성을 제어된 단계적 방식으로 부하 테스트하는 데 유용합니다.

코드 샘플:

# Node.js Version: Staged Shadow Requests

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
const express = require("express");
const axios = require("axios");
const port = 3000;

const app = express();
app.use(express.json());

async function sendShadowRequestStage1(data) {
  try {
    const response = await axios.post("https://shadow-system-stage1.example.com/api", data);
    console.log("Stage 1 Shadow Response:", response.data);
    return response.data;
  } catch (error) {
    console.error("Error in Stage 1 Shadow Request:", error);
    return null;
  }
}

async function sendShadowRequestStage2(data) {
  try {
    const response = await axios.post("https://shadow-system-stage2.example.com/api", data);
    console.log("Stage 2 Shadow Response:", response.data);
    return response.data;
  } catch (error) {
    console.error("Error in Stage 2 Shadow Request:", error);
    return null;
  }
}

async function sendLiveRequest(data) {
  try {
    const response = await axios.post("https://live-system.example.com/api", data);
    return response.data;
  } catch (error) {
    throw error;
  }
}

app.post("/api/process", async (req, res) => {
  const requestData = req.body;
  try {
    // First stage of the shadow request
    const stage1Response = await sendShadowRequestStage1(requestData);

    // Optionally pass stage 1 response to stage 2
    if (stage1Response && stage1Response.continue) {
      await sendShadowRequestStage2(stage1Response);
    }

    // Sending the live request to the live system
    const liveResponse = await sendLiveRequest(requestData);

    // Return only the live system's response to the user
    res.json(liveResponse);
  } catch (error) {
    res.status(500).json({ error: "Error processing live request" });
  }
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

## Explanation (Node.js):

- 단계별 실행:

- 이 Node.js 구현에서는 그림자 요청이 단계별로 전송됩니다. 첫 번째 그림자 요청(sendShadowRequestStage1)이 처리되며, 그 결과에 따라 두 번째 그림자 요청(sendShadowRequestStage2)이 트리거될 수 있습니다.
- 이 단계별 접근은 각 단계가 다음 단계에 영향을 미칠 수 있는 조건부 진행을 가능하게 합니다.

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

2. 제어된 흐름:

- 첫 번째 단계의 결과가 두 번째 단계가 발생하는지를 결정할 수 있습니다. 예를 들어, 첫 번째 단계의 응답에 필드가 있는 경우, 두 번째 그림자 요청이 전송됩니다. 이를 통해 복잡한 워크플로우를 단계별로 테스트하거나 모니터링할 수 있습니다.

# 사용 사례:

- 단계적 그림자 요청은 순차적 또는 절차별 검증이 필요한 시나리오에서 유용합니다. 이는 한 단계의 출력이 다음 단계에서 수행할 작업이나 테스트를 결정하는 시스템에서 필요할 수 있으며, 그림자 처리 과정을 더 세밀하게 제어할 수 있게 해줍니다.

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

# Python Version:

```js
import requests
from flask import Flask, request, jsonify

app = Flask(__name__)

def send_shadow_request_stage1(data):
    try:
        response = requests.post("https://shadow-system-stage1.example.com/api", json=data)
        print("Stage 1 Shadow Response:", response.json())
        return response.json()
    except Exception as e:
        print("Error in Stage 1 Shadow Request:", str(e))
        return None

def send_shadow_request_stage2(data):
    try:
        response = requests.post("https://shadow-system-stage2.example.com/api", json=data)
        print("Stage 2 Shadow Response:", response.json())
        return response.json()
    except Exception as e:
        print("Error in Stage 2 Shadow Request:", str(e))
        return None

def send_live_request(data):
    try:
        response = requests.post("https://live-system.example.com/api", json=data)
        return response.json()
    except Exception as e:
        raise e

@app.route('/api/process', methods=['POST'])
def process_request():
    request_data = request.get_json()
    # First stage of shadow request
    stage1_response = send_shadow_request_stage1(request_data)
    # Optionally pass stage 1 response to stage 2
    if stage1_response and stage1_response.get("continue"):
        send_shadow_request_stage2(stage1_response)
    try:
        # Sending the live request to the live system
        live_response = send_live_request(request_data)
        # Return only the live system's response to the user
        return jsonify(live_response)
    except Exception as e:
        return jsonify({'error': 'Error processing live request'}), 500

if __name__ == "__main__":
    app.run(port=3000)
```

## 설명 (파이썬):

- Staged Execution:

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

- 이 Python 구현에서는 그림자 요청이 단계적으로 처리됩니다. 첫 번째 그림자 요청 (send_shadow_request_stage1)이 실행되고, 해당 응답은 두 번째 그림자 요청 (send_shadow_request_stage2)이 트리거될지를 결정할 수 있습니다.
- 이 단계별 접근법은 단계별 유효성 검사를 허용하며, 각 단계의 결과가 다음으로의 진행을 영향을 미칩니다.

2. 제어된 흐름:

- 첫 번째 단계의 결과는 두 번째 단계를 조건적으로 트리거할 수 있습니다. 첫 번째 단계의 응답에 true 값을 가진 continue 키가 포함되어 있다면, 두 번째 단계가 실행됩니다. 이 제어된 흐름은 그림자 처리 과정이 구분된 단계로 관리되어야 할 시나리오에 유용합니다.

# 사용 사례:

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

- 스테이징된 셰도우 요청은 시스템의 동작이 점진적으로 확인되어야 하는 복잡한 워크플로우에서 유용합니다. 이 방법은 각 단계가 이전 단계의 성공적인 완료에 의존할 수 있는 테스트 시나리오에서 특히 유용하며, 다음 단계로 진행하기 전에 시스템의 각 부분이 기대한 대로 작동하는지 확인합니다.

# 차이점 요약:

Node.js:

- 각 셰도우 요청 단계를 처리하기 위해 비동기 함수를 사용합니다. 첫 번째 단계의 응답에 따라 흐름이 제어되며, 두 번째 단계는 조건에 따라 트리거됩니다.
- 이 구현은 여러 셰도우 시스템이 순차적인 프로세스에 관여하고 각 단계가 다음 단계에 영향을 줄 수 있는 시스템에서 이상적입니다.

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

파이썬:

- 각 단계가 순차적으로 처리되는 시뮬레이션 실행을 위해 동기 함수를 사용합니다. 두 번째 단계는 첫 번째 단계의 결과에 따라 트리거됩니다.
- 이 접근 방식은 시스템의 동작이 점진적으로 확인되어야 하는 테스트 또는 모니터링 시나리오에 적합합니다.

두 구현 모두 단계별 섀도 요청을 사용하여 각 단계를 거침에 따라 다단계 섀도 처리를 할 수 있게 합니다. 시스템의 각 부분을 순차적으로 테스트하거나 모니터링해야 하는 복잡한 시스템에 특히 유용하며, 각 단계가 올바르게 작동한 후에 다음 단계로 이동할 수 있도록 보장합니다.

# 8. 트래픽 분할 섀도 요청

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

목적:
트래픽 분할 쉐도우 요청은 지리적 위치, 사용자 유형 또는 특정 기준에 따라 교통을 여러 쉐도우 시스템으로 분할하는 것을 의미합니다. 이 방법은 다양한 조건에서 다른 시스템을 유효성 검사하는 데 도움이 됩니다.

사용처:

- 지리적 테스트: 사용자의 지리적 위치를 기준으로 서비스 또는 시스템의 다른 버전을 테스트하는 데 사용됩니다.
- 사용자 세분화: 프리미엄 사용자 대 표준 사용자와 같은 다양한 사용자 세그먼트에 대해 시스템을 유효성 검사하는 데 유용합니다.

코드 샘플:

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

# Node.js 버전: 트래픽 분할 그림자 요청

```js
const express = require('express');
const axios = require('axios');
const crypto = require('crypto');
const port = 3000;

const app = express();
app.use(express.json());

function shouldSendToShadowSystem() {
    const randomValue = crypto.randomInt(1, 101); // 1부터 100까지 랜덤 숫자 생성
    return randomValue <= 20; // 20%의 트래픽을 그림자 시스템으로 보냄
}

async function sendShadowRequest(data) {
    try {
        const response = await axios.post('https://shadow-system.example.com/api', data);
        console.log('그림자 응답:', response.data);
    } catch (error) {
        console.error('그림자 요청 전송 중 오류:', error);
    }
}

async function sendLiveRequest(data) {
    try {
        const response = await axios.post('https://live-system.example.com/api', data);
        return response.data;
    } catch (error) {
        throw error;
    }
}

app.post('/api/process', async (req, res) => {
    const requestData = req.body;
    // 트래픽 분할 로직에 따라 요청을 그림자 시스템으로 보낼지 여부를 결정
    if (shouldSendToShadowSystem()) {
        sendShadowRequest(requestData); // 논블로킹 호출
    }
    try {
        // 라이브 시스템에 라이브 요청 보내기
        const liveResponse = await sendLiveRequest(requestData);
        // 사용자에게는 라이브 시스템의 응답만 반환
        res.json(liveResponse);
    } catch (error) {
        res.status(500).json({ error: '라이브 요청 처리 중 오류' });
    }
});

app.listen(port, () => {
    console.log(`서버가 ${port} 포트에서 실행 중입니다`);
});Explanation (Node.js):
```

- 트래픽 분할 로직:

- 이 Node.js 구현에서 shouldSendToShadowSystem 함수는 간단한 난수 생성기 (crypto.randomInt)를 사용하여 요청을 그림자 시스템으로 보낼 지 여부를 결정합니다. 현재 설정은 난수가 20 이하인 경우에만 20%의 트래픽을 그림자 시스템으로 보냅니다.

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

2. 블로킹되지 않는 섀도우 요청:

- 요청이 섀도잉 대상으로 선택되면 라이브 요청의 처리를 차단하지 않고 섀도 시스템으로 전송됩니다. 라이브 시스템은 사용자 경험이 영향을 받지 않도록 요청을 평상시처럼 처리합니다.

# 사용 사례:

- 트래픽 분할 섀도우 요청은 대부분의 사용자에게 영향을 주지 않으면서 트래픽의 하위 집합으로 새 시스템을 테스트하고 싶을 때 유용합니다. 이 방법을 사용하면 안정적인 라이브 시스템에 대부분의 트래픽을 유지하면서 섀도우 시스템을 점진적으로 도입하고 모니터할 수 있습니다.

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

# Python 버전: 트래픽 분할 그림자 요청

```js
import random
import requests
from flask import Flask, request, jsonify
import threading

app = Flask(__name__)

def should_send_to_shadow_system():
    return random.randint(1, 100) <= 20  # 20%의 트래픽을 그림자 시스템에 보냄

def send_shadow_request(data):
    try:
        response = requests.post("https://shadow-system.example.com/api", json=data)
        print("그림자 응답:", response.json())
    except Exception as e:
        print("그림자 요청 보내기 오류:", str(e))

def send_live_request(data):
    try:
        response = requests.post("https://live-system.example.com/api", json=data)
        return response.json()
    except Exception as e:
        raise e

@app.route('/api/process', methods=['POST'])
def process_request():
    request_data = request.get_json()
    # 트래픽 분할 로직에 따라 요청을 그림자 시스템으로 보낼지 여부 결정
    if should_send_to_shadow_system():
        threading.Thread(target=send_shadow_request, args=(request_data,)).start()
    try:
        # 라이브 시스템으로 라이브 요청 보내기
        live_response = send_live_request(request_data)
        # 사용자에게는 라이브 시스템의 응답만 반환
        return jsonify(live_response)
    except Exception as e:
        return jsonify({'error': '라이브 요청 처리 중 오류 발생'}), 500

if __name__ == "__main__":
    app.run(port=3000)
```

## 설명 (Python):

- 트래픽 분할 로직:

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

- 이 Python 구현에서 should_send_to_shadow_system 함수는 요청을 그림자 시스템으로 보낼지를 결정하기 위해 Python의 random.randint를 사용합니다. 예시에서는 무작위 숫자가 1에서 20 범위 내에 속하는지 확인하여 트래픽의 20%를 분할합니다.

2. 비동기적 그림자 요청:

- 그림자 요청이 선택된 경우, 별도의 스레드 (threading.Thread)를 사용하여 비동기적으로 전송됩니다. 이로써 본 프로세스는 그림자 요청이 완료될 때까지 기다리지 않고 라이브 요청을 계속 처리할 수 있어 부드러운 사용자 경험을 유지합니다.

# 사용 사례:

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

- 트래픽 분할 그림자 요청은 일부 트래픽을 새로운 또는 실험적인 시스템으로 유도하고 싶을 때 이상적입니다. 이 방법을 사용하면 모든 사용자에게 잠재적인 위험이 노출되지 않고 그림자 시스템의 성능에 대한 실제 데이터를 수집할 수 있어서 단계적 롤아웃이나 프로덕션 환경에서의 테스트에 효과적인 전략입니다.

# 주요 차이점 요약:

Node.js:

- 랜덤 번호 생성기(crypto.randomInt)를 사용하여 어떤 요청을 그림자 시스템으로 보낼지 결정합니다. 트래픽 분할 논리는 20%의 트래픽을 그림자 시스템으로 라우팅하도록 설계되어 있어 새 시스템의 통제된 테스트를 가능하게 합니다.
- 라이브 시스템의 성능에 영향을 미치지 않도록 그림자 요청을 비동기적으로 처리합니다.

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

파이썬:

- traffic을 나누기 위해 random.randint를 사용하며, traffic의 20%를 shadow 시스템으로 보냅니다. 결정 과정은 Node.js 버전과 유사합니다.
- shadow 요청은 별도의 스레드에서 실행되어 라이브 요청 처리가 지연되지 않도록 보장됩니다.

두 구현 방식 모두 traffic을 분할하는 shadow 요청을 사용하여 일부 traffic을 shadow 시스템으로 보내어 신규 시스템이나 기능의 점진적인 테스트와 모니터링을 가능하게 합니다. 이 방법을 통해 전체 사용자 베이스에 대한 리스크를 최소화하는 동시에 shadow 시스템의 동작에 대한 현실 세계 데이터 수집이 가능해집니다.

# 결론:

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

그림자 요청의 유연성을 통해 여러 가지 테스트 및 모니터링 시나리오에 맞게 맞춤화할 수 있습니다. 시슽 기술을 이해하고 구현하는 것을 통해, 개발 팀은 그들의 시스템이 견고하고 신뢰할 수 있으며 제품 생산에 준비가 되어 있다는 것을 보장할 수 있습니다. Node.js, Bun, 또는 Python으로 작업 중이든지 상관없이, 이 코드 샘플들은 그림자 요청을 워크플로에 통합하는 방법을 보여주며, 실제 조건 하에서 시스템이 잘 작동하도록 보장하고 라이브 사용자 경험에 영향을 미치지 않도록 합니다.
