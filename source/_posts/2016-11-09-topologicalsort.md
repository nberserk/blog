---
layout: post
title:  "toplogical sort"
tags: algorithm
category: algorithm
---



## directed graph

아래 처럼 모든 unvisited node를 돌면서 `topologicalSort`를 불러주는 방법이 있는가 하면, 각 노드로 들어가는 edge를 수를 트래킹하면서(indegree map) 트래버스할 수 도 있다.


```java
boolean topologicalSort(int[][] map,int[] visited, int here, List<Integer> path ){
    if(visited[here] == 2) returnt true; // if visited return
    if (visited[here] == 1) return false; // detect cycle

    visited[here] = 1; // open

    for (int i = 0; i < map[here].length; i++){
        if(map[here][i]==1)
            topologicalSort(map, visited, i, path);
    }

    visited[here] = 2; // close
    path.add(here);
    return true;
}
```

## detect cycle

cycle의 경우, directed graph는 visited의 state(1:open, 2:close)까지 봐야 하는 반면, undirectional graph의 경우는 visited의 여부만 보면 된다. 즉 visited에 node에 들어가면 바로 cycle이라고 판정하면 된다. 

## Problems

알고리즘 이해가 되었다면 아래 문제들 풀어보시길.

- [course schedule](https://leetcode.com/problems/course-schedule/) : 기본.
- [alien dictionary @leetcode](https://leetcode.com/problems/alien-dictionary/) : 코너 케이스가 많으니 정신 바짝 차리고.
- [sequence reconstruction @leetcode](https://leetcode.com/problems/sequence-reconstruction/) : 응용


## Reference

- [explanation @geeks](http://www.geeksforgeeks.org/topological-sorting/)


