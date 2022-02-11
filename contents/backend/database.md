# 데이터베이스

CICS는 기본적으로 MariaDB를 사용한다.

## 설치

- [MariaDB 문서](https://mariadb.org/download/?t=repo-config)

> 설치는 반드시 최신 버전으로 하도록 한다.

## 초기 설정

### 보안 설정

`mariadb-secure-installation`을 사용하여 root의 비밀번호, 접속 권한 등을 설정한다.

### 문자 인코딩 설정

- [MariaDB Setting Character Sets and Collations 문서](https://mariadb.com/kb/en/setting-character-sets-and-collations/)

MariaDB의 기본 문자 인코딩 방식은 latin1, 기본 콜렉션은 latin1_swedish_ci으로 설정되어있다. 이를 utf8로 변경하여야 한다. CICS는 비 라틴계열 문자열을 사용하는 일이 많기 때문에 utf8mb4와 utf8mb4_general_ci 혹은 utf8mb4_unicode_ci를 추천한다.

> utf8은 이모지를 지원하지 않고, utf8mb4는 이모지를 지원한다는 차이가 있다.
> utf8mb4_general_ci는 À, Á, Å, å, ā, ă와 같은 문자를 A와 a로 묶어서 처리를 하지만 utf8mb4_unicode_ci는 전부 별도의 문자로 처리한다. 따라서 성능은 utf8mb4_general_ci가 낫지만 정확한 구분이 필요할 경우 utf8mb4_unicode_ci를 사용하도록 한다. 이는 개발하는 어플리케이션에 의존한다.

이 기본값을 수정하려면 `/etc/mysql/my.cnf`에 아래 내용을 추가해준다.

my.cnf

```toml
[mysql]
#...
default-character-set=utf8mb4
#...
[mysqld]
#...
collation-server = utf8mb4_unicode_ci
init-connect='SET NAMES utf8mb4'
character-set-server = utf8mb4
#...
```

### 대소문자 구별 설정

이 기본값을 수정하려면 `/etc/mysql/my.cnf`에 아래 내용을 추가해준다.

**설정 확인**

```sql
SHOW GLOBAL VARIABLES LIKE 'lower_case_table_names';
```

```
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 0     |
+------------------------+-------+
```

**설정**

```toml
[mariadb]
lower_case_table_names=1
```

- 0 : Unix 기반 시스템 기본값. 대소문자를 구별함
- 1 : Windows 기본값. 대소문자를 구별하지 않음
- 2 : 선언된 대로 저장되지만 비교는 소문자로 함
