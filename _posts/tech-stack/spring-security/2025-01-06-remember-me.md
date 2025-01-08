---
title: "[Spring Security] RememberMe 설정"
date: 2025-01-08
categories: "spring-security"
comments: true
---

Spring Security에서 제공하는 기능 중 `RememberMe` 라는 기능이 있다. 이름에서부터 알 수 있듯이 처음 인증에 성공하면 토큰이 제공되는데 이 토큰을 가지고 있을 경우 자동 로그인 처리를 해주는 기능이다.

전체적인 흐름은 아래와 같다.
1. **최초 로그인을 했을 때 인증에 성공할 경우 RememberMe가 체크되어있는지 확인**
2. **RememberMe가 체크되어 있을 경우 쿠키를 만들어서 클라이언트로 전달**
3. **이후 클라이언트는 이 쿠키를 가지고 서버로 요청을 보냄**

아래는 SecurityConfig에서 RememberMe 관련 설정이다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

  @Bean
  public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

    http.authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
    http.formLogin(Customizer.withDefaults());
    http.rememberMe(rememberMe -> rememberMe
          .alwaysRemember(true) // true로 되어있으면 사용자가 "기억하기" 체크하지 않아도 무조건 RememberMe가 동작된다
          .tokenValiditySeconds(3600) // 토큰이 유효한 시간(초 단위)
          .userDetailsService(userDetailsService()) // UserDetails를 조회하기 위해 사용되는 UserDetailsService 지정
          .rememberMeParameter("remember") // RememberMe 파라미터 명
          .rememberMeCookieName("remember") // 기억하기 인증을 위한 토큰을 저장하는 쿠키 이름
          .key("security")); // 기억하기 기능을 사용하기 위한 키

    return http.build();
  }

  @Bean
  public UserDetailsService userDetailsService() {
    UserDetails user = User.withUsername("user").password("{noop}1234").roles("USER").build();
    return new InMemoryUserDetailsManager(user);
  }
}
```

이렇게 설정 하고 어플리케이션을 실행하게 되면 아래와 같이 기본적으로 Spring Security에서 제공하는 로그인 페이지에서 우리가 흔히 알고 있는 기억하기 체크박스가 생성된 것을 볼 수 있다.

<p style="width:100%; text-align: center">
	<img src="/assets/images/tech-stack/spring-security/remember-me/rememberme1.png">
</p>
<br/>
이 상태에서 위의 설정대로 user / 1234 로 로그인을 해서 인증에 성공할 경우 remember이라는 이름으로 쿠키가 생성이 된 것을 볼 수 있다.

<p style="width:100%">
	<img src="/assets/images/tech-stack/spring-security/remember-me/rememberme2.png">
</p>

쿠키를 쓸때는 탈취같은 보안적인 요소도 빼놓을 수 없는데 Spring Security에서 쿠키는 암호화가 되어서 만들어진다.

쿠키는 아래와 같이 만들어진다.

`base64(username + ":" + expirationTime + ":" + algorithmName + ":" + algorithmHex(username + ":" + expirationTime + ":" + password + ":" + key))`

- username : UserDetailsService로 식별 가능한 사용자 이름
- password : 검색된 UserDetails에 일치하는 비밀번호
- expirationTime : remember-me 토큰이 만료되는 날짜와 시간 (밀리초)
- key : remember-me 토큰의 무결성 검증을 위한 키
- algorithmName : remember-me 토큰 서명을 생성하고 검증하는데 사용되는 알고리즘 (default : SHA256)

그래서 위의 생성된 쿠키를 base64 복호화를 진행하면 아래와 같이 나오게 된다.

<p style="width:100%">
	<img src="/assets/images/tech-stack/spring-security/remember-me/rememberme3.png">
</p>

때문에 이 쿠키가 만약 탈취당하더라도 유출되는 정보는 크게 중요하지 않고 SHA256는 단방향이기 때문에 복호화도 불가능하다.