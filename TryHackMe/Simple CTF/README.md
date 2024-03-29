# Simple CTF

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
### Task 1 Simple CTF
Deploy the machine and attempt the questions!

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/machine.png)

Answer the questions below

How many services are running under port 1000?

> 2

nmap을 사용해 사용중인 포트를 검색한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/nmap.png)

총 3개가 스캔되었는데 그 중 1000 아래인 것은 ftp 21과 http 80 2개이다.

What is running on the higher port?

> ssh

젤 높은 포트를 사용중인 것은 ssh 2222번 포트이다.

What's the CVE you're using against the application?

> CVE-2019-9053

CVE 번호를 알기 위해 처음 포트인 ftp 21번 포트를 알아봤다.

ftp 서버에서 anonymous 모드를 허용하고 있어 이것을 이용했다.

![ftp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/ftp.png)

pub 디렉터리 안에 ForMitch.txt 파일이 있으니 이것을 가져와 확인한다.

![ForMitch](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/ForMitch.png)

하지만 이 파일에서는 얻을 수 있는게 없었다.

그럼 이제 http 80번 포트를 알아본다. 

10.10.198.196 을 접속하면 apache default 페이지가 나왔다. 

![default](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/default.png)

10.10.198.196 안에 있는 디렉터리를 검색한다.

```
sudo gobuster dir -u http://10.10.198.196/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

검색하는 도중에 /simple 디렉터리가 있다고 나왔다.

![gobuster](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/gobuster.png)

이 디렉터리를 확인해 보면 cms made simple 이라는 페이지가 나온다.

![cms](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/cms.png)

사이트 아래쪽을 보면 cms made simple 버전이 있다.

![cmsversion](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/cmsversion.png)

이 버전의 취약점이 있는지 확인했는데 하나의 SQL Injection 취약점이 발견되는데 이 파일을 복사해 온다.

![cp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/cp.png)

복사한 파일을 확인한다.

![46635](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/46635.png)

파일 안에서 CVE 번호를 얻었다.

To what kind of vulnerability is the application vulnerable?

> sqli

searchsploit으로 검색하여 SQL Injection 취약점이 있는 것을 알아냈다.

![searchsploit](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/searchsploit.png)

What's the password?

> secret

복사한 46635.py 파일을 실행하면 print 에러가 나와 사용할 수 없다. python2.7을 사용하여 실행한다.

![python](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/python.png)

python2.7을 사용하면 `termcolor` 모듈이 없다고 한다. 그럼 아래 코드를 입력해 주면 된다.

```
python2.7 -m pip install termcolor
```

예제 코드 중 패스워드 크랙을 할 수 있는 예제가 있으니 따라하면 패스워드를 얻을 수 있다.

![code](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/code.png)

![crack](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/crack.png)

Where can you login with the details obtained?

> ssh

아이디 mitch와 패스워드 secret를 얻었으니 ssh 2222번 포트를 사용하여 ssh를 접속한다.

![ssh](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/ssh.png)

What's the user flag?

> G00d j0b, keep up!

user.txt 확인한다.

![user](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/user.png)

Is there any other user in the home directory? What's its name?

> sunbath

다른 홈 디렉터리가 있다고 하니 /home 디렉터리로 이동해 확인한다.

![sunbath](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/sunbath.png)

What can you leverage to spawn a privileged shell?

> vim

실행할 수 있는 쉘을 물어보고 있으니 sudo를 허용하는 리스트를 확인하면 vim을 사용할 수 있다.

![sudo](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/sudo.png)

What's the root flag?

> W3ll d0n3. You made it!

그럼 vim을 이용하여 root 디렉터리에 있는 파일을 확인한다.

```
sudo /usr/bin/vim /root
```

![vim](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/vim.png)

root.txt 파일이 있으니 다시 vim을 이용하여 확인한다.

```
sudo /usr/bin/vim /root/root.txt
```

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/Simple%20CTF/image/flag.png)
