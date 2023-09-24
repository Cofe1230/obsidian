# [[MapStruct]] 옵션 및 정책 설정
## ComponentModel
>매퍼를 빈으로 만들어야 하는 경우

```java
@Mapper(componentModel = "spring" )
public interface BoardMapper {
	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```

## unmmappedTargetPolicy
> Target이 되는 필드에 대한 정책  
> Target 필드는 존재하는데 source로 채워지지 않는 경우 적용되는 정책이다

>[!note] 정책 옵션
>- ERROR : 매핑 대상이 없는 경우, 매핑 코드 생성시 ERROR가 발생합니다.
>- WARN(default) : 매핑 대상이 없는 경우, 빌드시 WARNNING이 발생합니다. (DEFAULT)
>- IGNORE : 매핑 대상이 없는 경우 무시하고 매핑됩니다.

```java
@Mapper(unmappedTargetPolicy = ReportingPolicy.{ERROR,WARN,IGNORE} )
public interface BoardMapper {
	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```
## unmappedSourcePolicy
>target필드 속성으로 채우지 않는 source필드 속성이 있을 경우 적용되는 정책

>[!note] 정책 옵션
>- ERROR : 매핑되지 않는 source 필드가 있을 경우 빌드시 ERROR가 발생합니다
>- WARN(default) : 매핑되지 않는 source 필드가 있을 경우 빌드시 WARNING이 발생합니다
>- IGNORE : 매핑되지 않는 source필드가 있더라도 무시합니다
```java
@Mapper(unmappedSourcePolicy = ReportingPolicy.{ERROR,WARN,IGNORE} )
public interface BoardMapper {
	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```
## nullValueMappingStrategy
> Source가 NULL인 경우 제어할 수 있는 옵션

>[!note] 정책 옵션
>- RETURN_NULL(default) : Source Argument가 NULL일때 NULL을 return 한다
>- RETURN_DEFAULT : default값이 매핑 된다
>- beanmapping, itrables, arrays, map 은 [공식문서참조](https://mapstruct.org/documentation/stable/reference/html/#_using_mapstruct_with_the_java_module_system)

```java
@Mapper(nullValueMappingStrategy=NullValueMappingStrategy.{RETURN_NULL,RETURN_DEFUALT})
public interface BoardMapper extends GenericMapper<BoardDTO, BoardEntity> {
	BoardMapper INSTANCE = Mappers.getMapper(BoardMapper.class);
	BoardDTO toBoardDTO( BoardEntity boardEntity);
}
```
## nullValuePropertyMappingStrategy
>Source Property가 NULL인 경우 제어할 수 있는 옵션

>[!note] 정책 옵션
>SET_TO_NULL (default) : target property가 null로 set된다
>SET_TO_DEFAULT : 빈 값을 생성한다
>IGNORE : 원래 값과 동일하기 매핑된다.

## 이 외의 NULL 제어
> [공식문서참조](https://mapstruct.org/documentation/stable/reference/html/#_using_mapstruct_with_the_java_module_system) 10.6 ~


## 여러 매퍼를 하나의 같은 옵션에 묶기
>Mapper가 많아져서 Mapper마다의 설정 중 중복이 발생할 때 속성 설정을 공통화 할 수 있다  
>@MapperConfig 어노테이션을 사용해서 공통화할 속성을 설정해주고 config = 로 옵션 클래스를 설정해 준다

```java
@MapperConfig(unmappedTargetPolicy = ReportingPolicy.ERROR, uses = JsonMapper.class)
public interface MapStructMapperConfig {
}

@Mapper(config = MapStrutMapperConfgi.class )
public interface BoardMapper {
	BoardDTO toBoardDTO( BoardEntity boardEntity );
}
```
