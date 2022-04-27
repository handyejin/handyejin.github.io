---
layout: post
title: 공주 구하기
date: 2022-04-27 09:48:00
category: Algorithm
---

# [Stack, Queue] 공주 구하기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

정보 왕국의 이웃 나라 외동딸 공주가 숲속의 괴물에게 잡혀갔습니다.

정보 왕국에는 왕자가 N명이 있는데 서로 공주를 구하러 가겠다고 합니다.

정보왕국의 왕은 다음과 같은 방법으로 공주를 구하러 갈 왕자를 결정하기로 했습니다.

왕은 왕자들을 나이 순으로 1번부터 N번까지 차례로 번호를 매긴다.

그리고 1번 왕자부터 N번 왕자까지 순서대로 시계 방향으로 돌아가며 동그랗게 앉게 한다.

그리고 1번 왕자부터 시계방향으로 돌아가며 1부터 시작하여 번호를 외치게 한다.

한 왕자가 K(특정숫자)를 외치면 그 왕자는 공주를 구하러 가는데서 제외되고 원 밖으로 나오게 된다.

그리고 다음 왕자부터 다시 1부터 시작하여 번호를 외친다.

이렇게 해서 마지막까지 남은 왕자가 공주를 구하러 갈 수 있다.

<img src="https://cote.inflearn.com/public/upload/c0b0b7a761.jpg"/>

예를 들어 총 8명의 왕자가 있고, 3을 외친 왕자가 제외된다고 하자. 처음에는 3번 왕자가 3을 외쳐 제외된다.

이어 6, 1, 5, 2, 8, 4번 왕자가 차례대로 제외되고 마지막까지 남게 된 7번 왕자에게 공주를 구하러갑니다.

N과 K가 주어질 때 공주를 구하러 갈 왕자의 번호를 출력하는 프로그램을 작성하시오.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
8 3

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">7</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.lang.reflect.Array;
import java.util.*;

public class Main {
   public int solution(int n, int k, int[] arr){
       int answer=-1;
       int count=0;
       int idx=0;

       Set<Integer> set = new HashSet<>();
        //set의 크기가 n-1개가 되었을 때 반복을 멈춘다.
       while(set.size()!=n-1){

           if(!set.contains(arr[idx])){
               count++;
           }
           //count가 k가 되면 set에 추가해주고 다시 count를 0으로 정한다.
           if(count==k){
               set.add(arr[idx]);
               count=0;
           }
           if(idx==n-1){
               idx=0;
               continue;
           }
           idx++;


       }
       for(int i=0;i<n;i++){
           if(!set.contains(arr[i])){
               answer= arr[i];
           }

       }
       return answer;
   }

    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        int[] arr = new int[n];

        for(int i=0;i<n;i++){
            arr[i] = i+1;
        }
        System.out.println(T.solution(n,k));
    }

}
```

<div style="height:15px;"></div>

## 큐를 이용한 풀이

```java
import java.lang.reflect.Array;
import java.util.*;

public class Main {

    public int solution(int n, int k){
       int answer=0;
       //자료형이 Integer인 큐를 생성해준다.
        Queue<Integer> q = new LinkedList<>();

        //1~n까지 큐에 값을 넣어준다.
        for(int i=1;i<=n;i++){
            q.offer(i);
        }
        //큐가 비어있지 않은 동안에
        while (!q.isEmpty()){
            //1~k전까지 큐의 맨 앞 부분을 빼서 큐의 뒤에 넣어준다.
            for(int i=1;i<k;i++){
                q.offer(q.poll());
            }
            //k번째에 큐의 젤 앖에 있는 값을 빼준다.
            q.poll();
            //큐에 값이 1개일 때 정답니다.
            if(q.size()==1){
                answer=q.poll();
            }
        }
        return answer;
    }

    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        int[] arr = new int[n];

        for(int i=0;i<n;i++){
            arr[i] = i+1;
        }
        System.out.println(T.solution(n,k));
    }
}
```

- <span style="background-color:#f6f8fa">**큐(Queue)**</span>는 먼저 들어간 데이터가 먼저 나오는 **선입선출(FIFO)**구조로 저장하는 자료구조이다.
  - **큐에 값 추가**: queue.offer(x)
  - **큐의 값 제거**: queue.poll()
  - **큐의 젤 앞에 값 확인**: queue.peek()
  - **큐의 크기**: queue.size()
