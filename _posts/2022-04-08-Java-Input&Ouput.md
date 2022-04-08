---
layout: post
title: 자바 Scanner로 입력 받기
date: 2022-04-08
category: JAVA
---

# 기본 입출력

> 자바는 Scanner()함수를 이용해서 사용자와 상호작용 할 수 있다. 입력 받은 자료는 내부적으로 어떠한 처리를 한 뒤에 다시 사용자에게 그 값을 반환할 수 있다.
> <br>

- 사용자로부터 값을 입력 받고 싶을 때 Scanner라는 클래스를 사용한다.<br>
  `Scanner sc = new Scanner(System.in);`
- Scanner 입력 후 **alt+enter**을 하면 자동으로 *java.util.Scanner*가 import 된다.<br>

#### 예제1. 간단한 Scanner 예시

<script src="https://gist.github.com/handyejin/58d4dd6ac3cf87daa337c67f255aa246.js"></script>

- nextInt()는 정수를 입력받는다.
  <br>
- next()메소드는 공백 이전까지의 문자열을 입력받고
  <br>
  nextLine() 메소드는 문자열 전체를 입력받아 준다. <br>따라서 공백이 포함되는 경우에는 nextLine()을 쓰는것이 좋다.

#### 예제2. 파일 입출력 예시

<script src="https://gist.github.com/handyejin/445dff19446b316366d489d1f752abcb.js"></script>

- 파일이 없을 때 를 대비해서 파일 예외저리를 필수적으로 해주어야 한다.
