# @PostConstruct 과 @Transactional 사용시 유의할점

**TestService.java**
```java
@Service
@Transactional
@RequiredArgsConstructor
public class TestService {
  
  private final TestRepository testRepository;
  
  public void save(Test test) {
    testRepository.save(test);
  }
  
}
```

**TestRepository.java**
```java
@Repository
@RequiredArgsConstrucor
public class TestRepository {
  private final EntityManager em;
  
  public void save(Test test) {
    em.persist(test);
  }
}
```


**TestData.java**
```java
@Component
@Transactional
@RequiredArgsConstructuctor
public class TestData {

  private final TestService testService;
  private final TestRepository testRepository;
  
  @PostConstruct
  public void init() {
    Test test = new Test();
    test.setName("테스트");
    
    // 정상 작동 
    testService.save(test);
    
    // 정상 작동 X
    testRepository.save(test);
  }

}
```

TestData 의 init 메소드에서<br>
TestService > TestRepository 를 통해서 save 하면 Test 가 저장되지만, <br>
TestRepostiory 를 통해서 save 하면 Test 가 정상적으로 저장되지 않음 

|파일이름|트랜잭션 여부|
|---|---|
|TestData|O|
|TestService|O|
|TestRepository|X|

@Transactional 이 걸린 메소드에서 다른 메소드를 호출시<br>
호출된 메소드에 @Transactional 이 걸려 있으면 -> 두 메소드는 다른 트랜잭션<br>
걸려 있지 않다면 -> 동일한 트랜잭션

TestRepository에는 @Transactional이 없기 때문에 빈 등록 시 스프링 컨테이너는 EntityManager에 우선 가짜 프록시 객체를 주입해준 다음, <br>
다른 트랜잭션에서 해당 Repository에 접근하게 되면 그 트랜잭션의 EntityManager를 호출하여 트랜잭션을 처리해준다. 

저장되지 않는 이유는 @Transactional 과 @PostConstruct 가 적용되는 시점이 다르기 때문이다. 

@PostConstruct는 해당 빈 자체만 생성되었다고 가정하고 호출된다. <br>
이 말은 해당 빈에 관련된 AOP 등을 포함하여, 전체 스프링 애플리케이션 컨텍스트가 초기화되었다는 것을 보장해주지 않는다는 의미이다.

반면, AOP는 스프링의 후 처리기(Post Processer)가 완전히 동작을 끝낸 다음, <br>
스프링 애플리케이션 컨텍스트의 초기화가 완료되어야 적용된다.

즉, @PostConstruct가 먼저 실행되고 이후에 @Transactional AOP가 적용되기 때문에 새로운 Transaction 을 실행하지 않는 경우 (MemberRepository 를 바로 호출하는 경우) 에는 정상적으로 실행이 되지 않을 수 있다. 

라이프사이클을 우회해서 사용하는 방법은 아래와 같다. 
- AOP를 사용하지 않고 트랜잭션을 직접 코딩하는 방법
- 애플리케이션 컨텍스트가 완전히 초기화된 이벤트를 받아서 호출하는 방법
- 초기화하는 메서드와 초기화를 실행하는 메서드를 분리하는 방법

### 🙇‍♀️ 레퍼런스 
- [[Spring] @PostConstruct에서 @Transactional 처리 시 문제점](https://sorjfkrh5078.tistory.com/311)
- [@Transacional의 범위에 대해서 궁금한 점이 하나 있습니다!](https://www.inflearn.com/questions/270247)


