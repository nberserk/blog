---
layout: post
title: segment tree
tags: algorithm tree segment
published: true
---


트리. 트리는 알고리즘 계에서 혁신적인 발견의 하나인데, O(N)으로 걸릴 만한 것을 O(logN)으로 바꿔주는 마법 같은 data structure이다. 트리는 여러가지 쓰임새가 있지만 이중에서도 segment tree에 대해서 알아보자.

비슷한 용도로 쓰이는 [Binary Indexed Tree]({% post_url 2016-11-19-binaryindexedtree%})가 있는데, 구현이 segment tree보다 간단하기 때문에 더 많이 쓰인다. 따라서, segment tree는 비추!. 나중에 업데이트 할 계획

## When

N개의 배열에서 특정 range(start, end)의 구간합을 구한다고 생각해보자. naive로 하면 O(N)이 걸린다.
각 배열의 값이 변하지 않는다고 가정하면 O(1)으로 값을 구할 수 있다. psum[N]을 선언하고 0~n까지의 합을 미리 계산하여 캐쉬하고 있으면 psum[end] - psum[start]로 구간합을 바로 구할 수 있다.
만약 각 배열의 값이 변한다면 가장 효율적인 방법은 무엇일까.. 이때 segement tree를 이용하면 된다.


|     |time complexity|
|:---:|:-------------:|
|construction| [O(NlogN)](https://en.wikipedia.org/wiki/Segment_tree)|
| query | O(logN) |
| update | O(logN)|


## idea

heap이랑 구조가 비슷하다. root는 모든 범위의 합을 가지고 있고(0 to N-1), left node는 왼쪽 반, right node는 오른쪽 반을 가지게 계속 나누어서 트리를 구성하면 된다. 각 노드는 자신의 범위를 가지고 그 범위의 구간합을 계산하여 가진다. left와 right의 값이 같아질때까지 이것을 반복하면 segment tree가 완성되게 된다. 
예를 들어, 4개의 원소가 있는 구간트리를 도식화해 보면 아래처럼 구성 되게 된다. 



```
                  +--------+
                  | (0,3)  |
                  +--------+
      +--------+              +--------+
      |  (0,1) |              |  (2,3) |
      +--------+              +--------+
+--------+  +--------+   +--------+  +--------+
|  (0,0) |  |  (1,1) |   |   (2,2)|  |  (3,3) |
+--------+  +--------+   +--------+  +--------+
```

## implementation

기본적으로 [start, end] 구간의 중간을 잡아서 왼쪽,오른쪽을 계속 recursive하게 수행하면 된다. 종료 조건은 start와 end가 같을때이다. 그리고 left child의 인덱스는 2*i+1이 되고, 오른쪽 차일드의 인덱스는 2*i+2가 된다. 
그리고 트리의 길이는 원 배열 길이의 4배 정도를 잡아주면 된다. 


#### build

위 전제조건으로 구현해 보면 아래처럼 포현할 수 있다. 


```java
/**
 a: input array
tree: segment tree
 i: index of segment tree  
 */ 
int build(int* a, int* tree, int start, int end, int i){
    if (start==end){
        tree[i] = a[start];
        return tree[i];
    }

    int m = (end-start)/2;
    tree[i] = build(a,tree, start, m, 2*i+1) +
        build(a, tree, m+1, end, 2*i+2);
    return tree[i];
}
```

#### query

세그먼트 트리를 만들었으니 이제 임의의 범위를 주면 그 합을 리턴해주는 query함수를 만들어 보자.


```java
// start,end : start/end index of tree
// qs, qe : query start/end index
int query(int*st, int qs, int qe, int start, int end, int i){
    // 구간을 벗어남
    if (qe<start || qs>start){
        return 0;
    }
    // 쿼리가 범위 안에 있음
    if (qs<=start && end<=qe){
        return st[i];
    }

    // 걸쳐 있는 경우
    int m = (end-start)/2;
    return query(st,qs,qe,start, m, 2*i+1) + query(st, qs, qe, m+1,end,2*i+2);
}
```

#### update

원 배열의 값이 바뀔때 트리를 업데이트 해주는 함수. 해당 배열이 포함된 모든 노드의 값을 수정해 준다. O(logN)


```java
void update(int*st, int dest, int orgValue, int newValue){
    update2(st, dest, newValue-orgValue, 0, N-1, i);
}

void update2(int*st, int dest, int diff, int s,int e, int i){
    if (dest<s || dest>e) return;

    st[i] += diff;
    if (s==e){    
        return;
    }
    int m = (e-s)/2;
    update2(st, dest, diff, s, m, 2*i+1);
    update2(st, dest, diff, m+1, e, 2*i+2);    
}
```



## revision history


- 2015/11/16 initial draft
- 2016/11/8 typo fixed
