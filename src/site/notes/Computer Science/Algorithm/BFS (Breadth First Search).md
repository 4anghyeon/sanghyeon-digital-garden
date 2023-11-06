---
{"tags":["알고리즘"],"dg-publish":true,"dg-hide":true,"permalink":"/computer-science/algorithm/bfs-breadth-first-search/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---


# 너비 우선 탐색  (BFS)
![BFS Algorithm (1)..gif|400](/img/user/Computer%20Science/Algorithm/BFS%20Algorithm%20(1)..gif)
BFS는 [[Computer Science/Data Structure/그래프 (Graph)\|그래프 (Graph)]] 전체를 탐색하는 방법 중 하나로, 시작 정점에서 부터 정점에 인접한 모든 정점들을 우선 방문하는 방법이다.
더 이상 방문하지 않은 정점이 없을 때까지 방문하지 않은 모든 정점들에 대해 BFS를 적용한다.

BFS는 [[Computer Science/Data Structure/큐 (Queue)\|큐 (Queue)]]를 이용하여 구현한다.

## 장점
- 시간/공간 복잡도에 있어서 안정적이다.
- 간선의 비용이 모두 같을 경우, 최단 경로를 찾을 수 있다.
- DFS와 달리, 그래프에 사이클이 있더라도 탐색이 종료된다.

## 단점
- DFS에 비해 코드 구현이 어렵다.
- 경로가 깊어질수록 보다 많은 기억 공간을 필요로 하게 된다.

# 구현 
- JavaScript
- 그래프를 인접리스트로 표현
- 큐 사용


```js
// 편의를 위해 graph의 0 번째 index는 비워둠  
const graph = [[], [2, 3, 4], [1, 5], [1, 6, 7], [1, 8], [2, 9], [3, 10], [3], [4], [5], [6]];  
  
// 방문해야 할 노드를 담은 que  
let queue = [];  
  
// 방문한 노드  
const visited = new Set();  
  
const bfs = (graph, node) => {  
  queue = [node];  
  
  // queue가 빌때까지 반복  
  while (queue.length > 0) {  
    // 방문한 목록에 노드 추가  
    visited.add(node);  
  
    // queue 앞에서 제거  
    queue.shift();  
  
    // 현재 노드의 인접 노드들을 검색하며 방문하지 않은 노드라면 queue에 넣는다.  
    const adjacencyNode = graph[node];  
    adjacencyNode.forEach((node) => {  
      if (!visited.has(node)) {  
        queue.push(node);  
      }  
    });  
  
    // queue에 가장 앞에 노드를 다음 탐색할 노드로 지정  
    node = queue[0];  
  }  
};  
  
bfs(graph, 1);  
  
console.log(visited);
```