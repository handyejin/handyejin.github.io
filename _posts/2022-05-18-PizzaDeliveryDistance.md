---
layout: post
title: 피자 배달 거리
date: 2022-05-17 15:56:00
category: Algorithm
---

# [DFS,BFS 활용] 피자 배달 거리(삼성 SW역량평가 기출문제 : DFS활용)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N×N 크기의 도시지도가 있습니다. 도시지도는 1×1크기의 격자칸으로 이루어져 있습니다.

각 격자칸에는 0은 빈칸, 1은 집, 2는 피자집으로 표현됩니다. 각 격자칸은 좌표(행번호, 열 번호)로 표현됩니다.

행번호는 1번부터 N번까지이고, 열 번호도 1부터 N까지입니다.

도시에는 각 집마다 “피자배달거리”가 았는데 각 집의 피자배달거리는 해당 집과 도시의 존재하는

피자집들과의 거리 중 최소값을 해당 집의 “피자배달거리”라고 한다.

집과 피자집의 피자배달거리는 |x1-x2|+|y1-y2| 이다.

예를 들어, 도시의 지도가 아래와 같다면

<img src = "https://cote.inflearn.com/public/upload/15230e4e41.jpg"/>

(1, 2)에 있는 집과 (2, 3)에 있는 피자집과의 피자 배달 거리는 |1-2| + |2-3| = 2가 된다.

최근 도시가 불경기에 접어들어 우후죽순 생겼던 피자집들이 파산하고 있습니다.

도시 시장은 도시에 있는 피자집 중 M개만 살리고 나머지는 보조금을 주고 폐업시키려고 합니다.

시장은 살리고자 하는 피자집 M개를 선택하는 기준으로 도시의 피자배달거리가 최소가 되는 M개의 피자집을 선택하려고 합니다.

도시의 피자 배달 거리는 각 집들의 피자 배달 거리를 합한 것을 말합니다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
4 4<br>
0 1 2 0<br>
1 0 2 1<br>
0 2 1 2<br>
2 0 1 2<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">6</h5>

## 풀이

```java
import java.util.*;

class Point{
    int x;
    int y;
    Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}
public class Main {
    static ArrayList<Point> house, pz;
    static int[] p;
    static int len, n,m,answer = Integer.MAX_VALUE;

    //조합
    public void DFS(int L, int s){
        if(L==m){
            //도시의 피자배달 거리
            int sum =0;
            //각 집마다 피자 배달거리를 구해서 도시의 피자배달 거리를 구한다.
            for(Point h: house){
                int min = Integer.MAX_VALUE;
                for(int i: p){
                    min =Math.min(min,Math.abs(h.x - pz.get(i).x)+Math.abs(h.y - pz.get(i).y));
                }
                sum+=min;
            }

            //도시 피자배달 거리 중 최소값을 찾는다.
            answer = Math.min(sum,answer);

        }else{
            for(int i = s; i<len; i++){
                p[L] = i;
                DFS(L+1,i+1);
            }

        }

    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        m = sc.nextInt();

        house = new ArrayList<>();
        pz = new ArrayList<>();
        p = new int[m];

        for(int i = 0;i<n;i++){
            for(int j= 0;j<n;j++){
               int tmp = sc.nextInt();
                if(tmp==1){
                    house.add(new Point(i,j));
                }else if(tmp==2){
                    pz.add(new Point(i,j));
                }
            }
        }
        len = pz.size();
        T.DFS(0,0);
        System.out.println(answer);
    }
}
```
