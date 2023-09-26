# [ [[SpringBoot]] ] MapStruct
## MapStruct란? 
> [!note] MapSturct란?
> configuration 접근 방식의 규칙을 기반으로 한 Java bean 타입의 매핑 구현을 단순화 해주는 코드생성기
### 특징
>[!note] MapStruct의 특징
>- [[리플렉션]]을 사용하지 않고 컴파일 시점에 코드를 생성하여 런타임에서 안정성을 보장한다
>- 다른 매핑 라이브러리보다 속도가 빠르다
>- 반복되는 객체 매핑에서 발생하는 실수를 줄이고, 구현 코드를 자동으로 만들어 주기 때문에 사용이 쉽다
>- Annotation processor를 이용하여 객체 간 매핑을 자동으로 제공
>- Lombok의 getter, setter, builder를 이용하기 때문에 Lombok이 필수에 MapStruct보다 먼저 dependency가 추가되어야 한다
## 사용 방법
### [[MapStruct 기본 설정]]
### [[MapStruct Config 정책]]
### [[MapStruct 사용 방법]]
### [[MapStruct 테스트]]
## 참고
>[!note] 참고
>- 네이버 클라우드 플랫폼 : https://blog.naver.com/n_cloudplatform/222957490406
> - 단위 테스트 : https://marklee1117.tistory.com/121
> - mapstruct 참고 :  https://dev-splin.github.io/spring/Spring-ModelMapper,MapStruct/
> - date 변화 :https://aljjabaegi.tistory.com/670
> - @BeanMapping : https://mapstruct.org/documentation/stable/api/org/mapstruct/BeanMapping.html
> - 공식 문서 : https://mapstruct.org/documentation/stable/reference/html/#_using_mapstruct_with_the_java_module_system
> - @retention 사용한 예시가 있다 : https://mein-figur.tistory.com/entry/mapstruct-1
