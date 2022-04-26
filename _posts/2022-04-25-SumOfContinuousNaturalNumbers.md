---
layout: post
title: 연속된 자연수의 합
date: 2022-04-25 10:05:01
category: Algorithm
---

# [Two Pointers, Sliding window] 연속된 자연수의 합

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N입력으로 양의 정수 N이 입력되면 2개 이상의 연속된 자연수의 합으로 정수 N을 표현하는 방법의 가짓수를 출력하는 프로그램을 작성하세요.

만약 N=15이면

**7+8=15**

**4+5+6=15**

**1+2+3+4+5=15**

와 같이 총 3가지의 경우가 존재한다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
15
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int solution(int n){
        int sum = 0;
        int start =1;
        int answer = 0;
        //1~n-1까지 반복한다.
        //i<=n까지 반복할 경우 n도 포함되어서 2개 이상의 자연수 합이 나올 수 없다.
        for(int i = 1;i<n;i++){

            //sum 에 값들을 더해준다.
            sum+=i;

            //sum이 n을 초과할 경우 sum이 n보다 작아질 때까지 맨 앞의 숫자(start)를 빼준다.
            //start를 증가시켜준다.
            while(sum>n){
                sum-=start;
                start++;
            }
            if(sum==n){
                answer++;
                sum -= start;
                start++;
            }
        }
        return answer;

    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        System.out.println(solution(n));
    }
}
```
