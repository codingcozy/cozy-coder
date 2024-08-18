---
title: "리액트 ToDo 앱과 장고로 세분화된 Full-Stack 개발 마스터하기"
description: ""
coverImage: "/assets/img/2024-08-18-MasteringFull-StackDevelopmentBuildaReactTodoAppwithDjangoinMinutes_0.png"
date: 2024-08-18 11:09
ogImage: 
  url: /assets/img/2024-08-18-MasteringFull-StackDevelopmentBuildaReactTodoAppwithDjangoinMinutes_0.png
tag: Tech
originalTitle: "Mastering Full-Stack Development Build a React Todo App with Django in Minutes"
link: "https://medium.com/@devsumitg/mastering-full-stack-development-build-a-react-todo-app-with-django-in-minutes-e77c75f17d46"
isUpdated: false
---


![이미지](/assets/img/2024-08-18-MasteringFull-StackDevelopmentBuildaReactTodoAppwithDjangoinMinutes_0.png)

이거 봐봐 : ReactJS와 Django 프레임워크를 통합하여 멋진 웹 애플리케이션을 만드는 완벽한 방법

Buy Me a Coffee로 후원하여 개발 지식을 공유하는 제 열정을 지원해주세요. 여러분의 기부는 가치 있는 콘텐츠와 리소스를 만드는 데 도움이 됩니다. 지원해주셔서 감사합니다!

출발해봅시다 🚀

<div class="content-ad"></div>

오늘의 기술 세계에서는 풀 스택 개발이 매우 중요한 기술이며, Django와 React와 같은 강력한 기술을 결합하면 개발자로서 돋보일 수 있습니다.

이 튜토리얼에서는 백엔드로 Django, 프론트엔드로 React (Vite로 구동)를 사용하여 완전히 기능적인 Todo 앱을 만드는 과정을 안내해 드리겠습니다.

최종 결과물을 확인해보세요:

![최종 제품](/assets/img/2024-08-18-MasteringFull-StackDevelopmentBuildaReactTodoAppwithDjangoinMinutes_1.png)

<div class="content-ad"></div>

바로 시작해 볼까요!

## 왜?

ReactJS는 사용자 인터페이스를 구축하는 데 사용되는 인기있는 JavaScript 라이브러리이며, Django는 웹 애플리케이션을 구축하는 데 널리 사용되는 고수준 Python 웹 프레임워크입니다. 이 두 강력한 도구를 연결하면 견고한 웹 애플리케이션이 생성될 수 있습니다.

## 필수 조건

<div class="content-ad"></div>

시작하기 전에 ReactJS와 Django 프레임워크에 대한 기본 지식이 있어야 해요. 또한 머신에 Node.js와 Python이 설치되어 있어야 해요.

[참고: Virtual Environment 사용하기 좋아요.]

## Backend

### 단계 1: 필요한 패키지 설치

<div class="content-ad"></div>

프로젝트와 애플리케이션이 생성되면 몇 가지 패키지를 설치해야 합니다. Django의 경우 Django Rest Framework와 Django Cors Headers를 설치해야 합니다.

Django Rest Framework와 Django Cors Headers를 설치하려면 다음 명령을 실행하세요:

```js
pip install django djangorestframework django-cors-headers
```

## 단계 2: 새 Django 프로젝트 생성

<div class="content-ad"></div>

첫 번째 단계는 새로운 Django 프로젝트를 만드는 것입니다.

Django를 설치한 후, myproject라는 새 프로젝트를 만드세요

```js
django-admin startproject myproject
```

디렉토리를 변경하세요

<div class="content-ad"></div>

```js
cd myproject
```

## 단계 3: 할 일 앱 설정

프로젝트 내에서 할 일 항목을 관리하기 위한 새로운 앱을 생성하세요.

```js
python manage.py startapp todo
```

<div class="content-ad"></div>

## 단계 4: 앱과 패키지 추가하기

settings.py 파일에서 INSTALLED_APPS 목록에 앱과 패키지를 추가하세요.

```js
INSTALLED_APPS = [
    # ...
    # 👇 여기에 설치된 앱을 추가하세요
    'rest_framework',
    'corsheaders',
    'todo',
]

MIDDLEWARE = [
    # ...
    # 👇 이 줄을 여기에 추가하세요
    'corsheaders.middleware.CorsMiddleware',
    # 👇 이 줄을 아래 줄 전에 추가하세요 
    'django.middleware.common.CommonMiddleware',
]

# 👇 이 줄을 여기에 추가하세요
CORS_ORIGIN_ALLOW_ALL = True
```

이 코드 라인은 rest_framework, corsheaders, 그리고 todo 앱을 INSTALLED_APPS 목록에 추가하고, CorsMiddleware를 MIDDLEWARE 목록에 추가하며, 모든 출처에서의 교차 출처 요청을 허용합니다.

<div class="content-ad"></div>

## 단계 5: 모델 정의

todo/models.py 파일에서 할 일 항목을 저장할 Todo 모델을 정의하세요.

```python
from django.db import models

# 여기서 모델을 정의하세요.
class Todo(models.Model):
    title = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title
```

## 단계 6: 직렬화기 정의

<div class="content-ad"></div>

todo 앱 내에서 Todo 모델을 직렬화하기 위해 serializers.py 파일을 생성해보세요.

```python
from rest_framework import serializers
from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = '__all__'
```

클래스 기반 뷰

Django REST Framework에서 클래스 기반 뷰는 API 엔드포인트를 정의하는 강력하고 유연한 접근 방식을 제공합니다. 함수 기반 뷰와는 달리 클래스 기반 뷰는 함수가 아닌 클래스로 정의됩니다. 개발 프로세스를 단순화하고 코드 재사용성을 촉진하는 다양한 기능과 기능을 제공합니다.

<div class="content-ad"></div>

클래스 기반 뷰를 사용하면 모델 인스턴스에 대한 CRUD (생성, 읽기, 업데이트, 삭제) 작업을 다룰 수 있어요.

우리 Book 모델에 대한 클래스 기반 뷰를 구현하는 예제를 살펴보죠:

## 단계 7: 뷰 생성

이제 API 요청을 다룰 뷰를 정의하세요. todo/views.py에서:

<div class="content-ad"></div>


```js
from rest_framework import viewsets
from .models import Todo
from .serializers import TodoSerializer

class TodoViewSet(viewsets.ModelViewSet):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer
```

## 단계 8: URL 생성

Todo 뷰에 대한 URL을 매핑하기 위해 todo/urls.py 파일을 만듭니다.

```js
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import TodoViewSet

router = DefaultRouter()
router.register(r'todos', TodoViewSet)

urlpatterns = [
    path('api/', include(router.urls)),
]
```

<div class="content-ad"></div>

main myproject/urls.py에 todo/urls.py를 포함하세요.

```js
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('todo.urls')),
]
```

## 단계 9: 마이그레이션 생성 및 적용

모델에 가한 변경 사항을 적용하려면 마이그레이션을 생성하고 적용해야 합니다.

<div class="content-ad"></div>

```js
python manage.py makemigrations
python manage.py migrate
```
## 단계 10: 장고 서버 실행

마지막으로, 장고 개발 서버를 실행하세요.

```js
python manage.py runserver
```


<div class="content-ad"></div>

## 단계 11: 브라우저에서 Browsable API 인터페이스에 액세스하기

이제 Django 백엔드는 API를 통해 데이터를 제공할 준비가 되었습니다.

이제 Django 애플리케이션이 실행되고 있으므로 다음 단계를 따라 웹 브라우저를 통해 액세스할 수 있습니다:

1. 원하는 웹 브라우저를 엽니다.
2. 터미널 출력에 제공된 URL을 입력합니다. 일반적으로 다음과 같습니다.
http://127.0.0.1:8000/api/todos/ — 이 URL을 통해 Django REST Framework의 브라우저 API 인터페이스에 액세스할 수 있습니다.

<div class="content-ad"></div>

장고 REST 프레임워크 API와 상호 작용하려면 다음 단계를 수행하십시오:

이러한 클래스 기반 뷰를 사용하여 다음 엔드포인트에서 Todo 모델에 대한 CRUD 작업을 수행할 수 있습니다:

- GET (목록 가져오기): /todos/
- POST (생성): /todos/
- GET (조회): /todos/`pk`/
- PUT (업데이트): /todos/`pk`/
- PATCH (일부 업데이트): /todos/`pk`/
- DELETE (삭제): /todos/`pk`/

장고 REST 프레임워크 API와 상호 작용하려면 위의 단계를 수행하세요.

<div class="content-ad"></div>

I. 할 일 목록:
1. 요청 URL을 http://127.0.0.1:8000/api/todos/로 설정하여 모든 할 일 목록을 가져옵니다.
2. 브라우저에서 API 페이지로 이동한 후, "GET" 버튼을 찾습니다.
3. "GET" 버튼을 클릭하여 할 일 목록을 가져와 표시합니다.

II. 할 일 생성:
1. API 페이지에서 요청 URL이 http://127.0.0.1:8000/api/todos/로 설정되어 있는지 확인합니다.
2. 필요한 책 세부 정보를 입력란에 입력합니다.
3. "POST" 버튼을 클릭하여 할 일 세부 정보를 저장하고 새로운 할 일 항목을 생성합니다.

III. 특정 할 일 가져오기:
1. 웹 브라우저에서 http://127.0.0.1:8000/api/todos/2로 이동합니다 (2를 원하는 할 일 ID로 대체).
2. 이 URL에서 해당 ID를 가진 책의 세부 정보를 표시합니다.

IV. 할 일 업데이트:
1. 요청 URL을 http://127.0.0.1:8000/api/todos/2로 설정합니다 (2를 원하는 책 ID로 대체).
2. 할 일의 갱신된 세부 정보를 입력란에 수정합니다.
3. 변경 사항을 적용하고 할 일 정보를 업데이트하기 위해 "PUT" 버튼을 클릭합니다.

<div class="content-ad"></div>

## 할 일 삭제하기:
1. 요청 URL을 http://127.0.0.1:8000/api/todos/2로 설정합니다 (2를 삭제하려는 할 일의 ID로 대체합니다).
2. "DELETE" 버튼을 클릭하여 시스템에서 할 일을 제거합니다.

위 단계를 따라 하면 Django REST Framework API를 브라우저 API 인터페이스를 통해 효과적으로 상호 작용할 수 있습니다. 이 인터페이스는 테스트를 단순화하고 책 항목에 대한 CRUD 작업을 직관적으로 수행할 수 있는 방법을 제공합니다.

# 프론트엔드

## 단계 1: Vite로 리액트 프론트엔드 설정하기

<div class="content-ad"></div>

Vite는 현대 웹 개발에 최적화된 빠른 빌드 도구입니다.

React 프론트엔드를 설정하기 위해 Vite를 사용해봅시다.

```js
npm create vite@latest react-todo-app --template react
```

이제, React를 선택하고 JavaScript를 선택하세요.

<div class="content-ad"></div>

지금은 디렉토리를 변경해주세요:

```js
cd react-todo-app
```

프로젝트 디렉토리 안으로 들어가면 ToDo 앱을 위한 필요한 컴포넌트를 생성할 것입니다.

모든 패키지와 axios를 설치해주세요

<div class="content-ad"></div>

```js
npm 설치
npm axios 설치
```

## 단계 2: 컴포넌트 폴더 생성

주 프로젝트 루트 폴더의 src 폴더 안에 components폴더를 생성하세요.

## 단계 3: 다크 테마로 스타일링하기


<div class="content-ad"></div>

```js
@import url('https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@0,300;0,400;0,700;0,900;1,300;1,400;1,700;1,900&display=swap');  

/* 일반 스타일 */
body {
  font-family: 'Merriweather', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #121212;
  color: #e0e0e0;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* 컨테이너 스타일링 */
.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  background-color: #1e1e1e;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

/* 헤더 스타일링 */
.container h1 {
  font-size: 2rem;
  margin-bottom: 20px;
  color: #bb86fc;
  text-align: center;
}

/* 폼 스타일링 */
form {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #333;
  border-radius: 4px;
  background-color: #2e2e2e;
  color: #e0e0e0;
}

input[type="text"]::placeholder {
  color: #a1a1a1;
}

button {
  padding: 10px;
  border-radius: 4px;
  background-color: #bb86fc;
  color: #121212;
  font-weight: bold;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #a06bff;
}

/* 할 일 아이템 스타일링 */
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #2a2a2a;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.15);
}

.todo-item h3 {
  margin: 0;
  font-size: 0.9rem;
  color: #e0e0e0;
}

.todo-item input[type="checkbox"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
}

/* 스크롤바 스타일링 */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background-color: #121212;
}

::-webkit-scrollbar-thumb {
  background-color: #bb86fc;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background-color: #a06bff;
}


.right-todo {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

<div class="content-ad"></div>

```js
import './App.css'

import TodoList from './components/TodoList';

function App() {
  return (
    <>
      <div className="container">
            <h1>React ToDo App with Django Backend</h1>
            <TodoList />
        </div>
    </>
  )
}

export default App
```

App 컴포넌트는 기본 구조를 설정하는 함수형 컴포넌트로, 제목과 주요 TodoList 컴포넌트를 포함합니다. 이는 사용자 인터페이스의 기초를 제공하고 할 일 항목을 관리하는 논리와 연결합니다.

components/AddTodo.jsx 파일을 생성하고 엽니다

```js
import React, { useState } from 'react';
import axios from 'axios';

// 새로운 todo 항목을 추가하는 컴포넌트
const AddTodo = ({ onNewTodo }) => {
    // 새로운 todo 제목의 입력 값을 추적하는 상태
    const [title, setTitle] = useState('');

    // 폼 제출을 처리하는 함수
    const handleSubmit = (e) => {
        e.preventDefault(); // 기본 폼 제출 동작 방지

        // 현재 제목과 기본 완료 상태가 거짓인 새로운 todo 객체 생성
        const newTodo = { title, completed: false };

        // 서버에 새로운 todo를 추가하기 위해 POST 요청 보내기
        axios.post('http://127.0.0.1:8000/api/todos/', newTodo)
            .then(res => {
                // 새로 생성된 todo를 부모 컴포넌트로 전달
                onNewTodo(res.data);
                // 입력 필드 지우기
                setTitle('');
            })
            .catch(err => console.error(err)); // 콘솔에 발생한 에러 기록
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                value={title}
                onChange={(e) => setTitle(e.target.value)} // 입력이 변경될 때 제목 상태 업데이트
                placeholder="제목"
                required // 필수 입력 필드로 설정
            />
            <button type="submit">Todo 추가</button> {/* 폼 제출을 위한 버튼 */}
        </form>
    );
};

export default AddTodo;
```

<div class="content-ad"></div>

AddTodo 컴포넌트는 사용자가 새로운 할 일 항목을 입력하고 백엔드로 제출하는 간단한 인터페이스를 제공합니다. 이는 양식 제출을 관리하고 상태를 업데이트하며 API 상호작용을 효과적으로 처리합니다.

components/TodoItem.jsx 파일을 생성하고 열어서 아래 코드를 추가하세요.

```js
import React from 'react';
import axios from 'axios';

const TodoItem = ({ todo, onUpdate, onDelete }) => {
    const handleCheckboxChange = () => {
        const updatedTodo = { ...todo, completed: !todo.completed };

        // 서버의 할 일을 업데이트하기 위해 PUT 요청을 보냅니다.
        axios.put(`http://127.0.0.1:8000/api/todos/${todo.id}/`, updatedTodo)
            .then(res => {
                onUpdate(res.data); // 업데이트를 부모 컴포넌트에 알립니다.
            })
            .catch(err => console.error(err));
    };

    const handleDelete = () => {
        // 서버에서 할 일 삭제를 위해 DELETE 요청을 보냅니다.
        axios.delete(`http://127.0.0.1:8000/api/todos/${todo.id}/`)
            .then(() => {
                onDelete(todo.id); // 삭제를 부모 컴포넌트에 알립니다.
            })
            .catch(err => console.error(err));
    };

    return (
        <div className={`todo-item ${todo.completed ? 'completed' : ''}`}>
            <h3>{todo.title}</h3>
            <div className="right-todo">
                <input
                    type="checkbox"
                    checked={todo.completed}
                    onChange={handleCheckboxChange}
                />
                <button onClick={handleDelete}>삭제</button>
            </div>
        </div>
    );
};

export default TodoItem;
```

TodoItem 컴포넌트는 개별 할 일 항목을 표시하고 업데이트 또는 삭제할 수 있는 옵션을 제공하도록 설계되었습니다. 이는 Axios를 사용하여 Django 백엔드에 HTTP 요청을 보내서 할 일 항목의 상태를 관리합니다. 완료된 항목은 가로줄로 표시되고 작업을 업데이트하고 삭제하기 위한 사용자 상호작용을 제공합니다.

<div class="content-ad"></div>

```js
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import TodoItem from './TodoItem';
import AddTodo from './AddTodo';

// 할 일 목록을 관리하는 주요 컴포넌트
const TodoList = () => {
    // 할 일 목록을 저장하는 상태
    const [todos, setTodos] = useState([]);

    // 컴포넌트가 마운트될 때 서버에서 할 일 목록을 가져옴
    useEffect(() => {
        axios.get('http://127.0.0.1:8000/api/todos/')
            .then(res => setTodos(res.data)) // 가져온 할 일로 상태 업데이트
            .catch(err => console.error(err)); // 에러 발생 시 콘솔에 로그 출력
    }, []); // 의존성 배열이 비어있어 처음 렌더링 후 한 번만 실행

    // 새로운 할 일을 추가하는 함수
    const handleNewTodo = (newTodo) => {
        setTodos([...todos, newTodo]); // 새로운 할 일을 목록에 추가
    };

    // 기존 할 일을 업데이트하는 함수
    const handleUpdateTodo = (updatedTodo) => {
        // ID를 기준으로 업데이트할 할 일을 찾아서 교체
        const updatedTodos = todos.map(todo =>
            todo.id === updatedTodo.id ? updatedTodo : todo
        );
        setTodos(updatedTodos); // 새로운 목록으로 상태 업데이트
    };

    // 할 일을 삭제하는 함수
    const handleDeleteTodo = (id) => {
        // 특정 ID를 가진 할 일을 필터링하여 제거
        const filteredTodos = todos.filter(todo => todo.id !== id);
        setTodos(filteredTodos); // 필터링된 목록으로 상태 업데이트
    };

    return (
        <div>
            <h1>할 일 앱</h1>
            {/* 새로운 할 일을 추가하는 컴포넌트 */}
            <AddTodo onNewTodo={handleNewTodo} />
            {/* 각 할 일 항목을 렌더링하는 TodoItem 컴포넌트 사용 */}
            {todos.map(todo => (
                <TodoItem
                    key={todo.id} // 각 할 일 항목을 식별하기 위한 고유 키
                    todo={todo}
                    onUpdate={handleUpdateTodo} // 업데이트 이벤트 핸들러 전달
                    onDelete={handleDeleteTodo} // 삭제 이벤트 핸들러 전달
                />
            ))}
        </div>
    );
};

export default TodoList;
```

TodoList 컴포넌트는 CRUD(Create, Read, Update, Delete) 작업을 수행하여 할 일 항목 목록을 관리하는 React 컴포넌트입니다.

- 상태 관리: useState 훅을 사용하여 할 일 목록을 유지합니다.
- 데이터 가져오기: 초기 렌더링시 axios를 사용하여 서버에서 할 일 데이터를 가져와 상태를 업데이트합니다.
- 이벤트 핸들러: 새로운 할 일 추가(handleNewTodo), 기존 할 일 업데이트(handleUpdateTodo), 할 일 삭제(handleDeleteTodo)를 위한 함수를 포함합니다.
- 렌더링: 제목을 표시하고, 새 할 일을 생성하는 AddTodo 컴포넌트를 포함하며, 각 할 일 항목을 렌더링하기 위해 todos 배열을 매핑하고 각각의 TodoItem 컴포넌트를 렌더링하며 필요한 핸들러를 props로 전달합니다.

<div class="content-ad"></div>

전반적으로, TodoList는 할 일 항목과 백엔드 서버를 동기화하는 데 완벽한 인터페이스를 제공합니다.

# 단계 4: 프로젝트 실행

다음 명령어를 사용하여 장고 애플리케이션을 실행하십시오:

```js
python manage.py runserver
```

<div class="content-ad"></div>

리액트 프로젝트를 실행하세요

```js
npm run dev
```

웹 브라우저를 열고 http://localhost:5173/ 으로 이동하세요.

다음과 같이 할 일 앱이 표시됩니다,

<div class="content-ad"></div>

결과,

<img src="/assets/img/2024-08-18-MasteringFull-StackDevelopmentBuildaReactTodoAppwithDjangoinMinutes_2.png" />

YouTube에서 전체 데모 비디오를 확인해보세요:

# 결론

<div class="content-ad"></div>

ReactJS와 Django 프레임워크를 연결하는 것은 웹 애플리케이션을 만들 때 강력한 조합이 될 수 있습니다. 이 튜토리얼에서 안내한 단계를 따라가면 이 두 도구를 연결하고 견고한 웹 애플리케이션을 구축할 수 있습니다. 원활한 통신을 위해 React 앱과 Django API 간 CORS 오류를 처리하기 위해 필요한 패키지를 설치해야 합니다.

ReactJS와 Django 프레임워크를 연결하는데 도움이 되는 많은 튜토리얼과 자료가 온라인에 제공됩니다. 필요한 패키지를 설치하는 것과 CORS 오류를 처리하는 것이 중요합니다. 또한, 교차 출처 요청과 관련된 문제를 피하기 위해 CORS 오류를 처리해야 합니다. 이러한 고려 사항을 염두에 두면 두 강력한 도구를 자신있게 연결하고 견고한 웹 애플리케이션을 만들 수 있습니다.

이상입니다! ReactJS(Frontend) + Django 프레임워크(Backend)를 사용하여 Todo 애플리케이션을 성공적으로 만들었습니다.

좋은 코딩 되세요!

<div class="content-ad"></div>

만약 블로그에서 제공된 정보에 관한 질문이나 의견이 있으면 언제든지 연락해 주세요. 다시 한번 읽어주셔서 감사합니다!

## 자료

- Django 문서
- React 문서
- Vite 문서
- Django Rest Framework 문서
- Axios 문서
- Django에서 CORS 처리하는 방법
- React에서 CORS 처리하는 방법