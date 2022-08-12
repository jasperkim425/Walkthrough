# 우리는 이 파일에 플래그를...

![main]()

## Workstation
* OS : Window 10

## Walkthrough

우선 플래그 파일을 다운받는다.

다운받은 플래그 파일을 메모장으로 열어보면 내용이 깨져나온다.

![file]()

flag 파일이 원래 무슨 파일이였는지 알기 위해 https://www.checkfiletype.com/ 사이트에서 파일을 검색한다.

이 파일은 원래 gz으로 압축된 파일이였다.

![checkfiletype]()

이 파일의 확장자를 .gz으로 변경한 뒤 압축을 해제하면 다시 한번 flag 파일을 얻을 수 있다.

![gz]()

![unzip]()

압축 해제된 파일을 메모장으로 열어보면 플래그를 얻을 수 있다.

![flag]()

## Flag

> ABCTF{broken_zipper}