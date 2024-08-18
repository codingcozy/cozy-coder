---
title: "CSS로 3D 효과 만들기 시작부터 마스터까지"
description: ""
coverImage: "/assets/img/2024-08-19-Creating3DeffectsinCSS_0.png"
date: 2024-08-19 02:54
ogImage: 
  url: /assets/img/2024-08-19-Creating3DeffectsinCSS_0.png
tag: Tech
originalTitle: "Creating 3D effects in CSS"
link: "https://dev.to/logrocket/creating-3d-effects-in-css-1h6"
isUpdated: false
---


Written by Oscar Jite-Orimiono✏️

프론트엔드 개발자로서 당신은 아마도 HTML, CSS 및 JavaScript를 설명하는 데 사용되는 집 비유를 들어본 적이 있을 것입니다. HTML은 구조(벽, 방, 문 등)를 나타내고, CSS는 장식(페인팅, 레이아웃, 가구)을 담당하며, JavaScript는 전기와 배관으로 기능을 제공합니다.

CSS에 대해 별도로 살펴보죠. 일단 CSS는 멋진데다가 CSS 자체로도 강력하며 경우에 따라 JavaScript가 필요하지 않은 상황에서 기능을 제공할 수 있습니다. CSS를 사용하면 상상력이 유일한 제한이지만 너무 방심하지 말아야 합니다. 고수준의 기능성을 유지해야 하기 때문입니다.

이 글에서는 CSS를 사용하여 가상 3D 공간에서 요소를 제어하는 관련 속성 및 작업을 활용하여 3D 효과를 만드는 방법, 또한 그림자와 빛으로 시각적 환상을 다루는 방법을 배우게 될 것입니다.

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

## 어떻게 3D인지 알 수 있나요?

3D 공간에 있는 물체는 길이, 너비(“길이”와 기술적으로 같지만 일반적으로 더 짧은 쪽으로 간주됨), 그리고 높이(또는 깊이)와 같은 세 가지 차원을 갖습니다. 화면을 보면 2D로만 보이기 때문에, 3D 효과를 만들려면 transform과 같은 CSS 속성이 필요합니다.

transform 속성에는 요소의 3D 차원과 위치를 제어하는 여러 가지 작업이 있습니다. translateX()로 수평 위치를 제어하고, translateY()로 수직 위치를 제어하며, translateZ()로 높이와 깊이를 제어합니다.

![이미지](/assets/img/2024-08-19-Creating3DeffectsinCSS_0.png)

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


![이미지](/assets/img/2024-08-19-Creating3DeffectsinCSS_1.png)

만약 translateZ()를 추가하여 자식 요소의 z축 위치를 변경하려고 하면 아무 일도 일어나지 않을 거예요. z축은 뷰포트에 수직이기 때문에 자식 요소가 화면에서 멀어질 때 작아 보이고 가까워질 때 크게 보일 거에요. 그것이 위에서 물체를 바라볼 때 현실 세계에서 작동하는 방식이에요. 그러나 가상적으로는 같은 방식으로 동작하지 않아요. 왜냐하면 원근이 없기 때문이죠.

CSS에서 perspective 속성과 perspective() 연산자는 3D 변형이 작동하는 방식에 매우 중요해요. 원근은 당신의 시각점이며 깊이와 높이의 환상을 만들어냅니다.

perspective 속성은 부모 요소에 사용되고, perspective()는 자식 요소의 transform 속성과 함께 사용되요. 둘 다 같은 일을 하지만 - 대부분의 경우에요.


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

시각 속성이 translateZ()에 영향을 미치는 방법을 살펴보겠습니다. 간단한 애니메이션을 통해 이를 시연할 것입니다.

다음은 부모 및 자식 요소에 대한 HTML 및 CSS입니다:

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(255, 255, 255, 0.07);
  width: 400px;
  height: 500px;
  border: 0.5px solid rgba(255, 255, 255, 0.15);
  border-radius: 5px;
  z-index: 0;
}
.child {
  background: rgba(255, 255, 255, 0.25);
  width: 200px;
  height: 250px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 5px;
  z-index: 1;
}
```

우선, 부모 요소에 시각을 추가한 다음, 자식 요소에 변환 애니메이션을 추가하세요:

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
.parent{
  perspective: 1000px;
}
.child{
   animation: transform 5s infinite ease-in-out alternate-reverse;
}
@keyframes transform{
  0%{
    transform: translateZ(-200px)
  }
  100%{
    transform: translateZ(200px)
  }
}
```

<img src="https://res.cloudinary.com/practicaldev/image/fetch/s--ptMXG3Sq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Perspective-operator-css-3d.gif%3Fw%3D584" />

`perspective()` 연산자를 사용하면 동일한 결과를 얻을 수 있습니다. 그러나 `rotate()`와 함께 사용할 때는 다른 결과를 얻을 수 있습니다.

이제 x축을 따라 child를 회전시켜보고 무슨 일이 일어나는지 살펴보겠습니다:


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

```css
.child{
  transform: perspective(10px) rotateX(2deg);
}
```

![image](/assets/img/2024-08-19-Creating3DeffectsinCSS_2.png)

![image](/assets/img/2024-08-19-Creating3DeffectsinCSS_3.png)

![image](https://res.cloudinary.com/practicaldev/image/fetch/s--eNoJSPnF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Stretching-z-axis-3d-css.gif%3Fw%3D494)


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

원소의 표면 위에 있다고 생각해보세요. VR 헤드셋을 통해 이를 살펴볼 수 있다면 멋질 거예요.

또한 standalone 속성을 사용할 수도 있어요:

```js
.parent {
  perspective: 10px;
}
.child {
  transform: rotateX(2deg) translateZ(5px);
}
```

결과는 같지만, 뷰포트를 넘어가지 않을 거예요. 두 가지 모두 사용 사례가 있고, 계속 진행하면서 다른 변환 작업들도 볼 수 있을 거에요.

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

위의 예시에서 사용된 작은 perspective 값은 3D 효과를 과장하여 동작 방식을 보실 수 있도록 한 것입니다. 주로 요소를 위에서 바라보는 것이 좋으므로 1000px까지의 훨씬 큰 값이 적합할 것입니다. 작은 perspective 값은 요소가 더 3D로 보이게 합니다.

다음은 3D 변형 애니메이션이 있는 CodePen입니다.

이제 재미있는 부분으로 넘어가서 CSS로 일부 3D 효과를 만들어 봅시다!

## 반사와 그림자

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

물체가 빛과 상호 작용하는 방식은 그들이 3D 공간 안에 있다는 것을 알 수 있는 또 다른 방법입니다. perspective() 연산자를 사용하여 CSS에서 반사와 그림자를 생성하여 요소가 3D 공간 안에 떠 있는 것처럼 보이도록 할 수 있습니다.

### 3D 텍스트 반사

이 예제는 텍스트를 사용하지만 다른 HTML 요소를 사용할 수도 있습니다:

```js
<div class="content">
    <h1>공간 안의 텍스트</h1>
</div>
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

리플렉션을 만들고 있기 때문에 빛이 필요합니다. 이 빛 효과를 만들기 위해 text-shadow 속성이 필요합니다:

```js
.content h1 {
  position: relative;
  font-size: 5rem;
  font-weight: 600;
  color: #fff;
  margin: 3px;
  text-shadow: 0px 0px 10px #b393d350, 0px 0px 20px #b393d350;
  text-transform: uppercase;
}
```

<img src="/assets/img/2024-08-19-Creating3DeffectsinCSS_4.png" />

이제 가상 요소를 사용하여 리플렉션을 추가하는 시간입니다:

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
  .content h1::before {
    content: "";
    position: absolute;
    top: 80%;
    left: 0;
    height: 100%;
    width: 100%;
    background: #b393d3;
    transform: perspective(10px) rotateX(10deg) scale(1,0.2);
    filter: blur(1em);
    opacity: 0.5;
  }
```

먼저, 가상 요소에 텍스트와 동일한 차원을 부여합니다. 그런 다음 텍스트 섀도우와 동일한 색상의 배경을 추가합니다. 다음으로 transform 속성에 세 가지 작업을 추가합니다.

이미 처음 두 가지가 어떻게 작동하는지 봤으니 scale()에 대해 이야기해 봅시다. 자식 요소가 뷰포트를 벗어나느 경우를 막기 위해 요소를 x 및 y 방향으로 재조정하여 크기를 조절합니다.

<img src="/assets/img/2024-08-19-Creating3DeffectsinCSS_5.png" />

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

문구가 뜬구름처럼 보이며 바닥에 빛이 반사되고 있어요.

### 3D 그림자

이번에는 이미지를 사용하여 그림자를 만들어 볼 수도 있어요. 관련된 방법은 빛을 사용하는 것과 비슷해요.

<img src="/assets/img/2024-08-19-Creating3DeffectsinCSS_6.png" />

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

요소의 형태와 크기에 그림자가 잘 맞으면 좋겣다. 이 경우 로켓이 있으므로 그림자는 둥근 형태여야 합니다. 따라서 perspective() 연산자가 필요하지 않습니다.

다음은 HTML 코드입니다:

```js
<div class="content">
    <div class="rocket">
      <img src="rocket.png" alt="">
    </div>
  </div>
```

CSS에서 이미지에 원하는 크기를 지정한 다음, 그 아래에 가상 요소를 추가하세요.

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
.rocket::before {
  content: "";
  position: absolute;
  bottom: -10%;
  left: 0;
  height: 50px;
  width: 100%;
  border-radius: 50%;
  background: radial-gradient(rgba(0, 0, 0, 0.8), transparent, transparent);
  transition: 0.5s;
}
```

50%의 border-radius로 타원 모양을 만듭니다. 높이를 늘리거나 구체적인 폭을 추가할 수 있습니다. 그림자는 대부분이 검정색이므로 배경은 희미한 radial-gradient로 검은색에서 투명하게 변하는 형태입니다.

<img src="/assets/img/2024-08-19-Creating3DeffectsinCSS_7.png" />

마지막으로, 애니메이션을 추가해 봅시다:

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
.rocket:hover img{
  transform: translateY(-40px);
}
.rocket:hover::before {
  opacity: 0.8;
  transform: scale(0.8);
}
```

로켓을 위아래로 움직이고, 그 움직임에 따라 그림자도 변경됩니다. CodePen에서 로켓에 마우스를 올려보세요!

## 3D 텍스트 애니메이션

CSS box-shadow 속성을 사용하여 3D 효과를 가진 텍스트를 만들 수 있습니다. 이 효과를 만들기 위해 여러 개의 그림자 레이어가 필요합니다.

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

여기에 Markdown으로 변환한 내용입니다:

```css
.content {
  position: relative;
  transform: translate(-50%, -50%);
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  perspective: 1500px;
  transform-style: preserve-3d;
}
.content h1 {
  font-size: 5rem;
  font-weight: 600;
  color: #b393d3;
  text-shadow: 1px 4px 1px #480d35, 
               1px 5px 1px #480d35, 
               1px 6px 1px #480d35,/* to create the sides*/
               1px 10px 5px rgba(16, 16, 16, 0.5), 
               1px 15px 10px rgba(16, 16, 16, 0.4),
               1px 20px 30px rgba(16, 16, 16, 0.3); /* real shadow */
  margin: 3px;
  transform: rotateX(15deg) rotateY(-20deg) rotateZ(10deg);
  text-transform: uppercase;
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


![이미지](/assets/img/2024-08-19-Creating3DeffectsinCSS_8.png)

이제 애니메이션을 추가해 봅시다:

```js
.content:hover h1{
  transform: rotateX(2deg) rotateY(0deg) rotateZ(0deg);
  font-size: 4rem;
  transition: 500ms ease-in-out;
}
```

CodePen에서 호버 효과와 함께 3D 텍스트와 틸트 효과를 확인해보세요.


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

## 3D tilt effect

![3D tilt effect](/assets/img/2024-08-19-Creating3DeffectsinCSS_9.png)

Here's the HTML:

```html
<div class="text">
      <h2>Hover on the image</h2>
      <p>It uses a combination of all the rotate operators</p>
      <div class="tilt-img">
        <img src="7.jpg" alt="">
      </div>
    </div>
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

배경 이미지를 사용할 수 있지만 중요한 것은 명확한 부모 요소와 자식 요소가 있는지입니다.

CSS에서 transform 속성을 사용하여 이미지를 기울이고, 호버시에 다시 초기화합니다:

```js
.tilt-img {
  width: 600px;
  height: auto;
  margin-top: 20px;
  perspective: 1000px;
}
.tilt-img img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 10px;
  transform: rotateX(15deg) rotateY(-5deg) rotateZ(1deg);
  box-shadow: 5px 5px 2px rgba(0, 0, 0, 0.5);
  transition: 2s;
}
.tilt-img:hover img {
  transform: none;
  box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.5);
}
```

반대로 회전시킨 후 호버에 기울이거나 transform 값을 변경할 수 있습니다:

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

CSS로 3D 이미지를 기울이는 CodePen을 확인하세요.

## 기울기와 마우스 추적

여기서는 요소를 기울이고 마우스를 올리면 마우스의 움직임을 따라다니게 만들 것입니다. vanilla-tilt.js라는 JavaScript 라이브러리를 사용할 예정입니다.

![이미지](https://res.cloudinary.com/practicaldev/image/fetch/s--who5EYFC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Vanilla-tils-js-3d.gif%3Fw%3D750)

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

HTML로 시작해 보겠습니다. 카드와 일부 텍스트를 만들어봅시다.

```js
<div class="box">
    <div class="elements">
      <h2>Hello!</h2>
      <p>I'm a 3D card</p>
    </div>
    <div class="card">
    </div>
  </div>
```

다음으로 CSS로 스타일을 입혀봅시다.

```js
.box {
  position: relative;
  border-radius: 20px;
  transform-style: preserve-3d;
}
.box .card {
  position: relative;
  background: rgba(255, 255, 255, 0.07);
  width: 300px;
  min-height: 400px;
  backdrop-filter: blur(10px);
  border: 0.5px solid rgba(255, 255, 255, 0.15);
  border-radius: 20px;
  box-shadow: 0 25px 45px rgba(0,0,0,0.05);
}
.elements{
  position: absolute;
  top: 100px;
  left: 50px;
  width: 200px;
  height: auto;
  text-align: center;
  background: transparent;
  transform: translateZ(80px);
}
.elements h2{
  font-size: 3rem;
  font-weight: 600;
  color: #f6d8d5;
  text-transform: uppercase;
}
.elements p{
  font-size: 1.5rem;
  color: #b393d3;
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

카드에 3D 효과를 만드는 데 직접적으로 책임을 지는 CSS 속성은 transform과 transform-style입니다.

먼저, 상자에 적용된 transform-style 속성이 preserve-3d로 설정됩니다. 이는 카드와 중첩된 요소가 이제 3D 공간에 있는 것을 의미합니다. transform 속성은 텍스트의 부모 요소인 class 이름이 .elements에 적용됩니다. 여기서 translateZ(80px)는 텍스트를 뷰어 쪽으로 이동시킵니다.

이 두 속성을 결합하여 깊이의 환상을 만들고, 텍스트는 카드 위에 떠 있는 것처럼 보일 것입니다.

HTML 문서에 Vanilla-tilt CDN을 가져와서 링크하기 위해 cdnjs.com으로 이동하세요. 그런 다음, 기본 사이트로 이동해서 틸트를 제어하는 VanillaTilt.init JavaScript 함수를 복사하십시오. 이것을 닫힌 body 태그 앞에 추가하세요.

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
<script type="text/javascript">
        VanillaTilt.init(document.querySelector(".your-element"), {
                max: 25,
                speed: 400
        });
</script>
```

이제 할 일은 .your-element를 굴절 효과를 받는 요소의 클래스 이름으로 바꾸는 것뿐입니다.

기본적으로 최대 기울기 회전과 전환 속도는 각각 max와 speed로 정의됩니다. 반짝임, 이징, 또는 기울기 방향 및 각도를 추가할 수도 있습니다.

위의 "HELLO" 예제와 동일한 효과를 얻는 방법은 다음과 같습니다:

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
<script>
    VanillaTilt.init(document.querySelector(".box"), {
      max: 10,
      speed: 200,
      easing: "cubic-bezier(.03,.98,.52,.99)",
      reverse: true,
      glare: true,
      "max-glare": 0.1,
    });
  </script>
```

추가 기능 및 효과는 메인 사이트에서 확인할 수 있어요.

여기 CodePen 링크를 통해 3D 틸트 효과가 나타나는 것을 보실 수 있어요.

![이미지](https://res.cloudinary.com/practicaldev/image/fetch/s--CzCXCAyi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://blog.logrocket.com/wp-content/uploads/2024/07/Transform-transform-style-absence.gif%3Fw%3D906)


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

카드와 텍스트가 바로 연결된 것 같이 보이고, 기울기만 남아 있어요. 3D 효과가 포함되면 더 멋지지만요.

이번에는 이 3D 효과를 다른 실용적인 사용 사례로 살펴봅시다.

동일한 카드를 사용하여 호버시에 배경 이미지와 캡션을 추가해 보겠습니다:

```css
.box .card {
  position: relative;
  background: url(/Captions/8.jpg) center;
  background-size: cover;
  background-repeat: no-repeat;
  width: 300px;
  min-height: 400px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 10px;
  box-shadow: 0 25px 45px rgba(0, 0, 0, 0.05);
  z-index: 0;
}

.elements {
  position: absolute;
  top: 60%;
  left: 30%;
  width: 250px;
  height: auto;
  padding: 10px;
  border-radius: 5px;
  text-align: center;
  background: rgba(255, 255, 255, 0.25);
  transform: translateZ(80px);
  z-index: 2;
  opacity: 0;
  transition: opacity 500ms;
}

.box:hover .elements {
  opacity: 1;
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

여기 CodePen에서 호버 시 나타나는 캡션과 함께 3D 이미지 카드의 결과물이 있어요.

## 3D 버튼

버튼은 다양한 모양, 크기 및 목적으로 자주 사용되는 웹 구성 요소입니다. 버튼 유형에 관계없이 모두 하나의 공통점이 있습니다: 작동하려면 클릭해야 합니다. 그러나 실제로 "클릭"된 것을 몇 번 본 적이 있나요?

이것은 사용자가 무언가를 했음을 알 수 있도록 도와주는 작은 마이크로 상호작용이 될 것입니다. 이 3D 효과에 주로 사용할 CSS 속성은 transform 속성입니다.


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

<img src="/assets/img/2024-08-19-Creating3DeffectsinCSS_10.png" />

이 3D 버튼은 뚜렷한 두 부분으로 나뉩니다. "Click Me" 텍스트가 있는 상단과 더 밝은 색상이 적용된 하단 및 측면이 있습니다.

먼저 버튼에 대한 HTML을 시작해보겠습니다:

```js
<button class="btn"><span class="txt">Click Me</span></button>
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

CSS에서는 아래부터 시작합니다. 그것은 클래스 이름이 btn인 버튼입니다:

```js
.btn {
  position: relative;
  background: #17151d;
  border-radius: 15px;
  border: none;
  cursor: pointer;
}
```

다음으로 버튼의 상단 부분이 있습니다:

```js
.text {
  display: block;
  padding: 10px 40px;
  border-radius: 15px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  background: #480d35;
  font-size: 1.5rem;
  font-weight: 500;
  color: #b393d3;
  transform: translateY(-6px);
  transition: transform ease 0.1s;
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

테이블 태그를 마크다운 형식으로 변경해주세요.

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

<img src="/assets/img/2024-08-19-Creating3DeffectsinCSS_11.png" />

안타깝게도 카드의 테두리가 파이어폭스 브라우저에서 애니메이션 중에 일그러질 수 있지만, 그 원인은 명확하지 않습니다. 그 외에는 애니메이션이 부드럽게 실행됩니다. 다양한 브라우저 간의 성능을 비교하고 다양한 테두리 두께를 시도해보세요.

웹이 여러분의 캔버스이고, CSS가 여러분의 붓이 되는 것처럼요. 이 글에서는 변형 속성으로 3D 효과를 생성하는 방법을 배웠습니다. 다른 어떤 효과들을 떠올릴 수 있나요?

## 사용자의 CPU를 너무 많이 사용하는 프론트엔드인가요?

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

웹 프론트엔드가 점점 복잡해지면서 자원 소모가 큰 기능들이 브라우저로부터 더 많은 것을 요구합니다. 프로덕션 환경에서 모든 사용자의 클라이언트 측 CPU 사용량, 메모리 사용량 등을 모니터링하고 추적하는 것에 관심이 있다면 LogRocket을 시도해보세요.

![이미지](/assets/img/2024-08-19-Creating3DeffectsinCSS_12.png)

LogRocket은 웹앱, 모바일 앱 또는 웹사이트에서 발생하는 모든 일들을 기록하는 웹과 모바일 앱용 DVR과 같습니다. 문제가 발생하는 이유를 추측하는 대신 핵심 프론트엔드 성능 지표를 집계하고 보고하며, 사용자 세션과 응용 프로그램 상태를 재생하고, 네트워크 요청을 로그로 기록하며, 모든 오류를 자동으로 확인할 수 있습니다.

웹과 모바일 앱 디버그하는 방식을 현대화하세요 - 무료로 모니터링을 시작해보세요.