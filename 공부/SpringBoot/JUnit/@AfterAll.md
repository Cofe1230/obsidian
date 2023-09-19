# @AfterAll
## 설명
- 해당 annotation이 달린 메서드가 모든 테스트 이후에 실행된다
- 해당 메서드는 static이어야 한다
- 이전의 @AfterClass와 동일
## Example
```java
@AfterAll
static public void afterAll() {
System.out.println("##AfterAll Annotation 시작");
}
```
