---

title:  "[ALGORITHM]BOJ1700-멀티탭 스케줄링"

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

기숙사에서 살고 있는 준규는 한 개의 멀티탭을 이용하고 있다. 준규는 키보드, 헤어드라이기, 핸드폰 충전기, 디지털 카메라 충전기 등 여러 개의 전기용품을 사용하면서 어쩔 수 없이 각종 전기용품의 플러그를 뺐다 꽂았다 하는 불편함을 겪고 있다. 그래서 준규는 자신의 생활 패턴을 분석하여, 자기가 사용하고 있는 전기용품의 사용순서를 알아내었고, 이를 기반으로 플러그를 빼는 횟수를 최소화하는 방법을 고안하여 보다 쾌적한 생활환경을 만들려고 한다.

예를 들어 3 구(구멍이 세 개 달린) 멀티탭을 쓸 때, 전기용품의 사용 순서가 아래와 같이 주어진다면, 

1. 키보드
2. 헤어드라이기
3. 핸드폰 충전기
4. 디지털 카메라 충전기
5. 키보드
6. 헤어드라이기

키보드, 헤어드라이기, 핸드폰 충전기의 플러그를 순서대로 멀티탭에 꽂은 다음 디지털 카메라 충전기 플러그를 꽂기 전에 핸드폰충전기 플러그를 빼는 것이 최적일 것이므로 플러그는 한 번만 빼면 된다. 

### 입력

첫 줄에는 멀티탭 구멍의 개수 N (1 ≤ N ≤ 100)과 전기 용품의 총 사용횟수 K (1 ≤ K ≤ 100)가 정수로 주어진다. 두 번째 줄에는 전기용품의 이름이 K 이하의 자연수로 사용 순서대로 주어진다. 각 줄의 모든 정수 사이는 공백문자로 구분되어 있다. 

### 출력

하나씩 플러그를 빼는 최소의 횟수를 출력하시오. 



### 예제 입력 1 

```
2 7
2 3 2 3 1 2 7
```

### 예제 출력 1

```
2
```



## 나의 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        String[] input = bufferedReader.readLine().split(" ");
        int N = Integer.parseInt(input[0]);//멀티탭구멍수
        int K = Integer.parseInt(input[1]);//사용횟수
        int[] arr = new int[K];
        ArrayList<Integer> arrayList = new ArrayList<>();
        HashSet<Integer> multi = new HashSet<>();
        int answer = 0;

        String[] str = bufferedReader.readLine().split(" ");
        for (int i = 0; i < K; i++) {
            arr[i] = Integer.parseInt(str[i]);
            arrayList.add(Integer.parseInt(str[i]));
        }

        for (int i = 0; i < K; i++) {
            if (multi.contains(arr[i]) || multi.size() < N) {
                multi.add(arr[i]);
            } else {
                answer++;
                int index = Integer.MIN_VALUE;

                for (int cur : multi) {
                    if (arrayList.subList(i + 1, K).contains(cur)) {
                        int position = arrayList.subList(i + 1, K).indexOf(cur) + i + 1;
                        index = Math.max(position, index);
                    } else {
                        multi.remove(cur);
                        break;
                    }
                }
                if (multi.size() == N) {
                    multi.remove(arr[index]);
                }
                multi.add(arr[i]);
            }
        }
        System.out.println(answer);
    }
}

```

- 먼저 multi라는 hashSet을 만들어 주었다.

  👉 hashSet은 key값의 중복을 막고, key가 존재하는지 안하는지 판단하기 때문에 이 자료형을 사용하였다.

  만약, multi라는 hash에 값이 없거나, 꽉 채워지지 않았다면 현재 스케줄을 넣는다.

  ```java
  if (multi.contains(arr[i]) || multi.size() < N) {
    multi.add(arr[i]);//존재 하더라고 add를 해도 중복되지 않기 때문에 그냥 add해줬다.
  } 
  ```

- 만약 multi가 꽉찼을 경우, 어떤 한개를 빼고 새로운 것을 넣어야 한다.

  - 여기서 어떤 것을 뺄지 중요하다.

    👉 그리디하게 뺀다면 앞으로 나오지 않는 것이나 **or** 나올려면 가장 먼것을 빼는 것이 맞다 ‼️

- 그래서 multi에 있는 키를 가져와 앞으로 나오지 않는 것이나, 가장 먼 것을 빼주었다.

  ```java
  for (int cur : multi) {
    if (arrayList.subList(i + 1, K).contains(cur)) {//1. 뒤에 배열중 해당 키 값이 존재할 때
      int position = arrayList.subList(i + 1, K).indexOf(cur) + i + 1;
      //arr에서의 위치를 찾는다. 자른 배열이기 때문에 현재 위치를 더해줘야지 해당 위치가 제대로 나온다.
      index = Math.max(position, index);
      //존재하는 것중 가장 먼 것을 저장한다.
    } else {// 2. 뒤에 배열중 해당 키값이 존재하지 않을 때 -> 그것을 hash에서 빼주면 된다.
      multi.remove(cur);//해당 것 제거
      break;
    }
  }
  //2의 경우를 통과했을 시 multi는 1개가 줄었을 것이니 이 과정을 생략하기 위해서 if 문 사용했다.
  if (multi.size() == N) {
    multi.remove(arr[index]);
  }
  multi.add(arr[i]);
  ```

  👉 이때 ArrayList의 **subList**로 뒤에 배열 중에서 **comtains(key)** 를 사용하여서 해당 키값들이 존재하는 곳들중 가장 먼 위치를 저장한다.



## 느낀점

진짜 집중해서 한시간동안 풀었던거 같다. 처음에는 multi라는 것을 배열로 생각하다 보니 하나하나 검사하는게 어려웠고 **hash**를 떠올리게 되었다. 그리고 뒤에 배열중 아이템이 어디 인덱스에 있는지 찾는 것을 배열에서 하나하나 검사하려다가 **subList**로 잘라서 **IndexOf**를 사용해서 찾았다. 이 두개를 사용하지 않았으면 어떻게든 짰을 수 있겠지만 어려웠을 거 같다 ‼️ 아무튼 이거 정말 내 기준 고난도 문제였다.

- 문제 : https://www.acmicpc.net/problem/1700
- 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/1.BOJ/Q_1700_멀티탭스케줄링.java