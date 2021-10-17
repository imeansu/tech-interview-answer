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

## Blocking-NonBlocking-Synchronous-Asynchronous
출처
[http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/](http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)

### Blocking/NonBlocking

> 호출되는 함수가 바로 리턴하느냐 마느냐가 관심사이다
> 

Blocking

- 호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 함수에게 제어권을 넘겨주지 않고 대기하게 만든다

NonBlocking

- 호출된 함수가 바로 리턴해서 호출한 함수에게 제어권을 넘겨주고, 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있다

### Synchronous/Asynchronous

> 호출되는 함수의 작업 완료 여부를 누가 신경 쓰냐 가 관심사이다
> 

Synchronous

- 호출하는 함수가 호출되는 함수의 작업 완료 후 리턴을 기다리거나, 또는 호출되는 함수로 부터 바로 리턴을 받더라도 작업 완료 여부를 호출하는 함수 스스로 계속 확인하며 신경쓴다

Asynchronous

- 호출되는 함수에게 callback을 전달해서, 호출되는 함수의 작업이 완료되면 호출되는 함수가 전달받은 callback을 실행하고, 호출하는 함수는 작업 완료 여부를 신경쓰지 않는다

### NonBlocking-Sync

- 호출되는 함수는 바로 리턴하고, 호출하는 함수는 작업 완료 여부를 신경쓰는 것
- 신경쓰는 방법이 기다리거나 물어보거나 두 가지가 있는데, NonBlocking 함수를 호출했다면 사실 기다릴 필요는 없고 물어보는 일이 남는다
- NonBlocking 메서드 호출 후 바로 반환 받아서 다른 작업을 할 수 있게 되지만, 메서드 호출에 의해 수행되는 작업이 완료된 것은 아니며, 호출하는 메서드가 호출되는 메서드 쪽에 작업 완료 여부를 계속 문의

### Blocking-Async

- 별로 이점이 없어서 일부러 이 방식을 사용할 필요가 없음 (Blocking-Sync와 성능적 차이가 없지 않나...)
- 의도하지 않게 Blocking-Async로 동작하는 경우가 있다. (원래는 N-A를 추구하다가 의도와는 다르게)
    - 대표적인 케이스 - Node.js와 MySQL 조합
    - Node.js 쪽에서 callback 지옥을 헤치면서 Async로 전진해와도, 결국 DB 작업 호출 시에는 MySQL에서 제공하는 드라이버를 호출하게 되는데, 이 드라이버가 Blocking 방식

### I/O 멀티플렉싱 (I/O Multiplexing)

출처: [https://brunch.co.kr/@myner/41](https://brunch.co.kr/@myner/41)

- 입출력 다중화란 하나의 프로세스 혹은 스레드에서 입력과 출력을 모두 다룰 수 있는 기술
- 각 클라이언트 마다 별도의 프로세스나 스레드를 생성하는 것이 아닌 하나의 스레드에서 다수의 클라이언트에 연결된 소켓(파일 디스크립)을 관리하고 소켓에 이벤트가 발생할 경우에만 별도의 스레드를 만들어 해당 이벤트를 처리하도록 구현
- 입출력 함수는 여전히 블록킹으로 작동하겠지만, 입출력 함수를 호출하기 전에 어떤 파일에서 입출력이 준비가 되었는지 확인할 수 있다
