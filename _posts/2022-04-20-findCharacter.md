---
layout: post
title: 문자 찾기
date: 2022-04-20
category: Algorithm
---

# [String]문자 찾기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

한 개의 문자열을 입력받고, 특정 문자를 입력받아 해당 특정문자가 입력받은 문자열에 몇 개 존재하는지 알아내는 프로그램을 작성하세요.

## 풀이

```java

import java.util.Scanner;

public class Main {
    public int solution(String s, char find){
        int count = 0;
        s = s.toLowerCase();
        find = Character.toLowerCase(find);

        //string을 문자 배열로 만들어주어야함
        for(char x: s.toCharArray()){
            if(x==find){
                count++;
            }
        }

        return count;
    }

    public static void main(String[] args){
        Main m = new Main();
        Scanner scanner = new Scanner(System.in);
        String s = scanner.next();
        char find = scanner.next().charAt(0);

        System.out.print(m.solution(s,find));

    }
}
```

### 다른 사람 풀이

```java
import java.util.*;
class Main{
	public int solution(String str, char t){
		int answer=0;
		str=str.toUpperCase();
		t=Character.toUpperCase(t);
		//System.out.println(str+" "+t);
		/*for(int i=0; i<str.length(); i++){
			if(str.charAt(i)==t) answer++;
		}*/
		for(char x : str.toCharArray()){
			if(x==t) answer++;
		}
		return answer;
	}

	public static void main(String[] args){
		Main T = new Main();
		Scanner kb = new Scanner(System.in);
		String str=kb.next();
		char c=kb.next().charAt(0);
		System.out.print(T.solution(str, c));
	}
}

```
