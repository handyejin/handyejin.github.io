---
layout: post
title: Spring 스프링 빈과 의존관계
date: 2022-05-16 12:04:00
category: Spring
---

> 컴포넌트 스캔과 자동 의존관계 설정<br>
> 자바 코드로 직접 스프링 빈 등록하기<br>

# 컴포넌트 스캔과 자동 의존관계 설정

### 의존 관계

Controller는 Service를 통해 회원가입을 하고, 데이터를 조회할수 있다. <br>
이를 **의존관계가 있다**고 할 수 있다.

- `Autowired`: 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체의 의존관계를 외부에서 넣어주는 것을 **DI**, 의존성 주입이라고 한다.

### 컨트롤러, 서비스, 리포지토리

- 컨트롤러, 서비스, 리포지토리는 정형화된 패턴이기 때문에 스프링에서 어노테이션을 제공한다.

  - `@Controller`: 외부 요청을 받는다.
  - `@Service`: 비지니스 로직을 만들고 수행한다.
  - `@Repository`: 데이터를 저장한다.

### 회원 컨트롤러에 의존 관계 추가

```java
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

//DI 의존 관계를 주입해준다.

@Controller
public class MemberController {
    //new로 하지말고 spring 컨테이너에 등록하고 쓰면 된다.
    private final MemberService memberService;


    //autowired: 컨트롤러와 서비스를 연결해준다.
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

- `@Controller` 어노테이션으로 컨트롤러를 만들면<br>
  스프링 부트가 실행될 때 스프링 컨테이너가 해당 컨트롤러 객체를 생성해 스프링 컨테이너에 넣어 해당 객체는 스프링이 관리한다. <br>
  이를 `스프링 컨테이너에서 스프링 빈이 관리된다` 라고 한다

#### 회원 서비스 스프링 빈 등록

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;


//스프링이 memberService를 멤버서비스에 등록해준다.
@Service
public class MemberService {
    private final MemberRepository memberRepository ;


    //외부에서 repository를 넣어주도록 함.
    @Autowired
    public MemberService(MemberRepository memberRepository) {

        this.memberRepository = memberRepository;
    }
```

#### 회원 리포지토리 스프링 빈 등록

```java
@Repository
public class MemoryMemberRepository implements MemberRepository{}
```

<div style="height:4px"></div>

# 스프링 빈을 등록하는 2가지 방법

- 컴포넌트 스캔과 자동 의존관계 설정

  - `@Component` 어노테이션이 있으면 스프링 빈으로 자동 등록된다.
  - `@Component`를 포함하는 어노테이션도 스프링 빈으로 자동 등록된다. - `@Controller` - `@Service` - `@Repository`

  > 스프링이 컴포넌트를 스캔할 때 `@SpringBootApplication` 클래스의 패키지 및 하위 패키지의 파일들만 스캔

  > 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때 **싱글톤**으로 등록한다.(유일하게 하나만 등록해서 공유한다)<br> 따라서 같은 스프링빈이면 모두 같은 인스턴스다.

  <div style="height:9px"></div>

- 자바 코드로 직접 스프링 빈 등록하기

<div style="height:4px"></div>

<div style="height:12px"></div>

# 자바 코드로 직접 스프링 빈 등록하기

회원 리포지토리와 회원 서비스의 `@Service`, `@Repository`, `@Autowired` 애노테이션을 제거하고 진행한다.

- `SpringConfig.java`를 만들고 해당 클래스를 `Configuration`으로 선언한다. <br>
  그리고 등록할 인스턴스를 `@Bean`으로 선언해 스프링 빈을 등록한다.

  ```java
  package hello.hellospring;


  import hello.hellospring.repository.MemberRepository;
  import hello.hellospring.repository.MemoryMemberRepository;
  import hello.hellospring.service.MemberService;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;

  @Configuration
  public class SpringConfig {


      @Bean//스프링 빈을 등록한다는 의미
      public MemberService memberService(){
          return new MemberService(memberRepository());
      }

      @Bean
      public MemberRepository memberRepository(){
          return new MemoryMemberRepository();
      }
  }
  ```

  - 메소드의 리턴 객체들이 스프링 빈으로 스프링 컨테이너에 등록된다.
  - MemberService는 memberRepository()에서 등록된 MemoryMemberRepsitory()를 사용한다. <br>
    즉, 스피링 빈에 등록된 컴포넌트를 사용하는 것 이므로 `@Autowired`와 같은 기능을 한다.

### DI 방식

- **DI**(의존관계 주입)에는 **필드 주입, setter주입, 생성자 주입** 이렇게 3가지 방법이 있다.

  ##### 필드 주입

  ```java
  @Controller
  public class MemberController {
      @Autowired private  MemberService memberService;
  }
  }
  ```

  - 변경에 유연하지 못하다는 단점이 있다.

  ##### setter 주입

  ```java
  private  MemberService memberService;

  @Autowired
  public void setMemberService(MemberService memberService){
      this.memberService = memberService;
  ```

  - setter가 public이어야 한다는 특징 때문에 외부에 노출된다는 단점이 있다.

  ##### 생성자 주입

  ```java
      private final MemberService memberService;

      @Autowired
      public MemberController(MemberService memberService) {
          this.memberService = memberService;
      }
  ```

  - 스프링 컨테이너에서 애플리케이션이 조립되는 시점에, 객체가 생성되며 한 번 주입한다.
  - 객체 생성 시점 이외에 변경할 수 없도록 막을 수 있다.

  <div style="height:12px"></div>

## 요약

#### 어노테이션 활용 경우

1. 스프링 컨테이너가 동작할 때 먼저 컴포넌트 스캔 진행
2. `@Component` 어노테이션이 달린 클래스를 찾아 빈으로 등록(Service, Controller, Repository 등)
3. `@Autowired`을 찾아 의존성 주입을 수행
4. `MemberController`에 `MemberService`가 `@Autowired`로 되어있으므로, 컨테이너에 빈으로 올라가 있는 `MemberService`주입
5. `MemerService`에 `Repository`부분이 `Autowired`이므로, 컨테이너 빈으로 올라간 `Repository` 주입

#### springConfig를 통해 자바코드로 할 경우

1. 컴포넌트 스캔이 진행 - `@Configuration` 어노테이션에 `@Component`가 포함되어 있음.
2. `springConfig`를 통해 `MemberService`,` MemberRepository`가 `Bean`으로 등록
3. `MemberService`에 아래 코드로 인해서 `MemberService`와 `MemberReposiroty`는 의존성이 존재하고, `MemberService`에 `MemberRepository`를 주입

   ```java
   return new MemberService(memberRepository());
   ```

4. `MemberController` 등록
5. `@Autowired`를 통해 `memberService`를 등록해야 함.
