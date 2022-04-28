---
layout: post
title: 선택 정렬
date: 2022-04-28 10:02:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 선택 정렬

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개이 숫자가 입력되면 오름차순으로 정렬하여 출력하는 프로그램을 작성하세요.

정렬하는 방법은 선택정렬입니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
6<br>
13 5 11 7 23 15

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">5 7 11 13 15 23</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Arrays;
import java.util.Scanner;

//정렬로 풀면 nlog(n)
public class Main {
    public static char solution(int n, int[] arr){
        char answer='U';
        Arrays.sort(arr);
        for(int i=0;i<n-1;i++){
            if(arr[i]==arr[i+1]){
                answer='D';
                break;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n=sc.nextInt();
        int[] arr = new int[n];
        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(solution(n,arr));

    }
}
```
