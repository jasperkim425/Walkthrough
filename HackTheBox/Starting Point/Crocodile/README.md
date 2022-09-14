# Crocodile

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough

* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/machine.png)

* Task 1 | What nmap scanning switch employs the use of default scripts during a scan?

> -sC

`sudo nmap --help`을 사용해 script를 스캔하는 옵션을 확인한다.

![sC](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/sC.png)

이제 포트 검색을 한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/nmap.png)

* Task 2 | What service version is found to be running on port 21?

> vsftpd 3.0.3

* Task 3 | What FTP code is returned to us for the "Anonymous FTP login allowed" message?

> 230

* Task 4 | What command can we use to download the files we find on the FTP server?

> get

`ftp` 를 anonymous로 접속한다.

리스트를 확인하면 두개의 파일이 있다. 이 두개의 파일을 가져온다.

![ftp](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/ftp.png)

* Task 5 | What is one of the higher-privilege sounding usernames in the list we retrieved?

> admin

가져온 파일을 확인한다.

![cat](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/cat.png)

* Task 6 | What version of Apache HTTP Server is running on the target host?

> 2.4.41

* Task 7 | What is the name of a handy web site analysis plug-in we can install in our browser?

> Wappalyzer 

![wappalyzer](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/wappalyzer.png)

* Task 8 | What switch can we use with gobuster to specify we are looking for specific filetypes?

> -x

`sudo gobuster dir --help` 명령어를 사용하여 확장자를 검색하는 옵션이 있다.

![help](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/help.png)

* Task 9 | What file have we found that can provide us a foothold on the target?

> login.php

`gobuster`로 디렉터리 및 php 파일을 검색했더니 login.php 파일이 존재했다.

![gobuster](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/gobuster.png)

* Submit Flag | Submit root flag 

> c7110277ac44d78b6a9fff2232434d16

login.php 파일에서 ftp 서버에서 얻은 admin 계정과 패스워드로 로그인 한다.(admin:rKXM59ESxesUFHAd)

![login](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/login.png)

로그인하여 들어가면 플래그가 나온다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Crocodile/image/flag.png)
