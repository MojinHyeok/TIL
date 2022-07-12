2022.7.12

# Proxy의 활용 두번째 분석

분석.

![img](https://blog.kakaocdn.net/dn/bbU0F7/btrG9Tygiqf/H4tGsmHuapLlRvNN7xo1O0/img.png)

Web으로 통신을 한다면 기본적으로 HTTP통신을 하게 됩니다. 

하지만 HTTP를 통해 한다면 평문이기 때문에 정보가 다 돌아다니게 됩니다.

HTTPS 사용함으로 평문을 암호화 할 수 있게 됩니다.



HTTPS를 사용하면서 Wireshark를 통해 패킷을 분석하고 싶은데 Payload들이 다 암호화되어 버립니다.

그리하여 이것을 암호화를 풀기전 까지는 아무런 의미를 얻을수 없습니다.

HTTP는 User애플리케이션 단이기 때문에 Stream형태로 그냥 바로 해석하면 좋지만, 지금은 와이어샤크의 경우 Packet으로 받아 암호화된 데이터를 해석하는 경우입니다.

그리하여 Stream의 형태로 해석하기 위해 Proxy를 사용합니다.

예를들면 새로운 프록시 서버를 127.0.0.1로(루프백 IP) 설정을 하고 진행을 합니다.

프록시서버도 소켓이 열리게 되는데 

크롬이 구글을 열게되면 크롬의 소켓이 Proxy서버로 전달하게 되고 Proxy서버가 전달하게 됩니다.

그래서 크롬의 소켓이 Proxy 서버로 보내는 과정에서 Stream형태의 평문을 얻을 수 있고 그대로 해석할 수 있습니다.

루프백을 프록시로 줘버리면, 평문 스트림 데이터를 SSL 건너기 전에도 받아볼 수 있다.

Fiddler를 통해 자기 자신의 소켓통신을 분석해 볼 수 있습니다.



# Proxy의 활용 세 번째, 감시와 보호.

![img](https://blog.kakaocdn.net/dn/bzaUQp/btrG5rwKes3/3DQOor4eogHH8KtxLu9b50/img.png)

네트워크대역에 PC가 여러개가 존재할 때에 

모든 Pc에 대해 Proxy설정 3.3.3.100으로 설정을하고 해보겠습니다.

예를들어 3.3.3.2인 PC1번이 Youtube를 연결하려고할 때 Proxy서버를 거쳐 Youtube를 갈 수 밖에 없습니다.

이럴 때 youtube 라면 상관 없는데 악성코드가 있는 사이트에 접속해본다고 가정을 해보면 

이럴때도 똑같이 Proxy를 거쳐서 악성코드가 있느 사이트에 가게 됩니다. 

이럴때 Proxy서버가 악성코드 유입을 차단해주기도 합니다.(악성코드의 사이트가 다시보낼 때 Proxy에서 차단해주는 개념)

이럴 때 바이러스를 차단해주어 VirusWall이라고도합니다.



또한 감시역할을 하기도하는데.

모두 Proxy서버를 거쳐가기 때문에 모든 컴퓨터를 감시가 가능하기도 합니다.



# Proxy의 활용 네 번째, Reverse Proxy

Reverse Proxy

Client측이 아니라 Server측에서도 Proxy 구조가 존재합니다. 이를 Reverse Proxy라하며 보안이나 부하분산 등 다양한 장치에 적용되는 구조입니다

![img](https://blog.kakaocdn.net/dn/suGYm/btrG6drkZTE/gbppYRrOO8QytEykoiyKU1/img.png)

대부분 클라이언트에 대한 설명이었는데, 웹 서버에서 Proxy를 두는 것을 설명하겠습니다.

지금은 웹서버에서 앞에 Proxy서버를 두는 것을 가정합니다.

Google서버는 192.168.0.20이고 Proxy는 5.5.5.5라고 가정하였을 때

PC1이 DNS서버에게 구글의 웹서버 주소를 알려줘라고 하면 본서버를 알려주지않고 Proxy서버의 아이피인 5.5.5.5를 알려주게 됩니다. 그리고 Proxy서버는 다시 본웹서버에게 부탁하여 정보를 받아서 전달하게 됩니다.



이것을 왜 앞단에 두었을까?..

일단 Internet은 public이고 웹서버는 private를 가져 외부에서 접근이 붙가능하게 되고

Proxy서버는 감시+보호역할을 하고, 공격을 식별하고 차단도합니다.

이러한 경우를 WAF(WebApplication Firewall)라고 합니다.  (WebApplication이기 때문에 7계층에서의 StreamData를 주고받습니다.)

이러한 경우를 서버를 위한 Proxy인데 이러한 경우를 ReverseProxy라고합니다.



![img](https://blog.kakaocdn.net/dn/bOMaYZ/btrHaox3RLg/BI4TrZJzinvxtyi27d6mW1/img.png)