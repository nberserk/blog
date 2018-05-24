---
layout: post
title:  "ssh config"
tags: ssh
---

aws를 사용하다보면 ssh 작업을 많이 하게 되는데, 이 때 알아두면 너무 너무 편리한 팁들이다.

## passwordless ssh
나의 public key(.ssh/id_rsa.pub)를 대상 컴의 .ssh/authrized_keys 에 복사해주면
` ssh user@server ls / `를 패스워드 없이 바로 실행할 수 있다.

> cat ~/.ssh/id_rsa.pub | ssh stg 'cat >> .ssh/authorized_keys'


만약 .ssh 폴더가 없다면, `ssh-keygen -t rsa`로 생성해 주면 된다.

## .ssh/config
ssh의 여러가지 설정을 담고 있는 설정 파일이다.

```
Host my
     User darren
     Hostname 10.1.1.1
```

위 설정 파일을 사용하면 `ssh my`만 타이핑 하면, `ssh darren@10.1.1.1`로 번역되어 실행 된다.

```
Host bas
     User ec2-user
     Hostname bastion
     Port 352
     IdentityFile ~/.ssh/bastion.pem
```

위 설정 파일은 `ssh bas`를 `ssh -i ~/.ssh/bastion.pem ec2-user@bastion -p 352` 로 해석 된다.

## ssh tunneling
특정 포트를 포트 포워딩할 수도 있다. 보통 회사의 네트웍 같은 경우 80 포트를 제외한 나머지는 모두 방화벽으로 막혀 있는 경우가 많은데 이럴때 유용하게 사용할 수 있다.

```
Host myserver
    HostName mysql
    IdentityFile ~/.ssh/mysql.key
    LocalForward 9906 127.0.0.1:3306
    User mysqlUser
```

database컴의 3306 포트가 localhost:9906로 포트 포워딩 된다. localhost:9906에서 mysql의 port에 접근할 수 있다.

## ControlPath
control path를 사용하면 ssh connection을 캐쉬 해서 접속 시간을 줄일 수 있어, 반복적으로 ssh 명령을 불러야 할때 사용하면 유용하다.

```
Host *
	ControlPath            ~/.ssh/mux-%r@%h:%p
```

## proxyCommand

클라우드를 사용하면 회사 방화벽때문에 connector(bastion) 서버에 우선 접속한 뒤, 실제 서버로 연결해야 하는 경우가 많이 있다. 예를 들면 아래와 같은 경우.

```
 +----------+         +---------+        +-----------+
 |localhost | <---->  |bastion  |  <---> | myserver  |
 +----------+         +---------+        +-----------+
```
이런 경우 ssh를 두번 타고 들어가면 myserver까지 접속은 가능하다. 아래 처럼. -tt는 tty를 강제할당하는 명령임.

`ssh -tt bas ssh -v -i <key>.pem ec2-user@myserver ls`

이렇게 하면 명령어가 길어지므로, 아래처럼 proxyCommand를 사용하면 더 간편해 진다. proxyCommand는 ssh를 하기 전에 실행할 명령을 써 주는 용도로 사용되는데, 여기서는 미리 bas에 접속하여 tcp proxy를 설정하는 역할을 하게 된다. tcp proxy는 nc(netcat)를 통하여 동작한다. 아래처럼 셋팅이 되어 있으면, 위의 명령어가 `ssh myserver ls` 이렇게 간단해 진다.

```
Host myserver
     User darren
     Hostname 10.0.0.99
     Port 22
     IdentityFile ~/.ssh/mykey.pem
     ProxyCommand ssh -qaY bas 'nc -w 14400 %h %p'
```

## run multiple bash commands with ssh

bash의 [here document](http://tldp.org/LDP/abs/html/here-docs.html) 를 사용하면 된다. 아래 예제에서 `-o SendEnv`는 로컬의 환경변수를 remote로 전달하기 위해서 사용했다.

```bash
ssh -o SendEnv=AWS_ACCESS_KEY_ID -o SendEnv=AWS_SECRET_ACCESS_KEY dev <<EOF
echo $AWS_ACCESS_KEY_ID, $AWS_SECRET_ACCESS_KEY
cd /home/ec2-user
sudo aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
sudo aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
sudo aws configure set default.region us-west-2
ll
EOF
```


## Reference
- [ssh proxy](http://backdrift.org/transparent-proxy-with-ssh)
- [ssh portforward](http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/)


## revision history
* 2015/8/24 initial draft
* 8/28 ControlPath added
* 9/1 proxy command added
* 2017/3/29 added `run multiple bash commands`