---
layout: post
title: 등수 구하기
date: 2022-04-21
category: Algorithm
---

# [Array] 등수 구하기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N명의 학생의 국어점수가 입력되면 각 학생의 등수를 입력된 순서대로 출력하는 프로그램을 작성하세요.

같은 점수가 입력될 경우 높은 등수로 동일 처리한다.

즉 가장 높은 점수가 92점인데 92점이 3명 존재하면 1등이 3명이고 그 다음 학생은 4등이 된다.

<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
87 89 92 100 76

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">4 3 2 1 5</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int[] solution(int n, int[] arr){
        int[] answer = new int[n];
        int rank;
        for(int i=0;i<n;i++){
            //처음 등수는 1로 정한다.
            rank = 1;
            //반복문의 처음부터 끝까지 반복하면서 자신보다 높은 점수가 있을 때 rank를 하나씩 더해준다.
            for(int j = 0;j<n;j++){
                if(arr[i]<arr[j]){
                    rank++;
                }
            }
            answer[i] = rank;
        }
        return answer;

    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] arr = new int[n];

        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        for(int x: solution(n, arr)){
            System.out.print(x + " ");
        }
    }
}
```
