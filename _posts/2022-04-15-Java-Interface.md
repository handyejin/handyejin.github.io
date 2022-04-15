---
layout: post
title: 자바 인터페이스(Interface)
date: 2022-04-15
category: JAVA
---

# 인터페이스(Interface)

> **인터페이스**란 자바에서 **다중 상속**을 구현하게 해주는 고급 기술이다. 인터페이스는 추상 클래스와 비슷하게 느껴질 수 있다. <br>**추상 클래스**는 추상 메소드 외에 멤버 변수나 일반 메소드를 가질 수 있지만, <br>**인터페이스**에서는 반드시 **사전에 정의**된 추상 메소드와 상수만을 가질 수 있다는 점에서 **차이**가 있다.
> 인터페이스는 동일한 목적 하에 동일한 기능을 보장하게 하기 위하여 사용한다.

<br>
### 인터페이스 선언
```java
public interface Example{
    int a = 10;
    int b();
}
```
- 인터페이스는 **interface** 키워드를 사용해서 정의할 수 있다.
- **인터페이스의 변수**는 변경이 불가능하다.
- **인터페이스의 메소드**는 설계만 진행해야한다. 코드 작성시에는 오류가 발생한다.
<br> 메소드를 구현하는 곳에서 강제로 **오버라이드**된다.

### 인터페이스 구현

```java
public Example2 implements Example{}

```

- 인터페이스를 **구현**할 때 **implements** 키워드를 사용한다. 인터페이스를 구현하는 클래스는 인터페이스의 모든 메소드를 구현해야한다.

### 예시1

###### Dog.java

<script src="https://gist.github.com/handyejin/6b69e84df333f3b74293745bdb72fe82.js"></script>

###### Cat.java

<script src="https://gist.github.com/handyejin/a3e47e34f0341eed7f486fdc34304d69.js"></script>

- 인터페이스의 메소드에 코드를 작성하면 안된다.

###### Main.java

<script src="https://gist.github.com/handyejin/f84f160de178c07a6a98c05eb11e4d53.js"></script>

- 인터페이스는 **다중 상속**이 가능하기 때문에 Dog와 Cat클래스를 상속받을 수 있다.
- Dog와 Cat 클래스에서 정의된 crying(),show(), one(), tow()메소드는 인터페이스를 구현하는 Main클래스에서 **반드시 구현**되어야 한다.
