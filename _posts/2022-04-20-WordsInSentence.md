---
layout: post
title: 문장 속 단어
date: 2022-04-20
category: Algorithm
---

# [String]문장 속 단어

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

한 개의 문장이 주어지면 그 문장 속에서 가장 긴 단어를 출력하는 프로그램을 작성하세요.
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	it is time to study
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">study</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {

   public String solution(String str){
       String result="";
       int count = 0;
       int maxLength=0;

       String[] array = str.split(" ");

       for(String x:array){
           if(x.length()>maxLength){
               maxLength = x.length();
               result = x;
           }
       }
       return result;
   }


    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();

        System.out.println(main.solution(str));
    }
}
```

### indexOf(),substring() 이용한 방법

```java
import java.util.Scanner;

public class Main {

    public String solution(String str){
        String result="";
        int maxLength=Integer.MIN_VALUE, pos;
        while((pos= str.indexOf(' '))!=-1){
            String tmp = str.substring(0,pos);
            if(tmp.length()>maxLength){
                result = tmp;
                maxLength = tmp.length();

            }
            str = str.substring(pos+1);

        }
        if(str.length()>maxLength){
            result = str;
        }

        return result;
    }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();

        System.out.println(main.solution(str));
    }
}
```
