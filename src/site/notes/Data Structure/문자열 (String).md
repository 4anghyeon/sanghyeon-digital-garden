---
{"dg-publish":true,"permalink":"/data-structure/string/","dgPassFrontmatter":true,"created":"","updated":""}
---

![string thumbnail.png](/img/user/Data%20Structure/string%20thumbnail.png)

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
excalidraw test
[[Data Structure/문자열 (String)\|문자열 (String)]] 


</div></div>


## 문자열 (String)
문자열은 문자들의 집합, 배열로서 엄밀하게 자료 구조라고 볼 수 없지만 [[Data Structure/배열 (Array)\|배열 (Array)]]과 유사하기 때문에 자료 구조의 하위 항목으로 묶었다.

문자열을 검색하기 위한 일반적인 자료 구조는 다음과 같이 있다.
- [Trie/Prefix Tree](https://en.wikipedia.org/wiki/Trie)
- [Suffix Tree](https://en.wikipedia.org/wiki/Suffix_tree)

그리고 문자열 관련 알고리즘은 다음과 같이 있다.
- [Rabin Karp](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm) for efficient searching of substring using a rolling hash
- [KMP](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm) for efficient searching of substring

## 시간 복잡도
문자열은 배열과 유사하기 때문에 시간 복잡도 또한 같다.

|동작|Big-O|
|---|---|
|접근|O(1)|
|검색|O(n)|
|삽입|O(n)|
|삭제|O(n)|

#### 다른 문자열과 관련된 작업

|Operation|Big-O|Note|
|---|---|---|
|부분 문자열 찾기|O(n.m)|This is the most naive case. There are more efficient algorithms for string searching such as the [KMP algorithm](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)|
|문자열 연결|O(n + m)||
|문자열 자르기|O(m)||
|문자열 분리|O(n + m)||
|양 끝 공백 제거|O(n)||

## 기법

### 문자 세기

문자열에서 특정 문자의 개수를 셀 때는 보통 hash table이나 map을 사용한다. 만약 언어에서 문자를 검색하는 함수를 내장하고 있다면 그것을 사용하자

### 아나그램 (Anagram)

아나그램은 원래의 모든 글자중 하나씩만 사용하여 새로운 단어나 구를 만드는 놀이이다.

두 문자열이 아나그램인지 확인하는 방법은 다음과 같이 있다.

- 두 문자열을 모두 정렬하고, 정렬 결과가 같은지 확인한다. `O(n.log(n))`의 시간 복잡도를 가진다.
- 각 문자를 소수에 대응 시키고, 그 숫자들을 모두 곱했을 때 아나그램은 같은 배수를 가져야 한다. (소인수 분해) `O(n)`의 시간 복잡도를 가진다.
- 각 문자가 등장한 개수를 확인한다. `O(n)`의 시간 복잡도를 가진다.

### Palindrome

Palindrome은 앞으로 읽어도, 뒤로 읽어도 거꾸로해도 같은 문자열인 단어를 말한다.

문자열이 Palindrome인지 확인하는 방법은 다음과 같이 있다.

- 문자열을 뒤집고 같은지 확인해 본다.
- 투 포인터를 사용하여 하나는 앞에서 부터, 하나는 뒤에서 부터 시작하여 같은지 확인한다.

## 필수 문제

- LeetCode
    - [Valid Anagram](https://leetcode.com/problems/valid-anagram)
    - [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
    - [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## 추천 연습 문제

- LeetCode
    - [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
    - [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string)
    - [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)
    - [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
    - [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)