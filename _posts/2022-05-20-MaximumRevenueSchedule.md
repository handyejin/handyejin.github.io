---
layout: post
title: 최대 수입 스케쥴
date: 2022-05-20 06:10:00
category: Algorithm
---

# [Greedy Algorithm] 최대 수입 스케쥴(PriorityQueue 응용 문제)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수는 유명한 강연자이다. N개이 기업에서 강연 요청을 해왔다. 각 기업은 D일 안에 와서 강연을 해 주면 M만큼의 강연료를 주기로 했다.

각 기업이 요청한 D와 M를 바탕으로 가장 많을 돈을 벌 수 있도록 강연 스케쥴을 짜야 한다.

단 강연의 특성상 현수는 하루에 하나의 기업에서만 강연을 할 수 있다.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
6<br>
50 2<br>
20 1<br>
40 2<br>
60 3<br>
30 3<br>
30 1<br>

</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">150</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.PriorityQueue;
import java.util.Scanner;
class Info implements Comparable<Info>{
    int date;
    int money;


    public Info(int money, int date) {
        this.date = date;
        this.money = money;
    }

    //날짜가 높은 순으로 내림차순 정렬한다.
    @Override
    public int compareTo(Info o) {
        return o.date - this.date;
    }
}

public class Main {
    static int n;
    public int solution(ArrayList<Info> arr, int maxDate){
        int answer = 0;
        Collections.sort(arr);

        //금액이 큰 숫자부터 꺼내기 위해 reverseOrder()한다.
        PriorityQueue<Integer> pQ = new PriorityQueue<>(Collections.reverseOrder());

        int j =0;
        for(int i = maxDate;i>0;i--){
            for(; j<n;j++){
                // 현재 날짜가 i보다 작아지면 반복문을 빠져나온다.
                if(arr.get(j).date<i) break;

                //우선순위 큐에 금액을 추가한다.
                else{
                    pQ.add(arr.get(j).money);
                }
            }
            if(!pQ.isEmpty()){
                answer += pQ.poll();
            }

        }
        return answer;
    }
    public static void main(String[] args) {
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();

        //최대 날짜
        int maxDate = Integer.MIN_VALUE;
        ArrayList<Info> arr = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int money = sc.nextInt();
            int date = sc.nextInt();
            if (date > maxDate) {
                maxDate = date;
            }

            arr.add(new Info(money, date));

        }
        System.out.println(T.solution(arr, maxDate));
    }
}
```
