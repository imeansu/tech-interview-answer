## SOLID 원칙

- SRP(Single Responsibility Principle) 단일 책임 원칙
    - 클래스는 단 하나의 책임을 가져야 하며 클래스를 변경하는 이유는 단 하나이어야 한다
- OCP(Open-Closed Principle): 개방-폐쇄 원칙
    - 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다
    - 기존의 코드를 변경하지 않으면서(closed), 기능을 추가할 수 있도록(open) 설계 되어야 한다
- LSP(Liskov Substitution Principle): 리스코프 치환 원칙
    - 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다
    - 자식 클래스가 부모 클래스를 대체하기 위해서는 부모의 기능에 대해 오버라이드 하지 않도록 해야 함
    - 즉, 자식 클래스는 부모 클래스의 책임을 무시하거나 재정의하지 않고 확장만 수행하도록 해야 함
- ISP(Interface Segregation Principle): 인터페이스 분리 원칙
    - 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다
    - 한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다. 하나의 일반적인 인터페이스보다 여러개의 구체적인 인터페이스가 낫다
    - SRP는 객체의 단일 책임, ISP는 인터페이스의 단일 책임
- DIP(Dependency Inversion Principle): 의존 역전 원칙
    - 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안된다
    - 의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는 것에 의존
    - 구체적인 클래스보다 인터페이스나 추상 클래스와 관계를 맺어야 한다

출처: [https://victorydntmd.tistory.com/291](https://victorydntmd.tistory.com/291)

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
