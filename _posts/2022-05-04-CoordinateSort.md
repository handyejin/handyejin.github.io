---
layout: post
title: 좌표 정렬
date: 2022-05-04 11:30:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 좌표 정렬

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 평면상의 좌표(x, y)가 주어지면 모든 좌표를 오름차순으로 정렬하는 프로그램을 작성하세요.

정렬기준은 먼저 x값의 의해서 정렬하고, x값이 같을 경우 y값에 의해 정렬합니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
2 7<br>
1 3<br>
1 2<br>
2 5<br>
3 6<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">
1 2<br>
1 3<br>
2 5<br>
2 7<br>
3 6<br>
</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

//포인트 클래스의 객체를 정렬한다.
class Point implements Comparable<Point>{
    public int x,y;

    //생성자 초기화
    Point(int x, int y){
        this.x = x;
        this.y = y;
    }

    @Override
    public int compareTo(Point o) {
        //x좌표가 같으면 y비교
        //this는 앞에 object는 뒤
        //크다(양수) 같다(0) 작다(음수) 리턴

        //밑에 코드는 오름차순
        if(this.x==o.x) return this.y-o.y;
        else return this.x-o.x;

    }
}

public class Main {

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        ArrayList<Point> arr = new ArrayList<>();
        for(int i=0;i<n;i++){
            int x = sc.nextInt();
            int y = sc.nextInt();
            //포인트 객체 생성해서 arr 리스트에 추가
            arr.add(new Point(x,y));
        }
        //compareTo 정렬 기준에 따라서 정렬해준다.
        Collections.sort(arr);
        for(Point o: arr) System.out.println(o.x+" "+ o.y);
    }
}
```

-
