<aside>
💡 Oauth 2.0은 무엇인가

</aside>

- Open Authorization 2.0
- 인증을 위한 개방형 표준 프로토콜이다
- Third-party 프로그램에게 리소스 소유자를 대신해 리소스 서버에서 제공하는 리소스에 대한 접근 권한을 위임하는 방식으로 작동된다
- 구글, 페이스북, 네이버, 카카오 등 여러 소셜 계정을 기반으로 간편하게 인증한다
- Authentication 인증
    - 사용자가 접근 자격이 있는지 검사하는 단계 (로그인)
- Authorization 인가
    - 리소스에 접근할 권한을 부여하고 리소스에 대한 접근 권한이 있는 Access Token 부여
- Access Token
    - 서버로부터 리소스 정보를 얻을 때 사용되는 토큰으로 상대적으로 만료기간이 짧다
- Refresh Token
    - Access Token 만료 시 재발급을 받기 위한 용도로 사용된다. 만료기간이 길다

<aside>
💡 Spring Security 는 무엇이고, 사용 목적은 무엇인가
Spring Security 를 사용해봤다면, 개발 프로세스와 아키텍처를 그려서 설명하기
Spring Security 를 사용해보지 않았다면, 어떻게 프로젝트에 적용할 것인지 개발 프로세스와 아키텍처를 그려서 설명하기

</aside>

- 시스템에서 회원 관리에 대한 보안(인증과 인가, 권한)에 대한 처리를 해주기 위해 사용
- 인증과 권한에 대해 Filter 흐름에 따라 처리한다
- Filter는 Dispatcher Servlet에 가기 전에 적용되므로 가장 먼저 URL요청을 받는다
- 인증과 인가
    - 인증 : 해당 사용자가 등록된 본인이 맞는지 확인하는 절차
    - 인가 : 인증된 사용자가 요청한 자원에 접근 가능한지를 확인하는 절차
    - 기본적으로 인증과정을 거친 후 인가과정을 검증한다
- Spring Security는 인증과 인가를 위해 Principal을 아이디로, Credential을 비밀번호로 사용하는 Credential 기반의 인증방식을 사용한다
    - Principal(접근주체) : 보호받는 리소스에 접근하는 대상
    - Credential(비밀번호) : 리소스에 접근하는 대상의 비밀번호
- Spring Security 모듈
    - SecurityContextHolder
        - 보안 주체의 세부 정보를 포함하여 응용 프로그램의 현재 보안 컨텍스트에 대한 세부 정보가 저장된다
    - SecurityContext
        - Authentication을 보관하는 역할을 하며, 이 모듈을 통해 Authentication 객체를 꺼내올 수 있다
    - Authentication
        - 현재 접근하는 주체의 정보와 권한을 담은 인터페이스이다
        - 이 객체는 SecurityContext에 저장되며, SecurityContextHolder를 통해 SecurityContext
    - UsernamePasswordAuthenticationToken
        - User의 id가 principal, pw가 credential의 역할을 한다
        - 인증되기전과 인증완료된 객체를 생성
    - AuthenticationProvider
        - 인증되기 전의 Authentication 객체를 받아서 인증이 완료된 객체를 반환한다
    - AuthenticationManager
        - 인증이 성공한 객체를 생성하여 SecurityContext에 저장한다
    - ProviderManager
        - 실제 인증을 위한 로직을 가지고 있는 AuthenticationProvider를 리스트로 가지고있다
    - UserDetails
        - 인증에 성공하여 생성된 UserDetails 객체는 Authentication객체를 구현한 UsernamePasswordAuthenticationToken을 생성하기 위해 사용된다
    - UserDetailsService
        - UserDetailsService 인터페이스는 UserDetails 객체를 반환하는 단 하나의 메소드를 가지고 있는데, 일반적으로 이를 구현한 클래스의 내부에 UserRepository를 주입받아 DB와 연결하여 처리한다

<aside>
💡 JWT 는 무엇인가

</aside>

- Json Web Token
- Json 포맷을 이용해 인증에 필요한 정보들을 암호화한 토큰이다
- 사용자가 서버에 인증을 받을 때 사용되는 방식으로 최초 인증을 받으면 서버는 사용자에게 JWT토큰을 보낸다. 그 후 사용자는 JWT토큰을 HTTP헤더에 담아 보내서 인증을 받는다.
- JWT토큰이 생성될 때 Access Token과 Refresh Token이 함께 사용자에게 전송된다. 사용자는 Access Token을 이용해 인증을 받고, Access Token의 시간이 만료되면 Refresh Token을 이용해 새로운 Access Token을 발급받는다.
- 구조
    - Header.Payload.Signature 의 형태이다
    - Header(헤더)
        - JWT에서 사용할 타입과 해시 알고리즘의 정보가 담겨있다
    - Payload(내용)
        - 서버에서 첨부한 사용자 권한 정보와 데이터가 담겨있다
    - Signature(서명)
        - Header와 Payload의 내용을 각각 암호화하고 개인키를 합한 문자열을 다시 암호화한다

<aside>
💡 SSL은 무엇인가

SSL을 적용했을 때의 아키텍처를 그려서 설명하기

</aside>

- Secure Socket Layer(암호화 소켓층)
- SSL이 적용된 HTTP프로토콜은 HTTPS로 표기된다
- 웹 서버와 클라이언트의 통신 암호화 프로토콜이다
- SSL이 적용되지 않은 통신을 할 때 평문 그대로 전송이되지만 SSL이 적용된 통신을 할 떄는 평문이 암호화 돼서 전송되므로 복호화 키가 없으면 원래 내용을 알 수 없다
- TLS(Transport Layer Security)
    - SSL을 보완한 통신 프로토콜이다
    - SSL을 보완해서 나온 프로토콜이지만 인터넷 상에 적용되는 모든 보안 프로토콜을 SSL이라고 통틀어서 부른다
    - SSL은 2015년에 중지됐으므로 현재 사용중인 프로토콜은 모두 TLS이다
- 기본포트는 443번이다
- 통신 데이터가 암호화되어 패킷이 탈취되어도 정보를 안전하게 지킬 수 있다
- SSL인증서를 통해 도메인의 신뢰성을 검증할 수 있다
- 데이터 송수신 과정에서 암복호화가 발생하므로 속도가 느리다
