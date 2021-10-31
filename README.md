# 팀 git 교육

## 00. 개요
 
git 을 사용하고 있지만 cli 에 익숙하지 않거나 사용하는 커맨드만 사용하시는 분들이 좀 더 편안하게 사용할 수 있도록 도움을 줄 수 있는 문서입니다. 현재는 소스트리 같은 UI 툴도 많이 있지만 서버 작업과 같이 UI를 사용하지 못하는 환경에서도 동일하게 사용할 수 있는 것을 목표로 합니다.

#### 키워드
- 명령: add/commit 과 같이 `--help`의 `commands`섹션에 표기되는 내용입니다.
- 옵션: `-v`, `-A` 와 같이 `--help`의 `options`섹션에 표기되는 내용입니다.
- 코드릿의 `[|]`: 택일 항목으로 `[A|B]` 는 A 혹은 B 를 사용할 수 있다는 것을 의미합니다.
- 코드릿의 `<>`: `<>` 는 브랜치명이나 원격 url 같이 사용자 임의의 변수를 의미합니다. 

#### 준비물
아래 구문을 .bashrc 에 등록합니다.
```
alias gsl="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### Week 1
1 주차에는 기본 명령어들을 배워 봅니다. [링크](https://github.com/juneyoung/git_edu/tree/main/week1)
- git 용어 정의
- git 활성화와 원격 리파지토리 관리
- 원격 정보 가져오기
- 로컬 변경사항 등록하기
- 원격에 반영하기

### Week 2
2 주차에는 confict 상황을 해결할 수 있는 옵션들에 대해 배워봅니다. [링크](https://github.com/juneyoung/git_edu/tree/main/week2)
- 변경점 병합하기
- 변경 내용 임시저장하기
- 없었던 일로 하기
- 내역을 유지한 채로 취소하기

### Week 3
3 주차에는 팀에서 사용하는 에티켓과 브랜치 전략을 설명합니다. [링크](https://github.com/juneyoung/git_edu/tree/main/week3)
- hooks
- 커밋 단
- tag 와 버전 관리

### *. 참고자료
문서가 git 의 모든 부분을 커버하고 있지는 않기 때문에 도움이 될만한 자료를 첨부합니다.
- [git 공식 가이드](https://git-scm.com/about/branching-and-merging)
