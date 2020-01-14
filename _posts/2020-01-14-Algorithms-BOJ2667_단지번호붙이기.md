---

title:  "[ALGORITHM]BOJ2667-단지번호붙이기"

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

<그림 1>과 같이 정사각형 모양의 지도가 있다. 1은 집이 있는 곳을, 0은 집이 없는 곳을 나타낸다. 철수는 이 지도를 가지고 연결된 집들의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다. 대각선상에 집이 있는 경우는 연결된 것이 아니다. <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.

![img](https://www.acmicpc.net/upload/images/ITVH9w1Gf6eCRdThfkegBUSOKd.png)

### 입력

첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.

### 출력

첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.



### 예제 입력 1

```
7
0110100
0110101
1110101
0000111
0100000
0111110
0111000
```

### 예제 출력 1

```
3
7
8
9
```



## 나의 코드

### DFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int size;
    static int[][] map;
    static boolean[][] visited;
    static int count;
    public static void main(String[] args) throws Exception{
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        size = Integer.parseInt(bufferedReader.readLine());
        map = new int[size][size];
        visited = new boolean[size][size];

        for (int i = 0; i < size; i++){
            String[] input = bufferedReader.readLine().split("");
            for (int j = 0; j <size; j++){
                map[i][j] = Integer.parseInt(input[j]);
            }
        }

        int aptSet = 0;
        ArrayList<Integer> aptCount = new ArrayList<>();
        for (int i = 0; i < size; i++){
            for (int j = 0; j <size; j++){
                if (map[i][j] != 0 && !visited[i][j]){
                    aptSet++;
                    count = 0;
                    DFS(j, i);
                    aptCount.add(count);
                }
            }
        }
        System.out.println(aptSet);
        Collections.sort(aptCount);
        for (int a : aptCount){
            System.out.println(a);
        }
    }

    public static void DFS(int x, int y){
        count++;
        visited[y][x] = true;
        for (int i =0; i < 4; i++){
            int nextX = x + dx[i];
            int nextY = y + dy[i];

            if (nextX < 0 || size <= nextX || nextY < 0 || size <= nextY || visited[nextY][nextX] || map[nextY][nextX] == 0) continue;
            DFS(nextX, nextY);
        }
    }
}
```

### BFS

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int size;
    static int[][] map;
    static boolean[][] visited;
    static int count;
    public static void main(String[] args) throws Exception{
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        size = Integer.parseInt(bufferedReader.readLine());
        map = new int[size][size];
        visited = new boolean[size][size];

        for (int i = 0; i < size; i++){
            String[] input = bufferedReader.readLine().split("");
            for (int j = 0; j <size; j++){
                map[i][j] = Integer.parseInt(input[j]);
            }
        }

        int aptSet = 0;
        ArrayList<Integer> aptCount = new ArrayList<>();
        for (int i = 0; i < size; i++){
            for (int j = 0; j < size; j++){
                if (map[i][j] != 0 && !visited[i][j]) {
                    aptSet++;
                    BFS(j, i);
                    aptCount.add(count);
                }
            }
        }
        System.out.println(aptSet);
        Collections.sort(aptCount);
        for (int a : aptCount){
            System.out.println(a);
        }
    }

    public static void BFS(int x, int y){
        Queue<Point> queue = new LinkedList<>();
        queue.add(new Point(x, y));
        visited[y][x] = true;
        count = 0;
        while (!queue.isEmpty()){
            Point cur = queue.poll();
            count++;
            for (int i = 0; i < 4; i++){
                int nextX = cur.x + dx[i];
                int nextY = cur.y + dy[i];

                if (nextX < 0 || nextY < 0 || size <= nextX || size <= nextY || visited[nextY][nextX] || map[nextY][nextX] == 0) continue;
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



## 느낀점

BFS/ DFS의 가장 기본적인 문제라고 생각한다 ‼️ 개념을 잡고 싶으면 이문제를 다시 풀어봐야 겠다.그리고 뭔가 예전에는 BFS가 더 쉽다고 생각했지만 지금은 DFS가 구현이 더 간단해서 좋은거 같다. BFS하려면 x,y 좌료를 저장하는 큐를 만들어야 하고... 하지만 재귀라는 것은 알다가도 갑자기 이해 안되는 순간이 있기 때문에.. 상황에 맞는 풀이법으로 접근하도록 해야겠다 🧐



- 문제: https://www.acmicpc.net/problem/2667
- 저장소: 
  - DFS: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2667_단지번호붙이기_0114_DFS.java
  - BFS: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2667_단지번호붙이기_0114_BFS.java