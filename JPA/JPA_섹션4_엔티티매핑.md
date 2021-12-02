JPA에서 중요한 것은 

영속성 관리하는 법

설계하는 방법 이 중요합니다.(정적인 측면)



목차.

- 객체와 테이블 매핑
- 데이터베이스 스키마 자동 생성.
- 필드와 칼럼 매핑
- 기본 키 매핑



## 엔티티 매핑 소개

- 객체와 테이블 매핑 @Entity, @Table
- 필드와 칼럼 매핑 @Column
- 기본 키 매핑 @ Id
- 연관관계 매핑 @ManyToOne @JoinColumn(1대다 다대다 이런 형식을 어떻게 매핑할 것인가)?



## @Entity

- @Entity가 붙은 클래스는 JPA가 관리, 엔티티 라 합니다.
- @Entity가 붙지 않으면 JPA가 관리하지 않는 자기가 관리하고 싶은 클래스를 칭합니다.
- JPA를 사용해서 테이블과 매핑과 클래스는 **@Entity**가 필수입니다.
- 주의
  - **기본 생성자 필수**(파라미터가 없는 public 또는 protected 생성자)->JPA 스펙상 규정
  - final 클래스, enum,interface,inner 클래스 사용 x 
  - 저장할 필드에 final사용 x
- 속성 : name
  - JPA에서 사용하 엔티티 이름을 지정하는 것
  - 지정하지 않는다면 클래스 이름을 그대로 사용 합니다.

##  

## 데이터베이스 스키마 자동 생성.

- DDL을 애플리케이션을 실행 시점에 자동생성
- 테이블 중심-> 객체 중심
- 데이터베이스 방언을 활용해서 데이터베이스에 맞는 적절한 DDL생성
- 이렇게 생성된 DDL은 개발 장미에서만 사용
- JPA가 테이블을 자동으로 만들어준다는 개념

![img](https://blog.kakaocdn.net/dn/l99OR/btrmNnF91cJ/2AHKqP24CoL4UjU30rIL4k/img.png)

## **운영 장비에서는 절대 create, create-drop, update를 사용하면 안 된다.!**

개발 초기 단계는 create 또는 update
테스트 서버는 update 또는 validate
스테이징과 운영 서버는 validate 또는 none

자신의 로컬 서버에서만 사용하는 걸 권장..



#### DDL 생성 기능

각각 행에 마다 @Column(unique=true, length=10) 이런 걸 설정할 수 있습니다.

DDL 생성 기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.





## **필드와 칼럼 매핑**

![img](https://blog.kakaocdn.net/dn/cune0f/btrmQ4sCv5d/SGJghkFPSDJmwYAEn1PHI1/img.png)이런식으로사용

![img](https://blog.kakaocdn.net/dn/cyfLa0/btrmKZ0qiDs/K3EzbbdJYu93LxDxOcEqw1/img.png)

Date날짜(2013–10–11) , DateTime(11:11:11) 시간, TimeStamp(2013–10–11 11:11:11) 둘 다 포함

![img](https://blog.kakaocdn.net/dn/MaHfJ/btrmKC5qHzq/68Em6yH8yjKBDBpz74JKNK/img.png)

insertable, updateable, 변경 가능하게 할 건지, 등록 가능하게 할 건지..

nullable★ nullable을 false로 하면 not null제약 조건이 걸리게 됨 

unique는 잘 사용되지 않는다. 이름이 명확하지 않아 직관적인 분석이 힘들어서.



JPA에서 enum타입을 할 때에 두 가지가 선택 가능합니다.

EnumType.ORDINAL: enum 순서를 데이터베이스에 저장

EnumType.STRING: enum 이름을 데이터베이스에 저장

ORDINAL은 DB에 순서를 그대로 저장하기 때문에(0,1 이렇게 저장) STRING을 사용해야 합니다.

수정 변경이 어렵다는 측면도 존재해서 STRING을 사용해야 합니다. 운영 측면에서 큰 오류!



날짜는 최신 버전으로 LocalDate, LocalDateTime을 사용해도 상관없습니다.



## **기본키 매핑**

사용할 수 있는 어노테이션은 크게 @Id, @GeneratedValue로 두 가지입니다.

![img](https://blog.kakaocdn.net/dn/ySHgp/btrmQ4zwcvj/rFjowt6gcv6y6hrXRPPotK/img.png)

직접 할당 : @ID만 사용

자동 생성(@GeneratedValue)

- IDENTITY: 데이터베이스에 위임, MYSQL
- SEQUENCE: 데이터베이스 시퀀스 오브젝트 사용, ORACLE
  - @SequenceGenerator 필요
- TABLE: 키 생성용 테이블 사용, 모든 DB에서 사용
  - @TableGenerator 필요
- AUTO: 방언에 따라 자동 지정, 기본값





IDENTITY -특징

- 기본 키 생성을 데이터베이스에 위임(제일 큰 예시 MYSQL에서의 AI(오토 인크리먼트))
- 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용
  - (예: MySQL의 AUTO_ INCREMENT)
- JPA는 보통 트랜잭션 커밋 시점에 INSERT SQL 실행
- AUTO_ INCREMENT는 데이터베이스에 **INSERT SQL을 실행한 이후에 ID** 값을 알 수 있음(이래서 모아서 한꺼번에 하는 건 불가능)
- IDENTITY 전략은 em.persist() 시점에 즉시 INSERT SQL 실행하고 DB에서 식별자를 조회
- 나는 모르겠고 디비야 알아서 해줘~느낌

SEQUENCE-특징

- 데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한
- 데이터베이스 오브젝트(예: 오라클 시퀀스)
- 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용

TABLE-특징

- 키 생성 전용 테이블을 하나 만들어서 데이터베이스 시퀀스를 흉내 내는 전략
- 장점: 모든 데이터베이스에 적용 가능
- 단점: 성능
- 운영단계에선 별로 권장하지는 않음..

## **권장하는 식별자 전략**

- 기본 키 제약 조건: null 아님, 유일, 변하면 안 된다.
- 미래까지 이 조건을 만족하는 자연 키(주민등록번호)는 찾기 어렵다. 대리 키(대체키)를 사용하자.
- 예를 들어 주민등록번호도 기본 키로 적절하기 않다.
- 권장: Long형 + 대체키 + 키 생성 전략 사용