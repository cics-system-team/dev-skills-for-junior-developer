> [홈](https://github.com/cics-system-team/dev-skills-for-junior-developer) > 공통 > 버전관리

# 버전관리

## Git

자주 사용하는 기본적인 명령어 목록

- **clone** : 레포지토리 로컬에 복사
- **add** : 변경 파일 스태시(Stash)
- **commit** : 스태시된 파일 커밋
- **push** : 커밋된 내용 원격 저장소(Remote repository)에 업로드
- **pull** : 원격 저장소(Remote repository)에서 로컬 저장소(Locale repository)로 변경 사항 다운로드
- **branch** : 브랜치 생성
- **remote**
- **checkout** : 해당 브랜치로 이동
- **merge** : 브랜치 병합
- **log** : 커밋 내역 확인
- **reset** : 특정 커밋으로 이동

자주 사용하진 않지만 알아두면 좋은 명령어 목록

- tag: 현재 커밋에 태그 생성

## GitHub

## Git Flow

버전 관리를 **어떻게** 할 것인지 흐름을 정해놓은 것. 회사마다 방식이 다르다.

무형문화연구원에서는 기본적으로 `main` 브랜치, `develop` 브랜치, `release` 브랜치를 사용하며, 새로운 기능을 추가하거나, 수정할 경우 `develop` 브랜치에서 새로운 브랜치로 체크아웃하여 사용한다.


```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchOrder': 1}} }%%
gitGraph:
    commit id: "init"
    commit
    branch develop order: 2
    checkout develop
    commit
    branch bug1 order: 3
    checkout bug1
    commit
    commit
    checkout develop
    branch bug2 order:3
    commit
    checkout develop
    merge bug2
    checkout bug1
    commit
    checkout develop
    merge bug1
    checkout main
    merge develop tag: "v1.0.0"
    branch release order: 0
    checkout release
    commit
    checkout develop
    commit
    
```
