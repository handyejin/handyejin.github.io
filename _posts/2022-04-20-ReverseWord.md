---
layout: post
title: 단어 뒤집기
date: 2022-04-20
category: Algorithm
---

# [String]단어 뒤집기

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

N개의 단어가 주어지면 각 단어를 뒤집어 출력하는 프로그램을 작성하세요.
<br>

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;">
	3<br>
good<br>
Time<br>
Big<br>
</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px;">
doog<br>
emiT<br>
giB<br>
</h5>

## 풀이

```java
import java.util.Scanner;

public class Main {
    public static String[] solution(String[] array,int n){
        String[] result = new String[n];
        String tmp="";
        for(int i = 0;i<n;i++){

            //문자열을 뒤에서 부터 인덱싱해서 tmp문자열에 추가해준다.
            for(int j =array[i].length()-1;j>=0;j--){
                tmp+=array[i].charAt(j);

            }
            result[i] = tmp;
            tmp="";
        }
        return result;

    }
    public static void main(String[] args){
        Main main = new Main();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        String [] array = new String[n];

        for(int i = 0;i<n;i++){
            array[i] = sc.next();
        }

        String[] result = solution(array,n);
        for(String x : result){
            System.out.println(x);
        }


    }
}
```

### SpringBuilder을 이용한 풀이

```java
public class Answer {
    public ArrayList<String> solution(String[] array, int n){
        ArrayList<String> answer = new ArrayList<>();

        for(String x: array){

            //SrringBuilder객체를 만들어 문자열을 뒤집어줌
            String tmp = new StringBuilder(x).reverse().toString();
            //answer 리스트에 추가
            answer.add(tmp);
        }
        return answer;

    }
    public static void main(String[] args){
        Answer a = new Answer();
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        String [] array = new String[n];

        for(int i = 0;i<n;i++){
            array[i] = sc.next();
        }

        for(String x: a.solution(array,n)){
            System.out.println(x);

        }

    }
}
```

- **String은 불변(Immutable)**한 객체여서 연산 처리를 하게 될 때 새로운 객체를 만들어야한다. <br>
  **StringBuilder**을 사용하면 새로운 객체를 생성하지 않고 연산처리를 할 수 있기 때문에 속도도 빠르고 상대적으로 부하가 적다.
