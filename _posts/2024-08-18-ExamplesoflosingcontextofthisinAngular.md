---
title: "Angular에서 this의 컨텍스트를 잃는 예시들"
description: ""
coverImage: "/assets/img/2024-08-18-ExamplesoflosingcontextofthisinAngular_0.png"
date: 2024-08-18 11:01
ogImage: 
  url: /assets/img/2024-08-18-ExamplesoflosingcontextofthisinAngular_0.png
tag: Tech
originalTitle: "Examples of losing context of this in Angular"
link: "https://medium.com/itnext/practical-examples-of-losing-context-of-this-in-angular-ed7035ea85a7"
isUpdated: false
---


이러한 것을 잃어버리는 것은 예전에는 ES6 이전에 일반적인 두통이었습니다. 그런 다음 화살표 함수가 등장하면 과거의 문제가 사라졌습니다. 그렇다면 새로운 JavaScript 개발자는 왜 호출 컨텍스트의 차이를 배우려고 해야 할까요? (또는 상속, 호이스팅, 객체지향 원칙 등) 제가 이유를 보여드릴게요. 그 전에 간단하게 정의를 다시 한 번 살펴봅시다:

![이미지](/assets/img/2024-08-18-ExamplesoflosingcontextofthisinAngular_0.png)

## 함수 내에서 'this' :

JavaScript에서 'this'의 값은 함수가 호출되는 방식에 따라 달라집니다. (런타임 바인딩), 정의된 방식에 따라 달라지지 않습니다. 일반 함수가 객체의 메서드로 호출될 때 (obj.method()), 'this'는 해당 객체를 가리킵니다.

<div class="content-ad"></div>

## '화살표 함수'에서의 'this':

화살표 함수는 'this'를 부모 범위로부터 상속받습니다. 이 동작은 콜백 함수와 문맥을 유지하기 위해 화살표 함수가 특히 유용하다는 것을 의미합니다.

이 서론 다음에 저의 코딩 스타일 외에 이 간단한 Angular 컴포넌트에서 문제점을 찾으시면 얼마나 좋을까요:

```js
export type State = 'FAILED' | 'PERFECT';

@Component({
  ...
  template: `
      <input type="number" [formControl]="value"/> 
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class SelfValidatingFieldComponent {
  protected value = new FormControl(1, this.controlValidator);

  private innerStateThing: State = 'PERFECT';

  private controlValidator(control: AbstractControl): ValidationErrors | null {
    if (control.value < 2 && this.innerStateThing === 'PERFECT') {
      return null;
    }
    this.innerStateThing = 'FAILED';
    return { selfValidating: { value: '아이고, 이게 실패했네요' } };
  }
}
```

<div class="content-ad"></div>

간단한 입력 필드입니다. 여기에는 유효성 검사기가 있습니다. 유효성 검사기를 정의하여 이 필드는 일회용 필드입니다. 한 번 유효하지 않게 만들면 그대로 유지됩니다. 그 이유는 몰라도 되죠. 물론 이 코드는 제대로 작동하지 않을 것입니다.


![Image](/assets/img/2024-08-18-ExamplesoflosingcontextofthisinAngular_1.png)


디버깅해 보면 더욱 나빠집니다. 우리의 유효성 검사기가 두 번 호출됩니다. 먼저 Angular 내부의 FormControl2 클래스에서 this.validator(this)로 호출되고, 그 다음으로 util 순수 함수에서 호출됩니다.

```javascript
function executeValidators(control, validators) {
    return validators.map((validator) => validator(control));
}
```

<div class="content-ad"></div>

innerStateThing이 이를 FormControl2로 보고 두 번째에는 undefined로 볼 때 당신의 의도와는 다르게 동작합니다. 이런 상황이 발생하는 이유가 무엇일까요? new FormControl(1, this.controlValidator)은 우리의 validator를 FormControl 개체에 할당하고, 호출될 때 이를 this로 받게 됩니다. 두 번째 호출에 대해 말씀드리자면,

validator를 수정하고 다시 실패하게 만들어 봅시다. 이제 새로운 문제를 발견했다면 자유롭게 자랑해 보세요 (우리 클래스의 변경 사항을 강조했습니다):

```js
@Component({
  ...
  template: `
    <app-self-validating-field [customValidator]="customValidator"></app-self-validating-field>
  `,
})
export class App {
  private readonly treshold = 1;

  customValidator(val: number): boolean {
    return val < this.treshold;
  }
}

...
export class SelfValidatingFieldComponent {
  customValidator = input.required<CustomValidator>(); // <== 필수 사용자 지정 유효성 검사

  protected value = new FormControl(1);

  private innerStateThing: State = 'PERFECT';

  ngOnInit() {
    this.value.addValidators([this.controlValidatorFactory()]);
  }

  private controlValidatorFactory(): ValidatorFn { // <== 화살표 함수를 반환하는 팩토리, 컨텍스트는 SelfValidatingField입니다
    return (control: AbstractControl): ValidationErrors | null => {
      if (
        control.value < 2 &&
        this.customValidator()(control.value) && // <== 사용자 지정 validator 사용
        this.innerStateThing === 'PERFECT'
      ) {
        return null;
      }
      this.innerStateThing = 'FAILED';
      return { selfValidating: { value: 'oh no, this failed' } };
    };
  }
}
```

이제 controlValidatorFactory는 동작하지만 다시 오류가 발생했습니다.

<div class="content-ad"></div>


![Example of losing context of 'this' in Angular](/assets/img/2024-08-18-ExamplesoflosingcontextofthisinAngular_2.png)

Here's a clearer illustration. I assigned a function from App to a member of SelfValidatingField. Since it's a simple function, when calling it back from SelfValidatingField, even though its definition is in App, "this depends on how a function is invoked," resulting in it being undefined.

Fun fact: if I had used @Input instead of signal input this would be SelfValidatingFieldComponent.

The bottom line is, you never know where your function will be called from, so either:


<div class="content-ad"></div>

- 순수하게 만들어 보세요!
- 화살표 함수를 사용하여 속성 보유 `customValidator = (val: number): boolean =` 를 사용해 보세요!
- 팩토리를 사용해 보세요 (← 보세요, 야생 속의 디자인 패턴들!)

그리고 더 실용적이라고 말하는 친구에게 이 기사를 보내보세요!