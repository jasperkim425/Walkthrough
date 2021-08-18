# Wave a flag
![Wave_a_flag](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Wave%20a%20flag/image/Wave_a_flag.png)

## Hint
1. This program will only work in the webshell or another Linux computer.
2. To get the file accessible in your shell, enter the following in the Terminal prompt: $ wget https://mercury.picoctf.net/static/cfea736820f329083dab9558c3932ada/warm
3. Run this program by entering the following in the Terminal prompt: $ ./warm, but you'll first have to make it executable with $ chmod +x warm
4. -h and --help are the most common arguments to give to programs to get more information from them!
5. Not every program implements help features like -h and --help.

## Workstation
- Virtual Box : VMware Fusion 12.1.1
- OS : kali-linux-2021.1

## Walkthrough
This program 버튼을 클릭해 warm 프로그램 파일을 다운받을 수 있다.

![warm](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Wave%20a%20flag/image/warm.png)

warm은 프로그램 파일이므로 실행 권한을 부여한다.

```
$ sudo chmod +x warm
```

![chmod](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Wave%20a%20flag/image/chmod.png)


실행 권한을 부여한 후에 warm 파일을 실행시키면 -h 옵션을 이용해 사용법을 알아내라고 한다.

```
$ ./warm -h
```

![warm_help](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/General%20Skills/Wave%20a%20flag/image/warm_help.png)

## Flag
> picoCTF{b1scu1ts_4nd_gr4vy_30e77291}
