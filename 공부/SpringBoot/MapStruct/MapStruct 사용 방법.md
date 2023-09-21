# [ [[MapStruct]] ] 사용 방법
## 1. 기본 사용 방법
>**BoardEntity** ==> **BoardDTO** 매핑
```java
@Getter
@Builder
public class BoardEntity {
	private long board_id;
	private String title;
	private String content;
}
```
```java
@Getter @Setter
@Builder
public class BoardDTO {
	private long board_id;
	private String title;
	private String content;
}
```
>Mapper Interface를 만들어 준다
```java
@Mapper
public interface BoardMapper {
	BoardMapper INSTANCE = Mappers.getMapper( BoardMapper.class );
	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```
>Mapper Interface에 @Mapper를 붙이면 자동으로 구현체를 생성해준다
