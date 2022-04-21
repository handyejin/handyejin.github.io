---
layout: post
title: 멘토링
date: 2022-04-21
category: Algorithm
---

# [Array] 멘토링

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수네 반 선생님은 반 학생들의 수학점수를 향상시키기 위해 멘토링 시스템을 만들려고 합니다.

멘토링은 멘토(도와주는 학생)와 멘티(도움을 받는 학생)가 한 짝이 되어 멘토가 멘티의 수학공부를 도와주는 것입니다.

선생님은 M번의 수학테스트 등수를 가지고 멘토와 멘티를 정합니다.

만약 A학생이 멘토이고, B학생이 멘티가 되는 짝이 되었다면, A학생은 M번의 수학테스트에서 모두 B학생보다 등수가 앞서야 합니다.

M번의 수학성적이 주어지면 멘토와 멘티가 되는 짝을 만들 수 있는 경우가 총 몇 가지 인지 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
4 3<br>
3 4 1 2<br>
4 3 2 1<br>
3 1 4 2<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

#### 힌트

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">(3, 1), (3, 2), (4, 2)와 같이 3가지 경우의 (멘토, 멘티) 짝을 만들 수 있다.</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public static int solution(int[][] arr, int n, int m){
        int answer=0;
        int pi=0,pj = 0;
        int count =0;

        //멘토
        for(int i = 1;i<=n;i++){
            //멘티
            for(int j = 1;j<=n;j++){
                if(i==j)continue;
                //test의 수
                for(int k=0;k<m;k++){
                    //등수
                    for(int s = 0;s<n;s++){

                        //해당 위치에 본인 번호가 있으면 등수는 s등이다.
                        if(arr[k][s]==i){
                            pi = s;
                        }
                        if(arr[k][s]==j){
                            pj = s;
                        }
                    }
                    //pi등수가 pj보다 낮으면 count를 증가시킨다.
                    if(pi<pj){
                        count++;
                    }
                }
                //count가 총 문제 갯수가 되면 모든 테스트에서 pi가 pj의 등수보다 높다는 뜻이므로 answer을 증가시킨다.
                if(count==m){
                    answer++;
                }
                count = 0;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        //학생 수
        int n = sc.nextInt();
        //테스트 수
        int m = sc.nextInt();
        int[][] arr= new int[m][n];

        for(int i = 0;i<m;i++){
            for(int j = 0;j<n;j++){
                arr[i][j] = sc.nextInt();
            }
        }

        System.out.println(solution(arr,n,m));

    }
}

```

- 너무 어려웠던 문제이다.. 이 문제는 입력받은 대로 학생 번호의 등수가 메겨졌다고 잘못 이해해서 많이 헤멨다..
  <br> 문제를 잘 읽어야겠다고 또 한번 느꼈다.
