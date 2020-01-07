---

title:  "[Study]NVM & Node 설치 기록"

excerpt: "#NVM #node #node.js"



categories:

- Study

tags:

- Study

- NVM

- node.js

author_profile: true

read_time: false 

toc: true #Table Of Contents 목차 보여줌

toc_label: "Contents" # toc 이름 정의

toc_sticky: true # 스크롤 내릴때 같이 내려가는 목차

---



[React Native로 날씨앱 만들기]를 하기에 앞서 먼저 개발 환경을 설치하고자 한다.

### 📌 설치할 것

- **Node.js** -> 10이상

- **npn**(Node Version Manager) -> 6이상

- **시뮬레이터** -> 안드로이드 스튜디오, xcode

- iOS, Android 앱 **엑스포** **폰**에 설치

  https://apps.apple.com/kr/app/expo-client/id982107779

  👉 iOS, 안드로이드 앱 expo가 테스트 할 수 있는 유일한 방법(휴대폰에서)

- ‼️설치 완료 후

  ```
  npm install -g expo-cli
  ```

  

### NPM설치

🧐참고로 NPM은 Node Version Manager의 줄임말 !

1. 설치

   ```
   $ sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
   ```

2. 확인

   ```
   $ nvm ls
   
   -bash: nvm: command not found //이렇게 뜬다.
   ```

3. 터미널을 종료했다가 다시 켠다.

4. 확인

   ```
   $ vi ~/.bash_profile
   ```

   ```
   export NVM_DIR="$HOME/.nvm"
   [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
   //이 코드가 있는지 확인 한다.
   ```

5. 다시 종료했다가 켠다.(종료 했다 켜는건 만병통치)

   ```
   source ~/.bash_profile
   ```

6. 설치 확인 

   ```
   $ nvm ls
   ```

   ```
   //결과
               N/A
   node -> stable (-> N/A) (default)
   iojs -> N/A (default)
   ```



### Node 설치

1. 설치

   ```
   $ nvm install 6.10.1
   //가장 안정화된 버전이라고 한다.
   ```

2. 확인

   ```
   $ nvm ls
   ```

   ```
   ->      v6.10.1
   default -> 6.10.1 (-> v6.10.1)
   node -> stable (-> v6.10.1) (default)
   stable -> 6.10 (-> v6.10.1) (default)
   iojs -> N/A (default)
   lts/* -> lts/erbium (-> N/A)
   lts/argon -> v4.9.1 (-> N/A)
   lts/boron -> v6.17.1 (-> N/A)
   lts/carbon -> v8.17.0 (-> N/A)
   lts/dubnium -> v10.18.0 (-> N/A)
   lts/erbium -> v12.14.0 (-> N/A)
   ```

3. 노드 버전 확인

   ```
   $ node -v
   ```

   ```
   v6.10.1
   ```



### 다른 Node 버전 설치

1. 설치

   ```
   $ nvm install 12.1.0
   ```

   니꼴라스 따라서 같은 버전 설치함

2. 확인

   ```
   👉 node -v
   v12.1.0
   👉 npm -v
   6.9.0
   ```

3. 터미널 시작시 노드 기본 버전설정

   ```
   nvm alias default v12.14.0
   
   //결과
   default -> v12.14.0
   ```

   

