---
layout: post
title: 삽입 정렬
date: 2022-04-28 10:13:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 삽입 정렬

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개이 숫자가 입력되면 오름차순으로 정렬하여 출력하는 프로그램을 작성하세요.

정렬하는 방법은 삽입정렬입니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
6<br>
11 7 5 6 10 9

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">5 6 7 9 10 11</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int[] solution(int n, int arr[]){
        int key;
        int j;
        for(int i=1;i<n;i++){
            key = arr[i];
            for(j =i-1;j>=0;j--){
                //key보다 arr[j]가 크면 뒤로 밀어준다.
                if(arr[j]>key){
                    arr[j+1] = arr[j];
                }
                //key보다 작으면 멈춰준다.
                else break;
            }
            //j의 자리에 key값을 넣어준다.
            arr[j+1] = key;
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

- **삽입 정렬**은 자료 배열의 모든 요소를 **앞에서부터 차례대로 이미 정렬된 배열 부분과 비교**하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘이다.
