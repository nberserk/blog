---
layout: post
title:  "largest rectangle in histogram"
category: algorithm
---



> Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
> 
Given heights = [2,1,5,6,2,3],
return 10.

## idea

naive는 start, end 를 하나씩 정해서 돌면 `O(N^2)`.

모든 height의 왼쪽, 오른쪽 바운더리를 알면 맥스 값을 구할 수 있다. 왼쪽/오른쪽 바운더리는 자신보다 낮은 바를 만나면 결정된다. 따라서 ascending 순으로 리스트를 관리하다가 낮은 바가 나오면 오른쪽 바운더리는 결정이 되고, 왼쪽 바운더리는 리스트의 tail이 된다. 이렇게 각 height에 따라서 계산을 하면 최적해를 구할 수 있다. 이 리스트는 스택으로 구현하는 것이 좋고, push, pop, top 정도만 필요하므로.

위의 예제를 기반으로 시뮬레이션 해보면 [2,1,5,6,2,3]


i | stack | desc |
- | ----- | -- |
0 | 0 | |
1 | 0 | n\[i](1)< stack.top(2), stack의 탑인 2의 right는 1, left는 -1(stack이 empty이므로). left right 경계는 포함되지 않으므로 width = 1-(-1)-1 = 1이 됨. localMax=2*1=2 |
1 | 1 |  |
2 | 1,2 | |
3 | 1,2,3 | |
4 | 1,2  | stack pop. poped=3. n\[3]=6. 6의 right=4, left=2. width=4-2-1=1. localMax=6*1=6 |
4 | 1 | stack pop, poped=2, n\[2]=5, 5의 right=4, left=1. width=4-1-1=2. localMax=5*2=10 |
4 | 1,4 | |
5 | 1,4,5 | |
6 | 1,4 | stack pop, poped=5, n\[5]=3, 3의 right=6, left=4. width=6-4-1=1. localMax=3*1=3 |
6 | 1 | stack pop, poped=4, n\[4]=2, 2의 right=6, left=1, width=6-1-1=4. localMax=2*4=8 |
6 |  | stack pop, poped=1, n\[1]=1, 1의 right=6, left=-1, width=6-(-1)-1. localMax=1*6=6 |

따라서 largest area는 10이 된다. 

## implementation

```java
int largestRectangleArea(int[] heights) {
    int max = Integer.MIN_VALUE;
    Stack<Integer> stack = new Stack<>();
    for(int i=0;i<=N;i++){ // interate until N, 
        int h = i==N?0:heights[i];
        if(stack.isEmpty() || h >= heights[stack.peek()])
            stack.push(i);
        else{
            h=heights[stack.pop()];
            int width = stack.isEmpty() ? i : i-stack.peek()-1;
            max=Math.max(max, h*width);
            i--;
        }
    }
    return max;
}

```

## problems

- [largest rectangle in histogram @leetcode](https://leetcode.com/problems/largest-rectangle-in-histogram/)
- [Maximal Rectangle @leetcode](https://leetcode.com/problems/maximal-rectangle/)

## reference
- [my implementation & test cases](https://github.com/nberserk/codejam/blob/master/java/src/main/java/crackcode/stack/LargestRectangleHistogram.java)






