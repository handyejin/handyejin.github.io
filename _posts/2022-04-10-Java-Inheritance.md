---
layout: post
title: 자바 상속
date: 2022-04-10
category: JAVA
order: 2
---

# 상속(Inheritance)

> **상속**은 다른 클래스가 가지고 있는 정보를 자신이 포함하겠다는 의미이다.
> 상속을 해주는 클래스를 **부모클래스** 상속 받는 클래스를 **자식 클래스** 라고 한다.
> 상속을 하면 불필요한 코드의 수를 줄일 수 있다는 장점이 있다.

<br>
### extends
```
public class Student extends People{
    ...
}
```
```
public class Student extends People1, People2{
    //불가능!!!
}
```
- 상속을 받는 방법은 자식 클래스 뒤에 **extends** 키워드를 사용하고 부모틀래스를 써주면 된다.
- 자바에선 자식클래스가 여러 부모클래스로부터의  **다중 상속**이 **불가능**하다.

### 예제1. 사람 클래스를 상속받는 학생 클래스

###### People.java - 부모 클래스

<script src="https://gist.github.com/handyejin/a3ccad74b17d2fe2f703e62e69ca362e.js"></script>

- 이름, 나이, 키, 몸무게 필드를 가지고 있는 부모클래스 생성

###### Student.java - 자식 클래스

<script src="https://gist.github.com/handyejin/45ea1f8431d1bbc5f8981b159ce3682a.js"></script>

- 학번, 학년, 성적 필드 선언
- 이 때, 자식클래스는 부모클래스로부터 필드와 메서드를 물려받는다. 따라서 자식 클래스에서는 이름, 나이, 키, 무게 필드를 가져와서 사용할 수 있다.

###### Main.java

<script src="https://gist.github.com/handyejin/ddd0232861034ef60a5d6a5d18a95b98.js"></script>

- student 객체를 생성해주고 멤버 필드에 값을 넣어준다.
