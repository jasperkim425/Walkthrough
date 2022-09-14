# Sequel

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough

* CONNECT | Connect to Starting Point VPN before starting the machine

```
sudo openvpn starting_point_<id>.ovpn
```

* Spawn Machine | Click to Spawn the machine

![machine]()

* Task 1 | What does the acronym SQL stand for?

> Structured Query Language

* Task 2 | During our scan, which port running mysql do we find?

> 3306

nmap을 사용해 포트 검색을 한다.

![nmap]()

* Task 3 | What community-developed MySQL version is the target running?

> MariaDB

* Task 4 | What switch do we need to use in order to specify a login username for the MySQL service?

> -u

`sudo mysql --help` 명령어를 사용해 username 옵션을 찾는다.

![user]()


* Task 5 | Which username allows us to log into MariaDB without providing a password?

> root

* Task 6 | What symbol can we use to specify within the query that we want to display everything inside a table?

> *

* Task 7 | What symbol do we need to end each query with?

> ;

* Submit Flag | Submit root flag 

> 7b4bec00d1a39e3dd4e021ec3d915da8

`mysql`에 접속한다.

![mysql]()

이제 플래그를 찾으면 된다.

데이터베이스를 선택하여 테이블 목록을 확인한다. 그리고 테이블의 모든 것을 조회하면 된다.

![flag]()