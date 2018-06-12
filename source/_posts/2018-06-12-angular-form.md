---
title: angular form
date: 2018-06-12 11:50:30
tags:
 - angular
---

angular form에 관한 정리. 폼은 template based로 구성할 수도 있도, 타입스크립트로도 구성할 수 있는데 여기서는 template based만 설명할 예정. 그것이 더 깔끔하다고 생각되기 때문.

## form

```typescript
<form (ngSubmit)="onSubmit(heroForm)" #heroForm="ngForm" #form>
  <div class="form-group">
    <label for="name">Name
      <input class="form-control" name="name" required [(ngModel)]="hero.name">
    </label>
  </div>
  <button type="submit" [disabled]="!heroForm.form.valid">Submit</button>
</form>
```
위 예제 출처는 angular.io 공식 사이트에서 가져왔다.

여기서 우리가 주의해서 봐야할 포인트는

- `#heroForm` 과 `#form`은 [template reference variable](https://angular.io/guide/template-syntax#ref-vars) 임.
  - 기본적으로 `#form`은 dom reference-여기서는 `<form>`- 의 [ElementRef](https://angular.io/api/core/ElementRef) 가 연결됨.
  - heroForm에서 사용된 ngForm은 angular의 내장되어 있는 ngForm directive를 사용한 것임. 그래서 기존 dom에는 없는 `heroForm.form`을 사용할 수 있는 것임. 이것은 type은 [NgForm](https://angular.io/api/forms/NgForm)이 됨.
- template reference variable은 template 같은 템플릿 안에서 사용할 수 있음
- `[(ngModel)]` 은 양방향 바인딩한다는 뜻으로 typescript에 있는 hero.name 변수에 양방향 바인딩 된다.

