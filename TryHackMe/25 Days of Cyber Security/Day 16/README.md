# Day 16 | Scripting Help! Where is Santa?

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.3

## Walkthrough
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/attackbox.png)

Oh no! Santa π has taken off, leaving you -- the faithful elves behind! Can you help find Santa's location?

Luckily, the elves are OSINT masters and remember a thing or two. Specifically, they remember:

- Santa has a webpage at 10.10.255.161/static/index.html to help lost elves find their way home. Santa never told the elves what port number the webserver is on. Can you find out?!
- This webpage has a link somewhere on it, hidden away so anyone that isn't an elf can't find it.
- Santa's Sled has an API we can talk too. The key for the API is between 0 and 100, and it's an odd number. But be careful! After an unknown number of attempts, Santa's Sled will ban your IP address. 

Deploy the machine that is running Santa's Sled and allow a couple of minutes for the target (10.10.255.161) to start up. Using your Python skills from Day 15 to find the correct key for the API.

***

### What is the port number for the web server?

> 80

`nmap` λͺλ Ήμ΄λ₯Ό μ¬μ©ν΄ ν¬νΈ μ€μΊμ μ§ννλ€.

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/nmap.png)

### Without using enumerations tools such as Dirbuster, what is the directory for the API?  (without the API key)

> /api/

### Where is Santa right now?

> Winter Wonderland, Hyde Park, London.

Find out the correct API key. Remember, this is an odd number between 0-100. After too many attempts, Santa's Sled will block you. 

### To unblock yourself, simply terminate and re-deploy the target instance (10.10.255.161)

> 57

Day 15μμ μλμ κ°μ μΉνμ΄μ§μ λ§ν¬λ₯Ό κ°μ Έμ€λ μ½λλ₯Ό μ»μ μ μλ€.

![soup](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/soup.png)

ν΄λΉ μ½λλ₯Ό λ³΅μ¬ν΄ `webpage.py` νμΌμ λ³΅μ¬ ν `testurl.com` λΆλΆμ λ΄ μμ΄νΌλ₯Ό μλ ₯ ν μ€ννλ€.

![webpage](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/webpage.png)

μ€λ₯κ° λ°μν΄ μ€νμ΄ λμ§ μμλ€. ν΄λΉ μ½λλ₯Ό μλμ κ°μ΄ μμ  ν λ€μ μ€ννλ©΄ μΉνμ΄μ§ μ½λλ₯Ό μ»μ μ μλ€.

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

μμ κ°μ΄ μμ ν μ½λλ₯Ό λ€μ μ€ννλ©΄ μΉνμ΄μ§ λͺ¨λ  λ§ν¬κ° λμ¨λ€.

![link](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/link.png)

api keyμ λ¨μλ₯Ό μ°Ύμλ€. api keyλ 0λΆν° 100κΉμ§μ μ«μμ΄λ―λ‘ ν€λ₯Ό μ°Ύμ μλ‘μ΄ μ½λ `apikey.py`λ₯Ό λ§λ λ€.<br>
μ½λλ₯Ό λ§λ€ λ μμμ λλ¬΄ λ§μ νμλ₯Ό μλνλ©΄ IP μ£Όμλ₯Ό λ²€νλ€κ³  λμ΄ μμΌλ νμ λ¨Όμ  μ§ννλ€.

```
import requests 

for api_key in range(1,100,2):
    html = requests.get(f'http://10.10.255.161/api/{api_key}') 
    print(html.text)
```

μμ κ°μ΄ μ½λλ₯Ό λ§λ  ν μ€ννλ©΄ 57λ²μ§Έμμ μλ¬κ° λμ§ μμλ€.

![57](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/57.png)

ν΄λΉ api key μ£Όμλ₯Ό μλ ₯ν΄ λ€μ΄κ°λ©΄ santaκ° μλ μ£Όμλ₯Ό μ μ μλ€.

![locate](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2016/image/locate.png)
