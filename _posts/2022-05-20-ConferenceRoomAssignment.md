---
layout: post
title: 회의실 배정
date: 2022-05-19 12:02:00
category: Algorithm
---

# [Greedy Algorithm] 회의실 배정

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

한 개의 회의실이 있는데 이를 사용하고자 하는 n개의 회의들에 대하여 회의실 사용표를 만들려고 한다.

각 회의에 대해 시작시간과 끝나는 시간이 주어져 있고, 각 회의가 겹치지 않게 하면서 회의실을 사용할 수 있는 최대수의 회의를 찾아라.

단, 회의는 한번 시작하면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
1 4<br>
2 3<br>
3 5<br>
4 6<br>
5 7<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;
class Time implements Comparable<Time>{
    public int start;
    public int end;

    Time(int start, int end){
        this.start = start;
        this.end = end;
    }

    //끝나는 시간을 기준으로 오름차순 정렬한다.
    //끝나는 시간이 같다면 시작시간을 기준으로 오름차순 정렬한다.
    @Override
    public int compareTo(Time o) {
        if(o.end==this.end){
            return this.start - o.start;
        }else{
            return this.end- o.end;
        }

    }
}

public class Main {
    public int solution(ArrayList<Time> arr, int n){
        int answer = 0;
        Collections.sort(arr);

        int endTime = Integer.MIN_VALUE;
        for(Time t: arr){
            if(t.start>=endTime){
                answer++;
                endTime =t.end;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Main T= new Main();
        Scanner sc = new Scanner(System.in);
        int n  = sc.nextInt();
        ArrayList<Time> arr = new ArrayList<>();

        for(int i = 0;i<n;i++){
            int start = sc.nextInt();;
            int end = sc.nextInt();
            arr.add(new Time(start,end));
        }
        System.out.println(T.solution(arr, n));
    }
}
```
