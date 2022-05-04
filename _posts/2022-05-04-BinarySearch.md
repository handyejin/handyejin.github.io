---
layout: post
title: 이분검색
date: 2022-05-04 11:35:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 이분 검색

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

임의의 N개의 숫자가 입력으로 주어집니다. N개의 수를 오름차순으로 정렬한 다음 N개의 수 중 한 개의 수인 M이 주어지면

이분검색으로 M이 정렬된 상태에서 몇 번째에 있는지 구하는 프로그램을 작성하세요. 단 중복값은 존재하지 않습니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
8 32<br>
23 87 65 12 57 32 99 81

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">
3
</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
    public static int solution(int n, int m, int[] arr){
        //이분탐색을 하기 위해 정렬한다.
        Arrays.sort(arr);
        int lt = 0;
        int rt = n-1;

        while(lt<=rt){

            int mid = (lt+rt)/2;
            //mid의 값이 찾는 값이면 mid에 1을 더해 리턴한다.
            if(arr[mid]==m){
                return mid+1;
            }
            //mid의 값이 찾고자 하는 값 보다 크면
            //rt를 mid의 왼쪽으로 옮긴다.
            else if(arr[mid]>m){
                rt = mid-1;

            //mid의 값이 찾고자 하는 값보다 작으면
            //lt를 mid의 오른쪽으로 옮긴다.
            }else if(arr[mid]<m){
                lt = mid+1;
            }
        }
        return -1;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n  = sc.nextInt();
        int m = sc.nextInt();
        int[] arr = new int[n];
        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(solution(n, m, arr));
    }
}
```
