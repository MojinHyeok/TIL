## AOP가 필요한 상황

- 모든 메소드의 호출 시간을 측정하고 싶다면?
- 공통 관심 사항 vs 핵심 관심사항 
- 회원 가입 시간 회원 조회 시간을 측정하고 싶다면?

## EX)상황

상사님이 메소드마다 시간이 얼마나 걸리는지 확인을 해달라고 하는 상황

![img](https://blog.kakaocdn.net/dn/m922R/btrmHEnATRj/C5D6FcTHkp6kWUKRFkvlK0/img.png)

그래서 모든 메소드마다 시간을 구하는 코드를 넣고있는 상황 입니다.

### 여기서 **문제**

- 회원가입 ,회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.﻿
- 시간을 측정하는 로직은 공통 관심 사항이다.
- 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들기가 어렵다.
- 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야한다.

이러한 상황속에서 Spring의 고수께서 이러한 상황은 그냥 **AOP**를 사용하면 편리하게 해결할 수 있어요.!



AOP적용

AOP:Aspect Oriented Programming

공통 관심 사항 vs 핵심 관심 사항 분리

AOP의 개념

![img](https://blog.kakaocdn.net/dn/90t0z/btrmHE2dEzV/tUBYy4y3GV4LmKM7LBGt6K/img.png)AOP적용 전



![img](https://blog.kakaocdn.net/dn/dpg6Zq/btrmEF1Xchy/BcBFDRvoUcBnbTdceANIx0/img.png)AOP적용 후

AOP의 적용전에는 매 메소드마다 시간 측정 로직을 붙였다면

적용 후에는 시간 측정 로직을 한 군데에 모으고 필요하면 쓰는 개념.!

## **AOP의 적용**

![img](https://blog.kakaocdn.net/dn/cXaLFP/btrmKkvpzWn/VmyeP9sxevoUWQi3wUWv70/img.png)

AOP를 만들기 위해 클래스에 @Aspect 어노테이션을 적용시켜줘야 합니다.

@Around 는 자기가 적용하고 싶은 대상을 설정하는 것입니다.

## **해결**

- 회원가입, 회원 조회 등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리한다.
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들었다.
- 핵심 관심 사항을 깔끔하게 유지할 수 있다.
- 변경이 필요하면 이 로직만 변경하면 된다.
- 원하는 적용 대상을 선택할 수 있다.

스프링의 AOP 동작 방식 설명

![img](https://blog.kakaocdn.net/dn/bIXJN4/btrmJjjhbHO/5T0xNkkjsHhuJyvR7aiOfK/img.png)

적용 전에는 그냥 호출하는 방식

적용 후에는 가짜 멤버 서비스를 만들고 앞에 세워놓고 가짜 스프링 빈이 끝나면 joinPoint.proceed()를 실행한다. 