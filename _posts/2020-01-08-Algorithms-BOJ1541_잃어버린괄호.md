---

title:  "[ALGORITHM]BOJ1541-잃어버린괄호"

excerpt: "#BOJ #Algorithm #Greedy"



categories:

- ALGORITHM

tags:

- ALGORITHM

- Greedy

- BOJ

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

세준이는 양수와 +, -, 그리고 괄호를 가지고 길이가 최대 50인 식을 만들었다. 그리고 나서 세준이는 괄호를 모두 지웠다.

그리고 나서 세준이는 괄호를 적절히 쳐서 이 식의 값을 최소로 만들려고 한다.

괄호를 적절히 쳐서 이 식의 값을 최소로 만드는 프로그램을 작성하시오.

### 입력

첫째 줄에 식이 주어진다. 식은 ‘0’~‘9’, ‘+’, 그리고 ‘-’만으로 이루어져 있고, 가장 처음과 마지막 문자는 숫자이다. 그리고 연속해서 두 개 이상의 연산자가 나타나지 않고, 5자리보다 많이 연속되는 숫자는 없다. 수는 0으로 시작할 수 있다.

### 출력

첫째 줄에 정답을 출력한다.



### 예제 입력 1

```
55-50+40
```

### 예제 출력 1

```
-35
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] input = bufferedReader.readLine().split("-");
        int answer = innerCal(input[0]);
        for (int i = 1; i < input.length; i++){
            answer -= innerCal(input[i]);
        }
        System.out.println(answer);
    }

    public static int innerCal(String str){
        String[] input = str.split("\\+");
        int sum = 0;

        for (int i = 0; i < input.length; i++){
            sum += Integer.parseInt(input[i]);
        }
        return sum;
    }
}

```

- '최소값'을 만들어야 하기 때문에 최대한 - 가 많아야 한다.

   👉 그러기 위해서는 - 부터 다음 - 전까지 괄호를 묶으면 그 괄호 안에 값들은 -를 갖게 된다.

- 따라서 나는 입력을 받을 때 - 로 분리해서 받았다.

- 그리고 그 분리된 것들끼리 - 로 더해서 answer에 더해주었다.

  - 이때 중요한 것은 시작은 항상 + 이므로 첫번째 분리된 것은 - 를 붙여주면 안된다 !

    

## 느낀점

2달만에 다시 풀었던 문젠데 코드가 아주 거의 비슷하다. 웃겼던거는 

이번에 answer += innerCal(input[i]) * -1; 로 썼다는 것이다. 그래서 이전에 했던 코드를 보고

answer -= innerCal(input[i]); 고쳤다 키키 !

- 문제 : https://www.acmicpc.net/problem/1541
- 저장소1 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1541_잃어버린괄호.java
- 저장소2 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1541_잃어버린괄호_ver2.java