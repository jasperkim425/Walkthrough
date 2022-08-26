# Bounty Hacker

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
### Task 1 | Living up to the title. 

Deploy the machine.

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/machine.png)

Find open ports on the machine

nmap을 사용해 열려있는 포트를 스캔한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/nmap.png)

Who wrote the task list?

> lin

nmap에서 ftp의 anonymous 모드가 허용되어 있다고 나온다.

ftp 서버를 접속해 목록을 확인하면 2개의 파일이 있는데, 이 파일들을 get으로 모두 가져온다.

![ftp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/ftp.png)

가져온 2개의 파일을 확인해 보면 locks.txt 파일은 패스워드 모음? 같아 보이고, task.txt 파일엔 두 문장이 있었다.

아마도 lin 이란 사람이 쓴 것 같다.

![cat](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/cat.png)

What service can you bruteforce with the text file found?

> ssh

lin 이란 유저가 있는 것을 알아냈으니 22번에 열려있는 ssh를 사용해 접속을 시도한다.

하지만 패스워드가 걸려있다. 우리는 패스워드를 모르지만 ftp로 가져온 locks.txt 파일이 도움이 될 것 같다.

패스워드 무차별 대입 공격을 하기 위해 hydra 모듈을 사용한다.

![hydra](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/hydra.png)

-l 은 User 옵션이며, -P 는 패스워드 파일(소문자 -p 로 하면 패스워드만 입력)을 그 다음에 호스트와 사용할 서비스 ssh 그리고 -t 4 옵션으로 병렬 연결을 4개로 제한하여 사용한다.

What is the users password? 

> RedDr4gonSynd1cat3

위와 같이 무차별 대입 공격을 하면 lin 계정의 패스워드를 얻을 수 있다.

그럼 이제 ssh를 lin 계정으로 접속하면 성공적으로 접속이 가능하다.

또한 user.txt 파일도 얻을 수 있다.

![ssh](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/ssh.png)

user.txt

> THM{CR1M3_SyNd1C4T3}

root.txt

> THM{80UN7Y_h4cK3r}

lin 계정에서 사용가능한 sudo 명령어 리스트를 확인하면 /bin/tar 명령어는 root로 실행되는 것을 알 수 있다.

https://gtfobins.github.io/gtfobins/tar/#sudo 사이트에서 권한 상승할 수 있는 명령어를 가져와 입력하면 root 권한을 얻을 수 있으며, root.txt 파일도 얻었다.

![root](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Bounty%20Hacker/image/root.png)
