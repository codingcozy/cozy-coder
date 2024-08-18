---
title: "OpenLlama, Postgres, Nodejs 및 Nextjs로 100 무료 벡터 검색 방법"
description: ""
coverImage: "/assets/img/2024-08-04-100FreeVectorSearchwithOpenLlamaPostgresNodejsandNextjs_0.png"
date: 2024-08-04 18:34
ogImage:
  url: /assets/img/2024-08-04-100FreeVectorSearchwithOpenLlamaPostgresNodejsandNextjs_0.png
tag: Tech
originalTitle: "100 Free Vector Search with OpenLlama, Postgres, Nodejs and Nextjs"
link: "https://medium.com/javascript-in-plain-english/100-free-vector-search-with-openllama-postgres-nodejs-and-nextjs-e496856766f7"
isUpdated: true
---

요즘 벡터 검색을 시도해보고 싶은데 OpenAI나 Huggingface를 사용하기 싫고, 벡터 데이터베이스 회사에 돈을 지불하고 싶지 않다니까! 그럼 내가 돕겠어. 무료로 여러분의 기계에서 벡터 검색을 시작해볼까요?

![이미지](/assets/img/2024-08-04-100FreeVectorSearchwithOpenLlamaPostgresNodejsandNextjs_0.png)

# 우리가 하는 일은 뭔가요?

간단하게 다시 한 번 돌아가서 우리가 무엇을 하고, 그것을 왜 하는지 이야기해봅시다. AI 임베딩을 사용한 벡터 검색은 개념을 기반으로 검색을 만드는 방법이에요. 예를 들어 '애완동물'을 검색하면 개와 고양이의 결과가 나올 수 있어요. 이는 상품을 검색할 때 고객이 더 나은 결과를 얻을 수 있다는 점에서 매우 가치 있어요.

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

이를 달성하기 위해서는 먼저 찾고 싶은 텍스트를 가져와서 AI에게 보내 Embedding을 생성하도록 합니다. Embedding은 일반적으로 ~300부터 ~1500개의 부동 소수점 값으로 이루어진 긴 배열입니다.

고양이, 개 및 애완동물의 Embedding 값은 유사할 것입니다. 따라서 고양이와 개를 비교하면 가깝게 나타날 것이며, 개와 피자를 비교하면 가깝지 않을 것입니다.

벡터 데이터베이스를 사용하면 이러한 벡터를 저장할 수 있으며, 연결된 데이터(아마 원래 데이터의 텍스트)와 함께 저장할 수 있습니다. 데이터가 저장되면 새로운 벡터로 데이터베이스를 쿼리하여 가까운 결과를 얻을 수 있습니다. 예를 들어, 우리가 고양이와 개를 Embedding과 함께 데이터베이스에 저장하면 "애완동물"이라는 입력 텍스트를 취해 해당 Embedding을 생성한 다음 데이터베이스를 쿼리하여 고양이와 개를 반환할 가능성이 있습니다.

# 왜 Postgres와 OpenLlama를 선택해야 하는가?

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

포스트그레SQL(Postgres)은 로컬에 쉽게 설치하고 실행할 수 있는 환상적인 데이터베이스입니다. 포스트그레SQL의 pgvector 확장 기능을 통해 벡터 필드를 만들어 SQL 쿼리에서 사용할 수도 있습니다.

맥(Mac)에서 포스트그레스(Postgres)를 설치하는 여러 가지 방법이 있습니다. 제가 사용한 방법은 Postgres.app을 사용하여 포스트그레스를 설치한 것입니다.

OpenLlama은 AI 모델을 로컬에서 쉽게 설치하고 실행할 수 있는 방법입니다. Homebrew를 사용하여 OpenLlama를 설치하는 brew install ollama 명령어를 사용했습니다.

간단한 테스트 애플리케이션에는 1986년 공포 영화 Alien의 모든 대사를 불러올 것입니다.

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

# 준비 과정

OpenLlama에는 선택할 수 있는 다양한 모델이 있습니다. 이 애플리케이션에서는 빠른 임베딩 생성을 위해 snowflake-arctic-embed를 선택했습니다. 이를 설치하기 위해 ollama pull snowflake-arctic-embed 명령을 사용했습니다.

설정의 마지막 단계는 로컬 포스트그레스 데이터베이스를 생성하는 것입니다. 원하는 대로 이름을 지정할 수 있지만, 저는 영화 대사에서 라인을 검색하기 때문에 lines라고 이름을 지었습니다.

데이터베이스를 생성한 후에는 몇 가지 명령을 실행하기 위해 psql 명령을 사용할 수 있습니다. 첫 번째로 데이터베이스에 벡터 확장 기능을 추가하는 것입니다. 이를 통해 vector 필드 유형이 활성화됩니다. 이를 수행하기 위해 create extension 명령을 사용합니다.

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
CREATE EXTENSION vector;
```

이제는 라인 텍스트와 벡터를 보관할 테이블을 생성해야 합니다. 다음은 스크립트 내 라인의 위치인 position 값을 기반으로한 테이블과 인덱스를 생성하는 명령어입니다.

```js
CREATE TABLE lines (
  id bigserial PRIMARY KEY,
  position INT,
  text TEXT,
  embedding VECTOR(1024)
);
CREATE UNIQUE INDEX position_idx ON lines (position);
```

여기서 주목해야 할 점은 벡터의 크기입니다. 서로 다른 모델은 다양한 크기의 벡터를 생성합니다. 우리가 사용하는 snowflake 모델의 임베딩 크기는 1,024 숫자이므로 벡터 크기를 1,024로 설정했습니다.

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

동일한 AI를 저장 및 쿼리에 모두 사용하는 것이 좋습니다. 다른 모델을 사용하면 숫자가 정렬되지 않을 수 있습니다.

# 벡터 인덱스 생성

상상해보면, 두 개의 1,024 값이 포함된 부동 소수점 배열을 비교하는 것은 비용이 많이 들 수 있습니다. 그리고 그것들을 많이 비교하는 것은 매우 비용이 많이 들 수 있습니다. 그래서 이러한 새로운 벡터 데이터베이스는 그것을 더 효율적으로 만들기 위해 다른 인덱싱 모델을 개발했습니다. Postgres 벡터 지원은 다양한 종류의 인덱스를 갖고 있습니다. 우리는 세 가지 다른 인덱스를 만들기 위해 Hierarchical Navigable Small Worlds (HNSW) 유형을 사용할 것입니다:

```js
CREATE INDEX ON lines USING hnsw (embedding vector_ip_ops);
CREATE INDEX ON lines USING hnsw (embedding vector_cosine_ops);
CREATE INDEX ON lines USING hnsw (embedding vector_l1_ops);
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

왜 세 개일까요? 두 개의 벡터를 비교하는 여러 방법이 있기 때문이죠. 코사인이 있고, 일반적으로 기본값으로 설정되어 있으며 개념 비교에 좋습니다. 또한 유클리드 및 내적 비교도 있습니다. Postgres는 이러한 방법들(그리고 더 많은 방법들)을 모두 지원합니다.

사용하는 방법에 관계없이 해당 인덱스가 활성화되어 있는지 확인하여 빠른 쿼리를 얻을 수 있도록 하세요.

# 데이터베이스 로드

모델을 다운로드하고 Postgres를 설정한 후, 이제 영화 대사와 그들의 임베딩으로 데이터베이스를 로드할 수 있습니다. 저는 각각의 프로젝트를 github에 게시했습니다. 이 프로젝트에는 NextJS 앱 라우터 UI도 포함되어 있습니다. 스크립트는 load-embeddings 디렉토리에 있습니다. 원본 데이터는 이 스크립트 페이지에서 가져왔습니다.

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

데이터를로드하기 전에 .env.example 파일을 .env로 복사 한 다음 Postgres 연결 세부 정보와 일치하도록 값 변경해야합니다.

Postgres로 임베딩을로드하려면 Node 20 이상에서 node loader.mjs를 실행하십시오.

스크립트의 주요 부분은 임베딩 생성입니다:

```js
import ollama from "ollama";
...
  const response = await ollama.embeddings({
    model: "snowflake-arctic-embed",
    prompt: text,
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

"ollama" 라이브러리를 사용하여 텍스트 한 줄씩 가져와 눈송이 임베딩 모델을 호출합니다.

그런 다음 다음과 같이 INSERT 문을 사용하여 데이터베이스에 행을 삽입합니다:

```js
await sql`INSERT INTO lines
    (position, text, embedding)
  VALUES
    (${position}, ${text}, ${`[${response.embedding.join(",")}]`})
  `;
```

여기서 유일하게 주의해야 할 점은 숫자를 문자열로 모두 결합하고 괄호로 묶어 중첩하는 방식으로 임베딩 값을 포맷하는 것입니다."

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

데이터를 모두 데이터베이스에 로드했으니 이제 쿼리를 만드는 시간입니다.

# 첫 번째 쿼리 만들기

작동하는지 확인하기 위해 load-embeddings 디렉토리에 test-query.mjs 파일이 있습니다. 벡터 쿼리를 만들려면 먼저 모델을 실행하여 쿼리를 벡터로 변환해야 합니다. 다음과 같이 실행하세요:

```js
const response = await ollama.embeddings({
  model: "snowflake-arctic-embed",
  prompt: "food",
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

이 경우에는 프롬프트가 음식이고, 우리는 이를 임베딩으로 변환하는 loader 스크립트와 똑같은 프로세스를 사용합니다.

그런 다음 해당 벡터로 데이터베이스를 쿼리하기 위해 SQL SELECT 문을 사용합니다:

```js
const query = await sql`SELECT
  position, text
FROM
  lines
ORDER BY
  embedding <#> ${`[${response.embedding.join(",")}]`} 
LIMIT 10`;
```

우리는 데이터베이스의 레코드를 주어진 임베딩에 대한 유사성에 따라 정렬하기 위해 ORDER BY를 사용하고 LIMIT을 사용하여 가장 유사한 상위 10개만 반환합니다.

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

`ORDER BY`에서 `#` 구문은 어떤 비교 알고리즘을 사용할지 정의하는 데 중요합니다. 문서를 통해 사용할 수 있는 옵션은 다음과 같습니다:

```js
<-> - L2 거리
<#> - (부정적) 내적
<=> - 코사인 거리
<+> - L1 거리 (0.7.0에서 추가됨)
```

어떤 비교가 애플리케이션에 가장 적합한 결과를 제공하는지 스스로 결정할 수 있지만, 그 비교 방법에 기반하여 테이블을 적절하게 색인화해야 합니다.

내 머신에서 이 테스트 쿼리를 수행하면 다음과 같은 결과가 나왔습니다:

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
317 제이크는 옥수수 빵도 좋아 안 하는 것 같네.
```

해당 대화는 실제로 음식인 옥수수 빵을 언급하는 영화 속의 클래식한 대사입니다.

# 사용자 인터페이스 구현

조금 더 노력하여 이를 위해 NextJS 앱 라우터 인터페이스를 구현했습니다. 프로젝트의 루트 디렉터리에서 pnpm dev를 실행하고 데이터베이스가로드되고 .env 파일이 올바르게 설정된 후에 사용해볼 수 있습니다.

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

이 NextJS 앱은 데이터베이스에서 라인을 쿼리하는 데 정확히 같은 SELECT 작업을 사용합니다.

# 결론

명백히 외계인 대사를 검색하는 애플리케이션을 제작할 생각은 없을 것입니다. 그러나 여기에서 보여드린 내용을 통해 텍스트 내용, 제품 설명, 댓글 등 거의 모든 종류의 텍스트를 검색할 수 있습니다.

즐기세요!

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

# 평문으로 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 작가를 클랩하고 팔로우해 주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기
