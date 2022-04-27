---
layout: post
title: 응급실
date: 2022-04-27 10:48:00
category: Algorithm
---

# [Stack, Queue] 공주 구하기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

메디컬 병원 응급실에는 의사가 한 명밖에 없습니다.

응급실은 환자가 도착한 순서대로 진료를 합니다. 하지만 위험도가 높은 환자는 빨리 응급조치를 의사가 해야 합니다.

이런 문제를 보완하기 위해 응급실은 다음과 같은 방법으로 환자의 진료순서를 정합니다.

• 환자가 접수한 순서대로의 목록에서 제일 앞에 있는 환자목록을 꺼냅니다.

• 나머지 대기 목록에서 꺼낸 환자 보다 위험도가 높은 환자가 존재하면 대기목록 제일 뒤로 다시 넣습니다. 그렇지 않으면 진료를 받습니다.

즉 대기목록에 자기 보다 위험도가 높은 환자가 없을 때 자신이 진료를 받는 구조입니다.

현재 N명의 환자가 대기목록에 있습니다.

N명의 대기목록 순서의 환자 위험도가 주어지면, 대기목록상의 M번째 환자는 몇 번째로 진료를 받는지 출력하는 프로그램을 작성하세요.

대기목록상의 M번째는 대기목록의 제일 처음 환자를 0번째로 간주하여 표현한 것입니다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5 2<br>
60 50 70 80 90

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

<div style="height:16px;"></div>

#### 예시 입력2

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
6 3
70 60 90 60 60 60

</h5>

#### 예시 출력2

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">4</h5>

## 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
   public int solution(int n, int m,int[] risk){
       int answer = 0;
       Queue<Integer> queue = new LinkedList<>();

       //인덱스 번호를 큐에 저장해준다.
       for(int i=0;i<n;i++){
           queue.offer(i);
       }

       while(true){
           //idx는 큐의 가장 앞에 자료를 꺼낸 인덱스이다.
           int idx = queue.poll();
           boolean maxCheck=true;
           for(int i=0;i<n;i++){
                //idx위치의 자료보가 큰 자료가 있으면 큐의 맨 뒤에 인덱스를 저장해준다.
               if(risk[idx]<risk[i] &&queue.contains(i)){
                   maxCheck = false;
                   queue.offer(idx);
                   break;
               }
           }
           //가장 큰 경우엔 answer을 증가시키고 만약 idx가 입력받은 m과 같다면 return 해준다.
           if(maxCheck==true){
               answer++;
               if(idx==m){
                   return answer;
               }

           }

       }

   }

    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int[] risk = new int[n];
        for(int i=0;i<n;i++){
            risk[i]=sc.nextInt();
        }
        System.out.println(T.solution(n,m,risk));
    }
}
```

- 인덱스를 큐에 넣은 다음 큐에서 인덱스를 하나씩 꺼내서 배열과 비교해보는 방식으로 구현하였는데 복잡하였다.

<div style="height:15px;"></div>

## 객체를 저장하는 큐를 이용한 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

//번호와 우선순위를 담을 수 있는 클래스
class Person{
    int id;
    int priority;

    public Person(int id, int priority){
        this.id = id;
        this.priority=priority;
    }
}
public class Main {

    public int solution(int n, int m, int[] arr){
        int answer=0;
        //Person 객체를 저장할 수 있는 큐 만듬.
        Queue<Person> Q = new LinkedList<>();
        for(int i=0;i<n;i++){
            Q.offer(new Person(i,arr[i]));
        }
        while (!Q.isEmpty()){
            Person tmp = Q.poll();
            for(Person x: Q){
                //우선순위가 더 큰것이 발견됬을 때, 큐의 뒤쪽에 추가한다.
                if(x.priority>tmp.priority){
                    Q.offer(tmp);
                    tmp = null;
                    break;
                }
            }
            //우선순위가 가장 클 때 answer을 증가시킨다
            //이 때, m과 tmp의 id가 같으면 answer을 리턴한다.
            if(tmp!=null){
                answer++;
                if(m==tmp.id){
                    return answer;

                }
            }

        }
        return answer;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int[] risk = new int[n];
        for(int i=0;i<n;i++){
            risk[i]=sc.nextInt();
        }
        System.out.println(T.solution(n,m,risk));
    }
}

```

- 객체형태로 큐에 저장할 생각은 하지 못했었다. 큐를 객체형태로 만드니 코드가 훨씬 보기 좋아졌고 구현하기도 쉬웠다.
