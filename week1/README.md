# Week 1


## 00. 개요
 
첫주차에는 공간에 대해 배워보고 add/commit/push 로 공간 간 컨트롤을 자유롭게 익혀봅니다. 또 reset 을 이용하여 이미 발생한 행위에 대한 취소 방법도 익혀봅니다.

## 01. git 용어 정의
### 공간

![space](../src/stage_area.png)

상단에 있는 이미지는 git 의 공간 중 사용자 머신에 있는 3 가지 공간들에 대해 보여줍니다. 
- working directory: 사용자가 변경을 만들어내는 곳입니다.
- staging area: index 라고도 부르며(옵션에서 사용되기 때문에 중요함) 사용자가 add 를 했을 때 업데이트가 일어나는 공간입니다.
- local repository: 사용자가 commit 을 하면 변경이 일어나는 공간으로 원격에 올리기 전의 공간입니다.
- remote repository: 최종적으로 여러 사용자의 소스가 통합 관리되는 원격 공간입니다. 

### 파일의 상태

![file_lifecycle](../src/lifecycle.png)

- untracked: git 에서 관리하는 파일이 아님
- unmodified: 변경되지 않았음
- modified: 파일에 변경이 일어남 
- staged: git 이 파일의 생성/변경을 감지하고 commit 의 대상으로 인식함

## 02. 활성화와 원격 리파지토리 관리

### git 프로젝트 활성화 하기
clone 을 통해 원격 리파지토리를 가져오거나 init 명령어로 로컬에 환경 새팅을 할 수 있습니다.
```
$> git clone <원격 저장소>
$> git init
```
생성된 디렉토리의 하위에는 `.git` 디렉토리가 생성됩니다. 해당 디렉토리는 git 프로젝트의 전반적인 뼈대(Skeleton) 파일 정보를 가지고 있으며, 해당 디렉토리가 존재하여야만 git 프로젝트로 인식 됩니다.

### git remote 명령으로 원격 리파지토리 CRUD 하기
git 프로젝트는 복수의 원격 repository 를 허용합니다. 해당 기능을 활용하면 아직 머지되지 않은 동료의 소스를 로컬에서 돌려본다던지 하는 경우에 활용이 가능합니다.
```
$> git remote -v
$> git remote add upstream https://github.com/juneyoung/git_edu.git
$> git remote set-url upstream https://github.com/juneyoung/git_edu.git
$> git remote rename upstream upstream1
$> git remote remove upstream1
```
- `-v`: 해당 옵션은 원격 리파지토리의 list 를 보여줍니다.
- add/remove: 해당 명령은 원격 리파지토리의 `이름` 으로 생성과 삭제를 지원합니다.
- rename/set-url: 해당 명령은 원격 리파지토리 정보에 대해 수정을 수행합니다.

##### 예시
```
$> git diff origin/main # origin 원격 저장소의 main 브랜치와 비교합니다
$> git diff upstream/main # upstream 원격 저장소의 main 브랜치와 비교합니다
```
##### Trend
기존에는 git 을 생성하면 기본 branch 가 `master` 였는데, 현재는 `main` 으로 변경되었습니다.
## 03. 원격 정보 가져오기
git 은 기본적으로 `fast-forward` 정책을 사용하기 때문에 자신의 변경 사항을 원격에 반영하려면 원격의 정보를 먼저 동기화한 다음 수행해야 합니다.
### fetch 로 원격 정보 가져오기
fetch 로 원격 리파지토리의 정보만 가져옵니다. 
```
$> git fetch upstream
```
### pull 로 원격 정보 동기화 하기
pull 로 원격 리파지토리와 브랜치를 동기화합니다. 파일의 변경이 로컬에 반영됩니다.
```
$> git pull upstream main
```

## 04. 로컬 변경 사항 등록하기
### branch 만들어보기 
```
$> git branch
* main
$> git checkout -b week1
$> git checkout main
$> git branch -d week1
```
- no option: 현재 프로젝트가 가지고 있는 branch 정보를 리스트로 보여줍니다(*는 현재 위치)
- `-b`: 현재 위치에서 브랜치를 생성합니다.
- checkout: 브랜치를 다시 main 으로 이동합니다.
- `-d`: 브랜치를 삭제합니다.

### 파일 등록/변경하기
```
$> echo "test" > week1/sample
$> git add week1/sample
```
새로 파일을 추가하고 staging area 에 등록했다
- add: 신규로 파일을 staging area 등록한다
- `-i`: prompt 로 편리성을 향상시키는 옵션이다

#### 등록/변경의 취소
```
$> git status 
$> git restore --staged .
$> git reset --mixed HEAD^1
```
`reset` 혹은 `restore` 명령을 이용해서 stage 에 추가한 파일을 되돌릴 수 있다.
- status: 현재 디렉토리의 변경 상태를 보여줍니다.
- reset 의 `--mixed`: 명시하지 않았을 경우 사용되는 기본 모드이며 commit 은 되돌리지만 로컬의 작업 내용은 되돌리지 않는다.
- reset 의 `HEAD^1`: 현재 해시의 부모를 가르키고 명시하지 않는 경우 디폴트이다. `HEAD^2`와 같이 조부를 가르킬 수도 있다.

### 변경 내역을 등록하기
commit 명령어를 사용해서 staged 애 있는 목록을 로컬 리파지토리에 등록할 수 있습니다.
```
$> echo "test2" >> week1/sample # 파일을 변경해봅니다.
$> git diff
$> git add -A .
$> git diff --staged
$> git commit
```
- commit: 명령을 수행하면 commit message 를 작성하도록 에디터가 제공됩니다.
- commit `-m`: 옵션은 사용하면 에디터 없이 바로 메세지 작성이 가능합니다.
- diff: 변경된 파일의 내용을 보여줍니다.
- diff `--staged`: stage 된 파일의 변경 내역을 보여줍니다.
#### 로컬 리파지토리 등록 취소
reset 명령어를 활용하여 등록된 내용을 되돌릴 수 있습니다.
```
$> git reset --soft
```
- `--soft`:  `--mixed` 와 달리 커밋만 취소합니다.

## 05. 원격에 반영하기
push 명령어를 이용하여 원격에 반영합니다.
```
$> git push origin main
```
- `-f`: 포스 푸시합니다. 무조건 현재의 작업 내용으로 원격 리파지토리를 덮어씁니다.
- `--tags`: 현재 로컬에 생성된 태그를 원격 리파지토리에 반영합니다.


