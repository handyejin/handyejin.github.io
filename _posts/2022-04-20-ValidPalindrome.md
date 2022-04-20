---
layout: post
title: 유효한 팰린드롬
date: 2022-04-20
category: Algorithm
---

# [String]유효한 팰린드롬

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

앞에서 읽을 때나 뒤에서 읽을 때나 같은 문자열을 팰린드롬이라고 합니다.

문자열이 입력되면 해당 문자열이 팰린드롬이면 "YES", 아니면 “NO"를 출력하는 프로그램을 작성하세요.

단 회문을 검사할 때 알파벳만 가지고 회문을 검사하며, 대소문자를 구분하지 않습니다.

알파벳 이외의 문자들의 무시합니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	found7, time: study; Yduts; emit, 7Dnuof
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">YES
</h5>

## 풀이

```java
import java.util.Locale;
import java.util.Scanner;

public class Main {

    public String solution(String str){
        String answer="NO";

        str = str.toUpperCase();
        String tmp="";
        for(int i = 0;i<str.length();i++){
            //알파벳인 경우에만 찾아서 tmp문자열에 넣어준다.
            if(Character.isAlphabetic(str.charAt(i))){
                tmp+=str.charAt(i);
            }
        }
        //StringBuilder객체르 이용해서 문자열을 뒤집어준다.
        String tmp2 = new StringBuilder(tmp).reverse().toString();

        //원래의 문자열과 뒤집어진 문자열을 비교한다.
        if(tmp.equals(tmp2)){
            answer ="YES";
        }

        return answer;

    }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(main.solution(str));
    }
}
```

### replaceAll 정규식 이용 방법

```java
import java.util.Locale;
import java.util.Scanner;

public class Main {

    public String solution(String str){
        String answer="NO";

        //대문자 A-Z까지가 아니면 ""로 대체시킴
       str = str.toUpperCase().replaceAll("[^A-Z]","");
       String tmp2 = new StringBuilder(str).reverse().toString();
       if(str.equals(tmp2)){
           answer ="YES";
       }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(main.solution(str));
    }
}
```

- **replaceAll()** 함수는 대상 문자열을 원하는 문자 값으로 변환하는 함수이다.
- **첫번째 매개변수**는 **변환하고자 하는 대상**이 될 문자열
- **두번째 매개변수**는 **변환할 문자값**
- **replaceAll()** 은 **replace()** 와는 다르게 **정규 표현식**을 이용할 수 있다.
