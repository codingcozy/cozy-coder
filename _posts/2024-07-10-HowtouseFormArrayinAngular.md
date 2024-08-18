---
title: "Angular에서 FormArray 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-HowtouseFormArrayinAngular_0.png"
date: 2024-07-10 00:41
ogImage:
  url: /assets/img/2024-07-10-HowtouseFormArrayinAngular_0.png
tag: Tech
originalTitle: "How to use FormArray in Angular"
link: "https://medium.com/@umitbicici64/how-to-use-formarray-in-angular-cf603c93176b"
isUpdated: true
---

`<img src="/assets/img/2024-07-10-HowtouseFormArrayinAngular_0.png" />`

Angular의 FormArray를 사용하여 폼을 생성하는 방법에 대한 자세한 예제를 아래에서 찾아볼 수 있습니다. 이 예제에서는 사용자가 주소를 동적으로 추가하고 제거할 수 있는 주소 목록 폼을 생성할 것입니다.

단계 1: Angular 프로젝트 생성

먼저, 새로운 Angular 프로젝트를 만들어보겠습니다:

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
ng new form-array-example
```

그런 다음, 방금 만든 프로젝트의 디렉토리로 이동하세요.

```js
cd from-form-array-example
```

단계 2: 반응형 Forms 모듈 추가하기

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

Angular 프로젝트에 ReactiveFormsModule을 추가하려면 app.module.ts 파일을 수정해야 합니다. 다음은 그 방법입니다:

```js
import { ReactiveFormsModule } from "@angular/forms";
```

```js
@NgModule({
  declarations: [AppComponent, AddressFormComponent],
  imports: [BrowserModule, ReactiveFormsModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

단계 3: 폼 구조 생성

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

언더바가 포함된 답변을 다음과 같이 구성할 수 있습니다:

---

언더바를 사용하여 가로선을 만들 수 있습니다.

---

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
import { Component } from "@angular/core";
import { FormArray, FormBuilder, FormGroup, Validators } from "@angular/forms";
```

```js
@Component({
  selector: 'app-address-form',
  templateUrl: './address-form.component.html',
  styleUrls: ['./address-form.component.css']
})
export class AddressFormComponent {
  addressForm: FormGroup;
  constructor(private fb: FormBuilder) {
    this.addressForm = this.fb.group({
      addresses: this.fb.array([
        this.createAddress()
      ])
    });
  }
  createAddress(): FormGroup {
    return this.fb.group({
      street: ['', Validators.required],
      city: ['', Validators.required],
      state: ['', Validators.required],
      zip: ['', Validators.required]
    });
  }
  get addresses(): FormArray {
    return this.addressForm.get('addresses') as FormArray;
  }
  addAddress() {
    this.addresses.push(this.createAddress());
  }
  removeAddress(index: number) {
    this.addresses.removeAt(index);
  }
  onSubmit() {
    console.log(this.addressForm.value);
  }
}
```

Step 5: Creating the Template Structure

여기가 address-form.component.html 컴포넌트입니다.

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
<form [formGroup]="addressForm" (ngSubmit)="onSubmit()">
  <div formArrayName="addresses">
    <div *ngFor="let address of addresses.controls; let i=index" [formGroup]="address">
      <label>
        Street:
        <input formControlName="street">
      </label>
      <label>
        City:
        <input formControlName="city">
      </label>
      <label>
        State:
        <input formControlName="state">
      </label>
      <label>
        ZIP:
        <input formControlName="zip">
      </label>
      <button type="button" (click)="removeAddress(i)">Remove Address</button>
    </div>
  </div>
  <button type="button" (click)="addAddress()">Add Address</button>
  <button type="submit">Submit</button>
</form>
```

지금은 주소를 동적으로 추가하고 제거할 수 있는 양식을 만들었습니다. '주소 추가' 버튼을 클릭하여 새로운 주소 필드를 추가하거나 각 주소 그룹 내 '주소 제거' 버튼을 클릭하여 특정 주소를 삭제할 수 있습니다. 양식이 제출되면(Submit버튼을 누르면), 양식 데이터가 콘솔에 기록됩니다.

도움이 되었으면 좋겠습니다…
