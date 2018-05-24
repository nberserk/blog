---
layout: post
title:  "regular expression"
tags: regex
published: true
---

잘 알아두면 피가 되고 살이 되는 정규 표현식에 대해서 정리해 보자.

## lookahead, lookbehind

`4+9*10+7` 라는 식이 있다고 하자. 얘를 operator로 나누고 싶다고 하면 어떻게 해야 할까.

처음에는 lookahead를 사용해서 아래와 같은 regex를 만들었지만 +* 같은 operator와 숫자가 섞인다.

```
expression.split("(?=[-+()*/])");
--> [4, +9, *10, +7]
```

그래서 lookbehind까지 써주면 완벽하게 분리가 된다. isn't it cool ?

```
expression.split("(?<=[-+()*/])|(?=[-+()*/])");
--> [4, +, 9, *, 10, +, 7]
```

## references

- https://www.regular-expressions.info/tutorial.html


