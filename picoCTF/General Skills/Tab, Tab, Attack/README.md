# Tab, Tab, Attack
![Tab](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Tab%2C%20Tab%2C%20Attack/image/Tab.png)

## Hint
1. After `unzip`ing, this problem can be solved with 11 button-presses...(mostly Tab)...

## Workstation
- Virtual Box : VMware Fusion 12.1.1
- OS : kali-linux-2021.1

## Walkthrough
Addadshashanammu.zip 파일을 다운받는다.

<img src="https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Tab%2C%20Tab%2C%20Attack/image/Addadshashanammu.png" width="500px" height="300px" title="Addadshashanammu" alt="Addadshashanammu"></img><br/>

다운로드 디렉토리로 이동해 다운받은 zip 파일의 압축을 해제한다.

```
$ cd Downloads
$ unzip Addadshashanammu.zip
```

![unzip](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Tab%2C%20Tab%2C%20Attack/image/unzip.png)

디렉토리 하나를 얻을 수 있다.

위와 같이 해당 디렉토리의 하위 디렉토리의 끝까지 이동하면 fang-of-haynekhtnamet 파일이 있다.

파일을 실행시키면 플래그를 얻을 수 있다.

![fang-of-haynekhtnamet](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Tab%2C%20Tab%2C%20Attack/image/fang-of-haynekhtnamet.png)

## Flag
> picoCTF{l3v3l_up!_t4k3_4_r35t!_76266e38}
