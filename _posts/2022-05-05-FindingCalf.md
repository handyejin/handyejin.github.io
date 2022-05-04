---
layout: post
title: 송아지 찾기1(BFS:상태트리탐색)
date: 2022-05-04 12:00:00
category: Algorithm
---

# [BFS 기초] 송아지 찾기1(BFS)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수는 송아지를 잃어버렸다. 다행히 송아지에는 위치추적기가 달려 있다.

현수의 위치와 송아지의 위치가 수직선상의 좌표 점으로 주어지면 현수는 현재 위치에서 송아지의 위치까지 다음과 같은 방법으로 이동한다.

송아지는 움직이지 않고 제자리에 있다.

현수는 스카이 콩콩을 타고 가는데 한 번의 점프로 앞으로 1, 뒤로 1, 앞으로 5를 이동할 수 있다.

최소 몇 번의 점프로 현수가 송아지의 위치까지 갈 수 있는지 구하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5 14

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
    int [] visited;
    public int solution(int s, int e){
        Queue<Integer> q = new LinkedList();
        visited = new int[10001];
        //앞1, 뒤1, 앞5 이동하기 위해 배열에 저장
        int[] move = {1,-1,5};
        q.add(s);
        visited[s] = 1;

        int level = 0;
        while(!q.isEmpty()){
            int len = q.size();
            //큐의 크기만큼 반복하면서
            //큐에서 현재 위치를 꺼내어 앞1, 뒤1, 앞5로 이동한 거리를 큐에추가한다.
            //큐에 위치를 추가하면 visited를 1로 바꿔준다.
            for(int i = 0;i<len;i++){
                int tmp = q.poll();
                if (tmp==e) return level;
                for(int j=0;j<3;j++){
                    int x = tmp+move[j];
                    if(x>=0&& x<=10000&& visited[x]==0){
                        q.add(x);
                        visited[x] = 1;
                    }
                }
            }
            level++;
        }
        return level;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        // 현수의 위치
        int s = sc.nextInt();
        // 송아지의 위치
        int e = sc.nextInt();
        System.out.println(T.solution(s,e));
    }
}
```
