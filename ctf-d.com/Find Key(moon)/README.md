
# Find Key(moon)

![main]()

## Workstation
* OS : Window 10

## Walkthrough

moon.png 파일을 다운받는다.

![moon]()

이 사진 속에 숨겨진에 있는지 확인 하기 위해 https://stegonline.georgeom.net/upload 에서 확인해 본다.

stegonline에 이미지 파일이 계속 업로드 되지 않는다.

이미지 파일이 아닌것 같으니 HxD로 파일 원본 내용을 확인해 본다.

![hxd]()

파일이 깨져있지만 파일의 마지막 부분을 보면 flag.txt.UT가 계속 나오며 PK란 단어도 계속 나온다.

PK가 무엇인지 알기 위해 해당 코드 50 4B를 구글링 한 결과 해당 코드는 PKZIP이란 압축파일인 것을 알 수 있다.

![pk]()

반디집을 열어 moon.png 파일을 열어주면 flag.txt* 파일을 얻을 수 있다.

![unzip]()

해당 파일은 암호화 되어 있어 우선 가장 먼저 생각나는 사진 이름인 moon을 넣어주면 암호화가 풀려 flag를 얻을 수 있다.

![flag]()

## Flag
> sun{0kay_it_is_a_m00n}