# Agent Sudo

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
### Task 1 Author note
Welcome to another THM exclusive CTF room. Your task is simple, capture the flags just like the other CTF room. Have Fun!

If you are stuck inside the black hole, post on the forum or ask in the TryHackMe discord.

---

Deploy the machine

![machine]()

### Task 2 Enumerate
Enumerate the machine and get all the important information

---

How many open ports?

> 3

nmap을 사용해 열려있는 포트를 검색한다.

![nmap]()

21, 22, 80번 총 3개의 포트가 열려있다.

How you redirect yourself to a secret page?

> user-agent

80번 http 포트가 열려있으니 브라우저에 IP 주소를 검색하면 아래 사진과 같이 나온다.

![http]()

user-agent에 코드네임을 입력해 사이트를 접속할 수 있다고 되어 있다.

What is the agent name?

> chris

user-agent에 코드네임을 바꾸기 위해 FoxyProxy를 이용한다.(구글에 foxyproxy를 검색하면 Firefox 용 foxyproxy를 다운받을 수 있다.)

다운이 완료되면 오른쪽 상단에 여우 아이콘이 생긴다. 클릭해 options를 눌러 아래 사진과 같이 설정한다.

![foxyset]()

설정을 완료한 후 다시 오른쪽 상단에서 proxy를 눌러 활성화 시켜준다.

![foxyon]()

활성화가 되었다면 터미널에서 burpsuite를 실행시켜준다.

```
sudo burpsuite
```

burpsuite의 Proxy 탭의 Options에서 Proxy Listeners 부분에 아까 설정한 foxyproxy 설정과 같이 되어 있는지 확인한다.

![burpset]()

모든 설정이 완료되었다면 Intercept is off를 on으로 변경한 후 브라우저를 새로고침하면 아래와 같이 burpsuite가 인터셉트해 온다.

우클릭하여 Sent to Intruder를 클릭하면 Intruder 탭으로 이동하게 된다.

![sent]()

Intruder 탭으로 이동하여 User-Agent 부분을 공격하여 코드네임을 찾아낼 것이다.

그럴려면 우선 User-Agent 부분을 모두 지운 후 A를 입력한다.

A를 입력한 후 $Add$를 입력하여 공격할 부분을 정한다.

![add]()

Payloads 탭으로 들어가 공격할 범위를 Payloads Options에서 정하는데, 아까 웹사이트에서 agent R 이라고 했으니 코드네임은 대문자 알파벳인것 같다. 

그럼 A부터 R까지 입력하여 공격을 해본다.

모두 입력했다면 오른쪽 상단에 Start Attack를 입력하면 A부터 R까지 공격을 시도한다.

![payload]()

C에서 길이가 다른 것을 확인했다.

![attack]()

그럼 다시 Proxy 탭으로 돌아가 User-Agent에 C를 입력해 Forward를 눌러주면 /agent_C_attention.php 페이지로 이동하게 된다.

그럼 다시 Forward를 눌러주면 페이지가 /agent_C_attention.php로 이동하게 된다.

![c]()

![agentc]()

이로써 agent C의 이름인 chris를 알아냈다.

![httpc]()

### Task 3 Hash cracking and brute-force
Done enumerate the machine? Time to brute your way out.

---

FTP password

> crystal

fpt 접속을 위해 위 과정에서 알아낸 chris 이름으로 접속을 시도한다.

![ftp]()

패스워드가 걸려있어 chris를 입력해 봤지만 접속이 되지 않는다.

패스워드 알아내기 위해 hydra를 이용한다. 그 전에 passwords 위치를 확인한 후 hydra를 실행하면 패스워드를 얻는다..

![hydra]()

Zip file password

> alien

chris 계정의 패스워드 crystal을 알아냈으니 ftp를 접속한다.

ftp에는 총 3개의 파일이 있는데 모든 파일을 kali로 가져온다.

![ftpmget]()

txt 파일의 내용을 보면 이미지는 모두 가짜이며, 이미지 내 디렉터리 안에 진짜 이미지가 있다고 한다.

![cat]()

이미지에 숨겨져 있는 디렉터리를 찾기 위해 binwalk를 사용해 두 이미지를 확인하면 디렉터리 하나를 얻었다.

![binwalk]()

디렉터리 내부에 있는 txt 파일은 아무것도 없고 zip 파일을 암호가 걸려있다.

![cd]()

zip 파일의 패스워드를 크랙한다.

john으로는 패스워드가 추출되지 않아 --show 옵션으로 패스워드를 확인한다.

![john]()

alien이라는 패스워드를 알아냈다.

steg password

> Area51

zip 파일의 패스워드로 zip 파일을 압축해제 하면 아무것도 없던 To_agentR.txt에 내용이 생기며 이상한 코드가 있었다.

![unzip]()

![code]()

이상한 코드를 CyberChef에서 자동으로 코드 해석을 해주는데, Base64로 풀 수 있다.

![cyberchef]

Who is the other agent (in full name)?

> james

SSH password

> hackerrules!

이 코드로 아직 사용하지 않은 이미지 cute-alien.jpg 파일에 숨어있는 파일을 steghide로 확인하면 message.txt 파일이 숨겨져 있었다. 

이 파일 또한 암호가 걸려있지만 위에서 얻은 코드로 암호를 풀 수 있다.

message.txt 파일에서 새로운 agent의 이름인 james와 패스워드를 얻었다. 

![steghide]()

### Task 4 Capture the user flag
You know the drill.

---

What is the user flag?

> b03d975e8c92a7c04146cfa7a5a313c7

ssh를 james 계정으로 접속해 user 플래그를 찾는다.

![userflag]()

What is the incident of the photo called?

> Roswell alien autopsy

flag 외에 이미지 파일이 하나 있다. 해당 이미지를 kali로 가져와 구글에 이미지 검색을 하고 Foxnews에서 이미지 제목을 알아낼 수 있다.

```
sudo scp james@10.10.17.60:/home/james/Alien_autospy.jpg .
```

위와 같이 명령어를 입력하면 이미지 파일을 가져올 수 있다.

이 이미지를 구글에 검색하면 이미지 제목을 알 수 있다.

![google]()

![photo]()

### Task 5 Privilege escalation 
Enough with the extraordinary stuff? Time to get real.

---

CVE number for the escalation 

(Format: CVE-xxxx-xxxx)

> CVE-2019-14287

root 계정 접속을 위해 sudo -l 확인해보면 아래와 같이 나왔다.

![sudo-l]()

해당 코드의 취약점을 알아내기 위해 구글링하면 취약점이 있었다.

https://www.exploit-db.com/exploits/47502 에 나온대로 명령어를 입력하면 root 권한을 얻을 수 있다.

![root]()

What is the root flag?

> b53a02f55b57d4439e3341834d70c062

/root 폴더로 이동해 플래그를 확인한다.

![rootflag]()

(Bonus) Who is Agent R?

> DesKel

rootflag.txt 파일 아래에 Agent R의 이름이 있다.