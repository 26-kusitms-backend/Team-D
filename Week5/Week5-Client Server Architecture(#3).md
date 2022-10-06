# [백엔드 스터디 in 큐시즘 - D조] Week 5: 스프링의 전반적인 이해
## #3. 클라이언트-서버 구조의 프로젝트 아키텍처를 그려보고, 데이터 흐름 설명하기

<img src="./image/minhyuk.png">

- 클라이언트와 서버는 HTTP프로토콜을 이용해 서로 통신한다
- 클라이언트와 서버가 HTTP 통신할 때 주고 받는 메시지를 HTTP메시지라고 한다
- HTTP메시지는 start line, header, empty line, message body 총 4개의 부분으로 이루어져있다.
- 클라이언트가 서버에 요청할 때 날리는 메시지를 HTTP 요청 메시지라고 하고 서버가 클라이언트에게 응답할 떄 날리는 메시지를 HTTP 응답 메시지라고 한다
- HTTP 요청 메시지
    - start-line은 HTTP 메소드, URI의 path, query, HTTP 버전으로 이루어져있다
- HTTP 응답 메시지
    - start-line은 HTTP 버전, HTTP 상태 코드, 이유문구로 이루어져있다
- header는 message body의 크기, 압축, 요청 클라이언틑 정보 등 HTTP전송에 필요한 부가정보가 들어간다
- message body는 byte로 이루어진 실제 전송할 데이터가 들어간다