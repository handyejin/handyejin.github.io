---
layout: post
title: 점수 계산
date: 2022-04-21
category: Algorithm
---

# [Array] 점수 계산

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

OX 문제는 맞거나 틀린 두 경우의 답을 가지는 문제를 말한다.

여러 개의 OX 문제로 만들어진 시험에서 연속적으로 답을 맞히는 경우에는 가산점을 주기 위해서 다음과 같이 점수 계산을 하기로 하였다.

1번 문제가 맞는 경우에는 1점으로 계산한다. 앞의 문제에 대해서는 답을 틀리다가 답이 맞는 처음 문제는 1점으로 계산한다.

또한, 연속으로 문제의 답이 맞는 경우에서 두 번째 문제는 2점, 세 번째 문제는 3점, ..., K번째 문제는 K점으로 계산한다. 틀린 문제는 0점으로 계산한다.

예를 들어, 아래와 같이 10 개의 OX 문제에서 답이 맞은 문제의 경우에는 1로 표시하고, 틀린 경우에는 0으로 표시하였을 때,

점수 계산은 아래 표와 같이 계산되어, 총 점수는 1+1+2+3+1+2=10 점이다.

<img src = "https://cote.inflearn.com/public/upload/6080c8e8dc.jpg"/>

시험문제의 채점 결과가 주어졌을 때, 총 점수를 계산하는 프로그램을 작성하시오.

<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
10<br>
1 0 1 1 1 0 0 1 1 0

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">
10
</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int solution(int[] arr, int n){
        int answer=0;
        int count = 0;
        for(int i = 0;i<n;i++){
            //정답일 경우 누적 카운트와 함께 1점을 증가시키고, 카운트를 증가시킨다.
            if(arr[i]==1){
                answer = answer+ 1+count;
                count++;
            //오답일 경우 count를 0으로 만든다.
            }else if(arr[i]==0){
                count=0;
            }
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
        System.out.println(solution(arr,n));
    }
}
```
