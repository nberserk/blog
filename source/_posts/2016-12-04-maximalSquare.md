---
layout: post
title:  "maximal square"
tags: dp
category: algorithm
---



```
Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

For example, given the following matrix:

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
Return 4.

```

## idea

생각하기 어려운 dp 식일것 같은데, 아래 처럼 된다. dp(i,j)를 i,j에서 끝나는 maximal square라고 정의.

`dp(i,j) = Math.min(dp(i-1,j), dp(i,j-1), dp(i-1,j-1))+1`

`1`인 셀이면 dp(i,j)=1이 된다. 이웃들중의 가장 작은 값+1이 되는 것이다. 즉, 모두 인접한 이전 값들이 모두 1이라야 자신은 2가 될 수 있다.

initial condtion은 (x,0) 은 1이면 1, 아니면 0이 된다. (0,y)도 마찬가지이다.



```java
public int maximalSquare(char[][] matrix) {
        int row=matrix.length;
        if(row==0) return 0;
        int col = matrix[0].length;

        int ret=0;
        int[][] dp = new int[row][col];
        for(int y=0;y<row;y++){
            if(matrix[y][0]=='1'){
                dp[y][0]=1;
                ret=1;
            }
        }

        for(int x=0;x<col;x++){
            if (matrix[0][x]=='1'){
                dp[0][x]=1;
                ret=1;
            }
        }

        for (int y = 1; y < row; y++) {
            for (int x = 1; x < col; x++) {
                if(matrix[y][x]=='0') continue;
                dp[y][x] = Math.min(dp[y-1][x], Math.min(dp[y-1][x-1], dp[y][x-1]) ) +1;
                if(dp[y][x]>ret)
                    ret = dp[y][x];
            }
        }

        return ret*ret; // return area
    }
```

## problems

- [Maximal Square @leetcode](https://leetcode.com/problems/maximal-square/)
- [Maximal Rectangle @leetcode](https://leetcode.com/problems/maximal-rectangle/) : Square -> Rectangle






