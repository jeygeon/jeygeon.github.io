---
title: "[Spring Security] 로그인 관련 security config 설정(Form)"
toc: true
toc_sticky: true
date: 2025-01-06
categories: "spring-security"
comments: true
---

spring security 의존성을 추가한 뒤 기본적인 config 설정을 해보겠다.

### 📌 spring Security 의존성 추가
```groovy
implementation 'org.springframework.boot:spring-boot-starter-security'
```

위의 의존성을 추가하면 앞으로 모든 요청에 대하여 인증 여부를 검증하고, 인증이 승인되어야 자원에 접근이 가능하다.

인증 방식은 `formLogin 방식`과 `httpBasic 방식` 두 가지가 있다. httpBasic 방식은 Http 프로토콜에서 정의한 기본적인 인증 방식이다. (인증 받지 않은 사용자가 요청을 보내면 서버에서 401응답과 함께 인증을 요구하는 그런..)

이번 포스팅은 스프링 시큐리티에서 제공하는 formLogin 방식을 중점적으로 기본 설정을 알아보도록 하겠다.

그리고 기본적으로 로그인 페이지가 자동적으로 생성되어 렌더링되며, 해당 로그인 페이지에 로그인 할 수 있는 계정도 한개 자동으로 생성된다.

### 📌 formLogin 관련 설정
아래는 formLogin관련 기본 config 설정들이다.
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

  @Bean
  public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

    // 어떠한 요청도 인증을 받도록 설정
    http.authorizeHttpRequests(auth -> auth.anyRequest().authenticated());

    // formLogin 관련 설정
    http.formLogin(form -> form
        .loginPage("/loginPage")            // 기본 로그인 페이지가 아닌 커스텀 로그인 페이지 설정
        .loginProcessingUrl("/loginProc")   // 로그인 컨트롤러 url 설정
        .defaultSuccessUrl("/", true)       // true로 되어있으면 로그인 성공 시 무조건 / 경로로 이동 (default : false)
        .failureUrl("/failed")              // 로그인을 실패했을 때 타는 url 설정
        .usernameParameter("userId")        // username 파라미터 설정 > form의 id 태그의 name과 일치 해야한다
        .passwordParameter("passwd")        // password 파라미터 설정 > form의 pw 태그의 name과 일치 해야한다
        .successHandler(new AuthenticationSuccessHandler() {    // 로그인 성공했을 때 실행되는 handler 위에서 설정한 값(ex defulatSuccessUrl..)보다 우선시되서 실행된다.
          @Override
          public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
            System.out.println("authentication : " + authentication);
            response.sendRedirect("/home");
          }
        })
        .failureHandler(new AuthenticationFailureHandler() {    // 로그인 실패했을 때 실행되는 handler 위에서 설정한 값(ex failureUrl..)보다 우선시되서 실행된다.
          @Override
          public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException, ServletException {
            System.out.println("exception : " + exception.getMessage());
            response.sendRedirect("/login");
          }
        })
        .permitAll()
      );

    return http.build();
  }
}
```

### 📌 defaultSuccessUrl()
위의 코드에서 `defaultSuccessUrl()` 설정에 대해서 부가적으로 설명을 더하자면 `String defaultSuccessUrl`, `boolean alwaysUse` 이렇게 두가지 매개변수를 받을 수 있다. 첫 번째 인자는 로그인을 성공했을 경우 타게 되는 URL이고, 두 번째 인자는 항상 해당 URL을 사용할 것이냐는 의미이다.

말로는 설명이 어려워서 아래 예를 통해 쉽게 이해가 가능하다.

<p style="width:100%">
	<img src="/assets/images/tech-stack/spring-security/formlogin/defaultSuccessUrl.png">
</p>