---
layout: post
title: 올바른 괄호
date: 2022-04-27 06:42:00
category: Algorithm
---

# [Stack, Queue] 올바른 괄호

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

괄호가 입력되면 올바른 괄호이면 “YES", 올바르지 않으면 ”NO"를 출력합니다.

(())() 이것은 괄호의 쌍이 올바르게 위치하는 거지만, (()()))은 올바른 괄호가 아니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
(()(()))(()

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">NO</h5>

<br>

## 풀이

```java
import java.util.Scanner;
import java.util.Stack;

public class Main {
   public static String solution(String p){
       String answer;
       //괄호의 갯수 저장하는 변수
       int cnt=0;
       for(char x: p.toCharArray()){
           //여는 괄호일 때 cnt를 증가시킨다.
           if(x=='('){
               cnt++;
            //닫는 괄호일 때 cnt를 감소한다.
           }else if(x==')'){
               cnt--;
           }
       }
       //cnt가 0이고 처음 시작이 여는 괄호, 끝이 닫는 괄호일 땐 올바른 괄호이다.
       if(cnt==0 && p.charAt(0)=='('&&p.charAt(p.length()-1)==')'){
           answer = "YES";
       }else{
           answer="NO";
       }
       return answer;
   }


    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String p = sc.next();

        System.out.println(solution(p));
    }
}
```

## 스택을 이용한 풀이

```java
import java.util.Scanner;
import java.util.Stack;

public class Main {

public static String solution(String p){
    String answer="YES";

    //Character형 Stack객체를 생성해준다.
    Stack<Character> stack = new Stack<>();

    for(char x: p.toCharArray()){
        //여는 괄호일 때 스택에 추가해주고, 닫는 괄호일 때 스택에서 제거해준다.
        if(x=='('){
            stack.push(x);
        //닫는 괄호일 때
        //스택이 비어있다면 짝이 맞지 않으므로 올바르지 않은 괄호이다.
        }else{
            if(stack.isEmpty()){
                answer="NO";
                return answer;
            }else{
                stack.pop();
            }
        }
    }

    //여는 괄호가 더 많다면 올바르지 않은 괄호이다.
    if(stack.isEmpty()==false){
        answer="NO";
    }

    return answer;
}
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String p = sc.next();

        System.out.println(solution(p));
    }
}
```

- 괄호 문제는 **Stack**을 활용하는 대표적인 알고리즘이다.
- <span style="background-color:#f6f8fa">**Stack**</span>은 늦게 들어간 자료가 먼저 나오는 **후입선출** 자료구조이다.
  - 삽입: stack.push(x)
  - 삭제: stack.pop()
  - 스택이 비어있는지 확인: stack.isempty()
  - 스택 출력: stack.peed()
