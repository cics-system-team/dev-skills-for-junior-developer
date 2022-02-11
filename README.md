# dev-skills-for-junior-developer

신입 개발자를 위한 정보가 담긴 레포지토리

## 운영체제

- POSIX 명령어

## 프론트엔드

- HTML/CSS
- JavaScript
- React
  - [styled-components](https://styled-components.com/)
- 템플릿 엔진
  - [Thymeleaf](https://www.thymeleaf.org/) : Spring에서 사용하는 템플릿 엔진
  - [DTL](https://docs.djangoproject.com/en/4.0/topics/templates/) : Django에서 사용하는 템플릿 엔진

## 백엔드

### 웹프레임워크

- SpringBoot : Java/Kotlin 학습이 선행되어야 함
  - Gradle : SpringBoot 관리에 사용
  - yaml : SpringBoot 설정 파일에 사용
  - JPA : 데이터베이스 ORM
- Django : Python 학습이 선행되어야 함
  - venv

### 데이터베이스

- MySQL/MariaDB : 사용하는 데이터베이스
  - 초기 설정 익히기(utf8로 설정, 대소문자 설정 등)

## 서비스

### 웹서버

- NginX : 무형문화연구원에서 주로 사용하는 웹 서버
- Apache

### 배포/자동화

- POSIX 명령어 : nohup 명령어와 & 키워드의 사용법 익히기
- Github Actions : CI/CD를 할 수 있는 자동화 도구
- [PM2](https://pm2.keymetrics.io/) : 서비스 배포 시 사용하는 프로그램
  - 참고) [PM2를 활용한 Node.js 무중단 서비스하기](https://engineering.linecorp.com/ko/blog/pm2-nodejs/)

### 보안

- [HTTPS](/contents/service/https.md) : TLS 인증서를 사용한 안전한 HTTP 통신
