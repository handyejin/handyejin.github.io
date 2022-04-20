---
layout: post
title: 큰 수 출력하기
date: 2022-04-21
category: Algorithm
---

# [Array] 큰 수 출력하기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 정수를 입력받아, 자신의 바로 앞 수보다 큰 수만 출력하는 프로그램을 작성하세요.

(첫 번째 수는 무조건 출력한다)
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
	6<br>
7 3 9 5 6 12

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">7 9 6 12</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public static ArrayList<Integer> solution(int n, int[] arr){
        ArrayList<Integer> list = new ArrayList<Integer>();
        int answer[] = new int[n];
        for(int i = 0;i<n;i++){

            //첫 번째 수이거나, 자신의 바로 앞 숫자가 작을 때, list에 추가 해준다.
            if(i==0||arr[i]>arr[i-1]){
                list.add(arr[i]);
            }
        }
        return list;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int arr[] = new int[n];

        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        for(int x: solution(n,arr)){
            System.out.print(x+" ");
        }
    }
}
```
