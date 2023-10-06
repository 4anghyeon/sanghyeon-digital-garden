---
{"dg-publish":true,"dg-hide":false,"tags":["자료구조"],"permalink":"/computer-science/data-structure/trie/","dgPassFrontmatter":true,"noteIcon":""}
---

![trie tuhmbnail.png](/img/user/Computer%20Science/Data%20Structure/trie%20tuhmbnail.png)

트라이는 문자열을 효율적으로 검색하고 저장하기 위한 특별한 [[Computer Science/Data Structure/트리 (Tree)\|트리 (Tree)]]이다. 트라이 자료구조는 자동 검색이나 예측 문자같은 곳에 사용된다. 접두사 트리(prefix tree)라고 부르기도 한다.

트라이에서 노드의 모든 자식 노드들은 노드에 연결된 문자열의 공통 접두사를 공유한다. 루트는 빈 문자열을 갖는다. 

예를 들어 `["iphone", "ipad", "app", "apple", "appstore", "mac"]` 이라는 단어를 저장하는 트라이의 형태를 살펴보자.

![Pasted image 20231005101500.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231005101500.png)
빨간색으로 표시된 노드는 각 단어의 끝을 의미한다. 각 노드는 자손에 대한 포인터 목록과, 노드가 종류인지를 나타내는 변수를 포함한다.

# 삽입
"math"라는 단어를 추가한다고 해보자. 루트 노드에서부터 시작하여 "m", "a", "t", "h"를 순서대로 찾는다. 하지만 "m", "a" 이후 "t"가 존재하지 않기 때문에 새로운 노드를 만들어 연결해준다.
![Pasted image 20231005103640.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231005103640.png)

# 삭제
삭제는 노드를 하나씩 삭제하면서 해당 노드가 자식 노드를 가지고 있는지 확인해주는 과정이 필요하다. "apple"이라는 단어를 삭제할 때 단말 노드에서부터 위로 올라가면서 노드를 제거한다. 만약 자신 이외의 자식 노드를 포함하고 있으면 중단한다.
![Pasted image 20231005103958.png|450](/img/user/Computer%20Science/Data%20Structure/Pasted%20image%2020231005103958.png)


# 시간 복잡도
`m`은 문자열의 길이이다.

|동작|Big-O|
|---|---|
|검색|O(m)|
|삽입|O(m)|
|삭제|O(m)|
# 필수 문제
- LeetCode
	- [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree)


# 추천 연습 문제
- LeetCode
	- [Add and Search Word](https://leetcode.com/problems/add-and-search-word-data-structure-design)
	- [Word Break](https://leetcode.com/problems/word-break)
	- [Word Search II](https://leetcode.com/problems/word-search-ii/)


---

#자료구조 