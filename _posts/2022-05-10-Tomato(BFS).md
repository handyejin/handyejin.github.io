---
layout: post
title: 토마토(BFS)
date: 2022-05-09 11:57:00
category: Algorithm
---

# [DFS,BFS 활용] 토마토(BFS)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다.

토마토는 아래의 그림과 같이 격자 모양 상자의 칸에 하나씩 넣어서 창고에 보관한다.

<img src = "https://cote.inflearn.com/public/upload/a9d513f5a5.jpg"/>

창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면,

익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다.

하나의 토마토의 인접한 곳은 왼쪽, 오른쪽, 앞, 뒤 네 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며,

토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 현수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지, 그 최소 일수를 알고 싶어 한다.

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때,

며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
6 4<br>
0 0 -1 0 0 0<br>
0 0 1 0 -1 0<br>
0 0 -1 0 0 0<br>
0 0 0 0 -1 1<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">4</h5>

## 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

class Point{
    int x;
    int y;
    public Point(int x, int y){
        this.x  =x;
        this.y = y;
    }
}
public class Main {
    int answer = 0;
    int[] dx= {1,0,-1,0};
    int[] dy = {0,1,0,-1};
    boolean check  = true;

    public int BFS(int[][] box, int[][] visited, int n, int m){
        int day = 0;
        Queue<Point> q = new LinkedList<>();

        for(int i = 0;i<n;i++){
            for(int j = 0;j<m;j++){
                if(box[i][j]==1){
                    q.add(new Point(i,j));
                    visited[i][j] = 1;
                }
            }
        }
        while(!q.isEmpty()){
            int len = q.size();

            for(int j=0;j<len;j++){

                Point tmp = q.poll();
                int x = tmp.x;
                int y = tmp.y;

                for(int i=0;i<4;i++){
                    int mx = x+dx[i];
                    int my = y+dy[i];

                    if(mx>=0 && mx<n &&my>=0 &&my<m && box[mx][my]!=-1 && visited[mx][my]==0){
                        visited[mx][my] = 1;
                        q.add(new Point(mx,my));
                        box[mx][my]= 1;
                    }
                }
            }
            day++;
        }

        for(int i = 0;i<n;i++){
            for(int j = 0;j<m;j++){
                if(box[i][j]==0){
                    check = false;
                }
            }
        }
        if(check==false){
            answer = -1;
        }else{
            answer = day-1;
        }

        return answer;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int m = sc.nextInt();
        int n = sc.nextInt();

        int[][] box = new int[n][m];
        int[][] visited = new int[n][m];
        for(int i=0;i<n;i++){
            for(int j = 0;j<m;j++){
                box[i][j] = sc.nextInt();
            }
        }
        System.out.println(T.BFS(box, visited,n,m));

    }
}
```
