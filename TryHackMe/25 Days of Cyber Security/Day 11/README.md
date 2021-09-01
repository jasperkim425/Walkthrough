# Day 11 | Networking The Rogue Gnome

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2011/image/attackbox.png)

What type of privilege escalation involves using a user account to execute commands as an administrator?

> Vertical

What is the name of the file that contains a list of users who are a part of the sudo group?

> sudoers

Use SSH to log in to the vulnerable machine like so: `ssh cmnatic@10.10.67.206`

Input the following password when prompted: aoc2020

![ssh](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2011/image/ssh.png)

Enumerate the machine for executables that have had the SUID permission set. Look at the output and use a mixture of [GTFObins](https://gtfobins.github.io/) and your researching skills to learn how to exploit this binary.

`cp` 명령어를 찾기 위해 `whereis` 명령어를 사용해 위치를 찾고 SUID 권한을 주기 위해 `chmod u+s /bin/cp` 명령어를 입력한다.

![cp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2011/image/cp.png)

하지만 권한이 적용되지 않는다. 아마 root 권한이 아니라 안되는 것 같으니 https://gtfobins.github.io/gtfobins/bash/#suid 이 사이트에서 루트 권한을 얻는 방법을 알 수 있다.

`bash -q` 명령어로 루트 권한을 얻은 후 다시 SUID 권한을 준 후, cp 명령어의 권한을 확인하면 SUID 권한이 제대로 부여되었다.

![root](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2011/image/root.png)

You may find uploading some of the enumeration scripts that were used during today's task to be useful.



Use this executable to launch a system shell as root.

What are the contents of the file located at /root/flag.txt?

> thm{2fb10afe933296592}

SUID 권한이 부여된 cp 명령어로 /root/ 파일을 복사해 온다.

복사 후 /root/ 파일 안에 있는 flag.txt 파일을 확인한다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2011/image/flag.png)
