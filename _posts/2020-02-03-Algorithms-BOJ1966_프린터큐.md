---

title:  "[ALGORITHM]BOJ1966-프린터큐"

excerpt: "#BOJ #Stack #FILO"



categories:

- ALGORITHM

tags:

- ALGORITHM

- BOJ

- STACK

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

여러분도 알다시피 여러분의 프린터 기기는 여러분이 인쇄하고자 하는 문서를 인쇄 명령을 받은 ‘순서대로’, 즉 먼저 요청된 것을 먼저 인쇄한다. 여러 개의 문서가 쌓인다면 Queue 자료구조에 쌓여서 FIFO - First In First Out - 에 따라 인쇄가 되게 된다. 하지만 상근이는 새로운 프린터기 내부 소프트웨어를 개발하였는데, 이 프린터기는 다음과 같은 조건에 따라 인쇄를 하게 된다.

1. 현재 Queue의 가장 앞에 있는 문서의 ‘중요도’를 확인한다.
2. 나머지 문서들 중 현재 문서보다 중요도가 높은 문서가 하나라도 있다면, 이 문서를 인쇄하지 않고 Queue의 가장 뒤에 재배치 한다. 그렇지 않다면 바로 인쇄를 한다.

예를 들어 Queue에 4개의 문서(A B C D)가 있고, 중요도가 2 1 4 3 라면 C를 인쇄하고, 다음으로 D를 인쇄하고 A, B를 인쇄하게 된다.

여러분이 할 일은, 현재 Queue에 있는 문서의 수와 중요도가 주어졌을 때, 어떤 한 문서가 몇 번째로 인쇄되는지 알아내는 것이다. 예를 들어 위의 예에서 C문서는 1번째로, A문서는 3번째로 인쇄되게 된다.

### 입력

첫 줄에 test case의 수가 주어진다. 각 test case에 대해서 문서의 수 N(100이하)와 몇 번째로 인쇄되었는지 궁금한 문서가 현재 Queue의 어떤 위치에 있는지를 알려주는 M(0이상 N미만)이 주어진다. 다음줄에 N개 문서의 중요도가 주어지는데, 중요도는 1 이상 9 이하이다. 중요도가 같은 문서가 여러 개 있을 수도 있다. 위의 예는 N=4, M=0(A문서가 궁금하다면), 중요도는 2 1 4 3이 된다.

### 출력

각 test case에 대해 문서가 몇 번째로 인쇄되는지 출력한다.



### 예제 입력 1

```
3
1 0
5
4 2
1 2 3 4
6 0
1 1 9 1 1 1
```

### 예제 출력 1

```
1
2
5
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.LinkedList;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int testCase = Integer.parseInt(bufferedReader.readLine());

        while (testCase-- > 0) {
            String[] NM = bufferedReader.readLine().split(" ");
            int N = Integer.parseInt(NM[0]); //문서수
            int M = Integer.parseInt(NM[1]); //문서 결과 찾아야 하는

            LinkedList<Integer> list = new LinkedList<>();
            int[] priority = new int[N];
            String[] input = bufferedReader.readLine().split(" ");

            for (int i = 0; i < N; i++) {
                list.offer(Integer.parseInt(input[i]));
                priority[i] = Integer.parseInt(input[i]);
            }
            Arrays.sort(priority);
            int cur = N - 1;
            int answer = 0;

            while (!list.isEmpty()) {
                int temp = list.poll();
                if (temp != priority[cur]) {
                    list.offer(temp);
                    M = (M == 0) ? list.size() - 1 : --M;
                } else {
                    cur--;
                    answer++;
                    if (M == 0) {
                        System.out.println(answer);
                        break;
                    } else {
                        M--;
                    }
                }
            }
        }
    }
}
```

- 푸는 방법은 여러가지 있겠지만 내가 푼 방법은 

  ```
  - 큐: 우선순위가 들어가 있음.
  - priority List: 우선순위가 정렬되어 있으며, 큐에서 우선순위가 높은게 빠질 때 다음 우선순위를 본다.
  - 찾아야 하는 문서 번호가 M이라고 하면, 이것이 0일때 이 문서가 출력 되는것이다. 따라서 이 값을 -- ++ 하면서 위치를 조정해준다.
  ```

- 프린터큐 작동 방법

  ```java
  Arrays.sort(priority);//우선순위를 정렬해준다.
  
  while (!list.isEmpty()) {
    int temp = list.poll();
    if (temp != priority[cur]) {//현재 우선순위와 큐에서 뽑은 것이 같지 않은 경우, 
      list.offer(temp); //다시 뒤에 넣어줘야한다.즉 이것은 뽑은 것이 현재 우선순위와 같을때까지 해준다.
      M = (M == 0) ? list.size() - 1 : --M;//내가 찾고자 하는 문서는 앞으로 한칸씩 이동할 것이다.
      //만약 0인경우 -1을 할경우 리스트의 맨 뒤로 오기 때문에 list.size() -1을 해준다.
    } else {//현재 우선순위와 큐에서 뽑은 것이 같은 경우
      cur--;//다음 우선순위 볼 것이므로(오름차순 정렬으로 --을 해준다.)
      answer++;//우선순위 높은 것이 출력된 것이므로 +1을 해준다.
      if (M == 0) {//내가 찾고자하는 위치가 0이라는 것은 내가 찾고자 하는것이 출력되는 것. 
        System.out.println(answer);//결과 출력하고 끝 !
        break;
      } else {
        M--;//아니면 위치 변경, 위에서 M != 0임이 확정이므로, 그냥 --해주면 된다.
      }
    }
  }
  ```

- 기억해야 할 것!!!!!!

  ```java
  M = (M == 0) ? list.size() - 1 : --M;
  //if else 가 한줄에 된다. 신기하다. 다음에 꼭 써먹어 보도록 ‼️
  ```

  



##### 느낀점

이 문제는 3번째 푸는데도 어려운 문제다....ㅋㅋㅋㅋ그래도 맨 처음 풀었을 때에 비해서는 더 효율적으로 짠거 같다. 그리고 두번째랑은 코드가 아주 유사하다. 심지어 그때가 더 나은거 같기도..난 왜이렇게 이문제가 어려운걸까..다음에 풀때도 어려울지 궁금하다 ㅎㅅㅎ 다음에 풀때는 한번에 풀어내고 싶다 . !



> 문제 : https://www.acmicpc.net/problem/1966
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/026b0406353454d35f3766b4f156b30c59e6f46d/Algorithm/1.BOJ/Q_1966_프린터큐_0203.java
