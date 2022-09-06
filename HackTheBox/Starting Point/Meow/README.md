# Meow

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
* Connect | Connect to Starting Point VPN before starting the machine

Starting Point의 vpn을 다운받아 실행한다.

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine

vpn 실행 후 새로고침을 누르면 머신을 활성화 시킬 수 있다.

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/machine.png)

* Task 1 | What does the acronym VM stand for?

> Virtual Machine

VM이란 Virtual Machine의 약자이다.

* Task 2 | What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.

> Terminal

명령어를 입력하는 곳, 콘솔이나 쉘이라 불리는 것은 터미널(Terminal)이다.

* Task 3 | What service do we use to form our VPN connection into HTB labs?

> openvpn

첫번째에서 `openvpn`으로 vpn을 연결했다.

* Task 4 | What is the abbreviated name for a 'tunnel interface' in the output of your VPN boot-up sequence output?

> tun

tunnel interface의 약자는 tun이다. `ifconfig` 명령어로 확인 가능하다.

![ifconfig](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/ifconfig.png)

* Task 5 | What tool do we use to test our connection to the target with an ICMP echo request?

> ping

ICMP echo request로 연결 확인하는 것은 `ping` 이다.

![ping](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/ping.png)

* Task 6 | What is the name of the most common tool for finding open ports on a target?

> nmap

열려있는 포트 검색은 주로 `nmap` 을 사용한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/nmap.png)

* Task 7 | What service do we identify on port 23/tcp during our scans?

> telnet

`nmap` 으로 telnet 서버가 23번 포트에 열려있다.

* Task 8 | What username is able to log into the target over telnet with a blank password?

> root

`telnet` 서버가 열려있으니 접속한다.

하지만 아이디와 패스워드가 필요한데 admin, administrator, root 를 차례대로 입력하다가 `root`로 패스워드 없이 접속이 가능했다.

![telnet](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/telnet.png)

![root](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/root.png)

* Submit Flag | Submit root flag

> b40abdfe23665f766f9c61ecba8a4c19

목록을 확인하면 플래그를 얻을 수 있다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Meow/image/flag.png)
