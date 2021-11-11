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

## 컴파일, 빌드, 링크
출처: [https://r-son.tistory.com/6](https://r-son.tistory.com/6)

### 컴파일 (Compile)

- 개발자가 작성한 소스코드를 바이너리 코드로 변환하는 과정. (목적 파일이 생성됨)
- 컴퓨터가 이해할 수 있는 기계어로 변환하는 작업
- 자바에서는 컴파일 할 경우, JVM에서 실행가능한 바이트코드 형태의 클래스 파일이 생성
- .java 라는 자바 클래스 파일을 바탕으로 .class 라는 클래스 파일이 생성

### 빌드 (Build)

- 소스코드 파일을 실행 가능한 소프트웨어 산출물로 만드는 일련의 과정
- 빌드의 단계 중 컴파일이 포함
- 빌드 툴이 제공해주는 기능 : 전처리, 컴파일, 패키징, 테스팅, 배포
- 빌드 툴 : Ant, Maven, Gradle

### 링크 (Link)

- A라는 소스 파일에서 B 소스 파일에 존재하는 함수를 호출하는 경우, 소스 파일 각각을 컴파일만 하면 다른 곳에 존재하는 함수를 찾지 못함
- A와 B를 연결해주는 작업을 링크
- 정적 링크(static link)와 동적링크(dynamic link)
    - 정적 링크는 컴파일된 소스 파일을 연결해서 실행가능한 파일을 만드는 것
    - 동적 링크는 프로그램 실행 도중 프로그램 외부에 존재하는 코드를 찾아서 연결
    - 자바에서는 JVM이 프로그램 실행 도중 필요한 클래스를 찾아서 클래스패스에 로드해주는데 이는 동적링크의 예이다

## Docker를 사용해야 하는 이유
출처
[https://aws.amazon.com/ko/docker/](https://aws.amazon.com/ko/docker/)
[https://hanhyx.tistory.com/27](https://hanhyx.tistory.com/27)
[https://bcho.tistory.com/805](https://bcho.tistory.com/805)

### 도커(Docker) 란?

> 도커는 리눅스의 응용 프로그램들을 소프트웨어 컨테이너 안에 배치시키는 일을 자동화하는 오픈 소스 프로젝트
> 
- 다양한 프로그램, 다양한 운영체제 및 실행환경 등을 컨테이너로 추상화하여 동일한 인터페이스를 제공하여, 배포 및 관리를 단순화 시켜주는 것
- Container 자체에는 커널 등의 OS 이미지가 들어가 있지 않다
- 커널은 Host OS를 그대로 사용하되, Host OS와 컨테이너 OS의 다른 부분만 컨테이너 내에 같이 Packing 된다
- 컨테이너 내에서 명령어를 수행하면 실제로는 Host OS에서 그 명령어가 수행된다. 즉, Host OS 의 프로세스 공간을 공유한다

### 사용해야 하는 이유

***쉽고 빠른 실행 환경 구축, 가볍고 빠른 실행 속도, 하드웨어 자원 절감, 공유 환경 제공, 쉬운 배포***

1. 운영 표준화
    - 작은 컨테이너식 애플리케이션을 사용하면 손쉽게 배포하고, 문제를 파악하고, 수정을 위해 롤백할 수 있음
2. 원활하게 이전
    - Docker 기반 애플리케이션을 로컬 개발 시스템에서 프로덕션 배포로 원활하게 이전할 수 있음
3. 비용 절감
    - Docker 컨테이너를 사용하면 각 서버에서 좀 더 쉽게 더 많은 코드를 실행하여 사용률을 높이고 비용을 절감할 수 있음

### docker-compose

- 여러 개의 컨테이너가 하나의 애플리케이션으로 동작할 때, 이를 테스트 하려면 각 컨테이너를 하나씩 생성해야 한다
- 여러개의 컨테이너로 구성된 애플리케이션을 구축하기 위해서 run 명령어를 여러번 사용할 수 있지만, 테스트 단계에서는 매번 run 명령어에 옵션을 설정해서 진행하기에 번거로움
- 이를 위해 도커 컴포즈는 YAML 파일을 통해 여러 개의 컨테이너의 실행을 한 번에 관리하여 하나의 프로젝트처럼 다룰 수 있는 환경을 제공

## 컴파일, 인터프리터
출처
[https://jins-dev.tistory.com/222](https://jins-dev.tistory.com/222)
[https://velog.io/@jaeyunn_15/OS-Compiler-vs-Interpreter](https://velog.io/@jaeyunn_15/OS-Compiler-vs-Interpreter)

### 컴파일

- 프로그래밍 언어를 Runtime 이전에 기계어로 해석하는 작업 방식
- 이때 원래의 소스를 원시 코드, 바뀐 코드를 목적 코드(Object Code) 라고 한다
- 실행 시간이 빠름
- 컴파일 에러 - 디버깅 가능
- OS 및 빌드 환경에 종속적
- C / C++ , Java (Byte Code 로 바꾸는 과정)

### 인터프릿 (Interpret)

- 런타임 이후에 Row 단위로 해석(Interpret) 하며 프로그램을 구동
- 실행 시간은 느리며, 대신 런타임에 실시간 Debuggign 및 코드 수정이 가능
- Javascript, Python 등 스크립트 언어, Java byte code interpret
- 인터프리터는 해석을 위한 Virtual Machine 을 두고, 머신 위에서 Interpret 수행
- 이 때, 해석의 기반이 되는 머신 들이 OS 환경들을 지원해줌으로써, OS 및 플랫폼에 종속되지 않는 프로그램 구동 가능

## AB Test
출처
[https://brunch.co.kr/@digitalnative/19](https://brunch.co.kr/@digitalnative/19)

> 디지털 환경에서 전체 실사용자를 대상으로 대조군(Control Group)과 실험군(Experimental Group)으로 나누어서 어떤 특정한 UI나 알고리즘의 효과를 비교하는 방법론
> 

[사용자 분리 방법](https://www.notion.so/c7c8530bf2154268be5ae78650aa3236)

### AB Test 결과 신뢰

1. AA Test
    - AB Test 전에 분산된 트래픽에 대해 동일한 Variation을 동시에 보여주고 차이가 있는지 없는지 먼저 확인 후에 차이가 없다면 AB Test를 진행해서 차이가 발생하는지 확인
2. P-Value
    - 통계 분석에서 가장 널리 활용되는 유의성 검증 방식
    - 모집단을 샘플의 값을 활용하여 추정한다고 하였을 때, 샘플 수가 너무 적을 때 P-Value 값이 높아질 수 있음
    - 유의 수준?

### AB Test 를 해볼 수 있는 간단한 툴들

스크립트 방식으로 특정 스크립트를 AB Test를 원하는 영역에 넣으면 선택한 실험 타입에 따라 AB Test가 진행

- Optimizely - 다양한 통계적 분석값, 모바일 앱 AB Test를 지원
- Google Optimize360 - Google Analytics 및 Tag Manager 와의 Intergration

## 테스트 코드
출처
[https://galid1.tistory.com/783](https://galid1.tistory.com/783)
[https://loopstudy.tistory.com/25](https://loopstudy.tistory.com/25)
[https://ssowonny.medium.com/설마-아직도-테스트-코드를-작성-안-하시나요-b54ec61ef91a](https://ssowonny.medium.com/%EC%84%A4%EB%A7%88-%EC%95%84%EC%A7%81%EB%8F%84-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BD%94%EB%93%9C%EB%A5%BC-%EC%9E%91%EC%84%B1-%EC%95%88-%ED%95%98%EC%8B%9C%EB%82%98%EC%9A%94-b54ec61ef91a)
[https://sabarada.tistory.com/68](https://sabarada.tistory.com/68)

### 장점

- 테스트를 위해 서버를 실행하는 등의 시간을 절약할 수 있다
- 필요한 데이터를 미리 기입하고, 테스트가 끝나고 정리하는 등의 행동을 하지 않아도 된다
    - 다양한 유스케이스에 대한 테스트를 기억하고 관리하지 않아도 된다
- 단위테스트의 경우 수십 ms 이기 때문에 테스트가 매우 빠르다
- 문서로서 역할이 가능하다 → 테스트 코드는 개발자가 작성할 메소드가 어떻게 동작하였으면, 어떤 결과를 반환했으면 하는 것을 작성한 것이기 때문에 처음 코드를 보는 개발자들이 테스트 코드를 통해서, 코드의 동작을 좀 더 수월하게 이해할 수 있다
- 깔끔한 인터페이스를 얻어낼 수 있다
- 제품의 안정성을 높이고, 기능 추가 및 수정으로 인한 부작용(Side-effect)를 줄일 수 있다
- 디버깅을 쉽게 해준다 → 에러가 발생한 부분을 빠르게 파악할 수 잇다
- 재사용성 증가 → 코드만 실행하면 테스트가 끝난다

### 테스트 범위

- 통합 테스트 : 여러 작업 단위가 연계된 워크플로우를 테스트 하기 위한 수단 (객체 간, 서비스 간, 시스템 간)
- 기능 테스트 : 공개된 API 의 가장 바깥쪽에 해당하는 코드 검사 (Controller 호출, Security, http)
- 부하 테스트 : 주어진 단위 시간 동안 애플리케이션이 얼마나 많은 요청을 처리할 수 있는지 검사
- 인수 테스트 : 고객 또는 대리인이 정의되어진 모든 목적에 부합되는지 확인해보고자 하는 검사

### 단위 테스트가 필요한 이유

> 단위 테스트는 메서드 단위의 테스트로 어플리케이션이 기대한대로 동작함을 증명하고, 버그를 조기에 잡아내는 것을 기본 목적으로 한다
> 
- 연관 컴포넌트의 제작이 완료되지 않더라도 코드 푸쉬 가능 → mock 을 활용
- 단위 테스트 스위트를 이용하여 새로운 기능의 추가 및 기존 기능의 수정에 대해 이전 코드의 정상동작을 확인???
- Class 설계 변경에 대한 리팩토링이 일어날 때 정당성 부여 가능
    - 단위 테스트는 리팩토링에 의해 기존의 코드가 망가지지 않는다는 것을 보장
- 구현 코드의 품질 향상 기능
    - 단위 테스트가 너무 길어지거나 복잡해지는 것은 대상 코드에서 리팩토링이 필요하다는 의미
    - 또는 하나의 메서드에서 너무 많은 기능을 수행한다는 뜻도 될 수 있음

### F.I.R.S.T 단위 테스트 원칙

1. Fast : 유닛 테스트는 빨라야 한다
2. Isolated : 다른 테스트에 종속적인 테스트는 절대로 작성하지 않는다
3. Repeatable : 테스트는 실행할 때 마다 같은 결과를 만들어야 한다
4. Self-validating : 테스트는 스스로 결과물이 옳은지 그른지 판단할 수 있어야 한다. 특정 상태를 수동으로 미리 만들어야 동작하는 테스트 등은 작성하지 않는다
5. Timely : 유닛 테스트는 프로덕션 코드가 테스트를 성공하기 직전에 구성되어야 한다. 테스트 주도 개발 (TDD) 방법론에 적합한 원칙이지만 실제로 적용되지 않는 경우도 있다

### 테스트 코드 팁

1. given, when, then
    - 어떤 값이 주어지고(given), 무엇을 했을 때(when), 어떤 값을 원한다(then) 로 나누어 직관적이고 가독성 향상
    - 가독성이 중요한 이유는 문서로써의 역할을 하기도 하기 때문
2. 모든 response에 대한 테스트를 진행
    - 정상적으로 작동하는 부분만 테스트하는 것이 아닌, 실수나 오류를 발견하고 이를 줄이고 수정하기 위해 작성

- 목적
    - 캐시 서버를 어떨 때 쓰는지 몇가지 사례로 개념 잡기
    - 캐시 기본 개념 잠깐
    - 대표적인 분산 캐시 redis vs memcached 비교하고 언제 어떤걸 쓰는 것이 좋은지
    - 구체적인 레디스의 특징으로 대충 들어본 척 공부해본 척 할 수 있게
    - 우리는 redis를 어떻게 쓰고 있는지
- redis 개요
    - 뜻 : REmote DIctionary Server (외부 키-벨류 서버)
    - 사용처
        - 캐시 서버
        - Remote Data Store
            - A서버, B서버, C서버에서 데이터를 공유하고 싶을 때
        - 한 서버에서도 → Redis 자체가 Atomic을 보장(싱글 스레드)
        - 주로 쓰는 곳
            - 인증 토큰 등을 저장
            - Ranking 보드
            - 유저 API Limit
                
                ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8ff30899-5c73-482c-88d4-d6baa0ad851d/Untitled.png)
                
            - 잡 큐 - 비동기 작업 큐
- 캐시 개념
    - Cache는 나중에 올 요청의 결과를 미리 저장해두었다가 빠르게 서비스를 해주는 것
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ddb89aa3-1bc8-4fd2-8b02-a536f34ca25e/Untitled.png)
        
    
    Cache 구조 
    
    - Look aside Cache
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba903931-d27e-43c6-9ca1-693ecc65d279/Untitled.png)
        
    - Write Back
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1bfd3b9a-8b3b-4270-9a50-b5a35484188e/Untitled.png)
        
        `INSERT INTO table VALUES (1, "hello");
         INSERT INTO table VALUES (2, "world");
         INSERT INTO table VALUES (3, "!");`
        
        `INSERT INTO table VALUES (1, "hello"), (2, "world"), (3, "!");`
        
        Transaction, index 등 작업을 한번에
        
- 레디스 특징
    - memcached 와 비교
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b058519-c14a-4deb-a012-11cd7a7a1ac9/Untitled.png)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2271ce34-5322-4d6a-91eb-aeb9f9fba588/Untitled.png)
        
        공통점
        
        - In-Memory 기반
        - key-value 형식의 NoSQL
        
        차이점
        
        1. Memcached
            - 유일한 데이터 타입 String
            - 멀티스레드를 지원하기 때문에 멀티프로세스코어를 사용할 수 있음 → 스케일업 유리 (redis 3.0 부터 클러스터링 지원으로 동일해짐)
            - 트래픽이 몰려도 응답 속도 안정
            - slab 메모리 할당자 → 메모리 파편화 문제가 덜함 but, 데이터 변경이 잦으면 또 생김
            - 레디스에 비해 메타 데이터가 적다 → 메모리 사용량이 낮다
            
            > HTML과 같이 작고 변하지 않는 데이터 캐싱 
            간단하게 사용
            > 
        2. redis
            - 다양한 자료구조 Collection을 사용할 수 있음!! - 개발의 편의성, 개발의 난이도
            - Snapshot - 특정시점의 데이터를 디스크에 저장, 장애시 복구
            - 복제 - 여러개의 복제본, db 읽기를 확장하여 높은 가용성
            - 트랜젝션 - ACID 지원
            - Pub / Sub - 게시 / 구독
            - 위치기반 데이터 타입 지원 (Geo) - 두 위치의 거리, 사이에 있는 요소 찾기 등 → 맛집, 길찾기 서비스
            
            > 하나의 서버 기능을 담당하는 등 나머지 경우
            - 데이터 유형, 정렬 필요 시
            - pub/sub, 백업, 복원, 복제 기능들을 쓰고 싶을 때
            > 
        
- redis 자료구조
    - String, set, hash, list, sorted-set
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/303a03bc-25c6-48cf-afbc-dfaeb5ce0ee4/Untitled.png)
        
    - 웹 애플리케이션 서버 로그 통합
    - 날짜별 이벤트 페이지 방문 횟수 저장 - 날짜로 hash
        - 일회성 데이터: 이벤트 페이지에 대한 호출 횟수 요구
        - RDB에 필드 추가하면 속도 저하
    - 좋아요 처리하기 - set
    - 최근 조회 상품 목록 - list
    - 유저 랭킹 - sorted set
- 구조적 특징과 주의점
    - 싱글스레드 - 시간 복잡도 - O(n) 관련 명령어 주의
        - 단순 get / set 의 경우, 초당 10만 TPS 이상 가능
        - processInputBuffer
            
            들어오는 데로 조합해보고 command가 완성되면 바로 실행
            
            하나라도 1초가 걸리면...
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2bb2d75-f64a-4bdd-9891-92c765ff0293/Untitled.png)
            
        - 한번에 하나의 명령만 수행 가능 - 긴 시간이 필요한 명령을 수행하면 안됨
        - 쓰면 안되는 명령어
            - Keys
            - FlushAll, FlushDB
            - Delete Collections (리스트 전부 다 지우기)
            - Get All Collections
    - 메모리 관리: In-memory - 메모리 파편화, 가상 메모리
        - Physical Memory 이상을 사용하면 문제가 발생
            - Swap이 있다면 Swap 사용으로 해당 메모리 Page 접근시 마다 늦어짐
            - Swap이 없으면 OOM(out of memory)으로 죽을 수 있음
        - Maxmemory(논리적 메모리 사용량)를 설정하더라도 이보다 더 사용할 가능성이 큼
            - 엄청 좋다는 Jemalloc 이라는 메모리 할당을 씀 (직접 처리할 수 없음)
            - 그러나 페이지 단위로 할당이 되므로 RSS(실제 물리 메모리 사용량)이 더 높게
            - 메모리 파편화 (3.x대 버전의 경우, 2GB → 11GB의 RSS 사용 경우 자주 발생)
                
                ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/722962f0-a7bf-43b4-820c-c52833ff03ff/Untitled.png)
                
                ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f12f386-33be-488f-9d44-598681f31e83/Untitled.png)
                
    - replication (SPOF, 쓰기 담당/읽기 담당/백업용 - ProxySQL)
        - Primary - Secondary
        - Primary는 현재 메모리 상태를 저장하기 위해 Fork (저장해서 다른 노드로 보낼려고)
            - copy on write로 바로 두 배는 아니지만 write될 수록
        - 사진
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15c2edef-f825-4543-8aa5-32b3899d4a39/Untitled.png)
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/958d38b6-3670-42df-ba91-90bd9e4a3589/Untitled.png)
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/240499c1-2881-4d98-989b-9fc85a3e7477/Untitled.png)
            
- 우리의 레디스
    
    A서버, B서버, C서버에서 데이터를 공유하고 싶다 - 추천 topic이 생성 되었다는 것을!
    
    - Consistency(정합성) 문제 (SseEmitter 객체들이...) → remote data store
    - Distributed SSE with Spring SseEmitter and Redis Pub/Sub
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6d4ba47-394c-4bf6-a6d6-5a20fe0e3481/Untitled.png)
        
    - [https://drive.google.com/file/d/1DFT24_D3yC8yDHzVVahoSkdhaTJdFjE_/view?usp=sharing](https://drive.google.com/file/d/1DFT24_D3yC8yDHzVVahoSkdhaTJdFjE_/view?usp=sharing)
    - NoSQL 키 관리
        - nosql에서 데이터를 조회하기 위한 가장 기본적인 값, 키 하나가 하나의 레코드 정의
        - 단순성을 해치지 않으면서 정보를 효율적으로 저장 - 키 설계
        - select * from user_profile where userid = 'kris'
        - get user:profile:kris
        - RDB 는 값에 부분 정보, nosql 은 키 정보에 부분 정보
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/83c25a4f-e780-4773-845a-089e3cad4df2/Untitled.png)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/64bd462e-e169-4c46-820c-43067536575d/Untitled.png)
        
- 어려운 개념
    - redis를 저장소처럼 - redis Persistent, RDB, AOF
    - 메모리 제한으로 주기적 스케일 아웃, 백업 - redis cluster
    - 부하 분산 - Constant hashing
        - 서버가 하나 죽으면 딱 그 서버의 해시만 옮기면 됨
        - modular hashing과 비교 (나머지로 해싱)
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c29179b4-e5be-4aab-a353-ac10d03ccfb5/Untitled.png)
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60a7f18d-89da-4430-b639-80914e03d327/Untitled.png)
            
    - data grid - spring gemfire, hazlecast
    
- 클라우드 서비스 활용 - spof 등 해결
- 키 관리
- 질문
    
    멀티스레드 수평적 확장?
    
    - 스케일 아웃으로 spof 등 해결도 가능
    
    왜 Thread.sleep(100)을 하면 되는거지?
    
    redis pub / sub 원리
    
    vivox  이슈
    
- 출처
    
    이것이 레디스다 / 정경석
    
    [https://www.youtube.com/watch?v=Gimv7hroM8A&t=34s](https://www.youtube.com/watch?v=Gimv7hroM8A&t=34s)
    
    [https://www.youtube.com/watch?v=mPB2CZiAkKM](https://www.youtube.com/watch?v=mPB2CZiAkKM)
    
    [https://junghyungil.tistory.com/165](https://junghyungil.tistory.com/165)
    
    [https://down-develope.tistory.com/24](https://down-develope.tistory.com/24)
    
    [https://ssup2.github.io/theory_analysis/Consistent_Hashing/](https://ssup2.github.io/theory_analysis/Consistent_Hashing/)
    
    [https://www.geekyhacker.com/2019/08/07/distributed-sse-with-spring-sseemitter-and-redis-pub-sub/](https://www.geekyhacker.com/2019/08/07/distributed-sse-with-spring-sseemitter-and-redis-pub-sub/)
    
    [https://developer-mac.tistory.com/21](https://developer-mac.tistory.com/21)

## OAuth2.0

- OAuth는 표준 방식이다.
    
    OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용 되는, 접근 위임을 위한 개방형 표준이다. (위키백과) (OpenID Authentication의 줄임말)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89801d52-751c-4af3-951a-44531bdcc700/Untitled.png)
    
- OpenID와 OAuth
    
    OAuth는 `인증` 프로토콜이 아닌, `인가` 프로토콜이다.
    
    인증과 인가
    
    - 인증(Authentication)은 사용자가 일정 권한이 있다는 것을 **아이디와 패스워드로 증명**하는 것
    - 인가, 허가(Authorization)는 로그인 하고 나서, 내 계정으로만 할 수 있는 활동을 시도할 때 내 **로그인 정보를 보고 허용**해 주는 것
    
    ### OpenID와 OAuth
    
    OpenID도 인증을 위한 **표준 프로토콜**이고 **HTTP**를 사용한다는 점에서는 OAuth와 같다. 그러나 OpenID와 OAuth의 **목적은 다르다.**
    
    **OpenID의 주요 목적은 인증(Authentication)이지만, OAuth의 주요 목적은 허가(Authorization)이다.** 즉, OpenID를 사용한다는 것은 본질적으로 로그인하는 행동과 같다. OpenID는 OpenID Provider에서 사용자의 인증 과정을 처리한다. Open ID를 사용하는 여러 서비스(Relying Party)는 OpenID Provider에게 인증을 위임하는 것이다.
    
    물론 OAuth에서도 인증 과정이 있다. 가령 Facebook의 OAuth를 이용한다면 Facebook의 사용자인지 인증하는 절차를 Facebook(Service Provider) 처리한다. **하지만 OAuth의 근본 목적은 해당 사용자의 담벼락(wall)에 글을 쓸 수 있는 API를 호출할 수 있는 권한이나, 친구 목록을 가져오는 API를 호출할 수 있는 권한이 있는 사용자인지 확인하는 것이다.**
    
    OAuth를 사용자 인증을 위한 방법으로 쓸 수 있지만, OpenID와 OAuth의 근본 목적은 다르다는 것을 알아야 한다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94464c23-0d53-4470-9a76-24c9c1e5e0c8/Untitled.png)
    
- OAuth는 왜 쓸까?
    
    간단한 방법은 고객한테 네이버 아이디/비밀번호를 받아서 네이버에 로그인을 해보면 된다. 근데 이 방법은 말도 안되는 방법이다. 그래서 `OAuth`가 생겼다.
    
    `OAuth`는 쉽게 말해서 다른 서비스의 회원 정보를 **안전하게** 사용하기 위한 방법이라고 생각하면 된다. 여기에서 **안전하게**의 주체는, 회원 정보를 가지고 있는 주체, 우리의 **고객**이다. 즉, 우리의 **고객이 안전하게** 다른 서비스의 정보를 우리 서비스에 건네주기 위한 방법이다.
    
    `모두에게 좋다`
    
    SNS 업체에서는 자사 서비스가 갖고 있는 고객의 정보를 다른 서비스에 활용할 수 있게 해주면(고객의 동의 하에)
    
    1. 다른 서비스가 자사 서비스에 `의존적이게 되어 입지가 더 커질` 것이고
    2. `고객 역시` 이를 통해 더욱더 자사 서비스에 `의존적이게` 될 것이므로(예를 들어 많은 서비스에 페이스북 로그인을 통해서 회원가입을 하게 되면, 페이스북을 탈퇴하기가 더욱 힘들어 질 것이다)
    
    이는 매우 좋은 서비스 전략이 될 수 있다.
    
    마찬가지로 `써드 파티 서비스`(SNS를 이용하는 다른 서비스)의 경우, 대형SNS 기업의 고객 정보를 이용하면 `해당 SNS에서 제공해주는 많은 기능`을 통해 더욱더 좋은 서비스를 고객에게 제공할 수 있으므로 매우 좋은 전략이 될 수 있다.
    
    `고객`의 입장에서도 `가입이 편리`하고 이미 사용하고 있던 정보를 다른 서비스에서도 활용할 수 있어 좋다.
    
- Access Token
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60485cf5-f38b-43fe-aa06-f5c36bab68eb/Untitled.png)
    
    OAuth의 핵심은 Access Token
    
    Access Token은 임의의 문자열 값이고 문자열의 내용은 토큰을 발급해준 서비스(구글)만 알 수 있다. 
    
    (JWT는 기본정보가 Base64 인코딩 되어 있어서 알 수 있지만)
    
    이 토큰을 이용해서 우리(waggle)는 고객의 정보를 해당 서비스(구글)에 요청할 수 있다
    
    Access Token의 존재 자체가 고객이 정보를 넘겨주는 것을 동의함의 징표라고 할 수 있다.
    
    이걸 위해 우린 OAuth를 사용!
