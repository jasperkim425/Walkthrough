# CTF collection Vol.1

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.3

# Walkthrough
## Task 1 | Author note
Just another random CTF room created by me. Well, the main objective of the room is to test your CTF skills. For your information, vol.1 consists of 20 tasks and all the challenges are extremely easy. Stay calm and Capture the flag. :)

Note: All the challenges flag are formatted as THM{flag}, unless stated otherwise

## Task 2 | What does the base said?
Can you decode the following?

VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==

***

### Feed me the flag!

> THM{ju57_d3c0d3_7h3_b453}

아래 사이트에서 base64로 암호화된 코드를 해독할 수 있다.<br>
https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true)

![task1flag]()

## Task 3 | Meta meta 
Meta! meta! meta! meta...................................

***

### I'm hungry, I need the flag.

> THM{3x1f_0r_3x17}

파일을 다운받는다.

![task3file]()

다운받은 파일을 `exiftool` 모듈을 사용해 flag를 찾는다.

![task3flag]()

## Task 4 | Mon, are we going to be okay?
Something is hiding. That's all you need to know.

***

### It is sad. Feed me the flag.

> THM{500n3r_0r_l473r_17_15_0ur_7urn}

파일을 다운받는다.

![task4file]()

스테가노그래피 수법을 이용해 이미지 안에 텍스트를 암호화 했다.<br>
https://futureboy.us/stegano/decinput.html 이 사이트에서 이미지를 해독하여 flag를 얻을 수 있다.

![task4decode]()

![task4flag]()

## Task 5 | Erm......Magick
Huh, where is the flag?

***

### Did you find the flag?

> THM{wh173_fl46}

문제를 드래그하면 flag를 얻을 수 있다.

![task5flag]()

## Task 6 | QRrrrr
Such technology is quite reliable.

***

### More flag please!

> THM{qr_m4k3_l1f3_345y}

파일을 다운받는다.

![task6file]()

다운받은 QR 코드 이미지 파일은 아래 사이트에서 스캔할 수 있다.

https://4qrcode.com/scan-qr-code.php

![task6flag]()

## Task 7 | Reverse it or read it?
Both works, it's all up to you.

***

### Found the flag?

> THM{345y_f1nd_345y_60}

파일을 다운받는다.

![task7file]()

해당 파일이 깨져서 나오지만 잘 읽어보면 중간에 깨지지 않은 문자들 중에 flag를 얻을 수 있다.

![task7flag]()

## Task 8 | Another decoding stuff
Can you decode it?

3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L

***

### Oh, Oh, Did you get it?

> THM{17_h45_l3553r_l3773r5}

암호화 된 코드는 `https://gchq.github.io/CyberChef/#recipe=From_Base58('123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz',true)` 사이트에서 해독할 수 있다.

![task8flag]()

## Task 9 | Left or right
Left, right, left, right... Rot 13 is too mainstream. Solve this

MAF{atbe_max_vtxltk}

***

### What did you get?

> THM{hail_the_caesar}

해당 코드는 https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13) 해독할 수 있다.

![task9rot13]()

하지만 값이 나오지 않아 Amount 값을 계속 올렸더니 33에서 flag를 얻을 수 있었다.

![task9flag]()

## Task 10 | Make a comment
No downloadable file, no ciphered or encoded text. Huh .......

***

### I'm hungry now... I need the flag

> THM{4lw4y5_ch3ck_7h3_c0m3mn7}

해당 페이지에서 마우스 우클릭 후 Inspect Element를 클릭한다.

왼쪽에 마우스 버튼을 클릭 후 문제를 클릭하면 flag를 얻을 수 있다.

![task10flag]()

## Task 11 | Can you fix it?
I accidentally messed up with this PNG file. Can you help me fix it? Thanks, ^^

***

### What is the content?

> THM{y35_w3_c4n}

파일을 다운받는다.

![task11file]()

다운받은 파일을 열려고 png 파일의 헤더가 달라 열리지 않는다. 그래서 `xxd` 모듈을 사용해 hex 코드로 변환시켜준 파일을 텍스트 파일로 저장한다.

![task11xxd]()

![task11head]()

저장된 파일의 헤더를 나노 편집기로 png 시그니처인 `89504E470D0A1A0A`로 변경해준다.

![task11nano]()

변경한 파일을 https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')Render_Image('Raw') 사이트에서 파일을 열면 플래그를 얻을 수 있다.

![task11flag]()

## Task 12 | Read it 
Some hidden flag inside Tryhackme social account.

***

### Did you found the hidden flag?

> THM{50c14l_4cc0un7_15_p4r7_0f_051n7}

구글에 Tryhackme room reddit 이라고 검색한다.

https://www.reddit.com/r/tryhackme/comments/eizxaq/new_room_coming_soon/ 사이트에서 찾을 수 있다.

![task12flag]()

## Task 13 | Spin my head
What is this?

++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>++++++++++++++.------------.+++++.>+++++++++++++++++++++++.<<++++++++++++++++++.>>-------------------.---------.++++++++++++++.++++++++++++.<++++++++++++++++++.+++++++++.<+++.+.>----.>++++.

***

### Can you decode it?

> THM{0h_my_h34d}

https://www.dcode.fr/brainfuck-language 사이트에서 암호를 해독할 수 있다.

![task13flag]()

## Task 14 | An exclusive!
Exclusive strings for everyone!

S1: 44585d6b2368737c65252166234f20626d<br>
S2: 1010101010101010101010101010101010

***

### Did you crack it? Feed me now!

> THM{3xclu51v3_0r}

http://xor.pw/ 온라인 XOR 계산 사이트에서 두개의 코드를 입력하면 플래그를 얻을 수 있다.

![task14flag]()

## Task 15 | Binary walk
Please exfiltrate my file :)

***

### Flag! Flag! Flag!

> THM{y0u_w4lk_m3_0u7}

파일을 다운받는다.

![task15file]()

다운받은 파일을 `binwalk -e` 명령어를 사용해 자동으로 파일 안에 있는 내부파일을 추출하면 텍스트 파일을 찾았다.

![task15binwalk]()

텍스트 파일을 확인하면 플래그를 얻을 수 있다.

![task15flag]()

## Task 16 | Darkness
There is something lurking in the dark.

***

### What does the flag said?

> THM{7h3r3_15_h0p3_1n_7h3_d4rkn355}

파일을 다운받는다.

![task16file]()

`stegsolve`라는 모듈을 설치해야한다. 아래와 같이 명령어를 입력해 다운받는다.

```
$ wget http://www.caesum.com/handbook/Stegsolve.jar -O stegsolve.jar
$ chmod +x stegsolve.jar
```

![task16stegsolve]()

실행 권한을 줬다면 해당 파일을 실행 후 다운받은 이미지를 연다.

`./stegsolve.jar`

![task16open]()

해당 이미지를 추적하면 플래그를 얻을 수 있다.

![task16flag]()

## Task 17 | A sounding QR
How good is your listening skill?

P/S: The flag formatted as THM{Listened Flag}, the flag should be in All CAPS

***

### What does the bot said?

> THM{SOUNDINGQR}

파일을 다운받는다.

![task17file]()

다운받은 파일을 https://4qrcode.com/scan-qr-code.php 사이트에서 스캔하면 웹사이트 하나를 얻을 수 있다.

![task17qr]()

해당 웹사이트의 사운드에서 플래그를 얻을 수 있다.

![task17flag]()

## Task 18 | Dig up the past
Sometimes we need a 'machine' to dig the past

Targetted website: https://www.embeddedhacker.com/<br>
Targetted time: 2 January 2020

### Did you found my past?

> THM{ch3ck_th3_h4ckb4ck}

https://archive.org/web/ 사이트에서 웹페이지의 히스토리를 찾을 수 있다.

![task18web]()

위에서 주어진 사이트를 검색해 시간을 검색하면 플래그를 얻을 수 있다.

![task18flag]()

## Task 19 | Uncrackable!
Can you solve the following? By the way, I lost the key. Sorry >.<

MYKAHODTQ{RVG_YVGGK_FAL_WXF}

Flag format: TRYHACKME{FLAG IN ALL CAP}

***

### The deciphered text

> TRYHACKME{YOU_FOUND_THE_KEY}

https://www.dcode.fr/vigenere-cipher 사이트에서 문제를 해결할 수 있다. 주어진 문제를 입력 후 AUTOMATIC DECRYPTION를 입력하면 자동으로 해독하는데 그 중 플래그 있다.

![task19flag]()

## Task 20 | Small bases
Decode the following text.

581695969015253365094191591547859387620042736036246486373595515576333693

***

### What is the flag?

> THM{17_ju57_4n_0rd1n4ry_b4535}

https://www.rapidtables.com/convert/number/decimal-to-hex.html 사이트에서 코드를 Hex 코드로 변환할 수 있다.

![task20hex]()

변환되어 나온 Hex 코드를 https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto') 사이트에서 변환하면 플래그를 얻을 수 있다.

![task20flag]()

## Task 21 | Read the packet
I just hacked my neighbor's WiFi and try to capture some packet. He must be up to no good. Help me find it.

***

### Did you captured my neighbor's flag?

> THM{d0_n07_574lk_m3}

파일을 다운받는다.

![task21file]()

다운받은 파일을 `wireshark`로 연다.

`$ wireshark flag.pcapng`

와이어샤크에서 `tcp.stream eq 42` 필터를 입력하면 /flag.txt 파일을 GET 요청으로 가져온 흔적이 있다. 우리는 /flag.txt 오픈된 파일을 확인하면 플래그를 얻을 수 있다.

![task21flag]()