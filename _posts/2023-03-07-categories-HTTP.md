---
title: "[네트워크] HTTP"
excerpt: "응용 계층의 대표적인 프로토콜인 HTTP와 HTTPS에 대해 알아보자"

categories:
  - Network
tags:
  - [HTTP, HTTPS]

permalink: /Network/HTTP

toc: true
toc_sticky: true

date: 2023-03-07
last_modified_at: 2023-03-07
---
<hr>

## 📝 응용계층
먼저 네트워크 모델의 최상위 계층인 응용계층에 대해 알아보자. 응용계층은 최종적으로 사용자와의 인터페이스를 제공하는 계층이다.
사용자가 웹 서핑을 할때에는 웹 브라우저를 사용하고, 메일을 주고 받을 때에는 Outlook과 같은 메일 프로그램을 사용하는 경우를 예로 들 수 있다.
<br><br>
이와같이 응용계층은 이메일, 파일 전송, 웹 사이트 조회 등 어플리케이션에 대한 서비스를 사용자에게 제공하는 계층이다.
이때 어플리케이션은 서비스를 요청하는 측(사용자 측)에서 사용하는 어플리케이션과 서비스를 제공하는 측의 어플리케이션으로 분류된다.

<figure>
  <img src="/assets/images/posts_img/http/layer.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
- 클라이언트: 서비스 요청하는 측 <br>
➡️ 웹 브라우저(ex. Google Chrome), 메일 프로그램(ex. Outlook)
- 클라이언트: 서비스 제공하는 측 <br>
➡️ 웹 서버 프로그램, 메일 서버 프로그램
<br><br>
클라이언트와 서버 모두 응용 계층에서 동작한다.

<hr>

## 📝 HTTP
HTTP(Hypertext Transfer Protocol)은 앞서 배운 응용 계층의 대표적인 프로토콜로, 웹에서 브라우저와 서버 간에 데이터를 주고 받기위한 일종의 약속을 말한다.

HTTP 프로토콜은 일반적으로 TCP/IP 통신 위에서 동작하며 기본 포트는 80번이다.

<hr class="sub">

### ✔️ HTTP 특징

#### 1. 클라이언트 서버 구조
- 클라이언트가 서버에 요청을 보내고, 응답 대기
- 서버는 요청에 대한 결과를 만들어 응답을 보낸다.
- Request Response 구조


#### 2. 무상태 프로토콜(Stateless)
HTTP 프로토콜은 상태가 없는 프로토콜로, 서버가 클라이언트의 상태를 보존하지 않는다. 클라이언트가 이미 필요한 데이터를 다 담아서 요청하기 때문이다.
<br><br> 
상태가 없다는 말은 데이터를 주고 받기 위한 각각의 데이터 요청이 서로 독립적으로 관리된다는 것과 같다. 쉽게 말해 이전 데이터 요청과 다음 데이터 요청이 서로 관련이 없다는 말이다.

>**장점**<br>
- 무상태는 응답 서버를 쉽게 바꿀 수 있기 때문에 무한한 서버 증설이 가능하다.<br> ➡️ 서버의 확장성 높음(스케일 아웃)
- 다수의 요청 처리 및 서버의 부하를 줄일 수 있다.

>**한계**<br>
- 로그인이 필요한 서비스라면 유저의 상태를 유지해야 되기 때문에 브라우저 쿠키, 서버, 세션 토큰 등을 이용해 상태를 유지해야한다.

#### 3. 비연결성(Connectionless)
<figure>
  <img src="/assets/images/posts_img/http/connection.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
TCP&#47;IP의 경우 기본적으로 연결을 유지한다.
요청을 보내지 않더라도 계속 연결을 유지해야하기 때문에 서버 자원이 계속 소모된다.
<figure>
  <img src="/assets/images/posts_img/http/connectionless.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
반면, 비연결성을 가지는 HTTP에서는 실제로 요청을 주고받을 때만 연결을 유지하고 응답을 주고 나면 TCP&#47;IP 연결을 끊는다.
이를 통해 최소한의 자원으로 서버 유지가 가능하게 한다,
<figure>
  <img src="/assets/images/posts_img/http/http.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
HTTP 1.0 기준으로, HTTP는 연결을 유지하지 않는 모델이다.
트래픽이 많지 않고, 빠른 응답을 제공할 수 있는 경우, 비연결성의 특징은 효율적이다.

예를들어, 한 시간 동안 수천 명이 서비스를 사용해도, 실제 서버에는 초당 처리 요청 개수는 수십 개에 불과한 것이다.

하지만, 비연결성은 다음과 같은 한계가 있다.

웹 브라우저로 사이트를 요청하면 HTML뿐만 아니라, 자바스크립트, CSS, 이미지 등 수많은 자원이 함께 다운로드 된다.
이러한 자원들을 각각 보낼 때마다 연결을 끊고 다시 연결하며 반복하는 것은 비효율적이기 때문에 지금은 **HTTP 지속 연결(Persistent Connercions)**로 문제를 해결한다.

<hr class="sub">

### ✔️ HTTP 헤더
#### 1. 표현(Representation) 헤더
표현 헤더는 요청, 응답 둘 다 사용한다.
<figure>
  <img src="/assets/images/posts_img/http/representationHeader.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
HTTP메시지는 헤더와 바디로 구분할 수 있다.
HTTP바디에는 데이터 메시지 본문(Message body)을 통해서 표현(Representation) 데이터를 전달한다.
여기서 데이터를 실어 나르는 부분을 페이로드(Payload)라고 한다.

표현은 요청이나 응답에서 전달할 실제 데이터를 뜻하며 표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공한다.
HTTP헤더는 HTTP전송에 필요한 모든 부가정보들을 담기 위해 사용하며, 필요에 따라 임의의 헤더를 추가할 수 있다.
<br>
(ex. 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등.)

>- **Content-Type**: 표현 데이터의 형식
<img src="/assets/images/posts_img/http/contentType.png">
- **Content-Encoding**: 표현 데이터의 압축 방식
<img src="/assets/images/posts_img/http/contentEncoding.png">
- **Content-Language**: 표현 데이터의 자연 언어
<img src="/assets/images/posts_img/http/contentLang.png">
- **Content-Length**: 표현 데이터의 길이
<img src="/assets/images/posts_img/http/contentLen.png">
Transfer-Encoding은 전송 시 어떤 인코딩 방법을 사용할 것인가를 명시한다.
그러나 현재는 Transfer-Encoding보다는 Content-Encoding을 사용하며, Transfer-Encoding을 사용하는 경우 chunked의 방식으로 사용한다.<br> 
chunked 방식의 인코딩은 많은 양의 데이터를 분할하여 보내기 때문에 전체 데이터의 크기를 알 수 없어 표현 데이터의 길이를 명시해야 하는 Content-Length 헤더와 함께 사용할 수 없다.


#### 2. 요청(Request) 헤더
<h5 class="sub-title">From: 유저 에이전트의 이메일 정보</h5>
- 일반적으로 잘 사용하지 않음
- 검색 엔진에서 주로 사용
<h5 class="sub-title">Referer: 이전 웹 페이지 주소</h5>
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A → B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
- `Referer`를 사용하면 유입경로 수집 가능
- referer는 단어 referrer의 오탈자이지만 스펙으로 굳어짐
<h5 class="sub-title">User-Agent: 유저 에이전트 애플리케이션 정보</h5>
- 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- `user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36`
<h5 class="sub-title">Host: 요청한 호스트 정보(도메인)</h5>
- 필수 헤더
- 하나의 서버가 여러 도메인을 처리해야 할 때 호스트 정보를 명시하기 위해 사용
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때 호스트 정보를 명시하기 위해 사용
<figure>
  <img src="/assets/images/posts_img/http/host.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
<h5 class="sub-title">Origin: 서버로 POST 요청을 보낼 때, 요청을 시작한 주소를 나타냄</h5>
- 여기서 요청을 보낸 주소와 받는 주소가 다르면 CORS 에러가 발생한다.
- 응답 헤더의 Access-Control-Allow-Origin와 관련
<h5 class="sub-title">Authorization: 인증 토큰(ex. JWT)을 서버로 보낼 때 사용하는 헤더</h5>
- “토큰의 종류(ex. Basic) + 실제 토큰 문자”를 전송
- `Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l`

#### 3. 응답(Response) 헤더
<h5 class="sub-title">Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보</h5>
- `Server: Apache/2.2.22 (Debian)`
- ``Server: nginx``
<h5 class="sub-title">Date: 메시지가 발생한 날짜와 시간</h5>
- `Date: Tue, 15 Nov 1994 08:12:31 GMT`
<h5 class="sub-title">Location: 페이지 리디렉션</h5>
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 리다이렉트(자동 이동)
- 201(Created): Location 값은 요청에 의해 생성된 리소스 URI
- 3xx(Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴
<h5 class="sub-title">Allow: 허용 가능한 HTTP 메서드</h5>
- 405(Method Not Allowed)에서 응답에 포함
- `Allow: GET, HEAD, PUT`
<h5 class="sub-title">Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간</h5>
- 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
- `Retry-After: Fri, 31 Dec 2020 23:59:59 GMT(날짜 표기)`
- `Retry-After: 120(초 단위 표기)`

[List of HTTP headers](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields){:target="_blank"}

#### 4. 콘텐츠 협상 헤더
클라이언트가 선호하는 표현을 요청하는 헤더로 요청 시에만 사용한다.
>- **Accept**: 클라이언트가 선호하는 미디어 타입 전달
- **Accept-Charset**: 클라이언트가 선호하는 미디어 타입 전달
- **Accept-Encoding**: 클라이언트가 선호하는 미디어 타입 전달
- **Accept-Language**: 클라이언트가 선호하는 미디어 타입 전달

Accept-Language 헤더를 통해 클라이언트가 원하는 언어를 서버에 요청하는 방법을 알아보자.
한국어 브라우저에 특정 웹사이트에 접속했을 때 콘텐츠 협상(Accept-Language)이 없다면, 서버는 기본 으로 설정된 언어로 응답한다.
이와 같은 문제를 해결하기 위해 우선순위를 지정해 요청할 수 있다.
<figure>
  <img src="/assets/images/posts_img/http/accept.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

<hr>

## 📝 HTTPS
HTTPS는 HTTP Secure의 약자로, HTTP와 달리 요청과 응답으로 오가는 데이터를 **암호화** 한다. 때문에 기존의 HTTP 프로토콜을 더 안전하게(Secure) 사용할 수 있다. 

<hr class="sub">

### ✔️ 암호화
암호화 방법에 대해 설명하기 앞서, 암호학이란 무엇인지 알아보자.
>**암호학 crytography** <br>
cryto 비밀 + graphy 방법 <br>
암호화는 비밀을 지키는 방법을 의미한다. 그렇다면 비밀을 지키기 위한 요소들엔 무엇이 있을까?

<strong class="sub-title">비밀을 지키기 위한 요소들</strong>
- 기밀성(Confidentiality): 암호화 된 내용을 알 수 없어야함
- 무결성(Integrity): 내용이 원본과 같다는 것을 확신할 수 있도록 해주는 특성
- 인증(Authentication): 권한이 있는 사람만 접근할 수 있는 것

<strong class="sub-title">암호화 과정</strong>
<img src="/assets/images/posts_img/http/cryto.png">
- **평문(plain text)**: 보호되고 있지 않은 데이터
- **암호문(cipher text)**: 암호화 된 데이터
- **암호 알고리즘**: 예전엔 암호 알고리즘을 공개하지 않는 것으로 암호화 했다. 하지만 현대에 와서는 암호 알고리즘을 공개해 검증을 받는다. 대신 암호를 만들고 풀 때 비밀정보를 섞는데 이 정보를 열쇠라는 의미에서 **key** 라고 한다.

<hr class="sub">

### ✔️ 단방향 암호화 방식
단방향 암호화 방식은 암호화는 되는데 복호화는 되지 않는 암호법으로 기밀성이 아닌 **무결성**에 초점을 둔 방식이다.

단방향 암호화는 다른말로 HASH라고 한다. HASH를 사전적 의미를 찾아보면 '다지다'라는 뜻이 있다.
식재료 등을 다지면 원래 상태로 되돌릴 수 없다. 데이터도 마찬가지다.
데이터를 HASH하면 원래상태로 되돌릴 수 없다.

예를들어, earth 라는 단어를 sha256이라는 알고리즘을 사용하면 알 수 없는 값이된다.
이 값을 원래 단어로 되돌릴 수 없다.

우리가 HASH를 이용하는 이유는 '무결성'에 있다.
어떠한 정보가 원본으로부터 훼손되었거나 조작되지 않았는지 파악하는데 HASH가 유용하기 때문이다. 

> [MD5 file shechsum](https://emn178.github.io/online-tools/md5_checksum.html){:target="_blank"}
사이트에서 다운로드한 파일의 MD5 체크섬을 확인해 볼 수 있다. <br>
이 밖에도 CRC, MD5, RIPEMD160, SHA-1, SHA-256, SHA-512 등 다양한 HASH알고리즘이 있다.

또한 단방향 암호화 방식은 다음과 같은 상황에서도 활용할 수 있다.
- 무결성 체크
- 전자서명
- 파일의 식별자
- 사용자의 비밀번호를 서버에 저장할때
- 블록체인, 가상화폐, 비트코인 등

<hr class="sub">

### ✔️ 단방향 암호화 방식
#### 1. 대칭 키 암호화 방식
대칭 키 암호화 방식은 하나의 키만 사용한다. 암호화할 때 사용한 키로만 복호화가 가능하다.
<figure>
  <img src="/assets/images/posts_img/http/key1.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
두 개의 키를 사용해야하는 공개 키 방식에 비해서 연산 속도가 빠르다는 장점이 있다. 하지만 키를 주고 받는 과정에서 탈취 당했을 경우에는 암호화가 소용없어지기 때문에 키를 관리하는데 신경을 많이 써야 한다.

[AES encryption](https://aesencryption.net/){:target="_blank"}

#### 2. 공개 키(비대칭 키) 암호화 방식
비대칭 키 암호화 방식은 두 개의 키를 사용한다. 암호화할 때 사용한 키와 다른 키로만 복호화가 가능하다.
<figure>
  <img src="/assets/images/posts_img/http/key2.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

여기서 두 개의 키를 각각 공개 키, 비밀 키 라고 부른다.
- 공개 키: 누구든지 접근 가능. 누구든 이 공개 키를 사용해서 암호화한 데이터를 보내면
- 비밀 키를 가진 사람만 그 내용을 복호화할 수 있다.
- 보통 요청을 보내는 사용자가 공개 키를, 요청을 받는 서버가 비밀 키를 가진다.
- 이 때, 비밀 키는 서버가 해킹당하는 게 아닌 이상 탈취되지 않는다.

이러한 공개 키 방식은 공개 키를 사용해 암호화한 데이터가 탈취 당한다고 하더라도, 비밀 키가 없다면 복호화할 수 없으므로 대칭 키 방식보다 보안성이 더 좋다. 하지만 대칭 키 방식 보다 더 복잡한 연산이 필요하여 더 많은 시간을 소모한다는 단점이 있다.

<hr class="sub">

### ✔️ SSL/TLS 프로토콜
HTTPS는 HTTP 통신을 하는 소켓 부분에서 SSL 혹은 TLS라는 프로토콜을 사용하여 서버 인증과 데이터 암호화를 진행한다.<br>
(여기서 SSL이 표준화되며 바뀐 이름이 TLS이므로 사실상 같은 프로토콜이라고 생각하면 됨) SSL/TLS는 다음과 같은 특징을 가집니다.

- CA를 통한 인증서 사용
- 대칭 키, 공개 키 암호화 방식을 모두 사용

그럼 SSL/TLS 프로토콜이 어떤 과정을 거쳐 서버 인증과 데이터 암호화를 진행하는지 알아보자!

<h4 class="sub-title"> 인증서와 CA</h4>
HTTPS를 사용하면 브라우저가 서버의 응답과 함께 전달된 인증서를 확인할 수 있다. 이러한 인증서는 서버의 신원을 보증해줍니다. 이때 인증서를 발급해주는 공인된 기관들을 Certificate Authority, CA라고 부른다.
<figure>
  <img src="/assets/images/posts_img/http/ca.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
- 서버는 인증서를 발급받기 위해서 CA로 서버의 정보와 공개 키를 전달
- CA는 서버의 공개 키와 정보를 CA의 비밀 키로 암호화하여 인증서를 발급
- 서버는 클라이언트에게 요청을 받으면 CA에게 발급받은 인증서를 보내줌
<figure>
  <img src="/assets/images/posts_img/http/ca2.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
- 사용자가 사용하는 브라우저는 CA들의 리스트와 공개 키를 내장하고 있음
- 우선 해당 인증서가 리스트에 있는 CA가 발급한 인증서인지 확인
- 리스트에 있는 CA라면 해당하는 CA의 공개 키를 사용해서 인증서의 복호화를 시도

CA의 비밀 키로 암호화된 데이터(인증서)는 CA의 공개 키로만 복호화가 가능하므로, 정말로 CA에서 발급한 인증서가 맞다면 복호화가 성공적으로 진행되어야 한다.

- 복호화가 성공: 클라이언트는 서버의 정보와 공개 키를 얻게 됨과 동시에 해당 서버가 신뢰할 수 있는 서버임을 알 수 있게 된다.
- 복호화가 실패: 이는 서버가 보내준 인증서가 신뢰할 수 없는 인증서임을 확인하게 된다.

<h4 class="sub-title"> 대칭 키 전달</h4>
이제 사용자는 서버의 인증서를 성공적으로 복호화하여 서버의 공개 키를 확보했다. 

이 공개 키는 어디에 쓰면 될까?

바로 클라이언트와 서버가 함께 사용하게 될 대칭 키를 주고 받을 때 쓰게 된다. 대칭 키는 속도는 빠르지만, 오고 가는 과정에서 탈취될 수 있다는 위험성이 있다. 하지만 클라이언트가 서버로 대칭 키를 보낼 때 **서버의 공개 키**를 사용해서 암호화하여 보내준다면, 서버의 비밀 키를 가지고 있는게 아닌 이상 해당 대칭 키를 복호화할 수 없으므로 탈취될 위험성이 줄어들게 된다.
<figure>
  <img src="/assets/images/posts_img/http/ca3.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
- 클라이언트는 데이터를 암호화하여 주고받을 때 사용할 대칭 키를 생선한다.
- 클라이언트는 생성한 대칭 키를 서버의 공개 키로 암호화하여 전달합니다.
- 서버는 전달받은 데이터를 비밀 키로 복호화하여 대칭 키를 확보한다.

이렇게 서버와 클라이언트는 동일한 대칭 키를 갖게되었다.

<figure>
  <img src="/assets/images/posts_img/http/ca4.gif">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
- 이제 HTTPS 요청을 주고 받을 때 이 대칭 키를 사용하여 데이터를 암호화하여 전달하게 된다.
- 대칭 키 자체는 오고 가지 않기 때문에 키가 유출될 위험이 없어졌다.
- 따라서 요청이 중간에 탈취 되어도 제 3자가 암호화된 데이터를 복호화할 수 없게 된다.

HTTPS는 이러한 암호화 과정을 통해 HTTP보다 안전하게 요청과 응답을 주고받을 수 있게 해준다.

>💡 정리하자면,<br>
이렇게 서버와 클라이언트간의 CA를 통해 서버를 인증하는 과정과 데이터를 암호화하는 과정을 아우른 프로토콜을 SSL 또는 TLS이라고 말하고, HTTP에 SSL/TLS 프로토콜을 더한 것을 HTTPS라고 한다.