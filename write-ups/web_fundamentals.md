# Web Fundamentals

Learn how the web works!  
https://tryhackme.com/room/webfundamentals
****
  
# [Task 5] Mini CTF

**Tasks:**

There's a web server running on ***http://MACHINE_IP:8081*** Connect to it and get the flags!

* GET request. Make a ***GET*** request to the web server with path ***/ctf/get***  
* POST request. Make a ***POST*** request with the body ***"flag_please"*** to ***/ctf/post***  
* Get a cookie. Make a ***GET*** request to ***/ctf/getcookie*** and check the cookie the server gives you  
* Set a cookie. Set a cookie with name ***"flagpls"*** and value ***"flagpls"*** in your devtools and make a ***GET*** request to ***/ctf/sendcookie***  
  
****
**First let's get some help:**  
Lets check the --help for curl. The lab already told us how to make a post request ***"-X POST"*** and how to send data in a POST ***"--data"***; all we need now is to figure out how to get a cookie and set a cookie.  

```bash
root@kali:~# curl --help | grep cookie
 -b, --cookie <data|filename> Send cookies from string/file
 -c, --cookie-jar <filename> Write cookies to <filename> after operation
```

**#1 What's the GET flag?**

```bash
root@kali:~# curl http://10.10.57.93:8081/ctf/get
```

Answer: thm{162520bec925bd7979e9ae65a725f99f}

**#2 What's the POST flag?**

```bash
root@kali:~# curl http://10.10.57.93:8081/ctf/post --request POST --data "flag_please"
```

Answer: thm{3517c902e22def9c6e09b99a9040ba09}

**#3 What's the "Get a cookie" flag?**

From our help output above we see that ***"--cookie-jar"*** requires a file to write the cookie to. The trailing ***"-"*** at the end is used instead of a file to write to basically making it print to the terminal.

```bash
root@kali:~# curl http://10.10.57.93:8081/ctf/getcookie --cookie-jar -
```

Answer: thm{91b1ac2606f36b935f465558213d7ebd}

**#4 What's the "Set a cookie" flag?**

```bash
root@kali:~# curl http://10.10.57.93:8081/ctf/sendcookie -cookie "flagpls=flagpls"
```

Answer: thm{c10b5cb7546f359d19c747db2d0f47b3}  
  
WE DID IT! YAY!
