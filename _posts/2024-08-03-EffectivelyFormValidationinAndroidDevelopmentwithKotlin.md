---
title: "안드로이드 개발에서 Kotlin으로 효과적으로 폼 유효성 검사하는 방법"
description: ""
coverImage: "/assets/img/2024-08-03-EffectivelyFormValidationinAndroidDevelopmentwithKotlin_0.png"
date: 2024-08-03 19:54
ogImage: 
  url: /assets/img/2024-08-03-EffectivelyFormValidationinAndroidDevelopmentwithKotlin_0.png
tag: Tech
originalTitle: "Effectively Form Validation in Android Development with Kotlin"
link: "https://medium.com/@resmaadi/effectively-form-validation-in-android-development-with-kotlin-932bed9badf1"
---



<img src="/assets/img/2024-08-03-EffectivelyFormValidationinAndroidDevelopmentwithKotlin_0.png" />

# 소개

폼은 애플리케이션에서 중요한 요소 중 하나입니다. 적어도 사용하는 모든 앱에는 로그인하거나 등록하기 위한 하나 이상의 폼이 있습니다. 앱의 중요한 부분이라는 것을 알고 있는데, 좋은 폼은 좋은 경험을 가져야 합니다. 예를 들어, 폼에 유효하지 않은 문자를 입력할 때 경고가 표시되어야 합니다. Android Studio에서 EditText 구성 요소에서 `setError`를 사용할 수 있습니다. 작은 양의 폼만 필요한 앱에게는 쉬워 보일 수 있습니다. 그러나 앱이 더 복잡해지고 앱 자체가 전체 폼에서 생성된 것일 때, 시간이 지날수록 `setError`를 계속 사용하여 피곤함을 느낄 것입니다. 그래서 폼 유효성 검사를 보다 쉽게 관리할 수 있는 기능이 필요합니다. 또한 간단한 폼만 필요한 앱에게도 매우 유용합니다. 시간을 절약하고 다른 중요한 기능을 만드는 데 집중할 수 있습니다.

# 메소드


<div class="content-ad"></div>


![이미지](/assets/img/2024-08-03-EffectivelyFormValidationinAndroidDevelopmentwithKotlin_1.png)

이 함수의 주요 개념은 함수가 EditText 목록을 받고, 각 항목을 유효성 검사 규칙으로 확인할 것이며, 값이 유효성 검사 규칙을 통과하지 못한 경우 EditText의 오류 속성이 해당 입력 유형에 대한 메시지로 설정될 것입니다. 그래서 이 함수를 만드는 전체 단계는 다음과 같습니다:

## ValidationRules 유형 생성

필수는 아니지만 Map을 사용하는 대신 typealias를 사용하면 더 쉽게 사용할 수 있습니다.


<div class="content-ad"></div>

```js
typealias ValidationRules = Map<EditText, (String) -> Boolean>
```

## Create ValidationHelper object class

Before using the Validation function, you need to define the validation rules you want to apply. For example, you can use regex for email validation. Check out the code snippet below for an example of how to do this.

```js
object ValidationHelper {

    fun email(email: String): Boolean {
        val emailRegex = "^[A-Za-z0-9+_.-]+@(.+)\$"
        return email.matches(emailRegex.toRegex())
    }

    fun username(username: String): Boolean {
        return username.length >= 4
    }

    fun pass(password: String): Boolean {
        return password.length >= 8
    }

    fun matchpass(password: String, confirmPassword: String): Boolean {
        return password == confirmPassword
    }

    fun notempty(input: String): Boolean {
        return input.isNotEmpty()
    }

    fun phone(phoneNumber: String): Boolean {
        val phoneRegex = "^\\d{10,15}\$"
        return phoneNumber.matches(phoneRegex.toRegex())
    }

    fun number(input: String): Boolean {
        return input.toDoubleOrNull() != null
    }
}
```

<div class="content-ad"></div>

인터넷에서 검색하여 직접 유효성 검사 규칙을 만들 수 있어요.

## FormValidator 객체 클래스에 설정 유효성 검사 함수 만들기

이 부분이 중요해요. 헬퍼와 typealias를 만든 후에, 유효성 검사 규칙을 설정하고 EditText에 적용하기 위한 설정 함수가 필요해요.

```js
fun setup(
        forms: List<EditText>,
        validationRules: ValidationRules,
        actionButton: Button
    ) {
        forms.forEach { form ->
            form.doOnTextChanged { text, _, _, _ ->
                val isValid = validationRules[form]?.invoke(text.toString()) ?: false
                form.error = if (isValid) null else getErrorMessage(form, validationRules)
                actionButton.isEnabled = validationRules.all { (form, rule) -> rule(form.text.toString()) }
            }
        }
    }
```

<div class="content-ad"></div>

## 다음으로 getErrorMessage 함수를 사용해보세요

이 함수는 EditText ID를 통해 정의한 사용자 정의 오류 메시지를 가져오는 기능입니다. 실제로 기본 변수를 사용하거나 도우미와 함께 오류 메시지를 결합해서 더욱 효과적으로 사용할 수 있습니다.

```js
private fun getErrorMessage(form: EditText, validationRules: ValidationRules): String {
        return when (form) {
            in validationRules.keys -> when (form.id) {
                R.id.et_email -> "유효하지 않은 이메일 주소입니다"
                R.id.et_username -> "사용자 이름은 적어도 4자 이상이어야 합니다"
                R.id.et_password -> "비밀번호는 적어도 8자 이상이어야 합니다"
                R.id.et_confirm_password -> "비밀번호가 일치하지 않습니다"
                else -> "유효하지 않은 입력입니다"
            }
            else -> "유효하지 않은 입력입니다"
        }
    }
```

## 마지막으로, activity 클래스에서 해당 함수를 사용해주세요

<div class="content-ad"></div>

```kotlin
private fun formValidation() {
        with(binding) {
            val forms = listOf(etEmail, etPassword)
            val validationRules : ValidationRules = mapOf(
                etEmail to { ValidationHelper.email(it)},
                etPassword to { ValidationHelper.pass(it)}
            )
            FormValidator.setup(forms, validationRules, btnLogin)
        }
    }
```

축하합니다! 이제 FormValidation을 사용할 준비가 되었습니다.

# 결론

<img src="/assets/img/2024-08-03-EffectivelyFormValidationinAndroidDevelopmentwithKotlin_2.png" />


<div class="content-ad"></div>

양식은 애플리케이션에서 중요한 요소 중 하나에요. 그래서 효과적인 양식 유효성 검사를 통해 좋은 경험을 제공하는 것이 중요해요. 이를 통해 개발자는 더 쉽고 빠르게 코드를 작성할 수 있어요. 그리고 프로젝트가 끝나면 다음 프로젝트를 시작하고 푹 쉴 수 있어요.