---
title: "[네트워크] Token"
excerpt: "세션 인증 방식을 보완한 토큰 인증 방식에 대해 알아보자"

categories:
  - Network
tags:
  - [Hashing, Token, JWT]

permalink: /Network/Token

toc: true
toc_sticky: true

date: 2023-03-10
last_modified_at: 2023-03-10
---
<hr>

## ☁️ 해싱 (Hashing)
해싱은 해시 함수(Hash Function)을 사용해 암호화하며, 복호화가 가능한 다른 암호화 방식들과 달리 암호화만 가능하다.

해시 함수는 다음과 같은 특징을 가진다.

- 항상 같은 길이의 문자열을 리턴
- 서로 다른 문자열에 동일한 해시 함수를 사용하면 반드시 다른 결과값이 나온다.
- 동일한 문자열에 동일한 해시 함수를 사용하면 항상 같은 결과값이 나온다.

[SHA1 함수 직접 사용해보기](https://www.convertstring.com/ko/Hash/SHA1){:target="_blank"}

<hr class="sub">

### 🌈 레인보우 테이블과 솔트
항상 같은 결과값이 나온다는 특성은 보안상 위협이 될 수 있다.
레인보우 테이블에 기록된 값의 경우 유출되었을 때 해싱을 했더라도 해싱 이전의 값을 알아낼 수 있기 때문이다.

<figure>
  <img src="/assets/images/posts_img/Token/rainbow.png">
  <figcaption>레인보우 테이블 예시 (출처: 코드스테이츠)</figcaption>
</figure>

이때 활용할 수 있는 것이 바로 **솔트(Salt)**이다.

단어 뜻 그대로 소금을 치듯 해싱 이전 값에 임의의 값을 더해 데이터가 유출되더라도 해싱 이전의 값을 알아내기 어렵게 만드는 방법이다.

|비밀번호 + 솔트|해시 함수(SHA1) 리턴 값|
|:--:|:--:|
|‘password’ + ‘salt’	|‘C88E9C67041A74E0357BEFDFF93F87DDE0904214’|
|‘Password’ + ‘salt’	|‘38A8FDE622C0CF723934BA7138A72BEACCFC69D4’|
|‘kimcoding’ + ‘salt’|	‘8607976121653D418DDA5F6379EB0324CA8618E6’|

<hr class="sub">

### 🌈 해싱은 언제 쓰일까?
해싱은 데이터 그 자체를 사용하는 것이 아니라, 동일한 값의 데이터를 사용하고 있는지 여부만 확인하는 것이 목적이다.

>ex. 로그인
- DB에 사용자의 비밀번호를 해싱하여 저장
- 로그인했을때 입력한 비밀번호가 DB에 저장된 비밀번호가 일치하는지 확인
- 해싱한 값끼리 비교해서 일치했다면, 정확한 비밀번호를 입력했다는 뜻이므로 로그인 요청 처리

이처럼 해싱은 민감한 데이터를 다루어야 하는 상황에서 데이터 유출의 위험성은 줄이면서 데이터의 유효성을 검증하기 위해서 사용되는 단방향 암호화 방식이다.

<hr>

## 💳 토큰 (Token)
토큰은 사용자 인증정보와 권한 정보를 포함한 정보를 담은 암호화된 문자열을 말한다.
이를 이용해 특정 애플리케이션에 대한 사용자의 접근 권한을 부여할 수 있다.

토큰은 유저의 인증 상태를 클라이언트에 저장할 수 있다.
앞서 배운 세션 인증 방식의 비교해 서버의 부하나 메모리 부족 문제를 줄일 수 있다는 장점이 있다.

<hr class="sub">

### 🚌 토큰 인증 방식

<figure>
  <img src="/assets/images/posts_img/Token/token_flow.png">
  <figcaption>토큰 인증 방식의 흐름 (출처: 코드스테이츠)</figcaption>
</figure>
1. 사용자가 인증 정보를 담아 서버에 로그인 요청을 보낸다.
2. 서버는 데이터베이스에 저장된 사용자의 인증 정보를 확인한다.
3. 인증에 성공했다면 해당 사용자의 인증 및 권한 정보를 서버의 비밀 키와 함께 토큰으로 암호화한다.
4. 생성된 토큰을 클라이언트로 전달한다.
  - HTTP 상에서 인증 토큰을 보내기 위해 사용하는 헤더인 Authorization 헤더를 사용하거나, 쿠키로 전달하는 등의 방법을 사용
5. 클라이언트는 전달받은 토큰을 저장한다.
  - 저장하는 위치는 Local Storage, Session Storage, Cookie 등 다양
6. 클라이언트가 서버로 리소스를 요청할 때 토큰을 함께 전달한다.
  - 토큰을 보낼 때에도 Authorization 헤더를 사용하거나 쿠키로 전달할 수 있음
7. 서버는 전달받은 토큰을 서버의 비밀 키를 통해 검증한다. 이를 통해 토큰이 위조되었는지 혹은 토큰의 유효 기간이 지나지 않았는지 등을 확인할 수 있다.
8. 토큰이 유효하다면 클라이언트의 요청에 대한 응답 데이터를 전송한다.

<hr class="sub">

### 🚌 토큰 인증 방식의 장점
<h4 class="sub-title">무상태성</h4>
- 서버가 유저의 인증상태를 관리하지 않는다
- 서버는 비밀키를 통해 클라이언트에서 보낸 토큰의 유효성만 검증하면 되므로 무상태적인 아키텍처 구축이 가능하다.

<h4 class="sub-title">확장성</h4>
- 다수의 서버가 공통된 세션 데이터를 가질 필요가 없다.
- 이를 통해 서버를 확장하기 용이하다.

<h4 class="sub-title">어디서나 토큰 생성가능</h4>
- 토큰의 생성과 검증이 하나의 서버에서 이루어지지 않아도 되기 때문에 토큰 생성만을 담당하는 서버를 구축할 수 있다.
- 이를 잘 활용하면 여러 서비스 간의 공통된 인증 서버를 구현할 수 있다.

<h4 class="sub-title">권한 부여에 용이</h4>
- 토큰은 인증 상태, 접근 권한 등 다양한 정보를 담을 수 있기 때문에 사용자 권한 부여에 용이하다.
- 이를 활용해 어드민 권한 부여 및 정보에 접근할 수 있는 범위도 설정 가능하다.

<hr>

## 🌼 JWT (JSON Web Token)
JWT는 JSON 객체에 정보를 담고 이를 토큰으로 암호화하여 전송할 수 있는 기술로, 토큰 기반 인증 구현 시 대표적으로 사용하는 기술 중 하나이다.

- 클라이언트가 서버에 요청을 보낼때 인증정보를 암호화된 JWT 토큰으로 제공하고,
- 이 토큰을 검증해 인증정보를 확인할 수 있다.

<hr class="sub">

### 🍃 JWT 구성

<figure>
  <img src="/assets/images/posts_img/Token/jwt.png">
  <figcaption>(출처: 코드스테이츠)</figcaption>
</figure>

JSON 객체를 [base64](https://www.base64decode.org/){:target="_blank"}방식으로 인코딩 하면 각각 JWT의 Header, Payload가 된다.

> base64 방식은 원한다면 얼마든지 디코딩할 수 있는 인코딩 방식이다. 따라서 비밀번호와 같이 노출되어서는 안 되는 민감한 정보를 담지 않도록 해야한다.



<h4 class="sub-title">1. Header</h4>
- Header에는 HTTP의 헤더처럼 해당 토큰을 설명하는 데이터가 담겨 있다.
  - ex. 토큰의 종류, 시그니처를 만들 때 사용할 알고리즘 등
- JSON형태로 작성한다.

```javascript
{
  "alg": "HS256",
  "typ": "JWT"
}
```

<h4 class="sub-title">2. Payload</h4>
- HTTP의 페이로드와 마찬가지로 전달하려는 내용물을 담고 있는 부분
  - 어떤 정보에 접근한지에 대한 권한, 유저의 이름과 같은 개인정보, 토큰의 발급시간 및 만료시간
- JSON형태로 작성

```javascript
{
  "sub": "someInformation",
  "name": "phillip",
  "iat": 151623391
}
```

<h4 class="sub-title">3. Signature</h4>
- 토큰의 무결성을 확인할 수 있는 부분
- Header와 Payload가 완성되었다면, Signature는 이를 서버의 비밀 키(암호화에 추가할 salt)와 Header에서 지정한 알고리즘을 사용하여 해싱
- 예를들어, HMAC SHA256알고리즘을([JWT](https://jwt.io/){:target="_blank"}) 사용한다면 Signature는 아래와 같은 방식으로 생성된다.

```javascript
HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), secret);
```

따라서 누군가 권한을 속이기 위해 토큰의 Payload를 변조하는 등의 시도를 하더라도, <br>
토큰을 발급할 때 사용한 Secret을 모른다면 유효한 Signature를 만들어낼 수 없다. <br>
때문에 서버는 Signiture를 검증하는 단계에서 올바르지 않은 토큰임을 알아낼 수 있다.

<hr class="sub">

### 🍃 토큰 인증 방식의 한계
Signature을 사용해서 위조된 토큰을 알아낼 수는 있지만, 토큰 자체가 탈취된다면 토큰 인증 방식의 한계가 드러난다.

<h4 class="sub-title">무상태성</h4>
- 인증 상태를 관리하는 주체가 서버가 아니므로, 토큰이 탈취되어도 해당 토큰을 강제로 만료시킬 수 없다.
- 따라서 해커는 토큰이 만료될 때까지 사용자로 가장해 계속 요청을 보낼 수 있다.

<h4 class="sub-title">유효 기간</h4>
- 토큰이 탈취되는 상황을 대비해 유효기간을 짧게 설정하면, 사용자는 토큰이 만료될 때마다 다시 로그인 해야한다.
- 사용자 경험에 좋지 않다.
- 그렇다고 유효기간을 길게 설정하면 토큰이 탈취될 경우 더 치명적으로 작용할 수 있다.

<h4 class="sub-title">토큰의 크기</h4>
- 토큰에 여러 정보를 담을 수 있는 만큼, 많은 데이터를 담으면 그만큰 암호화 하는 과정도 길어지고 토큰의 크기도 커진다.
- 때문에 네트워크 비용 문제가 생길 수 있다.

<hr class="sub">

### 🍃 액세스 토큰(Access Token)과 리프레시 토큰(Refresh Token)
위와 같은 토큰 인증 방식의 한계를 극복하기 위해 고안된 방법이 액세스 토큰과 리프레시 토큰을 함께 사용하는 방법이다.

<h4 class="sub-title">Access Token</h4>
- **서버에 접근하기 위한 토큰**
- 앞서 다룬 토큰과 비슷한 역할
- 보안을 위해 보통 24시간 정도의 짧은 유효기간이 설정되어 있다.

<h4 class="sub-title">Refresh Token</h4>
- 서버 접근을 위한 토큰이 아닌 **액세스 토큰이 만료되었을 때 새로운 액세스 토큰을 발급받기 위해 사용되는 토큰**
- 리프레시 토큰은 액세스 토큰보다 긴 유효기간을 설정한다.

<br>

이렇게 두 가지의 토큰을 사용하면, 액세스 토큰이 만료되더라도 리프레시 토큰의 유효기간이 남아 있다면 사용자는 다시 로그인할 필요 없이 지속해서 인증 상태를 유지할 수 있다.

하지만 리프레시 토큰은 긴 유효기간을 가지고 있기 때문에 이 마저도 탈취된다면 토큰의 긴 유효 기간동안 악의적인 유저가 계속해서 액세스 토큰을 생성하고 사용자 정보를 해킹할 수도 있다. 이를 대비하기 위해 리프레시 토큰을 세션처럼 서버에 저장하고 이에 대한 상태를 관리하기도 한다.

아직까지 이 세상에 완벽한 보안방법은 없다. 무엇보다도 보안과 사용자 경험 사이의 적절한 균형을 찾는 게 중요하다.