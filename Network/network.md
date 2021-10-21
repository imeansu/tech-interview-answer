## HTTP의 GET과 POST 비교

- GET

    요청하는 데이터가 HTTP Request Message의 Header부분에 url이 담겨서 전송된다. url에 ? 뒤에 데이터가 붙어서 request가 보내지게 되어 url이라는 한정된 공간에 담겨가기 때문에 전송할 수 있는 데이터의 크기가 제한적이고 그대로 노출되기 때문에 비밀번호 같은 데이터에 대해서는 부적절하다

- POST

    HTTP Request Message의 Body 부분에 데이터가 담겨서 전송된다. 이 때문에 GET 방식보다는 전송할 수 있는 데이터의 크기가 크고 보안면에서 좀 더 낫다 (하지만, 암호화하지 않는 이상 비슷하다)

- GET은 서버에서 데이터를 가져오는 용도, POST는 서버의 값이나 상태를 변경, 추가하기 위해서 사용된다
- GET 방식은 브라우저에서 Caching 할 수 있기 때문에 POST 방식으로 요청해야 할 것을 보내는 데이터가 작고 보안적인 문제가 없다는 이유로 GET방식으로 요청한다면 기존에 Caching되었던 데이터가 응답될 가능성이 존재


## HTTP와 HTTPS

- HTTP 프로토콜
    - 개념
        - HyperText Transfer Protocol
        - 웹 상에서 클라이언트와 서버가 정보를 주고 받을 수 있도록 규약
        - TCP와 UDP를 사용하며, 80번 포트를 사용한다
            - 비연결성: 클라이언트가 요청하고 서버가 응답하면 연결이 끊어진다
            - 무상태(stateless): 요청과 응답이 있고난 이후에 서버는 아무런 상태도 저장하지 않는다
    - 문제점
        - TCP/IP는 도청이 가능하다: 평문이기 때문에 패킷을 수집하는 것만으로도 도청할 수 있다
        - 요청 상대를 확인하지 않기 때문에 위장이 가능하다: 누구인지, 허가된 상대인지, 어디서 보냈는지, 의미 없는 (DoS) 공격인지 확인할 수 없다
        - 완전성을 증명할 수 없다 → 변조가 가능하다: 중간자 공격을 당한다 하더라도 알길이 없다
- HTTPS 프로토콜
    - 개념
        - HyperText Transfer Protocol over Secure Socket Layer
        - 웹 통신 프로토콜인 HTTP에 보안이 강화된 프로토콜
        - 기본 TCP/IP로 443번 포트를 사용한다
        - SSL(Secure Socket Layer) or TLS(Transport Layer Security) 등 다른 프로토콜과 조합하여 암호화 한다


## TCP UDP

TCP(Transmission Control Protocol, 전송제어 프로토콜)

- 대부분 신뢰성과 순차적인 전달을 필요로 한다
- 신뢰성이 없는 인터넷을 통해 종단간에 신뢰성 있는 바이트 스트림을 전송하도록 특별히 설계
- 송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성
- 연결 설정은 3-way handshake를 통해 이루어짐
- 모든 TCP 연결은
    - 전이중(full-duplex, 전송이 양방향으로 동시에 일어날 수 있음 )
    - 점대점(point to point, 각 연결이 정확히 2개의 종단점을 가지고 있음)

UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)

- 비연결형 프로토콜
- IP 데이터그램을 캡슐화하여 보내는 방법과 연결을 설정하지 않고 보내는 방법을 제공
- 흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 하지 않는다
모두 사용자 프로세스의 몫
- 요청 또는 응답이 손실된다면, 클라이언트는 time out되고 다시 시도할 수 있으면 됨. 코드가 간단할 뿐만 아니라 TCP처럼 초기 설정에서 요구되는 프로토콜보다 적은 메시지 요구
- DNS 서버는 UDP

## HTTP 메서드와 역할

- GET
    - 요청받은 URI의 정보를 검색하여 응답, 리소스 취득
    - 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
- HEAD
    - GET 방식과 동일하지만, 응답에 BODY가 없고 응답 코드와 HEAD만 응답
    - 웹서버 정보 확인, 헬스체크, 버젼확인, 최종 수정일자 확인 등의 용도로 사용
- POST
    - 요청된 자원을 생성(create, insert)한다 (멱등 X)
    - 요청 데이터를 HTTP Body에 담아 웹서버로 전송
    - PRG: Post/Redirect/Get
        - 일시적인 리다이렉션
        - 중복요청 방지
        - Post 후 redirect 302, Location 으로 GET
- PUT
    - 요청된 자원을 수정(update)한다
    - 내용 갱신, 자원 전체를 갱신 (멱등성)
- PATCH
    - 리소스 부분 변경
    - 자원의 일부를 교체 (멱등 X)
- DELETE
    - 요청된 자원을 삭제할 것을 요청함
    - 안정성 문제로 대부분의 서버에서 비활성
- OPTIONS
    - 웹 서버에서 지원되는 메소드의 종류를 확인할 경우

캐시

- cache-control: max-age=60(초)
- 304 Not Modified + 헤더 메타 정보만 응답(바디 X)
- 검증 헤더와 조건부 요청
    - If-Modified-Since: Last-Modified
    - If-None-Match: ETag

## 공인 IP와 사설 IP

출처

[https://brunch.co.kr/@sangjinkang/61](https://brunch.co.kr/@sangjinkang/61)

### NAT(Network Address Translation)

> 사설 IP를 공인 IP로 변경에 필요한 주소 변환 서비스
라우터 등의 장비를 사용하여 다수의 사설 IP를 하나의 공인 IP 주소로 변환하는 기술
IP 주소와 Port 번호로 구성된 NAT Forwarding Table을 보관하고 있고 이에 맞게 주소 변환
> 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e38bc18-f5f6-49c9-9720-2af24d56fcaa/Untitled.png)

### NAT는 왜 필요한가?

- 공인 IP 주소를 절약
- 보안의 목적
    - 내부망과 공개망 사이에 방화벽(firewall)을 운영하여 외부 공격으로부터 내부망을 지킴 (IP masquerading, 마스쿼레이딩)

## Cookie & Session

HTTP는 비상태성(Stateless) 프로토콜로 상태 정보를 유지하지 않는다. 연결을 유지하지 않기 때문에 리소스 낭비가 줄어드는 것은 큰 장점이지만 통신할 때마다 매번 연결 설정을 해야 하며, 이전 요청과 현재 요청이 같은 사용자의 요청인지 알 수 없다는 단점이 존재한다. 쿠키와 세션을 통해서 HTTP의 Stateless한 문제점을 해결할 수 있다.

### 저장 위치

- 쿠키 : 클라이언트의 웹 브라우저가 지정하는 메모리 or 하드디스크
- 세션 : 서버의 메모리에 저장

### 만료 시점

- 쿠키 : 저장할 때 expires 속성을 정의해 무효화 시키면 삭제될 날짜 정할 수 있음
- 세션 : 클라이언트가 로그아웃하거나, 설정 시간동안 반응이 없으면 무효화 되기 때문에 정확한 시점을 알 수 없음

### 리소스

- 쿠키 : 클라이언트에 저장되고 클라이언트의 메모리를 사용하기 때문에 서버 자원을 사용하지 않음
- 세션 : 세션은 서버에 저장되고, 서버 메모리로 로딩 되기 때문에 세션이 생길 때마다 리소스를 차지함

### 용량 제한

- 쿠키 : 클라이언트도 모르게 접속되는 사이트에 의하여 설정될 수 있기 때문에 쿠키로 인한 문제가 발생하는 걸 막고자 한 도메인 당 20개, 하나의 쿠키 당 4KB로 제한해 둠
- 세션 : 클라이언트가 접속하면 서버에 의해 생성되므로 개수나 용량 제한 없음

### Cookie

- 사용 목적
    - 세션관리 : Login, shopping cart, game score, or anything else the server should remember
    - 개인화 : User preferences, themes, and other settings
    - 트래킹 : Recording and analyzing user behavior
- Secure
    - Response Header 에 Secure=Secure; Secure 저장 (request에는 없음)
    - HTTPS를 사용해야하만 전송한다
    - sessionId만 가지고 변조가 가능하기 때문에 HTTPS만 전송 가능한 Secure 쿠키를 통해 보안성을 높일 수 있다
- HttpOnly
    - 웹브라우저에서 자바스크립트 `document.cookie` 등을 통해서 현재 쿠키 값을 확인 할 수 있다
    - HttpOnly 설정을 하면 자바스크립트를 통해서 쿠키 값을 확인할 수 없게 한다
- Path
    - Path 옵션을 지정하면 해당 디렉토리와 그 하위 디렉토리에만 웹브라우저는 웹서버에게 전송
- Domain
    - 해당 도메인에만 동작할 수 있다. Path와 비슷함

## TCP흐름/혼잡/오류 제어
출처
[https://velog.io/@mu1616/TCPIP-혼잡-제어](https://velog.io/@mu1616/TCPIP-%ED%98%BC%EC%9E%A1-%EC%A0%9C%EC%96%B4)

### TCP 통신

- reliable network를 보장할 수 있도록 하는 프로토콜
- network congestion avoidance algorithm을 사용
- reliable network를 보장한다는 것의 4가지 문제점
    1. 손실 : packet이 손실 될 수 있는 문제
    2. 순서 바뀜
    3. Congestion : 네트워크가 혼잡한 문제
    4. Overload : receiver가 overload 되는 문제

### 흐름제어 (Flow Control)

- 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
- Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절 - 손실, 그에 따른 불필요한 재전송
- 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback
- 해결 방법
    1. Stop and Wait : 매번 전송한 패킷에 대한 확인 응답을 받아야만 다음 패킷 전송
    2. Sliding Window 
        - 수신측에서 설정한 윈도우 크기 만큼 송신측에서 확인 응답이 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법
        - 동작방식 : 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는 대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송
        - Window : handshaking을 통해 수신 호스트의 receive window size에 자신의 end window size를 맞추게 된다

### 혼잡제어 (Congestion Control)

- 네트워크 내에 패킷의 수가 과도하게 증가하는 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송 속도를 강제로 줄임
- 흐름 제어는 송신측과 수신측, 혼잡제어는 호스트와 라우터를 포함한 보다 넓은 관점
- 해결방법
    1. AIMD (Additive Increase / Multiplicative Decrease)
        - 처음에 패킷을 하나씩 보내고 문제가 없으면 window 크기(단위 시간 내에 보내는 패킷의 수)를 1씩 증가시켜가며 전송
        - 전송에 실패하면 윈도우 크기를 반으로 줄인다
        - 문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸림
    2. Slow Start (느린 시작)
        - 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size를 1씩 늘려준다. 즉, 한 주기가 지나면 window size가 2배로 된다
        - AIMD에 반해 지수 함수 꼴로 증가. 대신, 혼잡 현상이 발생하면 window size를 1로 떨어뜨리게 된다
        - 혼잡 현상이 발생하였던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후 부터는 완만하게 1씩 증가
    3. Fast Retransmit (빠른 재전송) 
        - 2, 3번 패킷 다음 5번이 오면 수신측에서는 순서대로 잘 도착한 마지막 패킷의 다음 순번을 ACK 패킷에 실어서 보냄 → 수신측은 받은 5,6,7 패킷에 대해 모두 ACK3을 보낸다
        - 이런 중복 ACK을 3개 받으면 재전송
        - 송신측은 자신이 설정한 타임아웃 시간이 지나지 않았어도 바로 해당 패킷을 재전송 할 수 있기 때문에 보다 빠른 전송률 유지
        - 참고로, 송신측에서 설정한 타임아웃까지 ACK을 받지 못하면 혼잡이 발생한 것으로 판단하여 혼잡 회피
    4. Fast Recovery (빠른 회복)
        - 혼잡 상태가 되면 window size를 1로 줄이지 않고 반으로 줄이고 선형증가. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서 부터는 순수한 AIMD 방식으로 동작
- 혼잡제어 정책
    1. TCP Tahoe
    2. TCP Reno
        - TCP Tahoe와 마찬가지로 Slow Start로 시작하여 임계ㅔ점을 넘어가면 AIMD 방식으로 변경
        - TCP Tahoe와의 차이점은 바로 3 ACK Duplicated 와 타임아웃을 구분한다는 점이다. TCP Reno는 3 ACK Duplicated가 발생하면 빠른회복 방식을 사용
        - 즉, 윈도우 크기를 1로 줄이는 것이 아니라 반으로 줄이고 윈도우 크기를 선형적으로 증가시킨다. 그리고 임계점을 줄어든 윈도우 값으로 설정

## HTTP 1.1 vs 2.0
출처
[https://seokbeomkim.github.io/posts/http1-http2/](https://seokbeomkim.github.io/posts/http1-http2/)
[https://velog.io/@zzzz465/HTTP1.1-2-3-의-차이점](https://velog.io/@zzzz465/HTTP1.1-2-3-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)\

### HTTP 1.1

1. 동작 방식
    - 기본저긍로 Connection 한 개당 하나의 요청 처리, 이 때문에 동시에 여러개의 리소스를 주고 받는 것이 불가능하고 순차적으로 이루어짐
2. 단점
    - HOL(Head Of Line) Blocking - 특정 응답의 지연
        - 네트워크에서 같은 큐에 있는 패킷이 첫 번째 패킷에 의해 지연될 때 발생
        - HTTP에서의 HOL Blocking
            - piplining : 하나의 connection을 통해 여러 개의 파일을 요청/응답 , 하지만 순차적으로 처리되어 HOL Blocking 발생
        - TCP에서의 HOL Blocking
            - TCP의 경우 패킷 LOSS가 발생하면 패킷을 재전송 - TCP를 사용하면 어쩔 수 없이 발생
    - RTT(Round Trip TIme) 증가
        - 3-way Handshake가 반복 - 불필요한 RTT증가와 네트워크 지연 초래
    - 무거운 Header 구조
        - 많은 메타 정보들이 저장 - 매 요청 마다 중복된 헤더값을 전송

### HTTP 2

1. Multiplexed Streams
    - 여러 요청/응답을 병렬로 처리 (하나의 TCP 연결에 여러 스트림 사용)
    - Connection 한 개로 동시에 여러 개의 메시지를 주고 받을 수 있으며 응답은 순서에 상관없이 Stream으로 주고 받는다. HTTP/1.1 의 Connection Keep-Alive, Pipelining의 개선 버전이라고 볼 수 있음
2. Stream Prioritization
    - 리소스 간의 의존관계에 따른 우선순위를 설정하여 리소스 로드 문제 해결
3. Server Push
    - 서버는 클라이언트가 요청하지 않은 리소스를 사전에 푸쉬를 통해 전송
4. Header Compression
    - 헤더 정보를 압축하기 위해 Header Table과 Huffman Encoding 기법을 사용하여 처리하는데 이를 HPACK 압축 방식이라 부르며 별도의 명세서(RFC 7531)로 관리
    - 헤더의 중복을 검출해 내고 해당 테이블에서의 index값 + 중복되지 않은 Header 정보를 Huffman Encoding 방식으로 인코딩한 데이터 전송
