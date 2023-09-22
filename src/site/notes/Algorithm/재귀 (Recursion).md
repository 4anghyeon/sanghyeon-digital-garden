---
{"dg-publish":true,"permalink":"/algorithm/recursion/","dgPassFrontmatter":true,"created":"","updated":""}
---

![Recursion thumbnail.png](/img/user/Algorithm/Recursion%20thumbnail.png)
# 개요
재귀는 어떠한 문제를 계산하기 위해 자기 자신에 의존하는 것을 말한다.

모든 재귀 함수는 두 부분을 포함하고 있다.
1. base case(종료 조건)가 정의 되어 있다. 즉, 재귀가 언제 중지 되어야 하는지 정의되어 있다.
2. 문제를 더 작은 하위 문제로 나누고, 재귀 호출을 한다.

재귀의 가장 일반적인 예제는 피보나치 수욜이다.
- Base cases: `fib(0) = 0` and `fib(1) = 1`
- Recurrence relation: `fib(i) = fib(i-1) + fib(i-2)`

자바스크립트로 구현
```js
function fib(n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2)
}
```

많은 코딩테스트에서는 이진 검색, 정렬 병합, 트리 탐색, DFS 등에서 많이 재귀를 사용한다. 여기서는 일반적으로 알려져 있지 않은 재귀 문제에 대해서 다뤄본다.

## 재귀에 대해 기억해야 할 것
- 항상 재귀의 종료 조건에 대해서 생각해라.
- 재귀는 순열에 대하여 유용하다.
- 재귀는 암시적으로 [[Data Structure/스택 (Stack)\|스택 (Stack)]]을 이용한다. 따라서 모든 재귀적 접근은 스택을 이용하여 계속해서 재작성될 수 있다. 재귀가 너무 깊게 가지 않도록 주의해야 한다. 그렇지 않으면 stack overflow를 발생시킬 수 있다.
- 종료 조건의 개수 - 위의 피보나치 수열 예제에서 보면 재귀적으로 `fib(n-2)`를 호출하고 있다. 이는 적어도 2개의 종료 조건을 가져야 한다는 말이 된다 즉, 코드가 가능한 입력 범위에 대하여 커버할 수 있어야 한다.

# 기법
## Memoization
몇몇 케이스에서는 이전 입력에 대한 결과를 필요로 할 수 있다. 피보나치 수열을 예제로 보면 `fib(5)`는 `fib(4)`와 `fib(3)`을 호출하고, `fib(4)`도 `fib(3)`을 호출한다. 즉 `fib(3)`이 두 번 호출 되었다. 이런 경우 memoization을 이용하면 알고리즘의 효율이 증가하고, 시간 복잡도가 `O(n)`이 되게 할 수 있다.

# 필수 문제
- LeetCode
	- [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
	- [Combinations](https://leetcode.com/problems/combinations/)
	- [Subsets](https://leetcode.com/problems/subsets/)


# 추천 연습 문제
- LeetCode
	- [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
	- [Subsets II](https://leetcode.com/problems/subsets-ii/)
	- [Permutations](https://leetcode.com/problems/permutations/)
	- [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

---
> 참조
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)