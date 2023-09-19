# @BeforeAll
## 설명
- 해당 메서드가 모든 테스트 메서드보다 가장 먼저 실행된다
- 해당 메서드는 static이어야 한다
- 이전의 @BeforeClass와 동일하다
## Example
```java
@BeforeAll
static public void beforeAll() {
System.out.println("##BeforeAll Annotation 시작");
}
```
