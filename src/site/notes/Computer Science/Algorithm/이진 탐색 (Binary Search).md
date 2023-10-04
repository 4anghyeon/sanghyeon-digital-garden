---
{"dg-publish":true,"permalink":"/computer-science/algorithm/binary-search/","dgPassFrontmatter":true,"noteIcon":"","created":"","updated":""}
---

이진 탐색은 데이터가 정렬되어 있는 상태에서 원하는 값을 찾아내는 알고리즘이다. 대상 데이터의 중앙값과 찾고자 하는 값을 비교해 검색 범위를 절반씩 줄이면서 대상을 찾는다.

|기능|특징|시간 복잡도|
|----|----|----|
|타깃 데이터 탐색|중앙값 비교를 통한 범위 축소 방식|`O(logN)`|

이진 탐색은 정렬 데이터에서 원하는 데이터를 검색할 때 사용되는 가장 일반적인 알고리즘 이다.

### 핵심 이론
이진 탐색은 오름차순으로 정렬된 데이터에서 다음 4가지 과정을 반복한다.

1. 현재 데이터셋의 중앙값(median)을 선택한다.
2. 중앙값이 타겟값보다 클 때 중앙값 기준으로 왼쪽 데이터셋을 선택한다.
3. 중앙값이 타겟값보다 작을 때 중앙값 기준으로 오른쪽 데이터셋을 선택한다.
4. 1 ~ 3을 반복하며 중앙값 = 타겟값이면 탐색을 종료한다.

- 타겟이 9일 경우 예시
![Pasted image 20230925142928.png](/img/user/Computer%20Science/Algorithm/Pasted%20image%2020230925142928.png)


## 관련 문제
- 백준
	- [1920번: 수 찾기 (acmicpc.net)](https://www.acmicpc.net/problem/1920)
	- [2343번: 기타 레슨 (acmicpc.net)](https://www.acmicpc.net/problem/2343)
	- [1300번: K번째 수 (acmicpc.net)](https://www.acmicpc.net/problem/1300)



---
> 참조
> Do it 알고리즘 코딩 테스트 - Java 편
