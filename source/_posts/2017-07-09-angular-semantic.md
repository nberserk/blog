---
layout: post
title:  "Angular2 에서 semantic js 사용하기"
tags: angular
published: true
---

Angular2와 React는 동시에 뜨더니 요즘은 React가 더 많이 쓰는것 같긴 하다. 하지만 그전에 이미 Angular2를 사용하기로 정했기에 계속 사용하고 있는데 문제는 Angular2와 딱 맞아 떨어지는 CSS Framework이 없어서 불편하다는 것.

Angular2는 typescript를 사용하고 IDE에서 fully 사용하려면 type definition도 필요히다. 그래서 선택에 제약이 좀 있다.
처음에는 [Angular Material](https://material.angular.io/)을 사용했는데, 보다시피 layout쪽 컴포넌트가 많이 약하다. 그래서인지 버전명에 아직도 beta가 붙어있다.
Bootstrap/Sematic 등은 모두 훌륭한데, Angular에서 완벽하게 사용하려면 java script를 사용해야 하는데 이것을 사용하려면 설정을 더 해야 한다.
새로운 ng-bootstrap이나 ng-semantic도 있긴 하지만 컴포넌트 지원이 완벽하진 않다.

그래서 내가 semantic의 javascript api를 사용하기 위해서 했던 방법을 공유해보고자 한다. 여기선 semantic에 대해서만 얘기했지만 bootstrap도 다를것은 없다.
그리고 장기적으로는 Material처럼 Angular native css framework이 나오는 것이 제일 좋겠다. 그것이 Angular Material이 되겠지만.. 이것만 쓰기에는 컴포넌트가 많이 부족하다.

# setup

우선 semantic는 내부적으로 jquery를 사용하고 있기 때문에 jquery를 추가 해줘야 된다. 패키징 방식에 따라서 아래처럼 CDN 링크를 사용할 수도 있고,
angular-seed라면, `{src: 'jquery/dist/jquery.min.js', inject: true},` 를 NPM_DEPENDENCIES에 추가해주면 된다.

```js
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.2.2/semantic.min.js"></script>
```

## typescript 에서 js 호출하기

우선 jQuery 변수를 전역으로 선언한뒤, ngAfterViewInit에서 jQuery의 API를 통해서 semanctic의 dropdown API를 콜하는 예제.
이 아이디어는 순수히 [ngSemantic](https://github.com/vladotesanovic/ngSemantic)을 참조한 것이다.
dropdown component인 경우 https://github.com/vladotesanovic/ngSemantic/blob/master/src/dropdown/dropdown.ts 을 참고하면 된다.

```js
declare var jQuery: any;

export class MyComponent implements AfterViewInit {
  ngAfterViewInit(): void {
    jQuery('#dropdown').dropdown();
  }
}
```

