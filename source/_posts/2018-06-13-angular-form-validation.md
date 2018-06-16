---
title: angular form validation
date: 2018-06-12 19:28:22
tags: 
- angular
---

앞서 기본적인 [angular form](/2018/06/12/angular-form) 에 대해서 알아봤는데, 이번에는 form validation에 대해서 알아보자.

## using built-in validator

angular에서 built-in validator의 종류는 다음과 같다.


| name | attribute selector | desc |
| ---- | --- | --- |
| [RequiredValidator](https://angular.io/api/forms/RequiredValidator) | [required] | |
| [MinLengthValidator](https://angular.io/api/forms/MinLengthValidator) | [minlength] |
| [MaxLengthValidator](https://angular.io/api/forms/MaxLengthValidator) | [maxlength] |
| [PatternValidator](https://angular.io/api/forms/PatternValidator) | [pattern] | |

기타로 EmailValidator, CheckboxRequiredValidator 등도 있다. 위의 Validator들은 아래처럼 selector로 directive가 적용되게 할 수 있다.

```typescript
<form (ngSubmit)="onSubmit(heroForm)" #heroForm="ngForm" #form>
  <div class="form-group">
    <label for="name">Name
      <input class="form-control" name="name" required minlength="2" maxlength="100" pattern="[a-z0-9]+" >
    </label>
  </div>
  <button type="submit" [disabled]="!heroForm.form.valid">Submit</button>
</form>
```

- input에 있는 required selector에 의해서 RequiredValidator가 적용됨
- 마찬가지로 minlength|maxlength selector에 의해서 MinLengthValidator와 MaxLengthValidator가 적용됨
- pattern selector에 의해서 소문자와 숫자만 입력할 수 있게 함.

## showerror component

기본적으로 Validator를 적용해서 ValidationErrors가 발행하게 되면 `control.errors`에 에러가 들어오게 된다.
아래는 이것을 이용해서 form의 에러를 핸들링하는 컴포넌트다.

```typescript
// show-errors.component.ts
import { Component, Input } from '@angular/core';
import { AbstractControlDirective, AbstractControl } from '@angular/forms';

@Component({
 selector: 'show-errors',
 template: `
   <ul *ngIf="shouldShowErrors()">
     <li style="color: red" *ngFor="let error of listOfErrors()">{{error}}</li>
   </ul>
 `,
})
export class ShowErrorsComponent {

 private static readonly errorMessages = {
   'required': () => 'This field is required',
   'minlength': (params) => 'The min number of characters is ' + params.requiredLength,
   'maxlength': (params) => 'The max allowed number of characters is ' + params.requiredLength,
   'pattern': (params) => 'The required pattern is: ' + params.requiredPattern,
   'custom': (params) => params.message
 };

 @Input()
 private control: AbstractControlDirective | AbstractControl;

 shouldShowErrors(): boolean {
   return this.control &&
     this.control.errors &&
     (this.control.dirty || this.control.touched);
 }

 listOfErrors(): string[] {
   return Object.keys(this.control.errors)
     .map(field => this.getMessage(field, this.control.errors[field]));
 }

 private getMessage(type: string, params: any) {
    if (params.message) {
      return params.message;
    }
   return ShowErrorsComponent.errorMessages[type](params);
 }

}
```
위 소스의 출처는 https://www.toptal.com/angular-js/angular-4-forms-validation 에서 가지고 왔고 getMessage부분을 약간 변형을 했다.


## custome validator

하다보면 custom validator가 필요한데 이런식으로 만들어보면 된다.

```typescript
import { Directive } from '@angular/core';
import { NG_VALIDATORS, Validator, FormControl, ValidationErrors } from '@angular/forms';


@Directive({
 selector: '[telephoneNumber]',
 providers: [{provide: NG_VALIDATORS, useExisting: TelephoneNumberFormatValidatorDirective, multi: true}]
})
export class TelephoneNumberFormatValidatorDirective implements Validator {

 validate(c: FormControl): ValidationErrors {
   const isValidPhoneNumber = /^\d{3,3}-\d{3,3}-\d{3,3}$/.test(c.value);
   const message = {
     'telephoneNumber': {
       'message': 'The phone number must be valid (XXX-XXX-XXX, where X is a digit)'
     }
   };
   return isValidPhoneNumber ? null : message;
 }
}
```

위 Validator를 적용하려면 앞서 예제와 같이 `<input>` tag에 validator의 selector인 `telephoneNumber`를 추가해주면 적용이 된다.

## Reference

- https://www.toptal.com/angular-js/angular-4-forms-validation

