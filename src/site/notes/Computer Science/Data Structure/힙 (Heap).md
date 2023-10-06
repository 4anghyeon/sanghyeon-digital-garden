---
{"dg-publish":true,"dg-hide":true,"permalink":"/computer-science/data-structure/heap/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

![heap thumbnail.png](/img/user/Computer%20Science/Data%20Structure/heap%20thumbnail.png)
힙은 [[Computer Science/Data Structure/트리 (Tree)\|트리 (Tree)]]기반의 자료 구조이며, [[Computer Science/Data Structure/트리 (Tree)#이진 트리 (Binary Tree)\|완전 이진 트리]]의 일종이다. 수의 집합에서 가장 큰 수와, 가장 작은 수를 빠르게 찾기 위해 만들어진 자료구조다. .

예를 들어 다음과 같은 배열이 있다고 해보자.
```js
let arr = [1, 2, 3, 4, 5, 6, 7, 8 ,9];
```

이 배열에서 최댓값을 구하기 위해선 배열을 모두 돌아야 하므로 시간 복잡도는 `O(n)`을 가지게 된다. 하지만 힙을 이용하면 이 작업을 `log(n)`으로 해결할 수 있다.

# 특징
1. 완전 이진 트리를 기초로 한다.
2. 최소힙, 최대힙 두 가지 종류로 나뉜다.
3. 중복 값을 허용한다.


# Max Heap(최대 힙)
부모 노드의 값이 자식 노드의 값보다 크거나 같은 힙이다. 즉, 루트 노드가 최댓값이다.
![Pasted image 20231004175301.png|400](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004175301.png)
### 구현
힙의 구현은 주로 [[Computer Science/Data Structure/배열 (Array)\|배열]]을 이용한다. 배열에 루트 노드부터 넣고, 아래로 내려가면서 부모 노드를 왼쪽에서 오른쪽으로 넣어준다.
![Pasted image 20231004180652.png|400](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004180652.png)

### 삽입
새로운 요소가 들어오면 힙의 마지막 노드, 즉 배열의 마지막 인덱스에 넣는다.
![Pasted image 20231004181456.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004181456.png)


새로 삽입된 노드는 자신의 부모 노드와 값의 크기를 비교하면서 자신보다 크거나 같은 노드를 만날 때 까지 부모 노드와 위치를 바꾸며 이동한다.
![Pasted image 20231004181225.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004181225.png)

![Pasted image 20231004192834.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004192834.png)

![Pasted image 20231004193008.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004193008.png)


### 삭제
최대 힙에서의 삭제는 가장 큰 값 즉, 루트 노드를 제거하는 것이다. 루트 노드를 제거한 후 가장 마지막 노드를 루트 노드로 올린다.
![Pasted image 20231004193048.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004193048.png)

![Pasted image 20231004193141.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004193141.png)

그리고 루트 노드는 자식 노드 중 자신보다 큰 자식 중에서도 값이 더 큰 자식 노드와 자리를 바꾸면서 바뀌지 않을 때 까지 내려간다.

![Pasted image 20231004193416.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004193416.png)

![Pasted image 20231004193634.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004193634.png)

# Min Heap (최소 힙)
최대 힙과 반대로 루트 노드가 가장 작은 값이다.
삽입과 삭제는 최대 힙의 방식과 같은 방식으로 진행한다.

![Pasted image 20231004193938.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231004193938.png)


# 시간 복잡도
| 동작             | Big-O     |
| ---------------- | --------- |
| 최대/최소값 찾기 | O(1)      |
| 삽입             | O(log(n)) |
| 삭제             | O(log(n)) |
| 힙화 하기        | O(n)      |


# 필수 문제
- LeetCode
	- [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
	- [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)


# 추천 연습 문제
- LeetCode
	- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
	- [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)