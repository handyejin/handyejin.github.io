---
layout: post
title: 섬나라 아일랜드
date: 2022-05-09 11:59:00
category: Algorithm
---

# [DFS,BFS 활용] 섬나라 아일랜드

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N\*N의 섬나라 아일랜드의 지도가 격자판의 정보로 주어집니다.

각 섬은 1로 표시되어 상하좌우와 대각선으로 연결되어 있으며, 0은 바다입니다.

섬나라 아일랜드에 몇 개의 섬이 있는지 구하는 프로그램을 작성하세요.

<img src = "https://cote.inflearn.com/public/upload/7c81fe29cd.jpg"/>

만약 위와 같다면 섬의 개수는 5개입니다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
7<br>
1 1 0 0 0 1 0<br>
0 1 1 0 1 1 0<br>
0 1 0 0 0 0 0<br>
0 0 0 1 0 1 1<br>
1 1 0 1 1 0 0<br>
1 0 0 0 1 0 0<br>
1 0 1 0 1 0 0<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">5</h5>

## 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

class Point{
    int x;
    int y;

    Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}
public class Main {
    static int[][] visited, arr;
    int[] dx = {1,0,-1,0,-1,1,-1,1};
    int[] dy = {0,1,0,-1,1,1,-1,-1};
    public int BFS(int n){
        int count = 0;
        Queue<Point> q = new LinkedList<>();
        for(int i = 0;i<n;i++){
            for(int j = 0;j<n;j++){
                if(arr[i][j]==1 && visited[i][j]==0){
                    visited[i][j]=1;
                    count++;

                    q.add(new Point(i,j));
                    while(!q.isEmpty()){
                        Point p = q.poll();
                        int x = p.x;
                        int y = p.y;
                        //현재 위치의 상하좌우 및 대각선 탐색
                        for(int k = 0;k<8;k++){
                            int nx = x+dx[k];
                            int ny = y+dy[k];
                            if(nx>=0 &&nx<n &&ny>=0 && ny<n){
                                if(visited[nx][ny]==0 &&arr[nx][ny]==1){
                                    visited[nx][ny] = 1;
                                    q.add(new Point(nx, ny));
                                }
                            }

                        }
                    }
                }
            }
        }
        return count;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        arr = new int[n][n];
        visited = new int[n][n];

        for(int i = 0;i<n;i++){
            for(int j = 0;j<n;j++){
                arr[i][j] = sc.nextInt();
            }
        }
        System.out.println(T.BFS(n));
    }
}
```
