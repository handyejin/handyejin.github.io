---
layout: post
title: 특정 문자 뒤집기
date: 2022-04-20
category: Algorithm
---

# [String]특정 문자 뒤집기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

영어 알파벳과 특수문자로 구성된 문자열이 주어지면 영어 알파벳만 뒤집고,

특수문자는 자기 자리에 그대로 있는 문자열을 만들어 출력하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	a#b!GE*T@S
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">S#T!EG*b@a</h5>

<br>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public String solution(String str){
        String answer="";

        //문자 배열 생성
       char[] s = str.toCharArray();
        int len = str.length();

        //lt는 문자열의 처음부터 접근하는 변수
        int lt = 0;

        //rt는 문자열의 마지막부터 접근하는 변수
        int rt = len-1;

        while(lt<rt){
            //lt번째 문자가 알파벳이 아니라면 lt를 1증가시킨다.
            if(Character.isAlphabetic(s[lt])==false) {
                lt++;
            //rt번째 문자가 알파벳이 아니라면 rt를 1감소시킨다.
            }else if(Character.isAlphabetic(s[rt])==false){
                rt--;
            //lt,rt모두 알파벳이면 두 문자를 바꾸어준다.
            }else{
                char tmp = s[lt];
                s[lt] = s[rt];
                s[rt] = tmp;
                lt++;
                rt--;
            }
        }
        //valudof가 String 형으로 변환해줌
        answer = String.valueOf(s);

        return answer;
    }

    public static void main(String[] args){
        Main main =new Main();
        Scanner sc = new Scanner(System.in);
        String str= sc.next();

        System.out.println(main.solution(str));

    }
}
```
