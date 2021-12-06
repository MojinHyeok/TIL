## **연관관계 매핑 기초**

어떻게 객체 지향적으로 설계하는지 에 대한 수업.



주된 목표는 

**객체와 테이블 연관관계의 차이를 이해**

**객체의 참조와 테이블의 외래 키를 매핑**





객체를 테이블에 맞추어 모델링(연관관계가 없는 객체)

![img](https://blog.kakaocdn.net/dn/bm0JLR/btrm0qXS8Pj/OoEO10xkXWNybQaZemhKp0/img.png)

하나의 팀에 여러 회원이 소속 될 수 있다.



객체를 테이블에 맞추어 모델링 (참조 대신에 외래 키를 그대로 사용)

![img](https://blog.kakaocdn.net/dn/ODP53/btrnbW1YCSW/akrprSOwAjOKfV2XqgXNVk/img.png)

객체를 테이블에 맞추어 모델링(외래 키 식별자를 직접 다룸)

![img](https://blog.kakaocdn.net/dn/brwt7w/btrnbJPbkyw/7dW9kbHMEKhCBSJJgSdbyK/img.png)

여기서 불편한것이 SetTeamID이가 그냥 객체를 넣으면 될 것 같은데 Teamid로 넣는 부분..

![img](https://blog.kakaocdn.net/dn/qzh9Z/btrm9DhomPb/jc5UJPIER52iVOoUw5Sz41/img.png)

연관관계가 없어 이렇게 계속 검색하는 모습.(팀을 찾을때)





**객체를 테이블에 맞추어 데이터 중심으로 모델링하면, 협력 관계를 만들 수 없다.**

- 테이블은 외래 키로 조인을 사용해서 연관된 테이블을 찾는다.
- 객체는 참조를 사용해서 연관된 객체를 찾는다.
- 테이블과 객체 사이에는 이런 큰 간격이 있다.



## **가장 기본의 연관관계 단방향을 알아보겠습니다.**



객체지향 모델링 (객체 연관관계 사용)

![img](https://blog.kakaocdn.net/dn/cc3zxc/btrm0pkpv12/6tmZKcmg296Bu6VpOe92qk/img.png)

이제는 Team_ID로 하지 않고 Team객체 그대로 넣어버리겠다.라는 개념

여기서 멤버가 N이고 Team이 1 인 N대 1 관계이다.

![img](https://blog.kakaocdn.net/dn/cSk4Zf/btrm83UDh07/lOcUtdy8eaUK75tGW7xg4k/img.png)

멤버 입장에서는 N이고 Team입장에선 One이기 때문에 @ManyToOne어노테이션을 사용합니다.

Member객체의 Team 레퍼런스랑 멤버 테이블의 Team_id(FK)랑 매핑해야 하므로 @JoinColumn을 사용합니다.!

이렇게 한다면 **객체지향 모델링**이 됩니다.

![img](https://blog.kakaocdn.net/dn/94qXy/btrnc3fAhtL/qopaHkA7CCvtCOxLLRpNHk/img.png)

이제 한번 사용해 보겠습니다.

![img](https://blog.kakaocdn.net/dn/zYoeS/btrm2pqXmC8/kYCckuvq6kTDShgx9jDPr0/img.png)

이제는 멤버 Team_ID를 넣는 것이 아니라 Team 객체를 넣게 되는 모습을 볼 수 있습니다.

그리고 Team을 찾을 때에도 바로 찾는 모습을 볼 수 있습니다.





## **양방향 연관관계의 연관관계의 주인**

어렵고 중요한 사항..



양방향 매핑!

![img](https://blog.kakaocdn.net/dn/eluUDg/btrm9DWnluy/BUXMiYbcu2nFKvhsGewNZ1/img.png)

단방향에선 멤버에서 팀을 갈 수는 있지만 팀에서 멤버로는 못 간다.. 

양방향에선 테이블 연관관계는 변한 것은 없다. 왜냐 테이블 입장에선 Join을 통해 알 수 있다.!

테이블은 외래 키 하나로 양방향이 다 있는 것이다. 하지만 문제는 객체입니다.

그래서 Team에서 List members를 넣어줘야 양방향이 가능하다.



코드 구현.

![img](https://blog.kakaocdn.net/dn/zxKdx/btrnc4Ts1Vo/Zjdj9AST6UYIoaxBBqvSE0/img.png)

팀에서 멤버는 일대다 이므로 @oneToMany로 작성한다. 그리고 mappedBY를 작성해줘야 한다.

나는 뭐랑 연결되어있지?를 확인하는 것. 나의 반대편 사이트에 이런 것이 걸려있다는 걸 확인하는 개념입니다.

![img](https://blog.kakaocdn.net/dn/bKZA9E/btrnc4lDbNg/U8SVwk6JIK58kP8RHGgk8K/img.png)

이렇게 팀에서 멤버들을 구할 수 있습니다.

 

다음은 중요한 mappedBy입니다. 

**객체와 테이블이 관계를 맺는 차이!**

- 객체 연관관계 = 2개(단 방향이 두개가 있는것.. )
  - 회원 -> 팀 연관관계 1개(단방향)
  - 팀 -> 회원 연관관계 1개(단방향)
- 테이블 연관관계 = 1개
  - 회원 <-> 팀의 연관관계 1개(양방향)
  - FK하나로 양쪽 다 아는 것이 가능합니다.





### **객체의 양방향 관계**

객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단뱡향 관계 2개다.
객체를 양방향으로 참조하려면 **단방향 연관관계**를 2개 만들어야 한다.

A -> B (a.getB())
B -> A (b.getA())

![img](https://blog.kakaocdn.net/dn/uk0DO/btrm9EHMXGE/KDCupTjkq4kCz6eo1dwO3k/img.png)

### **테이블의 양방향 연관관계**

- 테이블은 외래 키 하나로 두 테이블의 연관관계를 관리
- MEMBER.TEAM_ID 외래 키 하나로 양방향 연관관계 가짐
  (양쪽으로 조인할 수 있다.)

SELECT *
FROM MEMBER M
JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
SELECT *
FROM TEAM T
JOIN MEMBER M ON T.TEAM_ID = M.TEAM_ID



딜레마가 발생!!

멤버의 Team값을 토대로 업데이트할지 Team에서의 members를 토대로 업데이트할지 딜레마가 발생합니다.

둘 중 하나로 외래 키를 관리해야 하는 것.!

이 것에 대한 룰이 **연관관계의 주인입니다.**

**양방향 매핑 규칙**

- 객체의 두 관계 중 하나를 연관관계의 주인으로 지정
- 연관관계의 주인만이 외래 키를 관리(등록, 수정)
- 주인이 아닌 쪽은 읽기만 가능
- 주인은 mappedBy 속성 사용 X
- 주인이 아니면 mappedBy 속성으로 주인 지정



### **누구를 주인으로?!**

- 외래 키가 있는 곳을 주인으로 정해라!
- 여기서는 Member.team이 연관관계의 주인

![img](https://blog.kakaocdn.net/dn/cqTCX0/btrndrOo48f/lqqBgkjlEbS9x3K4DAlxBK/img.png)

List members는 읽기만 가능 member.team을 통해 값 변경

위 사진을 보면 Member 테이블에 FK가 있으니 그곳을 주인으로 설정해야 헷갈리지 않습니다.





양방향 매핑 시 가장 많이 하는 실수.(연관관계의 주인에 값을 입력하지 않음)

![img](https://blog.kakaocdn.net/dn/SdQ8D/btrm3Q3lXLP/oX1aI8WZKrjgdvXevPVDak/img.png)

연관관계의 주인은 Member에서 team이 주인입니다.

읽기 전용인 getMembers에 add해도 아무런 영향을 주지 않습니다.!



해결책!!

양방향 매핑 시 연관관계의 주인에 값을 입력해야 한다.

(순수한 객체 관계를 고려하면 항상 양쪽 다 값을 입력해야 한다.)

![img](https://blog.kakaocdn.net/dn/I9WgS/btrnbJIVLaf/idzBVDNGKXK3ndeJjFlxn0/img.png)