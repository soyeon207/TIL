# Custom Repository 구현 

스프링 데이터 JPA 로 repository 를 구현하게 되면 보통 인터페이스만 정의하고, 구현체는 직접적으로 만들지 않는다.
하지만, QueryDSL 을 사용하거나 커스텀하게 구현하고 싶은 경우 메소드를 직접 구현해야 할 때도 있다. 

repository 를 직접 구현하게 되는 경우 공통 인터페이스가 제공하는 기능까지 모두 구현을 해야하는데, 스프링 데이터 JPA 는 필요한 메소드만 구현할 수 있는 방법을 제공한다. 

① 사용자 정의 인터페이스 작성 
```java
public interface MembreRepositoryCustom {
  public List<Member> findMemberCustom();
}
```
- 인터페이스 이름은 자유롭게 적으면 된다. 


② 사용자 정의 구현 클래스 
```java
public interface MemberRepository extends JPARepository<Member, Long>, MemberRepositoryCustom {

}
```
- 리포지토리 인터페이스 이름 + Impl 로 지어야 한다. 
- 해당 이름으로 구현하면 스프링 데이터 JPA 가 사용자 정의 구현 클래스로 인식한다. 

③ repositoryImterface 
```java
public class MemberRepositoryImpl implements MemberRepositoryCustom {
  public List<Member> findMemberCustom() {
    ...
  }
}
```
- 리포지토리 인터페이스에서 사용자 정의 인터페이스를 상속 받으면 된다. 

‼️ 만약 Impl 대신에 다른 이름을 붙이고 싶은 경우 repository-impl-postfix 속성을 변경하면 된다. 
```java
@EnableJpaRepositories(basePackages = "japbook.jpashop.repository", repositoryImplementationPostfix = "Impl")
```

