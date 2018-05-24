---
layout: post
title:  "Maximum XOR"
tags: xor
category: algorithm
---

아래 문제를 보자

```
Given a non-empty array of numbers, a0, a1, a2, … , an-1, where 0 ≤ ai < 231.
Find the maximum result of ai XOR aj, where 0 ≤ i, j < n.
Could you do this in O(n) runtime?

Example:
Input: [3, 10, 5, 25, 2, 8]

Output: 28

Explanation: The maximum result is 5 ^ 25 = 28.

```

위 문제는 naive하게는 두원소를 가면서 매번 XOR를 해서 max를 구하면 된다. 이것은 O(N^2). 구현은 생략.

## idea

먼저 하나의 bit만 생각해보자. n번째 bit `a^b=1`이 되려면 1과 0이 둘다 있어야 한다. 그런데 xor는 `a^b=c <==> a^c=b`이 성립한다. 그러면 원래의 식은 `a^1=b`이 되고 각 수를 masking해서 나온 수를 set에 저장하고, 그것을 mask와 xor한 값이 그 set에 있으면 우리는 n번째 bit수를 1로 만들 수 있다는 얘기다.

아래 구현 참고.


```java
public int findMaximumXOR(int[] nums) {
        int N = nums.length;
        int mask=0;
        int max=0;
        for(int i=30;i>=0;i--){
            mask |= (1<<i);
            Set<Integer> set = new HashSet<>(); // prefix
            for(int n:nums){
                set.add(n&mask);
            }

            // now find out if there are two prefix with different i-th bit
            // if there is, the new max should be current max with one 1 bit at i-th position, which is candidate
            // and the two prefix, say A and B, satisfies:
            // A ^ B = candidate
            // so we also have A ^ candidate = B or B ^ candidate = A
            // thus we can use this method to find out if such A and B exists in the set
            int temp = max | (1<<i);
            for(int prefix: set){
                if(set.contains(prefix^temp) ){
                    max=temp;
                    break;
                }
            }
        }
        return max;
    }
    
```


## Problems

- [Maximum XOR  @leetcode](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) 





