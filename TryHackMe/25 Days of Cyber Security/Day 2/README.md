# Day 2 | Web Exploitation The Elf Strikes Back!

## Workstation
- Virtual Box : VMware Fusion 12.1.0
- OS : kali-linux-2020.04


## Walkthrough
At the bottom of the dossier is a sticky note containing the following message:

For Elf McEager:

You have been assigned an ID number for your audit of the system: ODIzODI5MTNiYmYw . Use this to gain access to the upload section of the site.
Good luck!

You note down the ID number and navigate to the displayed IP address (MACHINE_IP) in your browser.

***

Connect OpenVPN and start AttackBox

```
$ sudo openvpn (TryHackMe ID).ovpn
```

![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/attackbox.png)

![ip](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/ip.png)

### What string of text needs adding to the URL to get access to the upload page?

> ?id=ODIzODI5MTNiYmYw

![id](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/id.png)

### What type of file is accepted by the site?

> Image

페이지 소스를 확인해 보면 이미지 파일을 업로드 할 수 있는 것을 확인했다.

![source](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/source.png)

Bypass the filter and upload a reverse shell.

### In which directory are the uploaded files stored?

> /uploads/

`dirb` 모듈을 사용하여 사이트의 디렉터리를 찾을 수 있다.

![dirb](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/dirb.png)

### Activate your reverse shell and catch it in a netcat listener!

1. php-reverse-shell.php 파일를 복사하고 파일 이름을 업로드 시 이미지 파일(.jpg.php)로 인식할 수 있도록 변경한다.
![php](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/php.png)

2. 복사한 php 파일을 내용 중 IP와 Port를 알맞게 변경한다.(IP는 내 아이피 주소를 입력)
![setting](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/setting.png)

3. 리버스 쉘을 얻을 수 있게 netcat listener를 시작한다.
![nc](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/nc.png)

4. reverse.jpg.php 파일을 업로드 한다. 파일이 보이지 않을 경우 파일 타입을 All Files로 변경하여 진행한다.
![upload](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/upload.png)

### What is the flag in /var/www/flag.txt?

> THM{MGU3Y2UyMGUwNjExYTY4NTAxOWJhMzhh}

1. FireFox 브라우저 검색창에 업로드한 파일 파라미터를 입력하여 준다.
![file](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/file.png)

2. 업로드된 파일을 찾은 후 실행하면 아까 실행했던 netcat listener에서 쉘을 얻을 수 있다. 그리고 /var/www/flag.txt 파일을 확인하면 된다.
![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%202/image/flag.png)
