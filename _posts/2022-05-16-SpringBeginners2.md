---
layout: post
title: Spring 회원 관리 예제
date: 2022-05-16 03:20:00
category: Spring
---

> 비즈니스 요구사항 정리<br>
> 회원 도메인과 리포지토리 만들기<br>
> 회원 리포지토리 테스트 케이스 작성<br>
> 회원 서비스 개발<br>
> 회원 서비스 테스트<br>

# 비지니스 요구사항

- 데이터: 회원 아이디, 이름
- 기능: 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)
<div style="height:12px;"></div>

# 일반적인 웹 애프리케이션 계층 구조

![alt text](\public\img\WebApplicationTree.png)

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스: 핵심 비지니스 로직 구현
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 비지니스 도메인 객체
<div style="height:12px;"></div>

# 클래스 의존 관계

![alt text](\public\img\classTree.png)

- 회원 리포지토리(MemberRepository): 인터페이스 설계
  - 데이터 저장소가 선정되지 않았으므로.

# 회원 도메인과 리포지포리 만들기

#### 회원 객체

```java
package hello.hellospring.domain;

public class Member {

    //시스템이 저장하는 id값
    private Long id;
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

<div style="height:12px;"></div>
#### 회원 리포지토리 인터페이스

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    //회원 저장 기능
    Member save(Member member);

    //id로 회원을 찾음
    Optional<Member> findById(Long id);

    //회원 이름을 찾음
    Optional<Member> findByName(String name);

    //저장된 모든 회원 리스트를 반환
    List<Member> findAll();
}
```

- **Optional** : 자바 8에 들어간 기능으로 반환되는 객체가 null일 가능성이 있을 때 사용

#### 회원 리포지토리 메모리 구현체

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.*;

public class MemoryMemberRepository implements MemberRepository{

    //member와 id를 저장할 객체
    private static Map<Long,Member> store = new HashMap<>();

    //key 값 생성
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        //id는 시스템에서 정해줌.
        member.setId(++sequence);
        store.put(member.getId(),member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {

        //결과가 없을 수 있으므로 Optional.ofNullable로 감싸준다.
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {

       return store.values().stream()
               .filter(member -> member.getName().equals(name))
               .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore(){
        store.clear();
    }
}
```

- save(): 회원 저장
- findById(): store에서 아이디를 가지고 member을 꺼낸다.
- findByName(): store에서 이름을 가지고 찾는다.
- findAll(): 모든 회원을 반환한다

<div style="height:12px;"></div>
# 회원 리포지토리 테스트케이스 작성

개발한 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다. <br>
이런 방법은 실행하는데 오래 걸리고 한번에 실행하기 어렵다는 단점이 있다.
<br>
자바는 Junit이라는 프레임워크로 테스트를 실행해서 이런 문제를 해결한다.

- 테스트 클래스는 `src/text/java/` 하위 폴더에 생성한다.

#### 회원 리포지토리 메모리 구현체 테스트

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.Optional;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.*;

public class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    //메소드 실행 끝날 때마다 동작함
    @AfterEach
    public void afterEach(){

        //테스트가 끝날 때마다 저장소를 지운다.
        //테스트가 순서에 영향을 받지 않는다.
        repository.clearStore();
    }

    @Test
    public void save(){
        Member member = new Member();
        member.setName("손예진");

        repository.save(member);

        //id로 멤버를 찾음.
        Member result = repository.findById(member.getId()).get();

        //result와 member가 가튼지 확인인
       //System.out.println("result=" + (result==member));

        //기대했던 값이랑 실제값을 차례로 넣어서 비교한다.
        // Assertions.assertEquals(member,result);

        assertThat(member).isEqualTo(result);

    }

    @Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        //Optional<Member> result = repository.findByName("spring1");
        //get()붙여주면 optonal안붙여도 됨
        Member result = repository.findByName("spring1").get();
        assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);


        List<Member> result = repository.findAll();

        //result리스트의 크기가 2개가 나와야한다.
        assertThat(result.size()).isEqualTo(2);
    }
}
```

- 테스트는 각각 독립적으로 실행되어야 한다.
- `@AftefEach`: 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트릐 결과가 남을 수 있다. 이렇게 되면 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있다. 이 때, `@AfterEach`를 사용해 각 테스트가 종료될 때마다 메모리 DB에 저장된 데이터를 삭제한다.

### 테스트 주도 개발(TDD)

- 테스트 케이스를 먼저 만들고 기능을 만드는 것

<div style="height:12px;"></div>
# 회원 서비스 개발

도메인을 활용해 실제 비즈니스 로직을 작성한다.

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {
    private final MemberRepository memberRepository ;

    //외부에서 repository를 넣어주도록 변경함.
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }


    //회원 가입
    public Long join(Member member) {

        validateDuplicateMember(member);//중복 회원 검증

        memberRepository.save(member);
        return member.getId();
    }

    //메서드 추출: ctrl+alt+m
    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    //전체 회원 조회
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId){
        return memberRepository.findById(memberId);
    }
}
```

- 회원가입

  - 같은 이름이 있는 회원은 가입할 수 없다고 가정한다.

  ```java
    memberRepository.findByName(member.getName())
              .ifPresent(m -> {
                  throw new IllegalStateException("이미 존재하는 회원입니다.");
              });
  ```

  - `isPresent()`: Optional 의 메서드로, 어떤 값이 존재하면 소괄호 내의 로직을 수행한다.

<div style="height:12px;"></div>
# 회원 서비스 테스트 케이스 작성

#### Ctrl+Shitf+T

작성하려는 클래스 이름에 Ctrl+Shift+T를 누르면 테스트케이스를 만들 수 있다.

#### 테스트: given - when - then 기법

- given : 무언가 주어지고
- when: 어떤 로직을 실행했을 때
- then: 결과가 이러해야한다.

```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {

    MemberService memberService ;

    //MemberService에서 만든 repository와 다른 repository를 테스트하고 있다.
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach(){

        //같은 메모리의 repository를 사용할 수 있다.
        //DI
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach(){
        memberRepository.clearStore();
    }


    @Test
    void 회원가입() {
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외(){
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        //중복검증 메소드에 걸려서 예외가 터져야됌
        memberService.join(member1);

        //member2를 넣으면 IllegalStateException 예외가 터져야함
        IllegalStateException e = assertThrows(IllegalStateException.class, ()-> memberService.join(member2));
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

//        try{
//            memberService.join(member2);
//            fail();
//        }catch (IllegalStateException e){

           //then
//            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
//        }
    }

    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```

### DI: 테스트 케이스를 위해 인스턴스를 새로 만들면 기존 클래스와 다른 인스턴스 이므로 문제가 발생할 수 있다.

-> 클래스의 인스턴스 변수 값을 new로 생성하지 않고, 생성자를 통해서 값을 외부에서 넣도록 한다. 이를**DI(의존성 주입)**이라고 한다.
