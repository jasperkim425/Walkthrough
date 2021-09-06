# Day 1 | Web Exploitation A Christmas Crisis 

## Workstation
- Virtual Box : VMware Fusion 12.1.0
- OS : kali-linux-2020.04

## Walkthrough

Having read the lengthy dossier,  you get ready to hack your way back into Santa's Christmas Control Centre! You enter the IP address at the top of the screen into your browser search bar and press enter to load the page.

Note: Remember that machines can take up to five minutes to boot up fully!

***

### Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open FireFox on the AttackBox and copy/paste the machines IP into the browser search bar.

![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/attackbox.png)

FireFox 브라우저를 열어 `attackbox`의 아이피 주소를 입력한다.

![ip](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/ip.png)

아래와 같은 화면이 나왔다. 계정이 없으므로 `Register`로 로그인한다.

![register](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/register.png)

### What is the name of the cookie used for authentication?

> auth

개발자 도구에서 Storage 부분을 들어가면 쿠키를 확인할 수 있다.

![auth](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/auth.png)

### In what format is the value of this cookie encoded?

> Hexadecimal

### Having decoded the cookie, what format is the data stored in?

> JSON

<img src="https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/hint.png" width="330px" height="100px" title="hint" alt="hint"></img><br/>

힌트에서 알려준 주소를 검색하면 Cyberchef 페이지가 나온다. 위에서 얻은 쿠키의 정보를 Input 부분에 입력하면 다음과 같은 결과를 얻을 수 있다.

![cyberchef](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/cyberchef.png)

Figure out how to bypass the authentication.



### What is the value of Santa's cookie?

> 7b22636f6d70616e79223a22546865204265737420466573746976616c20436f6d70616e79222c2022757365726e616d65223a2273616e7461227d

Recipe 부분을 To Hex로 변경 후 Input 영역에 위에서 얻은 결과를 입력하고 username 부분을 santa로 변경하여 쿠키 값을 얻는다.

Delimiter 부분을 None으로 설정해 준다.

![santa](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/santa.png)

Now that you are the santa user, you can re-activate the assembly line!



### What is the flag you're given when the line is fully active?

> THM{MjY0Yzg5NTJmY2Q1NzM1NjBmZWFhYmQy}

Storage 부분에서 쿠키 값을 위에서 얻은 santa 쿠키로 변경 후 새로고침을 해주면 flag를 얻을 수 있다.

![cookies](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/cookies.png)

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%201/image/flag.png)
