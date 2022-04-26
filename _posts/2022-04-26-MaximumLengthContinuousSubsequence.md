---
layout: post
title: 최대 길이 연속부분수열
date: 2022-04-26 06:05:01
category: Algorithm
---

# [Two Pointers, Sliding window] 최대 길이 연속부분수열

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

0과 1로 구성된 길이가 N인 수열이 주어집니다.<br>
여러분은 이 수열에서 최대 k번을 0을 1로 변경할 수 있습니다.<br>
여러분이 최대 k번의 변경을 통해 이 수열에서 1로만 구성된 최대 길이의 연속부분수열을 찾는 프로그램을 작성하세요.

만약 길이가 길이가 14인 다음과 같은 수열이 주어지고 k=2라면

1 1 0 0 1 1 0 1 1 0 1 1 0 1
<br>
여러분이 만들 수 있는 1이 연속된 연속부분수열은

<img src="https://cote.inflearn.com/public/upload/19123bb35c.jpg"/>

이며 그 길이는 8입니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
14 2<br>
1 1 0 0 1 1 0 1 1 0 1 1 0 1
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">8</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
   public static int solution(int n, int k, int[] arr){
       int answer=0;
       int start=0;
       int tmp = k;
       int length = 0;

       for(int i=0;i<n;i++){

           if(arr[i]==0){
               //0을 발견한 위치가 처음이면 start에 현재 위치를 저장해준다음
               //length는 증가시키고, tmp는 감소시킨다.
               if(tmp==k){
                   start = i;
                   length++;
                   tmp--;
                //tmp가 1~k-1사이에 있으면 그냥 length를 증가시키고 tmp를 감소시킨다.
               }else if(tmp<k && tmp>0){
                   length++;
                   tmp--;
                //tmp가 0이 되었을 때 answer값과 현재 길이를 비교하고,
                //처음 0이 발견된 위치로 i를 바꿔서 다시 길이를 구해준다.
               }else if(tmp==0){
                   answer = Math.max(answer,length);
                   i = start;
                   length=0;
                   tmp = k;
               }
           }
           //1을 만나면 length를 증가시킨다.
           if(arr[i]==1){
               length++;
           }
       }
       //마지막 0을 만났을 때 확인
       answer = Math.max(answer,length);

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

## 다른 풀이

```java
import java.util.Scanner;

public class Main {
    public static int solution(int n, int k, int[] arr){
        int answer=0;

        int cnt = 0;
        int lt=0;
        for(int rt = 0;rt<n;rt++){
            //rt가 0을 만나면 cnt를 증가시킨다.
            if(arr[rt]==0){
                cnt++;
            }
            //cnt가 k보다 커지면 lt를 증가시키고 0이 발견되면 cnt를 감소시켜 반복문을 빠져나온다.
            while(cnt>k){
                if(arr[lt]==0){
                    cnt--;
                }
                lt++;
            }
            //rt-lt+1은 현재 길이
            //현재길이와 answer을 비교해준다.
            answer = Math.max(answer,rt-lt+1);


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
