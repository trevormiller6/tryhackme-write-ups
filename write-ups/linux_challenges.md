# Linux Challenges  
```
████████╗██████╗ ██╗   ██╗██╗  ██╗ █████╗  ██████╗██╗  ██╗███╗   ███╗███████╗
╚══██╔══╝██╔══██╗╚██╗ ██╔╝██║  ██║██╔══██╗██╔════╝██║ ██╔╝████╗ ████║██╔════╝
   ██║   ██████╔╝ ╚████╔╝ ███████║███████║██║     █████╔╝ ██╔████╔██║█████╗  
   ██║   ██╔══██╗  ╚██╔╝  ██╔══██║██╔══██║██║     ██╔═██╗ ██║╚██╔╝██║██╔══╝  
   ██║   ██║  ██║   ██║   ██║  ██║██║  ██║╚██████╗██║  ██╗██║ ╚═╝ ██║███████╗
   ╚═╝   ╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚═╝     ╚═╝╚══════╝
                           -- Linux Challenges --
```
https://tryhackme.com/room/linuxctf
# [Task 1] Linux Challenges Introduction
**#1 How many visible files can you see in garrys home directory?**
```bash
garry@ip-10-10-203-119:~$ ls -l
total 20
-rw-rw-r-- 1 garry ubuntu  190 Feb 19  2019 flag1.txt
-rwxrwxr-x 1 garry ubuntu 8656 Feb 20  2019 flag24
-rw-rw-r-- 1 garry ubuntu 3589 Feb 20  2019 flag29
```  
Answer: 3  

# [Task 2] The Basics

**#1 What is flag 1?**
```bash
garry@ip-10-10-203-119:~$ cat flag1.txt 
There are flags hidden around the file system, its your job to find them.

Flag 1: f40dc0cff080ad38a6ba9a1c2c038b2c

Log into bobs account to get flag 2.

Username: bob
Password: linuxrules
```
Answer: f40dc0cff080ad38a6ba9a1c2c038b2c

**#2 What is flag 2?**
```bash
garry@ip-10-10-203-119:~$ su bob
Password: 
bob@ip-10-10-203-119:/home/garry$ cd ~
bob@ip-10-10-203-119:~$ ls -l
total 52
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Desktop
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Documents
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Downloads
drwxrwxr-x 2 bob bob 4096 Feb 18  2019 flag13
-rw-rw-r-- 1 bob bob   65 Feb 20  2019 flag21.php
-rw-rw-r-- 1 bob bob   41 Feb 18  2019 flag2.txt
-rw-rw-r-- 1 bob bob   39 Aug 16 06:00 flag4.txt
-rw-rw-r-- 1 bob bob  149 Feb 18  2019 flag8.tar.gz
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Music
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Pictures
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Public
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Templates
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Videos
bob@ip-10-10-203-119:~$ cat flag2.txt 
Flag 2: 8e255dfa51c9cce67420d2386cede596
```
Answer: 8e255dfa51c9cce67420d2386cede596

**#3 Flag 3 is located where bob's bash history gets stored.**
```bash
bob@ip-10-10-203-119:~$ cat .bash_history 
9daf3281745c2d75fc6e992ccfdedfcd
```
Answer: 9daf3281745c2d75fc6e992ccfdedfcd  

**#4 Flag 4 is located where cron jobs are created.**
```bash
bob@ip-10-10-203-119:~$ crontab -e
```
This opens a text editor for cron the flag is the line that reads
```bash
0 6 * * * echo 'flag4:dcd5d1dcfac0578c99b7e7a6437827f3' > /home/bob/flag4.txt
```  
Answer: dcd5d1dcfac0578c99b7e7a6437827f3

**#5 Find and retrieve flag 5.**
```bash
bob@ip-10-10-203-119:~$ cd /
bob@ip-10-10-203-119:/$ find . -name flag5* -print 2>/dev/null
./lib/terminfo/E/flag5.txt
bob@ip-10-10-203-119:/$ cat /lib/terminfo/E/flag5.txt
bd8f33216075e5ba07c9ed41261d1703
```
Answer: bd8f33216075e5ba07c9ed41261d1703  

**#6 "Grep" through flag 6 and find the flag. The first 2 characters of the flag is c9.**
```bash
bob@ip-10-10-203-119:/$ find . -name flag6* -print 2>/dev/null
./home/flag6.txt
bob@ip-10-10-203-119:/$ cat /home/flag6.txt | grep c9
Sed sollicitudin eros quis vulputate rutrum. Curabitur mauris elit, elementum quis sapien sed, ullamcorper pellentesque neque. Aliquam erat volutpat. Cras vehicula mauris vel lectus hendreri
t, sed malesuada ipsum consectetur. Donec in enim id erat condimentum vestibulum c9e142a1e25b24a837b98db589b08be5 vitae eget nisi. Suspendisse eget commodo libero. Mauris eget gravida quam, 
a interdum orci. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Quisque eu nisi non ligula tempor efficitur. Etiam eleifend, odio vel bibendum mattis
, purus metus consectetur turpis, eu dignissim elit nunc at tortor. Mauris sapien enim, elementum faucibus magna at, rutrum venenatis ipsum.
```
Answer: c9e142a1e25b24a837b98db589b08be5

**#7 Look at the systems processes. What is flag 7.** 
```bash
bob@ip-10-10-203-119:/$ ps -aux | grep flag
root      1393  0.0  0.0   6008   268 ?        S    05:48   0:00 flag7:274adb75b337307bd57807c005ee6358 1000000
```
Answer: 274adb75b337307bd57807c005ee6358

**#8 De-compress and get flag 8.**
```bash
bob@ip-10-10-203-119:/$ find . -name flag8* -print 2>/dev/null
./home/bob/flag8.tar.gz
bob@ip-10-10-203-119:/$ cd ~
bob@ip-10-10-203-119:~$ tar xzf flag8.tar.gz
bob@ip-10-10-203-119:~$ ls -l
total 56
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Desktop
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Documents
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Downloads
drwxrwxr-x 2 bob bob 4096 Feb 18  2019 flag13
-rw-rw-r-- 1 bob bob   65 Feb 20  2019 flag21.php
-rw-rw-r-- 1 bob bob   41 Feb 18  2019 flag2.txt
-rw-rw-r-- 1 bob bob   39 Aug 16 06:00 flag4.txt
-rw-rw-r-- 1 bob bob  149 Feb 18  2019 flag8.tar.gz
-rw-rw-r-- 1 bob bob   33 Feb 18  2019 flag8.txt
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Music
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Pictures
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Public
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Templates
drwxr-xr-x 2 bob bob 4096 Feb 19  2019 Videos
bob@ip-10-10-203-119:~$ cat flag8.txt 
75f5edb76fe98dd5fc9f577a3f5de9bc
```
Answer: 75f5edb76fe98dd5fc9f577a3f5de9bc

**#9 By look in your hosts file, locate and retrieve flag 9.**
```bash
bob@ip-10-10-203-119:~$ cat /etc/hosts
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

127.0.0.1dcf50ad844f9fe06339041ccc0d6e280.com
```
Answer: 1dcf50ad844f9fe06339041ccc0d6e280

**#10 Find all other users on the system. What is flag 10.**
```bash
bob@ip-10-10-203-119:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
bob:x:1001:1001:Bob,,,:/home/bob:/bin/bash
5e23deecfe3a7292970ee48ff1b6d00c:x:1002:1002:,,,:/home/5e23deecfe3a7292970ee48ff1b6d00c:/bin/bash
alice:x:1003:1003:,,,:/home/alice:/bin/bash
mysql:x:112:117:MySQL Server,,,:/nonexistent:/bin/false
xrdp:x:113:118::/var/run/xrdp:/bin/false
whoopsie:x:114:120::/nonexistent:/bin/false
avahi:x:115:121:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
avahi-autoipd:x:116:122:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
colord:x:117:125:colord colour management daemon,,,:/var/lib/colord:/bin/false
geoclue:x:118:126::/var/lib/geoclue:/bin/false
speech-dispatcher:x:119:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/false
hplip:x:120:7:HPLIP system user,,,:/var/run/hplip:/bin/false
kernoops:x:121:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
pulse:x:122:127:PulseAudio daemon,,,:/var/run/pulse:/bin/false
rtkit:x:123:129:RealtimeKit,,,:/proc:/bin/false
saned:x:124:130::/var/lib/saned:/bin/false
usbmux:x:125:46:usbmux daemon,,,:/var/lib/usbmux:/bin/false
gdm:x:126:131:Gnome Display Manager:/var/lib/gdm3:/bin/false
garry:x:1004:1006:,,,:/home/garry:/bin/bash
```
Answer: 5e23deecfe3a7292970ee48ff1b6d00c:

# [Task 3] Linux Functionality

**#1 Run the command flag11. Locate where your command alias are stored and get flag 11.**
```bash
bob@ip-10-10-203-119:~$ flag11
You need to look where the alias are created...
bob@ip-10-10-203-119:~$ cat .bashrc | grep flag
alias flag11='echo "You need to look where the alias are created..."' #b4ba05d85801f62c4c0d05d3a76432e0
```  
Answer: b4ba05d85801f62c4c0d05d3a76432e0

**#2 Flag12 is located were MOTD's are usually found on an Ubuntu OS. What is flag12?**
```bash
bob@ip-10-10-83-68:~$ cd /etc/update-motd.d
bob@ip-10-10-83-68:/etc/update-motd.d$ ls -l
total 40
-rwxr-xr-x 1 root root 1177 Feb 18  2019 00-header
-rwxr-xr-x 1 root root 1157 Jun 14  2016 10-help-text
-rwxr-xr-x 1 root root  334 Nov 14  2018 51-cloudguest
-rwxr-xr-x 1 root root   97 May 24  2016 90-updates-available
-rwxr-xr-x 1 root root  299 Jul 22  2016 91-release-upgrade
-rwxr-xr-x 1 root root  111 Oct  1  2018 97-overlayroot
-rwxr-xr-x 1 root root  142 May 24  2016 98-fsck-at-reboot
-rwxr-xr-x 1 root root  144 May 24  2016 98-reboot-required
-rwxr-xr-x 1 root root  604 Nov  5  2017 99-esm
-rw-r--r-- 1 root root 1224 Feb 18  2019 logo.txt
bob@ip-10-10-83-68:/etc/update-motd.d$ cat 00-header 
#!/bin/sh
#
#    00-header - create the header of the MOTD
#    Copyright (C) 2009-2010 Canonical Ltd.
#
#    Authors: Dustin Kirkland <kirkland@canonical.com>
#
#    This program is free software; you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation; either version 2 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License along
#    with this program; if not, write to the Free Software Foundation, Inc.,
#    51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

[ -r /etc/lsb-release ] && . /etc/lsb-release

if [ -z "$DISTRIB_DESCRIPTION" ] && [ -x /usr/bin/lsb_release ]; then
# Fall back to using the very slow lsb_release utility
DISTRIB_DESCRIPTION=$(lsb_release -s -d)
fi

# Flag12: 01687f0c5e63382f1c9cc783ad44ff7f

cat logo.txt
```

Answer: 01687f0c5e63382f1c9cc783ad44ff7f

**#3 Find the difference between two script files to find flag 13.**
```bash
bob@ip-10-10-83-68:~$ find . -user bob -executable -print 2>/dev/null | grep flag
./flag13
bob@ip-10-10-83-68:~$ cd flag13
bob@ip-10-10-83-68:~/flag13$ ls
script1  script2
bob@ip-10-10-83-68:~/flag13$ diff script1 script2
2437c2437
< Lightoller sees Smith walking stiffly toward him and quickly goes to him. He yells into the Captain's ear, through cupped hands, over the roar of the steam... 
---
> Lightoller sees 3383f3771ba86b1ed9ab7fbf8abab531 Smith walking stiffly toward him and quickly goes to him. He yells into the Captain's ear, through cupped hands, over the roar of the steam
...
```

Answer: 3383f3771ba86b1ed9ab7fbf8abab531

**#4 Where on the file system are logs typically stored? Find flag 14.**

```bash
bob@ip-10-10-65-10:~$ ls -l /var/log | grep flag
-rwxr-xr-x 1 root              root  518561 Feb 18  2019 flagtourteen.txt
bob@ip-10-10-65-10:~$ cat /var/log/flagtourteen.txt | grep "^[0-9]"
71c3a8ad9752666275dadf62a93ef393
```
Answer: 71c3a8ad9752666275dadf62a93ef393

**#5 Can you find information about the system, such as the kernel version etc.**

```bash
bob@ip-10-10-65-10:/$ cat /etc/*release
FLAG_15=a914945a4b2b5e934ae06ad6f9c6be45
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.5 LTS"
NAME="Ubuntu"
VERSION="16.04.5 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.5 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
```
Answer: a914945a4b2b5e934ae06ad6f9c6be45

**#6 Flag 16 lies within another system mount.**

```bash
bob@ip-10-10-65-10:/$ ls -R /media
/media:
f

/media/f:
l

/media/f/l:
a

/media/f/l/a:
g

/media/f/l/a/g:
1

/media/f/l/a/g/1:
6

/media/f/l/a/g/1/6:
is

/media/f/l/a/g/1/6/is:
cab4b7cae33c87794d82efa1e7f834e6

/media/f/l/a/g/1/6/is/cab4b7cae33c87794d82efa1e7f834e6:
test.txt
```

Answer: cab4b7cae33c87794d82efa1e7f834e6

**#7 Login to alice's account and get flag 17. Her password is TryHackMe123**

```bash
bob@ip-10-10-65-10:/$ su alice
Password: 
alice@ip-10-10-65-10:/$ cd ~
alice@ip-10-10-65-10:~$ ls
flag17  flag19  flag20  flag22  flag23  flag32.mp3
alice@ip-10-10-65-10:~$ cat flag17
89d7bce9d0bab49e11e194b54a601362
```
Answer: 89d7bce9d0bab49e11e194b54a601362

**#8 Find the hidden flag 18.**
```bash
alice@ip-10-10-65-10:~$ ls -al
total 172
drwxr-xr-x 4 alice alice  4096 Feb 20  2019 .
drwxr-xr-x 6 root  root   4096 Feb 20  2019 ..
-rw------- 1 alice alice   518 Mar  7  2019 .bash_history
-rw-r--r-- 1 alice alice   220 Feb 18  2019 .bash_logout
-rw-r--r-- 1 alice alice  3771 Feb 18  2019 .bashrc
drwx------ 2 alice alice  4096 Feb 18  2019 .cache
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 flag17
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 .flag18
-rw-rw-r-- 1 alice alice 99001 Feb 19  2019 flag19
-rw-rw-r-- 1 alice alice    45 Feb 19  2019 flag20
-rw-rw-r-- 1 alice alice    96 Feb 19  2019 flag22
-rw-rw-r-- 1 alice alice    33 Feb 19  2019 flag23
-rw-rw-r-- 1 alice alice 10560 Feb 19  2019 flag32.mp3
-rw------- 1 alice alice    32 Feb 19  2019 .lesshst
-rw-r--r-- 1 alice alice   655 Feb 18  2019 .profile
drw-r--r-- 2 alice alice  4096 Mar  7  2019 .ssh
-rw------- 1 alice alice  3075 Feb 19  2019 .viminfo
alice@ip-10-10-65-10:~$ cat .flag18
c6522bb26600d30254549b6574d2cef2
```
Answer: c6522bb26600d30254549b6574d2cef2

**#9 Read the 2345th line of the file that contains flag 19.**
```bash
alice@ip-10-10-65-10:~$ sed -n 2345p flag19 
490e69bd1bf3fc736cce9ff300653a3b
```
Answer: 490e69bd1bf3fc736cce9ff300653a3b

# [Task 4] Data Representation, Strings and Permissions

**#1 Find and retrieve flag 20.**
```bash
alice@ip-10-10-65-10:~$ cat flag20 | base64 -d
02b9aab8a29970db08ec77ae425f6e68
```
Answer: 02b9aab8a29970db08ec77ae425f6e68

**#2 Inspect the flag21.php file. Find the flag.**
```bash
alice@ip-10-10-65-10:~$ vim /home/bob/flag21.php

<?=`$_POST[flag21_g00djob]`?>^M<?='MoreToThisFileThanYouThink';?>
```
Answer: g00djob

**#3 Locate and read flag 22. Its represented as hex.**
```bash
alice@ip-10-10-65-10:~$ cat flag22 | xxd -r -p
9d1ae8d569c83e03d8a8f61568a0fa7d
```
Answer: 9d1ae8d569c83e03d8a8f61568a0fa7d

**#4 Locate, read and reverse flag 23.**
```bash
alice@ip-10-10-65-10:~$ cat flag23 | rev
ea52970566f4c090a7348b033852bff5
```
Answer: ea52970566f4c090a7348b033852bff5

**#5 Analyse the flag 24 compiled C program. Find a command that might reveal human readable strings when looking in the source code.**
```bash
alice@ip-10-10-65-10:~$ find / -name flag24* -print 2>/dev/null
/home/garry/flag24
alice@ip-10-10-65-10:~$ cat /home/garry/flag24 | strings
/lib64/ld-linux-x86-64.so.2
libc.so.6
printf
__libc_start_main
__gmon_start__
GLIBC_2.2.5
UH-8
AWAVA
AUATL
[]A\A]A^A_
Nothing to see here!!
;*3$"
GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.11) 5.4.0 20160609
crtstuff.c
__JCR_LIST__
deregister_tm_clones
__do_global_dtors_aux
completed.7594
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
flag24.c
__FRAME_END__
__JCR_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
_edata
printf@@GLIBC_2.2.5
flag_24_is_hidd3nStr1ng
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
_Jv_RegisterClasses
__TMC_END__
_ITM_registerTMCloneTable
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.jcr
.dynamic
.got.plt
.data
.bss
.comment
```

Answer: hidd3nStr1ng

**#6 Flag 25 does not exist.**  
Answer not needed

**#7 Find flag 26 by searching the all files for a string that begins with 4bceb and is 32 characters long.**
```bash
alice@ip-10-10-225-224:/$ find / -xdev -type f -print0 2>/dev/null | xargs -0 grep -E '^[a-z0-9]{32}$' 2>/dev/null
Binary file /var/cache/apt/pkgcache.bin matches
Binary file /var/cache/apt/srcpkgcache.bin matches
/var/cache/apache2/mod_cache_disk/config.json:4bceb76f490b24ed577d704c24d6955d
```
Answer: 4bceb76f490b24ed577d704c24d6955d

**#8 Locate and retrieve flag 27, which is owned by the root user.**

```bash
alice@ip-10-10-98-158:/home/garry$ sudo -l
Matching Defaults entries for alice on ip-10-10-98-158.eu-west-1.compute.internal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alice may run the following commands on ip-10-10-98-158.eu-west-1.compute.internal:
    (ALL) NOPASSWD: /bin/cat /home/flag27
alice@ip-10-10-98-158:/home/garry$ sudo cat /home/flag27
6fc0c805702baebb0ecc01ae9e5a0db5
```
Answer: 6fc0c805702baebb0ecc01ae9e5a0db5

**#9 Whats the linux kernel version?**
```bash
alice@ip-10-10-98-158:/home/garry$ uname -a
Linux ip-10-10-98-158 4.4.0-1075-aws #85-Ubuntu SMP Thu Jan 17 17:15:12 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```
Answer: 4.4.0-1075-aws

**#10 Find the file called flag 29 and do the following operations on it:  
Remove all spaces in file.  
Remove all new line spaces.  
Split by comma and get the last element in the split.**  
```bash
alice@ip-10-10-98-158:~$ find / -name flag29 -print 2>/dev/null
/home/garry/flag29
```
```bash
alice@ip-10-10-98-158:~$ cat /home/garry/flag29 | tr -d " \n." | rev | cut -d "," -f 1 | rev
fastidiisuscipitmeaei
```

# [Task 5] SQL, FTP, Groups and RDP

**#1 Use curl to find flag 30.**

```bash
alice@ip-10-10-98-158:~$ curl localhost
flag30:fe74bb12fe03c5d8dfc245bdd1eae13f
```

**#2 Flag 31 is a MySQL database name.**
```bash
alice@ip-10-10-98-158:~$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.
```
```sql
mysql> show databases;
+-------------------------------------------+
| Database                                  |
+-------------------------------------------+
| information_schema                        |
| database_2fb1cab13bf5f4d61de3555430c917f4 |
| mysql                                     |
| performance_schema                        |
| sys                                       |
+-------------------------------------------+
5 rows in set (0.02 sec)
```
Answer: 2fb1cab13bf5f4d61de3555430c917f4  

**#3 Bonus flag question, get data out of the table from the database you found above!**
```sql
mysql> USE database_2fb1cab13bf5f4d61de3555430c917f4;

Database changed

mysql> show tables;
+-----------------------------------------------------+
| Tables_in_database_2fb1cab13bf5f4d61de3555430c917f4 |
+-----------------------------------------------------+
| flags                                               |
+-----------------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM flags;
+----+----------------------------------+
| id | flag                             |
+----+----------------------------------+
|  1 | ee5954ee1d4d94d61c2f823d7b9d733c |
+----+----------------------------------+
1 row in set (0.00 sec)
```
Answer : ee5954ee1d4d94d61c2f823d7b9d733c

**#4 Using SCP, FileZilla or another FTP client download flag32.mp3 to reveal flag 32.**

Answer: tryhackme1337

**#5 Flag 33 is located where your personal $PATH's are stored.**
```bash
bob@ip-10-10-98-158:~$ cat .profile 
#Flag 33: 547b6ceee3c5b997b625de99b044f5cf

# ~/.profile: executed by the command interpreter for login shells.
# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
# exists.
# see /usr/share/doc/bash/examples/startup-files for examples.
# the files are located in the bash-doc package.

# the default umask is set in /etc/profile; for setting the umask
# for ssh logins, install and configure the libpam-umask package.
#umask 022

# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
. "$HOME/.bashrc"
    fi
fi

# set PATH so it includes user's private bin directories
PATH="$HOME/bin:$HOME/.local/bin:$PATH"
```
Answer: 547b6ceee3c5b997b625de99b044f5cf

**#6 Switch your account back to bob. Using system variables, what is flag34?**
```bash
bob@ip-10-10-98-158:~$ echo $flag34
7a88306309fe05070a7c5bb26a6b2def
```
Answer: 7a88306309fe05070a7c5bb26a6b2def

**#7 Look at all groups created on the system. What is flag 35?**
```bash
bob@ip-10-10-98-158:~$ cat /etc/group | grep flag
flag35_769afb6:x:1005:
```
Answer: 769afb6

**#8 Find the user which is apart of the "hacker" group and read flag 36.**
```bash
bob@ip-10-10-98-158:~$ groups bob
bob : bob hacker
bob@ip-10-10-98-158:~$ find / -name flag36 -print 2>/dev/null
/etc/flag36
bob@ip-10-10-98-158:~$ cat /etc/flag36
83d233f2ffa388e5f0b053848caed1eb
```
Answer: 83d233f2ffa388e5f0b053848caed1eb  

**#9 Well done! You've completed the LinuxCTF room!**
