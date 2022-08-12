# 우리는 이 파일에 플래그를...

![main](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%9A%B0%EB%A6%AC%EB%8A%94%20%EC%9D%B4%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%94%8C%EB%9E%98%EA%B7%B8%EB%A5%BC/image/main.png)

## Workstation
* OS : Window 10

## Walkthrough

우선 플래그 파일을 다운받는다.

다운받은 플래그 파일을 메모장으로 열어보면 내용이 깨져나온다.

![file](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%9A%B0%EB%A6%AC%EB%8A%94%20%EC%9D%B4%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%94%8C%EB%9E%98%EA%B7%B8%EB%A5%BC/image/file.png)

flag 파일이 원래 무슨 파일이였는지 알기 위해 https://www.checkfiletype.com/ 사이트에서 파일을 검색한다.

이 파일은 원래 gz으로 압축된 파일이였다.

![checkfiletype](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%9A%B0%EB%A6%AC%EB%8A%94%20%EC%9D%B4%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%94%8C%EB%9E%98%EA%B7%B8%EB%A5%BC/image/checkfiletype.png)

이 파일의 확장자를 .gz으로 변경한 뒤 압축을 해제하면 다시 한번 flag 파일을 얻을 수 있다.

![gz](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%9A%B0%EB%A6%AC%EB%8A%94%20%EC%9D%B4%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%94%8C%EB%9E%98%EA%B7%B8%EB%A5%BC/image/gz.png)

![unzip](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%9A%B0%EB%A6%AC%EB%8A%94%20%EC%9D%B4%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%94%8C%EB%9E%98%EA%B7%B8%EB%A5%BC/image/unzip.png)

압축 해제된 파일을 메모장으로 열어보면 플래그를 얻을 수 있다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%9A%B0%EB%A6%AC%EB%8A%94%20%EC%9D%B4%20%ED%8C%8C%EC%9D%BC%EC%97%90%20%ED%94%8C%EB%9E%98%EA%B7%B8%EB%A5%BC/image/flag.png)

## Flag

> ABCTF{broken_zipper}
