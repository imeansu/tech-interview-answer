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
