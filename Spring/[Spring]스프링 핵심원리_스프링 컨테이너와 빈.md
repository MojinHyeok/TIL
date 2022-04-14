## **스프링 컨테이너 생성**

스프링 컨테이너가 생성되는 과정을 알아봅시다.

```
ApplicationContext applicationContext=new AnnotationConfigApplicationContext(AppConfig.class);
```

- ApplicationContext를 스프링 컨테이너라 한다. 
- ApplicationContext는 인터페이스이다.
- 스프링 컨테이너는 XML을 기반으로 만들 수 있고, 애노테이션 기반의 자바 설정 클래스로 만들 수 있다. 
- 직전에 AppConfig를 사용했던 방식이 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든 것이다.
- 자바 설정 클래스를 기반으로 스프링 컨테이너(ApplicationContext)를 만들어보자
  - new AnnotationConfigApplicationContext(AppConfig.class);
  - 이클래스는 ApplicationContext 인터페이스의 구현체이다.

 참고: 더 정확히는 스프링 컨테이너를 부를 때 BeanFactory, ApplicationContext로 구분해서 이야기한다. 이 부분은 뒤에서 설명하겠다. BeanFactory를 직접 사용하는 경우는 거의 없으므로 일반적으로 ApplicationContext를 스프링 컨테이너라 한다.



## **스프링 컨테이너의 생성과정**



**스프링 컨테이너 생성**

![img](https://blog.kakaocdn.net/dn/b5DjxK/btrtydQ8j9m/NzfSnnic6Z6mssmB3ORIK0/img.png)

- new AnnotationConfigApplicationContext(AppConfig.class)
- 스프링 컨테이너를 생성할 때는 구성정보를 지정해주어야 한다. 
- 여기서는 AppConfig.class를 구성정보로 지정했다.

**스프링 빈 등록**

![img](https://blog.kakaocdn.net/dn/PZ5tc/btrtsGfInN6/rzS8Km2GmUCWA5kEPqK0zK/img.png)

스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용하여 스프링 빈을 등록한다



**빈 이름**

- 빈 이름은 메서드 이름을 사용한다
- 빈 이름을 직접 부여할 수 도 있다.
  - @Bean(name="memberService2")



**스프링 빈 의존관계 설정 - 준비**

![img](https://blog.kakaocdn.net/dn/quKNI/btrtt5Nypvs/LXBRr1ADo4lxVn201oW1s1/img.png)

**스프링 빈 의존관계 설정 - 완료**

![img](https://blog.kakaocdn.net/dn/cjcz46/btrtvsV1Aoc/rF5KbB2k2KSCbEcLiIzhnk/img.png)

- 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
- 단순히 자바 코드를 호출하는 것 같지만, 차이가 있다. 이 차이는 뒤에 싱글톤 컨테이너에서 설명한다. 

**참고**
스프링은 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져 있다. 그런데 이렇게 자바 코드로 스프링 빈을 등록하면 생성자를 호출하면서 의존관계 주입도 한 번에 처리된다. 여기서는 이해를 돕기 위해 개념적으로 나누어 설명했다. 자세한 내용은 의존관계 자동 주입에서 다시 설명하겠다.





## **컨테이너에 등록된 모든 빈 조회**

스프링 컨테이너에 실제 스프링 빈들이 잘 등록되어있는지 확인해보는 과정을 진행해보겠습니다.

테스트 코드로 작성.

**코드**

```
package hello.core.beanfind;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import hello.core.AppConfig;

public class ApplicationContextInfoTest {

	AnnotationConfigApplicationContext ac =new AnnotationConfigApplicationContext(AppConfig.class);
	
	@Test
	@DisplayName("모든 빈 출력하기")
	void findAllBean() {
		System.out.println("===모든 빈 출력하기===");
		String[] beanDefinitionNames= ac.getBeanDefinitionNames();
		for(String beanDefinitionName : beanDefinitionNames ) {
			Object bean =ac.getBean(beanDefinitionName);
			System.out.println("name = "+ beanDefinitionName +" object = "+ bean);
		}
	}
	
	@Test
	@DisplayName("애플리케이션 빈 출력하기")
	void findApplicationBean() {
		System.out.println("=====애플리케이션 빈 출력하기====");
		String[] beanDefinitionNames= ac.getBeanDefinitionNames();
		for(String beanDefinitionName : beanDefinitionNames ) {
			BeanDefinition beanDefinition =ac.getBeanDefinition(beanDefinitionName);
			//Role ROLE_APPLICATION : 직접 등록한 애플리케이션 빈
			//Role ROLE_INFRASTRUCTURE: 스프링 내부에서 사용하는 빈
			if(beanDefinition.getRole()==BeanDefinition.ROLE_APPLICATION) {
				Object bean = ac.getBean(beanDefinitionName);
				System.out.println("name = "+ beanDefinitionName +" object = "+ bean);
			}
		}
	}
}
```

프린트 결과

![img](https://blog.kakaocdn.net/dn/3giYI/btrtuuMPh2J/F2IaoQxKEOkrDoXZAA2y51/img.png)



**모든 빈 출력하기**

- 실행하면 스프링에 등록된 모든 빈 정보를 출력할 수 있다.
- ac.getBeanDefinitionNames(): 스프링에 등록된 모든 빈 이름을 조회한다.
- ac.getBean(): 빈 이름으로 빈객체(인스턴스)를 조회한다.



**애플리케이션 빈 출력하기**

- 스프링이 내부에서 사용하는 빈은 제외하고, 내가 등록한 빈만 출력해 보자. 
- 스프링이 내부에서 사용하는 빈은 getRole()로 구분할수 있다.
  - ROLE_APPLICATION: 일반적으로 사용자 가정의 한빈
  - ROLE_INFRASTRUCTURE: 스프링이 내부에서 사용하는 빈

## **스프링 빈 조회 - 기본**

**스프링 컨테이너에서 스프링 빈을 찾는 가장 기본적인 조회 방법**

- ac.getBean(빈이름, 타입)
- ac.getBean(타입)
- 조회 대상 스프링 빈이 없으면 예외 발생
  - NoSuchBeanDefinitionException: No bean named 'xxxxx' available

```
package hello.core.beanfind;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoSuchBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import hello.core.AppConfig;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;

public class ApplicationContextBasicFindTest {

	AnnotationConfigApplicationContext ac =new AnnotationConfigApplicationContext(AppConfig.class);
	
	@Test
	@DisplayName("빈 이름으로 조회")
	void findBeanByName() {
		MemberService memberService=ac.getBean("memberService",MemberService.class);
		Assertions.assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
	}
	@Test
	@DisplayName("이름 없이 타입으로만 조회")
	void findBeanByType() {
		MemberService memberService=ac.getBean(MemberService.class);
		Assertions.assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
	}
	
	@Test
	@DisplayName("구체 타입으로 조회")
	void findBeanByName2() {
		MemberServiceImpl memberService=ac.getBean("memberService",MemberServiceImpl.class);
		Assertions.assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
	}
	
	
	@Test
	@DisplayName("빈 이름으로 조회X 에러 발생")
	void findBeanByNameX() {
		//두번째 파라미터의 함수를 실행시켰을 때 첫번째의 오류가 무조건 발생해야 성공이라는 의미.
		org.junit.jupiter.api.Assertions.assertThrows(NoSuchBeanDefinitionException.class,()->ac.getBean("xxxxx",MemberService.class));
	}
}
```



## **스프링 빈 조회 - 동일한 타입이 둘 이상**

타입으로 조회시 같은 타입의 스프링 빈이 둘 이상이면 오류가 발생한다. 이때는 빈 이름을 지정하자

ac.getBeansOfType()을 사용하면 해당 타입의 모든 빈을 조회할 수 있다.

```
package hello.core.beanfind;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

import java.util.Map;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import hello.core.AppConfig;
import hello.core.member.MemberRepository;
import hello.core.member.MemberService;
import hello.core.member.MemoryMemberRepository;

public class ApplicationContextSameBeanFindTest {

	AnnotationConfigApplicationContext ac =new AnnotationConfigApplicationContext(SameBeanConfig.class);
	
	@Test
	@DisplayName("타입으로 조회시 같은 타입이 둘 이상이 있으면, 중복 오류가 발생한다")
	void findBeanByTypeDuplicatie() {
		//스프링 입장에서 무엇을 선택할지 모르겠다.
		assertThrows(NoUniqueBeanDefinitionException.class, ()->ac.getBean(MemberRepository.class));
	}
	
	@Test
	@DisplayName("타입으로 조회시 같은 타입이 둘 이상 있으면, 빈 이름을 지정하면 된다")
	void findBeanByname() {
		MemberRepository memberRepository=ac.getBean("memberRepository1",MemberRepository.class);
		assertThat(memberRepository).isInstanceOf(MemberRepository.class);
	}
	@Test
	@DisplayName("특정 타입을 모두 조회하기")
	void findAllBeanByType() {
		Map<String, MemberRepository> beansOfType=ac.getBeansOfType(MemberRepository.class);
		for(String key : beansOfType.keySet()) {
			System.out.println("key = "+key +" value = "+ beansOfType.get(key));
		}
		System.out.println("beansOfType  ="+ beansOfType);
		assertThat(beansOfType.size()).isEqualTo(2);
	}
	
	@Configuration
	static class SameBeanConfig {
		
		@Bean
		public MemberRepository memberRepository1() {
			return new MemoryMemberRepository();
		}
		
		@Bean
		public MemberRepository memberRepository2() {
			return new MemoryMemberRepository();
		}
	}
}
```



## **스프링 빈 조회 - 상속관계**



부모 타입으로 조회하면, 자식 타입도 함께 조회한다.

그래서 모든 자바 객체의 최고 부모인 Object타입으로 조회하면, 모든 스프링 빈을 조회한다.

![img](https://blog.kakaocdn.net/dn/cgdfTm/btrtyeXeRYj/n0LujaQQbtxvdB5Egy68kk/img.png)

코드

```
package hello.core.beanfind;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

import java.util.Map;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import hello.core.AppConfig;
import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.discount.RateDiscountPolicy;
import hello.core.member.MemberRepository;

public class ApplicationContextExtendsFindTest {
	
	AnnotationConfigApplicationContext ac =new AnnotationConfigApplicationContext(TestConfig.class);
	
	@Test
	@DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 중복 오류가 발생한다.")
	void findBeanByParentTypeDuplicate() {
	    assertThrows(NoUniqueBeanDefinitionException.class, ()->ac.getBean(DiscountPolicy.class));
	}
	
	@Test 
	@DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면, 빈 이름을 지정하면 된다.")
	void findBeanByParentTypeBeanName() {
		DiscountPolicy rateDiscountPolicy = ac.getBean("rateDiscountPolicy",DiscountPolicy.class);
		assertThat(rateDiscountPolicy).isInstanceOf(RateDiscountPolicy.class);
	}
	
	@Test
	@DisplayName("부모 타입으로 모두 조회하기")
	void findAllBeanByType() {
		Map<String, DiscountPolicy> beansOfType=ac.getBeansOfType(DiscountPolicy.class);
		for(String key : beansOfType.keySet()) {
			System.out.println("key = "+key +" value = "+ beansOfType.get(key));
		}
		assertThat(beansOfType.size()).isEqualTo(2);
	}
	
	@Test
	@DisplayName("부모 타입으로 모두 조회하기 - Object")
	void findAllBeanByObjectType() {
		Map<String, Object> beansOfType=ac.getBeansOfType(Object.class);
		for(String key : beansOfType.keySet()) {
			System.out.println("key = "+key +" value = "+ beansOfType.get(key));
		}
	}
	
	@Configuration
	static class TestConfig{
		@Bean
		public DiscountPolicy rateDiscountPolicy() {
			return new RateDiscountPolicy();
		}
		@Bean
		public DiscountPolicy fixDiscountPolicy() {
			return new FixDiscountPolicy();
		}
	}
	
}
```



## **BeanFactory와 ApplicationContext**

beanFactory와 ApplicationContext입니다

![img](https://blog.kakaocdn.net/dn/7i43c/btrtB7JBogr/rTa5k7SykAStP37QHnvKV1/img.png)

**BeanFactory**

- 스프링 컨테이너의 최상위 인터페이스입니다.
- 스프링 빈을 관리하고 조회하는 역할을 담당합니다.
- getBean()을 제공합니다.
- 지금까지 우리가 사용했던 대부분의 기능은 BeanFactory가 제공하는 기능입니다.

**ApplicationContext**

- BeanFactory 기능을 모두 상속받아서 제공합니다.
- 빈을 관리하고 검색하는 기능을 BeanFactory가 제공하는 데 둘의 차이점이 뭘까?
- 애플리케이션을 개발할 때는 빈은 관리하고 조회하는 기능을 물론이고 수많은 부가기능이 필요하다.



**ApplicationContext가 제공하는 부가기능**

![img](https://blog.kakaocdn.net/dn/8l5sd/btrtxpEsCjs/t8kglaWmkIHH9zFM224lO0/img.png)

- 메시지 소스를 활용한 국제화 기능
  - 예를 들어서 한국에서 들어온 면한 국어로, 영어권에서 들어오면 영어로 출력 
- 환경변수
  - 로컬, 개발, 운영 등을 구분해서 처리
- 애플리케이션 이벤트
  - 이벤트를 발행하고 구독하는 모델을 편리하게 지원
- 편리한 리소스 조회
  - 파일, 클래스 패스, 외부 등에 서리 소스를 편리하게 조회