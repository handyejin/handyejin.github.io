---
layout: post
title: 학급 회장(해쉬)
date: 2022-04-26 06:52:00
category: Algorithm
---

# [HashMap, TreeSet] 학급 회장

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

학급 회장을 뽑는데 후보로 기호 A, B, C, D, E 후보가 등록을 했습니다.

투표용지에는 반 학생들이 자기가 선택한 후보의 기호(알파벳)가 쓰여져 있으며 선생님은 그 기호를 발표하고 있습니다.

선생님의 발표가 끝난 후 어떤 기호의 후보가 학급 회장이 되었는지 출력하는 프로그램을 작성하세요.

반드시 한 명의 학급회장이 선출되도록 투표결과가 나왔다고 가정합니다.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
15<br>
BACBACCACCBDEDE

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">C</h5>

## 풀이

```java
import java.util.HashMap;
import java.util.Scanner;

public class Main {
   public static char solution(int n, String s){
       char answer;
       int[] count = new int[5];
       int maxCount=Integer.MIN_VALUE;
       int maxIdx =-1;
       for(int i = 0;i<n;i++){
             if(s.charAt(i)=='A'){
               count[0]++;
           }else if(s.charAt(i)=='B'){
               count[1]++;
           }else if(s.charAt(i)=='C'){
               count[2]++;
           }else if(s.charAt(i)=='D'){
               count[3]++;
           }else{
               count[4]++;
           }
       }
       for(int i = 0;i<5;i++){
           if(maxCount<count[i]){
               maxCount = count[i];
               maxIdx = i;
           }
       }
       answer = (char) (maxIdx+'A');

       return answer;
   }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String s = sc.next();
        System.out.println(solution(n,s));
    }
}
```

- 처음에 풀었던 풀이는 HashMap을 만들지 않고 반 학생 수 만큼의 배열을 만들어 값에 따라 count를 증가시켜주는 방식으로 풀었다.
- 강의를 통해 **HashMap** 사용법을 알게 되었다.

## HashMap을 이용한 풀이

```java
import java.util.HashMap;
import java.util.Scanner;

public class Main {
public static char solution(int n, String s){
    char answer=' ';
    //키가 Character형이고, value가 Integer형인 Hash Map 객체 생성
    HashMap<Character,Integer> map = new HashMap<>();

    int max = Integer.MIN_VALUE;
    for(char x: s.toCharArray()){

        //getOrDefault(x,default): x의 키 값을 가져오고 없으면 default값을 가져옴
        map.put(x, map.getOrDefault(x,0)+1);
    }
    //map을 탐색할 때 keySet()을 사용하여 탐색
    for(char key: map.keySet()){
        if(max<map.get(key)){
            max = map.get(key);
            answer = key;
        }

    }
    return answer;
}
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String s = sc.next();
        System.out.println(solution(n,s));
    }
}
```

- HashMap 은 <span style="background-color:#f1f8ff; font-weight:700">Key-Value</span>를 저장하는 자료구조를 가지고 있다. HashMap은 해싱을 사용하기 때문에 많은 양의 데이터를 검색하는데 뛰어난 성능을 가지고 있다.
  - **HashMap 값 추가**: map.put(key,value)
  - **HashMap 값 삭제**: map.remove(key)
  - **HashMap 값 출력**: map.get(key)
- 값 출력 시 **map.getOrDefault(key,defualt)**를 사용하면 **_디폴트 값을 지정_**해줄 수 있다.
