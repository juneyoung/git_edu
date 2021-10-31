# Week 2


## 00. 개요

- merge 를 이용하여 자동 병합 수행하기
- stash 로 임시 저장하기
- rebase 와 revert
 
둘째 주차에는 충돌을 확인하고 해결하는 방법에 대해 알아봅니다. 또 해시 간 이동을하며 작업된 내용을 재조정하는 방법을 알아봅니다.

## 01. 변경점 병합하기
작업 중 신규 릴리즈 버전이 배포된 경우, 작업 중인 내용에 원격 리파지토리의 변경 사항을 반영할 필요가 있습니다. 이런 경우 `merge` 명령어나 ` rebase`를 사용하여 해결할 수 있습니다.
##### 실습 준비
아래 내용대로 준비를 합니다.
```
$> git checkout -b branch_a
$> git checkout -b branch_b
$> git checkout -b merge_test

$> echo "merge" > week2/merge_file
$> git add -A .; git commit -m 'Add merge_file'

$> git checkout branch_a
$> echo "Content A" > week2/merge_file
$> git add -A .; git commit -m 'Add A to merge_file'

$> git checkout branch_b
$> echo "Content B" > week2/merge_file
$> git add -A .; git commit -m 'Add B to merge_file'
```
`merge_test`, `branch_a`, `branch_b` 브랜치에서 모두 동일한 merge_file 을 수정했다고 가정합니다.
### merge 사용하기
```
$> git checkout merge_test
$> git merge branch_a
Auto-merging week2/merge_file
CONFLICT (content): Merge conflict in week2/merge_file
Automatic merge failed; fix conflicts and then commit the result.
```
두 브랜치가 모두 동일한 파일을 변경하였으므로 conflict 가 났습니다. 파일을 열어 충돌 구문을 수정해 줍니다.
```
$> vim week2/merge_file
<<<<<<< HEAD
merge
=======
Content A
>>>>>>> branch_a
```
`=======` 를 기준으로 상단은 현재 브랜치, 하단은 병합하고자 하는 브랜치의 컨텐츠를 나타냅니다. 에디터에서 수정 후 빠져나갑니다.
`git status`를 해보면 아래 메세지가 제공됩니다.
```
$> git status
On branch merge_branch
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   week2/merge_file
```
가이드에서 제공된 것처럼 변경 파일을 `add` 후 `commit` 하면 conflict 해결입니다.

### rebase 사용하기
rebase 를 사용하면 커밋 트리를 좀 더 선형으로 유지할 수 있습니다.
```
$> git checkout branch_b
$> git rebase merge_branch
$> vim week2/merge_file
$> git add -A .
$> git rebase --continue
$> git push -f origin merge_test
```
rebase 가르키고 있는 HEAD 의 위치를 옮긴 후 현재의 변경 사항을 적용합니다. 

## 02. 변경 내용 임시 저장하기
git 은 `fast-forward` 를 사용하기 때문에 pull 이 우선합니다. 같은 파일이 수정된 경우, stash 를 이용하면, 원격의 변경 사항을 먼저 받고 로컬의 변경을 적용할 수 있씁니다.

```
$> git stash
$> git stash list
$> git stash apply <stash id> 혹은 $> git stash pop
4> git stash drop
```
##### 예시
원격 서버에 알 수 없는 변경이 있습니다. 해당 소스는 유지한 채로 원격 리파지토리의 변경만 가져올 수 있습니다.
```
$> git stash; git pull <remote name> <remote branch|tag>; git stash pop
```

## 03. 없었던 일로 하기
git reset 을 이용하면 내역을 모두 지워버리며 되돌릴 수 있습니다.
```
$> git reset --soft  # commit 만 취소
$> git reset --mixed  # default. commit, add 취소
$> git reset --hard  # commit, add, 변경도 모두 되돌림
```

## 04. 내역을 유지한 채로 취소하기
revert 를 사용하면 내역을 유지한 채로 과거로 돌아갈 수 있습니다.
##### 실습 준비
1,2,3,4 커밋이 존재한다고 가정합니다.
```
$> git checkout main
$> echo 1 > week2/1; git add -A .; git commit -m '1';
$> echo 2 > week2/2; git add -A .; git commit -m '2';
$> echo 3 > week2/3; git add -A .; git commit -m '3';
$> echo 4 > week2/4; git add -A .; git commit -m '4';
```
### revert 사용하기 
추가한 3번 파일(커밋 해시 `575f03d`)을 제외하겠습니다.
```
$> git revert 575f03d
$> git push origin main
```
`git log` 로 확인시 아래와 같이 내역이 남으며 revert 커밋 히스토리가 남은 것을 볼 수 있습니다.
```
...
commit 8b8dde89f9fa61558a83c04df34bd0949a6754da
Author: 오준영 <jy.oh@widerplanet.com>
Date:   Sun Oct 31 22:35:19 2021 +0900

    Revert "3"
    
    This reverts commit 575f03dd0634665b56745ddb947694ca117e4dce.
...
```
#### cherry-pick 으로 revert 되돌리기
```
$> git cherry-pick 575f03d
```

