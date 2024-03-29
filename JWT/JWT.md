# JWT 등장 배경

JWT는 Json Web Token으로, 웹 애플리케이션에서 인증 및 정보 교환을 위해 사용하는 토근 기반의 인증 방식을 일컫는다.
이 인증 방식은 기존의 웹 애플리케이션이 가지고 있던 인증 및 보안 문제를 해결하기 위해 출현하게 되었다.

현재까지도 많은 레거시 환경에서 사용하고 있는 세션 인증 방식은 사용자가 웹 애플리케이션에 로그인을 수행하면,
서버에서는 해당 인증에 대한 세션을 유지하고, 서버에게로 매 요청을 보낼 때마다, 세션 ID를 쿠키 등으로 전송하여 인증을 유지하는 방식이다.

이 세션 기반 인증 방식은 서버에 세션의 상태를 계속 유지해야 하며, 확장성과 서버 부하에도 적잖은 문제가 된다.
물론, 그렇다고 해서 확장이 불가능한 것은 아니고, 세션 클러스터링을 통해 세션 기반 인증 방식도 멀티 서버 환경에서 동작이 가능하다.

여하튼, JWT는 이러한 단점들을 극복하기 위해 나왔는데, 이 JWT는 Claim이라는 JSON 객체를 사용하여 사용자 정보 및 추가적인 정보를
안전하게 저장하며, 세션 방식과 다르게 서버 측에 세션 상태를 유지하지 않고 토큰 자체가 사용자를 인증할 수 있는 매개체 역할을 수행한다.

# JWT의 장점
- **Stateless** : 앞서 언급한 바와 같이 세션 상태를 유지할 필요 없이 토큰 자체가 사용자를 인증하므로 사용자 정보를 저장할 필요가 없다.
- **Scalable** : 세션을 사용하지 않기 때문에 여러 노드에서 동일한 JWT 토큰 값을 가지고 사용자를 식별하고 요청을 처리할 수 있다.
- **Interoperable** : JSON 포멧을 사용하기 떄문에 다양한 플랫폼 간에 정보를 교환하기 용이하다.
- **Secure** : 토큰은 서명된 서버에서 생셩되며, 변경되지 않도록 보호되므로 안전한 인증 방식으로 사용된다.

# JWT 구성
JWT는 3개의 Base64 URL-safe 인코딩된 문자열로 구성된다.

JWT는 `Header`.`Payload`.`Signature` 와 같은 형태를 띈다.

헤더에는 총 2개의 정보를 담고있다.
- `typ` : 토큰 타입을 나타내며, JWT인 경우 **JWT**로 지정한다.
- `algorithm` : 토큰을 서명하기 위해 사용된 알고리즘을 나타낸다. 예를 들어, HMAC SHA256또는 RSA 와같은 형태로 작성한다.

페이로드는 클레임이라고 불리는 데이터를 포함한다. 앞서 언급되었던 클레임은 서버와 클라이언트가 공유하는 정보를 담고있다.
클레임은 총 3가지 유형으로 나뉜다.
- 등록되어진 클레임 : **iss(Issue)**, **sub(Subject)**, **aud(Audience)**, **exp(Expired At)**, **nbf(Not Before)**, **iat(Issued At)**, **jti(JWT ID)** 과 같은 사전 정의된 키를 사용한다.
- 공개 클레임 : 충돌을 피하기 위해 서버와 클라이언트 간 사전 약속된 정보를 담는다.
- 비공개 클레임 : 클라이언트와 서버 사이에 사전 협의된 사용자 정의 클레임이다.

서명: 서명은 헤더와 페이로드를 인코딩한 문자열을 비밀키로 사용하여 암호화한 값이다. 서버는 이 서명을 통해 이를 검증하고 토큰이 위변조 되었는지를 호가인한다.

이렇게 총 3가지의 구성을 통해 JWT가 생성되며 아래는 JWT의 예시이다.

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`


# 내가 계속 헷갈리는 포인트

기존 일반적인 Session 방식에서는 Spring security 에서 제공하는 UsernamePassowrdAuthenticationFilter를 통해, 사용자 정보를 식별하고 인증을 수행한다. 내부 매커니즘을 간단히 다뤄보자면,
최초 또는 패스워드 변경 시, PasswordEncoder를 통해 패스워드를 암호화하여 저장하고, 로그인을 수행했을 때, 암호화된 패스워드와 사용자가 입력하여 요청한 패스워드가 일치하는지를 `PasswordEncoder.match()`
를 통해, 인증하고 세션을 발급한 뒤, `JSESSION_ID`를 발급하여 사용자 인증 및 식별을 처리하였다.

JWT 인증방식도 사실 크게 다르지 않다.

로그인 방식은 전과 크게 다르지 않다. 사용자 패스워드를 암호화하여 저장하고, 로그인 당시 UsernamePasswordAuthenticationFilter를 통해, 사용자가 있는지 식별하고, 패스워드가 일치하는지 체크한다.
사실 상기 UsernamePasswordAuthenticationFilter는 인증 처리 방식에 불과하지 않는다.

나는 매번 이부분과 JWT 간의 연관성에 대해서 계속적으로 헷갈리고 있었는데 가장 큰 초점은 세션을 만들고 이를 관리하느냐, 아니면 JWT 토큰을 발급하느냐의 차이일 뿐이다.

다시말하면, JWT를 **언제 어떻게 발급**하고, **언제 어떻게 만료**되며, **언제 어떻게 인증 검증**을 수행하느냐에 포커스만 두면 되는 것이다.

# JWT 인증 매커니즘

일반적인 인증 매커니즘 순서에 따라서 나열해보겠다.
먼저 기본적인 인증 매커니즘은 이러하다.

1. HTTP 요청이 인증을 필요로 하는지 체크한다.
2. JWT 토큰이 존재하는지 체크한다.
  - 존재할 경우
    - 토큰이 유효한지 체크하고, 유효할 경우 복호화된 객체를 반환한다.
    - 복호화된 객체를 통해, Claim 정보를 토대로 사용자 존재 여부를 확인한다.
    - Claim 정보를 통해, 만료 여부 등 추가 유효성 검증을 수행하고 문제가 없으면 인증 처리를 완료하고 다음단계로 넘어간다.
  - 존재하지 않을경우, 혹은 존재하더라도 뭔가 실패했을 경우,
    - UsernamePasswordAuthenticationFitler의 로직에 따라, 요청 정보에 사용자 정보(ID, Password) 가 존재하는지 확인하고, 존재할 경우 사용자 정보가 일치하는지 확인한 뒤, 일치할경우 JWT 토큰을 발급하여 로그인 처리를 수행한다.
  - 이마저도 실패할 경우
    - 접근이 불가함을 확인하고 로그인 페이지로 리다이렉트 하거나, 기타 인증이 실패했을 경우에 해당하는 액션을 수행하면 된다.
   
자세한 사항은 [Spring Securiy + JWT](https://github.com/small-dogg/spring-security-jwt) 리포지토리를 확인을해보자.
