# [ [[SpringBoot]] ] Spring Security
## 개요
>[!note] Spring Security란?
> 스프링 기반 애플리케이션의 인증과 권한(인가)를 담당하는 스프링 하위 프레임워크

## 용어 설명
| 용어                 | 설명                                                           |
| -------------------- | -------------------------------------------------------------- |
| 인증(Authenticate)   | 접근하는 사용자가 본인이 맞는지 확인하는 절차                  |
| 인가(Authorization)  | 인증된 사용자가 요청한 자원에 접근 가능한 권한을 확인하는 절차 |
| 접근 주체(Princial)  | 보호받는 Resource에 접근하는 대상                              |
| 비밀번호(Credential) | Resource에 접근하는 대상의 비밀번호                            |

![[Pasted image 20231005111628.png]]

>Spring Security는 기본적으로 인증 절차를 거친 후에 인가 절차를 진행하게 되며, 인가 과젱에서 해당 리소스에 대한 접근 권한이 있는지 확인을 하게 된다. Spring Security에서는 이러한 인증과 인가를 위해 Principal을 아이디로, Credential을 비밀번호로 사용하는 **Credential 기반의 인증 방식**을 사용한다.

## 아키텍처
>Spring Security는 기본적으로 세션 - 쿠키 방식으로 인증을 한다

![[Pasted image 20231005113050.png]]

> 1. 사용자가 로그인을 요청 (Http Request)
> 2. AuthenticationFilter에서 UsernamePasswordAuthentication Token을 생성
> 3. AuthenticationManager에 전달
> 4. AuthenticationProvider에 유저 인증 요청
> 5. UserDetailService를 통해 User 조회 요청
> 6. UserDetails에서 User(db)를 통해 조회
> 7. User에 요청 정보가 있는 경우 유저 session을 생성
> 8. 인증이 성공할 경우 성공 UsernameAuthenticationToken 을 생성하여 AuthenticationManager로 전달
> 9.  UsernameAuthenticationToken을 AuthneticationFilter로 전달
> 10. 전달받은 UsernameAuthentication 을 LoginSuccessHandler 로 전송하고, spring security 인메모리 세션저장소인 SecurityContextHolder 에 저장
> 11. 유저에게 session ID 와 response

### AuthenticationFilter
> - 모든 Request 는 인증과 인가를 위해서 이 필터를 통과
> - SecurityContext 에 사용자의 세션 ID 가 있는지 확인 하고 세션 ID 가 없는 경우 다음 로직 수행
> - 인증 성공하는 경우 인증된 Authentication 객체를 SecurityContext 에 저장 후 AuthenticationSuccessHandler 실행
> - - 인증 실패하는 경우 AuthenticationFailureHandler 실행
### ### UsernamePasswordAuthenticationToken



## 참고
- [WebSecurity HTTP Security 차이](https://velog.io/@gkdud583/HttpSecurity-WebSecurity%EC%9D%98-%EC%B0%A8%EC%9D%B4) 
- https://velog.io/@soyeon207/SpringBoot-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0%EB%9E%80
- https://wikidocs.net/162150
- https://mangkyu.tistory.com/76
- https://gngsn.tistory.com/160