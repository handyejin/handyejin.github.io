---
layout: post
title: 교육과정 설계
date: 2022-04-27 10:11:00
category: Algorithm
---

# [Stack, Queue] 교육과정 설계

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수는 1년 과정의 수업계획을 짜야 합니다.

수업중에는 필수과목이 있습니다. 이 필수과목은 반드시 이수해야 하며, 그 순서도 정해져 있습니다.

만약 총 과목이 A, B, C, D, E, F, G가 있고, 여기서 필수과목이 CBA로 주어지면 필수과목은 C, B, A과목이며 이 순서대로 꼭 수업계획을 짜야 합니다.

여기서 순서란 B과목은 C과목을 이수한 후에 들어야 하고, A과목은 C와 B를 이수한 후에 들어야 한다는 것입니다.

현수가 C, B, D, A, G, E로 수업계획을 짜면 제대로 된 설계이지만

C, G, E, A, D, B 순서로 짰다면 잘 못 설계된 수업계획이 됩니다.

수업계획은 그 순서대로 앞에 수업이 이수되면 다음 수업을 시작하다는 것으로 해석합니다.

수업계획서상의 각 과목은 무조건 이수된다고 가정합니다.

필수과목순서가 주어지면 현수가 짠 N개의 수업설계가 잘된 것이면 “YES", 잘못된 것이면 ”NO“를 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
CBA<br>
CBDAGE

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">YES</h5>

<br>

## 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
    public static String solution(String required, String classTable){
        String answer="YES";
        Queue<Character> q = new LinkedList<>();
        //큐에 필수과목을 넣어준다.
        for(char x: required.toCharArray()){
            q.offer(x);
        }

        for(int i=0;i<classTable.length();i++){
            if(q.isEmpty()){
                break;
            }
            //현재 수업이 큐의 맨 앞에 자료라면 꺼내준다.
            if(classTable.charAt(i)==q.peek()){
                q.poll();

            //현재 수업이 큐의 맨 앞에 자료가 아닌데 뒤에 자료들에 포함되어있으면 "NO"를 리턴
            }else{
                if(q.contains(classTable.charAt(i))){
                    return "NO";
                }
            }
        }
        //필수과목을 모두 이수했는지 확인
        //이거 안해서 오래걸림.
        if(!q.isEmpty()){
            answer="NO";
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String required = sc.next();
        String cl = sc.next();

        System.out.println(solution(required,cl));
    }
}
```
