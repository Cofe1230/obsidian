# [ [[SpringBoot]] ] MapStruct
## MapStruct란?
> configuration 접근 방식의 규칙을 기반으로 한 Java bean 타입의 매핑 구현을 단순화 해주는 코드생성기
### 특징
>- 컴파일 시점에 코드를 생성하여 런타임에서 안정성을 보장한다
>- 다른 매핑 라이브러리보다 속도가 빠르다
>- 반복되는 객체 매핑에서 발생하는 실수를 줄이고, 구현 코드를 자동으로 만들어 주기 때문에 사용이 쉽다
>- Annotation processor를 이용하여 객체 간 매핑을 자동으로 제공
>- Lombok의 getter, setter, builder를 이용하기 때문에 Lombok이 필수에 MapStruct보다 먼저 dependency가 추가되어야 한다
## 사용 방법
### [[MapStruct 기본 설정]]
### [[MapStruct 사용 방법]]
### [[MapStruct 테스트]]
## 참고
##### 참고
https://medium.com/naver-cloud-platform/%EA%B8%B0%EC%88%A0-%EC%BB%A8%ED%85%90%EC%B8%A0-%EB%AC%B8%EC%9E%90-%EC%95%8C%EB%A6%BC-%EB%B0%9C%EC%86%A1-%EC%84%9C%EB%B9%84%EC%8A%A4-sens%EC%9D%98-mapstruct-%EC%A0%81%EC%9A%A9%EA%B8%B0-8fd2bc2bc33b
https://marklee1117.tistory.com/121
- 단위 테스트 : https://marklee1117.tistory.com/121
- https://dev-splin.github.io/spring/Spring-ModelMapper,MapStruct/