# Pawn

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine 

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/machine.png)

* Task 1 | What does the 3-letter acronym FTP stand for?

> File Transfer Protocol

* Task 2 | Which port does the FTP service listen on usually?

> 21

* Task 3 | What acronym is used for the secure version of FTP?

> SFTP

* Task 4 | What is the command we can use to send an ICMP echo request to test our connection to the target?

> ping

`ping` 명령어를 사용하면 ICMP echo request 를 보내 호스트가 열려있는지 확인할 수 있다.

![ping](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/ping.png)

* Task 5 | From your scans, what version is FTP running on the target?

> vsftpd 3.0.3

`nmap`을 사용해 FTP 버전을 알아낸다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/nmap.png)

* Task 6 | From your scans, what OS type is running on the target?

> Unix

`nmap`에서 `Service Info: OS: Unix`라고 OS 정보를 준다.

* Task 7 | What is the command we need to run in order to display the 'ftp' client help menu?

> ftp -h

`-h` 옵션은 help 옵션이다.

![ftp](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/ftp.png)

* Task 8 | What is username that is used over FTP when you want to log in without having an account?

> anonymous

FTP에서 `anonymous`는 익명 접속으로 파일 보기나 다운로드 밖에 할 수 없다.

* Task 9 | What is the response code we get for the FTP message 'Login successful'?

> 230

anonymous 접속을 시도한다. password를 원하는데 anonymous를 입력하면 로그인이 된다.

![login](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/login.png)

* Task 10 | There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.

> ls

`ls`는 list의 약자로 목록을 보는 명령어이다.

![ls](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/ls.png)

* Task 11 | What is the command used to download the file we found on the FTP server?

> get

`get` 명령어로 파일을 다운받을 수 있다.

![get](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/get.png)

* Submit Flag | Submit root flag 

> 035db21c881520061c53e0536e44f815

다운받은 텍스트 파일을 확인한다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Pawn/image/flag.png)
