---
title: "🤍 What is RESTful API?"
date: 2024-01-19
categories: [Spring, Spring Boot]
render_with_liquid: false
tags:
    [
        Spring,
        Java,
        Spring Boot,
        코딩자율학습단
    ]
---
# REST란?
> 월드 와이드 웹의 아키텍처 설계 및 개발을 안내하기 위해 만들어진 소프트웨어 아키텍처 스타일

- 분산형 인터넷 규모 하이퍼미디어 시스템의 아키텍처가 어떻게 작동해야 하는지에 대한 일련의 **제약 조건**을 정의
- 통일된 인터페이스, 구성 요소의 독립적 배포, 구성 요소 간의 상호 작용 확장성을 강조
- 캐싱을 촉진하여 사용자가 인식하는 **대기 시간을 줄이고 보안을 강화**하며 레거시 시스템을 **캡슐화**하는 계층형 아키텍처를 생성한다.

| ![An entity-relationship model of the concepts expressed in the REST architectural style](/assets/img/posts/2024-01-19-1.png) | 
|:--:| 
| REST 아키텍처 스타일로 표현된 Entity-관계 모델 |

즉, **인터넷과 같은 복잡한 대규모 고성능 통신을 안정적으로 지원할 수 있도록 만들어진 소프트웨어 아키텍처**다.

## REST의 구성
⚙️ 자원(Resource) - URI

⚙️ 행위(Verb) - HTTP Method

⚙️ 표현(Representations) - JSON, XML

## REST Constraints
> REST는 공식적으로 다음과 같은 제약 조건을 따라야 한다.

### Client-Server Architecture
- 클라이언트-서버 디자인 패턴은 사용자 인터페이스 문제와 데이터 저장소 문제를 분리하는 문제 분리 원칙을 적용
- 구성 요소의 독립성 향상

### Stateless
- 서버가 세션 정보를 유지하지 않음
- 들어오는 요청만을 단순히 처리하면 되므로 서비스의 자유도가 높음
- 대용량 애플리케이션에 이상적이며 세션 정보 보존으로 인해 발생하는 서버 부하 예방

### Cacheability
- 캐시 가능
- HTTP 기존 웹표준을 그대로 사용하기 때문에 HTTP가 가진 캐싱 기능 적용 가능
- HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현 가능

### Layered system
- REST 서버는 **다충 계층**으로 구성될 수 있고 로드 밸런싱, 암호화 계층을 추가해 **구조상의 유연성**을 둘 수 있고 Proxy, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 함

### Code on demand (optional)
- 서버가 클라이언트에게 실행 가능한 코드를 다운로드하여 실행할 수 있는 기능을 제공
- 유일하게 optional한 원칙이며, 현재는 **보안 및 호환성 문제**로 잘 사용되지 않음

### Uniform interface
- URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일

# RESTful API란?

> REST 아키텍처 제약 조건을 준수하는 애플리케이션

- 서버의 자원을 **클라이언트에 구애받지 않고 사용**할 수 있게 하는 설계 방식. **HTTP 요청에 대한 응답으로 서버의 자원을 반환**한다.
- 반환되는 데이터는 모든 기기에서 통용될 수 있다.
- 웹에서 실제로 REST 스타일을 모두 지키는 데는 어려움이 있다. 따라서, 모든 REST 제약 조건을 지키지 못하더라도 RESTful API라고 불리는 경우가 많다.
- REST는 그 자체로는 표준이 아니지만 RESTful 구현은 **HTTP, URI, JSON 및 XML**과 같은 표준을 사용한다.
- 서버는 클라이언트의 요청에 대한 응답으로 화면이 아닌 데이터를 전송한다.
- 이때 사용하는 응답 데이터가 `JSON(Javascript Object Notation)`이다.

❔ **API(Application Programming Interface)**

- 애플리케이션을 간편히 사용할 수 있게 하는 미리 정해진 일종의 약속으로 사용자와 프로그램 간 상호 작용을 돕는다.
- REST API는 클라이언트와 서버 사이의 상호 작용, 즉 **HTTP 요청에 따른 JSON 응답에 대한 약속**이다.

❔ **JSON**
> 키와 값으로 구성된 정렬되지 않은 속성(property)의 집합

```json
{
    "name": "망고",
    "breeds": "골든리트리버",
    "age": 2
}
```

## RESTful API 디자인 가이드
💡 **URI는 정보의 자원**을 표현해야 한다.

```
GET /members/delete/1
```

리소스명은 동사보다는 명사를 사용한다.

위와 같은 방식은 REST를 제대로 적용하지 않은 URI다. URI는 자원을 표현하는데 중점을 두어야 하기 때문에 `delete`와 같은 행위에 대한 표현이 들어가선 안된다.

💡 자원에 대한 행위는 **HTTP Method(GET, POST, PUT, DELETE)로 표현**한다.

```
DELETE /members/1
```

위 코드를 HTTP Method를 통해 수정해 아래와 같이 표현해야 한다.

정보를 가져오는 경우 GET, 추가의 경우 POST Method를 사용하여 표현한다.

위와 같이 URI는 자원을 표현하는 데에 집중하고 행위에 대한 정의는 HTTP Method를 통해 하는 것이 RESTful API를 설계하는 중심 규칙이다.

## RESTful API의 장점
- HTTP 프토토콜의 표준 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- **HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능**하다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

## RESTful API의 단점
- 표준이 존재하지 않아 관리가 어렵다.
- HTTP Method 형태가 제한적이다.
- 안티 패턴으로 설계될 가능성이 높다.


# 🔗 참고자료
* [코딩 자율학습 스프링 부트 3 자바 백엔드 개발 입문](https://www.gilbut.co.kr/book/view?bookcode=BN003778)
* [Wikipedia - REST API](https://en.wikipedia.org/wiki/REST)
* [NHN Cloud Meetup - REST API 제대로 알고 사용하기](https://meetup.nhncloud.com/posts/92)

# 📚 더 공부해야 할 것
* 안티 패턴