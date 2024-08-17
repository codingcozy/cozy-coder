---
title: "React 저장소에 리포지토리 및 어댑터 디자인 패턴 적용하기"
description: ""
coverImage: "/assets/img/2024-08-13-BuildingaBetterReactApplicationwithRepositoryandAdapterDesignPatterns_0.png"
date: 2024-08-13 11:12
ogImage: 
  url: /assets/img/2024-08-13-BuildingaBetterReactApplicationwithRepositoryandAdapterDesignPatterns_0.png
tag: Tech
originalTitle: "Building a Better React Application with Repository and Adapter Design Patterns "
link: "https://medium.com/javascript-in-plain-english/building-a-better-react-application-with-repository-and-adapter-design-patterns-3e089f43fbc8"
isUpdated: true
updatedAt: 1723863072096
---


이 기사에서는 함수형 컴포넌트와 훅을 사용하여 React 애플리케이션에 이러한 디자인 패턴을 구현하는 방법을 살펴볼 것입니다. 끝날 때쯤에는 React 코드베이스를 청결하고 유지보수 가능하게 유지하는 방법에 대해 더 많은 이해를 갖게 될 것입니다.

![image](/assets/img/2024-08-13-BuildingaBetterReactApplicationwithRepositoryandAdapterDesignPatterns_0.png)

현대 소프트웨어 개발에서 청결하고 모듈화되며 테스트 가능한 코드를 유지하는 것이 중요합니다. 사용자 인터페이스를 구축하는 인기있는 JavaScript 라이브러리 인 React는 컴포넌트 기반 아키텍처를 장려합니다. 그러나 애플리케이션이 성장함에 따라 데이터 및 API 상호 작용을 관리하는 것은 복잡해질 수 있습니다. 이것이 Repository 및 Adapter 패턴과 같은 디자인 패턴이 필요한 이유입니다.

# 디자인 패턴 이해하기

<div class="content-ad"></div>

# 1. Repository Pattern

Repository Pattern은 데이터 레이어를 추상화하여 도메인 객체에 액세스하는 데 사용되는 컬렉션과 같은 인터페이스를 제공합니다. 이 패턴은 도메인과 데이터 매핑 레이어 간을 중재하여 인메모리 도메인 객체 컬렉션처럼 작동합니다.

![이미지](/assets/img/2024-08-13-BuildingaBetterReactApplicationwithRepositoryandAdapterDesignPatterns_1.png)

# 2. Adapter Pattern

<div class="content-ad"></div>

어댑터 패턴은 호환되지 않는 인터페이스를 함께 작동하도록 허용합니다. 클래스의 인터페이스를 다른 인터페이스로 변환하여 클라이언트가 기대하는 인터페이스로 작동합니다. 이 패턴은 기존 클래스를 소스 코드를 수정하지 않고 다른 클래스와 함께 작동하도록 만드는 데 자주 사용됩니다. 

![이미지](/assets/img/2024-08-13-BuildingaBetterReactApplicationwithRepositoryandAdapterDesignPatterns_2.png)

# 프로젝트 설정하기

먼저 새로운 React 프로젝트를 생성해 봅시다. 이미 설치되어 있지 않다면 Create React App을 설치하고 프로젝트를 설정하세요:

<div class="content-ad"></div>

```js
npx create-react-app react-repo-adapter
cd react-repo-adapter
```

# 프로젝트 구조화

프로젝트를 관심사에 맞게 명확하게 구성하세요. 다음은 제안된 구조입니다:

```js
src/
├── adapters/
│   └── apiAdapter.js
├── components/
│   └── ItemsComponent.js
├── repositories/
│   └── dataRepository.js
├── services/
│   └── apiService.js
├── App.js
└── index.js
```

<div class="content-ad"></div>

# 리포지토리 구현

데이터 조작 작업을 관리하기 위한 데이터 리포지토리를 생성합니다.

src/repositories/dataRepository.js

```js
const DataRepository = (apiAdapter) => ({
    getAllItems: async () => {
        return await apiAdapter.get('/items');
    },

    getItemById: async (id) => {
        return await apiAdapter.get(`/items/${id}`);
    },

    createItem: async (data) => {
        return await apiAdapter.post('/items', data);
    },

    updateItem: async (id, data) => {
        return await apiAdapter.put(`/items/${id}`, data);
    },

    deleteItem: async (id) => {
        return await apiAdapter.delete(`/items/${id}`);
    }
});

export default DataRepository;
```

<div class="content-ad"></div>

# 어댑터 구현하기

HTTP 요청을 처리하기 위한 API 어댑터를 만들어보세요.

src/adapters/apiAdapter.js

```javascript
import axios from 'axios';

const ApiAdapter = (baseURL) => {
    const client = axios.create({
        baseURL: baseURL
    });

    return {
        get: async (url) => {
            const response = await client.get(url);
            return response.data;
        },

        post: async (url, data) => {
            const response = await client.post(url, data);
            return response.data;
        },

        put: async (url, data) => {
            const response = await client.put(url, data);
            return response.data;
        },

        delete: async (url) => {
            const response = await client.delete(url);
            return response.data;
        }
    };
};

export default ApiAdapter;
```

<div class="content-ad"></div>

# 컴포넌트에서 저장소와 어댑터 사용하기

이제 저장소와 어댑터를 사용하여 데이터를 가져와 표시하는 기능 컴포넌트를 만들어 봅시다.

src/components/ItemsComponent.js

```js
import React, { useEffect, useState } from 'react';
import DataRepository from '../repositories/dataRepository';
import ApiAdapter from '../adapters/apiAdapter';

const apiAdapter = ApiAdapter('https://api.example.com');
const dataRepository = DataRepository(apiAdapter);

const ItemsComponent = () => {
    const [items, setItems] = useState([]);

    useEffect(() => {
        const fetchItems = async () => {
            const data = await dataRepository.getAllItems();
            setItems(data);
        };
        fetchItems();
    }, []);

    return (
        <div>
            <h1>Items</h1>
            <ul>
                {items.map(item => (
                    <li key={item.id}>{item.name}</li>
                ))}
            </ul>
        </div>
    );
};

export default ItemsComponent;
```

<div class="content-ad"></div>

# 모두 함께 적용해 보기

마지막으로, 자신의 컴포넌트를 메인 App 컴포넌트에 통합하세요.

src/App.js

```js
import React from 'react';
import ItemsComponent from './components/ItemsComponent';

const App = () => (
    <div>
        <h1>리파지토리와 어댑터 패턴을 활용한 리액트</h1>
        <ItemsComponent />
    </div>
);

export default App;
```

<div class="content-ad"></div>

![image](/assets/img/2024-08-13-BuildingaBetterReactApplicationwithRepositoryandAdapterDesignPatterns_3.png)

# 간단한 영어로 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 반드시 박수를 보내고 작가를 팔로우해 주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기