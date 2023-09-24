# [ [[MapStruct]] ] 사용 방법
## 1. Mapper 생성 옵션
### Mapper를 Bean으로 등록하려는 경우
>[!note] note
>componentModel을 spring으로 지정하면 spring bean등록 된다  
>@Mapper(componentModel = "spring" )
>
```java
@Mapper(componentModel = "spring" )
public interface BoardMapper {

	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```
### Bean등록 안하고 편리하게 호출하는 방법
```java
@Mapper
public interface BoardMapper {
	BoardMapper INSTANCE = Mappers.getMapper(BoardMapper.class);
	
}

public class BoardDTOMapperTest {
	BoardMapper boardMapper = BoardMapper.INSTANCE;
}
```
### generic Mapper 생성
>추가 매핑 설정이 필요 없는 경우 genericMapper로 간편하게 매퍼를 만들 수 있다

```java
public interface GenericMapper<D,E> {
	D toDto(E e);
	E toEntity(D d);
	updateEntityfromDto(D d,@MappingTarget E e);
}

@Mapper
public interface BoardMapper extends GenericMapper<BoardDTO, BoardEntity> {
}
```

## 2. 기본 사용 방법
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

## 3. 이름이 다른 경우
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
## 4. 여러 객체를 하나에 매핑할 경우
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


## 5. 매핑에 여러 파라미터가 필요한 경우
```java
@Mapper
public interface BoardMapper {
	//필요한 파라미터를 추가한다
	@Mapping(source = "boardEntity.content", target = "board_content")
	BoardDTO toBoardDTO(BoardEntity boardEntity,long comment_id, String comment_content);
}
```

## 6.  객체나 컬렉션을 매핑하는 경우

```java
@Getter
@Builder
public class BoardEntity {
	private long id;
	private String title;
	private String content;
	private AddressEntity address;
}
```
```java
@Getter @Setter
@Builder
public class BoardDTO {
	private long id;
	private String title;
	private String content;
	private AddressDTO address;
}
```

>AddressEntity 와 AddressDTO의 필드명이 동일해서 따로 매핑이 필요 없는 경우
```java
@Mapper
public interface BoardMapper {

	BoardDTO toBoardDTO(BoardEntity boardEntity);
}
```
>따로 추가할 필요 없이 자동으로 매핑된다

>Address 내부 Entity와 DTO내의 이름이 달라서 매핑이 필요한 경우
```java
@Getter @Setter
@Builder
public class AddressDTO {
	private long id;
	private String addr;
}

@Getter
@Builder
public class AddressEntity {
	private long id;
	private String address;
}

@Mapper
public interface BoardMapper {

	BoardDTO toBoardDTO(BoardEntity boardEntity);

	@Mapping(source = "address", target ="addr")
	AddressDTO toAddresDTO(AddressEntity addressEntity);
}
```
>필드 명이 객체의 필드를 매핑해주면 자동으로 매핑된다

>컬렉션이 있다면
```java
@Getter
@Builder
public class BoardEntity {
	private long id;
	private String title;
	private String content;
	@Builder.Default
	private List<CommentEntity> comments = new ArrayList<>();
}

@Getter @Setter
@Builder
public class BoardDTO {
	private long id;
	private String title;
	private String content;
	@Builder.Default
	private List<CommentDTO> comments = new ArrayList<>();
}
```
>이 때 commentEntity와  CommentDTO 의 필드의 달라서 매핑이 필요하다면
```java
@Mapper
public interface BoardMapper {
	BoardMapper INSTANCE = Mappers.getMapper(BoardMapper.class);
	BoardDTO toBoardDTO(BoardEntity boardEntity);
	//필요한 부분만 매핑시켜주면 List는 자동으로 매핑된다.
	@Mapping(source = "contents", target = "content")
	CommentDTO toCommentDTO(CommentEntity comment);
}
```
>컬렉션 구현 유형은 다음과 같다 필요하다면 참고

| 인터페이스 유형        | 구현 유형             |
| ---------------------- | --------------------- |
| Iterable               | ArrayList             |
| Collection             | ArrayList             |
| List                   | ArrayList             |
| Set                    | LinkedHashSet         |
| SoredSet               | TreeSet               |
| NavigableSet           | TreeSet               |
| Map                    | LinkedHashMap         |
| SoredMap               | TreeMap               |
| NavigableMap           | TreeMap               |
| ConcurrentMap          | ConcurrentHashMap     |
| ConcurrentNavigableMap | ConcurrentSkipListMap |
|                        |                       |
## 7. 기존에 있는 인스턴스에 업데이트 매핑
>새 인스턴스를 생성하지 않고 기존의 인스턴스를 업데이트 하는 매핑
> 타겟에 @MappingTarget 어노테이션 추가
```java
@Mapper
public interface BoardMapper {

	void updateBoardFromDTO(BoardDTO boardDTO,@MappingTarget BoardEntity boardEntity);
}
```
## 8. 객체에 들어있는 필드를 풀어서 매핑
> target에 "." 은 target this로 target의 필드의 이름을 찾아 매핑시켜 준다
> 직접 다 써줘도 된다
```java
@Mapper
public interface BoardMapper {

@Mapping(source = "address", target = ".")
BoardDTO toBoardDTO( BoardEntity boardEntity);
}
```
## 9. 자유로운 매핑을 위한 옵션
### 매핑 시 표현식을 사용해 매핑
```java
@Mapper
public interface BoardMapper {
	//entity title에 title이라는 글자를 추가해서 매핑한 것
	@Mapping(target = "title", expression = "java(boardEntity.getTitle()+\"title\")")
	BoardDTO toBoardDTO(BoardEntity boardEntity);
}
```
>[!danger] 표현식 사용시 표현식 안에 한글을 넣으면 깨진다
### 매핑 시 default값 지정
```java
@Mapper(imports = UUID.class)
public interface BoardMapper {
//defaultValue, defaultExpression으로 default값 지정 가능
@Mapping(source = "title", target = "title", defaultExpression = "java(UUID.randomUUID().toString())")
@Mapping(source = "content", target = "content", defaultValue = "No Content")
BoardDTO toBoardDTO(BoardEntity boardEntity);

}
```
![[Pasted image 20230922212557.png]]
### 매핑 시 특정 필드 매핑 무시
```java
@Mapper
public interface BoardMapper {
	//특정 필드를 무시해야 한다면 ignore을 사용하면 된다 
	@Mapping(source = "id", target = "id", ignore = true)
	BoardDTO toBoardDTO(BoardEntity boardEntity);
}
```
### 반대로 매핑할 때 특별한 로직 추가가 없다면
>@InheritInverseConfiguration 어노테이션
```java
@Mapper
public interface BoardMapper {

	@Mapping(source = "title", target = "board_title")
	BoardDTO toBoardDTO(BoardEntity boardEntity);

	@InheritInverseConfiguration
	BoardEntity toBoardEntity(BoardDTO boardDTO);
}
```
### 다른 Mapper를 이용해야 할 때
>uses 속성을 사용해서 uses = {className}.class 로 설정해주면 객체에서 Mapper를 사용한다

```java
//Json String 변화가 필요하다면 JsonMapper를 사용하면 된다
@Mapper(uses=JsonMapper.class )
public interface BoardMapper {

	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```

### 특정 필드 매핑 시 지정 메소드 이용
> qualifiedByName에 매핑 할 때 이용할 메소드를 지정해주고 커스텀 메소드에는 @Named()를 이용해 매핑에 사용할 메소드임을 알려준다
```java

@Mapping(source = "requestDto.type", target = "type", qualifiedByName = "typeToEnum") MessageListServiceDto toMessageListServiceDto(String messageId, Integer count, RequestDto requestDto); 

	@Named("typeToEnum") 
	static Type typeToEnum(String type) { 
		switch (type.toUpperCase()) { 
			case "LMS": 
				return Type.LMS; 
			case "MMS": 
				return Type.MMS; 
			default: 
				return Type.SMS; 
		}
	}
```
### 사용자 정의 매퍼 메서드
>매핑이 까다로운 경우 직접 구현하는 것이 필요할 때가 있다.  
>이런 경우 default를 붙여 매핑 메서드를 직접 구현한다.

```java

@Mapper
public interface JsonMapper {
    ObjectMapper OBJECT_MAPPER = new ObjectMapper();

    default String toString(Object obj) {
        try {
            return OBJECT_MAPPER.writeValueAsString(obj);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```
