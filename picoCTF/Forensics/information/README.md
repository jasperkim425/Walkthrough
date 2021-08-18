# information
![information](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/Forensics/information/image/information.png)

## Hint
1. Look at the details of the file
2. Make sure to submit the flag as picoCTF{XXXXX}

## Workstation
- OS : macOS Big Sur

## Walkthrough
cat.jpg 파일을 다운받는다.

[https://29a.ch/photo-forensics/](https://29a.ch/photo-forensics/) 이 사이트에서 사진 포렌식을 진행할 수 있다.

다운받은 파일을 오픈하면 String Extraction 탭에서 사진에 대한 문자열을 추출할 수 있다.

![forensics](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/Forensics/information/image/forensics.png)

resource 부분에 cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9의 코드가 이상하다.

[https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/) 사이트에서 Base64 데이터를 디코딩할 수 있다.

![CyberChef](https://github.com/jasperkim425/Walkthrough/blob/main/picoCTF/Forensics/information/image/CyberChef.png)

## Flag
> picoCTF{the_m3tadata_1s_modified}
