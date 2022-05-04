---
layout: post
title: 최대점수 구하기(DFS)
date: 2022-05-04 12:32:00
category: Algorithm
---

# [DFS,BFS 활용] 최대점수 구하기(DFS)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

이번 정보올림피아드대회에서 좋은 성적을 내기 위하여 현수는 선생님이 주신 N개의 문제를 풀려고 합니다.

각 문제는 그것을 풀었을 때 얻는 점수와 푸는데 걸리는 시간이 주어지게 됩니다.

제한시간 M안에 N개의 문제 중 최대점수를 얻을 수 있도록 해야 합니다.

(해당문제는 해당시간이 걸리면 푸는 걸로 간주한다, 한 유형당 한개만 풀 수 있습니다.)

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5 20<br>
10 5<br>
25 12<br>
15 8<br>
6 3<br>
7 4<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">41</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Scanner;

public class Main {
    static int n,m;
    static int[][] arr;
    static int[] visited;
    static int maxScore= Integer.MIN_VALUE;

    public void DFS(int idx){
        if(idx==n){
            int time = 0;
            int score = 0;
            for(int i=0;i<n;i++){
                if(visited[i]==1){
                    time += arr[i][1];
                    score += arr[i][0];
                }
            }
            //최대 점수를 구한다.
            if(time<=m && score>maxScore){
                maxScore = score;
            }
        }
        else{
            //재귀 호출로 모든 경우의 수를 탐색한다.
            if(visited[idx]==0){
                visited[idx] = 1;
                DFS(idx+1);
                visited[idx] = 0;
                DFS(idx+1);
            }
        }
    }

    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        //문제 개수
        n = sc.nextInt();
        //제한 시간
        m = sc.nextInt();
        arr = new int[n][2];
        visited= new int[n];

        for(int i=0;i<n;i++){
            arr[i][0] = sc.nextInt();
            arr[i][1] = sc.nextInt();
        }

        T.DFS(0);
        System.out.println(maxScore);
    }
}
```
