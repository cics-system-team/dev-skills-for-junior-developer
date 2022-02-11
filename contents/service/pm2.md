# PM2

[PM2 홈페이지](https://pm2.keymetrics.io/)

여러 응용 프로그램들을 실행시키고, 관리하고, 쉽게 스케일업 할 수 있는 프로세스 관리 툴이다.

## 실행

- [PM2 프로세스 관리 문서](https://pm2.keymetrics.io/docs/usage/process-management/)

- [PM2 설정 파일 문서](https://pm2.keymetrics.io/docs/usage/application-declaration/)

- [PM2 환경 변수 문서](https://pm2.keymetrics.io/docs/usage/environment/)

명령줄로 실행하는 방법과 `json` 혹은 `js` 설정파일로 실행하는 방법이 있다.

설정 파일을 이용하여 실행할 경우 `pm2 start name.config.js`와 같은 방식으로 실행한다.

### 설정 파일 예시

1. React 앱 실행

frontend.config.js

```javascript
module.exports = {
  apps: [
    {
      name: 'cics-frontend',
      cwd: '.',
      args: ['--cwd', './cics-frontend', 'start'],
      script: 'yarn',
    },
  ],
};
```

2. SpringBoot(jar) 앱 실행

backend.config.js

```javascript
module.exports = {
  apps: [
    {
      name: 'cics-backend',
      cwd: '.',
      args: [
        '-jar',
        './cics-backend/build/libs/cics-backend-1.4.1-SNAPSHOT.jar',
        '--Dspring.profiles.active=aws',
      ],
      script: 'java',
      node_args: [],
    },
  ],
};
```

frontend.config.js

```javascript
module.exports = {
  apps: [
    {
      name: 'cics-frontend',
      cwd: '.',
      args: ['--cwd', './cics-frontend', 'start'],
      script: 'yarn',
    },
  ],
};
```

## 자동 실행

- [PM2 시동 스크립트 생성 문서](https://pm2.keymetrics.io/docs/usage/startup/)

PM2는 프로세스 재시작될 때 기존의 인스턴스를 다시 실행시켜주지 않음. 만약 지금 실행중인 인스턴스를 PM2를 재시작하거나 서버를 재시작할 때 다시 복구시키고 싶다면 `pm2 startup`과 `pm2 save`를 이용한다.

```
ubuntu@ip-172-31-11-149 ~/deploy
❯ pm2 startup
[PM2] Init System found: systemd
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu

ubuntu@ip-172-31-11-149 ~/deploy
❯ sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu


                        -------------

__/\\\\\\\\\\\\\____/\\\\____________/\\\\____/\\\\\\\\\_____
 _\/\\\/////////\\\_\/\\\\\\________/\\\\\\__/\\\///////\\\___
  _\/\\\_______\/\\\_\/\\\//\\\____/\\\//\\\_\///______\//\\\__
   _\/\\\\\\\\\\\\\/__\/\\\\///\\\/\\\/_\/\\\___________/\\\/___
    _\/\\\/////////____\/\\\__\///\\\/___\/\\\________/\\\//_____
     _\/\\\_____________\/\\\____\///_____\/\\\_____/\\\//________
      _\/\\\_____________\/\\\_____________\/\\\___/\\\/___________
       _\/\\\_____________\/\\\_____________\/\\\__/\\\\\\\\\\\\\\\_
        _\///______________\///______________\///__\///////////////__


                          Runtime Edition

        PM2 is a Production Process Manager for Node.js applications
                     with a built-in Load Balancer.

                Start and Daemonize any application:
                $ pm2 start app.js

                Load Balance 4 instances of api.js:
                $ pm2 start api.js -i 4

                Monitor in production:
                $ pm2 monitor

                Make pm2 auto-boot at server restart:
                $ pm2 startup

                To go further checkout:
                http://pm2.io/


                        -------------

[PM2] Init System found: systemd
Platform systemd
Template
[Unit]
Description=PM2 process manager
Documentation=https://pm2.keymetrics.io/
After=network.target

[Service]
Type=forking
User=ubuntu
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
Environment=PATH=/usr/local/lib/jvm/jdk-17.0.1+12/bin:/home/ubuntu/.local/share/zinit/polaris/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/binbin:/usr/sbin:/usr/bin
Environment=PM2_HOME=/home/ubuntu/.pm2
PIDFile=/home/ubuntu/.pm2/pm2.pid
Restart=on-failure

ExecStart=/usr/lib/node_modules/pm2/bin/pm2 resurrect
ExecReload=/usr/lib/node_modules/pm2/bin/pm2 reload all
ExecStop=/usr/lib/node_modules/pm2/bin/pm2 kill

[Install]
WantedBy=multi-user.target

Target path
/etc/systemd/system/pm2-ubuntu.service
Command list
[ 'systemctl enable pm2-ubuntu' ]
[PM2] Writing init configuration in /etc/systemd/system/pm2-ubuntu.service
[PM2] Making script booting at startup...
[PM2] [-] Executing: systemctl enable pm2-ubuntu...
Created symlink /etc/systemd/system/multi-user.target.wants/pm2-ubuntu.service → /etc/systemd/system/pm2-ubuntu.service.
[PM2] [v] Command successfully executed.
+---------------------------------------+
[PM2] Freeze a process list on reboot via:
$ pm2 save

[PM2] Remove init script via:
$ pm2 unstartup systemd


```

##
