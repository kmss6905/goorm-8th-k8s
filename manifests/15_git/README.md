# Git

https://git-scm.com  
https://rogerdudler.github.io/git-guide/index.ko.html

## 1. Git 설치 및 설정

https://git-scm.com/downloads

### 1) macOS

```bash
brew install git
```

### 2) Windows

```powershell
choco install git
```

```powershell
winget install --id Git.Git -e --source winget
```

### 3) Linux

```bash
apt-get install git
```

```bash
yum install git
```

## 2. Git 기초: Add, Commit

저장소

작업의 흐름:

- Work on stuff: 파일 생성, 수정, 삭제 등
- Add Change: 변경 사항 추가
- Commit: 추가한 변경사항 확정

### 1) 저장소 초기화

```bash
git init
```

### 2) 저장소 상태 확인

```bash
git status
```

### 3) 추가

```bash
git add <files>
```

```bash
git add .
```

### 4) 커밋

```bash
git commit
```

```bash
git commit -m "<message>"
```

의미 있는 단위로 추가하고 커밋
현재시제로 의미있는 메시지 작성

### 5) 추가 및 커밋

```bash
git commit -a -m "<message>"
```

새로운 파일은 추가되지 않음, 기존에 추적되고 있어야 함

### 6) 로그

```bash
git log
```

```bash
git log --oneline
```

## 3. Commit

커밋 수정(Amending)

```bash
git commit -m '<message>'
git add forgotten_file
git commit --amend
```

### 1) Ignoring 파일

`.gitignore` 파일: 추가 및 커밋하지 않기 위한 파일

- 시크릿, API 키, 자격증명
- 운영체제 시스템 파일: `.DS_Store`
- 로그
- 의존성 패키지

## 4. Branch

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/Git-branching-model.pdf/page1-1239px-Git-branching-model.pdf.jpg)

Master vs. Main

HEAD: 지금 바라보고 있는 커밋(책갈피)

### 1) 브랜치 확인

```bash
git branch
```

### 2) 브랜치 생성

```bash
git branch <branch-name>
```

### 3) 브랜치 이동

```bash
git checkout <branch-name>
```

```bash
git switch <branch-name>
```

> `checkout`은 너무 많은 기능을 가지고 있음  
> `switch`는 `checkout`의 브랜치 이동 기능을 별도로 구현

### 4) 브랜치 생성 및 이동

```bash
git checkout -b <branch-name>
```

```bash
git switch -c <branch-name>
```

### 5) 브랜치 상세 정보 확인

```bash
git branch -v
```

## 5. Merge

특정 커밋 합치기가 아니라 브랜치 전체 병합
합치길 원하는 브랜치로 이동 후 병합

```bash
git switch master
git merge bugfix
```

병합 종류:

- Fast-Forward
- 3-way

### 1) Fast-Forward

빨리 감기, 브랜치가 분기한 시점에 합치길 원하는 브랜치에 추가 커밋이 없는경우, 단순히 따라 잡으면 됨

![](https://git-scm.com/book/en/v2/images/basic-branching-3.png)

### 2) 3-way 병합

![](https://git-scm.com/book/en/v2/images/basic-merging-2.png)

### 3) Conflict(충돌)

3-way 병합은 실패할 때도 있음: 각 브랜치의 같은 파일의 같은 부분을 서로 다르게 수정한 경우

해결 방법:

1. 병합에 출동난 파일 열기
2. 파일 수정/충돌 마커 제거
3. 추가 및 커밋

### 4) 브랜치 삭제

```bash
git branch -d <branch-name>
```

## 6. Diff

### 1) 마지막 커밋과 현재 작업 디렉토리 비교


```bash
git diff
```

```bash
git diff HEAD
```

결과

```bash
diff --git a/hello b/hello
index ce01362..e00f89a 100644
--- a/hello
+++ b/hello
@@ -1 +1 @@
-hello
+hello wolrd
```

### 2) 마지막 커밋과 스테이징 비교

```bash
git diff --staged
```

```bash
git diff --cached
```

### 3) 특정 파일만 비교

파일을 지정하지 않으면 모든 파일에 대해 비교

```bash
git diff HEAD <filename>
```

```bash
git diff --staged <fileanem>
```

### 4) 브랜치 비교

```bash
git diff branchA..branchB
```

### 5) 커밋 비교

```bash
git diff commitA..commitB
```

## 7. 시간 여행 및 변경사항 취소

### 1) Switch

#### (1) 이전 커밋 이동

```bash
git checkout <commit-hash>
```

```bash
git switch --detach <commit-hash>
```

```bash
git switch --detach HEAD~<number>
```

- HEAD~1: HEAD의 한번 이전 커밋(부모)
- HEAD~2: HEAD의 두번 이전 커밋(조부모)

#### (2) 변경사항 취소

```bash
git checkout <commit-hash> <filename>
```

```bash
git switch --source <commit-hash> <filename>
```

### 2) Restore

#### (1) 최근 커밋으로 취소/변경

```bash
git checkout HEAD hello.txt
```

```bash
git checkout -- hello.txt
```

```bash
git restore hello.txt
```

#### (2) 특정 커밋의 파일로 변경

```bash
git checkout HEAD~1 hello.txt
```

```bash
git restore --source HEAD~1 hello.txt
```

#### (3) 스테이징 취소

```bash
git restore --staged <filename>
```

> `git status`에서도 상태 변경에 대한 예를 확인 할 수 있음

### 3) Reset

#### (1) 커밋 삭제

```bash
git reset <commit-hash>
```

커밋은 삭제되었지만 작업 디렉토리의 파일은 (변경된) 그대로임

```bash
git reset --hard <commit-hash>
```

`--hard`: 커밋 삭제 및 작업 디렉토리의 파일도 변경전으로 되돌림

### 4) Revert

```bash
git revert <commit-hash>
```

- `git reset`: 특정 위치로 되돌림, 커밋 삭제됨
- `git revert`: 특정 상태로 되돌아 가지만, 기존 커밋은 그대로, 되돌아 가기위한 새로운 커밋 생성

## 8. Github 기초

### 1) Clone

```bash
git clone <url>
```

### 2) Remote

#### (1) 원격 확인

```bash
git remote
```

```bash
git remote -v
```

#### (2) 원격 추가

```bash
git remote add <name> <url>
```

> 원격 이름은 관습적으로 `origin` 사용

### 3) Push

#### (1) 원격 푸시

```bash
git push <remote> <branch>
```

[Github - SSH Key](https://docs.github.com/ko/authentication/connecting-to-github-with-ssh)

#### (2) 원격 업스트림 설정

업스트림 설정

```bash
git push -u <remote> <branch>
```

다음 부턴 원격 및 브랜치 설정 하지 않아도 됨

```bash
git push
```

## 9. Fetch, Pull

### 1) 원격 저장소

원격 저장소의 추적하는 브랜치: `<remote>/<branch>`, `origin/main`

원격 브랜치 확인

```bash
git branch -r
```

원격 브랜치 이동

```bash
git switch --detach <remote>/<branch>
```

### 2) Fetch

원격 저장소의 변경사항을 로컬 저장소 까지만 반영함
추후 병합 가능

```bash
git fetch <remote>
```

```bash
git fetch <remote> <branch>
```

### 3) Pull

git pull = git fetch + git merge

원격 저장소의 변경사항을 로컬 저장소 및 작업 디렉토리 까지 병합

```bash
git pull <remote> <branch>
```

```bash
git pull
```

> 병합 시 충돌 날 수 있음

