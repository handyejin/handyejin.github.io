---
layout: post
title: 임시반장 정하기
date: 2022-04-21
category: Algorithm
---

# [Array] 임시반장 정하기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

김갑동 선생님은 올해 6학년 1반 담임을 맡게 되었다.

김갑동 선생님은 우선 임시로 반장을 정하고 학생들이 서로 친숙해진 후에 정식으로 선거를 통해 반장을 선출하려고 한다.

그는 자기반 학생 중에서 1학년부터 5학년까지 지내오면서 한번이라도 같은 반이었던 사람이 가장 많은 학생을 임시 반장으로 정하려 한다.

그래서 김갑동 선생님은 각 학생들이 1학년부터 5학년까지 몇 반에 속했었는지를 나타내는 표를 만들었다.

예를 들어 학생 수가 5명일 때의 표를 살펴보자.

<img src ="https://cote.inflearn.com/public/upload/f8a83920ca.jpg"/>

위 경우에 4번 학생을 보면 3번 학생과 2학년 때 같은 반이었고, 3번 학생 및 5번 학생과 3학년 때 같은 반이었으며,

2번 학생과는 4학년 때 같은 반이었음을 알 수 있다. 그러므로 이 학급에서 4번 학생과 한번이라도

같은 반이었던 사람은 2번 학생, 3번 학생과 5번 학생으로 모두 3명이다.

이 예에서 4번 학생이 전체 학생 중에서 같은 반이었던 학생 수가 제일 많으므로 임시 반장이 된다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
2 3 1 7 3<br>
4 1 9 6 8<br>
5 5 2 4 4<br>
6 5 2 6 7<br>
8 4 2 2 2<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">4</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public static int solution(int n, int[][] arr){
        int answer=0;
        int sameCount ;
        int maxSameCount = Integer.MIN_VALUE;
        ArrayList<Integer> sameClass = new ArrayList<Integer>();

        //현재 학생
        for(int i =0;i<n;i++){
            sameCount = 0;
            //학년
            for(int j = 0;j<5;j++){
                //학생번호
                for(int k = 0;k<n;k++){
                    //k번째 학생이 한번 같은 반이었음을 세었으므로,
                    //sameClass에 추가해 다음 차례에 같은 번이 되어도 횟수(sameCount)를 증가하지 않게 해준다.
                    if(sameClass.contains(k)==false&&k!=i&&arr[i][j]==arr[k][j]){
                        sameClass.add(k);
                        sameCount++;
                    }
                }
            }
            sameClass.clear();
            if(sameCount>maxSameCount){
                maxSameCount = sameCount;
                answer = i+1;

            }
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[][] arr = new int[n][5];

        for(int i=0;i<n;i++){
            for(int j = 0;j<5;j++){
                arr[i][j] = sc.nextInt();
            }
        }

        System.out.println(solution(n,arr));
    }
}
```

- 이 문제는 학년이 1학년부터 5학년 까지라는 것을 못보고, 배열을 NxN으로 선언해서 계속 오답이 나왔다,,
  <br> 문제를 잘 읽는것도 중요하다는 것을 깨닫게 해준 문제였다.
