DI에 대해서 Step 1~4에 맞게 실행해보겠습니다.

Step 1은 

객체 간 강한 결합 상태인 클래스 호출 방식입니다.

클래스 내에 선언과 구현이 모두 되어 있기 때문에 다양한 형태로 변화가 불가능합니다.

클래스와 클래스끼리 1:1으로 맵핑이 되어버립니다.

객체 간 강한 결합

MemberSerivce 구현체와 AdminSerivce 구현체를 HomeController에서 직접 생성하여 사용합니다.

MemberSerivce 또는 AdminService가 교체되거나 내부 코드가 변경되면 HomeController까지 수정해야 할 가능성이 있음

![img](https://blog.kakaocdn.net/dn/vWJKW/btrszlW9W9u/NCqwf7aiTRkI4BCkjnSu0k/img.png)

아래와 같이 코드 3개가 있다고 가정하겠습니다.

HelloMain

```
public class HelloMain {

	public static void main(String[] args) {
		HelloMessageKor helloMessageKor = new HelloMessageKor();
//		HelloMessageEng helloMessageEng = new HelloMessageEng();
		
		String greeting = helloMessageKor.helloKor("모진혁");
//		String greeting = helloMessageEng.helloEng("Mr. 모");
		
		System.out.println(greeting);
	}
	
}
```

HelloMessageEng

```
public class HelloMessageEng {

	public String helloEng(String name) {
		return "Hello " + name;
	}
	
}
```

HelloMessageKor

```
public class HelloMessageKor {

	public String helloKor(String name) {
		return "안녕하세요 " + name;
	}
	
}
```

코드의 간단한 설명은 Kor은 안녕하세요 이름이고 Eng는 Hello 이름입니다.

HelloMessageKor에서 클래스명 helloKor을 hello로 바꾸게 되면 Main에서도 greeting에서도 클래스명을 바꿔야 합니다.

이렇듯 어느 한쪽이 바꾸게 되면 여러 곳을 바꿔야 하는 것을 타이트한 결합이라고 합니다. 이런 것을 유지 보수가 힘들다고 합니다.

그리하여 이러한 점을 해결하려면 다음 Step인 Interface를 사용하면 됩니다.

다음은 두 번째 Step인 Interface입니다.

- 객체 간의 강한 결합을 다형성을 통해 결합도를 낮춥니다.
- 인터페이스 호출 방식이고 구현 클래스 교체가 용이하여 다양한 형태로 변화가 가능합니다.
- 하지만 인터페이스 교체 시 호출 클래스도 수정해야 합니다.

![img](https://blog.kakaocdn.net/dn/bYrF6D/btrswWcVGD9/Qs2MZkNC16HgdEYklgwhU0/img.png)

HelloMain

```
public class HelloMain {

	public static void main(String[] args) {
		HelloMessage helloMessage = new HelloMessageKor();
//		HelloMessage helloMessage = new HelloMessageEng();
		
		String greeting = helloMessage.hello("모진혁");
//		String greeting = helloMessage.hello("Mr. Mo");
		
		System.out.println(greeting);
	}
	
}
```

HelloMessage (Interface)

```
public interface HelloMessage {

	String hello(String name);
	
}
```

HelloMessageEng

```
public class HelloMessageEng implements HelloMessage {

	public String hello(String name) {
		return "Hello " + name;
	}
	
}
```

HelloMessageKor

```
public class HelloMessageKor implements HelloMessage {

	public String hello(String name) {
		return "안녕하세요 " + name;
	}
	
}
```

Step 1의 경우 

객체를 선언할 때에 HelloMessageKor인지 Eng인지 선언을 꼭 해줘야 했는데 Interface를 적용 한 이후에는

HelloMessage helloMessage = new HelloMeeesageKor() 이런 식으로 공통적으로 묶이는 것을 볼 수 있습니다.

이제는 new에서 선언하는 부분만 바꿔주면 되는 약간은 루즈한 결합이 됩니다.

그리고 인터페이스에서 hello라는 클래스로 선언해주었기 때문에 HelloMessageKor에서 클래스명을 바꾸고 싶어도 못 바뀌게 됩니다.

아직까지는 어느 한쪽이 바뀌게 되면 여러 가지가 바뀌게 되는 건 남아 있습니다.

Step 3은 팩토리 호출 방식입니다.

객체 간의 강한 결합을 Factory를 통해 결합도를 낮춥니다.

팩토리 호출 방식이고

팩토리 방식은 팩토리가 구현 클래스를 생성하므로 클래스는 팩토리를 호출합니다.

인터페이스 변경 시 팩토리만 수정하면 됩니다. 호출 클래스는 영향을 미치지 않습니다.

하지만 클래스에 팩토리를 호출하는 소스가 들어가야 해서 그것 자체가 팩토리에 의존함을 의미합니다.

![img](https://blog.kakaocdn.net/dn/zEdUz/btrsvedT79F/CSpyUV2GxTcsa62APsCYu1/img.png)

HelloMain

```
public class HelloMain {

	public static void main(String[] args) {
		HelloMessage helloMessage = HelloMessageFactory.getHelloMessage("kor");
//		HelloMessage helloMessage = HelloMessageFactory.getHelloMessage("eng");
		
		String greeting = helloMessage.hello("모진혁");
//		String greeting = helloMessage.hello("Mr. mo");
		
		System.out.println(greeting);
		
		System.out.println("----------------------------------------");
		
		HelloMessage kor1 = HelloMessageFactory.getHelloMessage("kor");
		HelloMessage kor2 = HelloMessageFactory.getHelloMessage("kor");
		System.out.println(kor1 + " ::::: " + kor2);
	}
	
}
```

HelloMessageFactory

```
public class HelloMessageFactory {

	public static HelloMessage getHelloMessage(String lang) {
		if("kor".equals(lang)) {
			return new HelloMessageKor();
		} else if("eng".equals(lang)) {
			return new HelloMessageEng();
		} else {
			return null;
		}
	}
	
}
```

HelloMessage (Interface)

```
public interface HelloMessage {

	String hello(String name);
	
}
```

HelloMessageEng

```
public class HelloMessageEng implements HelloMessage {

	public String hello(String name) {
		return "Hello " + name;
	}
	
}
```

HelloMessageKor

```
public class HelloMessageKor implements HelloMessage {

	public String hello(String name) {
		return "안녕하세요 " + name;
	}
	
}
```

하지만 팩토리에 단점 같은 경우는 싱글톤이 적용이 안되었다는 점입니다.

Main을 실행해 봤을 때

결과 값으로

안녕하세요 모진혁
\----------------------------------------
co[m.ssafy.hello.di3.HelloMessageKor@762efe5d](http://m.ssafy.hello.di3.HelloMessageKor@762efe5d/) ::::: co[m.ssafy.hello.di3.HelloMessageKor@5d22bbb7](http://m.ssafy.hello.di3.HelloMessageKor@5d22bbb7/)

위와 같이 주소가 다름을 알 수 있습니다.

마지막 Step 4인 Assembler를 통해 결합도를 낮추는 방법입니다.

- Ioc 호출 방식이고
- 팩토리 패턴의 장점을 더하여 어떠한 것 에도 의존하지 않는 형태가 됩니다.
- 실행 시점에 클래스 간의 관계가 형성이 됩니다.

![img](https://blog.kakaocdn.net/dn/tdX6K/btrstKRFTnC/3tv9kO3QMqVlOA79fiM8wk/img.png)

실행 순간에 조립기에 의해 생성이 되고 주입을 시작합니다.

외부 조립기 역할을 Spring Framework가 합니다.

코드를 통해 설명하겠습니다.

여기서는 외부에서 빈을 통해 생성하는 과정과 적용을 설명하겠습니다.

HelloMessage (Interface)

```
public interface HelloMessage {

	String hello(String name);
	
}
```

HelloMessageEng

```
public class HelloMessageEng implements HelloMessage {

	public HelloMessageEng() {
		System.out.println("HelloMessageEng Contructor Call!!!!!!!!!");
	}
	
	public String hello(String name) {
		return "Hello " + name;
	}
	
}
```

HelloMessageKor

```
public class HelloMessageKor implements HelloMessage {
	
	public HelloMessageKor() {
		System.out.println("HelloMessageKor Contructor Call!!!!!!!!!");
	}

	public String hello(String name) {
		return "안녕하세요 " + name;
	}
	
}
```

생성되는 시점을 보기 위해 생성자를 만들었습니다.

applicationContext.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


	<bean id="kor" class="com.ssafy.hello.di4.HelloMessageKor"></bean>
	<bean id="eng" class="com.ssafy.hello.di4.HelloMessageEng"></bean>
</beans>
```

HelloMain

```
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class HelloMain {

	public static void main(String[] args) {
		System.out.println("프로그램 시작");
		ApplicationContext context = new ClassPathXmlApplicationContext("com/ssafy/hello/di4/applicationContext.xml");
		System.out.println("xml readed!!!");
//		HelloMessage helloMessage = (HelloMessage) context.getBean("kor");
		HelloMessage helloMessage = context.getBean("kor", HelloMessageKor.class);
//		HelloMessage helloMessage = context.getBean("eng", HelloMessageEng.class);
		
		String greeting = helloMessage.hello("모진혁");
//		String greeting = helloMessage.hello("Mr. 모");
		
		System.out.println(greeting);
		
		System.out.println("----------------------------------------");
		
		HelloMessage kor1 = context.getBean("kor", HelloMessageKor.class);
		HelloMessage kor2 = context.getBean("kor", HelloMessageKor.class);
		System.out.println(kor1 + " ::::: " + kor2);
	}
	
}
```

print 결과

프로그램 시작

HelloMessageKor Contructor Call!!!!!!!!!
HelloMessageEng Contructor Call!!!!!!!!!
xml readed!!!
안녕하세요 모진혁
\----------------------------------------
co[m.ssafy.hello.di4.HelloMessageKor@682b2fa](http://m.ssafy.hello.di4.HelloMessageKor@682b2fa/) ::::: co[m.ssafy.hello.di4.HelloMessageKor@682b2fa](http://m.ssafy.hello.di4.HelloMessageKor@682b2fa/)

여기서는 주소가 같은 싱글톤이 적용되는 것을 볼 수 있습니다.

위에서는 프로그램에서 Context를 읽는 순간 bean을 생성하는 모습을 볼 수 있습니다.

step을 통해 결합도가 점차 루즈해지는 것을 볼 수 있었습니다.

다음은 Spring DI의 용어 정리입니다.

- Bean

  - 스프링이 IoC방식으로 관리하는 오브젝트를 말합니다.
  - 스프링이 직접 그 생성과 제어를 담당하는 오브젝트만을 Bean이라고 부릅니다.

- BeanFactory

  - 스프링이 IoC를 담당하는 핵심 컨테이너
  - Bean을 등록 생성 조회, 반환하는 기능을 담당
  - 일반적으로 BeanFactory를 바로 사용하지 않고 이를 확장한 ApplicationContext를 이용합니다.

- 애플리케이션

   

  콘텍스트(ApplicationContext)

  - BeanFactory를 확장한 IoC 컨테이너입니다.
  - Bean을 등록하고 관리하는 기본적인 기능은 BeanFactory와 동일합니다.
  - 스프링이 제공하는 각종 부가 서비스를 추가로 제공합니다.
  - BeanFactory라고 부를 때는 주로 빈의 생성과 제어의 관점에서 이야기를 하는 것이고, 애플리케이션 콘텍스트라고 할 때는 스프링이 제공하는 애플리케이션 지원 기능을 모두 포함해서 이야기하는 것입니다.

![img](https://blog.kakaocdn.net/dn/k83Fu/btrsxgoLrd0/R8jfGHUEcHNRg7SxpES3sK/img.png)