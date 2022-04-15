---
layout: post
title: 자바 추상화(Abstract)
date: 2022-04-10
category: JAVA
---

# 추상(Abstract)

> **추상 클래스**는 구체적이지 않은 클래스를 의미한다.<br>
> 추상 클래스를 사용하기 위해서 꼭 **상속**을 받아야 하며, 상속받은 추상메소드는 반드시 구현을 해주어야하기 때문에 설계적인 측면에서 의미가 있다.

<br>
### 추상 메소드

```
abstract void play(String songName);
```

- 위와 같이 추상메소드는 선언부만 있고, 구현부는 없다. <br>
  자식 클래스가 **오버라이딩**하여 사용해야 한다.

### 추상 클래스

```
//추상 클래스 정의
abstract class Player {
    //추상 메소드
    abstract void play(String songName);
}

```

- 클래스 앞에 **abstract** 키워드를 붙이면 추상 클래스가 된다.
- 이 때, Player을 상속 받는 클래스는 만드니 play()라는 추상 메소드를 상속받아 오버라이딩 해야한다.

### 예시1. Animal 추상클래스를 상속받는 Dog 클래스와 Cat 클래스

###### Animal.java

<script src="https://gist.github.com/handyejin/1211071b1bb9b4f6abe590dc5d6bee02.js"></script>

- **abstract** 키워드로 추상클래스와 추상메소드 정의

###### Dog.java

<script src="https://gist.github.com/handyejin/a85f4d5a4935f306f93e6a9193b0b890.js"></script>

- Dog 클래스는 추상클래스인 Animal 클래스를 **상속** 받는다.<br>
  이 때, 추상 메소드인 crying()을 반드시 **재정의**해주어야 한다.

###### Main.java

<script src="https://gist.github.com/handyejin/721966474c6ae715bb79d7bf032e089c.js"></script>

### 예시2. 음악 플레이어 클래스

###### Player.java

<script src="https://gist.github.com/handyejin/8c56dbbe5479cc56749087da19907ed5.js"></script>

- Play 추상 클래스에 재생, 일시정지, 정지 추상메서드를 작성한다.

###### Main.java

<script src="https://gist.github.com/handyejin/0f02721e10253f7397d21f72efea5ca9.js"></script>

- Main 클래스는 추상 클래스인 Player 클래스를 **상속** 받는다.<br> 이 때,
  Main 클래스는 play(), pause(), stop() 인 **추상 메서드**를 **재정의** 해주어야 한다.
