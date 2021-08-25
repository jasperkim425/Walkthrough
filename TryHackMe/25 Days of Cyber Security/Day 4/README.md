# Day 4 | Web Exploitation Santa's watching

## Workstation
- Virtual Box : VMware Fusion 12.1.0
- OS : kali-linux-2020.04

## Walkthrough

Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open FireFox on the AttackBox and copy/paste the machines IP (MACHINE_IP) into the browser search bar.

![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%204/image/attackbox.png)

Given the URL "http://shibes.xyz/api.php", what would the entire wfuzz command look like to query the "breed" parameter using the wordlist "big.txt" (assume that "big.txt" is in your current directory)

Note: For legal reasons, do not actually run this command as the site in question has not consented to being fuzzed!

> wfuzz -c -z file,big.txt http://shibes.xyz/api.php?breed=FUZZ

Use GoBuster (against the target you deployed -- not the shibes.xyz domain) to find the API directory. What file is there?

> site-log.php

```
$ sudo gobuster dir -u http://10.10.18.178/ -w /usr/share/dirb/wordlists/big.txt
```

![gobuster](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%204/image/gobuster.png)

![api](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%204/image/api.png)

Fuzz the date parameter on the file you found in the API directory. What is the flag displayed in the correct post?

> THM{D4t3_AP1}

[wordlist](https://assets.tryhackme.com/additional/cmn-aoc2020/day-4/wordlist)를 다운 받는다.

다운로드한 wordlist를 wfuzz 명령어에 사용한다.

```
$ sudo wfuzz -c -z file,/home/jasper/Downloads/wordlist -u http://10.10.18.178/api/site-log.php?date=FUZZ
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://10.10.18.178/api/site-log.php?date=FUZZ
Total requests: 63

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                   
=====================================================================

000000019:   200        0 L      0 W        0 Ch        "20201118"                                                
000000001:   200        0 L      0 W        0 Ch        "20201100"                                                
000000003:   200        0 L      0 W        0 Ch        "20201102"                                                
000000007:   200        0 L      0 W        0 Ch        "20201106"                                                
000000015:   200        0 L      0 W        0 Ch        "20201114"                                                
000000022:   200        0 L      0 W        0 Ch        "20201121"                                                
000000021:   200        0 L      0 W        0 Ch        "20201120"                                                
000000020:   200        0 L      0 W        0 Ch        "20201119"                                                
000000018:   200        0 L      0 W        0 Ch        "20201117"                                                
000000017:   200        0 L      0 W        0 Ch        "20201116"                                                
000000006:   200        0 L      0 W        0 Ch        "20201105"                                                
000000011:   200        0 L      0 W        0 Ch        "20201110"                                                
000000013:   200        0 L      0 W        0 Ch        "20201112"                                                
000000005:   200        0 L      0 W        0 Ch        "20201104"                                                
000000009:   200        0 L      0 W        0 Ch        "20201108"                                                
000000012:   200        0 L      0 W        0 Ch        "20201111"                                                
000000010:   200        0 L      0 W        0 Ch        "20201109"                                                
000000008:   200        0 L      0 W        0 Ch        "20201107"                                                
000000014:   200        0 L      0 W        0 Ch        "20201113"                                                
000000016:   200        0 L      0 W        0 Ch        "20201115"                                                
000000002:   200        0 L      0 W        0 Ch        "20201101"                                                
000000004:   200        0 L      0 W        0 Ch        "20201103"                                                
000000025:   200        0 L      0 W        0 Ch        "20201124"                                                
000000023:   200        0 L      0 W        0 Ch        "20201122"                                                
000000046:   200        0 L      0 W        0 Ch        "20201215"                                                
000000047:   200        0 L      0 W        0 Ch        "20201216"                                                
000000037:   200        0 L      0 W        0 Ch        "20201206"                                                
000000029:   200        0 L      0 W        0 Ch        "20201128"                                                
000000044:   200        0 L      0 W        0 Ch        "20201213"                                                
000000045:   200        0 L      0 W        0 Ch        "20201214"                                                
000000035:   200        0 L      0 W        0 Ch        "20201204"                                                
000000043:   200        0 L      0 W        0 Ch        "20201212"                                                
000000042:   200        0 L      0 W        0 Ch        "20201211"                                                
000000036:   200        0 L      0 W        0 Ch        "20201205"                                                
000000039:   200        0 L      0 W        0 Ch        "20201208"                                                
000000040:   200        0 L      0 W        0 Ch        "20201209"                                                
000000041:   200        0 L      0 W        0 Ch        "20201210"                                                
000000038:   200        0 L      0 W        0 Ch        "20201207"                                                
000000034:   200        0 L      0 W        0 Ch        "20201203"                                                
000000033:   200        0 L      0 W        0 Ch        "20201202"                                                
000000027:   200        0 L      0 W        0 Ch        "20201126"                                                
000000024:   200        0 L      0 W        0 Ch        "20201123"                                                
000000048:   200        0 L      0 W        0 Ch        "20201217"                                                
000000032:   200        0 L      0 W        0 Ch        "20201201"                                                
000000050:   200        0 L      0 W        0 Ch        "20201219"                                                
000000031:   200        0 L      0 W        0 Ch        "20201130"                                                
000000030:   200        0 L      0 W        0 Ch        "20201129"                                                
000000026:   200        0 L      1 W        13 Ch       "20201125"                                                
000000028:   200        0 L      0 W        0 Ch        "20201127"                                                
000000054:   200        0 L      0 W        0 Ch        "20201223"                                                
000000061:   200        0 L      0 W        0 Ch        "20201230"                                                
000000063:   200        0 L      0 W        0 Ch        "http://10.10.18.178/api/site-log.php?date="              
000000058:   200        0 L      0 W        0 Ch        "20201227"                                                
000000057:   200        0 L      0 W        0 Ch        "20201226"                                                
000000062:   200        0 L      0 W        0 Ch        "20201231"                                                
000000055:   200        0 L      0 W        0 Ch        "20201224"                                                
000000060:   200        0 L      0 W        0 Ch        "20201229"                                                
000000053:   200        0 L      0 W        0 Ch        "20201222"                                                
000000059:   200        0 L      0 W        0 Ch        "20201228"                                                
000000056:   200        0 L      0 W        0 Ch        "20201225"                                                
000000051:   200        0 L      0 W        0 Ch        "20201220"                                                
000000052:   200        0 L      0 W        0 Ch        "20201221"                                                
000000049:   200        0 L      0 W        0 Ch        "20201218"                                                

Total time: 0
Processed Requests: 63
Filtered Requests: 0
Requests/sec.: 0
```

20201125가 특별해 보이니 해당 payload를 파라미터 값에 넣어 검색하면 플래그를 얻을 수 있다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%204/image/flag.png)
