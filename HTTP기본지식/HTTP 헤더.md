## **HTTP 헤더**

### 용도

HTTP 전송에 필요한 모든 부가정보

예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트,...등등

HTTP BODY

message body

![img](https://blog.kakaocdn.net/dn/NF7wG/btrr2si2C3D/RElpRkrnkyvmVUdZWRwbw1/img.png)

- 메시지 본문을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드 (payload)
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

## **표현**

Content-Type:표현 데이터의 형식

Content-Encoding:표현 데이터의 압축 방식

Content-Language: 표현 데이터의 자연 언어

Content-Length:표현 데이터의 길이

표현 헤더는 전송, 응답 둘다 사용

### **Content-Type(표현 데이터의 형식 설명)**

![img](https://blog.kakaocdn.net/dn/bT3CXS/btrr9fQMIgC/XIAUKpkq1wtm7JIBNskWBK/img.png)

미디어 타입,문자 인코딩

예)

- text/html; charset=utf-8
- application/json
- image/png

**Content-Encoding(표현 데이터 인코딩)**
표현 데이터를 압축하기 위해 사용

데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가

데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제

예) gzip, deflate

## **Content-Language(표현 데이터의 자연 언어)**

표현 데이터의  자연 언어를 표현 

이 표현 데이터가 어떤 언어를 사용하였는지 확인 할 수 있는 용도?

## **협상(콘텐츠 네고시에이션)클라이언트가 선호하는 표현 요청..(어려움..)**

Accept: 클라이언트가 선호하는 미디어 타입 전달

Accept-Charset: 클라이언트가 선호하는 문자 인코딩

Accept-Encoding: 클라이언트가 선호하는 압축 인코딩

Accept-Language: 클라이언트가 선호하는 자연 언어.

협상 헤더는 요청시에만 사용 (최대한 맞춰주세요..!의 느낌)

**예시**를 통해 알아보도록

**Accept-Language 적용전**

![img](https://blog.kakaocdn.net/dn/AIDor/btrseRIkQTB/TdFgRGKCwUMRfF28xgBKc0/img.png)

서버는 본래 기본 영어로 사용하는 거라 클라이언트가 영어인지 한국어를 사용하는지 몰라서 기본적으로 영어를 보냅니다.

**Accept-Language 적용 후**

![img](https://blog.kakaocdn.net/dn/kDFPq/btrr2WEUC4a/Oaopeaar9RmEdAtl4IGoCk/img.png)

하지만 적용 한 이후에는 한국어도 지원을 하기 때문에 클라이언트가 요청을 하면 한국어로 번역된 홈페이지를 드립니다.! 라는 느낌

하지만 Accept-Language를 적용해도 아쉬운 사항이 존재하는데요

![img](https://blog.kakaocdn.net/dn/smXtA/btrseQJq2i4/2VLdcLKY5HxRx1EWmFhk11/img.png)

위의 상황은 기본적으로 독일어를 지원하는 상황에서 클라이언트는 한국어를 원하는 데 그래도 한국어가 없다면 그래도 독일어보다는 영어가 좋지 않을가 라는 상황입니다.

이러한 상황을 해결하기 위하여

## **협상과 우선순위(Quality Values)**

![img](https://blog.kakaocdn.net/dn/pXMWg/btrr9gWqTv8/5o9aQOak390qh5fyBleF81/img.png)

Quality Values(q)값 사용

0~1 , 클수로 높은 우선 순위

생략하면 1

Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
\1. ko-KR;q=1 (q생략)
\2. ko;q=0.9
\3. en-US;q=0.8
\4. en:q=0.7

협상과 우선순위를 적용하면

![img](https://blog.kakaocdn.net/dn/csuibE/btrsfSGZs1g/tIdrDcEK8aE9abgg9WrnrK/img.png)

위와 같은 상황에서 영어가 나오게 됩니다.!

## 

## **일반정보**

- From: 유저에이전트의이메일정보
- Referer: 이전웹페이지주소
- User-Agent: 유저에이전트애플리케이션정보
- Server: 요청을처리하는오리진서버의소프트웨어정보
- Date: 메시지가생성된날짜

### **Referer**

현재 요청된 페이지의 이전 웹페이지 주소

A-> B 로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청

Referer를 사용해서 유입 경로 분석 가능!

이전페이지를 뜻함.!

## **User-Agent(유저 에이전트 애플리케이션 정보)**

클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)

통계 정보

어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능

## **Server (요청을 처리하는 ORIGIN 서버의 소프트웨어 정보)**

Server : Apache/2.2.22(Debian)

server:nginx

## **특별한 정보**

- Host: 요청한호스트정보(도메인)
- Location: 페이지리다이렉션
- Allow: 허용가능한 HTTP 메서드
- Retry-After: 유저에이전트가다음요청을하기까지기다려야하는시간

## **Host(요청한 호스트 정보)**

- 진짜 중요한 필수.!!헤더
- 요청에서 사용
- 필수 하나의 서버가 여러 도메인을 처리해야할 때
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

![img](https://blog.kakaocdn.net/dn/bV2opX/btrr3mQU8Le/lr8syeBtzYcExsmL5kEAD0/img.png)

이러한 상황일 때 호스트를 구별하기 위한 용도로 쓰입니다.

## **쿠키**

Set-Cookie:서버에서 클라이언트로 쿠키 전달(응답)
Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달.

쿠키의 예시)

HTTP는 무상태 프로토콜이기 때문에 서버가 요청과 응답을 주고 받으면 연결이 끊어지기 때문에 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못합니다..(클라이언트와 서버는 서로 상태를 유지하지 않는다.)

그래서 로그인을 한 사용자인지 안한 사용자 인지 구분하기가 힘듭니다..

이걸 해결하기 위해 쿠키가 등장합니다.!

**로그인**

![img](https://blog.kakaocdn.net/dn/UGh8v/btrr5THt08Y/IlB9nbcBucUYYTvxIu60SK/img.png)

**로그인 이후 welcome 페이지 접근**

![img](https://blog.kakaocdn.net/dn/bPZ4Pz/btrsaS8RftD/hjIRDXeMAHvWXV4JsLuYpK/img.png)

웹브라우저는 요청을 보낼 때마다 쿠키를 무조건 검사하게 됩니다. 쿠키: user=홍길동을 만들어서 보냅니다.

그리하여 로그인한 유저인지 확인할 수 있습니다.

**쿠키(모든 요청에 쿠키 정보 자동 포함)**

![img](https://blog.kakaocdn.net/dn/oEN3m/btrr9fXA7qO/ns4hgg27C1m9jcLjnKQxs0/img.png)

## **쿠키**

- 예) set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-020 00:00:00 GMT; path=/; domain=.google.com;

- Secure

- 사용처

  ​

  - 사용자로그인세션관리
  - 광고정보트래킹

- 쿠키정보는항상서버에전송됨

  ​

  - 네트워크트래픽추가유발
  - 최소한의정보만사용(세션 id, 인증토큰)
  - 서버에전송하지않고, 웹브라우저내부에데이터를저장하고싶으면웹스토리지 (localStorage, sessionStorage) 참고

- 주의!

  ​

  - 보안에민감한데이터는저장하면안됨(주민번호, 신용카드번호등등)

## 쿠키- 생명주기

**Expire, max-age**

- Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT
- 만료일이되면쿠키삭제
- Set-Cookie: max-age=3600 (3600초)
- 0이나음수를지정하면쿠키삭제
- 세션쿠키: 만료날짜를생략하면브라우저종료시까지만유지
- 영속쿠키: 만료날짜를입력하면해당날짜까지유지