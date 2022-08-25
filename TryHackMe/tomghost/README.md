# tomghost

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
### Task 1 | Flags

![machine]()

nmap을 사용해 포트를 스캔한다.

![nmap]()

22, 53, 8009, 8080번 포트가 있다.

searchsploit을 사용해 스캔한 포트 8009번과 8080번의 취약점을 찾아본다.

![searchsploit]()

아무것도 검색되지 않는다. 8080번이 http 서비스이므로 10.10.90.15:8080으로 접속하면 아래와 같이 나온다.

![http]()

위와 같이 디폴트 페이지 같은 것이 나온다. 
아직 알아낸게 없으므로 8009번 포트의 Apache Jserv를 구글로 알아본다.

![google]()

구글에 검색하면 Ghostcat 취약점이 발표된 것이 있다.

Ghostcat 취약점이란 Tomcat 서버의 Webapp 하위 모든 파일을 읽을 수 있는 취약점이다. 

그럼 다시 한번 searchsploit에서 ghostcat 취약점을 알아본다.

![ghostcat]()

하나의 취약점을 두가지 방법으로 진행할 수 있다.

나는 Metasploit을 이용할 것이다.

우선 msfconsole을 접속한 후에 ghostcat에 대해 검색하여 알맞게 옵션을 입력해 주면 된다.

![msf]()

rhosts의 옵션만 입력한 후에 실행하면 아래와 같이 /WEB-INF/web.xml 파일을 읽어왔다.

![run]()

읽어온 아래부분을 확인하면 skyfuck과 문자열이 있는데 이것은 아이디와 패스워드 같아 보인다.

아이디와 패스워드가 맞는지 확인하기 위해 22번 포트에 열려있던 ssh를 이용하면 성공적으로 로그인이 된 것을 알 수 있다.

![ssh]()

그럼 user.txt 파일을 찾아보는데 이상한 파일 2개가 있는것을 알 수 있다.

두개의 파일은 뒤로하고 우선 user.txt 파일을 먼저 찾아보면 merlin이란 계정 안에 user.txt 파일이 있다.

![user]()

그럼 다시 skyfuck 안에 있는 2개의 파일을 보면 .asc와 .pgp 파일의 사용법을 알아본다.

https://superuser.com/questions/46461/decrypt-pgp-file-using-asc-key 사이트에서 두 파일의 사용법을 알았다.

![gpg]()

위와 같이 pgp 파일의 암호화를 풀려면 암호가 필요한데 skyfuck 계정의 암호로는 풀리지 않았다.

https://www.openwall.com/lists/john-users/2015/11/17/1 에서 asc 파일 크랙 방법이 있다.

asc 파일의 크랙을 시도하는데 gpg2john 파일이 검색되지 않는다.

![gpg2john]()

그럼 두 파일을 kali로 가져와 gpg2john을 사용하여 크랙한다.

![scp]()

두 파일을 가져오면 총 3개의 파일을 가져오게 된다. hash 파일은 ubuntu skyfuck 계정에서 생성된 파일로 삭제 후 다시 진행한다.

![rm]()

![john]()

크랙 결과 암호 하나를 얻었다.

다시 ubuntu로 돌아가 pgp 파일의 암호화를 해독할 때 위에서 구한 암호를 입력해 주면 merlin 계정의 암호를 구할 수 있다.

![merlin]()

merlin 계정으로 접속해 사용가능한 명령어를 확인하면 zip 파일이 root 권한으로 실행된다.

권한 상승을 위해 https://gtfobins.github.io/gtfobins/zip/#sudo 를 참고하여 명령어를 입력해 주면 root 권한을 얻는다.

![root]()

Compromise this machine and obtain user.txt

> THM{GhostCat_1s_so_cr4sy}

Escalate privileges and obtain root.txt

> THM{Z1P_1S_FAKE}