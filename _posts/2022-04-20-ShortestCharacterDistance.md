---
layout: post
title: 가장 짧은 문자 거리
date: 2022-04-20
category: Algorithm
---

# [String]가장 짧은 문자 거리

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

한 개의 문자열 s와 문자 t가 주어지면 문자열 s의 각 문자가 문자 t와 떨어진 최소거리를 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	teachermode e
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">1 0 1 2 1 0 1 2 2 1 0</h5>

<br>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
   public static ArrayList<Integer> solution(String str, char c){
       ArrayList<Integer> list = new ArrayList<Integer>();
        //lf: 현재 문자로부터 왼쪽으로 확인하는 변수
        //rt: 현재 문자로부터 오른쪽으로 확인하는 변수
       int lf ,rt;
       for(int i =0;i<str.length();i++ ){
           lf=i;
           rt=i;
           int count =0;
           //lf와 rt가 모두 문자열 인덱스를 벗어나지 않을 때까지 반복한다.
           while(lf>=0 || rt<str.length()){
               //lf가 찾는 문자열이면 list에 count를 추가해준 후 반복문을 빠져나간다.
               if(lf>=0&&str.charAt(lf)==c){
                   list.add(count);
                   break;
                //rf가 찾는 문자열이면 list에 count를 추가해준 후 반복문을 빠져나간다.
               }else if(rt<str.length()&&str.charAt(rt)==c){
                   list.add(count);
                   break;
                //발견되지 않았을 때 count를 증가시키고, lf는 -1, rf는 +1 해준다.
               }else{
                   count++;
                   lf--;
                   rt++;
               }
           }
       }
       return list;
   }
   public static void main(String[] args){
       Scanner sc = new Scanner(System.in);
       String str = sc.next();
       char c = sc.next().charAt(0);

       for(int x:solution(str,c)){
           System.out.print(x+" ");
       }

   }
}
```

<br>
## 다른 사람 풀이

```java
public class Main {
    public static int[] solution(String str, char c){
        int[] answer  =new int[str.length()];
        int p =1000;

       for(int i = 0;i<str.length();i++){
           if(str.charAt(i)==c){
               p=0;
               answer[i]=p;
           }else{
               p++;
               answer[i]=p;
           }
       }
       p = 1000;
       for(int i = str.length()-1;i>=0;i--){
           if(str.charAt(i)==c){
               p=0;
           }else {
               if(p<answer[i]){
                   p++;
                   //더 적은 거리를 answer에 저장
                   answer[i] = Math.min(answer[i],p);

               }
           }
       }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        char c = sc.next().charAt(0);

        for(int x:solution(str,c)){
            System.out.print(x+" ");
        }

    }
}
```

- 이 풀이는 p=1000으로 해놓고 **왼쪽**에서부터 주어진 문자와의 거리를 구한다.
- 이 후, **오른쪽**에서부터 주어진 문자와의 거리를 구해, **더 적은 거리**가 나왔을 때 값을 **교체**해준다.
