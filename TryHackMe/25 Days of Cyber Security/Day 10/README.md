# Day 10 | Networking Don't be sElfish!

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2010/image/attackbox.png)

Question #1 Using enum4linux, how many users are there on the Samba server (10.10.114.84)?

> 3

`enum4linux -U` 명령어를 사용해 User의 수를 찾을 수 있다.

```
$ sudo enum4linux -U 10.10.114.84
[sudo] password for jasper: 
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Tue Aug 31 04:10:31 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.114.84
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 10.10.114.84    |
 ==================================================== 
[+] Got domain/workgroup name: TBFC-SMB-01

 ===================================== 
|    Session Check on 10.10.114.84    |
 ===================================== 
[+] Server 10.10.114.84 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 10.10.114.84    |
 =========================================== 
Domain Name: TBFC-SMB-01
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ============================= 
|    Users on 10.10.114.84    |
 ============================= 
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: elfmcskidy       Name:   Desc: 
index: 0x2 RID: 0x3ea acb: 0x00000010 Account: elfmceager       Name: elfmceager        Desc: 
index: 0x3 RID: 0x3e9 acb: 0x00000010 Account: elfmcelferson    Name:   Desc: 

user:[elfmcskidy] rid:[0x3e8]
user:[elfmceager] rid:[0x3ea]
user:[elfmcelferson] rid:[0x3e9]
enum4linux complete on Tue Aug 31 04:10:48 2021

```

Question #2 Now how many "shares" are there on the Samba server?

> 4

`enum4linux -S` 명령어를 사용해 Share 된 디렉터리를 찾을 수 있다.

```
$ sudo enum4linux -S 10.10.114.84
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Tue Aug 31 04:13:47 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.114.84
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 10.10.114.84    |
 ==================================================== 
[+] Got domain/workgroup name: TBFC-SMB-01

 ===================================== 
|    Session Check on 10.10.114.84    |
 ===================================== 
[+] Server 10.10.114.84 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 10.10.114.84    |
 =========================================== 
Domain Name: TBFC-SMB-01
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ========================================= 
|    Share Enumeration on 10.10.114.84    |
 ========================================= 

        Sharename       Type      Comment
        ---------       ----      -------
        tbfc-hr         Disk      tbfc-hr
        tbfc-it         Disk      tbfc-it
        tbfc-santa      Disk      tbfc-santa
        IPC$            IPC       IPC Service (tbfc-smb server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        TBFC-SMB-01          TBFC-SMB

[+] Attempting to map shares on 10.10.114.84
//10.10.114.84/tbfc-hr  Mapping: DENIED, Listing: N/A
//10.10.114.84/tbfc-it  Mapping: DENIED, Listing: N/A
//10.10.114.84/tbfc-santa       Mapping: OK, Listing: OK
//10.10.114.84/IPC$     [E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
enum4linux complete on Tue Aug 31 04:14:13 2021

```

Question #3 Use smbclient to try to login to the shares on the Samba server (10.10.114.84). What share doesn't require a password?

> tbfc-santa

공유된 디렉터리를 smbclient 명령어를 사용해 접속해 본다.

![tbfc-hr](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2010/image/tbfc-hr.png)

![tbfc-it](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2010/image/tbfc-it.png)

![tbfc-santa](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2010/image/tbfc-santa.png)

tbfc-santa 디렉터리가 패스워드가 없이 접속됐다.

Question #4 Log in to this share, what directory did ElfMcSkidy leave for Santa?

> jingle-tunes

디렉터리를 조사하다가 텍스트 파일과 디렉터리를 발견했다.

텍스트 파일을 다운받은 후 jingle-tunes 디렉터리를 확인해 보니 아무것도 없었다.

![ls](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2010/image/ls.png)

발견한게 없으니 다운받은 텍스트 파일의 내용을 확인해 본다.

![cat](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2010/image/cat.png)

내용을 확인해 보니 jingle 파일에 Share 하기로 했다고 되어있다.
