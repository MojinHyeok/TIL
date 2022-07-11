2022 07 11 네트워크

# Router의 내부 구조와 Inline

![img](https://blog.kakaocdn.net/dn/nr7cq/btrG4HxMtqm/OlUQffcdyGObpC59j3kwkk/img.png)

Router -> L3스위치 중 하나.

L3: 패킷 가지고 경로를 선택한다.

Router는 Interface를 두개 가지고 있다.

Router는 Host라고할 수 있다. Ip를 가지고 있으니.

Computer라고 할 수 있냐? Yes

하드웨어끼리 통신을 하고싶어도 3계층 4계층 다 거친 이후에 2계층으로 전달하게 됩니다. 이러한 과정들은 많은 비용이 들게 되는데 

하드웨어수준에서 다 처리 되었다 -> 가속 되었다.



Inline-> Packet단위로 무엇을 하는 구나 라는느낌

Bypass 하든지 Drop하든지.

방화벽은 Inline에서 보안적인 요소가 들어가면 방화벽이라고한다.

패킷 필터링 방화벽.,



방화벽과 라우터는 둘다 L3 스위치에 속하고, IP로 통신합니다. 내부적으로 라우터는 by_pass인지 drop인지를 결정하고 어디로 패킷을 보낼지를 지정해줍니다(이 두 과정을 inline 처리라고 합니다) . 패킷을 전송할 때 NIC 수준에서 바로 전송하느냐, IP(L3) 수준에서 처리한 이후 전송하느냐, 사용자 프로세스 수준에서 처리한 이후 전송하느냐에 따라서 라우터는 처리속도가 달라집니다. 기본적으로 low level에서 처리할 수록 빠르다(가속). 방화벽이라는 것 또한 L3 스위치의 일종인데, 이친구는 보안적인 이유로 by_pass와 drop을 결정합니다. 기본적으로 내부구조는 거의 동일한데, 판단기준이 라우팅용인가 보안 관리용인가 차이가 있습니다.



# Proxy의 구조와 작동원리

![img](https://blog.kakaocdn.net/dn/bxNZ5n/btrG3kDcRgN/fYKjefsYykLgD0n5ti2QUk/img.png)

(Application)Proxy

PC가 인터넷에 연결되어 있는 상황.

인터넷에 연결되어 있는 네이버에 연결을 하고 싶은 상황.

Pc의 주소는 3.3.3.3 

Naver는 5.5.5.5라고가정.

3.3.3.3 Host가 5.5.5.5에게 TCP/IP요청을한 상황.

소켓통신.

이것이 일반적인데.

Proxy는 PC2번이 등장하게 됩니다. 

PC2는 9.9.9.9 인 상황 (proxy)

Proxy는 사전적 으로 대리자 역할인데

PC가 네이버에 접속하고싶을때 9.9.9.9인 PC2를 거쳐 Proxy인 Pc2가 네이버에게 요청을하여 전송하는 상황.



# Proxy의 활용 첫번 째. '우회'

Proxy를 왜 사용하는가.

첫 번째 우회.

![img](https://blog.kakaocdn.net/dn/wT823/btrG5sf69Cp/sZfKjE7kPAQrywLt3CASwK/img.png)

PC#1이 Proxy서버를 활용하여 네이버를 접속하게 되면 네이버 입장에서는 Pc1이온다고 판단하는 것이 아니라.

Proxy서버의 IP로 온다고 판단한다.

하지만 Proxy의 경우 통신의 모든 활동을 감청할 수 있다라는 점.