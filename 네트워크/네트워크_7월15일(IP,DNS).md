2022.07.15

# IP주소의 종류와 특징

IP는 주로 IPv4를 의미한다.

전체 주소의 길이는 32bit의 주소 체계를 가진다.

종류로서는 크게

Global, Private, Loopback이 존재한다.

Internet은 라우터와 DNS의 집합체이다.

Global IP 일때만 Routing을 한다.

Private일 경우에는 Routing을 하지 않는다. (설정을 하지 않는한.)

Private IP-> 작은 소규모 인터넷을 구축할 때 사용.

Global의 특징중 하나는 Internet에서 사용되어지는 Global IP중 고유성을 가진다.

Private의 경우는 쓰고있는 사람이많다. 고유성을 안가진다.



사설IP

A네트워크 8비트 개인ID 24 -> 10.x.x.x

B네트워크16/개인16 -> 172.16.x.x

C네트워크24/  개인8 -> 192.168.x.x

이런 경우는 주로 공유기(글로벌IP를 공유하는 느낌)에서 사용되어지는 



Loopback은 127.0.0.1을 주로 사용되어지는데 이것은 호스트 자기자신을 의미하게 됩니다.



# 전세계  인터넷을 멈추는 방법과 DNS

인터넷은 라우터와 DNS의 집합체이다.

DNS은 분산형 DB구조를 가진다.(계층적)

Root DNS 13대가 존재하는데, 이13대를 멈추면 전세계 인터넷이 멈추게 된다.



DomainName 

www(hostName).naver.com(도메인)

사람은 숫자보단 문자열을 잘 기억하기 때문에 ip주소를 외우기보다 문자열을 통해 하기 위해 DNS를 사용하였다.

www.naver.com 을 치게되면 어떤일이 일어나는가.

자기 PC메모리를 뒤져 DNS Cache를 검색하여 naver.com이 있는지 확인을 합니다.

그 이후에 host File을 검색하여 naver.com가 있는지 확인을 합니다 . 그래도 없다면

DNS를 사용합니다.(ISP가 제공하는 Cache DNS)





