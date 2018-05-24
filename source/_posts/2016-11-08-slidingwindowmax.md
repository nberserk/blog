---
layout: post
title:  "sliding window maximum/minimum"
tags: algorithm
---

Given array [A<sub>0</sub>,A<sub>1</sub>, - ,A<sub>n</sub> ], sliding window of size k which is moving from the very left of the array to the very right. find max/min of each sliding window.

## naive
O(NK) 로 가능.

## using priority_queue
슬라이딩 윈도우의 값들을 max/min heap으로 관리하면 O(NKlog(K)) 정도로 풀 수 있다. logK는 추가, O(K)는 삭제.

## linear

O(N)도 가능!

intuition:

- window안의 값을 deque로 관리하는데 head에는 최소 값, tail에는 최대값이 들어가게 관리. 
- whenever add a[i], `while(a[i]>head)` 이면 head 삭제. 윈도우의 값보다 작은 값들은 모두 버린다. 왜 그럴까? (버려지는 값들은 a[i]가 있는한 최대값이 될 수 없으므로).
- 그럼 sliding window가 범위를 벗어날때, 삭제는 어떻게 하면 될까? deque를 돌면서 지우면 될까? 그렇게 하면 O(N)을 만족할 수 없고, 우리는 tail값만 사용할 것이므로 tail이 현재 window에 있는지만 보면 된다. 따라서, 각 스텝마다 윈도우를 벗어나면 지우면 된다.


```java
	public int[] maxSlidingWindow(int[] nums, int k) {
        int N = nums.length;
        int R = N-k+1;
        if(R<=0) return new int[]{};
        int[] ret = new int[R];
        LinkedList<Integer> deque = new LinkedList<>(); //first:min, last:max

        for (int i = 0; i < N; i++) {
            int start = i-k+1;
            int end = i;

            while(!deque.isEmpty()&& deque.peekLast()<start)
                deque.removeLast();

            while(!deque.isEmpty() && nums[deque.peekFirst()] <= nums[end] )
                deque.removeFirst();
            deque.addFirst(end);

            if(start>=0)
                ret[start] = nums[deque.peekLast()];
        }
        return ret;
    }
```




## Problems
이해가 되었다면 아래 문제들을 풀어보자.

- [Sliding Window Maximum @leetcode](https://leetcode.com/problems/sliding-window-maximum/)

## Reference

- [explanation](https://abitofcs.blogspot.kr/2014/11/data-structure-sliding-window-minimum.html)
- [My implemetation & test cases](https://github.com/nberserk/codejam/blob/4cd0ba731762f9f65151bc656a53474f4d519501/java/src/main/java/crackcode/queue/SlidingWindowMaximum.java)
