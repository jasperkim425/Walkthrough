# Obedient Cat
![Obedient_Cat](https://github.com/jasperkim425/Walkthrough/tree/main/picoCTF/General%20Skills/Obedient%20Cat/image/Obedient_Cat.png)

## Hint
1. Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.
2. To get the file accessible in your shell, enter the following in the Terminal prompt: $ wget https://mercury.picoctf.net/static/33996e32dce022205a6a36f69aba56f0/flag
3. $ man cat

## Workstation
- Virtual Box : VMware Fusion 12.1.1
- OS : kali-linux-2021.1

## Walkthrough
Download flag 버튼을 클릭해 flag 파일을 다운받는다.
![flag](https://github.com/jasperkim425/Walkthrough/tree/main/picoCTF/General%20Skills/Obedient%20Cat/image/flag.png)

다운로드가 완료되면 터미널에서 다운로드 디렉터리로 이동해 내용을 확인한다.

```
$ cd Downloads
$ cat flag
```

![cat_flag](https://github.com/jasperkim425/Walkthrough/tree/main/picoCTF/General%20Skills/Obedient%20Cat/image/cat_flag.png)

## Flag
> picoCTF{s4n1ty_v3r1f13d_2aa22101}