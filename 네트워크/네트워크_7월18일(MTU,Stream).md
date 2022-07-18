# MTU와 Packet단편화

MTU는 1500이라 봐도 무방하다.

MSS 는 - 20 -20 (IP header, Tcp header)해서 1460이다.

단편화는 언제 왜 발생하는가.



단편화가 발생하는 시나리오.

pc가 IP가 3.3.3.3이고 MTC는 1500 pc1

IP가 4.4.4.4이고 MTU는 1400 pc2가 있꼬

ip가5.5.5.5이고 MTU는 1500인  서버가 있다고 가정,

중요한건 pc1는 라우터를 거쳐 pc2를 거쳐 server를 가는 상황.

라우터1번에서 라우터2번으로 넘어갈 때 단편화가 발생합니다.

이때 pc1의 1500만큼의 데이터를 1400으로 자르고 100남은 만큼을 IPheader(id값이 같은 IPheader)를 카피하여 붙입니다 하지만 100남은 패킷의 경우는 offset의 정보가 함께 저장되어집니다..

이렇게 두개의 패킷으로 분할이 된상황.

이렇게 분할된 패킷이 언젠가는 재조합 되어야하지않을가? 일반적으로 재조립은 수신하는 쪽에서 조립을 실시합니다.

2022년에도 단편화가 발생하는가??? 거의 발생하지않는다.

발생하지 않는데 발생하는 이유는 대부분 VPN(IPSec)때문에 발생한다.



# 가래떡과 Stream

![img](https://blog.kakaocdn.net/dn/482c8/btrHDBqFmYg/1wmjR5NJiuo7SYiGeHV4r1/img.png)

payload는 상대적인 개념

Packet <- 데이터 덩어리.

Segment -> TCP 

UDP는 Datagram이라고함.

헤더의경우 아래로 캡슐화를 진행하면서 계층별로 header가 붙이는 개념.



