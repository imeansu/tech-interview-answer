## java의 메인 메서드가 왜 public static void 일까?
출처 
[https://madplay.github.io/post/java-main-method-structure](https://madplay.github.io/post/java-main-method-structure)
[https://coco-log.tistory.com/138](https://coco-log.tistory.com/138)

### public

- 접근 지정자 또는 접근 제한자, 제한없이 어디에서나 사용 가능
- JVM이 실행되고 실행된 JVM이 main()을 실행
- 클래스 밖에, 패키지 밖에 존재하는 JVM이 main()에 접근할 수 있어야 하기 때문

### static

- 정적 메서드
- static으로 선언하게 되는 경우 자바가 컴파일 되는 시점에 정의
- static 요소를 static이 아닌 요소에서 호출한느 것은 불가능
- main 메서드처럼 프로그램의 시작점에 되는 요소는 객체를 생성하지 않앙도 작업을 수행해야 하기 때문에 static
- 실행되기 위해서는 메모리에 미리 올라가야 함 → static을 선언하여 메모리 할당을 하지 않아도 사용할 수 있게 만든다

### void

- 멀티 스레드를 염두하고 만들어진 언어이기 때문에
- 자식 스레드를 생성 시키고 실행시키는 데 main이 리턴 코드를 뱉고 프로그램 자체를 종료시켜버리면 안됨..

### main

- main이라는 이름은 JVM이 식별할 수 있는 키워드. 반드시 메인 메서드의 이름은 main
