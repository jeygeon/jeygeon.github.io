---
title: "[WEB] HTTP 인증 (Basic, Bearer)"
date: 2025-02-13
toc: true
toc_sticky: true
categories: "web"
comments: true
---

HTTP 통신에서는 특정 리소스에 대한 접근을 제한해야 할 때가 있다. 예를 들어, 내가 작성한 게시글은 나만 수정할 수 있어야 하며, 다른 사용자는 접근할 수 없어야 한다.

이를 위해 클라이언트는 **서버에 인증된 사용자임을 증명**해야 하며, **서버는 해당 사용자가 요청한 리소스에 접근할 권한이 있는지 검증** 해야 한다.

이때 사용되는 것이 Authorization 헤더이며, 형식은 다음과 같다
<p>
  <img src="/assets/images/web/authorization/1.png">
</p>

들어간 헤더의 값에 따라 서버는 적절한 응답 코드를 반환한다.
- 적절한 인증 정보가 들어갔을 경우: `201 OK`
- 인증 헤더가 누락 되었을 경우: `401 Unauthorized`
- 인증 헤더는 있지만 권한이 없는 사용자라면: `403 Forbidden`

Credential에 들어갈 인증 정보는 type에 따라서 달라지는데 type은 `basic`, `bearer` 두 가지가 존재한다.

## 📌 basic
basic은 사용자의 id, password로 인증하는 방식이다. id:password를 base64로 인코딩한 값이 Credential자리에 들어가게 된다.

<p>
  <img src="/assets/images/web/authorization/2.png">
</p>

base64를 사용했기 때문에 복호화가 매우 쉬워서 반드시 해당 타입을 사용할 때는 HTTPS(TLS)로 통신하는 것이 필수적이다.

basic방식은 간단하고 구현또한 쉽기 때문에 주로 내부 시스템이나 간단한 API인증에서 주로 사용한다.

## 📌 bearer
bearer 인증은 주로 OAuth2.0에서 주로 사용하는 인증 방식이지만 공식문서에 보면 단독으로도 사용 가능하다고 나와있다.

<p>
  <img src="/assets/images/web/authorization/3.png">
</p>
<br/>
bearer은 **소지자**라는 의미로 토큰을 가진 사람은 인증된 사용자로 간주 된다는 의미이다.

일반적인 인증과정은 아래와 같다

1. 사용자가 Id, Password를 입력하고 서버에 로그인 요청을 보낸다.
2. 서버에서는 인증된 사용자인지 확인한 뒤, **Access token**을 발급하여 클라이언트로 전달한다.
3. 클라이언트는 전달받은 토큰을 `Authorization: bearer <access_token>`의 형태의 헤더를 포함하여 API요청을 보낸다.
4. 서버는 토큰을 검증한다.

<br/>

|구분|basic|bearer|
|---|---|---|
|방식|토큰기반 인증|ID, Password Base64 인코딩|
|보안성|높은(ID/PW 직접 노출 X)|낮음(ID/PW 직접 전송)|
|세션관리|불필요|매 요청 마다 ID/PW 필요|
|사용예시|OAuth2.0, JWT기반 인증 API|내부 API, 간단한 인증|