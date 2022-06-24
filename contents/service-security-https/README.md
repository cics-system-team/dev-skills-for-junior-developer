> [홈](https://github.com/cics-system-team/dev-skills-for-junior-developer#서비스) > 서비스 > 배포/자동화 > PM2

# HTTPS 적용

TLS 인증서 발급에는 [Certbot](https://certbot.eff.org/)을 사용한다. Certbot은 인증서 발급 기관인 [Let's Encrypt](https://letsencrypt.org/)를 이용하며, 인증서 발급을 쉽게 도와주는 툴이다.

## Certbot 설치

[Certbot 페이지](https://certbot.eff.org/)에 접속하여 자신에 맞는 소프트웨어(웹서버)와 시스템(운영체제)를 선택한다.

> 무형문화연구원에서는 Nginx와 Ubuntu를 이용하여 웹서버를 구축중이기 때문에 이를 기준으로 설명합니다.

### 설치방법

1. snap 설치

운영체제로 Ubuntu를 사용한다면 snap을 설치할 필요가 없다. 우분투가 아니라면 [문서](https://snapcraft.io/docs/installing-snapd)를 보고 따라 설치한다.

**로그**

```shell
ubuntu@ip-172-31-11-149 ~
❯ snap
The snap command lets you install, configure, refresh and remove snaps.
Snaps are packages that work across many different Linux distributions,
enabling secure delivery and operation of the latest apps and utilities.

Usage: snap <command> [<options>...]

Commonly used commands can be classified as follows:

         Basics: find, info, install, remove, list
        ...more: refresh, revert, switch, disable, enable, create-cohort
        History: changes, tasks, abort, watch
        Daemons: services, start, stop, restart, logs
    Permissions: connections, interface, connect, disconnect
  Configuration: get, set, unset, wait
    App Aliases: alias, aliases, unalias, prefer
        Account: login, logout, whoami
      Snapshots: saved, save, check-snapshot, restore, forget
         Device: model, reboot, recovery
      ... Other: warnings, okay, known, ack, version
    Development: download, pack, run, try

For more information about a command, run 'snap help <command>'.
For a short summary of all commands, run 'snap help --all'.
```

(정상적으로 설치 된 모습)

2. snap 최신 버전으로 업데이트

쉘에 이하 명령어를 실행하여 snapd를 최신 버전으로 업데이트한다.

```shell
sudo snap install core; sudo snap refresh core
```

**로그**

```shell
ubuntu@ip-172-31-11-149 ~
❯ sudo snap install core; sudo snap refresh core
core 16-2.54.2 from Canonical✓ installed
snap "core" has no updates available
```

3. 기존 certbot 패키지 제거 후 snapd를 사용하여 다시 설치

운영체제에서 기본적으로 설치된 certbot이 있을 수 있으므로 `sudo apt-get remove certbot`(우분투) 명령어를 사용하여 제거한 후, `sudo snap install --classic certbot` 명령어로 snapd를 이용하여 설치해준다.

**로그**

```shell
ubuntu@ip-172-31-11-149 ~ 32s
❯ sudo apt-get remove certbot
E: Could not get lock /var/lib/dpkg/lock-frontend. It is held by process 12654 (unattended-upgr)
N: Be aware that removing the lock file is not a solution and may break your system.
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?

ubuntu@ip-172-31-11-149 ~
❯ sudo snap install --classic certbot
certbot 1.23.0 from Certbot Project (certbot-eff✓) installed
```

4. bin폴더로 심볼릭 링크 걸기

쉘에서 바로 사용할 수 있도록 /usr/bin 폴더에 링크를 건다.

```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

5. certbot으로 인증서 발급

웹서버가 nginx이므로 뒤에 `nginx` 옵션을 붙여 인증서를 발급한다. 나머지 설정은 certbot에서 자동으로 해 주므로 직접 수정할 필요가 없다.

```shell
sudo certbot --nginx
```

**로그**

```shell
ubuntu@ip-172-31-11-149 ~
❯ sudo certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): cicssystem@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: cics.center
2: api.cics.center
3: cdn.cics.center
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
Requesting a certificate for cics.center and 2 more domains

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/cics.center/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/cics.center/privkey.pem
This certificate expires on 2022-05-10.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for cics.center to /etc/nginx/sites-enabled/cics.center
Successfully deployed certificate for api.cics.center to /etc/nginx/sites-enabled/api.cics.center
Successfully deployed certificate for cdn.cics.center to /etc/nginx/sites-enabled/cdn.cics.center
Congratulations! You have successfully enabled HTTPS on https://cics.center, https://api.cics.center, and https://cdn.cics.center

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

6. 인증서 자동 갱신 테스트

쉘에 아래 명령어를 입력해 정상적으로 갱신되는지 테스트해본다.

```shell
sudo certbot renew --dry-run
```

7. crontab에 갱신 등록

tls인증서의 기간은 90일이므로 그 전에 갱신되도록 cron을 설정해준다.

### 전체 로그

```shell
ubuntu@ip-172-31-11-149 ~
❯ snap
The snap command lets you install, configure, refresh and remove snaps.
Snaps are packages that work across many different Linux distributions,
enabling secure delivery and operation of the latest apps and utilities.

Usage: snap <command> [<options>...]

Commonly used commands can be classified as follows:

         Basics: find, info, install, remove, list
        ...more: refresh, revert, switch, disable, enable, create-cohort
        History: changes, tasks, abort, watch
        Daemons: services, start, stop, restart, logs
    Permissions: connections, interface, connect, disconnect
  Configuration: get, set, unset, wait
    App Aliases: alias, aliases, unalias, prefer
        Account: login, logout, whoami
      Snapshots: saved, save, check-snapshot, restore, forget
         Device: model, reboot, recovery
      ... Other: warnings, okay, known, ack, version
    Development: download, pack, run, try

For more information about a command, run 'snap help <command>'.
For a short summary of all commands, run 'snap help --all'.

ubuntu@ip-172-31-11-149 ~
❯ sudo snap install core; sudo snap refresh core
core 16-2.54.2 from Canonical✓ installed
snap "core" has no updates available

ubuntu@ip-172-31-11-149 ~ 32s
❯ sudo apt-get remove certbot
E: Could not get lock /var/lib/dpkg/lock-frontend. It is held by process 12654 (unattended-upgr)
N: Be aware that removing the lock file is not a solution and may break your system.
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?

ubuntu@ip-172-31-11-149 ~
❯ sudo snap install --classic certbot
certbot 1.23.0 from Certbot Project (certbot-eff✓) installed

ubuntu@ip-172-31-11-149 ~ 16s
❯ sudo ln -s /snap/bin/certbot /usr/bin/certbot

ubuntu@ip-172-31-11-149 ~
❯ sudo certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): cicssystem@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: cics.center
2: api.cics.center
3: cdn.cics.center
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
Requesting a certificate for cics.center and 2 more domains

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/cics.center/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/cics.center/privkey.pem
This certificate expires on 2022-05-10.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for cics.center to /etc/nginx/sites-enabled/cics.center
Successfully deployed certificate for api.cics.center to /etc/nginx/sites-enabled/api.cics.center
Successfully deployed certificate for cdn.cics.center to /etc/nginx/sites-enabled/cdn.cics.center
Congratulations! You have successfully enabled HTTPS on https://cics.center, https://api.cics.center, and https://cdn.cics.center

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

```

## TLS 인증서 갱신
### 인증서 갱신 테스트
```
sudo certbot renew --dry-run
```

위 명령어가 동작하지 않으면 실제로 갱신할 때도 오류가 발생한다.


### 인증서 갱신
```
sudo certbot renew
```

