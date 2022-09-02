# Pickle Rick

## Workstation
- Virtual Box : VMware Workstation 16 Pro
- OS : kali-linux-2022.3

## Walkthrough

### Task 1 | Pickle Rick
This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle.

Deploy the virtual machine on this task and explore the web application: 10.10.47.189

![machine]()

You can also access the web app using the following link: https://10-10-47-189.p.thmlabs.com (this will update when the machine has fully started)

---

* What is the first ingredient Rick needs?

> mr. meeseek hair

nmap으로 포트 스캔을 한다.

![nmap]()

http 서버가 열려있으니 접속해 본다.

![http]()

페이지 소스를 확인해 보면 Username이 숨겨져 있었다.

![source]()

그럼 `gobuster`를 사용해 디렉터리를 검색한다.

![gobuster]()

assets 디렉터리를 발견했지만 안에는 아무것도 없었다.

![assets]()

robots.txt 파일도 검색했더니 이상한 문자열이 나왔다.

![robots]()

그럼 `nikto`를 사용해 웹 서버 취약점을 검색한다.

![nikto]()

gobuster에서 얻을 수 없었던 login.php 페이지가 있다고 한다.

![login]()

페이지 소스에서 얻은 Username이랑 robots.txt 파일에서 얻은 문자열을 입력하면 로그인이 되어 portal.php 파일로 이동된다.

또한 Command Panel이라고 되어있는 부분에 `id` 명령어를 입력하면 결과가 나온다.

![id]()

그럼 내부 디렉터리를 확인하면 Sup3rS3cretPickl3Ingred.txt 파일이 있는데 해당 파일을 cat 명령어로 실행하려고 했으나 실패했다.

![ls-al]()

![cat]()

하지만 다른 파일들에 login.php 와 portal.php 파일이 있는것으로 봐서 해당 페이지는 웹 서버 디렉터리 같아 보여 url에 텍스트 파일을 입력하면 첫번째 플래그가 나온다.

![1st]()

* Whats the second ingredient Rick needs?

> 1 jerry tear

그럼 clue.txt 파일도 같이 확인해 보면 파일 시스템을 확인하라고 한다.

![clue]()

`sudo -l` 명령어로 사용가능한 명령어를 확인하면 패스워드 없이 root 권한을 사용가능하다고 되어있다.

![sudo-l]()

그럼 `sudo id`를 사용하면 root 권한을 얻을 수 있다.

![sudo-id]()

`sudo ls -al /home`을 사용해 home 디렉터리를 확인하면 rick 계정이 있다.

![home]()

또 rick 계정 안에는 second ingredient이 있다.

![rick]()

이제 cat 명령어가 아까 사용이 되지 않기 때문에 more, less 명령어를 차례대로 확인해 봤는데 less 명령어에서 내용 확인이 가능했따.

```
sudo less second ingredient
```

![2nd]()

* Whats the final ingredient Rick needs?

> fleeb juice

그럼 이제 sudo 명령어로 root 권한을 사용할 수 있으니 root 디렉터리를 확인한다.

```
sudo ls -al /root
```

![root]()

그럼 3rd.txt 파일이 있으니 다시 한번 less 명령어로 내용을 확인한다.

```
sudo less 3rd.txt
```

![3rd]()