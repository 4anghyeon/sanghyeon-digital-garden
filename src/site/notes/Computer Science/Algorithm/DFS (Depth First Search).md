---
{"tags":["알고리즘"],"dg-publish":true,"dg-hide":true,"permalink":"/computer-science/algorithm/dfs-depth-first-search/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---


# 깊이 우선 탐색 (DFS)

![Depth First Search.gif|400](/img/user/Computer%20Science/Data%20Structure/Depth%20First%20Search.gif)
DFS는 [[Computer Science/Data Structure/그래프 (Graph)\|그래프 (Graph)]] 전체를 탐색하는 방법 중 하나로, 현재 노드와 연결된 노드들을 하나씩 갈 수 있는지 검사후, 특정 노드로 갈 수 있다면 그 노드에서 재귀 함수를 통해 더 이상 방문할 노드가 없을 때 까지 아래로 내려가며 같은 행위를 반복한다. 

DFS는 스택이나 재귀 함수를 통해 구현한다. (보통 재귀 함수를 통해 구현)
재귀 함수를 사용할 경우 무한루프에 빠지는 것을 막기 위해 반드시 방문한 노드를 표시한다.

DFS는 리프 노드에 도달 후 다시 부모 노드로 돌아가야 하는데, 이 과정을 backtracking이라 한다.

## 장점
- 가장 직관적이고 구현하기 쉬운 탐색 방법이다.
- 단지 현 경로상의 노드들만을 기억하면 되므로 저장공간의 수요가 비교적 적다.

## 단점
- 깊이가 지나치게 깊어지면 메모리 비용을 예측하기 힘들다.
- 찾은 해가 최단 경로임을 보장할 수 없다.


# 구현 
- JavaScript
- 그래프를 인접리스트로 표현
- 재귀 함수 사용

```js
// 편의를 위해 graph의 0 번째 index는 비워둠  
const graph = [[], [2, 5, 9], [1, 3], [2, 4], [3], [1, 6, 8], [5, 7], [6], [5], [1, 10], [9]];
// 방문한 정보를 담을 array 생성
const visited = [];  
  
const dfs = (graph, vertex, visited) => {  
  visited[vertex] = true; // 방문한 노드 표시  
  console.log(vertex);  
  // 인접 행렬 방문  
  for (let adjacentVertex of graph[vertex]) {  
    if (!visited[adjacentVertex]) {  
      dfs(graph, adjacentVertex, visited); // 방문하지 않았다면 방문  
    }  
  }  
}  
  
dfs(graph, 1, visited); // 1 2 3 4 5 6 7 8 9 10
```