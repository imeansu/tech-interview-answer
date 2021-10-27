
## RESTful

로이 필딩은 HTTP의 주요 저자 중 한 사람으로 그 당시 웹(HTTP)설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍쳐로써 REST를 발표

> URI 는 정보의 자원을 표현해야 한다
자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)으로 표현한다
> 

### REST의 특징

1. Uniform Interface : URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행
2. Stateless : API 서버는 들어오는 요청만 단순 처리. 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해짐
3. Cacheable : REST는 HTTP 기존 웹표준 그대로 사용. HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현 가능
4. Self-descriptiveness : REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조
5. Client - Server 구조 : REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구눕. 서로 개발해야 할 내용이 명확해지고 의존성 줄어짐
6. 계층형 구조 : REST 서버는 다중 계층으로 구성. 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있음???

### REST 단점

- Restriction of HTTP Method: 메소드 형태가 제한 적
- Absence of Standard: 표준의 부재
- RDBMS와 어색한 관계 : RESTful 한 테이블 구조가 필요하게 되는데, 이것보다는 NoSQL쪽이 더 잘 맞는 추세
## CORS (Cross-Origin Resource Sharing)
출처
[https://velog.io/@pilyeooong/CORS란-무엇인가](https://velog.io/@pilyeooong/CORS%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
[https://1eastar.medium.com/cors-탐구-d3a17f961b25](https://1eastar.medium.com/cors-%ED%83%90%EA%B5%AC-d3a17f961b25)

> 도메인 또는 포트가 다른 서버의 자원을 요청하는 메커니즘
> 
- SOP 의 사용 여부는 브라우저가 판단
    - 서버는 허용할지 말지를 결정하는 것 뿐, 실제 판단은 브라우저가
- SOP 가 안 지켜진 request에는 브라우저가 임의로 헤더를 몇 개 추가 (origin, referer 등)
- 서버에서 response 를 넘겨 줄 때도 브라우저는 헤더를 확인 (access-control-allow-origin)

### CORS 동작의 시나리오

1. Prefilght Request
    - 예비 요청과 본 요청으로 나누어서 서버로 전송 (브라우저가 알아서 요청하기 때문에 FE 개잘자가 따로 처리해줄 필요 없음)
    - 예비 요청을 함으로써 본 요청을 보내기 전 브라우저 스스로가 요청을 보내는 것에 대한 안전성을 확인
    - 서버는 이 예비 요청에 대한 응답으로 현재 자신이 어떤 것들을 허용하고 있고, 어떤 것들을 금지하고 있는지에 대한 정보를 응답 헤더에 담아서 보내준다
    - OPTIONS 메서드 사용
    - request 헤더 : Origin, Access-Control-Request-methods(or Headers)
    - response 헤더: Access-Control-Allow-(Origin or Methods or Headers), max-age 등등
2. Simple Request
    - 서버에게 바로 본 요청을 전송
    - 이 후 서버가 응답 헤더에 Access-Control-Allow-Origin 과 같은 값을 보내주면 그때 브라우저가 CORS 정책 위반 여부를 검사
3. Credentialed Request
    - 다른 출처 간 통신의 보안을 좀 더 강화하고자 할 때 사용
    - 쿠키와 같은 인증정보를 포함시킴
        - 다른 출처 사이트로의 요청에 쿠키, 인증 정보를 포함시키고자 하면 → credentials: 'include' 옵션 추가
## 웹서버 와 WAS(Web Application Server)
출처
[https://codechasseur.tistory.com/25](https://codechasseur.tistory.com/25)
[https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

### 웹서버

> 웹 브라우저 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램
> 
- 정적 컨텐츠를 제공하는 서버
    - 정적 컨텐츠 : 단순 HTML, CSS, javascript, 이미지, 파일 등 즉시 응답 간으한 컨텐츠
- 동적 컨텐츠를 요청 받으면 WAS에게 해당 요청을 넘겨주고, WAS에서 처리한 결과를 클라이언트에게 전달해주는 역할도 함
- 웹서버 예 : Apache server, Nginx 등

### WAS

> HTTP 를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어로서, 주로 동적 서버 컨텐츠를 수행하는 것으로 웹 서버와 구별되며, 대개 데이터베이스 서버와 같이 수행
> 
- WAS = Web Server + Web Container
- Container 란 JSP, Servlet 을 실행시킬 수 있는 소프트웨어. 즉, WAS 는 JSP, Servlet 구동 환경 제공
- 현재 WAS 가 가지고 있는 Web Server 도 정적인 컨텐츠를 처리하는 데 있어서 성능상 큰 차이가 없다
- 주요 기능
    - 프로그램 실행 환경과 DB 접속 기능 제공
    - 여러 개의 트랜잭션 관리 기능
    - 업무를 처리하는 비즈니스 로직 수행
- WAS 예 : Tomcat, JBoss 등

### 웹 서버를 쓰는 이유

- 기능을 분리하여 서버 부하 방지
    - 단순 정적 컨텐츠는 웹 서버, 앞단에서 빠르게 제공
- 물리적으로 분리하여 보안 강화
    - SSL 에 대한 암복호화 처리를 웹 서버에서 수행
- 여러 대의 WAS 를 연결 가능
    - Load Balancing 을 위해서 Web Server 사용
    - fail over, fail back 처리에 유리
    - 무중단 운영 및 장애 극복을 쉽게
- 여러 웹 애플리케이션 서비스 가능
    - 하나는 PHP, 하나는 JAVA 등
- 기타
    - 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server 에서 처리하면 효율적
