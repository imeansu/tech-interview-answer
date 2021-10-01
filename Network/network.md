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
