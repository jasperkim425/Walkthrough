# RootMe

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.3

## Walkthrough
### Task 1 | Deploy the machine
Connect to TryHackMe network and deploy the machine. If you don't know how to do this, complete the OpenVPN room first.

***

#### Deploy the machine
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/attackbox.png)

***

### Task 2 | Reconnaissance
First, let's get information about the target.

***

#### Scan the machine, how many ports are open?

> 2

nmap으로 포트를 검색하면 22, 80번 포트 2개가 열려있는 것을 알 수 있다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/nmap.png)

#### What version of Apache is running?

> 2.4.29

80번 포트에 열려있는 apache 서버의 버전을 확인한다.

#### What service is running on port 22?

> ssh

#### Find directories on the web server using the GoBuster tool.

`gobuster`로 웹서버의 디렉터리를 찾아보면 눈에 보이는 2개의 디렉터리가 있다.<br>

![gobuster](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/gobuster.png)

/uploads/ 디렉터리로 이동하면 업로드된 파일을 확인할 수 있는 페이지가 나온다.

![uploads](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/uploads.png)

/panel/ 디렉터리로 이동하면 파일을 업로드할 수 있는 페이지가 나왔다.

![panel](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/panel.png)

#### What is the hidden directory?

> /panel/

### Task 3 | Getting a shell
Find a form to upload and get a reverse shell, and find the flag.

***

#### user.txt

> THM{y0u_g0t_a_sh3ll}

리버스쉘을 얻기 위해 사용할 파일을 복사하고 `nano` 편집기로 내용을 알맞게 수정한다.

![cp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/cp.png)

수정할 내용은 TryHackMe OpenVPN의 IP 주소를 입력하고 포트는 4444로 지정해준다.

![nano](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/nano.png)

편집해준 파일을 /panel/ 페이지에 업로드 한다.

![upload](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/upload.png)

하지만 빨강색 알 수 없는 언어가 나와 /uploads/ 페이지에서 업로드가 되었는지 확인해 봤지만 아무것도 없었다.

![fail](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/fail.png)

php 파일은 업로드 할 수 없는 것을 확인했으니 업로드할 수 있는 확장자를 찾아본다.

![extension](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/extension.png)

가장 비슷한 `phtml` 확장자로 php 파일을 유지하며 변경해준다.

![mv](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/mv.png)

변경 후 파일을 업로드하면 성공되어 /uploads/ 페이지에 파일이 업로드 되었다.

![phtml](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/phtml.png)

리버스쉘 파일을 실행하기 전 netcat 리스너를 실행해 준다.

![nc](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/nc.png)

리스너를 실행 후 업로드된 파일을 클릭하여 실행하면 리버스쉘을 얻을 수 있다.

![shell](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/shell.png)

user.txt 파일을 찾기 위해 아래와 같이 입력하면 찾을 수 있다.

![userflag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/userflag.png)

### Task 4 | Privilege escalation
Now that we have a shell, let's escalate our privileges to root.

***

#### Search for files with SUID permission, which file is weird?

> /usr/bin/python

SUID 권한이 있는 파일을 찾기 위해 다음과 같은 명령어를 사용한다.

`find / -user root -perm -4000 -exec ls -l {} + 2>/dev/null`

위 명령어를 사용하면 python 파일이 user에게 SUID 권한이 있다는 것을 알아냈다.

![python](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/python.png)

#### Find a form to escalate your privileges.

#### root.txt

> THM{pr1v1l3g3_3sc4l4t10n}

https://gtfobins.github.io/gtfobins/python/ 에서 권한을 얻을 수 있는 방법을 알 수 있다.

![gtfobins](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/gtfobins.png)

위에서 코드를 얻었다면 알맞게 변경하여 실행하면 root 권한을 얻을 수 있다.

![rootflag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/RootMe/image/rootflag.png)
