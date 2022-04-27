---
layout: post
title: 괄호문자제거
date: 2022-04-27 09:30:00
category: Algorithm
---

# [Stack, Queue] 괄호문자제거

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

입력된 문자열에서 소괄호 ( ) 사이에 존재하는 모든 문자를 제거하고 남은 문자만 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
(A(BC)D)EF(G(H)(IJ)K)LM(N)

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">EFLM</h5>

<br>

## 풀이

```java
import java.util.Scanner;
import java.util.Stack;

public class Main {
    public String solution(String p){
        String answer = "";
        Stack<Character> stack = new Stack<>();

        for(char x: p.toCharArray()){
            //닫는 괄호가 나오면 여는 괄호가 나올때까지 stack에서 제거해준다.
            if(x==')'){
               while(stack.peek()!='('){
                   stack.pop();
               }
                //괄호까지 제거
                stack.pop();
                continue;
            }
            //여는 괄호일 때는 push 해준다.
            stack.push(x);
        }

        for(int i = 0;i< stack.size();i++){
            answer+=stack.get(i);
        }

        return answer;
    }

    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        String p = sc.next();
        System.out.println(T.solution(p));

    }
}
```
