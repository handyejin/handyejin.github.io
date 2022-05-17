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
