네트워크 강의



# TCP/IP통신과 MAC주소의변화

MAC주소의 변화에 대해서.

![image-20220717165854476](C:\Users\Mo.jh\AppData\Roaming\Typora\typora-user-images\image-20220717165854476.png)

소켓끼리 통신을 하게 되면  소켓은 Stream이라는 데이터를 만들고,

이 Stream데이터를 Segment로 분리하여 Packet이라는 통에 감싸게 됩니다.

그리고 L2계층인 이더넷에서 프레임의 단위로 감싸게 되어 이더넷 헤더와 함께 전송을 하게 됩니다.

그 이후에 PC에서 전송을하게되면서 출발지와 목적지를 라우터를 거치며 이더넷헤더의 정보가 업데이트 되게 됩니다.그래서 위의 사진에서 R5에서 오게 되면 출발지는 R5이고 목적지는 Server로 나옵니다.

L2구간을 통과할 때마다 헤더정보들이 바뀌게 된다.

L2구간은 Mac address가 중요하지 IP Address는 중요하지 않다.



# L2스위치와 ARP작동원리

ARP프로토콜: IP주소로 Mac Address를 알아내는 프로토콜.

L2구간에서는 Mac이 중요하다.

![img](https://blog.kakaocdn.net/dn/OJBs2/btrHwkoHbeQ/JVVqLge2ZxJHLGZdUmMCHK/img.png)

너네들 중에 IP주소가 192.168.0.1인 사람이 있는지 방송을 합니다.(Broadcast)

이것을 수신한 애들중 192.168.0.1인 PC가 응답을 보냅니다.(Unicast)

이 통신을 통해 Mac주소를 알게 됩니다.

이 정보들을 메모리에 저장하기 위해 ARP Cache에 저장하게 됩니다.



# 길 잃은 Packet의 소멸과 TTL

TTL에 관하여.

![img](https://blog.kakaocdn.net/dn/bES6pv/btrHvoZawyf/U7Tuu3PTd41YjeAf3LsCxK/img.png)

인터넷이란 라우터의 거대한 집합체이다.! (라우터 + DNS)

TTL(Time To Live)

Router가 계속 돌아다니면서 TTL이 1씩 감소하게 됩니다.

대략 8bit 2의 8승 0~255인데 Window는 현재 128

Looping : 똑같은 곳만 계속 돌아다니는 현상.

Looping을 방지하기 위해 TTL이 존재.

버려질 때 ICMP 을 활용하여 출발지주소에게 Packet이 소멸되었다고 말해줍니다.
보안상 때문에 위와 같은 활동을 안할 수도 있습니다.