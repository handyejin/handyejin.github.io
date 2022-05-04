---
layout: post
title: 동전교환
date: 2022-05-04 13:20:00
category: Algorithm
---

# [DFS,BFS 활용] 바둑이 승차(DFS)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

다음과 같이 여러 단위의 동전들이 주어져 있을때 거스름돈을 가장 적은 수의 동전으로 교환해주려면 어떻게 주면 되는가?

각 단위의 동전은 무한정 쓸 수 있다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
3<br>
1 2 5<br>
15<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

<div style="height:20px;"></div>

## 풀이

#### BFS 풀이

```java
import java.util.*;

public class Main {
   public int BFS(int n, int[] arr, int change){
       Queue<Integer> q = new LinkedList<>();
       int cnt = 0;

       ArrayList<Integer> visited = new ArrayList<>();
       visited.add(0);
       q.add(0);

       while(!q.isEmpty()){
           int len = q.size();
           for(int i = 0;i<len;i++){
               int poll = q.poll();
               if(poll==change){
                   return cnt;
               }

               for(int j =0;j<n;j++){
                   int tmp = poll+arr[j];
                   if(!visited.contains(tmp)){
                       visited.add(tmp);
                       q.add(tmp);
                   }
               }
           }
           cnt++;
       }
       return cnt;
   }
   public static void main(String[] args){
       Main T= new Main();
       Scanner sc = new Scanner(System.in);
       int n = sc.nextInt();
       int[] arr = new int[n];
       for(int i=0;i<n;i++){
           arr[i] = sc.nextInt();
       }
       int change = sc.nextInt();

       System.out.println(T.BFS(n,arr,change));
   }
}
```

<div style="height:20px;"></div>
#### DFS 풀이

```java
public class Main {
    static int n, change , answer = Integer.MAX_VALUE;
    static int[] arr;


    public void DFS(int Level, int sum ){
        if(sum>change) return;
        if(sum==change){
            answer = Math.min(answer,Level);
        }else{
            //모든 가지의 수를 돌면서, 다름 레벨과 그 레벨일 때 합을 넘겨준다.
            for(int i = 0; i<n;i++){
                DFS(Level+1, sum+arr[i]);
            }
        }

    }
    public static void main(String[] args){
        Main T= new Main();
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        arr = new int[n];
        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        change = sc.nextInt();

        T.DFS(0,0);
        System.out.println(answer);
    }
}
```
