---
title: "ORM - JPA - Hibetnate 정리 (+ Spring Data JPA)"
toc: true
toc_sticky: true
date: 2025-01-21
categories: "jpa"
comments: true
---

`JPA`에 대해서 검색을 해보면 `ORM`과 `Hibernate`가 꼭 나오는데 이번 포스트에서는 이 개념들이 정확히 뭘 의미하고 `JPA`와 `Spring Data JPA`가 뭐가 다른건지 정리해 보도록 하겠다.

<p style="width:100%">
  <img src="/assets/images/tech/jpa/orm-jpa-hibernate/image.png">
</p>

## 📌 ORM (Object-Relational Mapping)

`ORM`은 객체 관계 매핑을 의미하는데 Java와 같은 객체지향 언어에서 객체와 데이터 베이스의 테이블을 자동으로 매핑하는 방법이다.

ORM을 사용하지 않을 때 어플리케이션에서 관리되는 객체(클래스)들은 테이블과 매핑되게 설계되지 않기 때문에 테이블과 **불일치**가 존재할 수 밖에 없다.

`ORM`은 엔티티를 사용하여 이러한 불일치를 해결하며 직접 쿼리를 작성하는 것이 아닌 메소드를 실행함으로써 CRUD를 실행할 수 있게 하는 방법이다.

## 📌 JPA (Java Persistence API)

`JPA`는 ORM을 Java에서 구현할 수 있도록 설계된 인터페이스라고 할 수 있다.

실제로 JPA를 정의한 javax.persistence는 대부분 interface, enum, exception, annotation 들로 이루어져 있다.

위와 같은 이유 때문에 바로 사용할 수 없다. 이를 구현한 구현체를 사용해야 되는데, 가장 대표적인 구현체가 `Hibernate`이다.

**JPA의 주요한 특징**

1. ORM의 기능을 제공한다
2. ORM을 위한 명세이기 때문에 특정 구현체에 종속되지 않는다.
3. 기본적인 CRUD 작업을 간단히 처리하며, 트랜잭션 및 지연 로딩 등 고급 기능을 제공한다.

## 📌 Hibernate

하이버네이트는 JPA를 구현한 ORM 프레임워크이다.

아래 코드는 간단한 하이버네이트 예제이다.

### 엔티티 클래스

```java
@Entity
public class User {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  private String name;
  private String email;

  // Getters and Setters
}
```

### 하이버네이트 코드

```java
public class HibernateExample {
  public static void main(String[] args) {
    SessionFactory sessionFactory = new Configuration().configure().buildSessionFactory();
    Session session = sessionFactory.openSession();

    Transaction transaction = session.beginTransaction();

    // Create
    User user = new User();
    user.setName("John");
    user.setEmail("john@example.com");
    session.save(user);

    // Read
    User retrievedUser = session.get(User.class, user.getId());
    System.out.println("Retrieved User: " + retrievedUser.getName());

    transaction.commit();
    session.close();
    sessionFactory.close();
  }
}
```

## 📌 Spring Data JPA

`Spring Data JPA`는 Spring에서 제공하는 모듈 중 하나이다.

JPA(예: Hibernate)를 기반으로 동작하며, 개발 생산성을 높이기 위해 편리한 기능을 제공하는 프레임워크이다.

Spring Data JPA는 JPA를 구현한 구현체가 아니기 때문에 Hibernate와 같은 구현체가 반드시 필요하다. (Spring Data JPA는 자체적으로 데이터베이스와 통신하지 않는다.)
