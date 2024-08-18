---
title: "위성 이미지를 통한 그레이트 솔트레이크 축소 추적 Python 사용 방법"
description: ""
coverImage: "/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_0.png"
date: 2024-06-23 16:11
ogImage:
  url: /assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_0.png
tag: Tech
originalTitle: "Tracking The Great Salt Lake’s Shrinkage Using Satellite Images (Python)"
link: "https://medium.com/towards-data-science/tracking-the-great-salt-lakes-shrinkage-using-satellite-images-python-d8b3b04538cf"
isUpdated: true
---

<img src="https://miro.medium.com/v2/resize:fit:1400/1*HOgBDjBrNhopFc2jBrRXVw.gif" />

# 목차

- 🌅 위대한 솔트 레이크 수축 문제 소개
- 💾 Landsat-8 이미지 다운로드
- 📈 통계 파일로부터 위대한 솔트 레이크 지역의 시계열
- ⚙️ Landsat-8 이미지 처리
- 🗺️ 위대한 솔트 레이크 이미지 시각화
- 🎥 위대한 솔트 레이크 수축의 타임 랩스
- 📉 분류된 이미지로부터 위대한 솔트 레이크 지역의 시계열
- ⚖️ 통계 파일 및 이미지에서의 시계열 비교
- 📄 결론
- 📚 참고문헌

## 🌅 위대한 솔트 레이크 수축 문제 소개

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

유타 주의 미국 대소염소인, 위도 메리칼크 지역 치축하고 있어요. 여러 보고서에 의하면, 이 호수는 1986년 최대 규모 대비 30% 이상으로 줄어들었다고 합니다. 이 호수의 수위가 저하되는 이유로 기후 변화와 농업용 수분 분할 등이 언급되었습니다.

기후 변화 요인에 대해 보도된 바에 의하면 강수량 패턴이 변화되고 온도가 상승함에 따라 스노우팩이 감소하고 호수로의 유입량이 줄어들고 있다고 합니다.

두 번째 이유에 대해, 최근 몇 년 동안 도시 및 농업 지역이 확장되어 왔습니다. 도시 및 농업 분야의 수요 증가는 물 수위의 하락에 더해진 요인이 되었습니다.

장기적인 하강은 생태계에 심각한 영향을 미치며, 특히 공기와 물의 질에 영향을 미칩니다. 호법 지역의 노출은 먼지와 염분을 방출함으로써 공기와 물의 질에 대한 위험을 증가시킵니다.

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

상황이 동적이며 감소하는 수위를 추적하는 논의가 계속되고 있기 때문에 저는 이 게시물을 작성하기로 결정했습니다. 우리가 위성 이미지를 사용하여 호수 표면적의 변화를 모니터링하는 방법을 보여주기 위해서입니다. 이것은 축소의 지표로 사용됩니다.

저는 2014년부터 2023년까지 Landsat-8에 의해 촬영된 모든 이미지를 사용하여 구글 Colab에서 Python을 사용하여 그들을 분석했습니다. 지난 어떤 기간 동안 호수 표면적의 시계열을 추출하거나 심지어 이 접근 방법을 다른 호수에 적용하는 것에 관심이 있다면, 이 블로그 게시물은 여러분을 위한 것입니다.

## 💾 Landsat-8 이미지 다운로드

Landsat-8 이미지를 다운로드하기 위해 AρρEEARS라는 웹 앱을 사용했습니다. 이 웹 앱을 통해 관심 영역(AOI)에 대한 위성 이미지를 다운로드할 수 있습니다. 이미지는 자르고 메타데이터 및 통계 파일로 제공됩니다. AOI에 대한 폴리곤을 그리고 제품을 선택하고 시작 및 종료 날짜를 선택하기만 하면 됩니다. 이미 이 웹 앱에 관한 게시물을 작성해 놓았습니다. 여기서 확인해보세요:

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

이 게시물에서는 Great Salt Lake 주변에 폴리곤을 그리고 Landsat-8 ARD 제품을 선택하여 시작 시간과 종료 시간을 2014년부터 2023년까지 여름(6월, 7월 및 8월) 기간으로 설정했습니다. 여름 동안에만 촬영된 이미지를 고려한 결정은 두 가지 이유로부터 나왔어요: (1) 여름 동안 보다 맑은 이미지를 더 유추할 수 있는 가능성이 더 높고, (2) 이는 계산 시간을 제한하기 때문입니다. 이 여름 동안의 모든 연도에 대한 Landsat-8 이미지를 다운로드하므로 결과(호수의 추정 표면적)는 서로 다른 연도 간에 비교할 수 있어요.

데이터 정책: USGS 웹사이트에 따르면, "USGS에서 다운로드한 Landsat 데이터에는 제한이 없으며, 원하는 대로 사용하거나 재배포할 수 있습니다" (링크) 및 "LP DAAC로부터 획득한 LP DAAC의 모든 현재 데이터와 제품은 재사용, 판매 또는 재배포에 대한 제한이 없습니다" (링크).

## 📈 통계 파일에서 Great Salt Lake 지역의 시계열

AρρEEARS에서 요청을 제출한 후, 호수의 표면적을 추정할 두 가지 옵션이 있습니다. 옵션 1은 통계 CSV 파일에 보고된 미리 계산된 물 픽셀 수를 사용하는 것이며, 옵션 2는 분류된 이미지를 처리하여 물 픽셀을 계산하는 것인데, 이는 추후 설명하겠습니다. 옵션 1을 따르려면 AρρEEARS에서 "L08-002-QA-PIXEL-CU-Statistics-QA.csv" 및 "L08-002-QA-PIXEL-CU-lookup.csv" 두 파일을 다운로드하세요.

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

각 파일을 빠르게 살펴보겠습니다:

![이미지1](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_0.png)

![이미지2](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_1.png)

첫 번째 파일인 "L08-002-QA-PIXEL-CU-Statistics-QA.csv"에서는 각 이미지(행)에 대해 각 클래스(열)의 픽셀 수가 보고됩니다. 이러한 열 이름을 해독하려면 두 번째 파일인 "L08-002-QA-PIXEL-CU-lookup.csv"이 필요합니다. 'Water'로 레이블된 열을 필터링하면 물 픽셀에 대한 해당 클래스 이름(열)을 찾을 수 있습니다:

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

![Great Salt Lake](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_2.png)

수집된 코드들 중 물 클래스로 고려된 코드는 21890, 21952, 22146, 22208, 54662, 54724, 54918, 54980입니다. 따라서 "L08–002-QA-PIXEL-CU-Statistics-QA.csv" 파일에서 이 열들만 선택하고 픽셀 수를 합산하면 지난 10년간의 Great Salt Lake 지역의 시계열을 시각화할 수 있습니다. 이를 위한 Python 스크립트를 작성해 봅시다:

```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as mdates

csv_file_path = 'L08-002-QA-PIXEL-CU-Statistics-QA.csv'

# 물 클래스를 위한 열 이름
selected_columns = ['21890', '21952', '22146','22208','54662','54724','54918','54980']

# CSV 파일을 pandas DataFrame으로 읽기
df = pd.read_csv(csv_file_path)

# 'date' 열을 날짜 형식으로 변환
df['date'] = pd.to_datetime(df['Date'])

# 물 열 선택
selected_data = df[['Date'] + selected_columns]

# 각 행의 합산 계산
selected_data['sum'] = selected_data[selected_columns].sum(axis=1)

# 플롯
plt.figure(figsize=(10, 6))
plt.plot(selected_data['Date'], selected_data['sum'], marker='o', linestyle='-', color='b')

plt.title('시간에 따른 물 픽셀의 합산')
plt.ylabel('합계')
plt.grid(True)

locator = mdates.MonthLocator()
plt.gca().xaxis.set_major_locator(locator)

plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

출력 결과는:

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

![Tracking The Great Salt Lake's Shrinkage Using Satellite Images Python](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_3.png)

만약 AOI에서 각 이미지로 분류된 픽셀의 물의 수를 그래프로 그린다면, 각 Landsat 이미지가 전체 호수를 커버할 수 있는 것을 보장할 수 없습니다. 게다가 우리는 흐린 픽셀들을 걸러냅니다. 다시 말해, Landsat 이미지가 전체 호수를 커버하더라도, 만약 호수 위에 구름이 있다면, 깨끗한 픽셀의 물로 분류되는 숫자는 전체 호수의 표면을 과소 평가할 수 있습니다. 이 두 문제의 예시로 각 이미지를 고려해 보겠습니다.

![Example Image 1](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_4.png)

여기서 보듯이, 왼쪽 이미지(2015년 7월 1일)는 전체 호수를 커버하지 않고 일부가 잘린 상태입니다. 이 이미지를 기반으로 호수의 표면적을 계산하는 것은 오해를 불러일으킬 수 있으며, 시계열에서 상당한 하강을 보여줄 수 있습니다. 오른쪽 이미지(2015년 6월 22일)에 대해선, 전체 호수를 커버하고 있지만, 물 픽셀의 수가 표면적을 계산하기 위한 정확한 정보를 제공할 수 없을 수 있습니다. 보여진 것처럼, 흐린 픽셀이 걸러져 나가면서 호수의 표면적이 과소 평가되는 문제가 발생합니다.

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

또한, 시계열 그래프가 각 이미지의 총 물 픽셀을 표시하여 전반적인 감소 추세를 관측하기 어렵게 만듭니다.

이러한 문제를 해결하려면 코드를 약간 수정해야 합니다. 먼저, 각 이미지의 물 픽셀 수를 그리는 대신 데이터를 집계하고 각 연도별로 보고된 최대 물 픽셀 수를 얻어야 합니다. 둘째, 다음 변환을 사용하여 제곱 킬로미터 (sqKm)로 호수 면적을 그릴 것입니다:

호수 면적 (Km²) = (랜드셧 픽셀 크기) x (물 픽셀 수) / 10⁶

- 랜드셧 픽셀 크기 = 30m x 30m

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
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
from scipy.stats import linregress


csv_file_path = 'L08-002-QA-PIXEL-CU-Statistics-QA.csv'

# 물 픽셀을 위한 열 이름
selected_columns = ['21890', '21952', '22146', '22208', '54662', '54724', '54918', '54980']

# CSV 파일을 pandas DataFrame으로 읽기
df = pd.read_csv(csv_file_path)

# 'date' 열을 datetime 형식으로 변환
df['date'] = pd.to_datetime(df['Date'])

# 'date' 열에서 연도 추출
df['year'] = df['date'].dt.year

# 열 선택
selected_data = df[['year'] + selected_columns]

# 각 행의 합 계산
selected_data['sum'] = selected_data[selected_columns].sum(axis=1)

# 각 연도의 최대 값을 계산
max_values_per_year = selected_data[['year','sum']].groupby('year').max()

# 면적으로 변환 (평방 킬로미터)
max_values_per_year['area'] = max_values_per_year['sum'] * 30 * 30 / 1000000

# 플로팅
plt.figure(figsize=(12, 6))
plt.bar(max_values_per_year.index, max_values_per_year['area'], color='lightblue', label='연도별 최대 면적')
plt.plot(max_values_per_year.index, max_values_per_year['area'], marker='o', linestyle='-', color='b', label='추세선')

# 추세선을 위한 선형 회귀
slope, intercept, _, _, _ = linregress(max_values_per_year.index, max_values_per_year['area'])
trend_line = slope * max_values_per_year.index + intercept

# 추세선 방정식 표시
plt.plot(max_values_per_year.index, trend_line, color='red', linestyle='--', label=f'추세선: y = {slope:.4f}x + {intercept:.4f}')

plt.xticks(fontweight='bold')
plt.yticks(fontweight='bold')

plt.title('연도별 대소염소 지역 최대 면적')
plt.ylabel('최대 면적 (평방 킬로미터)', fontsize=12, fontweight='bold')
plt.grid(True)
plt.legend(loc='center')
plt.show()
```

위의 플롯을 참고해주세요.

우리가 데이터를 집계하는 코드 수정이 도움이 된 것으로 보입니다. 그러나 결과는 아직 정확하지 않을 수 있습니다. 왜냐하면 우리는 각 연도에 촬영된 모든 이미지에서 대소염소 호수의 최대 면적을 추출하기 때문입니다. 예를 들어, 2015년에 가장 명확한 이미지가 15%의 구름을 가지고 있다고 가정해 봅시다. 구름이 걸러지면 호수의 표면적을 15% 과소 평가하게 됩니다.

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

이 문제를 해결하기 위해서는, 호수 위에서 찍힌 원본 분류된 이미지들과 작업해야 합니다. 구체적으로는, 각 이미지마다 수집된 픽셀 중 물로 분류된 것을 오버레이하여 다운로드해야 합니다. 이 과정을 거치면 각 연도별로 가장 정확한 구름 없는 호수 이미지를 작성할 수 있으며 표면적을 정확하게 추출할 수 있게 될 것입니다. 이 방법에 대해 다음 섹션에서 다룰 예정입니다.

## ⚙️ Landsat-8 이미지 처리

이번과 다음 섹션에서는 통계 파일이 아닌 실제 Landsat-8 분류된 이미지를 다루게 됩니다.

AρρEEARS에서 또는 API를 통해 L08.002_QA_PIXEL_CU_doy`YYYYMMDD`\_aid0001와 같이 명명된 파일들을 수동으로 다운로드할 수 있습니다. API를 사용하려는 경우 검색 필드에 'QA_Pixel'을 입력하여 파일을 필터링한 후 AρρEEARS에서 모든 파일을 선택하고 '다운로드'를 클릭하세요. 그런 다음 아래 코드를 실행하여 API를 통해 파일을 다운로드할 수 있습니다:

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

단계 1: 폴더 만들기

```python
import os

# 폴더 이름 목록
folders = ['QA', 'Clear_Image', 'Map', 'Animation']

base_path = '/content'

# 폴더 생성
for folder in folders:
    folder_path = os.path.join(base_path, folder)
    os.makedirs(folder_path, exist_ok=True)
    print(f"'{folder}' 폴더가 '{folder_path}'에 생성되었습니다.")
```

단계 2: 토큰 설정

```python
import os
import requests

response = requests.post('https://appeears.earthdatacloud.nasa.gov/api/login', auth=('여러분의 사용자명', '여러분의 비밀번호'))
token_response = response.json()
token = token_response['token']
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

3단계: 텍스트 파일에 저장된 URL을 읽고 파일을 다운로드합니다:

```js
txtfile = 'Great-Salt-Lake-download-list.txt'
filenames = [] # 파일 이름을 저장하는 리스트
urls = [] # 파일 이름이 없는 URL을 저장하는 리스트

with open(txtfile) as file:
    for line in file:
        url = line.strip() # 줄 바꿈 문자 제거
        filename_index = url.find('L08.002') # 파일 이름의 인덱스 가져오기
        url_without_filename = url[:filename_index-1] # 파일 이름이 없는 URL 가져오기
        print(url_without_filename)
        filename = url[filename_index:] # 파일 이름 가져오기
        print(filename)

        response = requests.get(url_without_filename,
                                headers={'Authorization': 'Bearer {0}'.format(token)},
                                allow_redirects=True,
                                stream=True)
        dest_dir = "/content/QA"
        filepath = os.path.join(dest_dir, filename)
        os.makedirs(os.path.dirname(filepath), exist_ok=True)

        # 파일을 목적지 디렉토리에 쓰기
        with open(filepath, 'wb') as f:
          for data in response.iter_content(chunk_size=8192):
            f.write(data)
```

이 코드는 폴더를 생성하고 토큰을 설정하며 QA 폴더에 AppEARS 서버에서 분류된 Landsat-8 이미지를 다운로드합니다.

첫 번째 단계에서는 QA, Clear_Image, Map 및 Animation 네 가지 폴더를 만들었습니다. "QA" 폴더는 원본 분류된 이미지를 저장하기 위한 것이고, "Clear_Image" 폴더는 각 연도별로 물 위 구름이 없는 Great Salt Lake 이미지를 저장하는 용도이고, "Map"은 물 위 구름이 없는 이미지를 바탕으로한 Great Salt Lake 맵을 저장하는 곳이며, "Animation" 폴더는 타임랩스 파일을 저장하는 용도입니다.

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

위 단계를 올바르게 따르면 디렉토리에 "QA_Pixel" 파일이 있어야 합니다:

![이미지](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_6.png)

이제 디렉토리에 이미지가 분류되었으므로 이러한 이미지를 오버레이하고 각 연도별로 물로 분류된 픽셀을 유지하고 이러한 래스터 레이어를 Geotiff 파일에 저장해야 합니다. 이 작업은 다음과 같이 수행할 수 있습니다:

```js
import os
import numpy as np
import rasterio
from datetime import datetime
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
import pandas as pd

# 유지할 QA 값 정의
qa_values = [21890, 21952, 22146, 22208, 54662, 54724, 54918, 54980]

# 폴더 내의 모든 파일 나열
qa_folder = '/content/QA'
all_files = os.listdir('/content/QA')

# 연도별로 루프 실행
for year in range(2014, 2024):
    print(year)

    # 물 픽셀을 저장할 빈 배열 초기화
    water_pixels = None

    # 조건에 따라 파일 필터링
    QA_list = [file for file in all_files if str(year) in file]
    for QA_filename in QA_list:
        if QA_filename.endswith('.tif'):
            date_str = QA_filename.split('_')[4][3:]
            date_obj = datetime.strptime(date_str, '%Y%j')
            date = date_obj.date()
            print('Processing date:', date)

            # QA tif 파일 읽기
            qa_file = os.path.join(qa_folder, QA_filename)

            with rasterio.open(qa_file) as qa_src:
                # tif 파일에서 데이터 및 메타데이터 읽기
                qa_data = qa_src.read(1)

                # 모든 값이 nan인 경우 건너뛰기
                qa_data_zeros = np.all(qa_data == 1)

                if qa_data_zeros:
                    continue

                profile = qa_src.profile

                # 물값을 2로 대체하고 나머지는 NaN으로 변경
                qa_data = qa_data.astype(float)
                qa_data[np.isin(qa_data, qa_values)] = 2
                qa_data[~np.isin(qa_data, [2])] = np.nan

                # 물 픽셀을 배열에 추가
                if water_pixels is None:
                    water_pixels = qa_data.copy()
                else:
                    water_pixels = np.nanmax(np.stack([water_pixels, qa_data]), axis=0)

                # 물 픽셀을 갖는 새 래스터 생성
                profile.update(count=1)
                with rasterio.open(f'/content/Clear_Image/Great_Salt_Lake_{year}.tif', 'w', **profile) as dst:
                    dst.write(water_pixels.reshape(profile['height'], profile['width']), 1)
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

스크립트는 물 픽셀을 나타내는 QA 값을 정의한 후 지정된 QA 폴더의 파일을 나열합니다. 다음으로, 스크립트는 각 연도별로 물 픽셀을 저장할 배열을 초기화하고 2014년부터 2023년까지의 연도를 반복합니다. 각 연도의 QA 파일에 대해, 파일을 읽고 물 값을 임의의 숫자로 바꾼 다음 나머지 값을 NaN으로 설정합니다. 그런 다음 물 픽셀의 배열을 업데이트하고 물 픽셀이 있는 새 래스터를 생성하여 'Clear_Image' 폴더에 Geotiff 파일로 씁니다.

다음 애니메이션은 스크립트가 Great Salt Lake에서 촬영한 Landsat-8 이미지의 물 픽셀을 유지하여 2021년의 구름 없는 이미지를 생성하는 방법을 보여줍니다.

![애니메이션](https://miro.medium.com/v2/resize:fit:1400/1*DM_1YNYFpMGwXhNRYHC5yA.gif)

코드를 실행한 후에는 각 연도에 대해 Great Salt Lake의 완전하고 구름 없는 이미지가 디렉토리에 저장됩니다.

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

![Great Salt Lake Images](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_7.png)

## 🗺️ Visualization of The Great Salt Lake Images

We will first visualize the raster file to examine the cloud-free images and see how the lake's surface area changes from 2014 to 2023 before calculating its total area.

```python
import os
import matplotlib.pyplot as plt
import rasterio

def plot_raster_files(folder_path):
    # Get a list of all raster files in the folder
    raster_files = [f for f in os.listdir(folder_path) if f.endswith('.tif')]
    raster_files.sort()

    # Set up subplots
    rows, cols = 2, 5
    fig, axes = plt.subplots(rows, cols, figsize=(15, 8))

    # Loop through raster files and plot them in subplots
    for i, file_name in enumerate(raster_files):
        date_str = file_name.split('_')[3][0:5]
        file_path = os.path.join(folder_path, file_name)
        print(file_path)

        # Open the raster file
        with rasterio.open(file_path) as src:
            # Read raster data
            raster_data = src.read(1)

            # Plotting
            ax = axes[i // cols, i % cols]
            cmap = ListedColormap(['white', 'blue'])
            ax.imshow(raster_data, cmap=cmap, vmin=0, vmax=2)
            ax.set_title(date_str)
            ax.set_xticks([])
            ax.set_yticks([])

    # Adjust layout for better visualization
    plt.tight_layout()
    plt.show()

# Specify the folder path containing GeoTIFF images
Geotiff_folder = '/content/Clear_Image'
plot_raster_files(Geotiff_folder)
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

결과는 다음과 같습니다:

<img src="/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_8.png" />

먼저, 이미지가 전체 호수를 포함하고 구름이 없는 것을 볼 수 있습니다. 이는 우리가 통계 파일을 기반으로 호수 영역의 시계열을 그리는 데 논의한 문제를 해결했습니다. 두 번째로, 최대 면적이 2016 또는 2017에 발생했으며, 최소 면적은 2022에 발생했음을 알 수 있습니다.

## 🎥 Great Salt Lake 수축 타임랩스

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

Great Salt Lake의 표면적 변화를 동적으로 시각화하기 위해, 우리는 호수의 개별 지도를 내보내고 이를 연속으로 배치하여 2014년부터 2023년까지의 타임랩스를 만들 수 있습니다. 이를 수행하는 방법은 다음과 같습니다:

단계 1: 구름이 없는 GeoTIFF 파일에서 개별 지도를 내보냅니다.

```python
# 폴더 내의 모든 파일 나열
Geotiff_folder = '/content/Clear_Image'
all_files = os.listdir(Geotiff_folder)

# 파일 순회
for Lake_filename in all_files:
    if Lake_filename.endswith('.tif'):
      date_str = Lake_filename.split('_')[3][0:4]

      # 래스터 파일 열기
      with rasterio.open(os.path.join(Geotiff_folder,Lake_filename)) as src:
        # 래스터 데이터 읽기
        raster_data = src.read(1)
        cmap = ListedColormap(['white','blue'])
        plt.imshow(raster_data,cmap=cmap, interpolation='nearest', vmin=0, vmax=2)
        plt.title(f'Great Salt Lake {date_str}')
        # 축 제거
        plt.axis('off')
        # 그림을 파일로 저장
        plt.savefig(f'/content/Map/{date_str}.png')
```

단계 2: 이러한 지도를 병합하여 타임랩스를 작성합니다.

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
from PIL import Image
import os
import glob

folder_path = '/content/Map'
output_gif = os.path.join('/content/Animation', 'Lake_animation.gif')

# 폴더 내 모든 jpg 파일의 정렬된 목록을 가져옵니다
file_list = sorted(glob.glob(os.path.join(folder_path, '*.png'))

# 이미지의 원하는 차원을 설정합니다
width, height = 700, 500

# 이미지를 읽고 리스트에 저장합니다
frames = []
for file in file_list:
    frame = Image.open(file)

    # 이미지 크기 조정
    frame = frame.resize((width, height), Image.ANTIALIAS)

    # 이미지에 흰색 배경 추가
    background = Image.new('RGB', frame.size, (255, 255, 255))
    background.paste(frame)
    frame = background.convert('RGB')

    # 전역 색상 표를 사용하여 이미지를 P 모드로 변환
    frame = frame.convert('P', palette=Image.ADAPTIVE, colors=256)
    frames.append(frame)


frames[0].save(output_gif, save_all=True, append_images=frames[1:],
        duration=500,  # 밀리초로 지속 시간 설정
        loop=0,        # 루프 수 설정 (0은 무한을 의미)
        optimize=True)
```

이외에도 다양한 기능을 사용하여 이미지를 수정 및 GIF 애니메이션을 만들 수 있습니다. 위 코드에서 설정한 이미지 처리를 참고해 보세요.

시간 흐름은:

<img src="https://miro.medium.com/v2/resize:fit:1400/1*HOgBDjBrNhopFc2jBrRXVw.gif" />

## 📉 분류된 이미지로부터 그레이트 솔트 레이크 지역의 시계열 데이터입니다

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

선택지 1인 통계 파일을 기반으로 호수 표면적의 시계열 데이터를 추출한 섹션과 유사하게, 이제 흐릿하지 않은 Landsat-8 이미지에서 표면적을 추정할 수 있습니다. 다음 스크립트에서는 흐릿하지 않은 GeoTIFF 파일을 읽어서 물 픽셀을 세고, 제곱 킬로미터로 표면적을 변환하고, 이를 시계열로 나타내어 추세를 분석합니다.

```js
import os
import re
import rasterio
import matplotlib.pyplot as plt
from scipy.stats import linregress

folder_path = "/content/Clear_Image"
value_to_count = 2

files = [f for f in os.listdir(folder_path) if f.endswith('.tif')]
files.sort()

data = {'dates': [], 'pixel_counts': []}

for file in files:
    with rasterio.open(os.path.join(folder_path, file)) as src:
        array = src.read(1)
        pixel_count = (array == value_to_count).sum()
        date = file.split('_')[3][0:4]
        data['dates'].append(date)
        data['pixel_counts'].append(pixel_count)

# 데이터프레임 생성
df = pd.DataFrame(data)

# 면적으로 변환 (제곱 킬로미터)
df['area'] = df['pixel_counts'] * 30 * 30 / 1000000

# 그래픽 표현
plt.figure(figsize=(12, 6))
plt.bar(df['dates'], df['area'], color='lightblue', label='Max Area Per Year')
plt.plot(df['dates'], df['area'], marker='o', linestyle='-', color='b', label='Trend Line')

# 추세선을 위한 선형 회귀
slope, intercept, _, _, _ = linregress(df.index, df['area'])
trend_line = slope * df.index + intercept

# 추세 방정식 표시
plt.plot(df.index, trend_line, color='red', linestyle='--', label=f'Trend Line: y = {slope:.1f}x + {intercept:.1f}')

plt.xticks(fontweight='bold')
plt.yticks(fontweight='bold')

plt.title('Maximum Area of The Great Salt Lake Per Year')
plt.ylabel('Max Area (SqKm)', fontsize=12, fontweight='bold')
plt.grid(True)
plt.legend(loc='center')
plt.show()
```

위 코드를 실행하면 아래와 같은 플롯이 생성됩니다:

<img src="/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_9.png" />

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

위의 도표와 통계 파일을 기반으로 한 플롯 간의 주요 차이점은 이 플롯이 우리가 원본 Landsat-8 이미지에서 추정할 수 있는 호수의 가장 정확한 표면적을 나타낸다는 것입니다. 이 플롯은 2014년부터 2016년까지는 상당한 변화가 없고, 2017년과 2019년에는 작은 확장(약 4800 제곱킬로미터에서 약 5300 제곱킬로미터)을 보여주지만, 2019년부터 2022년에는 상당한 감소(약 5300 제곱킬로미터에서 약 4100 제곱킬로미터)가 있었습니다. 2022년부터 2023년에는 다시 그래프 상에서 증가함(약 4100 제곱킬로미터에서 약 4700 제곱킬로미터)이 관찰되었는데, 이는 작년에 받은 상당한 우천량으로 인해 젖은 해가 된 것으로 보입니다.

## ⚖️ 통계 파일 및 이미지로 추출된 시계열 비교

이전에 언급했듯이 QA 통계 파일을 기반으로 표면적을 분석하면 호수 위의 구름이 있는 픽셀에 대한 필터를 보상하지 않기 때문에 해당 지역을 과소평가하는 경향이 있습니다. 통계 파일을 통해 계산된 표면적과 분류된 Landsat-8 이미지에서 추정한 표면적을 비교해 보기 위해 두 가지를 동일한 레이아웃에 플롯하여 통계 파일만을 의존했을 때 얼마나 과소평가가 발생하는지 살펴봅시다:

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

이 스크립트는 각 접근 방법에서 보고된 물 픽셀의 수를 제곱 킬로미터 단위의 표면적으로 변환하고 이를 시각화합니다. 결과 그래프는 다음과 같습니다:

![그래프](/assets/img/2024-06-23-TrackingTheGreatSaltLakesShrinkageUsingSatelliteImagesPython_10.png)

해당 그래프는 각 접근 방법에서 계산된 표면적을 보여줍니다: 주황색 선과 빨간 막대는 통계 파일을 나타내며(옵션 1), 파란색 선과 막대는 구름이 없는 이미지를 나타냅니다(옵션 2). 그림에서 볼 수 있듯이, 통계 파일을 사용하면 호수 표면적의 증가 또는 감소의 일반적인 패턴을 추적할 수 있습니다. 그러나 앞서 언급한 대로, 통계 파일은 2015년에는 1000 제곱 킬로미터에서 2020년에는 150 제곱 킬로미터로 이르는 과소평가로 이어집니다. 2020년에 통계와 구름이 없는 이미지에서 계산된 표면적 간의 미세한 차이는 2020년에 캡처된 랜드섯 이미지 중 하나가 거의 구름이 없고 호수 대부분을 포함했기 때문에, 통계 파일과 구름이 없는 추정치 사이의 픽셀 수가 비슷하다는 점을 설명합니다. 반면, 2015년의 큰 차이는 2015년 랜드섯-8이 캡처한 최상의 맑은 이미지가 여전히 다양한 구름이 있는 픽셀을 가지고 있거나 전체 호수를 커버하지 않았기 때문입니다.

## 📄 결론

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

위성 이미지의 응용 중 하나는 물체나 풍경의 변화를 모니터링하는 것입니다. 이 연구에서는 Landsat-8에 의해 캡처된 이미지를 사용하여 2014년부터 2023년까지 대연몰의 표면 면적 변동을 분석했습니다. 이 지역은 지난 수십 년 동안 상당한 수축이 있어 지켜지고 있습니다. 두 가지 다른 방법으로 측정된 호수의 면적을 비교하여 우리의 분석에서 작은 오류가 어떻게 오해와 부정확한 정보로 이어질 수 있는지 강조해 보았습니다. 이 분석 결과, 2019년부터 2022년까지 대연몰의 상당한 수축, 2014년부터 2022년까지 10% 수축이 있었습니다. 흥미로운 점은 2023년의 다습한 해가 대연몰의 회복에 중요한 역할을 한 것으로 나타났습니다. 그 해에 10% 증가했습니다. 그러나 2023년을 넘어서의 회복은 불확실하며, 2024년에 추가 개선이 보장되지는 않습니다.

## 📚 참고 자료

2014년부터 2023년까지의 Landsat-8 이미지는 2024/02/20에 https://lpdaac.usgs.gov에서 AppEEARS를 사용하여 검색되었습니다. 이는 미국 지질조사원(USGS) 지구 자원 관측 및 과학(EROS) 센터가 운영하는 NASA EOSDIS Land Processes Distributed Active Archive Center (LP DAAC)에서 유지되었습니다.

📱 더 많은 유익한 콘텐츠를 위해 다른 플랫폼에서 저와 연결하세요! LinkedIn, ResearchGate, Github, Twitter.

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

이 링크를 통해 해당 게시물을 확인할 수 있습니다:
