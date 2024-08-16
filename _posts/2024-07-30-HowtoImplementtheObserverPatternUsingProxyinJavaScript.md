---
title: "자바스크립트에서 프록시를 사용하여 옵저버 패턴 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-30-HowtoImplementtheObserverPatternUsingProxyinJavaScript_0.png"
date: 2024-07-30 17:27
ogImage: 
  url: /assets/img/2024-07-30-HowtoImplementtheObserverPatternUsingProxyinJavaScript_0.png
tag: Tech
originalTitle: "How to Implement the Observer Pattern Using Proxy in JavaScript"
link: "https://medium.com/javascript-in-plain-english/how-to-implement-the-observer-pattern-using-proxy-in-javascript-d8fdebd04862"
isUpdated: true
---





<img src="/assets/img/2024-07-30-HowtoImplementtheObserverPatternUsingProxyinJavaScript_0.png" />

소프트웨어 개발에서 특히 JavaScript에서 Observer Pattern은 객체 간의 일대다 관계를 정의하는 행동 디자인 패턴입니다. 이 패턴을 사용하면 여러 옵저버 객체가 주제 객체를 청취하고 주제의 상태가 변경될 때 자동으로 통지를받을 수 있습니다. 이 패턴은 이벤트 시스템, 데이터 바인딩 등에서 보편적으로 사용됩니다.

JavaScript에서는 Proxy 개체를 활용하여 Observer Pattern을 구현할 수 있습니다. Proxy 개체를 사용하면 대상 개체에서 수행되는 작업을 가로채고 사용자 정의할 수 있습니다. 이는 속성 액세스, 할당, 나열 및 함수 호출과 같은 작업을 포함합니다.

이 문서에서는 JavaScript에서 Proxy를 사용하여 Observer Pattern을 구현하는 방법을 점진적으로 설명합니다. Observer 클래스를 생성하고 핸들러 개체를 정의하며 관찰 가능한 개체를 생성할 것입니다. 또한, 이 패턴이 적용된 일반적인 프론트엔드 시나리오를 보여줌으로써 Proxy 기반 옵저버 구현을 사용하여 문제를 해결하는 방법을 설명할 것입니다. 마지막으로, 이 문서를 요약하겠습니다.

<div class="content-ad"></div>

# 1. 옵저버 패턴이란?

옵저버 패턴은 객체(주제)가 종속된 객체(옵저버)의 목록을 유지하고 주로 메소드 중 하나를 호출하여 상태 변경 시 알림을 보내는 디자인 패턴입니다. 이 패턴은 분산 이벤트 처리 시스템을 구현하는 데 자주 사용됩니다.

# 2. 프록시를 사용한 옵저버 패턴 구현

## 단계 1: 옵저버 클래스 생성

<div class="content-ad"></div>

먼저, 관찰자(Observer) 클래스를 생성해야 합니다. 이 클래스에는 관찰자를 추가, 제거, 그리고 알림을 보내는 메서드가 포함될 것입니다.

```js
class Observer {
    constructor() {
        this.observers = [];
    }

    addObserver(observer) {
        this.observers.push(observer);
    }

    removeObserver(observer) {
        this.observers = this.observers.filter(obs => obs !== observer);
    }

    notifyObservers(message) {
        this.observers.forEach(observer => observer.update(message));
    }
}

class ConcreteObserver {
    update(message) {
        console.log('Received message:', message);
    }
}
```

이 예제에서 관찰자 클래스는 관찰자 목록을 유지하고 관찰자를 추가, 제거, 그리고 알림을 보내는 메서드를 제공합니다. ConcreteObserver 클래스는 특정 관찰자로, 받은 알림을 처리하는 update 메서드를 구현합니다.

## 단계 2: 핸들러 객체 정의

<div class="content-ad"></div>

다음으로, 관측 가능한 객체에서의 작업을 가로채고 처리하는 핸들러 객체를 정의합니다.

```js
const handler = {
    set(target, property, value, receiver) {
        target[property] = value;
        target.notifyObservers({ property, value });
        return true;
    }
};
```

이 예시에서 핸들러 객체는 관측 가능한 객체에서의 속성 할당 작업을 가로채는 set 메서드를 포함하고 있습니다. 관측 가능한 객체의 속성이 변경될 때마다 핸들러는 모든 관측자에게 알립니다.

## 단계 3: 관측 가능한 객체 생성하기

<div class="content-ad"></div>

그러면 관찰 가능한 객체를 생성하고 Proxy로 래핑합니다.

```js
class Observable extends Observer {
    constructor(target) {
        super();
        return new Proxy(target, handler);
    }
}

const observableObject = new Observable({ name: 'John', age: 30 });
```

이 예제에서 Observable 클래스는 Observer 클래스를 확장하고 대상 객체를 Proxy로 감싸서 속성 작업을 가로채고 처리합니다.

# 3. 프런트엔드 시나리오에서 옵서버 패턴 적용하기

<div class="content-ad"></div>

다음으로, Observer Pattern을 구현하기 위해 Proxy를 사용하는 클래식 프론트엔드 데이터 바인딩 시나리오를 보여드리겠습니다. 간단한 HTML 폼이 있고 양방향 데이터 바인딩을 구현해야 한다고 가정해봅시다.

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Observer Pattern with Proxy</title>
</head>
<body>
    <input type="text" id="nameInput" placeholder="Enter your name">
    <p id="nameDisplay"></p>
    <script>
        // Observer 및 ConcreteObserver 클래스
        class Observer {
            constructor() {
                this.observers = [];
            }
            addObserver(observer) {
                this.observers.push(observer);
            }
            removeObserver(observer) {
                this.observers = this.observers.filter(obs => obs !== observer);
            }
            notifyObservers(message) {
                this.observers.forEach(observer => observer.update(message));
            }
        }
        class ConcreteObserver {
            constructor(element) {
                this.element = element;
            }
            update(message) {
                this.element.textContent = message.value;
            }
        }
        // Handler 객체
        const handler = {
            set(target, property, value, receiver) {
                target[property] = value;
                target.notifyObservers({ property, value });
                return true;
            }
        };
        // Observable 클래스
        class Observable extends Observer {
            constructor(target) {
                super();
                return new Proxy(target, handler);
            }
        }
        // Observable 객체 생성
        const data = new Observable({ name: '' });
        // ConcreteObserver 생성
        const nameDisplayObserver = new ConcreteObserver(document.getElementById('nameDisplay'));
        data.addObserver(nameDisplayObserver);
        // 입력 변경 이벤트 처리
        document.getElementById('nameInput').addEventListener('input', (event) => {
            data.name = event.target.value;
        });
    </script>
</body>
</html>
```

이 예시에서는 Observable 객체인 data를 생성하고 그 name 속성을 입력 필드와 표시 단락에 바인딩합니다. 사용자가 입력 필드에 입력하면 data.name의 값이 변경됩니다. 핸들러는 이 변경을 가로채고 모든 관찰자에게 알립니다. 그리고 관찰자인 nameDisplayObserver가 표시 단락의 내용을 업데이트하여 양방향 데이터 바인딩을 구현합니다.

Proxy를 사용하여 Observer Pattern을 구현함으로써 객체의 속성 작업을 효과적으로 가로채고 처리하여 양방향 데이터 바인딩과 반응적인 업데이트를 가능하게 할 수 있습니다. 이 글에서는 Observer Pattern의 기본 개념을 소개하고 JavaScript에서 Proxy를 사용하여 이를 구현하는 방법을 자세히 설명했습니다. 이 글이 여러분이 프로젝트에서 이 강력한 디자인 패턴을 더 잘 이해하고 적용하는 데 도움이 되기를 바랍니다.

<div class="content-ad"></div>

# 친절한 한국어로 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 작가를 박수로 응원하고 팔로우해 주세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 보기