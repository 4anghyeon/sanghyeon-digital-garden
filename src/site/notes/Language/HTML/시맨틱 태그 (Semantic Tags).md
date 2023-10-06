---
{"dg-publish":true,"dg-hide":true,"tags":["HTML"],"permalink":"/language/html/semantic-tags/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

컴퓨터 과학에서 시맨틱(Semantic)은 의미론이라는 단어로 의미론은 프로그래밍 언어에서 문장,명령등에 계산적인 **의미**를 할당한다.

시맨틱은 다음과 같은 역할을 수행한다.

1. 프로그램이 단순한 수행이 아니라 자신이 무엇을 수행하는지 의도하는 바를 파악하도록 도움을 준다.
2. 컴파일러와 인터프리터가 소스 코드를 기계 코드로 변환할 때 도움을 준다.
3. 프로그램의 구조와 동작을 설계할 때 도움을 준다.


# 시맨틱 웹
이 시맨틱이 웹으로 넘어오면서는 다음과 같은 기능과 의미를 가진다.

> **시맨틱 웹**(Semantic Web)은 '의미론적인 웹'이라는 뜻으로,현재의 인터넷과 같은 분산환경에서 리소스(웹 문서, 각종 파일, 서비스 등)에 대한 [정보](https://ko.wikipedia.org/wiki/%EC%A0%95%EB%B3%B4 "정보")와 [자원](https://ko.wikipedia.org/wiki/%EC%9E%90%EC%9B%90 "자원") 사이의 관계-의미 정보(Semanteme)를 기계(컴퓨터)가 처리할 수 있는 [온톨로지](https://ko.wikipedia.org/wiki/%EC%98%A8%ED%86%A8%EB%A1%9C%EC%A7%80 "온톨로지") 형태로 표현하고, 이를 자동화된 기계(컴퓨터)가 처리하도록 하는 프레임워크이자 기술이다
> *출처: https://ko.wikipedia.org/wiki/%EC%8B%9C%EB%A7%A8%ED%8B%B1_%EC%9B%B9

즉, 웹에서 시맨틱 마크업을 사용하면 단순히 브라우저에 표시하는 그 이상의 의미를 지니게 되며 다음과 같은 장점이 있다.
- 검색 최적화에 도움이 된다.
- 시각 장애가 있는 사용자가 화면 판독기로 페이지를 탐색할 때 도움이 된다.
- 개발자에게 태그 안에 채워질 데이터 유형을 제시할 수 있다.

# HTML5에서의 시맨틱
의미론적인 구조를 가지고, 웹 페이지의 구조와 컨텐츠를 명확하게 표현하기 위한 태그들이 추가되었다.
예를 들어, HTML5이전에는 header임을 표현하기 위해 `<div id="header">` 혹은 `<div class="header">` 등으로 표현하였다. HTML5에는 `<header>`라는 태그가 추가되어 의미를 더 명확히 부여할 수 있게 되었다.

# 시맨틱 구조
일반적인 웹 구조를 시맨틱 태그를 이용하면 다음과 같이 표현할 수 있다.
![Pasted image 20231006194131.png|350](/img/user/Language/HTML/Pasted%20image%2020231006194131.png)

### header
소개 및 탐색에 도움을 주는 컨텐츠를 나타낸다. 제목, 로고, 검색 폼등의 요소 등이 포함될 수 있다.
![Pasted image 20231006191637.png](/img/user/Language/HTML/Pasted%20image%2020231006191637.png)

### nav
다른 페이지로의 링크를 보여주는 컨텐츠를 나타낸다. 메뉴, 목차, 색인 등이 포함될 수 있다.
![Pasted image 20231006191855.png](/img/user/Language/HTML/Pasted%20image%2020231006191855.png)

### aside
주요 내용과 간접적으로 관련된 컨텐츠를 나타낸다. 사이드바, 콜아웃 등이 포함될 수 있다.
![Pasted image 20231006192114.png|200](/img/user/Language/HTML/Pasted%20image%2020231006192114.png)


### section
HTML 문서의 독립적인 구획을 나타낸다. 보통은 제목을 포함하고 있다. 챕터, 내용, 연락처 정보 등이 포함될 수 있다.
![Pasted image 20231006192457.png](/img/user/Language/HTML/Pasted%20image%2020231006192457.png)

### article
독립적이고, 스스로 내용을 포함하고 있다. 따라서 독립적으로 구분하여 배포하거나 재사용할 수 있는 영역을 나타낸다. 게시판, 블로그 글, 뉴스 기사, 댓글 등이 포함될 수 있다.
![Pasted image 20231006193831.png](/img/user/Language/HTML/Pasted%20image%2020231006193831.png)


> [!NOTE] section안에 article? article 안에 섹션?
> 정의만으로는 무엇이 바깥에 와야하는지 정할 수 없다.
> 그때그때 알맞게 사용하도록하자.
> 

### footer
문서의 바닥글을 포함하는 요소이다. 저작권 표시, 소유권 표시, 연락처 정보, 사이트 맵등이 포함될 수 있다.
![Pasted image 20231006195723.png](/img/user/Language/HTML/Pasted%20image%2020231006195723.png)

