---
title: "코드를 깔끔하게 만드는 필수 React TypeScript 기능 10가지"
description: ""
coverImage: "/assets/img/2024-08-17-10EssentialReactTypeScriptOne-LinersforCleanerCode_0.png"
date: 2024-08-17 00:41
ogImage: 
  url: /assets/img/2024-08-17-10EssentialReactTypeScriptOne-LinersforCleanerCode_0.png
tag: Tech
originalTitle: "10 Essential React TypeScript One-Liners for Cleaner Code"
link: "https://medium.com/@cu.16bcs5007/10-essential-react-typescript-one-liners-for-cleaner-code-6c25ca3fa96c"
isUpdated: true
updatedAt: 1723863484256
---


<img src="/assets/img/2024-08-17-10EssentialReactTypeScriptOne-LinersforCleanerCode_0.png" />

## 1. Optional Chaining과 TypeScript의 Record 유형을 사용하여 중첩된 객체를 평평하게 만들기

복잡한 쿼리:

```js
const userDetails = user?.address?.street;
```

<div class="content-ad"></div>

한 줄 설명:

```js
const userDetails = user?.address?.street;
```

설명: 중첩된 속성에 안전하게 접근하기 위해 선택적 체이닝을 활용합니다.

## 2. 타입 확인 및 조건부 렌더링

<div class="content-ad"></div>

해설: 항목 배열을 통해 매핑하여 각 항목에 대해 `Item /` 구성 요소를 렌더링하고 항목 속성을 전파합니다.

복잡한 쿼리:

```js
if (typeof item === 'string') {
  return <p>{item}</p>;
} else {
  return <div>{item.name}</div>;
}
```

한 줄 설명:

<div class="content-ad"></div>

```js
const RenderItem = ({ item }: { item: string | { name: string } }) => typeof item === 'string' ? <p>{item}</p> : <div>{item.name}</div>;
```

설명: 항목의 유형에 따라 조건부 렌더링을 사용하기 위해 삼항 연산자를 활용했습니다.

## 3. 유형 검사 및 조건부 렌더링을 사용한 매핑

복잡한 쿼리:

<div class="content-ad"></div>

```js
const itemsList = items.map(item => {
  if (item.isActive) {
    return <ActiveItem key={item.id} {...item} />;
  } else {
    return <InactiveItem key={item.id} {...item} />;
  }
});
```

원 라이너:

```js
const itemsList = items.map(item => item.isActive ? <ActiveItem key={item.id} {...item} /> : <InactiveItem key={item.id} {...item} />);
```

설명: 조건부 렌더링을 위해 맵 함수 내에서 삼항 연산자를 사용합니다.

<div class="content-ad"></div>

## 4. TypeScript Generics을 사용하여 필터링 및 렌더링

복잡한 쿼리:

```js
const activeUsers = users.filter(user => user.isActive).map(user => <UserCard key={user.id} user={user} />);
```

한 줄로 표현하기:

<div class="content-ad"></div>

```js
const activeUsers = users.filter((user: User) => user.isActive).map(user => <UserCard key={user.id} user={user} />);
```

설명: TypeScript 유형 주석을 사용하여 한 줄에 필터링과 매핑을 결합합니다.

## 5. 유형 안전한 프로퍼티 액세스 및 렌더링

복잡한 쿼리:

<div class="content-ad"></div>

```js
const getName = (user: User | null) => {
  if (user && user.profile) {
    return user.profile.name;
  }
  return 'Anonymous';
};
```

한 줄로 표현한 버전:

```js
const getName = (user: User | null) => user?.profile?.name ?? 'Anonymous';
```

설명: 옵셔널 체이닝과 널 병합 연산자를 사용하여 속성에 안전하게 접근하고 기본값을 제공합니다.


<div class="content-ad"></div>

## 6. 안전한 유형별 동적 컴포넌트 렌더링

복잡한 쿼리:

```js
const Component = componentsMap[type];
if (Component) {
  return <Component {...props} />;
}
return null;
```

한 줄로 표현:

<div class="content-ad"></div>

```js
const DynamicComponent = ({ type, props }: { type: string, props: any }) => componentsMap[type] ? React.createElement(componentsMap[type], props) : null;
```

해설: 타입 키에 따라 동적으로 컴포넌트를 렌더링하며, 타입 안전성을 유지합니다.

## 7. 목록에서 타입 확인된 속성 액세스

복잡한 쿼리: 

<div class="content-ad"></div>

```js
const names = users.map(user => {
  if (user && user.name) {
    return user.name;
  }
  return 'No Name';
});
```

원 라이너:

```js
const names = users.map(user => user?.name ?? 'No Name');
```

설명: 옵셔널 체이닝과 널 병합 연산자를 활용하여 안전하게 name 속성에 접근하고 대체 값을 제공합니다.

<div class="content-ad"></div>

## 8. 복수 조건 결합 및 렌더링

복잡한 쿼리:

```js
if (isLoggedIn && user && user.isAdmin) {
  return <AdminDashboard />;
} else {
  return <Home />;
}
```

한 줄짜리:

<div class="content-ad"></div>

```js
const Dashboard = () => isLoggedIn && user?.isAdmin ? <AdminDashboard /> : <Home />;
```

- 설명: 논리 AND 연산자를 사용하여 여러 조건을 결합하여 렌더링할 컴포넌트를 결정합니다.

## 9. 배열 필터링과 타입 가드

복잡한 쿼리:

<div class="content-ad"></div>

```js
const validUsers = users.filter(user => {
  if (user && user.isActive) {
    return true;
  }
  return false;
});
```

원 라이너:

```js
const validUsers = users.filter((user): user is ActiveUser => user?.isActive);
```

설명: TypeScript 유형 가드 및 조건부 필터링을 사용하여 원 라이너를 만듭니다.

<div class="content-ad"></div>

## 10. 타입 확인된 조건에 따라 기본값 반환

복잡한 쿼리:

```js
const userRole = user && user.role ? user.role : 'Guest';
```

한 줄로 표현하기:

<div class="content-ad"></div>

```js
const userRole = user?.role ?? 'Guest';
```

설명: 옵셔널 체이닝과 널 병합 연산자를 사용하여 user.role이 정의되지 않은 경우 기본 역할을 제공합니다.

이 예시들은 TypeScript와 React 로직을 보다 간결한 한 줄로 압축하는 방법을 보여줍니다. 이를 통해 타입 안정성과 가독성을 유지할 수 있습니다.