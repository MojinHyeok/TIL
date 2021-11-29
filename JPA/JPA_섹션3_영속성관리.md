JPA의 내부 구조 동작 방식 (이론)

# JPA에서 가장 중요한 2가지

- 객체와 관계형 데이터베이스 매핑하기(Object Relational Mapping)
  - 정적인느낌
  - DB를 어떻게 설계하고 객체를 어떻게 설계해서 JPA에서 어떻게 매핑을 할 것인가?
- 영속성 컨텍스트
  - 실제 JPA내부에서 어떻게 작동되나?

# **엔티티 매니저 팩토리와 엔티티 매니저**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ae074b1d-5852-4db4-a62f-c025c49c9e71/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T113820Z&X-Amz-Expires=86400&X-Amz-Signature=cb197dbcc491b3c6568752183f0ce4165a696d2dddb30e77eafeecc70e0c6126&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

새로운 고객이 올때마다 엔티티매니저를 생성하여 내부적으로 데이터베이스 커넥션을 사용하여 DB를 사용하게 됩니다.

# **영속성 컨텍스트**

- JPA를 이해하는데 가장 중요한 용어
- "엔티티를 영구 저장하는 환경" 이라는 뜻
- EntityManager.persist(entity);
  - 엔티티 객체를 DB에 저장하는 개념
  - 엔티티를 영속성 컨텍스트라는 곳에 저장한다는개념

엔티티 매니저? 영속성 컨텍스트?

- 영속성 컨텍스트는 논리적인 개념
- 눈에 보이지 않는다
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c4b7de40-a0ec-4b9f-b744-78bf3101bc6b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114135Z&X-Amz-Expires=86400&X-Amz-Signature=0fb9b9e56b421274617130b5fc3d18066015a936d85519e6672c64ef0b3cba6f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

엔티티매니저를 생성하면 1:1 로영속성 컨텍스트가 생성이 됩니다.

쉽게말하여 엔티티매니저에 눈에 보이지 않는 공간에 영속성컨텍스트가 생성이 된다는 개념

# **엔티티의 생명주기**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4382a25c-9e48-478d-b008-ae2f205dcb0d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114145Z&X-Amz-Expires=86400&X-Amz-Signature=eab81101b1329ed4dd3f41d776a196f7af9d27f5c619b02d93d3be292430371b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

EntityManager.persist(entity); 통해 영속상태가 됩니다.

다음인 비영속/영속 상태에 대해서 알아봅시다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/67db9fc8-4344-413c-b36d-0d201fea4c90/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114202Z&X-Amz-Expires=86400&X-Amz-Signature=6e796081d8643f2970b6fddbbe1c7c4add4b9f3e374d17f0c9282cf33cacedf1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

그냥 객체를 생성한 상태

멤버객체를 생성하고 엔티티매니저에 아무것도 넣지않은 단계

JPA와 관계없이 객체만 생성한 상태

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fd003189-6303-4f51-b95d-09971847bf25/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114220Z&X-Amz-Expires=86400&X-Amz-Signature=493bea5cf4007ea5267c8ac89b9a351711630d6174521fe66231f1f572bb9e85&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
EntityManagerFactory emf= Persistence.createEntityMangerFactory(persistenceUnitName:"hello");
EntityManager em= emf.createEntityManger();

//비영속
Member member= new Member();
member.setID(100L);
member.setName("HelloJPA");

//영속
em.persist(member);
//하지만 em.persist하는 시점에 바로 DB에 쿼리가 날라가는것이 아니라
//트랜잭션을 커밋할 때에 DB에 쿼리가 날라갑니다.

//만약 커밋하기전에 em.detach(member)하면 DB에 쿼리가 날라가는것을 막습니다.
tx.commit();
```

# 영속성 컨텍스트의 이점(애플리케이션과 DB사이 중간계층에 존재)

- 1차 캐시
- 동일성(identity)보장
- 트랜잭션을 지원하기 쓰기 지연
- 변경 감지
- 지연 로딩

영속성 컨텍스트는 내부에 1차캐시라는 것을 들고 있습니다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a3d01594-9ea5-4d3c-82be-dcf054c89eb2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114306Z&X-Amz-Expires=86400&X-Amz-Signature=24ef83627f8baec82a5f899aca78d449cbf3e1ede476967d82fac4877339a278&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

지금은 엔티티매니저를 영속성 컨텍스트로 이해해도 무방하다.

```java
Member member= new Member();
member.setID(100L);
member.setName("HelloJPA");

//1차 캐시에 저장됨
em.persist(member);

//1차 캐시에서 조회
Member findMember = em.find(Member.class, "member1");
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2f77ef4b-0922-4bd3-88fb-c5b7aa9638ea/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114314Z&X-Amz-Expires=86400&X-Amz-Signature=0132a01417cb3c782b9b127dbccfccf723bf95a798e4bd240bdae6fdfe44f2ef&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위의 명령어 중 findMember을 실행한다면 DB에서 조회하는 것이 아니라 1차 캐시에서 조회를 시작합니다. 찾으면 멤버 값 그대로 반환

# 1차캐시에는 없고 DB에는 값이 존재할 때

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/55912837-9355-4633-874f-d76f80ddc0e9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114330Z&X-Amz-Expires=86400&X-Amz-Signature=cd4a06f5e707d3e65879e8607afd845b1920607aa0469c47aa49278ab7a70cc8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

먼저 1차 캐시에서 조회를 하고 없다면 DB를 조회하고 그 이후에 DB의 값들을 1차 캐시에 저장하고 member2를 그대로 반환하고 다음번엔 찾을 수 있게 합니다.

엔티티 매니저는 DB 트랜잭션 단위로 만들고 트랜잭션이 끝나면  같이 종료시킨다.

고객 요청이 하나 들어와서 끝나면 같이 종료시키는 개념

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/88b45e1f-dbd8-4820-8534-b57c18b351e7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114341Z&X-Amz-Expires=86400&X-Amz-Signature=5f1c3dc943ff44ac238d2d06169992b06c3d133519c258c6ab6acf2af9611aac&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위의 사진과 같이 SQL을 실행하기 전 값을 찾을 수 있음을 알 수 있습니다.

em.persist(member)에서 1차 캐시에 저장을 하기 때문에 찾을 수 있습니다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7832f16b-690d-4392-873e-66d1aed24b5d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114351Z&X-Amz-Expires=86400&X-Amz-Signature=fe6a5d1702143a74107b4a4c3cf4357c0bc6715314d762bf40a058ee50f4885a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위의 사진은 2번째의 개념입니다.

1번째를 실행하여서  ID키가 101L인 것을 DB에 insert하여 있다고 가정하고 시작합니다.

그리하여 맨 처음엔 값을 찾을 때는 1차캐시에 존재하지않아 select문을 통해 찾기시작하지만 2번째는 1차에 저장되어 select문을 하지않고 바로 찾음을 알 수 있습니다.

# 영속 엔티티의 동일성 보장

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/60cfeacf-fe89-4a0e-81ef-d34f930c5f4c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114401Z&X-Amz-Expires=86400&X-Amz-Signature=7c57e29d48a87964666596ccad046894d06f517cce36b8dfa0a4674594449294&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

# 엔티티 등록

# 트랜잭션을 지원하는 쓰기 지연

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7f2edc42-944e-4706-a1e1-96bf95b4dbc3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114412Z&X-Amz-Expires=86400&X-Amz-Signature=94808e38b438f1eb1508512c1683cce833469b12bb2770a261d3629246d111bd&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f6b0961f-400c-40dd-91d2-d0570a963d96/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114425Z&X-Amz-Expires=86400&X-Amz-Signature=207ac81226e96a24971575b813710a896f98977eb10c079d5c9d461ee1c1cbd8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b64e467b-5627-485a-8a02-c213e96344fe/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114433Z&X-Amz-Expires=86400&X-Amz-Signature=c326d6a00048d723dc9653f57c1b84425000ea810bb3bf0e4cd688eb5644693a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

굳이 왜 이렇게 하는가? 이유가 무엇인가?

em.persist를 할 때마다 한다면 최적의 여지가 없습니다.

마이바티스로 모아서 한꺼번에 하는건 어렵지만

JPA는 가능하다

# 엔티티 수정(변경 감지)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e5276bdc-ca88-4f15-a9a7-d0127918c85a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114443Z&X-Amz-Expires=86400&X-Amz-Signature=4aa4bbfef00bd764cf4c9c084136fb04e297f2f3bc76add7d22cec1af0e3a520&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

em.update가 필요할 거 같지만  그렇지않다

그 이유는 영속성 컨텍스트 안에 있습니다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a47012a5-8cc6-4ef4-895b-1ef019fe4a79/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114454Z&X-Amz-Expires=86400&X-Amz-Signature=dd158771e485abd928923351c12deaffc78a16c3fe374161b23e3857feb71f38&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

최초상태의 들어온 상태→스냅샷입니다.

commit을 누를때에 엔티티와 최초의 스냅샷을 다 비교하게 됩니다.

만약 비교하여 값이 변경 되었다면 Update쿼리를 생성하여 DB에 반영합니다.

# 플러시

영속성 컨텍스트의 변경내용을 데이터베이스에 반영

영속성 컨텍스트에 대한 변경 사항을 데이터베이스를 맞추는 작업

플러시 발생

- 변경 감지
- 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
- 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송( 등록 , 수정 , 삭제 쿼리)

# 영속성 컨텍스트를 플러시하는 방법

em.flush()←직접 사용하지는 않지만 알아는 두기

트랜잭션 커밋  - 플러시 자동호출

JPQL 쿼리 실행 - 플러시 자동호출(그렇구나 하고 넘어가기)

```java
Memmber member = new Member(id:200L, name:"member200");
em.persist(member);

em.flush();

system.out.print("=====");
tx.commit();
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b245d853-d773-4738-a33b-c52084a9f5f0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114506Z&X-Amz-Expires=86400&X-Amz-Signature=12ebfcb98fb0aa99a2d9f4046490bc6dd3cfca3f703efb6aa7510ba090583b77&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

flush를 해버리니 바로 DB에 반영→그이후 commit

flush를 한다고해도 1차 캐시가 지워지지는 않는다.

쓰기 지연 SQL 저장소에 있는  SQL들이 DB에 반영이 되는 과정이라고 생각.

플러시 모드 옵션

FlushModeType.Auto(커밋이나 쿼리를 실행할 때 플러시(기본값)

FlushModeType.Commit(커밋할 때만 플러시)

알고만 있어라 손대지말고 오토만 쓰는것을 권장,..

# 플러시는!

- 영속성 컨텍스트를 비우지 않음(이름때문에 오해)
- 영속성 컨텍스트의 변경내용을 데이터베이스에 동기화
- 트랜잭션이라는 작업 단위가 중요→ 커밋 직전에만 동기화 하면됨

# 준영속상태

- 영속 → 준영속
- 영속 상태의 엔티티가 영속성 컨텍스트에서 분리(detached)
- 영속성 컨텍스트가 제공하는 기능을 사용 못함

준영속 상태로 만드는 방법

1.em.detach(entity)

특정 엔티티만 준영속 상태로 전환.

```java
Member member= em.find(Member.class, primaryKey :150L);
member.setName("AAAA")'

em.detach(member);
tx.commit();
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5c77db7a-17a5-4b28-b258-9371092b0cdb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211129T114522Z&X-Amz-Expires=86400&X-Amz-Signature=40e78559d021b6b888961c805013163e9b564a27549c8d819cfb2399067d0efc&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

준영속상태여서 update를 하지않는다.

2.em.clear();(1차캐시를  지우는것)

- 영속성 컨텍스트를 완전히 초기화

```java
Member member= em.find(Member.class, primaryKey :150L);
member.setName("AAAA")'

em.clear();
Member membeer2=em.find(Member.class, primaryKey :150L);
tx.commit();
```

select문 두번 출력

3.em.close

- 영속성 컨텍스트를 종료