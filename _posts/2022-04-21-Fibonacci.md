---
layout: post
title: 피보나치 수열
date: 2022-04-21
category: Algorithm
---

# [Array] 피보나치 수열

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

1. 피보나키 수열을 출력한다. 피보나치 수열이란 앞의 2개의 수를 합하여 다음 숫자가 되는 수열이다.

2. 입력은 피보나치 수열의 총 항의 수 이다. 만약 7이 입력되면 1 1 2 3 5 8 13을 출력하면 된다.
   <br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
10
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">
1 1 2 3 5 8 13 21 34 55</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int[] solution(int n){
        int answer[] = new int[n];

        answer[0]=1;
        answer[1]=1;

        for(int i = 2;i<n;i++){
            //앞에 두 자리를 더하여 수열을 만들어준다.
            answer[i] = answer[i-1]+answer[i-2];
        }

        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        for(int x :solution(n)){
            System.out.print(x+" ");

        }

    }
}
```
