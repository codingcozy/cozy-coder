---
title: "데이터 바인딩 내부 작동 원리 기술적 분석"
description: ""
coverImage: "/assets/img/2024-07-07-HOWDATABINDINGWORKSINTERNALLY_0.png"
date: 2024-07-07 23:17
ogImage:
  url: /assets/img/2024-07-07-HOWDATABINDINGWORKSINTERNALLY_0.png
tag: Tech
originalTitle: "HOW DATA BINDINGWORKS INTERNALLY"
link: "https://medium.com/@sandeepkella23/how-data-bindingworks-internally-ca21d7f4d841"
isUpdated: true
---

안녕하세요! 오늘은 안드로이드에서 데이터 바인딩이 어떻게 작동하는지, 그리고 로직 코드를 어떻게 업데이트하는지에 대해 살펴보겠습니다. 이에 대해 깊이 있는 통찰을 얻을 수 있어요. 데이터 바인딩 컴파일러가 코드를 생성하는 방법, 바인딩 표현식을 어떻게 관리하는지, 그리고 데이터 변경이 UI로 효율적으로 전파되도록 하는 방법을 알아야 해요.

# 안드로이드 데이터 바인딩의 내부 동작

- 레이아웃 XML 구문 분석: 레이아웃 XML에서 데이터 바인딩을 사용할 때, 데이터 바인딩 컴파일러는 이 파일들을 빌드할 때 처리해요. `layout` 태그와 그 자식 요소를 인식하여 바인딩 레이아웃을 만들어요.

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

1. Markdown으로 이미지 태그를 변경하기

<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="user"
            type="com.example.User"/>
    </data>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.name}"/>
    </LinearLayout>
</layout>

2. 바인딩 클래스 생성: 데이터 바인딩 컴파일러는 각 레이아웃 파일에 대해 바인딩 클래스를 생성합니다. 이 클래스는 ViewDataBinding을 확장하며, 각 뷰에 대한 필드와 이러한 뷰에 데이터를 바인딩하는 메서드를 포함합니다. 예를 들어 activity_main.xml 파일의 경우 ActivityMainBinding 클래스를 생성합니다.

public final class ActivityMainBinding extends ViewDataBinding {
public final LinearLayout rootView;
public final TextView nameTextView;

    private User user;

    // 생성자
    public ActivityMainBinding(DataBindingComponent bindingComponent, View root) {
        super(bindingComponent, root, 0);
        this.rootView = (LinearLayout) root;
        this.nameTextView = (TextView) root.findViewById(R.id.nameTextView);
    }

    public void setUser(User user) {
        this.user = user;
        // 사용자 속성이 변경되었음을 알립니다.
        notifyPropertyChanged(BR.user);
        executeBindings();
    }

}

3. 바인딩 표현식: XML에 있는 바인딩 표현식은 생성된 바인딩 클래스의 메서드로 번역됩니다. 이러한 메서드는 바인딩된 데이터를 기반으로 뷰의 속성을 설정하는 역할을 합니다. 예를 들어, @'user.name'은 TextView의 텍스트 설정으로 번역됩니다.

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

```java
public void executeBindings() {
    synchronized (this) {
        long dirtyFlags = mDirtyFlags;
        mDirtyFlags = 0;
    }
    // batch finished
    User user = mUser;
    String userName = null;

    if ((dirtyFlags & 0x3L) != 0) {
        // read user.name
        if (user != null) {
            userName = user.getName();
        }
    }
    // update TextView
    if ((dirtyFlags & 0x3L) != 0) {
        TextViewBindingAdapter.setText(this.nameTextView, userName);
    }
}
```

### 4. Observable Data and Change Notifications

The data binding library supports various mechanisms to observe changes in data and update the UI accordingly. This is typically achieved using BaseObservable and ObservableField.

- BaseObservable: You can use BaseObservable to create a data class that notifies property changes. Annotate the getter with `@Bindable` and call `notifyPropertyChanged` when the setter is invoked.

```java
class User : BaseObservable() {
    @get:Bindable
    var name: String by Delegates.observable("John Doe") { _, _, _ ->
        notifyPropertyChanged(BR.name)
    }
}
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

- ObservableField: 간편한 observable 속성이 필요한 간단한 경우에는 전체 BaseObservable 오버헤드 없이 ObservableField를 사용할 수도 있습니다.

```js
class User {
    val name = ObservableField<String>("John Doe")
}
```

5. 양방향 데이터 바인딩: UI가 데이터 모델을 업데이트해야 하는 시나리오에는 양방향 데이터 바인딩이 사용됩니다. XML에서 특별한 구문이 필요하며 InverseBindingAdapter와 InverseBindingListener에 의해 용이하게 이루어집니다.

```js
<EditText android:layout_width="wrap_content" android:layout_height="wrap_content" android:text="@={user.name}" />
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

내부적으로는 UI에서 데이터 모델로의 변경 및 데이터 모델에서 UI로의 변경을 처리하기 위한 추가 코드를 생성합니다.

6. 라이프사이클 및 바인딩 실행: DataBindingUtil.setContentView 메서드는 액티비티나 프래그먼트에서 레이아웃을 바인딩하고 바인딩 클래스의 인스턴스를 생성하는 데 사용됩니다.

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        val user = User("John Doe", 25)
        binding.user = user
    }
}
```

바인딩 클래스의 executeBindings 메서드는 모든 바인딩 표현식을 평가하기 위해 호출됩니다. 데이터가 변경되면 notifyPropertyChanged 메커니즘을 통해 UI가 업데이트되도록 보장됩니다.

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

# 내부 메커니즘 요약

- 레이아웃 파싱: 데이터 바인딩 컴파일러는 레이아웃 XML을 구문분석하여 데이터 변수와 바인딩 표현식을 식별합니다.
- 바인딩 클래스 생성: 각 레이아웃에 대해 ViewDataBinding을 확장하는 바인딩 클래스가 생성되며, 데이터를 뷰에 바인딩하는 메서드가 포함됩니다.
- 바인딩 표현식 평가: 바인딩 표현식은 바인딩 클래스 내에서 데이터에 기반한 뷰 속성을 업데이트하는 메서드로 변환됩니다.
- Observable 데이터: 데이터 모델의 변경 사항은 Observable 데이터 클래스(BaseObservable, ObservableField, 또는 LiveData)를 사용하여 UI로 전파됩니다.
- 라이프사이클 관리: 데이터 바인딩 클래스는 데이터 바인딩된 뷰의 라이프사이클을 관리하고 효율적인 업데이트를 보장합니다.

이러한 단계와 메커니즘은 데이터 바인딩 라이브러리가 데이터 모델의 변화에 반응하여 UI를 효율적으로 자동으로 업데이트하도록 보장하며, 사용자에게 적응적이고 원활한 경험을 제공합니다.
