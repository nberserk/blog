---
layout: post
title:  "binary search"
tags: algorithm
category: algorithm
---

sorted array에서 타겟값을 찾는것. 반으로 나누어서 탐색공간을 1/2씩 줄여서 O(logN)만에 원하는 값을 찾을 수 있음. 엄청나게 느리게 증가하는 것임. 70억개중 하나 찾는 것은 33번만의 분기만에 찾을 수 있음.

## basic

아래가 기본형 binary search이다. sorted array a에서 target값을 찾는 것이며 없으면 -1을 리턴하는 기본형.

기본적인 로직.

1. a[mid] > target, target이 왼쪽에 있음. -> hi를 mid-1로 치환
1. a[mid] < target, target값이 오른쪽에 있음. -> lo를 mid+1로 치환

```java
    int binarySearch(int target, int[] a){
		int lo = 0;
		int hi = a.length - 1;
		while (lo<=hi) { // notice = is there.
			int mid = lo + (hi-lo)/2; // to prevent overflow, (hi-lo) used
			if (a[mid] > target ) {
				hi = mid-1;
			}else if (a[mid] < target) {
				lo = mid + 1;
			}else{
				return mid;
			}
		}

		return -1;
	}
```

## lower bound

> 1, 2, 3, 4, 4, 4, 7, 8, 9, 10

이제 첫번째 변형. 배열 a에서 중복되는 값이 있을 수 있고 이중에서 가장 첫번째(왼쪽) 값을 찾는다고 생각해보자. 어떻게 변형해야 할까.

기본적으로 (2)는 기본형과 동일하지만 1/3번째가 아래처럼 바뀜.
1. a[mid] > target, target이 왼쪽에 있음. -> hi를 mid로 치환. 여기서 -1을 하지 않는 이유는 target값이 배열의 최소값보다 작으면 index underflow가 나기 때문임.
1. a[mid] < target, target값이 오른쪽에 있음. -> lo를 mid+1로 치환
1. a[mid] == target, hi를 mid로 치환.
 	1. mid의 왼쪽에 동일값이 있을 수 있으므로 오른쪽을 버리지만 한편으로 이것이 마지막 target값일 수도 있으니 -1을 더하진 않음.



```java
	int lowerBound(int lo, int hi, int[] a, int key){
		while (lo<hi) {
			int mid = lo + (hi-lo)/2;
			if (a[mid] < key) {
				lo = mid+1;
			}else{
				hi = mid;
			}
		}
        if(a[lo]==key) return lo;
        return -1;
	}

```



## uppper bound
> 1, 2, 3, 4, 4, 4, 7, 8, 9, 10

이제 lower_bound와 반대로, 배열 a에서 중복되는 값이 있을 수 있고 이중에서 가장 마지막(오른쪽) 값을 찾는다고 생각해보자. 어떻게 변형해야 할까.

(1)는 기본형과 동일하지만 2/3번째가 아래처럼 바뀜.
1. a[mid] > target, target이 왼쪽에 있음. -> hi를 mid-1로 치환.
1. a[mid] < target, target값이 오른쪽에 있음. -> lo를 mid로 치환. mid+1로 치환하지 않는 이유는 index overflow가 나기 때문임.
1. a[mid] == target, lo를 mid로 치환.
 	1. mid의 오른쪽에 동일값이 있을 수 있으므로 왼쪽을 버리지만 한편으로 이것이 마지막 target값일 수도 있으니 +1을 더하진 않음.


```java
	int upperBound(int lo, int hi, int[] a, int key){
		while (lo<hi) {
			int mid = lo+(hi-lo+1)/2;
			if (a[mid]>key){
                hi = mid-1;
            }else{
				lo = mid;
			}
		}
        if(a[lo]==key) return lo;
        return -1;
	}
```




## in a rotated array

`{5,6,7,8,1,2,3}` 처럼 한번 rotated된 배열에서 특정 숫자를 찾는다고 생각해보자. 이때에도 바이너리 서치를 사용할 수 있을까? 답은 예스. 반으로 잘랐을때 왼쪽과 오른쪽 하나는 ascending이므로 이 것을 이용해서 target이 왼쪽에 있는지 오른쪽에 있는지 판단할 수 있다!

```java
    static int bsearchRotatedSimpler(int[] a, int key){
        int lo = 0;
        int hi = a.length-1;
        while(lo<=hi){
            int m = (lo+hi)/2;
            if(key==a[m]) return m;

            if(a[lo]<=a[m]){
                if(a[lo] <= key && key <= a[m])
                    hi=m-1;
                else lo=m+1;
            }else{
                if(a[m] <=key && key <=a[hi])
                    lo=m+1;
                else hi=m-1;
            }
        }

        return -1;
    }
```

## problems

- [two sum II @leetcode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
- [Search in rotated sorted array @leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- [Search in rotated sorted array II @leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) : tricky,
- [find minimum in rotated sorted array @leetcode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
- [find minimum in rotated sorted array II @leetcode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
- [find peak element @leetcode](https://leetcode.com/problems/find-peak-element/)
- [median of two sorted array @leetcode]( https://leetcode.com/problems/median-of-two-sorted-arrays/) : [explanation](https://discuss.leetcode.com/topic/4996/share-my-o-log-min-m-n-solution-with-explanation)

- [find Kth smallest piar distance @leetcode](- [find peak element @leetcode](https://leetcode.com/problems/find-peak-element/))


## reference

- [tutorial @topcoder](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-search/)
- [my implementation & test cases](https://github.com/nberserk/codejam/blob/master/java/src/main/java/crackcode/binarysearch/BinarySearch.java)

## history

- 11/28/2016 , binarySearchRotatedArray/searchRange added
- 11/7/2017, lowerBound/upperBound added