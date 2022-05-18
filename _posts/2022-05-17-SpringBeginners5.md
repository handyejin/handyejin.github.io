---
layout: post
title: Spring 스프링 DB 접근 기술
date: 2022-05-17 12:07:00
category: Spring
---

# 순수 JDBC

##### build.gradle 파일에 jdbc, h2데이터베이스 관련 라이브러리 추가

```java
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	runtimeOnly 'com.h2database:h2'
```

##### 스프링 부터 데이터베이스 연결 설정 추가(`resources/application.properties`)

```java
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

##### Jdbc리포지토리 구현

- 20년전 기술이므로, 참고만 한다.

```java
package hello.hellospring.repository;
import hello.hellospring.domain.Member;
import org.springframework.jdbc.datasource.DataSourceUtils;
import javax.sql.DataSource;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;
public class JdbcMemberRepository implements MemberRepository {
    private final DataSource dataSource;
    public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }
    @Override
    public Member save(Member member) {
        String sql = "insert into member(name) values(?)";

        Connection conn = null;
        PreparedStatement pstmt = null;

        //결과를 받는다.
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql,
                    Statement.RETURN_GENERATED_KEYS);

            pstmt.setString(1, member.getName());
            pstmt.executeUpdate();
            rs = pstmt.getGeneratedKeys();

            if (rs.next()) {
                member.setId(rs.getLong(1));
            } else {
                throw new SQLException("id 조회 실패");
            }
            return member;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
    @Override
    public Optional<Member> findById(Long id) {
        String sql = "select * from member where id = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setLong(1, id);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            } else {
                return Optional.empty();
            }
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
    @Override
    public List<Member> findAll() {
        String sql = "select * from member";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            rs = pstmt.executeQuery();
            List<Member> members = new ArrayList<>();
            while(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                members.add(member);
            }
            return members;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
    @Override
    public Optional<Member> findByName(String name) {
        String sql = "select * from member where name = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, name);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            }
            return Optional.empty();
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
    private Connection getConnection() {
        return DataSourceUtils.getConnection(dataSource);
    }
    private void close(Connection conn, PreparedStatement pstmt, ResultSet rs)
    {
        try {
            if (rs != null) {
                rs.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (pstmt != null) {
                pstmt.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (conn != null) {
                close(conn);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    private void close(Connection conn) throws SQLException {
        DataSourceUtils.releaseConnection(conn, dataSource);
    }
}
```

##### 스프링 설정 변경(`SpringConfig.java`)

```java
package hello.hellospring;


import hello.hellospring.repository.JdbcMemberRepository;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class SpringConfig {

    private DataSource dataSource;

    @Autowired
    public SpringConfig(DataSource dataSource){
        this.dataSource = dataSource;
    }

    @Bean//스프링 빈을 등록한다고 의미함.
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){

//        return new MemoryMemberRepository();
        return new JdbcMemberRepository(dataSource);
    }
}
```

- DataSource

  - 데이터베이스 커넥션을 획득할 때 사용하는 객체
  - 스프링이 데이터베이스 커넥션 정보를 바탕으로, 자체적으로 DataSource를 스프링 빈으로 생성해준다. 이를 생성자로 주입해 사용한다.

<div style="height:6px;"></div>

- 다른 코드들을 변경하지 않고, 인트페이스를 확장해서 `JdbcMemberRepository.java`를 만들고 `SpringConfig.java`만 수정했다.
  - 객체지향의 다형성을 편하게 활용할 수 있기 때문에 스프링을 사용한다.
  - 스프링의 **DI**를 사용함으로써 기존 코드들을 변경하지 않고, 설정만으로 구현 클래스를 변경할 수 있다.

<div style="height:24px;"></div>

# 스프링 통합 테스트

스프링 컨테이너와 DB까지 연결한 **통합 테스트**를 진행한다.

##### 회원 서비스 스프링 통합 테스트

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Commit;
import org.springframework.transaction.annotation.Transactional;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;


@SpringBootTest
@Transactional
public class MemberServiceIntegrationTest {

  //테스트를 할 때는 제일 편한 방법으로 한다.
  @Autowired MemberService memberService ;
  @Autowired MemberRepository memberRepository;


  @Test
  void 회원가입() {
      //given
      Member member = new Member();
      member.setName("spring");

      //when
      long saveId = memberService.join(member);

      //then
      Member findMember = memberService.findOne(saveId).get();
      assertThat(member.getName()).isEqualTo(findMember.getName());
  }

  @Test
  //@Commit : 해주면 끝나면 commit 한다.
  public void 중복_회원_예외(){
      //given
      Member member1 = new Member();
      member1.setName("spring");

      Member member2 = new Member();
      member2.setName("spring");

      //when
      memberService.join(member1);

      //member2를 넣으면 IllegalStateException 예외가 터져야함
      IllegalStateException e = assertThrows(IllegalStateException.class, ()-> memberService.join(member2));
      assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
  }
}
```

- `@SpringBootTest`: 스프링 컨테이너와 테스트를 함께 실행한다.
- `@Transactional`: 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. <br>
  DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
- `@Commit`을 붙인 테스트는 롤백하지 않고 변경내용을 DB에 반영하도록 설정할 수 있다.

<div style="height:6px;"></div>

#### 단위테스트

<div style="height:2px;"></div>

**단위테스트**는 스프링을 띄우지 않고 자바 코드로만 진행하며 가장 작은 단위의 테스트로만 진행하는 것이다.

<div style="height:4px;"></div>

이에반해, **통합테스트** 는 스프링도 띄우고 DB도 연동하여 진행하는 테스트이다.

- **보통 순수한 단위테스트가 더 좋은 테스트일 확률이 높다.**

<div style="height:24px;"></div>

# 스프링 JdbcTemplate

- 순수 Jdbc와 동일하게 환경설정을 한다.
- 스프링 JdbcTemplate과 MyBatis같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다.
- 하지만 SQL문은 직접 작성해야 한다.

##### 스프링 JdbcTemplate 회원 레포지토리

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;

import javax.sql.DataSource;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Optional;

public class JdbcTemplateMemberRepository implements MemberRepository{

    private final JdbcTemplate jdbcTemplate;

    //생성자가 1개만 있으면 autowired생략 가능
    @Autowired
    public JdbcTemplateMemberRepository(DataSource dataSource) {
        jdbcTemplate = new JdbcTemplate(dataSource);
    }

    @Override
    public Member save(Member member) {
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

        Map<String,Object> parameters = new HashMap<>();
        parameters.put("name",member.getName());

        Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
        member.setId(key.longValue());
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
//        return Optional.empty();
        List<Member> result =  jdbcTemplate.query("select * from member where id= ?", memberRowMapper(),id);
        //Optional로 바꿔서 반환.
        return result.stream().findAny();
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = jdbcTemplate.query("select * from member where name = ?",memberRowMapper(),name);
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member",memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum)->{
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
    }
}
```

- 생성자에서 JdbcTemplate 대신 DataSource를 인젝션 받는다.
- JdbcTemplate를 생성할 때도 dataSource를 인젝션 받는다.

  ```java
  List<Member> result =  jdbcTemplate.query(sql,결과,파라미터);
  ```

<div style="height:4px;"></div>

##### JdbcTemplate 을 사용하도록 스프링 설정 변경

```java
@Configuration
public class SpringConfig {

    private final DataSource dataSource;

    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
}

    @Bean
    public MemberRepository memberRepository() {
    // return new MemoryMemberRepository();
    // return new JdbcMemberRepository(dataSource);
    return new JdbcTemplateMemberRepository(dataSource);
    }
}
```

<div style="height:24px;"></div>

# JPA

- **JPA**는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다.

  - 스프링 JdbcTemplate을 사용해 반복 코드는 줄었지만, SQL문은 직접 작성해야한다. JPA를 사용하면 SQL문을 직접 작성하지 않아도 된다.
  - CRUD기능의 SQL문을 직접 작성할 필요가 없다.

  <div style="height:10px;"></div>

- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.

  <div style="height:10px;"></div>

- JPA를 사용하면 개발 생산성을 크게 높일 수 있다.

#### build.gradle 파일에 JPA, h2 데이터베이스 관련 라이브러리 추가

```java
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
runtimeOnly 'com.h2database:h2'
```

  <div style="height:6px;"></div>

#### 스프링 부트에 JPA 설정 추가

```java
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none
```

- **`show-sql`**: JPA가 생성하는 SQL을 출력
- **`ddl-auto`**: JPA는 테이블을 자동으로 생성하는 기능을 제공한다.
  - **`none`**: 해당 기능 끔
  - **`create`**: 엔티티 정보를 바탕으로 테이블 직접 생성

##### JPA 엔티티 매핑

```java
package hello.hellospring.domain;

import javax.persistence.*;

//ORM

@Entity //jpa가 관리하는 엔티티
public class Member {

    //시스템이 저장하는 id값

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY) //db가 id를 자동으로 생성해주는 것을 identity전략이라고한다.
    private Long id;

    @Column(name = "name")
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

- JPA를 사용하기 위해 엔티티 Entity를 맵핑 해야함
- JPA 는 **ORM**(Object Relational Mapping) 기술을 사용
  - **ORM**: 객체(Object)와 관계형(Relational) 데이터베이스의 테이블을 맵핑(Mapping)하는 기술
- 자바 코드에 선언한 어노테이션들을 보고, 괒계형 데이터베이스의 요소와 비교

<div style="height:6px;"></div>

- <span style="font-size:20px;">**`@Entity`**</span> - 해당 클래스 객체는 JPA가 관리하는 Entity가 된다.
- <span style="font-size:20px;">**`@Id`**</span> - SQL에서 선언한 `generated by default as identity`처럼, DB가 자동으로 생성해주는 값에 선언한다.

<div style="height:6px;"></div>

#### JPA 회원 리포지토리

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import javax.persistence.EntityManager;
import java.util.List;
import java.util.Optional;

public class JpaMemberRepository implements MemberRepository{

    //jpa는 EntityManager로 동작한다.
    //jpa를 쓰기 위해서 EntityManager을 주입 받아야한다.
    private final EntityManager em;

    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }

    @Override
    public Member save(Member member) {
        em.persist(member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        //find(조회할 타입, 식별자)
        Member member =  em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {
        // 값 찾을 때 :name 붙이기
        List<Member> result =  em.createQuery("select m from Member m where m.name = :name",Member.class)
                .setParameter("name",name)
                .getResultList();

        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        //객체를 대상으로 쿼리문을 날린다.
        // member객체 자체를 select하기 때문에 m으로 써도된다.
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }
}
```

- **EntityManager**

  - JPA는 **`EntityManager`**로 동작한다. 반드시 선언해주어야한다.
  - `build.gradle`에서 `data-jpa`를 설정해 두면, 스프링 부트가 자동으로 `EntityManager`을 만들어 설정한 DB와 통신을 편하게해준다.

    <div style="height:6px;"></div>

- **persist(e)**
  - DB에 해당 요소를 영속적으로 저장
    <div style="height:6px;"></div>
- **find(조회할 타입, PK)**

  - DB테이블에서 해당 타입에 `PK`를 갖는 요소를 찾아옴

     <div style="height:6px;"></div>

- **createQuery("JPQL쿼리문", 타입)**

  - `PK`를 사용하지 않고 조회할 때 `createQuery`를 사용해 **`JPQL`**을 작성해야한다.

  <div style="height:8px;"></div>

- **JPQL**
  - 객체(엔티티)를 대상으로 쿼리문을 보낸다.

<div style="height:8px;"></div>

#### 서비스 계층에 트랜잭션 추가

```java
import org.springframework.transaction.annotation.Transactional

@Transactional
public class MemberService {}
```

- `org.springframework.transaction.annotation.Transactional`을 사용
- 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋한다. 만약 런타임 예외가 발생하면 롤백한다.
- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.

<div style="height:6px;"></div>

#### JPA를 사용하도록 스프링 설정 변경

```java
package hello.hellospring;


import hello.hellospring.repository.*;
import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.sql.DataSource;

@Configuration
public class SpringConfig {

    private EntityManager em;

    public MemberRepository memberRepository(){
        return new JpaMemberRepository(em);
    }
}
```
