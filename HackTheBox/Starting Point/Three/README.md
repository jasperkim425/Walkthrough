# Three

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough

* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine]()

* Task 1 | How many TCP ports are open?

> 2

nmap으로 포트 스캔을 한다.

![nmap]()

* Task 2 | What is the domain of the email address provided in the "Contact" section of the website?

> thetoppers.htb

http 서버가 열려있으니 브라우저로 접속하여 Contact 부분을 확인한다.

![contact]()

* Task 3 | In the absence of a DNS server, which Linux file can we use to resolve hostnames to IP addresses in order to be able to access the websites that point to those hostnames?

> /etc/hosts

DNS 서버를 hosts에 등록한다.

![hosts]()

* Task 4 | Which sub-domain is discovered during further enumeration?

>  s3.thetoppers.htb 

서브 도메인을 찾기 전 서브도메인 리스트를 확인한다.

![sub]()

이제 gobuster를 사용해 서브도메인을 검색한다. `vhost` 옵션은 호스트에 대한 무차별 공격 옵션이다.

![gobuster]()

s3.thetoppers.htb 도메인이 검색되었다. 하지만 도메인을 검색해도 아무것도 나오지 않는다.

이 도메인을 사용하기 위해 hosts 파일에 등록한다.

![hosts2]()

등록이 완료되면 브라우저에서 정상적으로 접속할 수 있다.

![http]()

* Task 5 | Which service is running on the discovered sub-domain?

> Amazon S3

![s3]()

* Task 6 | Which command line utility can be used to interact with the service running on the discovered sub-domain?

> awscli

```
sudo apt install awscli
```

위 명령어를 입력해 설치한다.

* Task 7 | Which command is used to set up the AWS CLI installation?

>  aws configure

aws에 설정방법이 나와있다.

![configure]()

* Task 8 | What is the command used by the above utility to list all of the S3 buckets?

> aws s3 ls

https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/cli-services-s3-commands.html 사이트에서 명령어를 확인할 수 있다.

우리는 리스트 확인이 필요 하니 `ls` 명령어를 사용한다.

하지만 `endpoint URL`에 연결할 수 없다고 하니 endpoint를 다시 설정해 확인한다.

![ls]()

* Task 9 | This server is configured to run files written in what web scripting language?

> php

* Submit Flag | Submit root flag 

> a980d99281a28d638ac68b9bf9453c2b 

index.php 파일이 존재하므로 php 언어를 사용하고 있다.

그럼 우리는 명령어를 사용할 수 있게 명령 프롬프트 cmd 포함하는 php 파일을 하나 작성하여 업로드한다.

![shell]()

업로드한 파일을 실행하기 위해 파라미터 값에 `cmd=id` 를 입력해 본다.

![id]()

정상적으로 작동되는 것을 확인했으니 flag의 위치를 확인한다.

![locate]()

제일 마지막 부분에 flag의 위치가 나온다.

이제 파일을 읽어내면 된다.

![flag]()
