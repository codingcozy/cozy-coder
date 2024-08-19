---
title: "Vue 3에서 CSS 변수를 활용한 동적 테마 스위처 만들기"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 01:52
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Creating a Dynamic Theme Switcher in Vue 3 with CSS Variables"
link: "https://medium.com/@fortitudetechnologies/creating-a-dynamic-theme-switcher-in-vue-3-with-css-variables-423d012ac175"
isUpdated: true
updatedAt: 1724033220937
---


사용자 친화적 인터페이스에 대한 수요가 증가함에 따라 애플리케이션이 다크 모드와 라이트 모드와 같은 여러 테마를 지원해야 할 것으로 기대됩니다. 동적 테마 스위처는 사용자 경험을 향상시키는 데에 도움이 되며 애플리케이션에 개인화 수준을 더해줍니다. 이 글에서는 CSS 변수를 사용하여 Vue 3에서 동적 테마 스위처를 구축하는 방법에 대해 살펴보겠습니다. 사용자는 다크 모드, 라이트 모드 및 사용자 정의 테마 사이를 전환할 수 있습니다.

## 단계 1: 테마를 위한 CSS 변수 정의

CSS 변수(또는 CSS 사용자 지정 속성이라고도 함)는 실시간으로 동적으로 업데이트할 수 있기 때문에 테마 스타일을 정의하는 데 적합합니다. 전역 스타일을 저장하는 위치(src/assets/styles.css 또는 선호하는 위치)에서 라이트 및 다크 테마를 위한 몇 가지 CSS 변수를 정의해봅시다.

```css
//styles.css
:root {
    --background-color: #ffffff;
    --text-color: #000000;
    --primary-color: #d2e8de;
}

[data-theme="dark"] {
    --background-color: #090808;
    --text-color: #efefef;
    --primary-color: #374241;
}

[data-theme="grape"] {
    --background-color: #9919c4;
    --text-color: #8c64dc;
    --primary-color: #490e81;
}

[data-theme="lemon"] {
    --background-color: #be9523;
    --text-color: #e5df6c;
    --primary-color: #ea9e2c;
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

이 예시에서는 여러 테마를 정의했습니다. 각 테마는 동일한 세트의 CSS 변수(--배경-색상, --텍스트-색상 및 --기본-색상)를 수정합니다.

이러한 테마를 전역적으로 적용하려면 style 시트를 main.js로 가져와야 합니다.

```js
//main.js

import { createApp } from 'vue'
import App from './App.vue'
import './assets/styles.css'

createApp(App).mount('#app')
```

## 단계 2: 테마 전환 컴포넌트 작성하기

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

src/components 디렉토리 안에 ThemeSwitcher.vue 라는 새 컴포넌트를 만들어보세요:

```js
<script setup>
import { storeToRefs } from 'pinia';
import { useThemeStore } from '@/stores/themeStore.js';
import { watch } from "vue";

const themeStore = useThemeStore();
const { currentTheme } = storeToRefs(themeStore);

const updateTheme = (theme) => {
  themeStore.setTheme(theme);
};

watch(currentTheme, (newTheme) => {
  document.documentElement.setAttribute('data-theme', newTheme);
  localStorage.setItem('theme', newTheme);
});
</script>

<template>
  <div class="theme-switcher">
    <label>
      <input type="radio" name="theme" value="light" @change="updateTheme('light')" :checked="currentTheme === 'light'" />
      라이트
    </label>
    <label>
      <input type="radio" name="theme" value="dark" @change="updateTheme('dark')" :checked="currentTheme === 'dark'" />
      다크
    </label>
    <label>
      <input type="radio" name="theme" value="grape" @change="updateTheme('grape')" :checked="currentTheme === 'grape'" />
      포도
    </label>
    <label>
      <input type="radio" name="theme" value="lemon" @change="updateTheme('lemon')" :checked="currentTheme === 'lemon'" />
      레몬
    </label>

    <div class="card">
      <h2>카드 제목</h2>
      <p>이것은 카드 컴포넌트입니다. 테마 스위처가 스타일을 변경합니다.</p>
      <button class="primary-button">주 버튼</button>
    </div>

    <div class="form-group">
      <label for="name">이름:</label>
      <input type="text" id="name" placeholder="이름을 입력하세요" />
    </div>
  </div>
</template>

<style>
h1 {
  color: var(--primary-color);
  margin-bottom: 20px;
}

.card {
  background-color: var(--background-color);
  border: 1px solid var(--primary-color);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

.primary-button {
  background-color: var(--primary-color);
  color: var(--text-color);
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.primary-button:hover {
  background-color: darken(var(--primary-color), 10%);
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  color: var(--text-color);
}

.form-group input {
  padding: 10px;
  border: 1px solid var(--primary-color);
  border-radius: 4px;
  width: 100%;
  transition: border-color 0.3s ease, background-color 0.3s ease, color 0.3s ease;
}

.form-group input:focus {
  outline: none;
  border-color: darken(var(--primary-color), 10%);
  background-color: lighten(var(--background-color), 5%);
  color: var(--text-color);
}

</style>
```

이 컴포넌트를 사용하면 사용자가 사용자 정의 테마를 선택할 수 있습니다. currentTheme ref는 현재 선택된 테마를 저장합니다. updateTheme 함수는 새로 선택된 테마 이름을 전달받고 currentTheme을 업데이트하며, html 요소의 data-theme 속성도 업데이트하여 해당하는 CSS 변수를 트리거합니다. 또한, 테마가 "다크" 모드로 기본 설정되어 있습니다. 수정을 통해 이러한 설정을 로컬 저장소나 상태 저장소와 함께 사용하여 영구적인 설정을 가능하게 할 수 있습니다.

## 단계 3: 테마 스위처 컴포넌트 사용하기

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

애플리케이션에 테마 스위처를 통합하려면 App.vue나 다른 전역 컴포넌트에 포함시키세요:

```js
//App.vue
<template>
  <div id="app">
    <ThemeSwitcher />
  </div>
</template>

<script setup>
import ThemeSwitcher from './components/ThemeSwitcher.vue';
</script>

<style>
#app {
  background-color: var(--background-color);
  color: var(--text-color);
  padding: 20px;
  transition: background-color 0.3s ease, color 0.3s ease;
}

h1 {
  color: var(--primary-color);
}
</style>
```

이제 배경색, 텍스트 색상 및 기본 색상은 CSS 변수에 의해 제어되며 활성 테마에 따라 동적으로 변경됩니다. 배경 및 텍스트 색상에 대한 전환 효과는 테마를 전환할 때 부드러운 시각적 경험을 제공합니다.

<img src="https://miro.medium.com/v2/resize:fit:1200/1*ae9MMYsflT83Q-GK5T9NbQ.gif" />

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

## 단계 4: 테마 확장 및 사용자 정의

이 설정을 사용하여 새 테마를 추가하거나 기존 테마를 사용자 정의하는 것은 간단합니다. 새 테마를 위한 추가 CSS 변수를 styles.css에 정의하고 ThemeSwitcher.vue 구성 요소를 이러한 새 옵션을 포함하도록 업데이트하면 됩니다.

예를 들어, "blue" 테마를 추가하려면 다음과 같이 styles를 편집합니다:

```js
[data-theme="blueberry"] {
    --background-color: #0d4fde;
    --text-color: #0ec3e3;
    --primary-color: #0c408d;
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

```js
    <label>
      <input type="radio" name="theme" value="blueberry" @change="updateTheme('blueberry')"
             :checked="currentTheme === 'blueberry"/>
      Blueberry
    </label>
```

## 결론

CSS 변수를 활용한 Vue 3에서 동적 테마 스위처를 만드는 것은 사용자 경험을 향상시키는 강력하면서도 간단한 방법입니다. Vue 3의 반응성과 CSS 변수의 유연성을 활용하여 사용자의 기호에 쉽게 적응할 수 있는 매우 사용자 정의 가능한 테마 관리 시스템을 구축할 수 있습니다.

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

이 방식은 귀하의 애플리케이션의 시각적 매력을 향상시키는 것뿐만 아니라 사용자에게 보다 개인화된 경험을 제공하여 현대 웹 개발에서 점차적으로 중요해지고 있습니다. 전체적으로 테마 상태를 관리하고 시스템을 추가 테마로 확장하며 사용자 기호를 세션 간에 지속 가능한 채광하여 Vue 3 애플리케이션에서 견고한 테마 시스템의 기초가 마련되었습니다.