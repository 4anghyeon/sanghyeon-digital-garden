---
{"dg-publish":true,"tags":["자료구조"],"permalink":"/data-structure/array/","dgPassFrontmatter":true,"created":"","updated":""}
---

![array thumbnail.png](/img/user/Data%20Structure/array%20thumbnail.png)
배열은 같은 타입의 값 *(이는 언어 마다 다를 수 있다..)* 들을 연속적으로 저장하는 메모리 장소이다. 배열에서는 주로 2가지를 중요시 여긴다 요소의 인덱스와, 요소 그 자체다. 프로그래밍언어마다 배열을 구현하는 방법이 다르고, 이는 시간 복잡도에 영향을 줄 수 있다. 예를 들어 파이썬, 자바스크립트, Ruby, PHP 등에서는 배열의 크기는 동적이다. 즉, 배열을 만들 때 크기를 지정할 필요가 없다. 그래서 코딩테스트 등에서 사용하기 더 편리하다. 

#### 이점
- 하나의 변수에 같은 타입의 여러 값을 저장할 수 있다.
- 인덱스를 이용하면 요소에 접근하는 속도가 빠르다 (헤드부터 순회하는 연결 리스트(Linked List)와 반대 된다.)

#### 단점
- 배열의 중간에 요소를 추가 혹은 제거하는 것이 느리다. 왜냐하면 추가, 삭제 후 나머지 요소들을 전부 밀어야 하기 때문이다. (끝 요소를 추가, 삭제하는 것은 예외)
- 특정 언어의 경우 배열의 사이즈가 한번 정해지면, 그 이후에는 바꿀 수 없다. 만약 삽입으로 인해 배열의 크기를 초과할 경우 기존 배열의 요소를 전부 복사 후 새로운 배열을 할당해야 한다. 이는 `O(n)`의 시간 복잡도가 소요된다.

## 자바스크립트에서 배열

자바스크립트에서는 배열을 다음과 같이 만든다.

```js
// 1. 배열 리터럴
const arr = [1, 2, 3];

// 2. Array 생성자 함수
const arr2 = new Array(10); // [empty x 10]
const arr3 = new Array(1, 2, 3); // [1, 2, 3]

// 3. Array.of 함수
const arr4 = Array.of(1); // [1]

// 4. Array.from 함수
const arr5 = Array.from({ length: 2, 0: 'a', 1: 'b' }); // ['a', 'b']
const arr6 = Array.from("Hello"); // ['H', 'e', 'l', 'l', 'o']
```


> [!NOTE] **자바스크립트의 배열은 배열이 아니다.**
> 배열은 동일한 타입의 값이 연속적으로 나열된 저장소라고 했다. 하지만 자바스크립트에서 배열을 만들면 다른 타입의 값들이 들어가는걸 볼 수 있다. 즉, 배열의 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다. 이러한 배열을 **희소 배열** 이라고 한다. 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.
> - 일반적인 배열은 인덱스로 요소에 빠르게 접근할 수 있다. 하지만 요소를 삽입, 삭제하는 경우 효율적이지 않다.
> - 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수 밖에 없는 구조이지만, 삽입, 삭제의 경우 일반적인 배열보다 빠른 성능을 기대할 수 있다.


## 용어

- Subarray: 배열 내 연속적인 값들의 범위
    - ex) `[2, 3, 6, 1, 5, 4]`에서 `[3, 6, 1]`은 subarray다. 하지만 `[3, 1, 5]`는 연속적인 값이 아니기 때문에 subarray가 아니다.
- Subsequence: 요소의 위치를 변경하는 것이 아닌 일부 요소를 삭제 했을 때 얻을 수 있는 배열이다.
    - ex)  `[2, 3, 6, 1, 5, 4]`에서 `[3, 1, 5]`은 subsequence다. 하지만 `[3, 5, 1]`는 요소의 순서가 변경됐기 때문에 subsequence가 아니다.   


## 시간 복잡도

| **동작** | **Big-O** | **비고** |
| --- | --- | --- |
| 접근 | O(1) |  |
| 검색 | O(n) |  |
| 정렬된 배열 검색 | O(log(n)) |  |
| 삽입 | O(n) | 삽입시 모든 subsequent 요소들을 오른쪽으로 밀어야 한다. |
| 종점 삽입 | O(1) | 배열의 종점에 삽입을 할 경우 요소들의 shifting이 일어나지 않는다. |
| 삭제 | O(n) | 삭제시 모든 subsequent 요소들을 왼쪽으로 밀어야 한다. |
| 종점 삭제 | O(1) | 배열의 종점 요소를 삭제 할 경우 요소들의 shifting이 일어나지 않는다. |


## 기법

배열과 [[열\|열]]은 모두 sequences이기 때문에 여기 있는 대부분의 기술 들이 문자열 문제에도 적용이 된다.

#### Two pointers (투 포인터)
투 포인터는 말 그대로 2개의 포인터를 이용해 배열을 검색하는 방법이다. 이중 반복문을 사용하지 않고 시간 복잡도를 줄이고, 원하는 값에 도달하기 위해 사용할 수 있다.

- LeetCode
    - [Sort Colors](https://leetcode.com/problems/sort-colors/)
    - [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
- 백준
    - [2018번: 수들의 합 5](https://www.acmicpc.net/problem/2018)
    - [1940번: 주몽](https://www.acmicpc.net/problem/1940)
    - [1253번: 좋다](https://www.acmicpc.net/problem/1253)

#### Sliding window (슬라이딩 윈도우)
슬라이딩 윈도우 기법을 마스터하면 많은 subarray/substring 문제에 적용을 할 수 있다. 이 기법은 두 개의 포인터가 같은 방향으로 움직이므로 각각의 값들이 최대 2번씩 방문되는 것을 보장하고, 시간 복잡도는 `O(n)`이 된다.
투 포인터와 다른 점은 처음과 끝을 가리키는 포인터가 같이 움직인다는 점이다.

- LeetCode
    - [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
    - [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
    - [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
- 백준
    - [DNA 비밀번호](https://www.acmicpc.net/problem/12891)
    - [11003번: 최솟값 찾기](https://www.acmicpc.net/problem/11003)
    

#### Traversing from the right
배열을 무조건 왼쪽에서부터 순회할 필요는 없다. 오른쪽에서부터 순회하는 기법이다.

- LeetCode
    - [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
    - [Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)
    

#### 배열 정렬
만약 배열이 정렬되어 있다면 binary search(이진 탐색)이 가능하다. 그러면 시간 복잡도는 `O(n)` 보다 빠를 수 있다.

배열의 순서가 중요하지 않다면, 배열을 먼저 정렬하는게 문제를 간단하게 하는데 도움이 된다.

- LeetCode
    - [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
    - [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

#### Pre-computation
subarray의 합이나 곱을 계산하는게 포함되어있는 문제라면 누적합, 부분합 등을 사전에 계산해놓고 이용하면 유용할 수 있다.

- LeetCode
    - [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
    - [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

#### Index as a hash key
- LeetCode
    - [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
    - [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## 필수 문제
- LeetCode
    - [Two Sum](https://leetcode.com/problems/two-sum/)
    - [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
    - [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
    - [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)


## 추천 연습 문제
- LeetCode
    - [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
    - [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
    - [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
    - [3Sum](https://leetcode.com/problems/3sum/)
    - [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
    - [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

---
### 참조
> Do it 알고리즘 코딩 테스트 - Java 편
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)



> 참조
> Do it 알고리즘 코딩 테스트 - Java 편
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)



---
> 참조
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)





---
> 참조
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)