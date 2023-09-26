---
{"dg-publish":true,"permalink":"/algorithm/two-pointers/","dgPassFrontmatter":true,"created":"","updated":""}
---

![two pointers thumbnail.png](/img/user/Algorithm/two%20pointers%20thumbnail.png)
# 투 포인터
투 포인터는 리스트에서 두 개의 포인터를 이용하여 시간 복잡도를 개선하기 위한 알고리즘이다. 투 포인터의 시간 복잡도는 O(n)이다.

보통 리스트에서 연속된 부분합을 구할 때 사용한다.

startIndex와 endIndex를 정하고 특정 조건에 따라 둘을 증가시키면서 조건을 만족할 때 까지 반복한다.

예를 들어 10까지의 자연수 1~10 사이의 연속된 숫자의 합이 10이 되는 갯수를 확인하는 문제를 살펴보자.

![two pointers.png](/img/user/Algorithm/two%20pointers.png)