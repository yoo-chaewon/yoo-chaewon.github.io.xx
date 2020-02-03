---

title:  "[ALGORITHM]BOJ1918-후위표기식"

excerpt: "#BOJ #Stack #FILO"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- STACK

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

수식은 일반적으로 3가지 표기법으로 표현할 수 있다. 연산자가 피연산자 가운데 위치하는 중위 표기법(일반적으로 우리가 쓰는 방법이다), 연산자가 피연산자 앞에 위치하는 전위 표기법(prefix notation), 연산자가 피연산자 뒤에 위치하는 후위 표기법(postfix notation)이 그것이다. 예를 들어 중위 표기법으로 표현된 a+b는 전위 표기법으로는 +ab이고, 후위 표기법으로는 ab+가 된다.

이 문제에서 우리가 다룰 표기법은 후위 표기법이다. 후위 표기법은 위에서 말한 법과 같이 연산자가 피연산자 뒤에 위치하는 방법이다. 이 방법의 장점은 다음과 같다. 우리가 흔히 쓰는 중위 표기식 같은 경우에는 덧셈과 곱셈의 우선순위에 차이가 있어 왼쪽부터 차례로 계산할 수 없지만 후위 표기식을 사용하면 순서를 적절히 조절하여 순서를 정해줄 수 있다. 또한 같은 방법으로 괄호 등도 필요 없게 된다. 예를 들어 a+b*c를 후위 표기식으로 바꾸면 abc*+가 된다.

중위 표기식을 후위 표기식으로 바꾸는 방법을 간단히 설명하면 이렇다. 우선 주어진 중위 표기식을 연산자의 우선순위에 따라 괄호로 묶어준다. 그런 다음에 괄호 안의 연산자를 괄호의 오른쪽으로 옮겨주면 된다.

예를 들어 a+b*c는 (a+(b*c))의 식과 같게 된다. 그 다음에 안에 있는 괄호의 연산자 *를 괄호 밖으로 꺼내게 되면 (a+bc*)가 된다. 마지막으로 또 +를 괄호의 오른쪽으로 고치면 abc*+가 되게 된다.

다른 예를 들어 그림으로 표현하면 A+B*C-D/E를 완전하게 괄호로 묶고 연산자를 이동시킬 장소를 표시하면 다음과 같이 된다.

![img](https://www.acmicpc.net/JudgeOnline/upload/201007/4.png)

이러한 사실을 알고 중위 표기식이 주어졌을 때 후위 표기식으로 고치는 프로그램을 작성하시오

### 입력

첫째 줄에 중위 표기식이 주어진다. 단 이 수식의 피연산자는 A~Z의 문자로 이루어지며 수식에서 한 번씩만 등장한다. 그리고 -A+B와 같이 -가 가장 앞에 오거나 AB와 같이 *가 생략되는 등의 수식은 주어지지 않는다. 표기식은 알파벳 대문자와 +, -, *, /, (, )로만 이루어져 있으며, 길이는 100을 넘지 않는다. 

### 출력

첫째 줄에 후위 표기식으로 바뀐 식을 출력하시오



### 예제 입력 1

```
A*(B+C)
```

### 예제 출력 1

```
ABC+*
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Stack;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String input = bufferedReader.readLine();
        Stack<Character> stack = new Stack<>();

        for (int i = 0; i < input.length(); i++) {
            char cur = input.charAt(i);
            if ('A' <= cur && cur <= 'Z') {
                System.out.print(cur);
            } else {
                if (cur == '(') {
                    stack.push(cur);
                } else if (cur == ')') {
                    while (stack.peek() != '(') {
                        System.out.print(stack.pop());
                    }
                    stack.pop();
                } else {//operator
                    while (!stack.isEmpty() && priorityOP(cur) <= priorityOP(stack.peek())) {
                        System.out.print(stack.pop());
                    }
                    stack.push(cur);
                }
            }
        }

        while (!stack.isEmpty()) {
            System.out.print(stack.pop());
        }
    }

    public static int priorityOP(Character op) {
        if (op == '(' || op == ')') return 0;
        else if (op == '+' || op == '-') return 1;
        else return 2;
    }
}
```



###  👉**후위표기**👈식이란 ?

> - 연산자가 피연산자 뒤에 위치하는 후위 표기법이다.
>   - 이 것의 장점은 괄호가 필요없다는 점!
> - 중위표기식을 후위표기식으로 바꾸는 방법은
>   - 주어진 중요 표기식을 연산자의 우선순위에 따라 괄호로 묶어준다. 그런 다음 괄호 안의 연산자를 괄호의 오른쪽으로 옮겨주면 된다. 



### 중위표기식 👉 후위표기식

```
- 준비물 : Stack
- 입력이 피연산자이면 출력해준다.
- 입력이 연산자일 경우
	- ( : 스택에 넣어준다.
	- ) : ( 이 나올때까지 모든 연산자를 출력해준다.
	- + - / * : 스택에 넣어준다.
		이때, 연산자 우선순위가 중요하다.
		연산자 우선순위는 * / > + - 이다.
		즉, 연산자 우선 순위가 높으면 먼저 계산이 되어야 하는것이고, 
		즉, 이것은 우선 순위가 높을 수록 스택에 오래 머물면 안된다 !!! ‼️
```



##### 위의 순서로 코드를 짜주었다.

- 스택 연산

- ```java
  if ('A' <= cur && cur <= 'Z') {// 피연산자인 경우 출력
    System.out.print(cur);
  } else {
    if (cur == '(') { // ( 연산자는 그냥 push
      stack.push(cur);
    } else if (cur == ')') { // ) 이 나올때까지 모든 연산자 출력
      while (stack.peek() != '(') {
        System.out.print(stack.pop());
      }
      stack.pop();
    } else {// + - * / 인 경우, 
      //만약에 스택에 **/가 있고 현재 것이 + 이면 **/다 출력 되어야 함.
      while (!stack.isEmpty() && priorityOP(cur) <= priorityOP(stack.peek())) {
        System.out.print(stack.pop());
      }
      stack.push(cur);//현재꺼를 넣어준다.
    }
  }
  ```

- 우선순위

  ```java
  public static int priorityOP(Character op) {
    if (op == '(' || op == ')') return 0;
    else if (op == '+' || op == '-') return 1;
    else return 2;
  }
  ```

  

##### 느낀점

내가 생각하기에 **스택** 을 사용하는 문제중에 가장 기본적이고, 중요하면서도 난이도 있는 문제 같다. 그리고 자료 구조 수업을 들으면서 과제로 중위연산을 후위연산으로 바꾸는 것을 했었었다 ! 해결 방법은 어렵지 않지만 **후위표기식**을 매번 볼때마다 정확하게 기억에 안나기 때문이다. 이 문제는 꼭 기억해야겠다.~



> 문제 : https://www.acmicpc.net/problem/1918
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/ab8a525e0eb79ecfc298ab8feb5821e4112685d5/Algorithm/1.BOJ/Q_1918_후위표기식.java

