# Form

## Comment는 특별한 경우에만 작성되어야 한다.!!



주간보고나, 일간 보고와 같은 약간의 실용적이지 못한 해야되니까 하는 형식적인 일들

Coding Standards가 커멘트 작성을 강요하면 

- 프로그래머는 필요해서가 아니라 해야 하므로 커멘트를 작성함
- 이런 커멘트는 무의미하다.
- 이런 커멘트는 무시의 대상이 된다.

무의미한 커멘트는

양치기 소년의 말처럼 아무런 가치 없이 없어보이게 만든다.



comments should be rare -> prgrammers intent를 위해 반드시 필요할 때 그 커멘트를 읽는 모든 사람들이 감사해야 한다.



Comments are Failures

- 작성자의 의도가 잘 나타나게 프로그램을 작성한다면 커멘트가 불필요해짐.
- 모든 커멘트는 당신의 코드가 expressive하지 못하는 것을 나타내는 실패의 상징.



Bad Comments

- 중얼거림 말이 안끊어지는 커멘트. 
- 중복적인 설명이 들어가는 Comment
- 변경이력을 작성하는 Comment
- 반복문의 종료를 알리는 Comment
- 작성자를 알리는 Comment



공란처리하기

- 메소드와 메소드 사이
- private 변수들과  public 변수들 사이
- 변수 선언과 메소드 실행의 나머지 부분 사이.
- while 블록과 다른 코드 사이
- 서로 관련된 것들은 근접하게 해야함.
- vertical한 거리가 그들간의 관계를 의미함.



### 클래스란 무엇인가

private변수와 그 변수들에 대해서 동작하는 public 함수들의 집합.

- private 변수들을 작성함으로써 클래스를 작성한다.
- 그리고 그 private 변수들을 public 함수들로 조작한다.
- 외부에서는 private 변수들이 없는 것 처럼 보인다.

내부를 모르게 만든다면 내가 변경을 하더라도 외부에서 변화가 최소화 되게 됩니다.



왜 변수를 가지고 private로 선언이후 getter와 setter를 사용하는가?  -> 객체의 상태를 외부에서 사용할 수 있도록 하는 getter/setter등을 제공하는 것은 Bad Design

getter/setter는 cohesive하지 않다(응집도와 결합도 관련) 

- 하나의 변수만 사용하기 때문에 
- 클래스가 getter/setter를 많이 가질 수록 덜 cohesive해진다.

getter/setter가 없어야만 하나?..

- 안 쓸수는 없으니 최소화해라 그래야 cohesion을 최대화 할 수 있다.
- getter를 쓸 때 본래 그 변수를 그래도 노출하지 말아라..(getName()과 같이)
- 추상화를 통해 제공해라
- ![img](https://blog.kakaocdn.net/dn/b2hO3C/btrIeLyWRSk/fX0QRtoYSUtHisFtyeSGC0/img.png)



기계적으로 getGallonsOfGas()를 쓰게 된다면 디젤차의 경우는 가솔린을 사용하지 않게 되는데 그대로 상속받아 사용되어야 하는 경우가 나타날 수 있다.

이런 경우를 대비해 getPercentFuelRemaining()과 같이 남아 있는 연료를 알려달라 이런식으로 메소드명을 작성해주어야 더 좋다.



다형성

![img](https://blog.kakaocdn.net/dn/dm9F8D/btrIc98pn9V/sgZgeN2toPZY1Q3wk2NBI1/img.png)

잘 설계된 객체지향의 경우 CarDriver와 ElectirCar의 개발자 둘 다 외부를 상관쓰지 않고 독립적으로 개발이 가능하다.(추상화 매체인 Car만 유지된다면 독립적으로 개발 가능)

객체지향의 핵심  

- CarDriver의 경우  DiselCar나 NuclearCar에 대해 의존하지 않는다(영향을 안받는다).

