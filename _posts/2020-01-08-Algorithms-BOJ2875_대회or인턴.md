---

title:  "[ALGORITHM]BOJ2875-대회or인턴"

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

백준대학교에서는 대회에 나갈 때 2명의 여학생과 1명의 남학생이 팀을 결성해서 나가는 것이 원칙이다. (왜인지는 총장님께 여쭈어보는 것이 좋겠다.)

백준대학교는 뛰어난 인재들이 많아 올해에도 N명의 여학생과 M명의 남학생이 팀원을 찾고 있다.

그런데 올해에는 대회에 참여하려는 학생들 중 K명을 반드시 인턴쉽 프로그램에 참여하라는 학교의 방침이 생기게 되었다. 인턴쉽에 참여하는 학생은 대회에 참여하지 못한다.

백준대학교에서는 뛰어난 인재들이 많기 때문에, 많은 팀을 만드는 것이 최선이다.

여러분은 N명의 여학생과 M명의 남학생, K명의 인턴쉽에 참여해야하는 인원이 주어질 때 만들 수 있는 최대의 팀 수를 구하면 된다.

### 입력

첫째 줄에 N, M, K가 순서대로 주어진다. (0 ≤ M ≤ 100), (0 ≤ N ≤ 100), (0 ≤ K ≤ M+N),

### 출력

만들 수 있는 팀의 최댓값을 출력하면 된다.



### 예제 

```
6 3 2
```

### 예제 

```
2
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] input = bufferedReader.readLine().split(" ");
        int N = Integer.parseInt(input[0]);
        int M = Integer.parseInt(input[1]);
        int K = Integer.parseInt(input[2]);
        int total = N + M;

        int team = M;
        while (true){
            if (K <=total-3*team && team*2 <= N) break;
            team --;
        }
        System.out.println(team);
    }
}
```

- 먼저 나올수 있는 팀의 수의 가장 max값은 남학생 수라고 생각을 했다.

  남학생1명, 여학생2명이 한 팀이기 때문에 ! 

- 다음은 while문을 돌면서

  1. 팀에 속하지 않은 학생이 인턴수보다 커야한다.

     K <=total-3*team

  2. 남학생을 기준으로 팀을 계산했기 때문에 여학생도 이 것에 부합해야 한다.

     따라서, team*2 <= N 을 해주었다.

     👉 위 조건에 만족을 하면 break, 만족하지 못하면 team을 1개씩 줄여 나갔다.

  

## 느낀점

2달만에 다시 풀었던 문젠데 그때는 어렵게 생각해서 팀 수를 나눠서 계산했던거 같다. 하지만 이번에는 간단하게 생각해서 쉽게 나온거 같다!

- 문제 : https://www.acmicpc.net/problem/2875
- 저장소1 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2875_대회or인턴.java
- 저장소2 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_2875_대회or인턴_ver2.java