---
title: "타입에서 타입으로 제네릭을 활용한 타입 생성"
description: ""
coverImage: "/assets/img/2024-08-19-CreatingTypesfromTypesGenerics_0.png"
date: 2024-08-19 02:09
ogImage: 
  url: /assets/img/2024-08-19-CreatingTypesfromTypesGenerics_0.png
tag: Tech
originalTitle: "Creating Types from Types  Generics"
link: "https://medium.com/@msunil037/creating-types-from-types-generics-a355c992e460"
isUpdated: true
updatedAt: 1724033216279
---


유연하고 재사용 가능한 코드

![이미지](/assets/img/2024-08-19-CreatingTypesfromTypesGenerics_0.png)

제네릭은 매개변수를 사용하는 유형입니다. 소프트웨어 엔지니어링의 주요 부분은 정의되고 일관된 API가 있는 구성 요소를 구축하고 재사용하는 것입니다.

오늘의 데이터와 내일의 데이터를 처리할 수 있는 구성 요소는 대규모 소프트웨어 시스템을 구축하는 데 가장 유연한 기능을 제공할 것입니다.

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

이는 사용자들이 이러한 구성 요소를 사용하고 유형을 활용할 수 있도록 합니다.

```js
const names = ['Max', 'Manuel'];
// const names: string[]으로 해석됩니다.
 
const names = [];
// const names: any[]으로 해석됩니다.

const names: Array = [];
//에러: 제네릭 타입 'Array<T>'은 1개의 타입 인자가 필요합니다.

const names: Array<string> = [];
const promise: Promise<string> = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Done');
  }, 2000);
});

promise.then(data => {
  data.split(' ');
})
```

안녕하세요, 제네릭의 세계:-

```js
function identity<Type>(arg:Type): Type {
  return arg;
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

이 함수 호출하는 두 가지 방법: -

```js
let output = identity<string>("myString");
```

여기서는 타입을 수동으로 설정합니다.

```js
let output = identity("myString");
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

여기 컴파일러가 타입을 설정합니다

일반 타입 변수와 함께 작업하기:-

```js
function loggingIdeantity<Type>(arg: Array<type>):Array<Type>{
  console.log(arg.length);
  return arg;
}
```

일반 함수:-

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
function merge(objA, objB){
  return Object.assign(objA, objB);
}
console.log(merge({name : "Sunil"}, {age: 30));
mergedObj.age; // TypeScript에서 오류가 발생합니다.

//해결 방법

function merge(objA:T, objB:U){
  return Object.assign(objA, objB);
}
console.log(merge({name : "Sunil"}, {age: 30));
mergedObj.name;
```

```js
const mergedObj = merge({name: 'Sunil'}, 30);
```

오류가 나지 않을 것이지만 병합되지 않습니다. Object. assign은 두 개의 객체를 병합하려고 하기 때문에 하나는 객체이고 다른 하나는 다른 유형이면 객체 하나만 가져옵니다. 이것이 숫자 30을 위한 오류를 표시하는 제약 조건이 필요한 이유입니다.

```js
function merge<T extends Object, U extends Object>(objA:T, objB:U){
  return object.assign(objA, objB);
}

const mergedObj = merge({name: 'Sunil'}, 30);
console.log(mergedObj);

//Error: Argument of type '30' is not assignable to parameter of type 'object'
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

그냥 또 다른 일반적인 함수:

```js
interface Lengthy{
  length: number;
}

function countAndDescribe<T extends Lengthy>(element: T): [T, string]{
  let descriptionText = "값이 없어요";

  if(element.length === 1){
    descriptionText = "1개의 값이 있어요";
  } else if(element.length > 1){
    descriptionText = element.length + "개의 요소가 있어요.";
  }
  
  return [element, descriptionText];
}
```

```js
function identity<Type>(arg:Type):Type{
  return arg;
}

let myIdeantity:<Type>(arg: Type) => Type = identity;
```

또한 타입 내의 제네릭 타입 매개변수에 대해 다른 이름을 사용할 수도 있습니다. 중요한 것은 타입 변수의 수와 그 타입 변수가 사용되는 방식이 일치하면 됩니다.

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
function identity<Type>(arg:Type):Type{
  return arg;
}

let myIdeantity:<Input>(arg: Input) => Input= identity;
```

우리는 또한 제네릭 타입을 객체 리터럴 타입의 호출 시그니처로 작성할 수 있습니다.

```js
function identity<Type>(arg:Type):Type{
  return arg;
}

let myIdeantity:{<Type>(arg: Type): Type} = identity;
```

제네릭 인터페이스:-

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
interface 제네릭_항등_함수 {
  <타입>(인자: 타입): 타입;
}

function 항등<타입>(인자: 타입): 타입 {
  return 인자;
}

let 내항등: 제네릭_항등_함수 = 항등;
```

```js
interface 제네릭_항등_함수<타입>{
  (인자: 타입): 타입;
}

function 항등<타입>(인자: 타입): 타입 {
  return 인자;
}

let 내항등: 제네릭_항등_함수<number> = 항등;
```

참고: 제네릭 열거형과 네임스페이스를 만드는 것은 불가능합니다.

제네릭 클래스:- 


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

제네릭 클래스는 제네릭 인터페이스와 비슷한 모양을 가지고 있습니다. 제네릭 클래스에는 클래스 이름 뒤에 꺾쇠 괄호(``)로 둘러싸인 제네릭 유형 매개변수 목록이 있습니다.

```js
class Genericnumber<NumType>{
  zeroValue: NumType;
  add: (x:numType, y:NumType) => NumType;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x,y){
  return x + y;
}
```

이것은 GenericNumber 클래스를 매우 직접적으로 사용한 예제이지만, 숫자 타입만 사용하는 것으로 제한되지는 않았다는 것을 알 수 있습니다. 대신 문자열이나 더 복잡한 객체를 사용할 수도 있습니다.

```js
let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function (x, y) {
  return x + y;
};

console.log(stringNumeric.add(stringNumeric.zeroValue, "Test 1"));
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

일반 클래스는 정적 멤버가 클래스의 유형 매개변수를 사용할 수 없으므로 그들은 인스턴스 측면에서만 일반적이다.

또 다른 예시:

```js
class DataStorage<T extends string | number | boolean>{
  private data: T[] = [];
  addItem(item: T){
    this.data.push(item);
  }

  removeItem(item: T){
    if(this.data.indexOf(item) === -1){
      return;
    }
    this.data.splice(this.data.indexOf(item), 1);
  }

  getItems(){
    return [...this.data];
  }
}

const textStorage = new DataStorage<string>();
textStorage.addItem('Sunil');
textStorage.addItem('Muna');
textStorage.removeItem('Sunil');

console.log(textStorage.getItems());
```

일반적인 제한 사항:-

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

```typescript
function loggingIdentity<Type>(arg: Type): Type {
  console.log(arg.length);
}
```

위 코드는 오류를 발생합니다.

```typescript
Property 'length' does not exist on type 'Type'
```

해결책:
```typescript
function loggingIdentity<Type>(arg: Type[]): void {
  console.log(arg.length);
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
인터페이스 LengthWise{
  length: number;
}

function loggingIdentity<Type extends LengthWise>(arg: Type): Type{
  console.log(arg.length);
  return arg;
}
```

```js
loggingIdentity(3);

에러: 'number' 타입의 인수는 'LengthWise' 타입의 매개변수에 할당할 수 없습니다.

loggingIdentity({length: 10, value: 3});
```

제네릭 제약사항에서 매개변수 활용하기:-

아래 예제에서는 obj에 존재하지 않는 속성을 실수로 버리지 않도록 제약을 두어 두 타입 사이에 제약을 둡니다:

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
function getProperty<Type, key extends keyofType>(obj: Type, key: key){
  return obj[key];
}

let x = {a:1, b:2, c:3, d:4};
getProperty(x, "a");

getProperty(x, "m"); //Error: Argument of type "m" is not assignable to a parameter of type '"a" | "b" | "c" | "d"'.
```

제네릭에서 클래스 유형 사용하기:-

TypeScript에서 제네릭을 사용하여 팩토리를 만들 때, 클래스 유형은 생성자 함수로 참조해야 합니다.

```js
function create<Type>(c: {new (): Type}): Type{
  return new c();
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

더 발전된 예시는 프로토타입 속성을 사용하여 생성자 함수와 클래스 유형의 인스턴스 측면 간의 관계를 추론하고 제약을 가하는 데 사용됩니다.

```js
class BeeKeeper{
  hasmask: boolean = true;
}

class ZooKeeper{
  nameTag: string = "Sunil";
}

class Animal{
  numLegs: number = 4;
}

class Bee extends Animal {
  numLegs = 6;
  keeper: BeeKeeper = new BeeKeeper();
}

class Lion extends Animal{
  keeper: ZooKeeper = new ZooKeeper();
}

function createInstance <A extends Animal>(c: new () => A) : A {
  return new c();
}

createInstance(Lion).keeper.nameTag;
createInstance(Bee).keeper.hasMask;
```

위 패턴은 mixin 디자인 패턴을 구동하는 데 사용됩니다.

일반 유틸리티 유형:-

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
인터페이스 CourseGoal{
  title: string;
  description: string;
  completeUntil: Date;
}

function createCourseGoal(title: string, description: string, date: Date){
  let courseGoal: Partial<CourseGoal> = {};

  courseGoal.title = title;
  courseGoal.description = description;
  courseGoal.completeUntil = date;

  return courseGoal as CourseGoal;
}
```

제네릭 매개변수 세부 정보:-

```js
declare function create(): Container<HTMLDivElement, HTMLDivElement[]>;
declare function create<T extends HTMLElement>(element: T): Container<T, T[]>;
declare function create<T extends HTMLElement, U extends HTMLElement>(element: T, children: U[]): Container<t, U[]>;
```

제네릭 매개변수 기본값을 사용하면 다음과 같이 간소화할 수 있습니다.

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
함수 create를 선언합니다 <T extends HTMLElement = HTMLDivElement, U extends HTMLElement[] = T[]> (element?: T, children?: U): Container<T, U>;
```

```js
const div = create();

//const div : Container<HTMLDivElement, HTMLDivElement[]>

const p = create(new HTMLParagraphElement());

//const p : Container<HTMLParagraphelement, HTMLParagraphElement[]>
```

제네릭 매개변수 기본값은 다음 규칙을 따릅니다:

- 타입 매개변수는 기본값이 있는 경우 선택적으로 간주됩니다.
- 필수 타입 매개변수는 선택적 타입 매개변수 뒤에 올 수 없습니다.
- 타입 매개변수의 기본값은 해당 타입 매개변수의 제약 조건을 만족해야 합니다.
- 타입 인수를 지정할 때 필수 타입 매개변수의 인수만 지정해야 합니다. 지정되지 않은 타입 매개변수는 기본값으로 해결됩니다.
- 기본 타입이 지정되었고 인터페이스가 후보를 선택할 수 없는 경우 기본 타입이 추론됩니다.
- 기존의 클래스나 인터페이스 선언과 병합되는 클래스나 인터페이스 선언은 기존 타입 매개변수에 대한 기본값을 도입할 수 있습니다.
- 기존의 클래스나 인터페이스 선언과 병합되는 클래스나 인터페이스 선언은 새로운 타입 매개변수를 도입할 수 있습니다.