# Dancing

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
*CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_Point_<ID>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Dancing/image/machine.png)

* Task 1 | What does the 3-letter acronym SMB stand for?

> Server Message Block

* Task 2 | What port does SMB use to operate at?

> 445

* Task 3 | What is the service name for port 445 that came up in our Nmap scan?

> microsoft-ds

`nmap` 으로 포트 스캔을 하면 445번 포트에 열려있는 서비스를 확인한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Dancing/image/nmap.png)

* Task 4 | What is the 'flag' or 'switch' we can use with the SMB tool to 'list' the contents of the share?

> -L

* Task 5 | How many shares are there on Dancing?

> 4

`smbclient`를 사용해 리스트를 확인한다. 

![smb](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Dancing/image/smb.png)

* Task 6 | What is the name of the share we are able to access in the end with a blank password?

> WorkShares

* Task 7 | What is the command we can use within the SMB shell to download the files we find?

> get

* Submit Flag | Submit root flag 

> 5f61c10dffbc77a704d76016a22f1664

`smbclient`를 사용해 로그인 한다. WorkShares 공유폴더가 확인되었으니 '\\PC명\공유폴더명'을 입력해 로그인한다.

WorkShares 안에 두개의 디렉터리가 있는데 James.P 의 날짜가 혼자만 다르니 James.P를 확인해 flag.txt 파일을 `get`으로 가져온다.

![get](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Dancing/image/get.png)

`get`으로 가져온 flag.txt 파일을 확인한다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Dancing/image/flag.png)
