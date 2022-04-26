---
layout: post
title: K번째 큰 수
date: 2022-04-26 10:54:00
category: Algorithm
---

# [HashMap, TreeSet] K번째 큰 수

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

현수는 1부터 100사이의 자연수가 적힌 N장의 카드를 가지고 있습니다. 같은 숫자의 카드가 여러장 있을 수 있습니다.

현수는 이 중 3장을 뽑아 각 카드에 적힌 수를 합한 값을 기록하려고 합니다. 3장을 뽑을 수 있는 모든 경우를 기록합니다.

기록한 값 중 K번째로 큰 수를 출력하는 프로그램을 작성하세요.

만약 큰 수부터 만들어진 수가 25 25 23 23 22 20 19......이고 K값이 3이라면 K번째 큰 값은 22입니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
10 3<br>
13 15 34 23 45 65 33 11 26 42

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">143</h5>

<br>

## 풀이

```java
import javax.xml.transform.Templates;
import java.util.*;

public class Main {
    public static int solution(int n, int k, int[] arr){
        int answer =-1;

        //값을 추가할 때, 내림차순으로 자동정렬된다.
        TreeSet<Integer> Tset = new TreeSet<>(Collections.reverseOrder());

        //3중 반복문을 통해서 3장을 뽑는 모든 경우의 수를 더해준다.
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                for(int z=j+1;z<n;z++){
                    //중복을 제거해서 TreeSet객체에 추가해준다.
                    Tset.add(arr[i]+arr[j]+arr[z]);
                }
            }
        }
        int cnt=0;
        //Tset을 탐색하면서 k번째 때, 값을 리턴해준다.
        for(int x: Tset){
            System.out.println(x);
            cnt++;
            if(cnt==k){
                return x;
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        int[] arr = new int[n];

        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }

        System.out.println(solution(n,k,arr));
    }
}
```

- <span style="background-color:#f1f8ff; font-weight:700">TreeSet</span>이란 **이진 탐색 트리**라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스이다. 따라서 추가와 삭제시에는 시간이 더 걸리지만 **정렬, 검색에는 높은 성능**을 가지고 있다. TreeSet은 저장 시 **중복을 제거**해주고, 순서가 유지되지 않는다.
  <div style="height:20px;"></div>
- **TreeSet 사용법**
  - TreeSet 값 추가: set.add(value)
  - TreeSet 값 삭제: set.remove(value);
  - TreeSet 크기 구하기: set.size();
  - set.first(): 오름차순일 때 최솟값
  - set.last(): 오름차순일 때 최댓값
