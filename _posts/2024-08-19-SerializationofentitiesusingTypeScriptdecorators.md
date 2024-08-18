---
title: "TypeScript 데코레이터를 활용한 엔티티 직렬화"
description: ""
coverImage: "/assets/img/2024-08-19-SerializationofentitiesusingTypeScriptdecorators_0.png"
date: 2024-08-19 02:15
ogImage: 
  url: /assets/img/2024-08-19-SerializationofentitiesusingTypeScriptdecorators_0.png
tag: Tech
originalTitle: "Serialization of entities using TypeScript decorators"
link: "https://medium.com/@k.kharlamow/serialization-of-entities-using-typescript-decorators-99356b13e00a"
isUpdated: false
---


<img src="/assets/img/2024-08-19-SerializationofentitiesusingTypeScriptdecorators_0.png" />

프론트 엔드에서 더 복잡하거나 간단한 비즈니스 로직을 포함하는 애플리케이션을 작성할 때, "풍부한" 모델에서 모든 이 로직을 유지하는 것이 필요해집니다. 예를 들어 사용자 인터페이스에 표시되는 양식과 관련된 많은 종속 속성을 가진 개체의 생성 또는 편집을 처리해야 할 때입니다. 이러한 엔티티 및 해당 하위 엔티티의 상태 변경 핸들러를 응용 프로그램 레이어에 "퍼뜨린다면" 모델의 일관성을 서로 다른 조치, 리듀서, 유효성 검사기에서 쉽게 잃게 될 수 있습니다. 이러한 코드는 읽기, 디버깅 및 유지 관리하기 어려울 것입니다.

한 개체에서 속성 액세스 메서드 및 엔티티 상태를 변경하는 메서드를 호출할 수 있도록 모델 관리를위한 단일 진입점으로 집계 루트 패턴을 사용하여 이러한 엔티티의 불변성을 지원하는 것이 더 쉬워집니다. 그러나 여기에 또 다른 문제가 발생합니다: 직렬화. 예를 들어 특정 저장소 - localStorage, redux store에 전체 엔티티를 저장하거나 백엔드에 전송하여 저장하거나 이벤트로 사용자 인터페이스를 업데이트해야하는 경우가 있습니다. 이러한 경우에는 나중에 저장소에서 쿼리 할 때 엔티티에서 데이터를 압축 할 필요가 있습니다. 이는 페이지에 대한 데이터를 서버 측에서 수집해야하는 프로젝트에서 SSR이 사용되는 경우 특히 중요합니다.

직렬화 문제는 루트 엔티티에 관련된 모든 클래스에 serialize 메서드를 추가하여 "직면"해 결합할 수 있습니다. 이것은 다음과 같이 보일 것입니다:

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
interface Serializable<T> {
  serialize(): T;
}

enum VehicleType {
  Car = 'Car',
  Bus = 'Bus',
  Bike = 'Bike',
}

type SerializedVehicle = {
  readonly id: string;
  readonly name: string;
  readonly type: VehicleType;
  readonly wheelsNum: number;
};

class Vehicle implements Serializable<SerializedVehicle> {
  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly type: VehicleType,
    public readonly wheelsNum: number,
  ) {}

  serialize() {
    return {
      id: this.id,
      name: this.name,
      type: this.type,
      wheelsNum: this.wheelsNum,
    };
  }

  drive() {
    // ...do something
  }

  repair() {
    // ...do something
  }
  
  // ...more methods
}
```

이게 좀, 음. 어색하죠. 특히 중첩된 엔티티가 많은 경우 우울해질 정도로 그렇습니다. 그 중에서도 컬렉션이 포함되어 있을 수 있어서 이를 위한 직렬화 방법도 생각해야 합니다. 너무 많은 부수적인 작업이 필요하죠. 그래서 이 반복되는 로직을 대신 처리해줄 데코레이터를 작성해볼 생각이었습니다. 결국에는 코드가 이렇게 보이면 좋겠습니다:

```js
@serializable
class Vehicle {
  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly type: VehicleType,
    public readonly wheelsNum: number,
  ) {
    super();
  }

  drive() {
    // ...do something
  }

  repair() {
    // ...do something
  }
  
  // ...more methods
}

const car = new Vehicle(
  '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
  'car',
  VehicleType.Car,
  4
);

console.log(car.serialize());

// 예상 출력:
// {
//   id: '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
//   name: 'car',
//   type: 'Car',
//   wheelsNum: 4
// }
```

이와 같은 데코레이터는 serialize() 메서드를 가지고 있지 않은 데코레이트된 클래스에서의 호출을 가로채기 위해 Proxy 객체를 사용할 수 있습니다. Proxy 내부의 구현은 직렬화된 객체의 속성을 순회하고 그들로부터 데이터를 직렬화된 형태로 수집합니다.


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

여기 몇 가지 기술적인 어려움이 있습니다:
- TypeScript에서 객체가 serialize() 메서드를 가지고 있고 반환하는 유형을 알아야 합니다.
- 일부 속성을 직렬화 프로세스에서 제외하고 유연하게 만들고 싶습니다.
- 컬렉션과 같은 반복 가능한 객체도 배열과 같은 것으로 직렬화해야 합니다.
- 중첩된 엔티티도 이 데코레이터를 가지고 있다면 자동으로 직렬화되어야 합니다.

첫 번째 문제는 구현이 없으면 예외를 throw하는 serialize() 메서드가 있는 추상 클래스를 사용하여 해결할 수 있습니다:

```js
abstract class Serializable<T> {
  serialize(): T {
    throw new Error('Method not implemented.');
  }
}

@serializable
class Vehicle extends Serializable<SerializedVehicle> {
  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly type: VehicleType,
    public readonly wheelsNum: number,
  ) {
    super();
  }

  drive() {
    // ...무언가를 수행
  }

  repair() {
    // ...무언가를 수행
  }
}
```

데코레이터에 대한 추가 인수를 사용하여 직렬화 결과에서 일부 속성을 제외할 수 있습니다. 이는 다양한 매개변수 목록을 수용하는 함수로 랩핑하여 데코레이터를 사용하는 방법입니다.

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
function serializable(...propsToExclude: string[]) {
  return function serializableDecorator<T extends { new(...args: any[]): {} }>(SerializableClass: T) {
    return class extends SerializableClass {
      constructor(...args) {
        super(...args);
        
        return new Proxy(this, {
          get(target, prop) {
            if (prop !== 'serialize') {
              return target[prop];
            }
            
            return () => {
              let result: any = {};
              
              Object.keys(target).forEach((key) => {
                if (propsToExclude.includes(key)) {
                  return;
                }
                
                // ...details
              });
            }
          },
        });
      }
    }
  }
}

@serializable('checkExcluded')
class Vehicle extends Serializable<SerializedVehicle> {
  private checkExcluded = 'checkExcluded';

  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly type: VehicleType,
    public readonly wheelsNum: number,
  ) {
    super();
  }

  drive() {
    // ...do something
  }

  repair() {
    // ...do something
  }
}
```

To serialize typed collections into the array, you can add code that checks the object for the Symbol.iterator property:

```typescript
function serializable(...propsToExclude: string[]) {
  return function serializableDecorator<T extends { new(...args: any[]): {} }>(SerializableClass: T) {
    return class extends SerializableClass {
      constructor(...args) {
        super(...args);
        
        return new Proxy(this, {
          get(target, prop) {
            if (prop !== 'serialize') {
              return target[prop];
            }
            
            return () => {
              let result: any = {};
              
              // iterable object case
              if (typeof target[Symbol.iterator] === 'function'
                && typeof target !== 'string'
              ) {
                result = [];

                for (let value of target as unknown as Iterable<any>) {
                  if (!(value instanceof Serializable)) {
                    continue;
                  }

                  result.push(value.serialize());
                }

                return result;
              }
              
              // ...details
            }
          },
        });
      }
    }
  }
}

abstract class Collection<KEY, VALUE, SERIALIZED> extends Serializable<SERIALIZED[]> {
  protected data: Map<KEY, VALUE>;

  protected constructor() {
    super();
  }

  *[Symbol.iterator]() {
    for (const [,item] of this.data) {
      yield item;
    }
  }
}

@serializable()
class VehicleCollection extends Collection<Vehicle['id'], Vehicle, SerializedVehicle[]> {
  constructor(vehicles: Vehicle[]) {
    super();

    this.data = new Map(vehicles.map((vehicle) => [vehicle.id, vehicle]));
  }
}
```

Nested entities can be serialized by the condition of inheritance from the Serializable class. Proxy can be replaced by Object.assign(), which will make the code more concise. Full code of the decorator:

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
import { Serializable } from './Serializable';

export function serializable(...propsToExclude: string[]) {
  return function serializableDecorator<T extends { new(...args: any[]): {} }>(SerializableClass: T) {
    return class extends SerializableClass {
      constructor(...args) {
        super(...args);

        return Object.assign(this, {
          serialize() {
            // iterable object case
            if (typeof this[Symbol.iterator] === 'function') {
              const result = [];

              for (let value of this) {
                if (!(value instanceof Serializable)) {
                  continue;
                }

                result.push(value.serialize());
              }

              return result;
            }

            const result = {};

            Object.keys(this).forEach((key) => {
              if (typeof this[key] === 'function'
                || propsToExclude.includes(key)
                || (typeof this[key] === 'object'
                  && this[key] !== null
                  && !(this[key] instanceof Serializable))
              ) {
                return;
              }

              if (typeof this[key] === 'object'
                && this[key] !== null
                && typeof this[key][Symbol.iterator] === 'function'
                && typeof this[key] !== 'string'
              ) {
                result[key] = [];

                for (let value of this[key]) {
                  if (!(value instanceof Serializable)) {
                    continue;
                  }

                  result[key].push(value.serialize());
                }

                return;
              }

              if (this[key] instanceof Serializable) {
                result[key] = this[key].serialize();

                return;
              }

              result[key] = this[key];
            })

            return result;
          }
        });
      }
    };
  }
}
```

테스트를 확인해보세요:

```js
import { serializable } from './serializableDecorator';
import { Serializable } from './Serializable';

export enum VehicleType {
  Car = 'Car',
  Bus = 'Bus',
  Bike = 'Bike',
}

type SerializedVehicle = {
  readonly id: string;
  readonly name: string;
  readonly type: VehicleType;
  readonly wheelsNum: number;
};

@serializable('checkExcluded')
export class Vehicle extends Serializable<SerializedVehicle> {
  private checkExcluded = 'checkExcluded';
  private checkNotSerializable = Object.create({});

  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly type: VehicleType,
    public readonly wheelsNum: number,
  ) {
    super();
  }

  get checkGetter() {
    return 'test';
  }

  drive() {
    // ...무언가 수행
  }

  repair() {
    // ...무언가 수행
  }
}

abstract class Collection<KEY, VALUE, SERIALIZED> extends Serializable<SERIALIZED[]> {
  protected data: Map<KEY, VALUE>;

  protected constructor() {
    super();
  }

  *[Symbol.iterator]() {
    for (const [,item] of this.data) {
      yield item;
    }
  }
}

@serializable()
export class VehicleCollection extends Collection<Vehicle['id'], Vehicle, SerializedVehicle[]> {
  constructor(vehicles: Vehicle[]) {
    super();

    this.data = new Map(vehicles.map((vehicle) => [vehicle.id, vehicle]));
  }
}

type SerializableStreet = {
  readonly id: string;
  readonly name: string;
  readonly vehicles: SerializedVehicle[],
}

// 중첩된 직렬화 가능한 컬렉션 및 개체 예제
@serializable()
export class Street extends Serializable<SerializableStreet> {
  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly vehicles: VehicleCollection,
    public readonly firstVehicle: Vehicle,
  ) {
    super();
  }
}
```

검증 코드 (테스트를 작성해도 되지만 아마 시각적인 특징이 더 있을 것 같습니다):


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
import {Street, Vehicle, VehicleCollection, VehicleType} from './Vehicle';

const car = new Vehicle(
  '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
  'car',
  VehicleType.Car,
  4
);

console.log(car.serialize());

// 예상 출력:
// {
//   id: '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
//   name: 'car',
//   type: 'Car',
//   wheelsNum: 4
// }

const collection = new VehicleCollection([
  new Vehicle(
    '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
    'car',
    VehicleType.Car,
    4
  ),
  new Vehicle(
    '229ade70-d5cd-4841-a60f-ec8ddf141780',
    'bus',
    VehicleType.Bus,
    8
  ),
  new Vehicle(
    '96587162-9410-48b9-a5c6-89209ed4685c',
    'bike',
    VehicleType.Bike,
    2
  )
]);

console.log(collection.serialize());

// 예상 출력:
// [
//   {
//     id: '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
//     name: 'car',
//     type: 'Car',
//     wheelsNum: 4
//   },
//   {
//     id: '229ade70-d5cd-4841-a60f-ec8ddf141780',
//     name: 'bus',
//     type: 'Bus',
//     wheelsNum: 8
//   },
//   {
//     id: '96587162-9410-48b9-a5c6-89209ed4685c',
//     name: 'bike',
//     type: 'Bike',
//     wheelsNum: 2
//   }
// ]


const street = new Street(
  '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
  'Street Name',
  collection,
  new Vehicle(
    'ed0c0b19-9d54-42e5-b8d3-a4c0b1760781',
    'bike',
    VehicleType.Bike,
    2
  )
);

console.log(street.serialize());

// 예상 출력:
// {
//   id: '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
//   name: 'Street Name',
//   vehicles: [
//     {
//       id: '8247b4f6-13cc-49f3-aac4-e828a2f19c6e',
//       name: 'car',
//       type: 'Car',
//       wheelsNum: 4
//     },
//     {
//       id: '229ade70-d5cd-4841-a60f-ec8ddf141780',
//       name: 'bus',
//       type: 'Bus',
//       wheelsNum: 8
//     },
//     {
//       id: '96587162-9410-48b9-a5c6-89209ed4685c',
//       name: 'bike',
//       type: 'Bike',
//       wheelsNum: 2
//     }
//   ],
//   firstVehicle: {
//     id: 'ed0c0b19-9d54-42e5-b8d3-a4c0b1760781',
//       name: 'bike',
//       type: 'Bike',
//       wheelsNum: 2
//   }
// }
```

그 결과에 대해 아직 고려해야 할 몇 가지 문제가 있습니다:
1. 직렬화된 객체의 유형을 개발자에게 남겨 둡니다.
2. 반복 가능한 객체는 배열로 직렬화되며, 다른 속성은 무시됩니다. 이것은 형식화된 컬렉션을 직렬화하는 방법이지만, 이터레이터와 함께 모든 속성을 직렬화해야 하는 객체에 대해 추가 로직을 고민해야 할 필요가 있습니다.
3. 데코레이터의 구현이 꽤 복잡해 보이며, 리팩토링할 공간이 있을 것으로 생각됩니다.
4. 고려하지 않은 경우가 많이 있을 것으로 생각되며, 필요할 경우 고려할 수 있습니다.
5. 추상 클래스를 사용해 형식 지정하는 해결책은 혼란스럽습니다. 형식 지정은 데코레이터를 통해 구현될 수도 있지만, 어떻게 변형할지 조금은 알 수 없습니다.

이 문제에 대해 준비된 솔루션을 찾으면 이상적이지만, 기사에 제시된 방법은 완벽하지 않지만, 제3자 라이브러리를 사용할 수 없거나 특수한 경우에 코드를 작성해야 하는 경우에 기초로 사용할 수 있는 유용한 방법일 수 있습니다.

Github에서의 코드: https://github.com/BoesesGenie/ts-serializable-decorator


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

해당 주제에서 읽을 만한 것들:
1. 데코레이터 관련:
- TypeScript 데코레이터: https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/#decorators (해당 기사에서는 레거시 데코레이터를 사용함);
- TC39 진행 중: https://github.com/tc39/proposal-decorators (해당 기사에서 사용되지 않음);
2. Proxy 객체 관련:
- https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Proxy
- David Flanagan. JavaScript.The Complete Guide, 7th ed. 14장. 메타프로그래밍
- Nicholas C. Zakas. Understanding ECMAScript 6: JavaScript 개발자를 위한 정식 가이드. 12장. 프록시와 리플렉션 API

PS. 누아르 고양이 탐정은 자신의 누아르 성격을 장식하는 그림자를 드리고 있습니다.
PPS. 타입드 콜렉션에 관한 아이디어는 Ed Ishmukhametov(https://www.linkedin.com/in/ishmukhametoveduard/)에게 감사드립니다.