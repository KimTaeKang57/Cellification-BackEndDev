# HTTP 통신과 SOCKET 통신의 차이점

# HTTP 통신
- HTTP란 Hyper Text Transfer Protocol, HTML 파일을 전송하는 프로토콜이다. 웹 브라우저에서 통신이 일어나며, 초기에는 HTML 파일을 전송하려는 목적으로 만들어졌으나, 현재는 JSON, Image 파일 등 또한 전송한다.

### HTTP 통신의 통신 방식
<img width="561" alt="스크린샷 2022-06-15 오후 7 18 18" src="https://user-images.githubusercontent.com/83891837/173804352-09e723bb-e005-4e11-8eae-b85391aa55f1.png">

- HTTP 통신은 클라이언트에서 서버로 요청을 보내고 서버가 응답하는 방식으로 통신이 이루어진다. 응답에는 클라이언트의 요청에 따른 결과를 반환한다. 따라서 클라이언트의 요청이 있을 때 서버가 응답하는 방식인 단방향 통신이다.
- 응답을 받은 후 Connection이 끊어지는 것이 기본 동작이지만, 성능 상으로 필요하다면 Keep Alive 옵션을 주어 일정 시간 동안 Connection을 유지하는 것이 가능하다. 

* Keep Alive
- Http는 Stateless(무상태) 이며, Connectionless이다. 즉 데이터를 우리가 지정한 시간 내에서 계속 주고 받을 수 있다는 것이다. 데이터를 빈번하게 주고받아야한다면 좋은 기능이지만, 사용자가 많아 연결이 늘어나 새로운 사용자를 받아들이지 못하게 될 수도 있다. 그래서 사용자가 많고 유동이 많은 서비스에서는 사용이 권장되지 않는다.

Stateful / Statleless (상태 / 무상태)
- Stateful : Client와 Server의 동작, 상태 정보를 저장하는 형태
- Stateless : Client와 Server의 동작, 상태 정보를 저장하지 않는 형태

Connectionless(비 연결성)
- Client가 Server에 요청을 하고 응답을 받으면 바로 TCP/IP 연결을 끊어 연결을 유지 하지 않는 것이다. 

# Socket 통신
- Socket이란 두 프로그램이 서로 데이터를 주고 받을 수 있게 양쪽에 생성되는 통신 단자이다.
<img width="561" alt="스크린샷 2022-06-15 오후 7 29 56" src="https://user-images.githubusercontent.com/83891837/173806463-3f3e465b-6acd-4247-9245-ce6304304158.png">

### Socket 통신의 통신 방식
- 소켓 통신이란 서버와 클라이언트가 서로에게 데이터 전달을 해 양방향 연결이 이루어지는 통신 (클라이언트도 서버로 요청을 보낼 수 있고 서버도 클라이언트로 요청을 보낼수 있음).
- 보통 스트리밍, 실시간 채팅 등 데이터를 주고 받아야 하는 경우에 Connection을 자주 맺고 끊는 HTTP 통신보다 소켓 통신이 적합하다.
- Socket은 Connection을 들고 있기 때문에 HTTP 보다 많은 리소스가 소모된다.


### Socket 이란 무엇인가 ?
