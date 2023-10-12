---
{"tags":["JavaScript"],"dg-publish":true,"dg-hide":true,"permalink":"/language/java-script/java-script/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# 탄생
**1995년** 웹 브라우저 시장을 지배하고 있던 넷스케이프 커뮤니케이션즈가 웹 사이트를 동적으로 만들기 위한 시도로 Brendan Eich가 개발하였다.

처음 부터 이름이 JavaScript였던 것은 아니다. 첫 버전은 "Mocha" 라고 불렸고, 이후 "LiveScript"로 이름이 바뀌었다. 이후 Java의 인기를 빌어 "JavaScript"라는 이름으로 바뀌었다 (*즉, Java와 JavaScript는 전혀 관련이 없다*). 

![Pasted image 20231012132924.png|400](/img/user/Language/JavaScript/Pasted%20image%2020231012132924.png)
(JavaScript 최초의 버전이 탑재된 Netscape2.0)

# 표준화의 시작
**1996년** 마이크로소프트에서 JavaScript의 최초의 파생언어인 JScript를 발표한다. JScrpit는 IE에 탑재하기 위해 만들어졌다.
두 언어는 거의 비슷했지만, 완전히 같지는 않았다. 그렇기 때문에 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 이슈들이 발생했다.
이에 Netscape는 JavaScript의 표준화가 필요하다는 점을 인식하고 [ECMA International](https://www.ecma-international.org/)에 제안서를 제출했다.

![Pasted image 20231012140507.png|300](/img/user/Language/JavaScript/Pasted%20image%2020231012140507.png)
**1997년** ECMA-262라 불리는 ECMAScript 초판이 발행되었다.

# XMLHttpRequest
**1999년** 마이크로소프트의 아웃룩 웹 액세스 개발자들이 만든 기술로 IE5.0에 탐재되며 처음 공개되었다. XMLHttpRequest(XHR)은 훗날 등장하는 Ajax 기술의 근간이 된다.
이전의 웹페이지는 화면이 전환될 때마다 새로운 HTML을 요청하고, 전송받아 다시 렌더링하는 방식으로 진행됐다. 하지만 이 기술의 등장으로 서버와 브라우저가 비동기(asynchronous)방식으로 데이터를 교환할 수 있게 됐다. 즉, 웹페이지에서 변경이 필요한 일부분만 렌더링할 수 있게되었다.

![Pasted image 20231012142541.png|200](/img/user/Language/JavaScript/Pasted%20image%2020231012142541.png)
**2005년** Jesse James Garrett에 의해 Ajax라는 용어가 등장한다. Ajax기술은 이전에도 2004년 Gmail, 2006년 구글맵 등에 이미 쓰였지만 공식적인 용어가 등장한건 2006년 기사를 통해서다.

# jQuery의 등장
![Pasted image 20231012143013.png|200](/img/user/Language/JavaScript/Pasted%20image%2020231012143013.png)

**2006년** John Resig에 의해 HTML DOM을 쉽게 조작하고, 제어할 수 있게 해주는 라이브러리 jQuery가 등장했다. 이로 인해 개발자가 DOM을 더 쉽게 만질 수 있게 되었고, 크로스브라우징 이슈도 어느정도 해결 됐다.


# Google Chrome과 V8 엔진
**2008년** 구글이 구글 크롬 베타버전과 함께 새로운 자바스크립트 엔진인 V8을 공개했다. V8엔진의 등장으로 자바스크립트의 성능이 크게 증가하였고, 데스크톱 애플리케이션과 유사한 UX를 제공할 수 있는 언어로 정착했다.

[구글 크롬과 함께 나온 만화책](https://www.google.com/googlebooks/chrome/index.html)


# Node.js의 등장
![Pasted image 20231012144119.png|200](/img/user/Language/JavaScript/Pasted%20image%2020231012144119.png)
**2009년** Ryan Dahl이 구글의 V8엔진으로 빌드된 자바스크립트 런타임 환경을 발표했다. 이로써 백엔드 영역에서도 자바스크립트로 개발을 할 수 있게 되었고, 하나의 언어로 프론트와 서버를 모두 개발할 수 있는 시대가 되었다.

# SPA 라이브러리의 등장
**2011년** 구글의 AngularJS가 등장 했다.

![Pasted image 20231012144136.png|200](/img/user/Language/JavaScript/Pasted%20image%2020231012144136.png)
**2013년** Tom Occhino와 Jordan Walke가 React를 오픈소스화 시켰다.

**2014년** Evan You의 Vue.js가 등장 했다.

여러 SPA 라이브러리, 프레임워크들의 등장으로 프론트엔드 개발에 유연성과 확장성등 생산성이 크게 증가했다.

# ES6라 불리는 ES2015 업데이트
**2016년** `let`, `const` 키워드, 클래스 선언, `promise`등 많은 기능이 개선되고, 추가 되었다. 이 업데이트를 기점으로 ECMAScript가 매년 정기적으로 업데이트 된다.


# 그리고 현재...
자바스크립트는 지금도 꾸준하게 보완되고 있고, 널리 퍼지고 있다. 그에 따라 TypeScript등 JavaScript의 대체제가 탄생하고 여러가지 라이브러리, 프레임워크가 등장하고 있다. 더 자세한 자바스크립트와 브라우저의 Timeline은 갓브레인의 페이지에서 확인할 수 있다.
https://www.jetbrains.com/ko-kr/lp/javascript-25/
