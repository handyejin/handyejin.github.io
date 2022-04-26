---
layout: post
title: 공통원소 구하기
date: 2022-04-25
category: Algorithm
---

# [Two Pointers, Sliding window] 공통 원소 구하기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

A, B 두 개의 집합이 주어지면 두 집합의 공통 원소를 추출하여 오름차순으로 출력하는 프로그램을 작성하세요.
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
1 3 9 5 2<br>
5<br>
3 2 5 7 8<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">2 3 5</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;
import java.util.*;

public class Main {
   public static ArrayList<Integer> solution(int[] arr1, int[] arr2, int n, int m){
       ArrayList<Integer> answer = new ArrayList<Integer>();
       //2중 반복문을 이용하여 공통 원소를 찾는다.
       for(int i = 0;i<m;i++){
           for(int j = 0;j<m;j++){
               if(arr1[i]==arr2[j]){
                   answer.add(arr1[i]);
                   break;
               }
           }
       }
       Collections.sort(answer);

       return answer;

   }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr1 = new int[n];
        for(int i=0;i<n;i++){
            arr1[i] = sc.nextInt();
        }
        int m = sc.nextInt();
        int[] arr2 = new int[m];
        for(int i = 0;i<m;i++){
            arr2[i] = sc.nextInt();
        }
        for(long x: main.solution(arr1, arr2, n, m)){
            System.out.print(x+" ");
        }
    }
}
```

- 2중 반복문을 이용하여 공통 원소를 구하니 <span style="color:red">**시간 초과**</span>가 나왔다.
  <br>

## Two Pointers를 이용한 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;
import java.util.*;

public class Main {
    public static ArrayList<Integer> solution(int[] arr1, int[] arr2, int n, int m){
        ArrayList<Integer> answer = new ArrayList<Integer>();

        //두 배열을 정렬해준다.
        Arrays.sort(arr1);
        Arrays.sort(arr2);

        //p1은 arr1을 탐색하고, p2는 arr2를 탐색한다.
        int p1=0,p2=0;
        while(p1<n&&p2<m){
            //arr1의 현재 요소가 arr2의 요소보다 작다면 두 요소가 같아질 수 없으므로 p1을 증가시켜 다음 요소를 탐색하게 한다.
            if(arr1[p1]<arr2[p2]){
                p1++;
            }else if(arr1[p1]>arr2[p2]){
                p2++;
            }else{
                answer.add(arr1[p1]);
                p1++;
                p2++;
            }
        }
        return answer;
    }

    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr1 = new int[n];
        for(int i=0;i<n;i++){
            arr1[i] = sc.nextInt();
        }
        int m = sc.nextInt();
        int[] arr2 = new int[m];
        for(int i = 0;i<m;i++){
            arr2[i] = sc.nextInt();
        }

        for(long x: main.solution(arr1, arr2, n, m)){
            System.out.print(x+" ");
        }

    }
}
```

- **Two Pointer**을 만들어 반복문을 1번만 사용하여 **시간복잡도**를 줄였다.
