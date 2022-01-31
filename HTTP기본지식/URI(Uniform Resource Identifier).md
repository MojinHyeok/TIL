## **URI(Uniform Resource Identifier)**

URI? URL?

URI는 로케이터(locator), 이름(name) 또는 둘다 추가로 분류될 수 있다.

![img](https://blog.kakaocdn.net/dn/cEPUJZ/btrr2WxgkhC/egWFUW7teuPARJYOUNak4K/img.png)

![img](https://blog.kakaocdn.net/dn/l6cgI/btrr9ztWbd0/TJAjUT6ZPVBHnCL7KCQa81/img.png)

리소스를 식별한다. 자원을 식별하는 방법. 대부분 URL을 사용

URI 단어 뜻

- Uniform: 리소스식별하는통일된방식
- Resource: 자원, URI로식별할수있는모든것(제한없음)
- Identiﬁer: 다른항목과구분하는데필요한정보

URL분석

<https://www.google/com/search?q=hello&hl=kp> 

[https://www.google.com:443/search?q=hello&hl=ko](https://www.google.com/search?q=hello&hl=ko)[﻿](https://www.google.com/search?q=hello&hl=ko)

scheme://[userinfo@]host[:port][/path][?query][#fragment]

프로토콜 (https), 호스트명(www.google.com) , 포트번호 (443),패스(/search),쿼리 파라미터(q=hello&hl=ko)

## **URL**

## **scheme**

주로 프로토콜 사용

프로토콜 : 어떤 방식으로 자원에 접근할 것인가를 하는 약속 규칙 

예) http,https, ftp 등등

http는 80포트 https는 443포트를 주로 사용, 포트는 생략 가능

https는 http에 보안 추가된 기능Secure를 붙인것이다.

### **userinfo **

URL에 사용자정볼르 포함해서 인증

거의 사용하지 않음 위의 예시에서도 안나옴.

## **host**

호스트명

도메인명 또는 ip주소를 직접 사용가능

## **포트(port)**

접소 포트 

일반적으로 생략, 생략시http는80 https는 443

## **path**

리소스 경로(path), 계층적 구조

예시) /members/100

## **query**

key= value형태

?로 시작,&로 추가 가능 ?keyA=valueA&keyB=valueB

웹서버에 변수를 정보를 제공하는 느낌

## **fragment**

서버에 전송하는 정보는 아님

html 내부 북마크 등에 사용