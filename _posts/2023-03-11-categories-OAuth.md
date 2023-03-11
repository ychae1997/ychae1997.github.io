---
title: "[네트워크] OAuth"
excerpt: "외부 인증 중개서버로 사용자 인증 처리를 맡길 수 있는 OAuth에 대해 알아보자"

categories:
  - Network
tags:
  - [Hashing, Token, JWT]

permalink: /Network/Token

toc: true
toc_sticky: true

date: 2023-03-11
last_modified_at: 2023-03-11
---
<hr>

## 📝 OAuth

우리가 웹이나 앱에서 흔히 찾아볼 수 있는 소셜 로그인 인증 방식은 OAuth 2.0 라는 기술을 바탕으로 구현된다.

OAuth는 인증을 중개해주는 메커니즘이다. 보안된 리소스에 액세스하기 위해 클라이언트에게 권한을 제공하는 프로세스를 단순화하는 프로토콜이다.

즉, 이미 사용자 정보를 가지고 있는 웹 서비스(Naver, Kakao, Google 등)에서 사용자 인증을 대신해주고, 접근 권한에 대한 **토큰**을 발급한 후, 이를 이용해 내 서버에서 인증이 가능해진다.

<hr class="sub">

### ✔️ OAuth 는 언제, 왜 쓸까?
- 이미 가입된 계정을 이용해 빠르게 서비스에 가입할 수 있다.
- 서비스를 구현하는 개발자도 신규 회원가입이나 회원 관리를 신경 쓰지 않아도 되기 때문에 사용자와 기업 모두 소셜 로그인을 선호하고 있는 추세이다.
- 검증되지 않은 App에서 OAuth를 사용하여 로그인한다면, 유저의 민감한 정보가 직접 App에 노출될 일이 없고 인증 권한에 대한 허가를 미리 유저에게 구해야 하기 때문에 더 안전하게 사용할 수 있다.

<hr>

## 📝 OAuth 작동 메커니즘
### ✔️ OAuth의 주체
<figure>
  <img src="/assets/images/posts_img/OAuth/subject.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

<h4 class="sub-title">Resource Owner</h4>
- `Resource Owner:` OAuth 인증을 통해 소셜 로그인을 하고싶어하는 **사용자**
- Resource는 사용자의 이름, 전화번호 등의 **정보**를 뜻한다. 이러한 정보의 주인이 바로 사용자이기 때문에 Resource Owner라고 한다.

<h4 class="sub-title">Resource Server & Authorization Server</h4>
- `Resource Server:` 사용자가 소셜 로그인을 하기 위해서 사용하는, 이미 사용중인 서비스(Naver, Kakao, Google 등)의 서버 중 **사용자의 정보를 저장하고 있는 서버**
- `Authorization Server:` 이미 사용중인 서비스의 서버 중 **인증을 담당하는 서버**

<h4 class="sub-title">Application</h4>
- 사용자가 **소셜 로그인을 활용해 이용하고자하는 새로운 서비스**는 환경에 따라서 조금씩 다르게 불립니다. 여기서는 `Application`이라고 부르도록 하겠다.
- 경우에 따라서 Applicaiton을 Client와 Server로 세분화해서 지칭하기도 한다.

<hr class="sub">

### ✔️ OAuth 인증 방식의 종류와 흐름
- Grant Type : Authorization Server에서 Access Token을 받아오는 방식

#### 1. Implicit Grant Type
<figure>
  <img src="/assets/images/posts_img/OAuth/Implicit.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
1. 사용자가 Application에 접속합니다.
2. Application에서 Authorization Server로 인증 요청을 보냅니다.
3. Authorizaiton Server는 유효한 인증 요청인지 확인한 후 액세스 토큰을 발급합니다.
4. Authorization Server에서 Application으로 액세스 토큰을 전달합니다.
5. Application은 발급받은 액세스 토큰을 담아 Resource Server로 사용자의 정보를 요청합니다.
6. Resource Server는 Application에게서 전달 받은 액세스 토큰이 유효한 토큰인지 확인합니다.
7. 유효한 토큰이라면, Application이 요청한 사용자의 정보를 전달합니다.

>오늘날 소셜 로그인에서 Implicit Grant Type은 잘 사용하지 않는다.
기존 서비스에 로그인만 되어있다면 새로운 서비스에 바로 액세스 토큰을 내어주기 때문에 보안성이 조금 떨어지기 때문이다.
그래서 보통은 여기에 인증 단계를 한 단계 추가한 인증 방식인 Authorization Code Grant Type을 주로 사용하게 된다.

#### 2. Authorization Code Grant Type
<figure>
  <img src="/assets/images/posts_img/OAuth/AuthorizationCode.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
1. 사용자가 Application에 접속합니다.
2. Application에서 Authorization Server로 인증 요청을 보냅니다.
3. **Authorizaiton Server는 유효한 인증 요청인지 확인한 후 Authorization Code를 발급합니다.**
4. **Authorization Server에서 Application으로 Authorization Code를 전달합니다.**
5. **Application이 Authorization Code로 발급받은 Authorization Code를 전달합니다.**
6. Authorizaiton Server는 유효한 Authorization Code인지 확인한 후 액세스 토큰을 발급합니다.
7. Authorization Server에서 Application으로 액세스 토큰을 전달합니다.
8. Application은 발급받은 액세스 토큰을 담아 Resource Server로 사용자의 정보를 요청합니다.
9. Resource Server는 Application에게서 전달 받은 액세스 토큰이 유효한 토큰인지 확인합니다.
10. 유효한 토큰이라면, Application이 요청한 사용자의 정보를 전달합니다.

>**Authorization Code를 사용한 인증 단계가 추가로 있기 때문에 비교적 더 안전**하다. 또한, 원한다면 아래와 같이 토큰을 Application의 Client에 노출시키지 않고 Server에서만 관리하도록 만들 수도 있기 때문에 소셜 로그인을 구현하는 방식의 선택지가 늘어나게 된다.ㅁ

<figure>
  <img src="/assets/images/posts_img/OAuth/AuthorizationCode2.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
하지만, 사용자가 새로운 서비스를 이용하다가 액세스 토큰이 만료되었을 때, 매번 이 과정을 거쳐서 액세스 토큰을 다시 발급받아야 한다면 사용자 편의성에 있어서는 좋지 않다. 그렇기 때문에 액세스 토큰을 발급해줄 때 리프레시 토큰을 같이 발급해주기도 한다. 이 때, 리프레시 토큰을 사용해서 액세스 토큰을 받아오는 인증 방식을 Refresh Token Grant Type 이라고 한다.

#### 3. Refresh Token Grant Type
<figure>
  <img src="/assets/images/posts_img/OAuth/refreshToken.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>
1. Authorization Server로 리프레시 토큰을 보내주면,
2. Authorization Server는 리프레시 토큰을 검증한 다음 액세스 토큰을 다시 발급해주게 된다.
3. Application은 다시 발급 받은 액세스 토큰을 사용해서 Resource Server에서 사용자의 정보를 받아오게 된다.

<hr class="sub">

### ✔️ OAuth의 장점
<h4 class="sub-title">1. 쉽고 안전하게 새로운 서비스를 이용할 수 있다.</h4>
- 사용자는 OAuth를 통해 특정 사이트에 아이디, 비밀번호, 이름, 전화 번호 등의 정보를 일일이 입력하지 않아도 클릭 몇 번 만으로 손쉽게 가입할 수 있어 편리하다.
- 정보를 해당 서비스에 직접 노출하는 것이 아니기 때문에 직접 가입하는 것보다 더 안전하다.
- Application의 입장에서도 회원의 정보를 직접 가지고 있음으로 인해서 발생할 수 있는 회원 정보 유출의 위험성에서 부담을 덜 수 있다.

<h4 class="sub-title">2. 권한 영역을 설정할 수 있다.</h4>
- OAuth 인증을 허가한다고 해서 새로운 서비스가 사용중이던 서비스의 모든 정보에 접근이 가능한 것은 아니다. 사용자는 원하는 정보에만 접근을 허락할 수 있어 보다 더 안전하다.
- OAuth 설정 페이지에서는 Application에서 필요한 정보를 선택할 수 있다. 사용자는 이 중 원하는 정보만 선택적으로 제공할 수 있습니다.