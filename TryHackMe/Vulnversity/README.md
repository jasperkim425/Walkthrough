# Vulnversity

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
### Task 1 | Deploy the machine
Connect to our network and deploy this machine. If you are unsure on how to get connected, complete the OpenVPN room first.

---

Deploy the machine

![machine]()

### Task 2 | Reconnaissance
Gather information about this machine using a network scanning tool called nmap. Check out the Nmap room for more on this!

Don't have a Linux machine with nmap on? Deploy your own AttackBox and control it with your browser.

---

Scan this box: `nmap -sV <machines ip>`

nmap is an free, open-source and powerful tool used to discover hosts and services on a computer network. In our example, we are using nmap to scan this machine to identify all services that are running on a particular port. nmap has many capabilities, below is a table summarising some of the functionality it provides.
nmap flag	Description

-sV	Attempts to determine the version of the services running

-p <x> or -p-	Port scan for port <x> or scan all ports

-Pn	Disable host discovery and just scan for open ports

-A	Enables OS and version detection, executes in-build scripts for further enumeration 

-sC	Scan with the default nmap scripts

-v	Verbose mode

-sU	UDP port scan

-sS	TCP SYN port scan

* There are many nmap "cheatsheets" online that you can use too.

* Scan the box, how many ports are open?

> 6

`nmap -sV`를 사용해 포트를 스캔하면 6개의 포트가 열려있다.

![nmap]()

* What version of the squid proxy is running on the machine?

> 3.5.12

위 사진에서 Squid http proxy의 버전이 나와있다.

* How many ports will nmap scan if the flag -p-400 was used?

> 3

-p-400 플래그는 400 이하의 포트를 스캔하는 것이다. 총 3개가 나온다.

![p-400]()

* Using the nmap flag -n what will it not resolve?

> dns

`nmap -h` 로 도움말을 살펴보면 -n에 대해 나와있다.

![nmap-n]()

* What is the most likely operating system this machine is running?

> ubuntu

아래 사진에서 알 수 있듯이 ssh와 http가 ubuntu로 실행되고 있다.

![nmap]()

* What port is the web server running on?

> 3333

http 서버의 포트는 3333번이다.

Its important to ensure you are always doing your reconnaissance thoroughly before progressing. Knowing all open services (which can all be points of exploitation) is very important, don't forget that ports on a higher range might be open so always scan ports after 1000 (even if you leave scanning in the background)
### Task 3 | Locating directories using GoBuster
Using a fast directory discovery tool called GoBuster you will locate a directory that you can use to upload a shell to.

---

Lets first start of by scanning the website to find any hidden directories. To do this, we're going to use GoBuster.

GoBuster is a tool used to brute-force URIs (directories and files), DNS subdomains and virtual host names. For this machine, we will focus on using it to brute-force directories.

Download GoBuster here, or if you're on Kali Linux 2020.1+ run `sudo apt-get install gobuster`

To get started, you will need a wordlist for GoBuster (which will be used to quickly go through the wordlist to identify if there is a public directory available. If you are using Kali Linux you can find many wordlists under /usr/share/wordlists.

Now lets run GoBuster with a wordlist: `gobuster dir -u http://<ip>:3333 -w <word list location>`

GoBuster flag	Description

-e	Print the full URLs in your console

-u	The target URL

-w	Path to your wordlist

-U and -P	Username and Password for Basic Auth

-p <x>	Proxy to use for requests

-c <http cookies>	Specify a cookie for simulating your auth

* What is the directory that has an upload form page?

> /internal/

gobuster로 디렉터리를 검색한다.

![gobuster]()

/internal 에서 업로드 할 수 있는 폼이 나온다.

![internal]()

### Task 4 | Compromise the webserver
Now you have found a form to upload files, we can leverage this to upload and execute our payload that will lead to compromising the web server.

---

* What common file type, which you'd want to upload to exploit the server, is blocked? Try a couple to find out.

> .php

internal의 소스를 확인하면 index.php 파일이 보인다.

To identify which extensions are not blocked, we're going to fuzz the upload form.

To do this, we're going to use BurpSuite. If you are unsure to what BurpSuite is, or how to set it up please complete our BurpSuite room first.

We're going to use Intruder (used for automating customised attacks).

To begin, make a wordlist with the following extensions in:

Now make sure BurpSuite is configured to intercept all your browser traffic. Upload a file, once this request is captured, send it to the Intruder. Click on "Payloads" and select the "Sniper" attack type.

Click the "Positions" tab now, find the filename and "Add §" to the extension. It should look like so:

* Run this attack, what extension is allowed?

> .phtml

`touch shell.php`로 php 파일을 생성 후 shell.php 파일을 업로드한다.

이때 위에서 알려주듯이 burpsuite를 연결해 intercept is on으로 한 후 업로드하면 아래와 같이 인터셉트 한다.

![intercept]()

마우스 우클릭 해 go to Intruder를 입력해 Positions에서 아래와 같이 .php만 설정해 준다.

![positions]()

그 다음 Payloads의 list에 아래와 같이 입력한다.

![list]()

start attack을 누르면 .phtml만 길이가 다르게 나와 Response를 눌러 아래를 확인하면 Success가 나와 .phtml 확장자만 허용하는 것을 알아냈다.

![phtml]()

Now we know what extension we can use for our payload we can progress.

We are going to use a PHP reverse shell as our payload. A reverse shell works by being called on the remote host and forcing this host to make a connection to you. So you'll listen for incoming connections, upload and have your shell executed which will beacon out to you to control!

Download the following reverse PHP shell here.

To gain remote access to this machine, follow these steps:

1. Edit the php-reverse-shell.php file and edit the ip to be your tun0 ip (you can get this by going to http://10.10.10.10 in the browser of your TryHackMe connected device).

2. Rename this file to php-reverse-shell.phtml

3. We're now going to listen to incoming connections using netcat. Run the following command: nc -lvnp 1234

4. Upload your shell and navigate to http://<ip>:3333/internal/uploads/php-reverse-shell.phtml - This will execute your payload

5. You should see a connection on your netcat session

* What is the name of the user who manages the webserver?

> bill

```
sudo git clone https://github.com/pentestmonkey/php-reverse-shell.git
```

위 명령어를 입력해 php 리버스 쉘 코드를 가져온다.

![git]()

.phtml 확장자만 업로드가 가능하기 때문에 .phtml로 확장자를 바꿔준다.(sudo를 사용해 바꾼다.)

![mv]()

다시 한번 sudo를 사용해 리버스 쉘 코드를 알맞게 편집한다.

IP 주소에 openvpn tryhackme IP 주소를 입력한다.

![ip]()

이제 리버스 쉘 파일을 업로드 한 후 실행시키기 전 `sudo nc -lvnp 1234'로 리스닝 시킨다. 

그런 다음 http://<ip>:3333/internal/uploads/php-reverse-shell.phtml를 입력해 실행시키면 아래와 같이 리버스 쉘을 얻을 수 있다.

home 으로 이동해 계정을 확인한다.

![shell]()

* What is the user flag?

> 8bd7992fbe8a6ad22a63361004cfcedb

user.txt 파일도 확인한다.

![userflag]()

### Task 5 | Privilege Escalation
Now you have compromised this machine, we are going to escalate our privileges and become the superuser (root).

---

In Linux, SUID (set owner userId upon execution) is a special type of file permission given to a file. SUID gives temporary permissions to a user to run the program/file with the permission of the file owner (rather than the user who runs it).

For example, the binary file to change your password has the SUID bit set on it (/usr/bin/passwd). This is because to change your password, it will need to write to the shadowers file that you do not have access to, root does, so it has root privileges to make the right changes.

* On the system, search for all SUID files. What file stands out?

> /bin/systemctl

SUID를 찾기 위해 `find / -perm -4000 2>/dev/null을 사용해 확인하면 systemctl이 있다.

![find]()

Its challenge time! We have guided you through this far, are you able to exploit this system further to escalate your privileges and get the final answer?

* Become root and get the last flag (/root/root.txt)

> a58ff8579f0a9270368d33a9966c7fd5

systemctl의 root 권한 상승을 알아보기 위해 https://gtfobins.github.io/gtfobins/systemctl/#suid 에서 suid를 이용한 취약점을 알 수 있다.

하지만 위 사이트에서 나온대로 한줄씩 차례대로 입력해 주면 된다.

그 과정에서 ./systemctl ~ 명령어를 입력할때는 /bin 파일로 이동해야 한다.

나는 id를 /tmp/output으로 저장하는 것이 아닌 root.txt파일을 /tmp/output으로 저장하게 명령어를 수정했다.

![gtfo]()

모든 입력이 다 되었다면 /tmp/output 파일을 확인한다.

![rootflag]()

