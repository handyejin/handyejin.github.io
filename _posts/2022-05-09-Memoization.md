---
layout: post
title: 조합의 경우수(메모이제이션)
date: 2022-05-09 11:53:00
category: Algorithm
---

# [DFS,BFS 활용] 조합의 경우수(메모이제이션)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

<img src ="https://cote.inflearn.com/public/upload/8f99ebbe8d.jpg"/>로 계산합니다.

하지만 여러분은 이 공식을 쓰지않고 다음 공식을 사용하여 재귀를 이용해 조합수를 구해주는 프로그램을 작성하세요.

<img src = "https://cote.inflearn.com/public/upload/b4a8e9f795.jpg"/>

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5 3
</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">10</h5>

<div style="height:20px;"></div>

#### 예시 입력2

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
33 19

</h5>

#### 예시 출력2

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">818809200</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;
public class Main {
    static int n;
    static int r;
    static int count;
    static int[] visited;
    int[][] dy = new int[35][35];
    ArrayList<Integer> list = new ArrayList<>();

    public int DFS(int n, int r){
        //이미 구한 값이면 그 값을 리턴해준다.
        if(dy[n][r]>0){
            return dy[n][r];
        }
        if(r ==0 ||n==r){
            return 1;
        }else{
            //메모이제이션
           return dy[n][r] = DFS(n-1,r-1) + DFS(n-1, r);
        }
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        r = sc.nextInt();
        count = 0;
        //1~n
        visited = new int[n+1];

        int answer = T.DFS(n,r);
        System.out.println(answer);
    }
}
```
