---

title:  "[ALGORITHM]BOJ2178-미로탐색"

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

N×M크기의 배열로 표현되는 미로가 있다.

| 1    | 0    | 1    | 1    | 1    | 1    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1    | 0    | 1    | 0    | 1    | 0    |
| 1    | 0    | 1    | 0    | 1    | 1    |
| 1    | 1    | 1    | 0    | 1    | 1    |

미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, (1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램을 작성하시오. 한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다.

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.

### 입력

첫째 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 **붙어서** 입력으로 주어진다.

### 출력

첫째 줄에 지나야 하는 최소의 칸 수를 출력한다. 항상 도착위치로 이동할 수 있는 경우만 입력으로 주어진다.



### 예제 입력 1

```
4 6
101111
101010
101011
111011
```

### 예제 출력 1

```
15
```

### 예제 입력 2

```
4 6
110110
110110
111111
111101
```

### 예제 출력 2

```
9
```

### 예제 입력 3

```
2 25
1011101110111011101110111
1110111011101110111011101
```

### 예제 출력 3

```
38
```

### 예제 입력 4

```
7 7
1011111
1110001
1000001
1000001
1000001
1000001
1111111
```

### 예제 출력 4

```
13
```





## 나의 코드

### BFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int N, M;// 세로, 가로
    static int[][] map;
    static boolean[][] visited;

    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] inputNM = bufferedReader.readLine().split(" ");
        N = Integer.parseInt(inputNM[0]);
        M = Integer.parseInt(inputNM[1]);
        map = new int[N][M];
        visited = new boolean[N][M];

        for (int i = 0; i < N; i++) {
            String[] input = bufferedReader.readLine().split("");
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(input[j]);
            }
        }
        BFS(0, 0);
        System.out.println(map[N-1][M-1]);
    }

    public static void BFS(int x, int y) {
        Queue<com.company.Point> queue = new LinkedList<>();
        queue.add(new com.company.Point(x, y));
        visited[y][x] = true;

        while (!queue.isEmpty()){
            com.company.Point curPoint = queue.poll();
            if (curPoint.x == M-1 && curPoint.y == N-1) return;

            for (int i = 0; i < 4; i++){
                int nextX = curPoint.x + dx[i];
                int nextY = curPoint.y + dy[i];

                if (nextX < 0 || nextY < 0 || M <= nextX || N <= nextY || visited[nextY][nextX] || map[nextY][nextX] == 0) continue;
                visited[nextY][nextX] = true;
                map[nextY][nextX] += map[curPoint.y][curPoint.x];
                queue.add(new com.company.Point(nextX, nextY));
            }
        }
    }
}
class Point{
    int x;
    int y;
    Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}
```



### DFS- 시간초과

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int N, M;// 세로, 가로
    static int[][] map;
    static boolean[][] visited;
    static int min = Integer.MAX_VALUE;

    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] inputNM = bufferedReader.readLine().split(" ");
        N = Integer.parseInt(inputNM[0]);
        M = Integer.parseInt(inputNM[1]);

        map = new int[N][M];
        visited = new boolean[N][M];

        for (int i = 0; i < N; i++) {
            String[] input = bufferedReader.readLine().split("");
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(input[j]);
            }
        }

        DFS(0, 0, 0);
        System.out.println(min);
    }

    public static void DFS(int x, int y, int count) {
        visited[y][x] = true;
        count++;

        if (x == M - 1 && y == N - 1) {
            min = Math.min(min, count);
            return;
        }

        for (int i = 0; i < 4; i++) {
            int nextX = x + dx[i];
            int nextY = y + dy[i];

            if (nextX < 0 || nextY < 0 || M <= nextX || N <= nextY || visited[nextY][nextX] || map[nextY][nextX] == 0 || min < count)
                continue;
            visited[nextY][nextX] = true;
            DFS(nextX, nextY, count);
            visited[nextY][nextX] = false;
        }
    }
}
```

- BFS는 너비우선 탐색으로 범위를 넓혀가면서 탐색을 한다. 그렇기 때문에 목적지 닿는 곳을 최소로 찾을수 있을 것이다. 그리고 찾으면 종료된다.

- 하지만 DFS는 깊이우선 탐색으로 한가지 경로를 잡으면 끝까지 가고, 그 다음 경로로 끝가지 가고 이런식이다. 그렇기 때문에 최소값을 찾기 위해서 모든 경로를 다 본 다음에 최소값을 찾아내야 한다.

- **‼️ 따라서 최단 경로에는 BFS가 더 적합하다 ‼️**

  - 위 DFS경우 조건을 걸어서 최단 거리 없을거 같은 경우를 제외시켜주는 ? 그런걸 나중에 고민해서 더 짜봐야겠다.

- BFS일 경우 탐색 길을 누적하는 것으로 하였다. 그러면 목적지에 최단경로의 값이 들어가 있을 것이다.

  ```
  //입력1
  101111
  101010
  101011
  111011
  
  //출력1
  1 0 9 10 11 12
  2 0 8 0  12 0
  3 0 7 0  13 14
  4 5 6 0  14 15
  
  //입력2
  110110
  110110
  111111
  111101
  
  //출력2
  1 2 0 8 9 0
  2 3 0 7 8 0
  3 4 5 6 7 8
  4 5 6 7 0 9
  ```

  보기 어렵지만 아무튼 이런식으로 !



## 느낀점

BFS에 비해 DFS는 구현하기 더 간단하지만 시간초과 문제를 고려해야한다. 쓸모없는 탐색이 많을 수록 시간초과가 잘 된다. 그런 점에서 이 문제는 BFS가 더 적절했던거 같다. 문제를 보고 먼저 BFS/DFS중 어떤게 더 적합한 방법인지 고민하고 코드를 짜도록 해야겠다.  🧐이 문제를 통해 **최단거리에는 BFS가 더 적합하다는 것을 알게되었다.** 🧐



- 문제: https://www.acmicpc.net/problem/2178
- 저장소: 
  - DFS: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2178_미로탐색_0114_DFS.java
  - BFS: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2178_미로탐색_0114_BFS.java