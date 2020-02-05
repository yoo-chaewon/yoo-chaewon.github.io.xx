---

title:  "[ALGORITHM]PROGRAMMERS_DFS/BFS_타겟넘버"

excerpt: "#PROGRAMMERS #DFS"



categories:

- ALGORITHM

tags:

- ALGORITHM

- PROGRAMMERS

- DFS

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

##### 입출력 예

| numbers         | target | return |
| --------------- | ------ | ------ |
| [1, 1, 1, 1, 1] | 3      | 5      |

##### 

## 나의 코드

```java
class Solution {
    int answer = 0;
    int mtarget = 0;

    public int solution(int[] numbers, int target) {
        mtarget = target;
        DFS(0, 0, numbers, 0);

        return answer;
    }

    public void DFS(int start, int depth, int[] numbers, int result) {
        if (depth == numbers.length) {
            if (result == mtarget) {
                answer++;
            }
        }

        for (int i = start; i < numbers.length; i++) {
            DFS(i + 1, depth + 1, numbers, result + numbers[i]);
            DFS(i + 1, depth + 1, numbers, result - numbers[i]);
        }
    }
}
```

- 나는 number들을 맨 앞에서 부터 보면서 + 와 -를 다해봤다.

- 그리고 그 값이 target과 같으면 answer++를 해주었다.

- 모든 경우의 수를 다 본다.

  ```java
  public void DFS(int start, int depth, int[] numbers, int result) {
    if (depth == numbers.length) {
      if (result == mtarget) {
        answer++;
      }
    }
  
    for (int i = start; i < numbers.length; i++) {
      DFS(i + 1, depth + 1, numbers, result + numbers[i]);//+
      DFS(i + 1, depth + 1, numbers, result - numbers[i]);//-
    }
  }
  ```

  즉, 12가 있으면

  ```
  -1  --> +2 / -2 
  +1  --> +2 / -2
  ```

  이렇게 해준다. !

  모든 순열을 보는 것 이 아니라 visited도 필요없는 경우다 !



##### 느낀점

> 문제 : https://programmers.co.kr/learn/courses/30/lessons/43165#
>
> 저장소 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/DFS:BFS_타겟넘버.java

