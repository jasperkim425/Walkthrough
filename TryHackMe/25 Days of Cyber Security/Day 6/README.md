# Day 6 | Web Exploitation Be careful with what you wish on a Christmas night\

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
Deploy your AttackBox (the blue "Start AttackBox" button) and the tasks machine (green button on this task) if you haven't already. Once both have deployed, open Firefox on the AttackBox and copy/paste the machines IP (http://MACHINE_IP:5000) into the browser search bar (the webserver is running on port 5000, so make sure this is included in your web requests).

![attackbox]()

![ip]()

What vulnerability type was used to exploit the application?

> Stored crosssite scripting

What query string can be abused to craft a reflected XSS?

> q

Search query 부분에 hello를 쳐보니 파라미터 값에 `/?q=hello`가 나왔다.

![q]()

해당 hello 부분에 `<script>alert(1)</script>`를 사용해 xss 취약점이 있는 것을 확인했다.

![payload]()

Launch the OWASP ZAP Application

![zap]()

Run a ZAP (zaproxy) automated scan on the target. How many XSS alerts are in the scan?

> 2

Automated scan 부분에 스캔할 주소를 입력 후 스캔을 시작하면 `Alerts` 부분에 XSS 취약점이 총 3개가 나왔다. 하지만 여기서 Persistent와 Reflected는 같은 취약점이므로 2개를 발견했다.

![scan]()

Explore the XSS alerts that ZAP has identified, are you able to make an alert appear on the "Make a wish" website?

발견한 취약점 중 Dom Based 취약점의 URL를 복사해 브라우저에 입력하면 취약점을 찾은 것을 알 수 있다.

![Dom]()

![xss]()
