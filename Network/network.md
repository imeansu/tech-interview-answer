UDP(User Datagram Protocol, 사용자 데이터그램 프로토콜)

- 비연결형 프로토콜
- IP 데이터그램을 캡슐화하여 보내는 방법과 연결을 설정하지 않고 보내는 방법을 제공
- 흐름제어, 오류제어 또는 손상된 세그먼트의 수신에 대한 재전송을 하지 않는다
모두 사용자 프로세스의 몫
- 요청 또는 응답이 손실된다면, 클라이언트는 time out되고 다시 시도할 수 있으면 됨. 코드가 간단할 뿐만 아니라 TCP처럼 초기 설정에서 요구되는 프로토콜보다 적은 메시지 요구
- DNS 서버는 UDP