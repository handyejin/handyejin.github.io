---
layout: post
title: Spring AOP
date: 2022-05-19 04:52:00
category: Spring
---

# AOP

### AOP가 필요한 상황

- 모든 메소드의 호출 시간을 측정하고 싶다면<br>
  시간 측정 로직인 **공통 관심 사항**과 핵심 비즈니스 로직인 **핵심 관심사**항으로 나뉜다.
  이 때 각 메소드 별로 시간 측정 로직을 추가할 때 몇몇 문제가 발생한다.
  - 시간 측정 로직은 핵심 관심 사항이 아니다. 공통 관심 사항이다.
  - 시간 측정 로직과 핵심 비지니스 로직이 섞여서 유지보수가 어렵다.
  - 시간 측정 로직을 공통 로직으로 만들기 어렵다.
  - 시간 측정 로직을 변경할 때 모든 로직을 찾아가면서 변경해야 한다.
  <div style = "height:20px;"></div>

### AOP 적용

- **AOP**는 Aspect Oriented Programming으로 공통 관심 사항과 핵심 관심 사항의 분리를 편하게 해준다.

<!-- <div style="height:10px;"> -->

![alt text](\public\img\AOP1.png){: width="550" height ="450" }

<!-- </div> -->

<div style = "height:10px;"></div>

##### 시간 측정 AOP 등록

```java
package hello.hellospring.AOP;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

import javax.security.sasl.SaslServer;
import javax.swing.text.TableView;

//AOP를 쓸 수 있음
@Component
@Aspect
public class TimeTraceAop {

    //패키지 경로 하위 파일들에 실행한다.
    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString()+ " " + timeMs +
                    "ms");
        }
    }
}
```

- **시간 측정 AOP**를 등록해서 **문제점을 해결**한다.

  - 회원가입, 회원 조회 등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리한다.
  - 핵심 관심 사항을 깔끔하게 유지할 수 있다.
  - 변경이 필요하면 이 로직만 변경하면 된다.
  - **`@Around`**를 이용해 원하는 적용대상을 선택하 수 있다.

  <div style = "height:20px;"></div>

### 스프링의 AOP 동작 방식

##### AOP 적용 전 의존 관계

![alt text](\public\img\AOP2.png){: width="550" height ="450" }

  <div style = "height:10px;"></div>

##### AOP 적용 후 의존 관계

![alt text](\public\img\AOP3.png){: width="550" height ="450" }

- AOP를 적용할 대상을 지정하면 **프록시**라는 가짜 스프링 빈을 만든다. 가짜 스프링 빈이 끝나면 진짜 스프링 빈을 호출한다.
