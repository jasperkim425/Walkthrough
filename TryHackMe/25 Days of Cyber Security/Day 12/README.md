# Day 12 | Networking The Rogue Gnome

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
To solve Elf McSkidy's problem with the elves slacking in the workshop, he has created the CGI script: `elfwhacker.bat`

Deploy the instance attached to this task, use your NMAP skills from "Day 8 - What's Under the Christmas Tree?  to find out what port the webserver MACHINE_IP is running on...Visit the application and discover the installation version, weaponise this information by searching knowledgebases for exploits and Meterpreter payloads possible and whack those elves!.


As this is a Windows machine, please allow a minimum of five minutes for it to deploy before beginning your enumeration.


Bonus: There are at least two ways of escalating your privileges after you gain entry. Find these out and pivot at your leisure! (please note that this is optional for the day should you fancy the challenge...)

***

![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/attackbox.png)

### What is the version number of the web server?

> 9.0.17

일단 `nmap`으로 열려있는 포트를 찾는다.

```
$ sudo nmap -Pn 10.10.97.244
```

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/nmap.png)

8080번 포트에 http-proxy 서버가 열려있다. 해당 포트로 사이트로 접속한다.

![ip](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/ip.png)

Apache Tomcat 9.0.17 버전으로 만들어진 사이트가 있다.

### What CVE can be used to create a Meterpreter entry onto the machine? (Format: CVE-XXXX-XXXX)

> CVE-2019-0232

해당 Apache Tomcat 9.0.17 취약점이 있는지 검색했지만 아무것도 나오지 않아 Apache Tomcat 9.0으로 검색했더니 9.0.17까지 있는 취약점을 알아냈다.

https://www.exploit-db.com/exploits/47073

### Set your Metasploit settings appropriately and gain a foothold onto the deployed machine.

사이트의 취약점을 알아내기 위해서 글 위에서 제공한 `elfwhacker.bat`의 CGI 값을 찾아본다.

![cgi](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/cgi.png)

/cgi-bin/elfwhacker.bat에 취약점이 맞는지 확인하기 위해 파라미터 값에 `?&systeminfo` 입력해 명령어가 되는지 확인한다.

![sys](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/sys.png)

위 CGI 값이 취약한 것을 확인했다면 Metasploit 하기 위해 `msfconsole`을 실행 후 apache tomcat 9.0의 취약점을 이용하는 도구를 검색한다.

![search](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/search.png)

첫번째가 위에서 찾은 취약점이므로 `use 0` 입력 후 설정해야 할 값들을 확인한다.

![options](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/options.png)

`rhost` 값에는 공격할 IP 주소를 `lhost` 값에는 내 OpenVpn IP 값을 `targeturl`에 취약한 파라미터 값을 설정한 후 exploit을 한다.

![set](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/set.png)

meterpreter 환경을 얻었다는 것은 취약점 공격이 성공했다는 것이다.

### What are the contents of flag1.txt

> thm{whacking_all_the_elves}

디렉터리의 파일을 조사하면 flag를 얻을 수 있다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2012/image/flag.png)

### Looking for a challenge? Try to find out some of the vulnerabilities present to escalate your privileges!
