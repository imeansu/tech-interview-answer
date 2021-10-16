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
