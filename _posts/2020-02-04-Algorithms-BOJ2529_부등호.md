---

title:  "[ALGORITHM]BOJ2529-부등호"

excerpt: "#BOJ #DFS #완전탐색"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- DFS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

두 종류의 부등호 기호 ‘<’와 ‘>’가 k개 나열된 순서열  A가 있다. 우리는 이 부등호 기호 앞뒤에 서로 다른 한 자릿수 숫자를 넣어서 모든 부등호 관계를 만족시키려고 한다. 예를 들어, 제시된 부등호 순서열 A가 다음과 같다고 하자. 

A =>  < < < > < < > < >

부등호 기호 앞뒤에 넣을 수 있는 숫자는 0부터 9까지의 정수이며 선택된 숫자는 모두 달라야 한다. 아래는 부등호 순서열 A를 만족시키는 한 예이다. 

**3 < 4 < 5 < 6 > 1 < 2 < 8 > 7 < 9 > 0**

이 상황에서 부등호 기호를 제거한 뒤, 숫자를 모두 붙이면 하나의 수를 만들 수 있는데 이 수를 주어진 부등호 관계를 만족시키는 정수라고 한다. 그런데 주어진 부등호 관계를 만족하는 정수는 하나 이상 존재한다. 예를 들어 3456128790 뿐만 아니라 5689023174도 아래와 같이 부등호 관계 A를 만족시킨다. 

**5 < 6 < 8 < 9 > 0 < 2 < 3 > 1 < 7 > 4**

여러분은 제시된 k개의 부등호 순서를 만족하는 (k+1)자리의 정수 중에서 최댓값과 최솟값을 찾아야 한다. 앞서 설명한 대로 각 부등호의 앞뒤에 들어가는 숫자는 { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 }중에서 선택해야 하며 **선택된 숫자는 모두 달라야 한다**. 

### 입력

첫 줄에 부등호 문자의 개수를 나타내는 정수 k가 주어진다. 그 다음 줄에는 k개의 부등호 기호가 하나의 공백을 두고 한 줄에 모두 제시된다. k의 범위는 2 ≤ k ≤ 9 이다. 

### 출력

여러분은 제시된 부등호 관계를 만족하는 k+1 자리의 최대, 최소 정수를 첫째 줄과 둘째 줄에 각각 출력해야 한다. 단 아래 예(1)과 같이 첫 자리가 0인 경우도 정수에 포함되어야 한다. 모든 입력에 답은 항상 존재하며 출력 정수는 하나의 문자열이 되도록 해야 한다. 



### 예제 입력 1 

```
2
< > 
```

### 예제 출력 1 

```
897
021
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    static int size;
    static String[] input;
    static ArrayList<String> resultArr = new ArrayList<>();
    public static void main(String[] args) throws Exception{
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        size = Integer.parseInt(bufferedReader.readLine());
        input = bufferedReader.readLine().split(" ");

        boolean[] visited = new boolean[10];

        String result = "";
        DFS(0, visited, result );
        Collections.sort(resultArr);
        System.out.println(resultArr.get(resultArr.size()-1));
        System.out.println(resultArr.get(0));

    }

    public static void DFS(int depth, boolean[] visited, String result){
        if (size+1 == depth){
            for (int i = 0; i < input.length; i++){
                if (input[i].equals("<")){
                    if (!(result.charAt(i) < result.charAt(i+1))) return;
                }else {
                    if (!(result.charAt(i) > result.charAt(i+1))) return;
                }
            }
            resultArr.add(result);
            return;
        }

        for (int i = 0; i <= 9; i++){
            if (!visited[i]){
                visited[i] = true;
                DFS(depth + 1, visited, result + i);
                visited[i] = false;
            }
        }
    }
}
```

- 나는 모든 순열을 먼저 구했다.

  ```
  순열과 조합의 차이
  - 서로 다른 공 5개 : 1 2 3 4 5 중 3개를 뽑는다고 할 때
  - 순열: 서로 다른 공 5개중 임의로 3개 뽑아서 순서대로 나열을 하는 것이다.
    (135를 뽑았다면 -> 135/ 315/ 513/ 153/ 351/ 531)
  - 조합은 공을 뽑은 것으로 끝이 난다.
  즉, 순열은 뽑은 다음에 순서를 생각해야 하지만 조합은 뽑기만 하면 된다.
  ```

  왜냐하면 1 2 3과 3 2 1과는 다르기 때문에 

- **순열을 구하는 코드**

  ```java
  public static void DFS(int depth, boolean[] visited, String result){
    if (size+1 == depth){//순열이 부등호에 조합하는지 확인 !
      for (int i = 0; i < input.length; i++){
        if (input[i].equals("<")){
          if (!(result.charAt(i) < result.charAt(i+1))) return;
        }else {
          if (!(result.charAt(i) > result.charAt(i+1))) return;
        }
      }
      resultArr.add(result);
      return;
    }
    
    for (int i = 0; i <= 9; i++){// 각 부등호의 앞뒤에 들어가는 숫자는 { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 }때문.
      if (!visited[i]){
        visited[i] = true; //‼️
        DFS(depth + 1, visited, result + i);
        visited[i] = false; //‼️
      }
    }
  }
  ```

- 나온 순열의 중에서 부등호가 맞는 순열을 고르는 부분

  ```java
  if (size+1 == depth){
    for (int i = 0; i < input.length; i++){
      if (input[i].equals("<")){
        if (!(result.charAt(i) < result.charAt(i+1))) return;
      }else {
        if (!(result.charAt(i) > result.charAt(i+1))) return;
      }
    }
  ```

- 부등호에 맞는 순열을 고른 후, 최댓값과 최솟값을 구해야 하기 때문에 

  순열들을 다 list에 저장해서 정렬를 해주었다. 그리고 최소와 최대를 출력해주었다.



##### 느낀점

> 문제 : https://www.acmicpc.net/problem/2529
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2529_부등호.java

