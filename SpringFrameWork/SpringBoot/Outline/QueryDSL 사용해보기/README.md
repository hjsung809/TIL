# Query DSL 사용해보기

웹 애플리케이션에서 검색 조건은 다양하게 구성되고 시스템 운영 과정에서 변경되는 경우가 일반적이다. 다양한 검색 쿼리를 미리 등록해서 사용하면 비슷한 쿼리가 너무 많아 관리하기 어렵다. 그래서 동적 쿼리를 사용한다.

일반적으로 개발자들에게 많이 알려진 동적 쿼리는 마이바티스의 동적 쿼리다. 동적 태그를 이용하여 XML 파일에 등록되는 쿼리를 동적으로 구성할 수 있다. 조건에 따라서 쿼리의 특정 구문을 변형시킬 수 있기 때문에 유연한 쿼리를 사용할 수 있다. 

JPA에서는 @Query를 이용해서 애플리케이션에서 사용할 쿼리를 관리하는데, 이 쿼리는 프로젝트가 로딩되는 시점에 파싱되기 때문에 고정된 SQL만을 사용할 수 있다. 그래서 마이 바티스 처럼 동적을 쿼리를 처리하려면 QueryDSL(Query Domain Specific Language)을 이용해야한다.

QueryDSL은 오픈소스 프로젝트로 쿼리를 문자열이 아닌 자바 코드로 작성할 수 있는 일종의 JPQL 빌더이다. 지금은 JPA, JDO, JDBC Lucene, Hibernate Search, 자바 컬렉션 등을 다양하게 지원한다.



pom.xml에 Querydsl JPA와 Querydsl APT를 추가한다.

```xml
		<!-- https://mvnrepository.com/artifact/com.querydsl/querydsl-jpa -->
		<dependency>
		    <groupId>com.querydsl</groupId>
		    <artifactId>querydsl-jpa</artifactId>
		</dependency>
		<!-- https://mvnrepository.com/artifact/com.querydsl/querydsl-apt -->
		<dependency>
		    <groupId>com.querydsl</groupId>
		    <artifactId>querydsl-apt</artifactId>
		</dependency>
```

그리고 query DSL을 사용할 때 필요한 쿼리 타입이라는 쿼리용 클래스를 생성해줘야 하는데, 이는 플러그인이 엔티티 클래스를 기반으로 생성해준다. 이를위해 플러그인을 추가하자.

```xml
			<plugin>
				<groupId>com.mysema.maven</groupId>
				<artifactId>apt-maven-plugin</artifactId>
				<version>1.1.3</version>
				<executions>
					<execution>
						<goals>
							<goal>process</goal>
						</goals>
						<configuration>
							<outputDirectory>src/main/querydsl</outputDirectory>
							<processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
						</configuration>
					</execution>
				</executions>
			</plugin>
```

프로젝트를 저장하고 Maven -> Update Project를 실행해주면 플러그인이 다운로드되고,

src/main/querydsl 폴더가 생기고, 그 안에 엔티티 클래스를 기반으로한 쿼리 타입 클래스인 'Q엔티티 이름' 형태의 클래스가 생성된다.

![image-20210322154856645](README.assets/image-20210322154856645.png)



이제 동적 쿼리를 작성하고 테스트해보기 위해, 인터페이스를 새롭게 정의해야한다. QuerydslPredicateExcutor 인터페이스와 기존의 CrudRepository 인터페이스 2개를 상속하는 인터페이스를 생성한다.

```java
public interface DynamicBoardRepository extends CrudRepository<Board, Long>, QuerydslPredicateExecutor<Board>{
}
```

이 인터페이스는 CrudRepository가 생성해주는 메소드와 더불어 QuerydslPredicateExcutor가 제공해주는 메소드도 가지게 된다.

그 메소드는 아래를 보면 확인할 수 있다.

```java
public interface QuerydslPredicateExecutor<T> {

	/**
	 * Returns a single entity matching the given {@link Predicate} or {@link Optional#empty()} if none was found.
	 *
	 * @param predicate must not be {@literal null}.
	 * @return a single entity matching the given {@link Predicate} or {@link Optional#empty()} if none was found.
	 * @throws org.springframework.dao.IncorrectResultSizeDataAccessException if the predicate yields more than one
	 *           result.
	 */
	Optional<T> findOne(Predicate predicate);

	/**
	 * Returns all entities matching the given {@link Predicate}. In case no match could be found an empty
	 * {@link Iterable} is returned.
	 *
	 * @param predicate must not be {@literal null}.
	 * @return all entities matching the given {@link Predicate}.
	 */
	Iterable<T> findAll(Predicate predicate);

	/**
	 * Returns all entities matching the given {@link Predicate} applying the given {@link Sort}. In case no match could
	 * be found an empty {@link Iterable} is returned.
	 *
	 * @param predicate must not be {@literal null}.
	 * @param sort the {@link Sort} specification to sort the results by, may be {@link Sort#unsorted()}, must not be
	 *          {@literal null}.
	 * @return all entities matching the given {@link Predicate}.
	 * @since 1.10
	 */
	Iterable<T> findAll(Predicate predicate, Sort sort);

	/**
	 * Returns all entities matching the given {@link Predicate} applying the given {@link OrderSpecifier}s. In case no
	 * match could be found an empty {@link Iterable} is returned.
	 *
	 * @param predicate must not be {@literal null}.
	 * @param orders the {@link OrderSpecifier}s to sort the results by, must not be {@literal null}.
	 * @return all entities matching the given {@link Predicate} applying the given {@link OrderSpecifier}s.
	 */
	Iterable<T> findAll(Predicate predicate, OrderSpecifier<?>... orders);

	/**
	 * Returns all entities ordered by the given {@link OrderSpecifier}s.
	 *
	 * @param orders the {@link OrderSpecifier}s to sort the results by, must not be {@literal null}.
	 * @return all entities ordered by the given {@link OrderSpecifier}s.
	 */
	Iterable<T> findAll(OrderSpecifier<?>... orders);

	/**
	 * Returns a {@link Page} of entities matching the given {@link Predicate}. In case no match could be found, an empty
	 * {@link Page} is returned.
	 *
	 * @param predicate must not be {@literal null}.
	 * @param pageable may be {@link Pageable#unpaged()}, must not be {@literal null}.
	 * @return a {@link Page} of entities matching the given {@link Predicate}.
	 */
	Page<T> findAll(Predicate predicate, Pageable pageable);

	/**
	 * Returns the number of instances matching the given {@link Predicate}.
	 *
	 * @param predicate the {@link Predicate} to count instances for, must not be {@literal null}.
	 * @return the number of instances matching the {@link Predicate}.
	 */
	long count(Predicate predicate);

	/**
	 * Checks whether the data store contains elements that match the given {@link Predicate}.
	 *
	 * @param predicate the {@link Predicate} to use for the existence check, must not be {@literal null}.
	 * @return {@literal true} if the data store contains elements that match the given {@link Predicate}.
	 */
	boolean exists(Predicate predicate);
}
```



이제 테스트를 작성해보자.

```java
@Autowired
	private DynamicBoardRepository boardRepo;

@Test
	void testDynamicQuery() {
		String searchCondition = "CONTENT";
		String searchKeyword = "10";
		
		BooleanBuilder builder = new BooleanBuilder();
		QBoard qboard = QBoard.board;
		
		if(searchCondition.equals("TITLE")) {
			builder.and(qboard.title.like("%" + searchKeyword + "%"));
		} else if(searchCondition.equals("CONTENT")) {
			builder.and(qboard.content.like("%" + searchKeyword + "%"));
		}
		
		Pageable paging = PageRequest.of(0, 5);
		Page<Board> boardList = boardRepo.findAll(builder, paging);
		
		System.out.println("검색 결과");
		for(Board board : boardList) {
			System.out.println("--->" + board.toString());
		}
	}
```

BooleanBuilder를 통해 가변적인 파라미터 값에 따라 동적으로 AND나 OR에 해당하는 조건을 추가할 수 있다. 생성된 QBoard객체를 인자로 활용하여 다양한 조건을 추가할 수있다. Pageable객체 또한 마찬가지로 사용할 수 있고, DynamicRepository를 주입받고 QuerydslPredicateExecutor에 정의된 메소드에 Builder를 전달하면 우리가 원하는 쿼리를 실행할 수 있다.