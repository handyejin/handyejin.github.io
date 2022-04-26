---
layout: post
title: 연속 부분수열
date: 2022-04-25 10:03:01
category: Algorithm
---

# [Two Pointers, Sliding window] 연속 부분 수열

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 수로 이루어진 수열이 주어집니다.

이 수열에서 연속부분수열의 합이 특정숫자 M이 되는 경우가 몇 번 있는지 구하는 프로그램을 작성하세요.

만약 N=8, M=6이고 수열이 다음과 같다면

1 2 1 3 1 1 1 2

합이 6이 되는 연속부분수열은 {2, 1, 3}, {1, 3, 1, 1}, {3, 1, 1, 1}로 총 3가지입니다.

<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
8 6<br>
1 2 1 3 1 1 1 2

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int solution(int n, int m,int[] arr){
        int answer=0;
        int sum = 0;
        int start = 0;
        for(int i = 0;i<n;i++){
            sum+=arr[i];
            //sum이 m을 초과했을 때, 더하기 시작했던 지점의 값을 빼준다.
            while(sum>m){
                sum = sum - arr[start];
                start++;
            }
            //sum과 answer이 일치하면 answer을 증가시키고 더하기 시작했던 지점의 값을 sum에서 빼준다.
            if(sum==m){
                answer++;
                sum -= arr[start];
                start++;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        int n,m;
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        m = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(solution(n,m,arr));
    }
}
```

- **two pointers**와 **sliding windows**기법이 복합적으로 사용된 문제였다.
