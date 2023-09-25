---
{"dg-publish":true,"permalink":"/algorithm/sorting-and-searching/","dgPassFrontmatter":true,"created":"","updated":""}
---

# 개요
정렬은 순서대로 요소를 숫자순, 문자순, 오름차순, 내림차순 등으로 재배열 하는 것이다.

많은 기본 알고리즘이 `O(n^2)`으로 돌아가고, 이는 코딩 테스트에서 사용되어선 안된다. 코딩 테스트
에서 정렬 알고리즘을 처음부터 구현할 필요도 없다. 대신 [[Algorithm/이진 탐색 (Binary Search)\|이진 탐색]]을 사용할 수 있도록 입력을 정렬해야 한다.

정렬된 배열에서 [[Algorithm/이진 탐색 (Binary Search)\|이진 탐색]]을 사용하면 `O(n)`보다 빠르게 검색을 수행할 수 있다. 

# 시간 복잡도

|알고리즘|시간 복잡도|공간 복잡도|
|---|---|---|
|Bubble sort|O(n2)|O(1)|
|Insertion sort|O(n2)|O(1)|
|Selection sort|O(n2)|O(1)|
|Quicksort|O(nlog(n))|O(log(n))|
|Mergesort|O(nlog(n))|O(n)|
|Heapsort|O(nlog(n))|O(1)|
|Counting sort|O(n + k)|O(k)|
|Radix sort|O(nk)|O(n + k)|


# 기법
### 정렬된 인풋
만약 주어진 시퀀스가 정렬되어있다면 이진 탐색이 가장 먼저 해봐야 할 것이다.

### 제한된 범위의 인풋 정렬
[[Algorithm/계수 정렬 (Counting Sort)\|계수 정렬]]은 미리 값의 범위를 알고 있을 경우 사용할 수 있다.


## 필수 문제
- LeetCode
	- [Binary Search](https://leetcode.com/problems/binary-search/)
	- [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## 추천 연습 문제
- LeetCode
	- [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
	- [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
	- [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
	- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
	- [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)



---
> 참조
> Do it 알고리즘 코딩 테스트 - Java 편
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)