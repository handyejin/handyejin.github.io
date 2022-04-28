---
layout: post
title: 버블 정렬
date: 2022-04-28 10:09:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 버블 정렬

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개이 숫자가 입력되면 오름차순으로 정렬하여 출력하는 프로그램을 작성하세요.

정렬하는 방법은 버블정렬입니다.

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
import java.util.Scanner;

public class Main {
    public static int[] solution(int n, int arr[]){
        int tmp;
        for(int i=n-1;i>=0;i--){
            for(int j = 0;j<i;j++){
                if(arr[j]>arr[j+1]){
                    tmp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = tmp;
                }
            }
        }
        return arr;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        for(int x : solution(n,arr)){
            System.out.print(x+" ");
        }
    }
}
```

- **선택정렬**은 서로 _**인접한**_ 두 원소를 검사하여 정렬하는 알고리즘이다. 인접한 두개의 레코드를 비교하여 크기가 순서대로 되어있지 않으면 서로 ***교환***한다.
