## **Temporal Couplling**

함수들이 순서를 지키며 호출되어야 한다.

JDBC의 경우 connenction 연결 ->스테이트먼트 열고->...->연결 종료하고 이러한 과정이 있습니다. 



// file should be opened before processing

**fileCommand.process(file);**

// file should be closed after processing

위 방법은 실수할 여지도 많고 좋지않다.(누군가는 open안하고 실행하거나 실행하고 close를 안할 수도 있으니?)

전략패턴

```
fileCommandTemplate.process(myfile, new FileCommand(){
    public void process(File f){
        // file processing codes here
    }
});

class FileCommandTemplate{
    public void process(File file, FileCommand command){
        file.open();
        command.process(file);
        file.close();
    }
}
```



전략패턴과 컨텍스트 템플릿이라 불리우고, 외부에 전략을 둠으로 확장성 또한 고려되었고, 파일을 열고 닫는 부분까지 구성되어 있으므로 사용자는 주의하여 코딩하는 부담감을 줄일 수 있습니다.



## **CQS(Command Query Separate)**

Command- 내부상태 변경

Query- 내부 상태 변경 x

상태를 변경하는 함수는 값을 변경하면 안된다.

값을 반환하는 함수는 상태를 변경하면 안된다.

![img](https://blog.kakaocdn.net/dn/bUov7Z/btrs3btB3z1/PIM5G2CPCEMiZWkkGaD4F0/img.png)

일종의 약속..!입니다.(신뢰에 기반-> 저 사람의 행동이 예측가능하다..)

당신 코드의 독자들을 혼란스럽게 만들지 말라..



2번과 같은 경우는 User를 사용하기 위해서 매번 로그인을 해야하는 상황 혹은 User를 받고 싶지않은데 로그인할 때마다 User의 정보를 받는다는 것..



## **Tell Don't Ask**



What에 대해서 생각하라. How에 대해서 생각하지 마라. 

![img](https://blog.kakaocdn.net/dn/dz7lPD/btrs1RvUfMJ/VxAZ4p6FRPgzTW9X5Dez6K/img.png)

1번째 방식이 제일 나쁜 방법이고 2 번째와 세번째가 올바른 예시입니다. 

Tell Don't Ask를 잘 지킨다면, Query 로직이 없어지게 됩니다. 위의 예제에서도 user.isLoggedIn과 같은 로그인 상태를 가져오는 쿼리 부분이 사라졌습니다.



![img](https://blog.kakaocdn.net/dn/cIg5Z8/btrs66rlnnI/Ywg3DvA07tNOeem9isXWp0/img.png)

첫 사진의 o는 함수에서 너무 많은 부분을 알아야 한다는 점이 잘못 되었습니다. 최종적으로 o.DoSomething() 만 하면 된다. 배보다 배꼽이 더 큰 경우?



## **early returns**

메소드내에서 return이 있다면 위에 나타나야한다. 

함수를 다 읽지 않고, 현재 어떤 조건일 때 return 이 되는지 보고 싶을 경우가 존재. 다양한 경우를 생각한다면 return은 빠르게 나올 수록 좋다.

Loop문 중간에 return이 되는 경우는 문제가 된다(코드를 읽어나가는데 어려움)

코드가 동작하도록 하는 것보다 이해할 수 있게 하는 것이 더 중요.



## Null is not a error

에러를 null 로 처리하지 말자

에러는 예외를 던지는 방식으로 처리하자.