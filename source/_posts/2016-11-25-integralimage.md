---
layout: post
title:  "intergral image"
tags: dp
category: algorithm
---



```
Given a matrix A, write a matrix M for which every element [i,j] is the sum of all elements of A left and above A[i,j]
Considering the following matrix A:

[
  [3, 7, 1],
  [2, 4, 0],
  [9, 4, 2]
]
Compute M:

[
  [3,  10, 11],
  [5,  16, 17],
  [14, 29, 32]
]

```

## dp

`M(i,j)= M(i-1, j) + M(i, j-1) - M(i-1,j-1) + A(i,j)`


```java
    void integralImage(int[][] m){
        int row = m.length;
        int col = m[0].length;

        for (int y = 0; y < row; y++) {
            for (int x = 0; x < col; x++) {
                if(x==0 && y==0) continue;
                else if(x==0){
                    m[y][x] += m[y-1][x];
                }else if(y==0)
                    m[y][x] += m[y][x-1];
                else{
                    m[y][x] += m[y-1][x] + m[y][x-1] - m[y-1][x-1];
                }
            }
        }
    }
```


## problems



## reference 
- [integral image @leetcode](https://discuss.leetcode.com/topic/23361/given-a-matrix-a-write-a-matrix-m-for-which-every-element-i-j-is-the-sum-of-all-elements-of-a-left-and-above-a-i-j)



