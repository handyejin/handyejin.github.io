---
layout: post
title: Spring 회원 관리 예제 - 웹 MVC 개발
date: 2022-05-17 07:15:00
category: Spring
---

# 회원 웹 기능 - 홈 화면 추가

회원 등록 및 회원 조회 기능을 만들어본다.

##### 홈 컨트롤러 추가

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    //localhost:8080로 들어오면 home()이 호출됨됨
    //컨테이너 -> static - index.html은 무시된다.
   @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

- `@GetMapping("/")`: URL에서 `/`는 도메인이 처음 들어왔을 때의 페이지를 말한다.
- 웹 브라우저에서 요청이 들어오면, 톰캣 서버는 스프링 컨테이너에서 관련 컨트롤러를 찾고, 없으면 static폴더를 찾는다. <br>
  **컨트롤러가 정적 파일보다 우선순위가 높기 때문**에 static에 있는 `index.html`보다 `@GetMapping("/")`이 먼저 실행된다.

<div style="height:6px;"></div>

##### 회원 관리용 홈

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
          <a href="/members/new">회원 가입</a>
          <a href="/members">회원 목록</a>
        </p>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

<div style="height:26px;"></div>

# 회원 웹 기능 - 등록

##### 회원 등록 폼 컨트롤러

```java
 //GetMapping: 데이터를 조회할 때 씀
    @GetMapping("/members/new")
    public String createForm(){
       return "members/createMemberForm";
    }
```

- `members/createMemberForm`이런 url은 `src/java/resources/templates`에서부터의 경로이다. <br>
  즉, `templates` 디렉토리에 `members`라는 하위 디렉토레에 `createMemberForm.html`로 연결된다.

<div style="height:6px;"></div>

##### 회원 등록 폼 HTML `(resources/templates/members/createMemberForm)`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <form action="/members/new" method="post">
        <div class="form-group">
          <label for="name">이름</label>
          <!--name 은 서버로 넘어갈 때 키가 됨-->
          <input
            type="text"
            id="name"
            name="name"
            placeholder="이름을
입력하세요"
          />
        </div>
        <button type="submit">등록</button>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```

- `<form action="/members/new" method="post">`에 의해<br>
`/members/new` URL로 **POST방식**으로 데이터가 넘어간다.
<div style="height:5px;"></div>

##### 웹 등록 화면에서 데이터를 전달 받을 폼 객체 `(src/main/java/프로젝트명/controller/MemberForm.java)`

```java
package hello.hellospring.controller;

public class MemberForm {
    private String name;

    //값을 꺼냄
    public String getName() {
        return name;
    }

    //값을 넣어줌
    public void setName(String name) {
        this.name = name;
    }
}

```

<div style="height:6px;"></div>

##### 회원 컨트롤러에서 회원을 실제 등록하는 기능

```java
 @PostMapping("/members/new")
    public String create(MemberForm form){

       Member member = new Member();
       member.setName(form.getName());
       System.out.println("member = " + member.getName());

       memberService.join(member);

       //home 화면으로 보낸다.
       return "redirect:/";
    }
```

- `return "redirect:/"`: redirect 오른쪽의 주소로 URL을 요청함
- **`PostMapping()`** (POST방식): 데이터를 전달(등록)할 때 사용
- **`GetMapping()`** (GET방식): 데이터를 조회할 때 사용. URL에 직접 입력을 통해 동작함

<div style="height:26px;"></div>

# 회원 웹 기능 - 조회

##### 회원 컨트롤러에서 조회 기능

```java
    @GetMapping("/members")
    public String list(Model model){
        List<Member> members = memberService.findMembers();
        model.addAttribute("members",members);

        return "members/memberList";
    }
```

<div style="height:6px;"></div>

##### 회원 리스트 HTML

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <div>
        <table>
          <thead>
            <tr>
              <th>#</th>
              <th>이름</th>
            </tr>
          </thead>
          <tbody>
            <!--1.'$'를 이용해서 모델에 담긴 members를 꺼내옴
                2. loop를 돌면서, id와 name을 꺼내옴
            -->
            <tr th:each="member : ${members}">
              <td th:text="${member.id}"></td>
              <td th:text="${member.name}"></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

- th: **Thymeleaf** 템플릿엔진을 사용하는 부분
- `<tr th:each="member : ${members}">`에서<br>
  - **`th`**: **each**는 Thymeleaf의 **반복문** 문법이다.<br>
  - **`member`**: 반복되는 객체 하나에 대한 임시변수명이다.<br>
  - **`${member}`**: Model에서 members라는 키를 가지는 값을 찾아 가지고 온다.<br>
