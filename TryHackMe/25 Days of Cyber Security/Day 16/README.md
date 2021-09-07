# Day 16 | Scripting Help! Where is Santa?

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.3

## Walkthrough
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/attackbox.png)

Oh no! Santa 🎅 has taken off, leaving you -- the faithful elves behind! Can you help find Santa's location?

Luckily, the elves are OSINT masters and remember a thing or two. Specifically, they remember:

- Santa has a webpage at 10.10.255.161/static/index.html to help lost elves find their way home. Santa never told the elves what port number the webserver is on. Can you find out?!
- This webpage has a link somewhere on it, hidden away so anyone that isn't an elf can't find it.
- Santa's Sled has an API we can talk too. The key for the API is between 0 and 100, and it's an odd number. But be careful! After an unknown number of attempts, Santa's Sled will ban your IP address. 

Deploy the machine that is running Santa's Sled and allow a couple of minutes for the target (10.10.255.161) to start up. Using your Python skills from Day 15 to find the correct key for the API.

***

### What is the port number for the web server?

> 80

`nmap` 명령어를 사용해 포트 스캔을 진행한다.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/nmap.png)

### Without using enumerations tools such as Dirbuster, what is the directory for the API?  (without the API key)

> /api/

### Where is Santa right now?

> Winter Wonderland, Hyde Park, London.

Find out the correct API key. Remember, this is an odd number between 0-100. After too many attempts, Santa's Sled will block you. 

### To unblock yourself, simply terminate and re-deploy the target instance (10.10.255.161)

> 57

Day 15에서 아래와 같은 웹페이지의 링크를 가져오는 코드를 얻을 수 있다.

![soup](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/soup.png)

해당 코드를 복사해 `webpage.py` 파일에 복사 후 `testurl.com` 부분에 내 아이피를 입력 후 실행한다.

![webpage](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/webpage.png)

오류가 발생해 실행이 되지 않았다. 해당 코드를 아래와 같이 수정 후 다시 실행하면 웹페이지 코드를 얻을 수 있다.

```
# Import the libraries we downloaded earlier
# if you try importing without installing them, this step will fail
from bs4 import BeautifulSoup
import requests 

# replace testurl.com with the url you want to use.
# requests.get downloads the webpage and stores it as a variable
html = requests.get('http://10.10.255.161/') 

# this parses the webpage into something that beautifulsoup can read over
soup = BeautifulSoup(html.text, "lxml")
# lxml is just the parser for reading the html 

# this is the line that grabs all the links # stores all the links in the links variable
links = soup.find_all('a') 

for link in links:      
    # prints each link    
    if "href" in link.attrs:
        print(link["href"])
```

위와 같이 수정한 코드를 다시 실행하면 웹페이지 모든 링크가 나온다.

![link](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/link.png)

api key의 단서를 찾았다. api key는 0부터 100까지의 숫자이므로 키를 찾을 새로운 코드 `apikey.py`를 만든다.<br>
코드를 만들 때 위에서 너무 많은 횟수를 시도하면 IP 주소를 벤한다고 되어 있으니 홀수 먼저 진행한다.

```
import requests 

for api_key in range(1,100,2):
    html = requests.get(f'http://10.10.255.161/api/{api_key}') 
    print(html.text)
```

위와 같이 코드를 만든 후 실행하면 57번째에서 에러가 나지 않았다.

![57](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/57.png)

해당 api key 주소를 입력해 들어가면 santa가 있는 주소를 알 수 있다.

![locate](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/locate.png)
