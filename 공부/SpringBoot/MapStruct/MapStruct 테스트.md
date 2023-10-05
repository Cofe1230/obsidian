## JUnit5 테스트 코드
```java
public class BoardDTOMapperTest {

	BoardMapper boardMapper = BoardMapper.INSTANCE;
	
	@Test
	@DisplayName("자동매핑 값이 제대로 들어가는가?")
	void boardEntitytoBoardDTOtest() {
	
	// given
	CommentEntity comment = CommentEntity.builder().id(1).content("comment test 01").build();
	CommentEntity comment2 = CommentEntity.builder().id(2).content("comment test 02").build();
	List<CommentEntity> commentList = new ArrayList<>();
	commentList.add(comment);
	commentList.add(comment2);
	
	AddressEntity address = AddressEntity.builder().addressId(1).addressName("addressTest01").build();
	BoardEntity board = BoardEntity.builder().
								id(1).
								title("title01").
								content("content01").
								comments(commentList).
								address(address).
								build();
	
	// when
	BoardDTO result = boardMapper.toBoardDTO(board);
	
	// then
	// assertThat(result.getId()).isEqualTo(board.getId());
	assertThat(result.getContent()).isEqualTo(board.getContent());
	assertThat(result.getTitle()).isEqualTo(board.getTitle());
	
	//list 테스트
	for(int i=0; i<result.getComments().size(); i++) {
	assertThat(result.getComments().get(i).getContent()).isEqualTo(board.getComments().get(i).getContent());
	}
	assertThat(result.getAddressId()).isEqualTo(board.getAddress().getAddressId());
	assertThat(result.getAddressName()).isEqualTo(board.getAddress().getAddressName());
	System.out.println("id : "+result.getId()+" content : "+result.getContent());
	}
}
```