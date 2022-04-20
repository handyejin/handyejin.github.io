---
layout: post
title: 숫자만 추출
date: 2022-04-20
category: Algorithm
---

# [String]숫자만 추출

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

문자와 숫자가 섞여있는 문자열이 주어지면 그 중 숫자만 추출하여 그 순서대로 자연수를 만듭니다.

만약 “tge0a1h205er”에서 숫자만 추출하면 0, 1, 2, 0, 5이고 이것을 자연수를 만들면 1205이 됩니다.

추출하여 만들어지는 자연수는 100,000,000을 넘지 않습니다.
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	g0en2T0s8eSoft
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">208</h5>

## 풀이

```java
import java.util.Scanner;

//숫자 아스키코드 48~57
public class Main {
    public static int solution(String str){
        int answer;
        String number="";
        for(char x:str.toCharArray()){
            //문자가 숫자인지 확인해준다.
            if(Character.isDigit(x)){
                number+=x;
            }
        }
        //문자열을 정수로 변환한다.
        answer = Integer.parseInt(number);

        return answer;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        String str = sc.next();

        System.out.println(solution(str));
    }
}
```

- **String** 자료형 -> **int** 자료형: **Integer.ParseInt(str)**
- **int** 자료형 -> **String** 자료형: **String.valueOf(num)**
