---
title: "Google 스프레드시트에서 파일을 분할 비동기 프로세스로 여러 개 업로드하고 이어받기 업로드하기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 01:54
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Uploading Multiple Files with Split Asynchronous Processes and Resumable Upload in Google Spreadsheets"
link: "https://medium.com/google-cloud/uploading-multiple-files-with-split-asynchronous-processes-and-resumable-upload-in-google-d42eebbdfda9"
isUpdated: false
---


# 개요

이 샘플 스크립트는 Google 스프레드시트 내에서 JavaScript 및 HTML을 활용하여 분할 비동기 프로세스를 사용해 여러 파일을 업로드하는 방법을 보여줍니다.

# 설명

이전 보고서인 "Google 드라이브를 위한 여러 파일의 일시 중단 가능한 업로드와 비동기 프로세스"에서 파일을 비동기적으로 업로드하는 방법을 제시했습니다.

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

이 스크립트는 그 개념을 기반으로 하며, 업로드 파일들을 분할 비동기 프로세스를 이용하여 여러 개의 파일을 업로드하는 방법을 소개합니다.

다음은 프로세스의 요약입니다:

- 파일 선택: 사용자가 업로드할 파일을 선택합니다.
- 청킹: 각 선택한 파일은 더 작은 청크로 분할됩니다.
- 각 청크의 비동기 업로드: 각 파일의 각 청크는 각각 비동기적으로 업로드됩니다.
- 동기적인 청크 처리: 청크는 스크립트 내에서 동기적으로 처리됩니다.

이 접근법을 통해 리써머블 업로드 기능을 활용하여 파일을 분할 비동기 프로세스를 통해 업로드할 수 있습니다.

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

# 사용 방법

# 1. 스프레드시트 준비하기

여기서는 샘플로 자바스크립트와 HTML이 Google 스프레드시트에서 실행됩니다. 새 Google 스프레드시트를 만들고 스크립트 편집기를 엽니다.

# 2. 드라이브 API 활성화하기

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

Drive API를 고급 Google 서비스에서 활성화해 주세요. 참조

# 3. 스크립트

다음 Google Apps 스크립트 (code.gs)와 HTML (index.html)를 복사하여 붙여넣어 주세요.

code.gs

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
function getAuth() {
  // DriveApp.createFile(blob) // 이것은 "https://www.googleapis.com/auth/drive" 범위를 추가하는 데 사용됩니다.
  return ScriptApp.getOAuthToken();
}

function showSidebar() {
  var html = HtmlService.createHtmlOutputFromFile("index");
  SpreadsheetApp.getUi().showSidebar(html);
}
```

index.html

```js
<input type="file" id="file" multiple="true" />
<input type="button" onclick="run()" value="업로드" />
<div id="progress"></div>

<script src="https://cdn.jsdelivr.net/gh/tanaikech/ResumableUploadForGoogleDrive_js@2.0.2/resumableupload_js.min.js"></script>
</script>

<script>
function run() {
  google.script.run.withSuccessHandler(accessToken => ResumableUploadForGoogleDrive(accessToken)).getAuth();
}

function upload({ accessToken, file, idx }) {
  return new Promise((resolve, reject) => {
    let fr = new FileReader();
    fr.fileName = file.name;
    fr.fileSize = file.size;
    fr.fileType = file.type;
    fr.readAsArrayBuffer(file);
    fr.onload = e => {
      var id = `p_${idx}`;
      var div = document.createElement("div");
      div.id = id;
      document.getElementById("progress").appendChild(div);
      document.getElementById(id).innerHTML = "초기화 중입니다.";
      const f = e.target;
      const resource = {
        fileName: f.fileName,
        fileSize: f.fileSize,
        fileType: f.fileType,
        fileBuffer: f.result,
        accessToken: accessToken,
        folderId: "root",
      };
      const ru = new ResumableUploadToGoogleDrive();
      ru.Do(resource, function (res, err) {
        if (err) {
          reject(err);
          return;
        }
        console.log(res);
        let msg = "";
        if (res.status == "Uploading") {
          msg = Math.round((res.progressNumber.current / res.progressNumber.end) * 100) + `% (${f.fileName})`;
        } else {
          msg = `${res.status} (${f.fileName})`;
        }
        if (res.status == "Done") {
          resolve(res.result);
        }
        document.getElementById(id).innerText = msg;
      });
    };
  });
}

async function ResumableUploadForGoogleDrive(accessToken) {

  const n = 2; // 청크 크기를 조정할 수 있습니다. 기본값은 2입니다.

  const f = document.getElementById("file");
  const files = [...f.files];
  const splitFiles = [...Array(Math.ceil(files.length / n))].map((_) => files.splice(0, n));
  for (let i = 0; i < splitFiles.length; i++) {
    const res = await Promise.all(splitFiles[i].map(async (file, j) => await upload({ accessToken, file, idx: `${i}_${j}` })));
    console.log(res);
  }
}
</script>
```

- 이 예제에서는 파일이 루트 폴더에 업로드됩니다. 특정 폴더에 파일을 업로드하려면 folderId: "root", 를 folderId: "###당신의 폴더 ID###",로 수정해주세요.
- 이 예제에서는 청크 크기가 2입니다. 파일이 매 2개마다 업로드됩니다. 이를 변경하려면, const n = 2;의 2를 자신의 상황에 맞게 수정해주세요.

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

# 4. 테스트

이 스크립트를 실행하면 다음 결과가 나타납니다.

![image](https://miro.medium.com/v2/resize:fit:920/0*wzkUAstazI9nzwXy.gif)

# 참고문헌

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

- ResumableUploadForGoogleDrive_js
- Google Drive를 위한 파일 멈춤 전송
- Google Drive를 위한 비동기 프로세스로 다중 파일의 멈춤 전송