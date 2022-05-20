---
layout: post
title: 씨름선수
date: 2022-05-19 12:00:00
category: Algorithm
---

# [Greedy Algorithm] 씨름선수

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수는 씨름 감독입니다. 현수는 씨름 선수를 선발공고를 냈고, N명의 지원자가 지원을 했습니다.

현수는 각 지원자의 키와 몸무게 정보를 알고 있습니다.

현수는 씨름 선수 선발 원칙을 다음과 같이 정했습니다.

“A라는 지원자를 다른 모든 지원자와 일대일 비교해서 키와 몸무게 모두 A지원자 보다 높은(크고, 무겁다) 지원자가

존재하면 A지원자는 탈락하고, 그렇지 않으면 선발된다.”

N명의 지원자가 주어지면 위의 선발원칙으로 최대 몇 명의 선수를 선발할 수 있는지 알아내는 프로그램을 작성하세요.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
172 67<br>
183 65<br>
180 70<br>
170 72<br>
181 60<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

class Person implements Comparable<Person>{
    public int height, weight;

    Person(int height, int weight){
        this.height = height;
        this.weight = weight;
    }
    //키를 기준으로 내림차순 정렬 한다.
    @Override
    public int compareTo(Person o) {
        return o.height - this.height;
    }
}
public class Main {
    public int solution(int n, ArrayList<Person> arr){
        int count = 0;
        Collections.sort(arr);
        int maxWeight = Integer.MIN_VALUE;

        //키를 기준으로 내림차순 정렬 했을 때
        //다음 사람의 몸무게가 더 크다면 선발되고
        //몸무게가 더 적다면 탈락이다.
        for(Person p: arr ){
            if(p.weight>maxWeight){
                count++;
                maxWeight = p.weight;
            }
        }
        return count;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        ArrayList<Person> arr = new ArrayList<>();
        for(int i = 0;i<n;i++){
            int height = sc.nextInt();
            int weight = sc.nextInt();
            arr.add(new Person(height, weight));

        }
        System.out.println(T.solution(n, arr));

    }
}
```
