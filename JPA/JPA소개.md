지금 시대는 **객체**를 **관계형 DB**에 관리.

SQL 중심적인 개발의 문제점

- 무한 반복, 지루한 코드
  - insert,upadte,select 자바객체를 SQL로 SQL을 자바 객체로..
- SQL에 의존적인 개발을 피하기 어렵다..

객체 지향 프로그래밍은 추상화, 캡슐화, 정보은닉, 상속 다형성 등 시스템의 복잡성을 제어할 수 있는 다양한 장치들을 제공한다.



객체를 자바 컬렉션에 저장 하듯이 DB에 저장할 수는 없을가?



## **JPA**

- Java Persistence API
- 자바 진영의 ORM 기술 표준

## **ORM**

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

JPA는 애플리케이션과 JDBC 사이에서 동작

![img](https://blog.kakaocdn.net/dn/Uqbja/btrmEc60evs/ExNjnPs8uHm4NB1vKk9Un1/img.png)

## **JPA동작 -저장**

![img](https://blog.kakaocdn.net/dn/bLcKxq/btrmKkWJv4O/B07udmcYKK9zZFF8muZGZk/img.png)

JPA를 쓰게 되면 멤버 객체를 넘기면 JPA가 멤버 객체를 분석하여 적절한 Insert쿼리 생성 JDBC API를 생성하여 DB에 적용합니다.

## **JPA 동장 - 찾기**

![img](https://blog.kakaocdn.net/dn/RqY83/btrmDXoqXp5/zK40Tly4Nps7N1qfQZB6y0/img.png)

pk값만 넘기면 JPA가 적절한 Select 쿼리를 만들어 JDBC API를 통해 DB에 보내고 값을 받습니다.

## JPA 소개

JPA는 인터페이스의 모음

하이버네이트,EclipseLink,DataNucleus

대부분(90%) 하이버네이트를 사용



## **JPA를 왜 사용해야 하는가**

- SQL 중심적인 개발에서 객체 중심으로 개발
- 생산성
- 유지보수
- 패러다임의 불일치 해결
- 성능
- 데이터 접근 추상화와 벤더 독립성
- 표준

## **생산성-JPA와 CRUD**

-  저장: jpa.persist(member)
-  조회: Member member = jpa.find(memberId)
-  수정: member.setName(“변경할 이름”)
-  삭제: jpa.remove(member)
- 이게엄청 편리~!합니다!

## **유지보수**

기존 : 필드 변경시 모든 SQL 수정

JPA: 필드만 추가하면 됨, SQL은 JPA가 처리한다.



## **지연 로딩과 즉시 로딩**

지연 로딩: 객체가 실제 사용될 때 로딩
즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회

![img](https://blog.kakaocdn.net/dn/c6f3iZ/btrmJkiuPUW/trmtDheCDY4f8RNRdTvcP1/img.png)