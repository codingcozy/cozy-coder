---
title: "Angular 점진적으로 구축하는 완전 동적이고 유연한 리액티브 폼 사용하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_0.png"
date: 2024-07-13 00:12
ogImage: 
  url: /assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_0.png
tag: Tech
originalTitle: "Angular: Build gradually, your fully dynamic, flexible reactive form"
link: "https://medium.com/gitconnected/angular-build-gradually-your-fully-dynamic-flexible-reactive-form-0fce96a6a051"
---


역동적 양식에는 동적 구성 요소가 포함되지 않은 1종과 동적 구성 요소가 포함된 1종이 있음을 알고 계시나요? 여기에서 두 가지 모두에 대해 배울 수 있습니다. Angular 동적 양식을 만드는 방법에 대한 견고한 이해를 얻게 되며, 기타 다양한 Angular 주제에 대해도 좋은 이해가 가능해질 것입니다.

![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_0.png)

# 소개

Angular에서 완전히 동적이고 유연한 반응형 양식을 만드는 학습을 위해 코드 구현이 단계별로 실용적이면서 쉽게 따라갈 수 있는 포스팅입니다. 이 게시물의 첫 번째 부분은 동적 구성 요소가 없는 메타데이터 배열을 기반으로 한 간단한 동적 양식을 만드는 것부터 시작하여, 각 양식 요소에 대해 동적 구성 요소를 구현하는 두 번째 부분까지 이어지고, 마지막으로 Angular Material 라이브러리를 사용하여 UI를 꾸미는 것까지 포함됩니다. 모든 단계별 커밋이 포함된 완전한 예제 리포지토리가 제공됩니다.

<div class="content-ad"></div>

이 게시물은 초기에 Todd Motto의 게시물을 참조하여 영감을 받았습니다. 코드 구현은 Angular 16(16.2.12)을 기반으로 하고 모듈 기반 컴포넌트를 사용했습니다. 그러나, 독립형 컴포넌트를 기반으로 한 Angular 17 구현에 대한 포괄적인 게시물이 하나 더 있습니다. 따라서 독자는 두 게시물을 읽어 보고 나중에 새로운 Angular 17 기능들을 비교하며 볼 수 있을 것입니다. Angular 17 게시물은 아래에서 찾아볼 수 있습니다:

독자가 Angular의 기본 개념(컴포넌트, 서비스, 옵저버블 등)과 특히 반응형 폼과 관련된 주제들(formbuilder, formgroup 등)에 이미 어느 정도 익숙하다고 가정합니다. 또한 이 게시물에서는 편집기/IDE 도구로 VS Code를 사용할 예정입니다.

만약 처음부터 새로운 Angular/Material 프로젝트를 시작하고 싶다면, 제 다른 게시물을 살펴보시는 것이 도움이 될 것입니다:

더 좋은 방법은 처음 커밋을 얻기 위해 repo의 첫 번째 커밋을 받아 보시는 것입니다. 받으셨다면 "npm install"을 실행하여 필요한 패키지들도 설치해야 합니다. 설명이 아래에 제공되고, 그 후에는 이 게시물의 나머지 부분을 따라가며 동적 폼을 단계별로 구현해 나가시면 됩니다.

<div class="content-ad"></div>

# 초기 저장소

초기 저장소에는 Angular의 표준인 "app.module.ts" 파일과 "AppComponent"이 포함되어 있습니다. 또한 프로젝트에서 사용되는 모든 Angular Material 모듈을 모아놓은 "material.module.ts"라는 별도의 모듈을 사용합니다. 그리고 간단한 헤더(툴바)를 제공하는 "HomeComponent"("home" 폴더)를 포함했습니다. 이러한 모듈과 컴포넌트는 거의 모든 Angular 프로젝트에서 "표준"으로 간주되지만, 이 초기 저장소에 포함된 나머지 구성 요소, 서비스, 파일 등은 아래에 나와 있습니다.

![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_1.png)

제품에 대한 동적 양식을 구축할 계획이며, 해당 필드의 초기값을 얻을 수 있는 항목입니다. 데모를 위해 몇 가지 샘플 데이터가 포함된 외부 JSON 파일을 사용할 것이며, 실제 데이터베이스에서 데이터를 가져오는 대신 사용할 것입니다. 초기 데모 데이터는 프로젝트의 "assets" 하위 폴더에 위치한 "items.json" 및 "categories.json" 파일에서 제공됩니다. 실제로 항목 집합과 이후 카테고리 집합의 정보를 사용할 것입니다. 이는 항목과 카테고리 필드를 해당 엔티티에 맞추어야 한다는 것을 의미합니다. 따라서 "IItem" 및 "ICategory"라는 두 인터페이스가 필요합니다. 각각의 파일인 "iitem.ts" 및 "icategory.ts"는 "dataObjects" 하위 폴더에 위치해 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_2.png" />

시작 값에 대한 데이터를 검색하려면 서비스가 필요합니다. 초기 예제 데이터 서비스는 "DataService"이며 "data.service.ts"에서 찾을 수 있습니다. 이 서비스에는 "getItems" 및 "getCategories"에 대한 2개의 데이터 검색 메서드와 경고 메시지를 발생시키는 매우 간단한 오류 처리기가 포함되어 있습니다.

선택한 항목의 데이터(필드 값)를 검색할 수 있어야 하므로 하나의 필드와 버튼으로 구성된 매우 간단한 양식을 구현했습니다. 이것이 "RequestDataComponent"("request-data" 하위 폴더)입니다.

그러나 사용자가 요청한 다른 ID 값에 대해 어떻게 응답해야 할까요? 작업을 수행할 다른 서비스가 필요합니다. 이것이 "DataChangeService"이며 "data-change.service.ts"에 있습니다. 이 서비스는 Subjects("setItem" 및 "setCategory")를 사용하여 사용자가 ID 값의 변경을 트리거(또는 트리거할 수 있음)할 때마다 작동합니다. 그런 다음 이 서비스의 옵저버블("getItem" 및 "getCategory" 메서드)을 구독하여 사용자가 입력한 ID 값들을 모니터링하고 적절한 데이터(예: 레코드)를 검색하여 그에 따라 대응할 수 있습니다.

<div class="content-ad"></div>

👉 저장소의 첫 번째 커밋을 여기서 다운로드하고 "npm install"을 실행하는 것을 잊지 마세요. 그 후에 저장소를 실행하면 앱의 초기 화면을 볼 수 있어요:

![앱 초기 화면](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_3.png)

이제 동적 컴포넌트 없는 동적 폼에 대해 먼저 다뤄볼 준비가 되었어요.

# A. 동적 컴포넌트 없이 동적 폼

<div class="content-ad"></div>

동적 형식에 대한 초기 접근 방식에서 동적 형식을 구현하는 단계를 진행하겠습니다. 이 개념은 공식적으로 동적 형식 구축 예시로 제시된 소개와 거의 유사합니다. 저는 '배우면서 하는' 방식으로 구현을 제시하여 왜 이것이 필요한지 쉽게 이해할 수 있도록 하려고 노력할 것입니다. 그러면 여러분들은 공식 예시를 더 잘 이해할 수 있을 것입니다. 공식 예시에서 클래스를 사용하지만, 저는 객체를 선호합니다. 왜냐하면 처음에는 매우 적은 객체 속성을 사용할 수 있기 때문이고, 그런 다음 단계마다 왜 추가하고 사용해야 하는지 설명할 수 있기 때문입니다. 물론, 필요 인터페이스 및 사용자 정의 유형을 제공하여 객체를 '보호'할 것입니다.

형식을 구현할 것이므로, 먼저 형식 구성 요소가 필요합니다. 예를 들면:

```js
ng g c form --skip-tests=true --module=app
```

여기서 첫 번째 근본적이면서 중요한 개념은 이 형식이 나중에 형식-컨테이너 구성 요소의 역할을 할 것이라는 것입니다. 두 번째로, 우리가 field/controls 속성을 가진 객체 배열을 사용함으로써 반응적 형식을 구축할 것입니다. 이러한 field/controls 객체 배열을 가지고 있으면, 각 컨트롤 객체를 추가하고이 배열을 반복하여 우리의 반응적 형식을 구축할 수 있습니다. 많은 경우에 '컨트롤'과 '필드'라는 용어를 서로 교환해서 사용할 것입니다.

<div class="content-ad"></div>

## 모든 필드/컨트롤에 대한 공통 기본 속성

저희 양식의 모든 컨트롤에 대해 공통으로 적용되는 몇 가지 속성부터 시작해 보죠. 예를 들어:

- controlType

이는 컨트롤의 기본 유형을 정의해야 합니다. 예를 들어: input, select, checkbox, button 등이 있을 수 있습니다.

<div class="content-ad"></div>

- controlName

이 컨트롤을 반응형 폼 그룹 배열에 추가할 때 사용할 식별 이름입니다.

- fieldLabel

이 컨트롤에 대한 중요한 정보를 제공하기 위해 사용할 텍스트/문자열입니다. 예: 입력 필드의 레이블 또는 버튼 텍스트로 사용됩니다.

<div class="content-ad"></div>

- inputType

필드의 성격을 좀 더 구체화하는 데 사용되어야 합니다. 예: "입력" 유형의 필드의 경우, 이 속성은 '텍스트', '숫자', '비밀번호' 등의 값을 가질 수 있습니다. 버튼인 경우 '제출' 값을 가져야 합니다.

다음 배열에서 우리는 폼 필드 중 처음 2개만 정의합니다:

```js
const ItemsFormFields = [

  {
    controlType: 'input',
    controlName: 'itemName',
    fieldLabel: '아이템 이름:',
    inputType: '텍스트',

  },
  {
    controlType: 'button',
    controlName: 'submitButton',
    fieldLabel: '제출:',
    inputType: '제출',
  },

]
```

<div class="content-ad"></div>

## 폼 컴포넌트

지금은 이 배열을 "form.component.ts" 파일 내에 넣을 것입니다. 그 이후에는 FormBuilder 서비스를 주입하고 각 컨트롤을 추가하여 폼 (폼 그룹)을 쉽게 생성할 수 있습니다. 폼 필드 배열을 반복하면서 다음과 같이:

```js
const fbGroup = this.formBuilder.group({}); 
this.formFields.forEach((field) => { 
  fbGroup.addControl(field['controlName'], new FormControl("")); 
}); 
this.dynFormGroup = fbGroup;
```

아래에서 FormComponent에 대한 전체 예제 코드를 찾을 수 있습니다:

<div class="content-ad"></div>

기본 선택기를 "dyn-form"으로 변경했습니다.

동일한 "반복" 로직은 HTML 폼 템플릿에서 이전과 동일하게 사용할 수 있습니다. 필드 객체 배열을 반복하고 ngFor 및 ngSwitch와 같은 표준(내장) Angular 구조 지시문을 사용하여 HTML 코드를 모양을 만들 수 있습니다. 다음은 그 방법입니다:

"request-data.component.scss"에 간단한 스타일이 추가되었습니다. 마지막으로 "request-data.component.html"에 폼 컴포넌트 선택기 'dyn-form'을 추가했습니다.

출력은 다음과 같습니다:

<div class="content-ad"></div>


![Image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_4.png)

👉 Find the 2nd Commit of the repo [here](link_here).

## Adding a new form field/element

Now it is very easy to add a new form field. We can do that just by adding an object to the form fields array. E.g. we can just add like this:


<div class="content-ad"></div>


![image1](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_5.png)

And the result is:

![image2](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_6.png)

Note, that we have also adjusted the style sheet/html, to arrange the fields vertically. But, mind that the style sheet is not a goal here.


<div class="content-ad"></div>

👉 해당 저장소의 3번째 커밋을 찾을 수 있습니다.

## 초기 값 설정

이제 우리는 형식 필드에 일부 초기 값들을 표시하기 위해 코드를 약간 변경할 수 있습니다. 이를 위해 ItemsFormArray의 각 객체에 하나의 속성을 더 추가할 수 있습니다. 예를 들어, 초기값이라는 이름의 속성을 추가할 수 있습니다. 이 속성은 배열의 각 필드 객체에 대해 필수적일 필요가 없습니다. 예를 들어, 버튼 필드에는 이 속성을 빠뜨릴 수 있습니다.

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_7.png" />

<div class="content-ad"></div>

그러면 우리는 폼의 생성된 컨트롤에 초기값을 할당하는 코드를 변경할 수 있어요. 우리 목적을 위해, 우리는 setFormValues()와 같은 새로운 메소드를 사용할 수 있어요.

Angular 반응형 폼에서는 폼 컨트롤에 값을 설정하는 몇 가지 방법을 사용할 수 있어요. 전체 폼 그룹에 대해 setValue 대신에 각 폼 컨트롤에 대해 patchValue를 사용하는 것이 더 나을 것 같아요. 이것은 setValue 메소드가 모든 폼 컨트롤을 설정해야 하기 때문에 오류가 발생할 수 있기 때문이에요. 따라서 우리 경우에는 setFormValues() 메소드를 사용하여 폼 컨트롤에 초기값을 입력하기 위해 patch를 사용했어요.

```js
setFormValues(): void {
    for(let control in this.dynFormGroup.controls){
      this.formFields.forEach((field) => {
        if(field['controlName'] === control){
          this.dynFormGroup.controls[control].patchValue(field['initialValue'])
        }
      });
      console.log(">===>> " + control + " - " + this.dynFormGroup.controls[control].value);
    }
  }
```

우리는 ngOnInit() 메소드 내에서 우리 폼을 초기화한 후에 setFormValues() 메소드를 호출할 수 있어요. 아래에서 결과를 볼 수 있어요:

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_8.png" />

중요: 폼 템플릿의 폼 버튼 요소에 ngDefaultControl 속성이 추가되었습니다,

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_9.png" />

에러 메시지 "Error: NG01203: No value accessor for form control name: 'submitButton'."가 나오지 않도록 하기 위해 추가되었습니다.

<div class="content-ad"></div>


![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_10.png)

👉 레포지토리에서 4번째 커밋을 찾아보세요.

## 폼 필드 속성 표준화

지금까지 우리는 객체 배열에서 폼 필드 속성을 정의하는 표준 방식을 사용하지 않았습니다. 따라서 앞으로의 속성 정의 및 사용에 일관성을 유지하기 위해 정규화할 때가 되었습니다. 이를 위해 인터페이스를 사용할 수 있습니다. 또한 배열을 폼에서 분리하고 모두에 대해 별도의 파일을 사용하는 것이 좋습니다: 속성 정의를 위한 인터페이스 및 필드 배열. 사실, 이들을 앱의 dataObjects 폴더에 넣을 것입니다.


<div class="content-ad"></div>

초기 IFormField.ts

초기 itemFormFields.ts

위에서 주목한 것처럼, initialValue 속성은 주석 처리되어 있습니다. 이는 get-data 컴포넌트(및 관련 데이터 서비스)를 통해 값을 가져오기를 원하기 때문입니다.

## 가져온 데이터로부터 폼 컨트롤 값 설정

<div class="content-ad"></div>

그래서 원하는 것은 검색한 데이터 개체(예: 데이터베이스에서의 레코드)의 값들을 ItemsFormFields 배열의 각 필드에 대한 initialValue 속성에 전달하는 것입니다. 이를 위해 ItemsFormFields 배열의 각 객체를 검색한 데이터(예: '레코드')의 해당 필드/열에 연결해야 합니다. 이런 작업을 하려면 인터페이스에 다른 속성을 추가해야 하며, 그런 다음 ItemsFormFields 배열에서 사용할 수 있습니다. 새로운 속성의 이름은 원하는 대로 지을 수 있지만, 의미가 잘 전달되는 이름을 사용하는 것이 좋습니다. 예를 들어 dbColumn, dataColumn 등을 사용할 수 있습니다. 여기서는 'dataField'라고 이름을 지어 보겠습니다.

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_11.png" />

이제 IItem 인터페이스에서 '데이터 필드'를 사용하여 ItemsFormFields 배열을 업데이트할 수 있습니다:

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_12.png" />

<div class="content-ad"></div>

실제로 할 일은 RequestDataComponent에서 가져온 아이템 객체의 값을 얻어오고(DataService를 통해 'Get It' 버튼을 클릭할 때), FormComponent의 일치하는 폼 컨트롤 값으로 설정하는 것이에요.

RequestDataComponent 템플릿에서는 FormComponent 템플릿 선택기를 호스팅하기 때문에, 부모-자식 관계를 설정해서, RequestDataComponent로부터 가져온 아이템 객체를 FormComponent로 @Input으로 전달할 수 있어요.

## DataChangeService 사용

그러나 이를 위해 두 컴포넌트 간에 공유 서비스로 DataChangeService를 사용하는 것이 좋아요. 이미 초기부터 DataChangeService를 가지고 있었다는 것을 기억해봐요. 그러면 DataChangeService를 FormComponent에 주입하고, ngOnInit 라이프사이클 훅 메서드에서 getItem() observable을 구독할 수 있어요.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_13.png)

값을 할당하기 위해서는 값을 업데이트할 때마다(사실, 사용자가 RequestDataComponent의 'Get It' 버튼을 클릭한 후에) 'updateFormFieldsInitialValues()'와 같은 새로운 메소드를 만들어서 해당 값을 할당할 수 있습니다. 그리고 'updateFormFieldsInitialValues()'를 호출한 후에 'setFormControlValues()'를 호출하여 form controls의 값을 설정해야 합니다. 업데이트된 form 컴포넌트는 다음과 같습니다:

그리고 결과는 예상한 대로입니다:

![Image](https://miro.medium.com/v2/resize:fit:1400/0*1k4fIKUN-nWRVhjf.gif)


<div class="content-ad"></div>

👉 저장소에서 지금까지 5번째 커밋을 찾으려면 여기를 클릭해주세요.

## 선택 필드 추가하기

다음 단계는 양식에서 드롭다운 컨트롤을 위한 선택 필드를 추가하는 것입니다. 이를 위해 ItemsFormFields 배열에 새로운 필드 오브젝트를 추가하고 controlType 값을 'select'로 설정해야 합니다. 예시:

![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_14.png)

<div class="content-ad"></div>

우리가 몇 가지 변경 사항을 안내하는 몇 가지 사항이 있습니다. 먼저, 'select' 폼 요소는 'input'이 아닌 'select' HTML 태그를 기반으로 하기 때문에 inputType은 누락될 수 있으며, 따라서 IFormField 인터페이스에서 선택 사항으로 만들 수 있습니다. 다음으로 선택할 수 있는 옵션 값에 대한 사항입니다. 이를 위해 IFormField 인터페이스에 배열 형식의 또 다른 속성을 추가할 수 있습니다. 예를 들어 "options?: any[]"와 같이 추가하고 ItemsFormFields 배열에 추가된 객체에서 사용할 수 있습니다.

어떤 유형의 배열이든 유연하지만 일관성을 유지해야 합니다. 따라서 더 나은 접근 방식은 다른 사용자 지정 인터페이스를 사용하는 것입니다. 일반적으로 이는 키-값 쌍 객체여야 하므로 키와 옵션 값에 대해 적어도 2개의 속성을 정의해야 합니다. 그러나 미래에 유용할 수 있는 몇 가지 더 많은 선택 사항 속성을 추가할 수 있습니다. 이렇게 하여 'IFormOptions' 인터페이스를 추가하고 options 속성 유형으로 사용할 수 있습니다. 예를 들어 "options?: IFormOptions[]":을 IFormField 인터페이스에서 사용할 수 있습니다. 그런 다음 두 인터페이스가 모두 포함된 IFormOptions.ts 파일을 사용합니다.

그리고 ItemsFormFields 배열에 추가된 객체는 다음과 같습니다:

<div class="content-ad"></div>


![Image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_16.png)

카테고리를 데이터 서비스에서 반환받아 배열에 채우려고 의도적으로 빈 배열을 남겨 두었습니다. 하지만 다시 말하자면, 선택된 항목의 값을 업데이트하기 위해 DataChangeService를 사용할 것입니다.

이제 해 봅시다. 먼저 FormComponent 템플릿을 수정하여 "select" 요소를 포함하는 ng switch-case를 추가해야 합니다.

![Image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_17.png)


<div class="content-ad"></div>

```js
. . .
<div *ngSwitchCase="'select'">
  <div class="form-field">
     <label>{field.fieldLabel}</label>
     <select [formControlName]="field.controlName" [id]="field.controlName">
     <option *ngFor="let opt of field.options" [value]="opt.optionKey">{opt.optionValue}</option>
      </select>
  </div>
</div>    
. . .
```

위의 HTML 조각은 select 요소를 표시합니다. 그러나 선택한 키 값을 폼 컨트롤 값으로 반환하며, 우리의 경우에는 카테고리 ID 번호입니다. 또한, 이 초기 구현은 한 가지 옵션만 선택하는 것을 고려하고 있습니다. 나중에 "다중 선택"으로 변경하는 방법에 대해서는 나중에 알아보겠습니다.

다음으로 FormComponent 클래스에서 DataService를 주입해야 합니다(Constructor를 통해). 그런 다음, select 필드의 옵션을 가져와 할당하기 위한 메서드를 사용할 것입니다. 우리는 여러 개의 select 필드가 있을 수 있기 때문에, 어떤 것을 업데이트할 지를 명시하는 것이 좋습니다. 이를 위해 해당 controlName 속성의 값을 사용할 수 있습니다. 다음 메서드 "updateOptions()"를 사용하여 작업을 수행할 수 있습니다:

```js
. . .
  updateOptions(cotrolName: string) {
    this.dataServise.getCategories().subscribe((categories: ICategory[]) => {
      //console.log('>===>> updateItem() - categories', categories);
      let options: IFormOptions[] = [];
      categories.forEach((category: ICategory) => {
        options.push({optionKey: category.categoryId, optionValue: category.categoryName});
      });
      this.formFields.forEach((field) => {
        if( field.controlName === cotrolName && field.controlType === 'select') {
          field.options = options;
        }
      });

      this.changeService.setCategories(categories);
    });
  }
. . .
```

<div class="content-ad"></div>

이 번역은 한 번만 이루어지는 전화통화여야 하며, 이 방법은 ngOnInit 라이프사이클 훅(양식 초기화 전)에서 호출할 수 있습니다.

```js
. . . 
this.updateOptions('itemCategories'); 
. . .
```

하지만 원한다면 데이터 변경이벤트를 구독하여 카테고리 변경 사항을 계속 업데이트할 수도 있습니다. 여기서는 그럴 필요는 없습니다.

아래에서 결과를 맛볼 수 있습니다:

<div class="content-ad"></div>


<img src="https://miro.medium.com/v2/resize:fit:1400/0*zKEEhcbfK0pAs6kU.gif" />

👉 so far 코드를 찾아보세요 (저장소의 6번째 커밋).

## Select/Dropdown 요소를 다중 선택 가능하게 만들고 검색된 값(value) 중 하나를 미리 설정하세요

여기서 더 필요한 단계 하나는 드롭다운 요소를 미리 선택된 값으로 설정하는 것입니다. 이는 폼의 선택 드롭다운 요소를 '다중 선택 가능'으로 만들어야 한다는 뜻입니다. 처음에는 간단해 보이지만, 해야 할 첫 번째 일은 'multiple' 속성을 설정하는 것입니다:


<div class="content-ad"></div>


![Image 1](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_18.png)

This seems to be working fine:

![Image 2](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_19.png)

However, there is a tricky point hidden here. If a fetched item has any assigned categories, these should be found in the item property "categoryNames" (see the IItem interface). So, we can iterate through them, and for every match in optionValues, we can set the respective 'isOptionSelected' property to 'true'. We can do so by modifying the updateFormFieldsInitialValues() method.


<div class="content-ad"></div>

여기가 까다로운 부분입니다. multiple 속성을 적용하면 false여도 여전히 존재합니다. 예를 들어 multiple=false 입니다. 이는 반응형 폼이 select 컨트롤에 대한 값을 배열로 항상 예상하도록 만들어 단일 값이 아닌 경우에는 initialValue가 적용되지 않습니다. 이 문제를 해결하려면 initialValue 속성의 유형을 배열도 허용하도록 변경해야 합니다. 예를 들어 'initialValue?: any | any[];'로 변경해야 합니다. 그런 다음, updateFormFieldsInitialValues() 메서드를 다음과 같이 변경하면 됩니다: 

그런 다음, setFormControlValues() 메서드는 이전과 마찬가지로 폼 컨트롤 값 할당을 다루게 됩니다.

여기에 몇 가지 개선 사항이 있습니다. 우리가 항상 다중 선택 폼 엘리먼트를 사용하지 않기 때문에 IFormField 인터페이스에 더미ultipleOptions?: boolean;'와 같은 추가적인 선택 속성을 추가로 제어할 수 있고, 이것을 Objects(e.g. the ItemsFormFields) 배열에서 사용할 수 있습니다.

마지막으로, 사용자가 하나 이상의 옵션을 선택할 수 있는 경우에 대비하여 유저에게 관련 정보를 제공하기 위해 HTML 코드를 개선할 수도 있습니다. 이를 위해 'promptText?: string;'와 같은 다른 속성을 추가하고 사용할 수 있습니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_20.png" />

ItemsFormFields 배열에서:

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_21.png" />

‘select’ 요소에 대한 업데이트된 HTML은 다음과 같습니다:

<div class="content-ad"></div>


. . .
*ngSwitchCase="'select'"
    .form-field
        label {field.fieldLabel}
        select [formControlName]="field.controlName" [id]="field.controlName" [multiple]="field['multipleOptions']"
            option [ngValue]="null" disabled {field['promptText']}
            *ngFor="let opt of field.options"
                option [ngValue]="opt.optionKey" {opt.optionValue}
. . . 


The result is as it is expected:

![image](https://miro.medium.com/v2/resize:fit:1400/0*OV-CRdmWFtgdTMGU.gif)

👉 Find the so far code (7th Commit of the repo), here.


<div class="content-ad"></div>

## 체크박스 추가하기

체크박스는 "input" 패밀리에 속하기 때문에 다음과 같이 하나의 필드 오브젝트를 추가할 수 있습니다:

![checkbox](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_22.png)

이것만으로도 체크박스가 나타나게 됩니다. 하지만, 아마 이미 초기값을 true로 설정한 것을 눈치채셨을 것입니다. 이는 UI에 "기본" 초기값이 적용되어 있어서 어떠한 데이터도 가져오지 않은 상태에서도 나타나는 것이 문제되지 않습니다.

<div class="content-ad"></div>

어떤 "기본" 초기값을 설정하려면, 폼 그룹에 새로운 컨트롤을 추가할 때 initializeForm() 메서드를 약간 변경해야 합니다. 지금까지 우리는 아무 값도 없이 새로운 컨트롤을 추가해 왔습니다:

```js
fbGroup.addControl(field.controlName, new FormControl(""));
```

하지만 이제 다음과 같이 변경할 수 있습니다:

```js
fbGroup.addControl(field.controlName, new FormControl((field.initialValue !== undefined && field.initialValue !== null) ? field.initialValue : ''));
```

<div class="content-ad"></div>

마침내, 체크박스가 'checked' 속성을 사용하기 때문에 'input' 필드의 경우에는 템플릿에 추가해야 합니다. 따라서

업데이트된 'input' 요소에 대한 HTML은 다음과 같습니다:

```js
. . .
<div *ngSwitchCase="'input'">
    <div class = "form-field">
        <label>{field.fieldLabel}</label>
        <input [formControlName]="field.controlName" [id]="field.controlName" [type]="field.inputType" [checked]="field.initialValue === true" >
    </div>
</div>
. . .
```

결과는 다음과 같습니다:

<div class="content-ad"></div>

<img src="https://miro.medium.com/v2/resize:fit:1400/0*64P8PCcroayExJ9F.gif" />

데모 목적으로 'isItemEnabled' 데이터 필드를 IItem 인터페이스와 /assets/items.json 파일의 데이터에 추가했습니다. 또한 데모 목적으로 ItemsFormFields 배열의 itemId 필드에 초기값으로 999를 설정했습니다.

👉 레포지토리의 8번째 커밋을 찾으려면 이 링크를 클릭하세요.

## 라디오 버튼 그룹 추가

<div class="content-ad"></div>

이전의 체크박스 경우와 마찬가지로 라디오 버튼 그룹 또한 'input' 패밀리에 속합니다. 따라서 우리는 다음과 같이 하나의 라디오 필드 객체를 추가할 수 있습니다:

![라디오 버튼 그룹 이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_23.png)

이것은 괜찮지만, 라디오 버튼 그룹을 위한 원시 HTML 코드는 약간 특별합니다. 사실, 각 옵션은 라벨-인풋 태그 쌍이 필요합니다. 그리고, 우리는 체크된 라디오 버튼을 설정하기 위해 checked 속성도 사용해야 합니다 (이전에 체크박스에서 했던 것과 같이).

'input' 요소에 대한 HTML의 라디오 버튼 케이스를 구별하고 렌더링하기 위해 ngIf — ngIfElse 쌍을 ng-template 내장 지시어와 함께 사용할 수 있습니다.

<div class="content-ad"></div>

위와 같이 HTML 템플릿은 다음과 같습니다 ( 'input' 요소에 대한 섹션에 주목해주세요 ):

그다음으로, FormComponent 클래스 코드와 특히 updateFormFieldsInitialValues() 메소드에서 일부 조정이 또한 필요합니다. 다시 말해, 이것은 항목이 가져올 때 사전 선택된 라디오 버튼을 설정하는 데 필요합니다.

updateFormFieldsInitialValues()는 다음과 같이 됩니다:

결과:

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:1400/0*mbBaWwMznEN68eK2.gif)

👉 해당 레포지토리의 9번째 커밋을 찾아보세요.

## datetimepicker 요소 추가

input HTML form-element 태그는 UI에서 날짜, 시간 또는 날짜 및 시간 값을 가져오기 위해 3가지 유형을 제공합니다. 여기서는 날짜와 시간을 사용할 것입니다. 여기서 사용할 유형은 "datetime-local"입니다. 따라서 아래와 같은 하나의 객체만 ItemsFormFields 배열에 추가해도 기본 datetime(날짜 및 시간) 피커로 작업을 수행하는 데 충분합니다:

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_24.png)

우리는 이를 사용할 것입니다. 우리 아이템의 itemCrTimestamp 데이터 필드는 초를 포함한 날짜 및 시간 스탬프 값을 가져옵니다. 예시: “itemCrTimestamp”: “2024–02–01 07:25:20”. 그러나 여기서 기본 datetime 피커는 초 선택을 제공하지 않습니다.

이 작업을 수행하는 데 충분합니다. 다른 코드 조정이 필요하지 않습니다! 결과는 아래와 같이 나타납니다:

![image](https://miro.medium.com/v2/resize:fit:1400/0*4qbbem8ycdlpanpj.gif)

<div class="content-ad"></div>

👉 해당 레포지토리의 마지막 커밋(10번째 커밋)을 찾아보세요.

# 리팩토링!

## 관심사를 나누다...

지금까지는 잘 진행되고 있습니다. 하지만 그렇게 좋지 않아요. 우리는 코드를 조금 엉망으로 두어 개발 프로세스를 가속화하고 기능(예: 폼 엘리먼트/컨트롤이 추가된 후의 구체적 결과를 보고)를 확인했어요. 문제는 FormComponent가 폼 컨트롤을 생성하는 것 외에도 다른 도메인 로직 작업을 수행한다는 것입니다. 예를 들어 데이터(DataService를 통해 항목 객체와 카테고리를 가져오기)를 가져오고 formFields 배열의 객체를 업데이트하는 작업 등이죠. 이것들은 FormComponent의 역할이 아닙니다. FormComponent는 formFields 배열에서 폼 컨트롤을 생성하고 이 객체 배열에 대한 변경 사항이 있을 때만 업데이트하는 것에만 집중해야 합니다. 따라서 리팩토링을 통해 우리의 컴포넌트와 서비스가 "관심사를 나누는" 원칙을 더 잘 따를 수 있습니다.

<div class="content-ad"></div>

그럼, 지금까지 어떻게 코드를 개선했는지 살펴보겠습니다.

**FormComponent**

FormComponent에서 도메인 로직이 모두 제거되었습니다. FormComponent는 현재 DataChangeService의 "getFormFields" observable에만 subscribe하여 formFields 배열을 얻습니다. DataService 및 ItemFomFields 상수는 FormComponent에게 알려지지 않습니다. IFormField 인터페이스만 필요합니다. "updateOptions()" 및 "updateFormFieldsInitialValues()" 메서드가 DataChangeService로 이동되었습니다. "itemChangeSubscription"은 "formFieldsSubscription"으로 이름이 변경되었습니다(어떤 Form Fields 배열 서비스라도 사용할 수 있음을 강조하기 위함).

**FormComponent (form.component.ts)**

<div class="content-ad"></div>

The RequestDataComponent

RequestDataComponent은 여전히 DataService에 액세스합니다. 이유는 카테고리 배열을 초기화하고 "Get It" 버튼을 클릭할 때마다 항목을 가져와야 하기 때문입니다. 그런 다음 DataChangeService를 업데이트합니다. 카테고리는 한 번만 필요하지만 초기 옵션 배열 설정 시에 필요하기 때문에 GetDataComponent에서는 초기화 단계에서 카테고리를 업데이트합니다.

그러나 여기서 전체 도메인 로직을 제거할 수 있습니다. 이를 위해 RequestDataComponent는 이제 DataChangeService(이제 ItemsFormFieldsService로 변경)만 다룰 수 있습니다. 따라서 RequestDataComponent는 ItemsFormFieldsService에만 접근하여 사용자가 항목 ID 값을 변경할 때마다 항목 ID를 전달합니다. 이를 위해 ItemsFormFieldsService의 "setItemId()" 메서드에 액세스합니다.

따라서 "getItem()", "getCategories()", "updateItem()", updateCategories()" 등과 같은 모든 관련 메서드 및 구독 해제 메서드 등을 제거할 수 있습니다. 이제 해당 메서드들은 무의미합니다.

<div class="content-ad"></div>

게다가 "가져오기" 버튼과 따라서 "onFormSubmit()" 메서드도 제거할 수 있습니다. 이를 위해 ngOnInit 라이프사이클 후크 내에서 FormGroup의 valueChanges를 구독함으로써 수행할 수 있습니다. 사용자가 Item id 값을 변경할 때마다, ItemsFormFieldsService의 "setItemId()"가 그에 맞게 업데이트됩니다.

요청 데이터 구성요소 (request-data.component.ts)

데이터 변경 서비스 - ItemsFormFieldsService

데이터 변경 서비스는 도메인 모델 (예: DataService)과 비즈니스 객체 모델을 설명하는 메타데이터 간의 "다리"가 되어야 합니다 ("IFormField", "ItemFomFields"와 같은). 따라서 업데이트된 메타데이터 집합을 제공하기 위해 사용되어야 합니다. Items에 관한 데이터에 사용될 것이므로 더 나은 이름은 "ItemsFormFieldsService"입니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_25.png)

“updateOptions()” 및 “updateFormFieldsInitialValues()” 메서드가 FormComponent에서 개인 메서드로 이동되었습니다. 이들 메서드는 분할 및 리팩토링되었습니다. ItemsFormFieldsService는 (업데이트된) formFields 배열만 제공하기 위해 하나의 BehaviorSubject만 사용합니다. 실제로 FormComponent와의 유일한 "접촉"점인 ItemsFormFieldsService는 ngOnInit() 라이프사이클 훅 메서드에서 구독하고 있으며 formFields 배열의 변경 사항에 대해 업데이트를 유지합니다. 따라서 formFields 배열은 개체의 동적 formFields 배열로 간주될 수 있습니다. 따라서 itemIdChange$$ 및 categories$$는 더 이상 사용되지 않으며 getItem() 및 getCategories() 메서드에서 observable로 노출되지 않기 때문에 제거되었습니다. 또한 다른 속성/변수 또는 메서드에 대한 명칭 변경도 이루어졌습니다.

또한 DataService가 ItemsFormFieldsService에 주입되었습니다. 따라서 ItemsFormFieldsService는 이와 처리하는 책임이 전적으로 있게 되었습니다. ItemsFormFieldsService는 삽입된 DataService를 사용하여 생성자에서 카테고리를 한 번만 가져옵니다. 그런 다음 formFields 배열의 "itemCategories" 선택 필드/요소의 값(옵션)을 업데이트하기 위해 개인 “setFormFieldSelectOptions()” 메서드를 호출합니다.

말했듯이, 새로운 공개 메서드 “setItemId()”는 RequestDataService에 의해 Item ID가 변경될 때마다 액세스됩니다. “setItemId()” 메서드는 또한 DataService에 액세스하여 ID (itemId)가 업데이트될 때마다 IItem 개체를 가져옵니다. 그런 다음 formFields 배열을 업데이트하기 위해 개인 (및 리팩토링된) “updateFormFieldsInitialValues()” 메서드를 호출합니다.

<div class="content-ad"></div>

ItemsFormFieldsService(items-form-fields.service.ts)

IFormField 인터페이스 및 ItemFormFields 배열, 그리고 기타 업데이트 사항

일관성을 유지하기 위해 IFormField.ts 파일이 업데이트되었습니다. "standardInputType"이라는 특수/사용자 정의 유형이 추가되었는데, 이는 표준 HTML "input" 태그와 "type" 속성 유형의 용도입니다. 또한, IFormField 인터페이스에는 readOnly, optionsSize, minValue, maxValue 및 stepValue와 같은 더 많은 속성이 추가되었습니다.

ItemsFormFields 배열에 새로운 객체(필드)가 추가되었습니다. "itemId", "itemDescription" 및 "itemModelYear"입니다. 또한, ItemsFormFields 배열의 다른 속성에도 수정이 가해져 해당 속성을 추가된 속성을 사용하여 어떻게 설정할 수 있는지 보여줍니다.

<div class="content-ad"></div>

IFormField 인터페이스 (IFormField.ts)

ItemFormFields 배열 (itemFormFields.ts)

FormComponent의 HTML 템플릿 또한 그에 맞게 변경되었습니다.

FormComponent의 HTML 템플릿 (form.component.html):

<div class="content-ad"></div>

마지막으로, 데모 목적을 위해 항목.json에서 이전에 놓친 일부 업데이트가 여기저기 이뤄졌음을 알립니다 (예: itemDescription 및 itemCrTimestamp). 이 업데이트 내용은 리포지토리의 커밋에서 찾으실 수 있습니다.

결과는 예상대로입니다:

![이미지](https://miro.medium.com/v2/resize:fit:1400/0*rnhUPGwyswVPGEIo.gif)

👉 리팩터링 후 11번째 커밋을 여기서 찾아보세요.

<div class="content-ad"></div>

이제 우리는 동적 구성 요소 없이 동적 폼을 구축하는 방법에 대한 단계별 안내서의 끝쪽에 있습니다. 그러나 동적 구성 요소를 다루는 방법을 살펴보기 전에 우리의 폼에 일부 유효성 검사기를 추가하는 방법을 살펴보아야 합니다.

## 유효성 검사기 추가

폼에 유효성 검사기를 추가하려면 폼 필드 유효성 검사기를 위한 필요한 메타데이터를 준비하고 원하는 폼 필드 객체에 사용해야 합니다. 아마도 이미 짐작했겠지만, 유효성 검사기를 추가하는 것은 우리의 'IFormField' 인터페이스의 또 다른 속성으로 추가할 수 있습니다. 명백한 일관성 이유로 'validators' 속성의 유형으로 사용할 인터페이스를 만들어야 합니다. 아래에 해당 인터페이스 ('IFormFieldValidator')의 구현을 찾을 수 있습니다. 이 인터페이스는 단지 3개의 속성(유형)을 포함합니다:

```js
export interface IFormFieldValidator {
  validatorName: string;          // 예: 'required', 'minLength', 'maxLength', 'pattern', 'email', 'min', 'max'
  validator: any;                 // 예: Validators.required, Validators.minLength(2), Validators.maxLength(10), Validators.pattern('^[a-zA-Z]+$'), Validators.email, Validators.min(1), Validators.max(100)
  validatorErrorMessage: string;  // 예: '이 필드는 필수입니다', '이 필드는 최소 2자 이상이어야 합니다', '이 필드는 10자를 초과할 수 없습니다', '이 필드는 영문자만 포함해야 합니다', '이 필드는 유효한 이메일 주소여야 합니다', '이 필드는 최소 1이어야 합니다', '이 필드는 100을 넘을 수 없습니다'
}
```

<div class="content-ad"></div>

저희 인터페이스에 'validators' 옵션 속성을 추가한 방법이 여기 있어요:

![Interface code sample](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_26.png)

아래에는 ItemsFormFields 배열에 몇 가지 예제를 추가한 방법과 우리의 경우에서 일부 내장 Angular 유효성 검사기를 사용하는 방법을 보여드렸어요:

![Validators code sample](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_27.png)

<div class="content-ad"></div>

다음으로 FormComponent에서 'bindValidators()' 메소드를 사용하여 메타데이터의 정의된 유효성 검사기 집합을 바인딩할 수 있습니다(ItemsFormFields 배열 내). 이 메소드를 'initializeForm()' 메소드에서 호출하여 폼(form group)에 추가하는 모든 컨트롤에 적용할 수 있습니다.

![image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_28.png)

여기서 마지막 단계는 UI에 이를 적용하기 위해 폼 템플릿을 수정하는 것입니다. 이를 위해 주요 ngFor 루프 반복문의 끝에 해당 validator를 선택하기 위한 ngIf 블록을 추가할 수 있습니다. 즉, 에러가 있는 폼 요소에 적합한 validator를 선택하는 것입니다.

![image](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_29.png)

<div class="content-ad"></div>

이것이에요! 예상대로 결과가 나왔어요:

![image](https://miro.medium.com/v2/resize:fit:1400/0*gzLwsuSevVqsihlQ.gif)

👉 12번째 커밋을 찾아보세요. 발리데이터가 추가된 이후에 이 링크에서 확인하세요.

이후에, 우리는 객체의 배열을 기반으로 폼 필드 메타데이터 설정을 가지고 동적 폼을 구현하는 방법을 단계별로 설명하는 첫 번째 부분까지 모두 끝냈어요.

<div class="content-ad"></div>

이 첫 번째 부분의 중요한 결론 중 일부는 다음과 같습니다:

- 제어 서식 생성을 위한 양식 객체 모델로 사용할 수 있는 객체의 데이터 모델을 설정할 수 있습니다.
- 데이터 모델(객체 배열)을 반복 처리하여 각각 필요한 속성을 제공하고 FormGruop에 제어 서식을 추가하여 반응형 양식을 구축할 수 있습니다.
- 표준 내장 Angular 구조 디렉티브인 ngFor 및 ngSwitch와 같은 구조적 디렉티브를 사용하여 데이터 모델에서 제공된 제어 서식을 렌더링하기 위해 양식 템플릿에 양식 HTML 코드를 구성할 수 있습니다.
- 부모-자식 관련 컴포넌트 간에 데이터를 전송하는 대신 서비스를 사용하고 활용함으로써, 컴포넌트를 느슨하게 연결되고 확장 가능하며 유지 관리하기 쉽게 유지할 수 있습니다.

이건 정말 멋진 방법이에요! 그러나 이 동적 양식은 동적 구성 요소를 사용하지 않습니다. 그러니 이제 동적 구성 요소를 사용하는 대체 방안을 살펴보는 시간입니다. 그리고 지금까지 본 방법들 대부분도 적용할 수 있습니다.

그런데요. Angular 동적 구성 요소에 조금 더 심층적으로 파고들고 싶다면, 공식 문서 또는 아래 나의 포스트인 "Angular: any component 👉 -` dynamic component! "에서 간략한 내용을 확인하면 매우 도움이 될 것입니다:

<div class="content-ad"></div>

# B. 동적 구성 요소가 있는 동적 폼

이전의 초기 접근 방식은 다소 "동적"인 폼을 구축하기 위한 일정 수준의 유연성을 제공합니다. 여기에서는 별도의 구성 요소, "후보",을 동적으로 로딩하는 방법을 알아볼 것입니다.

## 핵심 접근 방식

- 우리의 폼은 폼 생성 관리와 동적 구성 요소의로드를 관리하기 위한 컨테이너 구성 요소의 역할을 할 것입니다. 폼은 구성 요소에 대해 아무것도 알지 못합니다.
- 동적 구성 요소를 폼 템플릿에 적용하는 데 사용될 도우미 지시문이 사용될 것입니다.
- 일련의 독립적인 구성 요소들은 "그 자리에서" 동적으로 로딩하기위한 후보 구성 요소입니다. 각각은 특정 유형의 HTML 표준 폼 요소에 전용으로 할당될 수 있습니다.
- 모든 구성 요소, 도우미 지시문 및 폼 구성 요소(폼 컨테이너)는 별도의 폴더 안에 넣을 수 있으며 앱의 나머지 부분에는 폼 구성 요소 만 노출하는 단일 모듈에 의해 지원될 것입니다. 최대한 유연성을 제공합니다.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_30.png" />

# 실행

우리의 구현에는 지금까지 커밋된 코드(12번째 커밋)를 사용할 것입니다. 먼저, 새로운 모듈 파일을 만들어주세요. 예를 들어 'dynamic-form.module.ts'와 같이 만드시면 됩니다. 해당 모듈은 '/app' 아래에 'dynamic-form'이라는 새 하위 폴더 내에 생성될 것입니다.

<div class="content-ad"></div>

```js
ng g m dynamic-form
```

웹 애플리케이션의 환경 설정을 시행하기 위해, FormComponent를 app.module.ts에서 삭제하고 dynamic-form.module.ts 내부에 추가하세요. declarations 배열에 FormComponent를 추가하고 exports 배열에도 추가하세요. 더불어, 초기에는 imports 배열에 MaterialModule 및 ReactiveFormsModule 모듈을 추가해 주세요.

그 후에, DynamicFormModule은 다음과 같습니다:

이후, dynamic-form 서브 폴더(module)에 다음 후보 동적 컴포넌트들을 생성하세요.

<div class="content-ad"></div>


ng g c dynamic-form/input --skip-tests=true --module=/dynamic-form/dynamic-form.module
ng g c dynamic-form/button --skip-tests=true --module=/dynamic-form/dynamic-form.module
ng g c dynamic-form/select --skip-tests=true --module=/dynamic-form/dynamic-form.module
ng g c dynamic-form/checkbox --skip-tests=true --module=/dynamic-form/dynamic-form.module
ng g c dynamic-form/radio --skip-tests=true --module=/dynamic-form/dynamic-form.module
ng g c dynamic-form/datetime --skip-tests=true --module=/dynamic-form/dynamic-form.module


Finally, let's create the helper directive


ng g d dynamic-form/apply-form-control --skip-tests=true --module /dynamic-form/dynamic-form.module


Please note that we are not focusing on testing in this case, hence the use of the ‘--skip-tests=true’ flag.

<div class="content-ad"></div>

이제 확인하면 구성 요소가 "dynamic-form" 하위 폴더로 생성된 것을 볼 수 있습니다:

![dynamic-form components](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_31.png)

그리고 이러한 구성 요소가 DynamicFormModule에도 추가되었습니다:

![DynamicFormModule components](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_32.png)

<div class="content-ad"></div>

잠시 후에는 FormComponent(컨테이너) 템플릿을 변경하고 각 독립 컴포넌트 템플릿을 작성할 것입니다. 그런데 이제는 헬퍼 디렉티브의 역할을 살펴보겠습니다.

## 헬퍼 디렉티브

이전에 저의 "Angular: any component 👉⇢ dynamic component!"라는 게시물을 읽으셨다면 디렉티브가 어떤 후보 동적 컴포넌트의 실제 로더임을 알고 계셔야 합니다. 따라서 동적 컴포넌트는 다른 컴포넌트에서 로드될 수 있지만, 우리의 경우 디렉티브를 사용하는 것이 우선적이며, 실제로 여러 후보 컴포넌트들을 동적으로 로드하기 위한 유일한 논리적 접근 방법입니다.

우리의 디렉티브의 주요 역할은 주입된 ViewContainerRef 인스턴스를 사용하여 ViewContainerRef 인스턴스에서 제공하는 현재 뷰 컨테이너에 후보 컴포넌트를 동적으로 생성(인스턴스화)하는 것입니다. 이를 위해 ViewContainerRef 클래스의 createComponent 메서드를 사용할 것입니다.

<div class="content-ad"></div>

저희의 동적 컴포넌트 중 어떤 것이든 동적 폼의 뷰 조각이 될 것입니다. 즉, 컴포넌트는 인스턴스화될 때 해당 폼 및 해당 유형에 따라 올바르게 작동하기 위해 필요한 메타데이터에 대해 알려져야 합니다. 이러한 이유로 각 인스턴스화된 컴포넌트에 전달해야 하는 두 가지 주요 매개변수가 있습니다. 폼 그룹 인스턴스와 필드 프로퍼티입니다. 이 두 가지는 저희 FormComponent 및 해당 템플릿에서 관리됩니다.

동적 컴포넌트에서 이 두 가지를 이용할 수 있게 하려면 먼저 이를 저희 지시문에 전달해야 합니다. 아마도 예상대로, FormComponent 템플릿 내부에서 저희 지시문을 사용하고, 따라서 지시문은 2개의 각각의 @Input 데코레이터를 통해 이를 액세스할 수 있습니다. 그런 다음, 지시문은 ComponentRef 인스턴스를 통해 동적 컴포넌트에 전달합니다. 동적 컴포넌트에 전달된 두 매개변수를 동적 컴포넌트의 템플릿에서 사용할 것입니다.

저희 지시문을 구축하는 데 하나 더해야 할 단계는 후보 동적 컴포넌트에 대해 알려주는 것입니다. 이는 createComponent 메서드가 인스턴스화해야 하는 컴포넌트 클래스의 유형을 필요로 하기 때문입니다.

이곳의 핵심은 'ItemsFormFields' (‘IFormField’ 인터페이스를 기반으로 한 객체 배열의 알려진 메타데이터)에서 'controlType' 프로퍼티 값과 일치하는 키가 있는 키-값 쌍 (프로퍼티) 객체를 사용하는 것입니다. 이 객체를 'ItemsFormFields.ts' 파일에 const로 정의하고 내보냄으로써 이것을 정의할 수 있습니다.

<div class="content-ad"></div>

일관성을 높이고 오타 에러를 피하기 위해 “IFormFields.ts” 파일에 새로운 사용자 정의 유형 "dynControlType"을 생성하고 "IFormFields" 인터페이스의 controlType 속성의 유형을 string에서 "dynControlType"으로 변경할 수도 있습니다.

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_33.png" />

모든 것을 마친 후 우리의 지시문은 다음과 같습니다:

## 후보 동적 컴포넌트

<div class="content-ad"></div>

이제 후보 동적 구성 요소와 작업할 시간입니다. 먼저, 각 동적으로 생성된 구성 요소에 전달해야 할 두 가지 주요 매개변수를 처리하기 위해 2가지 구성 요소 속성을 정의해야 합니다. 이 두 요소는 모든 후보 동적 구성 요소 클래스에 공통됩니다. 이들을 명명할 때, 지시문에서 사용하는 @Input 데코레이터와 동일한 이름을 사용할 수 있습니다. 'formField'와 'formGroup'를 사용하지만 저는 field와 fGroup를 각각 선호합니다. 이것을 InputComponent에서 확인해 봅시다.

후보 동적 구성 요소에 대한 한 가지 더 유용한 적응은 FormComponent의 스타일 시트를 추가하는 것입니다. 이렇게 하면 FormComponent의 스타일 시트를 계속 사용할 수 있으며, 사용되는 모든 동적 구성 요소에 대한 중앙 지점으로 유지할 수 있습니다:

![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_34.png)

위의 모든 코드 조정은 공통 사항이며, 모든 후보 동적 구성 요소에 적용할 수 있습니다.

<div class="content-ad"></div>

이제 컴포넌트 템플릿을 형성할 수 있어요. 여기서 우리의 초기 작업 대부분은 FormComponent 템플릿에서 해당 HTML 코드를 복사해서 컴포넌트 (예: InputComponent) 템플릿에 붙여넣는 것이에요.

input.component.html

이제 SelectComponent와 ButtonComponent에 대해서도 반복할 수 있어요:

select.component.html

<div class="content-ad"></div>

buton.component.html

마침내, FormComponent 템플릿은 다음과 같이 됩니다: form.component.html

동적 컴포넌트를 사용하지 않은 이전 버전과 거의 동일한 결과입니다.

<div class="content-ad"></div>

![image](https://miro.medium.com/v2/resize:fit:1400/0*Wnu9dKSxx07nRpTG.gif)

👉 여기서 13번째 커밋을 찾으세요.

## 모든 동적 컴포넌트 사용하기

아마도 알아차렸겠지만, 지금까지 우리는 단지 3개의 동적 컴포넌트를 사용했습니다: InputComponent, SelectComponent 그리고 물론 ButtonComponent입니다. 이것은 InputComponent가 "스위스 아미 나이프" 컴포넌트이기 때문이며, 그동안 이것은 또한 datetime, 체크박스 및 라디오 컴포넌트를 포함하기 위해 사용되고 있습니다. 그러나 우리가 동적 컴포넌트를 사용할 때 이는 그리 좋은 생각이 아닙니다. 따라서 우리는 이러한 컴포넌트들을 적절히 조정할 수 있습니다.

<div class="content-ad"></div>

datetime.component.html

radio.component.html

checkbox.component.html

input.component.html 는 다음과 같습니다:

<div class="content-ad"></div>

결국, ItemsFormFields 배열에서 각 필드 객체도 해당 controlType 속성에 적절한 값으로 업데이트되었음을 유의해 주세요.

입력 패밀리에 속하는 구성 요소를 따로 사용하는 이유가 궁금할 수도 있습니다. 그 이유는 서로 독립적으로 만들어 변경 사항이 해당 컴포넌트에만 영향을 미치도록 하기 위해서입니다. 동시에 이것은 코드를 쉽게 확장 가능하고 유지보수하기 쉽게 유지합니다.

결과는 이전과 동일합니다.

👉 마지막으로 커밋된 변경 사항(14번 커밋)을 여기에서 찾아보세요.

<div class="content-ad"></div>

이제 원시 HTML 요소를 사용한 마지막 커밋이었습니다. 이제 Material 라이브러리를 사용할 시간입니다.

# Material UI 컴포넌트 사용하기

아마도 이미 알아차렸겠지만, 저희 프로젝트 앱은 초기부터 Material UI가 설치된 상태로 시작되었습니다. 따라서 다음 단계는 Material UI 컴포넌트를 사용하여 폼과 해당 동적 컴포넌트를 더욱 멋지게 만드는 것입니다. 폼과 동적 컴포넌트 템플릿에서 중요한 수정 사항이 있습니다.

form.component.html

<div class="content-ad"></div>

위 문구를 한국어로 번역해보겠습니다.


input.component.html

button.component.html

checkbox.component.html

radio.component.html


<div class="content-ad"></div>

`select.component.html` 파일입니다.

HTML 템플릿과 FormComponent의 스타일시트(form.component.scss 파일)에 중요한 변경 사항 외에도 material.module.ts 파일에 모든 필요한 Material 모듈을 import 했습니다. 또한 코드에서 몇 가지 사소한 조정을 했습니다(e.g. ItemsFormFieldsService의 "updateRadioOptions" 메소드, IFormField/ItemsFormFields 배열에 속성 추가, FormComponent의 "fornCardTitle" 등).

아래에 결과를 확인할 수 있습니다:

<img src="https://miro.medium.com/v2/resize:fit:1400/0*AgcifzfU-IUmi_Hj.gif" />

<div class="content-ad"></div>

👉 여기서 마지막 15번째 커밋을 찾아보세요.

알아차리셨을 것 같지만, 우리는 날짜 및 시간 구성요소를 빼 두었습니다. 이것은 Angular Material 날짜 선택기 구성요소가 기본적으로 시간 선택을 지원하지 않기 때문입니다. 사용자는 캘린더 UI에서 오직 날짜나 날짜 범위만을 선택할 수 있고, 시간은 선택할 수 없습니다. 그러나 날짜 및 시간 선택을 다루기 위해 사용할 수 있는 제 3 자 라이브러리도 있습니다.

다음 게시물에서 제가 작성한 내용을 통해, HTML 기본 날짜 및 시간 요소를 "Material 호환" 날짜 선택기 구성요소로 변경하는 주요 솔루션과 도전 과제에 대해 더 읽을 수 있습니다:

## 날짜 및 시간 구성요소 업데이트

<div class="content-ad"></div>

위에서 언급한 포스트를 읽을 수 있다면, 여기에서 빠르게 @ng-matero/extensions를 Luxon 라이브러리와 함께 구현해 봅시다. 아래 명령어를 실행하십시오:

```js
npm install @ng-matero/extensions@16.2.0

npm i @angular/material-luxon-adapter@16.2.13 @ng-matero/extensions-luxon-adapter@16.0.0

npm i --save-dev @types/luxon
```

이후에는 @ng-matero/extensions의 Datetimepicker와 Luxon 날짜 및 시간 라이브러리가 추가되었으며, Luxon의 TypeScript 선언 유형 파일도 추가되었습니다. 이후에는 material.module.ts 파일을 다음과 같이 업데이트해야 합니다:

Markdown 형식으로 표를 변경합니다:

![material.module.ts 변경 내용](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_35.png)


<div class="content-ad"></div>

다음 단계는 ng-matero 날짜 및 시간 선택기의 테마를 지정하는 것입니다. 이를 위해 다음과 같이 "mtx_theme.scss" 테마 파일을 생성할 수 있습니다:

그리고 이를 프로젝트의 styles.scss 파일에 가져와야 합니다:

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_36.png" />

이후에는 DatetimeComponent를 업데이트하고 @ng-matero datetime-picker 및 MTX_DATETIME_FORMATS 제공자에 맞추어 정렬해야 하며, 또한 해당 컴포넌트 템플릿도 업데이트해야 합니다.

<div class="content-ad"></div>

datetime.component.ts

datetime.component.html

이제 ItemsFormFieldsService에 "dateTimeString()" 메소드를 만들거나 추가하여 itemCrTimestamp: Date; 값을 ng-matero datetime picker 및 Luxon이 지원하는 형식으로 변환해야 합니다. 이 메소드는 datetime controlType 컴포넌트에서 "updateFormFieldsInitialValues()" 메소드로부터 호출될 수 있습니다.

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_37.png" />

<div class="content-ad"></div>

마지막으로 해야 할 일은 수동적 이벤트 리스너를 지원하여 해당 경고 메시지를 피하는 것입니다:

<img src="/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_38.png" />

해당 경고 메시지는 데이트타임 피커의 날짜 선택을 처음 클릭했을 때 나타납니다. 이를 해결하기 위해, 예를 들어 "/src" 디렉토리 아래에 "zone-flags.ts" 파일을 생성할 수 있습니다. 해당 파일에는 다음 줄을 포함해야 합니다:

```js
(window as any)['__zone_symbol__PASSIVE_EVENTS'] = ['scroll'];
```

<div class="content-ad"></div>

그러면 angular.json 파일의 polyfills 섹션(architecture-` options-` polyfills 및 zone.js 앞에)에 추가해야 합니다:

![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_39.png)

마지막으로, 컴파일을 위해 tsconfig.app.json 파일의 파일 섹션에 포함해야 합니다:

![이미지](/assets/img/2024-07-13-AngularBuildgraduallyyourfullydynamicflexiblereactiveform_40.png)

<div class="content-ad"></div>

결과가 꽤 좋네요:

![image](https://miro.medium.com/v2/resize:fit:1400/0*TeHHjgATdduUmyWv.gif)

👉 16 번째 커밋을 찾아보세요, 여기에 있어요., 그리고 👉 최종 저장소의 커밋 (17 번째 커밋)은 일부 미세한 변경 사항 (이름 바꾸기, 서식 변경) 후에 여기에 있습니다.

# 결론

<div class="content-ad"></div>

와우!, 우리 성공했어요! 동적 구성 요소를 사용하든 사용하지 않든 두 가지 접근 방식을 모두 구현할 수 있습니다. 동적 구성 요소 없이 1차 접근 방식은 원소가 적은 양식에 적합한 선택일 수 있으며, 동적 구성 요소는 더 큰 프로젝트에 대해 더 많은 유연성, 재사용성 및 유지 보수성을 제공합니다.

Angular 동적 폼에 대한 이해가 좋아진 것 같아요. 여러분이 자신만의 동적 폼 구현을 시작할 때 이 글을 참고할 수 있습니다. 마지막으로, Angular 17과 더 정교한 구현을 위해서 꼭 저의 관련 글을 확인하지 않으셨으면 좋겠어요.

이상으로 마칩니다! 즐겁게 읽어주셔서 감사합니다 👏 그리고 기대해 주세요!