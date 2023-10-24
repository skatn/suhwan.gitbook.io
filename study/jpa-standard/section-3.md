# 섹션 3. 영속성 관리 - 내부 동작 방식

## 영속성 컨텍스트란?

* JPA를 이해하는데 가장 중요한 용어
* **엔티티를 영구 저장하는 환경** 이라는 뜻
* 엔티티 매니저를 통해서 영속성 컨텍스트에 접근

<figure><img src="../../.gitbook/assets/화면 캡처 2023-10-24 162705.png" alt=""><figcaption></figcaption></figure>

## 엔티티의 생명주기

* 비영속 (new/transient): 영속선 컨텍스트와 전혀 관계가 없는 새로운 상태
* 영속 (managed): 영속성 컨텍스트에 관리되는 상태
* 준영속 (detached): 영속성 컨텍스트에 저장되었다가 분리된 상태
* 삭제 (removed): 삭제된 상태

#### 비영속

객체를 생성한 상태

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
```

#### 영속

엔티티 매니저를 통해 영속성 컨텍스트에 저장한 상태

```java
em.persist(member);
```

#### 준영속

영속성 컨텍스트가 더이상 관리하지 않는 상태

```java
em.detach(member);
```

#### 삭제

영속성 컨텍스트에서 객체를 삭제한 상태

```java
em.remove(member);
```

## 영속성 컨텍스트의 이점

* 1차 캐시
* 동일성(identity) 보장
* 트랜잭션을 지원하는 쓰기 지연
* 변경 감지
* 지연 로딩

#### 1차 캐시

영속성 컨텍스트는 내부에 엔티티를 보관하는 1차 캐시라는걸 들고 있다. 네트워크를 통해 데이터베이스에 접근하는것 보다 서버 메모리에 접근하는 비용이 훨씬 싸기 때문에 1차 캐시를 통해 데이터베이스 접근 횟수를 줄여 성능을 개선할 수 있다. 1차 캐시는 Map의 형태로 @Id(PK)를 key로, 엔티티 객체를 value로 저장한다.

1차 캐시 조회

조회하려는 엔티티가 1차 캐시에 있으면 1차 캐시에서 바로 가져온다.

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");

em.persist(member); //영속화, 1차 캐시에 저장
Member findMember = em.find(Member.class, "member1"); //1차 캐시에서 조회
```

<figure><img src="../../.gitbook/assets/화면 캡처 2023-10-24 184517.png" alt=""><figcaption></figcaption></figure>

데이터베이스에서 조회

조회하려는 엔티티가 1차 캐시에 없으면 DB에서 조회 후 1차 캐시에 저장한다.

```java
Member findMember2 = em.find(Member.class, "member2");
```

<figure><img src="../../.gitbook/assets/화면 캡처 2023-10-24 185225.png" alt=""><figcaption></figcaption></figure>

