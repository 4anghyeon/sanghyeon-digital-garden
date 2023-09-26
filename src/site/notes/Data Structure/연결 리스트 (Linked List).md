---
{"dg-publish":true,"permalink":"/data-structure/linked-list/","dgPassFrontmatter":true,"created":"","updated":""}
---

![Linked List Thumbnail.png](/img/user/Data%20Structure/Linked%20List%20Thumbnail.png)
# 연결 리스트 (Linked List)
[[Data Structure/배열 (Array)\|배열]]과 마찬가지로, 연결 리스트는 연속적인 데이터를 보여주는 자료구조이다. 메모리에 연속된 공간에 저장되는 배열과는 달리 연결 리스트는 메모리의 물리적 배치에 순서가 지정되어있지 않다.

연결 리스트는 인덱스를 갖지 않으며 데이터와 링크 필드를 갖고 있는 노드로 구성되어 있다. 링크에는 다음에 올 노드의 위치를 담고 있다.

### 이점
연결 리스트는 데이터를 삽입, 삭제할 경우 다음 노드의 위치를 끊고 새로운 노드의 위치를 넣으면 되므로 구조를 재정렬할 필요가 없다.

### 단점
하지만 특정 데이터를 검색할 때는 Head(가장 첫 번째 노드)에서부터 순차적으로 검색해가며 자료를 찾아야 한다.

# 연결 리스트의 종류
### 싱글 연결 리스트
각각의 노드가 다음 노드를 가리키고, 마지막 노드는 `null`을 가리킨다.

### 이중 연결 리스트
각각의 노드가 다음 노드와 이전 노드를 가리키는 두 개의 포인터를 가지고 있다. 마지막 노드의 다음 노드는 `null`을 가리킨다.

### 순환 연결 리스트
마지막 노드가 처음 노드를 가리키고, 첫 번째 노드의 이전이 마지막 노드를 가리키는 연결 리스트를 말한다.

# 시간 복잡도
| 동작 | Big-O |
| ---- | ----- |
| 접근 | O(n)  |
| 검색 | O(n)  |
| 삽입 | O(1)  |
| 삭제 | O(1)  |

# 구현
자바스크립트로 구현하면 다음과 같다.
```js
// Linked List 구현
class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }

    addFirst(data) {
        if (!this.head) this.head = new Node(data)
        else {
            const node = new Node(data, this.head);
            node.next = this.head;
            this.head = node;
        }
        this.size++;
    }

    addLast(data) {
        let current = this.head;
        if (!current) current = new Node(data);
        else {
            while(current.next) {
                current = current.next;
            }
            current.next = new Node(data);
        }
        this.size++;
    }

    add(data, index = null) {
        if (!this.head) this.head = new Node(data);
        else {
            let current = this.head;
            let i = 0;
            if (index > this.size) throw "out of index"
            if (index !== null) {
                if (index === 0) {
                    let temp = this.head;
                    this.head = new Node(data);
                    this.head.next = temp;
                    this.size++;
                    return;
                }
            }
            while (current.next !== null) {
                i++;
                if (index === i) {
                    let temp = current.next;
                    let node = new Node(data);
                    node.next = temp;
                    current.next = node;
                    break;
                }
                current = current.next;

            }
            if (index === null) current.next = new Node(data);
        }
        this.size++;
    }

    remove(index = null) {
        let current = this.head;
        if (current) {
            let prev = current;
            let i = 0;
            if (index !== null) {
                if (index === 0) {
                    this.head = current.next
                    this.size--;
                    return;
                }
            }
            while (current.next) {
                i++;
                prev = current;
                current = current.next;
                if (i === index) break;
            }
            prev.next = current.next;
            this.size--;
        }
    }

    get(index = null) {
        let current = this.head;
        let i = 0;
        while (current) {
            if (index === null) console.log(current.data);
            else {
                if (i === index) {
                    console.log(current.data);
                    break;
                }
            }
            current = current.next;
            i++;
        }
    }
}
let list = new LinkedList();
list.add("1")
list.add("2")
list.addFirst("3")
list.addLast("4")
list.add("5", 1)
list.remove(2);
list.get();
```


### 일반적인 루틴
많은 연결 리스트 질문에 대한 솔루션이 아래 중 하나를 이용한다.
- 연결 리스트의 노드 개수
- 제자리에서 연결 리스트 뒤집기
- 투 포인터를 사용해 연결 리스트의 중간 노드를 찾기
- 두 개의 연결 리스트 합치기


# 기법
### 보조/더미 노드
헤드 노드와 테일 노드에서 작업을 수행해야 할 때 더미 노드를 추가하면 많은 엣지 케이스를 처리하는데 도움이 될 수 있다. 더미 노드가 있으면 NPE를 피할 수 있다. 하지만 작업이 전부 끝나고 더미 노드를 제거하는 것을 잊으면 안된다.

### 투 포인터
[[Algorithm/투 포인터 (Two Pointers)\|투 포인터]]는 연결 리스트에서 일반적인 접근 방법이다.
- 순환 탐지: 하나의 포인터가 다른 하나보다 두배 만큼 증가하고, 만약 두 포인터가 만나게 된다면 순환이다.
- 중간 노드 얻기: 하나의 포인터가 다른 하나보다 두배 만큼 증가하고, 빠른 포인터가 끝에 다다르면 나머지 포인터가 중간을 가리킨다.

# 필수 문제
- LeetCode
	- [Reverse a Linked List](https://leetcode.com/problems/reverse-linked-list/)
	- [Detect Cycle in a Linked List](https://leetcode.com/problems/linked-list-cycle/)

# 추천 문제
- LeetCode
	- [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
	- [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
	- [Remove Nth Node From End Of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
	- [Reorder List](https://leetcode.com/problems/reorder-list/)