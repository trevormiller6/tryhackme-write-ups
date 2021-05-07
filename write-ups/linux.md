# Learn Linux - *The True Ending*

The Goal is to capture the flag in /root/root.txt.  
https://tryhackme.com/room/zthlinux
****
**<u>Background:</u>**  
The file is owned by root so we need to get our hands on a user with root privs as we are currently the user shiba4 and can't run sudo.
``` shell
shiba4@nootnoot:/$ sudo -l
[sudo] password for shiba4:
Sorry, user shiba4 may not run sudo on nootnoot.
```

The instructions for the challenge say that everything we need to know is in the lesson. So we know that to capture the flag all we need are the commands that were gone over in this lesson.  

Given that we just took the lesson we can extrapolate a theme the author used to get the passwords of the other users. He had us use a file owned by the current user to get the credentials of another user. The ctf feels like task 33 where we had to ***find*** the binary shiba4 owned by shiba3 to get shiba4's password. 
****

**<u>The Hunt:</u>**  
Given what we know lets see what files are owned by shiba4 since that is the user we currently are.  

```shell
shiba4@nootnoot:/$ find . -user shiba4 -print 2>/dev/null
./home/shiba4
./home/shiba4/.profile
./home/shiba4/.bashrc
./home/shiba4/.bash_logout
./etc/shiba/shiba4
```

This is a dead end. Let's see what files the other users own. lets start with shiba1.

```shell
shiba4@nootnoot:/$ find . -user shiba1 -print 2>/dev/null
./home/shiba1
./home/shiba1/.profile
./home/shiba1/.gnupg
./home/shiba1/.bashrc
./home/shiba1/.bash_history
./home/shiba1/.local
./home/shiba1/.local/share
./home/shiba1/.cache
./home/shiba1/.bash_logout
```

Another dead end. Let's try shiba2

```shell
shiba4@nootnoot:/$ find . -user shiba2 -print 2>/dev/null
./var/log/test1234
./home/shiba1/shiba1
./home/shiba2
./home/shiba2/.profile
./home/shiba2/.bashrc
./home/shiba2/.bash_history
./home/shiba2/.local
./home/shiba2/.local/share
./home/shiba2/.bash_logout
```

shiba2 owns a file in /var/log which is interesting as shiba4 and shiba1 did not. We know shiba2's password from task 11. So let's switch to shiba2 and see what's in the file.

```shell
shiba4@nootnoot:/$ su shiba2
Password:
shiba2@nootnoot:/$ cat /var/log/test1234
nootnoot:notsofast
```

Jackpot! From the lesson we can see the author was using the user nootnoot in all the screenshots and was running sudo. Let's switch to nootnoot and see if we can cat the file /root/root.txt

```shell
shiba2@nootnoot:/$ su nootnoot
Password:
nootnoot@nootnoot:/$
```

For fun lets see what commands we are allowed to run.

```shell
nootnoot@nootnoot:/$ sudo -l
[sudo] password for nootnoot:
User nootnoot may run the following commands on nootnoot:
    (ALL : ALL) ALL
    
```
Now we know that we can run any command we want as root. so finally let's see what's in the file /root/root.txt

 ```shell
nootnoot@nootnoot:/$ sudo cat /root/root.txt
ad91979868d06e19d8e8a9c28be56e0c
```
****

The flag is *"ad91979868d06e19d8e8a9c28be56e0c"* and with that we complete the room.  
For bonus points I looked up the MD5 hash that is the flag and it's the [hash](https://nickfinder.com/Noot) of the text "noot"

**WE DID IT!**  