---
layout: post
title: 뒤집은 소수
date: 2022-04-21
category: Algorithm
---

# [Array] 뒤집은 소수

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 자연수가 입력되면 각 자연수를 뒤집은 후 그 뒤집은 수가 소수이면 그 소수를 출력하는 프로그램을 작성하세요.

예를 들어 32를 뒤집으면 23이고, 23은 소수이다. 그러면 23을 출력한다. 단 910를 뒤집으면 19로 숫자화 해야 한다.

첫 자리부터의 연속된 0은 무시한다.
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
9<br>
32 55 62 20 250 370 200 30 100

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">
23 2 73 2 3
</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {

    //정수를 문자열로 바꾸어서 뒤집어준 후, 다시 정수로 바꾸어줌.
   public static int reverse(int n){
       String tmp = Integer.toString(n);
       tmp = new StringBuilder(tmp).reverse().toString();
       int result = Integer.parseInt(tmp);

       return result;
   }

   public static ArrayList<Integer> solution(int[] arr, int n){
       ArrayList<Integer> answer = new ArrayList<Integer>();
       int check ;
       for(int i =0;i<n;i++){
           arr[i] = reverse(arr[i]);
       }

        //현재수를 2부터 현재 수의 전까지 나눈 후, 나누어 떨어지면 소수가 아니다.
       for(int i = 0;i<n;i++){
           check=0;
           for(int j = 2;j<arr[i];j++){
               if(arr[i]%j==0){
                   check = 1;
                   break;
               }
           }
           //1일 때도 소수가 아니다.
           if(arr[i]!=1&&check==0){
               answer.add(arr[i]);
           }
       }

       return answer;
   }
   public static void main(String[] args){
       Scanner sc = new Scanner(System.in);
       int n = sc.nextInt();
       int[] arr = new int[n];

       for(int i = 0;i<n;i++){
           arr[i] = sc.nextInt();
       }
       for(int x: solution(arr,n)){
           System.out.print(x+" ");
       }


   }
}
```

## 다른 사람의 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {

    //소수인지 판별해주는 메서드
    public static boolean isPrime(int n){
        if (n==1) return false;
        for(int i = 2;i<n;i++){
            if(n%i==0){
                return false;
            }
        }
        return true;
    }

    public static ArrayList<Integer> solution(int[] arr, int n){
        ArrayList<Integer> answer = new ArrayList<Integer>();
        for(int i=0;i<n;i++){
            int tmp = arr[i];
            int res = 0;


            while(tmp>0){
                //tmp를 10으로 나눈 나머지를 구하여 가장 마지막 자리의 숫자 값을 구한다.
                int t = tmp%10;

                //현재 res에 10을 곱한 후, 나머지인 t를 더하여 res에 저장한다.
                res = res*10+t;

                //tmp를 10으로 나누어서 마지막 자리 숫자를 제거한다.
                tmp/=10;
            }

            //res가 소수라면 answer 리스트에 추가해준다.
            if(isPrime(res)==true){
                answer.add(res);
            }

        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];

        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        for(int x: solution(arr,n)){
            System.out.print(x+" ");
        }


    }
}
```
