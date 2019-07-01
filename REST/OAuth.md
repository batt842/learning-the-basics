# OAuth in RESTful API

Kerberos하고 개념이 꽤 다르구나. Kerberos는 클라이언트가 여러 서버에 접근할 수 있는 거고, OAuth는 클라이언트가 사용하는 서비스가 다른 서비스에 접근할 수 있게 해주는 거네. 다른 프로토콜이 지원하는지는 모르겠지만 athorization을 통해 접근 권한 grant가 가능하다.
소셜로그인에서 쓰는 건 OAuth2.0의 Authorization Code Grant, 웹앱이나 모바일앱에서는 Implicit Grant를 사용하는 것 같다. 둘 모두 resource server에 접근하기 위해 authorization server로부터 access token을 발급받는 과정이 핵심이며, 아직은 그 과정의 차이가 뭔지 모르겠다. 이 외에도 OAuth2.0에는 네 가지 인증 방식이 있다. 그건 패스.

이걸 MSA나 REST에서는 어떻게 사용하나?

## with MSA
내부망에서라면 보안에 문제가 없겠지만, 웹앱이나 모바일앱에서 API 게이트웨이로 호출할 때는 다양한 정보(특히 리소스 URI)가 노출된다. HTTPS로 암호화할 수는 있겠지만, 공개 프로토콜인 이상 추가적인 접근 제어가 필요하다. 이 경우 애플리케이션에서 API로의 access token을 획득하는 authorization이 필요하다. 그 때 적절히 사용할만한 것이 OAuth이고.
https://minwan1.github.io/2018/03/11/2018-03-11-Spring-OAuth%EA%B5%AC%ED%98%84/

## with Spring


## Other authentication protocols
LDAP, Kerberos, SAML, etc...