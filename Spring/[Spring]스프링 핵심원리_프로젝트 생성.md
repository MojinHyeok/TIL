## **프로젝트 만들기.**

프로젝트는 start.spring.io에서 프로젝트를 만들겠습니다.

![img](https://blog.kakaocdn.net/dn/lZD7S/btrs9LODTKI/h8MSTAhCtUwMMZXZXsnmik/img.png)

위와 같이 셋팅을 마친 이후에 Sts에서 Imort를 진행 후

![img](https://blog.kakaocdn.net/dn/edONcN/btrteiZ3d4l/22G1iyyRnezKhSDACU9oJk/img.png)

Add Gradle Nature를 클릭하여 Gradle환경을 만듭니다.



![img](https://blog.kakaocdn.net/dn/6GzJ7/btrtakc0rVB/gWoF9eilmns8m0eGFYOal1/img.png)

그 이후 Refresh Gradle Project를 클릭하시면 프로젝트 생성은 완료가 됩니다.



## **비즈니스 요구사항과 설계**

- 회원
  - 회원을가입하고조회할수있다. 
  - 회원은일반과 VIP 두가지등급이있다.
  - 회원데이터는자체 DB를구축할수있고, 외부시스템과연동할수있다. (미확정) 
- 주문과할인정책
  - 회원은상품을주문할수있다.
  - 회원등급에따라할인정책을적용할수있다.
  - 할인정책은모든 VIP는 1000원을할인해주는고정금액할인을적용해달라. (나중에변경될수 있다.)
  - 할인정책은변경가능성이높다. 회사의기본할인정책을아직정하지못했고, 오픈직전까지고민을 미루고싶다. 최악의경우할인을적용하지않을수도있다. (미확정)

요구사항을보면회원데이터, 할인정책같은부분은지금결정하기어려운부분이다. 그렇다고이런정책이 결정될때까지개발을무기한기다릴수도없다. 우리는앞에서배운객체지향설계방법이있지않은가!



## **회원 도메인 설계**

- 회원
  - 회원을가입하고조회할수있다. 
  - 회원은일반과 VIP 두가지등급이있다.
  - 회원데이터는자체 DB를구축할수있고, 외부시스템과연동할수있다. (미확정) 

**회원 도메인 협력 관계**

![img](https://blog.kakaocdn.net/dn/kBrZZ/btrs8aaoQpD/RC7WMscEaOlokQ3OclHJ5k/img.png)

**회원 클래스 다이어그램**

![img](https://blog.kakaocdn.net/dn/suc8x/btrs9jyGBuL/CZ4zkRjTg0clIuddGHOgT0/img.png)

**회원 객체 다이어그램**

![img](https://blog.kakaocdn.net/dn/USaxR/btrtjQuYmyL/FpqKQerXsVIkZb1UXZ3pU1/img.png)

회원 서비스 : MemberServiceImpl



## **회원 도메인 개발**

**Grade를 Enum타입**

```
package hello.core.member;

public enum Grade {
	BASIC,
	VIP

}
```

**회원 엔티티**

```
package hello.core.member;

public class Member {

	private Long id;
	private String name;
	private Grade grade;
	public Member(Long id, String name, Grade grade) {
		super();
		this.id = id;
		this.name = name;
		this.grade = grade;
	}
	public Long getId() {
		return id;
	}
	public void setId(Long id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Grade getGrade() {
		return grade;
	}
	public void setGrade(Grade grade) {
		this.grade = grade;
	}
	
	
}
```

생성자와 getter,setter를 생성하였습니다.



### **회원 저장소**

**회원 저장소 인터페이스**

```
package hello.core.member;

public interface MemberRepository {

	void save(Member member);
	
	Member findeById(Long memberId);
}
```

**메모리 회원 저장소 구현체**

```
package hello.core.member;

import java.util.HashMap;
import java.util.Map;

public class MemoryMemberRepository implements MemberRepository{

	private static Map<Long,Member> store= new HashMap<>();
	
	
	@Override
	public void save(Member member) {
		store.put(member.getId(), member);	
	}

	@Override
	public Member findeById(Long memberId) {
		return store.get(memberId);
	}

}
```

데이터베이스가 아직 확정이 안되었다. 그래도 개발은 진행해야하니 가장 단순한, 메모리 회원 저장소를 구현해서 우선 개발, 

참고 : HashMap은 동시성 이슈가 발생할 수 있다. 이런 경우 ConcurrentHashMap을 사용하기를 권장.



**회원서비스 인터페이스**

```
package hello.core.member;

public interface MemberService {
	
	void join(Member member);
	
	Member findMember(Long memberId);

}
```



**회원 서비스 구현체**

```
package hello.core.member;

public class MemberServiceImpl  implements MemberService{

	private final MemberRepository memberRepository= new MemoryMemberRepository();
	//오버라이드 되어 MemberRespository로 선언되어도 new MemoryMemberRepository의 save와 findByid와 같은 것들로 선언이 됩니다.
	
	@Override
	public void join(Member member) {
		memberRepository.save(member);
		
	}

	@Override
	public Member findMember(Long memberId) {
		return memberRepository.findeById(memberId);
	}

}
```



## **회원 도메인 실행과 테스트**

**회원 도메인 - 회원 가입 main**

```
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;

public class MemberApp {
	
	public static void main(String[] args) {
		MemberService memberService= new MemberServiceImpl();
		Member member=new Member(1L,"memberA",Grade.VIP);
		memberService.join(member);
		Member findMember = memberService.findMember(1L);
		System.out.println("new member = " + member.getName());
		System.out.println("find Member = " + findMember.getName());
		//테스트결과
		//new member = memberA
		//find Member = memberA

		
	}
}
```

애플리케이션 로직으로 이렇게 테스트 하는 방법은 좋은 방법이 아니기 때문에, JUnit테스트를 사용하기를 권장합니다.



**회원 도메인 - 회원 가입 테스트**

```
package hello.core.member;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

public class MemberServiceTest {
	
	MemberService memberService = new MemberServiceImpl();
		
	@Test
	void join() {
		//given
		Member member=new Member(1L,"memberA",Grade.VIP);
		//when
		memberService.join(member);
		Member findMember = memberService.findMember(1L);
		//tehn
		Assertions.assertThat(member).isEqualTo(findMember);
	}
}
```

## **주문과 할인 도메인 설계**

- 주문과할인정책
  - 회원은상품을주문할수있다.
  - 회원등급에따라할인정책을적용할수있다.
  - 할인정책은모든 VIP는 1000원을할인해주는고정금액할인을적용해달라. (나중에변경될수 있다.)
  - **할인정책은변경가능성이높다**. **회사의기본할인정책을아직정하지못했고, 오픈직전까지고민을 미루고싶다.** 최악의경우할인을적용하지않을수도있다. (미확정)



**주문 도메인 협력, 역할, 책임**

![img](https://blog.kakaocdn.net/dn/bSDqiy/btrtaj6szX6/OmmcUPo3XmiFYywUAFhc4k/img.png)

\1. 주문생성: 클라이언트는주문서비스에주문생성을요청한다.
\2. 회원조회: 할인을위해서는회원등급이필요하다. 그래서주문서비스는회원저장소에서회원을 조회한다.
\3. 할인적용: 주문서비스는회원등급에따른할인여부를할인정책에위임한다.
\4. 주문결과반환: 주문서비스는할인결과를포함한주문결과를반환한다.





**주문 도메인 전체**

![img](https://blog.kakaocdn.net/dn/bsAiCg/btrtajrU7zN/iy1RniLx60IYkKuYCkRmD0/img.png)

역할과 구현을 분리해서 자유롭게 구현 객체를 조립할 수 있게 설계, 덕분에 회원 저장소는 물론, 할인 정책 또한 유연하게 대처가능



**주문 도메인 클래스 다이어그램**

![img](https://blog.kakaocdn.net/dn/GPw4C/btrtb2XIpbO/fYlOUurYRXQBNeTKEnOMCK/img.png)

**주문 도메인 객체 다이어그램 1**

![img](https://blog.kakaocdn.net/dn/l5MHA/btrtgsBxRJO/T41nY1KEo541d9aSy6x22K/img.png)

회원을 메모리에서 조회하고, 정액 할인 정책을 지원해도 주문 서비스를 변경하지 않아도 된다.

역할들의 협력 관계를 그대로 재사용 가능



**주문 도메인 객체 다이어그램 2**

![img](https://blog.kakaocdn.net/dn/o4keY/btrtk0YAv41/Zjvv67AGtK0El6fV82PL5K/img.png)

회원 메모리가 아닌 실제 DB에서 조회하고, 정률 할인 정책을 지원해도 주문 서비스를 변경하지 않아도 된다.

협력 관계를 그대로 재사용 가능.



## **주문과 할인 도메인 개발**

**할인 정책 인터페이스**

```
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
	
	//return 할인 대상 금액
	int discount(Member member, int price);

}
```

**정액 할인 정책 구현체**

```
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class FixDiscountPolicy implements DiscountPolicy{

	private int discountFixAmount= 1000;//1000원할인
	
	@Override
	public int discount(Member member, int price) {
		if(member.getGrade()==Grade.VIP)return discountFixAmount;
		return 0;
	}

}
```

**주문 엔티티**

```
package hello.core.order;

public class Order {

	private Long memberId;
	private String itemName;
	private int itemPrice;
	private int discountPrice;
	public Order(Long memberId, String itemName, int itemprice, int discountPrice) {
		super();
		this.memberId = memberId;
		this.itemName = itemName;
		this.itemPrice = itemprice;
		this.discountPrice = discountPrice;
	}
	
	public int calculatePrice() {
		return itemPrice - discountPrice;
	}

	public Long getMemberId() {
		return memberId;
	}

	public void setMemberId(Long memberId) {
		this.memberId = memberId;
	}

	public String getItemName() {
		return itemName;
	}

	public void setItemName(String itemName) {
		this.itemName = itemName;
	}

	public int getItemPrice() {
		return itemPrice;
	}

	public void setItemPrice(int itemPrice) {
		this.itemPrice = itemPrice;
	}

	public int getDiscountPrice() {
		return discountPrice;
	}

	public void setDiscountPrice(int discountPrice) {
		this.discountPrice = discountPrice;
	}

	@Override
	public String toString() {
		return "Order [memberId=" + memberId + ", itemName=" + itemName + ", itemPrice=" + itemPrice
				+ ", discountPrice=" + discountPrice + "]";
	}
	
}
```

할인을 적용하는 메소드도 구현.



**주문 서비스 인터페이스**

```
package hello.core.order;

public interface OrderService {
	Order createOrder(Long memberId,String itemName, int itemPrice);
	
}
```

**주문 서비스 구현체**

```
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;
import hello.core.member.MemoryMemberRepository;

public class OrderServiceImpl  implements OrderService{

	private final MemberRepository memberRepository = new MemoryMemberRepository();
	private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
	
	@Override
	public Order createOrder(Long memberId, String itemName, int itemPrice) {
		Member member=memberRepository.findeById(memberId);
		int discountPrice = discountPolicy.discount(member, itemPrice);
	
		return new Order(memberId,itemName,itemPrice,discountPrice);
	}
	
}
```

주문 생성 요청이 온다면, 회원 정보를 조회하고, 할인 정책을 적용한 다음 주문 객체를 생성해서 반환하는 작업.

메모리 회원 리포지토리와, 고정 금액 할인 정책을 구현체로 생성한다.



## **주문과 할인 도메인 실행과 테스트**



**주문과 할인 정책 실행**

```
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.order.Order;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

public class OrderApp {

	public static void main(String[] args) {
		MemberService memberService=new MemberServiceImpl();
		OrderService orderService =new OrderServiceImpl();
		Long memberId=1L;
		Member member= new Member(memberId, "memberA",Grade.VIP);
		memberService.join(member);
		Order order = orderService.createOrder(memberId, "itemA", 10000);
		System.out.println("order = "+order);
		System.out.println("order.calculatePrice = " +order.calculatePrice());
		//출력문
		//order = Order [memberId=1, itemName=itemA, itemPrice=10000, discountPrice=1000]
		//order.calculatePrice = 9000

	}
}
```



**JUnit Test**

```
package hello.core.order;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;

public class OrderServicetest {

	
	MemberService memberService = new MemberServiceImpl();
	OrderService orderService =new OrderServiceImpl();
	@Test
	void createOrder() {
		Long memberId=1L;
		Member member=new Member(memberId,"memberA",Grade.VIP); 
		memberService.join(member);
		
		Order order=orderService.createOrder(memberId, "itemA", 10000);
		Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
	}
}
```

강의는 인프런의 김영한님의 스프링 핵심 원리 기본편을 수강하였습니다.