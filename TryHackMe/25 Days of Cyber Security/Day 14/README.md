# Day 13 | Networking Coal for Christmas

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
#### Task #1

While hunting and searching for any hints or clues

Santa uncovers some details and shares the news

Rudolph loved to use Reddit and browsed aplenty

His username was 'IGuidetheClaus2020'

Many OSINT investigations start with only a username. A user's posting history can possibly lead to further information. Sometimes, it's the smallest of clues that help us out. Comb through Rudolph's Reddit history and answer questions #1-5 below. You may need to use partial clues with a search engine to fill in the gaps.

Watch TheCyberMentor's video on solving this task!

Learning Objectives:

1. Identify important information based on a user's posting history.
2. Utilize outside resources, such as search engines, to identify additional information, such as full names and additional social media accounts.

Additional Resources:

While Rudolph's posting history is enough for us to identify that he has other social media accounts, sometimes we are not that lucky. Great tools exist that allow us to search for user accounts across social media platforms. Sites, such as https://namechk.com/, https://whatsmyname.app/ and https://namecheckup.com/ will identify other possible accounts quickly for us. Tools, such as https://github.com/WebBreacher/WhatsMyName and https://github.com/sherlock-project/sherlock do this as well. Simply enter a username, hit search, and comb through the results. It's that easy!

#### Task #2

Well it looks like you have uncovered Rudolph's Twitter

Now we can read into all of his chitter

Go through his profile and give it some views

The deeper you dig, the better the clues


By finding another account belonging to our user, we open up the possibility of gathering even more information. Utilize the information found on Rudolph's Twitter account to answer questions #6-11.

Learning Objectives:

1. Identify important information based on a user's posting history.
2. Use reverse image searching to identify where a photo was taken and possibly identify additional information, such as other user accounts.
3. Utilize image EXIF data to uncover critical details, such as exact photo location, camera make and model, the date the photo was taken, and more.
4. Use discovered emails to search through breached data to possibly identify user passwords, name, additional emails, and location.

Additional Resources

This task was created to identify common critical steps in an OSINT investigation. Reverse image searching can help not only identify where an image was taken, but it can assist with identifying websites where that photo exists as well as similar photos (possibly from the same photoset), which can be incredibly useful in an investigation. While Google Images is used in our example, other sites should also be utilized to be as thorough as possible. No one site is perfect when it comes to reverse image searching (or any tool for that matter). Sites like https://yandex.com/images/ , https://tineye.com/ and https://www.bing.com/visualsearch?FORM=ILPVIS are great as well. Additionally, do not neglect the possibility of EXIF data existing in an image. While a lot of sites strip this data, not all do. It never hurts to look and can provide a wealth of information when the data is still there.

Finally, breached data can be incredibly useful from an investigative standpoint. Breach data does not just include passwords. It often has full names, addresses, IP information, password hashes, and more. We can often use this information to tie to other accounts. For example, say we find an account with the email of v3ry1337h4ck3r@gmail.com. If we search that email for breached data, we might find a password or hash associated with it. If unique enough, we can search that password or hash in a breach database and use it to identify other possible accounts. We can do the same with usernames, IPs, names, etc. The possibilities are vast and one email address can lead to a slew of information.

Websites such as https://haveibeenpwned.com/ will help identify if an account has ever been breached and will, at a minimum, inform us if an account existed at one point. However, it does not provide any password information. Free sites such as http://scylla.sh/ will provide password information and are easy to search through. The data on free sites can tend to be older and not up to date with the latest breach information, but these sites are still a powerful resource. Lastly, paid sites such as https://dehashed.com/ offer up to date information and are easily searchable at affordable rates.

Wrapping Up

It looks like finding Rudolph was a bit too easy
His OPSEC would make any security pro queasy
To the Windy City, Rudolph was tracked
Christmas is saved, we brought Rudolph back

***

What URL will take me directly to Rudolph's Reddit comment history?

> https://www.reddit.com/user/IGuidetheClaus2020/comments/

Google에 위 글에서 알려준 `IGuidetheClaus2020`을 검색한다.

![google]()

두가지 페이지가 나오는데 Reddit Comment를 찾는 것이니 Reddit 페이지로 들어간 후 Comment 탭을 클릭한다.

![comment]()

According to Rudolph, where was he born?

> Chicago

Comment를 읽어보면 단서를 얻을 수 있다.

![born]()

Rudolph mentions Robert.  Can you use Google to tell me Robert's last name?

> May

Google에 `Rudolph mentions Robert`를 검색하면 Wikipedia에서 Robert의 마지막 이름을 얻을 수 있다.

![may]()

On what other social media platform might Rudolph have an account?

> Twitter

`IGuidetheClaus2020`을 검색했을 때 첫번째에 있던 Twitter 계정을 찾을 수 있다.

![twitter]()

What is Rudolph's username on that platform?

> IGuideClaus2020

해당 트위터 계정에 들어가면 사용자이름을 얻을 수 있다.

![username]()

What appears to be Rudolph's favorite TV show right now?

> Bachelorette

스크롤해 트윗한 내용을 찾아보면 Bachelorette라는 TV show를 찾을 수 있다.

![show]

Based on Rudolph's post history, he took part in a parade.  Where did the parade take place?

> Chicago

Parade에 간 사진 2개를 찾을 수 있는데 그 중 하나의 이미지 주소를 복사한다.

![two]()

![copy]()

복사한 이미지 주소를 https://images.google.com 에 검색한다.

![image]()

Chicago에서 parade가 열린 것을 확인할 수 있다.

![search]()

Okay, you found the city, but where specifically was one of the photos taken?

> 41.891815, -87.624277

아래에 사진의 주소를 적은 글이 있다.

![photo]()

해당 글을 클릭해 이미지의 주소를 복사한다.

![location]()

http://exif.regex.info/ 사이트에 들어가 복사한 이미지 주소를 검색하면 위치와 플래그를 알 수 있다.

![exif]()

Did you find a flag too?

> {FLAG}ALWAYSCHECKTHEEXIFD4T4

Has Rudolph been pwned? What password of his appeared in a breach?

> spygame

https://scylla.sh/ 사이트가 없어 답만 기재했다.

Based on all the information gathered.  It's likely that Rudolph is in the Windy City and is staying in a hotel on Magnificent Mile.  What are the street numbers of the hotel address?

> 540

트위터 글에서 @Marriot 태그를 볼 수 있다. 해당 태그를 확인하면 Marriot 호텔인 것을 알 수 있다.

![hotel]()

구글에 Marriot Magnificent Mile를 검색하면 도로주소를 알 수 있다.

https://www.marriott.co.uk/hotels/travel/chidt-chicago-marriott-downtown-magnificent-mile/

![street]()