Function Structure

첫 시작은 

1.Function Structure에서 **Arguments**입니다.

인자가 많아지면 복잡도가 증가한다.

인자의 개수는 3개가 최대이다.

클린 코딩의 예시를 말해줍니다.

![img](https://blog.kakaocdn.net/dn/mw1Em/btrsMa12syg/dle3KTrhQtLQBQeNUhBsU1/img.png)

startdate, enddate의 경우 Date Range로 선언하여 한 클래스로 만들어버리는 걸 추천!

생성자에 많은 수의 인자를 넘겨야 한다면?

이럴때는 Builder 패턴을 적용하는 것이 더 좋다.!

![img](https://blog.kakaocdn.net/dn/dgpsGW/btrsA7MzjSX/joKEy5L3IHmgX4myBZSww0/img.png)

위와 같은 코드는 자기가 선언해줘서 위치는 자기가 알지만 남들이 보면 어떠한 의도로 파라미터를 전달하는지 알기가 어렵다. 이와 같은 경우에 Builder를 사용해주면 좋다

![img](https://blog.kakaocdn.net/dn/N5UjK/btrsOhlTLsc/zzxKN6PJwEZ1kAscQLkFm0/img.png)

이렇게 필수 파라미터와 옵션 파라미터를 나누어 사용합니다.

![img](https://blog.kakaocdn.net/dn/nm7WO/btrsMWvyRrQ/hN5o2uFs3cf7pRuZLic5SK/img.png)

위와 같이 조금 더 이해하기 쉬운 코드로 바뀐 모습을 볼 수 있습니다.!

코드를 Null, 에러 체크로 더럽히지 말아라~!

null 체크를 하는 것 자체가 잘못되었다는 단서..

팀원이나 단위 테스트를 못 믿는다는 말

null 여부를 지속적으로 조사할 것이 아니라 단위 테스트에서 검증해야 함..

null체크보다 차라리 에러 처리를 잡는 것이.. 더 좋은.. 

\2. Stepdown Rule

간단하게 말하면 모든 public은 위에 모든 private은 아래에

![img](https://blog.kakaocdn.net/dn/buFs5f/btrsMVXHtlv/m0Q7G7yDroJtkQskE0mCXK/img.png)

private함수는 주로 public에서 사용하는 메서드인데 

여기서 함수명만 잘 작명했다면 public part만 사용자들에게 전달한다면 이해가 될 것이다라는 말입니다.

중요한 부분은 위로, 상세한 부분은 아래로 ex) 잡지의 예 : 헤드라인, ( 하이라이트만 보면 된다..라는 의미)

작성자는 자기의 의도를 남들에게 잘 전달할 수 있다.

독자는 맨 위로 이해가 된다면 아래는 안 읽어도 된다.

 

 