---
title: "[네트워크] Cookie &#47; Session"
excerpt: "쿠키의 개념과 작동원리 및 이를 이용한 세션 인증 방식에 대해 알아보자"

categories:
  - Network
tags:
  - [Cookie, Session]

permalink: /Network/Cookie_and_Session

toc: true
toc_sticky: true

date: 2023-03-09
last_modified_at: 2023-03-09
---
<hr>

## 🍪 Cookie
>HTTP 프로토콜의 **무상태성**을 보완해주는 도구

서버는 클라이언트의 쿠키를 이용해서 데이터를 가져올 수 있다.
쿠키를 이용하는 것은 서버에서 클라이언트에 쿠키를 전송하는 것 뿐만 아니라, 클라이언트에서 서버로 쿠키를 다시 전송하는 것도 포함된다.
<br>

쿠키는 헤더에 담아 보내며, 보통 로그인 유지, 추천광고, 모달창 `오늘 보지 않기` 옵션 등에 쓰인다.
<hr class="sub">
### 👀 쿠키 옵션
앞서 언급한 것처럼 서버는 쿠키를 이용하여 데이터를 저장하고 이 데이터를 다시 불러와 사용할 수 있다.
이 데이터는 저장한 이후 특정 조건들이 만족되어야 다시 가져올 수 있다.

이런 조건들은 아래 코드처럼 http 헤더를 사용해 쿠키 옵션으로 표현할 수 있다.

```javascript
'Set-Cookie':[
            'cookie=yummy', 
            'Secure=Secure; Secure',
            'HttpOnly=HttpOnly; HttpOnly',
            'Path=Path; Path=/cookie',
            'Doamin=Domain; Domain=codestates.com'
        ]
```

<h4 class="sub-title">1. Domain</h4>
도메인이란 우리가 흔히 사용하는 `www.google.com`과 같은 서버에 접속할 수 있는 이름이다.
쿠키옵션에서 도메인은 포트 및 서브 도메인 정보(ex.`www`), 세부 경로를 포함하지 않는다.

따라서 요청할 `http://www.localhost.com:3000/users/login`이라면 여기서 Domain은 `localhost.com`이다.

만약 쿠키 옵션에서 도메인 정보가 존재한다면 클라이언트에서는 쿠키의 도메인 옵션과 서버의 도메인이 일치해야만 쿠키를 전송할 수 있다.
예를들어, `naver.com`에서 받은 쿠키는 `google.com`에 전송할 수 없다.

<h4 class="sub-title">2. Path</h4>
`Path`는 세부경로로, 서버가 라우팅할 때 사용하는 경로를 말한다.
요청해야하는 URL이 `http://www.localhost.com:3000/users/login`라면 `Path`는 `/users/login`이 된다. (*기본값: `/`)

`Path`는 설정된 경로를 포함하는 하위 경로로 요청을 하더라도 쿠키를 서버에 전송할 수 있다. 즉 `Path`가 `/users`로 설정되어 있다면, `/users`의 하위 경로인 `/users/codestates` 로 요청을 하더라도 쿠키 전송이 가능하다.

<h4 class="sub-title">3. MaxAge or Expires</h4>
쿠키의 유효기한을 정하는 옵션이다. 쿠키가 계속 남아있다면 탈취위험이 있어 보안에 좋지 않기 때문에 유효기간을 설정하는 것이 좋다.

- `MaxAge`: 쿠키가 유효한 시간을 초 단위로 설정하는 옵션.
- `Expires`: 쿠키의 유효한 날짜를 정할 수 있는 옵션. 이때 옵션읭 값은 클라이언트의 시간을 기준으로 한다. 이후 지정된 시간, 날짜를 초과하게 되면 쿠키는 자동으로 파괴된다.

위 옵션 여부에 따라 쿠키는 세션쿠키(Session Cookie), 영속성 쿠키(Persistent Cookie)로 나뉜다.

>- **세션 쿠키:** `MaxAge`또는 `Expires`옵션이 없는 쿠키, 브라우저가 실행 중일 때 사용할 수 있는 임시 쿠키이다. 브라우저를 종료하면 해당 쿠키는 삭제됨
- **영속성 쿠키:** 브라우저의 종료 여부와 관계없이 `MaxAge`또는 `Expires`에 지정된 유효시간만큼 사용이 가능한 쿠키

<h4 class="sub-title">4. Secure</h4>
사용하는 프로토콜에 따른 쿠키의 전송여부를 결정하는 옵션.
- `Secure : true` HTTPS를 이용하는 경우에만 쿠키를 전송할 수 있다.

`Secure`옵션이 없다면 프로토콜에 상관없이(`http://`, `https://`) 쿠키를 전송할 수 있다.

>단, 도메인이 `localhost`인 경우, HTTPS가 아니어도 쿠키 전송이 가능하다.

<h4 class="sub-title">5. HttpOnly</h4>
자바스크립트로 브라우저의 쿠키에 접근이 가능한지 여부를 결정한다.
- `HttpOnly : true` 자바스크립트로 쿠키 접근 불가

옵션을 명시하지 않는 경우 기본값으로 `false`가 지정된다. 이 경우 `document.cookie`를 이용해 자바스크립트로 쿠키에 접근할 수 있으므로 쿠키가 **탈취될 위험**이 있다.

<h4 class="sub-title">6. SameSite</h4>
Cross-Origin 요청을 받은 경우, 요청에서 사용한 메소드(ex. `GET`, `POST`, `PUT`, `PATCH` 등)와 해당 옵션의 조합을 기준으로 서버의 쿠키 전송 여부를 결정하게 된다.

- `Lax`: Cross-Origin 요청이라면 `GET` 메소드에 대해서만 쿠키를 전송할 수 있다.
- `Strict`: 단어 그대로 가장 엄격한 옵션. Cross-Origin이 아닌 *`same-site`인 경우에만 쿠키 전송이 가능하다.
- `None`: Cross-Origin에 대해 가장 관대한 옵션으로 항상 쿠키를 보내줄 수 있다. 다만 쿠키 옵션 중 `Secure`옵션이 필요하다.

*`same-site`: 요청을 보낸 Origin과 서버의 도메인, 프로토콜, 포트가 같은 경우를 말하며, 이 중 하나라도 다르면 Cross-Origin으로 구분된다.

서버에서 이러한 옵션들을 지정한 다음 서버에서 클라이언트로 쿠키를 처음 전송하게 된다면 헤더에 `Set-Cookie`라는 프로퍼티로 쿠키를 담아 전송한다.

이후 클라이언트에 서버에게 쿠키를 전송해야 한다면 클라이언트는 헤더에 `Cookie`라는 프로퍼티에 쿠키를 담아 서버에 쿠키를 전송하게 된다.

<hr class="sub">
>
이러한 쿠키의 특성을 이용하여 서버는 클라이언트에 인증정보를 담은 쿠키를 전송하고, 클라이언트는 전달받은 쿠키를 서버에 요청과 함께 전송하여 Stateless한 인터넷 연결을 Stateful하게 유지할 수 있다.
<br><br>
하지만 기본적으로 쿠키는 오랜 시간 동안 유지될 수 있고, `HttpOnly` 옵션을 사용하지 않았다면 자바스크립트를 이용해서 쿠키에 접근할 수 있기 때문에 쿠키에 민감한 정보를 담는 것은 위험하다.
<br><br>
이런 인증정보를 이용해 공격자가 유저인척 서버에 요청을 보낸다면 서버는 누가 요청을 보낸 건지 의심하지 않고 이를 인증된 유저의 요청으로 취급하게 된다. 이때 개인정보와 같은 민감한 정보를 공격자가 탈취한다면 2차 피해가 일어날 수 있다.

<hr>

## ✅ Session
 >사용자가 인증에 성공한 상태

서버가 Client에 유일하고 암호화된 ID를 부여하고, 중요 데이터는 **서버**가 관리한다.
클라이언트는 쿠키로 세션 아이디를 저장

<hr class="sub">

### 👀 세션기반 인증(Session-based Authentication)
<figure>
  <img src="/assets/images/posts_img/cookie_and_session/session.png">
  <figcaption>출처: 코드스테이츠</figcaption>
</figure>

1. `클라이언트`: 웹사이트에 접속해 서버에 요청 (로그인 등)
2. `서버`: 접속한 클라이언트에게 세션 ID를 부여해서 응답 (인증 성공)
3. `클라이언트`: 해당 세션 ID를 헤더 쿠키에 넣어 데이터 요청 (장바구니에 물건 넣기)
4. `서버`: 세션 ID를 통해 클라이언트를 구별하여 데이터를 알맞게 응답
5. `클라이언트`: 접속을 종료한다.(쿠키 갱신 혹은 삭제)
6. `서버`: 세션 ID를 제거한다.(세션 정보 삭제)

클라이언트에서 세션 정보를 없애기 위해서는 `res.cookie`로 쿠키의 값을 무효한 값으로 갱신하거나, `res.clearCookie`로 쿠키를 삭제해버리면 된다.

>세션은 각각 클라이언트에게 세션ID를 부여하고, **정보를 서버에 저장한다.** 즉, 접속한 상태에만 유효한 세션 ID를 통해 사용자를 구별하여 알맞게 응답하는 것이다.

<hr class="sub">

### 👀 express-session
Node.js에는 이런 세션을 대신 관리해 주는 express-session 이라는 모듈이 존재한다.

`express-session`은 세션을 위한 미들웨어로, `express` 서버에서 쉽게 세션을 위한 공간을 다룰 수 있도록 만들어 준다.
```javascript
const express = require('express');
const session = require('express-session');

const app = express();

app.use(
  session({
    secret: '@codestates',
    resave: false,
    saveUninitialized: true,
    cookie: {
      domain: 'localhost',
      path: '/',
      maxAge: 24 * 6 * 60 * 10000,
      sameSite: 'none',
      httpOnly: false,
      secure: true,
    },
  })
);
```
`express-session`를 사용해 위와 같이 세션의 옵션을 지정할 수 있다.
언뜻 보면 쿠키 옵션과 비슷해 보이지만, 세션의 경우 `secret` 옵션의 비밀키를 이용해 암호화해 세션 id라는 것을 생성한다. 그리고 이것을 클라이언트에게 쿠키로 전송한다.


쿠키로 전송된 세션 id는 이에 종속되고 고유한 세션 객체를 가지며 이는 서버에 저장된다.
이때 세션 객체는 유저별로 독립적으로 생성된 객체이므로 유저별로 각각 다른 데이터를 저장할 수 있다.

따라서 클라이언트에 유저의 개인정보를 담지 않고도 서버가 클라이언트의 세션 id를 이용해 유저의 인증여부를 판단할 수 있다.

세션 객체는 `req.session`으로 접근 가능하며, 이를 통에 세션에 임의의 데이터를 저장하거나 불러올 수 있다.

세션 객체 다루는 법은 다음 링크 참고
[GitHub: express-session](https://github.com/expressjs/session#reqsession){:target="_blank"}

<hr>

## 🍪 vs ✅

||설명|접속상태 저장경로|장점|단점|
|:--:|:--:|:--:|:--:|:--:|
|Cookie|쿠키는 그저 http의 stateless한 점을 보완해주는 도구|클라이언트|서버에 부담 덜어줌|쿠키 그 자체는 인증이 아님(보안 취약)|
|Session|접속 상태를 서버가 가짐(stateful) 접속 상태와 권한 부여를 위해 세션아이디를 쿠키로 전송|서버|신뢰할 수 있는 유저인지 서버에서 추가로 확인 가능|하나의 서버에만 접속 상태를 가지므로 분산에 분리|