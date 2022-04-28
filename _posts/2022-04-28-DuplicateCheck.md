---
layout: post
title: 중복 확인
date: 2022-04-28 11:25:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 중복 확인

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수네 반에는 N명의 학생들이 있습니다.

선생님은 반 학생들에게 1부터 10,000,000까지의 자연수 중에서 각자가 좋아하는 숫자 하나 적어 내라고 했습니다.

만약 N명의 학생들이 적어낸 숫자 중 중복된 숫자가 존재하면 D(duplication)를 출력하고,

N명이 모두 각자 다른 숫자를 적어냈다면 U(unique)를 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
8<br>
20 25 52 30 39 33 43 33

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">D</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Arrays;
import java.util.Scanner;

//정렬로 풀면 nlog(n)
public class Main {
    public static char solution(int n, int[] arr){
        char answer='U';
        //정렬
        Arrays.sort(arr);
        //정렬된 배열을 앞에서 부터 확인해서 다음 자료와 같으면 중복이므로 answer을 'D'로 변경한 후, 반복을 끝낸다.
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
