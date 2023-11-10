---
{"tags":["CS","WEB"],"dg-publish":true,"dg-hide":true,"permalink":"/computer-science/web/rest/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---

# REST란
- <span style='color:#eb3b5a'>Re</span>presentational <span style='color:#eb3b5a'>S</span>tate <span style='color:#eb3b5a'>T</span>ransfer
- 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든것을 의미한다.
- REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍쳐 스타일이다.
- REST는 네트워크 상에서 Client와 Server사이의 통신 방식 중 하나이다.
- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 자원에 대한 CRUD Option을 적용하는 것을 의미한다.

# REST 구성요소
1. **자원(Resource): URI**
    - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
    - 자원을 구별하는 ID는 '/group/:group_id' 와 같은 HTTP URI 다.
    - Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태에 대한 조작을 Server에 요청한다.
2. **행위(Verb): HTTP Method**
    - GET, POST, PUT, DELETE
3. **표현**
    - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
    - JSON 혹은 XML을 통해 데이터를 주고 받는 것이 일반적이다.

# REST 특징
1. **Server - Client**
    - Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    - Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
    - 서로 간 의존성이 줄어든다.
2. **Stateless (무상태)**
    - HTTP 프로토콜은 Stateless Protocol 이므로 REST 역시 무상태성을 갖는다.
    - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
        - 각 API 서버는 Client의 요청만을 단순 처리한다.
        - 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
3. **Cacheable**
	- 응답을  캐시할  수 있도록 한다.
4. **Layered System (계층화)**
    - Client는 REST API Server만 호출한다.
    - REST Server는 다중 계층으로 구성될 수 있다.
        - API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
5. **Code-On-Demand(optional)**
6. **Uniform Interface**
      - 모든 리소스에 대하여 동일한 인터페이스를 사용한다. 예를 들어, GET은 리소스의 정보를 가져오는데만, POST는 새 리소스를 사용하는데만 사용한다.

# REST의 장단점

## 장점

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

## 단점
- 표준이 존재하지 않는다.
- 사용할 수 있는 메소드가 4가지 밖에 없다.

# REST가 필요한 이유
- 애플리케이션 분리 및 통합
- 다양한 클라이언트의 등장
- 최근의 서버 프로그램은 다양한 브라우저와 안드로이드, 아이폰과 같은 모바일 디바이스에서도 통신을 할 수 있어야 한다.

---
# What is REST API?

- API(Application Programming Interface): 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환가능 하도록 하는 것
- REST 기반으로 서비스 API를 구현한 것
- 최근 OpenAPI, 마이크로 서비스등을 제공하는 업체 대부분은 REST API를 제공한다.

# REST API 설계 규칙

1. URI는 정보의 자원을 표현해야 한다.
    1. resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
    2. resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다.
    3. resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다.
    4. resource의 스토어 이름으로는 복수 명사를 사용해야 한다.
        - ex) _GET /Member/1_ → _GET /mebers/1_
2. 자원에 대한 행위는 HTTP Method로 표현한다.
    1. URI에 HTTP Method가 들어가면 안된다.
        - ex) _GET /members/delete/1_ → _DELETE /members/1_
    2. URI에 행위에 대한 동사 표현이 들어가면 안된다. (즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)
        - ex) _GET /members/show/1_ → _GET /members_
        - ex) _GET /members/insert/2_ → _POST /members/2_
3. 슬래시 구분자(/ )는 계층 관계를 나타내는데 사용한다.
    - ex) *[http://restapi.example.com/house/apartments*](http://restapi.example.com/house/apartments*)
4. URI 마지막 문자로 슬래시(/ )를 포함하지 않는다.
    - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 한다.