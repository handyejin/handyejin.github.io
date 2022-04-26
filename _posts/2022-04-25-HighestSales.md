---
layout: post
title: 최대 매출
date: 2022-04-25
category: Algorithm
---

# [Two Pointers, Sliding window] 최대 매출

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수의 아빠는 제과점을 운영합니다. 현수 아빠는 현수에게 N일 동안의 매출기록을 주고 연속된 K일 동안의 최대 매출액이 얼마인지 구하라고 했습니다.

만약 N=10이고 10일 간의 매출기록이 아래와 같습니다. 이때 K=3이면

12 15 <span style="color:red">11 20 25</span> 10 20 19 13 15

연속된 3일간의 최대 매출액은 11+20+25=56만원입니다.

<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
10 3<br>
12 15 11 20 25 10 20 19 13 15

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">56</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
   public static int solution(int n, int k, int[] arr){
       int maxSum = Integer.MIN_VALUE;
       int sum;
       //2중 반복문 사용하여 k일 동안의 최대 매출액 구하기
       for(int i = 0;i<n-k+1;i++){
           sum = 0;
           for(int j = i;j<i+k;j++){
               sum+=arr[j];
           }
           if(sum>maxSum){
               maxSum = sum;
           }
       }

       return maxSum;
   }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(solution(n,k,arr));


    }
}
```

- 2중 반복문을 이용하여 공통 원소를 구하면 <span style="color:red">**시간 초과**</span>가 나온다.
- **O(n^2)** 만큼 시간이 소요된다.
  <br>

## Sliding window를 이용한 풀이

```java
import java.util.Scanner;

public class Main {
    public static int solution(int n, int k, int[] arr){
        int sum = 0;
        int answer;
        int tmp ;
        //첫번째 k번째만큼 합을 구한다.
        for(int i = 0;i<k;i++){
            sum+= arr[i];
        }
        answer = sum;
        //k번째부터 반복하면서 앞에 값을 빼주고 뒤에 값을 더해주면서 비교해준다.
        for(int i = k;i<n;i++){
            sum +=arr[i] - arr[i-k];
            answer = Math.max(answer,sum);

        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(solution(n,k,arr));
    }
}
```

- 입력받을수를 k개를 짤라서 더해주고 다음 차례로 넘어갈 때마다 앞의 값을 빼주고 뒤에 값을 더해주는 방법을 사용하였다. <br> 이를 이용하면 **중복되는 요소**를 재사용하여 불필요한 연산을 줄일 수 있다. 이 방법이 **Sliding window** 알고리즘이다.
