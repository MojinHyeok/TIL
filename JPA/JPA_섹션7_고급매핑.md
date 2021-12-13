## 고급 매핑

## **상속관계 매핑**

- 관계형 데이터베이스는 상속 관계 X
- 슈퍼타입 서브타입 관계라는 모델링 기법이 객체 상속과 유사
- 상속관계 매핑: 객체의 상속과 구조와 DB의 슈퍼타입 서브타입 관계를 매핑

![img](https://blog.kakaocdn.net/dn/NyZOu/btrnCPwcATU/Bft9v9bJgdcIMkkqOFyrAK/img.png)

왼쪽의 그림은 DB쪽 논리 모델구성하는 것 슈퍼타입 서브타입

오른쪽의 그림은 객체에서의 상속 관계

슈퍼타입 서브타입 논리 모델을 실제 물리 모델을 구성하는 방법은 크게 3가지가 존재

- 각각테이블로변환 -> 조인전략
- 통합테이블로변환 -> 단일테이블전략
- 서브타입테이블로변환 -> 구현클래스마다테이블전략


- @Inheritance(strategy=InheritanceType.XXX)(기본적으로는 단일테이블 전략을 사용)  

  ​

  - JOINED: 조인전략
  - SINGLE_TABLE: 단일테이블전략
  - TABLE_PER_CLASS: 구현클래스마다테이블전략

- @DiscriminatorColumn(name=“DTYPE”) 

- @DiscriminatorValue(“XXX”)

#### 첫번 째 조인 전략

![img](https://blog.kakaocdn.net/dn/lj7Rk/btrnE9H7k6S/gKrVt4oIl4mBHXh5PfxnLk/img.png)

Item이라는 테이블을 만든 이후 조인으로 데이터를 구성하는 방법

Item에는 Item_ID를 두고 insert로 ITEM 과  ALBUM을 두고 select의 조인을 통해 값을 구하는 방법(정규화된 방식)

장점 

- 정규화가 잘 되어있습니다.
- 외래 키 참조 무결성 제약조건 활용가능()
- 저장공간 효율화

단점

- 조회시 조인을 많이 사용, 성능이 저하되는 점.
- 조회 쿼리가 복잡함
- 데이터 저장시 insert sql 2번 호출
- 테이블이 많아 복잡하다는 점?.

#### **두번 째 단일 테이블 전략**

![img](https://blog.kakaocdn.net/dn/cM0iPe/btrnE9H7q5H/uKUeM9geYaAEj1cW78p6s1/img.png)

그냥 컬럼을 다 넣어 DTYPE이라는 것을 활용하여 구분하는 방법(단순하고 성능이 괜춘)

장점

- 조인이 필요 없으므로 일반적으로 조회 성능이 빠르다
- 조회 쿼리가  되게 단순함 한 테이블만 보므로

단점

- 자식 엔티티가 매핑한 컬럼은 모두 null허용
- 단일 테이블에 모든 것을 저장하므로 테이블이 커질 수 있고, 상황에 따라서 조회 성능이 오히려 느려질 수 있다.

세번 째 구현 클래스마다 테이블 전략(별로 안중요)

![img](https://blog.kakaocdn.net/dn/kYgrT/btrnN19THrq/jLDdMrmlPuOhcL1r1hTiyK/img.png)

클래스마다 각각 정보를 가지고 있으며 시작하는 방법(NAME PRICE의 공통사항을 다가지고옴, item을 없애고)

->쓰면 안되는 전략

**이 전략은 데이터베이스 설계자와 ORM 설계자가 둘다 추천하지 않는 전략**

장점

- 서브타입을명확하게구분해서처리할때효과적
- not null 제약조건사용가능

단점

- 여러자식테이블을함께조회할때성능이느림(UNION SQL 필요) 
- 자식테이블을통합해서쿼리하기어려움

**중요한점은 단일테이블과 조인전략에 대해 장단점을 가지고 가야한다는 점!**

**기본으로 조인전략을 깔고 단순한 테이블에 대해서는 단일 테이블 전략을 사용!**

@MappedSuperclass

공통 매핑 정보가 필요할 때 사용 (id,name)

![img](https://blog.kakaocdn.net/dn/kMonL/btrnPk2AzYE/rANFDkFTNThxRKhR5j2SN1/img.png)

예시)member extends BaseEntity이렇게 하여 사용합니다.

상속관계매핑X 

- 엔티티X, 테이블과매핑X 
- 부모클래스를상속받는자식클래스에매핑정보만제공
- 조회, 검색불가(em.ﬁnd(BaseEntity) 불가) 
- 직접생성해서사용할일이없으므로추상클래스권장
- 테이블과관계없고, 단순히엔티티가공통으로사용하는매핑 정보를모으는역할
- 주로등록일, 수정일, 등록자, 수정자같은전체엔티티에서공통 으로적용하는정보를모을때사용
- 참고: @Entity 클래스는엔티티나 @MappedSuperclass로지 정한클래스만상속가능