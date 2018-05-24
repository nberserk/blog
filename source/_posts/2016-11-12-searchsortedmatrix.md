---
layout: post
title:  "search sorted 2d matrix"
tags: algorithm
category: algorithm
---

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.
Integers in each column are sorted in ascending from top to bottom.


```
  [1,   4,  7, 11, 15]
  [2,   5,  8, 12, 19]
  [3,   6,  9, 16, 22]
  [10, 13, 14, 17, 24]
  [18, 21, 23, 26, 30]
```

> 문제 출처, [search 2d matrix 2 @leetcode](https://leetcode.com/problems/search-a-2d-matrix-ii/)

## binary search ?

binary search를 사용하면 각 row에서 log(col)만에 target이 있는지 없는지 판단 가능. 즉, `O(col*log(row))`의 time complexity.

## Linear

더 잘 할 수도 있는데, `O(row+col)` 만에 가능함.

top right 코너에서 왼쪽으로 가면 작은값, 오른쪽으로 가면 큰 값이다. 따라서, 한번 이동할때마다 row나 컬럼을 탐색공간에서 제거할 수 있다. 같은 방식으로 bottom left 코너에서도 가능. 하지만 top left나 bottom right에서는 불가능함. 그 이유는 모두 증가하는 방향이기 때문임.


```java
public boolean searchMatrix_linear(int[][] matrix, int target) {
        int row = matrix.length;
        if(row==0) return false;
        int col = matrix[0].length;

        int r = 0;
        int c =col-1;
        while(r<row&&c>=0){
            if(matrix[r][c]==target ) return true;
            else if (matrix[r][c]>target) c--;
            else if (matrix[r][c]<target) r++;
        }
        return false;
}        
```

