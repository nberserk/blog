---
layout: post
title:  "rolling hash"
tags: algorithm
published: true
---

스트링에서 특정 substring이 있는지를 판단할때 유용하게 사용할 수 있는 rolling hash 알고리즘에 대해서 소개. 쉽고 유용한 알고리즘임.

## intuition

```
pattern = "abc"
text = "bcabc"
P = len(pattern)
T = len(text)
```
brute force방식의 time complexity는 (P*T) 이다.

우선 아이디어는 hash function을 정의해서 hash값을 비교하여 같은지 비교해서 match 후보들을 빠르게 필터링하는 것이다. hash function을 빠르게 구할수 있어야 한다.

그래서 hash function을 이렇게 정의한다.

```
base = 30(랜덤한 수)
MOD = 큰 소수
h(S) = { s(0)*base^0 + s(1)*base^1 ... s(n-1)*base^(n-1) } % MOD
```

그러면 text[0:2]까지의 해쉬는 패턴의 해쉬와 맞지 않으므로 문자열 비교없이 다음으로 넘어갈 수 있다.

```
h(pattern) = 'a'*30^2 + 'b'*30 + 'c'*30^0
h(text[0:2]) = 'b'*30^2 + 'c'*30  + 'a'*30^0
h(text[1:2]) =         'c'30^2 + 'a'*30 + 'b'*30^0
```
그럼 이제 text[1:2]의 해쉬를 구해야 하는데
다음으로 넘어갈때는 이전 해쉬값에서 -'b'*30^2를 뺀값에 base를 곱하고 새로 추가되는 'b'만 더해주면 된다. 이것을 식으로 나타내면 아래처럼 된다.

> h(n) = base*{h(n-1)-p[n]*30^(P-1)} + p[n+P]

이때 한가지 주의할 점은 hash가 같다고 해도 문자열은 다를 수 있으므로 실제로 한번 체크를 해줘야 한다는 것이다. 이렇게 하면 time complexity를 O(T+P) 정도로 줄 일 수 있다.

## problem

- [Repeated String match](https://leetcode.com/problems/repeated-string-match/description/) , [solution](https://github.com/nberserk/codejam/blob/master/java/src/main/java/leetcode/RepeatedStringMatch_686.java)

