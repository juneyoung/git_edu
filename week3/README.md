# Week 3


## 00. 개요
 
마지막 주차에는 훅을 이용한 커밋 메세지 자동화, 커밋 단장, tag 와 버전 관리에 대해 알아봅니다.

## 01. hooks
git 은 라이프 사이클 훅을 이용해 다양한 옵션을 제공하고 있습니다. 여기에서는 prepare-commit-msg 훅을 이용해 자동으로 prefix 를 다는 방법에 대해 알아봅니다.

[더 많은 hooks 보러가기](https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-Hooks)
### prepare-commit-msg 훅 작성하기
이 훅은 커밋 에디터가 작성되기 전에 사용됩니다. 현재 팀에서는 `<이슈>: <커밋 메세지>`와 같은 형식으로 남기고 있으므로 자동으로 이슈를 추가하도록 만들어 봅니다.
```
$> cd ./git/hooks
$> mv prepare-commit-msg.sample prepare-commit-msg
$> cat /dev/null > prepare-commit-msg
$> vim prepare-commit-msg
```
훅의 컨텐츠는 bash 문법으로 작성하면 됩니다.
```
#!/bin/sh
# 현재 active 브랜치 명을 가져옵니다
NAME=$(git branch | grep '*' | sed 's/* //')

# master 가 아닌 경우에 <브랜치명>:<메세지로 작성합니다>
if [ "$NAME" == "master" ]; then
	echo $(cat "$1") > "$1"
else
	echo "$NAME"': '$(cat "$1") > "$1"
fi
```
<b>반드시</b> executable 권한을 부여해야 합니다.

## 02. 커밋 단장
커밋은 반드시 의미 있는 단위로 이루어져야 되돌리기도 쉽고 관리하기도 쉽습니다. 만약 하나의 이슈를 커밋하고 해당 이슈에서 실수를 발견해서 조치했다면 2개의 커밋이 발생하게 됩니다. 이런 경우 하나로 합치는 것이 좋습니다.

```
$> git rebase -i HEAD~2
```
- `-i`: 인터렉티브 옵션입니다. 이 경우 prompt 가 제공됩니다. 

interactive 모드에 진입했다면 합치고자 하는 커밋의 해시에 명령어를 수행합니다. 이 경우에는 `s`(squish)를 사용합니다.

## 03. tag 와 버전 관리
`tag` 는 한번 발행하면 언제든지 그 위치로 돌아갈 수 있도록 꼽아 놓는 깃발입니다. 현재 팀에서는 tag 로 버전 관리를 하고 있습니다. 간단하게 tag 를 등록하고 삭제 해봅니다.
```
# 생성과 원격 등록
$> git tag 0.01
$> git push origin main --tags

# 삭제와 원격 태그 삭제
$> git tag -d 0.01
$> git push origin :0.01
```

## 99. 그 외 에티켓
- 공용 리파지토리에는 force push 를 하지 않습니다.

