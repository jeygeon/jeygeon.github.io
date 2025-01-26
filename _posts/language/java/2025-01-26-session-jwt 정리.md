---
title: "[WEB] Session - JWT 정리"
toc: true
toc_sticky: true
date: 2025-01-26
categories: java
comments: true
---

웹 개발을 하면서 사용자 인증 방식을 구현하는 방법에는 크게 `session`을 이용한 방법과 `jwt`를 이용한 두가지 방법이 있다. (쿠키는..제외..)

전통적으로 `session`을 이용한 사용자 인증 방식을 많이 사용했지만 내가 운영하는 서비스나 서버 상태에 따라서 사용자 인증을 `session`을 사용할지 `jwt`를 사용할지는 각각의 장단점이 확실히 존재하기에 잘 고민해봐야 하는 부분인 것 같다.

## 📌 Session
<p>
	<img src="/assets/images/language/java/session-jwt/session.png">
</p>

이미지에서처럼 세션 인증 방식은 사용자의 인증 정보를 서버에서 직접 관리를 하게 된다. 

이렇게 서버에서 직접 관리하기 때문에 <span style="color:blue">보안성은 높지만</span> 사용량이 많아질수록 <span style="color: red">서버의 부하가 많이 갈 수 있다는 단점</span>이 있다.

그렇지만 비교적 <span style="color:blue">간단하게 구현</span>할 수 있고, <span style="color:blue">강제 로그아웃 처리</span>가 쉽다는 장점이 존재한다.

또한 **Scale-out**으로 서버가 여러개가 존재할 시 세션을 공유해야 하는 작업이 추가된다. 이에 경우에는 세션을 사용한 인증 방식보다는 JWT를 사용하는 방식이 더 효율적일 것이다.

> 💡 **Scale-out 이란?**
> 한 대의 서버로 운영되던 서비스가 점차 크기가 확장돼서 한대의 서버로 운영이 힘들다고 판단이되면, 정상적인 운영을 위해 서버의 사양을 높이던가 서버를 증설해서 여러대를 두어야 한다.
<br/>
**Scale-in** : CPU나 메모리 증설로 서버 자체의 스펙을 높이는 것
**Scale-out** : 서버의 개수를 증설해서 시스템을 확장하는 것이다. 이 경우에는 들어오는 요청을 분배해주는 로드밸런싱이 필수적이다.

## 📌 JWT (Json Web Token) 
<p>
	<img src="/assets/images/language/java/session-jwt/jwt3.png">
</p>
JWT는 서버가 사용자의 로그인 정보를 토큰(JSON방식)으로 변환하여 클라이언트에 전달하는 방식이다. 이후 클라이언트는 헤더에 이 토큰을 포함하여 요청을 보내고 서버에서는 이 토큰을 검증하는 방식이다.

JWT는 아래와 같이 3가지 구조로 구성된다.
<p style="width:100%">
	<img src="/assets/images/language/java/session-jwt/jwt1.png">
</p>
<br/>
<a href="https://jwt.io/" target="_blank">https://jwt.io</a> 여기에 접속하면 아래와 같이 JWT토큰을 만들어 볼 수 있다.
<p style="width:100%">
	<img src="/assets/images/language/java/session-jwt/jwt2.png">
</p>
<br/>
JWT를 이용한 사용자 인증 방식은 요청을 보낼때마다 요청에 담긴 토큰을 이용해서 사용자 인증을 진행하기 때문에 <span style="color:blue">서버 확장성이 용이</span>하다. 그리고 서버가 사용자의 상태를 저장하지 않기 때문에 <span style="color:blue">서버의 부담이 적다</span>는 특징이 있다.

## 정리
<table>
  <thead>
    <tr>
      <th style="text-align: center;">비교항목</th>
      <th style="text-align: center;">세션 기반 인증</th>
      <th style="text-align: center;">JWT 기반 인증</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;">저장 위치</td>
      <td style="text-align: center;">DB, 메모리, Redis 등</td>
      <td style="text-align: center;">클라이언트</td>
    </tr>
    <tr>
      <td style="text-align: center;">서버 부하</td>
      <td style="text-align: center;">서버에서 세션 관리 필요</td>
      <td style="text-align: center;">서버에서 관리 필요 없음</td>
    </tr>
    <tr>
      <td style="text-align: center;">확장성</td>
      <td style="text-align: center;">서버 확장 시 세션 공유 필요</td>
      <td style="text-align: center;">서버 확장에 용이</td>
    </tr>
    <tr>
      <td style="text-align: center;">속도</td>
      <td style="text-align: center;">비교적 빠름</td>
      <td style="text-align: center;">서명 검증 과정이 포함되어 상대적으로 느림</td>
    </tr>
  </tbody>
</table>