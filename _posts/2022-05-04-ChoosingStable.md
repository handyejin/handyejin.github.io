---
layout: post
title: 마구간 정하기(결정 알고리즘)
date: 2022-05-04 11:48:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 마구간 정하기(결정 알고리즘)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 마구간이 수직선상에 있습니다. 각 마구간은 x1, x2, x3, ......, xN의 좌표를 가지며, 마구간간에 좌표가 중복되는 일은 없습니다.

현수는 C마리의 말을 가지고 있는데, 이 말들은 서로 가까이 있는 것을 좋아하지 않습니다. 각 마구간에는 한 마리의 말만 넣을 수 있고,

가장 가까운 두 말의 거리가 최대가 되게 말을 마구간에 배치하고 싶습니다.

C마리의 말을 N개의 마구간에 배치했을 때 가장 가까운 두 말의 거리가 최대가 되는 그 최대값을 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5 3<br>
1 2 8 4 9

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">17</h5>
3

<div style="height:20px;"></div>

## 풀이

```java
import java.lang.reflect.Array;
import java.util.Arrays;
import java.util.Scanner;

public class Main {
    public int count (int[] arr, int capacity){
        int count = 1;
        int q = 0;
        for(int i=0;i<arr.length;i++){
            if(arr[i]-arr[q]>=capacity){
                count++;
                q = i;
            }

        }
        return count;
    }
    public int solution(int n, int c, int[] arr){
        //차이가 가장 큰 경우: rt
        int answer = 0;
        Arrays.sort(arr);

        int lt = 1;
        int rt = arr[n-1];
        while(lt<=rt){
            int mid = (lt+rt)/2;
            if(count(arr,mid)<c){
                rt = mid-1;
            }else if(count(arr,mid)>=c){
                answer = mid;
                lt = mid+1;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int c = sc.nextInt();
        int[] arr = new int[n];
        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(T.solution(n,c,arr));
    }
}
```
