# Day 3 | Web Exploitation Christmas Chaos

## Workstation
- Virtual Box : VMware Fusion 12.1.0
- OS : kali-linux-2020.04

## Walkthrough
Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open Firefox on the AttackBox and copy/paste the machines IP (MACHINE_IP) into the browser search bar.

AttackBox를 실행한다.

![attackbox](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/attackbox.png)

실행한 AttackBox IP 주소를 Firefox에 검색한다.

![ip](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/ip.png)

Use BurpSuite to brute force the login form.  Use the following lists for the default credentials:

|Username|Password|
|:---:|:---:|
|root|root|
|admin|password|
|user|12345|

Use the correct credentials to log in to the Santa Sleigh Tracker app. Don't forget to turn off Foxyproxy once BurpSuite has finished the attack!

버프스위트(Burpsuite)를 실행한다.

```
$ Burpsuite
```

버프스위트를 실행한 뒤 프록시 설정을 한다.

파이어폭스(Firefox) → 설정(Preferences) → 네트워크 설정(Network Settings) → 프록시 설정(Manual proxy configuration)

![proxy](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/proxy.png)

프록시 설정을 했다면 Proxy → Intercept → Intercept is on 부분을 체크후 브라우저에서 admin amdin으로 로그인 시도 시 아래와 같은 화면이 나오는데 마우스 우클릭 하여 Send to Intruder를 클릭한다.

![intercept](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/intercept.png)

Positions 메뉴에서 Attack Type을 Cluster bomb을 설정한다.

![intruder](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/intruder.png)

Attack Type을 설정했다면 Payloads 메뉴로 넘어가 ID 부분을 공격할 Payload list를 위에 주어진 표를 확인하여 설정한다.

![id](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/id.png)

이번엔 Password 부분을 공격할 Payload도 표를 보고 설정한다.

![passwd](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/passwd.png)

모든 Payloads를 설정했다면 오른쪽 상단에 Start Attack 버튼을 클릭하면 길이 부분이 다른 하나를 찾을 수 있다.

![attack](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/attack.png)

What is the flag?

> THM{885ffab980e049847516f9d8fe99ad1a}

admin 12345로 로그인한다.

![login](https://github.com/jasperkim425/Walkthrough/tree/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%203/image/login.png)
