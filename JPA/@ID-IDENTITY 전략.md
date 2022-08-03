# @ID IDENTITY 전략 

### 💡 IDENTITY 전략과 최적화<br>
IDENTITY 전략은 식별자 값을 얻기 위해서 추가로 DB 를 조회해야 한다.<br>
JDBC3 에 추가된 Statement.getGeneratedKeys() 를 사용하면 데이터를 저장하면서 동시에 생성된 기본 키 값도 얻어올 수 있다.

### 🚨 쓰기지연<br>
엔티티가 영속 상태가 되려면 식별자가 필요한데, IDENTITY 식별자 생성 전략은 엔티티를 DB 에 저장해야지만 식별자를 구할 수 있기 때문에<br>
em.persist() 를 호출하는 즉시 INSERT 쿼리 문이 DB 에 전달된다.<br>
그렇기 때문에 이 전략은 트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.
