## Junit이란?
- Java 진영의 대표적인 Test Framework  
- 단위 테스트 (Unit Test)를 위한 도구 제공  
- Annotation을 기반으로 테스트를 지원  
- Assert로 테스트 케이스의 기대값에 대해 수행 결과를 확인 할 수 있다  

## [[JUnit모듈]]  
- [[JUnit Jupiter]]  
- [[JUnit Platform]]  
- [[JUni Vintage]]  
## [[JUnit LifeCycle Annotation]]
| Annotation | Description                                         |
|:----------:|:--------------------------------------------------- |
|   [[@Test]]    | 테스트용 메소드를 표현                              |
| [[@BeforeEach]] | 각 테스트가 시작되기 전에 실행되어야 하는 메소드    |
| [[@AfterEach]] | 각 테스트 메소드가 시작된 후 실행되어야 하는 메소드 |
| [[@BeforeAll]] | 테스트 시작 전에 실행되어야 하는 메소드 (static)    |
| [[@AfterAll]]  | 테스트 종료 후에 실행되어야 하는 메소드 (static)    | 

## JUnit Main Annotation
- [[@SpringBootTest]]  
- [[@ExtendWith]]  
- [[@WebMvcTest]]  
- [[@AutoConfigureMockbean]]  
## 링크
https://goddaehee.tistory.com/212
- 순환 참조 문제 : https://k3068.tistory.com/32  
- N+1 문제 : https://programmer93.tistory.com/83
	- fetch join을 했을때 중복 문제 : strict
- 