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

## 2. 이름이 다른 경우
```java
@Mapper
public interface BoardMapper {

	// 이름이 다른 경우 직접 지정해준다. BoardEntity.title -> BoardDTO.board_title
	@Mapping(source = "title", target = "board_title")
	BoardDTO toBoardDTO(BoardEntity boardEntity);

	//이름이 다른게 여러개라면 @Mappings로 여러개를 바꿔주면 된다
	@Mappings({
		@Mapping(source = "title", target = "board_title"),
		@Mapping(source = "content", target = "board_content")})
	BoardDTO toBoardDTO(BoardEntity boardEntity);
}
```
## 3. 여러 객체를 하나에 매핑할 경우
```java
@Mapper
public interface BoardMapper {

	//필드명이 같아도 각각 지정하여 매핑할 수 있다
	@Mappings({
		@Mapping(source = "commentEntity.content", target = "comment_content"),
		@Mapping(source = "boardEntity.content", target = "board_content")})
	BoardDTO toBoardDTO(BoardEntity boardEntity,CommentEntity commentEntity);
}
```


## 4. 매핑에 여러 파라미터가 필요한 경우
```java
@Mapper
public interface BoardMapper {
	//필요한 파라미터를 추가한다
	@Mapping(source = "boardEntity.content", target = "board_content")
	BoardDTO toBoardDTO(BoardEntity boardEntity,long comment_id, String comment_content);
}
```

## 5.  객체를 매핑하거나 입출력이 다른 경우
