### 스프링 AOP (Aspect Oriented Programming)

AOP: 관점 지향 프로그래밍
로직을 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화

- 흩어진 관심사 (Crosscutting Concerns)
    
    소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 흩어진 관심사라고 부른다
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e175d5c-020e-48c6-9e31-214fb2723047/Untitled.png)
    
    Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지
    
- AOP의 장점
    1. 어플리케이션 전체에 흩어진 공통 기능이 하나의 장소에서 관리된다는 점
    2. 다른 서비스 모듈들이 본인의 목적에만 충실하고 그외 사항들은 신경쓰지 않아도 된다는 점
- AOP 용어
    1. Target
        - 부가기능을 부여할 대상
    2. Aspect
        - 부가기능 모듈, 핵심기능에 부가되어 의미를 갖는 특별한 모듈
        - 객체지향 모듈을 오브젝트라 부르는 것과 비슷
    3. Advice
        - 실질적으로 부가기능을 담은 구현체
        - Aspect가 무엇을 언제할지를 정의
    4. JoinPoint
        - 어드바이스가 적용될 수 있는 위치, 끼어들 수 있는 지점
        - Spring에서는 메소드 조인 포인트만 제공
    5. PointCut
        - 부가기능이 적용될 대상(메소드)를 선정하는 방법
        - JoinPoint의 상세한 스펙을 정의한 것
        - 'A란 메서드의 진입 시점에 호출할 것' 과 같은 것
- 스프링 AOP의 특징
    1. 프록시 패턴 기반의 AOP 구현체
        - 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위함
        - 타겟을 감싸서 타겟의 요청을 대신 받아주는 랩핑(Wrapping) 오브젝트
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03b04fec-91dc-4684-b335-6ed2dfaf7201/Untitled.png)
    
- 실행 시점
    - @Before (이전) : 어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행
    - @After (이후) : 타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타겟 메소드가 완료 되면 어드바이스 기능을 수행
    - @AfterReturning (정상적 반환 이후)타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
    - @AfterThrowing (예외 발생 이후) : 타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
    - @Around (메소드 실행 전후) : 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출전과 후에 어드바이스 기능을 수행
- Filter, Interceptor, AOP
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7f2b7765-1636-4c94-8dc5-98499a1f2dd7/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/153736bb-d4c2-429e-8b87-feb42dbea884/Untitled.png)
        
    1. Filter
        - 요청과 응답을 거른 뒤 정제하는 역할
        - 전체적인 Reqeust단에서 어떤 처리가 필요할때
            - 인증, 데이터압축, 암호화 필터 등
            - 인코딩 변환 XSS 방어 등
        - 필터는 스프링 컨텍스트 외부에 존재하여 스프링과 무관한 자원에 대해 동장
        - DispatcherServlet 이전에 실행
        - 자원의 처리가 끝난 후 응답 내용에 대해서도 변경하는 처리 가능
    2. Interceptor
        - 요청에 대한 작업 전/후로 가로챈다
        - 스프링 컨텍스트 내부에서 진행되므로 모든 빈 객체에 접근할 수 있다
        - 세션 및 쿠키 체크하는 http 프로토콜 단위로 처리해야하는 업무
            - 로그인 체크, 권한 체크
    3. AOP
        - OOP를 보완하기 위해 나온 개념
        - Interceptor나 Filter와는 달리 메소드 전후의 지점에 자유롭게 설정 가능
        - Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터의 차이
            - Advice는 JoinPoin나 ProceedingJoinPoint 등을 활용
            - 반면, Interceptor는 Filter와 유사하게 HttpServletReqeust 등을 파라미터로 사용
        - 비스니스 단에서 세밀하게 조정하고 싶을 때
            - 로깅, 트랜잭션, 에러처리 등

## Spring @Transactional
출처
[https://goddaehee.tistory.com/167](https://goddaehee.tistory.com/167)
[https://mommoo.tistory.com/92](https://mommoo.tistory.com/92)
[https://velog.io/@kdhyo/JavaTransactional-Annotation-알고-쓰자-26her30h](https://velog.io/@kdhyo/JavaTransactional-Annotation-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90-26her30h)
[https://tecoble.techcourse.co.kr/post/2021-05-25-transactional/](https://tecoble.techcourse.co.kr/post/2021-05-25-transactional/)
[https://mangkyu.tistory.com/154](https://mangkyu.tistory.com/154)

> Spring 에서는 트랜잭션 처리를 지원하는데 그 중 어노테이션 방식으로 @Transactional 을 선언하여 사용하는 방법이 일반적이며, 선언적 트랜잭션이라 부른다
> 
- 트랜잭션을 담당하는 기술 코드를 완전히 분리시키는 것을 원함
- 해당 로직을 클래스 밖으로 빼내서 별도의 모듈로 만드는 AOP를 고안 및 적용하게 되었고, 이를 적용한 트랜잭션 어노테이션을 지원하게 됨

### Spring @Transactional 기능 제공 방식

- JPA 의 객체 변경 감지는 transaction이 commit 될 때, 작동 → Spring 은 @Transactional 어노테이션을 선언한 메서드가 실행되기 전, transaction begin 코드를 삽입하여 메서드가 실행된 후, transaction commit 코드를 삽입하여, 객체 변경감지를 수행하게 유도
- Spring 의 코드 삽입 방법은 크게 2가지
    1. 바이트 코드 생성 (CGLIB 사용) - SpringBoot 기본 방식 → 굳이 interface 필요 없음
    2. 프록시 객체 사용 → Spring 기본 방식 → interface 반드시 필요
- 클래스, 메서드 위에 @Transactional 이 추가되면, 이 클래스에 트랜잭션 기능이 적용된 프록시 객체가 생성된다
- 이 프록시 객체는 @Transactional 이 포함된 메소드가 호출 될 경우, PlatformTransactionManager 를 사용하여 트랜잭션을 시작하고, 정상 여부에 따라 Commit 또는 Rollback 한다

### @Transactional 옵션

1. isolation
    
    트랜잭션에서 일관성 없는 데이터 허용 수준, 격리 수준을 설정
    
    - READ_UNCOMMITED, READ_COMMITED, REPEATEABLE_READ, SERIALIZABLE
2. propagation
    
    트랜잭션 동작 도중 다른 트랜잭션을 호출할 때, 전파 옵션
    
    - REQUIRED (Default) : 이미 진행중인 트랜잭션이 있다면 해당 트랜잭션 속성을 따로고, 진행 중이 아니라면 새로운 트랜잭션을 생성한다
    - REQUIRES_NEW : 항상 새로운 트랜잭션을 생성한다. 이미 진행중인 트랜잭션이 있다면 잠깐 보류하고 해당 트랜잭션 작업을 먼저 진행한다
    - SUPPORT : 이미 진행 중인 트랜잭션이 있다면 해당 트랜잭션 속성을 따르고, 없다면 트랜잭션을 설정하지 않는다
    - NOT_SUPPORT : 이미 진행중인 트랜잭션이 있다면 보류하고, 트랜잭션 없이 작업을 수행한다
    - MANDATORY : 이미 진행중인 트랜잭션이 있어야만, 작업을 수행한다. 없다면 Exception 을 발생시킨다
    - NEVER : 트랜잭션이 진행중이지 않을 때 작업을 수행한다. 트랜잭션이 있다면 Exception 을 발생시킨다
    - NESTED : 진행 중인 트랜잭션이 있다면 중첩된 트랜잭션이 실행되며, 존재하지 않으면 REQUIRED 와 동일하게 실행
3. noRollbackFor
    
    특정 예외 발생 시 rollback 하지 않는다 / @Transactional(noRollbackFor = Exception.class)
    
4. rollbackFor
    
    특정 예외 발생 시 rollback 한다 / @Transactional(rollbackFor = Exception.class)
    
    @Transactional 은 기본적으로 Unchecked Exception, Error 만을 rollback 함. 모든 예외에 대해서 rollback을 진행하고 싶ㅇ르 경우 위와 같이 설정해야 함
    
5. timeout
    
    지정한 시간 내에 메소드 수행이 완료되지 않으면 rollback 한다 (-1일 경우 timeout 을 사용하지 않는다)
    
6. readOnly
    
    트랜잭션을 읽기 전용으로 설정한다 (Default = false)
    
    true 시 insert, update, delete 실행 시 예외 발생
    
    aop/tx 스키마로 트랜잭션 선언을 할 때는 이름 패턴을 이용해 읽기 전용 속성으로 만드는 경우가 많다. 보통 get이나 find 같은 이름의 메소드를 모두 읽기전용으로 만들어서 사용하면 편리하다
    

개인적인 궁금증 해결
Auto Increment 옵션은 트랜잭션의 범위 밖에서 동작한다
따라서 테스트 환경에서 @Transactional 로 롤백이 되어도 id 는 감소하지 않는다
이유는 동시성 때문

## Spring boot Test
공식 문서: [https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing)
출처 
[https://goddaehee.tistory.com/211?category=367461](https://goddaehee.tistory.com/211?category=367461)

1. @SpringBootTest - 통합 테스트, 전체 - Bean 전체
2. @WebMvcTest - 단위 테스트, MVC 테스트 - MVC 관련된 Bean
3. @DataJpaTest - 단위 테스트, Jpa 테스트 - JPA 관련 Bean
4. @RestClientTest - 단위 테스트, Rest API 테스트 - 일부 Bean
5. @JsonTest - 단위 테스트, Json 테스트 - 일부 Bean

### @SpringBootTest

- 실제 운영 환경에서 사용될 클래스들을 통합하여 테스트
- 단위 테스트와 같이 기능 검증을 위한 것이 아니라 Spring framework 에서 전체적으로 플로우가 제대로 동작하는지 검증하기 위해 사용
- 장점
    - 애플리케이션의 설정, 모든 Bean 을 모두 로드하기 때문에 운영환경과 가장 유사한 테스트가 가능
    - 전체적인 Flow 를 쉽게 테스트 가능
- 단점
    - 애플리케이션의 설정, 모든 Bean 을 모두 로드하기 때문에 시간이 오래 걸리고 무겁다
    - 테스트 단위가 크기 때문에 디버깅이 어려운 편
1. 설정
    - properties
        - 프로퍼티를 {key=value} 형식으로 직접 추가할 수 있다
        - 기본적으로 클래스 경로 상의 application.yml 등(.properties) 를 통해 설정을 수행하지만, 테스트를 위한 다른 설정이 필요하다면 다른 프로퍼티를 로드할 수 있다
    - webEnvironment
        - Mock
            - WebApplicationContext 를 로드하여 내장된 서블릿 컨테이너가 아닌 Mock 서블릿을 제공
            - 기본값은 Mock 서블릿을 로드하여 구동
            - @AutoconfigureMockMvc 와 함께 사용하면 별다른 설정 없이 간편하게 MockMvc 를 사용한 테스트를 진행할 수 있다 (MockMvc 에 대한 추가 학습!!)
        - RANDOM_PORT
            - EmbeddedWebApplicationContext 를 로드하여 실제 서블릿 환경을 구성
            - 임의의 port listen
        - DEFINED_PORT
            - RANDOM_PORT와 동일하게 실제 서블릿 환경을 구성
            - 포트는 프로퍼티에서 지정한 포트를 listen
        - NONE
            - 기본적인 ApplicationContext 를 로드
        - TestTestTemplate
            - 추가 학습!!!
2. @MockBean
    - Mock 객체를 빈으로 등록할 수 있음
    - ApplicationContext에 Mock 객체를 빈으로 등록
    - @MockBean 으로 선언된 객체와 같은 이름과 타입으로 이미 빈이 등록되어 있다면 해당 빈은 Mock 객체로 대체됨
3. @Transactional
    - 테스트 완료 후 자동으로 rollback 처리
    - 하지만 WebEnvironment.RANDOM_PORT, DEFINED_PORT 를 사용하면 실제 테스트 서버는 별도의 스레드에서 테스트를 수행하기 때문에 트랜잭션이 롤백되지 않는다
        - [https://stackoverflow.com/questions/64591281/transactional-annotation-in-spring-test](https://stackoverflow.com/questions/64591281/transactional-annotation-in-spring-test)
        
        <aside>
        💡 On the contrary, if you launched a local instance of Spring using `@SpringBootTest(webEnvironment = WebEnvironment.DEFINED_PORT)` for instance), it is detached from your unit test environment, hence:
        
        </aside>
        
4. @ActiveProfiles
    - 프로파일 전략을 사용 중이라면 원하는 프로파일 환경값 설정이 가능

### @WebMvcTest

- MVC를 위한 테스트, 컨트롤러가 예상대로 동작하는 지 테스트하는데 사용된다
- 다음 내용만 스탬하도록 제한 (보다 가벼운 테스팅이 가능)
    - @Controller, @ControllerAdvice, @JsonComponent, Converter, GenericConverter, Filter, HandlerInterceptor
- MockBean, MockMVC 를 자동 구성하여 테스트 가능하도록 한다
- Spring Security 의 테스트도 지원
- @WebMvcTest 를 사용하기 위해 테스트할 특정 컨트롤러 클래스를 명시하도록 한다

**장점**

1. WebApplication 관련된 Bean 들만 등록하기 때문에 통합 테스트보다 빠르다
2. 통합 테스트를 진행하기 어려운 테스트를 진행 가능하다

ex) 결제 모듈 API 를 콜하면 안되는 상황에서 Mock 을 통해 가짜 객체를 만들어 테스트 가능

**단점**

1. 요청부터 응답까지 모든 테스트를 Mock 기반으로 테스트하기 때문에 실제 환경에서는 제대로 동작하지 않을 수 있다

### @DataJpaTest

- JPA 관련된 설정만 로드
- 설정이 정상적인지, JPA를 사용해서 데이터를 올바르게 조회, 생성, 수정, 삭제 하는지 등의 테스트가 가능하다
- @Entiry 클래스를 스캔하여 스프링 데이터 JPA 저장소를 구성한다 (다른 컴포넌트를 스캔하지 않음)
- @Transactional 어노테이션을 포함하고 있기 때문에 따로 선언하지 않아도 됨
- 트랜잭션 기능이 필요하지 않으면 @Transactional(propagation = Propagation.NOT_SUPPORTED)
- 기본적으로 in-memory embedded database 에 대한 테스트를 진행
- real database 를 사용하고자 하는 경우 @AutoConfigureTestDatabase 사용
- @AutoConfigureTestDatabase Default 설정 값은 Any 이다 (기본적으로 내장된 데이터소스를 사용)
- @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE) 을 지정하면 실제 DB도 사용 가능하며, @ActiveProfiles("test") 등의 프로파일 설정도 가능

### @RestClientTest

- REST 클라이언트 테스트가 가능
- REST 통신의 JSON 형식이 예상대로 응답을 반환하는지 등 테스트
- 예를 들면, Apache HttpClient 나 Spring 의 RestTemplate 을 사용하여 외부 서버에 웹 요청을 보내는 경우에 이에 응답할 Mock 서버를 만드는 것이라고 생각하면 된다
- restTemplate 을 사용하는 부분을 Mocking 해서 결과가 정상적으로 넘어올거라는 가정하에 테스트 코드를 작성할 수도 있지만 → Dto 클래스와 API 의 Json 결과 포맷에 차이가 있는 등의 이슈를 확인할 수 없음
    
    출처: [https://jojoldu.tistory.com/341](https://jojoldu.tistory.com/341)
    

### @JsonTest

- JSON serialization 과 deserialization 테스트를 편하게 할 수 있음
- JSON 의 직렬화, 역직렬화를 수행하는 라이브러리인 Gson 과 Jackson 의 테스트 제공
- (JacksonTester, GsonTest, BasicJsonTester)
