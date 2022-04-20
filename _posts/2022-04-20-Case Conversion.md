---
layout: post
title: 대소문자 변환
date: 2022-04-20
category: Algorithm
---

# [String]대소문자 변환

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

대문자와 소문자가 같이 존재하는 문자열을 입력받아 대문자는 소문자로 소문자는 대문자로 변환하여 출력하는 프로그램을 작성하세요.
<br><br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	StuDY

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">sTUdy</h5>

## 풀이

```java
import java.util.Scanner;
public class Main {
    public String solution(String str){

       String result = "";
       for (int i = 0;i<str.length();i++){
           char ch = str.charAt(i);
           if(Character.isUpperCase(ch)==true){
               result +=Character.toLowerCase(ch);

           }else if(Character.isLowerCase(ch)==true){
               result +=Character.toUpperCase(ch);
           }
       }
       return result;

    }
    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.next();

        System.out.println(main.solution(str));


    }
}
```

<br>

### 아스키코드 이용

```java
import java.util.Scanner;

public class Main {
    public String solution(String str){
        String result3 ="";
        for(char x:str.toCharArray()){
            //x가 대문자인 경우
            if(x>=65&&x<=90){
                result3+=(char)(x+32);
            }
            //x가 소문자인 경우
            else if(x>=97 &&x<=122){
                result3+=(char)(x-32);
            }
        }

        return result3;
    }
    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.next();

        System.out.println(main.solution(str));
    }
}
```
