---
{"dg-publish":true,"permalink":"/data-structure/stack/","dgPassFrontmatter":true,"created":"","updated":""}
---

![stack thumbnail.png](/img/user/Algorithm/stack%20thumbnail.png)
스택은 후입선출(LIFO, Last-In-First-Out)의 구조를 가지는 자료구조이다.
![Pasted image 20230930175535.png|400](/img/user/Algorithm/Pasted%20image%2020230930175535.png)

그림과 같이 한 쪽에서만 데이터를 넣고 뺄 수 있다. 정해진 Stack의 사이즈보다 많은 데이터를 쌓으려고할 때 `StackOverFlow`가 일어난다. Stack은 추상적인 자료 구조로서 [[Data Structure/배열 (Array)\|배열]]과 [[Data Structure/연결 리스트 (Linked List)\|연결 리스트]]로 구현할 수 있다.
Stack은 중첩 혹은 재귀 함수를 구현하는데 있어서 중요한 방법이며, [[DFS (Depth First Search)\|DFS]]를 구현하는데 사용된다.

## 시간 복잡도
|동작|Big-O|
|---|---|
|Top/Peek|O(1)|
|Push|O(1)|
|Pop|O(1)|
|isEmpty|O(1)|
|Search|O(n)|


## 필수 질문
- LeetCode
	-  [Valid Parentheses](https://leetcode.com/problems/valid-parentheses)
	- [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks)

## 추천 연습 문제
- LeetCode
	- [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)
	- [Min Stack](https://leetcode.com/problems/min-stack)
	- [Asteroid Collision](https://leetcode.com/problems/asteroid-collision)
	- [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)
	- [Basic Calculator](https://leetcode.com/problems/basic-calculator)
	- [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii)
	- [Daily Temperatures](https://leetcode.com/problems/daily-temperatures)
	- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)
	- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)