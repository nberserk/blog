---
layout: post
title:  "next permutation"
tags: 
category: algorithm
---



```
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

## idea
idea는 큰 숫자를 찾아서 앞으로 보내줘야 하는데 해당 수를 찾는게 첫번째. 그렇게 하려면 끝에서 부터 이동하면서 `a[i-1]<a[i]` 을 만족하는 위치(i)를 찾고, i와 바꾸게 될 인덱스 j를 찾는다. a[i:N]까지는 descending이므로 a[i-1]보다 크면서 가장 작은 a[j]를 선택하고 swap한다. 그후 a[i:N]을 reverse하게 되면 next permutation이 된다. 

아래 예를 보자. 4->9가 처음으로 증가하는 부분이고, 4보다 큰 값은 7이 선택되었다. 그러면 다시 9,8,4,1이 되는데 역시 descding 이다. 그러니 이것을 가장 작은 수로 바꾸려면 reverse인, 1,4,8,9로 바꿔주면 된다. 


```
[6，3，4，9，8，7，1] -> [6, 3, 7, 1, 4, 8, 9]
     i-1 i    j
```


```java
    void nextPermutation(int[] nums){
        int n = nums.length;
        if(n<=1) return;
        // find first pos nums[i-1]< nums[i] from the end
        int i=n-1;
        for (; i >=1; i--) {
            if( nums[i-1] < nums[i])
                break;
        }

        // find min but greater than nums[i-1]
        // we can use descending order property to find j
        int j = nums.length-1;
        for (; j >i; j--) {
            if (nums[i-1]< nums[j]){
                break;
            }
        }
        swap(nums, i-1, j);

        // still descending order from nums[i] to the end
        reverse(nums, i);
    }
```

## problems

- [NextPermutation @leetcode](https://leetcode.com/problems/next-permutation/)

## reference 




