---
layout: post
title: 자바 Final 키워드
date: 2022-04-15
category: JAVA
---

# Final 키워드

> **final**키워드는 절대로 변하지 않는 특정한 것을 정하고 싶을 때 사용한다. final 키워드는 **변수**(variable), **메서드**(method), **클래스**(class)에 사용될 수 있다.

### final 변수

```java
final int number = 10;
number = 5; //(x)
```

- 변수를 선언할 때 final 키워드를 이용하면, **변경할 수 없는 상수값**이 된다.

### final 메서드

```java
public final void show()
```

- 메소드에 final을 선언하면, **재정의가 불가능한 메소드**가 된다.

```java
final public class Parent{}
```

- 클래스에 final을 선언하면 **상속이 불가능한 완전한 클래스**가 된다.

### 예시1. final 메소드

###### Parent.java

<script src="https://gist.github.com/handyejin/de42ae48fcd1f5cdee3c8209b9e9be52.js"></script>

###### Main.java

<script src="https://gist.github.com/handyejin/77884aa08b1963d0233d226aa43a1b26.js"></script>

- Parent 클래스에서 show() 메서드에 **final**을 선언했기 때문에 Main.java에서는 **재정의가 불가능** 하다.

### 예시2. final 클래스

###### Parent.java

- <script src="https://gist.github.com/handyejin/87cecc67bf9c0738ecf9acc2da481de4.js"></script>

- 클래스에 final을 선언하면 아래처럼 **상속이 불가능**하다.
  ```java
  public class Main extends Parent{} //(x)
  ```
