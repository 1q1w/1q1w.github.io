---
layout: single
title: "[HTTP&NETWORK] HTTP 인증 방식"
categories:
  - HTTP&NETWORK
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-06-18
---



이번 시간에는 HTTP 인증 방식에 대해 알아보자

-----------

#### 1.  BASIC 인증

BASIC 인증은 HTTP/1.0에 구현된 인증 방식으로 현재도 일부 사용되고 있다.  BASIC 인증은 웹 서버와 대응하고 있는 클라이언트 사이에서 이뤄지는 인증 방식이다.  절차는 다음과 같다.

1) 클라이언트가 리퀘스트 송신

2) BASIC 인증이 필요한 리소스가 있는 경우 서버는 상태코드 401 Authorization Required와 함꼐 인증의 방식(BASIC), Request-URI의 보호 공간을 식별하기 위한 문자열(realm)을 WWW-Authenticate 헤더 필드에 포함시켜 리스폰스를 반환

3) 상태 코드 401을 받은 클라이언트는 BASIC 인증을 위해 유저ID와 패스워드를 서버에 송신해야 함.  송신하는 문자열은 유저ID와 패스워드를 콜론 ':' 으로 연결한 문장을 Base64라 불리는 형식으로 인코드한 것.  예를 들어 ID가 guest이고 패스워드가 guest인 경우 guest:guest 문자열이 되는데 이를 Base64 인코드하면 Z3Vlc3Q6Z3Vlc3Q= 이 된다.  이 문자열을 Authorization 헤더 필드에 포함해서 리퀘스트를 송신.  유저 에이전트에 브라우저를 사용하는 경우 이용자가 ID와 패스워드를 입력하면 브라우저가 자동적으로 Bast64로 변환

4) AUthorization 헤더 필드를 포함한 리퀘스트를 수신한 서버는 인증 정보가 정확한지 여부를 판단.  인증 정보가 정확하면 Request-URI 리소스를 포함한 리스폰스를 반환



BASIC 인증에서 Base64 인코딩 형식을 하고 있지만 이는 암호화가 아니기 때문에 쉽게 복호화가 가능하다.  즉 HTTPS 등에서 암호화되지 않은 통신 경로 상에서 BASIC 인증을 해서 도청된 경우 ID와 패스워드를 뺏길 수 있다.  또한 한번 BASIC 인증을 하면 일반 브라우저에서는 로그아웃 할 수 없다는 문제도 있다.  BASIC 인증은 사용상의 문제와 많은 웹에서 요구되는 보안 등급에 미치지 못한다는 면에서 많이 사용되지 않고 있다.





#### 2. DIGEST 인증

BASIC 인증의 약점을 보완하며 HTTP/1.1에서 소개된 DIGEST 인증은 챌린지 리스폰스 방식이 사용되고 있어 BASIC 인증과 같이 패스워드를 그대로 보내는 일은 없다.  **챌린지 리스폰스** 방식은 최초에 상대방에게 인증 요구를 보내고 상대방 측에서 받은 챌린지 코드를 사용해 리스폰스 코드를 계산한다.  이 값을 상대에게 송신하여 인증을 하는 방식이다.  리스폰스 코드라는 패스워드와 챌린지 코드를 이용해서 계산한 결과를 상대에게 보내기 때문에 BASIC 인증에 비해 패스워드가 누출될 가능성이 줄어들게 된다.  절차는 다음과 같다.

1) 클라이언트가 리퀘스트 송신

2) 인증이 필요한 경우 서버는 상태코드 401 Authorization Required와 함께 챌린지 리스폰스 방식의 인증에 필요한 챌린지 코드(nonce)를 WWW-Authenticate 헤더 필드에 포함해서 리스폰스를 반환한다.  WWW-Authenticate 헤더 필드에 반드시 포함되어야 하는 정보는 'realm'과 'nonce' 두 개이다.  nonce는 401 리스폰스를 반환할 때마다 생성되는 유일한 문자열이다.  이 문자열은 Base64이거나 16진수가 권장되고 있으며, 문자열의 내용에 대해서는 구현된 서버에 의존하고 있다.

3) 상태코드 401을 수신한 클라이언트는 DIGEST 인증을 위해 필요한 정보를 Authorization 헤더 필드에 포함해서 리스폰스를 반환한다.  이 때 반드시 포함되어야 하는 정보는 'username', 'realm', 'nonce', 'uri', 'response' 이다.  이 중 'realm', 'nonce'는 서버에서 받은 것을 사용한다.  username은 지정된 realm에서 인증 가능한 사용자 이름이다.  uri는 Request-URI에 있는 URI지만 프록시에 의해 변경되는 경우도 있기 때문에 복사해서 기록한다.  response는 Request-Digest라 불리는데 패스워드 문자열을 MD5로 계산한 것으로 이것이 리스폰스 코드이다.

4) Authorization 헤더 필드를 포함한 리퀘스트를 받은 서버는 인증 정보가 정확한지를 판단한다.  정확한 경우 Request-URI의 리소스를 포함한 리스폰스를 반환한다.



DIGEST 인증은 BASIC 인증에 비해 높은 보안 등급을 제공하지만, HTTPS 클라이언트 인증 등과 비교하면 낮은 보안 등급이다.  패스워드의 도청을 방지하기 위한 보호 기능은 제공하지만 이 외에 위장을 방지하는 기능은 제공하지 않는다.  BASIC 인증과 마찬가지로 사용상의 문제와 웹에서 요구되는 보안 등급에 미치지 못한다는 점에서 많이 사용되지 않고 있다.





#### 3. SSL 클라이언트 인증

ID와 패스워드를 사용한 인증은 이 두 정보가 정확하다면 본인으로 인증할 수 있다.  그러나 이 정보가 도난되면 제3자가 위장을 하는 경우가 있다.  이를 방지하기 위한 대책의 하나로 SSL 클라이언트 인증이 사용되는 경우가 있다.  SSL 클라이언트 인증은 HTTPS의 클라이언트 인증서를 이용한 인증 방식이다.  절차는 다음과 같다.

SSL 클라이언트 인증을 할 떄는 사전에 클라이언트에 클라이언트 증명서를 배포하고 인스톨 해두어야 한다.

1) 인증이 필요한 리퀘스트가 있으면 서버는 클라이언트 증명서를 요구하는 'Certificate Request'라는 메시지를 송신

2) 유저는 송신하는 클라이언트 증명서를 선택하고 'Client Certificate'라는 메시지를 송신

3) 서버는 클라이언트 증명서를 검증하여 결과가 정확하면 클라이언트의 공개키를 취득.  그 이후에 HTTPS에 의한 암호를 개시



SSL 클라이언트 인증은 대부분 단독으로 사용되지 않고 폼 베이스 인증과 합쳐 2-factor 인증의 하나로 이용되고 있다.  2-factor 인증이란 예를 들면 패스워드라는 한 개의 요소만이 아닌 이용자가 가진 다른 정보를 병용해서 인증을 하는 방법이다.  즉 첫 번째 인증 정보로서 SSL 클라이언트 인증을 사용하여 클라이언트의 컴퓨터를 인증하고 다른 인증 정보로 패스워드를 사용하여 본인 확인을 하는 방식이다.

SSL 클라이언트 인증은 클라이언트 증명서를 이용해야 한다.  이를 위해 비용이 필요해서 중요 정보가 아니거나 개인 웹 사이트에서는 사용하기 어려운 면이 있다.





#### 4. 폼 베이스 인증

폼 베이스 인증은 HTTP 프로토콜 사양으로 정의된 인증 방식은 아니다.  클라이언트가 서버 상의 웹 애플리케이션에 자격 정보를 송신하여 그 자격 정보의 검증 결과에 따라 인증을 하는 방식이다.  이것은 웹 애플리케이션에 따라 제공되는 인터페이스나 인증 방법이 다양하다.  대부분의 경우 사전에 등록해 둔 자격 정보인 유저 ID와 패스워드를 입력해서 이것을 웹 애플리케이션측에 송신하고 검증 결과를 토대로 검증 성공 여부를 결정한다.

HTTP가 표준으로 제공하는 BASIC 인증이나 DIGEST 인증은 사용상의 문제와 보안상의 문제로 거의 사용되지 않는다.  또한 보안 등급이 높은 SSL 클라이언트 인증도 도입 비용이나 운용 비용 등의 문제로 널리 사용되지 못하고 있다.  웹 사이트의 인증 기능으로서 표준적인 것이 존재하지 않아 각각의 웹 애플리케이션에서 구현하는 폼 베이스 인증을 채용하는 경우가 많다.  안전한 방법으로 구현하면 높은 보안 등급을 유지할 수 있지만 구조적인 문제가 있는 웹 사이트도 종종 발견할 수 있다.

폼 베이스 인증은 사양이 정해져있지 않지만 일반적으로 세션 관리를 위해 쿠키를 사용한다.   폼 베이스 인증은 클라이언트가 송신해온 ID와 패스워드가 사전에 등록된것과 일치하는지 검증하면서 시작된다.  HTTP는 stateless 프로토콜로 방금 전에 인증을 했던 유저의 상태를 유지시킬 수 없다.  따라서 다음에 그 유저가 액세스해도 다른 유저와 구별하지 못한다.  그래서 세션 관리와 쿠키를 사용해 HTTP에 없는 상태 관리 기능을 보충한다.  절차는 다음과 같다.

1) 서버에 ID와 패스워드 등의 자격 정보를 포함한 리퀘스트를 송신.  보통은 POST 메소드가 사용되어 엔티티 바디에 자격 정보를 저장.  이 때 HTML 폼 화면 표시와 입력 데이터의 송신에는 HTTPS 통신을 이용

2) 서버는 인증된 유저를 식별하기 위해 **세션ID**를 발행.  클라이언트에서 수신한 자격 정보를 검증하는것으로 인증을 하고 그 유저의 인증 상태를 세션ID와 연관지어 서버 측에 기록.  클라이언트 측에 송신할 떄는 Set-Cookie 헤더 필드에 세션ID를 저장해서 리스폰스를 반환.  세션ID는 다른 유저와 구별하기 위한 문자열.  세션ID는 도난당하거나 유추되지 않도록 해야할 필요가 있어 어려운 문자열을 사용하고 서버 측에서 유효 기한을 관리하는 등 보안을 유지할 필요가 있다.  또한 크로스사이트스크립팅 등의 취약성이 존재하는 경우 피해를 줄이기 위해 쿠키에 httponly 속성을 부여해두는것이 좋다.

3) 서버 측에서 세션ID를 받은 클라이언트는 쿠키로 해당 세션ID를 저장.  다음 번에 서버에 리퀘스트를 송신하는 경우 브라우저가 자동으로 쿠키를 송출하기 때문에 세션ID가 서버에 송신되게 됨

위 절차는 하나의 예이며 다른 방법으로 구현된 것도 있다.  폼베이스 인증에서 자격정보를 교환하는 방법은 표준화되어있지 않을 뿐만 아니라 패스워드 등의 자격정보를 서버 측에 어떻게 보존해야 하는지도 표준화되어있지 않다.  일반적으로 salt라는 부가 정보를 사용해서 해시 알고리즘으로 계산한 값을 저장하지만 평문의 패스워드를 서버에 그대로 보존하고 있는 곳도 종종 있다.  이런 경우 패스워드가 누출될 위험이 있다.
