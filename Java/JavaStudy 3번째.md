## 목표

자바가 제공하는 다양한 연산자 학습

## 학습할 것

- 산술 연산자
- 비트 연산자
- 관계 연산자
- 논리 연산자
- instanceof
- assignment(=) operator
- 화살표(->) 연산자
- 3항 연산자
- 연산자 우선 순위

## **산술연산자.**

산술 연산자는 수학에서 산술적 계산을 수행하는 연산자입니다. Java에서 산술 연산자는 정수, 실수, 문자, 문자열 등의 데이터 타입에서 사용할 수 있으며, 변수나 상수의 값을 계산하거나 연산 결과를 반환하는 데 사용됩니다.

Java에서는 다음과 같은 산술 연산자를 제공합니다.

1. 덧셈 연산자 (+): 두 피연산자의 합을 계산합니다.
2. 뺄셈 연산자 (-): 왼쪽 피연산자에서 오른쪽 피연산자를 뺀 값을 계산합니다.
3. 곱셈 연산자 (*): 두 피연산자의 곱을 계산합니다.
4. 나눗셈 연산자 (/): 왼쪽 피연산자를 오른쪽 피연산자로 나눈 결과를 계산합니다.
5. 나머지 연산자 (%): 왼쪽 피연산자를 오른쪽 피연산자로 나눈 나머지 값을 계산합니다.

예를 들어, 다음과 같이 두 변수를 선언하고 산술 연산자를 이용하여 값을 계산할 수 있습니다.

```
java code
int num1 = 10;
int num2 = 5;

int sum = num1 + num2;// 10 + 5 = 15int sub = num1 - num2;// 10 - 5 = 5int mul = num1 * num2;// 10 * 5 = 50int div = num1 / num2;// 10 / 5 = 2int mod = num1 % num2;// 10 % 5 = 0
```

나눗셈 연산자를 진행하면서 주의해야 할 사항들은 다음과 같습니다.

1. **정수 나눗셈의 결과**

```
int num1 = 10;
int num2 = 3;
int result = num1 / num2;
System.out.println(result);// 출력 결과: 3
```

위 코드에서는 **num1** 변수를 **num2** 변수로 나누어 **result** 변수에 저장하고, 결과값을 출력합니다. 이 경우 **result** 변수에는 **3**이 저장됩니다. 이는 **num1 num2**가 모두 **int** 타입의 변수이기 때문입니다. **따라서, 정확한 결과값을 얻고자 한다면 num1 또는 num2 중 하나를 double 또는 float 타입으로 변환하여 계산해야 합니다.( 자바에서는 두 숫자의 데이터 타입이 다를 경우, 작은 데이터 타입에서 큰 데이터 타입으로 자동 형변환이 일어나게 됩니다.**

**따라서, int 타입과 double 타입의 연산에서는 int 타입이 double 타입으로 자동 형변환되어 나눗셈이 수행됩니다. 이렇게 되면 결과값이 double 타입으로 계산되어 소수점 이하 자리수까지 포함된 결과가 나오게 됩니다.)**

```
int num1 = 10;
double num2 = 3;
double result = num1 / num2;
System.out.println(result);// 출력 결과: 3.3333333333333335
```

위 코드에서는 **num1** 변수를 **num2** 변수로 나누어 **result** 변수에 저장하고, 결과값을 출력합니다. 이 경우 **result** 변수에는 3.3333333333333335가 저장됩니다. 이는 **num2**가 **double** 타입의 변수이기 때문입니다.

**2.0으로 나누는 경우**

```
int num1 = 10;
int num2 = 0;
int result = num1 / num2;// ArithmeticException 예외 발생
```

위 코드에서는 **num1** 변수를 **num2** 변수로 나누어 **result** 변수에 저장하려고 합니다. 하지만 **num2** 변수가 0으로 초기화되어 있기 때문에 **ArithmeticException** 예외가 발생합니다. 이를 방지하기 위해서는 num2가 0인지 먼저 확인하고, 0이 아닐 때에만 나눗셈을 수행해야 합니다.

## **비트연산자**

비트 연산자는 프로그래밍에서 사용되는 연산자 중 하나로, 2진수 형태의 데이터를 다루는 데 사용됩니다. Java에서는 다음과 같은 비트 연산자들을 지원합니다.

**AND(&) 연산자**

```
int a = 5;// 0101 (2진수)int b = 3;// 0011 (2진수)int result = a & b;// 0001 (2진수)
System.out.println(result);// 1 출력
```

위의 코드에서는 비트 연산자 &를 사용하여 a와 b의 비트를 비교하고, 두 비트가 모두 1인 경우에만 1을 반환하여 result 변수에 저장합니다. 이후, result 변수를 출력하면 1이 출력됩니다.

**OR(|) 연산자**

```
int a = 5;// 0101 (2진수)int b = 3;// 0011 (2진수)int result = a | b;// 0111 (2진수)
System.out.println(result);// 7 출력
```

위의 코드에서는 비트 연산자 |를 사용하여 a와 b의 비트를 비교하고, 두 비트 중 하나라도 1인 경우 1을 반환하여 result 변수에 저장합니다. 이후, result 변수를 출력하면 7이 출력됩니다.

**XOR(^) 연산자**

```
int a = 5;// 0101 (2진수)int b = 3;// 0011 (2진수)int result = a ^ b;// 0110 (2진수)
System.out.println(result);// 6 출력
```

위의 코드에서는 비트 연산자 ^를 사용하여 a와 b의 비트를 비교하고, 두 비트가 서로 다른 경우 1을 반환하여 result 변수에 저장합니다. 이후, result 변수를 출력하면 6이 출력됩니다.

**NOT(~) 연산자**

```
int a = 5;// 0101 (2진수)int result = ~a;// 1010 (2진수)
System.out.println(result);// -6 출력
```

위의 코드에서는 비트 연산자 ~를 사용하여 a의 비트를 모두 반전시키고, 결과 값을 result 변수에 저장합니다. 이후, result 변수를 출력하면 -6이 출력됩니다. 이때, 결과 값이 음수인 이유는 부호 비트가 1이기 때문입니다.

(Java에서 정수형 데이터 타입은 2의 보수를 사용하여 음수 값을 표현합니다. 따라서, 1010은 음수 값으로 해석되어야 합니다.

1010을 2의 보수로 변환하면 0101(1의 보수) + 1 = 0110(2의 보수)이 됩니다. 이때, 첫 번째 비트가 1이므로 이는 음수 값을 나타냅니다. 그리고 2의 보수인 0110을 10진수로 변환하면 -6이 됩니다.

따라서, 1010은 Java에서 음수 값으로 해석되어 -6이 출력되는 것입니다.)

## **관계연산자**

두 개의 값을 비교하여 그 결과를 참(true) 또는 거짓(false)으로 반환합니다.

비교적 간단한 연산자여서 간단한 예시만 사용한 이후에 넘어가겠습니다.

```
int a = 1;
int b = 2;

System.out.println(a == b); // false
System.out.println(a != b); // true
System.out.println(a > b);  // false
System.out.println(a >= b); // false
System.out.println(a < b);  // true
System.out.println(a <= b); // true
```

## **논리연산자**

논리적인 값을 비교하여 그 결과를 참(true) 또는 거짓(false)으로 반환합니다

Java에서 제공하는 논리 연산자는 다음과 같습니다.

1. && (논리 곱, AND)
2. || (논리 합, OR)
3. ! (논리 부정, NOT)

논리연산자들은 주로 if문과 반복문에서 조건식을 작성할 때 사용됩니다. 아래 예시를 통해 각 연산자의 사용법을 살펴보겠습니다.

```
int a = 10;
int b = 20;
int c = 30;

// 논리 곱(AND) 두 조건 다 성립해야함if (a > b && b < c) {
  System.out.println("a는 b보다 크고, b는 c보다 작습니다.");
}

// 논리 합(OR) 둘 중하나만 성립하면 OKif (a > b || b < c) {
  System.out.println("a는 b보다 크거나, b는 c보다 작습니다.");
}

// 논리 부정(NOT) 조건문이 아닐 때 Okif (!(a > b)) {
  System.out.println("a는 b보다 크지 않습니다.");
}
```

각각의 논리 연산자를 사용하여 a, b, c의 값을 비교하고, 결과에 따라 적절한 메시지를 출력합니다.

OR을 사용할 때 앞의 조건이 충족된다면 뒤 조건은 실행하지 않습니다.

위의 예시를 보면, 논리 연산자를 사용하여 여러 조건식을 조합할 수 있다는 것을 알 수 있습니다. 이를 이용하여 다양한 조건식을 작성할 수 있습니다.

## **instance of**

instanceof는 Java에서 객체가 특정 클래스의 인스턴스인지를 검사하는 연산자입니다. 이를 통해 객체의 타입을 동적으로 확인할 수 있습니다.

instanceof 연산자는 다음과 같은 형식으로 사용됩니다.

```
객체 instanceof 클래스
```

여기서 객체는 검사하려는 객체를, 클래스는 검사하려는 클래스를 나타냅니다. 객체가 해당 클래스의 인스턴스일 경우 true를 반환하고, 그렇지 않을 경우 false를 반환합니다.

다음은 instanceof의 예시입니다.

```
class Animal {}

class Dog extends Animal {}

class Cat extends Animal {}

public class Main {
  public static void main(String[] args) {
    Animal animal1 = new Animal();
    Animal animal2 = new Dog();
    Animal animal3 = new Cat();

    System.out.println(animal1 instanceof Animal);// true
    System.out.println(animal2 instanceof Animal);// true
    System.out.println(animal3 instanceof Animal);// true
    System.out.println(animal2 instanceof Dog);// true
    System.out.println(animal3 instanceof Dog);// false
    System.out.println(animal1 instanceof Dog);// false
  }
}
```

위의 코드에서는 Animal, Dog, Cat 클래스를 정의하고, 각각의 객체를 생성합니다. 그리고 instanceof를 사용하여 각 객체의 타입을 검사합니다. 결과적으로 animal1, animal2, animal3은 모두 Animal 클래스의 인스턴스이므로 true를 반환합니다. animal2는 Dog 클래스의 인스턴스이기 때문에 true를 반환하고, animal3은 Cat 클래스의 인스턴스이므로 false를 반환합니다.

## **assignment(=) operator**

assignment operator는 변수에 값을 할당할 때 사용됩니다. 다음과 같은 형식으로 사용됩니다.

```
변수 = 값;
```

여기서 변수는 할당하려는 변수를, 값은 변수에 할당할 값을 나타냅니다. 할당 연산자는 오른쪽의 값을 왼쪽의 변수에 할당합니다. 이때 할당 연산자는 오른쪽에서 왼쪽으로 진행됩니다. 즉, 오른쪽의 값을 계산한 후에 왼쪽의 변수에 값을 할당합니다.

## **화살표 연산자**

메서드를 하나의 식으로 표현한 것이 화살표 연산자입니다.

화살표 연산자(->)는 람다식(lambda expression)에서 사용됩니다.

람다식(lambda expression)은 Java 8에서 새로 추가된 기능 중 하나로, 함수형 프로그래밍을 지원하기 위해 만들어졌습니다. 람다식은 함수형 인터페이스(functional interface)를 구현하는 익명 클래스를 간결하게 표현한 것입니다.

함수형 인터페이스는 다음과 같은 특징을 가집니다.

Java 8부터 함수형 프로그래밍을 지원하기 위해 함수형 인터페이스라는 개념이 등장했습니다.

함수형 인터페이스는 딱 하나의 추상 메서드만을 가지는 인터페이스를 말합니다. 이러한 인터페이스는 람다식을 사용하여 구현할 수 있으므로 함수형 프로그래밍에서 중요한 역할을 합니다.

Java에서는 함수형 인터페이스를 @FunctionalInterface 어노테이션을 사용하여 표시할 수 있습니다.

이 어노테이션을 사용하면 인터페이스 내부에 추상 메서드가 두 개 이상인 경우 컴파일러가 에러를 발생시킵니다.

람다식은 다음과 같은 형식으로 작성됩니다.

```
(매개변수) -> {실행문}
```

여기서 매개변수는 메서드에서 사용할 매개변수를, 실행문은 메서드의 내용을 작성합니다. 실행문이 한 줄일 경우 중괄호를 생략할 수 있습니다.

다음은 람다식을 사용하는 예시입니다.

```
@FunctionalInterface
public interface Runnable {
	public void run ();
}

public class Main {
  public static void main(String[] args) {
// 익명 클래스를 사용하는 경우
    Runnable runnable1 = new Runnable() {
      @Override
      public void run() {
        System.out.println("Hello, world!");
      }
    };

// 람다식을 사용하는 경우
    Runnable runnable2 = () -> {
      System.out.println("Hello, world!");
    };

// 실행
    runnable1.run();
    runnable2.run();
  }
}
```

위의 코드에서는 람다식을 사용하여 Runnable 인터페이스를 구현하는 방법과, 익명 클래스를 사용하는 방법을 비교합니다. 람다식을 사용하면 코드가 간결해지고 가독성이 좋아집니다.

화살표 연산자(->)는 람다식에서 매개변수와 실행문을 구분하는 역할을 합니다. 화살표 연산자 왼쪽에는 매개변수를, 오른쪽에는 실행문을 작성합니다.

예를 들어, 다음과 같은 람다식은 x와 y를 더한 결과를 반환합니다.

```
(x, y) -> x + y
```

위의 람다식은 다음과 같은 메서드와 같은 역할을 합니다.

```
public int sum(int x, int y) {
  return x + y;
}
```

또한, 화살표 연산자는 다음과 같은 형식으로 사용될 수 있습니다.

```
(매개변수1, 매개변수2, ...) -> {실행문}
```

람다식은 함수형 프로그래밍에서 매우 유용한 개념으로, 코드의 가독성과 유지보수성을 높여줍니다. 하지만 람다식이 남발될 경우 코드의 가독성을 해치는 경우도 있습니다.

## **3항 연산자**

조건연산자의 결과에 따라 평가식을 반환하는 연산자.

삼항 연산자는 아래와 같은 구조를 가집니다.

```
조건식 ? 표현식1(true) : 표현식2(false)
```

조건식은 boolean 타입을 반환하는 식이며, 표현식 1과 표현식 2는 각각 조건식이 true 또는 false일 때 반환될 값입니다. 조건식이 true일 경우에는 표현식 1이 반환되며, false일 경우에는 표현식 2가 반환됩니다.

아래는 삼항 연산자의 예시입니다.

```
int x = 10;
int y = 20;
int result = (x > y) ? x : y;
System.out.println(result);// 20
```

위 코드에서는 x와 y를 비교하여 x가 y보다 큰 경우 x를, 그렇지 않은 경우 y를 result 변수에 저장하고 출력합니다.

삼항 연산자는 if-else 문과 유사한 역할을 하지만, 코드를 간결하게 작성할 수 있으며, 단일한 표현식으로 사용할 수 있는 장점이 있습니다.

하지만, 복잡한 조건식을 사용하는 경우 코드의 가독성이 떨어질 수 있으므로 적절한 사용이 필요합니다. 또한, 삼항 연산자를 남용하면 코드가 복잡해지고 유지보수가 어려워질 수 있으므로 주의가 필요합니다.

## **연산자 우선순위**

연산자 우선순위는 여러 개의 연산자가 하나의 표현식에서 함께 사용될 때, 어떤 연산자가 먼저 계산되어야 하는지를 결정하는 규칙입니다. 예를 들어, 2 + 3 * 4와 같은 표현식에서는 * 연산자가 먼저 계산되어야 하므로 결과는 14가 됩니다. 그러나 만약 2 + 3 * 4를 (2 + 3) * 4와 같이 괄호를 사용하여 표현하면 덧셈 연산이 먼저 이루어지므로 결과는 20이 됩니다.

| 연산자                   | 우선순위             |
| ------------------------ | -------------------- |
| ()                       | 최우선               |
| !, ~, ++, --, +, -       | 단항연산자           |
| *, /, %                  | 곱셈, 나눗셈, 나머지 |
| +, -                     | 덧셈, 뺄셈           |
| <<, >>, >>>              | 시프트 연산자        |
| <, >, <=, >=, instanceof | 관계 연산자          |
| ==, !=                   | 같다, 같지 않다      |
| &                        | 비트 AND             |
| ^                        | 비트 XOR             |
|                          |                      |
| &&                       | 논리 AND             |
|                          |                      |
| ?:                       | 조건 연산자          |
| =, +=, -=, *=, /=, %=    | 할당 연산자          |

배운점.

```
int start = 0;
int end = 10;
int mid = (start + end )//5//위의 코드의 문제점
start = 2_000_000_000;
end = 2_100_000_000;
mid = (start + end ) / 2;
System.out.print.ln( mid );//-97483648//위의 코드의 경우 오버플로우가난 상태에서 나누기를 해버려서 원하는 값이 안나옵니다.// 21억 + 21억을 해버리면 int형은 21억이 최대인데 초과해버려서 오버플로우가 발생.//해결법
start = 2_000_000_000;
end = 2_100_000_000;
mid = start + (end - start) / 2;
System.out.println(mid)//2050000000
```

XOR - > 같으면 0 다르면 1

```
// TODO numbers라는 int형 배열이 있다.// 해당 배열에 들어있는 숫자들은 오직 한 숫자를 제외하고는 모두 두번씩 들어있다.//오직 한 번만 등장하는 숫자를 찾는 코드를 작성하라.public static void main(String[] arsgs){
	Hello hello = new Hello();
	int result = hello.solution(new int[] {5,2,4,1,2,4,5,});
    System.out.println(result);
    }

// 5^ 0 = 5// 5 ^ 5 = 0// 101// 101// ---// 000// XOR을 활용하여 답을 얻는 문제private int solution(int numbers){
    	int result = 0;
        for(int nuber : numbers){
        	result ^= number;
        }
        return result;
    }
```