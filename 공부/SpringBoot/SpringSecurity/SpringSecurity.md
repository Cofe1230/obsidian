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
> 3. AuthenticationManager로 전달
> 4. 

## 참고
- [WebSecurity HTTP Security 차이](https://velog.io/@gkdud583/HttpSecurity-WebSecurity%EC%9D%98-%EC%B0%A8%EC%9D%B4) 
- 