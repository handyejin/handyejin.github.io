---
layout: post
title: 자바 다형성
date: 2022-04-18
category: JAVA
---

# 다형성(Polmorphism)

> **다형성**은 하나의 객체가 여러 가지 타입을 가질 수 있는 것을 의미한다.
> 다형성은 **부모 클래스 타입의 참조 변수로 하위 클래스의 객체를 참조**할 수 있게 해준다.
> <br>다형성의 개념을 이용할 때 프로그램의 소스 코드를 유연하게 구성할 수 있다.

<br>
### 참조 변수의 다형성

```java
class Parent{..}
class Child extends Parent{..}

Parant p1 = new Parnet(); //허용
Child c1 = new Child();   //허용
Parent p2 = new Child();  //허용
Child c2 = new Parent();  //오류 발생
}
```

- 부모 클래스 타입의 참조변수로 자식 클래스타입의 인스턴스를 참조할 수 있다.
  <br>하지만 자식클래스타입의 참조변수로는 부모 클래스 타입의 인스턴스를 참조할 수 없다.

### 참조 변수의 타입 변환

- 서로 상속 관계에 있는 클래스 사이에만 타입 변환을 할 수 있다.
- 자식 클래스 타입에서 부모 클래스 타입으로의 타입 변환은 **생략**할 수 있지만, 부모 클래스 타입에서 자식 클래스 타입으로의 변환은 반드시 **명시**해야한다.

  - <span style = "background-color:#f6f8fa">**(변환할 타입 클래스의 이름) 변환할 참조변수**</span>

### 예제1. 과일 정보 프로젝트

###### Fruit.java(부모 클레스)

<script src="https://gist.github.com/handyejin/2b08165a09c211913ab3d94d9b7f491f.js"></script>

- Fruit 라는 부모클래스에 과일 이름, 가격, 신선도를 출력해주는 show()메서드가 있다.

###### Banana.java(자식 클레스)

<script src="https://gist.github.com/handyejin/45c10b70953284c581f6f73d0398934c.js"></script>

###### Peach.java(자식 클레스)

<script src="https://gist.github.com/handyejin/f68cf13fd07492da6b210efd5e886130.js"></script>

- Fruit 클래스를 상속받아 생성자를 정의해준다.

###### Msin.java

<script src="https://gist.github.com/handyejin/1dce217cd261c64c6124d92afcf2eb24.js"></script>

- 사용자가 어떤 값을 입력했는지에 따라서 다른 인스턴스를 참조할 수 있다.
- 코드는 한줄인데 어떤 인스턴스형을 참조했는지에 따라서 show()의 실행이 달라진다. 이것이 **다형성**이다.
