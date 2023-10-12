# [ [[SpringDataJPA]] ] JPA Auditing
## 개요
>[!note] 개요
> 서비스를 운영할 때 **사용자의 기본적인 로그**를 DB에 남겨야 할 때가 있다. 이를테면 **마지막 로그인 시간**이라던지 **엔티티 생성 시간**, **변경된 시간과 변경한 사람의 이름**등등.
> 이 때, 생성일/수정일/생성자를 자동화할 때 사용하는게 바로 **JPA Auditing** 이다.
>
## SpringDataJPA 에서의 Auditing
> Spring Data JPA 에서는 위의 Auditing 기능을 제공한다.  
> Auditing은 Spring Data JPA 에서만 사용할 수 있는 개념은 아니다.  
> JPA 자체적으로도 Auditing 기능을 사용할 수 있지만 Spring Data JPA 에서는 더 깔끔하고 쉽게 제공한다.

## JPA Auditing 구현
### 1 . BaseEntity 추상 클래스를 생성한다
```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class TimeEntity {
	
	@CreatedDate
	@Column(updatable = false)
	private LocalDateTime createDate;
	
	@LastModifiedDate
	private LocalDateTime modifiedDate;
}
```
>[!note] @MappedSuperclass
>엔티티의 공통 매핑 정보가 필요할 때 주로 사용한다.
>
>즉, **부모 클래스(엔티티)에 필드를 선언하고 단순히 속성만 받아서 사용**하고싶을 때 사용하는 방법이다.

>[!note] @EntityListeners(AuditingEntityListener.class)
>엔티티를 DB에 적용하기 전, 이후에 **커스텀 콜백**을 요청할 수 있는 어노테이션이다.
>
>`@EntityListeners` 의 인자로 **커스텀 콜백**을 요청할 클래스를 지정해주면 되는데, Auditing 을 수행할 때는 JPA 에서 제공하는 `AuditingEntityListener.class` 를 인자로 넘기면 된다.

>[!note] @Column(updatable = false)
> 해당 BaseEntity를 JPA가 테이블에 접근하는 시점에만 JPA가 사용하도록 하고 싶은데 만약 개발자에 의해 수정되면 안되기 때문에 updatable을 false로 해주는 것을 권장한다.

- **`CreatedDate`**
    - 해당 엔티티가 생성될 때, 생성하는 시각을 자동으로 삽입해준다.
- **`CreatedBy`**
    - 해당 엔티티가 생성될 때, 생성하는 사람이 누구인지 자동으로 삽입해준다.
    - 생성하는 주체를 지정하기 위해서 `AuditorAware<T>` 를 지정해야 한다.
- **`LastModifiedDate`**
    - 해당 엔티티가 **수정될 때**, 수정하는 시각을 자동으로 삽입해준다.
- **`LastModifiedBy`**
    - 해당 엔티티가 **수정될 때**, 수정하는 주체가 누구인지 자동으로 삽입해준다.
        - 생성하는 주체를 지정하기 위해서 `AuditorAware<T>` 를 지정해야 한다.

### 2. 사용할 엔티티에 BaseEntity 상속
```java
@Entity
@Getter
@Builder
public class Board extends TimeEntity {

	@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
	private long boardId;
	private String title;
	private String content;
	private long hitCount;
	private long replyCount;
}
```
### 3. @EnableJpaAuditing 추가
```java
@EnableJpaAuditing
@SpringBootApplication
public class Study03H2Application {

	public static void main(String[] args) {
		SpringApplication.run(Study03H2Application.class, args);
	}
}
```
```java
@Configuration
@EnableJpaAuditing
public class JpaConfig {
}
```
> `@EnableJpaAuditing` 어노테이션을 Application에 직접 달면 단위 테스트시 문제가 발생하므로 별도 config 클래스를 생성해 어노테이션을 추가

## 참고
-  [JpaAuditing 기능으로 Entity 등록, 수정 시간 관리하기](https://rachel0115.tistory.com/entry/JPA-JpaAuditing-%EA%B8%B0%EB%8A%A5%EC%9C%BC%EB%A1%9C-Entity-%EB%93%B1%EB%A1%9D-%EC%88%98%EC%A0%95-%EC%8B%9C%EA%B0%84-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0#%40MappedSuperclass-1)
- [JPA 게시판 LocalDateTime format 변경하기](https://dev-coco.tistory.com/117)
- [JPA 엔티티에 자동으로 생성 날짜, 변경 날짜 추가하기](http://yoonbumtae.com/?p=2973)
- [JPA Auditing 으로 날짜/시간 처리 자동화](https://wecandev.tistory.com/121)
- [JPA Auditing 기능을 사용해서 생성, 수정 일자 자동화하기](https://wonit.tistory.com/484)
