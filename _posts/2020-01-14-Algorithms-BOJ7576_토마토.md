---

title:  "[ALGORITHM]BOJ7576-토마토"

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

철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자 모양 상자의 칸에 하나씩 넣어서 창고에 보관한다. 

![img](https://www.acmicpc.net/upload/images/tmt.png)

창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토의 인접한 곳은 왼쪽, 오른쪽, 앞, 뒤 네 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지, 그 최소 일수를 알고 싶어 한다.

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.

### 입력

첫 줄에는 상자의 크기를 나타내는 두 정수 M,N이 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M,N ≤ 1,000 이다. 둘째 줄부터는 하나의 상자에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 상자에 담긴 토마토의 정보가 주어진다. 하나의 줄에는 상자 가로줄에 들어있는 토마토의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다. 

### 출력

여러분은 토마토가 모두 익을 때까지의 최소 날짜를 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.



### 예제 입력 1

```
6 4
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1
```

### 예제 출력 1

```
8
```

### 예제 입력 2

```
6 4
0 -1 0 0 0 0
-1 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 1
```

### 예제 출력 2

```
-1
```

### 예제 입력 3

```
6 4
1 -1 0 0 0 0
0 -1 0 0 0 0
0 0 0 0 -1 0
0 0 0 0 -1 1
```

### 예제 출력 3

```
6
```

### 예제 입력 4

```
5 5
-1 1 0 0 0
0 -1 -1 -1 0
0 -1 -1 -1 0
0 -1 -1 -1 0
0 0 0 0 0
```

### 예제 출력 4

```
14
```

### 예제 입력 5

```
2 2
1 -1
-1 1
```

### 예제 출력 5

```
0
```



## 나의 코드-BFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int N; //세로
    static int M; //가로
    static int[][] map;
    static boolean[][] visited;

    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] MN = bufferedReader.readLine().split(" ");
        M = Integer.parseInt(MN[0]);
        N = Integer.parseInt(MN[1]);
        map = new int[N][M];
        visited = new boolean[N][M];
        Queue<com.company.Point> queue = new LinkedList<>();

        for (int i = 0; i < N; i++) {
            String[] input = bufferedReader.readLine().split(" ");
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(input[j]);
                if (map[i][j] == 1) {
                    queue.add(new com.company.Point(j, i));
                }
            }
        }
        BFS(queue);
        System.out.println(checkMap());
    }

    public static void BFS(Queue<com.company.Point> queue) {
        while (!queue.isEmpty()) {
            com.company.Point curPoint = queue.poll();
            visited[curPoint.y][curPoint.x] = true;
            for (int i = 0; i < 4; i++) {
                int nextX = curPoint.x + dx[i];
                int nextY = curPoint.y + dy[i];

                if (nextX < 0 || nextY < 0 || M <= nextX || N <= nextY || visited[nextY][nextX] || map[nextY][nextX] == -1 || map[nextY][nextX] == 1)
                    continue;
                visited[nextY][nextX] = true;
                map[nextY][nextX] += map[curPoint.y][curPoint.x] + 1;
                queue.add(new com.company.Point(nextX, nextY));
            }
        }
    }

    public static int checkMap() {
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] == 0) return -1;
                if (map[i][j] != -1) max = Math.max(max, map[i][j]);
            }
        }
        return max - 1;
    }
}

class Point {
    int x;
    int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}


```

- 이 문제는 시작점이 1개가 아니라 여러곳일 수도 있다는 것이 핵심인거 같다.
- 일단 시작점이 될 수 있는 것을 모두 큐에 담고, 그것들을 하나씩 넓혀 가는 것이다.
  - 이게는 큐가 완벽하게 적합하다. 
  - 시작점이 A와 B가 있어 큐에 AB가있는데 A로부터 퍼진 것들이 B뒤로 오기 때문에 퍼짐을 동시에 할수 있게 되는 것이다 ! (말이 좀 어렵나? 아무튼.)



## 느낀점

재밌당

- 문제: https://www.acmicpc.net/problem/7576
- 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_7576_토마토_0114_BFS.java