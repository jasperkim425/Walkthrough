# Appointment

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough

* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine 

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Appointment/image/machine.png)

* Task 1 | What does the acronym SQL stand for?

> Structured Query Language

* Task 2 | What is one of the most common type of SQL vulnerabilities?

> SQL injection

* Task 3 | What does PII stand for?

> Personally Identifiable Information

* Task 4 | What does the OWASP Top 10 list name the classification for this vulnerability?

> A03:2021-Injection

https://owasp.org/www-project-top-ten/ 사이트에서 확인 가능하다.

![A03](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Appointment/image/A03.png)

* Task 5 | What service and version are running on port 80 of the target?

> Apache httpd 2.4.38 ((Debian))

`nmap`을 사용해 80번 포트의 버전을 확인한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Appointment/image/nmap.png)

* Task 6 | What is the standard port used for the HTTPS protocol?

> 443

* Task 7 | What is one luck-based method of exploiting login pages?

> brute-forcing

* Task 8 | What is a folder called in web-application terminology?

> directory

* Task 9 | What response code is given for "Not Found" errors?

> 404

* Task 10 | What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?

> dir

* Task 11 | What symbol do we use to comment out parts of the code?

> #

* Submit Flag | Submit root flag

> e3d0796d002a446c0e622226f42e9672

`nmap`에서 80번 http 서버가 열려있다.

브라우저로 검색하면 아래와 같이 로그인 페이지가 나온다.

![http](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Appointment/image/http.png)

admin:admin을 입력했는데 로그인이 되지 않는다.

SQL Injection을 하기 위해 `admin'#:admin'#`을 입력했더니 플래그를 얻을 수 있었다.

![admin](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Appointment/image/admin.png)

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Appointment/image/flag.png)
