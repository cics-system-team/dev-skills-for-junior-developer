# 버전관리

## Git

## GitHub

## Git Flow

버전 관리를 **어떻게** 할 것인지 흐름을 정해놓은 것.

무형문화연구원에서는 기본적으로 `main` 브랜치, `develop` 브랜치, `release` 브랜치를 사용하며, 새로운 기능을 추가하거나, 수정할 경우 `develop` 브랜치에서 새로운 브랜치로 체크아웃하여 사용한다.

```mermaid
gitGraph:
options
{
    "nodeSpacing": 60,
    "nodeRadius": 6
}
end
commit
branch develop
checkout develop
commit
commit
checkout master
commit
merge develop
branch release
checkout release
commit
checkout develop
commit
```
