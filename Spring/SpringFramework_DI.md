Spring 삼각형

- Enterprise Application 개발 시 복잡함을 해결하는 Spring의 핵심
  - POJO
  - PSA
  - IoC/DI
  - AOP

POJO

- 특정 프레임워크나 기술에 의존적이지 않은 자바 객체

- 특정 Frameworkd에 종속되지 않는다.
- 테스트하기 용이하며, 객체지향 설계를 자유롭게 적용할 수 있다.

IOC/DI

- DI는 유연하게 확장 가능한 객체를 만들어 두고 객체 간의 의존관계는 외부에서 다이나믹하게 설정

AOP

- **관심사**의 분리를 통해서 소프트웨어의 모듈성을 향상
- 공통 모듈을 여러 코드에 쉽게 적용가능

AOP의 예시



공통적으로 호출하는 것을 메소드를 만들지 않고 설정으로 호출할 수 있게 해주는 것.



SpringFramework의 특징

- 경량 컨테이너
  - 언제든지 스프링 컨테이너로부터 필요한 객체를 가져와 사용할 수 있따.
-  DI(의존성 지원)패턴 지원
  - 스프링은 설정파일(xml version2.xx)이나 어노테이션(주로 version 3.x)을 통해서 객체 간의 의존 관계를 설정 할 수 있습니다.
  - 따라서, 객체는 의존하고 있는 객체를 직접 생성하거나 검색할 필요가 없다.
- IoC(제어의 반전)
  - IoC는 스프링이 갖고 있는 핵심적인 기능이다.
  - 자바의 객체 생성 및 의존관계에 있어 모든 제어권은 개발자에게 있엇다.
  - 스프링에서 객체에 대한 생성과 생명주기를 관리할 수 있는 기능을 제공하고 있는데 이런이유로 [Spring Container]또는 [IoC Container]라고 부른다.
- 스프링은 영속성과 관려된 다양한 API를 지원
  - 스프링은 JDBC를 비롯하여 Mybatis,Hibernate,JPA등 DB처리를 위해 널리 사용되는 라이브러리와 연동을 지원하고 있습니다.

## **SpringFramework Module**

![img](https://blog.kakaocdn.net/dn/k3hAX/btrmEF0GspU/ylvoOtdjzk1zndXyVVvTf0/img.png)

Spring Core=가장 기반이 되는 모듈

스프링은 내부적으로 싱글톤과 팩토리 패턴을 지원한다. 

이러한 것은 설정을 통해 풀수도 있다.

프레임은 레고블럭이다~.자기에게 필요한 것을 사용하면 된다.

![img](https://blog.kakaocdn.net/dn/OWsjj/btrmDRm1te7/mVh47XF2fMR8pJ1gkfC9Sk/img.png)스프링 모듈 알고만 가자.



## **DI**

![img](https://blog.kakaocdn.net/dn/blCphg/btrmAJXnPW8/L8hPY6PJntrGImsNUPxYgk/img.png)XML사용법

property=setter

constructor=생성자를 이용한다.



## 스프링 빈 설정 Annotation(프로젝트를 할때 주로 이걸 사용)

Annotation

- 어플리케이션의 규모가 커지고 빈의 개수가 많아질 경우 XML 파일을 관리하는 것이 번거로움

- 빈으로 사용될 클래스에 특별한 annotation을 부여해주면 자동으로 빈 등록 가능

- 오브젝트 빈 스캐너 로 빈 스캐닝 을 통해 자동등록

  - 빈 스캐너는 기본적으로 클래스 이름을 빈의 아이디로 사용

  - 정확히는 클래스 이름의 첫 글자만 소문자로 바꾼 것을 사용

  - ![img](https://blog.kakaocdn.net/dn/xoT43/btrmEc5xBMi/nRWQk9Yf046fscE4doO4kK/img.png)

    Component는 크게 3가지로 나눕니다(아래에 포함안되거나 애매하면 Component)

    - Controller
    - Service
    - Repository(Dao)

Autowired를 사용하면 타입이 MemberDao인 것을 저기에 주입 해라!!!

