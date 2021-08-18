# Hack The Box Invite Code

## Workstation
- Virtual Box : VMware Fusion 12.1.0
- OS : kali-linux-2020.04

## Walkthrough

```
https://www.hackthebox.eu/
```

Hack The Box 사이트로 이동 후 가입을 진행하려고 하는데 가입을 위해 가입페이지를 해킹해 invite code(초대 코드)를 찾으라고 명시되어 있다.

![invitecode]()

아무런 정보가 없으므로 힌트를 얻기로 한다.

![hint]()

콘솔을 확인하라고 명시되어 있다. F12를 눌러 개발자 도구에서 console 부분을 확인한다.

![console]()

하단에 보면 This page loads an interesting javascript file. See if you can find it :)이라고 적혀있다. 가입 페이지는 자바스크립트 언어로 구성되어 있으며 코드를 확인해보라고 되어있다.

그럼 마우스 오른쪽 버튼을 눌러 페이지 소스 보기를 누른다.

![source]()

페이지 소스를 확인하면 다음과 같이 많은 코드들이 작성되어 있다. 하나하나 해석하기 힘들어 `cmd + f` 버튼 눌러 페이지 찾기 버튼으로 invite code(초대 코드)를 찾아보도록 한다. 우선 invite만 입력해 확인해 본다.

![invite]()

젤 하단에 /js/inviteapi.min.js부분만 js확장자로 끝나는 것을 확인할 수 있으며 자바스크립트 언어로 만들어진 파일이라는 것을 알 수 있다. 

/js/inviteapi.min.js를 클릭해 들어가면 이상한 자바스크립트 코드가 작성되어 있다.

![inviteapi]()

주석으로 처리된 부분을 확인하면 난독화된 이상한 자바스크립트 코드가 있다고 되었다.

필자는 자바스크립트를 배우지 않아 이 코드를 해석할 수가 없다. [beautifier.io](beautifier.io) 사이트로 들어가 이상한 자바스크립트 코드를 입력 후 Beautify Code 버튼을 누르면 보기 좋게 코드가 다시 작성되어 표시되었다. 또한 makeInviteCode()라는 함수가 생성된 것을 확인할 수 있다.

![beautifier]()

url 부분에 /api/invite/how/to/generate 페이지를 들어가 확인한다.

![generate]()

이 페이지는 뭔가 이상하다고 나온다. 

가입 페이지 소스 코드 화면(https://www.hackthebox.eu/invite)에서 개발자 도구로 들어간 뒤 makeInviteCode()를 확인해 본다.

![makeinvitecode]()

data 부분의 이상한 문자들이 BASE64로 인코딩되어 있음을 확인했다. 힌트 부분에서 데이터가 암호화되어 있다고 한다. 

http://www.base64decode.org에 접속해 암호화된 코드를 삽입 후 디코드 시킨다.

![base64_1]()

초대코드를 생성하기 위해서는 /api/invite/generate에 POST 요청을 하라고 되어있다.

칼리 리눅스 터미널에서 `curl` 명령어를 사용해 초대 코드를 생성해보겠다. 

curl은 url를 사용하여 데이터를 전송하기 위한 명령줄 도구 및 라이브러리다.

`-XPOST` 옵션을 사용해 초대코드를 생성해본다.

![curl]()

초대 코드가 나온 것이 아니라 또 하나의 암호화된 코드가 나왔다. 

다시 www.base64decode.org에 접속해서 암호화된 코드를 해독한다. 

![code]()

이제 초대코드가 생성됐다.

hackthebox에서 초대 코드 입력 후 가입을 진행하면 된다.