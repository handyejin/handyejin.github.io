---
layout: post
title: 뮤직비디오(결정 알고리즘)
date: 2022-05-04 11:40:00
category: Algorithm
---

# [Sorting and Searching(정렬, 이분검색과 결정 알고리즘)] 뮤직비디오

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

지니레코드에서는 불세출의 가수 조영필의 라이브 동영상을 DVD로 만들어 판매하려 한다.

DVD에는 총 N개의 곡이 들어가는데, DVD에 녹화할 때에는 라이브에서의 순서가 그대로 유지되어야 한다.

순서가 바뀌는 것을 우리의 가수 조영필씨가 매우 싫어한다. 즉, 1번 노래와 5번 노래를 같은 DVD에 녹화하기 위해서는

1번과 5번 사이의 모든 노래도 같은 DVD에 녹화해야 한다. 또한 한 노래를 쪼개서 두 개의 DVD에 녹화하면 안된다.

지니레코드 입장에서는 이 DVD가 팔릴 것인지 확신할 수 없기 때문에 이 사업에 낭비되는 DVD를 가급적 줄이려고 한다.

고민 끝에 지니레코드는 M개의 DVD에 모든 동영상을 녹화하기로 하였다. 이 때 DVD의 크기(녹화 가능한 길이)를 최소로 하려고 한다.

그리고 M개의 DVD는 모두 같은 크기여야 제조원가가 적게 들기 때문에 꼭 같은 크기로 해야 한다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
9 3<br>
1 2 3 4 5 6 7 8 9

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">17</h5>

<div style="height:20px;"></div>

## 풀이

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
    //capacity 용량으로 dvd가 몇장 만들어지는지 확인한다.
    public int count(int [] arr, int capacity){
        int cnt = 1;
        int sum = 0;
        for(int x: arr){
            if(sum+x<=capacity){
                sum+=x;
            }else{
                cnt++;
                sum=x;

            }
        }
        return cnt;
    }

    public  int solution(int n, int m, int[] arr){
        int answer = 0;
        //lt 는 배열의 최댓값
        int lt = Arrays.stream(arr).max().getAsInt();
        int rt = Arrays.stream(arr).sum();

        //이진탐색으로 최소 용량 크기를 찾는다.
        while(lt<=rt){
            int mid = (lt+rt)/2;

            if(count(arr, mid)<=m){
                answer = mid;
                rt = mid-1;
            }else {
                lt = mid+1;
            }
        }
        return answer;

    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int[] arr = new int[n];

        for(int i=0;i<n;i++){
            arr[i] = sc.nextInt();
        }
        System.out.println(T.solution(n,m,arr));
    }
}
```

- **결정 알고리즘(이진탐색)**을 이용해서 최소 용량을 탐색한다.
