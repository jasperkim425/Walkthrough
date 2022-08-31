# Investigating Windows

## Workstation
- Virtual Box : TryHackMe

## Walkthrough
### Task 1 | Investigating Windows

This is a challenge that is exactly what is says on the tin, there are a few challenges around investigating a windows machine that has been previously compromised.

Connect to the machine using RDP. The credentials the machine are as follows:

Username: Administrator
Password: letmein123!

Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

---

Whats the version and year of the windows machine?

> Windows server 2016

Start Machine을 누르면 windows machine이 하나 생성된다.

![machine]()

그럼 시작 메뉴 -> 설정(Settings) -> 시스템(System) -> 정보(About)를 확인하면 윈도우 버전을 알 수 있다.

![version]()

Which user logged in last?

> Administrator

시작 메뉴에서 Command Prompt를 시작하면 Administrator 폴더가 나오는것을 보아 Administrator이 마지막 로그인한 계정인 것 같다.

![admin]()

When did John log onto the system last?

Answer format: MM/DD/YYYY H:MM:SS AM/PM

> 03/02/2019 5:48:32 PM

John 계정을 찾기 위해 cmd에서 net users 명령어를 입력해 계정 목록을 확인한다.

![netusers]()

John 계정의 마지막 접속시간을 확인한다.

![john]()

What IP does the system connect to when it first starts?

> 10.34.2.3

첫번째 질문에서 시작할 때 연결된 IP 주소가 나와있다.

![start]()

What two accounts had administrative privileges (other than the Administrator user)?

Answer format: username1, username2

> Jenny, Guest

administrator의 그룹에 속해 있는 계정을 확인한다.

![localgroup]()

Whats the name of the scheduled task that is malicous.

> Clean file system

Task scheduled을 확인하기 위해 시작 메뉴에서 마우스 우클릭 -> Computer Management -> System Tools -> Task Management의 라이브러리를 확인한다.

그 중 매일 실행되는 두개의 스케쥴이 있는데 clean file system이란 스케쥴이 매우 악의적인 것 같다.

![task]()

What file was the task trying to run daily?

> nc.ps1

그럼 위에서 확인한 스케쥴이 어떤 프로그램을 실행시키는지 확인하기 위해 Actions 메뉴로 이동한다.

![actions]()

What port did this file listen locally for?

> 1348

위에서 nc.ps1 -l 1348이란 명령어가 있었는데 nc 프로그램을 1348번 포트로 리스닝하겠다는 명령어이다.

When did Jenny last logon?

> Never

net users에 있던 jenny의 마지막 접속 시간을 확인한다.

![jenny]()

At what date did the compromise take place?

Answer format: MM/DD/YYYY

> 03/02/2019

위에서 마지막 세팅을 확인한다.

At what time did Windows first assign special privileges to a new logon?

Answer format: MM/DD/YYYY HH:MM:SS AM/PM

> 03/02/2019 04:04:49 PM

special login을 확인하기 위해 시작 메뉴 우클릭 -> Event Viewer -> Windows logs -> Security 에서 03/02/2019에서 시간을 찾으면 되는데,

이때 힌트를 보자면 49초를 확인하라고 한다. 03/02/2019에서 49초로 끝나는 것을 찾는다.

![special]()

What tool was used to get Windows passwords?

> mimikatz

아까 Task Management에서 Gameover에서 Actions를 확인해 보면 mim.exe 파일을 실행해 logonPassword를 가져와 o.txt 파일에 저장하는 명령어가 있다.

![game]()

C:\TMP\ 경로로 찾아가 o.txt 파일을 확인해 보면 어떤 프로그램을 사용했는지 나와있다.

![o]()

What was the attackers external control and command servers IP?

> 76.32.97.132

무슨 IP 서버를 이용했는지 알기 위해 hosts에 등록된 목록을 확인한다.

경로는 C:\Windows\System32\drivers	\etc\hosts에 있다.

google.com이 공격 ip로 사용한 것 같다.

![hosts]

What was the extension name of the shell uploaded via the servers website?

> .jsp

웹 서버에 쉘을 업로드 하기 위해 어떤 파일을 사용했는지 묻고 있으니 웹서버 파일에 들어가 확인하면 된다.

경로는 C:\inetpub\wwwroot 에 들어가면 .jsp 파일이 2개나 있다.

![jsp]()

What was the last port the attacker opened?

> 1337

공격자가 열어논 포트 확인을 위해 힌트를 보면 Firewall 방화벽을 확인하라고 한다.

그럼 방화벽 설정으로 들어가 Inbound Rules에서 Allow outside connections for development를 확인하면 된다.

![port]()

Check for DNS poisoning, what site was targeted?

> google.com

hosts 목록에서 확인했다.