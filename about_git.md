what is a Git?
============

## Git의 세 가지 상태
1. **Committed** : 데이터가 *로컬 데이터베이스* (Git 디렉토리)에 저장되어 있음을 의미한다.

2. **Modified** : 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 상태이다.

3. **Staged** : 현재 수정한 파일을 커밋하기 위해 Staging Area에 올려 둔 상태이다.
<br/><br/>

![Alt text](./image.png)
<br/><br/><br/><br/>

## 1. 시작하기
### 사용자 정보 설정
- 사용자 정보는 Git을 설치한 뒤 한 번만 설정하면 된다.
```
$ git config --global user.name "username"
$ git config --global user.email id@example.com
```
- 설정 확인은 ```git comfig --list```로 하면 된다.
<br/><br/><br/><br/>

## 2. Git의 기초
### Git 저장소 만들기
- 아직 버전 관리를 하지 않는 로컬 디렉토리 하나를 Git 저장소에 연동하는 방법
  
    1. 원하는 디렉토리로 이동하여 다음 명령어를 실행한다.
       ```
       $ git init (하위 디렉토리인 .git을 만드는 명령)
       ```
    2. ```git add```와 ```git commit``` 명령으로 파일을 추가 및 커밋한다.
       ```
       $ git add *.c
       $ git add LICENCE
       $ git commit -m 'initial project version'
       ```
- Git 저장소를 Clone하는 방법
  
    - ```git clone``` 명령어 이용
      ```
      $ git clone <url> <directory_name>
      ```

### 수정 및 저장소에 저장
- **Tracked file** vs **Untracked file**
  
  Tracked : 이미 스냅샷에 포함되어 있던 파일, Unmodified와 Modified로 나뉨
  
  Unmodified : 수정하지 않은 파일

  Modified : 수정한 파일

  ![파일의 라이프사이클](./image-1.png)

### 파일 상태 확인하기
```
$ git status
(실행 메시지)
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

### 커밋 히스토리 조회하기
- ```git log```를 이용하여 히스토리를 조회한다.
  - 옵션 (더 많은 옵션은 [여기](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0)에서 확인)

  1. **-p, --patch** : 각 커밋의 diff 결과를 보여준다.
  2. **-2** : 최근 두 개의 결과만 보여준다.
  3. **--stat** : 각 커밋의 통계 정보를 보여준다.
  4. **--pretty=oneline** : 기본 형식 외에 다른 형식으로 보여준다.
   
         oneline , short , full , fuller 등등 옵션이 있다.

  5. **--pretty=format** : 원하는 포맷으로 결과를 출력한다.
     ```
     $ git log --pretty=format:"%h - %an, %ar : %s"
     ca82a6d - Scott Chacon, 6 years ago : changed the version number
     085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
     a11bef0 - Scott Chacon, 6 years ago : first commit
     ```
  6. **--since=2.weeks** : 지난 (2주) 동안 이루어진 커밋만 조회한다.

### 되돌리기
- 완료한 커밋을 수정해야 할 때 :
  파일을 수정하고 Staging Area에 추가한 뒤 ```--amend``` 옵션을 이용한다.
  ```
  $ git commit --amend
  ```
- Stage 하는 것을 깜빡하여 일부 파일을 빠뜨리고 커밋한 경우 :
  ```
  $ git commit -m 'initial commit'
  $ git add forgotten_file
  $ git commit --amend
  -> 하나의 커밋으로 기록됨
  ```
- Staging Area 내부의 파일을 Unstage로 변경하고 싶을 때 :
  ```git reset``` 명령어를 이용한다.
  ```
  $ git reset mistake_file
  ```
- Modified 파일을 최근 커밋된 버전으로 되돌리고 싶을 때 :
  ```
  $ git checkout -- <file_name>
  ```

### 리모트 저장소
: 네트워크 어딘가에 존재하는 저장소
(cf. 로컬 시스템에 위치할 수도 있음)

- **리모트 저장소 확인하기**
  - ```git remote```를 이용해 확인한다.
  - 저장소를 clone한 경우 origin 이라는 저장소가 자동으로 등록되므로 origin이 보인다.
    ```
    $ git remote
    origin
    ```
  - ```git remote show <remote_name>``` 명령을 이용하면 특정 리모트 저장소의 구체적인 정보를 확인할 수 있다.
- **리모트 저장소 추가하기**
  ```
  $ git remote add <remote_name> <url>
  ```
- **리모트 저장소에서 데이터 가져오기**
  
  1. ```git fetch <remote_name>``` : 로컬에는 없지만, 리모트 저장소에는 있는 모든 데이터를 가져온다. (Merge는 되지 않음)
  2. ```git pull``` : 리모트 저장소에 있는 데이터를 모두 가져와서 Merge한다.
- **리모트 저장소에 push 하기**
  ```
  $ git push <remote_name> <branch_name>
  ```
- **리모트 저장소 이름 변경 및 삭제**
  - **이름 변경**
    ```
    $ git remote rename <remote_name> <new_name>
    ```
  - **삭제**
    ```
    $ git remote remove <remote_name>
    ```