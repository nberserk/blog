---
layout: post
title:  "binary indexed tree"
tags: tree
category: algorithm
---

binary indexed tree(bit)는 [Segment tree]({% post_url 2015-11-11-segmenttree %})처럼 range sum을 구하는 것에 최적화된 알고리즘이지만, segment tree보다 구현이 훨씬 쉽다. 대신 이해는 좀 더 여렵다. 그래서 segment tree는 이제 안녕.



|     |time complexity|
|:---:|:-------------:|
|construction| O(NlogN)|
| sum | O(logN) |
| update | O(logN)|



## idea

integer는 2^n승의 조합으로 표현할 수 있는것처럼, cumulative sum도 그렇게 나타낼 수 있겠다는 것.
2진수 표현에서 착안.
`tree(i) = sum from i-2^r+1 to i`
라고 정의 r은 i를 2진수로 나타내었을때 마지막 1이 있는 자리수. 0b10은 1, 0b1은 0 


아래 7개 원소를 가지는 배열이 있고 cumulative sum을 계산하게 되면 아래표와 같다. 

```
idx     1   2   3   4   5   6   7
n[i]    1   3   2   2   4   7   2
cul sum 1   4   6   8   12  19  21   
```

이것을 tree로 표현하는데, parent node는 left child node의 cumulative sum을 가지고 있게 설계를 하면 다음 트리와 같은 모양이 나온다.

- 4는 [1-4] 모든 값을 합
- 2는 1과 2의 합
- 3은 3만
- 5는 5만 

```
                4[100]
                  +8
        2[010]             6[110]
         +4                  +11
    1[001]  3[011]    5[101]    7[111]
    +1          +2         +4        +2
```

그렇다면 여기서 더 응용을 해서, 

- 5까지의 누적 합을 구하고 싶으면, 노드 5와 노드4를 더하면 되고
- 6까지의 누적 합을 구하고 싶으면, 노드 6(5-6)과 노드4(1-4)를 더하면 된다.

공교롭게도, 이 모든 것은 last 1bit를 빼주면서 계속 반복해주면 된다.  last 1bit는 `i&-i`로 구할 수 있다.
이것을 예로 들어보면 아래처럼 될 수 있다.

```
6[110] 의 경우
    6[110],     [5-6]까지의 합. -> 010을 빼줌
    4[100],     [1-4]까지의 합, -> 100을 빼줌
    0. 종료.
```

## 2D


## problems

- [Range sum query 2D @leetcode](https://leetcode.com/problems/range-sum-query-2d-mutable/)


## reference

- [intuition behind BIT](https://cs.stackexchange.com/questions/10538/bit-what-is-the-intuition-behind-a-binary-indexed-tree-and-how-was-it-thought-a)
