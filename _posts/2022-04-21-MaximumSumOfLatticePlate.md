---
layout: post
title: 격자판 최대 합
date: 2022-04-21
category: Algorithm
---

# [Array] 격자판 최대 합

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

5\*5 격자판에 아래롸 같이 숫자가 적혀있습니다.

<img src = "https://cote.inflearn.com/public/upload/4897574b00.jpg"/>

N\*N의 격자판이 주어지면 각 행의 합, 각 열의 합, 두 대각선의 합 중 가 장 큰 합을 출력합니다.

<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
10 13 10 12 15<br>
12 39 30 23 11<br>
11 25 50 53 15<br>
19 27 29 37 27<br>
19 13 30 13 19<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">155</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
   public static int solution(int n, int[][] arr){
       int answer=0;
       int maxSum = 0;
       int rowSum = 0;
       int colSum = 0;
       int diagonalSum1 = 0;
       int diagonalSum2 = 0;

       //행의 최대 합 구하기
       for(int i=0;i<n;i++){
           rowSum = 0;
           for(int j=0;j<n;j++){
               rowSum +=arr[i][j];
           }
           if(maxSum<rowSum){
               maxSum = rowSum;
           }
       }
       //열의 최대 합 구하기
       for(int i=0;i<n;i++){
           colSum = 0;
           for(int j=0;j<n;j++){
               colSum +=arr[j][i];
           }
           if(maxSum<colSum){
               maxSum = colSum;
           }
       }
       //두 대각선 합 구하기
       for(int i = 0;i<n;i++){
           for(int j = 0;j<n;j++){
               if(i==j){
                   diagonalSum1+=arr[i][j];
               }else if(i+j==n-1){
                   diagonalSum2+=arr[i][j];
               }
           }
       }
       //현재 최대 합 보다 두 대각선 중 더 큰 값이 크다면 최대합 변경해주기
       if(maxSum<Math.max(diagonalSum1,diagonalSum2)){
           maxSum = Math.max(diagonalSum1,diagonalSum2);
       }
       answer = maxSum;
       return answer;
   }

   public static void main(String[] args){
       Scanner sc = new Scanner(System.in);
       int n = sc.nextInt();
       int[][] arr = new int[n][n];
       for(int i=0;i<n;i++){
           for(int j = 0;j<n;j++){
               arr[i][j] = sc.nextInt();
           }
       }
       System.out.println(solution(n,arr));

   }
}
```

## 다른 사람의 풀이

```java
import java.util.Scanner;

public class Main {
   public static int solution(int n, int[][] arr){
       int answer=Integer.MIN_VALUE;
       int rowSum ,colSum;
       int diagonalSum1=0,diagonalSum2=0;
       //행 열의 합 동시에 구해주기
       for(int i =0;i<n;i++){
           rowSum = colSum=0;
           for(int j = 0;j<n;j++){
               rowSum += arr[i][j];
               colSum += arr[j][i];
           }
           answer = Math.max(answer,colSum);
           answer = Math.max(answer,rowSum);
       }
       for(int i = 0;i<n;i++){
           diagonalSum1 += arr[i][i];
           diagonalSum2 +=arr[i][n-i-1];
       }
       answer = Math.max(answer,diagonalSum1);
       answer = Math.max(answer,diagonalSum2);


       return answer;
   }

   public static void main(String[] args){
       Scanner sc = new Scanner(System.in);
       int n = sc.nextInt();
       int[][] arr = new int[n][n];
       for(int i=0;i<n;i++){
           for(int j = 0;j<n;j++){
               arr[i][j] = sc.nextInt();
           }
       }
       System.out.println(solution(n,arr));

   }
}
```

- 이 코드는 행과 열의 합을 동시에 구해서 더 짧게 코드를 완성하였다.
