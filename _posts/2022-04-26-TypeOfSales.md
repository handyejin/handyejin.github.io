---
layout: post
title: 매출액의 종류
date: 2022-04-26 10:35:00
category: Algorithm
---

# [HashMap, TreeSet] 매출액의 종류

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수의 아빠는 제과점을 운영합니다. 현수아빠는 현수에게 N일 동안의 매출기록을 주고 연속된 K일 동안의 매출액의 종류를

각 구간별로 구하라고 했습니다.

만약 N=7이고 7일 간의 매출기록이 아래와 같고, 이때 K=4이면

20 12 20 10 23 17 10

각 연속 4일간의 구간의 매출종류는

첫 번째 구간은 [20, 12, 20, 10]는 매출액의 종류가 20, 12, 10으로 3이다.

두 번째 구간은 [12, 20, 10, 23]는 매출액의 종류가 4이다.

세 번째 구간은 [20, 10, 23, 17]는 매출액의 종류가 4이다.

네 번째 구간은 [10, 23, 17, 10]는 매출액의 종류가 3이다.

N일간의 매출기록과 연속구간의 길이 K가 주어지면 첫 번째 구간부터 각 구간별

매출액의 종류를 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
7 4<br>
20 12 20 10 23 17 10

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3 4 4 3</h5>

<br>

## 풀이

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Scanner;

public class Main {
    public static ArrayList<Integer> solution(int[] arr, int n, int k){
        ArrayList<Integer> answer = new ArrayList<>();
        HashMap<Integer,Integer> map = new HashMap<>();
        //슬라이딩 윈도우 시작점
        int lt=0;

        //첫번째 구간을 맵에 저장해준다.
        for(int i = 0;i<k;i++){
            map.put(arr[i],map.getOrDefault(arr[i],0)+1);
        }

        //Key의 갯수를 확인해서 answer리스트에 추가해준다.
        answer.add(map.size());

        //두번째 구간부터 탐색한다.
        for(int i = k;i<n;i++){
            //첫번째 값을 지운다. 이 때, 값이 1이면 키를 지운다
            if(map.get(arr[lt])==1){
                map.remove(arr[lt]);
            }else{
                map.put(arr[lt],map.get(arr[lt])-1);
            }
            lt++;
            map.put(arr[i],map.getOrDefault(arr[i],0)+1);
            answer.add(map.size());
        }

        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        int[] arr = new int[n];
        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        for(int x:solution(arr,n,k) ){
            System.out.print(x+" ");
        }
    }
}
```

- **HashMap**과 **SlidingWindow 기법**을 사용한 풀이이다.
