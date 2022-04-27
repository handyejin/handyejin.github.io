---
layout: post
title: 크레인 인형뽑기(카카오)
date: 2022-04-27 09:36:00
category: Algorithm
---

# [Stack, Queue] 크레인 인형뽑기(카카오)

> [출처](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-%EC%BD%94%ED%85%8C%EB%8C%80%EB%B9%84/)

## 문제

게임개발자인 죠르디는 크레인 인형뽑기 기계를 모바일 게임으로 만들려고 합니다.

죠르디는 게임의 재미를 높이기 위해 화면 구성과 규칙을 다음과 같이 게임 로직에 반영하려고 합니다.

<img src="https://cote.inflearn.com/public/upload/d637c6aff5.jpg"/>

게임 화면은 1 x 1 크기의 칸들로 이루어진 N x N 크기의 정사각 격자이며 위쪽에는 크레인이 있고 오른쪽에는 바구니가 있습니다.

(위 그림은 5 x 5 크기의 예시입니다). 각 격자 칸에는 다양한 인형이 들어 있으며 인형이 없는 칸은 빈칸입니다.

모든 인형은 1 x 1 크기의 격자 한 칸을 차지하며 격자의 가장 아래 칸부터 차곡차곡 쌓여 있습니다.

게임 사용자는 크레인을 좌우로 움직여서 멈춘 위치에서 가장 위에 있는 인형을 집어 올릴 수 있습니다. 집어 올린 인형은 바구니에 쌓이게 되는 데,

이때 바구니의 가장 아래 칸부터 인형이 순서대로 쌓이게 됩니다.

다음 그림은 [1번, 5번, 3번] 위치에서 순서대로 인형을 집어 올려 바구니에 담은 모습입니다.

<img src="https://cote.inflearn.com/public/upload/e7f1732dc7.jpg"/>

만약 같은 모양의 인형 두 개가 바구니에 연속해서 쌓이게 되면 두 인형은 터뜨려지면서 바구니에서 사라지게 됩니다.

위 상태에서 이어서 [5번] 위치에서 인형을 집어 바구니에 쌓으면 같은 모양 인형 두 개가 없어집니다.

<img src="https://cote.inflearn.com/public/upload/437bd16905.jpg"/>

크레인 작동 시 인형이 집어지지 않는 경우는 없으나 만약 인형이 없는 곳에서 크레인을 작동시키는 경우에는 아무런 일도 일어나지 않습니다.

또한 바구니는 모든 인형이 들어갈 수 있을 만큼 충분히 크다고 가정합니다. (그림에서는 화면표시 제약으로 5칸만으로 표현하였음)

게임 화면의 격자의 상태가 담긴 2차원 배열 board와 인형을 집기 위해 크레인을 작동시킨 위치가 담긴 배열 moves가 매개변수로 주어질 때,

크레인을 모두 작동시킨 후 터트려져 사라진 인형의 개수를 구하는 프로그램을 작성하세요.

#### 예시 입력

<h5 style = "margin-top:3px; margin-left:2px;font-weight:550">
5<br>
0 0 0 0 0<br>
0 0 1 0 3<br>
0 2 5 0 1<br>
4 2 4 4 2<br>
3 5 1 3 1<br>
8<br>
1 5 3 5 1 2 1 4<br>

</h5>

#### 예시 출력

<h5 style = "margin-top:3px; margin-left:2px; font-weight:550">4</h5>

<br>

## 풀이

```java
import java.util.Scanner;
import java.util.Stack;

public class Main {
    public int solution(int n, int m, int[][] board, int[] moves){
        int answer = 0;
        Stack<Integer> stack = new Stack<>();

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                //인형이 있으면 인형을 꺼내서 스택에 넣어준다.
                if(board[j][moves[i]-1]!=0){
                    //스택의 상단과 꺼낸 인형이 같으면 answer을 2만큼 증가시켜주고 스택의 상단을 제거한다.
                    if(!stack.isEmpty()&&stack.peek()==board[j][moves[i]-1]){
                        answer+=2;
                        stack.pop();
                    }else{
                        stack.push(board[j][moves[i]-1]);
                    }
                    board[j][moves[i]-1]=0;
                    //break를 하지 않으면 인형이 또 꺼내진다.
                    break;
                }
            }
        }
        return answer;
    }
    public static void main(String[] args){
        Main T = new Main();
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[][] board = new int[n][n];
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                board[i][j]=sc.nextInt();
            }
        }
        int m = sc.nextInt();
        int[] moves = new int[m];
        for(int i=0;i<m;i++){
            moves[i] = sc.nextInt();
        }
        System.out.println(T.solution(n,m,board,moves));
    }
}
```
