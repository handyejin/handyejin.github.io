---
layout: post
title: 합이 같은 부분집합(DFS:아마존 인터뷰)
date: 2022-05-04 12:14:00
category: Algorithm
---

# [DFS,BFS 활용] 합이 같은 부분집합(DFS:아마존 인터뷰)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 원소로 구성된 자연수 집합이 주어지면, 이 집합을 두 개의 부분집합으로 나누었을 때

두 부분집합의 원소의 합이 서로 같은 경우가 존재하면 “YES"를 출력하고, 그렇지 않으면 ”NO"를 출력하는 프로그램을 작성하세요.

둘로 나뉘는 두 부분집합은 서로소 집합이며, 두 부분집합을 합하면 입력으로 주어진 원래의 집합이 되어 합니다.

예를 들어 {1, 3, 5, 6, 7, 10}이 입력되면 {1, 3, 5, 7} = {6, 10} 으로 두 부분집합의 합이 16으로 같은 경우가 존재하는 것을 알 수 있다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
6<br>
1 3 5 6 7 10

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">YES</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Scanner;

public class Main {

    static int[] visited;
    static int[] arr;
    static int n;
    static String answer = "";

    public void DFS(int idx){
        //정점 끝에 도달했을 때, 방문한 요소의 합과 방문하지 않은 요소의 합을 구해 비교해준다.
        if(idx==n){
            int sum1 =0;
            int sum2 = 0;
            for(int i=0;i<n;i++){
                if(visited[i]==1){
                    sum1+=arr[i];
                }
                else if(visited[i]==0){
                    sum2+=arr[i];
                }
            }
            if(sum1==sum2){
                answer = "YES";
            }
            return;
        }

        else{
            //아직 방문하지 않았으면 방문 표시를 해주고
            //다음 인덱스를 매개변수로 재귀호출한다.
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
        n = sc.nextInt();
        arr = new int[n];
        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        visited = new int[n];
        T.DFS(0);
        if(answer ==null || answer.isEmpty()){
            System.out.println("NO");
        }else{
            System.out.println("YES");
        }
    }
}
```
