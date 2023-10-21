# 섹션 2. JPA 시작하기

## 데이터베이스 방언

각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩 다르다. 이렇게 SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능을 방언이라고 한다.

{% hint style="info" %}
데이터베이스마다 어떤 부분이 다를까?

* 가변 문자: MySQL은 `VARCHAR`, Oracle은 `VARCHAR2`
* 문자열을 자르는 함수: SQL 표준은 `SUBSTRING()`, Oracle은 `SUBSTR()`
* 페이징: MySQL은 `LIMIT`, Oracle은 `ROWNUM`
{% endhint %}

JPA는 특정 데이터베이스의 SQL을 방언을 통해 해결해주기 때문에 개발자는 특정 데이터베이스에 종속되지 않고 개발할 수 있다.

<figure><img src="../../.gitbook/assets/JPA 기본편 - 데이터베이스 방언.png" alt="" width="563"><figcaption></figcaption></figure>

## JPA 사용

JPA를 사용하기 위해선 persistence.xml에 있는 설정 정보를 토대로 `EntityManagerFactory`를 생성해야한다. 그리고 `EntityManager`를 만들어 준다. `EntityManager`를 통해 데이터베이스에 CRUD를 할 수 있다.

<figure><img src="../../.gitbook/assets/화면 캡처 2023-10-21 195135.png" alt="" width="563"><figcaption></figcaption></figure>

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
EntityManager em = emf.createEntityManager();
// code
em.close();
emf.close();
```

#### 맛보기실습

Member 클래스

```java
@Entity
public class Member {
    @Id
    private Long id;
    private String name;
    //Getter, Setter ...
}
```

Member 저장

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

try {
    Member member = new Member();
    member.setId(1L);
    member.setUsername("memberA");

    em.persist(member);     //member 저장
    tx.commit();
} catch (Exception e) {
    tx.rollback();  //예외 발생시 트랜잭션 롤백
} finally {
    em.close();
}

emf.close();
```

{% hint style="info" %}
스프링과 JPA를 함께 사용하면 트랜잭션 커밋, 롤백을 알아서 처리해준다.
{% endhint %}

Member 조회

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

try {
    Member findMember = em.find(Member.class, 1L);  //member 조회
    tx.commit();
} catch (Exception e) {
    tx.rollback();  //예외 발생시 트랜잭션 롤백
} finally {
    em.close();
}

emf.close();
```

Member 수정

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

try {
    Member findMember = em.find(Member.class, 1L);  //member 조회
    findMember.setUsername("hello");
    tx.commit();
} catch (Exception e) {
    tx.rollback();  //예외 발생시 트랜잭션 롤백
} finally {
    em.close();
}

emf.close();
```

Member 삭제

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

try {
    Member findMember = em.find(Member.class, 1L);  //member 조회
    em.remove(findMember);
    tx.commit();
} catch (Exception e) {
    tx.rollback();  //예외 발생시 트랜잭션 롤백
} finally {
    em.close();
}

emf.close();
```

{% hint style="danger" %}
**주의**

`EntityManagerFactory`는 하나만 생성해서 애플리케이션 전체에 공유해서 사용한다.

`EntityManager`는 쓰레드간에 공유하면 안된다.

JPA의 모든 데이터 변경은 트랜잭션 안에서 실행해야 된다.
{% endhint %}

## JPQL

JPQL은 SQL을 추상화한 언어로 테이블이 아닌 객체를 대상으로 하는 **객체 지향 쿼리**를 의미한다.

```java
List<Member> result = em.createQuery("select m from Member m", Member.class)
        .getResultList();
```



