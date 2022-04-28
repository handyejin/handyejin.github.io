---
layout: post
title: Least Recently Used
date: 2022-04-28 11:20:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] Least Recently Used

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

캐시메모리는 CPU와 주기억장치(DRAM) 사이의 고속의 임시 메모리로서 CPU가 처리할 작업을 저장해 놓았다가

필요할 바로 사용해서 처리속도를 높이는 장치이다. 워낙 비싸고 용량이 작아 효율적으로 사용해야 한다.

철수의 컴퓨터는 캐시메모리 사용 규칙이 LRU 알고리즘을 따른다.

LRU 알고리즘은 Least Recently Used 의 약자로 직역하자면 가장 최근에 사용되지 않은 것 정도의 의미를 가지고 있습니다.

캐시에서 작업을 제거할 때 가장 오랫동안 사용하지 않은 것을 제거하겠다는 알고리즘입니다.

<img src="https://cote.inflearn.com/public/upload/c366c701c2.jpg"/>

캐시의 크기가 주어지고, 캐시가 비어있는 상태에서 N개의 작업을 CPU가 차례로 처리한다면 N개의 작업을 처리한 후

캐시메모리의 상태를 가장 최근 사용된 작업부터 차례대로 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5 9<br>
1 2 3 2 6 2 3 5 7

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">7 5 3 2 6</h5>

<div style="height:20px;"></div>

## 리스트를 이용한 풀이

```java
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
   public static void solution(int s, int n, int[] arr){
       ArrayList<Integer> list = new ArrayList<>();
       for(int x: arr){
           //리스트의 크기가 꽉차지 않았을 때,
           if(list.size()<s){
               //hit가 아니면 리스트에 추가해준다.
               if(!list.contains(x)){
                   list.add(x);
                //hit이면 0부터 찾아 제거해주고, 리스트에 추가해준다.
               }else{
                   for(int i=0;i< list.size();i++){
                       if(list.get(i)==x){
                           list.remove(i);
                           list.add(x);
                           break;
                       }
                   }
               }
           }
           //리스트의 크기가 5를 넘었을 때
           else{
               //hit가 아니면 가장 오래된 0번째 자료를 지워주고, x를 리스트의 마지막에 추가해준다.
               if(!list.contains(x)){
                   list.remove(0);
                   list.add(x);
                //hit이면 리스트에서 지우고 리스트의 마지막에 추가해준다.
               }else{
                   for(int i=0;i< list.size();i++){
                       if(list.get(i)==x){
                           list.remove(i);
                           list.add(x);
                           break;
                       }
                   }
               }
           }
       }
       for(int i= list.size()-1;i>=0;i--){
           System.out.print(list.get(i)+" ");
       }
   }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int s = sc.nextInt();
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i =0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        for(int x: solution(s,n,arr)){
            System.out.print(x+" ");
        }


    }
}
```

## 배열을 이용한 풀이

```java
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
   public static int[] solution(int s, int n, int[] arr){
       int[] cache = new int[s];

       for(int x: arr){
           int pos=-1;
           //hit인지 확인
           for(int i=0;i<s;i++){
               //x가 hit라면 position에 i를 저자안다.
               if(x==cache[i]){
                   pos = i;
               }
           }
           //hit가 아니라면 뒤에서부터 오른쪽으로 값을 옮겨준다.
           if(pos==-1){
               for(int i=s-1;i>=1;i--){
                   cache[i] = cache[i-1];
               }
           //hit라면 pos앞 자료들을 오른쪽으로 옮겨준다.
           }else{
               for(int i = pos;i>=1;i--){
                   cache[i] = cache[i-1];
               }
           }
           //비어있는 0번자리에 자료를 저장한다.
           cache[0] = x;
       }
       return cache;
   }


    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int s = sc.nextInt();
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i =0;i<n;i++){
            arr[i] = sc.nextInt();
        }

       for(int x: solution(s,n,arr)){
           System.out.print(x+" ");
       }


    }
}
```
