---

title:  "[ALGORITHM]BOJ1698-숨바꼭질"

excerpt: "#BOJ #BFS #DFS"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- DFS/BFS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.

### 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

### 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.



### 예제 입력 1

```
5 17
```

### 예제 출력 1

```
4
```

### 힌트

수빈이가 5-10-9-18-17 순으로 가면 4초만에 동생을 찾을 수 있다.



## 나의 코드-BFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    static int MAX = 100000;
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] input = bufferedReader.readLine().split(" ");
        int N = Integer.parseInt(input[0]);//수빈위치
        int K = Integer.parseInt(input[1]);//동생위치
        Queue<Integer> queue = new LinkedList<>();
        queue.add(N);
        int[] map = new int[MAX + 1];
        while (!queue.isEmpty()) {
            int temp = queue.poll();
            if (temp == K){
                System.out.print(map[temp]);
                return;
            }
            int[] next = new int[3];
            next[0] = temp - 1;
            next[1] = temp + 1;
            next[2] = temp * 2;

            for (int i = 0; i < 3; i++) {
                if (next[i] < 0 || next[i] > MAX || map[next[i]] != 0) {
                    continue;
                }
                map[next[i]] = map[temp] + 1;
                queue.add(next[i]);
            }
        }
    }
}
```

- 이 문제는 **최소한으로 목적지**를 구하는 문제로, 따라서 BFS로 접근해서 풀었다.

- 각 정점을 보고 각 정점마다 x-1, x+1, x*2를 해준다는 점에서 아주 **BFS**적이다.

- 처음에는 map에 값을 누적하지 않고, class를 만들어 위치(position)와 움직인 횟수(count)를 해주었다.

  - 이렇게 하면 방문했던 곳을 또 방문할 수 있다.(많이 반복할수록 최소로 방문하는 것에서 멀어진다.) ->visited 배열을 또 사용해야함.

  - 따라서 map에 누적을 해주고,

    ```java
    if (next[i] < 0 || next[i] > MAX || map[next[i]] != 0) {
      continue;
    }
    //map[next[i]] != 0 라는 조건을 추가해주었다.
    ```

    



## 느낀점

DFS로 접근했다가 다시 다 지우고 다시 풀었다...앞으로 문제를 잘 읽고 **생각**‼️ 부터 하고 풀어야겠다.

> - 문제 : https://www.acmicpc.net/problem/1697
> - 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1697_숨바꼭질_0116.java