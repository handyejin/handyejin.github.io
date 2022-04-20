---
layout: post
title: 중복 문자 제거
date: 2022-04-20
category: Algorithm
---

# [String]중복 문자 제거

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

소문자로 된 한개의 문자열이 입력되면 중복된 문자를 제거하고 출력하는 프로그램을 작성하세요.

중복이 제거된 문자열의 각 문자는 원래 문자열의 순서를 유지합니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	ksekkset
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">kset</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
   public String solution(String str){
       String answer="";

       //문자열에서 발견된 문자를 담을 ArrayList객체 생성
       ArrayList<Character> array = new ArrayList<Character>();
       char[] s = str.toCharArray();
       int check = 0;

       for(char x: s){
           for(int i = 0;i< array.size();i++){
               //array에서 문자 x가 포함되어있으면 check를 1로 변경하여 중복을 체크해준다.
               if(array.contains(x)){
                   check=1;
                   break;
               }
           }
           //중복되지 않았으면, array에 문자 x를 추가해주고 answer에 x를 추가한다.
           if(check==0){
               array.add(x);
               answer+=x;
            //중복이라면 answer에 x를 추가하지 않고, 다시 check를 0으로 변경한다.
           }else{
               check = 0;
           }
       }
       return answer;
   }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc  = new Scanner(System.in);
        String str = sc.next();
        System.out.println(main.solution(str));

    }
}
```

<br>
## indexOf()를 이용한 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
public String solution(String str){
    String answer="";
    for(int i = 0;i<str.length();i++){
        //문자가 처음 발견되는 위치와 현재 인덱스가 같을 때만 answer에 추가한다.
        //중복된 문자일 경우에는 현재 인덱스와 처음 발견되는 위치가 다르다.
        if(str.indexOf(str.charAt(i))==i){
            answer+=str.charAt(i);
        }
    }
    return answer;
}
    public static void main(String[] args){
        Main main = new Main();
        Scanner sc  = new Scanner(System.in);
        String str = sc.next();
        System.out.println(main.solution(str));

    }
}
```

- **indexOf()** 는 특정 문자나 문자열이 앞에서부터 **처음 발견되는 인덱스**를 반환하고, 만약 찾지 못했을 경우 "-1"을 반환한다.
