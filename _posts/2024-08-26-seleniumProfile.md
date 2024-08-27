---
title: "셀레니움(selenium)에서 크롬 브라우저 프로필 설정하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-seleniumProfile_cover.png"
date: 2024-08-27 02:37
ogImage: 
  url: /assets/img/2024-08-26-seleniumProfile_cover.png
tag: Tech
originalTitle: ""
link: ""
isUpdated: true
updatedAt: 1724034988840
---


셀레니움(Selenium)에서 Chrome 브라우저의 프로필을 설정하려면, ChromeDriver에 Chrome 사용자 프로필을 로드하는 방법을 설정해야 합니다. 이를 위해 JavaScript와 Node.js를 사용하는 경우, `selenium-webdriver` 라이브러리와 `chrome` 옵션을 활용할 수 있습니다. 아래는 셀레니움으로 Chrome 프로필을 설정하는 방법을 단계별로 설명합니다.

### 1. 셀레니움 WebDriver 설치

먼저, `selenium-webdriver` 패키지를 설치해야 합니다. Node.js가 설치되어 있다면, 아래 명령어를 실행하여 패키지를 설치합니다.

```bash
npm install selenium-webdriver
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

### 2. ChromeDriver 설치

ChromeDriver를 다운로드하고 설치해야 합니다. [ChromeDriver 다운로드 페이지](https://developer.chrome.com/docs/chromedriver/downloads?hl=ko)에서 운영체제에 맞는 버전을 다운로드하여 적절한 위치에 저장합니다.

![UI](/assets/img/2024-08-26-seleniumProfile_0.png)


![UI](/assets/img/2024-08-26-seleniumProfile_1.png)

m1 mac을 사용하시는 분들은 "chromedriver-mac-arm64" 로 된 파일을 받으시면 됩니다. 

Mac에서 ChromeDriver를 설치하고 사용하는 방법을 안내드리겠습니다. ChromeDriver의 경로를 설정하는 것은 셀레니움(Selenium) WebDriver를 사용할 때 매우 중요합니다. 

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

### 1. ChromeDriver 다운로드

1. [ChromeDriver 다운로드 페이지](https://sites.google.com/chromium.org/driver/)로 이동합니다.
2. 현재 사용하는 Chrome 버전에 맞는 ChromeDriver 버전을 선택합니다. (Chrome의 버전은 `chrome://settings/help`에서 확인할 수 있습니다.)
3. `chromedriver_mac64.zip` 파일을 다운로드합니다.

### 2. ChromeDriver 설치

1. 다운로드한 ZIP 파일을 추출합니다. 터미널을 열고 다음 명령어를 실행하여 추출합니다:

   ```bash
   unzip chromedriver_mac64.zip
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

2. `chromedriver` 파일을 `/usr/local/bin` 또는 `/usr/bin` 디렉토리에 이동시켜 시스템 전체에서 사용할 수 있도록 합니다. 이 디렉토리는 PATH 환경 변수에 포함되어야 합니다.

   ```bash
   sudo mv chromedriver /usr/local/bin/
   ```

   만약 `/usr/local/bin`이 PATH에 포함되어 있지 않다면, PATH에 추가해 줍니다:

   ```bash
   export PATH=$PATH:/usr/local/bin
   ```

   이 명령어는 현재 세션에만 유효하므로, 영구적으로 설정하려면 `~/.bash_profile`, `~/.zshrc`, 또는 사용하는 쉘의 설정 파일에 추가해 주세요.


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

### 경로 확인 및 문제 해결

- **ChromeDriver 경로 확인**: `chromedriver`가 `/usr/local/bin` 또는 `/usr/bin`에 있는지 확인하려면 다음 명령어를 사용하여 경로를 확인할 수 있습니다.

   ```bash
   which chromedriver
   ```

- **버전 호환성**: ChromeDriver와 Chrome 브라우저의 버전이 호환되는지 확인하세요. 버전 불일치가 문제를 일으킬 수 있습니다.

이 단계들을 따라 하면, Mac에서 ChromeDriver를 설치하고 셀레니움과 함께 사용할 수 있을 것입니다.


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

### 3. JavaScript 코드 작성

다음은 셀레니움 WebDriver를 사용하여 Chrome 프로필을 설정하는 기본적인 코드 예제입니다. 이 코드에서는 Chrome 브라우저를 열 때 특정 프로필을 로드합니다.

```javascript
const { Builder } = require('selenium-webdriver');
const chrome = require('selenium-webdriver/chrome');
const path = require('path');

// Chrome 사용자 프로필 경로를 설정합니다.
const profilePath = path.resolve('C:/Users/YourUsername/AppData/Local/Google/Chrome/User Data/Profile 1');

// ChromeOptions 객체를 생성하고 프로필 경로를 설정합니다.
const options = new chrome.Options();
options.addArguments(`user-data-dir=${profilePath}`);
options.addArguments('profile-directory=Profile 1');  // 특정 프로필 폴더를 지정합니다.

// WebDriver 인스턴스를 생성하고 ChromeDriver를 사용하여 브라우저를 실행합니다.
const driver = new Builder()
    .forBrowser('chrome')
    .setChromeOptions(options)
    .build();

// 브라우저에서 특정 웹사이트를 여는 예제입니다.
driver.get('https://www.example.com')
    .then(() => console.log('Webpage opened successfully'))
    .catch(err => console.error('Failed to open webpage:', err));
```

### 코드 설명

1. **`profilePath`**: Chrome 사용자 데이터 디렉토리 경로를 설정합니다. `Profile 1` 부분은 로드할 특정 프로필 폴더를 의미합니다. 운영체제와 사용자 이름에 맞게 경로를 수정하세요.

경로는 chrome://version/ 에 들어가셔서 프로필 경로에 있는 내용을 복사해서 사용하면 됩니다. 

![UI](/assets/img/2024-08-26-seleniumProfile_2.png)



2. **`chrome.Options()`**: Chrome 옵션 객체를 생성하고, `user-data-dir` 인수를 통해 프로필 경로를 설정합니다. `profile-directory` 인수는 사용할 프로필을 지정합니다.

3. **`Builder()`**: WebDriver를 생성하고, Chrome 옵션을 설정하여 Chrome 브라우저를 실행합니다.

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

### 주의 사항

- **경로 확인**: `profilePath`는 실제 Chrome 프로필이 저장된 경로와 일치해야 합니다. 프로필 경로는 운영체제 및 Chrome 버전에 따라 다를 수 있습니다.
  
- **ChromeDriver 버전**: 사용 중인 Chrome 브라우저와 ChromeDriver의 버전이 호환되는지 확인하세요.

이 방법을 통해 셀레니움으로 Chrome 브라우저를 열 때 특정 프로필을 사용할 수 있습니다.