---
{"dg-publish":true,"permalink":"/data-structure/queue/","dgPassFrontmatter":true,"created":"","updated":""}
---

![queue thumbnail.png](/img/user/Data%20Structure/queue%20thumbnail.png)
큐는 선입선출(FIFO, First-In-First-Out)의 구조를 가지는 자료구조다. 한쪽 끝에 요소를 추가  (enqueue)하고, 다른 쪽 끝에서 요소를 제거(dequeue) 한다. 큐는 [[Data Structure/배열 (Array)\|배열]] 또는 단일 [[Data Structure/연결 리스트 (Linked List)\|연결 리스트]]를 통해서 구현할 수 있다.
[[Algorithm/BFS (Breadth First Search)\|BFS 알고리즘]] 또한 일반적으로 queue를 이용하여 구현한다.

![queue example.png|500](/img/user/Data%20Structure/queue%20example.png)

## 시간 복잡도
| 동작   | Big-O |
| ------- | ----- |
| Enqueue | O(1)  |
| Dequeue | O(1)  |
| Front   | O(1)  |
| Back    | O(1)  |
| isEmpty | O(1)  |

## 자바스크립트로 구현
```js
class QueueNode {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}


class Queue {

  constructor() {
    this.front = null;
    this.back = null;
    this.size = 0;
  }

  // Queue의 뒤에 데이터 추기
  add(value) {
    this.size++;
    let node = new QueueNode(value);
    if (!this.front) {
      this.front = node;
    } else if (!this.back) {
      this.front.next = node;
      this.back = node;
    } else {
      let back = this.back;
      back.next = node;
      this.back = node;
    }
  }

  // 맨 앞 데이터 출력
  peek() {
    if (this.front) {
      return this.front.value;
    } else {
      return null;
    }
  }

  // 맨 앞 데이터 출력 하고 삭제
  poll() {
    if (this.front) {
      this.size--;
      let front = this.front;
      this.front = front.next;
      return front.value;
    } else {
      return null;
    }
  }
}

const queue = new Queue();

queue.add(1);
queue.add(2);
queue.add(4)
console.log(queue.peek());
console.log(queue.poll());
console.log(queue.poll());
console.log(queue.poll());
console.log(`size: ${queue.size}`);
```

## 필수 문제
- LeetCode
	- [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues)

## 추천 연습 문제
- LeetCode
	-  [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks)
	- [Design Hit Counter (LeetCode Premium)](https://leetcode.com/problems/design-hit-counter)


---
> 참조
> [Array cheatsheet for coding interviews | Tech Interview Handbook](https://www.techinterviewhandbook.org/algorithms/array/)