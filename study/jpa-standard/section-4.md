# 섹션 4. 엔티티 매핑

엔티티 매핑 종류

* 객체와 테이블 매핑
* 필드와 컬럼 매핑
* 기본 키 매핑
* 연관관계 매핑

## 객체와 테이블 매핑

#### @Entity

클래스에 `@Entity` 을 지정해주면 JPA가 해당 클래스를 관리하고 테이블과 매핑해준다.

```java
@Entity(name = "Member") // name: 엔티티 이름 지정, 기본값으로 클래스 이름 사용
public class Member {}
```

{% hint style="warning" %}
**주의**

* 기본 생성자 필수
* final 클래스, enum, interface, inner 클래스 사용  X
* 저장할 필드에 final 사용 X
{% endhint %}

#### @Table

엔티티와 매핑할 테이블을 지정한다. 해당 어노테이션을 생략하면 엔티티명을 테이블명으로 사용한다.

```java
@Entity
@Table(name = "Member")  // 기본값: 엔티티 이름
public class Member{}
```

<table><thead><tr><th width="237">속성</th><th width="313">기능</th><th width="170">기본값</th></tr></thead><tbody><tr><td>name</td><td>매핑할 테이블 이름</td><td>엔티티 이름을 사용</td></tr><tr><td>catalog</td><td>데이터베이스 catalog 매핑</td><td></td></tr><tr><td>schema</td><td>데이터베이스 schema 매핑</td><td></td></tr><tr><td>uniqueConstraints(DDL)</td><td>DDL 생성 시에 유니크 제약 조건 생성</td><td></td></tr></tbody></table>

