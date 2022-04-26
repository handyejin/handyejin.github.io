---
layout: post
title: 모든 아나그램 찾기
date: 2022-04-26 10:44:00
category: Algorithm
---

# [HashMap, TreeSet] 모든 아나그램 찾기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

S문자열에서 T문자열과 아나그램이 되는 S의 부분문자열의 개수를 구하는 프로그램을 작성하세요.

아나그램 판별시 대소문자가 구분됩니다. 부분문자열은 연속된 문자열이어야 합니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
bacaAacba<br>
abc

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">3</h5>

## 풀이

```java
import java.util.HashMap;
import java.util.Scanner;

public class Main {
    public static int solution(String s, String t){
        int answer=0;
        int length = t.length();
        int lt=0;

        //아나그램 판별 변수
        boolean check=true;
        //s를 저장할 HashMap 객체
        HashMap<Character,Integer> map = new HashMap<>();
        //t를 저장할 HashMap 객체
        HashMap<Character,Integer> find= new HashMap<>();

        //첫번째 구간부터 t의 길이만큼 map에 저장한다.
        for(int i=0;i<length;i++){
            map.put(s.charAt(i),map.getOrDefault(s.charAt(i),0)+1);
            find.put(t.charAt(i),find.getOrDefault(t.charAt(i),0)+1);
        }
        //첫번째 구간이 아나그램인지 확인해준다.
        for(char x: map.keySet()){
            if(map.get(x)!=find.getOrDefault(x,-1)){
                check=false;
                break;
            }
        }
        //첫번째 구간이 아나그램이라면 answer값을 증가시킨다.
        if(check==true){
            answer++;
        }else{
            check=true;
        }
        //다음 구간부터 탐색한다.
        for(int i=length;i<s.length();i++){
            //첫번째 요소 값을 지운다. 이 때, 값이 1이라면 key를 Map에서 제거한다.
            if(map.get(s.charAt(lt))==1){
                map.remove(s.charAt(lt));
            }else{
                map.put(s.charAt(lt),map.get(s.charAt(lt))-1);
            }
            //다음 요소를 맵에 추가해준다.
            map.put(s.charAt(i),map.getOrDefault(s.charAt(i),0)+1);
            //아나그램인지 확인한다.
            for(char x:map.keySet()){
                if(map.get(x)!=find.getOrDefault(x,-1)){
                    check=false;
                    break;
                }
            }
            //아나그램이 맞다면 answer을 증가시킨다.
            if(check==true){
                answer++;
            }else{
                check=true;
            }
            lt++;
        }
        return answer;

    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        String t = sc.next();

        System.out.println(solution(s,t));
    }
}
```

- **HashMap**과 **Sliding Window기법**을 사용한 풀이이다.
