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

## JWT(Json Web Token)

- JWT 해보면서 이해하기: [https://jwt.io/](https://jwt.io/)

    ## **JWT 토큰 구성**

    Source : Cookies vs. Tokens: The Definitive Guide –https://dzone.com/articles/cookies-vs-tokens-the-definitive-guide

    ![https://i2.wp.com/www.opennaru.com/wp-content/uploads/2018/08/JWT_Stacks.png?zoom=2&fit=1200%2C300](https://i2.wp.com/www.opennaru.com/wp-content/uploads/2018/08/JWT_Stacks.png?zoom=2&fit=1200%2C300)

    JWT는 세 파트로 나누어지며, 각 파트는 점로 구분하여 xxxxx.yyyyy.zzzzz 이런식으로 표현됩니다. 순서대로 헤더 (Header), 페이로드 (Payload), 서명 (Sinature)로 구성합니다. Base64 인코딩의 경우 “+”, “/”, “=”이 포함되지만 JWT는 URI에서 파라미터로 사용할 수 있도록 URL-Safe 한  Base64url 인코딩을 사용합니다.

    Header는 토큰의 타입과 해시 암호화 알고리즘으로 구성되어 있습니다. 첫째는 토큰의 유형 (JWT)을 나타내고, 두 번째는 HMAC, SHA256 또는 RSA와 같은 해시 알고리즘을 나타내는 부분입니다.Payload는 토큰에 담을 클레임(claim) 정보를 포함하고 있습니다. Payload 에 담는 정보의 한 ‘조각’ 을 클레임이라고 부르고, 이는 name / value 의 한 쌍으로 이뤄져있습니다. 토큰에는 여러개의 클레임 들을 넣을 수 있습니다.클레임의 정보는 등록된 (registered) 클레임, 공개 (public) 클레임, 비공개 (private) 클레임으로 세 종류가 있습니다.마지막으로 Signature는 secret key를 포함하여 암호화되어 있습니다.

- 세션: 서버에서 기억하고 꺼내오고 처리해야함 - stateful

    stateful 이라는 것은 다 기억하고 있다는 것이고 제어가 가능하다는 것!

    → 브라우저로 로그인, 폰으로 로그인 하면 하나는 끊어버리는 등의 관리가 가능

    서비스가 커지거나, 로드밸런싱 등을 하면 인증 서버 따로 구성하고, 공용 캐시(redis)로 세션 저장

    [https://hyuntaeknote.tistory.com/6](https://hyuntaeknote.tistory.com/6)

- JWT: 서버에서 기억할 필요가 없음 - stateless

    토큰 자체가 정보를 가지고 있음 → CPU만 이걸 처리하고 따로 IO가 필요없음

    - JWT in MSA

        ### JWT in MSA

        ### 1. Access Token in MSA

        일반적으로 권한에 따른 접근 제어가 필요한 웹 서비스는 먼저 로그인을 통해 사용자 인증(Authentication)을 진행한다. 권한 서비스(Authorizatioin Service)는 인증을 통과한 클라이언트에게 액세스 토큰(Access Token)을 발급한다. 보통 액세스 토큰은 권한을 가리키는 임의의 문자열로 구성되어 있는데, 권한을 참조한다는 의미에서 참조 토큰(By Reference Token)이라 부른다. 모놀리스(Monolith) 아키텍처에서는 참조 토큰을 액세스 토큰으로 사용해도 큰 문제가 없다. 하지만 수많은 서비스 간 API 호출이 발생하는 MSA(Micro Service Architecture)나 클라우드 환경에서는 액세스 토큰이 가리키는 권한을 확인하기 위해 모든 서비스들이 권한 서비스와 통신을 해야 한다. 서비스가 늘어날수록 권한 서버가 받는 부하는 기하급수적으로 늘어날 수 있다. 이는 MSA의 확장성(Scalability)에 부담을 줄 수 있다.

        ![https://image.toast.com/aaaadh/real/2020/techblog/5%2812%29.png](https://image.toast.com/aaaadh/real/2020/techblog/5%2812%29.png)

        ### 2. JWT as Access Token in MSA

        참조 토큰 대신 JWT를 액세스 토큰으로 사용할 수 있다. JWT는 자체적으로 필요한 정보를 모두 담을 수 있기 때문에 값 토큰(By Value Token)이라 한다. JWT 액세스 토큰은 MSA(Micro Service Architecture) 환경의 인증과 접근 제어에 적합하다. 서비스는 JWT에 포함된 값을 기준으로 권한을 확인할 수 있다. 서비스와 권한 서비스의 통신은 JWT 서명을 인증하기 위한 공개 키를 조회하는 게 전부다. 하지만 JWT를 액세서 토큰으로 사용하면 장점만 있는 것은 아니다. 단점으로는 사용자에 대한 권한이나 정보가 변경되는 경우 JWT를 새로 발급해야 하며, 경우에 따라 JWT의 크기가 커질 수 있다. 그리고 JWT의 헤더나 페이로드는 디코딩(Decoding)하면 바로 내용을 확인할 수 있기 때문에 JWT의 모든 값들은 클라이언트에게 공개된다. 외부에 노출되어서는 안되거나 민감한 값이 노출될 수 있어 보안 문제로 이어질 수 있는 담점이 있다.

        ![https://image.toast.com/aaaadh/real/2020/techblog/6%2811%29.png](https://image.toast.com/aaaadh/real/2020/techblog/6%2811%29.png)

        ### 3. API Gateway between Access Token and JWT in MSA

        클라이언트, 권한 서비스, 서비스 사이에 API Gateway를 위치시키면 JWT를 클라이언트에게 숨기면서 서비스간 통신 시 사용할 수 있다. API Gateway는 클라이언트에게 받은 액세스 토큰을 권한 서비스를 통해 JWT로 받아 액세스 토큰 대신 서비스로 넘겨준다.

        ![https://image.toast.com/aaaadh/real/2020/techblog/7%2810%29.png](https://image.toast.com/aaaadh/real/2020/techblog/7%2810%29.png)

        # 결론

        JWT의 넓은 범용성, 무결성 보장, 필요한 값을 자체 포함할 수 있는 성질 때문에 많은 곳에서 JWT를 사용하고 있고, 앞으로 더 많은 곳에서 사용할 수 있을 것이다. 특히 MSA에서 서비스 간 통신 시 권한 서비스와의 의존성을 줄일 수 있어 서버와 서버 간 통신에 매우 유용하다. MSA 환경에서 권한 인가의 한 가지 방법으로 JWT를 사용한다면 더 MSA에 어울리고 더 클라우드 네이티브(Cloud Native)한 서비스를 만들 수 있을 것이라고 생각한다.

        [JWT를 소개합니다. : NHN Cloud Meetup](https://meetup.toast.com/posts/239)

    통제할 수 없음, 토큰이 유효한 기간 동안은 사용자가 탈퇴를 하거나 해도 어찌할 수 없음

    탈취 당해도 어찌할 수 없음

    만약, 이를 관리하기 위해 JWT를 저장하게 된다면 결국 세션과 같아짐

- 그래서 나온 것이 refresh token (서버측에서 제어하기 위해서)

    access는 수명을 짧게, refresh는 db에 저장해서 

    access가 수명이 끝나면 다시 로그인하지 않고 refresh로 재발급을 진행

    다만,

    1. 짧은 수명으로 <보안이 제대로 되어 있지 않은 서버, JS SDK client on a non https site that puts the AT in a cookie > 에서 탈취 당해도 공격, 위험 시간을 단축

        인가는 access로 진행하도록 하면 중간에 탈취당해도 오래 쓰지 못함

    2. 세션처럼 관리할 수 없지만, refresh 토큰을 삭제하면 다시 로그인을 해야하므로 서버측에서 관리할 수 있는 방안을 어느정도 마련

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7535ab7f-3583-48ab-8cfc-39fcc0e3d924/Untitled.png)

    - refresh token의 목적 및 필요성에 대해서는 의견이 분분

        [Why Does OAuth v2 Have Both Access and Refresh Tokens?](https://stackoverflow.com/questions/3487991/why-does-oauth-v2-have-both-access-and-refresh-tokens)

- JWT 정리

    장점

    - 수평 스케일 아웃이 용이
    - 세션 관리, IO부담을 줄일 수 있음(CPU만 사용)
    - 내장된 만료
    - 토큰 자체가 정보를 가지고 있음
        - 정보 교류
        - 서명이 되어 있어 정보 조작 검증 가능

    단점

    - 클라이언트에 저장되어 서버측에서 사용자 정보를 조작해도 토큰에 적용할 수 없다
    - 토큰 자체 정보를 가지고 있음 → 네트워크 데이터 트래픽 증가
- waggle-waggle JWT
    1. 사용자가 firebase accesstoken을 주면 서버에서 다시 한번 확인하고 그 내용을 바탕으로 이름, 이메일, openID 를 만들어 저장
    2. 이 accessToken은 우리가 핸들링하지 못함
    3. 우리 자체  access Token & refresh Token 발급 
    4. 요청이 들어올 때마다 token 유효성 확인
        - 코드

            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1426b2fc-8d27-43b4-939c-102b31150487/Untitled.png)

    5. 멤버 프로필 수정은 토큰 주인 자신의 것만 가능
        - 코드

            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84bfd997-d422-4807-8c2d-dd5a6ee2f113/Untitled.png)

    6. 한계
        - Photon Webhook 요청은 헤더 값의 AppId로만 확인 후 통과...
