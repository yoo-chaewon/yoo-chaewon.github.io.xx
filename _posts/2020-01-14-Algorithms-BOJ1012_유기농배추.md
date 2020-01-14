---

title:  "[ALGORITHM]BOJ1012-유기농배추"

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

차세대 영농인 한나는 강원도 고랭지에서 유기농 배추를 재배하기로 하였다. 농약을 쓰지 않고 배추를 재배하려면 배추를 해충으로부터 보호하는 것이 중요하기 때문에, 한나는 해충 방지에 효과적인 배추흰지렁이를 구입하기로 결심한다. 이 지렁이는 배추근처에 서식하며 해충을 잡아 먹음으로써 배추를 보호한다. 특히, 어떤 배추에 배추흰지렁이가 한 마리라도 살고 있으면 이 지렁이는 인접한 다른 배추로 이동할 수 있어, 그 배추들 역시 해충으로부터 보호받을 수 있다.

(한 배추의 상하좌우 네 방향에 다른 배추가 위치한 경우에 서로 인접해있다고 간주한다)

한나가 배추를 재배하는 땅은 고르지 못해서 배추를 군데군데 심어놓았다. 배추들이 모여있는 곳에는 배추흰지렁이가 한 마리만 있으면 되므로 서로 인접해있는 배추들이 몇 군데에 퍼져있는지 조사하면 총 몇 마리의 지렁이가 필요한지 알 수 있다.

예를 들어 배추밭이 아래와 같이 구성되어 있으면 최소 5마리의 배추흰지렁이가 필요하다.

(0은 배추가 심어져 있지 않은 땅이고, 1은 배추가 심어져 있는 땅을 나타낸다.)

| **1** | **1** | 0     | 0     | 0     | 0    | 0    | 0     | 0     | 0     |
| ----- | ----- | ----- | ----- | ----- | ---- | ---- | ----- | ----- | ----- |
| 0     | **1** | 0     | 0     | 0     | 0    | 0    | 0     | 0     | 0     |
| 0     | 0     | 0     | 0     | **1** | 0    | 0    | 0     | 0     | 0     |
| 0     | 0     | 0     | 0     | **1** | 0    | 0    | 0     | 0     | 0     |
| 0     | 0     | **1** | **1** | 0     | 0    | 0    | **1** | **1** | **1** |
| 0     | 0     | 0     | 0     | **1** | 0    | 0    | **1** | **1** | **1** |

### 입력

입력의 첫 줄에는 테스트 케이스의 개수 T가 주어진다. 그 다음 줄부터 각각의 테스트 케이스에 대해 첫째 줄에는 배추를 심은 배추밭의 가로길이 M(1 ≤ M ≤ 50)과 세로길이 N(1 ≤ N ≤ 50), 그리고 배추가 심어져 있는 위치의 개수 K(1 ≤ K ≤ 2500)이 주어진다. 그 다음 K줄에는 배추의 위치 X(0 ≤ X ≤ M-1), Y(0 ≤ Y ≤ N-1)가 주어진다.

### 출력

각 테스트 케이스에 대해 필요한 최소의 배추흰지렁이 마리 수를 출력한다.



### 예제 입력 1

```
2
10 8 17
0 0
1 0
1 1
4 2
4 3
4 5
2 4
3 4
7 4
8 4
9 4
7 5
8 5
9 5
7 6
8 6
9 6
10 10 1
5 5
```

### 예제 출력 1

```
5
1
```

## 나의 코드

### DFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int N, M;
    static int[][] map;
    static boolean[][] visited;
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int testCase = Integer.parseInt(bufferedReader.readLine());
        while (testCase-- > 0){
            String[] input = bufferedReader.readLine().split(" ");
            M = Integer.parseInt(input[0]);
            N =Integer.parseInt(input[1]);
            int K = Integer.parseInt(input[2]);
            map = new int[N][M];
            visited = new boolean[N][M];

            for (int i = 0; i < K; i++){
                String[] point = bufferedReader.readLine().split(" ");
                int x = Integer.parseInt(point[0]);
                int y = Integer.parseInt(point[1]);
                map[y][x] = 1;
            }

            int answer = 0;
            for (int i = 0; i < N; i++){
                for (int j = 0; j < M; j++){
                    if (map[i][j] == 1 && !visited[i][j]){
                        answer++;
                        DFS(j, i);
                    }
                }
            }
            System.out.println(answer);
        }
    }

    public static void DFS(int x, int y){
        visited[y][x] = true;

        for (int i = 0; i < 4; i++){
            int nextX = x + dx[i];
            int nextY = y + dy[i];

            if (nextX < 0 || nextY < 0 || M <= nextX || N <= nextY || visited[nextY][nextX] || map[nextY][nextX] == 0) continue;
            DFS(nextX, nextY);
        }
    }
}
```

- 떨어져 있는 영역을 구하는 문제로 DFS랑 BFS다 구현할 수 있는 문제다. 나는 DFS가 구현이 더 간단해서 DFS로 먼저 풀어보았다.
- 그냥 1인 영역을 찾아서 그 1인 영역 전체를 visited를 true로 만드는 식이다. 이때 DFS는 깊이우선으로 보는 것이고, BFS는 넓이 우선으로 보는 것이고. 나는 이런 문제에서 DFS가 구현이 간단해서 더 좋다.

### BFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int N, M;
    static int[][] map;
    static boolean[][] visited;
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int testCase = Integer.parseInt(bufferedReader.readLine());
        while (testCase-- > 0){
            String[] input = bufferedReader.readLine().split(" ");
            M = Integer.parseInt(input[0]);
            N =Integer.parseInt(input[1]);
            int K = Integer.parseInt(input[2]);
            map = new int[N][M];
            visited = new boolean[N][M];

            for (int i = 0; i < K; i++){
                String[] point = bufferedReader.readLine().split(" ");
                int x = Integer.parseInt(point[0]);
                int y = Integer.parseInt(point[1]);
                map[y][x] = 1;
            }

            int answer = 0;
            for (int i = 0; i < N; i++){
                for (int j = 0; j < M; j++){
                    if (map[i][j] == 1 && !visited[i][j]){
                        answer++;
                        BFS(j, i);
                    }
                }
            }
            System.out.println(answer);
        }
    }

    public static void BFS(int x, int y){
        Queue<Point> queue = new LinkedList<>();
        queue.add(new Point(x, y));
        visited[y][x] = true;

        while (!queue.isEmpty()){
            Point curPoint = queue.poll();
            for (int i = 0; i < 4; i++){
                int nextX = curPoint.x + dx[i];
                int nextY = curPoint.y + dy[i];

                if (nextX < 0 || nextY < 0 || M <= nextX || N <= nextY || visited[nextY][nextX] || map[nextY][nextX] == 0) continue;
                queue.add(new Point(nextX, nextY));
                visited[nextY][nextX] = true;
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

- DFS로만 풀어봤는데 BFS로 그냥 한번 풀어봤다. 
  - 1인 영역을 큐에 다 넣고 했어도 될거 같고 내방법으로 했어도 될거 같다. 



## 느낀점

DFS/BFS는 조금만 응용되어도 어려워서 기본을 완전히 이해하는게 중요한거 같다. 어려운 문제도 쉽게 풀고 싶다 !



- 문제: https://www.acmicpc.net/problem/1012
- 저장소: 
  - DFS: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1012_유기농배추_0114_DFS.java
  - BFS: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1012_유기농배추_0114_BFS.java