---
layout: post
title: 미로의 최단거리 통로(BFS)
date: 2022-05-09 11:56:00
category: Algorithm
---

# [DFS,BFS 활용] 미로의 최단거리 통로(BFS)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

7\*7 격자판 미로를 탈출하는 최단경로의 길이를 출력하는 프로그램을 작성하세요.

경로의 길이는 출발점에서 도착점까지 가는데 이동한 횟수를 의미한다.

출발점은 격자의 (1, 1) 좌표이고, 탈출 도착점은 (7, 7)좌표이다. 격자판의 1은 벽이고, 0은 도로이다.

격자판의 움직임은 상하좌우로만 움직인다. 미로가 다음과 같다면

<img src = "https://cote.inflearn.com/public/upload/88ff3b120f.jpg"/>

위와 같은 경로가 최단 경로의 길이는 12이다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
0 0 0 0 0 0 0<br>
0 1 1 1 1 1 0<br>
0 0 0 1 0 0 0<br>
1 1 0 1 0 1 1<br>
1 1 0 1 0 0 0<br>
1 0 0 0 1 0 0<br>
1 0 1 0 0 0 0<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">12</h5>

## 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

class Point{
    int x;
    int y;

    int cnt;
    public Point(int x, int y, int cnt){
        this.x  =x;
        this.y = y;

        this.cnt = cnt;
    }
}
public class Main {
    int answer = 0;

    int[] dx= {1,0,-1,0};
    int[] dy = {0,1,0,-1};
    boolean check  = true;
    public int BFS(int[][] box, int[][] visited){

        Queue<Point> q = new LinkedList<>();
        q.add(new Point(0,0, 0));
        visited[0][0] = 1;

        while(!q.isEmpty()){
            int len = q.size();

            for(int j=0;j<len;j++){
                //큐의 맨 앞에 값을 받는다.
                Point tmp = q.poll();
                int x = tmp.x;
                int y = tmp.y;
                int cnt = tmp.cnt;

                if(x==6 && y==6){
                    return cnt;
                }
                //현재 위치의 상하좌우 탐색
                for(int i=0;i<4;i++){
                    int mx = x+dx[i];
                    int my = y+dy[i];

                    //nx,ny가 배열 범위를 초과하지 않고, 도로이고, 방문하지 않았다면 방문표시를 해주고, 큐에 추가해준다.
                    if(mx>=0 && mx<7 &&my>=0 &&my<7 && box[mx][my]==0 && visited[mx][my]==0){
                        visited[mx][my] = 1;
                        q.add(new Point(mx,my,cnt+1));

                    }
                }
            }

        }

        return -1;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);


        int[][] miro = new int[7][7];
        int[][] visited = new int[7][7];
        for(int i=0;i<7;i++){
            for(int j = 0;j<7;j++){
                miro[i][j] = sc.nextInt();
            }
        }

        System.out.println(T.BFS(miro, visited));

    }
}
```
