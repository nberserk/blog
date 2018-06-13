---
title: angular attribute directive
date: 2018-06-12 19:20:43
tags: angular
---

angular에서 3대 구성요소중 하나인 attribute directive에 대해서 정리.

## attribute directive

highlight-directive.ts

```typescript
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() highlightColor: string;
  constructor(el: ElementRef) {
    el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```

test.html

```html
<p appHighlight highlightColor="yellow">Highlighted in yellow</p>
```

위 예제는 [여기](https://angular.io/guide/attribute-directives)서 가져왔음.

- selector 문법인 `[tag]`는 attribute selector 이다. 예제는 https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors 여기를 보면 된다.
- 위 html의 appHighlight를 사용하는 것만으로 p 에 HighlightDirective 구현을 적용하게 된다. 그렇게 되면 디렉티브의 변수인 highlightColor를 attribute처럼 사용할 수 있다.
- directive의 selector에 app prefix를 붙이는 것은 충돌방지를 위한 것. 특별한 의미 없고 angular측에서 권장하는 practice.





