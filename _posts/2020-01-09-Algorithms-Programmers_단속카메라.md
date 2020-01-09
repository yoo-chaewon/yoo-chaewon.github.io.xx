---

title:  "[ALGORITHM]Programmers-단속카메라"

excerpt: "#Programmers #Algorithm #Greedy"



categories:

- ALGORITHM

tags:

- ALGORITHM

- Greedy

- Programmers

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



## 문제

 고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.

고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요.

**제한사항**

- 차량의 대수는 1대 이상 10,000대 이하입니다.
- routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
- 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
- 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

**입출력 예**

| routes                                   | return |
| ---------------------------------------- | ------ |
| [[-20,15], [-14,-5], [-18,-13], [-5,-3]] | 2      |

**입출력 예 설명**

-5 지점에 카메라를 설치하면 두 번째, 네 번째 차량이 카메라를 만납니다.

-15 지점에 카메라를 설치하면 첫 번째, 세 번째 차량이 카메라를 만납니다.

## 나의 코드

### ver1

```java
class Solution {
    public static int solution(int[][] routes) {
        int answer = 1;

        Arrays.sort(routes, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                Integer route1 = o1[0];
                Integer route2 = o2[0];
                return route1.compareTo(route2);
            }
        });

        int start = routes[0][0];
        int end = routes[0][1];
        for (int i = 1; i < routes.length; i++){
            if (!(start <= routes[i][0] && routes[i][0] <= end)){
                answer++;
                start = routes[i][0];
                end = routes[i][1];
            }else{
                start = Math.max(start, routes[i][0]);
                end = Math.min(end, routes[i][1]);
            }
        }
        return answer;
    }
}
```

- start와 end범위를 새로 만들어가면서 그 범위안에 들어가지 않으면 anwer++를 해주었다. 
- 이건 한 두세달 전에 짠거라서..아무튼 start를 기준으로 정렬해주었으니 start가 필요있나 싶었다. 그래서 이번에는 start없이 다시 짜보았다.



### ver2

```java
import java.util.*;
class Solution {
    public int solution(int[][] routes) {
        int answer = 1;
        Arrays.sort(routes, (a,b) -> Integer.compare(a[0], b[0]));

        int end = routes[0][1];
        for (int i = 0; i < routes.length-1; i++){
            end = Math.min(end, routes[i][1]);
            if (end < routes[i+1][0]){
                answer++;
                end = routes[i+1][1];
            }
        }
        return answer;
    }
}
```

- 어차피 start로 정렬을 해 주었으니 이전 start보다는 값이 크기 때문에 이전 값에 포함이 될 것이다.

- 따라서 **end만 고려**하였다.

- **end의 값을 현재와 이전 것 중 가장 작은 것으로 저장**한다.

  - 그 이유는, 단속카메라를 중복으로 설치하지 않기 위함이다. 

    - 예를들어  [-18, 5] [-18, -15] [-15 -10] [-10 -5] [-5 5]가 있으면, 

       [-18, 5] [-18, -15] 📷 [-15 -10] [-10 -5] [-5 5]

      저기에 카메라를 설치하면 

      [-18, 5] [-18, -15] [-15 -10] 📷 [-10 -5] [-5 5] 

      여기에 설치하면 중복이 된다.

       [-18, 5] [-18, -15]📷 [-15 -10] [-10 -5] 📷 [-5 5] 이게 더 적게 설치하는 방법일 것이다.

      만약, 항상 end = routes[i][1]로 한다면, 아마 전자에 설치되고 후자에도 설치가 된다.

      즉, **카메라가 볼 수 있는 마지막 영역이라고 생각하면 될 것 같다 . ❗️**

  - end보다 다음 것이 클 경우 answer+1을 해주고, 

  - end를 그 다음 것의 end로 업데이트 해준다. 👉 **이것은 카메라가 볼 수 있는 마지막 영역** ‼️

  

## 느낀점

뭔가 더 좋은 코드를 짤 수 있을거 같다. routes[i+1][0] 이런식으로 짜는게 좀 찜찜하다ㅠㅠ 친구들과 이야기 하면서 더 좋은 방법을 생각해서 다시 짜봐야 겠다..

머리속에서 정확히 이해되지않는 문제다..약 70%이해한것 같다....꼭 100퍼센트로 이해하고 싶다.

그리고 sort를 더 간단하게 할 수 있는 것을 발견 하였다. 꼭 기억해서 다음에는 이걸로 풀어볼 것이다.

```java
Arrays.sort(routes, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0]-o2[0];
            }
        });
//‼️‼️‼️‼️이거 기억하기‼️‼️‼️‼️‼️‼️
Arrays.sort(routes, (a,b) -> Integer.compare(a[0], b[0]));
```

- 문제 : https://programmers.co.kr/learn/courses/30/lessons/42884
- 저장소1 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/Greedy_단속카메라_ver2.java
- 저장소2 : https://github.com/yoo-chaewon/HELLO_JAVA/blob/master/Algorithm/2.PROGRAMMERS/Greedy_단속카메라_ver2.java