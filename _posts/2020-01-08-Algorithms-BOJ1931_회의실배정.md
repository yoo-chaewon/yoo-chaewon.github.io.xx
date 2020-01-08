---

title:  "[ALGORITHM]BOJ1931-회의실배정"

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

한 개의 회의실이 있는데 이를 사용하고자 하는 N개의 회의들에 대하여 회의실 사용표를 만들려고 한다. 각 회의 I에 대해 시작시간과 끝나는 시간이 주어져 있고, 각 회의가 겹치지 않게 하면서 회의실을 사용할 수 있는 최대수의 회의를 찾아라. 단, 회의는 한번 시작하면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다. 회의의 시작시간과 끝나는 시간이 같을 수도 있다. 이 경우에는 시작하자마자 끝나는 것으로 생각하면 된다.

### 입력

첫째 줄에 회의의 수 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N+1 줄까지 각 회의의 정보가 주어지는데 이것은 공백을 사이에 두고 회의의 시작시간과 끝나는 시간이 주어진다. 시작 시간과 끝나는 시간은 231-1보다 작거나 같은 자연수 또는 0이다.

### 출력

첫째 줄에 최대 사용할 수 있는 회의 수를 출력하여라.



### 예제 입력 1

```
11
1 4
3 5
0 6
5 7
3 8
5 9
6 10
8 11
8 12
2 13
12 14
```

### 예제 출력 1

```
4
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.Comparator;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(bufferedReader.readLine());
        int[][] map = new int[N][2];

        for (int i = 0; i < N; i++) {
            String[] input = bufferedReader.readLine().split(" ");
            map[i][0] = Integer.parseInt(input[0]);
            map[i][1] = Integer.parseInt(input[1]);
        }

        Arrays.sort(map, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                if (o1[1] == o2[1]) return o1[0] - o2[0];
                return o1[1] - o2[1];
            }
        });

        int answer = 0;
        int cur = -1;

        for (int i = 0; i < N; i++) {
            if (cur <= map[i][0]) {
                answer++;
                cur = map[i][1];
            }
        }
        System.out.println(answer);
    }
}
```

- 이 문제의 요점은 어떻게 하면 가장 많은 회의를 할 수 있을 지다.

  1. 회의시작 빠른 순

  2. 회의 짧은 순

  3. 회의 일찍 끝나는 순 ✔️

     👉 가장 빨리 끝나는 회의를 고려한다면 그만큼 뒤에 회의가 많이 남아 있는 것 !!

- 아무튼 위에 것을 떠올린다면 구현은 간단하다.

  - 회의의 종료시간으로 정렬을 해야한다. 

    👉 이때 이차원 배열의 특정 기준으로 정렬을 해야하기 때문에 comparator를 구현할줄 알아야 한다!

  - '한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다.' 라는 조건이 있다

    - [1,1] [2,2] [1,2] 가 있다면 종료시간만 고려해서 정렬을 하게 된다면 [1,1] [2,2] [1,2] 일 것이다. 그러면 총 2번의 회의가 가능 하다.

      하지만, [1,1] [1,2] [2,2] 로 된다면 총 3번의 회의가 가능하다!

      👉 따라서 만약에 끝나는 시간이 같은 경우에는 시작 시간 순으로 정렬을 하도록 하였다.

      ```java
      Arrays.sort(map, new Comparator<int[]>() {
                  @Override
                  public int compare(int[] o1, int[] o2) {
                      if (o1[1] == o2[1]) return o1[0] - o2[0];//종료시간 같을 경우
                      return o1[1] - o2[1];
                  }
              });
      ```

      



## 느낀점

이제 Comparator을 스스로 구현할 수 있어서 뿌듯하다! 아직도 o1-o2이거 순서는 헷갈리기는 하지만 ❗️아무튼 오름차순은 o1-o2로 기억하기!

- 문제: https://www.acmicpc.net/problem/1931
- 저장소: https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1931_회의실배정_ver2.java