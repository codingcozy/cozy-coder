---
title: " Docker로 Apache Spark와 Minio를 사용하여 Apache Iceberg 활용하는 방법"
description: ""
coverImage: "/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_0.png"
date: 2024-06-22 17:09
ogImage:
  url: /assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_0.png
tag: Tech
originalTitle: "🖥️Using Apache Iceberg with Apache Spark and Minio — Docker"
link: "https://medium.com/towardsdev/%EF%B8%8Fusing-apache-iceberg-with-apache-spark-and-minio-docker-dee475b55a8f"
isUpdated: true
---

<img src="/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_0.png" />

이 게시물에서는 Apache Iceberg(데이터 테이블 형식), 분산 처리 엔진인 Apache Spark, 고성능 객체 저장 솔루션인 Minio를 결합하는 방법에 대해 탐구합니다. 주요 초점은 이러한 구성 요소를 Docker 컨테이너 내에 설정하여 통제된 환경에서 격리된 환경을 제공하는 데 있습니다. 이러한 기술을 결합하여 ACID 트랜잭션, 스키마 진화 및 Minio 내에서 효율적인 데이터 파티셔닝을 통한 효율적인 데이터 관리 기능을 확보할 수 있습니다.

# 아키텍처 개요

<img src="https://miro.medium.com/v2/resize:fit:1400/1*gM-qHwR03S6IEh32mgJpjA.gif" />

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

# 컴포넌트 개요 이론적 개요📖

실제 구현에 들어가기 전에, 사용할 기술들인 Apache Iceberg, Apache Spark, 그리고 Minio에 대한 간단한 이론적 개요를 살펴보겠습니다:

![이미지](/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_1.png)

- Apache Iceberg: 차세대 데이터 테이블 형식

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

- 데이터 레이크 및 데이터 웨어하우스용으로 설계됨: Iceberg는 대규모 데이터의 복잡성을 처리하도록 특별히 구축되었습니다. 데이터 레이크와 데이터 웨어하우스 내에서 강력한 데이터 관리 기능을 제공하면서 분석 워크로드의 성능을 최적화합니다.
- ACID 트랜잭션: Iceberg는 ACID(원자성, 일관성, 고립성, 지속성) 트랜잭션을 지원하여 동시 쓰기 시나리오에서도 데이터 무결성과 일관성을 보장합니다. 이는 동일한 데이터에 여러 애플리케이션이나 프로세스가 동시에 쓰기를 하는 경우에 데이터 유효성을 유지하고 데이터 손상을 방지하는 데 중요합니다.
- 스키마 진화: Iceberg는 기존 데이터 형식과 달리 데이터 테이블의 스키마를 손실 없이 진화시킬 수 있습니다. 이를 통해 분석 요구 사항이나 데이터 소스가 시간이 지남에 따라 변경될 때 데이터 구조를 조정할 수 있습니다. 기존 데이터를 다시 작성하지 않고도 열을 추가, 제거 또는 수정할 수 있습니다.
- 타임 트래블 쿼리: Iceberg는 타임 트래블 쿼리를 지원하여 언제든지 과거 데이터 스냅샷에 액세스할 수 있습니다. 이는 변경 사항 감사, 분석 파이프라인 디버깅 및 역사적 분석에 매우 유용합니다. Iceberg는 데이터 버전을 추적하여 필요한 경우 특정 버전의 테이블을 검색할 수 있습니다.
- 효율적인 파티셔닝: Iceberg는 특정 열을 기준으로 데이터 테이블을 파티션하여 읽기 성능과 데이터 관리를 최적화합니다. 해당 파티션 값에 따라 데이터 파일을 지능적으로 저장하므로 Spark가 특정 쿼리에 대한 관련 데이터만 효율적으로 스캔할 수 있습니다. 이는 쿼리 속도를 크게 향상시킵니다.
- 데이터 조직 및 압축: Iceberg는 자동으로 Parquet 또는 ORC와 같은 효율적인 파일 형식으로 데이터를 구성하여 효율적인 데이터 압축과 열 액세스를 용이하게 합니다. 또한 데이터 압축을 수행하여 저장 공간을 최소화하고 시간이 지남에 따라 읽기 성능을 향상시킵니다.

2. Apache Spark: 통합 분석 엔진

![이미지](/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_2.png)

- 대규모 데이터 처리: Apache Spark는 대규모 데이터셋을 클러스터로 효과적으로 처리하는 강력한 오픈 소스 분산 처리 엔진입니다. 관계형 데이터베이스, NoSQL 데이터베이스, CSV 파일 및 Minio와 같은 객체 저장소를 포함한 다양한 데이터 원본을 지원합니다.
- 인메모리 처리: Spark는 인메모리 계산을 활용하여 성능을 향상시키며, 자주 액세스되는 데이터를 디스크 기반 솔루션에 비해 더 빠른 처리를 위해 메모리에 유지합니다.
- 구조적, 반구조적 및 비구조적 데이터: Spark는 CSV, JSON과 같은 구조적 데이터, XML과 같은 반구조적 데이터, 텍스트와 같은 비구조적 데이터를 포함한 다양한 데이터 형식을 처리할 수 있습니다. 이러한 유연성으로 인해 현대 데이터 생태계에서 다양한 데이터 유형을 다루는 데 이상적입니다.
- 머신러닝 및 스트림 처리: 전통적인 분석 이상으로 Spark는 머신러닝 파이프라인 및 실시간 데이터 처리까지 확장됩니다. Spark는 TensorFlow, PyTorch와 같은 인기있는 머신러닝 라이브러리를 효율적인 모델 훈련 및 배포를 위해 통합합니다. 또한 Apache Flink와 같은 스트림 처리 프레임워크를 지원하여 거의 실시간 데이터 분석을 수행할 수 있습니다.
- Iceberg와의 원활한 통합: Spark는 네이티브 Iceberg 지원을 제공하여 Spark 애플리케이션에서 Iceberg 테이블을 직접 읽고 쓰기 및 쿼리할 수 있습니다. 이를 통해 Spark 워크플로우 내에서 데이터 관리를 간편하게 할 수 있습니다.

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

3. Minio: 고성능 객체 저장 서버

![Minio Image](/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_3.png)

- 오픈소스 및 비용 효율적: Minio는 무료 및 오픈소스 객체 저장 솔루션으로, 대형 클라우드 공급업체가 제공하는 프로프리어터리 객체 저장 서비스에 대안으로 경제적입니다. 자체 데이터 저장 인프라를 관리하고 데이터 레이크 또는 데이터 웨어하우스 내에서 해당 기능을 활용할 수 있습니다.
- 확장성 및 성능: Minio는 확장성을 고려하여 제작되었습니다. 데이터 저장 필요가 증가함에 따라 더 많은 노드를 Minio 클러스터에 쉽게 추가할 수 있어 증가하는 데이터 양을 처리할 수 있습니다. 또한 효율적인 데이터 액세스를 제공하여 Spark 애플리케이션에 대한 효율적인 데이터 액세스를 제공합니다.
- S3 호환성: Minio는 Amazon S3와 API 호환성이 있어서 S3와 작동하는 기존 도구 및 애플리케이션과의 원활한 통합이 가능합니다. 이는 전통적인 클라우드 객체 저장에서 더 경제적인 온프레미스 솔루션으로의 원활한 전환을 용이하게 합니다.
- 내구성 및 신뢰성: Minio는 데이터 중복 및 복제 메커니즘을 제공하여 데이터가 장애에 대비하여 보호되도록 합니다. 다중 노드 또는 저장 장치 간의 데이터 복제를 구성하여 하드웨어 문제 발생 시 데이터 손실 위험을 최소화할 수 있습니다.

4. Docker 통합

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

<img src="/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_4.png" />

도커는 구성 요소의 실행을 격리하고 관리하는 데 사용할 수있는 컨테이너화 플랫폼을 제공합니다. 이 아키텍처에서 도커가 어떻게 적합한지 살펴보겠습니다:

- **도커 이미지**: 각 서비스(Spark Master, Spark Worker 및 Minio)를 위한 도커 이미지를 만들 수 있습니다. 필요한 모든 종속성(Spark, Minio 이진 파일, Iceberg 라이브러리 등)을 포함하여 일관된 환경을 제공하고 다양한 기기에 배포를 단순화합니다.
- **도커 콤포즈**: 도커 콤포즈와 같은 도구를 사용하여 모든 서비스의 구성 및 배포를 함께 관리할 수 있습니다. 이 도구는 서비스, 종속성 및 환경 변수를 단일 YAML 파일에 정의하여 전체 환경 설정 프로세스를 간편화합니다.

도커를 활용하면 이동성이 뛰어나고 격리된 개발 또는 프로덕션 환경을 구축할 수 있습니다.

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

# 도커 컴포즈 구성

아래는 Minio, Spark 마스터 및 Spark 워커 서비스를 설정하는 Docker Compose 파일입니다.

```js
version: '3.9'
services:
  minio:
    image: minio/minio
    container_name: minio
    environment:
      MINIO_ACCESS_KEY: minioadmin
      MINIO_SECRET_KEY: minioadmin
    ports:
      - "9000:9000"
      - "9001:9001"
    command: server /data --console-address ":9001"
    volumes:
      - minio_data:/data

spark-master:
    image: bitnami/spark:latest
    container_name: spark-master-minio-iceberg
    environment:
      - SPARK_MODE=master
      - SPARK_SUBMIT_ARGS=--packages org.apache.iceberg:iceberg-spark3-runtime:0.12.0
    ports:
      - "7077:7077"
      - "8080:8080"
  spark-worker:
    image: bitnami/spark:latest
    container_name: spark-worker-minio-iceberg
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark-master:7077
      - SPARK_SUBMIT_ARGS=--packages org.apache.iceberg:iceberg-spark3-runtime:0.12.0
    depends_on:
      - spark-master
    ports:
      - "8081:8081"
volumes:
  minio_data:
```

# Docker Compose 파일 설명

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

- Minio 서비스: Minio 서버를 실행하여 포트 9000(API)과 9001(콘솔)을 노출합니다. Minio 데이터는 minio_data라는 이름의 Docker 볼륨에 저장됩니다.
- Spark Master 서비스: Delta Lake 및 Iceberg를 위한 패키지가 포함된 Spark 마스터 노드를 실행합니다. Spark UI 및 마스터 통신을 위한 포트 7077 및 8080이 노출됩니다.
- Spark Worker 서비스: Spark 워커 노드를 실행하고 Spark 마스터에 연결됩니다. Delta Lake 및 Iceberg를 위한 패키지가 포함되어 있습니다.

# 이미지 빌드

다음 명령을 실행하여

```js
docker-compose up -d
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

모든 서비스가 정상적으로 작동 중이거나 문제가 발생한 경우 알림이 표시됩니다 (문제가 발생하지 않기를 희망합니다 😄)

![Image 5](/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_5.png)

![Image 6](/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_6.png)

먼저 http://localhost:9001/browser를 방문하여 아래 이미지를 확인해보세요.

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

<img src="/assets/img/2024-06-22-UsingApacheIcebergwithApacheSparkandMinioDocker_7.png" />

모든 서비스가 실행되면 Apache Iceberg를 사용하여 Apache Spark 및 Minio로 읽기와 쓰기를 하는 간단한 데이터 파이프라인을 생성해 봅시다.

# Spark 작업 설정 및 실행하기

다음의 Python 코드는 Minio와 상호 작용하고 Iceberg를 사용하여 Spark를 사용하는 방법을 보여줍니다.

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

# Minio 클라이언트 초기화

제공된 자격 증명으로 Minio 서버에 연결합니다.

```js
from minio import Minio
client = Minio(
    "127.0.0.1:9000",
    access_key="your_admin_name_account",
    secret_key="your_password",
    secure=False
)
```

## 설명

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

- Minio 초기화: 이 코드는 Minio 클라이언트를 초기화하여 127.0.0.1의 9000 포트에서 실행 중인 Minio 서버에 연결합니다. 액세스 키와 시크릿 키로 minioadmin을 사용하고, SSL을 사용하지 않는 연결이므로 secure를 False로 설정합니다.

# 버킷 관리

버킷이 존재하는지 확인하고, 존재하지 않으면 생성합니다.

```js
minio_bucket = "my-first-bucket"
found = client.bucket_exists(minio_bucket)
if not found:
    client.make_bucket(minio_bucket)
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

## 설명

- Bucket 존재 여부 확인: bucket_exists 메서드는 Minio에 my-first-bucket이라는 이름의 버킷이 이미 있는지 확인합니다.
- Bucket 생성: 버킷이 존재하지 않는 경우 make_bucket 메서드를 사용하여 my-first-bucket이라는 새 버킷을 만듭니다.

# 파일 업로드

Minio에 지정된 버킷에 CSV 파일을 업로드합니다.

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
목적지_파일 = 'data.csv'
원본_파일 = './data/data.csv'  # 프로젝트 폴더에이 파일이 존재하는지 확인하세요
client.fput_object(minio_bucket, 목적지_파일, 원본_파일)
```

## 설명

- 파일 업로드: fput_object 메서드는 로컬 파일 ./data/data.csv를 Minio 버킷 my-first-bucket으로 객체 이름이 data.csv인 파일로 업로드합니다.

## 코드 실행 후🎦

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

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*rZUjqaNCls7_73eojd2RQw.gif)

# SparkSession 구성

Spark를 Iceberg 및 Minio를 스토리지 백엔드로 사용하도록 구성합니다.

```js
from pyspark.sql import SparkSession
iceberg_builder = SparkSession.builder \
    .appName("iceberg-spark-minio-example") \
    .config("spark.jars.packages", "org.apache.hadoop:hadoop-aws:3.3.4,org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.0,org.apache.iceberg:iceberg-hive-runtime:1.5.0") \
    .config("spark.sql.extensions", "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions") \
    .config("spark.sql.catalog.spark_catalog", "org.apache.iceberg.spark.SparkCatalog") \
    .config("spark.sql.catalog.spark_catalog.type", "hadoop") \
    .config("spark.sql.catalog.spark_catalog.warehouse", f"s3a://{minio_bucket}/iceberg_data/") \
    .config("spark.hadoop.fs.s3a.access.key", "your_admin_name_account") \
    .config("spark.hadoop.fs.s3a.secret.key", "your_pasword") \
    .config("spark.hadoop.fs.s3a.endpoint", "http://localhost:9000") \
    .config("spark.hadoop.fs.s3a.path.style.access", "true") \
    .enableHiveSupport()
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

## 설명

- SparkSession 빌더: Iceberg 및 Minio를 사용하기 위해 구성된 Spark 세션 빌더를 생성합니다.
- 애플리케이션 이름: "iceberg-spark-minio-example"로 애플리케이션 이름을 설정합니다.
- JAR 패키지: Hadoop AWS 및 Iceberg에 필요한 JAR를 포함합니다.
- Spark 확장: Spark SQL을 위해 Iceberg 확장 기능을 활성화합니다.
- 카탈로그 구성: 카탈로그 유형을 Hadoop으로 구성하고, 웨어하우스 위치를 Minio 버킷을 가리키는 S3 경로로 설정합니다.
- Minio 자격 증명: Minio의 액세스 및 시크릿 키를 설정합니다.
- 엔드포인트 구성: S3 엔드포인트를 로컬 Minio 서버를 가리키도록 구성하고, S3 URI에 대한 경로 스타일 액세스를 활성화합니다.

# Iceberg용 SparkSession 빌드

Iceberg 테이블과 상호 작용하기 위한 Spark 세션을 빌드합니다.

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
iceberg_spark = iceberg_builder.getOrCreate();
```

## 설명

- SparkSession 생성: getOrCreate 메서드는 지정된 구성으로 Spark 세션을 초기화합니다.

# 데이터 로드

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

스파크 데이터프레임으로 CSV 파일을 읽습니다.

```js
df = iceberg_spark.read.format("csv").option("header", "true").option("inferSchema", "true").load(source_file);
```

## 설명

- CSV 파일 읽기: ./data/data.csv 파일을 스파크 데이터프레임으로 로드합니다. header 옵션은 첫 번째 행을 열 이름으로 사용하도록 설정되었고, inferSchema 옵션은 데이터 유형을 자동으로 추정하도록 설정되었습니다.

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

# 아이스버그 테이블 위치 정의

미니오 내 아이스버그 테이블의 위치를 지정합니다.

```js
iceberg_table_location = f"s3a://{minio_bucket}/iceberg_data/default"
```

## 설명

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

- Iceberg Table 위치: Minio에 Iceberg 테이블 데이터를 저장하는 데 사용되는 S3 경로를 정의합니다. 경로는 버킷 이름과 하위 디렉터리 iceberg_data/default을 사용하여 구성됩니다.

# Iceberg 테이블에 쓰기

DataFrame 데이터를 Minio의 Iceberg 테이블에 씁니다.

```js
df.write \
    .format("iceberg") \
    .mode("append") \
    .saveAsTable("iceberg_table_name")  # Iceberg 테이블의 이름
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

## 설명

- DataFrame 작성: Iceberg 형식을 사용하여 append 모드로 iceberg_table_name이라는 Iceberg 테이블에 DataFrame을 작성합니다.

## 코드 실행 후🎦

<img src="https://miro.medium.com/v2/resize:fit:1400/1*oUTGnwgU4yk2nG31-kuIWA.gif" />

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

# Iceberg 테이블에서 읽기

Iceberg 테이블에서 데이터를 읽어 스키마와 몇 가지 샘플 레코드를 표시합니다.

```js
iceberg_df = iceberg_spark.read.format("iceberg").load(f"{iceberg_table_location}/iceberg_table_name")
iceberg_df.printSchema()
iceberg_df.show()
```

## 설명

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

- 아이스버그 테이블 읽기: 아이스버그 테이블에서 데이터를 읽어와 DataFrame에로드합니다.
- 스키마 출력: DataFrame의 스키마를 표시합니다.
- 데이터 표시: DataFrame에서 몇 가지 샘플 레코드를 표시합니다.

## 코드 실행 후🎦

<img src="https://miro.medium.com/v2/resize:fit:1400/1*qnK1mFZ6ecqTXBGCtEWi0w.gif" />

# 전체 코드🖥️

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
from minio import Minio
from pyspark.sql import SparkSession
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

# Minio 클라이언트 및 버킷 생성
client = Minio(
    "127.0.0.1:9000",
    access_key="minioadmin",
    secret_key="minioadmin",
    secure=False
)

minio_bucket = "my-first-bucket"

found = client.bucket_exists(minio_bucket)
if not found:
    client.make_bucket(minio_bucket)

destination_file = 'data.csv'
source_file = './data/data.csv' ## 프로젝트 폴더 내에 파일이 있어야 함

# Minio에 파일 업로드
client.fput_object(minio_bucket, destination_file, source_file,)

# Iceberg와 Minio 설정을 사용한 SparkSession 빌더 생성
iceberg_builder = SparkSession.builder \
    .appName("iceberg-concurrent-write-isolation-test") \
    .config("spark.jars.packages", "org.apache.hadoop:hadoop-aws:3.3.4,org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.0,org.apache.iceberg:iceberg-hive-runtime:1.5.0") \
    .config("spark.sql.extensions", "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions") \
    .config("spark.sql.catalog.spark_catalog", "org.apache.iceberg.spark.SparkCatalog") \
    .config("spark.sql.catalog.spark_catalog.type", "hadoop") \
    .config("spark.sql.catalog.spark_catalog.warehouse", f"s3a://{minio_bucket}/iceberg_data/") \
    .config("spark.hadoop.fs.s3a.access.key", "minioadmin") \
    .config("spark.hadoop.fs.s3a.secret.key", "minioadmin") \
    .config("spark.hadoop.fs.s3a.endpoint", "http://localhost:9000") \
    .config("spark.hadoop.fs.s3a.path.style.access", "true") \
    .enableHiveSupport()

# Iceberg를 위한 SparkSession 생성
iceberg_spark = iceberg_builder.getOrCreate()

# Iceberg 테이블을 Minio에 쓰기
df.write \
    .format("iceberg") \
    .mode("append") \
    .saveAsTable("iceberg_table_name")  # Iceberg 테이블 이름

# Iceberg 테이블에서 데이터 읽기
iceberg_df = iceberg_spark.read.format("iceberg").load(f"{iceberg_table_location}/iceberg_table_name")

# 데이터프레임 스키마 및 데이터 출력
print("**************************")
print("This the Dataframe schema ")
print("**************************")
iceberg_df.printSchema()

print("**************************")
print("******Dataframe Data******")
print("**************************")
iceberg_df.show()
```

# GitHub

프로젝트 링크

# Summary

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

이 설정을 사용하면 Spark 및 Iceberg로 대규모 데이터를 효율적으로 관리하고 처리할 수 있고, 확장 가능한 객체 저장소로 Minio를 사용할 수 있습니다. Docker를 활용하면 이러한 구성 요소의 배포와 관리가 간편하고 재현 가능해집니다.

즐거운 학습이 되길 바래요 😉
