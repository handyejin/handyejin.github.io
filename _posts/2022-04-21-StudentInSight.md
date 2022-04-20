---
layout: post
title: 보이는 학생
date: 2022-04-21
category: Algorithm
---

# [Array] 보이는 학생

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

선생님이 N명의 학생을 일렬로 세웠습니다. 일렬로 서 있는 학생의 키가 앞에서부터 순서대로 주어질 때, 맨 앞에 서 있는

선생님이 볼 수 있는 학생의 수를 구하는 프로그램을 작성하세요. (앞에 서 있는 사람들보다 크면 보이고, 작거나 같으면 보이지 않습니다.)
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
8<br>
130 135 148 140 145 150 150 153

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">7 5</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static int solution(int n, int[]arr){
        //answer: 선생님이 볼 수 있는 학생 수
        int answer=0;
        //maxHeight: 가장 키가 큰 학생을 저장하는 변수
        int maxHeight=-1;
        for(int i=0;i<n;i++){
            //현재 학생이 maxHeight보다 클 경우에 maxHeight에 학생의 키를 저장해주고, answer변수 증가
            if(maxHeight<arr[i]){
                maxHeight = arr[i];
                answer++;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc  = new Scanner(System.in);

        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i = 0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println((solution(n,arr)));
    }
}
```
