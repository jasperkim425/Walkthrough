# Redeemer

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough
* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_Point_<ID>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/machine.png)

* Task 1 | Which TCP port is open on the machine?

> 6379

`nmap`을 사용해 포트 스캔을 했는데 1000 포트까지는 아무것도 검색이 되지 않았다. 

그래서 추가로 10000 포트 까지 검색했더니 포트가 나왔다. 

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/nmap.png)

* Task 2 | Which service is running on the port that is open on the machine?

> redis

* Task 3 | What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database

> In-memory Database

구글에 redis를 검색해 홈페이지를 들어갔더니 정답이 나와있다. 

![in](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/in.png)

* Task 4 | Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.

> redis-cli

다시 구글에 command-line redis를 검색하면 다시 한번 홈페이지에서 정답을 얻었다.

![cli](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/cli.png)

* Task 5 | Which flag is used with the Redis command-line utility to specify the hostname?

> -h

* Task 6 | Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?

> INFO

홈페이지에서 command를 확인한다.

![info](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/info.png)

* Task 7 | What is the version of the Redis server being used on the target machine?

> 5.0.7

* Task 8 | Which command is used to select the desired database in Redis?

> SELECT

![select](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/select.png)

* Task 9 | How many keys are present inside the database with index 0?

> 4

redis-cli를 설치한다.

```
sudo apt install redis-cli
```

그리고 아래와 같이 입력해 터미널에 접속한다.

```
sudo redis-cli -h 10.129.90.247
```

`INFO` 명령어를 사용하면 아래에 key 개수가 나와있다.

![key](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/key.png)

* Task 10 | Which command is used to obtain all the keys in a database?

> KEYS * 

![keys](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/keys.png)

* Submit Flag | Submit root flag 

> 03e1d2b376c37ab3f5319922053953eb

`KEYS *` 명령어를 입력해 키를 얻는다.

`"flag"`라는 키를 `GET`으로 데이터를 확인한다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/HackTheBox/Starting%20Point/Redeemer/image/flag.png)



