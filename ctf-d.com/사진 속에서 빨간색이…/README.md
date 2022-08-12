# 사진 속에서 빨간색이…

![main](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%82%AC%EC%A7%84%20%EC%86%8D%EC%97%90%EC%84%9C%20%EB%B9%A8%EA%B0%84%EC%83%89%EC%9D%B4%E2%80%A6/image/Main.png)

## Workstation
* OS : Windows 10

## Walkthrough

hidden.png 파일이다.

![hidden](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%82%AC%EC%A7%84%20%EC%86%8D%EC%97%90%EC%84%9C%20%EB%B9%A8%EA%B0%84%EC%83%89%EC%9D%B4%E2%80%A6/image/hidden.png)

문제에서는 빨간색이 이상하다고 한다.

https://stegonline.georgeom.net/upload 사이트에서 이미지 분석을 진행할 수 있다.

![stegonline](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%82%AC%EC%A7%84%20%EC%86%8D%EC%97%90%EC%84%9C%20%EB%B9%A8%EA%B0%84%EC%83%89%EC%9D%B4%E2%80%A6/image/stegonline.png)

비트 변환(Browse Bit Planes)을 진행하면 RED 0 일때 플래그가 나왔다.

![flag](https://github.com/jasperkim425/Walkthrough/blob/main/ctf-d.com/%EC%82%AC%EC%A7%84%20%EC%86%8D%EC%97%90%EC%84%9C%20%EB%B9%A8%EA%B0%84%EC%83%89%EC%9D%B4%E2%80%A6/image/flag.png)


## Flag
> tjctf{0dd5_4nd_3v3n5}
