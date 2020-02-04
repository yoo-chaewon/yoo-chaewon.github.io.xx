---

title:  "[ALGORITHM]BOJ6603-로또"

excerpt: "#BOJ #DFS"



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

독일 로또는 {1, 2, ..., 49}에서 수 6개를 고른다.

로또 번호를 선택하는데 사용되는 가장 유명한 전략은 49가지 수 중 k(k>6)개의 수를 골라 집합 S를 만든 다음 그 수만 가지고 번호를 선택하는 것이다.

예를 들어, k=8, S={1,2,3,5,8,13,21,34}인 경우 이 집합 S에서 수를 고를 수 있는 경우의 수는 총 28가지이다. ([1,2,3,5,8,13], [1,2,3,5,8,21], [1,2,3,5,8,34], [1,2,3,5,13,21], ..., [3,5,8,13,21,34])

집합 S와 k가 주어졌을 때, 수를 고르는 모든 방법을 구하는 프로그램을 작성하시오.

### 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스는 한 줄로 이루어져 있다. 첫 번째 수는 k (6 < k < 13)이고, 다음 k개 수는 집합 S에 포함되는 수이다. S의 원소는 오름차순으로 주어진다.

입력의 마지막 줄에는 0이 하나 주어진다. 

### 출력

각 테스트 케이스마다 수를 고르는 모든 방법을 출력한다. 이때, 사전 순으로 출력한다.

각 테스트 케이스 사이에는 빈 줄을 하나 출력한다.



### 예제 입력 1

```
7 1 2 3 4 5 6 7
8 1 2 3 5 8 13 21 34
0
```

### 예제 출력 1

```
1 2 3 4 5 6
1 2 3 4 5 7
1 2 3 4 6 7
1 2 3 5 6 7
1 2 4 5 6 7
1 3 4 5 6 7
2 3 4 5 6 7

1 2 3 5 8 13
1 2 3 5 8 21
1 2 3 5 8 34
1 2 3 5 13 21
1 2 3 5 13 34
1 2 3 5 21 34
1 2 3 8 13 21
1 2 3 8 13 34
1 2 3 8 21 34
1 2 3 13 21 34
1 2 5 8 13 21
1 2 5 8 13 34
1 2 5 8 21 34
1 2 5 13 21 34
1 2 8 13 21 34
1 3 5 8 13 21
1 3 5 8 13 34
1 3 5 8 21 34
1 3 5 13 21 34
1 3 8 13 21 34
1 5 8 13 21 34
2 3 5 8 13 21
2 3 5 8 13 34
2 3 5 8 21 34
2 3 5 13 21 34
2 3 8 13 21 34
2 5 8 13 21 34
3 5 8 13 21 34
```



## 나의 코드

```java
import java.io.InputStreamReader;
import java.util.Scanner;

public class Main {
    static int lotto = 6;
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(new InputStreamReader(System.in));
        int size = -1;
        while ((size = scanner.nextInt()) != 0){
            int[] map = new int[size];
            int[] result = new int[lotto];

            for (int i = 0; i < size; i++){
                map[i] = scanner.nextInt();
            }

            DFS(0, 0, map, result);
            System.out.println();
        }
    }

    public static void DFS(int start, int depth, int[] map, int[]result){
        if (depth == lotto){
            for (int i = 0; i < lotto; i++){
                System.out.print(result[i] + " ");
            }
            System.out.println();
            return;
        }

        for (int i = start; i < map.length; i++){
            result[depth] = map[i];
            DFS(i+1, depth+1, map, result);
        }
    }
}
```

- 우선 출력 결과를 보면

  ```
  //입력
  7 1 2 3 4 5 6 7
  
  //출력
  1 2 3 4 5 6
  1 2 3 4 5 7
  1 2 3 4 6 7
  1 2 3 5 6 7
  1 2 4 5 6 7
  1 3 4 5 6 7
  2 3 4 5 6 7
  ```

  **중복이 없다**  그리고 **사전순이다**

  따라서, visited는 사용할 필요가 없다. 만약 12345 와 12354 둘이 다르면 visited를 사용해야 겠지만.

  그리고, 맨 뒤에것들끼리 보면 값이 바뀌는 것이 DFS로 접근 했다는 것을 알 수 있다.(나는 그렇게 생각했다 !)

  123456을 보고 다음 123457이렇게 탐색 !

  

- depth변수는 지금까지 글자수라고 생각하면 쉽다. 그 숫자가 로또의 숫자 수만큼이 되면 출력을 해주는 것이다.

  ```java
  if (depth == lotto){
    for (int i = 0; i < lotto; i++){
      System.out.print(result[i] + " ");
    }
    System.out.println();
    return;
  }
  ```

- 전체 완전 탐색은 이런식으로 이루어진다.

  ```java
  for (int i = start; i < map.length; i++){
    result[depth] = map[i]; //결과의 마지막에 맵의 값을 하나씩 넣어준다.
    DFS(i+1, depth+1, map, result);
  }
  ```

  예를 들어) 1234567이 있을 때,  start 가 5면,  123456 보고, for문이기 때문에 123457 이렇게 탐색을 하는 것이다 !



##### 느낀점

사실 이 문제는 100퍼센트 이해하지는 못한거 같다. 그때도 너무 어려웠고, 이번에도 긴가민가 풀어서 돌렸는데 답이 나온 것이다. 중복이 없다는 것, 사전 정렬이라는 것이 핵심인 거 같다. 

> 문제 : https://www.acmicpc.net/problem/6603
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_6603_로또_0204.java

