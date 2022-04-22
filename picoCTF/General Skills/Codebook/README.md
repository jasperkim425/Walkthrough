# Codebook
![Codebook](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/Cryptography/Mod%2026/image/Mod_26.png)

## Hint
1. On the webshell, use ls to see if both files are in the directory you are in
2. The str_xor function does not need to be reverse engineered for this challenge.

## Workstation
- Virtual Box : VMware Workstation 16
- OS : kali-linux-2022.1

## Walkthrough
문제를 풀기 위해 두개의 코드를 다운받는다.

![download](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/Cryptography/Mod%2026/image/Mod_26.png)

우선 다운받은 `code.py` 파일을 살펴보면 `codebook.txt` 파일을 이용해야 할 것 같다.

```
$ cat code.py

import random
import sys



def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])


flag_enc = chr(0x13) + chr(0x01) + chr(0x17) + chr(0x07) + chr(0x2c) + chr(0x3a) + chr(0x2f) + chr(0x1a) + chr(0x0d) + chr(0x53) + chr(0x0c) + chr(0x47) + chr(0x0a) + chr(0x5f) + chr(0x5e) + chr(0x02) + chr(0x3e) + chr(0x5a) + chr(0x56) + chr(0x5d) + chr(0x45) + chr(0x5d) + chr(0x58) + chr(0x31) + chr(0x0d) + chr(0x58) + chr(0x0f) + chr(0x02) + chr(0x5a) + chr(0x10) + chr(0x0e) + chr(0x5d) + chr(0x13)



def print_flag():
  try:
    codebook = open('codebook.txt', 'r').read()
    
    password = codebook[4] + codebook[14] + codebook[13] + codebook[14] +\
               codebook[23]+ codebook[25] + codebook[16] + codebook[0]  +\
               codebook[25]
               
    flag = str_xor(flag_enc, password)
    print(flag)
  except FileNotFoundError:
    print('Couldn\'t find codebook.txt. Did you download that file into the same directory as this script?')



def main():
  print_flag()



if __name__ == "__main__":
  main()

```

그리고 `codebook.txt` 파일도 확인한다.

```
$ cat codebook.txt
azbycxdwevfugthsirjqkplomn
```

영문자가 뒤섞여있다.

이 두개의 파일을 가지고 `python` 모듈을 사용하면 플래그를 얻을 수 있다.

```
$ python code.py codebook.txt
picoCTF{c0d3b00k_455157_d9aa2df2}
```

## Flag
> picoCTF{c0d3b00k_455157_d9aa2df2}
