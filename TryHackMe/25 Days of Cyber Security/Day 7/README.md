# Day 7 | Networking The Grinch Really Did Steal Christmas

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
<img src="https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/aoc-pcaps.png" width="450px" height="300px" title="aoc-pcaps" alt="aoc-pcaps"></img><br/>


Open "pcap1.pcap" in Wireshark. What is the IP address that initiates an ICMP/ping?

> 10.11.3.2

```
$ sudo wireshark pcap1.pcap
```

wireshark를 실행 후 아래로 스크롤 하다 보면 ICMP 프로토콜로 통신한 IP 주소를 찾을 수 있다.

![icmp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/icmp.png)

If we only wanted to see HTTP GET requests in our "pcap1.pcap" file, what filter would we use?

> http.request.method == GET

![get](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/get.png)

Now apply this filter to "pcap1.pcap" in Wireshark, what is the name of the article that the IP address "10.10.67.199" visited?

> reindeer-of-the-week

File → Export Objects → HTTP 를 클릭한다.

![export](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/export.png)

텍스트 파일 중 크기가 다른 파일 하나를 발견했다.

![text](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/text.png)

Let's begin analysing "pcap2.pcap". Look at the captured FTP traffic; what password was leaked during the login process?

There's a lot of irrelevant data here - Using a filter here would be useful!

> plaintext_password_fiasco

```
$ sudo wireshark pcap2.pcap
```

wireshark를 실행 후 FTP 트래픽을 확인하라고 되어있으니 다음 필터를 사용하여 FTP 프로토콜을 확인하면 password를 알 수 있다.

```
tcp.port == 21
```

![ftp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/ftp.png)

Continuing with our analysis of "pcap2.pcap", what is the name of the protocol that is encrypted?

> SSH

필터 적용을 해제 후 첫 번째 트래픽을 보면 SSH 프로토콜이 암호화되어 있다.

![ssh](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/ssh.png)

Analyse "pcap3.pcap" and recover Christmas!

What is on Elf McSkidy's wishlist that will be used to replace Elf McEager?

> Rubber ducky

pcap3.pcap 파일를 와이어샤크를 사용해 연다.

```
$ sudo wireshark pcap3.pcap
```

스크롤 하여 아래로 내려보면 HTTP 포트로 트래픽이 오고 간 흔적이 있다.

![pcap3](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/pcap3.png)

다음 필터를 적용하면 `christmas.zip` 파일이 있다. 

![christmas](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/christmas.png)

File → Export Objects → HTTP를 사용하여 해당 파일을 다운받는다.

![save](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/save.png)

다운받은 파일의 압축을 해제하고 파일을 읽는다.

![elf](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%207/image/elf.png)
