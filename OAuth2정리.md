### OAuth 2.0 기반의 인증 및 권한부여 방식으로 Resource Owner Password Credentials
인증 및 권한부여를 위해서 OAuth 프레임워크의 현재(2017.03.13) 버전은 2.0

https://tools.ietf.org/html/rfc6749

OAuth는 서버와 클라이언트 사이에 인증을 완료하면 서버는 권한부여의 결과로써 access token을 전송합니다. 
클라이언트는 access token을 이용해서 접근 및 서비스를 요청할 수 있습니다. 
서버는 access token 기반으로 서비스와 권한을 확인하여 접근을 허용할지 말지를 결정하고, 결과 데이터를 클라이언트에게 보내줍니다. 
서버는 access token을 기반으로 클라이언트를 확인하여 서비스하기 때문에, 
세션(session)이나 쿠키(cookie)를 이용해 클라이언트의 상태정보를 유지할 필요가 없습니다.

### OAuth를 구성하고 있는 주요 4가지 객체(Roles)
- resource owner(자원 소유자)는 protected resource(보호된 자원)에 접근하는 권한을 제공합니다.
- resource server(자원 서버)는 access token을 사용해서 요청(request)을 수신할 때, 권한을 검증한 후 적절한 결과를 응답합니다.
- client(클라이언트)는 resource owner(자원 소유자)의 protected resource(보호된 자원)에 접근을 요청을 하는 애플리케이션(application)입니다.
- authorization Server(권한 서버)는 client(클라이언트)가 성공적으로 access token을 발급받은 이후에 resource owner(자원 소유자)를 인증하고 
obtaining authorization(권한 부여)를 합니다.

> ### Protocol Flow
     +----------+
     | Resource |
     |   Owner  |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier      +---------------+
     |         -+----(A)-- & Redirection URI ---->|               |
     |  User-   |                                 | Authorization |
     |  Agent  -+----(B)-- User authenticates --->|     Server    |
     |          |                                 |               |
     |         -+----(C)-- Authorization Code ---<|               |
     +-|----|---+                                 +---------------+
       |    |                                         ^      v
      (A)  (C)                                        |      |
       |    |                                         |      |
       ^    v                                         |      |
     +---------+                                      |      |
     |         |>---(D)-- Authorization Code ---------'      |
     |  Client |          & Redirection URI                  |
     |         |                                             |
     |         |<---(E)----- Access Token -------------------'
     +---------+       (w/ Optional Refresh Token)

   Note: The lines illustrating steps (A), (B), and (C) are broken into
   two parts as Protocol Flowthey pass through the user-agent.

                     Figure 3: Authorization Code Flow
                     
    +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+

---
### (A) Client(클라이언트)가 Resource Owner(자원 소유자)에게 권한 요청(Authorization Request)하게 됩니다. 
이 때 권한 요청은 Resource Owner(자원 소유자)에게 직접 하거나, Resource Server(권한 서버)를 통해 간접적으로 이루어 질 수도 있습니다.

### (B) Authorization Grant(권한 증서)는 자원 소유자가 자원에 접근할 수 있는 권한을 부여하였다는 확인증으로 
Client(클라이언트)가 access token 을 요청하여 얻어오는데 사용됩니다. 이 권한 증서는 총 4개의 타입이 있습니다. 
-Authorization Code: used with server-side Applications
-Implicit: used with Mobile Apps or Web Applications (applications that run on the user's device)
#-Resource Owner Password Credentials: used with trusted Applications, such as those owned by the service itself
-Client Credentials: used with Applications API access

> ### B-1. Authorization Code는 Client(클라이언트)가 Resource Owner(자원 소유자)에게 직접 권한 부여를 요청하는 대신, 
Resource Owner(자원 소유자)가 Authorization Server(권한 서버)에서 인증을 받고 권한을 허가 합니다. 
소유자가 권한을 허가하게 되면 Authorization Code(권한 코드)가 발급되고, 이 Authorization Code(권한 코드)를 클라이언트에게 전달하게 됩니다. 
클라이언트는 이 코드를 권한 서버에 보내주면서 자신이 권한 허가를 받았다는 사실을 알리고 access token을 받게 됩니다. 
이 방법은 보안상 이점이 있습니다. 
access token을 바로 Client(클라이언트)로 곧바로 전달하지 않기 때문에 전달과정에서 생길 수 있는 잠재적인 유출 위험을 방지하는데 도움을 줍니다.

> ### B-2.Implicit는 Authorization Code(권한 코드)를 간소화한 절차입니다. 
Authorization Code(권한 코드) 방식에서 access token을 얻기 위한 중간 매개체로 Authorization Code(권한 코드)를 사용했던 것과 달리, 
이 방식은 Authorization Code(권한 코드)가 별도로 발급되지 않고 access token이 바로 발급됩니다. 
과정이 줄어들어 편해지는 대신 보안성은 낮아집니다.

> ### B-3. Resource Owner Password Credentials 이 방식에서는 자원 소유자의 계정 아이디와 비밀번호 같은 계정 인증 정보가 
access token을 얻기 위한 Authorization Grant(권한 증서)로 사용됩니다. 
계정정보를 애플리케이션에 직접 입력해야 하므로, 신뢰할 수 있어야 합니다. 
access token 을 얻은 후에는 리소스 요청을 위해 계정 아이디, 비밀번호를 Client(클라이언트)가 보관하고 있을 필요는 없습니다.

> ### B-4. Client Credentials 이 방식은 클라이언트 인증 방식이라고도 합니다. 
자원 소유자가 유저가 아닌, 클라이언트인 상황에서 활용되는 방식입니다.
Client(클라이언트)가 관리하는 리소스에만 접근할 경우로 권한이 한정되어 있을 때 활용할 수 있습니다.
즉 Client(클라이언트)가 곧 Resource Owner(자원 소유자)가 되는 상황입니다.
Client(클라이언트)는 자기를 인증할 수 있는 정보를 Authorization Server(권한 서버)에 보내면서 access token을 요청하게 됩니다.

(C) 권한 증서(Authorization Grant)를 받은 클라이언트(Client)는 최종 목적인 access token을 권한 서버에 요청합니다.
(D) 요청을 받은 Authorization Server(권한 서버)는 클라이언트(Client)가 보내온 권한 증서(Authorization Grant)의 유효성을 검증합니다. 
유효하다면 access token을 발급하고 결과를 클라이언트(Client)에 알려줍니다.
(E) access token을 받은 클라이언트(Client)는 자원 서버(Resource Server)에 자원을 요청할 수 있게 됩니다.
(F) 요청을 받은 자원 서버(Resource Server)는 access token의 유효성을 검증하고 유효하다면 요청을 처리해줍니다.

### - Access Token은 요청 절차를 정상적으로 종료한 클라이언트에게 발급됩니다. 
이 토큰은 보호된 자원에 접근할 때 권한 확인용으로 사용됩니다. 
문자열 형태이며 클라이언트에 발급된 권한을 대표하게 됩니다. 
계정 아이디와 비밀번호 등 계정 인증에 필요한 형태들을 이 토큰 하나로 표현함으로써, 
리소스 서버는 여러 가지 인증 방식에 각각 대응 하지 않아도 권한을 확인 할 수 있게 됩니다.

### - Refresh Token은 한번 발급받은 access token은 사용할 수 있는 시간이 제한되어 있습니다. 
사용하고 있던access token이 유효기간 종료 등으로 만료되면, 새로운 액세스 토큰을 얻어야 하는데 그때 이 refresh token 이 활용됩니다. 
권한 서버가 access token을 발급해주는 시점에 refresh token도 함께 발급하여 클라이언트에게 알려주기 때문에, 
전용 발급 절차 없이 refresh token을 미리 가지고 있을 수 있습니다. 토큰의 형태는 access token과 동일하게 문자열 형태입니다. 
단 권한 서버에서만 활용되며 리소스 서버에는 전송되지 않습니다.

클라이언트가 권한 증서를 가지고 권한서버에 access token 을 요청하면, 
권한 서버는 access token과 refresh token 을 함께 클라이언트에 알려줍니다. 
그 후 클라이언트는 access token을 사용하여 리소스 서버에 각종 필요한 리소스들을 요청하는 과정을 반복합니다. 
그러다가 일정한 시간이 흐른 후 액세스 토큰이 만료되면, 리소스 서버는 이후 요청들에 대해 정상 결과 대신 오류를 응답하게 됩니다. 
오류 등으로 액세스 토큰이 만료됨을 알아챈 클라이언트는, 전에 받아 두었던 refresh token을 권한 서버에 보내어 새로운 액세스 토큰을 요청합니다. 
갱신 요청을 받은 권한 서버는 refresh token 의 유효성을 검증한 후, 문제가 없다면 새로운 액세스 토큰을 발급해줍니다.
이 과정에서 옵션에 따라 refresh token 도 새롭게 발급 될 수 있습니다.

### - Authorization Code와 Implicit는 OAuth 2.0 RFC에 따르면 credential client 즉, 서비스에 접근하는 외부 앱을 위한 방법입니다. 
Authorization Code는 자신만의 서버를 갖고 동작하는 서비스일 경우, 
Implicit는 백엔드 서버 없이 순수 클라이언트에서만 동작하는 앱일 경우에 적당한 방법입니다. 
Implicit이 client id가 하드코딩 되어 있기 때문에 약간의 보안상 불이익이 있다는 점을 제외하면 Authorization Code와 비슷한 방식이라 할 수 있습니다.
Authorization Code와 Implicit는 3rd-party를 위한 인증방식을 제공하기 때문에 우린처럼 1st-party 인증 서비스로 적당하지 않습니다.

Client Credentials는 사용자 인증을 받아야 하는 입장에서 논의될 대상이 아닙니다. 
만약 사용자 인증 및 권한부여가 아니라, 해당 앱이나 서비스 자체에 인증 및 권한부여를 해야 한다면 놀랍도록 아름다운 방법일 수 있겠으나 
그러한 서비스가 필요한 경우 Firebase와 같은 PaaS 서비스를 사용하지 않는 이유가 무엇인지 고민해 봐야 할 듯 합니다.

마지막으로 남은 방법은 Resource Owner Password Credentials 입니다. 
OAuth 2.0 스펙 문서를 보면 id와 password를 사용해서 access token을 발급받을 수 있습니다. 
자신의 Authorization Server(권한 서버)에 접속해서 인증하기 때문에 client id 와 secret을 하드코딩 하게 됩니다. 
그렇기 때문에 보안상 위험이 있을 수 있으나, access token을 발급받기 위해서 사용자 id와 password도 동시에 필요하기 때문에 
보안상 위험을 상호 보안 할 수 있습니다. 
OAuth 2.0 문서에 Resource Owner Password Credentials의 경우 access token을 발급받은 이후에 id와 password를 삭제할 것을 권고하고 있습니다.

### grant_type 상세설명
http://jsonobject.tistory.com/369
