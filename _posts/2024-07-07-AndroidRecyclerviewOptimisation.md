---
title: "안드로이드 RecyclerView 최적화 방법 5가지"
description: ""
coverImage: "/assets/img/2024-07-07-AndroidRecyclerviewOptimisation_0.png"
date: 2024-07-07 03:15
ogImage:
  url: /assets/img/2024-07-07-AndroidRecyclerviewOptimisation_0.png
tag: Tech
originalTitle: "Android Recyclerview Optimisation"
link: "https://medium.com/@farimarwat/android-recyclerview-optimisation-3d5408ae31cf"
isUpdated: true
---

![RecyclerView Optimization](/assets/img/2024-07-07-AndroidRecyclerviewOptimisation_0.png)

# 소개

RecyclerView는 안드로이드에서 대량의 데이터를 효과적이고 유연하게 표시하는 강력한 구성 요소입니다. 그러나 적절한 최적화 없이 사용하면 지연, 애플리케이션이 응답하지 않음(ANR) 오류 및 메모리 누수 등의 성능 문제로 이어질 수 있습니다.

이 글에서는 RecyclerView 사용 시 흔한 잘못된 방법을 살펴보고, 사용자 경험이 원활해지도록 최적화하는 방법을 배워보겠습니다. 여러 성능 문제가 있는 기본 구현부터 시작하여 단계적으로 향상시켜 보겠습니다. 이 글을 끝까지 따라오면 효율적인 RecyclerView 구현을 위한 모베스트(최상위) 관행 세트를 갖게 될 것입니다.

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

# 나쁜 관행들

우리는 몇 가지 흔한 나쁜 관행을 포함하는 RecyclerView의 기본 구현을 살펴보며 시작해보겠습니다. 이 예제는 우리가 최적화를 위한 출발점으로 사용될 것입니다.

```kotlin
class BadPracticeAdapter(private val itemList: List<Item>) : RecyclerView.Adapter<BadPracticeAdapter.ViewHolder>() {

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val titleTextView: TextView = itemView.findViewById(R.id.titleTextView)
        val imageView: ImageView = itemView.findViewById(R.id.imageView)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_layout, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val item = itemList[position]

        // 나쁜 관행: onBindViewHolder 내부에서 계산 수행
        val titleLength = item.title.length
        if (titleLength > 10) {
            holder.titleTextView.text = item.title.substring(0, 10) + "..."
        } else {
            holder.titleTextView.text = item.title
        }

        // 나쁜 관행: onBindViewHolder 내부에서 비트맵 디코딩
        val bitmap = BitmapFactory.decodeFile(item.imagePath)
        holder.imageView.setImageBitmap(bitmap)
    }

    override fun getItemCount(): Int {
        return itemList.size
    }
}
```

# 나쁜 관행 식별하기

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

- onBindViewHolder에서 계산 수행: onBindViewHolder 메서드에는 제목의 길이를 결정하고 조건에 따라 수정하는 등의 계산이 포함되어 있습니다. 이는 지연 및 스크롤 성능 저하로 이어질 수 있습니다.
- Bitmap 직접 사용: 이미지를 로드하기 위해 BitmapFactory.decodeFile을 직접 사용하는 것은 비효율적이며 메모리 누수와 성능 문제를 일으킬 수 있습니다.
- 데이터 준비 미흡: 데이터가 효율적으로 준비되지 않아 반복적이고 불필요한 계산이 발생합니다.

# OnBindViewHolder 최적화

이제 계산과 조건 로직을 onBindViewHolder 메서드 밖으로 이동하여 최적화해 봅시다. 이렇게 하면 RecyclerView의 성능 및 부드러운 스크롤링이 향상됩니다.

## 최적화된 코드

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

먼저, 어댑터에 전달하기 전에 데이터를 전처리합니다. 이는 제목과 이미지를 미리 준비하는 과정을 포함합니다.

```kotlin
data class Item(val title: String, val imagePath: String, var preparedTitle: String = "", var preparedBitmap: Bitmap? = null)

// 데이터 전처리
fun prepareData(items: List<Item>): List<Item> {
    for (item in items) {
        // 제목 준비
        item.preparedTitle = if (item.title.length > 10) {
            item.title.substring(0, 10) + "..."
        } else {
            item.title
        }

        // 비트맵 준비
        item.preparedBitmap = BitmapFactory.decodeFile(item.imagePath)
    }
    return items
}

class OptimizedAdapter(private val itemList: List<Item>) : RecyclerView.Adapter<OptimizedAdapter.ViewHolder>() {

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val titleTextView: TextView = itemView.findViewById(R.id.titleTextView)
        val imageView: ImageView = itemView.findViewById(R.id.imageView)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_layout, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val item = itemList[position]

        // 최적화: 전처리된 데이터 사용
        holder.titleTextView.text = item.preparedTitle
        holder.imageView.setImageBitmap(item.preparedBitmap)
    }

    override fun getItemCount(): Int {
        return itemList.size
    }
}
```

# 개선 사항

- 데이터 전처리: 제목과 비트맵을 미리 처리하여 onBindViewHolder 내의 계산 및 조건 로직을 피합니다.
- 부드러운 스크롤링: onBindViewHolder에서 무거운 작업을 제거함으로써 더 부드러운 스크롤링을 보장합니다.

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

# 효율적인 이미지 로딩

이미지를 효율적으로 로딩하는 것은 RecyclerView에서 부드러운 스크롤을 유지하고 메모리 문제를 예방하는 데 중요합니다. 나쁜 예시에서 보여주는 것처럼 Bitmap 클래스를 직접 사용하는 것은 메모리 누수와 성능 저하로 이어질 수 있습니다. 대신, Glide와 같은 이미지 로딩 라이브러리를 사용할 수 있습니다. Glide는 이미지 로딩 및 캐싱을 처리하는 데 최적화되어 있습니다.

## Glide의 장점은?

- 메모리 효율성: Glide는 이미지 캐싱과 메모리 관리를 자동으로 처리합니다.
- 부드러운 성능: Glide는 이미지를 비동기적으로 로딩하여 UI 스레드 블로킹을 방지합니다.
- 사용하기 쉬움: Glide는 다양한 소스에서 이미지를 로드하기 위한 간단한 API를 제공합니다.

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

## 프로젝트에 Glide 추가하기

먼저, Glide를 사용하여 프로젝트에 Glide를 추가하세요. 이를 위해서 build.gradle 파일에 아래 종속성을 포함시키세요:

```js
implementation 'com.github.bumptech.glide:glide:4.12.0'
```

## Glide로 코드 최적화하기

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

여기 Glide를 사용하여 이미지를로드하도록 어댑터를 수정하는 방법이 있어요:

```kotlin
class OptimizedAdapter(private val itemList: List<Item>) : RecyclerView.Adapter<OptimizedAdapter.ViewHolder>() {

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val titleTextView: TextView = itemView.findViewById(R.id.titleTextView)
        val imageView: ImageView = itemView.findViewById(R.id.imageView)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_layout, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val item = itemList[position]

        // 최적화된: 전처리된 데이터 사용
        holder.titleTextView.text = item.preparedTitle

        // 최적화된: Glide로 이미지로드
        Glide.with(holder.imageView.context)
            .load(item.imagePath)
            .placeholder(R.drawable.placeholder) // 선택 사항: 플레이스홀더
            .error(R.drawable.error) // 선택 사항: 오류 이미지
            .into(holder.imageView)
    }

    override fun getItemCount(): Int {
        return itemList.size
    }
}
```

# 개선 사항

- Glide 사용: Glide는 이미지로드, 캐싱 및 메모리 관리를 효율적으로 처리합니다.
- 비동기로딩: 이미지는 비동기로로드되므로 부드러운 스크롤링과 반응이 빠른 UI가 보장됩니다.

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

# 결론

이러한 최적화 방법을 따르면 RecyclerView의 성능을 크게 향상시킬 수 있습니다:

- onBindViewHolder 안에서 계산을 피하고 데이터 전처리를 해주세요.
- 중복 계산을 줄이기 위해 어댑터 외부에서 데이터를 준비해주세요.
- Glide를 사용하여 효율적인 이미지로딩을 하여 부드러운 스크롤링과 좋은 메모리 관리를 보장해주세요.
- 페이징을 사용해주세요.
