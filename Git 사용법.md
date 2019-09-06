# Git

> Git은 분산버전관리시스템(DVCS)이다.

- 참고자료
  - [Git 입문자](<https://backlog.com/git-tutorial/kr/>)
  - [Pro Git](https://git-scm.com/book/en/v2)

## 설정

####  1. author 설정

```bash
$ git config --global user.name {사용자 이름}
$ git config --global user.email {사용자 이메일}
```

- `commit` 시 이력을 남기는 작성자(author) 정보를 설정한다.

- 설정 정보를 확인하려면, 아래의 명령어를 입력한다.

  ```bash
  $ git config --global list2
  ```




## 기초 명령어(로컬 저장소 활용)

1. 로컬 저장소 초기화

   ```bash
   $ git init
   Initialized empty Git repository in C:/Users/hyooo/바탕 화면/test/.git/
   (master) $
   ```

   - `.git/` 폴더가 생성되며, git에 대한 모든 정보가 해당 폴더에 기록된다.
   - `bash` 에 `(master)` 라는 표기를 통해 현재 저장소로 활용되고 있음을 알 수 있다.
   -  `.gitignore` 파일을 생성하여 프로젝트에서 이력에 담지 않을 파일을 관리하자.
     - `__pycache__/`, `ipynb_checkpoints/` 등등..
     - [gitignore.io](https://www.gitignore.io/)를 통해 자동 생성도 가능하다.

2. `add`

   ```bash
   $ git add a.txt
   $ git add .
   $ git add my_folder/
   $ git status
   On branch master
   
   No commits yet
   
   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)
           new file:   a.txt
   ```

   - `working tree(directory)` : 현재 작업 공간
   - `INDEX(staging area)` : 커밋 대상 파일 공간
   - `add` 명령어를 통해서 `working tree` 에서 `staging area` 로 해당 파일 상태를 이동시킨다.

3. `commit`

   ```bash
   $ git commit -m {커밋메시지}
   [master (root-commit) 503fb9f] Add a.txt
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 a.txt
   ```

   - `commit` 을 통해서 현재 시점의 스냅샷을 찍는다. (이력을 남긴다.)

   - 현재까지 `commit` 이력을 확인하기 위해서는 아래의 명령어를 입력한다.

     ```bash
     $ git log
     commit 503fb9f68ac0f06e2e58ceae83347b5ac84a5166 (HEAD -> master)
     Author: hyooo1017 <hyooo1017@gmail.com>
     Date:   Thu Sep 5 09:33:34 2019 +0900
     
         Add a.txt
     ```

4. `status`

   ```bash
   $ git status
   ```

   - `CLI` 에서 항상 현재 상태를 확인하기 위해서 `status` 명령어를 입력해보자!!!





## 원격 저장소(remote repository) 활용

1. 원격 저장소 설정

   ```bash
   $ git remote add origin {원격저장소 url}
   ```

   - `origin` 이란 이름으로 `url` 을 설정한다.

2. 원격 저장소 확인

   ```bash
   $ git remote -v
   origin  https://github.com/hyooo1017/test.git (fetch)
   origin  https://github.com/hyooo1017/test.git (push)
   ```

3. 원격 저장소 `push`

   ```bash
   $ git push origin master
   Enumerating objects: 3, done.
   Counting objects: 100% (3/3), done.
   Delta compression using up to 8 threads
   Compressing objects: 100% (2/2), done.
   Writing objects: 100% (2/2), 230 bytes | 38.00 KiB/s, done.
   Total 2 (delta 0), reused 0 (delta 0)
   To https://github.com/hyooo1017/test.git
      503fb9f..030292d  master -> master
   ```

4. 원격 저장소 `pull`

   ```bash
   $ git pull origin master
   ```




### 멀캠 - 집 개인 활용 시나리오

1. 멀캠 도착했을 때,

   ```bash
   $ git pull origin master
   ```

2. 멀캠에서 작업 다하고, `push`

   ```bash
   $ git add .
   $ git commit -m {}
   $ git push origin master
   ```

3. 집에 도착해서,

   ```bash
   $ git pull origin master
   ```

4. 집에서 작업 다하고, `push`

   ```bash
   $ git add .
   $ git commit -m {}
   $ git push origin master
   ```

## 단순 되돌리기

1. unstaged : `add` 명령어의 반대 작업

   - 상황

     ```bash
     $ touch file.txt
     $ git add file.txt
     ```

   - `git status`

     ```bash
     $ git status
     On branch master
     Your branch is ahead of 'origin/master' by 2 commits.
     ```

   (use "git push" to publish your local commits)

   ```bash
   Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            deleted:    file1.txt
   ```

   - 취소

     ```bash
     $ git restore --staged file.txt # 1. 최근 command
     $ git reset HEAD file.txt # 2. 과거 command
     ```

2. 커밋 메시지 수정 : `commit` 의 반대 상황

   - 상황

     ```bash
     $ git commit -m 'Add file.txt'
     ```

   - 확인

     ```bash
     $ git log --online
     ```

   - 커밋 메시지 수정

     ```bash
     $ git commit --amend
     ```

     - vim 에디터로 커밋 메시지를 수정할 수 있게끔 해준다.
       - `i` : 편집 모드
       - esc + `:wq` : 저장 및 종료
         - `w` : write, `q` : quit

     - **중요!!** 커밋 메시지를 수정하면 커밋 해시값이 변경되어 전혀 다른 이력으로 기록된다. 따라서 원격 저장소에 올라간 이력은 절대 변경하지 않는다!

   - 추가 활용 (커밋 대상 파일을 누락했을 때)

     ```bash
     $ git add file1.txt
     $ git commit -m 'Add file1.txt file2.txt'
     $ git add file2.txt # 누락된 파일을 staging
     $ git commit --amend # 커밋 수정
     ```

3. `working directory` 변경사항 버리기

   - 상황

     file1.txt를 고양이가 지워버렸다.

   - `git status`

     ```bash
     $ git status
     On branch master
     Your branch is ahead of 'origin/master' by 2 commits.
       (use "git push" to publish your local commits)
     
     Changes not staged for commit:
       (use "git add <file>..." to update what will be committed)
       (use "git restore <file>..." to discard changes in working directory)
             modified:   a.txt
     ```

   - 해결

     ```bash
     $ git restore file1.txt # 1. 최근 command
     $ git  checkout --file1.txt # 과거 command
     ```

   - **주의!!** 커밋하지 않은 이력은 다시 되돌릴 수 없다. 즉, 이 명령어를 사용하면 다시 되돌릴 수는 없다!

4. 이력에서 특정 파일 제거하기

   로컬에 실제 파일이 삭제되지는 않지만, 삭제되는 커밋이 발생됨.

   따라서, `.gitignore` 와 같이 활용하면, 더이상 해당 파일이 관리되지 않도록 할 수 있음.

   예시) `.ipynb_checkpoints`, `__pycache__/`

   - `.gitignore` 등록 (VS code가 편리함)

   ```bash
   __pycache__
   ```

   - 명령어

     ```bash
     $ git rm --cached __pycache__
     $ git commit -m ''
     ```

     

