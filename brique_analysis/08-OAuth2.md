# OAuth2

* **O**pen **Auth**entication (인증)
* **O**pen **Auth**orization (인가)
---

## 1. Overview
> OAuth란,  
자사가 아닌 Partner 서비스에게, 자사 서비스 사용자의 인증을 거쳐서 API의 접근 권한을 전달하는 방식의 대표적인 구현체

OAuth를 이해하기 위해서 알아야 할 개념은 다음과 같음
- REST API 서비스
- Authentication (인증)
- Authorization (인가)
---
### 1.1 REST API
**REST(REpresentational State Transfer)란,**
- 엄격한 의미로 REST는 N/W Architecture의 원리 모음으로, 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법을 말하며,
- 장비간 통신을 위해 CORBA, RPC, SOAP 등의 복잡한 방법을 사용하는 대신, 간단하게 HTTP를 이용하는 것을 목적으로 하고,
- 자원 지향 구조(Resource Oriented Architecture)로 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 URI를 부여하는 것

**REST API 서비스**
- Internet Contents를 소비하기 위한 장비 및 프로그램의 증가와
- 다양한 Client(IOS, Android, 브라우저, etc.)가 존재하는 상황에서,  
하나의 Client에 대응하기 위한 서버를 구성하는 건 비효율적인 일이며,  
**_하나의 서버로 여러 대의 Client를 대응하도록 할 때 필요한 것이 RESTful API_**
---
### 1.2 Security
REST API 서비스에 대한 보안은 다양한 관점에서 고려해야 하며,  
보안을 이해하기 위해서 알아야 할 개념은 다음과 같음
- Authentication (인증)
- Authorization (인가)
- N/W Level Encryption (네트워크 레벨 암호화)
- Message Integrity (메시지 무결성 보장)
- Message Encryption (메시지 본문 암호화) 
---
**1.2.1. Authentication (인증)**
- 누가 서비스를 사용하는지를 확인하는 절차로, 웹 사이트에 사용자 ID와 비밀번호를 이용해서 사용자를 확인하는 과정을 예로 들 수 있으며,
- API도 마찬가지로 API를 호출하는 대상 (단말기, 다른 서버, 사용자)을 확인하는 절차가 필요하고 이를 API 인증이라 정의함

인증 방식은 다음과 같이 다양한 방법이 존재하며,
- API Key 방식
- API Token 방식
- HTTP Basic Auth
- Digest access Authentication
- Client 인증
- IP White List를 이용한 Tunneling
- Bi-directional Certification (Mutual SSL)
- 제 3자 인증

각 인증방식을 좀더 세부적으로 알아보면,

**API Key 방식은,**
>가장 기초적인 방법으로 API Key란 특정 사용자만 알 수 있는 일종의 문자열로 구성되며,  
>API를 사용하고자 하는 개발자는 API 제공사의 Portal 페이지로부터 API Key를 발급 받고, API를 호출할 때 API Key를 메시지 안에 넣어서 호출하는 방식으로,  
>서버는 메시지 안에서 API Key를 읽어 이 API가 누가 호출한 API인지를 인증하는 방식

**API Token 방식은,**
>사용자 ID와 비밀번호로 사용자를 인증한 후, 정해진 기간 동안만 유효한 API Token을 발급하고, 그 API Token으로 사용자를 인증하는 방식으로,  
>사용자 비밀번호는 주기적으로 바뀔 수 있고, 일반적으로 사용자들은 다른 서비스에도 동일한 비밀번호를 사용하기 때문에, 비밀번호 노출 시 다른 서비스까지 Hacking 당할 위험에 대한 가능성을 최소화하기 위함

![API Token 이미지]()

**HTTP Basic Auth**
>가장 기본적이고 단순한 형태의 인증 방식으로, ID와 비밀번호를 HTTP Header에 Base64 Encoding 형태로 넣어서 인증을 요청하는 방식으로,  
>중간에 Packet을 가로채서 Header를 Base64로 Decoding하면 ID와 비밀번호가 그대로 노출되기 때문에 HTTPS 프로토콜을 사용해야 함

**Digest Access Authentication**
>HTTP Basic Auth가 Base64 형태로 비밀번호를 보내는 단점을 보강하여 나온 인증 Protocol로써,  
>Client가 인증을 요청할 때, 서버로부터 nonce라는 일종의 난수 값을 받은 후, 사용자 ID와 비밀번호를 이 난수 값을 이용해서 Hash화 하여 서버로 전송하는 방식

![Digest Access 이미지]()

**Client 인증**
>추가적인 보안 강화를 위해서 사용자 인증 뿐만 아니라, Client 인증 방식을 추가하는 것으로,  
>Facebook의 경우, API Token을 발급 받기 위해서, 사용자 ID와 비밀번호 뿐만 아니라 Client ID와 Client Secret을 같이 입력 받도록 하며,  
>Client ID는 특정 어플리케이션이나 앱에 대한 등록ID이고, Client Secret은 Facebook 개발자 포털에서 앱을 등록하면 앱 별로 발급되는 일종의 비밀번호임

**IP White List를 이용한 Tunneling**
>API를 호출하는 Client의 API가 일정한 경우에 사용할 수 있는 손쉬운 방법으로, 특정 API URL에 대해 들어오는 IP 주소를 White List로 유지하는 방식으로,  
>API Server 앞 단에, HAProxy나 Apache 같은 웹 서버를 배치하고, IP List를 제한 하거나, H/W 방화벽 자체에 들어올 수 있는 IP List를 제한함  
>설정만으로 가능한 방법으로, 서버 간의 통신이 있는 경우 권장됨

**Bi-directional Certification (Mutual SSL)**
>가장 높은 수준의 인증 방식을 제공할 수 있는 개념으로,  
>HTTPS 통신을 사용할 때 서버에 공인 인증서를 놓고 단방향 SSL을 제공하는 것이 일반적이나,  
>Client에도 인증서를 놓고 양방향으로 SSL을 제공하면서 API 호출에 대한 인증을 Client의 인증서를 이용하는 방식으로,  
>일반 서비스에는 사용되지 않으며, 높은 인증 수준을 제공하는 몇몇 서비스나 특정 서버 간 통신에 사용하는 것을 권장함

**제 3자 인증**
>Facebook이나 Twitter 같은 API 서비스 제공자들이 Partner 어플리케이션에 많이 적용하는 방법으로,  
>사용자가 만든 어플리케이션(서비스)을 Facebook 계정을 이용해서 인증하는 방식  
>자사가 아닌 Partner 서비스에게 자사 서비스 사용자의 인증을 거쳐서 API의 접근 권한을 전달하는 방식으로,  
**_이러한 인증 방식의 대표적인 구현체가 바로 OAuth2 이며_**, 제 3자 인증 뿐만 아니라, 자사의 어플리케이션을 인증하기 위해, Client로부터 직접 ID와 비밀번호를 받는 등, Client 유형에 따라 다양한 시나리오를 제공

---

**1.2.2. Authorization (인가)**

인증이 끝나면 다음 단계는 권한에 대한 인증, 즉 인가 과정이 필요한데,  
사용자가 인증(Authentication)을 통해서 시스템 내의 사용자 임을 확인 받았지만, API 호출을 위해서 적절한 권한이 있는지에 대한 검증 과정을 의미함

API 권한 인가 방식은 다음의 두 가지로 나뉘며,

- RBAC (Role Based Access Control)
- ACL (Access Control List)

API 권한 인가를 처리하는 위치는 다음과 같음
- Client에 의한 권한 인가 처리
- Gateway에 의한 권한 인가 처리
- API 서버에 의한 권한 인가 처리

**RBAC(Role Based Access Control) 는,**
>사용자의 Role을 기반으로 이뤄지며, 정해진 Role에 권한을 연결해 놓고, 이 Role을 가진 사용자에게 해당 권한을 부여하는 방식

예를 들어,
- 일반관리자 - 사용자관리, 게시물 관리, 회원가입 승인
- 마스터 관리자 - 게시판관리, 메뉴관리, 사용자관리, 게시물관리, 회원가입승인

과 같은 Role을 생성한 후, 사용자에게 “마스터 관리자”라는 Role을 부여하는 경우가 이에 해당함

![RBAC 이미지]()

**ACL(Access Control List) 은,**
>RBAC 방식이 권한을 Role이라는 중간 매개체를 통해서 사용자에게 부여하는 방식에 반해서, 사용자 또는 그룹과 같은 권한 부여대상에게 직접 권한을 부여하는 방식

예를 들어, 사용자에게 직접 “사용자관리, 게시물관리, 회원가입승인＂ 권한을 부여하는 경우가 이에 해당함

![ACL 이미지]()

**Client에 의한 API 권한 인가 처리는**
>API를 호출하는 Client 쪽에서 사용자의 권한에 따라 API를 호출하는 방식으로, Client가 신뢰할 수 있는 경우에만 사용되며,  
>기존의 WEB UX 로직이 서버에 배치되어 있는 형태 (Struts나 Spring MVC와 같은 WEB Layer가 있는 경우)에 주로 사용함

사용자 API를 예로 들면,  
사용자 로그인 정보(세션 정보)를 보고 사용자 정보를 조회한 후에 API를 호출하는 방식이며,  
Mobile Device 등에 제공하는 API는 사용자 Role을 갖는 API와 같이 별도의 권한 인가가 필요 없는 API를 호출하는 구조를 갖게 됨

![Client에 의한 권한인가 이미지]()

**Gateway에 의한 API 권한 인가 처리**
>API 호출이 들어오면 API Access Token을, API Token Management 정보를 이용해서 사용자 정보와 권한 정보로 변환한 후, 접근하고자 하는 API에 대해서 권한 인가를 처리하는 방식으로 API 별로 접근하고자 하는데 필요한 권한을 체크

HTTP GET /users/{id}의 API를 예로 들면,  
- 이 URL에 대한 API를 호출하기 위해서는 일반 사용자 권한을 가지고 있는 사용자의 경우, 호출하는 ID와 URI상의 {id}가 일치할 때 호출을 허용하고, 같지 않을 때는 호출을 불허해야 함
- 만약 사용자가 관리자 권한을 가진 경우, 호출하는 사용자 ID와 URL상의 {id}가 일치하지 않더라도 호출을 허용해야 함

- API에 따라 사용자 ID나 권한 인증에 필요한 정보가 URL이 아닌, HTTP Body의 JSON 형태나 HTTP Header 등에 들어가 있는 경우, 별도의 권한 통제 로직을 Gateway 단에서 구현해야 하는 부담과 HTTP 메시지 전체를 일일이 Parsing해야 하는 오버로드가 발생하기 때문에, 공통 필드 등으로 API 권한 처리를 하지 않는 경우, 사용하기가 어려운 방식임

![Gateway에 의한 권한인가 이미지]()

**API 서버에 의한 API 권한 인가 처리**
>장 일반적이고 보편적인 방법으로, API 요청을 처리하는 API 서버의 비즈니스 로직 단에서 권한 처리를 하는 방식으로, 
>API Gateway 방식과 비교했을 때, 각 비즈니스 로직에서 API 메시지를 각각 Parsing 하기 때문에, API 별로 권한 인가 로직을 구현하기가 용이하며,
>권한 인가에 필요한 필드들을 API Gateway에서 변환해서 API 서버로 전달해 주는 경우, 구현을 더욱 간략하게 할 수 있음

예를 들어, API Access Token을 이용해서 API를 호출하는 경우, 
- API Gateway가 Access Token을 권한 인가에 필요한 사용자 ID, Role 등으로 변환 후, API 서버에 전달하게 되면,
- 각 비즈니스 로직은 권한 인가에 필요한 사용자 정보 등을 별도로 DB를 뒤지지 않고 이 헤더의 내용만으로 API 권한 인가를 처리할 수 있음

![API Server에 의한 권한인가 이미지]()

---

**1.2.3. N/W Level Encryption (네트워크 레벨 암호화)**
- 인증과 인과 과정이 끝나고 API를 호출하게 되면, N/W을 통해서 데이터가 송수신 되는데, 이를 N/W Protocol 단에서 암호화 처리
- HTTP에서의 N/W Level 암호화는 일반적으로 HTTPS 기반의 보안 프로토콜을 사용

**1.2.4. Message Integrity (메시지 무결성 보장)**
- 메시지가 중간에 Hacker와 같은 외부 요인에 의해 변조되지 않게 방지하는 것
- 무결성 보장을 위해 메시지에 대한 Signature를 생성해서 메시지와 같이 보낸 후에 검증하는 방식을 주로 사용
- N/W Level 암호화를 완벽하게 하는 경우, 사용할 필요성이 없음

**1.2.5. Message Encryption (메시지 본문 암호화)**
- N/W Level Encryption을 사용할 수 없거나, N/W Level의 암호화를 신뢰할 수 없는 경우, 추가적으로 메시지 자체를 암호화 하는 방법
- 어플리케이션 단에서 전체 메시지 또는 특정 필드만 암호화 하는 두 가지 방법으로 접근

---

## 2. OAuth2

>OAuth is an open standard for access delegation, commonly used as a way for Internet users to grant websites or applications access to their information on other websites but without giving them the passwords. This mechanism is used by companies such as Amazon, Google, Facebook, Microsoft and Twitter to permit the users to share information about their accounts with third party applications or websites.

### 2.1 Concept
>OAuth 인증을 진행할 때, 해당 서비스 제공자는 ‘제 3자가 어떤 정보나 서비스에 사용자의 권한으로 접근하려 하는데 허용하겠느냐’ 라는 안내 메시지를 보여주는 것

**인증과 허가를 포함하는 제 3자 인증의 대표적인 구현체**
- Authentication: 인증
- Authorization: 허가

예를 들어,  
일반 로그인은 사원이 63빌딩에 출입하는 것이라면 (사원증이 있어야 출입 가능),
OAuth는 1층에서 방문증을 수령한 후 63 빌딩에 출입하는 것 (방문증만 있어도 출입 가능)

**OAuth1 의 특징**
- API 인증 시, Third Party Application에게 사용자의 비밀번호를 노출하지 않고 인증 할 수 있다는 점
- 인증(Authentication)과 API 권한(Authorization) 부여를 동시에 할 수 있다는 점

**OAuth1의 동작방식**
- OAuth1은 기본적으로 User / Consumer / Service Provider가 있어야 함
- OAuth1 인증을 3-legged OAuth 라고도 하는데 결국 주체가 셋 이라는 말

![OAuth1 동작 이미지]()

**OAuth1 프로세스**
1. 사용자가 Twitter 로그인 요청
2. 사용자를 Twitter (Service Provider) 로그인 화면으로 Redirect
3. Twitter 로그인 진행
4. 서비스 (Consumer)로 인증 토큰 (Access Token)이 전달

![OAuth1 Process 이미지]()

**인증 토큰의 장점**
- 사용자 ID와 비밀번호를 몰라도 토큰을 통해 허가 받은 API에 접근 가능
- 필요한 API에만 제한적으로 접근할 수 있도록 권한 제어 가능
- 저장되어 있는 인증 토큰이 유출되더라도 Twitter의 관리자 화면에서 인증 토큰의 권한 취소 가능
- 사용자가 Twitter(Service Provider)의 비밀번호를 변경해도 인증 토큰은 계속 유효

**OAuth2 의 개선사항**
- OAuth1.0과 호환되지 않으며 용어부터 많은 것이 다름
- 모바일에서의 사용성 문제나 서명과 같은 개발이 복잡하고 기능과 규모의 확장성 등을 지원하기 위해 만들어진 표준으로, 표준이 매우 크고 복잡해서 이름도 “OAuth2.0 Authorization Framework”

**OAuth1에서 개선된 사항**
- 용어변경
>- Resource Owner: 사용자
>- Resource Server: REST API 서버
>- Authorization Server: 인증서버 (API 서버와 같을 수 있음)
>- Client: Third Party Application (서비스)
- 간단하고 직관적
>- OAuth1에서는 HTTPS가 필수
>- Signature 없이 생성 및 호출 가능
>- URL 인코딩이 필요 없음
- 더 많은 인증 방법을 지원
>- 이전에는 HMAC을 이용한 암호화 인증만 지원
>- OAuth2은 여러 인증 방식을 통해 WEB / Mobile등 다양한 시나리오에 대응 가능
>- Access Token의 Life Time을 지정하여 만료일 설정 가능
- 대형 서비스로의 확장성 지원
>- 커다란 서비스는 인증 서버를 분리하거나 다중화 할 수 있어야 함
>- Authorization Server의 역할을 명확히 하여 이에 대한 고려가 되었음

**OAuth2를 사용하는 서비스**
- 2013년 까지만 해도 1.0만 지원하거나 2.0으로 개선하는 인터넷 서비스 기업이 많았지만,
현재는 대부분 2.0만 지원한다고 봐도 무방함 (1.0은 자체 로그인에만 사용하는 기업이 많음)
- Facebook, Instagram, Google, LinkedIn, Twitter, …

---

### 2.2 Compose
OAuth2는 다음의 4가지 항목으로 구성됨
- Resource Server
- Resource Owner
- Authorization Server
- Client

**Resource Server는,**
>보호된 정보를 제공하는 서버(웹 어플리케이션)을 의미하며, 자원서버 또는 API Server가 이에 해당함

**Resource Owner는,**
> Resource Server에 계정을 가지고 있는 사용자로, Client가 그들의 계정을 통해 Resource Server에 접근하는 것을 인가(Authorize)함

**Authorization Server는,**
> Client가 Resource Server의 서비스를 사용할 때 사용되는 Access Token(접근 토큰)을 발행하는 인가 서버에 이에 해당함

**Client는,**
> Resource Owner를 대신하여 Resource Server의 서비스를 사용하고자 하는 Application 또는 Third Party Application이 이에 해당함

---

### 2.3 Process

Facebook을 인용해서, OAuth의 사용 절차를 살펴보면,
- Resource Owner (Facebook 사용자)
- Authorization Server (Facebook Authorization Server)
- Resource Server (Facebook REST API Server)
- Client (Facebook REST API Server를 사용하는 Application)

![OAuth2 Process 이미지]()

1. 사용자가 어떤 사이트를 이용해보려고 하는데, 아이디를 Facebook으로 가입할 수 있다는 버튼을 발견
2. 버튼을 누르면 Facebook 로그인 창이 나오고, 로그인을 수행. (1 ~ 2번에 해당)
3. 로그인 후, 해당 사이트의 접근을 허용할 것인가? 라는 확인 창 표시 (3번에 해당)
4. 허용을 하게 되면 해당 사이트에서 로그인 목적으로 사용할 수 있는 Access Token 발급 (4번에 해당)
5. Access Token의 id와 access_token으로 서버의 제한된 Resource를 expiration_date까지 사용 (5 ~ 6번에 해당)

---

### 2.4 Authorization Grant Type
> Client가 Access Token을 요청하기 위해서는 Resource Owner의 인가를 받아야 하는데 OAuth는 서로 다른 용도를 위해 다음의 4가지 Authorization Grant Type을 제공

- Authorization Code
- Implicit
- Resource Owner Password
- Client Credentials

**2.4.1. Authorization Code (인가 코드 승인)**
> 이 방식은 HTTP Redirection 기반의 흐름으로, Client는 Resource Owner의 웹 브라우저와 상호 작용할 수 있어야 하며, Authorization Server로부터의 요청을 처리할 수 있어야 함

![Authorizatin Code Type 이미지]()

1. Client가 User Agent를 Auth Server로 Redirect 함으로써 흐름이 시작 (State 값과 Auth Server로부터 응답을 수신할 Redirect URI 포함)
2. 유효한 인증 이력이 없는 인증 요청에 대해 Auth Server는 로그인 페이지로 응답
3. Resource Owner는 Auth Server의 인증을 통해 Client를 인가
4. Auth Server는 정상적인 Resource Owner의 인증/인가 요청을 Client로 Redirect (인가 코드와 상태 값 포함)
5. 인가 코드를 포함한 요청을 받은 Client는 Auth Server로 Access Token 요청을 전송하여 그 결과로 Access Token을 획득 (인가 코드와 Client 인증 정보 포함)

> 이 방식은 Client의 인증 과정을 거치고 Auth Server와 Client 간에만 Access Token이 포함된 통신을 수행하는 등 보안 상 많은 이점을 포함하고 있음

**2.4.2. Implicit (암시적 승인)**
> 브라우저에서 Java script와 같은 Script 언어로 동작하는 Client들을 지원하기 위한 유형으로, 웹 브라우저의 신뢰도가 높고, 신뢰할 수 없는 사용자나 애플리케이션에 노출될 염려가 적을 때 사용 (모바일 앱 또는 단말기에서 동작하는 웹 애플리케이션에 주로 사용)

![Implicit Type 이미지]()

1. Client가 User Agent를 Auth Server로 Redirect 함으로써 흐름이 시작 (State 값과 Auth Server로부터 응답을 수신할 Redirect URI 포함)
2. 유효한 인증 이력이 없는 인증 요청에 대해 Auth Server는 로그인 페이지로 응답
3. Resource Owner는 Auth Server의 인증을 통해 Client를 인가
4. Auth Server는 정상적인 Resource Owner의 인증/인가 요청에 Access Token으로 응답
5. Client는 Access Token을 획득

> 이 방식은 재발급 Token을 사용할 수 없기 때문에 Access Token이 만료되면 Client는 접근이 필요한 경우, 승인 흐름을 다시 진행해야 함

**2.4.3. Resource Owner Password (자원 소유자 패스워드 승인)**
> 이 방식은 사용자 이름과 비밀번호를 Access Token으로 교환함

![Resource Owner Password Type 이미지]()

1. Client는 사용자에게 사용자 인증 정보를 요청
2. 입력 받은 사용자 인증 정보와 Client 정보를 포함하여 Auth Server로 Access Token을 요청
3. Auth Server는 정상적인 Resource Owner의 인증/인가 요청에 대해 Access Token으로 응답

> 이 방식은 Resource Owner의 비밀번호가 Client에 노출되기 때문에 다른 유형의 승인 방식을 사용할 수 없는 경우에만 사용해야 함

**2.4.4. Client Credentials (클라이언트 인증 정보 승인)**
> 이 방식은 Client가 데이터를 소유하고 있어서 Resource Owner에게 접근을 위임 받을 필요가 없거나, 외부에서 애플리케이션에 접근 위임이 이미 허용되었을 경우에 사용 (사용자와 관계없는 애플리케이션 API 접근 시에 주로 사용)

![Client Credentials Type 이미지]()

1. Client는 Client 인증 정보를 담은 요청을 Auth Server로 전송
2. Auth Server는 정상적인 Client 인증/인가 요청에 대해 Access Token으로 응답

> 이 방식은 정확한 사용 사례에 따라 사용되어야 하며, 고도의 비밀 유지가 필요한 Client를 인증하는 것은 매우 위험하므로, 이런 경우에는 인증 정보를 규칙적으로 바꿔줘야 함

---

### 2.5 Token Store 종류

OAuth2에서 사용되는 Token은 다음과 같은 방식으로 저장될 수 있으며,

- In-Memory Token Store
- JDBC Token Store
- JWT Token Store
- Redis Token Store

**2.5.1. In-Memory Token Store**
> JAVA 내부에서 Map, Queue 구조의 메모리를 사용하는 저장소 (기본)

**2.5.2. JDBC Token Store**
> DBC를 사용해서 DB에 저장하는 방식

**2.5.3. JWT Token Store**
> 외부저장소가 아닌 JWT(JSON Web Token)를 이용하는 방식으로, Token에 JSON정보를 인코딩하여 저장함

**Auth2의 문제점**
- API를 호출할 때 마다 Access Token이 유효한지 실제 OAuth 서버를 통해 검증
- 매번 OAuth에서 해당 토큰의 만료여부 등을 DB에서 조회하고, 새로 갱신 시 업데이트 작업을 수행

**Claim 기반의 Token**
- OAuth 서버에서 수행하는 Access Token의 검증과정에 대한 부담을 줄이기 위해,
- Token 자체에 만료일 및 사용자에 대한 추가정보 (계정) 등을 저장하는 방식

**JWT란,**
- Claim 기반 방식을 사용하는 것으로,
- Claim은 사용자에 대한 Property 등을 지칭하며, Token 자체가 정보를 가지는 방식으로, JWT는 Claim을 JSON을 이용해서 정의하며,
- Token의 변조 방지, 즉 무결성(Integrity) 보장을 위한 서명방식 및 서명을 포함하는 구조를 가짐

**JWT Token의 구조를 살펴보면,**  
마침표(.)를 구분자로 다음의 세 부분으로 나뉘며,
- JOSE Header (JSON Object Signing and Encryption)
- JWT Claim Set
- Verify Signature
> {서명 방식을 정의한 JSON을 BASE64 인코딩}.{JSON Claim을 BASE64 인코딩}.{JSON Claim에 대한 서명}과 같음

**2.5.4. Redis Token Store**
> Redis(Key Value 저장소)에 Token 정보를 저장하는 방식

---

### 2.6 Case Study ###

1. Spring.io를 이용한 프로젝트 생성 [(https://start.spring.io)](https://start.spring.io/)
2. Maven project 및 dependency 선택 Web, actuator, security
3. 생성된 Maven project import
4. Oauth2관련 Dependency 는 별도로 추가
```
[OAuth 관련 Dependency]
<dependency>
	<groupId>org.springframework.security.oauth</groupId>
	<artifactId>spring-security-oauth2</artifactId>
</dependency>
```

5. application.properties 설정
```
spring.application.name=api
management.context-path=/admin

security.user.name=user
security.user.password=password
```

6. REST API를 위한 Controller 생성
```
@RestController
public class APIController {
  @RequestMapping("/")
  public String index() {
  return "Hello World";
  }
}
```

7. 인증 설정을 위한 Configuration Class 생성
```
@Configuration
@EnableAuthorizationServer
public class OAuthServerConfig extends AuthorizationServerConfigurerAdapter {
	@Autowired
    private AuthenticationManager authenticationManager;

	@Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
    endpoints.authenticationManager(authenticationManager);
    }

	@Override
    public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
    	security.checkTokenAccess("isAuthenticated()");
    }

	@Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
    	// @formatter:off
        clients.inMemory()
        .withClient("my-trusted-client")
        .authorizedGrantTypes("authorization_code", "password", "refresh_token", "implicit")
        .authorities("ROLE_CLIENT", "ROLE_TRUSTED_CLIENT")
        .scopes("read", "write", "trust")
        .resourceIds("oauth2-resource")
        .accessTokenValiditySeconds(600)
        .and()
        .withClient("my-client-with-registered-redirect")
        .authorizedGrantTypes("authorization_code")
        .authorities("ROLE_CLIENT")
        .scopes("read", "trust")
        .resourceIds("oauth2-resource")
        .redirectUris("http://anywhere?key=value") // "https://www.getpostman.com/oauth2/callback"
        .and()
        .withClient("my-client-with-secret")
        .authorizedGrantTypes("client_credentials", "password")
        .authorities("ROLE_CLIENT")
        .scopes("read")
        .resourceIds("oauth2-resource")
        .secret("secret");
        // @formatter:on
	}
}
```

8. REST API 서비스를 위한 Configuration Class 생성
```
@Configuration
@EnableResourceServer
public class APIServerConfig extends ResourceServerConfigurerAdapter {
	@Override
	public void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests().antMatchers("/", "/admin/**").access("#oauth2.hasScope('read')").anyRequest().authenticated();
	}
}
```

9. 인증 테스트
```
[Client Credentials]
curl -H "Accept: application/json" my-client-with-secret:secret@localhost:8080/oauth/token -d grant_type=client_credentials

[Resource Owner Password (Trusted Client)]
curl -u my-trusted-client: http://localhost:8080/oauth/token -d "grant_type=password&username=user&password=password"

[Authorization Code (인가코드 승인)]
1. http://localhost:8080/oauth/authorize?client_id=my-client-with-registered-redirect&response_type=code
2. curl -u my-client-with-registered-redirect: http://localhost:8080/oauth/token -d "grant_type=authorization_code&code=코드값“
3. curl -H "Authorization: Bearer 인증코드" "http://localhost:8080"
```
