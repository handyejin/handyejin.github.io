---
layout: post
title: 문자열 압축
date: 2022-04-20
category: Algorithm
---

# [String]문자열 압축

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

알파벳 대문자로 이루어진 문자열을 입력받아 같은 문자가 연속으로 반복되는 경우 반복되는

문자 바로 오른쪽에 반복 횟수를 표기하는 방법으로 문자열을 압축하는 프로그램을 작성하시오.

#### 예시 입력1

<h5 style = "margin-top:3px; margin-left:2px;">
	KKHSSSSSSSE
</h5>

#### 예시 출력1

<h5 style = "margin-top:3px; margin-left:2px;">K2HS7E</h5>

<br>
#### 예시 입력2

<h5 style = "margin-top:3px; margin-left:2px;">
	KSTTTSEEKFKKKDJJGG
</h5>

#### 예시 출력2

<h5 style = "margin-top:3px; margin-left:2px;">KST3SE2KFK3DJ2G2</h5>

<br>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static String solution(String str){
        String answer ="";
        int count=1;

        for(int i=0;i<str.length();){
            //i번째 문자와 이후 같은 문자가 나올때까지 count를 증가시킨다.
            for(int j = i+1;j<str.length();j++){
                if(str.charAt(i)==str.charAt(j)){
                    count++;
                }else{
                    break;
                }
            }
            answer += str.charAt(i);
            //count가 1개 이상일 때만 answer에 개수를 추가해준다.
            if(count>1){
                answer+=String.valueOf(count);
            }
            i+=count;
            count = 1;

        }

        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String str = sc.next();

        System.out.println(solution(str));
    }
}
```
