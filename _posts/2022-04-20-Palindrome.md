---
layout: post
title: 회문 문자열
date: 2022-04-20
category: Algorithm
---

# [String]회문 문자열

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

앞에서 읽을 때나 뒤에서 읽을 때나 같은 문자열을 회문 문자열이라고 합니다.

문자열이 입력되면 해당 문자열이 회문 문자열이면 "YES", 회문 문자열이 아니면 “NO"를 출력하는 프로그램을 작성하세요.

단 회문을 검사할 때 대소문자를 구분하지 않습니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	gooG
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
       String answer= "";
       str = str.toLowerCase();
       int len = str.length();
       int check = 0;

       //문자열의 중간까지만 반복하면서 앞문자와 대칭되는 뒷문자가 일치하지 않는 경우를 확인함
       for(int i = 0;i<len/2;i++){
           if(str.charAt(i)!=str.charAt(len-1-i)){
               check=1;
               break;
           }
       }
       if(check==0){
           answer="YES";
       }else{
           answer="NO";
       }
       return answer;
   }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        System.out.println(main.solution(str));

    }
}
```

### StringBuilder을 이용한 방법

```java
import java.util.Locale;
import java.util.Scanner;

public class Main {

    public String solution(String str){
        String answer= "NO";

        //StringBuilder객체를 이용해서 문자열을 뒤집는다.
        String tmp = new StringBuilder(str).reverse().toString();
        //원래 문자열과 뒤집은 문자열을 대소문자 무시하고 비교한다.
        if(str.equalsIgnoreCase(tmp)){
            answer = "YES";
        }

        return answer;
    }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        System.out.println(main.solution(str));

    }
}
```
