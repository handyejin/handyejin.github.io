---
layout: post
title: 두 배열 합치기
date: 2022-04-25
category: Algorithm
---

# [Two Pointers, Sliding window] 두 배열 합치기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

오름차순으로 정렬이 된 두 배열이 주어지면 두 배열을 오름차순으로 합쳐 출력하는 프로그램을 작성하세요.
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
3<br>
1 3 5<br>
5<br>
2 3 6 7 9<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">1 2 3 3 5 6 7 9</h5>

## 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public static ArrayList<Integer> solution(int[] arr1, int[] arr2, int n, int m){
        ArrayList<Integer> answer = new ArrayList<Integer>();
        //idx1은 arr1 탐색, idx2는 arr2 탐색
        int idx1=0,idx2=0;

        //idx1과 idx2가 하나라도 배열 범위를 초과할 시 반복문을 끝낸다.
        while (idx1<n||idx2<m){
            //현재 arr1의 요소가 더 작다면 answer리스트에 추가해주고 idx1을 하나 높여서 다음 요소를 비교한다.
            if(arr1[idx1]<arr2[idx2]){
                answer.add(arr1[idx1]);
                idx1++;
            //현재 arr2의 요소가 더 작다면 answer리스트에 추가해주고 idx2을 하나 높여서 다음 요소를 비교한다.
            }else if(arr1[idx1]>arr2[idx2]){
                answer.add(arr2[idx2]);
                idx2++;
            //같다면 두 요소 모두 추가해준 후, 인덱스를 둘다 높여준다.
            }else{
                answer.add(arr1[idx1]);
                answer.add(arr2[idx2]);
                idx1++;
                idx2++;
            }
            //반복문이 끝나고 남은 요소들을 리스트에 추가해준다.
            if(idx1>=n){
                for(int i = idx2;i<m;i++){
                    answer.add(arr2[i]);
                }
                break;
            }else if(idx2>=m){
                for(int i = idx1;i<n;i++){
                    answer.add(arr1[i]);
                }
                break;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr1 = new int[n];
        for(int i=0;i<n;i++){
            arr1[i] = sc.nextInt();
        }
        int m = sc.nextInt();
        int[] arr2 = new int[m];
        for(int i = 0;i<m;i++){
            arr2[i] = sc.nextInt();
        }

        for(int x: solution(arr1, arr2, n, m)){
            System.out.print(x+" ");
        }

    }
}
```

- 이 문제는 1차원 배열에서 두 개의 포인터(idx1,idx2)를 조적하여 원하는 결과를 얻는 **Two Pointers**알고리즘을 사용하였다.
  <br> 두개의 포인터를 사용하여서 기존의 방식보다 **시간을 개선** 할 수 있었다.
